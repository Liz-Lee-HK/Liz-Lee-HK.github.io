---
title: 从零实现CLIP多模态对比学习模型 — 图像-文本跨模态检索与零样本分类
date: 2026-06-10 08:00:00
categories: Projects
tags:
  - CLIP
  - Contrastive Learning
  - PyTorch
  - Multimodal
  - Transformer
  - Vision-Language
toc: true
---

## 项目概述

独立实现并训练了一个完整的CLIP（Contrastive Language-Image Pre-training）多模态对比学习模型。该项目涉及图像编码器与文本编码器的联合训练、对称对比损失函数的设计，以及零样本图像分类的评估流水线。

## 背景与动机

CLIP是OpenAI于2021年提出的里程碑式多模态模型，它首次证明了自然语言监督可以有效地训练视觉模型。理解和复现CLIP对于深入掌握多模态AI、对比学习以及视觉-语言对齐技术至关重要。本项目是我在HKU攻读期间对多模态学习的一次系统性实践。

## 技术方案

### 1. 双塔编码器架构

- **图像编码器**：分别实验了ResNet50（CNN架构）和Vision Transformer ViT-B/16（Transformer架构），使用预训练权重冻结，提取图像特征
- **文本编码器**：选用RoBERTa-base作为文本编码器，同样冻结预训练权重，提取文本语义表征
- **投影头**：在图像和文本编码器之上各自添加可训练的线性投影层，将两者映射到相同的嵌入空间

### 2. 对称对比损失

- 实现从零开始的对称对比损失函数（Symmetric Contrastive Loss）：同时计算图像→文本和文本→图像两个方向的cross-entropy loss，取平均值
- 这与InfoNCE loss同源，目标是最大化匹配的图-文对之间的余弦相似度，同时最小化不匹配对之间的相似度

### 3. 训练与评估流水线

- 在ImageNet-1K的50类子集上训练投影头参数
- 使用Hugging Face Trainer API管理训练循环，支持梯度累积、学习率预热、混合精度训练
- 零样本评估：在测试集上直接使用文本Prompt（「a photo of a {class_name}」）进行分类，无需任何fine-tuning

## 项目成果

- 成功实现CLIP模型在零样本设定下的图像分类，验证了对比学习在多模态对齐中的有效性
- 完成了CNN（ResNet50）和Transformer（ViT）两种图像编码器方案的对比实验
- 使用safetensors格式存储模型权重，支持高效加载与推断
- 模型可为每个测试图像输出Top-10预测结果，便于后续分析与可视化
