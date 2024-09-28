# 问题 C: 几乎是素数

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=2) | [洛谷 UVA10539](https://www.luogu.com.cn/problem/UVA10539) | [UVA 10539](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1480)

## 题目描述

### 原题面

Almost prime numbers are the non-prime numbers which are divisible by only a single prime number.

In this problem your job is to write a program which finds out the number of almost prime numbers within a certain range.

!!! info

    Wikipedia 关于“几乎是素数数”的介绍：[Almost prime - Wikipedia](https://en.wikipedia.org/wiki/Almost_prime)

### 输入要求

First line of the input file contains an integer $N (N \le 600)$ which indicates how many sets of inputs are there. Each of the next N lines make a single set of input. Each set contains two integer numbers low and high $(0 < low \ge high < 1012)$.

### 输出要求

For each line of input except the first line you should produce one line of output. This line contains a single integer, which indicates how many almost prime numbers are within the range (inclusive) low . . . high.

### 样例

<div class="grid" markdown>

```text
3
1 10
1 20
1 5
```

```text
3
4
1
```

</div>

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 埃式筛 + 二分查找 $O(n\log n)$

### 埃式筛 + 二分查找

!!! warning

    注意二分查找中 `lower_bound()` 和 `upper_bound()`。

```cpp hl_lines="18-19"
#include <bits/stdc++.h>
#define int long long int
using namespace std;
const int MAXn = 1e6 + 9, MAX = 1e12 + 9;
vector<int> prime;
bool notp[MAXn + 9];

signed main() {
    for(int i = 2; i <= MAXn; i ++) {
        if(!notp[i]) {
            for(int j = i * i; j <= MAXn; j += i) notp[j] = true;
            for(int j = i * i; j <= MAX; j *= i) prime.push_back(j);
        }
    }
    sort(prime.begin(), prime.end());
    int l, r;
    scanf("%lld%lld", &l, &r);
    auto pl = lower_bound(prime.begin(), prime.end(), l);
    auto pr = upper_bound(prime.begin(), prime.end(), r);
    printf("%lld\n", pr - pl);
    return 0;
}
```
