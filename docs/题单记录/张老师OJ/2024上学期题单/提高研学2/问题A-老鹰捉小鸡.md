# 问题A - 老鹰捉小鸡

## 题目描述

### 原题面

老师和小明等 $n\:(3\le n\le 100)$ 个小朋友玩老鹰捉小鸡游戏，老师当老鹰，排在第 $1$ 位的小朋友当“母鸡”，其他 $n-1$ 位小朋友当“小鸡”。但是，当“母鸡”很辛苦，所以过一段时间“母鸡”需要排到队伍最后成为“小鸡”，让第2位小朋友当“母鸡”……

请你编一个程序，模拟 $m\:(1 < m < 100)$ 次位置变换的过程。

### 输入要求

输入一行两个数，分别表示 $m$ 和 $n$ 的值。

### 输出要求

输出 $m$ 行，每行输出符合题意的位置序列（每行数字之间，用空格分隔）。

### 样例

<div class="grid" markdown>

```text
6 4
```

```text
1 2 3 4 5 6
2 3 4 5 6 1
3 4 5 6 1 2
4 5 6 1 2 3
```

</div>

## 解法

根据规律直接输出，空间复杂度 $O(1)$。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int v, n;
	scanf("%d%d", &v, &n);
	int beg = 1;
	for(int i = 1; i <= n; i ++) {
		int now = beg;
		for(int j = 0; j < v; j ++) {
			printf("%d ", now);
			now ++;
			if(now > v) now = 1;
		}
		printf("\n");
		beg ++;
		if(beg > v) beg = 1;
	}
	return 0;
}
```
