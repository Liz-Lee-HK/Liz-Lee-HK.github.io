---
title: MCP 文件传输方案调研 — Base64 vs. 文件中转服务
date: 2026-06-10 18:00:00
categories: Study
tags:
  - MCP
  - Agent
  - 文件传输
  - 架构设计
  - DeepAgents
toc: true
---

## 问题背景

MCP 的 tool 调用以 JSON 参数为主，当前正式规范没有定义通用的二进制文件上传机制，因此文件处理通常依赖 base64 内联或外部文件引用等工程方案。当 MCP 工具需要处理文件（如图片 PS、PDF 解析、文档转换）时，文件本身如何从 Agent 侧传到 MCP 服务侧就成了一个核心设计问题。

以**图片 PS 处理 MCP 服务**为例：用户在 Agent 对话中说"帮我把这张照片的背景去掉"，Agent 需要把图片文件传给 MCP 服务端处理。这个"传文件"的动作有两种主流方案。

---

## 方案一：Base64 内联传输

### 接口设计

MCP Server 的 tool 参数定义为接收 base64 字符串：

```json
{
  "name": "remove_background",
  "description": "移除图片背景",
  "inputSchema": {
    "type": "object",
    "properties": {
      "image_base64": {
        "type": "string",
        "description": "图片的 base64 编码字符串"
      },
      "mime_type": {
        "type": "string",
        "description": "图片 MIME 类型，如 image/png",
        "enum": ["image/png", "image/jpeg", "image/webp"]
      },
      "options": {
        "type": "object",
        "description": "PS 处理选项",
        "properties": {
          "feather": { "type": "number", "description": "边缘羽化程度" },
          "format": { "type": "string", "enum": ["png", "jpeg"] }
        }
      }
    },
    "required": ["image_base64", "mime_type"]
  }
}
```

Server 端处理逻辑：

```python
import base64
from mcp.server import Server
import mcp.types as types

server = Server("photoshop-server")

@server.call_tool()
async def handle_call_tool(name: str, arguments: dict) -> list[types.ContentBlock]:
    if name == "remove_background":
        # 解码 base64
        image_bytes = base64.b64decode(arguments["image_base64"])
        mime_type = arguments["mime_type"]

        # PS 处理
        result_bytes = process_remove_background(image_bytes)

        # 返回 base64 结果
        return [
            types.ImageContent(
                type="image",
                data=base64.b64encode(result_bytes).decode(),
                mimeType="image/png"
            )
        ]
```

### 完整数据流

```
User → Agent: "去掉这张照片的背景"
Agent: 读取本地文件 → base64 编码
Agent → MCP Server: JSON-RPC { "image_base64": "iVBORw0KGgo...", "mime_type": "image/png" }
MCP Server: base64 解码 → 处理 → base64 编码结果
MCP Server → Agent: { "type": "image", "data": "iVBORw0KGgo...", "mimeType": "image/png" }
Agent → User: 展示处理后的图片
```

---

## 方案二：文件中转服务（File URL）

### 接口设计

MCP Server 的 tool 参数定义为接收 file_url（或 staging_path）：

```json
{
  "name": "remove_background",
  "description": "移除图片背景，通过文件 URL 引用图片",
  "inputSchema": {
    "type": "object",
    "properties": {
      "file_url": {
        "type": "string",
        "format": "uri",
        "description": "图片文件的访问 URL，服务器将通过此 URL 下载文件"
      },
      "staging_path": {
        "type": "string",
        "description": "或使用服务器本地暂存路径（需预先上传到 Staging Service）"
      },
      "options": {
        "type": "object",
        "properties": {
          "feather": { "type": "number" },
          "format": { "type": "string", "enum": ["png", "jpeg"] },
          "result_callback_url": {
            "type": "string",
            "format": "uri",
            "description": "处理结果回调地址，大文件异步处理时使用"
          }
        }
      }
    },
    "required": ["file_url"]
  }
}
```

Server 端处理逻辑：

```python
import httpx
from mcp.server import Server
import mcp.types as types

server = Server("photoshop-server")

@server.call_tool()
async def handle_call_tool(name: str, arguments: dict) -> list[types.ContentBlock]:
    if name == "remove_background":
        file_url = arguments["file_url"]

        # 从 URL 下载文件
        async with httpx.AsyncClient() as client:
            response = await client.get(file_url)
            image_bytes = response.content

        # PS 处理
        result_bytes = process_remove_background(image_bytes)

        # 上传结果到文件服务器，返回 URL
        result_url = await upload_to_staging(result_bytes, "result.png")

        return [
            types.TextContent(
                type="text",
                text=json.dumps({
                    "status": "success",
                    "result_url": result_url,
                    "message": "处理完成，点击链接下载结果"
                })
            )
        ]
```

