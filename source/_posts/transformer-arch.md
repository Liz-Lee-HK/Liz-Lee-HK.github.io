---
title: Transformer 架构八股复盘
date: 2026-06-06 14:00:00
categories: Study
tags:
  - Transformer
  - Attention
  - 八股
  - LLM
toc: true
---

## 整体架构

Transformer 是 Google 在 2017 年 "Attention Is All You Need" 中提出的 seq2seq 架构，完全基于自注意力机制，摒弃了 RNN 的循环结构。

### 核心组件

| 组件 | 作用 |
|------|------|
| Multi-Head Self-Attention | 让每个 token 关注序列中的所有 token |
| Feed-Forward Network (FFN) | 对每个 token 做非线性变换 |
| Layer Normalization | 稳定训练 |
| Residual Connection | 梯度流动、防止退化 |
| Positional Encoding | 注入位置信息 |

## Self-Attention 计算流程

给定输入 $X \in \mathbb{R}^{n \times d}$，计算如下：

1. **线性投影**：$Q = XW^Q$, $K = XW^K$, $V = XW^V$
2. **注意力分数**：$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right)V$

### 为什么除以 $\sqrt{d_k}$？

防止点积过大导致 softmax 梯度进入饱和区。$d_k$ 越大，点积的方差越大，除以 $\sqrt{d_k}$ 可保持方差为 1。

### Multi-Head 的意义

不同的 head 关注不同的子空间（语法 / 语义 / 位置关系等），拼接后做线性投影：

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W^O$$

## 面试要点

- **时间复杂度**：$O(n^2 d)$，主要瓶颈在 $QK^\top$
- **参数量**：$4d^2$（Q/K/V/O 各 $d^2$）
- **Post-LN vs Pre-LN**：Pre-LN 训练更稳定，现代 LLM（GPT、LLaMA）都用 Pre-LN
- **为什么要 Positional Encoding？** Attention 本身是置换不变的，需要显式注入位置

## 参考资料

- [Attention Is All You Need (2017)](https://arxiv.org/abs/1706.03762)
