# Examples — CS Note Refinement Walkthroughs

This file provides annotated examples of the Notes Refiner skill in action.
Each example shows: raw input notes → Phase analysis → output knowledge base
structure.

---

## Example 1: TCP Three-Way Handshake (计算机网络)

### Input — Raw Classroom Notes (`tcp-notes.md`)

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
检测到主题: 计算机网络 — TCP 连接管理
概念清单:
  1. TCP (协议) — 假定已知
  2. 三次握手 (Three-Way Handshake) — 提及但未完整解释 (Mentioned)
  3. SYN 报文 — 提及但未定义 (Mentioned)
  4. SYN-ACK 报文 — 提及但未定义 (Mentioned)
  5. ACK 报文 — 提及但未定义 (Mentioned)
  6. SYN Flood 攻击 — 提及但缺少详细机制 (Mentioned)
  7. 半连接队列 (Half-Open Connection Queue) — 命名但未解释 (Mentioned)
  8. SYN Cookie — 命名，无工作原理 (Mentioned)
  9. TCP 状态机 — 列出部分状态但不完整 (L1)
```

### Phase 2 Gap Table (excerpt)

| # | Concept | Status | Depth | Priority | Suggested Expansion |
|---|---------|--------|-------|----------|---------------------|
| 1 | Three-Way Handshake | Mentioned | L1 | **High** | Full mechanism, why 3 (not 2 or 4), sequence numbers, state machine |
| 2 | SYN packet | Mentioned | L1 | High | TCP header structure, flags field, sequence number initialization |
| 3 | SYN Flood | Mentioned | L1 | Medium | Attack mechanism detail, backlog queue, impact |
| 4 | SYN Cookie | Mentioned | L1 | Medium | Cryptographic defense mechanism, how it avoids state |
| 5 | TCP state machine | L1 | L1 | Low | Full state diagram, all transitions, RFC 793 reference |

### Output Knowledge Base Structure

```
# TCP 连接管理 — 知识库

1. TCP 协议概述 [入门]
   1.1 TCP 在协议栈中的位置
   1.2 TCP 报文头部结构 [含 Mermaid 图]
   1.3 关键字段：Sequence Number, Acknowledgment Number, Flags

2. 三次握手 (Three-Way Handshake) [入门]
   2.1 为什么需要握手？(Context Gap → 历史动机)
   2.2 三次握手步骤详解 [含 sequenceDiagram]
   2.3 为什么是三次？两次与四次的数学分析 (Derivation Gap)
   2.4 TCP 状态变迁 [含 stateDiagram-v2]

3. SYN Flood 攻击与防御 [进阶, 前置: 2]
   3.1 攻击原理与半连接队列 (Application Gap)
   3.2 SYN Cookie 防御机制 — 密码学原理 (Derivation Gap)
   3.3 其他防御手段：SYN Proxy, Rate Limiting (Relationship Gap)

4. 术语表
   | SYN | 同步序列号 | TCP 连接建立时的初始报文，携带初始序列号 ISN |
   | SYN Cookie | SYN Cookie | 一种无状态防御 SYN Flood 的密码学技术 |
   | ...

5. 交叉引用索引
6. 经典问题与练习 ("为什么需要随机 ISN？")
7. 延伸阅读 (RFC 793, RFC 4987, Kurose & Ross Ch.3)
```

---

## Example 2: Red-Black Tree Insertion (数据结构)

### Input — Raw Classroom Notes (`rbtree-notes.md`)

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

删除太复杂了不想记
```

### Phase 2 Gap Table (excerpt)

| # | Concept | Status | Depth | Priority | Suggested Expansion |
|---|---------|--------|-------|----------|---------------------|
| 1 | 红黑树性质证明 | Assumed | L1 | **High** | 证明为什么这5条性质保证 O(log n) 高度 |
| 2 | 插入算法 | Mentioned | L2 | **High** | 完整伪代码，4种case的详细图解，时间复杂度推导 |
| 3 | 旋转操作 | Mentioned | L1 | High | 左旋/右旋的完整伪代码，不变量分析 |
| 4 | 叔叔节点 | Orphaned | L1 | Medium | 定义，在插入算法中的角色 |
| 5 | 删除操作 | Mentioned | L1 | Medium | 虽原笔记称"太复杂"，应补充基本思想和主要case |
| 6 | 与AVL树比较 | Missing | — | Medium | 为什么红黑树在实际中更常用？ |

### Phase 3 Expansion — Example for "Case Analysis of Insertion"

