# 问题 H: 奶牛的高度差

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=7) | [洛谷 P2880 [USACO07JAN] Balanced Lineup G](https://www.luogu.com.cn/problem/P2880) (1) | RMQ 问题 :white_check_mark:
{ .annotate }

1.  题目来源：USACO 2007/1 月赛

## 题目描述

### 原题面

每天，农夫 John 的 $n\:(1\le n\le 5\times 10^4)$ 头牛总是按同一序列排队。

有一天，John 决定让一些牛们玩一场飞盘比赛。他准备找一群在队列中位置连续的牛来进行比赛。但是为了避免水平悬殊，牛的身高不应该相差太大。John 准备了 $q\:(1\le q\le 1.8\times10^5)$ 个可能的牛的选择和所有牛的身高 $h_i\:(1\le h_i\le 10^6, 1\le i\le n)$。他想知道每一组里面最高和最低的牛的身高差。

### 输入要求

第一行两个数 $n,q$。

接下来 $n$ 行，每行一个数 $h_i$。

再接下来 $q$ 行，每行两个整数 $a$ 和 $b$，表示询问第 $a$ 头牛到第 $b$ 头牛里的最高和最低的牛的身高差。

### 输出要求

输出共 $q$ 行，对于每一组询问，输出每一组中最高和最低的牛的身高差。

### 样例

<div class="grid" markdown>

```text
6 3
1
7
3
4
2
5
1 5
4 6
2 2
```

```text
6
3
0
```

</div>

## 解法

### ST 表

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 1.8e5 + 9;
const int MAXl = 25;
int mx[MAXn][MAXl], mn[MAXn][MAXl];

int main() {
    for(int i = 0; i <= MAXn - 3; i ++)
        for(int j = 0; j <= MAXl - 1; j ++)
            mn[i][j] = INT_MAX;
    int n, m;
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i ++)
        scanf("%d", &mx[i][0]), mn[i][0] = mx[i][0];
    for(int j = 1; j <= MAXl; j ++) {
        int alen = 1 << j;
        int blen = 1 << (j - 1);
        for(int i = 1; i <= n - alen + 1; i ++)
            mx[i][j] = max(mx[i][j - 1], mx[i + blen][j - 1]),
            mn[i][j] = min(mn[i][j - 1], mn[i + blen][j - 1]);
    }
    for(int i = 1, l, r, lg, len; i <= m; i ++) {
        scanf("%d%d", &l, &r);
        lg = log2(r - l + 1);
        len = 1 << lg;
        printf("%d\n", max(mx[l][lg], mx[r - len + 1][lg])
                     - min(mn[l][lg], mn[r - len + 1][lg]));
    }
    return 0;
}
```

### 线段树

```cpp
#include <bits/stdc++.h>
#define calm ((a + b) >> 1)
using namespace std;
const int MAXn = 1.8e5 + 9;
int d[MAXn + 9], tx[(MAXn << 2) + 9], tn[(MAXn << 2) + 9];
int n;

void fini(int a, int b, int p) {
    if(a == b) return tx[p] = tn[p] = d[a], void();
    int m = calm;
    fini(a, m, p << 1);
    fini(m + 1, b, p << 1 | 1);
    tx[p] = max(tx[p << 1], tx[p << 1 | 1]);
    tn[p] = min(tn[p << 1], tn[p << 1 | 1]);
}

int fgetmx(int x, int y, int a, int b, int p) {
    if(x <= a && b <= y) return tx[p];
    int m = calm, mx = 0;
    if(x <= m) mx = max(mx, fgetmx(x, y, a, m, p << 1));
    if(m + 1 <= y) mx = max(mx, fgetmx(x, y, m + 1, b, p << 1 | 1));
    return mx;
}

int fgetmn(int x, int y, int a, int b, int p) {
    if(x <= a && b <= y) return tn[p];
    int m = calm, mn = INT_MAX;
    if(x <= m) mn = min(mn, fgetmn(x, y, a, m, p << 1));
    if(m + 1 <= y) mn = min(mn, fgetmn(x, y, m + 1, b, p << 1 | 1));
    return mn;
}

int main() {
    int m;
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i ++)
        scanf("%d", &d[i]);
    fini(1, n, 1);
    for(int i = 1, l, r; i <= m; i ++) {
        scanf("%d%d", &l, &r);
        printf("%d\n", fgetmx(l, r, 1, n, 1) - fgetmn(l, r, 1, n, 1));
    }
    return 0;
}
```
