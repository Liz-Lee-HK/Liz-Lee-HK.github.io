---
title: 大模型代码生成策略对比研究 — 从Baseline到Enhanced Self-Refine的推理优化
date: 2026-06-10 07:00:00
categories: Projects
tags:
  - Code Generation
  - LLM
  - Self-Refine
  - Prompt Engineering
  - HumanEval
  - Qwen
toc: true
---

## 项目概述

基于HumanEval基准（164个Python函数生成任务），系统性地研究并实现了四种大模型代码生成策略：Baseline（零样本）、Self-Refine（自我反思迭代）、Lever（多样本投票）以及Enhanced Self-Refine（增强型自我修正）。该项目通过严谨的实验设计，揭示了不同推理策略在准确率、Token消耗和运行时间之间的权衡关系。

## 背景与动机

代码生成是大语言模型最受关注的应用方向之一。然而，单次零样本生成往往存在准确率瓶颈——模型可能在边界条件、类型推断和逻辑细节上出错。如何通过推理阶段的策略优化来提升代码生成质量，是LLM应用落地的核心问题。

## 技术方案

### 1. 四种策略实现

- **Baseline（准确率96.34%）**：零样本生成，使用标准Prompt模板直接要求模型输出Python函数，采用贪心解码确保确定性输出
- **Self-Refine（准确率98.78%）**：迭代式自我修正循环（最多3轮），模型先生成初始代码，然后对自身代码进行自我评估，识别错误后生成修正版本
- **Lever（准确率100%）**：对每个任务生成3个候选样本，通过实际执行验证（execution-based verification）进行评分排序，选择最优结果；使用NumPy实现高效的排序与选择逻辑
- **Enhanced Self-Refine（准确率100%）**：在Self-Refine基础上引入错误分类机制（语法错误、运行时错误、逻辑错误），针对不同错误类型提供定向反馈；增加二次重试通道和超时保护

### 2. Token消耗与延时权衡分析

- 详细记录了四种策略在全部164个任务上的Token消耗量和运行时间
- Enhanced Self-Refine虽然达到100%准确率，但Token消耗约为Baseline的3倍
- Lever策略在准确率和资源消耗之间取得了最优平衡

## 项目成果

- 最佳策略（Lever & Enhanced Self-Refine）达到100%准确率
- 完成64页详细实验报告，包含方法对比、失败案例分析、Token经济学分析和改进建议
- 验证了execution-based verification作为LLM输出质量控制手段的有效性
- 为后续Agent系统中的代码生成模块提供了策略选择的经验依据
