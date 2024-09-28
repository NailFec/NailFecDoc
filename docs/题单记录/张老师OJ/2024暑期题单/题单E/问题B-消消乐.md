# 问题 B: 消消乐

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=1) | [洛谷 P9753 [CSP-S 2023] 消消乐](https://www.luogu.com.cn/problem/P9753) (1)
{ .annotate }

1.  题目来源：CSP-S 2023 第二轮 T2

## 题目描述

### 原题面

小 L 现在在玩一个低配版本的消消乐，该版本的游戏是一维的，一次也只能消除两个相邻的元素。

现在，他有一个长度为 $n$ 且仅由小写字母构成的字符串。我们称一个字符串是可消除的，当且仅当可以对这个字符串进行若干次操作，使之成为一个空字符串。

其中每次操作可以从字符串中删除两个相邻的相同字符，操作后剩余字符串会拼接在一起。

小 L 想知道，这个字符串的所有非空连续子串中，有多少个是可消除的。

对于所有测试数据有：$1 \le n \le 2 \times 10^6$，且询问的字符串仅由小写字母构成。

### 输入要求

输入的第一行包含一个正整数 $n$，表示字符串的长度。

输入的第二行包含一个长度为 $n$ 且仅由小写字母构成的的字符串，表示题目中询问的字符串。

### 输出要求

输出一行包含一个整数，表示题目询问的答案。

### 样例

<div class="grid" markdown>

```text
8
accabccb
```

```text
5
```

</div>

一共有 $5$ 个可消除的连续子串，分别是 `cc`、`acca`、`cc`、`bccb`、`accabccb`。

## 解法

!!! info ""

    此题解法众多，以下方法不是最优解。类似题目：[洛谷 P5658 [CSP-S2019] 括号树](https://www.luogu.com.cn/problem/P5658)

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn = 2e6 + 9;
lli dp[MAXn], match[MAXn], pre[MAXn][26];

int main() {
    int n;
    scanf("%d", &n);
    string s;
    getline(cin, s);
    getline(cin, s);
    s = "#" + s;
    for(int i = 2; i <= n; i++) {
        if(s[i] == s[i - 1]) match[i] = i - 1;
        else match[i] = pre[i - 1][s[i] - 'a'];
        if(match[i] > 1) {
            for(int j = 0; j < 26; j ++)
                pre[i][j] = pre[match[i] - 1][j];
            pre[i][s[match[i] - 1] - 'a'] = match[i] - 1;
        }
    }
    lli ans = 0;
    for(int i = 1; i <= n; i++) {
        if(match[i]) dp[i] = dp[match[i] - 1] + 1;
        ans += dp[i];
    }
    printf("%lld", ans);
    return 0;
}
```
