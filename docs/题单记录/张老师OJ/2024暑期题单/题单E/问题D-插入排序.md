# 问题 D: 插入排序

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=3) | [洛谷 P7910 [CSP-J 2021] 插入排序](https://www.luogu.com.cn/problem/P7910) (1)
{ .annotate }

1.  题目来源：CSP-J 2021 第二轮 T2

## 题目描述

### 原题面

插入排序是一种非常常见且简单的排序算法。小 Z 是一名大一的新生，今天 H 老师刚刚在上课的时候讲了插入排序算法。

假设比较两个元素的时间为 $\mathcal O(1)$，则插入排序可以以 $\mathcal O(n^2)$ 的时间复杂度完成长度为 $n$ 的数组的排序。不妨假设这 $n$ 个数字分别存储在 $a_1, a_2, \ldots, a_n$ 之中，则如下伪代码给出了插入排序算法的一种最简单的实现方式：

这下面是 C/C++ 的示范代码：

```cpp
for (int i = 1; i <= n; i++)
    for (int j = i; j >= 2; j--)
        if (a[j] < a[j-1]) {
            int t = a[j-1];
            a[j-1] = a[j];
            a[j] = t;
        }
```

这下面是 Pascal 的示范代码：

```pascal
for i:=1 to n do
    for j:=i downto 2 do
        if a[j]<a[j-1] then
            begin
                t:=a[i];
                a[i]:=a[j];
                a[j]:=t;
            end;
```

为了帮助小 Z 更好的理解插入排序，小 Z 的老师 H 老师留下了这么一道家庭作业：

H 老师给了一个长度为 $n$ 的数组 $a$，数组下标从 $1$ 开始，并且数组中的所有元素均为非负整数。小 Z 需要支持在数组 $a$ 上的 $Q$ 次操作，操作共两种，参数分别如下：

$1~x~v$：这是第一种操作，会将 $a$ 的第 $x$ 个元素，也就是 $a_x$ 的值，修改为 $v$。保证 $1 \le x \le n$，$1 \le v \le 10^9$。**注意这种操作会改变数组的元素，修改得到的数组会被保留，也会影响后续的操作**。

$2~x$：这是第二种操作，假设 H 老师按照**上面的伪代码**对 $a$ 数组进行排序，你需要告诉 H 老师原来 $a$ 的第 $x$ 个元素，也就是 $a_x$，在排序后的新数组所处的位置。保证 $1 \le x \le n$。**注意这种操作不会改变数组的元素，排序后的数组不会被保留，也不会影响后续的操作**。

H 老师不喜欢过多的修改，所以他保证类型 $1$ 的操作次数不超过 $5000$。

小 Z 没有学过计算机竞赛，因此小 Z 并不会做这道题。他找到了你来帮助他解决这个问题。

### 输入要求

第一行，包含两个正整数 $n, Q$，表示数组长度和操作次数。

第二行，包含 $n$ 个空格分隔的非负整数，其中第 $i$ 个非负整数表示 $a_i$。

接下来 $Q$ 行，每行 $2 \sim 3$ 个正整数，表示一次操作，操作格式见【**题目描述**】。

### 输出要求

对于每一次类型为 $2$ 的询问，输出一行一个正整数表示答案。

### 样例

<div class="grid" markdown>

```text
3 4
3 2 1
2 3
1 3 2
2 2
2 3
```

```text
1
1
2
```

</div>

## 解法

注意到修改操作的次数并不多，而查询操作的次数较多，因此需要使用较低的时间复杂度完成查询操作，而修改操作时间复杂度可以更高。不能使用模拟的方法完成。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> pii;
const int MAXn = 8000 + 9;
int d[MAXn], p[MAXn];
vector<pii> v;    // first: val; second: i

bool cmp(const pii & x, const pii & y) {
    if(x.first == y.first) return x.second < y.second;
    return x.first < y.first;
}

int main() {
    int n, q;
    scanf("%d%d", &n, &q);
    for(int i = 1; i <= n; i ++) {
        scanf("%d", &d[i]);
        v.push_back({d[i], i});
    }
    sort(v.begin(), v.end(), cmp);
    for(int posi = 1; pii x : v) p[x.second] = posi ++;
    for(int i = 1, op, ia, ib; i <= q; i ++) {
        scanf("%d%d", &op, &ia);
        if(op == 1) {
            scanf("%d", &ib);
            if(d[ia] == ib) continue;
            for(int j = 1; j <= n; j ++)
                // 1 2 3 4 5 6
                // 1 5 3 4 5 6
                if(((d[j] < d[ia]) || (d[ia] == d[j] && j < ia))
                && ((d[j] > ib)    || (d[j] == ib && j > ia)))
                    p[j] ++, p[ia] --;
                else
                if(((d[j] > d[ia]) || (d[ia] == d[j] && j > ia))
                && ((d[j] < ib)    || (d[j] == ib && j < ia)))
                    p[j] --, p[ia] ++;
            d[ia] = ib;
        }
        else printf("%d\n", p[ia]);
    }
    return 0;
}
```
