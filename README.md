# My Agent Skills

个人专属 [Agent Skills](https://agentskills.io/home) 集合，参照 [Anthony Fu's Skills](https://github.com/antfu/skills) 的仓库结构与规范整理，保留其「一个技能 = 一个目录 + SKILL.md + references/」的输出结构，但去掉了 submodule 与 CLI 机制——本仓库全部为手写技能，结构极简。

## Installation

```bash
pnpx skills add makursi/my-agent-skills --skill='*'
```

## Skills

| Skill | Description |
|-------|-------------|
| [notes-refiner](skills/notes-refiner) | 将零散的计算机科学课堂笔记精炼为结构化、学术深度的知识库文档（5 阶段流水线） |

## Repository Structure

```
.
├── AGENTS.md               # 技能生成与维护工作流
├── README.md               # 本文件
└── skills/
    └── {skill-name}/       # 每个技能一个目录，kebab-case 命名
        ├── SKILL.md        # 技能入口（frontmatter + 概览 + 触发词）
        └── references/     # 按需加载的详细指令（渐进式披露）
```

## Adding a New Skill

手写新技能：在 `skills/{name}/` 下创建 `SKILL.md` + `references/`，frontmatter 需含 `name`、`description`（含 "Use when..." 触发词）、`metadata`。详见 [AGENTS.md](AGENTS.md)。

从文档生成技能：让 agent 参考 [AGENTS.md](AGENTS.md) 中的生成工作流，读取官方文档后按统一规范生成。

## License

MIT
