---
title: 边坡卫士 — 基于卫星遥感与机器学习的地质灾害智能监测平台
date: 2026-06-10 03:00:00
categories: Projects
tags:
  - Remote Sensing
  - Machine Learning
  - PyTorch
  - Google Earth Engine
  - GIS
  - Spring Boot
  - Vue
toc: true
---

## 项目概述

「边坡卫士（Slope Warrior）」是一个面向地质工程领域的智能监测Web平台。系统整合卫星遥感数据（Sentinel-2）与地面监测数据，利用机器学习模型反演植被覆盖、冠层含水量、生物量等关键生态参数，为边坡稳定性评估与地质灾害预警提供数据驱动的决策支持。

## 背景与动机

传统边坡监测依赖人工巡检和离散传感器，成本高、覆盖范围有限。卫星遥感技术能以近乎零成本实现大范围的连续监测，但原始遥感数据需要经过复杂的反演算法才能转化为可用的生态指标。本项目试图将遥感科学、机器学习与Web工程结合，打造一个从数据采集到分析可视化的全链路平台。

## 技术方案

### 1. 遥感反演算法

- 基于Sentinel-2多光谱卫星影像，计算NDVI（归一化植被指数）、EVI等植被指标
- 使用Google Earth Engine API进行大规模遥感数据的自动化获取与预处理
- 实现GLCM（灰度共生矩阵）纹理分析，提取图像纹理特征辅助边坡分类

### 2. 机器学习建模

- 利用PyTorch构建回归模型，从遥感数据中反演生物物理参数：LAI（叶面积指数）、FVC（植被覆盖率）、Cab（叶绿素含量）、CWC（冠层含水量）、FAPAR（光合有效辐射吸收比例）
- 基于scikit-learn构建生物量反演与碳储量估算模型
- 使用rasterio + GDAL进行GeoTIFF栅格数据的读取、处理和可视化

### 3. 全栈Web平台

- **后端**：Spring Boot 3 + MyBatis-Plus + MySQL + Redis，JWT认证
- **算法服务**：Python Flask独立部署，提供遥感反演与模型推理的REST API
- **前端**：Vue 3 + Element Plus + ECharts，交互式地图与图表可视化
- **运维**：Docker容器化部署，Nginx反向代理，PM2进程守护

## 项目成果

- 实现了基于Sentinel-2影像的全自动植被指标计算与生物量反演流水线
- Web平台支持边坡基础信息管理、遥感数据分析、监测预警的一站式操作
- 利用百度AI开放平台的图像识别能力辅助边坡破坏类型分类
- 项目架构支持多区域扩展，可灵活接入新的遥感数据源与算法模型
