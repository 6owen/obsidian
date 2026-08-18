---
title: ADB 自动化
type: project
aliases:
  - Termux ADB 自动化
tags:
  - llm-wiki
  - automation
sources:
  - "[[obsidian/日常/ADB钉钉打卡]]"
  - "[[obsidian/脚本/termux/2. 像素点击触发]]"
  - "[[obsidian/脚本/termux/3. 钉钉打卡]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# ADB 自动化

## 当前状态

已形成可运行方向：Termux/Python 在 Android 设备上通过无线 ADB 控制本机，执行定时点击流程，并用截图、日志或邮件反馈结果。

## 组成

- 环境：Android 11+、Termux、Python、android-tools。
- 连接：配对、无线连接、连接状态检测。
- 动作：亮屏、解锁、Home、启动应用、滑动、像素点击、截图。
- 调度：固定/随机时间、状态接口、防重复执行。
- 反馈：日志和邮件通知。

## 风险与缺口

- 原始脚本包含敏感配置，本 Wiki 不保存具体值；参见[[wiki/concepts/敏感信息治理]]。
- 像素坐标和固定等待时间对 UI 变化很脆弱。
- 缺少明确的失败重试上限、执行幂等和截图判定规则。
- 自动化使用应符合目标应用规则和实际授权范围。

## 下一步

- 先轮换并移除原文中的真实凭据。
- 把配置改为环境变量，建立 `.env.example`。
- 用状态识别替代纯坐标点击，增加失败截图和可观测日志。

