# Agent Skills 生成与维护指南

本仓库是个人技能集合。所有技能遵循 Agent Skills 目录规范：**一个技能 = `skills/{name}/` 一个目录**，入口为 `SKILL.md`，详细指令放 `references/`。

## 目录结构

```
.
├── AGENTS.md               # 本文件：生成与维护工作流
├── README.md               # 集合介绍与安装
├── vendor/                 # 同步技能的源仓库 submodule
│   └── {skill-name}/
└── skills/
    └── {skill-name}/       # 技能目录，kebab-case 命名
        ├── SKILL.md        # 技能入口（<100 行）
        ├── SYNC.md         # 同步技能专属：源路径 + git SHA + 同步日期
        ├── CHANGELOG.md    # 可选：版本历史（手写技能）
        ├── EXAMPLES.md     # 可选：示例
        ├── CONTEXT.md      # 可选：技能词汇表
        └── references/     # 按需加载的详细指令
            └── *.md        # 每个概念/工作流一个文件
```

## SKILL.md 规范

```markdown
---
name: {skill-name}
description: >-
  第三人称描述，≤1024 字符，必须包含 "Use when..." 触发词（中英文均可）。
  这是 agent 决定是否加载该技能时唯一能看到的内容。
metadata:
  author: makursi
  version: "2026.1.1"        # 更新日期
  source: https://...        # 可选：若为从文档生成的技能，记录来源 URL
---

# {技能标题}

简洁概览。保持入口轻量（<100 行），超出部分拆到 references/。
```

- **渐进式披露**：`SKILL.md` 只做概览与索引，详细指令放 `references/*.md`，agent 按需读取。
- **Reference 链接**：使用相对路径 `[guide](references/guide.md)`。
- **Reference 分类前缀**（借鉴 [antfu/skills](https://github.com/antfu/skills)）：`references/` 文件名以类别前缀命名——`core-`（主干工作流/事实源）、`features-`（能力点）、`best-practices-`（质量标准）、`advanced-`（深水区）等，可按需新增类别；`SKILL.md` 的引用表按类别分节或加类别列。
- **命名**：技能目录与 `name` 均为 kebab-case。

## 工作流一：手写新技能

1. 创建 `skills/{name}/SKILL.md`，按上述规范写 frontmatter + 概览。
2. 拆分详细指令到 `references/*.md`，每个文件一个概念。
3. 在 `SKILL.md` 中用表格列出引用（参考 `skills/notes-refiner/SKILL.md` 的样式）。
4. 在 README.md 的技能表中登记。

## 工作流二：从文档生成技能

适用于「某个工具/框架没有现成技能，想从官方文档生成」的场景。

1. 确认工具没有官方维护的 Agent Skill（若有，优先同步而非生成）。
2. 读取官方文档，聚焦 agent 能力与实用模式：
   - 忽略用户指南、入门介绍、安装引导等 agent 训练数据已覆盖的内容。
   - 只保留 API 用法、配置选项、关键模式、边界行为。
3. 按上述 SKILL.md 规范生成，`metadata.source` 记录官方文档 URL 与生成日期。
4. 不建 submodule、不记录 git SHA——来源信息只存在于 frontmatter。

## 工作流三：从独立技能仓库同步

适用于「技能在独立 GitHub 仓库中开发成熟，收录进本集合仓库」的场景（同步模式：独立源仓库为技能更新的真源，本仓库只收录副本）。借鉴 [antfu/skills](https://github.com/antfu/skills) 的 submodule + SHA 追踪，但不引入其脚本工具链。

### 初次收录

1. 挂 submodule：`git submodule add https://github.com/{owner}/{repo} vendor/{name}`。
2. 确认源仓库 frontmatter 符合上述 SKILL.md 规范（含 metadata）；不符合则在源仓库修好后再收录。
3. 只搬运行时文件到 `skills/{name}/`：
   - 必须：`SKILL.md`、`references/*.md`
   - 可选：`CONTEXT.md`（技能词汇表）
   - 不搬：`package.json`、`README.md`、`.gitignore`、`docs/adr/` 等开发脚手架。
4. 创建 `skills/{name}/SYNC.md`，记录源路径、git SHA、同步日期。
5. 在 README.md 技能表登记，必要时在根 CONTEXT.md 沉淀新词条。

### 更新同步技能

1. `git submodule update --remote vendor/{name}` 拉取上游最新。
2. `git -C vendor/{name} diff {SYNC.md 中的旧 SHA}..HEAD` 查看上游变更。
3. 覆盖 `skills/{name}/` 的运行时文件，更新 SYNC.md 的 SHA 与日期。
4. **不在本仓库手改同步技能**——一切演进（含 frontmatter、references 分类前缀重命名）都在源仓库完成并推送，再同步过来。

### SYNC.md 格式

```markdown
# Sync Info

- **Source:** `vendor/{name}`（{源仓库 URL}）
- **Git SHA:** `{同步时的源仓库 HEAD SHA}`
- **Synced:** {YYYY.M.D}
```

## 工作流四：发布技能到独立仓

适用于「为本仓库**手写**的成熟技能建立独立 GitHub 仓库，供 `skills add` 单独安装」的场景（发布渠道模式：集合仓为真源，独立仓单向导出，见 `docs/adr/0001`）。已按工作流三收录的同步技能不适用本工作流——其独立仓即真源。

1. 独立仓命名 `skill-{name}`，public、MIT；另写独立仓 `README.md`（技能简介 + 安装命令 + 指回集合仓的真源说明）。
2. 只导出运行时文件：`SKILL.md`、`references/*.md`，可选 `EXAMPLES.md`、`CHANGELOG.md`；不导出 `CLAUDE.md`、`token-optimization.md` 等开发脚手架。
3. 导出前确保 frontmatter 符合本仓库规范，`metadata.source` 指向独立仓 URL。
4. 在根 README.md 技能表挂独立仓链接，必要时在根 CONTEXT.md 沉淀新词条。
5. 后续同步：技能在集合内发版后，手动把运行时文件覆盖到独立仓并 commit & push。不写同步脚本（见维护约定）。

## 维护约定

- **不引入**：scripts/、meta.ts 等脚本工具链——submodule + SYNC.md 手工维护即可（原「不引入 submodule」约定已被同步模式取代）。
- **手写技能**（无独立源仓库，如 notes-refiner）更新时同步更新 CHANGELOG.md 与 SKILL.md 中的 `metadata.version`；**同步技能**的版本与结构演进在源仓库完成，本仓库只按工作流三覆盖。
