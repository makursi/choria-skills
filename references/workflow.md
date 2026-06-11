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

**Math/CS boundary domains** (content is mathematical but has CS overlap):
- Graph theory, information theory, probability theory, group theory,
  mathematical logic, combinatorics, number theory, linear algebra
- Do NOT reject. Present this notice and wait for user confirmation:

```
检测到您的笔记涉及 [领域名]，该领域横跨数学和计算机科学。
本 Skill 将从算法与数据结构角度进行扩展（含伪代码、复杂度分析）。
如需纯数学角度（定理证明、抽象代数结构），建议不使用此 Skill。
是否继续？
```

**Non-CS signals** (if ALL content falls here, reject):
- Biology, chemistry, physics, medicine, history, literature, law, economics,
  etc. with NO CS terminology or code
- Pure mathematics with no CS application context AND not in a boundary domain

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
- Ask: "是否可以进入 Phase 2 分析阶段？" (skip if user opted to auto-proceed)

### Edge Cases — Phase 1

| Situation | Action |
|-----------|--------|
| **Very long files** (>500 lines) | Read in 200-line chunks; extract progressive summaries per chunk, then a meta-summary per file |
| **Very short notes** (<20 lines) | Flag: "笔记内容极少，扩展后内容将主要来自外部研究而非原文。是否继续？" |
| **Empty .md files** | Skip with note: "跳过空文件: X" |
| **Non-.md files present** | List them: "以下非 .md 文件已忽略: [list]" |
| **Images referenced** | Record path and alt text. Do not attempt to read image content. |
| **Notes with only code blocks** (>80% code) | Reject: "代码为主的笔记 — 此 Skill 聚焦理论模型与形式化分析，不适用于以代码为主的笔记。建议使用通用 AI 对话处理。" Phase 1 终止。 |

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

### Step 2.2: Depth Assessment (Dual-Layer Scoring Model)

For each **Defined** concept, compute a dual-layer score, then map to an L-level.

**Step 1 — Coverage Score (0–5)**: One point for each causal chain element that has
non-empty content in the source notes:

| Element | Scoring |
|---------|---------|
| 问题 (Problem) | Present and non-empty = 1 |
| 资源 (Resource) | Present and non-empty = 1 |
| 抽象 (Abstraction) | Present and non-empty = 1 |
| 机制 (Mechanism) | Present and non-empty = 1 |
| 策略 (Strategy) | Present and non-empty = 1 |

Coverage = sum (range: 0–5).

**Step 2 — Depth Bonus**: Count analysis layers with meaningful content in source:

| Analysis Layer | Scoring |
|---------------|---------|
| 权衡 (Trade-offs) | ≥2 alternatives compared on measurable dimensions = +1 |
| 关系 (Relationships) | At least 1 outgoing edge described = +1 |
| 最小示例 (Minimal Example) | Concrete instance/walkthrough present = +1 |
| 关键要点 (Key Takeaways) | Summarizing takeaway present = +1 |
| 验证 (Verification) | Code/pseudocode present = +1 |

Depth Bonus = sum (range: 0–5).

**Step 3 — Map to L-Level**:

| Level | Criteria | Expansion Needed |
|-------|----------|-----------------|
| **L1** | Coverage 0–1 | Heavy — build causal chain from scratch; add analysis layers |
| **L2** | Coverage 2–3 | Significant — fill missing causal chain elements; add analysis layers |
| **L3** | Coverage 4–5 AND Depth Bonus ≥ 1 | Light — add depth to weak elements, edge cases, historical context |
| **L4** | Coverage 5 AND Depth Bonus ≥ 3, OR coverage 5 with 机制+策略 complete closed loop | Minimal — minor refinements only |

### Step 2.3: CS-Specific Gap Detection

Beyond terminology gaps, check for these CS-specific deficiencies:

