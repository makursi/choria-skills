# Notes Refiner

A Claude Code skill that transforms scattered **Computer Science & Technology** classroom notes (Markdown format) into a structured, academically-deep knowledge base document through AI-powered knowledge expansion and systematic organization.

## Features

- **Knowledge Expansion**: Automatically identifies knowledge gaps (unexplained terms, missing derivations, assumed background) and fills them to academic-level completeness
- **Knowledge Structuring**: Organizes fragmented concepts into a hierarchical framework with prerequisite-ordered chapters and Mermaid concept relationship diagrams
- **9-Element Cognitive Lens System**: Every expanded concept follows a fixed 5-element causal chain (问题 → 资源 → 抽象 → 机制 → 策略), with 4 analysis layers (权衡, 关系, 最小示例, 关键要点) embedded into the causal chain sections. 验证 is an optional standalone verification subsection. This is a cognitive model for reconstructing underlying mental models, not a rigid template — the goal is to reorganize knowledge by conceptual structure, not lecture order.
- **Bilingual CS Terminology**: English CS terms preserved with Chinese annotations on first use; code and formulas remain English-only

## Domain Scope

**Computer Science & Technology only.** Covers the following subfields:

| Category | Subfields |
|----------|-----------|
| Theory | Algorithm Design & Analysis, Data Structures, Theory of Computation, Discrete Math, Mathematical Logic |
| Systems | Operating Systems, Computer Networks, Computer Architecture, Compilers, Distributed Systems |
| Data & AI | Database Systems, Artificial Intelligence, Machine Learning, Deep Learning, NLP, Computer Vision |
| Software | Programming Language Theory, Software Engineering, Design Patterns, Software Architecture |
| Security & Applications | Cryptography, Network Security, Computer Graphics, HCI |

Non-CS notes are rejected in Phase 1 by domain gating. Math/CS boundary domains (graph theory, information theory, probability theory) trigger a notice to confirm the CS-centric direction.

## Installation

```bash
npx skills add makursi/note-refiner
```

## Updating

```bash
npx skills update note-refiner
```

Check [CHANGELOG.md](CHANGELOG.md) to see what's new before updating.

## Usage

1. Prepare your `.md` classroom note files
2. In Claude Code, say: **"refine my CS notes"** (or reference specific file paths)
3. The skill auto-loads and proceeds through 5 phases with minimal confirmation prompts:

| Phase | Name | Output |
|-------|------|--------|
| 1 | Ingest & Parse | Concept inventory, domain verification |
| 2 | Analyze & Identify Gaps | Gap analysis table (terminology audit + depth assessment) |
| 3 | Expand Knowledge | Academic-level expansions in 问题 → 资源 → 抽象 → 机制 → 策略 causal chain with embedded analysis layers (silent) |
| 4 | Structure Systematically | Hierarchical outline + Mermaid concept map |
| 5 | Generate Knowledge Base | Complete `.md` knowledge base document |

The output is saved to the source directory as `[topic-slug]-knowledge-base.md`.

## Confirmation Behavior

- Gaps <= 8: confirms topic (Phase 1) and final outline (Phase 4) — 2 prompts
- Gaps > 8: adds gap priority confirmation (Phase 2) — 3 prompts
- User says "auto-proceed": only final outline shown for approval — 1 prompt

## Output Document Structure

Generated knowledge bases contain 4 sections:

1. Metadata header (date + source files only)
2. Hierarchical table of contents
3. Mermaid concept relationship diagram
4. Main chapters (问题/资源/抽象/机制/策略 causal chain with embedded 权衡/关系/最小示例/关键要点, LaTeX formulas, formal definitions, comparison tables, glossary, optional verification code)

## File Structure

```
notes-refiner/
├── SKILL.md                    # Skill entry point (<=100 lines)
├── EXAMPLES.md                 # CS domain usage examples
├── CLAUDE.md                   # Claude Code collaboration guide
├── CHANGELOG.md                 # Version history
├── README.md                   # This file
└── references/
    ├── workflow.md             # 5-phase detailed workflow instructions
    ├── output-schema.md        # Output document template & formatting rules
    └── expansion-guide.md      # CS knowledge expansion methodology
```

## Trigger Keywords

- Chinese: `整理笔记` `知识扩展` `知识体系化` `笔记精炼` `课堂笔记`
- English: `note refinement` `knowledge expansion` `knowledge structuring` `CS study notes`

## License

MIT
