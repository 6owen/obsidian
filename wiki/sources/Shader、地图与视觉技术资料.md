---
title: Shader、地图与视觉技术资料
type: source
aliases: []
tags:
  - llm-wiki
  - graphics
  - gis
sources:
  - "[[obsidian/Shader/0. 笔记一]]"
  - "[[obsidian/3D地图制作/🍔 3D地形]]"
  - "[[obsidian/地图/1. 创建自己的地图]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# Shader、地图与视觉技术资料

## 摘要

这一组笔记形成了从 GPU 图形基础、GLSL/UV/纹理，到 WebGL/Three.js 案例，再到 GIS 数据处理、地图瓦片和 Blender 三维地形的连续学习链路。

## Shader 与 GLSL

- [[obsidian/Shader/0. 笔记一|Shader 学习入口]]记录 The Book of Shaders、LearnOpenGL 等学习来源。
- `Shader/八股/` 系列覆盖 Shader 定义、GLSL 类型与语法、变量限定符、uniform、顶点/片元着色器、UV 坐标和纹理采样。
- [[obsidian/Shader/八股/6. uv 坐标|UV 坐标]]与[[obsidian/Shader/八股/7. 在片元着色器中贴图|纹理采样]]是后续图形案例的关键基础。
- `Shader/噪音/` 下 5 篇当前为空白，只登记为待补充主题。
- SQL 子目录是零散数据库速记，不属于图形主线，但保留在本来源组：[[obsidian/Shader/SQL/0. Mac 上 MySQL]]、[[obsidian/Shader/SQL/1. 谷歌上 SQL 教程]]、[[obsidian/Shader/SQL/2. 小结]]。

## 下雨 Shader 案例

- [[obsidian/Shader/案例-创建下雨效果/1. 创建点、线、面]]：缓冲区与图元绘制。
- [[obsidian/Shader/案例-创建下雨效果/2. 创建背景贴图]]：Three.js 平面、ShaderMaterial 与纹理 uniform。
- [[obsidian/Shader/案例-创建下雨效果/3. 创建噪音背景]]及[[obsidian/Shader/案例-创建下雨效果/files/3. 创建噪音|files 版本]]：伪随机噪声。
- [[obsidian/Shader/案例-创建下雨效果/4. 将画布分割为10x10]]：使用 `fract` 划分重复网格。

## 地图与三维地形

- [[obsidian/地图/0. 开发笔记]]记录行政区划代码和边界数据来源。
- [[obsidian/地图/1. 创建自己的地图]]总结“给图片定义坐标系 → GDAL 切瓦片 → 前端按层级加载”的自定义地图流程。
- [[obsidian/地图/下载好看的带名称的和省界的地图]]记录地图素材获取方向。
- [[obsidian/3D地图制作/裁剪地图]]记录 DataV 边界数据导入 QGIS。
- [[obsidian/3D地图制作/🍔 QGIS基础操作]]记录 GIS、投影、坐标系、图层、高程数据等概念。
- [[obsidian/3D地图制作/🍔 Blender基础操作]]记录 Blender 视图、模式与对象操作。
- [[obsidian/3D地图制作/🍔 3D地形]]形成 QGIS 处理 DEM、重投影/拉伸，再由 Blender 置换、材质、相机、光照和渲染的完整工作流。

## 关联知识

- [[wiki/concepts/Shader 与 GLSL]]
- [[wiki/concepts/UV 坐标与纹理采样]]
- [[wiki/concepts/地图数据与三维地形工作流]]
- [[wiki/entities/Three.js]]
- [[wiki/entities/QGIS]]
- [[wiki/entities/Blender]]
- [[wiki/projects/Shader 下雨效果]]
- [[wiki/projects/自定义地图与三维地形]]
