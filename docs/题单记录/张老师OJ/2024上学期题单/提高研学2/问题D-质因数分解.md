# 问题D - 质因数分解

题目来源：[NOIP 2012 普及组 T1/4](https://www.luogu.com.cn/problem/P1075)

## 题目描述

输入一个正整数 $n$，$n$ 是两个不同的质数的乘积，试求出较大的那个质数。

### 样例

<div class="grid" markdown>

```text
21
```

```text
7
```

</div>

## 解法

### 欧拉筛

此题由于数据大小，欧拉筛并不能获得全部分数。但对于较小的数据范围、大量询问的情况，欧拉筛是最优解。

### 暴力枚举

从小到大暴力枚举每个非 $1$ 正整数，直到找到 $k$，满足 $k \mid n$，答案即为 $n / k$。

注意：不能从大到小枚举，如 $91 = 7 \times 13$，从小到大枚举 $7$ 次，而从大到小枚举 $91 - 13 + 1 = 79$ 次。

```cpp hl_lines="28-30"
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 2e7 + 9;
bitset<MAXn> ved;
vector<int> prime;
int mn[MAXn];

int main() {
	mn[1] = 1;
	int n;
	scanf("%d", &n);
	if(n <= (int)2e7) {
		for(int i = 2; i <= MAXn - 3; i ++) {
			if(!ved[i]) prime.push_back(i);
			for(int pj : prime) {
				if(i * pj > MAXn - 3) break;
				ved[i * pj] = 1;
				mn[i * pj] = pj;
				if(i % pj == 0) break;
			}
		}
		printf("%d", n / mn[n]);
		return 0;
	}
	for(int i = 2; ; i ++)
		if(!(n % i)) return 0 * printf("%d", n / i);
	// TLE:
	// for(int i = n - 1; ; i --)
	// 	if(!(n % i)) return 0 * printf("%d", i);
	return 0;
}
```
