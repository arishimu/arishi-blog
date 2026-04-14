---
title: 群论
published: 2026-02-07
updated: 2026-02-07
description: 'Learn Group Theory in Y minutes'
image: ''
tags: ['Math', 'Group Theory']
category: 'Group Theory'
draft: false
---

不定时施工．

## Q&A

### 子群

:::note
由于非空子群 $H \le G$ 继承了 $G$ 的结合律，以及在 $G$ 中幺元、逆元的唯一性，相比逐一验证群公理，我们可以简化检查。
:::

设非空子集 $H \subseteq G$，那么 $H \le G$ 当且仅当
$$
\forall a, b \in H, ab^{-1} \in H
$$

### 循环群

### 有限循环群

#### 生成元的阶

在有限循环群 $G = \langle g \rangle = \{e, g, g^2, \cdots, g^{n-1}\}$ 中，我们有
$$
g^n = e
$$
即生成元的阶等于群的阶。

:::tip[证明]
由封闭性，
$$
g^n = g^k, \quad 0 \le k < n
$$
我们断言 $k = 0$，否则有 $g^{n - k} = e$，与 $|G| = n$ 矛盾。
:::

我们有
$$
g^{k} = g^{k \bmod n}
$$

#### 所有生成元

在有限循环群 $G = \langle g \rangle = \{e, g, g^2, \cdots, g^{n-1}\}$中，$g^k$ 是 $G$ 的生成元，当且仅当 $k \perp n$。

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
