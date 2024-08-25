# 快速 IO

## 自带函数

```cpp
ios::sync_with_stdio(false);
cin.tie(0);
cout.tie(0);
```

略快于 `scanf()` 和 `printf()`，但注意不能混用。

## 普通方法

代码来自《深入浅出·进阶篇篇》 ©[洛谷](luogu.com.cn)。

!!! info

    此部分代码不处理读取到了文件结尾的情况。

### getchar

```cpp
int read() {
    int ret = 0, sgn = 0, ch = getchar();
    while(!isdigit(ch)) sgn |= ch == '-', ch = getchar();
    while(isdigit(ch)) ret = ret * 10 + ch - '0', ch = getchar();
    return sgn ? -ret : ret;
}
```

### getchar_unlocked

!!! warning

    仅适用于 Linux。

```cpp
int read() {
    int ret = 0, sgn = 0, ch = getchar_unlocked();
    while(!isdigit(ch)) sgn |= ch == '-', ch = getchar_unlocked();
    while(isdigit(ch)) ret = ret * 10 + ch - '0', ch = getchar_unlocked();
    return sgn ? -ret : ret;
}
```

### getc

```cpp
const int SIZE = 1 << 14;    // 16KB
char getc() {
    static char buf[SIZE], *begin = buf, *end = buf;
    if(begin == end) {
        begin = buf;
        end = buf + fread(buf, 1, SIZE, stdin);
    }
    return *begin++;
}
```

## 复杂方法

代码来自 ©[disangan233](https://www.luogu.com.cn/user/72679)。

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define db double
namespace fast_io {
    char buf[1<<12],*p1=buf,*p2=buf,sr[1<<23],z[23],nc;int C=-1,Z=0,Bi=0,ny;
    char gc() {return p1==p2&&(p2=(p1=buf)+fread(buf,1,1<<12,stdin),p1==p2)?EOF:*p1++;}
    ll read() {
        ll x=0;ny=1;while(nc=gc(),(nc<48||nc>57)&&nc!=EOF)if(nc==45)ny=-1;Bi=1;if(nc<0)return nc;
        x=nc-48;while(nc=gc(),47<nc&&nc<58&&nc!=EOF)x=(x<<3)+(x<<1)+(nc^48),Bi++;return x*ny;
    }
    db gf() {int a=read(),y=ny,b=(nc!='.')?0:read();return(b?a+(db)b/pow(10,Bi)*y:a);}
    int gs(char *s) {char c,*t=s;while(c=gc(),c<=32);*s++=c;while(c=gc(),c>32)*s++=c;return s-t;}
    void ot() {fwrite(sr,1,C+1,stdout);C=-1;}
    void flush() {if(C>1<<22) ot();}
    template <typename T>
    void write(T x,char t) {
        int y=0;if(x<0)y=1,x=-x;while(z[++Z]=x%10+48,x/=10);
        if(y)z[++Z]='-';while(sr[++C]=z[Z],--Z);sr[++C]=t;flush();
    }
    void write(char *s) {int l=strlen(s);for(int i=0;i<l;i++)sr[++C]=*s++;sr[++C]='\n';flush();}
}
using namespace fast_io;
```

### 读取长整型

```cpp
ll num = read();
```

### 读取浮点数

```cpp
db num = gf();
```

### 读取字符串

```cpp
char str[100];
int length = gs(str);
```

### 输出长整型或浮点数

```cpp
write(12345, '\n');
write(3.14159, ' ');
```

### 输出字符串

```cpp
char str[] = "Hello, World!";
write(str);
```

### 输出缓冲区内容

```cpp
return ot(), 0;
```

### 刷新缓冲区

```cpp
flush();
```
