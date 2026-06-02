---
name: notes-refiner
description: >-
  Transform scattered Computer Science & Technology Markdown notes into a single
  academically-deep, hierarchically-structured knowledge base document with
  Chinese-English bilingual CS terminology, algorithm analysis, code examples,
  and textbook/paper references. Serves ONLY computer science topics: algorithms,
  data structures, OS, networks, databases, compilers, architecture, AI/ML, PL,
  software engineering, distributed systems, cryptography, and related CS fields.
  Use when user wants to refine/expand/organize CS notes, build a CS knowledge
  base from .md notes, create structured study materials from classroom notes, or
  mentions 笔记精炼, 笔记整理, knowledge expansion, note refinement, knowledge
  structuring, CS study notes, 知识体系化, 知识扩展, or 课堂笔记整理.
---

# 笔记精炼师 (Notes Refiner)

Transform scattered CS classroom notes into an academically-deep, hierarchically
structured knowledge base. **CS domain only** — if notes are not Computer Science
related, inform the user and stop.

## Quick Start

1. Tell the skill where your `.md` notes live (directory path or specific files).
2. Provide the CS topic if not obvious from the notes (e.g., "操作系统内存管理").
3. The skill proceeds through 5 phases, confirming major decisions with you.
   Say "自动继续" at any point to skip confirmations for the rest of the run.

## Workflow Overview

Proceed through these 5 phases in order. Do not skip phases. Present intermediate
results for user confirmation between phases unless the user opted to auto-proceed.

### Phase 1 — Ingest & Parse
- Glob/read all `.md` files from the specified path.
- **Domain gate**: If content is clearly NOT Computer Science, inform the user
  this skill only serves CS topics and stop.
- Extract headings, CS terminology, code blocks, formulas, and build a flat
  concept inventory.
- Ask for topic confirmation if ambiguous.

### Phase 2 — Analyze & Identify Gaps
- Classify every concept: Defined / Mentioned-but-unexplained / Assumed-background
  / Orphaned.
- Assess depth level (L1=definition only → L4=full academic treatment).
- Identify CS-specific gaps: missing complexity analysis, missing pseudocode,
  derivation gaps, unstated assumptions, missing protocol comparisons.
- Present a gap analysis table to the user.

### Phase 3 — Expand Knowledge
- For each prioritized gap, supplement:
  - Definitions and first principles
  - Mathematical derivations (LaTeX `$$...$$`)
  - Algorithm pseudocode and time/space complexity
  - Historical context and motivation
  - Comparison with related approaches (e.g., TCP vs UDP)
  - Concrete code examples with language tags
- Preserve English CS terms with 中文 on first use: "Backpropagation (反向传播)"

### Phase 4 — Structure Systematically
- Build a hierarchical topic tree: Subject → Subtopics → Concepts → Details.
- Sequence for learning: prerequisites before dependents.
- Tag each section: difficulty (入门/进阶/高级) and prerequisites.
- Generate a Mermaid concept map (flowchart / classDiagram / stateDiagram).

### Phase 5 — Generate Knowledge Base
- Assemble the final document following the 8-section output schema.
- Write to `[topic-slug]-knowledge-base.md` in the source directory.
- Run quality checks before finalizing (glossary completeness, link validity,
  LaTeX correctness, consistent heading hierarchy).

## Output Document Structure

The generated knowledge base contains 8 sections:

1. **Metadata header** — topic, CS subfield tags, date, source files
2. **Table of Contents** — numbered hierarchical listing
3. **Concept Map** — Mermaid diagram of concept relationships
4. **Main Chapters** — structured content with code blocks, LaTeX, pseudocode,
   complexity tables, protocol comparisons
5. **CS Glossary** — English term → 中文 → definition (alphabetically sorted)
6. **Cross-Reference Index** — internal links between related concepts
7. **Classic Problems & Exercises** — relevant exam/interview problems with hints
8. **Further Reading** — textbook chapters, paper DOIs, RFCs, official docs

## Reference Files

- [Workflow Details](references/workflow.md) — complete 5-phase instructions,
  CS-specific judgment rules, edge case handling
- [Output Schema](references/output-schema.md) — exact template, formatting rules,
  code/LaTeX/Mermaid conventions
- [Expansion Guide](references/expansion-guide.md) — gap taxonomy, academic depth
  standards, citation formats, subfield quick reference
- [Examples](EXAMPLES.md) — annotated before/after examples of CS note refinement
