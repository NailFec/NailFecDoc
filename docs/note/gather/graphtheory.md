# 图论

## 存图

!!! info

    难度【4】，辞典P60，深进P0，一本通P0，[OIwiki/图的存储](https://oi-wiki.org/graph/save/)

区别：书写难度、空间复杂度、判断是否连边的时间复杂度

稀疏图：

对于多次建图、有离线部分、[【6】Kruskal 算法](#kruskal-算法) 需要按边权排序，需要额外单独存放边

### 【4】邻接矩阵

```cpp
int g[MAXn][MAXn];

void f(int p, int q, int v) {
    g[p][q] = v;
    // g[q][p] = v;
}
```

### 邻接表

使用 [【4】vector](#4vector) 实现方便排序入点，使用 [【4】链式前向星](#4链式前向星) 实现方便 [【8】网络流](#网络流) 算法存放双边时能够快速求出反边

#### 【4】vector

```cpp
typedef pair<int, int> pii;
vector<pii> g[MAXn];

void f(int p, int q, v) {
    g[p].push_back(pii{q, v});
    // g[q].push_back(pii{p, v});
}
```

#### 【4】链式前向星

```cpp
struct node {
    int p, v, nxt;
} e[MAXn << 1];
int head[MAXn], cnt = 0;

// a 加到 b 头部
void add(int a, int b, int v) {
    e[++cnt].p = a;
    e[cnt].v = v;
    e[cnt].nxt = head[b];
    head[b] = cnt;
}

void visit(int a) {
    for(int p = head[a]; p; p = e[p].nxt) {
        int b = e[p].p;
        int v = e[p].v;
    }
}
```

## 遍历

!!! info

    难度【4~5】，辞典P88，深进P0，一本通P0，[OIwiki/dfs](https://oi-wiki.org/graph/dfs/) & [OIwiki/bfs](https://oi-wiki.org/graph/bfs/)

### 【4】深度优先遍历

```cpp
bool ved[MAXn];
void dfs(int p) {
    ved[p] = true;
    for(int q : g[p])
        if(!ved[q]) dfs(q);
}
```

### 【4】广度优先遍历

```cpp
bool ved[MAXn];
queue<int> que;
que.push(1);
while(!que.empty()) {
    int p = que.front();
    que.pop();
    for(int q : g[p])
        if(!ved[q]) {
            ved[q] = true;
            que.push(q);
        }
}
```

### 【4】泛红算法

= 洪水填充算法 = 图的染色

## 最短路

!!! info

    难度【6~7】，辞典P194，深进P167，一本通P119，[OIwiki](https://oi-wiki.org/graph/shortest-path/)

- [P3371 【模板】单源最短路径（弱化版）](https://www.luogu.com.cn/problem/P3371)
- [P4779 【模板】单源最短路径（标准版）](https://www.luogu.com.cn/problem/P4779)
- [P4568 [JLOI2011] 飞行路线](https://www.luogu.com.cn/problem/P4568)
- [P1629 邮递员送信](https://www.luogu.com.cn/problem/P1629)

经典问题：1 两点最短路、2 单源最短路、3 单源次短路、4 多源最短路

| 算法 | 作用 | 限制 | 检测负环 | 时间复杂度 |
|---|---|---|---|---|
| [Floyd](#6floyd-算法) | 1 | 所有 | 能 | $O(n^3)$ |
|  [Bellman-Ford](#6bellman-ford-算法) | 2 | 所有 | 能 | $O(mn)$ |
| [Dijkstra](#6dijkstra-算法) | 2 | 非负权图 | 不能 | $O(m\log m)$ |
| [Johnson](#6johnson-算法) | 1 | 所有 | 能 | $O(mn\log m)$ |

### 【6】Dijkstra 算法

朴素算法时间复杂度 $O(n^2 + m)$，使用堆优化时间复杂度 $O(m\log m)$。
在 $m$ 与 $n$ 同级时，堆优化的时间复杂度低；在 $m$ 与 $n^2$ 同级时，朴素算法的[时间复杂度](#最短路)低。

- 优点：时间复杂度低
- 缺点：仅适用于非负权图

### 【6】SPFA 算法

- 缺点：一个点可能多次入队，因此[时间复杂度](#最短路)高

### 【6】Bellman-Ford 算法

### 【6】Floyd 算法

= Floyd-Warshall 算法

### 【6】Johnson 算法

### 技巧

1. 当一部分边具有特殊限制，如限制走的次数，可以将图复制多次为**分层图**
2. 善于拆分节点、合并节点，创建虚拟节点、虚拟边
3. 注意自环、负环、重边等情况
4. 查找所有节点到某一结点的最短路，可以将图反向建边

## 负环

## 差分约束

## 最小生成树

### Prim 算法

### Kruskal 算法

### 次小生成树

## 最小树形图

## 拓补排序

## 欧拉图 欧拉回路

### 二分图

= 偶图

### 强连通分量

### 割点 割边

## 传递闭包

## 2-SAT

## 网络流

## 图的支配集 独立集 覆盖集

## 匈牙利算法

## KM 算法

## 一般图的匹配

---

todo:

### 有向无环图

### 连通图 强连通图

### 双连通