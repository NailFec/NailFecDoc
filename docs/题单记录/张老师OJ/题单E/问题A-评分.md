# 问题 A: 评分

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=0) (1)
{ .annotate }

1.  题目来源：“科大国创杯”2023年安徽省青少年信息学科普日活动初中组 T1

## 题目描述

### 原题面

小可可在观看跳水比赛。

有 $n$ 名选手来参加跳水比赛，有 $m$ 名评委。在每位选手跳水之后，每位评委会给出他的分数。为了保证尽量公正客观，每位选手的得分是所有评委给出的分数中去掉最大值和最小值（如果有多个最大值/最小值，只去掉一个）之后，剩下的分数的平均值。

最后得分最大的选手获得第一名，得分第二大的选手获得第二名，以此类推。但是可能会出现同分的情况，在这种情况下，小可可会默认编号较小的选手排名更靠前。即，如果 $3$ 号选手和 $5$ 号选手的得分都是 $70$，那么小可可会认为 $3$ 号选手的排名比 $5$ 号选手更靠前。

现在小可可已经知道了所有选手得到所有评委的分数，他想让你帮他算出来选手的排名表，即对于 $1 \le i \le n$，算出排名第 $i$ 的选手的编号是什么。

对于 $30\%$ 的数据，满足 $n, m \le 3$；  
对于 $60\%$ 的数据，满足 $n, m \le 10$；  
对于 $100\%$ 的数据，满足 $2 \le n \le 100, 3 \le m \le 100, 0 \le a_{i,j} \le 100$。

### 输入要求

第一行两个整数 $n, m$，分别表示选手个数和评委个数。

 接下来 $n$ 行每行 $m$ 个整数，第 $i$ 行第 j 个整数 $a_{i,j}$ 表示在第 $i$ 个选手跳水之后，第 $j$ 个评委给出的分数。

### 输出要求

输出一行 $n$ 个整数，第 $i$ 个整数表示排名为 $i$ 的选手的编号。

### 样例

<div class="grid" markdown>

```text
4 4
4 70 69 34
18 43 85 71
100 50 69 80
67 82 90 43
```

```text
3 4 2 1
```

</div>

四位选手的去掉最大、最小值之后的平均分分别是：$51.5$, $57$, $74.5$, $74.5$，但由于三号选手编号比四号选手小，所以排名从 $1$ 到 $4$ 的选手分别为：$3$, $4$, $2$, $1$。

## 解法

每位选手都是由 $m$ 位评委评分的，因此求平均值步骤可以省去。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
typedef pair<lli, int> pii;
vector<pii> d;    // first: v; second: num

bool cmp(const pii & x, const pii & y) {
    if(x.first == y.first) return x.second < y.second;
    return x.first > y.first;
}

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i ++) {
        lli sum = 0;
        int mx = INT_MIN, mn = INT_MAX;
        for(int j = 1, ia; j <= m; j ++) {
            scanf("%d", &ia);
            sum += ia;
            mx = max(mx, ia);
            mn = min(mn, ia);
        }
        sum -= mx + mn;
        d.push_back({sum, i});
    }
    sort(d.begin(), d.end(), cmp);
    for(pii x : d) printf("%d ", x.second);
    return 0;
}
```
