# 问题 D: 超级弹珠

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=3) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T4

## 题目描述

### 原题面

MedalPluS 和他的小伙伴 NOIRP 发掘了一个骨灰级别的游戏——超级弹珠。
游戏的内容是：在一个 $n\times n$ 的矩阵里，有若干个敌人，你的弹珠可以摧毁敌人，但只能攻击你所在的行、列里的所有敌人，然后你就可以获得他们的分数之和，现在请你选择一个你的位置，使得能击杀的敌人最多，注意，你不能和敌人在一个地方。

### 输入要求

输入有两行，第一行一个正整数，接下来 $n$ 行，每行 $n$ 列，如果有敌人则为一个正整数，否则为 $0$。

### 输出要求

输出共一行，最多分数，如果连你的容身之地都没有，请输出“$\texttt{Fail}$”。

### 样例

<div class="grid" markdown>

```text
4
1 1 1 0
1 1 1 1
1 1 1 1
0 1 1 1
```

```text
6
```

</div>

## 解法

预处理每一行和每一列的 $1$ 的数量，每次查询时间复杂度 $O(1)$。

将一行和一列之和相加，需要注意交点被加了两次。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn=1000+9;
int datum[MAXn+9][MAXn+9];
int L[MAXn+9],H[MAXn+9];

int main() {
	int n;
	scanf("%d",&n);
	bool fail=true;
	for(int i=1; i<=n; i++) {
		for(int j=1; j<=n; j++) {
			scanf("%d",&datum[i][j]);
			if(datum[i][j]==0) fail=false;
			H[i]+=datum[i][j];
			L[j]+=datum[i][j];
		}
	}

	if(fail) {
		printf("Fail");
		return 0;
	}
	int res=0;
	for(int i=1; i<=n; i++)
		for(int j=1; j<=n; j++) {
			if(!datum[i][j]) {
				res=max(res,H[i]+L[j]);
			}
		}
	printf("%d",res);
	return 0;
}
```
