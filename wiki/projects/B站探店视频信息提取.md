---
title: B站探店视频信息提取
type: project
aliases: []
tags:
  - llm-wiki
  - ai
  - video
sources:
  - "[[obsidian/技术/分析b站探店视频中的店铺/1. 思路]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# B站探店视频信息提取

## 目标

从探店视频中提取店铺名称，避免对所有视频帧无差别 OCR。

## 已选择的 MVP

1. 从字幕中用店、馆、餐厅、烧烤、咖啡等关键词筛选可能相关的片段。
2. 对对应时间点的视频画面截图。
3. 使用 PaddleOCR 等工具识别店招文字。
4. 让 AI 综合字幕和 OCR 结果，输出候选店名。

## 迭代方向

- 第一版可以人工完成 Gemini 字幕分析、截图和 OCR，验证流程是否有效。
- 关键词不足时再引入 TF-IDF、Word2Vec 或更强的文本分类。
- 后续需要定义评估集：店名召回率、误识别率、每分钟视频处理成本。

## 相关页面

- [[wiki/concepts/AI 辅助内容与产品工作流]]

