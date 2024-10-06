# 问题G - 集训题单

题目来源：[第四届上海市青少年算法竞赛（小学组）T5/5](https://www.iai.sh.cn/problem/801)

## 题目描述

### 原题面

小爱老师正在准备本次信息学集训的选题，为此他已经准备了 $n$ 道备选试题，每题都有一个难度值，其中第 $i$ 道题的难度值为 $a_i$。

由于集训时长的限制，小爱准备从这些备选试题中选出 $m$ 道试题组成正式的集训题单。为了保证集训的质量及难度，选出的 $m$ 道试题中需保证至少有 $k$ 道试题的难度不低于给定的难度值 $X$。

请你帮助小爱计算一下，一共有多少种不同的选题方式？由于答案可能很大，请输出最终方案数 $\% 998244353$ 即可。  
（注意：选出相同的试题但前后顺序不同，均认为是同一种选法。）

- 对于 $50\%$ 的数据，$1 \leq n \leq 20$；
- 对于 $100\%$ 的数据，$1 \leq n \leq 10^3$ ，$1 \leq k \leq m \leq n$ ，$1 \leq a_i , X \leq 10^9$。

### 输入要求

输入共三行：  
第一行，两个正整数 $n,m$。  
第二行，$n$ 个正整数，分别表示 $a_1,a_2,...,a_n$。  
第三行，两个正整数 $k,X$。

### 输出要求

输出满足条件的方案数对 $998244353$ 取模后的结果。

### 样例

<div class="grid" markdown>

```text
3 2
10 20 30
1 20
```

```text
3
```

</div>

样例 1 解释：可以选 {10,20},{10,30},{20,30} 共 3 种选法。

<div class="grid" markdown>

```text
4 2
5 10 15 20
1 12
```

```text
5
```

</div>

样例 1 解释：可以选 {5,15},{5,20},{10,15},{10,20},{15,20} 共 5 种选法。

## 解法

通过排列组合计算方案数。其中组合数的计算有多种方法，各有利弊。

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const lli mod = 998244353;
const int MAXn = 1e3 + 9;
lli d[MAXn], C[MAXn][MAXn], fac[MAXn];
int can, cant;

// C[m][n] = C[m][n - 1] + C[m - 1][n - 1]
// 时间：O(n^2)；空间：O(n^2)

lli calC(int m, int n) {
	if(m < 0 || n < 0 || m > n) return 0;
	if(C[m][n]) return C[m][n];
	if(m == 0 || m == n) return C[m][n] = 1;
	return C[m][n] = (calC(m, n - 1) + calC(m - 1, n - 1)) % mod;
}

int main() {
	int n, m;
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= n; i ++)
		scanf("%lld", &d[i]);
	int k; lli x;
	scanf("%d%lld", &k, &x);
	for(int i = 1; i <= n; i ++)
		d[i] >= x ? can ++ : cant ++;
	lli ans = 0;
	for(int i = k, j = m - k; i <= min(m, can) && j >= 0; i ++, j --)
		(ans += calC(i, can) * calC(j, cant) % mod) %= mod;
	printf("%lld", ans);
	return 0;
}
```

```cpp
#include <bits/stdc++.h>
#define lli long long int
using namespace std;
const lli mod = 998244353;
const int MAXn = 1e3 + 9;
lli d[MAXn], fac[MAXn], ifac[MAXn];
int can, cant;

// C[m][n] = n! / m! / (n - m)!
// 时间：O(n)；空间：O(n)

lli modExp(lli base, lli exp, lli mod) {
	lli res = 1;
	while (exp > 0) {
		if (exp % 2 == 1) (res *= base) %= mod;
		(base *= base) %= mod;
		exp /= 2;
	}
	return res;
}

void calfac(int x) {
	fac[0] = fac[1] = 1;
	for (int i = 2; i <= x; i++)
		fac[i] = fac[i - 1] * i % mod;
	ifac[x] = modExp(fac[x], mod - 2, mod);
	for (int i = x - 1; i >= 0; i--)
		ifac[i] = ifac[i + 1] * (i + 1) % mod;
}

lli C(int n, int k) {
	if (k > n || k < 0) return 0;
	return fac[n] * ifac[k] % mod * ifac[n - k] % mod;
}

int main() {
	int n, m;
	scanf("%d%d", &n, &m);
	for (int i = 1; i <= n; i ++)
		scanf("%lld", &d[i]);
	int k; lli x;
	scanf("%d%lld", &k, &x);
	for (int i = 1; i <= n; i ++)
		d[i] >= x ? can ++ : cant ++;
	calfac(max(can, cant));
	lli ans = 0;
	for (int i = k; i <= min(m, can); i ++) {
		int j = m - i;
		if (j >= 0 && j <= cant)
			ans = (ans + C(can, i) * C(cant, j) % mod) % mod;
	}
	printf("%lld", ans);
	return 0;
}
```

使用 **卢卡斯定理** 快速计算组合数。详见：

- OIwiki：[卢卡斯定理 - OIwiki](https://oi-wiki.org/math/number-theory/lucas/)
- 洛谷：[P3807 【模板】卢卡斯定理/Lucas 定理](https://www.luogu.com.cn/problem/P3807)

以下代码原码转载自 [iai.sh.cn上“张老师”的题解](https://www.iai.sh.cn/contribution/7687)。

```cpp title="©张老师 from iai.sh.cn"
#include<bits/stdc++.h>
#define P 998244353
typedef long long ll;
using namespace std;
int n,m,k,X;
int a[1005];
ll f[1005];
void init()
{
	f[0]=1;
	for(int i=1;i<=1005;i++) f[i]=f[i-1]*i%P;
}
ll pow_mod(ll a, ll x)
{
	ll ret=1;
	while(x)
	{
		if(x&1) ret=ret*a%P;
		a=a*a%P;
		x>>=1;
	}
	return ret;
}
ll Lucas(int n, int m)
{
	ll ans=1;
	while( n && m)
	{
		ll nn=n%P, mm=m%P;
		if(nn<mm) return 0;
		ans=ans*f[nn]*pow_mod(f[mm]*f[nn-mm]%P, P-2)%P;
		n/=P, m/=P;
	}
	return ans;
}
int main()
{
	ll ans=0;
	int kk=0;
	init(); 
	cin>>n>>m;
	for(int i=0;i<n;i++)
		cin>>a[i];
	cin>>k>>X;
	for(int i=0;i<n;i++)
		if(a[i]>=X) kk++;
	for(int i=k;i<=kk && i<=m;i++)
		ans=(ans+Lucas(kk,i)*Lucas(n-kk,m-i)%P)%P;
	cout<<ans<<endl;
	return 0;
}
```