### 文件中转服务（Staging Service）设计

这是方案二的核心基础设施，一个独立于 MCP 的轻量 HTTP 文件暂存服务：

```
Staging Service API:
  POST   /upload          — 上传文件，返回 staging_path
  GET    /files/{name}    — 下载文件
  DELETE /files/{name}    — 删除文件
  GET    /files           — 列出所有暂存文件
  GET    /health          — 健康检查
```

```python
# Staging Service 核心逻辑（FastAPI 实现）
from fastapi import FastAPI, UploadFile, File
import uuid, os, json
from datetime import datetime

app = FastAPI()
STAGING_DIR = "/var/staging"
os.makedirs(STAGING_DIR, exist_ok=True)
MAX_AGE_HOURS = 24

@app.post("/upload")
async def upload_file(file: UploadFile = File(...)):
    ext = os.path.splitext(file.filename)[1]
    staging_name = f"{uuid.uuid4()}{ext}"
    staging_path = os.path.join(STAGING_DIR, staging_name)

    content = await file.read()
    with open(staging_path, "wb") as f:
        f.write(content)

    return {
        "staging_path": f"/staging/{staging_name}",
        "original_name": file.filename,
        "bytes": len(content),
        "expires_in_hours": MAX_AGE_HOURS
    }
```

### 完整数据流

```
User → Agent: "去掉这张照片的背景"
Agent → Staging Service: POST /upload (multipart form-data, 原始图片)
Staging Service → Agent: { "staging_path": "/staging/uuid.png" }
Agent → MCP Server: JSON-RPC { "file_url": "http://staging:3201/staging/uuid.png" }
MCP Server → Staging Service: GET /staging/uuid.png → 下载文件到服务器本地
MCP Server: 处理图片
MCP Server → Staging Service: POST /upload (处理结果)
Staging Service → MCP Server: { "staging_path": "/staging/result-uuid.png" }
MCP Server → Agent: { "result_url": "http://staging:3201/staging/result-uuid.png" }
Agent → Staging Service: GET /staging/result-uuid.png → 下载结果展示
```

---

## 两种方案适用边界评估

### 核心维度对比

| 维度 | Base64 内联 | 文件中转服务 |
|------|------------|------------|
| **文件大小** | < 1MB（最佳）;  | 无上限（受 staging 服务配置限制） |
| **传输开销** | ~33% 膨胀（base64 编码固有开销） | 无膨胀，原始二进制传输 |
| **延迟** | 单次往返，低延迟 | 多次往返（上传→处理→下载），延迟较高 |
| **内存压力** | 文件同时存在于 Agent、JSON-RPC 消息、Server 内存中 | 文件流式传输，内存友好 |
| **上下文窗口** | 大文件 base64 字符串撑爆 LLM 上下文 | 仅 URL 进入上下文，几乎不占空间 |
| **架构复杂度** | 极简，零额外依赖 | 需要独立 Staging Service + 生命周期管理 |
| **安全性** | 文件内容进入 JSON-RPC 日志，难以审计 | 可做访问控制、自动过期清理、病毒扫描 |
| **离线/本地** | 完美支持 stdio 传输 | 需网络可达的 staging 服务 |
| **多文件/批量** | 每个文件独立编码，消息可能超大 | 并发上传到 staging，tool 传 URL 列表 |
| **断点续传** | 不支持 | 可支持（staging 服务实现分块上传） |
| **结果缓存** | 不支持（每次都重新传） | staging 服务天然支持缓存和复用 |

### 适用场景矩阵

```
                    文件小 (< 1MB)
                         │
            ┌────────────┼────────────┐
            ▼            │            ▼
       本地部署      混合部署      远程部署
            │            │            │
            ▼            ▼            ▼
      ┌─────────┐  ┌──────────┐  ┌──────────┐
      │ Base64  │  │ Base64   │  │ Staging  │
      │ 内联     │  │ (小文件)  │  │ Service  │
      │ ★★★★★   │  │ +Staging │  │ ★★★★★   │
      └─────────┘  │ (大文件)  │  └──────────┘
                   │ ★★★★     │
                   └──────────┘

                    文件大 (> 10MB)
                         │
            ┌────────────┼────────────┐
            ▼            │            ▼
       本地部署      混合部署      远程部署
            │            │            │
            ▼            ▼            ▼
      ┌─────────┐  ┌──────────┐  ┌──────────┐
      │ Base64  │  │ Staging  │  │ Staging  │
      │ 分块上传 │  │ Service  │  │ Service  │
      │ ★★★     │  │ ★★★★★    │  │ ★★★★★   │
      └─────────┘  └──────────┘  └──────────┘
```

### 决策建议

