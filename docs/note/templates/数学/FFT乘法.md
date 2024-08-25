# FFT 乘法

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
