---
publish: false
---

<!-- input: 年份日记、日记模板与 weekly / publish 属性。
output: 文件组织和日记、周记的发布说明。
pos: 日记目录说明；更新时同步本注释和相关写作说明。 -->

> 一旦我所属的文件夹有所变化，请更新我。

日记和周记按年份保存，正文在 Obsidian 维护，Postly 从 GitHub 获取已发布内容。
日期笔记使用 `YYYY-MM-DD.md`；周记也使用日期命名，用 `weekly` 属性区分。

| 文件 | 地位 | 功能 |
| --- | --- | --- |
| `2023/` | 年份归档 | 从 Arvin 导入的 39 篇历史周记及 assets |
| `2024/` | 年份归档 | 原有历史日记、从 Arvin 导入的 14 篇周记及 assets |
| `2025/` | 年份归档 | 2025 年的生活文章 |
| `2026/` | 年份归档 | 2026 年的日记、周记和附件 |
| `README.md` | 目录说明 | 写作与发布约定，不作为博客正文 |

日记模板位于 `obsidian/Obsidian 模板/1. 日记模板.md`，Daily notes 已指向该模板。
Daily notes 的新文件保存目录仍为 `obsidian/日记`，写完可归入对应年份。

- `weekly: false`：普通日记，发布到 Log.life；模板默认此值。
- `weekly: true`：周记，发布到 Weekly，只需切换这个开关。
- `publish: true` 且 `draft: false`：允许网站发布；新日记默认不发布。

日记不需要再填写 `collection`。兼容旧笔记时，显式 `weekly` 优先于 `collection`；不含 weekly 的其他笔记继续使用原分类。
`title / date / slug` 在新日记中从文件名自动填入；日期形状的值必须保留引号。
已有文章的 slug 保留原值，文件改名不改变已发布链接；之后修改 weekly、date 或 slug 会改变文章 URL。
`part / parent / status / updated` 保留原有笔记组织用途，不控制博客发布。

8 篇从 Postly 迁入的周记已由 `weekly-YYYYMMDD.md` 改名为 `YYYY-MM-DD.md`，其中 4 篇仍是草稿。
原仓库副本保持 `publish: false`，后续只编辑 Obsidian 原文。正文和已有发布状态保持不变。

提交并推送此仓库后，在 Postly 执行 `pnpm sync:content` 获取更新。
本机测试入口为 `http://localhost:3002/obsidian-test`。

从本机 `arvin/blog/weekly` 导入的 53 篇周记按原始日期归档，时间为 2023-03-13 至 2024-05-13。
172 个素材已保存在对应年份的 `assets/`，静态图经 Sharp 压缩，总体积由约 639 MB 降至约 141 MB；动图和视频保留原文件。
文章的实际图片与视频引用已改成本地相对路径，不再依赖旧图床。