```
原笔记缺口: "父红 → 要看叔叔" — 过于简略，缺少伪代码和每种case的详细分析

扩展后内容:
- 完整 INSERT-FIXUP 伪代码 (12行，带行号)
- 4种case的Mermaid流程图
- 每种case的图示（用ASCII art表示树结构变化）
- 时间复杂度: O(log n)，因为fixup最多旋转2次（与AVL树关键区别！）
- 为什么插入新节点初始化为红色？(证明：只有性质4可能被违反)
```

### Output Knowledge Base Structure

```
# 红黑树 — 知识库

1. 二叉搜索树回顾 [入门]
   1.1 BST 性质与操作，失衡问题 (动机)

2. 红黑树定义与性质 [入门]
   2.1 5条红黑性质及直观含义
   2.2 黑高 (Black-Height) 定义
   2.3 定理：有n个内部节点的红黑树高度 ≤ 2 log(n+1) [含证明]

3. 旋转操作 [入门]
   3.1 左旋 (Left Rotate) [伪代码 + 图示]
   3.2 右旋 (Right Rotate) [伪代码 + 图示]
   3.3 旋转的复杂度与不变量

4. 插入操作 [进阶, 前置: 2, 3]
   4.1 插入算法概述与 RB-INSERT 伪代码
   4.2 Case 分析：RB-INSERT-FIXUP [4种case含流程图]
   4.3 插入示例：逐步构建一个红黑树
   4.4 复杂度分析：O(log n)，至多2次旋转

5. 删除操作概览 [高级, 前置: 4]
   5.1 删除的基本思想与主要case分类
   5.2 与插入的关键差异

6. 红黑树 vs AVL 树 [进阶]
   | 维度 | 红黑树 | AVL 树 |
   | 平衡条件 | 宽松（5条性质） | 严格（|BF| ≤ 1）|
   | 插入旋转次数 | ≤ 2 | ≤ 1 (但需要回溯) |
   | ... |

7. 术语表 / 交叉引用 / 经典问题 (实现一个 `std::map` 的简化版) / 延伸阅读 [CLRS Ch.13]
```

---

## Example 3: Multi-File Short Notes — OS Memory Management (操作系统)

### Input — Three Short Notes Files

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
检测到子领域: 操作系统 — 内存管理 (虚拟内存 + 页面置换)
评估: 笔记非常简短 (L1-L2)，几乎所有概念都需要深度扩展
```

### Phase 4 Structure (multi-file synthesis)

```
# 操作系统内存管理 — 知识库

1. 内存管理概述 [入门]
   1.1 为什么需要内存管理？(Context Gap — 单道→多道程序的演进)
   1.2 地址类型：物理地址 vs 虚拟地址

2. 虚拟内存 (Virtual Memory) [入门]
   2.1 核心思想与动机 (来自 file1，扩展历史：1960s, Atlas 计算机)
   2.2 MMU 与地址翻译过程 [流程图]
   2.3 页表 (Page Table) 结构与多级页表 (Derivation Gap)
   2.4 TLB (Translation Lookaside Buffer) 原理与命中率分析

3. 缺页中断 (Page Fault) [进阶, 前置: 2]
   3.1 缺页中断处理流程详解 [来自 file3，扩展 stateDiagram]
   3.2 缺页的几种类型：Minor vs Major Fault
   3.3 性能影响与优化 (prefaulting, huge pages)

4. 页面置换算法 (Page Replacement) [进阶, 前置: 2, 3]
   4.1 FIFO 与 Belady 异常 [来自 file2，扩展证明和示例]
   4.2 LRU (Least Recently Used) — 实现方法 (计数器 vs 栈)
   4.3 Clock 算法 (Second Chance) — LRU 的近似实现
   4.4 算法对比表 (时间复杂度, 实现难度, Belady豁免)

5. 现代操作系统实例 [高级, 前置: 4]
   5.1 Linux: Buddy System + Slab Allocator
   5.2 Linux: 页面回收 (kswapd, LRU lists)
   5.3 Windows: Working Set Management
```

---

## Pattern Summary

These examples illustrate key patterns of the Notes Refiner:

1. **短笔记扩展为王** — 原文可以简短，输出必须丰满。每个"提到但没解释"的概念都触发深度扩展。
2. **多文件合并** — 跨文件的关联概念自动合并到统一层级结构中。
3. **缺口驱动** — 不为了"看起来多"而写内容，每次扩展都精准填补一个已识别的缺口。
4. **标注可见** — 每节都有难度标签和前置要求，读者可选择阅读路径。
5. **CS 特征** — 伪代码、复杂度分析、协议对比表、RFC 引用始终贯穿。
