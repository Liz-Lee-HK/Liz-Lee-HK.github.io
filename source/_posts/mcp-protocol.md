---
title: MCP（Model Context Protocol）模型上下文协议详解
date: 2026-06-10 15:00:00
categories: Study
tags:
  - MCP
  - Agent
  - 协议
  - LLM
  - Anthropic
  - 工具调用
toc: true
---

## 什么是 MCP？

**MCP（Model Context Protocol）** 是 Anthropic 于 2024 年底开源的一套标准化协议，旨在统一大语言模型与外部工具、数据源之间的交互方式。它被业界通俗地比喻为「AI 界的 USB-C」——正如 USB-C 统一了设备的物理连接标准，MCP 试图统一 LLM 与外部世界的通信标准。

在 MCP 出现之前，每个 LLM 应用都需要为不同的数据源和工具编写定制化的连接代码，形成 M×N 的集成矩阵。MCP 将其简化为 M+N 的架构：模型只需实现 MCP 客户端，工具只需实现 MCP 服务端，双方通过统一协议通信。

## 为什么需要 MCP？

### MCP 之前的困境

传统的 LLM 工具调用（Function Calling）存在几个核心问题：

1. **碎片化**：OpenAI、Anthropic、Google 各自的 Function Calling / Tool Use 格式各不相同，切换模型意味着重写工具定义
2. **缺乏发现机制**：模型无法主动「发现」可用的工具，依赖开发者预先硬编码工具列表
3. **无标准化的资源访问**：想要让模型读取文件、数据库、API，每种数据源需要单独集成
4. **缺乏权限与安全模型**：工具调用的权限控制完全依赖开发者自行实现

### MCP 的核心价值

| 痛点 | MCP 解决方案 |
|------|-------------|
| 多模型适配 | 统一的 JSON-RPC 协议，与具体模型解耦 |
| 工具发现 | 服务端主动暴露工具列表（`tools/list`），客户端动态获取 |
| 资源访问 | 标准化的 Resources 抽象，统一文件、数据库、API 的读取方式 |
| 安全控制 | 传输层支持 OAuth 2.0、API Key 等认证机制 |
| 生态复用 | 社区共享的 MCP Server，一次构建、到处使用 |

## MCP 架构

MCP 采用经典的 **Client-Server** 架构：

```
┌──────────────┐     JSON-RPC      ┌──────────────┐
│  MCP Client  │ ◄──────────────► │  MCP Server  │
│  (LLM Host)  │   over stdio/    │  (Tool/Data) │
│              │   HTTP+SSE       │              │
└──────────────┘                  └──────────────┘
```

### 三种核心原语（Primitives）

MCP 定义了三种资源抽象：

#### 1. Resources（资源）
模型可以**读取**的上下文数据。类似 REST API 的 GET 端点。

```
// 客户端请求资源列表
{ "method": "resources/list" }

// 服务端返回
{ "resources": [
    { "uri": "file:///docs/report.pdf", "name": "年报" },
    { "uri": "postgres://db/sales", "name": "销售数据库" }
  ]
}
```

#### 2. Tools（工具）
模型可以**执行**的操作。类似 REST API 的 POST 端点，但带有结构化的输入输出定义。

```json
{
  "name": "search_web",
  "description": "搜索互联网获取最新信息",
  "inputSchema": {
    "type": "object",
    "properties": {
      "query": { "type": "string", "description": "搜索关键词" }
    },
    "required": ["query"]
  }
}
```

#### 3. Prompts（提示模板）
预定义的 Prompt 模板，方便用户快速发起特定类型的对话。

### 传输层

MCP 支持两种传输方式：
- **Stdio（标准输入输出）**：本地进程通信，延迟最低，适用于本地工具
- **HTTP + SSE（Server-Sent Events）**：远程通信，适用于云端服务和分布式部署

## MCP vs. 其他协议对比

### MCP vs. Function Calling

