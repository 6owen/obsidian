---
publish: false
---

<!-- input: 年份日记、日记素材目录、日记模板与 weekly / publish 属性。
output: 文件组织、日记周记发布说明与 Log.life 统一分类与入口。
pos: 日记目录说明；更新时同步本注释和相关写作说明。 -->

> 一旦我所属的文件夹有所变化，请更新我。

日记和周记按年份保存，正文在 Obsidian 维护，Postly 从 GitHub 获取已发布内容。
日期笔记使用 `YYYY-MM-DD.md`；周记也使用日期命名，用 `weekly` 属性区分。

| 文件 | 地位 | 功能 |
| --- | --- | --- |
| `2023/` | 年份归档 | 从 Arvin 导入的 39 篇历史周记 |
| `2024/` | 年份归档 | 原有历史日记、从 Arvin 导入的 14 篇周记 |
| `2025/` | 年份归档 | 2025 年的生活文章 |
| `2026/` | 年份归档 | 2026 年的日记与周记 |
| `assets/` | 日记素材 | 日记与周记独立使用的图片、动图和视频 |
| `README.md` | 目录说明 | 写作与发布约定，不作为博客正文 |

日记模板位于 `obsidian/Obsidian 模板/1. 日记模板.md`，Daily notes 已指向该模板。
Daily notes 的新文件保存目录仍为 `obsidian/日记`，写完可归入对应年份。

- `weekly: false`：普通日记，发布到 Log.life；模板默认此值。
- `weekly: true`：周记，也发布到 Log.life；此属性只标记文章类型。
- `publish: true` 且 `draft: false`：允许网站发布；新日记默认不发布。

日记不需要填写 `collection`，普通日记和周记统一使用来源默认的 `log-life`。`weekly` 不覆盖 collection，也不改变 URL。
`title / date / slug` 在新日记中从文件名自动填入；日期形状的值必须保留引号。
已有文章的 slug 保留原值；修改 date、slug 或 collection 会改变文章 URL，切换 weekly 不改变 URL。旧 `/weekly/...` 链接永久跳转到对应的 `/log-life/...`。
`part / parent / status / updated` 保留原有笔记组织用途，不控制博客发布。

8 篇从 Postly 迁入的周记已由 `weekly-YYYYMMDD.md` 改名为 `YYYY-MM-DD.md`，其中 4 篇仍是草稿。
原仓库迁移副本和旧附件目录已清理，后续只编辑 Obsidian 原文。正文和已有发布状态保持不变。

提交并推送此仓库后，在 Postly 执行 `pnpm sync:content` 获取更新。
本机日记入口为 `http://localhost:3002/log-life`，统一展示已发布的普通日记与周记。

从本机 `arvin/blog/weekly` 导入的 53 篇周记按原始日期归档，时间为 2023-03-13 至 2024-05-13。
172 个历史素材与 2026 年周记封面统一保存在 `obsidian/日记/assets/`，另保留 1 张旧周记未引用原图，共 174 个文件；历史素材中的静态图经 Sharp 压缩，总体积由约 639 MB 降至约 141 MB；动图和视频保留原文件。
文章的实际图片与视频引用已改成本地相对路径，不再依赖旧图床。

年份目录中的正文和 `cover` 使用 `../assets/文件名` 引用日记素材；例如 `![](../assets/订婚了.webp)`。
Obsidian 新附件的全局默认位置仍为 `obsidian/assets`。日记素材归档到 `obsidian/日记/assets` 后，日记根目录使用 `assets/文件名`，年份目录使用 `../assets/文件名` 引用。
