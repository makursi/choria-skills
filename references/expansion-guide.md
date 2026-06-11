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
- Identify which of the four layers this term belongs to (资源/抽象/机制/策略)
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
- Comparison table organized by the four layers:
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

---

## 资源 → 抽象 → 机制 → 策略 Structure (Four-Layer Framework)

Every expanded concept in Phase 3 must follow this four-section structure, in order:

```markdown
### N.m Concept Name

**资源 (Resource)** — 底层物理约束和硬件基础是什么？
  什么资源限制催生了这个抽象？容量、速度、成本的边界在哪？

**抽象 (Abstraction)** — 在资源之上建立了什么模型？
  形式化定义、数学性质、不变量、前置/后置条件。

**机制 (Mechanism)** — 抽象如何被实现？
  推导步骤、状态转移、协议交互——用 LaTeX 数学推导和状态图描述。

**策略 (Strategy)** — 有哪些可选方案？如何权衡？
  真实系统中谁采用了什么方案？为什么？理论假设在实践中的偏差？

**验证 (Verification)** *(可选)* — 极简代码/伪代码，仅验证对原理的理解。
  非知识主体。不含错误处理、边界检查、生产代码结构。
```

The four sections are always in 资源 → 抽象 → 机制 → 策略 order. Not all concepts fill
every section equally — use judgment:

| Concept Type | 资源层着重点 | 抽象层着重点 | 机制层着重点 | 策略层着重点 |
|-------------|------------|------------|------------|------------|
| Algorithm (Dijkstra) | 图计算需求、最短路径问题的普遍性 | 贪心选择性质、距离上界不变式 | 优先队列运作、松弛操作推导、正确性证明 | 与 Bellman-Ford/A* 的理论对比、适用图类型 |
| Protocol (TCP) | IP 网络不可靠、带宽-RTT 乘积约束 | 可靠字节流抽象、连接模型 | 三次握手状态转移、序列号/确认号机制、滑动窗口推导 | Reno vs CUBIC vs BBR 拥塞控制策略选择 |
| Theorem (CAP) | 分布式系统中网络分区的物理现实 | 一致性/可用性/分区容忍性的形式化定义 | 证明思路（构造反例） | 真实系统在 CAP 谱系中的定位 (CP vs AP) |
| Language feature (Python GIL) | CPython 内存管理需求、引用计数限制 | GIL 的并发模型抽象 | GIL 获取/释放的时间片机制 | 无 GIL 方案比较 (自由线程/子进程/async) |
| Data structure (B+ Tree) | 磁盘 I/O 瓶颈、块设备特性 | 有序索引抽象、范围查询语义 | 节点分裂/合并机制、填充率约束推导 | PostgreSQL vs InnoDB B+ 树实现策略差异 |

---

## General Expansion Checklist (Apply Where Applicable)

The following list is a flexible prompt. Apply only the dimensions that fit the
concept. Do NOT force all dimensions onto every concept.

- Definition and first principles — start from fundamentals
- Mathematical derivation — show steps, not just results (LaTeX)
- Formal specification — preconditions, postconditions, invariants
- Time/space complexity — with derivation, best/average/worst case
- Historical context — who, when, what resource constraint motivated it
- Comparison with alternatives — table format if >2 alternatives, organized by four layers
- Cross-references — link to dependent and related concepts
- Resource constraint analysis — what physical/hardware limits drive the design
- Theory-practice gap — when do theoretical assumptions break down in real systems?
- Verification code *(optional)* — minimal, in 验证 subsection, not knowledge body

---

## Subfield-Specific Depth Standards (HARD REQUIREMENTS)

Non-negotiable per subfield. General checklist above is advisory; these are mandatory.

