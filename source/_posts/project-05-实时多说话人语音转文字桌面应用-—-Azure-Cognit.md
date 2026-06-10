---
title: 实时多说话人语音转文字桌面应用 — Azure Cognitive Services集成
date: 2026-06-10 05:00:00
categories: Projects
tags:
  - Speech-to-Text
  - Azure Cognitive Services
  - Python
  - Real-Time
  - Cross-Platform
  - tkinter
toc: true
---

## 项目概述

一款跨平台桌面应用，支持在会议或听证会场景中同时对多说话人进行实时语音识别与翻译。用户可为每位说话人独立配置音频输入设备和目标语言，语音流经Azure Cognitive Services实时转写后，通过WebSocket传输至翻译服务完成多语输出。

## 背景与动机

在法律和企业会议场景中，多说话人的发言往往交织在一起，传统的单通道语音识别无法区分不同说话人，更无法实现说话人维度的多语翻译。需要一套能动态管理说话人、独立配置语言对、并实时展示转写与翻译结果的工具。

## 技术方案

- **Python + tkinter GUI**：轻量级桌面UI，支持说话人动态增删、音频设备独立绑定
- **Azure Cognitive Services Speech SDK**：每说话人独立实例化Recognizer，实现多路并发语音识别
- **sounddevice跨平台音频采集**：兼容Windows、macOS、Linux的底层音频I/O
- **WebSocket实时通信**：与eBRAM翻译微服务对接，将识别文本实时发送至翻译流水线
- **多语言支持**：每位说话人可配置多组语言对（如粤语→英语、普通话→英语）

## 项目成果

- 支持最多8个说话人同时在线的实时语音识别
- 端到端延迟控制在2秒以内（语音输入 → 翻译文本输出）
- 支持Windows/macOS/Linux三平台部署
- 为eBRAM在线仲裁平台提供语音输入端的完整解决方案
