---
title: WeeklyPub — Nature期刊论文聚合与展示平台
date: 2026-06-10 01:00:00
categories: Projects
tags:
  - Django
  - Vue
  - Web Scraping
  - Celery
  - Redis
  - Nature
toc: true
---

## 项目概述

一个自动化论文聚合Web应用，每周定时抓取Nature.com的最新研究论文，提取标题、摘要、链接和配图，并通过Django后端与Vue前端呈现为可浏览的论文摘要墙。

## 背景与动机

在北大担任技术员期间，团队每周需要追踪Nature等顶刊的最新发表动态。手动浏览官网效率低且容易遗漏。WeeklyPub将这一流程自动化——通过Celery定时任务在每周日晚8点自动抓取、去重、入库，团队只需打开展示页面即可浏览一周精选论文。

## 技术方案

- **定时抓取**：Celery Beat调度器每周日20:00触发爬虫任务，使用BeautifulSoup解析Nature文章列表页，逐篇抓取标题、摘要、DOI链接和配图URL
- **去重与存储**：基于文章URL唯一性进行去重，通过Django ORM写入MySQL数据库
- **前后端分离**：Django REST API提供文章列表接口，Vue 3 + Element Plus构建响应式前端，支持文章卡片浏览和搜索
- **任务队列**：Redis作为Celery的消息代理与结果后端

## 项目成果

- 稳定运行数月，累计聚合数百篇Nature研究论文
- 团队论文浏览效率提升显著（从手动浏览官网约30分钟 → 平台浏览约5分钟）
- 验证了Django + Celery + Redis技术栈在定时数据采集场景下的可靠性与可维护性