- [ ] **Complexity analysis missing**: Algorithm mentioned but no time/space complexity
- [ ] **Formal specification missing**: Algorithm/protocol lacks preconditions, postconditions, invariants
- [ ] **Derivation gaps**: Result stated without derivation (e.g., "thus O(n log n)" without showing work)
- [ ] **Protocol comparison missing**: Protocol described alone without comparison to alternatives (TCP without mentioning UDP, or vice versa)
- [ ] **Problem statement missing (Type 7)**: Concept described but the original problem/goal it addresses is never stated. Apply removal test.
- [ ] **Resource constraint missing**: Abstraction/mechanism discussed without mentioning underlying physical/hardware constraints
- [ ] **Trade-off analysis missing (Type 8)**: Multiple approaches mentioned but not compared on commensurable dimensions; no Pareto frontier logic
- [ ] **Assumption gaps**: Conclusion relies on unstated assumptions ("assuming a balanced tree...")
- [ ] **Historical context missing**: Important algorithm or concept with no origin story (who, when, why)

### Step 2.4: Relationship Mapping

Build a mental concept graph with 6 edge types:
- **Depends-on**: X requires understanding Y (record as prerequisite)
- **Is-a**: X is a type/instance of Y
- **Contrasts-with**: X and Y solve similar problems differently (TCP vs UDP)
- **Builds-on**: X extends or generalizes Y (Red-Black Tree builds on BST)
- **Trade-offs-with**: X and Y compete on shared goals with conflicting dimension preferences (FIFO ↔ LRU: simplicity vs hit rate). Feeds the 权衡 analysis layer.
- **Solves**: concept X directly addresses problem Y. Feeds the 问题 causal chain element — anchors each concept's problem statement.

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

**Decision rule for whether to show the table**:
- If gap count > 8: show the table and ask for confirmation
- If gap count <= 8: summarize orally, proceed silently to Phase 3
- If user opted to auto-proceed: skip confirmation regardless

When showing the table:
```
| # | Concept | Status | Depth | Gap Type | Priority | Suggested Expansion |
|---|---------|--------|-------|----------|----------|---------------------|
| 1 | Dijkstra | Mentioned | L1 | 形式化, 问题 | High | Add formal specification (preconditions, invariants), O((V+E)log V) derivation, comparison with Bellman-Ford |
| 2 | LRU vs FIFO | Defined | L2 | 权衡 | Medium | Multi-dimensional comparison table (hit rate, implementation complexity, Belady's anomaly) |
| 3 | ... | ... | ... | ... | ... | ... |
```
Ask: "请确认以上缺口分析。是否需要调整优先级或跳过某些概念的扩展？"

### Edge Cases — Phase 2

| Situation | Action |
|-----------|--------|
| **>50 concepts detected** | Suggest scoping: "检测到超过50个概念。建议聚焦于一个子领域。当前检测到的子领域包括：[A, B, C]。请选择聚焦范围。" |
| **All concepts at L4** | "笔记已达到较高深度，缺口较少。将主要进行结构化整理和补充性扩展。" |
| **Contradictory definitions across files** | Flag in the table: "[!] 文件A和文件B对X的定义存在差异" |
| **Many orphaned concepts** | Ask: "以下概念在笔记中孤立提及，是否需要保留并扩展，还是可以移除？[list]" |

---

## Phase 3 — Expand Knowledge (SILENT — no user prompts)

Phase 3 runs silently. The user reviews its output implicitly when the Phase 4
structure outline is presented.

**Scope constraint**: Only expand concepts from the source note inventory. Do
NOT generate new topic branches, "you might also want to know" tangents, or
concepts not mentioned in the original notes. The goal is to fill gaps in what
the student already has — not to write a textbook chapter on the broader subject.

### Step 3.1: Depth Tier Decision (APPLY BEFORE EXPANDING)

Phase 2 assigned each gap a priority (High / Medium / Low). Use two expansion tiers:

| Priority | Tier | Expansion Rule |
|----------|------|----------------|
| **High / Medium** | Full | Complete 5-element causal chain: 问题 → 资源 → 抽象 → 机制 → 策略. All analysis layers (权衡, 关系, 最小示例, 关键要点) embedded. Full derivations, formal specifications, comparisons, historical context. Optional 验证 if it materially deepens understanding. |
| **Low** | Light | 抽象 (formal definition + 1-2 mathematical properties) + 策略 (1 sentence: what scenario uses which approach). 关键要点 (1 sentence). Skip 问题, 资源, 机制 derivations, historical background, comparison tables, 验证, and remaining analysis layers. |

