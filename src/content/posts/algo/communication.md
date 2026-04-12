---
title: 算法竞赛中的通信题
published: 2026-04-12
updated: 2026-04-12
description: '初次接触通信题．'
image: ''
tags: [Algorithm, Communication]
category: 'Algorithm'
draft: false
---

## 例题

### 信道存在篡改行为

:::note[纸条翻转]
这是一道通信题。

Bob 刚刚和 Alice 玩了猜数游戏。正确猜出 Alice 心中的数字后，他会在一张纸条上写下长度为 8 的 01 字符串来记录该数字。

然而，如果纸条被翻转，所记录的 01 字符串也会被反转，导致无法确定该字符串应从左到右还是从右到左读取。为避免歧义，Bob 必须设计一种编码与解码方案，使得无论纸条是否被翻转，都能根据纸条上的信息唯一还原出原始数字。

过了一段时间后，Bob 重新找到了这张纸条，他需要根据纸条上的信息还原出记录的数字。

:::important[交互协议]
您的程序在每组测试中将被运行两次。在第一次运行中，您的程序扮演写纸条的 Bob。在第二次运行中，您的程序扮演读纸条的 Bob。

每次运行包含多组测试数据。

**第一次运行**\
第一行输入字符串 `write`，第二行输入一个整数 $T$ ($1 \le T \le 100$)，表示测试数据组数。对于每组测试数据：

- 唯一的一行输入一个整数 $x$ ($0 \le x \le 100$)，表示要记录的整数。
- 您的程序应输出一行，包含一个长度为 $8$ 的字符串，由字符 `0` 和 `1` 组成，表示纸条上记录的信息。

**第二次运行**\
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
