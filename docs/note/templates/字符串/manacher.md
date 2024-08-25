# manacher

```cpp
#include<bits/stdc++.h>
#define int long long int
using namespace std;
const int MAXn = 1.1e7 + 9;
string os, s;
int d[(MAXn << 1) + 9];

signed main() {
    getline(cin, os);
    s = "@";
    for(char c : os)
        s.push_back('#'),
        s.push_back(c);
    s.push_back('#');
    int len = s.length() - 1, ans = 1;
    for(int t = 1, r = 0, m = 0; t <= len; t ++) {
        if(t <= r) d[t] = min(d[(m << 1) - t], r - t + 1);
        while(s[t - d[t]] == s[t + d[t]]) d[t] ++;
        if(d[t] + t > r) r = d[t] + t - 1, m = t;
        ans = max(ans, d[t] - 1);
    }
    printf("%lld", ans);
    return 0;
}
```
