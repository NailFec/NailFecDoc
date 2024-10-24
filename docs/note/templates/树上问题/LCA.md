# LCA

## 倍增求 LCA

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 5e5 + 9;
vector<int> g[MAXn];
vector<int> order;
int depth[MAXn];    // depth[tp] = 1
int fa[MAXn][26];

void dfs(int x, int f, int dph) {
	order.push_back(x);
	fa[x][0] = f;
	depth[x] = dph;
	for(int y : g[x]) if(y != f)
		dfs(y, x, dph + 1);
}

void dbz(int & x, int cdph) {
	for(int k = 24; k >= 0 && cdph; k --) {
		if((1 << k) <= cdph) {
			x = fa[x][k];
			cdph -= (1 << k);
		}
	}
}

// void dbz(int & x, int cdph) {
// 	if(cdph == 0) return;
// 	for(int k = 24; k >= 0; k --) {
// 		if((1 << k) <= cdph) {
// 			x = fa[x][k];
// 			dbz(x, cdph - (1 << k));
// 			return;
// 		}
// 	}
// }

// void dbz(int & x, int cdph) {
// 	while(cdph) {
// 		x = fa[x][0];
// 		cdph --;
// 	}
// }

int LCA(int x, int y, int k) {
	if(x == y) return x;
	for(int k = 24; k >= 0; k --) {
		if(fa[x][k] != fa[y][k]) {
			x = fa[x][k];
			y = fa[y][k];
		}
	}
	return fa[x][0];
}

int main() {
	int n, m, tp;
	scanf("%d%d%d", &n, &m, &tp);
	for(int i = 1, x, y; i <= n - 1; i ++) {
		scanf("%d%d", &x, &y);
		g[x].push_back(y);
		g[y].push_back(x);
	}
	dfs(tp, 0, 1);
	for(int k = 1; k < 25; k ++) {
		for(int x : order) {
			fa[x][k] = fa[fa[x][k - 1]][k - 1];
		}
	}
	for(int i = 1, u, v, q; i <= m; i ++) {
		scanf("%d%d", &u, &v);
		if(depth[u] > depth[v]) swap(u, v);
		dbz(v, depth[v] - depth[u]);
		q = LCA(u, v, 24);
		printf("%d\n", q ? q : tp);
	}
	return 0;
}
```

## Tarjan 求 LCA
