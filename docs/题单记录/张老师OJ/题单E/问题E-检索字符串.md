# 问题 E: 检索字符串

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=4) | 字符串 Ad-Hoc

## 题目描述

### 原题面

对于给定的 $n$ 个字符串，它们的任意顺序排列之后得到了串联字符串。比如：$n=3$，字符串分别为“$\texttt{ab}$”，“$\texttt{cd}$”，“$\texttt{ef}$”，串联子串如下：

“$\texttt{abcdef}$”，“$\texttt{abefcd}$”，“$\texttt{cdabef}$”，“$\texttt{cdefab}$”，“$\texttt{efabcd}$”，“$\texttt{efcdab}$” 都是串联字符串。“$\texttt{acdbef}$” 不是串联子串，因为它不是有效的排列连接。

现在，程序给定一个需要检索的字符串，查询该检索字符串中是否含有上述各种串联字符串，如果有的话，按从小到大的顺序，依次输出其对应的位置编号（如有多个，用空格分隔）。如果没有的话，输出 $-1$。

### 输入要求

第一行，输入 $n$ 的值。（$1\le n<10$）

接下来的 $n$ 行，每行输入需要串联的字符串 $P_i$。（$1\le |Pi|\le 10$）

在接下来的第 $n+1$ 行，输入需要检索的字符串 $S$。（$1\le |S|\le 1000$）

### 输出要求

一行，输出符合题意的位置编号（如果有多个，按从小到大输出，用空格分隔。如果没有，输出-1）。

### 样例

<div class="grid" markdown>

```text
2
foo
bar
barfoothefoobarman
```

```text
0 9
```

</div>

<div class="grid" markdown>

```text
3
bar
foo
the
barfoofoobarthefoobarman
```

```text
6 9 12
```

</div>

<div class="grid" markdown>

```text
4
word
good
best
word
wordgoodgoodgoodbestword
```

```text
-1
```

</div>

## 解法

对于每个可能的字符串，它们的长度 $len \equiv \sum_{i=1}^{n} {|P_i|}$。因此遍历每个长度为 $len$ 的区间，判断该区间是否符合条件。

以下不同解法的区别在于判断该区间是否符合条件。

### 全排列 + 哈希

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> pii;
const int BASE1 = 13131, P1 = 1e9 + 7;
const int BASE2 =  1331, P2 = 1e9 + 9;
const int MAXn = 30 + 9;
string oris[MAXn];
vector<int> v;
vector<pii> msc[11];
const int MAXm = 1e4 + 9;
int h1[MAXm], b1[MAXm];
int h2[MAXm], b2[MAXm];

pii fsha(int l, int r) {
    int ans1 = (h1[r] - 1ll * h1[l - 1] * b1[r - l + 1] % P1 + P1) % P1;
    int ans2 = (h2[r] - 1ll * h2[l - 1] * b2[r - l + 1] % P2 + P2) % P2;
    return {ans1, ans2};
}

int main() {
    int n, len = 0;
    scanf("%d", &n);
    getline(cin, oris[1]);
    for(int i = 1; i <= n; i ++) {
        getline(cin, oris[i]);
        v.push_back(i);
        len += oris[i].length();
    }
    do {
        int sha1 = 0, sha2 = 0;
        string s = "";
        for(int p : v)
            s += oris[p];
        for(char c : s)
            sha1 = (1ll * sha1 * BASE1 + c) % P1,
            sha2 = (1ll * sha2 * BASE2 + c) % P2;
        msc[sha1 % 10].push_back({sha1, sha2});
    }
    while(next_permutation(v.begin(), v.end()));
    string ppc;
    getline(cin, ppc);
    int m = ppc.length();
    ppc = "#" + ppc;
    h1[0] = h2[0] = b1[0] = b2[0] = 1;
    for(int i = 1; i <= m; i ++) {
        h1[i] = (1ll * h1[i - 1] * BASE1 + ppc[i]) % P1;
        h2[i] = (1ll * h2[i - 1] * BASE2 + ppc[i]) % P2;
        b1[i] = (1ll * b1[i - 1] * BASE1) % P1;
        b2[i] = (1ll * b2[i - 1] * BASE2) % P2;
    }
    bool can = false;
    for(int i = 1; i + len - 1 <= m; i ++) {
        pii sha = fsha(i, i + len - 1);
        int md = sha.first % 10;
        for(pii x : msc[md]) {
            if(sha == x) {
                can = true;
                printf("%d ", i - 1);
                break;
            }
        }
    }
    if(!can) printf("-1");
    return 0;
}
```

### 搜索

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn = 100 + 9;
string d[MAXn];
int n;
string s;

bool used[MAXn];
bool dfs(int i, int j) {
    if(i - 1 == j) return true;
    int len = j - i + 1;
    for(int k = 1; k <= n; k ++) {
        if(!used[k] && d[k].length() <= len && s.substr(i, d[k].length()) == d[k]) {
            used[k] = true;
            if(dfs(i + d[k].length(), j))
                return true;
            used[k] = false;
        }
    }
    return false;
}

int main() {
    int len = 0;
    scanf("%d", &n);
    getline(cin, d[1]);
    for(int i = 1; i <= n; i ++) {
        getline(cin, d[i]);
        len += d[i].length();
    }
    getline(cin, s);
    int m = s.length();
    s = "#" + s;
    bool can = false;
    for(int i = 1; i + len - 1 <= m; i ++)
        if(memset(used, 0, sizeof(used)) && dfs(i, i + len - 1)) {
            can = true;
            printf("%d ", i - 1);
        }
    if(!can) printf("-1");
    return 0;
}
```
