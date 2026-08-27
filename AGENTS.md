# Agent Skills 生成与维护指南

本仓库是个人技能集合。所有技能遵循 Agent Skills 目录规范：**一个技能 = `skills/{name}/` 一个目录**，入口为 `SKILL.md`，详细指令放 `references/`。

## 目录结构

```
.
├── AGENTS.md               # 本文件：生成与维护工作流
├── README.md               # 集合介绍与安装
└── skills/
    └── {skill-name}/       # 技能目录，kebab-case 命名
        ├── SKILL.md        # 技能入口（<100 行）
        ├── CHANGELOG.md    # 可选：版本历史
        ├── EXAMPLES.md     # 可选：示例
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

## 工作流三：从独立技能仓库导入

适用于「技能在独立 GitHub 仓库中开发成熟，收编进本集合仓库」的场景（收编模式：本仓库副本为真源，独立仓视为开发草稿）。

1. 用 `gh`（WSL 版）查看源仓库文件清单，确认技能结构与文件职责：
   ```
   gh repo view {owner}/{repo} --json name,description
   gh api repos/{owner}/{repo}/contents --jq '.[].path'
   ```
2. 只搬运行时文件到 `skills/{name}/`：
   - 必须：`SKILL.md`、`references/*.md`
   - 可选：`CONTEXT.md`（技能词汇表，便于未来维护）
   - 不搬：`package.json`、`README.md`、`.gitignore`、`docs/adr/` 等开发脚手架。
3. 规范化 frontmatter：
   - 保留 `name`、`description`、作者有意设置的字段（如 `disable-model-invocation`）。
   - 补充本仓库 `metadata`：`author`、`version`（日期格式 `YYYY.M.D`）、`source`（源仓库 URL）。
4. 在 README.md 技能表登记，必要时在根 CONTEXT.md 沉淀新词条。

## 维护约定

- **不引入**：scripts/、meta.ts、submodule、GENERATION.md 等重型机制。
- 更新技能时同步更新 CHANGELOG.md 与 SKILL.md 中的 `metadata.version`。