| 维度 | Function Calling | MCP |
|------|-----------------|-----|
| 定义方式 | 模型特定（OpenAI/Anthropic 格式各异） | 统一 JSON Schema |
| 工具发现 | 硬编码开发商定 | 动态发现 `tools/list` |
| 资源抽象 | 无，每次需手动处理上下文 | Resources 原语统一抽象 |
| 生态 | 各自为战 | 社区共建，开源 Server 库 |
| 模型耦合 | 强耦合 | 松耦合 |

### MCP vs. A2A（Agent-to-Agent）

Google 提出的 **A2A（Agent-to-Agent Protocol）** 常与 MCP 对比：

| 维度 | MCP | A2A |
|------|-----|-----|
| 定位 | 模型 ↔ 工具/数据的连接 | Agent ↔ Agent 的协作 |
| 提出方 | Anthropic | Google |
| 核心场景 | 单 Agent 使用外部能力 | 多 Agent 之间的任务委派 |
| 关系 | 互补，非竞争 | 互补，非竞争 |

> 简单理解：MCP 是 Agent 的「手和眼」，让 Agent 能操作外部世界；A2A 是 Agent 的「嘴和耳」，让 Agent 之间能沟通协作。

## 实战：MCP Server 开发示例

以下是一个最小化的 Python MCP Server，暴露一个计算器工具：

```python
from mcp.server import Server, NotificationOptions
from mcp.server.models import InitializationCapabilities
import mcp.types as types

# 创建 MCP Server
server = Server("calculator-server")

@server.list_tools()
async def handle_list_tools() -> list[types.Tool]:
    return [
        types.Tool(
            name="calculate",
            description="执行数学表达式计算",
            inputSchema={
                "type": "object",
                "properties": {
                    "expression": {
                        "type": "string",
                        "description": "数学表达式，如 '2 + 3 * 4'"
                    }
                },
                "required": ["expression"]
            }
        )
    ]

@server.call_tool()
async def handle_call_tool(
    name: str, arguments: dict
) -> list[types.TextContent]:
    if name == "calculate":
        result = eval(arguments["expression"])
        return [types.TextContent(type="text", text=str(result))]
    raise ValueError(f"Unknown tool: {name}")

async def main():
    async with mcp.server.stdio.stdio_server() as (read_stream, write_stream):
        await server.run(
            read_stream,
            write_stream,
            InitializationCapabilities(
                sampling={},
                experimental={},
                roots={}
            ),
            notification_options=NotificationOptions()
        )

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

## MCP 生态现状

截至 2026 年中，MCP 生态已相当成熟：

- **官方 SDK**：Python、TypeScript、Java、Kotlin 均已支持
- **社区 Server**：数百个开源 MCP Server，覆盖数据库（PostgreSQL、SQLite）、文件系统、搜索引擎（Brave、Exa）、项目管理（GitHub、Jira）、云服务（AWS、GCP）等
- **框架集成**：LangChain、LlamaIndex、CrewAI 等主流 Agent 框架均已支持 MCP
- **模型支持**：Claude Desktop、Cursor、Continue 等 AI 应用原生支持 MCP

## 面试要点

1. **一句话定义**：MCP 是 Anthropic 提出的开源标准协议，统一 LLM 与外部工具/数据的交互方式
2. **三大原语**：Resources（读数据）、Tools（执行操作）、Prompts（模板）
3. **与 Function Calling 的核心区别**：MCP 是模型无关的协议层抽象，Function Calling 是具体模型的 API 格式
4. **与 A2A 的区别**：MCP 连接工具和数据（外部能力），A2A 连接 Agent（多智能体协作），两者互补
5. **传输方式**：Stdio（本地低延迟）+ HTTP/SSE（远程分布式）

## 参考资源

- [MCP 官方规范](https://modelcontextprotocol.io/)
- [MCP GitHub 仓库](https://github.com/modelcontextprotocol)
- [Anthropic MCP 介绍博客](https://www.anthropic.com/news/model-context-protocol)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
