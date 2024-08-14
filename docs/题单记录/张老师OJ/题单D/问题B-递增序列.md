# 问题 B: 递增序列

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=1) | [洛谷 P1062](https://www.luogu.com.cn/problem/P1062) (1)
{ .annotate }

1.  题目来源：NOIP 2006 普及组 T4

## 题目描述

### 原题面

给定一个正整数 $k$（$3\leq k\leq 15$），把所有 $k$ 的方幂及所有有限个互不相等的 $k$ 的方幂之和构成一个递增的序列，例如，当 $k = 3$ 时，这个序列是：

$1, 3, 4, 9, 10, 12, 13, \ldots$

（该序列实际上就是：$3^0,3^1,3^0+3^1,3^2,3^0+3^2,3^1+3^2,3^0+3^1+3^2,…$）

请你求出这个序列的第 $N$ 项的值，用 $10$ 进制数表示。

例如，对于 $k = 3$，$N = 100$，正确答案应该是 $981$。

### 输入要求

两个由空格隔开的正整数 $k, N$（$3\leq k\leq 15$，$10\leq N\leq 1000$）。

### 输出要求

一个正整数。整数前不要有空格和其他符号。

### 样例

<div class="grid" markdown>

```text
3 100
```

```text
981
```

</div>

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 法1：进制转换 $O(\log n)$
- 法2：生成法 $O(n)$

### 法1：进制转换

类似快速幂算法。

??? 快速幂模板

    在 $O(\log n)$ 的时间复杂度求出 $y^x$：
    
    ```cpp
    while(x) {
        if(x & 1) ans *= y;
        y *= y;
        x >>= 1;
    }
    ```

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;

signed main() {
    int k, n;
    scanf("%lld%lld", &k, &n);
    int ans = 0, x = 1;
    while(n) {
        if(n & 1) ans += x;
        x *= k;
        n >>= 1;
    }
    printf("%lld", ans);
    return 0;
}
```

### 法2：生成法

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;
const int MAXn = 1e3 + 9;
int d[MAXn];

signed main() {
    int k, n;
    scanf("%lld%lld", &k, &n);
    d[1] = 1;
    int p1 = 1, p2 = 1;
    for(int i = 2; i <= n; i ++) {
        int r1 = d[p1] * k, r2 = d[p2] * k + 1;
        d[i] = min(r1, r2);
        if(d[i] == r1) p1 ++;
        if(d[i] == r2) p2 ++;
    }
    printf("%lld", d[n]);
    return 0;
}
```
