# ST 表

## 读入和打表

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
}
```

## 查询

```cpp
    int l, r;
    scanf("%d%d", &l, &r);
    lg = log2(r - l + 1);
    len = 1 << lg;
    printf("%d\n", max(mx[l][lg], mx[r - len + 1][lg])
                 - min(mn[l][lg], mn[r - len + 1][lg]));
```