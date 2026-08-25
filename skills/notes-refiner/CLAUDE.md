# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working on this skill in the `skills/notes-refiner/` directory of the my-agent-skills repository.

## Project Overview

This is a Claude Code skill — a domain-specific instruction set loaded by Claude Code to perform specialized tasks. This skill refines scattered Computer Science & Technology `.md` classroom notes into a single structured knowledge base document with academic depth.

## Architecture

Skills use **progressive disclosure**: SKILL.md is the lightweight entry point (<=100 lines) that the agent reads to decide whether to load the skill. Detailed instructions live in `references/` and are read on-demand during execution.

```
skills/notes-refiner/
├── SKILL.md                  # Entry point: frontmatter (name + version + description) + workflow overview (~95 lines)
├── references/
│   ├── workflow.md           # Phase-by-phase execution guide with checklists
│   ├── output-schema.md      # Exact template for generated knowledge base documents
│   └── expansion-guide.md    # Gap taxonomy, academic depth standards, citation formats
├── EXAMPLES.md               # 3 annotated CS examples (input notes → output knowledge base)
└── CHANGELOG.md              # Version history
```

## Conventions

- **Frontmatter `description`**: max 1024 chars, third person, must include "Use when..." triggers. This is the ONLY thing the agent sees when deciding which skill to load.
- **SKILL.md**: keep under 100 lines. Split anything beyond into `references/*.md`.
- **Reference linking**: use relative paths like `[guide](references/guide.md)`.
- **No scripts needed**: this skill is purely instructional — all work is done by the AI following the reference files.
- **Skill is installed via CLI**: `pnpx skills add makursi/my-agent-skills --skill='*'`

## Scope

This skill serves **only** Computer Science & Technology topics (algorithms, data structures, OS, networks, databases, compilers, architecture, AI/ML, PL, software engineering, distributed systems, cryptography). Non-CS notes are rejected in Phase 1 with an explanation. Math/CS boundary domains (graph theory, information theory, etc.) trigger a notice to confirm the CS-centric expansion direction.

## Key Design Decisions

- **Flexible confirmation gates**: <=8 gaps → 2 prompts (topic + outline); >8 gaps → 3 prompts (topic + priorities + outline). User can say "auto-proceed" to skip all but the final outline.
- **Output**: 6-section document.
- **9-element cognitive lens system**: Every expanded concept follows a 5-element causal chain 问题 (Problem) → 资源 (Resource) → 抽象 (Abstraction) → 机制 (Mechanism) → 策略 (Strategy), with 4 analysis layers (权衡, 关系, 最小示例, 关键要点) embedded into the causal chain sections. 验证 (Verification) is an optional standalone subsection. This is a cognitive lens system, not a rigid template — analysis layers are embedded, not separate headings.
- **Expansion dimensions**: "Apply where applicable" — subfield-specific depth standards are the hard requirements.
- **No emoji**: Clean academic style throughout.
