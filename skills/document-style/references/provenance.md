# 溯源注记（背景，不参与判定）

本文件的唯一用途：解释主规范（rules.md）为何这样定、外部指南与仓库规则的冲突如何裁决、哪些外部来源已失效。**不进入检查逻辑。** 需要"为什么"时才读本文件。

## 冲突裁决原则

外部指南之间存在差异，与仓库规则冲突时**一律以仓库规则为准**（rules.md 已固化），例外只通过例外清单（见 SKILL.md）表达。已知冲突点：

| 议题 | 仓库规则 | 外部指南差异 | 裁决 |
|------|---------|-------------|------|
| 引号 | 弯引号 `“ ”` / `‘ ’` | sparanoid 指北、TapTap 指南推荐直角引号 `「」` | 以仓库为准（弯引号）。两指南均声明该选择"为一致性调整，非官方强制" |
| 中文与数字间空格 | 可有可无，但全篇统一 | sparanoid 强制加空格 | 以仓库为准：允许两种写法，但禁止混用 |
| 全角数字 | 一律禁止 | sparanoid 允许设计稿/海报中极少量全角数字对齐 | 以仓库为准：一律半角 |
| 中文数词 | 无规则 | TapTap：前后皆中文且数字较小时用中文数词（"排行榜前十的游戏"） | 不强制；作为可选项保留在背景中 |

## 外部来源状态（12 个参考链接）

素材研究（`output/`）核实结果，供 Agent 引用时规避失效链接：

- ✅ **可直接获取**：sparanoid《中文文案排版指北》（github.com/sparanoid/chinese-copywriting-guidelines）、阮一峰《为什么文件名要小写？》、W3C CLREQ《中文排版需求》、GB 3100-1993《国际单位制及其应用》（zh.wikisource.org）、Google Developer Documentation Style Guide（developers.google.com/style）。
- ⚠️ **有替代来源**：华为《产品手册中文写作规范》（原 taodocs 链接不可靠，可用华为云社区转载 bbs.huaweicloud.com/blogs/377533 或人人文库转载）；DaoCloud 写作规范（原 guide.daocloud.io 改版，替代：github.com/DaoCloud/DaoCloud-docs 的 style_zh.md）；LeanCloud 文档风格指南（原 open.leancloud.cn 域名失效，替代：blog.taptap.dev/pages/chinese-copywriting-guide）；GB/T 15835-2011《出版物上数字用法》（原 moe.gov.cn PDF 不稳定，替代：国家标准全文公开系统 openstd.samr.gov.cn 或高校转载 PDF）。
- ❌ **不可用**：lengoo 简体中文规范指南（PDF 522）、豌豆荚文案风格指南（Google Docs 需登录，可参考博客园转载）、刘方《技术写作技巧在日汉翻译中的应用》（原 PDF 404，仅余学术引用）。

## 可自动化的工具（背景参考）

pangu.js 系列、autocorrect（Rust/Node/Python 等）可自动完成中英/中数间加空格。主规范不依赖它们；本技能 v1 为纯提示词实现，脚本化留作 v2 演进。