1. **本地 stdio 模式 + 小文件** → Base64 内联，简单高效，零运维成本
2. **远程 HTTP/SSE 模式** → 文件中转服务，避免 base64 撑爆上下文窗口
3. **大文件（>10MB）** → 优先用文件中转服务，base64 编码后消息体过大，JSON-RPC 可能拒绝
4. **生产环境 / 多租户** → 文件中转服务 + S3 持久化 + 访问控制
5. **原型验证 / Demo** → Base64 内联，最快跑通

---

## DeepAgents 实现示例

以下使用 DeepAgents (LangChain) 分别实现两种方案的 Agent 端调用。

### 环境准备

```bash
pip install deepagents langchain-mcp-adapters httpx
```

### 方案一：Base64 内联 — Agent 端

```python
import base64
import asyncio
from pathlib import Path
from deepagents import create_deep_agent
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain.chat_models import init_chat_model


async def base64_approach():
    # 1. 连接 MCP Server
    client = MultiServerMCPClient({
        "photoshop": {
            "transport": "streamable_http",
            "url": "http://localhost:8000/mcp"
        }
    })
    tools = await client.get_tools()

    # 2. 创建 DeepAgent
    llm = init_chat_model("model_name", temperature=0)
    agent = create_deep_agent(
        model=llm,
        tools=tools,
        system_prompt="You are an image processing assistant."
    )

    # 3. Agent 调用时，读取本地文件并编码为 base64
    #    （实际由 agent 在 thinking 后自动发起 tool call）
    image_path = Path("/Users/user/photo.png")
    image_bytes = image_path.read_bytes()
    image_base64 = base64.b64encode(image_bytes).decode()
    mime_type = "image/png"

    # 4. 调用 MCP tool（DeepAgent 内部自动转换参数格式）
    result = await agent.ainvoke({
        "messages": [
            {
                "role": "user",
                "content": f"帮我把这张照片的背景去掉。\nimage_base64: {image_base64}\nmime_type: {mime_type}"
            }
        ]
    })

    return result


asyncio.run(base64_approach())
```

### 方案二：文件中转服务 — Agent 端

```python
import httpx
import asyncio
from pathlib import Path
from deepagents import create_deep_agent
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain.chat_models import init_chat_model


STAGING_SERVICE_URL = "http://localhost:3201"


async def upload_to_staging(file_path: Path) -> dict:
    """将文件上传到 Staging Service"""
    async with httpx.AsyncClient(timeout=60.0) as client:
        with open(file_path, "rb") as f:
            response = await client.post(
                f"{STAGING_SERVICE_URL}/upload",
                files={"file": (file_path.name, f, "application/octet-stream")}
            )
        return response.json()


async def download_result(staging_path: str, output_path: Path):
    """从 Staging Service 下载处理结果"""
    async with httpx.AsyncClient() as client:
        response = await client.get(f"{STAGING_SERVICE_URL}{staging_path}")
        output_path.write_bytes(response.content)


async def staging_approach():
    # 1. 上传文件到 Staging Service
    image_path = Path("/Users/user/photo.png")
    staging_result = await upload_to_staging(image_path)
    staging_path = staging_result["staging_path"]
    file_url = f"{STAGING_SERVICE_URL}{staging_path}"

    print(f"File staged at: {file_url}")

    # 2. 连接 MCP Server
    client = MultiServerMCPClient({
        "photoshop": {
            "transport": "streamable_http",
            "url": "http://localhost:8000/mcp"
        }
    })
    tools = await client.get_tools()

    # 3. 创建 DeepAgent
    llm = init_chat_model("model_name", temperature=0)
    agent = create_deep_agent(
        model=llm,
        tools=tools,
        system_prompt="You are an image processing assistant. "
                      "Use the file_url parameter to reference images "
                      "that have been pre-uploaded to the staging service."
    )

    # 4. 调用 Agent，传递 file_url
    result = await agent.ainvoke({
        "messages": [
            {
                "role": "user",
                "content": f"帮我把这张照片的背景去掉。图片已上传到暂存服务，"
                           f"file_url 为: {file_url}"
            }
        ]
    })

    # 5. 如果 MCP Server 返回了 result_url，下载结果
    #    （具体取决于 tool 返回格式）
    return result


asyncio.run(staging_approach())
```

### 进阶：封装 FileTransferMiddleware

为了复用文件传输逻辑，可以将 Staging Service 封装为 DeepAgent 可用的 tool：

