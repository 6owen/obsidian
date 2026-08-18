---
title: Web-to-API 包装构想
type: project
aliases:
  - Web-to-API wrapping
tags:
  - llm-wiki
  - ai
  - architecture
sources:
  - "[[obsidian/AI 应用/Web-to-API wrapping：]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# Web-to-API 包装构想

## 构想

由 API 服务调度多个已登录的浏览器环境，通过 Chrome DevTools Protocol 附着并驱动网页端 AI 服务。每个账号运行在隔离容器中，并通过 VNC/noVNC 完成人工登录和调试。

## 架构草图

客户端 → API Server → Scheduler/Router → 多个隔离的 Chrome 容器 → 目标网页。

## 关键设计点

- 人工登录一次并持久化独立用户数据目录。
- Playwright 只通过 CDP 接管已登录浏览器。
- 多账号需要隔离、容量调度、会话粘性、健康检查和故障恢复。

## 风险与待核实

- 需要核对目标服务的使用条款、自动化限制和账号风险。
- 网页结构变化会使自动化不稳定。
- 登录态、Cookie 和调试端口都是高敏感资产，不能暴露到公网。
- 目前是架构想法，未见正式实现和验证记录。

