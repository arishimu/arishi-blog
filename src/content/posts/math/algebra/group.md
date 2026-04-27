---
title: 群论
published: 2026-02-07
updated: 2026-04-21
description: 'Learn Group Theory in Y minutes'
image: ''
tags: ['Math', 'Group Theory']
category: 'Group Theory'
draft: false
---

不定时施工．

## Q&A

### 置换

#### 对称群

给定任意含 $n$ 个元素的集合 $S$，存在 $n!$ 种置换 $\lambda: S \rightarrow S$，它们在函数复合 $\circ$ 下形成群 $S_n$，称为**对称群**。

#### 置换群

:::note
考虑两个有启发性的例子
$$
\begin{array}{ll}
\mathbb Z_p^* = a \mathbb Z_p^*, &1 \le a \le p - 1\\
\mathbb Z_n^* = a \mathbb Z_n^*, &0 \le a < n, a \perp p
\end{array}
$$
思考更一般的情形：对于任意的有限群 $G$ 和群元 $a \in G$，是否总有
$$
G = aG = \{ag: g \in G\}
$$
这样的置换？
:::

:::tip
考虑将 $a$ 作用于 $x$，在有向图上连接 $x \mapsto ax$。

1. 考虑 $x$ 的出度：封闭性保证 $ax \in G$；
2. 对于非平凡的 $a \ne e$，不会有自环 $ax \ne x$；
3. 考虑 $x$ 的入度：若 $az = x$，逆元将保证 $z$ 是存在且唯一的。

综上，$x$ 的入度和出度均为 $1$。在有限的情况下，该作用必为置换。
更具体地，$G$ 会被划分为若干个有向环 (轨道)，我们将看到这正是置换的循环分解！

任取 $g \in G$，用 $a$ 不断作用于 $g$ 得到
$$
g, ag, a^2g, ..., a^{m-1}g
$$
群是有限的，终有 $a^m = e$ 使得 $a^mg = g$，我们便得到了 $g$ 所处的轨道。环长 $m$ 正是 $a$ 的阶数，因此只要选定了 $a$，环长、环数都是确定的。我们把公因子提出来，这正是 (左) 陪集 $\langle a \rangle g$。
:::

在 $n$ 阶群中，$a$ 一共有 $n$ 种选取方式，因此能定义 $n$ 种上面的置换映射 $\lambda_a: x \mapsto ax$，它们又构成了新的集合
$$
\overline{G} = \{\lambda_a : a \in G\}
$$
我们断言：在函数复合 $\circ$ 下，$\overline{G}$ 构成群。封闭性、幺元都是显然的，结合律请回顾离散数学的函数作为二元关系的定义，最后不难验证 $\lambda_a$ 的逆元是 $\lambda_{a^{-1}}$：这只需要把所有有向边取反即可。我们称 $\overline{G}$ 是**置换群**。

:::important
一共有 $n!$ 个置换，而左乘 $a$ 属于特殊的一类置换，共 $n$ 个，处在一个比较小的结构，但这个结构十分精妙：置换群是对称群的子群，这也可以直接作为定义。

为什么我们更偏爱置换群呢？因为相比对称群，置换群的置换足够简单：仅含单个参数的规则 $x \mapsto ax$ 就能描述整个置换，而无需给出所有元素被置换后的具体下落。
:::

### 子群

:::note
由于非空子群 $H \le G$ 继承了 $G$ 的结合律，以及在 $G$ 中幺元、逆元的唯一性，相比逐一验证群公理，我们可以简化检查。
:::

设非空子集 $H \subset G$，那么 $H \le G$ 当且仅当
$$
\forall a, b \in H, ab^{-1} \in H
$$

### 循环群

#### 定义

对任何群 $G$，我们可以使用群元 $g$ 生成的
$$
\langle g \rangle = \{g^k : k \in \mathbb Z\}
$$
来洞悉 $G$ 的结构。

:::tip[$\langle g \rangle$ 是群吗？]
是的，$\langle g \rangle \le G$。首先 $\langle g \rangle \subset G$，其次 $g^{k_1-k_2} \in \langle g \rangle$。
:::

容易发现，$\langle g^{-1} \rangle = \langle g \rangle$，因此至少有两个群元共享同一个循环子群，除非是平凡子群。

:::important

1. 在任何群 $G$ 的内部，处处都是循环子群 $\langle a \rangle$；
2. 任何群元 $a$ 的阶，都可被视作循环子群 $\langle a \rangle$ 的阶。

:::

#### 无限循环群

:::note[生成元]
一个无限循环群 $\langle g \rangle$，生成元有且仅有 $g$ 和 $g^{-1}$。
:::

:::tip[证明]
取另一生成元 $g^a$，那么任取群元 $g^b$，方程 $(g^a)^x = g^b$ 总有整数解，即
$$
ax = b
$$
其中 $a, b, x \in \mathbb Z$。在如此严苛的要求下，仅有 $a = \pm 1$ 能够满足这一点：因为 $a \mid b$，而 $b$ 又是任意整数。
:::

#### 有限循环群

现在我们着眼于有限的情形。

:::note[何谓循环？]
在有限循环群 $G = \langle g \rangle = \{e, g, g^2, \cdots, g^{n-1}\}$ 中，我们有
$$
g^n = e
$$
即生成元的阶等于群的阶。进而我们有
$$
g^{k} = g^{k \bmod n}
$$
这意味着在有限循环群上，我们可以把群元间的运算
$$
g^a g^b = g^{a+b} = g^{(a + b) \bmod n}
$$
理解成群指数在 $\mathbb Z_n$ 上的加法。
:::

:::tip[证明]
由封闭性，
$$
g^n = g^k, \quad 0 \le k < n
$$
我们断言 $k = 0$，否则有 $g^{n - k} = e$，与 $|G| = n$ 矛盾。
:::

我们不加证明地给出显然的事实：$g^N = e$ 当且仅当 $n \mid N$，最小的 $N$ 即为 $g$ 的阶。我们开始关心其他群元 $g^k$ 的阶。

:::note[任何群元的阶]
$n$ 阶循环群 $\langle g \rangle$ 中，任意群元 $g^k$ 的阶
$$
\mathrm{ord}(g^k) = \frac{n}{\gcd(k, n)}
$$
这符合直觉，但又不太显然。
:::

:::note[生成元]
在有限循环子群 $\langle g \rangle = \{e, g, g^2, \cdots, g^{n-1}\}$ 中，$g^k$ 是生成元，当且仅当 $k \perp n$。这意味着 $\langle g \rangle$ 恰有 $\varphi(n)$ 个生成元。
:::

:::tip[证明]
$$
\begin{aligned}
\{g^k \in G : \langle g^k \rangle = G\}
&= \{g^k \in G : \forall\, 0 \le y < n, \exists\, x \in \mathbb Z, g^y = (g^k)^x\} \\
&= \{g^k \in G : \forall\, 0 \le y < n, \exists\, x \in \mathbb Z, y \equiv kx \pmod n\} \\
&= \{g^k \in G : \forall\, 0 \le y < n, \gcd(k, n) \mid y\} \\
&= \{g^k \in G : \gcd(k, n) \mid \gcd(0, 1, \cdots, n - 1)\} \\
&= \{g^k \in G : \gcd(k, n) \mid 1\} \\
&= \{g^k \in G : \gcd(k, n) = 1\} \\
&= \{g^k \in G : k \perp n\}
\end{aligned}
$$
:::
