# Examples -- CS Note Refinement Walkthroughs

Each example: raw input → Phase 2 gaps → output structure.

---

## Example 1: TCP Three-Way Handshake (Computer Networks)

### Input (`tcp-notes.md`)

```markdown
# TCP 连接建立

## 三次握手
- 客户端发 SYN，服务器回 SYN-ACK，客户端发 ACK
- 为什么要三次？两次不行，四次浪费

## SYN Flood
- 攻击者发大量 SYN，不完成握手 → 半连接队列被占满
- 防御：SYN Cookie

## 状态
CLOSED -> SYN-SENT -> ESTABLISHED
服务器：CLOSED -> LISTEN -> SYN-RCVD -> ESTABLISHED
```

### Phase 2 Gap Table

| # | Concept | Status | Depth | Priority |
|---|---------|--------|-------|----------|
| 1 | Three-Way Handshake | Mentioned | L1 | High |
| 2 | SYN/ACK packet structure | Mentioned | L1 | High |
| 3 | SYN Flood mechanism | Mentioned | L1 | Medium |
| 4 | SYN Cookie | Mentioned | L1 | Medium |
| 5 | TCP state machine | L1 | L1 | Low |

Gap count: 5 (<= 8) — Phase 2 confirmation skipped.

### Phase 3 Expansion snippet — Three-Way Handshake

```
### 2.1 三次握手 (Three-Way Handshake)

**Why** — TCP operates over IP (unreliable, connectionless). Both endpoints
must agree on initial sequence numbers and confirm bidirectional communication
with minimal round trips. Defined in RFC 793 (1981).

**How** — Three steps:
1. Client → Server: SYN, seq=x (client → SYN-SENT)
2. Server → Client: SYN-ACK, seq=y, ack=x+1 (server → SYN-RCVD)
3. Client → Server: ACK, ack=y+1 (both → ESTABLISHED)

Why exactly three? Two: server can't distinguish delayed duplicate SYN from
fresh one. Four: server's SYN and ACK can piggyback.

**What** — Full state machine, pseudocode, comparison with UDP handshake...
```

### Output structure

```
# TCP 连接管理 -- Knowledge Base (知识库)

## 目录
1. TCP 协议概述
2. 三次握手 (Three-Way Handshake)
3. SYN Flood 攻击与防御
4. TCP 状态机

## 概念关系图 [Mermaid sequenceDiagram — top-level only]

## [Main chapters — Why/How/What per concept]

```

---

## Example 2: Red-Black Tree Insertion (Data Structures)

### Input (`rbtree-notes.md`)

```markdown
# 红黑树

性质：
1. 节点是红色或黑色  2. 根是黑色  3. 叶子(NIL)是黑色
4. 红节点的子节点必须是黑色
5. 任意节点到其所有后代叶子的黑色节点数相同

插入：
- 新节点为红色，看父节点颜色
  - 父黑 → 直接插入
  - 父红 → 看叔叔
    - 叔叔红 → 变色
    - 叔叔黑 → 旋转+变色 (LL/LR/RR/RL)
```

### Phase 3 Expansion snippet — RB-Insert-Fixup

```
### 4. 插入操作 (Red-Black Tree Insertion)

**Why** — Standard BST insertion may violate RB properties. New nodes are
colored RED: only risks violating "no two consecutive reds," easier to fix
than black-height. At most 2 rotations needed (AVL: up to O(log n)).

**How** — Insert as BST, color RED, then RB-INSERT-FIXUP loops:
Case 1: Uncle RED → recolor, move up
Case 2: Uncle BLACK, inner grandchild → rotate to outer
Case 3: Uncle BLACK, outer grandchild → rotate + recolor

**What** — Pseudocode (RB-INSERT + RB-INSERT-FIXUP), complexity O(log n),
comparison with AVL tree...
```

### Output structure

```
# 红黑树 -- Knowledge Base (知识库)

## 目录
1. 二叉搜索树回顾
2. 红黑树定义与性质
3. 旋转操作
4. 插入操作
5. 删除操作概览
6. 红黑树 vs AVL 树

## 概念关系图 [Mermaid classDiagram — top-level only]

## [Main chapters — Why/How/What, no tags]

```

---

## Example 3: Multi-File Short Notes — OS Memory Management

### Input (3 files, ~30 lines total)

**File 1: `virtual-memory.md`**
```
虚拟内存: 每个进程独立地址空间, MMU 地址翻译, 页表, TLB 是页表的缓存
```

**File 2: `page-replacement.md`**
```
页面置换: FIFO / LRU / Clock. FIFO 有 Belady 异常. LRU 实现困难.
```

**File 3: `page-fault.md`**
```
缺页中断: 访问页面 → 页表项 invalid → 缺页中断 → OS 从磁盘读入 → 更新页表 → 重新执行
```

### Output structure

```
# 操作系统内存管理 -- Knowledge Base (知识库)

## 目录
1. 内存管理概述
2. 虚拟内存 (Virtual Memory)
   2.1 核心思想与动机
   2.2 MMU 与地址翻译过程
   2.3 页表结构与多级页表
   2.4 TLB 原理与命中率分析
3. 缺页中断 (Page Fault)
   3.1 处理流程详解
   3.2 Minor vs Major Page Fault
4. 页面置换算法 (Page Replacement)
   4.1 FIFO 与 Belady 异常
   4.2 LRU 实现方法
   4.3 Clock 算法
   4.4 算法对比表
5. 现代操作系统实例
   5.1 Linux Buddy System + Slab
   5.2 Linux kswapd 与 LRU lists

## 概念关系图 [Mermaid flowchart — top-level only]

## [Main chapters — Why/How/What]

```

---

## Pattern Summary

1. **Short notes drive deep expansion** — brief source → thorough output. Every Mentioned/Assumed concept triggers expansion.
2. **Multi-file synthesis** — cross-file concept relationships merge into unified hierarchy.
3. **Gap-driven, not volume-driven** — every expansion fills a specific gap. No padding.
4. **Why → How → What** — motivation first, then mechanism, then definition/code/comparisons.
5. **CS signatures** — pseudocode, complexity analysis, protocol comparison tables, RFC/citation references.
6. **Source-bounded scope** — fill gaps in existing notes. No new topic branches.
7. **Minimal metadata** — header has date + source files only. No difficulty/prerequisite/type tags.
8. **Clean academic style** — no emoji, no decorative symbols. Structured knowledge only.
