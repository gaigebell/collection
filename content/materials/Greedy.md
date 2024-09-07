---
title: Greedy Algorithm
draft: false
tags:
  - material
  - greedy
---

### 常见问题

#### 最优装载问题

> 给 $n$ 个物品，第 $i$ 个物品的重量为 $w_i$ ，选择尽量多的物品，使得总重量不超过 $C$

给重量从小到大排序，尽可能选即可。

与此类似的还有“用尽可能少的钞票支付”等。

这类题需要最纯粹的贪心。

#### 部分背包问题

> 给 $n$ 个物品，第 $i$ 个物品的重量为 $w_i$ ，价值为 $v_i$ ，每个物品可以只选一部分，价值 $v'=\dfrac{w'}{w_i}v_i$，选择的价值尽可能大，使得总重量不超过 $C$

考察衡量物品的指标，这里可以选用性价比 $\dfrac{v_i}{w_i}$ 作为贪心指标，性价比越大越划算，先把性价比高的选完即可。

这类问题启示我们在运用贪心的时候，要充分考虑对象的性质，利用合适的贪心指标排序贪心。

#### 乘船问题

> 有 $n$ 个人，第 $i$ 个人的重量为 $w_i$ 。每艘船的载重量均为 $C$ ，最多可乘坐两个人。求用最少的船装载所有人的方案

**贪心策略：最轻的人和最重的人配对坐船。**

反证法：



P1809 过河问题

> 有 $n$ 个人，每个人都有一个渡河时间 $T$，，船划到对岸的时间等于船上渡河时间较长的人所用时间。
>
> 船一次只能乘坐两人。最少要花费多少时间，才能使所有人都过河。
>
> 注意，只有船在东岸（西岸）的人才能坐上船划到对岸。

### 区间问题

#### 最多不相交区间

> 给 $n$ 个区间，选择尽量多的区间使得这些区间两两不相交

**贪心策略：每次选择选右端点最小的。**

#### 区间选点问题

> 给定 $n$ 个区间，在数轴上选尽量少的点，使得每个区间内至少有一个点

**贪心策略：将区间按右端点从小到大排序，每次检查区间，如果区间不被任何一个点覆盖，则在右端点增加一个覆盖点。**



##### P2887 [USACO07NOV]Sunscreen G

