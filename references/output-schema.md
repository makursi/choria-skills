# Output Schema — CS Knowledge Base Document Template

This document defines the exact structure, formatting rules, and conventions for
the generated knowledge base Markdown file.

---

## Document Template (4 Sections)

```markdown
# [Topic Name] -- Knowledge Base (知识库)

> **生成日期**: YYYY-MM-DD
> **源文件**: [file1.md, file2.md, ...]

---

## 目录 (Table of Contents)

<!-- Numbered, hierarchical TOC generated in Phase 4 -->

---

## 概念关系图 (Concept Map)

```mermaid
[Top-level skeleton: mindmap / flowchart / classDiagram / stateDiagram]
```

---

## [Chapter content from Phase 4 structure]

<!-- Each chapter: Why → How → What -->

---
```

---

## Section Template

Every content section follows this pattern:

```markdown
## N. Section Title

**Why** — motivation, historical context, problem it solves

**How** — mechanism, derivation, design rationale, proof sketch

**What** — formal definition, pseudocode, complexity, code examples, comparisons

### N.m 小结 (Summary)

[1-3 sentence key takeaways — only for high/medium priority concepts]
```

Sections open directly with Why. No difficulty badges, prerequisite labels, or type tags.
Concept ordering (prerequisites before dependents) signals learning dependencies.

---

## Code Block Conventions

### Language Selection Decision Tree

Apply in order. First match wins:

1. **User-specified language** — highest priority, applies globally
2. **AI / Machine Learning** → Python
3. **SQL queries** → SQL
4. **Low-level CS** (pointers, memory, syscalls, kernel) → C
5. **Application-layer** (frameworks, APIs, toolchains) → TypeScript
6. **Default fallback** → TypeScript

All code blocks must carry a language tag (`` ```c ``, `` ```python ``, `` ```typescript ``,
`` ```sql ``, `` ```pseudocode ``, `` ```bash ``, `` ```text ``).
Inline code uses backticks (`O(log n)`, `malloc()`, `SELECT`).

---

## LaTeX Formula Conventions

- **Display math** (centered, own line): `$$...$$`
- **Inline math** (within text): `$...$`
- Every symbol in every formula must be defined.

### Common CS Notation

| Concept | LaTeX | Rendered |
|---------|-------|----------|
| Big-O | `$O(n \log n)$` | $O(n \log n)$ |
| Theta | `$\Theta(n^2)$` | $\Theta(n^2)$ |
| Omega | `$\Omega(2^n)$` | $\Omega(2^n)$ |
| Summation | `$\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$` | $\sum_{i=1}^{n} i$ |
| Floor/Ceil | `$\lfloor x \rfloor$, $\lceil x \rceil$` | $\lfloor x \rfloor$, $\lceil x \rceil$ |
| Set notation | `$\{x \mid x \in S \land P(x)\}$` | $\{x \mid x \in S\}$ |
| Logarithm | `$\log_2 n$, $\ln n$` | $\log_2 n$, $\ln n$ |
| Arrow | `$\rightarrow$, $\Rightarrow$, $\leftarrow$` | $\rightarrow$, $\Rightarrow$ |
| Sub/Superscript | `$a_i$, $x^{2}$` | $a_i$, $x^{2}$ |
| Matrix | `$\begin{pmatrix} a & b \\ c & d \end{pmatrix}$` | Matrix |
| Probability | `$\Pr[X \geq k] \leq \frac{E[X]}{k}$` | Markov bound |

---

## Comparison Table Format

```markdown
| 维度 | Option A | Option B | Option C |
|------|---------|---------|---------|
| **特性1** | ... | ... | ... |
| **时间复杂度** | $O(n)$ | $O(n \log n)$ | $O(n^2)$ |
| **空间复杂度** | $O(1)$ | $O(n)$ | $O(n)$ |
| **适用场景** | ... | ... | ... |
```

Example dimensions: 连接模型, 可靠性, 顺序保证, 拥塞控制, 头部开销, 适用场景.

---

## Mermaid Diagram Conventions

Choose diagram type by content type. Keep to top-level skeleton only (1-2 layers deep):

| Content | Type | Skeleton |
|---------|------|----------|
| Hierarchical knowledge | `mindmap` | `mindmap\n  root((Topic))\n    Subfield1\n      ConceptA\n    Subfield2\n      ConceptB` |
| Process/protocol flow | `flowchart TD` | `flowchart TD\n    A[State1] -->|action| B[State2]\n    B -->|action| C[State3]` |
| Class/type hierarchy | `classDiagram` | `classDiagram\n    Base <|-- Derived1\n    Base <|-- Derived2` |
| State machine | `stateDiagram-v2` | `stateDiagram-v2\n    [*] --> State1\n    State1 --> State2: event\n    State2 --> [*]` |
| Request-response flow | `sequenceDiagram` | `sequenceDiagram\n    A->>B: Request\n    B-->>A: Response` |
