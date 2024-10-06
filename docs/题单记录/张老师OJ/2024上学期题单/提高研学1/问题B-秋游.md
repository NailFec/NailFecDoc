# 问题B - 秋游

## 题目描述

### 原题面

某学校五年级共有 $n$ 个班级，每班人数为 $s_1,s_2,\dots s_n$，预定车辆座位数为 $36$ 座，每人一座。每班有两位教师同行。写一个程序输入 $n$ 和 $s$，输出需多少辆车？

### 输入要求

输入的第一行包含整数 $n\:(1\le n\le 10)$，班级个数。

输入第二行到第 $n + 1$ 行为每班学生数。

### 输出要求

输出的唯一的一个整数为需要车辆数。

### 样例

<div class="grid" markdown>

```text
4
32
33
34
33
```

```text
4
```

</div>

$4$ 个班级人数为 $32+33+34+33=132$，加 $8$ 位教师，合计为 $140$ 人，需要 $4$ 辆旅游车。

## 解法

计算人数，并对 $36$ 向上取整即可。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;

int main() {
	lli ans = 0;
	int n;
	scanf("%d", &n);
	ans += n * 2;
	for(int i = 1, ia; i <= n; i ++)
		scanf("%d", &ia), ans += ia;
	printf("%lld", (ans - 1) / 36 + 1);
	return 0;
}
```