[P2887 [USACO07NOV]Sunscreen G](https://www.luogu.com.cn/problem/P2887)

画图，然后看图简化问题

![2887(1)](C:\Users\47328\Desktop\信息学\notes\2887(1).png)

将所有奶牛看成一段段区间，将每一瓶防晒霜看作点，问题转化为有 $\sum_{i=1}^{L}cover_i$ 个点，最多能覆盖多少条线段。

于是运用上述贪心思想就可以解决本题：将奶牛按右端点从小到大排序，然后防晒霜也从小到大排序，枚举防晒霜，再枚举奶牛，能覆盖的就覆盖即可。

可以发现，当我们将奶牛按右端点从小到大排序之后，我们总是优先处理右端点小的奶牛，就是说我们总是优先照顾不易满足的奶牛，后面的奶牛右端点更远，越有机会被满足。如果不这样处理，那么容易满足的奶牛先被满足，可以满足这些奶牛的其它防晒霜就被浪费了，前面不易满足的奶牛反而没有得到满足，所以按右端点从小到大排序的方案最优。

```C++
sort(a + 1,a + 1 + C,cmp);
sort(b + 1,b + 1 + L,cmp1);
for(int i = 1;i <= C;i ++)
	for(int j = 1;j <= L;j ++)
		if(b[j].num > 0 && a[i].l <= b[j].val && b[j].val <= a[i].r)
		{
			ans ++;
			b[j].num --;break;
		}
printf("%d",ans);
```

主要思想：当服务有限的时候，优先服务那些不易被满足的对象。



#### 区间覆盖问题

> 给 $n$ 个区间 $[a_i,b_i]$，选择尽量少的区间覆盖一段指定的区间 $[s,t]$

**贪心策略：将区间按右端点从小到大排序，每次选择覆盖点 $s$ 的区间中右端点最大的一个，然后将 $s$ 更新为次区间的右端点，一直选到覆盖完为止。**

##### [USACO 2004 December Silver] Cleaning Shifts

[[USACO 2004 December Silver] Cleaning Shifts](http://poj.org/problem?id=2376)





### 字典序最小问题

#### P2870 [USACO07DEC]Best Cow Line G

**贪心策略：令 $S'$ 为 $S$ 反转后的字符串，比较二者的字典序，若 $S$ 较小，就从开头取字符，若 $S'$ 较小，就从末尾取字符，若相同则任取。**



```C++
l = 0;r = N - 1;
for(register int i = 1;l <= r;i ++)
{
	flag = false;
	for(register int j = 0;l + j <= r;j ++) 
	{
		if(S[l + j] < S[r - j]) {flag = true;break;}
		else if(S[l + j] > S[r - j]) {flag = false;break;}
	}
	if(flag) putchar(S[l ++]);
	else putchar(S[r --]);
	if(i % 80 == 0) putchar('\n');
}
```





### 交换相邻法确定贪心顺序

#### P1223 排队接水

[P1223 排队接水](https://www.luogu.com.cn/problem/P1223)

有 $n$ 个人在一个水龙头前排队接水，假如每个人接水的时间为 $T_i$，请编程找出这 $n$ 个人排队的一种顺序，使得 $n$ 个人的平均等待时间最小。

求一个顺序，使得
$$
\rm{Ans} =\min\{\dfrac{1}{n}\sum_{i = 1}^{n}\sum_{j = 1}^{i}T_j\}
$$
钦定顺序可以考虑当两个人交换位置的情况：

第  $i$ 个人等待时间为 $S$ ，第 $i + 1$ 个人等待时间为 $S+T_i$ ，第 $i+2$ 个人等待时间为 $S+T_i+T_{i+1}$

总等待时间为 $3S+2T_i+T_{i+1}$ ，如果 $i$ 和 $i + 1$ 交换位置，则总等待时间变为 $3S+2T_{i+1}+Ti$

二者作差
$$
\begin{aligned}
&3S+2T_i+T_{i+1}-(3S+2T_{i+1}+Ti)\\
= & T_i-T_{i+1}
\end{aligned}
$$
若不交换位置的情况更优，则有 $T_i-T_{i+1}<0$ ，推得 $T_i<T_{i+1}$ 由此可知打水时间小的排在前面更优。

#### P1080 [NOIP2012 提高组] 国王游戏

[P1080 [NOIP2012 提高组] 国王游戏](https://www.luogu.com.cn/problem/P1080)

有关顺序的问题，我们通常考察交换相邻的两个人对结果带来什么影响。

根据题意，现在考察大臣 $i$ 与大臣 $i+1$ ，为了方便，先不考虑向下取整的情况
$$
c(i) = \dfrac{\prod_{j=1}^{i-1}L_j}{R_i},c(i+1)=\dfrac{\prod_{j=1}^{i}L_j}{R_{i+1}}
$$
考虑交换二人的位置
$$
c'(i)=\dfrac{\prod_{j=1}^{i-1}L_j\cdot L_{i+1}}{R_i},c'(i+1)=\dfrac{\prod_{j=1}^{i-1}L_j}{R_{i+1}}
$$
假设原来大臣 $i+1$ 手上的金币更多，那么观察可得，交换顺序后， $i+1$ 手上的金币一定减少，$i$ 手上的金币一定增多，若使这一次交换有效，即使得 $\max\{c'(i),c'(i+1)\}< \max\{c(i),c(i+1)\}$ ，则有
$$
c'(i)=\dfrac{\prod_{j=1}^{i-1}L_j\cdot L_{i+1}}{R_i}<\dfrac{\prod_{j=1}^{i}L_j}{R_{i+1}}=c(i+1)
$$
可以发现，交换有效的条件是 $L_iR_i>L_{i+1}R_{i+1}$ ，所以贪心策略应是令 $L_iR_i$ 较小的大臣尽可能往前排。



### 流水作业调度（交换相邻法的进阶应用）

> 有 $n$ 个作业要在 $A$，$B$ 两个车间加工，每个作业 $i$ 都必须先花时间 $a_i$ 在 $A$ 中加工，然后花 $b_i$ 在 $B$ 中加工，确定 $n$ 个作业的加工顺序，使得加工总时间最短。

贪心策略：

直观上，让 $A$ 没有空闲，让 $B$ 的空闲时间尽量的短，于是猜测把在 $A$ 机器上加工时间最短的部件最先加工，这样使得 $B$ 机器能在最短的空闲时间内开始加工，把在 $B$ 机器上加工时间最短的部件最后加工，使 $A$ 机器能用最短的时间等待 $B$ 机器完工。

Johnson 算法：设 $N_1$ 为 $a<b$ 的作业集合，$N_2$ 为 $a\geqslant b$ 的作业集合，将 $N_1$ 的作业按 $a$ 非减序排序，$N_2$ 中的作业按照 $b$ 非增序排序，则 $N_1$ 作业接 $N_2$ 作业构成最优顺序。时间复杂度 $O(n\log n)$

正确性证明：

设 $S=\{J_1,J_2,\cdots ,J_n\}$ 为待加工部件的作业顺序，若 $A$ 机器开始加工 $S$ 中的部件时，$B$ 机器还在加工其它部件，$t$ 时刻后 $B$ 机器可以加工 $A$ 机器加工过的部件。在这样的条件下，加工 $S$ 中任务所需的最短时间 $T(S,t)=\min\{a_i+T(S-\{J_i\},b_i+\max\{t - a_i,0\})\}$

其中 $b_i+\max\{t - a_i,0\}$ 表示 $B$ 机器下一次空闲所需的时间，首先 $B$ 机器一定要完成当前的工作，用时 $b_i$ 。$B$ 机器加工完后，如果 $A$ 机器还没加工完，那么就要等 $t-a_i$ 。如果 $A$ 机器已经加工完了，那就不用等了。

使用交换相邻法证明：

假设最佳方案中，先加工 $J_i$ ，然后加工 $J_j$ ，则有：
$$
\begin{aligned}
T(S,t) & = a_i+T(S-\{J_i\},b_i +\max\{t-a_i,0\})\\[2ex]
& = a_i + a_j + T(S-\{J_i,J_j\},b_j + \max\{b_i+\max\{t-a_i,0\}-a_j,0\})\\[2ex]
& = a_i + a_j + T(S-\{J_i,J_j\},T_{ij})
\end{aligned}
$$

$$
\begin{aligned}
T_{ij} &= b_j + \max\{b_i+\max\{t-a_i,0\}-a_j,0\}\\[2ex]
& = b_j + b_i -a_j +\max\{\max\{t-a_i,0\},a_j-b_i\}\\[2ex]
& = b_j+b_i-a_j+\max\{t-a_i,a_j-b_i,0\}\\[2ex]
& =\begin{cases}
b_j+b_i-a_i-a_j+t & ,\max\{t-a_i,a_j-b_i,0\}=t-a_i\\[2ex]
b_j& ,\max\{t-a_i,a_j-b_i,0\}=a_j-b_i\\[2ex]
b_j+b_i-a_j& ,\max\{t-a_i,a_j-b_i,0\}=0\\[2ex]
\end{cases}
\end{aligned}
$$

若交换作业 $J_i$ 和作业 $J_j$ 的顺序，则有：
$$
T'(S,t)=a_i+a_j+T(S-\{J_i,J_j\},T_{ji})\\
$$
又因为
$$
T_{ji}=b_i+b_j-a_i-a_j+\max\{t,a_j,a_i+a_j-b_j\}
$$
按假设
$$
T_{ij}\leqslant T_{ji}\\
\max\{t,a_i+a_j-b_i,a_i\}\leqslant \max\{t,a_i+a_j-b_j,a_j\}\\
a_i+a_j+\max\{-b_i,-a_j\} \leqslant a_i+a_j +\max\{-b_j,-a_i\}\\
$$

$$
\min\{b_j,a_i\}\leqslant \min\{b_i,a_j\}\tag{1}
$$



由此可得，当满足 $(1)$ 式的时候，$J_i$ 排在 $J_j$ 的前面更优。 

```cpp
#include<cstdio>
#include<iostream>
#include<algorithm>

using namespace std;
int n,ans,ord[1005];
struct job{
	int a;
	int b;
	int id;
	int m;
}J[1005];

bool cmp(job x,job y)
{
	return x.m < y.m;
}

int main()
{
	scanf("%d",&n);
	for(int i = 1;i <= n;i ++) scanf("%d",&J[i].a),J[i].id = i;
	for(int i = 1;i <= n;i ++) scanf("%d",&J[i].b),J[i].m = min(J[i].a,J[i].b);//将作业集合分为两个部分，a < b 和 a >= b  
	sort(J + 1,J + 1 + n,cmp);//对指标排序 
	for(int i = 1,l = 1,r = n;i <= n;i ++)//将 N1 和 N2 接起来 
	{
		if(J[i].m == J[i].a) ord[l] = i,l ++;
		else ord[r] = i,r --;
	}
	int k = 0;
	for(int i = 1;i <= n;i ++)//模拟计算加工时间 
	{
		k += J[ord[i]].a;
		if(ans < k) ans = k;
		ans += J[ord[i]].b;
	}
	printf("%d\n",ans);
	for(int i = 1;i <= n;i ++)
	{
		printf("%d ",J[ord[i]].id); 
	}
	return 0;
 } 
```









### 反悔法贪心（常用优先队列维护）



#### CF865D Buy Low Sell High

[CF865D Buy Low Sell High](https://www.luogu.com.cn/problem/CF865D)

首先 $n$ 这么大好像没办法 DP ，考虑贪心。

先想想这里一次决策是什么？

1. 买入
2. 卖出

贪心策略是什么？

最低价买入最高价卖出？错，例如连续的四天 $p_1<p_2>p_3<p_4,p_2 < p_4$ ，如果按照这个思路，那么收益只有 $p_4 - p_1$ ，然而最大收益有可能是 $p_2+p_4 - p_1-p_3$ ，如 1 6 4 8 ，最大收益应当是 9 .通过反例，我们否决这一策略。

一有收益就卖出？也不对，这个反例更好举， 1 2 3 4 

那么正确的贪心策略是什么？按照时间顺序纯粹贪心，一出现收益就获取收益，并且提供反悔机制。

设计反悔机制：每次都买入股票，当某一天手中某一只股票可以收益的时候，我们就将它卖出，但是要提供反悔机会，使其可以反悔在另外一天卖出，例如第 $i$ 天买入股票，第 $j$ 天卖出，后来发现第 $k$ 天卖出更划算，于是有 $-p_i+p_j-p_j+p_k$ ，也就是说，当在第 $j$ 天卖出的时候，要给机会 $-p_j$ ，于是反悔机制设计完成。

贪心策略 1 ：每天买入，代价 $-p_i$

贪心策略 2 ：手上有股票可以收益的时候卖出，代价 $p_j-p_i$

反悔策略 ：以前有一股卖出的股票现在卖出更优，代价 $-p_j+p_k$



```cpp
#include<iostream>
#include<cstdio>
#include<queue>

using namespace std;
typedef long long ll;
ll n,p[300005],val1;
priority_queue<ll > q1; 
ll ans;

int main()
{
	scanf("%lld",&n);
	for(int i = 1;i <= n;i ++)
	{
		scanf("%lld",&p[i]);
		if(!q1.empty())
			if((val1 = q1.top()) + p[i] > 0)
			{
				ans += (val1 + p[i]);//收益
				q1.pop();
				q1.push(-p[i]);//提供反悔的机会
			}
		q1.push(-p[i]);//买入
	}

	printf("%lld",ans);
	return 0;
}
```





#### P3045 [USACO12FEB]Cow Coupons G

[P3045 [USACO12FEB]Cow Coupons G](https://www.luogu.com.cn/problem/P3045)

##### 概述

对于给出一次决策多种选择的情况，如果用贪心法解决，考虑运用反悔法。我们在反悔法中常常关注每一次新的决策，利用优先队列设置反悔机会，达到局部最优而求全局最优。

##### 纯粹贪心

对于本题，一次决策给出了两种选择：

1. 用优惠券
2. 不用优惠券

并且限制了优惠券的数目。如果我们直接看这两种选择贪心，那么首先利用最纯粹的贪心思想要想到一个结论：全部优惠券都要用上。因为优惠券总是比原价便宜，所以不用就亏了。

于是我们先按优惠券的价格给奶牛排序，先花光优惠券。

##### 反悔贪心

现在我们的优惠券花光了，然后我们要对剩下的奶牛进行决策。

这时剩下的奶牛都要用原价买吗？不一定，可能存在 $c_i-c_j+p_j<p_i$ 的情况，即在先前使用优惠券的奶牛 $j$ 中撤回优惠券并用到奶牛 $i$ 身上可能使决策更优，所以我们要设置反悔机制，令使用了优惠券的奶牛有反悔的机会。

现在要考察两种选择：

1. 从先前使用过优惠券的奶牛 $j$ 中回收优惠券用在 $i$ 身上，代价 $c_i-c_j+p_j$
2. 用原价购买奶牛 $i$ ，代价 $p_i$

我们可以用数据结构优化决策的复杂度，用小根堆 ```q1``` 储存用过优惠券的奶牛的回收代价，用小根堆 ```q2``` 储存没有用过优惠券的奶牛原价购买的代价，用小根堆 ```q3``` 储存没有用优惠券的奶牛如果使用优惠券的代价，于是我们每次取两个堆的堆顶，比较 $c_k-c_j+p_j$ 和 $p_i$ ，然后决定是用原价购买 $i$ ，还是撤回 $j$ 的优惠券购买 $k$ 。决策之后，将情况改变的奶牛按当前情况加入不同的堆中，再继续贪心，如此反复直到情况最优，手上的钱花光为止就得到答案了。

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<queue>

using namespace std;
typedef long long ll;
ll N,K,M,x,y,ans,tmp,tmp1,tmp2,tmp3,tmp4;
priority_queue<pair<ll ,int > > qc,qpc,qp;
bool chosen[50005];
struct cows{
	ll p;
	ll c;
	int id;
}a[50005];

bool cmpc(cows x,cows y)
{
	if(x.c != y.c) return x.c < y.c;
	return x.p > y.p;
}

int main()
{
	scanf("%lld%lld%lld",&N,&K,&M);
	for(int i = 1;i <= N;i ++)
	{
		scanf("%lld%lld",&x,&y);
		a[i].p = x;a[i].c = y;a[i].id = i;
		qc.push(make_pair(-y,i));
		qp.push(make_pair(-x,i));
	}
	sort(a + 1,a + 1 + N,cmpc);
	for(int i = 1;i <= min(N,K);i ++)
	{
		M -= a[i].c;
		if(M < 0)
		{
			printf("%lld",ans);
			return 0;
		}
		ans ++,chosen[i] = 1;
		qpc.push(make_pair(-(a[i].p-a[i].c),i));
	}
	for(int i = K + 1;i <= N;i ++)
	{
		for(;chosen[qc.top().second];) qc.pop();
		for(;chosen[qp.top().second];) qp.pop();
		tmp = -qpc.top().first;tmp1 = -qp.top().first;tmp2 = -qc.top().first;
		if(tmp1 > tmp2 + tmp)
		{
			M -= (tmp2 + tmp);
			if(M < 0) break;
			chosen[qc.top().second] = 1;
			tmp3 = qc.top().second;tmp4 = qpc.top().second;
			qc.pop();qpc.pop();ans ++;
			qc.push(make_pair(-a[tmp4].c,tmp4));qpc.push(make_pair(-(a[tmp3].p - a[tmp3].c),tmp3));
		}
		else
		{
			M -= tmp1;
			if(M < 0) break;
			chosen[qp.top().second] = 1;
			qp.pop();ans ++;
		}
	}
	printf("%lld",ans);
	return 0;
}
```



#### CF436E Cardboard Box※

[CF436E Cardboard Box](https://www.luogu.com.cn/problem/CF436E)

##### 线段树

考虑我们已经得到了最优解。

将各关卡按照 $b_i$ 从小到大排序，令选择的两星关卡中用时最长的一关 $k$ ，由于用时最长，那么 $k$ 之前的关卡都至少完成了一颗星，否则令没有星的关卡 $k'$ 作为用时最长的两星关卡比当前方案更优，同时 $k$ 之后的关卡都至多完成了一颗星。

枚举 $L$ ，令前 $L$ 个关卡至少获得一颗星，现在我们至少获得了 $L$ 颗星，如果没有达到目标，那么在 $L$ 之前可能有关卡要变成两颗星，代价  $b_i - a_i$ ，$L$  之后可能有关卡变成一颗星，代价 $a_i$ ， 令 $c_i=\begin{cases}b_i-a_i&(i\leqslant L)\\a_i &(i>L)\end{cases}$ ，然后取 $c_i$ 最小的 $w-L$ 个即可。

用权值线段树或者树状数组维护代价，最开始将所有获得一颗星的代价计入权值线段树中。枚举 $L$ ，每一次在所有代价中选最小的 $w-L$ 个，其实就是前 $w-L$ 个权值，然后更新答案，并将新的代价 $b_L-a_L$ 存进线段树中，继续枚举。 过程中我们记录总价值最小的 $L$ ，在最后用优先队列维护前 $L$ 个关卡从一星变两星的代价和后面 $n-L$ 个关卡零星变一星的代价，再逐一取出 $w-L$ 个，这些关卡就是解。

##### 反悔贪心

考虑每次决策增加 1 颗星，有以下四种方式：

1. 选择一个没有选星星的位置  $i$，付出 $a_i$ 的代价选一颗星
2. 选择一个已经选了一颗星的位置 $i$，然后付出 $b_i−a_i$ 的代价选上第二颗星
3. 选择一个已经选了一颗星的位置 $i$，再选一个没有选星星的位置 $j$，将原来 $i$ 位置上选的那颗星星反悔不选，然后在位置 $j$上选上两颗星，代价 $b_j−a_i$
4. 选择一个已经选了两颗星的位置 $i$，再选一个没有选星星的位置 $j$，将原来 $i$ 位置上的第二颗星星反悔不选，再在 $j$ 位置上选两颗星星，代价 $b_j−(b_i−a_i)$

可以用优先队列维护这几种情况，然后每次选 1 颗星，就是在这 4 种情况中选择最优的一种。

复杂度 $O(n\log n)$

如何发现这四种情况？通过列举例子发现！前两个贪心策略很好想，第一个反悔策略也可以从题目中的样例发现，但是第二个反悔策略要通过自己出样例发现，第二个反悔策略可以在 $w>n$ 的样例中发现。

例如：

```
5 9
5 10
6 9
10 20
10 20
25 30
```

依靠前三个策略，发现答案是 ```22221``` 但是正确答案是 ```22212``` ，问题在于没有令最后两个关卡从 ```21``` 变为 ```12``` 也就是选了两颗星星的关卡第二颗反悔不选，令另外一个没有星星的关卡选两颗。

##### 不反悔贪心

如果不反悔贪心，那么每一步必须保证没有后效性地最优。

很明显如果只有 $a_i$，排序可以满足这个条件。

这题最朴素的错误做法是用一个小根堆，刚开始把所有 $a_i$ 丢进去，然后 $m$ 次每次取堆顶，如果选了 $a_i$ 把 $b_i−a_i$ 也丢进去。

如果 $a_i$ 都很大，$b_i−a_i$ 都很小这样显然是亏的。

假如某个 $i$ 打出 2 星是当前最优，如果 $a_i$ 已经取了按照上面的朴素贪心是可行的，但是如果 $a_i$ 都没取，$a_i$ 不一定是单个中的最优。

于是我们考虑将所有星星同等化，先用一个优先队列维护所有 $b_i$ ，如果当前零星关卡的 $b_i$ 中的最小值小于当前一星关卡中 $a_i$ 的两个最小值之和，那么就把 $i$ 打到 1 星，并将 $b_i - a_i$ 压入一星关卡的队列中。

##### 决策单调性优化 DP

不会...下次...大学再学...

```cpp
//反悔贪心参考程序
#include<iostream>
#include<cstdio>
#include<queue>

using namespace std;
typedef long long ll;
const ll inf = 999999999999999;
int n,w,flag,ans[300005];
ll a[300005],b[300005],minval,mincost,val1,val2,val3,val4,id1,id2,id3,id4,id5;
priority_queue<pair<ll, ll> > q1,q2;
/*
令每一次贪心操作都增加1星 
贪心1：每次将0星关卡打到1星
贪心2：每次将1星关卡打到2星
反悔1：每次将1星关卡反悔为0星，将另一0星关卡打到 2星
反悔2：每次将2星关卡反悔为1星，将另一0星关卡打到 2星 
*/
priority_queue<pair<ll,ll> > q3,q4,q5; 


pair<ll ,ll> mp(ll x,ll y)
{
	return make_pair(x,y);
}

int main()
{
//	cout << inf << endl << 0x3f3f3f3f3f3f;
	scanf("%d%d",&n,&w);
	for(int i = 1;i <= n;i ++)
	{
		scanf("%lld%lld",&a[i],&b[i]);
		q1.push(mp(-a[i],i));//一开始没有打关卡 
		q5.push(mp(-b[i],i));
//		q2.push(mp(-(b[i]-a[i]),i));
	}
	for(int i = 1;i <= w;i ++)
	{
		minval = inf;
		for(;!q1.empty() && ans[q1.top().second] != 0;) q1.pop();//除去不合法的情况 
		for(;!q2.empty() && ans[q2.top().second] != 1;) q2.pop();
		for(;!q3.empty() && ans[q3.top().second] != 1;) q3.pop();
		for(;!q5.empty() && ans[q5.top().second] != 0;) q5.pop();
		for(;!q4.empty() && ans[q4.top().second] != 2;) q4.pop();
		if(!q1.empty())//当前关卡没打过，打1星 
		{
			val1 =  -q1.top().first;
			if(val1 < minval) 
			{
				minval = val1;flag = 1;
				id1 = q1.top().second;
			}
		}
		if(!q2.empty())//当前关卡打过1星，再打1星 
		{
			val2 = -q2.top().first;
			if(val2 < minval)
			{
				minval = val2;flag = 2;
				id2 = q2.top().second;
			}
		}
		if(!q3.empty() && !q5.empty())//以前打过1星的，反悔不打，将另一关打成2星
		{
			val3 = -q3.top().first - q5.top().first;
			if(val3 < minval) 
			{
				minval = val3;
				flag = 3;id3 = q3.top().second;id5 = q5.top().second;
			}
		}
		if(!q4.empty() && !q5.empty())//以前打过2星的，反悔退回1星，将另一关打成2星 
		{
			val4 = -q4.top().first - q5.top().first;
			if(val4 < minval)
			{
				minval = val4;
				flag = 4;id4 = q4.top().second;id5 = q5.top().second;
			}
		}
		if(flag == 1)
		{
			mincost += minval;
			ans[id1] = 1;
			q1.pop();
			q2.push(mp(-b[id1]+a[id1],id1));//可以打2星了 
			q3.push(mp(a[id1],id1));//加入反悔录 
		}
		if(flag == 2)
		{
			mincost += minval;
			ans[id2] = 2;q2.pop();
			//要消除打1星的记录 ，以ans为标记，对q3弹出所有ans[i]=2的元素 
			q4.push(mp(b[id2] - a[id2],id2));//加入反悔录 
		}
		if(flag == 3)//
		{
			mincost += minval;
			ans[id3] = 0;ans[id5] = 2;
			q5.pop();q3.pop();
			q1.push(mp(-a[id3],id3));
			q4.push(mp(b[id5] - a[id5],id5));
			q5.push(mp(-b[id3],id3));
		}
		if(flag == 4)
		{
			mincost += minval;
			ans[id4] = 1;ans[id5] = 2;
			q5.pop();q4.pop();
			q2.push(mp(- b[id4] + a[id4],id4));
			q3.push(mp(a[id4],id4));
			q4.push(mp(b[id5] - a[id5],id5));
		}
	}
	printf("%lld\n",mincost);
	for(int i = 1;i <= n;i ++) printf("%d",ans[i]);
	return 0;
} 
```



### 其它

#### P4952 [USACO04MAR]Financial Aid（利用数据结构）

[P4952 [USACO04MAR]Financial Aid](https://www.luogu.com.cn/problem/P4952)

注意到只要求中位数最大，所以可以考虑纯粹贪心，枚举一个奶牛，令他的成绩作为中位数，令比成绩他大的 $\dfrac{n-1}{2}$ 个奶牛和成绩比他小的  $\dfrac{n-1}{2}$ 个奶牛的奖学金之和尽可能的小。

如果直接枚举与计算，时间复杂度 $O(n^3)$ ，所以我们考虑优化。

首先从算法上进行一些小优化，将奶牛的成绩从小到大排序，那么我们要枚举的范围就是其中的 $[\dfrac{n - 1}{2},C-\dfrac{n - 1}{2}]$ ，并且成绩大的奶牛都在后面，成绩小的奶牛都在前面，我们从成绩大的奶牛开始枚举，一旦枚举到合法的就可以直接退出。

我们希望更高效地计算除去中位数奶牛的其它奶牛的最小奖学金总和，考虑利用数据结构维护。

我们发现，从枚举 $i$ 到枚举 $i - 1$ ，$i$ 变成了成绩比中位数大的奶牛， $i-1$ 从成绩比中位数小的奶牛中被踢出，于是可以考虑用优先队列维护成绩比中位数大的奶牛的奖学金，每次枚举就往堆中加元素即可。但是如何维护成绩比中位数小的奶牛的奖学金？继续用优先队列维护？但是从堆中除去 $i - 1$ 有点麻烦。这时想到维护它们奖学金的单调性和位置的范围，就能想到用单调队列。用优先队列从小到大维护选择了的奶牛的成绩，剩下没有选的奶牛装进单调队列里维护它们奖学金不降，枚举的过程中如果优先队列中的奶牛被弹出了，就从单调队列中的奶牛里抽一个填进去，这样时间复杂度就降至 $O(n\log n)$ 

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<queue>

using namespace std;
typedef long long ll;
int N,C;
ll pos[100005];
ll F,sum1,sum2,q3[100005],head = 1,tail = 0,ans = -1;
priority_queue<ll > q2;
priority_queue<pair<ll ,int> > q1;
priority_queue<pair<int ,ll> > q4;
struct cows{
	ll A;
	ll Q;
}c[100005];

bool cmp(cows x,cows y)
{
	return x.A < y.A;
}

void myUpdate(int x)//单调队列更新
{
	for(;head <= tail && pos[head] >= x;) head ++;//弹出不在位置范围中的奶牛
	return ;
}

void myPush(int x,ll val)//单调队列更新
{
	for(;head <= tail && q3[tail] > val;) tail --;
	tail ++;q3[tail] = val;pos[tail] = x;
	return ;
}

int main()
{
	scanf("%d%d%lld",&N,&C,&F);
	for(int i = 1;i <= C;i ++)
		scanf("%lld%lld",&c[i].A,&c[i].Q);
	sort(c + 1,c + 1 + C,cmp);
	for(int i = 1;i < C - N/2;i ++) q1.push(make_pair(-c[i].Q,c[i].A));
	for(int i = 1;i <= N/2;i ++)
	{
		sum1 -= q1.top().first;
		q4.push(make_pair(q1.top().second,-q1.top().first));//选择成绩比中位数小的奶牛
		q1.pop();
	}
	for(;!q1.empty();)
	{
		myPush(q1.top().second,-q1.top().first);//剩下的丢进单调队列里面
		q1.pop();
	}
	for(int i = 1;i <= N/2;i ++)
	{
		q2.push(c[C - i + 1].Q);//选择成绩比中位数大的奶牛
		sum2 += c[C - i + 1].Q;
	}
	for(int i = C - N/2;i > N/2;i --)//枚举中位数
	{
		if(sum1 + sum2 + c[i].Q <= F)
		{
			ans = c[i].A;//得到答案并退出
			break;
		}
		if(c[i].Q < q2.top())//i 将成为成绩比中位数大的奶牛
		{
			sum2 -= q2.top();//维护最小奖学金之和
			q2.pop();q2.push(c[i].Q);//更新
			sum2 += c[i].Q;
		}
		if(i - 1 == q4.top().first)//被选择了的将成为中位数
		{
			sum1 -= q4.top().second;//维护最小奖学金之和
			q4.pop();q4.push(make_pair(pos[head],q3[head]));//更新堆
			sum1 += q3[head];//从单调队列里抽一个出来填补
			head ++;//更新单调队列
		}
		else myUpdate(c[i - 1].A);//更新单调队列
	}
	printf("%lld",ans);
	return 0;
}
```

