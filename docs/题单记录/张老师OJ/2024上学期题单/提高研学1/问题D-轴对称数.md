# 问题D - 轴对称数

## 题目描述

### 原题面

输入一个双位数位的整数，判断是否是轴对称数（回文数）。

### 输入要求

输入一个双位数位的整数 $n$。$n$ 的位数不超过 $20$ 位。

### 输出要求

一行，如果是在输出“$\texttt{YES}$”，否则输出“$\texttt{NO}$”。

### 样例

<div class="grid" markdown>

```text
1001
```

```text
YES
```

</div>

## 解法

此写法同时适用于非双位数位的数。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	string s;
	getline(cin, s);
	for(int i = 0, j = s.size() - 1; i <= j; i ++, j --)
		if(s[i] != s[j]) return 0 * printf("NO");
	printf("YES");
	return 0;
}
```
