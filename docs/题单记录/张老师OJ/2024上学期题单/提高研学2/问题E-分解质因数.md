# 问题E - 分解质因数

## 题目描述

### 原题面

给出一个合数 $n$，我们希望把 $n$ 分解质因数，即分解为若干个质数相乘的形式。

### 输入要求

输入一个正整数 $n\:(1<n\le 1\times 10^9)$，输入保证 $n$ 为合数。

### 输出要求

输出数据包含若干行，每行两个正整数 $p,a$，中间用一个空格隔开。表示 $n$ 包含 $a$ 个质因子 $p$，要求按 $p$ 的值从小到大输出。

### 样例

<div class="grid" markdown>

```text
120
```

```text
2 3
3 1
5 1
```

</div>

## 解法

### 欧拉筛

欧拉筛的过程中，也可以 $O(n)$ 地筛出每一个数的最小质因数。对于较小的数据范围和多次询问，使用欧拉筛最优。

```cpp hl_lines="19"
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 2e7 + 9;
bitset<MAXn> ved;
vector<int> prime;
int mn[MAXn];
map<int, int> ans;

int main() {
	mn[1] = 1;
	int n;
	scanf("%d", &n);
	for(int i = 2; i <= MAXn - 3; i ++) {
		if(!ved[i]) prime.push_back(i), mn[i] = i;
		for(int pj : prime) {
			if(i * pj > MAXn - 3) break;
			ved[i * pj] = 1;
			mn[i * pj] = pj;
			if(i % pj == 0) break;
		}
	}
	while(n != 1) {
		ans[mn[n]] ++;
		n /= mn[n];
	}
	for(auto it = ans.begin(); it != ans.end(); it ++)
		printf("%d %d\n", (*it).first, (*it).second);
	return 0;
}
```

### 暴力枚举

```cpp
#include <bits/stdc++.h>
using namespace std;
map<int, int> ans;

int main() {
	int n;
	scanf("%d", &n);
	for(int i = 2; n != 1; i ++)
		while(!(n % i)) n /= i, ans[i] ++;
	for(auto it = ans.begin(); it != ans.end(); it ++)
		printf("%d %d\n", (*it).first, (*it).second);
	return 0;
}
```
