# 问题A - 分析

## 题目描述

### 原题面

给定两个正整数 $a, b$，判断 $a \times b$ 是否为偶数。

对于全部的测试点，$1 \le a, b \le 2^{64} - 1$。

### 输入要求

一行，两个正整数 $a, b$，中间用一个空格隔开。

### 输出要求

如果 $a \times b$ 是偶数，输出 $\texttt{Yes}$，反之输出 $\texttt{No}$。

### 样例

<div class="grid" markdown>

```text
1 3
```

```text
No
```

</div>

## 解法

适用于更大的数据范围，检查两数的末尾，是否存在一个数的末尾是偶数。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	string a, b;
	cin >> a >> b;
	(a.back() - '0') % 2 && (b.back() - '0') % 2
	? printf("No") : printf("Yes");
	return 0;
}
```
