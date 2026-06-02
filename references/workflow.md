# Workflow Details — 5-Phase Execution Guide

This document contains the complete step-by-step instructions, checklists, and
decision rules for each of the 5 phases. Follow these instructions precisely.

---

## Phase 1 — Ingest & Parse

### Step 1.1: Locate and Read Source Files

- If user provided a directory path, glob for `**/*.md` recursively.
- If user provided specific file paths, verify each exists and ends with `.md`.
- List all discovered files to the user. Skip non-`.md` files but mention them.

### Step 1.2: Domain Gate (CRITICAL — do not skip)

Scan the content of ALL files for CS domain markers:

**CS-positive signals** (any one is sufficient):
- CS technical terms: algorithm, data structure, process/thread, TCP/IP, SQL,
  compiler, cache, register, instruction set, neural network, hash, tree/graph,
  deadlock, semaphore, syscall, interrupt, DMA, pipeline, etc.
- Code blocks in any programming language
- LaTeX formulas related to CS (Big-O, summations, recurrence relations)
- Architecture diagrams, protocol flows, state machines
- References to CS textbooks, papers, RFCs, or documentation

**Non-CS signals** (if ALL content falls here, reject):
- Biology, chemistry, physics, medicine, history, literature, law, economics,
  etc. with NO CS terminology or code
- Pure mathematics with no CS application context

**Rejection message** if not CS:
```
检测到您的笔记内容不属于计算机科学与技术领域。此 Skill 仅服务于 CS 相关笔记。
请使用通用 AI 对话处理非 CS 内容。如果您认为这是误判，请说明笔记中的 CS 主题。
```

### Step 1.3: Parse and Build Concept Inventory

For each file:
1. Extract heading hierarchy (H1-H4).
2. Extract **bold** and *italic* terms (likely key concepts).
3. Extract code blocks (record language tags and line counts).
4. Extract LaTeX formulas within `$$` or `$` (record for later use).
5. Extract named entities: algorithm names, protocol names, tool names, people.

Build a **concept inventory** — a flat list of every significant CS term or
named entity. Each entry should have:
- Term (English preferred, with 中文 if present in source)
- Source file and heading context
- Occurrence count across all files

### Step 1.4: Topic Confirmation

If the CS subfield is clear from the notes (e.g., "操作系统", "机器学习"), state
it and proceed. If ambiguous (notes cover multiple unrelated CS areas), ask:
```
检测到笔记涉及多个 CS 子领域：[领域A, 领域B, 领域C]。
请确认本次需要精炼的主题范围。可以多选或指定一个总主题名称。
```

### Step 1.5: Summary Presentation

Present to user:
- Files processed: N files, total M lines
- Concept inventory size: K unique terms detected
- Detected CS subfield(s): [list]
- Ask: "是否可以进入 Phase 2 分析阶段？" (or auto-proceed)

### Edge Cases — Phase 1

| Situation | Action |
|-----------|--------|
| **Very long files** (>500 lines) | Read in 200-line chunks; extract progressive summaries per chunk, then a meta-summary per file |
| **Very short notes** (<20 lines) | Flag: "笔记内容极少，扩展后内容将主要来自外部研究而非原文。是否继续？" |
| **Empty .md files** | Skip with note: "跳过空文件: X" |
| **Non-.md files present** | List them: "以下非 .md 文件已忽略: [list]" |
| **Images referenced** | Record path and alt text. Do not attempt to read image content. |
| **Notes with only code blocks** (>80% code) | Flag as "代码为主的笔记 — 将在 Phase 3 补充解释和上下文" |

---

## Phase 2 — Analyze & Identify Gaps

### Step 2.1: Terminology Audit

Classify every concept from the inventory into one of four categories:

| Category | Definition | Example |
|----------|------------|---------|
| **Defined** | Explicitly explained with definition, derivation, or example | "红黑树是一种自平衡二叉搜索树，满足以下5条性质..." |
| **Mentioned** | Named but not explained — a clear knowledge gap | "这里使用了 Dijkstra 算法" (no explanation of what it is or how it works) |
| **Assumed** | Used as prerequisite — reader is expected to already know it | "显然这需要 O(log n) 的查找" (no justification) |
| **Orphaned** | Mentioned once with no connection to other concepts | A lone bullet: "- CAP 定理" with no further context |

### Step 2.2: Depth Assessment

