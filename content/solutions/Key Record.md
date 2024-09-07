---
title: Key Record
draft: false
tags:
  - solution
---

### [F - Gather Coins (atcoder.jp)](https://atcoder.jp/contests/abc369/tasks/abc369_f)

#dp #ds/segment_tree #ds/binary_index_tree

有一个 $H$ 行， $W$ 列的网格. 有 $N$ 枚硬币在这个网格上. 你从 $(1,1)$ 出发，只能向下或向右走，最终要到达 $(H,W)$ ，在这个过程中捡起尽可能多的金币. 输出最多可以捡起的金币数量和对应的方案.

$2\leq H,W \leq 2\times 10^5, 1\leq N\leq \min(HW-2,2\times 10^5)$

考虑 DP ，不妨先想想最朴素的办法，设 $f(i,j)$ 表示走到格子 $(i,j)$ 的最大金币数. 转移方程显然，但是复杂度爆炸. 这时看到 $1\leq N \leq \min(HW-2,2\times 10^5)$ ，不妨想想 $f(i)$ 表示走到第 $i$ 个金币可以捡起的最大金币数.

这时要想到赋予这些金币一些顺序，于是不妨第一关键字 $x$ 从小到大排序，然后第二关键字 $y$ 从小到大排序.

这样第 $i$ 枚金币在序列中的左边的金币同时是其在网格中上面的金币. 现在只要考虑如何高效地从这些上面的金币中再找到网格中位于 $i$ 左边的金币，并求其最大的 $f(j)$. 

$$
f(i)= \max_{j\leqslant i \,且\,j\,在\,i\,左边}\{f(j)\} + 1
$$

用树状数组或线段树维护. 以 $y$ 作为定义域，维护 $f$ 的最大值. 每次先查询、DP，然后将当前的信息丢进树状数组里维护. 这样树状数组维护的就是所有在 $i$ 上面，所有关于 $y$ 区间的 $f$ 最大值.

对应的方案直接记录路径即可.

时间复杂度 $O(N\log W)$，最后记录对应方案复杂度 $O(H + W)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int H, W, N;
> int tr[maxn + 5], tr_rec[maxn + 5];
> int f[maxn + 5], g[maxn + 5], lst, ans, stop[maxn + 5];
> string seq;
> pair<int ,int > tmp;
> 
> struct coin{
> 	int x;
> 	int y;
> }a[maxn + 5];
> 
> bool cmp(coin p,coin q)
> {
> 	if(p.x != q.x) return p.x < q.x;
> 	return p.y < q.y;
> }
> 
> void BIT_modify(int x,int u,int v)
> {
> 	for(;x <= W;x += x&(-x))
> 	{
> 		if(tr[x] < u)
> 		{
> 			tr[x] = u;
> 			tr_rec[x] = v;
> 		}
> 	}
> 	return ;
> }
> 
> pair<int ,int > BIT_query(int x)
> {
> 	int res = 0;int rec = 0;
> 	for(;x;x -= x&(-x))
> 	{
> 		if(res < tr[x])
> 		{
> 			res = tr[x];
> 			rec = tr_rec[x];
> 		}
> 	}
> 	
> 	return make_pair(res,rec);
> }
> 
> int main()
> {
> 	cin >> H >> W >> N;
> 	for(int i = 1;i <= N;i ++)
> 	{
> 		cin >> a[i].x >> a[i].y;
> 	}
> 	
> 	a[N + 1].x = H;a[N + 1].y = W;
> 	a[0].x = 1;a[0].y = 1;
> 	sort(a + 1,a + 1 + N,cmp);
> 	
> 	for(int i = 1;i <= N;i ++)
> 	{
> 		tmp = BIT_query(a[i].y);
> 		f[i] = tmp.first + 1;
> 		g[i] = tmp.second;
> 		BIT_modify(a[i].y, f[i], i);
> 	}
> 	
> 	for(int i = 1;i <= N;i ++)
> 	{
> 		if(ans < f[i])
> 		{
> 			ans = f[i];
> 			lst = i;
> 		}
> 	}
> 	
> 	for(int i = ans;i >= 1;i --)
> 	{
> 		stop[i] = lst;
> 		lst = g[lst];
> 	}
> 	
> 	stop[ans + 1] = N + 1;
> 	
> 	for(int i = 1;i <= N + 1;i ++)
> 	{
> 		for(int j = 1;j <= a[stop[i]].x - a[stop[i - 1]].x;j ++)
> 			seq += 'D';
> 		for(int j = 1;j <= a[stop[i]].y - a[stop[i - 1]].y;j ++)
> 			seq += 'R';
> 	}
> 	
> 	cout << ans << endl << seq;
> 	
> 	return 0;
> }
> ```