```python
from langchain_core.tools import tool
from pathlib import Path
import httpx

STAGING_URL = "http://localhost:3201"

@tool
async def stage_file(file_path: str) -> str:
    """
    将本地文件上传到暂存服务，返回文件 URL。

    Args:
        file_path: 本地文件的绝对路径
    Returns:
        暂存服务中的文件 URL
    """
    path = Path(file_path)
    async with httpx.AsyncClient(timeout=60.0) as client:
        with open(path, "rb") as f:
            resp = await client.post(
                f"{STAGING_URL}/upload",
                files={"file": (path.name, f)}
            )
        data = resp.json()
        return f"{STAGING_URL}{data['staging_path']}"


@tool
async def download_result(file_url: str, save_path: str) -> str:
    """
    从暂存服务下载处理结果到本地。

    Args:
        file_url: 暂存服务中的文件 URL
        save_path: 本地保存路径
    Returns:
        保存后的本地文件路径
    """
    async with httpx.AsyncClient() as client:
        resp = await client.get(file_url)
        Path(save_path).write_bytes(resp.content)
        return save_path


# 将中转 tool 和 MCP tool 合并给 Agent
async def create_agent_with_staging():
    mcp_client = MultiServerMCPClient({
        "photoshop": {
            "transport": "streamable_http",
            "url": "http://localhost:8000/mcp"
        }
    })
    mcp_tools = await mcp_client.get_tools()

    # 合并 staging 工具和 MCP 工具
    all_tools = mcp_tools + [stage_file, download_result]

    llm = init_chat_model("model_name", temperature=0)
    return create_deep_agent(
        model=llm,
        tools=all_tools,
        system_prompt=(
            "You have access to image processing tools via MCP and file staging tools. "
            "To process an image: "
            "1. Use stage_file to upload the local image to the staging service "
            "2. Call the photoshop MCP tools with the returned file_url "
            "3. Use download_result to save the processed image locally"
        )
    )
```

---

## 拓展：SEP-1306 Binary Mode（MCP 协议演进方向）

MCP 官方社区已提出 **SEP-1306** 提案（2025年8月），旨在为 MCP 协议原生支持二进制文件上传。核心思路是 **客户端中介上传**：

### 能力声明

```json
{
  "capabilities": {
    "elicitation": {
      "binary": {
        "maxFileSize": 104857600,
        "supportedMimeTypes": ["image/*", "application/pdf"]
      }
    }
  }
}
```

### 文件请求 Schema

```json
{
  "mode": "binary",
  "requestedSchema": {
    "type": "object",
    "properties": {
      "imageFile": {
        "type": "file",           // 新增类型，非标准 JSON Schema
        "accept": ["image/*"],
        "maxSize": 5242880,
        "required": true
      }
    }
  },
  "uploadEndpoints": {
    "imageFile": {
      "url": "https://ps-server.example.com/mcp/upload/session-abc",
      "method": "POST",
      "uploadId": "550e8400-..."
    }
  }
}
```

### 流程

```
1. Server 声明需要 binary 文件 → 提供 upload endpoint
2. Client（Host）从用户文件系统读取文件
3. Client 直接 POST 二进制到 upload endpoint（不经过 LLM 上下文）
4. 仅 file_id / reference 传回 LLM 上下文
```

> 注意：截至 2026 年中，SEP-1306 仍为 Proposal 状态，尚未进入 MCP 正式规范。当前生产环境仍需使用 Base64 或 Staging Service 方案。

---

## 总结

| | Base64 内联 | 文件中转服务 | SEP-1306 (未来) |
|---|---|---|---|
| **适用文件** | < 1MB | 任意大小 | 任意大小 |
| **架构复杂度** | 极简 | 中等（需独立服务） | 低（协议原生支持） |
| **上下文污染** | 严重（大文件时） | 无 | 无 |
| **传输效率** | -33%（编码膨胀） | 100% | 100% |
| **成熟度** | 即刻可用 | 即刻可用（需自建） | 提案阶段 |
| **推荐场景** | 本地 / 小文件 / 原型 | 远程 / 大文件 / 生产 | 未来标准方案 |

**一句话建议**：原型和小文件用 Base64 内联快速验证，生产环境和大文件用文件中转服务，同时关注 SEP-1306 的进展作为长期标准方案。

---

## 参考资源

- [SEP-1306: Binary Mode Elicitation](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1306)
- [mcp-file-staging-service](https://github.com/danielrosehill/mcp-file-staging-service)
- [Multi-Modal MCP Servers (HackerNoon)](https://hackernoon.com/multi-modal-mcp-servers-handling-files-images-and-streaming-data)
- [MCP 官方规范](https://modelcontextprotocol.io/)
- [DeepAgents GitHub](https://github.com/langchain-ai/deepagents)
- [langchain-mcp-adapters](https://github.com/langchain-ai/langchain-mcp-adapters)
- [MCP Can't Upload Files — Production Workaround](https://dev.to/kenimo49/mcp-cant-upload-files-heres-what-i-learned-building-a-production-workaround-7c1)