Apply this rule BEFORE the 5-element causal chain template below.

### Step 3.2: 5-Element Causal Chain Structure

Every expanded concept (full tier) follows this 5-section structure in fixed order:

```
### N.m Concept Name

**问题 (Problem)** — 该概念要达成的系统/人类目标。
  操作定义：移除该问题 → 该技术没有存在的理由。
  三态：Full（系统/算法级）/ Minimal（语言/工具级，一句）/ None（纯定义级，跳过此节）。

**资源 (Resource)** — 不可改变的物理/硬件/现实约束。
  操作定义：移除该约束 → 世界物理性质改变。

**抽象 (Abstraction)** — 在资源和问题之上建立的模型。
  形式化定义、数学性质、不变量、前置/后置条件。
  [融入: 关系 — is-a 类型层级, contrasts-with 结构对比]

**机制 (Mechanism)** — 抽象如何被实现。
  推导步骤、状态转移、协议交互——用 LaTeX 数学推导和状态图描述。
  不依赖伪代码作为主要描述工具。
  [融入: 关系 — depends-on / builds-on 依赖链]
  [融入: 最小示例 — 具体实例的逐步推演：具体数值、逐步 trace。展示概念在真实数据上的运作，不是抽象描述]

**策略 (Strategy)** — 真实系统实际采用的方案。
  谁采用了什么方案，为什么。
  [融入: 权衡 — 多维度对比（≥2 可量化维度），帕累托前沿，为何不存在同时最优的方案，真实系统选择了哪个点]

**验证 (Verification)** *(可选)* — 极简代码/伪代码，仅验证对原理的理解。
  非知识主体。不含错误处理、边界检查、生产代码结构。必须带语言标签。

*[1-3 句关键要点，无标题，每个概念强制出现在末尾]*
```

For light-tier concepts:

```
### N.m Concept Name

**抽象 (Abstraction)** — Formal definition + 1-2 key mathematical properties.

**策略 (Strategy)** — [1 sentence: what scenario uses which approach.]

*[1 sentence key takeaway]*
```

Not every full-tier concept fills all 5 causal chain sections equally:
- A protocol concept may have more 机制 (state machine) and 策略 (congestion control variants)
- A theorem may have more 抽象 (formal statement) and 机制 (proof sketch), no 验证, 最小示例 may be low priority
- A language feature may have Minimal 问题 (1 sentence), more 抽象 (semantics) and 策略 (alternative designs)
- 验证 is optional; include only when code/pseudocode materially deepens understanding.
  Light-tier concepts never include 验证.
- 问题 follows three-state logic: Full / Minimal (1 sentence) / None (pure definition concepts — skip entirely).
  Only 问题 has three-state; other causal chain elements use two-state (Full / absent-in-light-tier).

### Step 3.3: Expansion Dimensions (Apply Where Applicable)

The following dimensions are a general checklist. Apply only those that fit the
specific concept. The **hard requirements** are the subfield-specific depth
standards defined in the Expansion Guide — those are non-negotiable.

Causal chain prompts (fixed order, visible as headings):
- **问题**: State the original goal; three-state (Full/Minimal/None); apply removal test
- **资源**: Identify immutable constraints; apply removal test
- **抽象**: Formal definition and first principles; mathematical properties, invariants, pre/post-conditions; embed is-a/contrasts-with relationships
- **机制**: Mathematical derivation (LaTeX, show steps); state transitions; protocol interactions; embed depends-on/builds-on relationships; embed 最小示例 (concrete instance trace)
- **策略**: What real systems use; embed 权衡 (multi-dimensional comparison, Pareto frontier)

