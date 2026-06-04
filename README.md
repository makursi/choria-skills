# Notes Refiner

A Claude Code skill that transforms scattered **Computer Science & Technology** classroom notes (Markdown format) into a structured, academically-deep knowledge base document through AI-powered knowledge expansion and systematic organization.

## Features

- **Knowledge Expansion**: Automatically identifies knowledge gaps (unexplained terms, missing derivations, assumed background) and fills them to academic-level completeness
- **Knowledge Structuring**: Organizes fragmented concepts into a hierarchical framework with prerequisite-ordered chapters and Mermaid concept relationship diagrams
- **Golden Circle (Why → How → What)**: Every expanded concept follows motivation → mechanism → definition order for logical comprehension
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

## Usage

1. Prepare your `.md` classroom note files
2. In Claude Code, say: **"refine my CS notes"** (or reference specific file paths)
3. The skill auto-loads and proceeds through 5 phases with minimal confirmation prompts:

| Phase | Name | Output |
|-------|------|--------|
| 1 | Ingest & Parse | Concept inventory, domain verification |
| 2 | Analyze & Identify Gaps | Gap analysis table (terminology audit + depth assessment) |
| 3 | Expand Knowledge | Academic-level expansions in Why → How → What format (silent) |
| 4 | Structure Systematically | Hierarchical outline + Mermaid concept map |
| 5 | Generate Knowledge Base | Complete `.md` knowledge base document |

The output is saved to the source directory as `[topic-slug]-knowledge-base.md`.

## Confirmation Behavior

- Gaps <= 8: confirms topic (Phase 1) and final outline (Phase 4) — 2 prompts
- Gaps > 8: adds gap priority confirmation (Phase 2) — 3 prompts
- User says "auto-proceed": only final outline shown for approval — 1 prompt

## Output Document Structure

Generated knowledge bases contain 6 sections:

1. Metadata header (date + source files only)
2. Hierarchical table of contents
3. Mermaid concept relationship diagram
4. Main chapters (Why/How/What structure, code blocks, LaTeX formulas, pseudocode, comparison tables)
5. CS glossary (English → 中文 → definition)
6. Cross-reference index

## File Structure

```
notes-refiner/
├── SKILL.md                    # Skill entry point (<=100 lines)
├── EXAMPLES.md                 # CS domain usage examples
├── CLAUDE.md                   # Claude Code collaboration guide
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
