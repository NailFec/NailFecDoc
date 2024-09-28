# 问题C - 第 n 小质数

## 题目描述

输入一个正整数 $n$，求出第 $n$ 小的质数。

### 样例

<div class="grid" markdown>

```text
10
```

```text
29
```

</div>

## 解法

判断素数和素数筛法的算法众多，此题最优的解法为欧拉筛。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 1e6 + 9;    // 10000: 104729
bitset<MAXn> ved;
vector<lli> prime;

int main() {
	lli n;
	scanf("%lld", &n);
	for(int i = 2; i <= MAXn - 3; i ++) {
		if(!ved[i]) prime.push_back(i);
		for(lli pj : prime) {
			if(i * pj > MAXn - 3) break;
			ved[i * pj] = 1;
			if(i % pj == 0) break;
		}
	}
	printf("%lld", prime[n - 1]);
	return 0;
}
```