Analysis layer prompts (embedded, not separate headings):
- **权衡**: ≥2 alternatives compared on quantifiable dimensions; Pareto frontier; why no simultaneously optimal solution; real system positioning
- **关系**: Outgoing edges only — is-a/contrasts-with → 抽象, depends-on/builds-on → 机制; structured list format
- **最小示例**: Concrete numbers/instance walkthrough in 机制; show the concept operating step by step
- **关键要点**: 1-3 sentences at end, mandatory, no heading

General expansion prompts:
- Time/space complexity with derivation
- Historical context (who, when, what resource constraint)
- Cross-references to dependent concepts
- Theory-practice gap — when theoretical assumptions break down in real systems?
- Verification code *(optional)* — minimal, in 验证 subsection, not knowledge body

### Step 3.4: Bilingual Terminology Rules

- **First use**: English term with 中文 in parentheses.
  Example: "反向传播 (Backpropagation) 是训练神经网络的核心算法..."
- **Subsequent uses**: Either form is acceptable. Prefer the form that reads more
  naturally in context. Code, formulas, and diagrams always use English.
- **Glossary entry**: Every English CS term must appear in the final glossary
  with its 中文 equivalent and definition.

### Step 3.5: Batch Research and Citation (DO NOT SERIALIZE PER-GAP)

Research ALL gaps in batch, not one at a time:

**Round 1 — Broad sweep**: 1-2 WebSearch queries covering all gap concepts from Phase 2.
Use broad queries that can cover multiple related concepts at once.

**Round 2 — Deep fetch**: 3-4 WebFetch calls for concepts where Round 1 was insufficient
(high-priority gaps, or gaps where search snippets were too thin).

**Round 3 — Write**: Compose all expansions in one pass, referencing the collected research.
Cite sources using these formats:

- **Textbook**: [CLRS, Ch. 12] or [CSAPP, §6.2]
- **Paper**: [Author (Year), "Title", Venue. DOI]
- **RFC**: [RFC 793 — Transmission Control Protocol]
- **Official docs**: [Python 3.12 Documentation — asyncio]
- **Online course**: [MIT 6.824 Lecture 5 — Go Concurrency]

Never fabricate citations. If a source cannot be found, write:
`[待补充引用 / Citation TBD]`.

### Step 3.6: Quality Checks per Expansion

For each expanded concept, verify:
- [ ] Definition is self-contained (no circular references)
- [ ] All symbols in LaTeX formulas are defined
- [ ] Complexity analysis includes derivation steps, not just final Big-O
- [ ] **形式化规约自洽**: 前置/后置条件、不变式逻辑完整
- [ ] Historical context is factually accurate (cross-check if uncertain)
- [ ] English terms follow the first-use parenthesis rule
- [ ] **问题→资源→抽象→机制→策略 因果链顺序正确**: 不跳跃层级；问题陈述在约束之前，约束驱动抽象设计，抽象通过机制落地，策略在机制之上做选择
- [ ] **问题通过移除测试**: 移除该问题 → 该概念无存在理由（Full/Minimal/None 三态正确应用）
- [ ] **资源通过移除测试**: 移除该约束 → 世界物理性质改变
- [ ] **关系融入正确**: is-a/contrasts-with → 抽象小节内；depends-on/builds-on → 机制小节内；至少 1 条出边
- [ ] **权衡融入正确**: 策略小节内包含 ≥2 可量化维度的方案对比，帕累托前沿逻辑
- [ ] **最小示例融入正确**: 机制小节内包含具体数值/实例的逐步推演，非抽象描述
- [ ] **关键要点**: 1-3 句，无标题，每个概念强制出现在末尾
- [ ] **验证代码 (如存在)**: 位于独立的 验证 小节，位于 策略 之后、关键要点之前，极简且聚焦核心概念，不包含错误处理/边界检查/生产结构

### Edge Cases — Phase 3

