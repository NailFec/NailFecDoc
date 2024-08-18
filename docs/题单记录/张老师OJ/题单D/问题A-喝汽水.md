# 问题 A: 喝汽水

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1012&pid=0) (1)
{ .annotate }

1.  题目来源：第十九届全国青少年信息学奥林匹克联赛“西南位育杯”上海赛区小学组复赛 T2

<!--https://max.book118.com/html/2021/1128/5213312124004123.shtm-->

## 题目描述

### 原题面

求知小学的 $n$ 个学生到东方绿洲秋游，休息时喝汽水。在绿洲商店商品牌上写着：

1. 买 $1$ 瓶汽水定价 $1.40$ 元，喝 $1$ 瓶汽水（瓶不带走）$1$ 元；
2. 为节约资源，规定 $3$ 个空瓶可换回 $1$ 瓶汽水，或 $20$ 个空瓶可换回7瓶汽水；
3. 为方便顾客，可先借后还。例如借 $1$ 瓶汽水，还 $3$ 个空瓶；或借 $7$ 瓶汽水，还 $20$ 个空瓶。

问 $n$ 个学生每人喝 $1$ 瓶汽水，至少需多少元？

### 输入要求

输入正整数 $n$ 的值

### 输出要求

输出至少需付款多少元（四舍五入保留小数 $2$ 位）

### 样例

<div class="grid" markdown>

```text
11
```

```text
10.40
```

</div>

## 解法

解法和时间复杂度： (1)
{ .annotate }

1.  此处时间复杂度为该解法的**算法部分**的时间复杂度，不是严谨的**整题**的时间复杂度

<!---->
- 法1：动态规划 $O(n)$ , $O(m+n)$  
- 法2：贪心 $O(\frac{n}{20})$ , $O(\frac{mn}{20})$  
- 法3：完全贪心 $O(1)$ , $O(m)$  

第一个时间复杂度为本题时间复杂度，第二个为如果有 $m$ 次询问的时间复杂度。

### 题目分析

相当于以下 $3$ 种方案：

- 20人，18.2元
- 3人，2.8元
- 1人，1.0元

### 法1：动态规划

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;
const int MAXn = 1e7 + 9;
int d[MAXn];

signed main() {
    int n;
    scanf("%lld", &n);
    fill(d, d + n + 1, LLONG_MAX);
    d[0] = 0;
    for(int i = 1; i <= n; i ++) {
        if(i -  1 >= 0) d[i] = min(d[i], d[i -  1] +  10);
        if(i -  3 >= 0) d[i] = min(d[i], d[i -  3] +  28);
        if(i - 20 >= 0) d[i] = min(d[i], d[i - 20] + 182);
    }
    printf("%lld.%lld0\n", d[n]/10, d[n]%10);
    return 0;
}
```

### 法2：贪心

from: 张老师

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, t, x, y;
    double p, m;
    cin >> n;
    p = 1.4;
    m = 2 * n;
    for(x = 0; x <= n/20; x ++) {
        t = n - 20 * x;
        y = t / 3;
        t = n - 20 * x - 3 * y;
        if((13 * x + 2 * y) * p + t < m)
            m = (13 * x + 2 * y) * p + t;
    }
    cout << fixed << setprecision(2) << m << endl;
    return 0;
}
```

### 法3：完全贪心

```cpp
#include <bits/stdc++.h>
#define int long long int
using namespace std;

signed main() {
    int n;
    scanf("%lld", &n);
    int ans = 0;
    ans += (n / 20) * 182; n %= 20;
    ans += (n /  3) *  28; n %=  3;
    ans += n * 10;
    printf("%lld.%lld0\n", ans/10, ans%10);
    return 0;
}
```
