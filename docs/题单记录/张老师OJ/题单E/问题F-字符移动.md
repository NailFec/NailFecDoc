# 问题 F: 字符移动

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1013&pid=5) | 字符串 Ad-Hoc

## 题目描述

### 原题面

在一个由$\texttt{“L}$”，$\texttt{“R}$”，$\texttt{“X}$”三个字符组成的字符串中进行移动操作。比如，一次移动操作是指将一个“$\texttt{XL}$”替换为一个“$\texttt{LX}$”，或者用一个“$\texttt{RX}$”替换为一个“$\texttt{XR}$”。现给定起始字符串 $S$ 和结束字符串 $T$，请编写代码，当且仅当存在一系列移动操作使得 $S$ 串可以转换成 $T$ 串时，返回“$\texttt{True}$”，否则返回“$\texttt{False}$”。

### 输入要求

第一行输入 $S$ 串（$1\le |S|\le 10000$）。

第二行输入 $T$ 串（$1\le |T|\le 10000$）。

### 输出要求

输出符合题意的值。

### 样例

<div class="grid" markdown>

```text
RXXLRXRXL
XRLXXRRLX
```

```text
True
```

</div>

<div class="grid" markdown>

```text
RXRXXLRXLX
XRXRLXRXXL
```

```text
False
```

</div>

## 解法

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s, t;
    getline(cin, s);
    getline(cin, t);
    int ls = s.length(), lt = t.length();
    if(ls != lt) return 0 * printf("False");
    s = "#" + s; t = "#" + t;
    int i = 1, j = 1;
    while(i <= ls && j <= lt) {
        while(i <= ls && s[i] == 'X') i ++;
        while(j <= lt && t[j] == 'X') j ++;
        if(i <= ls && j <= lt)
            if(s[i] != t[j]) return 0 * printf("False");
            else if(s[i] == 'L' && i < j) return 0 * printf("False");
            else if(s[i] == 'R' && i > j) return 0 * printf("False");
            else i ++, j ++;
    }
    while(i <= ls) if(s[i++] != 'X') return 0 * printf("False");
    while(j <= lt) if(t[j++] != 'X') return 0 * printf("False");
    printf("True");
    return 0;
}
```
