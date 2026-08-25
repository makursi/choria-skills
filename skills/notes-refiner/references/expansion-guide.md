# Expansion Guide — CS Knowledge Gap Taxonomy & Methodology

This document defines what "academic depth" means for Computer Science notes,
how to identify and classify knowledge gaps, and the methodology for filling them.

---

## Gap Taxonomy — 6 Types of CS Knowledge Gaps

### Type 1: Terminology Gap (术语缺口)

**Symptom**: A CS term is used but never defined.
**Example**: "使用 LRU 缓存淘汰策略" — LRU is mentioned but not explained.

**Expansion required**:
- Full name (Least Recently Used)
- Identify which causal chain element this term belongs to (问题/资源/抽象/机制/策略)
- Definition and core idea
- How it works: 资源约束 → 抽象模型 → 机制运作 → 策略选择
- Complexity and theoretical properties
- Comparison with alternatives in the same layer

### Type 2: Derivation Gap (推导缺口)

**Symptom**: A result or formula is stated without showing the derivation.
**Example**: "因此归并排序的时间复杂度为 O(n log n)" — no recurrence or solving steps.

**Expansion required**:
- State the recurrence: $T(n) = 2T(n/2) + \Theta(n)$
- Solve step-by-step (recursion tree, Master Theorem, or substitution method)
- Show intermediate work, not just the final result
- Explain the intuition behind each step
- Use LaTeX mathematical derivation only; do not rely on pseudocode to convey the derivation

### Type 3: Formal Specification Gap (形式化缺口)

**Symptom**: An algorithm or protocol is described in prose only, without formal structure.
**Example**: "Dijkstra 算法每次选择距离最小的未访问节点" — no formal specification.

**Expansion required**:
- Preconditions and postconditions stated formally
- Loop invariants with justification
- Time and space complexity with full derivation
- Correctness argument (invariant, termination, optimality)
- Comparison with alternatives at the formal level (which invariants differ?)
- Optional: concise pseudocode in the 验证 (Verification) subsection only — not the primary description

### Type 4: Motivation Gap (动机缺口 / 资源约束缺口)

**Symptom**: A concept is presented without motivation or the resource constraints that drove its creation.
**Example**: "TCP 使用三次握手建立连接" — no explanation of WHY three, or what IP-layer
unreliability problem it addresses.

**Expansion required**:
- What resource constraint or physical limitation motivated this design?
- What problem was this designed to solve? (资源层)
- Historical development (who, when, why)
- What alternatives were considered or previously used?
- Why this design won (if applicable)
- How it evolved over time

### Type 5: Relationship Gap (关联缺口)

**Symptom**: Related concepts are presented in isolation with no cross-references.
**Example**: Notes cover "QuickSort" and "Merge Sort" separately — no comparison.

**Expansion required**:
- Comparison table organized by the causal chain elements:
      - 问题层差异 (what distinct problems do they solve best?)
      - 资源层差异 (memory access patterns, cache behavior)
  - 抽象层差异 (divide-and-conquer variants, pivot vs. merge)
  - 机制层差异 (in-place partitioning vs. auxiliary array)
  - 策略层差异 (pivot selection strategies, hybrid approaches)
- When to use which (practical guidance grounded in theoretical properties)
- How they relate theoretically (both are divide-and-conquer; different recurrence structures)
- How they compose or build on each other

### Type 6: Instantiation Gap (实例化缺口)

**Symptom**: Theory is explained but no real-world instantiation is shown.
**Example**: "哈希表提供 O(1) 平均查找时间" — no real system example or theory-practice deviation.

**Expansion required**:
- Real-world systems that use this (e.g., "Python's `dict`, Redis, database hash indexes")
- Theory-practice gap: when do the theoretical assumptions break down?
- Edge cases and failure modes (conceptual, not code-level)
- Performance characteristics in practice (cache effects, collision handling strategies)
- Optional: concise verification code in the 验证 subsection

### Type 7: Problem Gap (问题缺口)

**Symptom**: A concept is described in terms of what it is and how it works, but the
original driving problem it was designed to solve is never stated.
**Example**: "TCP uses a three-way handshake to establish a connection" — no statement
of the problem it solves: how to securely establish bidirectional communication over
an unreliable IP layer while preventing stale duplicate SYNs.

