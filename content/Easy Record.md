---
title: Easy Record
draft: false
tags:
  - solution
---

 
## [Dashboard - Codeforces Round 971 (Div. 4) - Codeforces](https://codeforces.com/contest/2009)
 
### [Problem - A - Codeforces](https://codeforces.com/contest/2009/problem/A)

给两个整数 $a,b \,(a\leq b)$. 找 $c\,(a\leq c\leq b)$ 使得 $(c-a)+(b-c)$ 最小，输出这个最小值.

$1\leq t\leq 55,1\leq a\leq b\leq 10$

$(c-a)+(b-c) = b-a$ 与 $c$ 无关， what can I say 👐

### [Problem - B - Codeforces](https://codeforces.com/contest/2009/problem/B)

你在玩一款音游 osu!mania. 节奏谱由 $n$ 行 4 列的字符组成， 越靠近底部的离你越近，你要从最下面打到最上面，每行只有一个音符，输出每行音符在哪里出现.

Just do it.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> int t, n, ans[505];
> string str;
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n;
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			cin >> str;
> 			for(int j = 0;j < 4;j ++)
> 				if(str[j] == '#')
> 					ans[i] = j + 1; 
> 		}
> 		for(int i = n;i >= 1;i --)
> 			cout << ans[i] << ' ';
> 		cout << endl;
> 	}
> 	return 0;
>  } 
> ```

### [Problem - C - Codeforces](https://codeforces.com/contest/2009/problem/C)

#math #greedy 

青蛙 Freya 正在二维平面上旅行，最开始在 $(0,0)$ ，她想去 $(x,y)$ . 在一次行动中，她只能朝她面向的方向跳 $d(0\leq d\leq k)$ 个单位.

最开始她面朝 $x$ 轴的正方向，跳一次后面朝 $y$ 轴正方向，再跳一次回到 $x$ 轴正方向，后面每跳一次就改变一次方向.

问：她最小行动次数是多少.

$1\leq t\leq 10^4,0\leq x,y\leq 10^9,1\leq k\leq 10^9$

因为可以跳 0 个单位，所以可以贪心每次都跳尽可能远 ，然后如果某个维度跳到了，这个维度就一直跳 0.

如果 $x$ 维度先跳到了，那就要一直跳 0 直到 $y$ 维度跳到，答案为 $step_y\times 2$.

如果 $y$ 维度先跳到了，那么也是一直跳 0 ，不过只需跳到 $x$ 最后一跳之前，$x$ 最后一跳之后就已经到达目的地了，答案为 $step_x \times 2 - 1$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> int t, x, y, k, ans, xx, yy;
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> x >> y >> k;
> 		xx = x/k; yy = y/k;
> 		if(x % k > 0) xx ++;
> 		if(y % k > 0) yy ++;
> 		if(xx > yy) cout << xx*2 - 1 << endl;
> 		else cout << yy*2 << endl;
> 	}
> 	return 0;
> }
> ```


### [Problem - D - Codeforces](https://codeforces.com/contest/2009/problem/D)

#combinatorics #math #enumerate

在一个二维平面上有 $n$ 个不同的点，保证所有的点 $(x_i,y_i)$ 满足 $0\leq y_i \leq 1$. 问这些点能组成多少个直角三角形.

$1\leq t\leq 10^4,3\leq n\leq 2\cdot 10^5, 0\leq x_i\leq n, 0\leq y_i \leq 1$

因为 $y$ 要么是 0 要么是 1， 能组成的直角三角形只有以下四种类型：

![[tri.png]]

1. $(x,0),(x,1),(p,0),p\neq x$， $p$ 的个数是 $y=0$ 的所有点数减去 1
2. $(x,0),(x,1),(p,1),p\neq x$， $p$ 的个数是 $y=1$ 的所有点数减去1
3. $(x,1),(x-1,0),(x+1,0)$， 1 个
4. $(x,0),(x-1,1),(x+1,1)$， 1 个

枚举关键点，直接计数即可. 时间复杂度 $O(n)$.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn =2e5;
> int t;
> ll n, ans,x,y, upcnt, downcnt;
> bool up[maxn + 5],down[maxn + 5];
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n;
> 		for(int i = 0;i <= n;i ++) up[i] = down[i] = 0;
> 		upcnt = 0;downcnt = 0;ans = 0;
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			cin >> x >> y;
> 			if(y == 0)
> 				down[x] = 1,downcnt ++;
> 			else up[x] = 1,upcnt ++;
> 		}
> 		for(int i = 0;i <= n;i ++)
> 		{
> 			if(down[i])
> 			{
> 				if(up[i]) ans += downcnt - 1;
> 				if(i > 0 && i < n)
> 					if(up[i - 1] == 1 && up[i + 1] == 1)
> 						ans ++;
> 			}
> 			if(up[i])
> 			{
> 				if(down[i]) ans += upcnt - 1;
> 				if(i > 0 && i < n)
> 					if(down[i - 1] == 1 && down[i + 1] == 1)
> 						ans ++;
> 			}
> 		}
> 		cout << ans << endl;
> 	}
> 	return 0;
> }
> ```

### [Problem - E - Codeforces](https://codeforces.com/contest/2009/problem/E)

#binary-search #math 

Klee 有一个长度为 $n$ 的数组 $a = \{k,k+1,...,k+n-1\}$ ，他想选一个下标 $i$ 使得 $x=|a_1+a_2+\cdots+a_i - a_{i+1}-\cdots - a_n|$ 最小. 求这个最小值.

$1\leq t\leq 10^4,2\leq n,k \leq 10^9$

等差数列求和重写一下 $x$

$$
\begin{aligned}
x &= \left|\dfrac{(k+k+i-1)\cdot i}{2}-\dfrac{(k+i+k+n-1)\cdot(n-i)}{2}\right|\\[2ex]
&= \dfrac{1}{2}\left|2i^2+(4k-2)i-(2k+n-1)n\right| \\[2ex]
&= \dfrac{1}{2}\left|2\left(i+\dfrac{2k-1}{2}\right)^2-\left(\dfrac{2k-1}{2}\right)^2\cdot 2  -(2k+n-1)n\right|
\end{aligned}
$$

后面一堆都是常数，只关注含 $i$ 的二次函数，发现在 $[1,n]$ 上是单调的，但是绝对值又使得其不单调，那就先去掉绝对值，然后二分 $i$ 使 $x$ 尽可能接近 0 ，这个过程中不断取更优的 $i$ 最后得到的就是答案.

时间复杂度 $O(\log n)$


> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const ll inf = 1e14;
> int t;
> ll n,k, sum, L, R, mid, res, ans, tmp;
> 
> ll calc(ll x)
> {
> 	return (k+k+x-1)*x/2 - (k+x+k+n-1)*(n-x)/2;
> }
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n >> k;
> 		L = 1;R = n;res = 1;
> 		ans = inf;
> 		for(;L <= R;)
> 		{
> 			mid = (L + R) >> 1;
> 			tmp = calc(mid);
> 			if(tmp <= 0)
> 			{
> 				res = mid;
> 				ans = min(ans,abs(tmp));
> 				L = mid + 1;
> 			}
> 			else
> 			{
> 				ans = min(ans,abs(tmp));
> 				R = mid - 1;
> 			}
> 		}
> 		cout << ans << endl;
> 	}
> 	return 0;
>  } 
> ```

