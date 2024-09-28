# 问题 E: 海克斯

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=4) (1)
{ .annotate }

1.  题目来源：“科大国创杯”2023年安徽省青少年信息学科普日活动小学组 T4

## 题目描述

### 原题面

为了写作业，小可可和小多在下一种奇怪的棋——hex 棋。

如下是一个这种棋的棋盘，它可能可以帮助你理解下面的题意：

<!-- ![pic](image.png){ width="400" } -->
<center><img src="../image.png" alt="hex graph" width="400px" /></center>
<!---->

这种棋的规则如下：棋盘由 $N \times N$ 个六边形格子构成。

称两个格子相连通，当且仅当两个格子对应的六边形共边。将从上往下第 $i$ 行从左到右第 $j$ 个格子称为 $(i, j)$。对于一个不在边界上的格子 $(i, j)$，它和 $(i, j + 1)$、$(i, j − 1)$、$(i + 1, j)$、$(i + 1, j − 1)$、$(i − 1, j)$、$(i − 1, j + 1)$ 这些格子相连通，而边界上的格子只与上述格子中存在的格子相连通。

两人轮流下棋，小可可先手，小可可每次选一个空的格子下一个红色棋子，小多每次选一个空的格子下一个蓝色棋子，如果小可可将上下两条边界用红色棋子连通了，那么小可可胜；如果小多将左右两条边界用蓝色棋子连通了，那么小多胜。

接下来给出若干个局面，请你判断每一局是小可可胜，还是小多胜，还是目前没有人获得胜利（容易证明，不可能两人都达到获胜条件）。

对于 $20 \%$ 的数据，满足 $1 \le N \le 3$。  
对于另外 $40 \%$ 的数据，满足给出的棋局已经分出胜负。  
对于 $100 \%$ 的数据，满足 $1 \le T \le 10, 1 \le N \le 100$。

### 输入要求

第一行一个正整数 $T$，代表他们下了 $T$ 盘棋。

对于每一盘棋：

输入一行一个正整数 $N$，代表目前这盘棋的棋盘的大小。

之后 $N$ 行，每行 $N$ 个 $−1, 0, 1$ 中的整数，第 $i$ 行的第 $j$ 个整数代表格子 $(i, j)$ 的状态，如果为 $−1$ 则该格子中为蓝色棋子，如果为 $0$ 则该格子为空，如果为 $1$ 则该格子中为红色棋子。

### 输出要求

输出共 $T$ 行，请对于每个局面，输出一行一个字符串：如果小可可胜，则输出 $\texttt{ke}$；如果小多胜，则输出 $\texttt{do}$；如果目前两人都还未获胜，则输出 $\texttt{yet}$。

### 样例

<div class="grid" markdown>

```text
3
4
0 1 0 -1
0 -1 1 0
-1 -1 1 0
0 0 1 0
4
0 1 1 -1
0 -1 1 0
-1 -1 1 0
0 0 1 0
4
0 1 -1 -1
0 -1 1 1
-1 -1 1 0
0 0 1 0
```

```text
yet
ke
do
```

</div>

在第一个棋盘中，不存在将上下边界连通的红色棋子序列，也不存在将左右边界连通的蓝色棋子序列，故目前未分出胜负。

在第二个棋盘中，上下两个边界由 $(1, 3)$、$(2, 3)$、$(3, 3)$、$(4, 3)$ 这些红色棋子连通了，所以小可可获胜了。

在第三个棋盘中，左右两个边界由 $(3, 1)$、$(2, 2)$、$(1, 3)$、$(1, 4)$ 这些蓝色棋子连通了，所以小多获胜了。

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 暴搜 - 深搜 / 广搜 $O(n)$

### 暴搜 - 深搜 / 广搜

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 100 + 9;
// -1: blue; 1: red; 0: blank
int d[MAXn][MAXn];
bool ved[MAXn][MAXn];
int n;
int px[] = {0,  0, 1,  1, -1, -1};
int py[] = {1, -1, 0, -1,  0,  1};

bool dfsf1(int i, int j) {
    if(i < 1 || j < 1 || i > n || j > n) return false;
    if(d[i][j] != -1) return false;
    if(j == n) return true;
    if(ved[i][j]) return false;
    ved[i][j] = true;
    for(int a = 0; a <= 5; a ++)
        if(dfsf1(i + px[a], j + py[a])) return true;
    return false;
}

bool dfs1(int i, int j) {
    if(i < 1 || j < 1 || i > n || j > n) return false;
    if(d[i][j] != 1) return false;
    if(i == n) return true;
    if(ved[i][j]) return false;
    ved[i][j] = true;
    for(int a = 0; a <= 5; a ++)
        if(dfs1(i + px[a], j + py[a])) return true;
    return false;
}

int main() {
    int T;
    scanf("%d", &T);
    for(int _ = 1; _ <= T; _ ++) {
        scanf("%d", &n);
        memset(d, 0, sizeof(d));
        for(int i = 1; i <= n; i ++)
            for(int j = 1; j <= n; j ++)
                scanf("%d", &d[i][j]);
        memset(ved, 0, sizeof(ved));
        bool can = false;
        for(int i = 1; i <= n; i ++)
            if(dfsf1(i, 1)) {
                printf("do\n");
                can = true;
                break;
            }
        if(can) continue;
        memset(ved, 0, sizeof(ved));
        for(int j = 1; j <= n; j ++)
            if(dfs1(1, j)) {
                printf("ke\n");
                can = true;
                break;
            }
        if(!can) printf("yet\n");
    }
    return 0;
}
```
