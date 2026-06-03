# Examples -- CS Note Refinement Walkthroughs

This file provides annotated examples of the Notes Refiner skill in action.
Each example shows: raw input notes → Phase analysis → output knowledge base
structure.

---

## Example 1: TCP Three-Way Handshake (Computer Networks)

### Input -- Raw Classroom Notes (`tcp-notes.md`)

```markdown
# TCP 连接建立

## 三次握手
- 客户端发 SYN
- 服务器回 SYN-ACK
- 客户端发 ACK
- 为什么要三次？两次不行，四次浪费

## SYN Flood
- 攻击者发大量 SYN，不完成握手
- 半连接队列被占满
- 防御：SYN Cookie

## 状态
CLOSED -> SYN-SENT -> ESTABLISHED
服务器：CLOSED -> LISTEN -> SYN-RCVD -> ESTABLISHED
```

### Phase 1 Analysis

```
检测到主题: 计算机网络 -- TCP 连接管理
概念清单:
  1. TCP (协议) -- 假定已知
  2. 三次握手 (Three-Way Handshake) -- 提及但未完整解释
  3. SYN 报文 -- 提及但未定义
  4. SYN-ACK 报文 -- 提及但未定义
  5. ACK 报文 -- 提及但未定义
  6. SYN Flood 攻击 -- 提及但缺少详细机制
  7. 半连接队列 (Half-Open Connection Queue) -- 命名但未解释
  8. SYN Cookie -- 命名，无工作原理
  9. TCP 状态机 -- 列出部分状态但不完整
```

### Phase 2 Gap Table (excerpt)

| # | Concept | Status | Depth | Priority | Suggested Expansion |
|---|---------|--------|-------|----------|---------------------|
| 1 | Three-Way Handshake | Mentioned | L1 | High | Full mechanism, why 3 (not 2 or 4), sequence numbers, state machine |
| 2 | SYN packet | Mentioned | L1 | High | TCP header structure, flags field, sequence number initialization |
| 3 | SYN Flood | Mentioned | L1 | Medium | Attack mechanism detail, backlog queue, impact |
| 4 | SYN Cookie | Mentioned | L1 | Medium | Cryptographic defense mechanism, how it avoids state |
| 5 | TCP state machine | L1 | L1 | Low | Full state diagram, all transitions, RFC 793 reference |

Gap count: 5 (<= 8) -- Phase 2 confirmation skipped. Proceeding to Phase 3.

### Phase 3 Expansion -- Example Concept (Three-Way Handshake)

```markdown
### 2.1 三次握手 (Three-Way Handshake)

**Why** -- TCP operates over IP, which is an unreliable, connectionless protocol.
To provide reliable, ordered delivery, both endpoints must agree on initial
sequence numbers and confirm bidirectional communication is possible. The
three-way handshake, defined in RFC 793 (1981), solves this problem with
minimal round trips.

**How** -- The handshake proceeds in three steps:
1. Client → Server: SYN, seq=x (client enters SYN-SENT)
2. Server → Client: SYN-ACK, seq=y, ack=x+1 (server enters SYN-RCVD)
3. Client → Server: ACK, ack=y+1 (both enter ESTABLISHED)

Why exactly three? Two is insufficient: with only SYN and SYN-ACK, the server
cannot distinguish a delayed duplicate SYN from a fresh one, leading to
half-open connections. Four is redundant: the server's SYN and ACK can be
piggybacked into a single segment.

**What** -- ...
```

### Output Structure

```
# TCP 连接管理 -- Knowledge Base (知识库)

> **生成日期**: 2026-06-02
> **源文件**: tcp-notes.md

## 目录
1. TCP 协议概述
2. 三次握手 (Three-Way Handshake)
3. SYN Flood 攻击与防御
4. TCP 状态机

## 概念关系图 [Mermaid sequenceDiagram]

## [Main chapters -- Why/How/What per concept, no section tags]

## 术语表
| SYN | 同步序列号 | TCP 连接建立时的初始报文 ... |
| SYN Cookie | SYN Cookie | 一种无状态防御 SYN Flood 的密码学技术 ... |

## 交叉引用索引
```

---

## Example 2: Red-Black Tree Insertion (Data Structures)

### Input -- Raw Classroom Notes (`rbtree-notes.md`)

```markdown
# 红黑树

性质：
1. 节点是红色或黑色
2. 根是黑色
3. 叶子(NIL)是黑色
4. 红节点的子节点必须是黑色
5. 任意节点到其所有后代叶子的黑色节点数相同

插入：
- 新节点为红色
- 看父节点颜色
  - 父黑 → 直接插入
  - 父红 → 要看叔叔
    - 叔叔红 → 变色
    - 叔叔黑 → 旋转+变色
      - LL: 右旋
      - RR: 左旋
      - LR: 左旋再右旋
      - RL: 右旋再左旋

```

### Phase 3 Expansion -- Example Concept (RB-Insert-Fixup)

```markdown
### 4. 插入操作 (Red-Black Tree Insertion)

**Why** -- A standard BST insertion may violate Red-Black properties.
The fixup procedure restores them while preserving the O(log n) guarantee.
New nodes are always colored RED because it only risks violating property 4
(no two consecutive reds), which is easier to fix than property 5 (black-height).

**How** -- Insert as in a regular BST, color the new node RED, then call
RB-INSERT-FIXUP. The fixup loops while the violation (red parent) persists:

- Case 1: Uncle is RED → recolor parent, uncle black; grandparent red; move up
- Case 2: Uncle is BLACK, node is inner grandchild → rotate to outer position
- Case 3: Uncle is BLACK, node is outer grandchild → rotate + recolor

At most 2 rotations are needed (compare AVL: up to O(log n) rotations).

**What** -- ...

```pseudocode
RB-INSERT(T, z)
 1  y ← T.nil
 2  x ← T.root
 3  while x ≠ T.nil
 4      y ← x
 5      if z.key < x.key: x ← x.left
 6      else: x ← x.right
 7  z.p ← y
 8  if y == T.nil: T.root ← z
 9  else if z.key < y.key: y.left ← z
