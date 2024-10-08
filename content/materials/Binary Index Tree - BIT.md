---
title: Binary Index Tree - BIT
draft: false
tags:
  - ds/binary_index_tree
  - "#material"
---
 


## 树状数组学习笔记

[toc]





## 基本原理
每一个区间你只要知道了一半，就能知道另一半，节省储存信息的结点

通过利用二进制的进位特点，只要每个数加上其二进制下最低位的 $1$ ($+\operatorname{lowbit}(x)$)，就能得到其贡献区间和的对象，减去 $\operatorname{lowbit}(x)$ 就能得到回答时爱要加上的区间的下标。

支持单点修改和区间查询。

![[Pasted image 20240927172010.png]]

## 基础操作

单点修改
```cpp
void BIT_Add(int i,int x)
{
	for(;i <= n;i += i&(-i)) BIT[i] += x;
	return ;
}
```

区间 $[1,x]$ 查询
```cpp
int BIT_Sum(int i)
{
	int res = 0;
	for(;i;i -= i&(-i)) res += BIT[i];
	return res;
}
```


---
## 一些题目


---
## 区间修改与区间询问


### 维护二阶前缀和

[A Simple Problem with Integers](http://poj.org/problem?id=3468)

本可以用线段树做的题目我们也可以尝试魔改一下树状数组去解决。比如很普通的区间加和区间和询问，在最原版的树状数组中是不支持的，那么我们考虑如何让树状数组支持区间加。

先在考虑对区间 $[l,r]$ 加上 $x$ ，设前缀和数组 $s(i) = \sum_{j = 1}^i a_j$，那么区间加后会有一下变化：

- $i < l\;,\; s'(i) = s(i)$
- $l \leqslant i \leqslant r\;,\;s'(i) = s(i) + (i-l+1)x = xi +s(i)-lx+x$
- $i > r\;,\;s'(i) = s(i) + (r-l+1)x$

我们可以发现，新的前缀和数组与 $i$ 和一些常数有关系，于是我们可以将前缀和数组表示成以下形式：
$$
s(i) = ai+b=(\sum x_j)\cdot i + \sum y_j
$$
考虑差分，然后用树状数组 $\operatorname{BITa}$ 和 $\operatorname{BITb}$ 维护上式中的 $a$ 和 $b$ ，那么每做一次区间加操作，就有：
- $\operatorname{BITb}$ 的 $l$ 位置上加上 $-x(l-1)$
- $\operatorname{BITa}$ 的 $l$ 位置上加上 $x$
- $\operatorname{BITb}$ 的 $r+1$ 位置上加上 $xr$
- $\operatorname{BITa}$ 的 $r+1$ 位置上加上 $-x$

更一般地，如果操作得到的结果可以用 $i$ 的 $n$ 次多项式来表示，那么就可以使用 $n + 1$ 个树状数组来维护每个项的系数。

核心代码：

```cpp
if(ch == 'C')
{
	scanf("%lld%lld%lld",&l,&r,&x);
	Add(0, l, -x * (l - 1));
	Add(1, l, x);
	Add(0, r + 1, x * r);
	Add(1,r + 1, -x);
}
else
{
	scanf("%lld%lld",&l,&r);
	ans = 0;
	ans += Sum(0, r) + Sum(1, r) * r;
	ans -= Sum(0, l - 1) + Sum(1, l - 1) * (l - 1);
	printf("%lld\n",ans);
}
```



### 维护三阶前缀和

#### P4062 [Code+#1]Yazid 的新生舞会 ※

[P4062 [Code+#1]Yazid 的新生舞会](https://www.luogu.com.cn/problem/P4062)

我们希望统计“新生舞会”子区间，先要发现它的性质。

从简单的情况入手，例如考虑只有 0 和 1 的序列，如果统计 1 的“新生舞会”子区间，那么条件是 $cnt(l,r)>\lfloor\dfrac{r-l+1}{2}\rfloor$ . 自然地，我们想到维护一个前缀和快速求得 $cnt(l,r)$ ，记 $S_i$ 表示 $[1,i]$ 中 1 的个数，那么上式改写为：
$$
\begin{aligned}
S_r-S_{l-1}&>\dfrac{r-l+1}{2}\\[2ex]
2S_r-r &> 2S_{l-1}-(l-1)
\end{aligned}
$$
于是我们发现，只要统计满足以上偏序关系的偏序对的个数就能得到 1 的“新生舞会”子区间个数。

但是如果对每一个数都做一遍统计，不难发现其复杂度是 $O(n^2)$ 的，我们需要改进复杂度。

继续从简单情况入手，我们观察 $2S_i-i$ 的性质：

|   $i$    |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| :------: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  $a_i$   |  0   |  0   |  1   |  0   |  1   |  1   |  0   |  0   |
| $2S_i-i$ |  -1  |  -2  |  -1  |  -2  |  -1  |  0   |  -1  |  -2  |

我们可以发现一个重要性质：**$2S_i-i$ 总是以 $a_i=1$ 的位置为开头或者结尾组成一段公差为 $-1$ 的等差序列。**

可以发现 $[1,2]$，$[3,4]$，$[5,5]$，$[6,8]$ 的 $2S_i-i$ 都组成了等差序列，那么如何利用这个性质？首先一个显然的结论是等差序列内部不会对答案产生贡献，因为内部总是递减，一定不满足之前推出的偏序关系，所以我们如果考虑插入等差序列（每一个数都作为一段等差序列的开头或者结尾 $O(n)$） 并查询等差序列与前面的等差序列产生的贡献（期望 $O(n\log n)$），就可能解决此题。

现在做一个小推广，假设统计以下序列中，5 的“新生舞会”子区间

|  $i$  |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| $a_i$ |  1   |  2   |  5   |  3   |  5   |  5   |  4   |  2   |

显然将 5 看作 1，其它数看作 0 就是上一个表格的情况。插入一段由 $a_i$ 中 $[3,4]$ 对应的 $2S_i-i$ 组成的等差序列 等价于给 $2S_i-i$ 的值域区间 $[-1,-2]$ 加上权值 1 ，也就是区间加。查询等差序列 $[3,4]$ 与前面产生的贡献就是：求等差序列中，每一个数与前面满足偏序关系的数的个数之和。例如这个例子，就是统计前面与 $-1$ 满足偏序关系的 $2S_i-i$ 的个数加上前面与 $-2$ 满足偏序关系的 $2S_i-i$ 的个数。

那么用简洁的表达式表达，就是：

记 $v(x)$ 为 $2S_i-i = x$ 的 $i$ 个数，也就是 $x$ 的权值；令 $p=2S_l-l$ 为等差序列开头的那个数 ,令 $q=2S_r-r$ 为等差序列结尾的那个数，那么由 $[l,r]$ 中的 $2S_i-i$ 组成的等差序列与前面的等差序列产生的贡献就是：
$$
\sum_{q\leqslant k\leqslant p}\sum_{t < k} v(t)
$$
为了提高运算速度，考虑用前缀和优化，并用树状数组维护前缀和。记 $g(x)=\sum_{k\leqslant x} v(k)$ ，是 $2S_i-i\leqslant x$ 的 $i$ 的个数，也就是 $v(x)$ 的前缀和；记 $h(x) = \sum_{k\leqslant x} g(k)$ ，表示对于所有 $2S_i-i\leqslant x$ 的 $2S_i-i$ ，满足 $2S_j-j\leqslant 2S_i-i$ 的 $j$ 的个数之和，也就是 $g(x)$ 的前缀和；用上面的例子，我们知道查询 $[3,4]$ 组成的等差序列与前面的贡献就是 $h((-1)-1)-h((-2)-2)$ 。普遍意义下，查询开头为 $p=2S_l-l$ ，结尾为 $q=2S_r-r$ 的等差序列与前面的贡献是 $h(p-1)-h(q-2)$ ，这样就能求得答案。

于是我们可以制作下表：

| $x=S_i-i$ |  -3  |  -2  |  -1  |  0   |  1   |
| :-------: | :--: | :--: | :--: | :--: | :--: |
|  $v(x)$   |  0   |  1   |  1   |  0   |  0   |
|  $g(x)$   |  0   |  1   |  2   |  2   |  2   |
|  $h(x)$   |  0   |  1   |  3   |  5   |  7   |

然后考虑加入一段等差序列的问题：上面也已经说了，插入一段以 $[l,r]$ 中的 $ 2S_i-i$ 组成的等差序列等价于在 $2S_i-i$ 的权值域 $v$ 中对区间 $[2S_r-r,2S_l-l]$ 区间加 1.于是我们考虑对 $v$ 区间加 1 后，对 $h$ 有什么影响就好了，这里的思考思路参考《挑战程序设计竞赛》中处理树状数组区间加和区间查询的思路。

现在我们将目光转向 $v,g,h$ 三个函数，以下的 $[l,r]$ 都是指三个函数的定义域 $x = S_i-i$ 上的区间（前面的是指 $a_i$）

考虑令满足 $l\leqslant x\leqslant r$ 的 $v(x)$ 全部加 1.

记操作前的 $x$ 对应的 $g$ 函数为 $g(x)$ ，操作后为 $g'(x)$ ，$h$ 函数同理。分类讨论：

- $x<l$ ，则 $g(x)$ 和 $h(x)$ 不受影响，即 $g'(x)=g(x),h'(x)=h(x)$

- $l\leqslant x\leqslant r$ ，则
  $$
  \begin{aligned}
  g'(x) &= g(x)+x-l+1\\[2ex]
  h'(x) &= h(x)+\sum_{k = l}^{x} (k-l+1)\\[1ex]
  &=h(x)+\dfrac{(x-l+1)(x-l+2)}{2}\\[1ex]
  &= h(x) + \dfrac{1}{2}\left(x^2+(3-2l)x+l^2-3l+2\right)
  \end{aligned}
  $$
  可以借用上面给出的表格与例子推一推。

- $r<x$ ，则
  $$
  \begin{aligned}
  g'(x) &= g(x) + r-l+1\\[2ex]
  h'(x)&=h(x) + \sum_{k=l}^r(k-l+1)+(x-r)(r-l+1)\\[1ex]
  &=h(x) + \dfrac{(r-l+2)(r-l+1)}{2}+(x-r)(r-l+1)\\[1ex]
  &= h(x) + \dfrac{1}{2}\left(2(r-l+1)x+(r-l+2)(r-l+1)-2r(r-l+1)\right)
  \end{aligned}
  $$

- 

然后我们发现，变化量与 $x^2$ ，$x$ 和常数项（$x^0$）有关，于是用三个树状数组 $\rm{bit0}$，$\rm{bit1}$，$\rm{bit2}$ 分别维护这三项，具体做法是：

对于 $[l,r]$ 区间加 1：

- 令 ${\rm{bit0}}(l)$ 加上 $l^2-3l+2$；令 ${\rm bit1}(l)$ 加上 $3-2l$ ；令 ${\rm bit2}(l)$ 加上 $1$
- 令 ${\rm bit0}(r + 1)$ 加上 $(r-l+2)(r-l+1)-2r(r-l+1)-(l^2-3l+2)$ ；令 ${\rm bit1(r + 1)}$ 加上 $2(r-l+1)-(3-2l)$ ；令 ${\rm bit2}(r + 1)$ 加上 $-1$

查询的时候，例如查 $h(x)$ 就是 
$$
h(x)=\dfrac{\operatorname{Query}({\rm bit0},x)+\operatorname{Query}({\rm bit1},x)\times x+\operatorname{Query}({\rm bit2},x)\times x^2}{2}
$$
这里乘上 $\dfrac{1}{2}$ 是因为我们刚刚修改的时候没有乘上 $\dfrac{1}{2}$ .

以上式子还要慢慢理解。这里再放出一张图，用于理解二阶前缀和的变化，也就是 $g(x)$ 的变化，至于 $h(x)$ 的变化，确实需要一定的数学功底与思维能力理解（这几条式子我也推了很久，推了 2h ，而且有几次推错了，甚至有几次多项式相乘展开都算错了，我数学太菜了...）

![[4062（1）.png]]

综上所述，我们就可以将记录一种数出现的所有位置，然后将原序列简化为仅包含 0 和 1 的序列，维护一下 $h(x)$ 并统计答案，逐个击破即可。最后要注意的就是不要傻傻地将负数作为下标了，设置一个偏移量 $\Delta$ 就可以避免数组越界地情况。时间复杂度 $O(n\log n)$

```cpp
#include<iostream>
#include<cstdio>
#include<vector> 

using namespace std;
typedef long long ll;
const ll dlt = 500003;
ll ans,t[5][1000010];
int n,type,x;
vector<ll > a[500005];

int lowbit(int num)
{
	return num&(-num);
}

void bitadd(int ty,int i,ll val)
{
	for(;i <= n + dlt;i += lowbit(i)) t[ty][i] += val;
	return ;
}

ll bitq(int ty,int i)
{
	ll res = 0;
	for(;i;i -= lowbit(i)) res += t[ty][i];
	return res;
}

ll BIT_Query(ll num)
{
	ll res = 0;
	res = ((bitq(0,num) + bitq(1,num)*num + bitq(2,num)*num*num)/2);
	return res;
}

void BIT_Add(ll l,ll r,ll val)//维护三个树状数组
{
	ll tmp0 = l*l - (ll)3*l + (ll)2;
	ll tmp1 = (ll)3 - l - l;
	ll tmp2 = (ll)1;
	bitadd(0,l,tmp0*val);
	bitadd(1,l,tmp1*val);
	bitadd(2,l,tmp2*val);
	tmp0 = -tmp0 + (r - l + (ll)1)*(r - l + (ll)2) - (ll)2*r*(r - l + (ll)1) ;
	tmp1 = -tmp1 + (ll)2*(r - l + (ll)1);
	tmp2 = -tmp2;
	bitadd(0,r + 1,tmp0*val);
	bitadd(1,r + 1,tmp1*val);
	bitadd(2,r + 1,tmp2*val);
	return ;
}

void solve(ll num)
{
	ll cnt = 0;ll lt = 0;ll rt = 0;
	BIT_Add(dlt,dlt,1);\\dlt 是偏移量
	if(a[num][0] > 1) lt = -a[num][0] + 1 ,rt = -1,BIT_Add(lt + dlt,rt + dlt,1); 
	for(int i = 0;i < a[num].size() - 1;i ++)//插入，查询，统计答案
	{
		cnt ++;rt = cnt + cnt - a[num][i];lt = cnt + cnt - a[num][i + 1] + 1;
		ans += BIT_Query(rt - 1 + dlt) - BIT_Query(lt - 2 + dlt);
		BIT_Add(lt + dlt,rt + dlt,1);
	}
	cnt = 0;
	BIT_Add(dlt,dlt,-1);
	if(a[num][0] > 1) lt = - a[num][0] + 1,rt = -1,BIT_Add(lt + dlt,rt + dlt,-1);
	for(int i = 0;i < a[num].size() - 1;i ++)//清空树状数组
	{
		cnt ++;lt = cnt + cnt - a[num][i + 1] + 1;rt = cnt + cnt - a[num][i];
		BIT_Add(lt + dlt,rt + dlt,-1);
	}
	return ;
}

int main()
{
	scanf("%d%d",&n,&type);
	for(int i = 1;i <= n;i ++)
	{
		scanf("%d",&x);
		a[x].push_back(i);//记录位置
	}
	for(int i = 0;i <= n;i ++)//逐个击破
		if(!a[i].empty())
		{
			a[i].push_back(n + 1);
			solve(i);
		}
	printf("%lld",ans);
	return 0;
 } 
```





---

## 维护偏序
### P1908 逆序对
[P1908 逆序对](https://www.luogu.com.cn/problem/P1908)

维护满足以下二维偏序的 $(i,j)$ 对数
$$
\begin{cases}
i<j\\
a_i >a_j
\end{cases}
$$
原理是：如果 $a_i$ 本应出现在 $i'$ 的位置上，但是它提前出现了 $i<i'$ 那么一定有比它小的元素在它以后出现，产生逆序对。
由于在枚举时，第一维是顺序的，我们考虑如何维护第二维。

1. 首先计算 $a_i$ 本应处在的位置，我们可以按照 $a_i$ 升序给序列排序，然后直接得到每个 $a_i$ 的新位置 $b_i$ 。
2. 然后按第一维顺序枚举到 $i$ 查看在 $b_i$ 之前有多少元素没有填，这些没有填的元素必定在 $i$ 以后出现，所以产生逆序对，统计答案。

关键代码：
```cpp
for(int i = 1;i <= n;i ++)
{
	scanf("%lld",&a[i]);
	b[i].val = a[i];
	b[i].id = i;
}
sort(b + 1,b + 1 + n,cmp);
for(int i = 1;i <= n;i ++)
	a[b[i].id] = i;

for(int i = 1;i <= n;i ++)
{
	BIT_Add(a[i],1);
	ans += (i - BIT_Sum(a[i]));
}
```

---

### P1966 [NOIP2013 提高组] 火柴排队
[P1966 [NOIP2013 提高组] 火柴排队](https://www.luogu.com.cn/problem/P1966)

距离定义为 $\sum(a_i-b_i)^2$ ，如果同时考虑多个交换无法发现一些性质，一般来说，我们可以考察一次交换给距离带来什么影响。

假设我们交换 $a_i$ 和 $a_{i+1}$ ，那么：
$$
\begin{aligned}
\Delta &= -2a_{i+1}b_i-2a_ib_{i+1}+2a_ib_i+2a_{i+1}b_{i+1}\\
&=-2(a_i-a_{i+1})(b_i-b_{i+1})
\end{aligned}
$$
我们可以发现，当 $a_i-a_{i+1}$ 与 $b_i-b_{i+1}$ 同号时，$\Delta<0$ ，距离减小，由此可得当所有火柴满足 $\forall a_i<a_j ,b_i<b_j$ 时火柴间的距离最小。这意味着，在原序列中排名相同的 $a,b$ 应该配对，就是说 $a_i$ 在 $a$ 中的大小排第 $2$ ，那么它应该与在 $b$ 中大小排第 $2$ 的 $b_i$ 配对才能使火柴间的距离最小。  
这时我们可以发现，题目希望我们维护二维偏序：
$$
\begin{cases}
\operatorname{rank}_a(a_i)<\operatorname{rank}_a(a_j)\\
\operatorname{rank}_b(b_i)<\operatorname{rank}_b(b_j)
\end{cases}
$$
通过研究几个例子可以发现，使距离最小的交换次数就是序列中关于以上二维偏序的逆序对的个数。

![[Pasted image 20240927172054.png]]

先将 $a$ 和 $b$ 排序，然后给每个数一个自己序列内的排名，然后以任意一维作为关键字排序，然后就是做逆序对的操作。

核心代码：

```cpp
sort(tmp + 1,tmp + 1 + n,cmp1);
for(int i = 1;i <= n;i ++) mapb[tmp[i].b] = i;//给一个排名，顺便离散化
sort(tmp + 1,tmp + 1 + n,cmp2);
for(int i = 1;i <= n;i ++) mapa[tmp[i].a] = i;
for(int i = 1;i <= n;i ++)
{
	num[mapa[tmp2[i].a]].a = i;//将排名应用到原数组上
	num[mapb[tmp2[i].b]].b = i;
}
sort(num + 1,num + 1 + n,cmp2);//任一维作为关键字排序，这里选择 a
for(int i = 1;i <= n;i ++)
{
	BIT_Add(num[i].b);//求逆序对的操作
	ans = (ans%modn + (i - BIT_Sum(num[i].b))%modn)%modn;
}
```


---

### P3605 [USACO17JAN]Promotion Counting P

[P3605 [USACO17JAN]Promotion Counting P](https://www.luogu.com.cn/problem/P3605)

可以轻易发现，仍然是求逆序对的个数，但是这次不在序列上，而是在树上。考虑将树转化为序列，不必使用树链剖分，只要用后序遍历然后给每个结点标上一个优先值，能够表示结点是那些点的父亲即可。

![[Pasted image 20240927172111.png]]

核心代码：

```cpp
void TreeSort(int u)//后序遍历
{
	if(a[u].first_son == 0)
	{
		cnt ++;
		a[u].rank = cnt;
		a[u].l = a[u].r = cnt;
		a[u].leaf = 1;
		return ;
	}
	a[u].l = inf;a[u].r = 0;
	for(int i = a[u].first_son;i;i = a[i].nxt_bro)//儿子兄弟表示法
	{
		TreeSort(i);
		a[u].l = min(a[u].l,a[i].l);//记录对应区间
		a[u].r = max(a[u].r,max(a[i].r,a[i].rank));
	}
	cnt ++;a[u].rank = cnt;
	return ;
}

```

```cpp
TreeSort(1);
sort(a + 1, a + 1 + n, cmp);//求逆序对常规操作
for(int i = 1;i <= n;i ++)
{
	BIT_Add(a[i].rank);
	if(a[i].leaf == 1) continue;
	a[i].ans = a[i].r - a[i].l + 1 - (BIT_Sum(a[i].r) - BIT_Sum(a[i].l - 1));//区间查询逆序对个数
}
sort(a + 1,a + 1 + n, cmp2);
for(int i = 1;i <= n;i ++)
	printf("%d\n",a[i].ans);
```

---

## 离线解题技巧

### P2184 贪婪大陆（计数，差分）

[P2184 贪婪大陆](https://www.luogu.com.cn/problem/P2184)

对于统计种类数，常常可以考虑构造差分数组，然后用树状数组高效解决。

考虑查询一个区间时，哪些埋入地雷的区间对答案有贡献。不难发现：

1. 左端点在查询的区间中，而右端点不在
2. 右端点在查询的区间中，而左端点不在
3. 左右端点都在查询的区间中

可以顺便想一想哪些区间对答案没有贡献。可以发现：

1. 左右端点在区间的左边
2. 左右端点在区间的右边

结合以上讨论，要想到查询区间 $[l,r]$ 中地雷的种类，那么只要知道 $[1,r]$ 中左端点的个数和 $[1,l-1]$ 中右端点的个数即可，因为统计出 $[1,l-1]$ 中的右端点，就可以与已经统计出的左端点相消，从而将 $[1,l-1]$ 中的地雷区间的贡献消除，得到正确的贡献。

一些启示：

1. 本题只关注种类数量，并不关心区间内具体有哪些地雷，于是可以考虑构造并利用差分数组解决这类统计问题。
2. 运用差分的时候就要想前缀和，设计如何计算正确答案。
3. 根据对象对答案贡献的性质设计差分数组，如这里地雷种类的贡献是区间性的，那么要考虑哪里 +1 哪里 -1

```cpp
int main()
{
	scanf("%d%d",&n,&m);
	for(int i = 1;i <= m;i ++)
	{
		scanf("%d%d%d",&opt,&x,&y);
		if(opt == 1)
		{
			BIT_Add(x,0,1);
			BIT_Add(y,1,1);
		}
		else
			printf("%d\n",BIT_Query(y,0) - BIT_Query(x - 1,1));
	}
	
	return 0;
}
```



### P1972 [SDOI2009]HH的项链（离线，计数，差分）

[P1972 [SDOI2009]HH的项链](https://www.luogu.com.cn/problem/P1972)

可以考虑树状数组离线回答询问。有一种巧妙的差分数组构造。考虑以下情况：

![[Pasted image 20240927172124.png]]

从当前的指针开始，第一个数字 $i$ 在树状数组对应位置中 $+1$ ，以后的数字 $i$ 都不打标记，这样只有第一个 $i$ 会对答案贡献 $1$ ，表示这个区间内有一种数字 $i$。指针更新的时候，记得去除以前的标记，给最新的数字打上标记。

将询问按左端点降序排列后，从序列最右往左更新每个数的标记，更新到询问左端点时就回答询问，然后指针再向左移动，继续更新标记与回答询问，时间复杂度 $O(n)$。注意要离散化一下。

核心代码：

```cpp
sort(q + 1,q + 1 + m,cmp);
for(int i = 1,j = n;i <= m;i ++)
{
	for(;j >= q[i].l;j --)//打标记
	{
		if(pre[a[j]] == 0)
		{
			pre[a[j]] = j;
			BIT_Add(j,1);
		}
		else
		{
			BIT_Add(pre[a[j]],-1);
			pre[a[j]] = j;
			BIT_Add(j,1);
		}
	}
	q[i].ans = BIT_Sum(q[i].r) - BIT_Sum(q[i].l - 1);//区间查询
}
sort(q + 1,q + 1 + m,cmp2);
for(int i = 1;i <= m;i ++)
	printf("%d\n",q[i].ans);
```

本题也可以用可持久化线段树或者莫队在线解决，有待学习。

与此类似的题目 [P4113 [HEOI2012]采花](https://www.luogu.com.cn/problem/P4113)
### 小结

对于一些支持单点修改与区间询问，但不是维护区间和的问题，我们可以考虑将问题转化为权值统计问题，构造一种赋权值的方法，然后将询问离线下来，排序后用树状数组维护解决。

---
## 维护不同的信息

### P3586 [POI2015]LOG
[P3586 [POI2015]LOG](https://www.luogu.com.cn/problem/P3586)
修改操作是单点修改，可以考虑使用树状数组，也可以用线段树。

难点在于每次选出 $c$ 个正数，并将它们都减去 $1$，询问能否进行 $s$ 次操作。
对于 $\geqslant s$ 的数来说，每次都选它们一定可行，而且这些数无论有多大，也只能操作 $s$ 次，它们不能替别的数减 $1$ 。

对于剩下的 $<s$ 的数，如果设为 $a_i$ ， 那么对于所有 $i$ ，只能贡献 $a_i$ 次操作，贡献完后，剩下的操作要由别的数贡献，所以假设大于等于 $s$ 的数有 $k_1$ 个，那么要求 $\sum a_i\geqslant (c-k_1)\cdot s$ 。
求 $\geqslant s$ 的数的个数 $k_1$ ，然后判断剩余数的总和是否 $\geqslant (c-k_1)\cdot s$ 即可。

可以搞一个类似权值线段树的东西，每次修改就原权值减 $1$ ，新权值加 $1$， 这样就可以迅速得知 $k_1$ 。再开一个树状数组，记一下权值的某个区间内的总和，这样可以快速求得剩余数的总和。要离散化一下，然后解决本题。

---
## 二维树状数组
### P4054 [JSOI2009]计数问题

[P4054 [JSOI2009]计数问题](https://www.luogu.com.cn/problem/P4054)

树状数组的本质其实是前缀和，只不过树状数组支持单点修改。这一道题求某一特殊权值在某矩阵内出现的次数，可以想到维护每一种权值的出现次数，然后我们可以模仿二维前缀和，希望维护一个 $f(x,i,j)$ 表示 $x$ 在前 $i$ 行前 $j$ 列中出现的次数。我们可以建立维护横行的树状数组，然后在纵列上再开一个树状数组维护这些横行树状数组，这或许有些抽象，可以想象有一个树状数组 $b(i)$ 要维护前 $i$ 列中所有前 $j$ 行的前缀和信息，$1\leqslant j \leqslant n$，显然我们要在这个 $b(i)$ 中再开一个树状数组才能完整维护这些信息，所以 $b(i)$ 就扩展为了 $b(i,j)$ ，然后维护的时候，就先横行这一维正常，然后贡献给纵列这一维，然后纵列正常维护即可。

核心代码：

```cpp
void BIT_Add(int x,int y,int w,int val)
{
	for(int i = x;i <= n;i += lowbit(i))
		for(int j = y;j <= m;j += lowbit(j))
			bit[w][i][j] += val;
	return ;
}

int BIT_Sum(int x,int y,int w)
{
	if(x == 0 || y == 0) return 0;
	int res  = 0;
	for(int i = x;i > 0;i -= lowbit(i))
		for(int j = y;j > 0;j -= lowbit(j))
			res += bit[w][i][j];
	return res;
}

int main()
{
	...
	ans = BIT_Sum(xx,yy,z) - BIT_Sum(x - 1,yy, z) - BIT_Sum(xx,y - 1, z) + BIT_Sum(x - 1,y - 1,z);
	printf("%d\n",ans);
	...
}
```

