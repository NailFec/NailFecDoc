# 排列组合

## 排列

### 概念

从 $n$ 个不同的元素中取出 $m(m\le n)$ 个元素进行排序。

$$ P^m_n = \prod_{i=0}^{m-1}(n-i) = \frac{n!}{(n-m)!} $$

### 常用性质

- $P^m_m=m!$（0的阶乘等于1）
- $P^m_n=nP^{m-1}_{n-1}$
- $P^m_n=mP^{m-1}_{n-1}+P^m_{n-1}$

## 组合

### 概念

从 $n$ 个不同的元素中取出 $m(m\le n)$ 个元素，不考虑排序。

$$C^m_n=\frac{P^m_n}{P^m_m}=\frac{P^m_n}{m!}=\frac{\prod_{i=0}^{m-1}(n-i)}{m!}=\frac{n!}{m!(n-m)!}$$

### 常用性质

- $C^0_n=C^n_n=1$
- $C^m_n=C^{n-m}_n$
- $C^m_n=\frac{n}{m}C^{m-1}_{n-1}=\frac{k+1}{n-k}C^{k+1}_n=\frac{n}{n-k}C^k_{n-1}$
- $C^m_n=C^m_{n-1}+C^{m-1}_{n-1}$
- $C^k_nC^m_k=C^m_nC^{k-m}_{n-m}=C^{k-m}_nC^m_{n-k+m}$ , $m \le k \le n$
- $\sum_{i=0}^n{C^i_n}=2^n$
- $\sum_{i=0}^{n}{(-1)^{i} C^i_n} = 0$
- $\sum_{i=0}^{r}{C^i_r P^{r-i}_m P^i_n} = P^r_{m+n}$
- $C^0_n+C^2_n+C^4_n+\dots =C^1_n+C^3_n+C^5_n+\dots =2^{n-1}$
- $\sum_{i=0}^{n}{iC^i_n}=n \cdot 2^{n-1}$

二项式定理：$(a+b)^n=\sum_{i=0}^{n}C^i_na^{n-i}b^i=\sum_{i=0}^{n}C^i_na^ib^{n-i}$
