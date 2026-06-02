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
- Definition and core idea
- How it works (access pattern, eviction rule)
- Implementation approaches (linked list + hash map, or `LinkedHashMap`)
- Time complexity of operations
- Comparison with other eviction policies (FIFO, LFU, ARC)

### Type 2: Derivation Gap (推导缺口)

**Symptom**: A result or formula is stated without showing the derivation.
**Example**: "因此归并排序的时间复杂度为 O(n log n)" — no recurrence or solving steps.

**Expansion required**:
- State the recurrence: $T(n) = 2T(n/2) + \Theta(n)$
- Solve step-by-step (recursion tree, Master Theorem, or substitution method)
- Show intermediate work, not just the final result
- Explain the intuition behind each step

### Type 3: Algorithmic Gap (算法缺口)

**Symptom**: An algorithm is described in prose only, without structured steps.
**Example**: "Dijkstra 算法每次选择距离最小的未访问节点" — no full algorithm.

**Expansion required**:
- Structured pseudocode with line numbers
- Walk-through on a small concrete example (e.g., a 5-node graph)
- Time and space complexity with derivation
- Correctness argument (invariant, termination, optimality)
- Common implementation pitfalls

### Type 4: Context Gap (背景缺口)

**Symptom**: A concept is presented without motivation or historical context.
**Example**: "TCP 使用三次握手建立连接" — no explanation of WHY three, not two or four.

**Expansion required**:
- What problem was this designed to solve?
- Historical development (who, when, why)
- What alternatives were considered or previously used?
- Why this design won (if applicable)
- How it evolved over time

### Type 5: Relationship Gap (关联缺口)

**Symptom**: Related concepts are presented in isolation with no cross-references.
**Example**: Notes cover "QuickSort" and "Merge Sort" separately — no comparison.

**Expansion required**:
- Comparison table (time, space, stability, in-place, cache behavior)
- When to use which (practical guidance)
- How they relate theoretically (both are divide-and-conquer)
- How they compose or build on each other

### Type 6: Application Gap (应用缺口)

**Symptom**: Theory is explained but no practical application or code is shown.
**Example**: "哈希表提供 O(1) 平均查找时间" — no code or real-world use case.

**Expansion required**:
- Working code example in a relevant language
- Real-world systems that use this (e.g., "Python's `dict`, Redis, database hash indexes")
- Edge cases and failure modes
- Performance characteristics in practice (cache effects, collision handling)

---

## Academic Depth Standards

### What "Academic Depth" Means per CS Subfield

#### Algorithms & Data Structures

A concept has academic depth when it covers:
- [ ] Formal definition with mathematical notation
- [ ] Full pseudocode of all operations (not just the "interesting" one)
- [ ] Time complexity analysis: best / average / worst / amortized
- [ ] Space complexity analysis
- [ ] Correctness proof (at least a sketch with loop invariants)
- [ ] Comparison with alternative data structures/algorithms
- [ ] Implementation considerations (cache, concurrency, real-world constants)
- [ ] At least one concrete example walked through step-by-step

#### Operating Systems

A concept has academic depth when it covers:
- [ ] The problem the OS mechanism solves (why user-space can't do it)
- [ ] Hardware support required (interrupts, MMU, privileged instructions)
- [ ] Data structures used inside the kernel
- [ ] System call interface and usage example
- [ ] Performance implications (context switch cost, cache pollution)
- [ ] How major OSes implement it (Linux vs Windows vs macOS vs FreeBSD)
- [ ] Historical evolution (e.g., from CTSS to CFS to EEVDF in Linux scheduling)

#### Computer Networks

A concept has academic depth when it covers:
- [ ] Where it sits in the OSI/TCP-IP stack
- [ ] Packet/segment format (header fields explained)
- [ ] State machine or sequence diagram of the protocol
- [ ] Error handling and edge cases (timeout, retransmission, duplication)
- [ ] Security considerations
- [ ] Relevant RFC(s) cited
- [ ] Comparison with alternative protocols at the same layer

#### Databases

A concept has academic depth when it covers:
- [ ] Formal definition (relational algebra, normal form, etc.)
- [ ] SQL examples (DDL + DML + query with EXPLAIN)
- [ ] Physical storage implications (B+ tree, heap file, WAL)
- [ ] Performance characteristics (I/O complexity, locking behavior)
- [ ] How major DBMSs implement it (PostgreSQL, MySQL/InnoDB, SQLite)

#### AI / Machine Learning

A concept has academic depth when it covers:
- [ ] Mathematical formulation (loss function, optimization objective)
- [ ] Derivation of update rules (gradients)
- [ ] Bias-variance trade-off analysis
- [ ] Pseudocode of training and inference
- [ ] Computational complexity
- [ ] Key assumptions and when they break
- [ ] Relationship to other models/algorithms
- [ ] Key papers that introduced or advanced the concept

#### Programming Languages & Compilers

A concept has academic depth when it covers:
- [ ] Formal grammar/syntax (BNF or similar notation)
- [ ] Static vs. dynamic semantics
- [ ] Type system implications
- [ ] Compilation/runtime behavior (what the compiler/interpreter actually does)
- [ ] Code examples showing both common and edge-case usage
- [ ] Comparison with analogous features in other languages

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
| Algorithms | Complexity analysis, correctness proof | Amortized analysis, lower bounds |
| Data Structures | All operations, invariants | Deletion (often harder than insertion), persistence |
| OS | Mechanism + policy separation, hardware support | Real scheduling parameters, actual syscall overhead |
| Networks | Layering, encapsulation, state machines | Congestion control dynamics, security vulnerabilities |
| Databases | ACID, isolation levels, query plans | Phantom reads scenarios, write-ahead logging details |
| Compilers | Parse trees, IR, optimization passes | Error recovery in parsers, register allocation |
| AI/ML | Loss function, optimization, generalization | Hyperparameter sensitivity, failure modes |
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
4. Code examples are minimal but complete (can run with minimal modification).
5. Historical claims are attributed to specific people, papers, and years.
6. The expansion connects to at least one other concept in the knowledge base.

### An expansion is INSUFFICIENT when:

1. It only rephrases the original notes in different words.
2. It states "X is Y" without explaining why X is Y.
3. It uses the concept to define itself (circular).
4. It includes code that is obviously broken.
5. It claims "this is the best approach" without comparison or justification.
6. It cites a source that doesn't actually say what's claimed.