### [Problem - F - Codeforces](https://codeforces.com/contest/2009/problem/F)

#binary-search #prefix_sum #math 

给一个长度为 $n$ 的数组 $a$ , 定义 $c_i$ 为 $a$ 的第 $i$ 轮转数组，$c_i = \{a_i,a_{i+1},...,a_n,a_1,...,a_{i-1}\}$

构造数组 $b = \{c_1,c_2,...,c_n\}$ .

有 $q$ 个询问，每次询问给出 $l,r$ ，求出 $\sum_{i = l}^r b_i$ 

$1\leq t \leq 10^4,1\leq n, q\leq 2\cdot 20^5,1\leq a_i \leq10^6, 1\leq l\leq r\leq n^2$

考虑 $[l,r]$ 之间 $b$ 的结构.


| block   | ... | p   | p+1 | ... | q   | ... |
| ------- | --- | --- | --- | --- | --- | --- |
| pointer |     | $l$ |     |     | $r$ |     |

可以发现，中间第 $p+1,...,q-1$ 这些轮转数组，它们的总和都是一样的，我们如果求出 $l, r$ 分别落在哪个轮转数组里，就可以计算出中间夹着的轮转数组个数 $k$，它们对答案的贡献是 $k\times sum$. 然后再计算两端轮转数组对答案的贡献即可.

对于第 $i$ 轮转数组，其左端点为 $(i-1)\cdot n+1$ ，右端点为 $i\cdot n$ ，二分即可求出 $l,r$ 落在哪个轮转数组里. 

计算两端轮转数组的话，可以先求以下数列的前缀和，然后根据下标规律推出 $l$ 在 p 轮转数组的下标 $L$， $r$ 在 q 轮转数组的下标 $R$.

| $a_1$  | $a_2$  | ... | $a_n$         | $a_1$  | ... | $a_{n-1}$ |
| ------ | ------ | --- | ------------- | ------ | --- | --------- |
| 第1轮转起点 | 第2轮转起点 | ... | 第1轮转终点；第n轮转起点 | 第2轮转终点 | ... | 第n轮转终点    |
| $s_1$  | $s_2$  | ... | $s_n,t_n$     | $t_2$  | ... | $t_n$     |

左端就是 ${\rm prefix\_sum}(t_p) - {\rm prefix\_sum}(s_p - 1 + L - 1)$

右端就是 ${\rm prefix\_sum}(s_q + R - 1) - {\rm prefix\_sum}(sq - 1)$

这样计算两端的复杂度就是 $O(1)$

总时间复杂度 $O(q\log n)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> int t;
> ll a[maxn + maxn + 5],L,R,suma,lb,rb,n,q,ans,psum[maxn + maxn + 5];
> 
> ll bisearch(ll x)
> {
> 	ll lp = 1;ll rp = n;ll mid = 0;ll res = 1;
> 	for(;lp <= rp;)
> 	{
> 		mid = (lp + rp) >> 1;
> 		if((mid - 1)*n + 1 <= x)
> 		{
> 			res = mid;
> 			lp = mid + 1;
> 		}
> 		else rp = mid - 1;
> 	}
> 	return res;
> }
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n >> q;suma = 0;
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			cin >> a[i];
> 			suma += a[i];
> 			psum[i] = psum[i - 1] + a[i];
> 		}
> 		for(int i = 1;i < n;i ++)
> 		{
> 			a[n + i] = a[i];
> 			psum[n + i] = psum[n + i - 1] + a[n + i];
> 		}
> 			
> 		for(int qq = 1;qq <= q;qq ++)
> 		{
> 			cin >> L >> R;
> 			ans = 0;
> 			lb = bisearch(L);
> 			rb = bisearch(R);
> 			ans = (rb - lb - 1)*suma;
> 			ans += psum[lb + n - 1] - psum[(L - ((lb - 1)*n + 1)) + lb - 1];
> 			ans += psum[R - 1 - (rb-1)*n + rb] - psum[rb - 1];
> 			cout << ans << endl;
> 		}
> 	}
> 	return 0;
> }
> ```