For each **Defined** concept, assign a depth level:

| Level | Criteria | Expansion Needed |
|-------|----------|-----------------|
| **L1** | Definition only, one sentence | Heavy — add derivation, examples, context |
| **L2** | Definition + brief explanation, no examples | Significant — add examples, derivations, comparisons |
| **L3** | Definition + explanation + 1 example | Light — add depth, edge cases, historical context |
| **L4** | Full treatment: definition, derivation, examples, comparisons, historical context | Minimal — minor refinements only |

### Step 2.3: CS-Specific Gap Detection

Beyond terminology gaps, check for these CS-specific deficiencies:

- [ ] **Complexity analysis missing**: Algorithm mentioned but no time/space complexity
- [ ] **Pseudocode missing**: Algorithm described in prose but no structured pseudocode
- [ ] **Derivation gaps**: Result stated without derivation (e.g., "thus O(n log n)" without showing work)
- [ ] **Protocol comparison missing**: Protocol described alone without comparison to alternatives (TCP without mentioning UDP, or vice versa)
- [ ] **Code example missing**: Programming concept described without concrete code
- [ ] **Assumption gaps**: Conclusion relies on unstated assumptions ("assuming a balanced tree...")
- [ ] **Historical context missing**: Important algorithm or concept with no origin story (who, when, why)
- [ ] **Trade-off analysis missing**: Design choice presented without alternatives or trade-offs

### Step 2.4: Relationship Mapping

Build a mental concept graph:
- **Depends-on**: X requires understanding Y (record as prerequisite)
- **Is-a**: X is a type/instance of Y
- **Contrasts-with**: X and Y solve similar problems differently (TCP vs UDP)
- **Builds-on**: X extends or generalizes Y (Red-Black Tree builds on BST)

Flag any concept that:
- Depends on a concept not present in any source (missing prerequisite)
- Has no connections at all (orphaned)

### Step 2.5: Gap Prioritization

Score each gap:

| Factor | Weight | How to assess |
|--------|--------|---------------|
| **Criticality** | High | How many other concepts depend on this one? (count dependency edges) |
| **Depth deficit** | Medium | L4 - current level (L1=3, L2=2, L3=1, L4=0) |
| **User emphasis** | Medium | Does the note structure suggest this is important? (frequency, heading depth, bold/italic) |

Priority = Criticality × 2 + Depth deficit + User emphasis

### Step 2.6: Present Gap Analysis

Show the user a table:
```
| # | Concept | Status | Depth | Priority | Suggested Expansion |
|---|---------|--------|-------|----------|---------------------|
| 1 | Dijkstra | Mentioned | L1 | High | Add algorithm steps, pseudocode, O((V+E)log V) derivation, comparison with Bellman-Ford |
| 2 | ... | ... | ... | ... | ... |
```
Ask: "请确认以上缺口分析。是否需要调整优先级或跳过某些概念的扩展？"

### Edge Cases — Phase 2

| Situation | Action |
|-----------|--------|
| **>50 concepts detected** | Suggest scoping: "检测到超过50个概念。建议聚焦于一个子领域。当前检测到的子领域包括：[A, B, C]。请选择聚焦范围。" |
| **All concepts at L4** | "笔记已达到较高深度，缺口较少。将主要进行结构化整理和补充性扩展。" |
| **Contradictory definitions across files** | Flag in the table: "⚠ 文件A和文件B对X的定义存在差异" |
| **Many orphaned concepts** | Ask: "以下概念在笔记中孤立提及，是否需要保留并扩展，还是可以移除？[list]" |

---

## Phase 3 — Expand Knowledge

### Step 3.1: Expansion Principles

For each gap (process one at a time, high priority first), expand to L4 academic
depth. Every expanded concept must include:

1. **Definition & First Principles** — What is it? What problem does it solve?
   Start from the most fundamental level the target audience needs.
2. **Derivation** (if applicable) — Mathematical derivation with LaTeX, showing
   intermediate steps, not just the final result.
3. **Algorithm / Pseudocode** (if applicable) — Structured pseudocode using
   standard conventions (see output-schema.md).
4. **Complexity Analysis** (if applicable) — Time and space complexity with
   derivation. Best/average/worst case.
5. **Concrete Code Example** (if applicable) — Working code in the most relevant
   language, with comments.
