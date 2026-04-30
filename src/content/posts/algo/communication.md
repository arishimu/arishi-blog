---
title: 通信题
published: 2026-04-12
updated: 2026-04-27
description: '如何使用受限的信道完成通信？'
image: ''
tags: [Algorithm, Communication]
category: 'Algorithm'
draft: false
---

## 例题

### XOR-Hashing

:::note[棋盘翻转]

> 因为在情人节偷吃了鼠鼠的奶酪，Alice 和 Bob 被鼠鼠分别关在两个房间里！

**这是一道通信题**。

鼠鼠在一个 $64 \times 64$ 的棋盘的每一格上放置了一枚硬币。
每枚硬币要么正面朝上，要么背面朝上。其中有一枚硬币是假的，但它与其他硬币在外观上没有区别。

鼠鼠会先将棋盘拿进 Alice 所在的房间，并告诉她哪一枚硬币是假的。
Alice 需要选择棋盘上的一行硬币，将其全部翻面一次，再选择棋盘上的一列硬币，将其全部翻面一次。

随后鼠鼠会将 Alice 操作过的棋盘拿进 Bob 所在的房间，让 Bob 指认假币。
如果 Bob 成功指认出假币，两人就能重见天日；否则他们就要遭受惩罚！

现在你需要设计一个程序，在第一次运行时扮演 Alice，在第二次运行时扮演 Bob，最终指认出假币。

**Interaction Protocol**

你的程序将会在每次测试中运行两次。

在 Alice Round，你的程序将扮演 Alice；在 Bob Round，你的程序将扮演 Bob。

> **Alice Round**
>
> - 输入的第一行是单词 `Alice`。
> - 接下来 64 行，每行包含一个长度为 $64$ 的 $01$ 串，表示棋盘上的各枚硬币是否正面朝上。
> - 最后一行包含两个整数 $i, j (1 \le i, j \le 64)$，表示棋盘第 $i$ 行第 $j$ 列的硬币是假币。
>
> 你需要输出一行两个整数 $x, y (1 \le x, y \le 64)$，表示将棋盘第 $x$ 行上的所有硬币翻面一次，再将第 $y$ 列
> 上的所有硬币翻面一次。
> 如果你的输出不符合格式要求，你将会收到 `Wrong Answer` 的评测结果。
>
> **Bob Round**
>
> 在 Bob 阶段，你的程序将被重启。
>
> - 输入的第一行是单词 `Bob`。
> - 接下来 $64$ 行，每行包含一个长度为 $64$ 的 $01$ 串，表示棋盘上的各枚硬币是否正面朝上。
>
> 你需要输出一行两个整数 $x, y (1 \le x, y \le 64)$，表示指认棋盘第 $x$ 行第 $y$ 列的硬币为假币。
> 如果你成功指认出假币，交互器将返回 `Accepted` 的评测结果。

:::

本质是给定一个长度为 $4096$ 的 $01$ 串，通过翻转操作来传递一个 $[0, 4096)$ 内的整数。

棋盘很大，为了避免 Bob 查表，我们为棋盘状态定义**哈希值**，任务就转化成：让不同翻转情况后的棋盘哈希值不同，让 Bob 能区分。

信道只能传输二进制数，于是 Alice 和 Bob 可以事先协商，为每个二进制位赋权，并定义棋盘状态的哈希值为**加权异或和**，用哈希值作为传输信息的介质，这便是 XOR-Hashing。

具体地，设赋权后初始棋盘的哈希值为 $H$，现在 Alice 想要传递答案 $A$，为了让 Bob 收到哈希值为
$$
A = H \oplus F
$$
的棋盘，需要权值为
$$
F = H \oplus A
$$
的翻转。如何赋权使得 Alice 能根据 $F$ 得知需要翻转的行列号，即构建 $F$ 到行列号的双射？

理论上这需要解含 $4096$ 个方程（翻转情况数）、$128$ 个变量（行列异或值）的方程组，解出来的 $128$ 个值分别作为权值有关行列异或和的约束。但二人发现：$A$ 是 $12$ 位二进制数，因此所赋权值也是 $12$ 位二进制数，而行列号都是 $6$ 位二进制数。

二人容易想到一种双射：让 $F$ 的高 $6$ 位是行号，低 $6$ 位是列号。要达成这种效果的赋权是什么样的呢？很快发现赋权方案其实很简单：让第 $i$ 行异或和为 $64i$，第 $j$ 列异或和为 $j$。

```cpp
#include <iostream>
#include <vector>

void Alice() {
    int i, j;
    std::vector<std::string> B(64);
    for (auto &L : B) std::cin >> L;
    std::cin >> i >> j; i--, j--;
    int H = 0;
    for (int q = 0; q < 64; q++) if (B[0][q] == '1') H ^= q;
    for (int p = 0; p < 64; p++) if (B[p][0] == '1') H ^= p << 6;
    int flip = H ^ (i << 6 | j);
    int fx = flip >> 6, fy = flip & 0x3f;
    std::cout << fx + 1 << ' ' << fy + 1 << '\n';
}

void Bob() {
    std::vector<std::string> B(64);
    for (auto &L : B) std::cin >> L;
    int ans = 0;
    for (int j = 0; j < 64; j++) if (B[0][j] == '1') ans ^= j;
    for (int i = 0; i < 64; i++) if (B[i][0] == '1') ans ^= i << 6;
    int x = ans >> 6, y = ans & 0x3f;
    std::cout << x + 1 << ' ' << y + 1 << '\n';
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::string id;
    std::cin >> id;
    if (id == "Alice") Alice();
    else if (id == "Bob") Bob();
}
```

