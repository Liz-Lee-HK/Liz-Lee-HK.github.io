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

**MCP（Model Context Protocol）** 是 Anthropic 于 2024 年底开源的一套标准化协议，旨在统一大语言模型与外部工具、数据源之间的交互方式。它被业界通俗地比喻为「AI 界的 USB-C」——简单来说就是一套AI与工具之间对话的语法。

在 MCP 出现之前，每个 LLM 应用都需要为不同的数据源和工具编写定制化的连接代码，形成 M×N （M是模型数，N是工具数）的集成矩阵。MCP 将其简化为 M+N 的架构：模型只需实现 MCP 客户端（模型的总和为客户端，M个模型做M个适配），工具只需实现 MCP 服务端（工具的总和为服务端，N个工具做N个适配），双方通过统一协议通信。

## 为什么需要 MCP？

### 一句话说明MCP的作用
可以让你的AI能调用外部工具/数据，真正做实事而不是单纯思考，让AI长出手。（帮你做日程规划，CC能帮你用figma生成webApp）

### MCP 之前的困境

传统的 LLM 工具调用（Function Calling）存在几个核心问题：

1. **碎片化**：各家大模型没有一套统一的Function Calling 格式
2. **缺乏发现机制**：开发者必须在代码里手动写死有哪些工具、接口地址、调用方式，模型运行时不知道环境里还有哪些可用工具。没有通用的「工具注册→查询→发现」流程，工具新增 / 下线都要改代码，模型无法动态感知。
3. **无标准化的资源访问**：访问各类资源没有统一接口范式。
。想要让模型读取文件、数据库、API，每种数据源需要单独集成
4. **缺乏权限与安全模型**：各家 LLM 协议都没定义：身份校验、调用权限、访问范围、操作审计、沙箱隔离等通用安全规则。权限控制、防越权、调用限流、日志审计全都要开发者从零手写，实现参差不齐，也容易出现安全漏洞。

### MCP 的核心价值

| 痛点 | MCP 解决方案 |
|------|-------------|
| 多模型适配 | 统一的 JSON-RPC 协议，与具体模型解耦 |
| 工具发现 | 服务端主动暴露工具列表（`tools/list`），客户端动态获取 |
| 资源访问 | 标准化的 Resources 抽象，统一文件、数据库、API 的读取方式 |
| 安全控制 | 传输层支持 OAuth 2.0、API Key 等认证机制 |
| 生态复用 | 社区共享的 MCP Server，一次构建、到处使用 |



## MCP 架构

MCP 的整体架构可以表示为以下层级关系：

```
Model Context Protocol (MCP) Architecture
├── 基础协议层
│   ├── 底层协议：JSON-RPC 2.0
│   ├── 会话模型：有状态持久会话
│   └── 核心目标：上下文交换 + 采样协调
│
├── 三大核心组件
│   ├── 1. MCP Host（主机）
│   │   ├── 角色：容器/调度中心
│   │   ├── 例子：Claude Desktop、VS Code、Cursor
│   │   └── 职责：管理客户端、控制权限、协调AI模型
│   │
│   ├── 2. MCP Client（客户端）
│   │   ├── 角色：通信实例（1:1连接Server）
│   │   ├── 运行位置：Host内部
│   │   └── 职责：收发请求、传递上下文、中转数据
│   │
│   └── 3. MCP Server（服务端）
│       ├── 角色：能力提供者
│       ├── 运行位置：独立进程/服务
│       └── 职责：暴露资源、提供工具、权限验证
│
├── 核心能力模型
│   ├── Resources：可读取的数据（文件、数据库、API）
│   ├── Tools：可执行的操作（查询、脚本、函数调用）
│   ├── Prompts：可复用的提示词模板
│   └── Sampling：客户端向模型发起生成请求
│
└── 关键特性
    ├── 清晰的安全边界（Host控制权限）
    ├── 解耦的架构（Host/Client/Server职责分离）
    ├── 统一的通信标准（解决M×N碎片化问题）
    └── 支持复杂多步骤AI工作流
```

### 三大核心组件详解

#### 1. MCP Host（主机）
Host 是用户直接交互的AI应用，如 Claude Desktop、VS Code + Cursor。它是整个 MCP 系统的容器与调度中心，内部管理一个或多个 MCP Client 实例，负责权限控制（决定 Client 能连哪些 Server）、UI 渲染和 AI 模型的协调调度。

#### 2. MCP Client（客户端）
Client 是 Host 内部的通信实例，与 Server 保持 **1:1 连接**。它不直接面向用户，而是作为 Host 与 Server 之间的消息中转站——收发 JSON-RPC 请求、传递上下文数据、中转工具调用结果。每个 Client 连接到特定的一个 Server。

#### 3. MCP Server（服务端）
Server 是能力的实际提供者，运行在独立进程或远程服务中。它暴露 Resources（数据）、Tools（操作）和 Prompts（模板），并负责权限验证。社区中已有数百个开箱即用的 MCP Server，覆盖文件系统、数据库、搜索引擎、云服务等场景。

### MCP通信过程举例

一个典型的 MCP 调用流程是这样的：
用户 → Host：在 Claude Desktop 里输入 “帮我读取我的 Notion 笔记并做个摘要”。
Host → Client：创建一个 MCP 客户端实例，连接到 Notion MCP Server。
Client → Server：发送请求，“请给我用户授权的 Notion 笔记内容”。
Server → Client：验证权限后，返回笔记的结构化数据。
Client → Host/AI 模型：把数据传给 Claude 模型。
AI 模型生成摘要 → Host → 用户：把摘要结果返回给用户，同时可以再通过客户端调用其他服务端工具（比如把摘要保存到文件）。

### 核心能力模型

MCP 为 Server 定义了四种可供暴露的能力：

#### Resources（资源）
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

#### Tools（工具）
模型可以**执行**的操作。类似 REST API 的 POST 端点，带有结构化的输入输出定义。

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

#### Prompts（提示模板）
预定义的 Prompt 模板，方便用户快速发起特定类型的对话。

#### Sampling（采样）
与前三者方向相反——Sampling 允许 Server 反向请求 Client 调用 LLM 完成生成任务。设计上也便于 Human-in-the-loop 审批流。

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
2. **三大组件 + 四大能力**：Host（调度中心）/ Client（通信实例）/ Server（能力提供）；Resources（读数据）、Tools（执行操作）、Prompts（模板）、Sampling（反向生成请求）
3. **与 Function Calling 的核心区别**：MCP 是模型无关的协议层抽象，Function Calling 是具体模型的 API 格式
4. **与 A2A 的区别**：MCP 连接工具和数据（外部能力），A2A 连接 Agent（多智能体协作），两者互补
5. **传输方式**：Stdio（本地低延迟）+ HTTP/SSE（远程分布式）

## 参考资源

- [MCP 官方规范](https://modelcontextprotocol.io/)
- [MCP GitHub 仓库](https://github.com/modelcontextprotocol)
- [Anthropic MCP 介绍博客](https://www.anthropic.com/news/model-context-protocol)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