**Distinction from Type 4 (Motivation Gap)**: Type 4 asks "why was it designed this
way?" (design decision level). Type 7 asks "what goal does it achieve?" (problem/need
level). Removal test: remove the problem → the technology has no reason to exist.
Remove the motivation → the technology still exists but you don't know why it looks
this way.

**Expansion required**:
- State the original problem / human or system goal this concept addresses
- If the problem did not exist, explain why this concept would be meaningless
- Distinguish between "the problem" and "the problem context" (situating background)

### Type 8: Trade-off Gap (权衡缺口)

**Symptom**: Multiple approaches / algorithms / strategies are mentioned but not
analytically compared on commensurable dimensions.
**Example**: "Page replacement algorithms include FIFO, LRU, and Clock" — no
comparison on implementation complexity, hit rate, Belady's anomaly, or hardware
support requirements.

**Distinction from Type 5 (Relationship Gap)**: Type 5 asks "how are X and Y
related?" (topological connection — depends-on, is-a, builds-on). Type 8 asks
"how do X and Y conflict/trade-off against shared goals?" (value judgment — fast
vs. cheap, accurate vs. stable). Relationships are structural; trade-offs are
decisional.

**Expansion required**:
- Compare on at least two quantifiable dimensions
- Explain why no simultaneously optimal solution exists (Pareto frontier)
- State which point on the frontier real systems choose and why

---

## 9-Element Cognitive Lens System (九元素认知透镜系统)

The 9-element framework is NOT a rigid template. It is a **constrained cognitive lens
system** — a mental model for describing CS concepts through a fixed causal chain plus
variable analysis layers.

### Causal Chain (Fixed Order — Always Visible as Section Headings)

These 5 elements form a logical dependency chain. They appear as explicit subsection
headings in the output, in this order:

```
问题 (Problem) → 资源 (Resource) → 抽象 (Abstraction) → 机制 (Mechanism) → 策略 (Strategy)
```

**1. 问题 (Problem)** — What human/system goal does this concept achieve?
- Operational test: remove the problem → the technology has no reason to exist.
- Three-state logic: Full Problem (system/algorithm level) / Minimal Problem (language/tool level, 1 sentence) / None (pure definition concepts, skip entirely).
- Distinguish from 资源: "what we want" (Problem) vs. "what constrains us" (Resource).

**2. 资源 (Resource)** — What immutable physical/hardware/reality constraints exist?
- Operational test: remove the constraint → the physical world changes.
- If no meaningful resource constraint applies (rare for CS concepts), write 1 brief sentence and proceed.

**3. 抽象 (Abstraction)** — What model is built on top of resources to address the problem?
- Formal definition, mathematical properties, invariants, pre/post-conditions.
- Embed: **关系 — is-a type hierarchies and contrasts-with structural comparisons.**

**4. 机制 (Mechanism)** — How is the abstraction realized?
- Derivation steps, state transitions, protocol interactions — described with LaTeX derivations and state diagrams.
- Embed: **关系 — depends-on / builds-on dependency chains.**
- Embed: **最小示例 (Minimal Example)** — a concrete instance walkthrough (numbers, trace, step-by-step instantiation of the concept). Not abstract description; show the concept operating on specific data.

**5. 策略 (Strategy)** — What is actually chosen in real systems?
- Which systems adopt which approach, and what do they actually do.
- Embed: **权衡 (Trade-offs)** — multi-dimensional comparison, Pareto frontier logic, why no simultaneously optimal solution exists, which point real systems pick and why.

### Analysis Layers (Flexible — Embedded into Causal Chain Sections)

These 4 elements are NOT visible as separate headings. They are cognitive lenses the
agent uses while composing the 5 causal chain sections, with content embedded at the
designated insertion points:

| Analysis Layer | Embedded Into | Content |
|---------------|---------------|---------|
| **权衡 (Trade-offs)** | 策略 (Strategy) | Why choices differ across dimensions; Pareto frontier; no free lunch |
| **关系 (Relationships)** | 抽象 (is-a, contrasts-with) + 机制 (depends-on, builds-on) | Outgoing edges only — structured list per relationship type; reverse index is internal to agent |
| **最小示例 (Minimal Example)** | 机制 (Mechanism) | Concrete instance walkthrough — specific numbers, step-by-step trace |
| **关键要点 (Key Takeaways)** | End of each concept (no heading) | 1-3 sentences, mandatory for every concept |

### Standalone Optional Element