| Situation | Action |
|-----------|--------|
| **Cannot find sufficient information** | Mark section with `[待补充 / To Be Supplemented]` and note what's missing. Do not fabricate. |
| **Highly mathematical topic** (e.g., PAC learning bounds) | Use `$$` display math for key formulas, `$` inline for symbols. Provide intuitive explanation alongside formal derivation. |
| **Pure code topic** (e.g., "Python generators") | Focus on: language semantics, underlying implementation (CPython), comparison with other languages, common patterns and pitfalls. Apply Minimal 问题 (1 sentence). |
| **System design topic** (e.g., "Load balancing") | Include architecture diagrams (Mermaid or text), trade-off tables, real-world system examples (Nginx, HAProxy, AWS ELB). |
| **Security/crypto topic** | Include threat models, attack vectors, and "don't roll your own crypto" warnings where appropriate. |
| **Conflicting sources** | Present both viewpoints with attribution: "来源A认为...而来源B认为...目前学界/业界的主流观点是..." |

---

## Phase 4 — Structure Systematically

### Step 4.1: Build Topic Tree

Organize concepts from the source inventory into a nested hierarchy. Only include
concepts that appear in the source notes — do not add synthetic subtopics.

```
Root (Main CS Topic)
├── 1. Subfield / Major Topic A
│   ├── 1.1 Concept X
│   ├── 1.2 Concept Y (prerequisite: 1.1)
│   └── 1.3 Concept Z (prerequisite: 1.1, 1.2)
├── 2. Subfield / Major Topic B
│   └── ...
└── N. Subfield / Major Topic N
```

Each concept's content is internally organized as 问题→资源→抽象→机制→策略(+可选验证→关键要点).
These are content layers, not sub-headings in the table of contents.

### Step 4.2: Sequence for Learning

Reorder concepts so that:
- Prerequisites always come before dependents
- Foundational concepts (definitions, principles) come before applications
- If a circular dependency exists (A needs B, B needs A), flag it explicitly and
  suggest reading order

### Step 4.3: Generate Concept Map

Create a Mermaid diagram showing top-level concept relationships only
(root → major branches, 1-2 layers deep). Do NOT enumerate every concept.
Choose the diagram type based on content:

| Content Type | Diagram Type |
|-------------|-------------|
| Hierarchical knowledge | `mindmap` |
| Process/protocol flow | `flowchart TD` or `sequenceDiagram` |
| Class/type hierarchy | `classDiagram` |
| State machine | `stateDiagram-v2` |
| System architecture | `flowchart LR` |

### Step 4.4: Generate Table of Contents

Numbered, with indentation reflecting the tree. Use `1.` `1.1` `1.1.1` style.

### Step 4.5: Present Structure for Approval

ALWAYS show the complete outline (TOC + tags + concept map) before generating
the final document. This is the primary confirmation gate.

```
以上是知识库的结构大纲。请确认：
1. 层级结构是否合理？
2. 是否需要增加、删除或移动某些章节？
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

1. Metadata header (date + source files only)
2. Table of Contents
3. Concept Map (Mermaid — top-level skeleton only)
4. Main Chapters (问题→资源→抽象→机制→策略 structure with embedded analysis layers, formulas, tables, optional verification code)

### Step 5.2: Formatting Consistency

Follow all formatting conventions in [output-schema.md](output-schema.md):
LaTeX delimiters, table alignment, verification code conventions, and terminology rules.

### Step 5.3: Quality Checklist

Before finalizing, verify these 3 critical checks:

- [ ] LaTeX formulas: all symbols defined, correct delimiters (`$$` / `$`)
- [ ] Mermaid diagrams: valid syntax, top-level skeleton only
- [ ] Verification code (if present): minimal, language-tagged, in 验证 subsection only

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
- 对比表格: P
- 参考引用: Q
```

### Edge Cases — Phase 5

| Situation | Action |
|-----------|--------|
| **Output too large** (>3000 lines) | Split into main document + appendix files. Flag early in Phase 4 if this seems likely. |
| **Mermaid syntax unsure** | Test the diagram mentally for common errors: unclosed brackets, missing arrows, unmatched quotes. If uncertain, add an HTML comment: `<!-- Verify this Mermaid diagram renders correctly -->` |
| **File already exists** | Ask: "文件已存在。覆盖 / 重命名 / 取消？" |
