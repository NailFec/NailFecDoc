# 问题 J: 最长路径

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=9) | [eolymp's basecamp 11452](https://basecamp.eolymp.com/en/problems/11452)

## 题目描述

### 原题面

There is a directed graph $G$ with $n$ vertices and $m$ edges. The vertices are numbered $1, 2, ..., n$, and for each $i\:(1 \le i \le m)$, the $i$-th directed edge goes from vertex $x_i$ to vertex $y_i$.

$G\:$ does not contain directed cycles.

Find the length of the longest directed path in $G$. Here, the length of a directed path is the number of edges in it.

### 输入要求

The first line contains two integers: number of vertices $n\:(2 \le n \le 10^5)$ and number of edges $m\:(1 \le m \le 10^5)$. Each of the next $m$ lines contains two integer $x_i, y_i\:(1 \le x_i, y_i \le n)$ that describe the edge of the graph.

### 输出要求

Print the length of the longest directed path in $G$.

### 样例

<div class="grid" markdown>

```text
4 5
1 2
1 3
3 2
2 4
3 4
```

```text
3
```

</div>

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 法1：拓扑排序 + 动态规划 $O(n)$
- 法2：最短路 - SPFA 算法 $O(mn^2)$

### 法1：拓扑排序 + 动态规划

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;
const int MAXn = 2e5 + 9;
vector<int> g[MAXn];
int rd[MAXn], dp[MAXn];
queue<int> q;

signed main() {
    int n, m;
    scanf("%lld%lld", &n, &m);
    for(int i = 1, u, v; i <= m; i ++) {
        scanf("%lld%lld", &u, &v);
        u ++; v ++;
        g[u].push_back(v);
        rd[v] ++;
    }
    for(int i = 1; i <= n; i ++)
        if(!rd[i]) q.push(i);
    int ans = 1;
    while(!q.empty()) {
        int now = q.front();
        q.pop();
        for(int next : g[now]) {
            rd[next] --;
            if(!rd[next]) q.push(next);
            dp[next] = max(dp[next], dp[now] + 1);
            ans = max(ans, dp[next]);
        }
    }
    printf("%lld", ans);
    return 0;
}
```

### 法2：最短路 - SPFA 算法

读入每条边的值，并记为其负值。使用 SPFA 等可以在负权图中寻找最短路的算法找到图中最短路，并输出路径长度的负值。

由于 SPFA 等算法只能寻找单源最短路，因此需要再从每个点出发，跑一遍 SPFA 等算法，时间复杂度较高，不能通过。

以下提供类似题目 [P1807 最长路](https://www.luogu.com.cn/problem/P1807)，只需要求出某两点之间的路径中最长的路径，使用 SPFA 算法，按照上述思路的实现。（该题亦可使用 拓扑排序 + 动态规划 的方式实现）

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;
typedef pair<int, int> pii;
const int MAXn = 1000 + 9;
vector<pii> g[MAXn];
int dis[MAXn];
bool ved[MAXn];
queue<int> q;

signed main() {
    for(int i = 0; i <= MAXn - 3; i ++) dis[i] = LLONG_MAX;
    int n, m;
    scanf("%lld%lld", &n, &m);
    for(int i = 1, u, v, w; i <= m; i ++) {
        scanf("%lld%lld%lld", &u, &v, &w);
        g[u].push_back({v, -w});
    }
    dis[1] = 0;
    q.push(1);
    ved[1] = true;
    while(!q.empty()) {
        int now = q.front();
        q.pop();
        ved[now] = false;
        for(pii x : g[now]) {
            int nxt = x.first;
            if (dis[nxt] > dis[now] + x.second) {
                dis[nxt] = dis[now] + x.second;
                if (!ved[nxt]) {
                    q.push(nxt);
                    ved[nxt] = true;
                }
            }
        }
        
    }
    dis[n] == LLONG_MAX ? printf("-1") : printf("%lld", -dis[n]);
    return 0;
}
```
