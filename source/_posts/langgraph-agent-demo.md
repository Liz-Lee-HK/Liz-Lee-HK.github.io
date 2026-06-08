---
title: LangGraph 构建 ReAct Agent 实验
date: 2026-06-07 10:30:00
categories: Projects
tags:
  - LangGraph
  - Agent
  - ReAct
  - Demo
---

## 项目概述

用 LangGraph 搭建一个 ReAct 风格的 Agent，能调用搜索和计算两个工具完成多步推理。

项目地址：[GitHub (coming soon)](https://github.com/Liz-Lee-HK)

## 技术栈

- **LangGraph** — 状态图编排
- **OpenAI API** — LLM 推理
- **Tavily Search** — 网络搜索工具
- **Python 3.11+**

## 核心设计

### ReAct 循环

```
观察 → 思考 → 行动 → 观察 → 思考 → 行动 → ... → 最终回答
```

### LangGraph 状态图

1. `agent` 节点：LLM 决定下一步（调用工具 or 回答用户）
2. `tools` 节点：执行工具调用
3. 条件边：判断是否需要继续调用工具

### 关键代码片段

```python
from langgraph.graph import StateGraph, END

workflow = StateGraph(AgentState)
workflow.add_node("agent", agent_node)
workflow.add_node("tools", tool_node)
workflow.set_entry_point("agent")
workflow.add_conditional_edges("agent", should_continue)
workflow.add_edge("tools", "agent")
```

## 踩坑记录

- LangGraph 的 `interrupt` 机制在人工审批场景很好用，但要配合 `checkpointer` 使用
- Tool 的 schema 描述要写清楚，否则 LLM 容易用错参数
- 建议加 `max_iterations` 限制防止死循环

## TODO

- [ ] 加入 Memory（SqLite / Postgres checkpointer）
- [ ] 支持 MCP 协议的工具接入
- [ ] 前端对话界面
