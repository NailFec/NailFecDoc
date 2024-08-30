# 问题 E: 相邻数之和

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=4) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T5

## 题目描述

### 原题面

一个由 $n$ 个数组成的数列，所有相邻 $m$ 个数的和有 $n-m+1$ 个，求其中的最大值。

### 输入要求

共两行：

第一行，$2$ 个整数 $n,\: m\:(1\le m\le n\le 1\times 10^5)$，数与数之间以一个空格隔开。

第二行，$n$ 个整数 $a\:(1\le a\le 500)$，数与数之间以一个空格隔开。

### 输出要求

一行，一个整数。

### 样例

<div class="grid" markdown>

```text
6 3
11 19 9 12 5 20
```

```text
40
```

</div>

## 解法

使用前缀和，每次查询时间复杂度 $O(1)$。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn=1e5+9;
lli adatum[MAXn+9];    // adatum[i]=[1,i]

int main() {
	int n,m;
	scanf("%d%d",&n,&m);
	int ina;
	for(int i=1; i<=n; i++) {
		scanf("%d",&ina);
		adatum[i]=adatum[i-1]+ina;
	}
	lli res=0;
	for(int i=1; i<=n-m+1; i++) {
		res=max(res,adatum[i+m-1]-adatum[i-1]);
//		printf("%lld\n",adatum[i+m-1]-adatum[i-1]);
	}
	printf("%lld",res);
	return 0;
}
```
