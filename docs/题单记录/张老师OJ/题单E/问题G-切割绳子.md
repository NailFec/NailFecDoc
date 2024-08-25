# 问题 G: 切割绳子

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=6) | 二分

## 题目描述

### 原题面

有 $n$ 条绳子，每条绳子的长度已知且均为正整数。绳子可以以任意正整数长度切割，但不可以连接。现在要从这些绳子中切割出 $m$ 条长度相同的绳段，求绳段的最大长度是多少？

### 输入要求

第一行是一个不超过 $1000$ 的正整数 $n$。

第二行是 $n$ 个不超过 $10^6$ 的正整数，表示每段绳子的长度。

第三行是一个不超过 $10^8$ 的正整数 $m$。

### 输出要求

绳段的最大长度，若无法切割，则输出“$\texttt{Failed}$”。

### 样例

<div class="grid" markdown>

```text
4
20 40 35 38
7
```

```text
17
```

</div>

## 解法

### 普通二分

计算中心点 $m$ 的几种方法：

- `m = (l + r) >> 1`
- `m = l + ((r - l) >> 1)`
- `m = (l & r) + ((l ^ r) >> 1)`

以下代码的二分区间为 $[l, r)$。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 1e3 + 9;
int d[MAXn];
int n, m;

bool check(int x) {
    int ans = 0;
    for(int i = 1; i <= n; i ++)
        ans += d[i] / x;
    return ans >= m;
}

int ef() {
    int l = 0, r = 1e9 + 1;
    while(l + 1 < r) {
        int m = l + ((r - l) >> 1);
        if(check(m)) l = m;
        else r = m;
    }
    return l;
}

int main() {
    scanf("%d", &n);
    for(int i = 1; i <= n; i ++)
        scanf("%d", &d[i]);
    scanf("%d", &m);
    int ans = ef();
    ans == 0 ? printf("Failed") : printf("%d", ans);
    return 0;
}
```

### 倍增

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 1e3 + 9;
int d[MAXn];
int n, m;

bool check(int x) {
    int ans = 0;
    for(int i = 1; i <= n; i ++)
        ans += d[i] / x;
    return ans >= m;
}

int bz() {
    int ans = 0;
    for(int i = 20; i >= 0; i --) {
        int t = ans | (1 << i);
        if(check(t)) ans = t;
    }
    return ans;
}

int main() {
    scanf("%d", &n);
    for(int i = 1; i <= n; i ++)
        scanf("%d", &d[i]);
    scanf("%d", &m);
    int ans = bz();
    ans == 0 ? printf("Failed") : printf("%d", ans);
    return 0;
}   
```