6. **Historical Context** — Who developed it? When? What problem motivated it?
7. **Comparison with Alternatives** — How does it differ from related approaches?
   A comparison table when >2 alternatives exist.
8. **Cross-References** — Link to dependent and related concepts.

### Step 3.2: Bilingual Terminology Rules

- **First use**: English term with 中文 in parentheses.
  Example: "反向传播 (Backpropagation) 是训练神经网络的核心算法..."
- **Subsequent uses**: Either form is acceptable. Prefer the form that reads more
  naturally in context. Code, formulas, and diagrams always use English.
- **Glossary entry**: Every English CS term must appear in the final glossary
  with its 中文 equivalent and definition.

### Step 3.3: Research and Citation

Use WebSearch and WebFetch to research concepts when the source notes are
insufficient. Cite sources using these formats:

- **Textbook**: [CLRS, Ch. 12] or [CSAPP, §6.2]
- **Paper**: [Author (Year), "Title", Venue. DOI]
- **RFC**: [RFC 793 — Transmission Control Protocol]
- **Official docs**: [Python 3.12 Documentation — asyncio]
- **Online course**: [MIT 6.824 Lecture 5 — Go Concurrency]

Never fabricate citations. If a source cannot be found, write:
`[待补充引用 / Citation TBD]`.

### Step 3.4: Quality Checks per Expansion

For each expanded concept, verify:
- [ ] Definition is self-contained (no circular references)
- [ ] All symbols in LaTeX formulas are defined
- [ ] Pseudocode is syntactically clear and consistent
- [ ] Complexity analysis includes derivation steps, not just final Big-O
- [ ] Code example compiles/runs (best-effort — flag if untested)
- [ ] Historical context is factually accurate (cross-check if uncertain)
- [ ] English terms follow the first-use parenthesis rule

### Edge Cases — Phase 3

| Situation | Action |
|-----------|--------|
| **Cannot find sufficient information** | Mark section with `[待补充 / To Be Supplemented]` and note what's missing. Do not fabricate. |
| **Highly mathematical topic** (e.g., PAC learning bounds) | Use `$$` display math for key formulas, `$` inline for symbols. Provide intuitive explanation alongside formal derivation. |
| **Pure code topic** (e.g., "Python generators") | Focus on: language semantics, underlying implementation (CPython), comparison with other languages, common patterns and pitfalls. |
| **System design topic** (e.g., "Load balancing") | Include architecture diagrams (Mermaid or text), trade-off tables, real-world system examples (Nginx, HAProxy, AWS ELB). |
| **Security/crypto topic** | Include threat models, attack vectors, and "don't roll your own crypto" warnings where appropriate. |
| **Conflicting sources** | Present both viewpoints with attribution: "来源A认为...而来源B认为...目前学界/业界的主流观点是..." |

---

## Phase 4 — Structure Systematically

### Step 4.1: Build Topic Tree

Organize all concepts into a nested hierarchy:

```
Root (Main CS Topic)
├── 1. 子领域/大主题 A
│   ├── 1.1 概念 X  [入门]
│   │   ├── 1.1.1 定义与原理
│   │   ├── 1.1.2 推导/算法
│   │   └── 1.1.3 应用与示例
│   ├── 1.2 概念 Y  [进阶, 前置: 1.1]
│   └── 1.3 概念 Z  [高级, 前置: 1.1, 1.2]
├── 2. 子领域/大主题 B
│   └── ...
└── N. 子领域/大主题 N
```

### Step 4.2: Sequence for Learning

Reorder concepts so that:
- Prerequisites always come before dependents
- Difficulty progresses from 入门 → 进阶 → 高级 within each subtree
- Foundational concepts (definitions, principles) come before applications
- If a circular dependency exists (A needs B, B needs A), flag it explicitly and
  suggest reading order

### Step 4.3: Tag Each Section

Every section (H2 and H3) must be tagged:

- **Difficulty**: `入门 (Beginner)` / `进阶 (Intermediate)` / `高级 (Advanced)`
  - 入门: Assumes only basic CS literacy (programming 101, basic math)
  - 进阶: Assumes undergraduate-level CS knowledge
  - 高级: Assumes graduate-level or specialized knowledge
- **Prerequisites**: Section numbers that should be read first, or "无" if none
- **Type**: One of: `定义` / `推导` / `算法` / `应用` / `比较` / `历史` / `系统设计`

### Step 4.4: Generate Concept Map

