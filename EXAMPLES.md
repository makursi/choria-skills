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

**资源 (Resource)** — TCP 运行于 IP 之上。IP 是不可靠、无连接的尽最大努力交付协议：
数据包可能丢失、重复、乱序。两个端点必须在不可靠的 IP 层之上建立双向可靠的通信，
且必须在最少的往返次数内完成。

**抽象 (Abstraction)** — TCP 提供可靠的、面向连接的字节流抽象。连接是一个
双方约定的状态对 (state pair)，由四元组 (src IP, src port, dst IP, dst port)
唯一标识。三次握手的目标：同步初始序列号 (ISN) 并确认双向通信能力。

**机制 (Mechanism)** — 三步状态转移 (RFC 793, 1981):
1. Client → Server: SYN, seq=x (client → SYN-SENT)
2. Server → Client: SYN-ACK, seq=y, ack=x+1 (server → SYN-RCVD)
3. Client → Server: ACK, ack=y+1 (both → ESTABLISHED)

为什么恰好三次？两次握手的问题：服务器无法区分延迟的重复 SYN 和新 SYN——
可能导致半开连接。四次不必要：服务器的 SYN 和 ACK 可以合并 (piggyback)。
三次是满足"双向确认 + 防重复"的最少次数。

**策略 (Strategy)** — 初始序列号 (ISN) 的选择策略至关重要：
- 固定 ISN → 攻击者可预测，导致连接劫持
- 随机 ISN → 现代 TCP 实现的标准做法
- SYN Cookie → 无状态防御 SYN Flood 的策略变体
真实系统：Linux 内核使用随机 ISN + SYN Cookie 混合策略。

**验证 (Verification)** — TCP 状态机构成序列图，见概念关系图 (Mermaid)。
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

## [Main chapters — 资源/抽象/机制/策略 per concept]

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

**资源 (Resource)** — 二叉搜索树 (BST) 的插入在最坏情况下退化为 O(n) 高度。
平衡需求源于对 O(log n) 查找/插入/删除的保证。红黑树选择放宽 AVL 的严格平衡
约束，以换取更少的旋转次数（最多 2 次 vs. AVL 的 O(log n) 次）。

**抽象 (Abstraction)** — 红黑树是满足五条性质的自平衡 BST。核心不变量：
黑高度在所有根到叶路径上相同。新节点着红色：只可能违反性质 4（连续红色），
修正成本低于修复黑高度失衡。

**机制 (Mechanism)** — RB-INSERT-FIXUP 循环分析三种情况：
- Case 1: 叔叔为红色 → 重新着色父、叔、祖父，问题点上升至祖父
- Case 2: 叔叔为黑色，节点为内侧孙子 → 旋转父节点转化为 Case 3
- Case 3: 叔叔为黑色，节点为外侧孙子 → 旋转祖父 + 重新着色，终止
每次迭代要么上升两层（Case 1），要么在至多 2 次旋转后终止。
总旋转数 ≤ 2，总时间复杂度 O(log n)。

**策略 (Strategy)** — 红黑树 vs AVL 树的取舍：
- 红黑树：插入更快（≤2 旋转），查找略慢（高度 ≤ 2 log(n+1) vs AVL 的 ≤ 1.44 log n）
- AVL 树：查找更快（更严格平衡），插入更慢（可能 O(log n) 次旋转）
- 选择红黑树：写密集型工作负载。选择 AVL 树：读密集型工作负载。
实际应用：Linux CFS 调度器、C++ std::map、Java TreeMap 使用红黑树。

**验证 (Verification)** *[可选，仅验证核心逻辑]*
```pseudocode
RB-INSERT-FIXUP(T, z):
  while z.p.color == RED:
    if z.p == z.p.p.left:    // 父是左子
      y = z.p.p.right        // 叔叔
      if y.color == RED:     // Case 1
        z.p.color = BLACK; y.color = BLACK
        z.p.p.color = RED; z = z.p.p
      else:
        if z == z.p.right:   // Case 2
          z = z.p; LEFT-ROTATE(T, z)
        z.p.color = BLACK    // Case 3
        z.p.p.color = RED; RIGHT-ROTATE(T, z.p.p)
    else: // 对称处理
  T.root.color = BLACK
```
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

## [Main chapters — 资源/抽象/机制/策略, no tags]

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
   5.3 Windows 内存管理策略对比

## 概念关系图 [Mermaid flowchart — top-level only]

## [Main chapters — 资源/抽象/机制/策略]

```

**说明**: 此示例中大多数概念不附验证代码——OS 概念以原理和策略为主。
例外：页面置换算法的对比表放在策略层，可选附极简伪代码验证 FIFO vs LRU 行为差异。

---

## Pattern Summary

1. **Short notes drive deep expansion** — brief source → thorough output. Every Mentioned/Assumed concept triggers expansion.
2. **Multi-file synthesis** — cross-file concept relationships merge into unified hierarchy.
3. **Gap-driven, not volume-driven** — every expansion fills a specific gap. No padding.
4. **资源 → 抽象 → 机制 → 策略** — resource constraints first, then formal models, then mechanisms, then strategic choices.
5. **CS signatures** — formal specifications, complexity analysis, protocol comparison tables, RFC/citation references.
6. **Source-bounded scope** — fill gaps in existing notes. No new topic branches.
7. **Verification code is optional** — minimal, in its own subsection, never the knowledge body. Light-tier concepts skip it entirely.
8. **Clean academic style** — no emoji, no decorative symbols. Structured knowledge only.
