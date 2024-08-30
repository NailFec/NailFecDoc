# 问题 B: 挑战赛

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=1) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T2

## 题目描述

### 原题面

童程学院举办了“计算机常识”、“编程高手”两场挑战赛，分别有，个学生报名。报名“计算机常识”挑战赛的学生编号为 $x$，报名“编程高手"挑战赛的学生编号为 $y_m$，求有多少个学生既报名了“计算机常识"挑战赛，又报名了“编程高手”挑战赛。已知报名同一场比赛的学生编号不会重复。

### 输入要求

输入共三行；

第一行，两个整数 $n,\: m \: (1 \le n,m \le 1000)$，两个整数之间使用一个空格隔开；

第二行，$n$ 个正整数$x_1 \dots x_n \: (1 \le x_i \le 1000)$，表示报名“计算机常识”挑战赛的学生编号，编号之间使用一个空格隔开。

第三行，$m$ 个正整数$y_1 \dots y_n \: (1 \le y_i \le 1000)$，表示报名“编程高手”挑战赛的学生编号，编号之间使用一个空格隔开。

注意：输入学生编号为乱序。

### 输出要求

一个整数，表示既报名了“计算机常识”挑战赛，又报名了“编程高手”挑战赛的学生人数。

### 样例

<div class="grid" markdown>

```text
6 6
1 3 5 7 9 10
1 2 3 5 7 9
```

```text
5
```

</div>

## 解法

### 数组

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXn=1000+9;
bool datum[MAXn+9];

int main() {
	int n,m;
	scanf("%d%d",&n,&m);
	int ia;
	for(int i=1; i<=n; i++) {
		scanf("%d",&ia);
		datum[ia]=true;
	}

	int res=0;
	for(int j=1; j<=m; j++) {
		scanf("%d",&ia);
		if(datum[ia])
			res++;
	}
	printf("%d",res);
	return 0;
}
```

### bitset

```cpp
#include <bits/stdc++.h>
using namespace std;
bitset<1005> A, B, C;

int main() {
	int a, b, in;
	scanf("%d%d", &a, &b);
	for(int i = 1; i <= a; i ++) {
		scanf("%d", &in);
		A[in] = 1;
	}
	for(int i = 1; i <= b; i ++) {
		scanf("%d", &in);
		B[in] = 1;
	}
	C = A & B;
	printf("%d", C.count());
	return 0;
}
```