### 不忠实的信道

:::note[纸条翻转]

**这是一道通信题**。

Bob 刚刚和 Alice 玩了猜数游戏。正确猜出 Alice 心中的数字后，他会在一张纸条上写下长度为 8 的 01 字符串来记录该数字。

然而，如果纸条被翻转，所记录的 01 字符串也会被反转，导致无法确定该字符串应从左到右还是从右到左读取。为避免歧义，Bob 必须设计一种编码与解码方案，使得无论纸条是否被翻转，都能根据纸条上的信息唯一还原出原始数字。

过了一段时间后，Bob 重新找到了这张纸条，他需要根据纸条上的信息还原出记录的数字。

**Interaction Protocol**

您的程序在每组测试中将被运行两次。在第一次运行中，您的程序扮演写纸条的 Bob。在第二次运行中，您的程序扮演读纸条的 Bob。

每次运行包含多组测试数据。

**第一次运行**

第一行输入字符串 `write`，第二行输入一个整数 $T$ ($1 \le T \le 100$)，表示测试数据组数。对于每组测试数据：

- 唯一的一行输入一个整数 $x$ ($0 \le x \le 100$)，表示要记录的整数。
- 您的程序应输出一行，包含一个长度为 $8$ 的字符串，由字符 `0` 和 `1` 组成，表示纸条上记录的信息。

**第二次运行**

您的程序将被重启以进行第二次运行。

第一行输入字符串 `read`，第二行输入一个整数 $T$ ($1 \le T \le 100$)，表示测试数据组数。对于每组测试数据：

- 唯一的一行输入一个长度为 $8$ 的字符串，由字符 `0` 和 `1` 组成，这正是您在第一次运行中输出的字符串，字符顺序可能被翻转。
- 您的程序应输出一行，包含一个整数，表示您从字符串中还原的整数。

注意：第二次运行的测试数据顺序可能和第一次运行时不同。只有您对所有测试数据都正确还原了整数的答案才被视为正确。
:::

形式化地，给定消息集 $M$，$|M| = 101$，字母表 $\Sigma = \{0, 1\}$，码长 $n = 8$，信道存在算子 $G = \{\mathrm{id}, \mathrm{rev}\}$，其中反转算子
$$
\begin{aligned}
\mathrm{rev}: \Sigma^n &\rightarrow \Sigma^n \\
b_0b_1...b_{n-1} &\mapsto b_{n-1}b_{n-2}...b_0
\end{aligned}
$$
求一套编解码方案 $(\mathrm{Enc}, \mathrm{Dec})$，使得
$$
\forall m \in M, \forall g \in G, \mathrm{Dec}(g(\mathrm{Enc}(m))) = m
$$
其中 $\mathrm{Enc}: M \rightarrow \Sigma^n$，$\mathrm{Dec}: \Sigma^n \rightarrow M$。

:::tip
在 $\Sigma^n$ 中任取两码，我们无法确保它们来自不同的消息。

由于存在篡改行为，$\Sigma^n$ 中互反的码不可区分。特别地，回文串与其自身不可区分。因此它们实质上构成了一系列等价类。形式化地，$\{c, \mathrm{rev}(c)\}$ 是不可区分关系下的等价类，并且
$$
\bigcap_{m \in M} \{\mathrm{Enc}(m), g(\mathrm{Enc}(m))\} = \empty
$$

在 $\{0, 1\}^8$ 中，一共有 $2^4 = 16$ 个回文，$2^4 + (2^8-2^4) / 2 = 16 + 120 = 136$ 个等价类。

由于 $101 < 136$，$\mathrm{Enc}$ 可被设计成单射：为消息分配不同的等价类 (可选择字典序较小者作为代表元)。

$\mathrm{Dec}$ 在 $\mathrm{Enc}(M)$ 上被设计成 $\mathrm{Enc}^{-1}$，在 $\Sigma^n \setminus \mathrm{Enc}(M)$ 上编码失败。
:::

```cpp
#include <bitset>
#include <iostream>
#include <sstream>
#include <vector>

std::vector<int> enc, dec;

void init() {
    enc.assign(101, -1);
    dec.assign(256, -1);
    std::stringstream ss;
    for (int d = 0, cnt = 0; d < 256 && cnt < 101; d++) {
        ss.clear();
        ss << std::bitset<8>(d);
        std::string s = ss.str(), r = s;
        std::reverse(r.begin(), r.end());
        if (s > r) continue;
        enc[cnt] = d;
        dec[d] = cnt;
        dec[std::stoi(r, nullptr, 2)] = cnt++;
    }
}

void alice() {
    int x;
    std::cin >> x;
    std::cout << enc[x] << '\n';
}

void bob() {
    std::string s;
    std::cin >> s;
    std::cout << dec[std::stoi(s, nullptr, 2)] << '\n';
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    init();
    std::string id; int T = 1;
    std::cin >> id >> T;
    if (id == "write") while (T--) alice();
    if (id == "read") while (T--) bob();
}
```
