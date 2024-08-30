# 问题 F: 蛋糕装盒

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=5) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T6

## 题目描述

### 原题面

童童的妈妈做了 $x$ 个蛋糕，现有 $y$ 个蛋糕盒可以包装。一个容量为 $V$ 的蛋糕盒能装入体积不超过 $V$ 的蛋糕，一个蛋糕只能用一个蛋糕盒来装，一个蛋糕盒也只能用来装一个蛋糕。买一个蛋糕盒的价格由蛋糕盒的容量决定，容量为 $V$ 的蛋糕盒的价格为 $V$。

请你帮童童妈妈算一算，怎样花最少的钱把 $x$ 个蛋糕全部装盒。

### 输入要求

共三行。

第一行，两个正整数 $x, \: y\:(1\le X,y\le 1\times 10^5)$，分别表示需要包装的蛋糕个数和现有蛋糕盒的个数。

第二行，$x$ 个正整数，依次表示每个蛋糕的体积，数字之间使用一个空格隔开。

第三行，$y$ 个正整数，依次表示每个糕盒的容积，数字之间使用一个空格隔开。

（$1\le \text{蛋糕的体积}\le 1\times 10^4,\: 1\le \text{蛋糕盒的容积}\le 1\times 10^4$）

### 输出要求

一行，一个整数。如果能将所有的蛋糕装入已有的盒子，则输出用掉的蛋糕盒所花的最少钱数；如果无法将所有的蛋糕装入 $y$ 个盒子，则输出 $-1$。

### 样例

<div class="grid" markdown>

```text
3 4
10 1 5
1 8 11 5
```

```text
17
```

</div>

<div class="grid" markdown>

```text
3 4
5 10 15
1 5 8 11
```

```text
-1
```

</div>

## 解法

简单贪心。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const int MAXn=1e5+9;
int datum[MAXn+9];
multiset<int> boxs;

int main() {
	int x,y;
	scanf("%d %d",&x,&y);
	int maxb=-1,maxd=-1;
	for(int i=1; i<=x; i++) {
		scanf("%d",&datum[i]);
		maxd=max(maxd,datum[i]);
	}
	int ina;
	for(int i=1; i<=y; i++) {
		scanf("%d",&ina);
		maxb=max(maxb,ina);
		boxs.insert(ina);    // O(log(size))
	}
	if(maxd>maxb||x>y) {
		printf("-1");
		return 0;
	}
	
	sort(datum+1,datum+x+1);
	lli res=0;
	for(int i=1; i<=x; i++) {
		auto it=boxs.lower_bound(datum[i]);    // O(log(size))，不小于给定值的第1个元素
		if(it==boxs.end()){
			printf("-1");
			return 0;
		}
		res+=*it;
		boxs.erase(it);    // O(1)
	}
	printf("%lld",res);
	return 0;
}
```

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;

int main() {
	int x,y;
	scanf("%d %d",&x,&y);
	vector<int> vcake(x), vbox(y);
	for(int i=0;i<x;i++)
		scanf("%d",&vcake[i]);
	for(int i=0;i<y;i++)
		scanf("%d",&vbox[i]);
	
	sort(vcake.begin(),vcake.end());
	sort(vbox.begin(),vbox.end());
	if(x>y||vcake.back()>vbox.back()){
		printf("-1");
		return 0;
	}
	multiset<int> sbox(vbox.begin(),vbox.end());
	lli res=0;
	for(int i=0; i<x; i++) {
		auto it=sbox.lower_bound(vcake[i]);    // O(log(size))，不小于给定值的第1个元素
		if(it==sbox.end()){
			printf("-1");
			return 0;
		}
		res+=*it;
		sbox.erase(it);    // O(1)
	}
	
	printf("%lld",res);
	return 0;
}
```
