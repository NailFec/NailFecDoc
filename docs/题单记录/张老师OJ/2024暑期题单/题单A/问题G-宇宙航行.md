# 问题 G: 宇宙航行

传送门：[ZWGOJ](http://81.68.64.169/problem.php?cid=1009&pid=6) (1)
{ .annotate }

1.  题目来源：2024NOC-童创AI 高中组 复赛 T7

## 题目描述

### 原题面

小程作为航天局的一名侦测员，一直非常细心的工作。

某一天探测器接收到一条神秘的宇宙信号，分析了很久都无法破译，航天局派遣小程前往探索。

小程驾驶飞船依次通过若干黑洞完成空间跳跃，但黑洞与黑洞之间可能存在一定距离，这段距离需要飞船航行。飞船在每次进入黑洞前，小程都会把这段航行的速度与航行时间发送给你，请你编写程序依次计算每段的距离。

### 输入要求

第一行一个整数 $n\:(1\le n \le 10)$，表示有几段距离。

接下来有 $2n$ 行，每行一个非负整数。（$1\le \text{该整数的数位} \le 1000$）

每两行表示一组数据，依次表示速度（$\text{m/s}$）和时间（$\text{s}$）。

### 输出要求

输出 $n$ 行，每行一个整数，表示一段距离。

### 样例

<div class="grid" markdown>

```text
4
0
0
12120
111
10020546
180
1280
5000
```

```text
0
1345320
1803698280
6400000
```

</div>

## 解法

题意省流：给出 $T\:(1\le T\le 10)$ 组数据，使用高精度算法求出两个数的乘积。

### 高精度乘法

```cpp
#include <bits/stdc++.h>
using namespace std;
//const int MAXl=10;
const int MAXl=1000+9;
int a[MAXl+9], b[MAXl+9], c[2*MAXl+9];

int main(){
	int n;
	scanf("%d",&n);
	string stra,strb;
	for(int iout=1;iout<=n;iout++){
		cin >> stra >> strb;
		if(stra=="0"||strb=="0"){
			printf("0\n");
			continue;
		}
		int lena=stra.length(), lenb=strb.length();
		for(int i=1;i<=lena+lenb+9;i++)
			c[i]=0;
		for(int i=1;i<=lena;i++) a[i]=stra[lena-i]-'0';
		for(int i=1;i<=lenb;i++) b[i]=strb[lenb-i]-'0';
		for(int i=1;i<=lenb;i++)
			for(int j=1;j<=lena;j++)
				c[i+j-1]+=a[j]*b[i];
		for(int i=1;i<=lena+lenb;i++)
			if(c[i]>9){
				c[i+1]+=c[i]/10;
				c[i]%=10;
			}
		int endc=lena+lenb;
		while(c[endc]==0&&endc>1)
			endc--;
		for(int i=endc;i>=1;i--)
			printf("%d",c[i]);
		printf("\n");
	}
	return 0;
}
```

### 快速傅里叶变换

代码修改自 ©[Trilarflagz](https://www.luogu.com.cn/user/100708)。

```cpp
#include <bits/stdc++.h>
#define ld long double
#define mem0(x) memset((x),0,sizeof(x))
#define fill0(x) fill((x), (x)+MAXn, cp(0,0))
using namespace std;
typedef complex<ld> cp;
const int MAXn=1e4+9;
const ld PI = acos(-1);
cp a[MAXn+9], b[MAXn+9];
int rev[MAXn+9], ans[MAXn+9];
char s1[MAXn+9], s2[MAXn+9];

void fft(cp *a, int n, int inv) {    // a: 要操作的系数, n: 序列长度
    for(int i=0; i<n; i++) {
        if(i<rev[i])    // 防止同一个元素交换两次，回到它原来的位置
            swap(a[i], a[rev[i]]);
    }
    for(int h=1; h<n; h*=2) {    // h: 准备合并序列的长度的二分之一
        cp wn = exp(cp(0, inv*PI/h));    // 求单位根w_n^1
        for(int j=0; j<n; j+=h*2) {    // j: 合并到了哪位
            cp w(1, 0);
            for(int k=j; k<j+h; k++) {    // 左半
                cp x=a[k];
                cp y=w*a[k+h];
                a[k]=x+y;    // 蝴蝶变换
                a[k+h]=x-y;
                w*=wn;    // 求w_n^k
            }
        }
    }
    if(inv==-1)    // IFFT求倒数
        for(int i=0; i<n; i++)
            a[i]/=n;
    return;
}

int main() {
    int t;
    scanf("%d",&t);
    for(int iout=1; iout<=t; iout++){
        mem0(s1); mem0(s2); mem0(rev); mem0(ans);
        fill0(a); fill0(b);
        scanf("%s%s", s1, s2);
        int len1=strlen(s1), len2=strlen(s2);
        int n=max(len1, len2);
        // 存放在实部
        for(int i=0; i<len1; i++)
            a[i]=(ld)(s1[len1-i-1]-'0');
        for(int i=0; i<len2; i++)
            b[i]=(ld)(s2[len2-i-1]-'0');
        int k=1, s=2;    // k: 转化成二进制的位数
        while((1<<k)<2*n-1){
            k++;
            s<<=1;
        }
        // 初始化每个位置最终到达的位置（位反转优化）
        int len=1<<k;
        for(int i=0; i<len; i++)
            rev[i]=(rev[i>>1]>>1)|((i&1)<<(k-1));
        
        fft(a, s, 1);
        fft(b, s, 1);
        for(int i=0; i<s; i++)
            a[i]*=b[i];	
        fft(a, s, -1);
        
        // 进位保存答案的每一位
        for(int i=0; i<s; i++) {
            // 实部四舍五入（虚部应约为0）
            ans[i]+=(int)(a[i].real()+0.5);
            ans[i+1]+=ans[i]/10;
            ans[i]%=10;
        }
        while(!ans[s]&&s>-1) s--;
        if(s==-1)
            printf("0\n");
        else{
            for(int i=s; i>=0; i--)
                printf("%d", ans[i]);
            printf("\n");
        }
    }
    return 0;
}
```
