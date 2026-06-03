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

# Notes Refiner (笔记精炼师)

Transform scattered CS classroom notes into an academically-deep, hierarchically
structured knowledge base. **CS domain only** -- if notes are not Computer Science
related, inform the user and stop.

## Quick Start

1. Tell the skill where your `.md` notes live (directory path or specific files).
2. Provide the CS topic if not obvious from the notes (e.g., "操作系统内存管理").
3. The skill proceeds through 5 phases. Confirmation prompts are minimized:
   - If gaps > 8: confirms topic, gap priorities, and final outline (3 prompts).
   - If gaps <= 8: confirms topic and final outline only (2 prompts).
   - Say "auto-proceed" to skip all intermediate prompts -- only the final
     structure outline is shown for approval.
   Phase 3 (expansion) always runs silently in the background.

## Workflow Overview

Proceed through these 5 phases in order. Do not skip phases. Confirmations follow
the flexible scheme described above.

### Phase 1 -- Ingest & Parse
- Glob/read all `.md` files from the specified path.
- **Domain gate**: If content is clearly NOT Computer Science, inform the user
  this skill only serves CS topics and stop. If content lies in a math/CS
  boundary domain (graph theory, information theory, probability, etc.), inform
  the user of the CS-centric expansion approach and confirm before proceeding.
- Extract headings, CS terminology, code blocks, formulas, and build a flat
  concept inventory.
- Ask for topic confirmation if ambiguous.

### Phase 2 -- Analyze & Identify Gaps
- Classify every concept: Defined / Mentioned-but-unexplained / Assumed-background
  / Orphaned.
- Assess depth level (L1=definition only --> L4=full academic treatment).
- Identify CS-specific gaps: missing complexity analysis, missing pseudocode,
  derivation gaps, unstated assumptions, missing protocol comparisons.
- Present a gap analysis table to the user (if gap count > 8).

### Phase 3 -- Expand Knowledge (silent)
- For each prioritized gap, supplement using the Why/How/What structure:
  - **Why** -- problem motivation, historical context, real-world need
  - **How** -- derivation, proof sketch, design rationale, mechanism
  - **What** -- definition, pseudocode, complexity, code examples, comparisons
- Preserve English CS terms with 中文 on first use: "Backpropagation (反向传播)"
- Expansion dimensions are "apply where applicable" -- not every concept needs
  all dimensions. Subfield-specific depth standards are the hard requirements.
- Only expand concepts present in the source notes. Do not invent new topic
  branches the user did not mention.

### Phase 4 -- Structure Systematically
- Build a hierarchical topic tree using only concepts from the source notes.
- Sequence for learning: prerequisites before dependents.
- Generate a Mermaid concept map (flowchart / classDiagram / stateDiagram).
- Present the structure outline for user approval.

### Phase 5 -- Generate Knowledge Base
- Assemble the final document following the 6-section output schema.
- Write to `[topic-slug]-knowledge-base.md` in the source directory.
- Run quality checks before finalizing (glossary completeness, link validity,
  LaTeX correctness, consistent heading hierarchy).

## Output Document Structure

The generated knowledge base contains 6 sections:

1. **Metadata header** -- date, source files
2. **Table of Contents** -- numbered hierarchical listing
3. **Concept Map** -- Mermaid diagram of concept relationships
4. **Main Chapters** -- structured content with code blocks, LaTeX, pseudocode,
   complexity tables, protocol comparisons, organized by Why/How/What
5. **CS Glossary** -- English term to 中文 to definition (alphabetically sorted)
6. **Cross-Reference Index** -- internal links between related concepts

## Reference Files

- [Workflow Details](references/workflow.md) -- complete 5-phase instructions,
  CS-specific judgment rules, edge case handling
- [Output Schema](references/output-schema.md) -- exact template, formatting rules,
  code/LaTeX/Mermaid conventions
- [Expansion Guide](references/expansion-guide.md) -- gap taxonomy, academic depth
  standards, citation formats, subfield quick reference
- [Examples](EXAMPLES.md) -- annotated before/after examples of CS note refinement
