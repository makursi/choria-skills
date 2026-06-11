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

<!-- Each chapter: 资源 → 抽象 → 机制 → 策略 → 验证(可选) -->

---
```

---

## Section Template

Every content section follows this pattern:

```markdown
## N. Section Title

### 资源 (Resource)
底层物理约束和硬件基础。什么资源限制催生了这个抽象？容量、速度、成本边界在哪？

### 抽象 (Abstraction)
在资源之上建立了什么模型？形式化定义、数学性质、不变量、前置/后置条件。

### 机制 (Mechanism)
抽象如何被实现？推导步骤、状态转移、协议交互——用 LaTeX 数学推导和状态图描述。

### 策略 (Strategy)
有哪些可选方案？如何权衡？真实系统中谁采用了什么方案？为什么？
理论假设在实践中的偏差。

### 验证 (Verification) *(可选)*
极简代码/伪代码，仅用于验证对上述原理的理解。
非知识主体。不含错误处理、边界检查、生产代码结构。

### N.m 小结 (Summary)

[1-3 sentence key takeaways — only for high/medium priority concepts]
```

Sections open directly with 资源. No difficulty badges, prerequisite labels, or type tags.
Concept ordering (prerequisites before dependents) signals learning dependencies.

The 验证 (Verification) subsection is optional and appears only when code/pseudocode
materially deepens understanding. Subfield guidance:
- **Algorithms & Data Structures, AI/ML, PL & Compilers**: usually include verification
- **OS, Networks, Databases, Distributed Systems, Cryptography**: include sparingly
- **Low-priority concepts (Light tier)**: never include verification

---

## 验证代码约定

验证代码是可选辅助材料，非知识主体。使用时遵守：

- **位置**: 放在 `### 验证 (Verification)` 小节，在 资源→抽象→机制→策略 四层之后
- **规则**: 极简，仅验证核心概念理解。不含错误处理、边界检查、生产代码结构
- **语言标记**: 所有代码块必须带语言标签 (```python, ```c, ```typescript, ```sql, ```pseudocode)
- **语言选择**: AI/ML → Python; 底层CS → C; 应用层 → TypeScript; SQL → SQL

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
