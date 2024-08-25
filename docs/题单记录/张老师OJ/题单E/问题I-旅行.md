# 问题 I: 旅行

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=8)

## 题目描述

### 原题面

乐乐要是开始他的背包旅行，这次共 $n$ 个城市，编号为 $1 \sim n$。乐乐从编号为 $1$ 的城市出发，前往编号为 $n$ 的城市。每个城市都有一件物品，重量为 $w_i$，价值为 $v_i$。乐乐从一个城市到另一个城市，如果背包中的物品重量为 $a$，行走距离为 $b$ 时，花费的体力为 $a \times b$，乐乐最多只能背总重量为 $W$ 的物品。乐乐希望到达 $n$ 时，背包中的物品价值最大，同时花费的体力最小。$n$ 个城市之间共 $m$ 条单向路，且无环，从一个城市出发之后，无法再回到这个城市。

### 输入要求

第一行 $3$ 个整数 $n,\: m,\:W$。

接下来 $n$ 行，每行 $2$ 个整数表示 $w_i$ 和 $v_i$。

接下来 $m$ 行，每行 $3$ 个整数 $x_i,\:y_i,\:z_i$，无重边，无自环。数据保证 $1$ 可以到达 $n$。

### 输出要求

输出 $2$ 个整数，表示最大值价值和获得最大值情况下最小的体力消耗。

### 样例

<div class="grid" markdown>

```text
5 6 10
2 2
1 3
3 5
4 2
2 3
1 2 1
2 4 5
2 5 3
1 3 4
3 4 2
4 5 2
```

```text
10 20
```

</div>

![graph](../igraph1.png){width=300px}

```text
背包容量：10
（X代表装不下背包，该方案被舍弃）
------------------------------------------------------------------
法1：1 → 3 → 4 → 5
选择：1345  134  135  145  345  13  14  15  34  35  45  1  3  4  5
容量：X11   9    7    8    9    5   6   4   7   5   6   2  3  4  2
价值：X12   9    10   7    10   7   4   5   7   8   5   2  5  2  3
体力：X36   36   28   24   20   28  24  16  20  12  8   16 12 8  0
---
体力计算：1: 16; 3: 12; 4: 8; 5: 0
------------------------------------------------------------------
法2：1 → 2 → 5
选择：125  12  15  25  1  2  5
价值：5    3   4   3   2  1  2
体力：11   11  8   3   8  3  0
---
体力计算：1: 8; 2: 3; 5: 0
------------------------------------------------------------------
法3：1 → 2 → 4 → 5
选择：1245  124  125  145  245  12  14  15  24  25  45  1  2  4  5  
价值：9     7    5    8    7    3   6   4   5   3   6   2  1  4  2
体力：31    31   23   24   15   23  24  16  15  7   8   16 7  8  0
---
体力计算：1: 16; 2: 7; 4: 8; 5: 0
------------------------------------------------------------------
答案路线：1(不选) → 3(选) → 4(选) → 5(选)
最大价值：10
最小体力：20
```

## 解法

DAG 图，使用拓扑排序；使用 01 背包完成动态规划状态转移。

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;
typedef pair<signed, int> pii;
const int MAXn = 8e3 + 9;
const int MAXw = 1e3 + 9;
int w[MAXn], v[MAXn], rd[MAXn];
vector<pii> g[MAXn];
queue<signed> que;
vector<signed> ord;
int dp[MAXn][MAXw], ans[MAXn][MAXw];
bool can[MAXn];
 
signed main() {
    // freopen("test/travel10.in", "r", stdin);
    // freopen("test/travel10.out", "w", stdout);
    for(int i = 0; i <= MAXn - 3; i ++)
        for(int j = 0; j <= MAXw - 3; j ++)
            ans[i][j] = LLONG_MAX - 1;
    int n, m, W;
    scanf("%lld%lld%lld", &n, &m, &W);
    for(int i = 1; i <= n; i ++)
        scanf("%lld%lld", &w[i], &v[i]);
    for(int i = 1, x, y, z; i <= m; i ++) {
        scanf("%lld%lld%lld", &x, &y, &z);
        g[x].push_back({y, z});
        rd[y] ++;
    }
    // 拓扑排序
    for(int i = 1; i <= n; i ++)
        if(rd[i] == 0)
            que.push(i), ord.push_back(i);
    while(!que.empty()) {
        signed u = que.front();
        que.pop();
        for(pii x : g[u]) {
            rd[x.first] --;
            if(rd[x.first] == 0)
                que.push(x.first), ord.push_back(x.first);
        }
    }
    // 01 背包
    can[1] = true;
    for(int j = 0; j <= W; j ++) {
        ans[1][j] = 0;
        if(j >= w[1]) dp[1][j] = v[1];
    }
    int i = 0;
    while(i < n && ord[i] != 1) i ++;
    for(; i < ord.size(); i ++) {
        signed now = ord[i];
        if(!can[now]) continue;
        for(pii x : g[now]) {
            signed nxt = x.first;
            int dis = x.second;
            can[nxt] = true;
            for(int j = W; j >= 0; j --) {
                if(j >= w[nxt]) {
                    if(dp[now][j - w[nxt]] + v[nxt] > dp[nxt][j]) {
                        dp[nxt][j] = dp[now][j - w[nxt]] + v[nxt];
                        ans[nxt][j] = ans[now][j - w[nxt]] + dis * (j - w[nxt]);
                    }
                    else if(dp[now][j - w[nxt]] + v[nxt] == dp[nxt][j])
                        ans[nxt][j] = min(ans[nxt][j], ans[now][j - w[nxt]] + dis * (j - w[nxt]));
                }
                if(dp[now][j] > dp[nxt][j]) {
                    dp[nxt][j] = dp[now][j];
                    ans[nxt][j] = ans[now][j] + dis * j;
                }
                else if(dp[now][j] == dp[nxt][j])
                    ans[nxt][j] = min(ans[nxt][j], ans[now][j] + dis * j);
            }
        }
    }
    int fdp = 0, fans = LLONG_MAX;
    for(int j = 0; j <= W; j ++)
        if(dp[n][j] > fdp || (dp[n][j] == fdp && ans[n][j] < fans))
            fdp = dp[n][j], fans = ans[n][j];
    printf("%lld %lld", fdp, fans);
    return 0;
}
```
