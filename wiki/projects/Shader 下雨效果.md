---
title: Shader 下雨效果
type: project
aliases: []
tags:
  - llm-wiki
  - graphics
sources:
  - "[[obsidian/Shader/案例-创建下雨效果/1. 创建点、线、面]]"
  - "[[obsidian/Shader/案例-创建下雨效果/2. 创建背景贴图]]"
  - "[[obsidian/Shader/案例-创建下雨效果/3. 创建噪音背景]]"
  - "[[obsidian/Shader/案例-创建下雨效果/4. 将画布分割为10x10]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# Shader 下雨效果

## 当前状态

已有分阶段实验笔记，包含图元绘制、背景贴图、随机噪声和网格分割。尚未见到一篇汇总最终效果、参数说明和验收结果的项目复盘。

## 技术路径

1. 建立缓冲区并理解点、线、面的绘制。
2. 使用 Three.js 平面和 ShaderMaterial 加载背景纹理。
3. 通过二维伪随机函数生成噪声背景。
4. 用 `fract` 将画布拆成重复的 10×10 单元。
5. 后续应把时间 uniform、雨滴形状、速度、密度和层次组合成完整效果。

## 下一步

- 补一张最终效果截图或视频。
- 记录完整 vertex/fragment shader 与可调参数。
- 区分背景噪声、雨滴分布和运动动画各自的职责。
- 写出性能基线和移动端表现。

## 相关页面

- [[wiki/concepts/Shader 与 GLSL]]
- [[wiki/concepts/UV 坐标与纹理采样]]
- [[wiki/entities/Three.js]]

