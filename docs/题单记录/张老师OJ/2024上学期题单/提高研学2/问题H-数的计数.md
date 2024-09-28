# 问题H - 数的计数

题目来源：[NOIP 2001 普及组 T1/4](https://www.luogu.com.cn/problem/P1028)

## 题目描述

### 原题面

给出正整数 $n$，要求按如下方式构造数列：

1. 只有一个数 $n$ 的数列是一个合法的数列。
2. 在一个合法的数列的末尾加入一个正整数，但是这个正整数不能超过该数列最后一项的一半，可以得到一个新的合法数列。

请你求出，一共有多少个合法的数列。两个合法数列 $a, b$ 不同当且仅当两数列长度不同或存在一个正整数 $i \leq |a|$，使得 $a_i \neq b_i$。

对于全部的测试点，保证 $1 \leq n \leq 10^3$。

!!! info
    注意此题原题表达有歧义。如当 $n = 245$ 时，有 $11|22|245$ 和 $1|122|245$ 这样的情况。这样的情况应算作 $2$ 种情况。

### 输入要求

输入只有一行一个整数，表示 $n$。

### 输出要求

输出一行一个整数，表示合法的数列个数。

### 样例

<div class="grid" markdown>

```text
6
```

```text
6
```

</div>

满足条件的数列有：$(6)$、$(6, 1)$、$(6, 2)$、$(6, 3)$、$(6, 2, 1)$、$(6, 3, 1)$。

## 解法

### 记忆化搜索

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 1e3 + 9;
lli d[MAXn];    // d[i]: 以 i 开始的所有情况

void f(int x) {
	for(int y = 0; y <= (x >> 1); y ++) {
		if(!d[y]) f(y);
		d[x] += d[y];
	}
}

int main() {
	d[0] = 1;
	int n;
	scanf("%d", &n);
	f(n);
	printf("%lld", d[n]);
	return 0;
}
```

### 动态规划

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 1e3 + 9;
lli d[MAXn];    // d[i]: 以 [1, i] 开始的所有情况，类似前缀和

int main() {
	int n;
	scanf("%d", &n);
	for(int i = 1; i <= n; i ++)
		d[i] = d[i - 1] + d[i >> 1] + 1;
	printf("%lld", d[n >> 1] + 1);
	// printf("%lld", d[n] - d[n - 1]);
	return 0;
}
```
