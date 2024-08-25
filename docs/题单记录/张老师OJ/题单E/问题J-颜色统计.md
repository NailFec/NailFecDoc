# 问题 J: 颜色统计

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=9) | [洛谷 P1558 色板游戏](https://www.luogu.com.cn/problem/P1558) | [POJ 2777 Count Color](http://poj.org/problem?id=2777)

## 题目描述

### 原题面

阿宝上学了，今天老师拿来了一块很长的涂色板。

色板长度为 $L$，$L$ 是一个正整数，所以我们可以均匀地将它划分成 $L$ 块 $1$ 厘米长的小方格。并从左到右标记为 $1, 2, \dots L$。

现在色板上只有一个颜色，老师告诉阿宝在色板上只能做两件事：

1. `C A B C` 指在 $A$ 到 $B$ 号方格中涂上颜色 $C$。
2. `P A B` 指老师的提问：$A$ 到 $B$ 号方格中有几种颜色。

学校的颜料盒中一共有 $T$ 种颜料。为简便起见，我们把他们标记为 $1, 2, \dots T$。开始时色板上原有的颜色就为 $1$ 号色。面对如此复杂的问题，阿宝向你求助，你能帮助他吗？

### 输入要求

第一行有3个整数 $L \:(1 \le L \le 10^5)$，$T \:(1 \le T \le 30)$ 和 $O \:(1 \le O \le 10^5)$。 在这里 $O$ 表示事件数。

接下来 $O$ 行，每行以 `C A B C` 或 `P A B` 得形式表示所要做的事情（这里 $A, B, C$ 为整数，可能 $A> B$，这样的话需要你交换 $A$ 和 $B$）。

### 输出要求

对于老师的提问，做出相应的回答。每行一个整数。

### 样例

<div class="grid" markdown>

```text
2 2 4
C 1 1 2
P 1 2
C 2 2 2
P 1 2
```

```text
2
1
```

</div>

## 解法

!!! info ""

    如果数据更大，颜色数量更多，需要使用 莫队 / 分块 / CDQ 分治 / 树套树 等做法，如 [洛谷 P1903 [国家集训队] 数颜色 / 维护队列](https://www.luogu.com.cn/problem/P1903)、[洛谷 P4690 [Ynoi2016] 镜中的昆虫](https://www.luogu.com.cn/problem/P4690)、[洛谷题单 - Ynoi 大分块系列](https://www.luogu.com.cn/training/44148#information)。

以下为线段树解法。

```cpp
#include <bits/stdc++.h>
#define calm ((a + b) >> 1)
using namespace std;
const int MAXn = 1e5 + 5;    // 点数
const int MAXt = 30 + 9;    // 颜色数
const int MAXq = 1e5 + 5;    // 询问数
typedef bitset<MAXt> bs;
bs t[MAXn * 4 + 9], tag[MAXn * 4 + 9];

void fini(int a, int b, int p) {
    if(a == b) return t[p][1] = 1, void();
    int m = calm;
    fini(a, m, p << 1);
    fini(m + 1, b, p << 1 | 1);
    t[p] = t[p << 1] | t[p << 1 | 1];
}

void pushdown(int p) {
    tag[p << 1] = tag[p];
    tag[p << 1 | 1] = tag[p];
    t[p << 1] = t[p];
    t[p << 1 | 1] = t[p];
    tag[p] = 0;
}

void fset(int x, int y, int a, int b, int p, int v) {
    if(x <= a && b <= y) {
        t[p] = 0; t[p][v] = 1;
        tag[p] = 0; tag[p][v] = 1;
        return;
    }
    if(tag[p].count()) pushdown(p);
    int m = calm;
    if(x <= m) fset(x, y, a, m, p << 1, v);
    if(m + 1 <= y) fset(x, y, m + 1, b, p << 1 | 1, v);
    t[p] = t[p << 1] | t[p << 1 | 1];
}

bs fquery(int x, int y, int a, int b, int p) {
    if(x <= a && b <= y) return t[p];
    if(tag[p].count()) pushdown(p);
    int m = calm;
    bs res = 0;
    if(x <= m) res |= fquery(x, y, a, m, p << 1);
    if(m + 1 <= y) res |= fquery(x, y, m + 1, b, p << 1 | 1);
    return res;
}

int main() {
    int n, t, q;
    scanf("%d%d%d", &n, &t, &q);
    fini(1, n, 1);
    char c; string s;
    getline(cin, s);
    for(int _ = 1; _ <= q; _ ++) {
        c = getchar();
        if(c == 'C') {
            int ia, ib, ic;
            scanf("%d%d%d", &ia, &ib, &ic);
            getline(cin, s);
            if(ia > ib) swap(ia, ib);
            fset(ia, ib, 1, n, 1, ic);
        }
        else if(c == 'P') {
            int ia, ib;
            scanf("%d%d", &ia, &ib);
            getline(cin, s);
            if(ia > ib) swap(ia, ib);
            bs res = fquery(ia, ib, 1, n, 1);
            printf("%d\n", res.count());
        }
    }
    return 0;
}
```
