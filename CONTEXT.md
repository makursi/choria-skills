# Agent Skills Collection

A personal repository of AI agent skills following the [Agent Skills](https://agentskills.io/home) directory specification. Each skill is a self-contained instruction set that an AI agent loads on-demand to perform a specialized task.

## Language

**Skill**:
A self-contained instruction set loaded by an AI agent to perform a specialized task. One skill = one directory under `skills/`, entry point is `SKILL.md`.
_Avoid_: plugin, extension, tool, module

**SKILL.md**:
The entry-point file of a skill. Contains YAML frontmatter (name, description, metadata) and a lightweight overview (<100 lines). This is the only file the agent reads when deciding whether to load the skill.
_Aavoid_: entry.md, main.md

**Frontmatter**:
YAML metadata block at the top of `SKILL.md` that controls skill discovery and loading. Required fields: `name`, `description` (≤1024 chars, third-person, must include "Use when..." triggers). Optional: `metadata` (author, version, source).
_Avoid_: header, metadata block

**Progressive disclosure**:
The design pattern where `SKILL.md` provides a lightweight overview and `references/` provides detailed instructions loaded on-demand by the agent during execution. Keeps entry points small and context windows efficient.
_Avoid_: lazy loading, on-demand loading

**Reference**:
A detailed instruction file inside `references/`, one concept per file. Linked from `SKILL.md` via relative paths. Agents read these only when the workflow reaches that concept.
_Aavoid_: doc, guide, appendix

**Domain gate**:
A constraint that limits a skill to a specific domain. The skill checks input early and rejects out-of-scope content with an explanation rather than attempting partial work.
_Avoid_: scope check, filter

**Cognitive lens system**:
The structured framework a skill uses to organize expanded knowledge. For example, notes-refiner uses a 5-element causal chain (问题→资源→抽象→机制→策略) with embedded analysis layers. Not a rigid template — a thinking model.
_Aavoid_: template, output format, schema

**Gap taxonomy**:
The classification system a skill uses to identify missing or incomplete knowledge in input materials. Each gap type prescribes a specific expansion approach.
_Aavoid_: gap types, classification

**Knowledge base**:
The final output document produced by a skill. In notes-refiner: a single structured Markdown file with metadata, table of contents, concept map, and hierarchical chapters.
_Aavoid_: output, result, document

**Skill directory**:
The `skills/{skill-name}/` directory containing all files for one skill. Name is kebab-case. Contains `SKILL.md` (required), `references/` (optional), `CHANGELOG.md` (optional), `EXAMPLES.md` (optional).
_Aavoid_: skill folder, skill path

**Ke-bab-case**:
Naming convention for skill directories and the `name` field in frontmatter. Lowercase words separated by hyphens (e.g., `notes-refiner`, `code-review`).
_Avoid_: snake_case, camelCase, PascalCase

**技能源仓库 (source skill repo)**:
技能在独立 GitHub 仓库中开发成熟的来源（如 `makursi/skill-document-style`），是本集合导入流程的原料。来源信息只记在 `SKILL.md` 的 `metadata.source`。
_Avoid_: 上游仓库、外部仓库

**导入 (import)**:
从技能源仓库挑选运行时文件（`SKILL.md`、`references/*.md`，可选 `CONTEXT.md`）迁入 `skills/{name}/`，规范化 frontmatter 并在 README/根 CONTEXT.md 登记的过程。见 AGENTS.md 工作流三。
_Avoid_: 安装、同步（安装指 `skills add` 拉取安装，同步指镜像模式的持续更新，均非导入）

**收编模式 (adoption model)**:
本集合仓库副本为真源、独立源仓库视为开发草稿的治理方式；导入后以本仓库副本为准，不再回同步。
_Avoid_: 镜像模式（独立仓为真源、本仓库定期快照）
