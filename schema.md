---
title: 个人记忆系统结构规则
type: system
created: 2026-08-18
updated: 2026-08-18
---

# 个人记忆系统结构规则

## 两层内容

### 人类原始记忆层

`obsidian/` 保存人类原始记忆。Codex 默认只读取这一层，不改写其中的笔记和附件。

原来的 `obsidian/技术/`、`obsidian/AI 应用/`、`obsidian/Prompt/`、`obsidian/Shader/`、`obsidian/日记/`、`obsidian/随笔/` 等目录继续保持原样。整个 `obsidian/` 在逻辑上相当于 LLM Wiki 方法论中的 Raw，不要求再创建名为 `raw/` 的目录。

### AI 维护的 Wiki 层

`wiki/` 保存 Codex 从原始记忆中提炼和持续维护的页面：

- `wiki/sources/`：原始笔记的来源摘要。
- `wiki/concepts/`：概念、理论、方法和技术。
- `wiki/entities/`：人物、组织、产品和工具。
- `wiki/projects/`：个人项目、目标和推进状态。
- `wiki/decisions/`：重要决定、依据、结果与后续复盘。
- `wiki/synthesis/`：跨多篇笔记形成的综合分析、模式与时间线。

## 页面属性

Wiki 页面统一使用以下 Obsidian 属性：

```yaml
---
title: 页面标题
type: source | concept | entity | project | decision | synthesis
aliases: []
tags:
  - llm-wiki
sources: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: draft | reviewed
---
```

`sources` 使用 Vault 内原始笔记的 `[[双链]]`，不得把其他 AI 生成的 Wiki 页面伪装成原始来源。

## 页面内容

页面按需要包含：

- 摘要或当前状态
- 核心内容
- 时间变化
- 相关页面
- 原始来源
- 冲突、缺口与待核实问题

不为满足模板而生成空章节。

## 更新原则

1. 先搜索已有 Wiki，再决定更新旧页面还是创建新页面。
2. 一条来源可以更新多个 Wiki 页面，一个 Wiki 页面也可以引用多条来源。
3. 新旧记录不一致时，保留不同时间的说法及来源，不静默覆盖。
4. 事实、个人表述和 AI 推断必须明确区分。
5. 每次只修改受新记录影响的页面。
6. `wiki/log.md` 只追加，不改写旧记录。
7. 人工确认过的页面标记为 `status: reviewed`；后续更新不得删除人工内容。

## 排除范围

以下内容不是个人记忆来源，默认不参与摄入：

- `wiki/`
- `.obsidian/`
- `.git/`
- `.agents/`
- `.claude/`
- `.claudian/`
- `.copilot/`
- `.opencode/`
- `obsidian/.obsidian/`
- `obsidian/.git/`
- `obsidian/.agents/`
- `obsidian/.claude/`
- `obsidian/.copilot/`
- `obsidian/.opencode/`
- `obsidian/copilot/`
- `AGENTS.md`
- `purpose.md`
- `schema.md`
- `obsidian/README.md`

## 相关文件

- [[purpose|个人记忆系统目标]]
- [[wiki/index|Wiki 索引]]
- [[wiki/log|Wiki 变更日志]]
