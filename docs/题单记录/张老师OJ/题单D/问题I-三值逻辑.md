# 问题 I: 三值逻辑

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=8) | [洛谷 P9869](https://www.luogu.com.cn/problem/P9869) (1)
{ .annotate }

1.  题目来源：NOIP 2023 提高组 T2

## 题目描述

### 原题面

小 L 今天学习了 Kleene 三值逻辑。

在三值逻辑中，一个变量的值可能为：真（$\mathit{True}$，简写作 $\mathit{T}$）、假（$\mathit{False}$，简写作 $\mathit{F}$）或未确定（$\mathit{Unknown}$，简写作 $\mathit{U}$）。

在三值逻辑上也可以定义逻辑运算。由于小 L 学习进度很慢，只掌握了逻辑非运算 $\lnot$，其运算法则为：
$\lnot \mathit{T} = \mathit{F}, \lnot \mathit{F} = \mathit{T}, \lnot\mathit{U} = \mathit{U}.$

现在小 L 有 $n$ 个三值逻辑变量 $x_1,\cdots, x_n$。小 L 想进行一些有趣的尝试，于是他写下了 $m$ 条语句。语句有以下三种类型，其中 $\leftarrow$ 表示赋值：

1. $x_i \leftarrow v$，其中 $v$ 为 $\mathit{T}, \mathit{F}, \mathit{U}$ 的一种；
2. $x_i \leftarrow x_j$；
3. $x_i \leftarrow \lnot x_j$。

一开始，小 L 会给这些变量赋初值，然后按顺序运行这 $m$ 条语句。

小 L 希望执行了所有语句后，所有变量的最终值与初值都相等。在此前提下，小 L 希望初值中 $\mathit{Unknown}$ 的变量尽可能少。

在本题中，你需要帮助小 L 找到 $\mathit{Unknown}$ 变量个数最少的赋初值方案，使得执行了所有语句后所有变量的最终值和初始值相等。小 L 保证，至少对于本题的所有测试用例，这样的赋初值方案都必然是存在的。

对于所有测试数据，保证： (1)
{ .annotate }

1.  对于每个测试点的数据范围和特殊限制，见 [洛谷 P9869](https://www.luogu.com.cn/problem/P9869)
<!---->

- $1 \le t \le 6$，$1 \le n,m \le 10 ^ 5$；
- 对于每个操作，$v$ 为 `TFU+-` 中的某个字符，$1 \le i,j \le n$。


### 输入要求

**本题的测试点包含有多组测试数据。**

输入的第一行包含两个整数 $c$ 和 $t$，分别表示测试点编号和测试数据组数。对于样例，$c$ 表示该样例与测试点 $c$ 拥有相同的限制条件。

接下来，对于每组测试数据：

- 输入的第一行包含两个整数 $n$ 和 $m$，分别表示变量个数和语句条数。
- 接下来 $m$ 行，按运行顺序给出每条语句。
  - 输入的第一个字符 $v$ 描述这条语句的类型。保证 $v$ 为 `TFU+-` 的其中一种。
  - 若 $v$ 为 `TFU` 的某一种时，接下来给出一个整数 $i$，表示该语句为 $x_i \leftarrow v$；
  - 若 $v$ 为 `+`，接下来给出两个整数 $i,j$，表示该语句为 $x_i \leftarrow x_j$；
  - 若 $v$ 为 `-`，接下来给出两个整数 $i,j$，表示该语句为 $x_i \leftarrow \lnot x_j$。

### 输出要求

对于每组测试数据输出一行一个整数，表示所有符合条件的赋初值方案中，$\mathit{Unknown}$ 变量个数的最小值。

### 样例

<div class="grid" markdown>

```text
1 3
3 3
- 2 1
- 3 2
+ 1 3
3 3
- 2 1
- 3 2
- 1 3
2 2
T 2
U 2
```

```text
0
3
1
```

</div>

对于上述样例 (1) ：第一组测试数据中，$m$ 行语句依次为
{ .annotate }

1.  对于其余 $3$ 个样例点，见 [洛谷 P9869](https://www.luogu.com.cn/problem/P9869)
<!---->

- $x_2 \leftarrow \lnot x_1$；
- $x_3 \leftarrow \lnot x_2$；
- $x_1 \leftarrow x_3$。

一组合法的赋初值方案为 $x_1 = \mathit{T}, x_2 = \mathit{F}, x_3 = \mathit{T}$，共有 $0$ 个$\mathit{Unknown}$ 变量。因为不存在赋初值方案中有小于 $0$ 个$\mathit{Unknown}$ 变量，故输出为 $0$。

第二组测试数据中，$m$ 行语句依次为

- $x_2 \leftarrow \lnot x_1$；
- $x_3 \leftarrow \lnot x_2$；
- $x_1 \leftarrow \lnot x_3$。

唯一的赋初值方案为 $x_1 = x_2 = x_3 = \mathit{U}$，共有 $3$ 个$\mathit{Unknown}$ 变量，故输出为 $3$。

第三组测试数据中，$m$ 行语句依次为

- $x_2 \leftarrow \mathit{T}$；
- $x_2 \leftarrow \mathit{U}$；

一个最小化 $\mathit{Unknown}$ 变量个数的赋初值方案为 $x_1 = \mathit{T}, x_2 = \mathit{U}$。$x_1 = x_2 = \mathit{U}$ 也是一个合法的方案，但它没有最小化 $\mathit{Unknown}$ 变量的个数。

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 搜索 $O(n)$

### 搜索

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 1e5 + 9;
int n, m;
struct table{
    int p2;
    bool b;    // 0: same;  1: reverse
} tmpT;
vector<table> graph[MAXn + 9];
struct ori{
    bool m;    // 0: TFU;  1: +-
    int val0;    // TFU -> 012
    int val1;    // +- -> +inc -inc
} tmpO;
ori datum[MAXn + 9];

int retTFU(char in) {
    if(in == 'T') return 0;
    if(in == 'F') return 1;
    if(in == 'U') return 2;
    else cerr << "retTFU error\n";
    return 2;
}

int revTFU(int in) {
    if(in == 0) return 1;
    if(in == 1) return 0;
    if(in == 2) return 2;
    else cerr << "revTFU error\n";
    return 2;
}

bool visited[MAXn + 9];
bool haveU = false;
lli gsize = 0;
void dfssize(int p) {
    gsize ++;
    visited[p] = true;
    if(datum[p].m == 0 && datum[p].val0 == 2)
        haveU = true;
    for(table i : graph[p])
        if(!visited[i.p2]) dfssize(i.p2);
}

bool can = true;
int dfsval[MAXn+9];
void dfscan(int p) {
    gsize++;
    visited[p] = true;
    for(table i : graph[p]) {
        if(!visited[i.p2]) {
            if(i.b) dfsval[i.p2] = (dfsval[p] == 0) ? 1 : 0;    // reverse
            else dfsval[i.p2] = (dfsval[p] == 0) ? 0 : 1;    // same
            dfscan(i.p2);
        }
        else{    // visited[i.p2]
            int should;
            if(i.b) should = (dfsval[p] == 0) ? 1 : 0;    // reverse
            else should = (dfsval[p] == 0) ? 0 : 1;    // same
            if(dfsval[i.p2] != should) can = false;
        }
    }
}

int main() {
    int non, T;
    scanf("%d %d", &non, &T);
    for(int _ = 1; _ <= T; _++) {
        scanf("%d %d", &n, &m);
        lli res = 0;
        for(int i = 1; i <= MAXn+3; i++) {
            datum[i].m = 1;
            datum[i].val1 = i;
        }
        for(int i = 1; i <= MAXn+3; i++)
            graph[i].clear();
        memset(visited, 0, sizeof(visited));

        char ina;
        int inb, inc;
        string instr;
        for(int iout = 1; iout <= m; iout++) {
            getline(cin, instr);
            ina = getchar();
            scanf("%d", &inb);
            if(ina == '+' || ina == '-') {
                scanf("%d", &inc);
                if(ina == '+')
                    datum[inb] = datum[inc];
                else{    // ina == '-'
                    if(datum[inc].m) {    // +-
                        datum[inb].m = 1;
                        datum[inb].val1 = -datum[inc].val1;
                    }
                    else{    // TFU
                        datum[inb].m = 0;
                        datum[inb].val0 = revTFU(datum[inc].val0);
                    }
                }
            }
            else{    // ina == TFU
                datum[inb].m = 0;
                datum[inb].val0 = retTFU(ina);
            }
        }

        for(int i = 1; i <= n; i++)
            if(datum[i].m) {    // +-
                tmpT.p2 = abs(datum[i].val1);
                tmpT.b = (datum[i].val1<0);
                graph[i].push_back(tmpT);
                tmpT.p2 = i;
                graph[abs(datum[i].val1)].push_back(tmpT);
            }
        for(int i = 1; i <= n; i++)
            if(!visited[i] && !datum[i].m) {    // TFU
                gsize = 0;
                haveU = false;
                dfssize(i);
                if(haveU) res += gsize;
            }
        for(int i = 1; i <= n; i++)
            if(!visited[i]) {
                gsize = 0;
                can = true;
                dfsval[i] = 0;
                dfscan(i);
                if(!can) res += gsize;
            }
        printf("%lld\n", res);
    }
    return 0;
}
```
