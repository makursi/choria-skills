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

<!-- Each chapter: 问题 → 资源 → 抽象 → 机制 → 策略 → 验证(可选) → 关键要点 -->

---
```

---

## Section Template

Every content section follows this pattern:

```markdown
## N. Section Title

### 问题 (Problem)
该概念要达成的系统/人类目标。操作定义：移除该问题 → 该技术没有存在的理由。
三态：Full（系统/算法级）、Minimal（语言/工具级，一句）、None（纯定义级，跳过）。

### 资源 (Resource)
不可改变的物理/硬件/现实约束。操作定义：移除该约束 → 世界物理性质改变。

### 抽象 (Abstraction)
在资源和问题之上建立的模型。形式化定义、数学性质、不变量、前置/后置条件。
[融入: 关系 — is-a 类型层级, contrasts-with 结构对比]

### 机制 (Mechanism)
抽象如何被实现。推导步骤、状态转移、协议交互——用 LaTeX 数学推导和状态图描述。
[融入: 关系 — depends-on / builds-on 依赖链]
[融入: 最小示例 — 具体实例的逐步推演，具体数值，展示概念在真实数据上的运作]

### 策略 (Strategy)
真实系统实际采用的方案。谁采用了什么方案，为什么。
[融入: 权衡 — 多维度对比（≥2 可量化维度），帕累托前沿，为何不存在同时最优的方案，真实系统选择了哪个点]

### 验证 (Verification) *(可选)*
极简代码/伪代码，仅验证对原理的理解。非知识主体。
不含错误处理、边界检查、生产代码结构。必须带语言标签。

*[1-3 句关键要点，无标题，每个概念强制出现]*
```

Sections open directly with 问题. No difficulty badges, prerequisite labels, or type tags.
Concept ordering (prerequisites before dependents) signals learning dependencies.

The causal chain sections (问题 → 资源 → 抽象 → 机制 → 策略) are always in this fixed order.
Analysis layers (权衡, 关系, 最小示例, 关键要点) are NOT separate headings — they are embedded
into the causal chain sections as shown. 验证 is the only optional standalone subsection.
关键要点 is mandatory for every concept, appears at the end with no heading.

---

## 验证代码约定

验证代码是可选辅助材料，非知识主体。使用时遵守：

- **位置**: 放在 `### 验证 (Verification)` 小节，在 问题→资源→抽象→机制→策略 五层之后、关键要点之前
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