| Subfield | Must Cover |
|----------|-----------|
| **Algorithms & Data Structures** | Formal definition with math notation; preconditions/postconditions + invariants; time (best/avg/worst/amortized) + space complexity with derivation; correctness proof sketch; comparison with alternatives at formal level; step-by-step example (concrete numbers, not code). Verification code encouraged. |
| **Operating Systems** | 资源层: hardware support (interrupts, MMU, privileged instructions); 抽象层: kernel data structures + syscall interface semantics; 机制层: kernel implementation principles; 策略层: Linux vs Windows vs macOS vs FreeBSD design choices and rationale; historical evolution. Verification code sparing. |
| **Computer Networks** | 资源层: physical bandwidth/RTT/buffer constraints; OSI/TCP-IP layer location; 抽象层: service model; 机制层: packet/segment format with header fields, state machine or sequence diagram, error handling (timeout, retransmission, duplication); 策略层: congestion control algorithm comparison; security considerations; relevant RFC(s) cited. Verification code sparing. |
| **Databases** | 抽象层: formal definition (relational algebra, normal form); 机制层: physical storage principles (B+ tree, heap file, WAL); 策略层: PostgreSQL vs MySQL/InnoDB vs SQLite design choices; performance (I/O complexity, locking semantics). SQL verification occasionally meaningful. |
| **AI / Machine Learning** | Mathematical formulation (loss, objective); gradient derivation; bias-variance trade-off theoretical analysis; computational complexity; key assumptions and failure modes; relationship to other models; key papers. Verification code (minimal NumPy-like) encouraged when helpful. |
| **PL & Compilers** | Formal grammar/syntax (BNF); static vs dynamic semantics; type system theoretical implications; compilation/runtime behavior; comparison with analogous features in other languages at theoretical level. Verification code encouraged for language features. |
| **Distributed Systems** | Consistency model formalization; failure model assumptions; CAP trade-off analysis; network partition behavior; consensus protocol details; real-world system theoretical design choices. Verification code rarely useful. |
| **Cryptography** | Security model + assumptions formalization; attack model + threat analysis; key size + security parameter theoretical justification; side-channel considerations; implementation pitfalls + warnings (conceptual). Verification code can help understanding. |

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

| Subfield | Must Check For | Common Missing Elements |
|----------|---------------|------------------------|
| Algorithms | Complexity analysis, correctness proof | Amortized analysis, lower bounds, formal invariants |
| Data Structures | All operations, invariants | Deletion (often harder than insertion), persistence semantics |
| OS | Mechanism + policy separation, hardware support | Real scheduling parameters, actual syscall overhead |
| Networks | Layering, encapsulation, state machines | Congestion control dynamics, security vulnerabilities |
| Databases | ACID, isolation levels, query plans | Phantom reads scenarios, write-ahead logging details |
| Compilers | Parse trees, IR, optimization passes | Error recovery in parsers, register allocation theory |
| AI/ML | Loss function, optimization, generalization | Hyperparameter sensitivity, failure modes, model assumptions |
| Cryptography | Security model, attack model, key sizes | Side-channel attacks, implementation pitfalls |
| Distributed Systems | Consistency model, failure model, CAP | Network partition scenarios, consensus edge cases |
| PL Theory | Type safety, evaluation semantics | Soundness/completeness of type systems |

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
6. The expansion connects to at least one other concept in the knowledge base.
7. The 资源 → 抽象 → 机制 → 策略 flow is logical — resource constraints drive
   abstraction design, abstractions are realized through mechanisms, strategies
   are choices made on top of mechanisms.
8. Verification code (if present) is minimal and focused — it verifies only
   the core concept understanding, without error handling, bounds checking,
   or production-grade structure. The reader should confirm their understanding,
   not learn API usage.

### An expansion is INSUFFICIENT when:

1. It only rephrases the original notes in different words.
2. It states "X is Y" without explaining why X is Y.
3. It uses the concept to define itself (circular).
4. Formal definitions have logical gaps or mathematical derivations skip steps.
5. It claims "this is the best approach" without comparison or justification.
6. It cites a source that doesn't actually say what's claimed.
7. It jumps directly to 机制/策略 without first establishing 资源/抽象 foundations.