10  else: y.right ← z
11  z.left ← T.nil; z.right ← T.nil
12  z.color ← RED
13  RB-INSERT-FIXUP(T, z)
```

| 操作 | 平均 | 最坏 |
|------|------|------|
| 插入 (含 fixup) | O(log n) | O(log n) |
| 旋转次数 | O(1) | <= 2 |
| 空间 | O(1) auxiliary | O(1) auxiliary |

### 红黑树 vs AVL 树

| 维度 | 红黑树 | AVL 树 |
|------|--------|--------|
| 平衡条件 | 宽松 (5 条性质) | 严格 (|BF| <= 1) |
| 插入旋转次数 | <= 2 | <= 1 (但需回溯至根) |
| 查找性能 | 稍慢 (树稍高) | 稍快 (严格平衡) |
| 插入性能 | 更快 (旋转少) | 较慢 (回溯重平衡) |
| 实际使用 | Linux CFS, C++ std::map, Java TreeMap | 查找密集型应用 |
```

### Output Structure

```
# 红黑树 -- Knowledge Base (知识库)

> **生成日期**: 2026-06-02
> **源文件**: rbtree-notes.md

## 目录
1. 二叉搜索树回顾
2. 红黑树定义与性质
3. 旋转操作
4. 插入操作
5. 删除操作概览
6. 红黑树 vs AVL 树

## 概念关系图 [Mermaid classDiagram]

## [Main chapters -- Why/How/What, no tags]

## 术语表
## 交叉引用索引
```

Scope note: "删除操作" is included despite the student writing "太复杂了不想记"
because it IS mentioned in the source. The expansion provides a conceptual
overview rather than full pseudocode, respecting the student's depth preference
while ensuring the knowledge base is complete.

---

## Example 3: Multi-File Short Notes -- OS Memory Management

### Input -- Three Short Notes Files

**File 1: `virtual-memory.md`**
```markdown
# 虚拟内存
每个进程有自己的地址空间
MMU 负责地址翻译
页表存在内存里
TLB 是页表的缓存
```

**File 2: `page-replacement.md`**
```markdown
# 页面置换
- FIFO
- LRU
- Clock
FIFO 有 Belady 异常
LRU 实现困难
```

**File 3: `page-fault.md`**
```markdown
缺页中断处理流程：
1. 访问页面
2. 页表项 invalid → 触发缺页中断
3. OS 从磁盘读入页面
4. 更新页表
5. 重新执行指令
```

### Phase 1 Summary

```
文件数: 3, 总行数: ~30
概念清单: 10 个 (虚拟内存, MMU, 页表, TLB, FIFO, LRU, Clock,
         Belady异常, 缺页中断, 页表项)
检测到子领域: 操作系统 -- 内存管理
评估: 笔记非常简短 (L1-L2)，几乎所有概念都需要深度扩展
缺口数量: ~8 -- Phase 1 + Phase 4 确认
```

### Phase 4 Structure

```
# 操作系统内存管理 -- Knowledge Base (知识库)

> **生成日期**: 2026-06-02
> **源文件**: virtual-memory.md, page-replacement.md, page-fault.md

## 目录

1. 内存管理概述
   1.1 为什么需要内存管理？
   1.2 地址类型：物理地址 vs 虚拟地址

2. 虚拟内存 (Virtual Memory)
   2.1 核心思想与动机
   2.2 MMU 与地址翻译过程 [流程图]
   2.3 页表 (Page Table) 结构与多级页表
   2.4 TLB 原理与命中率分析

3. 缺页中断 (Page Fault)
   3.1 缺页中断处理流程详解 [来自 file3]
   3.2 Minor vs Major Page Fault
   3.3 性能影响与优化

4. 页面置换算法 (Page Replacement)
   4.1 FIFO 与 Belady 异常 [来自 file2]
   4.2 LRU -- 实现方法
   4.3 Clock 算法 (Second Chance)
   4.4 算法对比表

5. 现代操作系统实例
   5.1 Linux: Buddy System + Slab Allocator
   5.2 Linux: kswapd 与 LRU lists
```

---

## Pattern Summary

1. **Short notes drive deep expansion** -- the source can be brief, the output
   must be thorough. Every Mentioned or Assumed concept triggers expansion.
2. **Multi-file synthesis** -- cross-file concept relationships are automatically
   merged into a unified hierarchy.
3. **Gap-driven, not volume-driven** -- every expansion fills a specific
   identified gap. No padding.
4. **Why → How → What** -- every concept tells the story in order: motivation
   first, then mechanism, then definition.
5. **CS signatures** -- pseudocode, complexity analysis, protocol comparison
   tables, RFC citations run throughout.
6. **Minimal metadata** -- header has only date and source files. Sections have no
   difficulty/prerequisite/type tags.
7. **Source-bounded scope** -- expansions fill gaps in existing notes. No new
   topic branches.
8. **Clean academic style** -- no decorative symbols. Just structured knowledge.