**验证 (Verification)** — Optional independent subsection at the end of a concept.
Minimal code/pseudocode to check understanding. NOT the knowledge body. No error
handling, no bounds checking, no production structure. Distinct from 最小示例:
- 最小示例 = instantiate the concept (concrete trace, numbers)
- 验证 = check understanding (code/pseudocode)

### Concept Type Weighting Guide

Not every concept fills all 5 causal chain sections equally. Use judgment:

| Concept Type | 问题 | 资源 | 抽象 [嵌入:关系-is-a] | 机制 [嵌入:关系-dep, 最小示例] | 策略 [嵌入:权衡] |
|-------------|------|------|---------------------|--------------------------|----------------|
| Algorithm (Dijkstra) | Full: shortest-path problem in graphs | 图计算是普遍需求 | 贪心选择性质、距离上界不变式 | 优先队列运作、松弛推导、正确性证明; 最小示例: concrete graph trace | 与 Bellman-Ford/A* 对比表; 权衡: 速度 vs 通用性 |
| Protocol (TCP) | Full: reliable bidirectional communication over unreliable network | IP 不可靠/无连接; 带宽-RTT 乘积 | 可靠字节流抽象、连接模型 | 三次握手状态机、seq/ack 机制、滑动窗口推导; 最小示例: packet sequence trace | Reno vs CUBIC vs BBR; 权衡: 公平性 vs 吞吐; 真实系统选择 |
| Theorem (CAP) | Full: consistency guarantee under network partition | 网络分区的物理现实、消息延迟下界 | 一致性/可用性/分区容忍性形式化定义 | 证明思路（构造反例）; 最小示例: low priority | CP vs AP 系统定位; 权衡: 放弃哪一项 |
| Language feature (GIL) | Minimal: thread safety in CPython without complex locking | CPython 引用计数内存管理 | GIL 并发模型抽象 | GIL 获取/释放时间片机制; 最小示例: thread interleave trace | 无 GIL 方案比较; 权衡: simplicity vs concurrency |
| Data structure (B+ Tree) | Full: ordered range queries on disk-resident data | 磁盘 I/O 瓶颈、块设备特性 | 有序索引抽象、范围查询语义 | 节点分裂/合并、填充率约束; 最小示例: insertion trace on concrete tree | PostgreSQL vs InnoDB B+ 树实现差异; 权衡: fanout vs depth

---

## General Expansion Checklist (Apply Where Applicable)

The following list is a flexible prompt. Apply only the dimensions that fit the
concept. Do NOT force all dimensions onto every concept. The **hard requirements**
are the subfield-specific depth standards below — those are non-negotiable.

### Causal Chain Elements (Fixed Order)

- **问题 (Problem)** — State the original goal. Three-state: Full / Minimal (1 sentence) / None (skip entirely). Apply removal test.
- **资源 (Resource)** — Identify immutable physical/hardware/reality constraints. Apply removal test. If none, 1 brief sentence.
- **抽象 (Abstraction)** — Formal definition, mathematical properties, invariants, pre/post-conditions. Embed is-a/contrasts-with relationships.
- **机制 (Mechanism)** — Derivation steps, state transitions, protocol interactions. LaTeX derivations. Embed depends-on/builds-on relationships + 最小示例 (concrete instance walkthrough).
- **策略 (Strategy)** — What real systems actually use. Embed 权衡 (multi-dimensional comparison, Pareto frontier).

### Analysis Layers (Embedded, Not Separate Headings)

- **权衡 (Trade-offs)** — ≥2 quantifiable dimensions. Pareto frontier. Why no simultaneously optimal solution. Real system positioning.
- **关系 (Relationships)** — Outgoing edges only: is-a/contrasts-with → 抽象; depends-on/builds-on → 机制. Structured list per type.
- **最小示例 (Minimal Example)** — Concrete numbers/instance trace in 机制. Show the concept operating step by step.
- **关键要点 (Key Takeaways)** — 1-3 sentences at end of each concept. Mandatory. No heading.

### General Dimensions

- Mathematical derivation — show steps, not just results (LaTeX)
- Time/space complexity — with derivation, best/average/worst case
- Historical context — who, when, what resource constraint motivated it
- Cross-references — link to dependent and related concepts
- Theory-practice gap — when do theoretical assumptions break down in real systems?
- Verification code *(optional)* — minimal, in 验证 subsection, not knowledge body

---

