# KMP

## 拼接 KMP

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 1e6 + 9;
int b[MAXn << 1];

int main() {
    string s, t;
    cin >> s >> t;
    s = "#" + t + "#" + s;
    int slen = s.size() - 1;
    int tlen = t.size();
    for(int i = 2; i <= slen; i ++) {
        int p = b[i - 1];
        while(p && s[p + 1] != s[i]) p = b[p];
        if(p) b[i] = p + 1;
        else b[i] = s[1] == s[i] ? 1 : 0;
        if(b[i] == tlen) printf("%d\n", i - 2 * tlen);
    }
    for(int i = 1; i <= tlen; i ++)
        printf("%d ", b[i]);
    return 0;
}
```

## 匹配 KMP
