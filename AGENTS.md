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

## 维护约定

- **不引入**：scripts/、meta.ts、submodule、GENERATION.md 等重型机制。
- 更新技能时同步更新 CHANGELOG.md 与 SKILL.md 中的 `metadata.version`。