## Subfield-Specific Depth Standards (HARD REQUIREMENTS)

Non-negotiable per subfield. General checklist above is advisory; these are mandatory.
Each entry now includes 问题 layer requirements and 权衡 requirements per the 9-element framework.

| Subfield | Must Cover |
|----------|-----------|
| **Algorithms & Data Structures** | 问题: full problem statement — what computational task does this algorithm/data structure solve? 抽象: formal definition with math notation; preconditions/postconditions + invariants. 机制: time (best/avg/worst/amortized) + space complexity with derivation; correctness proof sketch; 最小示例: step-by-step trace with concrete numbers. 策略 + 权衡: comparison with alternatives on ≥2 quantifiable dimensions; Pareto frontier. Verification code encouraged. |
| **Operating Systems** | 问题: what system-level problem does this mechanism address (e.g., isolation, fair scheduling, protection)? 资源: hardware support (interrupts, MMU, privileged instructions). 抽象: kernel data structures + syscall interface semantics. 机制: kernel implementation principles. 策略 + 权衡: Linux vs Windows vs macOS vs FreeBSD design choices and rationale; why no single design dominates. Historical evolution. Verification code sparing. |
| **Computer Networks** | 问题: what communication problem does this protocol/mechanism solve? 资源: physical bandwidth/RTT/buffer constraints; OSI/TCP-IP layer location. 抽象: service model. 机制: packet/segment format with header fields, state machine or sequence diagram, error handling (timeout, retransmission, duplication); 最小示例: packet sequence trace on concrete scenario. 策略 + 权衡: congestion control algorithm comparison on throughput/fairness/latency; security considerations; relevant RFC(s) cited. Verification code sparing. |
| **Databases** | 问题: what data management problem (consistency, concurrency, durability)? 抽象: formal definition (relational algebra, normal form). 机制: physical storage principles (B+ tree, heap file, WAL); 最小示例: query execution trace. 策略 + 权衡: PostgreSQL vs MySQL/InnoDB vs SQLite design choices; performance (I/O complexity, locking semantics); why different isolation levels exist (trade-off: correctness vs throughput). SQL verification occasionally meaningful. |
| **AI / Machine Learning** | 问题: what prediction/generation/decision problem? Mathematical formulation (loss, objective). 机制: gradient derivation; computational complexity; key assumptions and failure modes. 策略 + 权衡: bias-variance trade-off theoretical analysis; relationship to other models; key papers. Verification code (minimal NumPy-like) encouraged when helpful. |
| **PL & Compilers** | 问题: what language design or compilation problem? 抽象: formal grammar/syntax (BNF); static vs dynamic semantics; type system theoretical implications. 机制: compilation/runtime behavior. 策略 + 权衡: comparison with analogous features in other languages at theoretical level; why language X chose approach A over B. Verification code encouraged for language features. |
| **Distributed Systems** | 问题: what coordination/consistency/reliability problem under partial failure? 抽象: consistency model formalization; failure model assumptions. 机制: consensus protocol details; network partition behavior. 策略 + 权衡: CAP trade-off analysis — which dimension each real system sacrifices; real-world system theoretical design choices. Verification code rarely useful. |
| **Cryptography** | 问题: what security goal (confidentiality, integrity, authentication, non-repudiation)? 抽象: security model + assumptions formalization; attack model + threat analysis. 机制: key size + security parameter theoretical justification; side-channel considerations. 策略 + 权衡: why scheme X chosen over Y (security margin vs performance); implementation pitfalls + warnings (conceptual). Verification code can help understanding. |

---

## Citation Formats

### CS Classic Textbooks (use abbreviated names)

| Abbreviation | Full Title | Authors |
|-------------|-----------|--------|
| CLRS | Introduction to Algorithms, 4th Ed. | Cormen, Leiserson, Rivest, Stein |
| CSAPP | Computer Systems: A Programmer's Perspective, 3rd Ed. | Bryant, O'Hallaron |
| SICP | Structure and Interpretation of Computer Programs | Abelson, Sussman |
| TAOCP | The Art of Computer Programming | Knuth |
| OSTEP | Operating Systems: Three Easy Pieces | Arpaci-Dusseau |
| K&R | The C Programming Language, 2nd Ed. | Kernighan, Ritchie |
| APUE | Advanced Programming in the UNIX Environment | Stevens |
| Dragon Book | Compilers: Principles, Techniques, and Tools | Aho, Lam, Sethi, Ullman |
| AI:AMA | Artificial Intelligence: A Modern Approach, 4th Ed. | Russell, Norvig |
| PRML | Pattern Recognition and Machine Learning | Bishop |
| Deep Learning | Deep Learning | Goodfellow, Bengio, Courville |

