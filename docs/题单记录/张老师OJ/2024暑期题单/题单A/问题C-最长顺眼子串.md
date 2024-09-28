# 问题 C: 最长顺眼串

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=2) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T3

## 题目描述

### 原题面

童童认为像“$\texttt{abcd}$”，“$\texttt{adfz}$”，这样按 ASCII 编码由小到大排列，且连续的字符串称为顺眼串，像“$\texttt{aaa}$”，“$\texttt{ccbbbaaa}$”，“$\texttt{addba}$”，这种后面字符比前面字符编码小或者相等的都不是顺眼串，现在要求输入一个字符串，找出其中的最长顺眼串，输出它的长度。

### 输入要求

一行，一个仅含有小写字母的字符串，长度不超过 $1000$。

### 输出要求

一行，一个整数，表示最长顺眼串的长度。

### 样例

<div class="grid" markdown>

```text
abcdefghhhhhahcde
```

```text
8
```

</div>

<div class="grid" markdown>

```text
cdeabcfghjkmnxyzxxxccc
```

```text
13
```

</div>

## 解法

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	string str;
	getline(cin,str);
	int len=str.length();
	int res=1;
	int i=0;
	while(i<len){
		int j;
		for(j=1;i+j<len;j++){
			if(str[i+j-1]>=str[i+j]) break;
			res=max(res,j+1);
		}
		i+=j;
	}
	
	printf("%d",res);
	return 0;
}
```
