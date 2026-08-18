---
title: UV 坐标与纹理采样
type: concept
aliases:
  - UV
  - Texture Sampling
tags:
  - llm-wiki
  - graphics
sources:
  - "[[obsidian/Shader/八股/6. uv 坐标]]"
  - "[[obsidian/Shader/八股/7. 在片元着色器中贴图]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# UV 坐标与纹理采样

UV 是贴图空间中的二维坐标，通常归一化到 `[0, 1]`。它把模型或画布上的位置映射到纹理图像上的取样位置。

## 核心关系

- 顶点携带 UV，顶点着色器将其传给片元阶段；光栅化会在三角形内部插值。
- 片元着色器使用 sampler 和 UV 读取纹理颜色。
- 修改 UV 而不修改几何，可以实现平移、缩放、重复、镜像、扭曲和程序化图案。
- `fract` 可把连续坐标压回 `[0,1)`，从而产生网格重复；这也是下雨案例划分 10×10 单元的基础。
- 坐标原点和 Y 轴方向可能因图形 API、图片格式或框架而不同，组合前要明确约定。

## 相关页面

- [[wiki/concepts/Shader 与 GLSL]]
- [[wiki/projects/Shader 下雨效果]]