### In-Text Citation Style

```
[CLRS, Ch. 12.3] — for textbook chapter references
[Dijkstra, 1959] — for seminal papers
[RFC 793, §3.4] — for protocol specifications
[Python 3.12 docs: asyncio] — for language/library documentation
```

---

## Subfield-Specific Expansion Quick Reference

| Subfield | Must Check For | Common Missing Elements | 9-Element Watch |
|----------|---------------|------------------------|-----------------|
| Algorithms | Complexity analysis, correctness proof | Amortized analysis, lower bounds, formal invariants | 问题 often implicit — make explicit; 权衡 with alternative algorithms |
| Data Structures | All operations, invariants | Deletion (often harder than insertion), persistence semantics | 资源: memory hierarchy impact; 最小示例: concrete insertion/deletion trace |
| OS | Mechanism + policy separation, hardware support | Real scheduling parameters, actual syscall overhead | 问题: what system problem does this solve? 权衡: why Linux chose X over Y |
| Networks | Layering, encapsulation, state machines | Congestion control dynamics, security vulnerabilities | 问题: what communication failure does this handle? 权衡: throughput vs fairness |
| Databases | ACID, isolation levels, query plans | Phantom reads scenarios, write-ahead logging details | 权衡: isolation level vs performance; 问题: what anomaly does this prevent? |
| Compilers | Parse trees, IR, optimization passes | Error recovery in parsers, register allocation theory | 问题: what compilation challenge? 权衡: compile time vs runtime performance |
| AI/ML | Loss function, optimization, generalization | Hyperparameter sensitivity, failure modes, model assumptions | 权衡: bias-variance; 最小示例: concrete forward pass with numbers |
| Cryptography | Security model, attack model, key sizes | Side-channel attacks, implementation pitfalls | 问题: what security property? 权衡: security margin vs performance |
| Distributed Systems | Consistency model, failure model, CAP | Network partition scenarios, consensus edge cases | 问题: what coordination problem? 权衡: which dimension sacrificed under partition? |
| PL Theory | Type safety, evaluation semantics | Soundness/completeness of type systems | 问题: what language design problem? 权衡: expressiveness vs safety |

---

## Quality Heuristics

### An expansion is GOOD when:

1. A student who only reads the expansion (without the original notes) can
   understand the concept fully.
2. Every symbol in every formula is defined.
3. Every claim about performance is backed by analysis (not assertion).
4. Formal definitions are self-consistent and non-circular; mathematical
   properties are derived or cited.
5. Historical claims are attributed to specific people, papers, and years.
6. The expansion connects to at least one other concept in the knowledge base
   (outgoing 关系 edge to another concept).
7. The 问题 → 资源 → 抽象 → 机制 → 策略 causal chain is logical: the problem is
   stated before constraints, constraints drive abstraction design, abstractions
   are realized through mechanisms, strategies are choices made on top of mechanisms.
8. 权衡 is grounded in ≥2 quantifiable dimensions with a clear Pareto frontier;
   no claim of universal superiority without trade-off acknowledgment.
9. 最小示例 (in 机制) is a concrete instance walkthrough with specific numbers or
   data — it shows the concept operating, not describing it.
10. 关键要点 (1-3 sentences, no heading) captures the essence at the end of the concept.
11. Verification code (if present) is minimal and focused — it verifies only
    the core concept understanding, without error handling, bounds checking,
    or production-grade structure. The reader should confirm their understanding,
    not learn API usage.

### An expansion is INSUFFICIENT when:

1. It only rephrases the original notes in different words.
2. It states "X is Y" without explaining why X is Y.
3. It uses the concept to define itself (circular).
4. Formal definitions have logical gaps or mathematical derivations skip steps.
5. It claims "this is the best approach" without comparison or justification
   (missing 权衡).
6. It cites a source that doesn't actually say what's claimed.
7. It jumps directly to 机制/策略 without first establishing 问题/资源/抽象 foundations.
8. The problem statement is missing for a concept that clearly addresses a
   problem (Type 7 gap unfilled).
