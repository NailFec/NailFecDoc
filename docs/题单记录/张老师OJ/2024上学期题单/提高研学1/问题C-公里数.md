# 问题C - 公里数

## 题目描述

### 原题面

某辆车的里程表出现了故障：它总是跳过数字 $3$ 和数字 $8$。也就是说，当前显示已走过两公里时，如果车子再向前走一公里，那么将显示 $4$ 公里，而不是 $3$ 公里（数字 $3$ 跳过了）。 再比如，当前是 $15229$ 公里，车子再向前走一公里，显示的是 $15240$ 公里，而不是 $15230$ 公里。数字 $8$ 也同样跳过。

请你写一个程序，根据里程表上显示的数字，计算车子真正走了多少公里。

### 输入要求

一行，一个正整数，表示故障里程表显示的公里数 $s\:(1\le s\le 1000000)$。

### 输出要求

一行，一个正整数，表示正确的公里数。

### 样例

<div class="grid" markdown>

```text
15
```

```text
12
```

</div>

$1$ 到 $15$ 数字中，包含数字 $3$、$8$、$13$。

## 解法

### 暴力解法

时间复杂度较劣的解法有：暴力枚举所有小于等于给定数的数，判断是否包含特殊数字。或使用**生成法**生成所有小于等于给定数且包含特殊数字的数的数。

### 进制转换

把原数化为 $8$ 进制数，再进行 $8$ 进制转 $10$ 进制即可。时间复杂度 $O(\log{n})$。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;

int main() {
	lli ans = 0;
	char c; int ia;
	while(scanf("%c", &c) != EOF && c <= '9' && c >= '0') {
		ia = c - '0';
		if(ia >= 8) ia --;
		if(ia >= 3) ia --;
		(ans *= 8) += ia;
	}
	printf("%lld", ans);
	return 0;
}
```