Create a Mermaid diagram showing the top-level concept relationships. Choose the
diagram type based on content:

| Content Type | Diagram Type | Example |
|-------------|-------------|---------|
| Hierarchical knowledge | `mindmap` | Subject → subtopics → concepts |
| Process/protocol flow | `flowchart TD` or `sequenceDiagram` | TCP handshake, compiler phases |
| Class/type hierarchy | `classDiagram` | Design patterns, data structures |
| State machine | `stateDiagram-v2` | Process states, protocol states |
| System architecture | `flowchart LR` | OS layers, network stack |

### Step 4.5: Generate Table of Contents

Numbered, with indentation reflecting the tree. Use `1.` `1.1` `1.1.1` style.

### Step 4.6: Present Structure for Approval

Show the user the complete outline (TOC + tags + concept map) before generating
the final document. Ask:
```
以上是知识库的结构大纲。请确认：
1. 层级结构是否合理？
2. 难度标注是否准确？
3. 是否需要增加、删除或移动某些章节？
确认后进入 Phase 5 生成最终文档。
```

### Edge Cases — Phase 4

| Situation | Action |
|-----------|--------|
| **Flat structure** (all concepts at same level) | Group related concepts under synthetic parent headings. Avoid a single-level outline. |
| **Deep nesting** (>4 levels) | Flatten by splitting the deepest level into subsections within the parent, or promote to sibling sections with "Part 1 / Part 2" naming. |
| **Unclear prerequisite chain** | Default order: theory → technique → application → comparison. |
| **Very broad topic** (>8 top-level sections) | Suggest splitting into multiple knowledge base documents or adding a "Part I / Part II" division. |

---

## Phase 5 — Generate Knowledge Base

### Step 5.1: Assemble the Document

Follow the exact schema in [output-schema.md](output-schema.md). Build sections
in order:

1. Metadata header
2. Table of Contents
3. Concept Map (Mermaid)
4. Main Chapters (with all tags, code, formulas, tables)
5. CS Glossary
6. Cross-Reference Index
7. Classic Problems & Exercises
8. Further Reading

### Step 5.2: Formatting Consistency

- All code blocks: language tag (`` ```python ``, `` ```cpp ``, `` ```pseudocode `` etc.)
- All inline code: backticks (`O(log n)`, `malloc()`, `SELECT`)
- Display math: `$$...$$` on its own line
- Inline math: `$...$`
- Tables: aligned columns with `|---|`, use bold for headers
- Terminology: first use = English (中文), subsequent uses = natural flow

### Step 5.3: Quality Checklist

Before finalizing, verify ALL of the following:

- [ ] Every English CS term in the main chapters appears in the Glossary
- [ ] Every section (H2, H3) has a difficulty tag
- [ ] All cross-reference links point to valid sections
- [ ] No orphaned concepts remain (every concept connects to at least one other)
- [ ] LaTeX formulas: all symbols defined, correct delimiters (`$$` / `$`)
- [ ] Mermaid diagrams: valid syntax, renders correctly
- [ ] Code blocks: language tags present, reasonable indentation
- [ ] Heading hierarchy: no skipped levels (H2 → H3, never H2 → H4)
- [ ] No `[待补充]` markers left without explanation
- [ ] File naming: `[topic-slug]-knowledge-base.md` in the source directory

### Step 5.4: Write Output

Write the final document to `[topic-slug]-knowledge-base.md` in the same directory
as the source notes. The topic-slug is derived from the confirmed topic name:
- "操作系统内存管理" → `os-memory-management-knowledge-base.md`
- "TCP 三次握手" → `tcp-handshake-knowledge-base.md`

### Step 5.5: Present Summary

After writing, summarize for the user:
```
知识库已生成: `[file-path]`
- 总章节数: N
- 扩展概念数: M
- 术语表条目: K
- 代码示例: P
- 参考引用: Q
```

### Edge Cases — Phase 5

| Situation | Action |
|-----------|--------|
| **Output too large** (>3000 lines) | Split into main document + appendix files. Flag early in Phase 4 if this seems likely. |
| **Mermaid syntax unsure** | Test the diagram mentally for common errors: unclosed brackets, missing arrows, unmatched quotes. If uncertain, add an HTML comment: `<!-- Verify this Mermaid diagram renders correctly -->` |
| **File already exists** | Ask: "文件已存在。覆盖 / 重命名 / 取消？" |
