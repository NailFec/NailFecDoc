# 问题E - 整数去重

## 题目描述

### 原题面

给定含有 $n$ 个整数的序列，要求对这个序列进行去重操作。所谓去重，是指对这个序列中每个重复出现的数，只保留该数第一次出现的位置，删除其余位置。

### 输入要求

第一行包含一个正整数 $n\:(1 \le n \le 20000)$，表示第二行序列中数字的个数。

第二行包含 $n$ 个整数，整数之间以一个空格分开。每个整数大于等于 $10$、小于等于 $5000$。

### 输出要求

输出只有一行，按照输入的顺序输出其中不重复的数字，整数之间用一个空格分开。

### 样例

<div class="grid" markdown>

```text
5
10 12 93 12 75
```

```text
10 12 93 75
```

</div>

## 解法

使用布尔数组记录该数是否曾经出现过。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 5e3 + 9;
bitset<MAXn> bs;

int main() {
	int n;
	scanf("%d", &n);
	for(int i = 1, ia; i <= n; i ++) {
		scanf("%d", &ia);
		if(!bs[ia]) printf("%d ", ia);
		bs[ia] = 1;
	}
	return 0;
}
```
