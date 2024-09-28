# 问题 D: 最小公倍数的最小和

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=3) | [洛谷 UVA10791](https://www.luogu.com.cn/problem/UVA10791) | [UVA 10791](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1732)

## 题目描述

### 原题面

LCM (Least Common Multiple) of a set of integers is defined as the minimum number, which is a multiple of all integers of that set. It is interesting to note that any positive integer can be expressed as the LCM of a set of positive integers. For example $12$ can be expressed as the LCM of $1$, $12$ or $12$, $12$ or $3$, $4$ or $4$, $6$ or $1$, $2$, $3$, $4$ etc.

In this problem, you will be given a positive integer $N$. You have to find out a set of at least two positive integers whose LCM is $N$. As infinite such sequences are possible, you have to pick the sequence whose summation of elements is minimum. We will be quite happy if you just print the summation of the elements of this set. So, for $N = 12$, you should print $4+3 = 7$ as LCM of $4$ and $3$ is $12$ and $7$ is the minimum possible summation.

### 输入要求

The input file contains at most 100 test cases. Each test case consists of a positive integer $N (1 \le N \le 2^{31} − 1)$.

Input is terminated by a case where $N = 0$. This case should not be processed. There can be at most $100$ test cases.

### 输出要求

Output of each test case should consist of a line starting with ‘$\texttt{Case #: }$’ where $\texttt{#}$ is the test case number. It should be followed by the summation as specified in the problem statement. Look at the output for sample input for details.

### 样例

<div class="grid" markdown>

```text
12
10
5
0
```

```text
Case 1: 7
Case 2: 7
Case 3: 6
```

</div>

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 数学 $O(\sqrt n)$

### 数学

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long int

signed main() {
    int n, T = 0;
    while(scanf("%lld", &n) != EOF && n != 0) {
        T ++;
        if(n == 1) {printf("Case %lld: 2\n", T); continue; }
        int m = n, ans = 0, now = 0, k = 0;
        for(int i = 2; i * i <= n && m != 1; i ++) {
            now = 1;
            while(m % i == 0 && m != 1) {
                now *= i;
                m /= i;
            }
            if(now != 1) k ++, ans += now;
        }
        if(m != 1) {
            ans += m;
            k ++;
        }
        if(k == 1) printf("Case %lld: %lld\n", T, n + 1);
        else printf("Case %lld: %lld\n", T, ans);
    }
    return 0;
}
```
