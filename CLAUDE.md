# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code skill — a domain-specific instruction set loaded by Claude Code to perform specialized tasks. This skill refines scattered Computer Science & Technology `.md` classroom notes into a single structured knowledge base document with academic depth.

## Architecture

Skills use **progressive disclosure**: SKILL.md is the lightweight entry point (≤100 lines) that the agent reads to decide whether to load the skill. Detailed instructions live in `references/` and are read on-demand during execution.

```
SKILL.md                      # Entry point: frontmatter + workflow overview (94 lines)
references/
  workflow.md                 # Phase-by-phase execution guide with checklists
  output-schema.md            # Exact template for generated knowledge base documents
  expansion-guide.md          # Gap taxonomy, academic depth standards, citation formats
EXAMPLES.md                   # 3 annotated CS examples (input notes → output knowledge base)
```

## Conventions

- **Frontmatter `description`**: max 1024 chars, third person, must include "Use when..." triggers. This is the ONLY thing the agent sees when deciding which skill to load.
- **SKILL.md**: keep under 100 lines. Split anything beyond into `references/*.md`.
- **Reference linking**: use relative paths like `[guide](references/guide.md)`.
- **No scripts needed**: this skill is purely instructional — all work is done by the AI following the reference files.
- **Skill directory must be symlinked** from `C:\Users\29634\.claude\skills\notes-refiner` to this project directory.

## Skill Installation

Skills are loaded from `C:\Users\29634\.claude\skills\`. A symlink is required:

```bash
ln -s "C:/Users/29634/Desktop/Note-Refiner-SKill" "C:/Users/29634/.claude/skills/notes-refiner"
```

After symlinking, Claude Code picks up the skill on next launch (or refresh). No build step, no package manager.

## Scope

This skill serves **only** Computer Science & Technology topics (algorithms, data structures, OS, networks, databases, compilers, architecture, AI/ML, PL, software engineering, distributed systems, cryptography). Non-CS notes are rejected in Phase 1 with an explanation.
