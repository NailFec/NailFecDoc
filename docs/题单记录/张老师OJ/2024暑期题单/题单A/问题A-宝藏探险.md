# 问题 A: 宝藏探险

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=0) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T1

## 题目描述

### 原题面

在一个遥远的程序员王国里，有一座神秘的宝藏之地，传说中只有解开了特定密码才能获得宝藏。这个密码并不是复杂的数字或符号组合，而是由元音字母构成的神秘字符串。传说中只有那些懂得统计元音字母数量的勇者才有机会找到宝藏。童童在前往宝藏地的路上，来到了一个神秘的岔道，路边出现了一块石碑，上面写着：“只有计算出这段神秘字符串中的元音字母数量，才能选择正确的方向前进。”

### 输入要求

一行，包含一个由小写字母组成的字符串 $s\:(1 \le |s| \le 1 \times 10^4)$，输入可以包含空格。

### 输出要求

一个整数，表示字符串中元音字母的数量。

### 样例

<div class="grid" markdown>

```text
hello world
```

```text
3
```

</div>

<div class="grid" markdown>

```text
please enter
```

```text
5
```

</div>

## 解法

注意：输入可能包含空格。可以通过循环 `cin` 字符串并多次计算，或使用 `getline()` 读取整行，或循环使用 `getchar()` 读取整行。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
char datum[5]= {'a','e','i','o','u'};

int main() {
	string instr;
	getline(cin,instr);
	lli res=0;
	for(ulli i=0; i<instr.length(); i++) {
		for(int j=0; j<=4; j++)
			if(instr[i]==datum[j])
				res++;
	}
	printf("%lld",res);
	return 0;
}
```

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
char datum[5]= {'a','e','i','o','u'};

int main() {
	lli res=0;
	char chr;
	while(scanf("%c",&chr)!=EOF)
		for(int i=0; i<=4; i++)
			if(datum[i]==chr)
				res++;
	printf("%lld",res);
	return 0;
}
```
