---
title: Shader 与 GLSL
type: concept
aliases:
  - 着色器
  - GLSL
tags:
  - llm-wiki
  - graphics
sources:
  - "[[obsidian/Shader/八股/1. 介绍]]"
  - "[[obsidian/Shader/八股/2. 基本数据类型、复合数据类型和结构体]]"
  - "[[obsidian/Shader/八股/3. 语法]]"
  - "[[obsidian/Shader/八股/4. 基础用法]]"
  - "[[obsidian/Shader/八股/5. 变量定义]]"
  - "[[obsidian/Shader/八股/8. 理解顶点着色器和片元着色器]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# Shader 与 GLSL

Shader 是在 GPU 上并行执行的图形程序。笔记的核心区分是：顶点着色器负责顶点位置等逐顶点计算，片元着色器负责屏幕片元的颜色等逐片元计算。

## 知识骨架

- GLSL 采用类似 C 的语法，包含 `int`、`float`、`bool`，以及 `vec`、`mat`、数组、sampler、struct 等复合类型。
- `attribute`/输入变量传入逐顶点数据，`uniform` 向多次着色调用提供共享值，顶点输出可在光栅化后插值为片元输入。
- 常见内建项包括顶点阶段的 `gl_Position`、片元颜色输出，以及纹理采样函数。
- GPU 的价值来自对大量顶点或片元执行相同程序；调用次数取决于几何顶点量和覆盖的片元量。
- 时间、分辨率、鼠标、纹理等可通过 uniform 驱动动画和交互。

## 学习顺序

GLSL 类型与语法 → 顶点/片元管线 → 坐标与插值 → uniform → 纹理采样 → 形状/矩阵/重复图案 → 随机与噪声 → 完整视觉案例。

## 相关页面

- [[wiki/concepts/UV 坐标与纹理采样]]
- [[wiki/entities/Three.js]]
- [[wiki/projects/Shader 下雨效果]]

