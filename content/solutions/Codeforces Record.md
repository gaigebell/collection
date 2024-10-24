---
title: Codeforces Record
draft: false
tags:
  - solution
---

### 981 (Div. 3)

#### [Problem - 2033C - Codeforces](https://codeforces.com/problemset/problem/2033/C)

#dp 

C 比 D 难... 好吧是我太菜了...

给一个数组 $\{a_n\}$ ，定义打扰值为满足 $a_j = a_{j + 1}, 1\leqslant j < n$ 的 $j$ 的个数.

你可以交换 $a_i$ 和 $a_{n-i+1}$ ，并且可以进行任意次操作.

求经过任意次操作之后，数组的最小打扰值是多少？

$1\leqslant t\leqslant 10^4$

$2\leqslant n \leqslant 10^5$

如果我们以正常的思维去想的话，我们会想什么时候交换最合适. 但是只要这样想， 我们每次交换 $a_i$ 和 $a_{n-i+1}$ 之后，总是具有后效性的.

比如我们从外往内考虑交换，交换完外面一层，去交换里面一层，在里面的交换就会影响前面交换产生的效益.

这迫使我们去想一种没有后效性的解决方案.

将交换操作变成放置操作.

我们不要想给定的数组，考虑一开始整个数组都是空的，然后往空数组 $b$ 从外往里添加数对 $(a_i,a_{n-i+1})$.

数对只有两种放置方法，$...,a_i,...,a_{n-i+1},...$ 或者 $...,a_{n-i+1},...,a_i,...$ 而且一旦放置，就与上一次产生效益，并且下一次放置不会影响这一次效益，只会产生新的效益.

于是我们考虑 DP. 设 $f(i,1/0)$ 表示放置了前 $i$ 对数对，放置顺序是 $0:a_i,a_{n-i+1}/1:a_{n-i+1},a_i$ 的最小打扰值.

转移方程如下：

$$
\begin{aligned}
f(i,0) = \min\{f(i-1,0)+val_0,f(i-1,1)+val_1\} \\[2ex]
f(i,1) = \min\{f(i-1,0)+val_1,f(i-1,1)+val_0\}
\end{aligned}
$$
其中，效益的计算如下

$$
\begin{aligned}
val_0 = [a_i=a_{i-1}]+[a_{n-i+1}=a_{n-(i-1)+1}] \\[2ex]
val_1 = [a_i=a_{n-(i-1)+1}]+[a_{n-i+1}=a_{i-1}]
\end{aligned}
$$

时间复杂度 $O(n)$

> [!code]- Code
> 
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 1e5;
> const int inf = 1e9;
> int t, n, a[maxn + 5], f[maxn + 5][2];
> int half, cnt1, cnt2, ans;
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         cin >> n;
>         for(int i = 1;i <= n;i ++)
>         {
>             cin >> a[i];
>             f[i][0] = f[i][1] = inf;
>         }
>         f[1][0] = f[1][1] = 0;
>         half = n/2;ans = 0;
>         if(n & 1) half ++;
>         else if(a[half] == a[half + 1])ans = 1;
>         for(int i = 2;i <= half;i ++)
>         {
>             cnt1 = 0;cnt2 = 0;
>             if(a[i] == a[i - 1]) cnt1 ++;
>             if(a[n - i + 1] == a[n - (i - 1) + 1]) cnt1 ++;
>             if(a[i] == a[n - (i - 1) + 1]) cnt2 ++;
>             if(a[n - i + 1] == a[i - 1]) cnt2 ++;
>             f[i][0] = min(f[i - 1][0] + cnt1, f[i - 1][1] + cnt2);
>             f[i][1] = min(f[i - 1][0] + cnt2, f[i - 1][1] + cnt1);
>         }
>         ans += min(f[half][0],f[half][1]);
>         
>         cout << ans << endl;
>     }
>     return 0;
> }
> ```


#### [Problem - 2033D - Codeforces](https://codeforces.com/problemset/problem/2033/D)

#greedy #prefix_sum 

给一个数组 $\{a_n\}$ ，求其中最多有多少互不相交的美丽区间.

$[l,r]$ 是一个美丽区间，当且仅当 $\sum_{i=l}^r a_i = 0$

$1\leqslant t\leqslant 10^4$

$1\leqslant n\leqslant 10^5$

$\sum_{i=l}^r a_i = 0$ 对于这样的条件，我们可以考虑 $\{a_n\}$ 的前缀和数组 $\{s_n\}$. 

如果区间 $[l,r]$ 是美丽区间，那么一定有 $s_r = s_{l-1}$

所以我们可以找出所有的美丽区间.

然后就是经典的贪心问题：[[Greedy#最多不相交区间]]

我们只要把所有区间按右端点由小到大排序，然后贪心选取即可.

时间复杂度 $O(n\log n)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 1e5;
> ll t, n, a[maxn + 5], s[maxn + 5];
> map<ll, int> pos_map;
> int bcnt,lst,ans;
> struct seg{
>     int lp;
>     int rp;
> }b[maxn + 5];
> 
> bool cmp(seg x, seg y)
> {
>     if(x.rp != y.rp) return x.rp < y.rp;
>     return x.lp < y.lp;
> }
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         cin >> n;bcnt = 0;
>         for(int i = 1;i <= n;i ++)
>         {
>             cin >> a[i];
>             s[i] = s[i - 1] + a[i];
>         }
>         pos_map.clear();
>         for(int i = 0;i <= n;i ++)
>         {
>             if(pos_map.count(s[i]) == 0) pos_map[s[i]] = i;
>             else
>             {
>                 bcnt ++;
>                 b[bcnt].lp = pos_map[s[i]] + 1;
>                 b[bcnt].rp = i;
>                 pos_map[s[i]] = i;
>             }
>         }
>         sort(b + 1,b + 1 + bcnt, cmp);
>         lst = -1;ans = 0;
>         for(int i = 1;i <= bcnt;i ++)
>         {
>             //cout <<'('<< b[i].lp << ','<<b[i].rp <<')'<<endl;
>             if(b[i].lp > lst)
>             {
>                 ans ++;
>                 lst = b[i].rp;
>                 
>             }
>         }
>         cout << ans << endl;
>     }
>     return 0;
> }
> ```
> 

### 974 (Div. 3)

#### [Problem - 2014B - Codeforces](https://codeforces.com/problemset/problem/2014/B)

#math/classification 

一棵树第 $i$ 年长出 $i^i$ 片新叶子. 一开始在第 1 年有 1 片叶子. 叶子最多存活 $k$ 年. 问第 $n$ 年树上的叶子是否是偶数.

$1\leq t\leq 10^4$

$1\leq n \leq 10^9, 1\leq k \leq n$

奇数年长出奇数片叶子，偶数年长出偶数片叶子.

第 $n$ 年存活的树叶是最后 $p=\min(n,k)$ 年长出来的树叶.

- $n$ 为偶数
	- $\left\lfloor\dfrac{p}{2}\right\rfloor$ 是奇数年的个数，如果为偶数，则总数为偶数，否则为奇数
- $n$ 为奇数
	- $\left\lceil\dfrac{p}{2}\right\rceil$ 是奇数年的个数，如果为偶数，则总数为偶数，否则为奇数


#### [Problem - 2014C - Codeforces](https://codeforces.com/problemset/problem/2014/C)

#binary-search 

给一个数组 $a$ , 令其最大的一个元素 $a_j=a_j + x$.

一个元素不满意意味着当其值严格小于数组平均值的一半.

找到这个最小的 $x$ 使得超过一半元素不满意.

$1\leq t \leq 10^4$

$1\leq n \leq 2\cdot 10^5, 1\leq a_i \leq 10^6$

二分套二分

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const ll inf = 1e12 + 5;
> const int maxn = 2e5;
> ll t, n, a[maxn + 5], sum;
> ll L, R, mid, rr, half_n, ans;
> 
> int bisearch(ll x)
> {
>     int lt = 1;int rt = n - 1;int mm = 0;
>     int res = 0;
>     for(;lt <= rt;)
>     {
>         mm = (lt + rt) >> 1;
>         if(2*n*a[mm] < sum + x)
>         {
>             res = mm;
>             lt = mm + 1;
>         }
>         else rt = mm - 1;;
>     }
>     return res;
> }
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         cin >> n;
>         half_n = n/2;
>         half_n ++;
>         sum = 0;
>         for(int i = 1;i <= n;i ++)
>         {
>             cin >> a[i];
>             sum += a[i];
>         }
>         sort(a + 1,a + 1 + n);
>         L = 0;R = inf;
>         mid = 0;ans = -1;
>         for(;L <= R;)
>         {
>             mid = (L + R) >> 1;
>             rr = bisearch(mid);
>             //cout << mid << ":" << rr << endl;
>             if(rr >= half_n)
>             {
>                 ans = mid;
>                 R = mid - 1;
>             }
>             else L = mid + 1;
>         }
>         cout << ans << endl;
>     }
>     return 0;
> }
> ```

#### [Problem - 2014D - Codeforces](https://codeforces.com/problemset/problem/2014/D)

#difference #2pointers

Robin 要工作 $n$ 天. 访客可以连续访问 $d$ 天. Robin 有 $k$ 件事情要做，第 $i$ 件事情从 $l_i$ 天持续到 $r_i$ 天. 现在分别找出使得访问期间 Robin 要做的不同事情最多和最少的来访日.

$1\leq t\leq 10^4$

$1\leq n \leq 10^5, 1\leq d, k\leq n$

准备两个数组，一个在每件事情开始的日子 $l_i$ 处 +1 ，另一个在每件事情结束后的日子 $r_i+1$ 处 +1.

准备两个指针 $L$ 和 $R$ . 每当 $R$ 扫过事情开始的日子时，给答案加上先前在数组中记录的当天新开始的事情数量. 每当 $L$ 扫过事情结束后的日子时，给答案减去先前在数组中记录的结束的事情数量 .

保持 $R - L + 1 = d$

扫描一遍得解，时间复杂度 $O(n)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int inf = 1e6;
> const int maxn = 2e5;
> int t, n, d, k, l, r, L, R;
> int cnt, pl[maxn + 5],mi[maxn + 5];
> int maxa, mina, max_pos, min_pos, ans1, ans2;
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         cin >> n >> d >> k;
>         for(int i = 1;i <= n;i ++) pl[i] = 0, mi[i] = 0;
>         for(int i = 1;i <= k;i ++)
>         {
>             cin >> l >> r;
>             pl[l] ++;
>             mi[r + 1] ++;
>         }
>         maxa = -1;mina = inf;
>         cnt = 0;
>         L = 1;R = 1;
>         for(;R < d;R ++)
>             if(pl[R] > 0)
>                 cnt += pl[R];
>         for(;R <= n;L ++, R ++)
>         {
>             if(pl[R] > 0)
>                 cnt += pl[R];
>             if(mi[L] > 0)
>                 cnt -= mi[L];
>             if(cnt > maxa)
>             {
>                 maxa = cnt;
>                 max_pos = L;
>             }
>             if(cnt < mina)
>             {
>                 mina = cnt;
>                 min_pos = L;
>             }
>         }
>         cout << max_pos << ' ' << min_pos << endl;
>     }
>     return 0;
> }
> ```

#### [Problem - 2014E - Codeforces](https://codeforces.com/problemset/problem/2014/E)

#graph/shortest-path #graph/shortest-path/layer 

有一张无向图，$n$ 个点 $m$ 条边，每条边都有对应的时间花费. 另外还有 $h$ 匹马，这 $h$ 匹马分别在不同的节点上，一旦骑上马，就不能下马，骑马通过的边是正常时间花费的一半.

A 从 1 出发，B 从 $n$ 出发，问他们最快什么时候可以相遇.

$1\leq t\leq 10^4$

$2\leq n\leq 2\cdot 10^5,1\leq m\leq 2\cdot 10^5, 1\leq h\leq n$

$2\leq w_i \leq 10^6$

如果没有马，那就跑两次最短路，分别得到最短时间花费 $f,g$ ，然后枚举相遇点取 $\min_i\{\max(f(i),g(i))\}$ 即为答案.

但是本题有马，也就是说在图上，我们有两种状态，一种是没有骑马，另一种是骑了马. 我们要给图分层，然后在跑最短路. 在某些情况下，可能先去骑马，然后折返用时会更短，在普通设置下状态只有位置，由于无后效性（[[Shortest Path#^311b6a]]），普通的最短路不能正确计算. 分层图这一个过程就很像 DP. [[Shortest Path#^712498]]

令 $f(i,0/1)$ 表示到位置 $i$ 不骑马/骑马的最短用时. $u\rightarrow v$ 的转移如下

- 没有骑马
	- $u$ 有马
		- $f(u,0)\rightarrow f(v,1)$
		- $f(u,0)\rightarrow f(v,0)$
	- $u$ 没马
		- $f(u,0)\rightarrow f(v,0)$
- 骑着马
	- $f(u,1)\rightarrow f(v,1)$


同样取答案即可.

时间复杂度：一次最短路 $O((2n+2m)\log 2m)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const ll inf = 1e15 + 5;
> const int maxn = 2e5;
> ll n, m, h, t, u, v, w;
> bool horse[maxn + 5], vis[maxn + 5][2];
> vector<pair<ll, ll> > G[maxn + 5];
> ll f[maxn + 5][2], g[maxn + 5][2], ans;
> 
> 
> void Dijkstra(int typ)
> {
>     if(typ)
>     {
>         for(int i = 1;i <= n;i ++) f[i][1] = f[i][0] = inf;
>         f[1][0] = 0;
>     }
>     else
>     {
>         for(int i = 1;i <= n;i ++) g[i][1] = g[i][0] = inf;
>         g[n][0] = 0;
>     }
>     // dis, now, sta
>     priority_queue<pair<ll, pair<ll, ll> > > q;
>     for(int i = 1;i <= n;i ++) vis[i][0] = vis[i][1] = 0;
>     if(typ) q.push(make_pair(0,make_pair(1,0)));
>     else q.push(make_pair(0,make_pair(n,0)));
>     for(;!q.empty();)
>     {
>         int now = q.top().second.first;
>         int sta = q.top().second.second;
>         int nxtsta = 0;
>         q.pop();
>         if(vis[now][sta]) continue;
>         vis[now][sta] = 1;
>         for(int i = 0, dest = 0, ti = 0;i < G[now].size();i ++)
>         {
>             dest = G[now][i].first;
>             ti = G[now][i].second;
>             if(sta == 1)
>             {
>                 ti = ti/2;
>                 nxtsta = 1;
>                 if(typ)
>                 {
>                     if(f[now][sta] + ti < f[dest][nxtsta])
>                     {
>                         f[dest][nxtsta] = f[now][sta] + ti;
>                         q.push(make_pair(-f[dest][nxtsta],make_pair(dest,nxtsta)));
>                     }
>                 }
>                 else{
>                     if(g[now][sta] + ti < g[dest][nxtsta])
>                     {
>                         g[dest][nxtsta] = g[now][sta] + ti;
>                         q.push(make_pair(-g[dest][nxtsta],make_pair(dest,nxtsta)));
>                     }
>                 }
>             }
>             else
>             {
>                 nxtsta = 0;
>                 if(typ)
>                 {
>                     if(f[now][sta] + ti < f[dest][nxtsta])
>                     {
>                         f[dest][nxtsta] = f[now][sta] + ti;
>                         q.push(make_pair(-f[dest][nxtsta],make_pair(dest,nxtsta)));
>                     }
>                 }
>                 else{
>                     if(g[now][sta] + ti < g[dest][nxtsta])
>                     {
>                         g[dest][nxtsta] = g[now][sta] + ti;
>                         q.push(make_pair(-g[dest][nxtsta],make_pair(dest,nxtsta)));
>                     }
>                 }
>                 if(horse[now] == 1)
>                 {
>                     nxtsta = 1;
>                     ti = ti/2;
>                     if(typ)
>                     {
>                         if(f[now][sta] + ti < f[dest][nxtsta])
>                         {
>                             f[dest][nxtsta] = f[now][sta] + ti;
>                             q.push(make_pair(-f[dest][nxtsta],make_pair(dest,nxtsta)));
>                         }
>                     }
>                     else{
>                         if(g[now][sta] + ti < g[dest][nxtsta])
>                         {
>                             g[dest][nxtsta] = g[now][sta] + ti;
>                             q.push(make_pair(-g[dest][nxtsta],make_pair(dest,nxtsta)));
>                         }
>                     }
>                 }
>             }
>         }
>     }
>     return ;
> }
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         for(int i = 1;i <= n;i ++) horse[i] = 0;
>         for(int i = 1;i <= n;i ++) G[i].clear();
>         cin >> n >> m >> h;
>         for(int i = 1;i <= h;i ++)
>         {
>             cin >> u;
>             horse[u] = 1;
>         }
>         for(int i = 1;i <= m;i ++)
>         {
>             cin >> u >> v >> w;
>             G[u].push_back(make_pair(v,w));
>             G[v].push_back(make_pair(u,w));
>         }
> 
>         Dijkstra(0);
>         Dijkstra(1);
> 
>         ans = inf;
>         for(int i = 1;i <= n;i ++)
>         {
>             ll tmp1 = max(f[i][1],g[i][1]);
>             ll tmp2 = max(f[i][0],g[i][0]);
>             ll tmp3 = max(f[i][0],g[i][1]);
>             ll tmp4 = max(f[i][1],g[i][0]);
>             ans = min(ans,min(tmp1,min(tmp2,min(tmp3,tmp4))));
>         }
> 
>         if(ans == inf) ans = -1;
>         cout  << ans << endl;
> 
>     }
>     return 0;
> }
> ```

不过话说有没有可能两个人都要去同一个地方取马然后出现实际上一个人骑了马，另一个没有骑马的情况呢. 这似乎是不可能的. 先到的直接奔着另一个的路上去即可.


---

### 973 (Div. 2)

#### [Problem - 2013A - Codeforces](https://codeforces.com/problemset/problem/2013/A)

有 $n$ 个水果. 搅拌机 1s 最多可以搅拌 $x$ 个水果. Zhan 1s 可以最多将 $y$ 个水果放进搅拌机.搅拌完成的水果会立刻拿出搅拌机不耗时. 问最快要多少秒搅拌完这 $n$ 个水果.

$$
ans = \left\lceil\dfrac{n}{\min(x,y)}\right\rceil
$$


#### [Problem - 2013B - Codeforces](https://codeforces.com/problemset/problem/2013/B)

#greedy #math #property 

进行 $n-1$ 次操作，每次操作选下标为 $i,j\,(1\leq i < j \leq n)$ 的数，删掉 $a_i$ ，令 $a_j = a_j - a_i$.

求最终剩下的数最大是多少.

答案总可以写成

$$
ans = a_n - K
$$
让 $K$ 尽可能小，可以贪心

$$
K = a_{n - 1}-a_{n-2}-\cdots-a_2-a_1
$$



#### [Problem - 2013C - Codeforces](https://codeforces.com/problemset/problem/2013/C)

#interactive #construction #string 

现在有一串密码，只由 0 和 1 组成. 告诉你字符串的长度为 $n$ ，接下来你有两种操作

- `? s` 询问 $s$ 是否为密码的子串.
- `! s` 回答密码

在 $2n$ 次询问以内找到密码.

我们可以一个字符一个字符地猜.

先随便猜 0 或 1. 然后在这一个字符的基础上，往右加字符，直到猜 0 和猜 1 都不对，这就意味着不能再往右边加字符了，这个时候开始往左边加字符.

这个时候，只要猜加 0 就可以了，如果询问得到肯定回答那么这就是正确的字符串. 如果得到否定的回答，显然只能加 1 了，就不要再猜 1 了.

同时还要注意自己正在猜的字符串的长度，一旦长度到了 $n$ 就没有必要继续询问了.

这样最多确实只需要 $2n$ 次询问.


> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> int t, n, cnt;
> string gue, ans;
> 
> int ask(string str)
> {
>     cout << "? " << str << endl;
>     cout.flush();
>     int res = 0;
>     cin >> res;
>     return res;
> }
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         cin >> n;
>         gue = "";
>         ans = "";
>         cnt = 0;
>         for(;cnt < n;)
>         {
>             gue = ans + "0";
>             if(ask(gue))
>             {
>                 ans = gue;
>                 cnt ++;
>             }
>             else
>             {
>                 gue = ans + "1";
>                 if(ask(gue))
>                 {
>                     ans = gue;
>                     cnt ++;
>                 }
>                 else
>                 {
>                     break;
>                 }
>             }
>         }
>         for(;cnt < n;)
>         {
>             gue = "0" + ans;
>             if(ask(gue))
>             {
>                 ans = gue;
>                 cnt ++;
>             }
>             else
>             {
>                 ans = "1" + ans;
>                 cnt ++;
>             }
>             
>         }
>         cout << "! " << ans << endl;
>         cout.flush();
>     }
>     return 0;
> }
> ```



---

### 970 (Div. 3)
#### [Problem - 2008C - Codeforces](https://codeforces.com/problemset/problem/2008/C)
#math #binary-search #brute-force 

定义好序列：

- $a_{i - 1} < a_i,\forall 2\leq i \leq n$
- $a_{i} - a_{i - 1} <a_{i + 1} - a_i,\forall 2\leq i\leq n$

想构造一个 $l\leq a_i\leq r$ 的好序列，问最长的长度是多少.

$1\leq t\leq 10^4, 1\leq l \leq r \leq 10^9$

最优的构造方案即为 $a_1 = l, a_{i} - a_{i - 1} = i - 1, \forall i \geq 2$

于是，解
$$
a_n = \dfrac{n(n-1)}{2}+l\leq r
$$
因为 $n\geq 1$ ，所以可以二分 $n(n-1)\leq 2(r-l)$

> [!code]- Code 
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> ll t,l,r;
> 
> ll bisearch(ll tar)
> {
> 	ll L = 1;ll R = 1000000000;
> 	ll mid = 0;ll res = 1;
> 	for(;L <= R;)
> 	{
> 		mid = (L + R) >> 1;
> 		if(mid*(mid - 1ll) <= tar)
> 		{
> 			res = mid;
> 			L = mid + 1;
> 		}
> 		else R = mid - 1;
> 	}
> 	return res;
> }
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> l >> r;
> 		cout << bisearch(r-l+r-l) << endl;
> 	}
> 	return 0;
> }
> ```


#### [Problem - 2008D - Codeforces](https://codeforces.com/problemset/problem/2008/D)

#graph #combinatorics/permutation

在一个排列 $p$ 中，称从 $i$ 可达 $j$ 当且仅当进行若干次 $i=p_i$ 迭代后 $i=j$. 每个排列中的数字要么被涂成黑色，要么被涂成白色. 令 $F(i)$ 表示从 $i$ 可达的黑色数字数量. 求所有 $F(i)$.

$1\leq t\leq 10^4,1\leq n\leq 2\cdot 10^5,1\leq p_i\leq n$

从某个数字开始，不断迭代，最终一定会形成一个环，环内所有数字的 $F(i)$ 是相同的. 所以只需找出所有环，并计算每个环里面的黑色数字个数即可. 

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int t, n, p[maxn + 5];
> string str;
> bool colo[maxn + 5],vis[maxn + 5];
> int head[maxn + 5],ans[maxn + 5];
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n;
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			cin >> p[i];
> 			head[i] = i;
> 			vis[i] = 0;ans[i] = 0;
> 		}
> 		cin >> str;
> 		for(int i = 0;i < n;i ++)
> 		{
> 			if(str[i] == '0') colo[i + 1] = 1;
> 			else colo[i + 1] = 0;
> 		}
> 		
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			if(vis[i] == 0)
> 			{
> 				for(int j = i;vis[j] == 0;j = p[j])
> 				{
> 					if(colo[j] == 1) ans[i] ++;
> 					head[j] = i;
> 					vis[j] = 1;
> 				}
> 			}
> 		}
> 		
> 		for(int i = 1;i <= n;i ++)
> 			cout << ans[head[i]] << ' ';
> 		
> 		cout << endl;
> 	}
> 	return 0;
> }
> ```
#### [Problem - 2008E - Codeforces](https://codeforces.com/problemset/problem/2008/E)

#dp #string #greedy 

交替字符串定义：奇数位字符相同，偶数位字符相同，长度为偶数

给一个字符串，通过下面 2 种操作将其变为交替字符串：

- 删掉其中一个字符，只能用 1 次
- 用任意字符替换其中一个字符，可以使用任意次

问最少操作数

$1\leq t\leq 10^4,1\leq n \leq 2\cdot 10^5$

考虑给定的字符串为偶数位，那么不需要删除操作，只需替换操作. 贪心，奇数位上出现次数最多的作为最终的奇数位字符，偶数位上同理，将其他字符替换为这两种字符即可. 用了排序复杂度 $O(n\log n)$.

考虑给定的字符串为奇数位，必须用一次删除操作，但是在哪个位置删除不确定，如果枚举删除位置复杂度 $O(n^2)$ . 考虑别的办法. 每个位置都有删除和不删除两种选择，考虑 DP ，设 $f(i,a,b,0/1,0/1)$ 表示奇数位为 $a$ ，偶数位为 $b$ ，处理前 $i$ 个字符，第 $i$ 个字符作为偶数位/奇数位，没有使用/使用了删除操作的最小操作数. 转移如下：

- $i$ 为奇数
	- $f(i,a,b,1,0) = f(i - 1,a,b,0,0) + [s_i = a]$
	- $f(i,a,b,0,1) = \min\left(f(i-1,a,b,0,0)+1,f(i-1,a,b,1,1)+[s_i=b]\right)$
- $i$ 为偶数
	- $f(i,a,b,0,0) = f(i - 1,a,b,1,0) + [s_i = b]$
	- $f(i,a,b,1,1) = \min\left(f(i-1,a,b,1,0)+1,f(i-1,a,b,0,1)+[s_i=a]\right)$

时间复杂度 $O(26^2n)$ ，实际实现可略去 $a,b$ 两个维度，否则空间复杂度过大.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int t, n;
> string str,nstr;
> int lcnt[36],odcnt,evcnt;
> int chcntod,chcntev,ans;
> int f[maxn + 5][2][2];
> 
> 
> bool cmp(int x,int y)
> {
> 	return x > y;
> }
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n;
> 		cin >> str;
> 			
> 		if(n&1)
> 		{
> 			ans = maxn + 17;
> 
> 			for(int a = 1;a <= 26;a ++)
> 			{
> 				for(int b = 1;b <= 26;b ++)
> 				{
> 					for(int i = 0;i <= n;i ++)
> 						for(int j = 0;j <= 1;j ++)
> 							for(int k = 0;k <= 1;k ++)
> 								f[i][j][k] = maxn + 5;
> 					f[0][0][0] = 0;
> 					for(int i = 1, ad = 0;i <= n;i ++)
> 					{
> 						if(i & 1)
> 						{
> 							if(str[i - 1] - 'a' + 1 == a) ad = 0;
> 							else ad = 1;
> 							f[i][1][0] = min(f[i][1][0],f[i - 1][0][0] + ad);
> 							f[i][0][1] = min(f[i][0][1],f[i - 1][0][0] + 1);
> 							if(str[i - 1] - 'a' + 1 == b) ad = 0;
> 							else ad = 1;
> 							f[i][0][1] = min(f[i][0][1],f[i - 1][1][1] + ad);
> 						}
> 						else
> 						{
> 							if(str[i - 1] - 'a' + 1 == b) ad = 0;
> 							else ad = 1;
> 							f[i][0][0] = min(f[i][0][0],f[i - 1][1][0] + ad);
> 							f[i][1][1] = min(f[i][1][1],f[i - 1][1][0] + 1);
> 							if(str[i - 1] - 'a' + 1 == a) ad = 0;
> 							else ad = 1;
> 							f[i][1][1] = min(f[i][1][1],f[i - 1][0][1] + ad);
> 						}
> 					}
> 					//cout << char(a + 'a' - 1) << ' ' << char(b + 'a' - 1) << ' ' << f[n][0][1] << endl;
> 					ans = min(ans, f[n][0][1]);
> 				}
> 			}
> 			cout << ans << endl;
> 		}
> 		else
> 		{
> 			odcnt = evcnt = 0;
> 			for(int i = 1;i <= 26;i ++) lcnt[i] = 0;
> 			for(int i = 1;i <= n;i += 2)
> 			{
> 				lcnt[str[i - 1] - 'a' + 1] ++;
> 				odcnt ++;
> 			}
> 			sort(lcnt + 1,lcnt + 26 + 1,cmp);
> 			chcntod = odcnt - lcnt[1];
> 			for(int i = 1;i <= 26;i ++) lcnt[i] = 0;
> 			for(int i = 2;i <= n;i += 2)
> 			{
> 				lcnt[str[i - 1] - 'a' + 1] ++;
> 				evcnt ++;
> 			}
> 			sort(lcnt + 1,lcnt + 26 + 1,cmp);
> 			chcntev = evcnt - lcnt[1];
> 			cout << chcntod + chcntev << endl;
> 		}
> 			
> 	}
> 	return 0;
> }
> ```

#### [Problem - 2008F - Codeforces](https://codeforces.com/problemset/problem/2008/F)

#probabilty/expectation #combinatorics #math #prefix_sum 

盒子里有 $n$ 个小球，每个小球都有自己的价值，求从盒子里随机取两个小球的价值乘积的期望.
$1\leq t\leq 10^4,2\leq n\leq 2\cdot 10^5,0\leq a_i\leq 10^9$

答案如下
$$
\dfrac{1}{tot}\sum_{i=1}^n\sum_{j=i+1}^na_ia_j
$$

$tot = \binom{n}{2}$

时间复杂度 $O(n^2)$

变换一下，得到

$$
\dfrac{1}{tot} \sum_{i=1}^n a_i\sum_{j = i + 1}^n a_j
$$

可以用前缀和预处理，时间复杂度降为 $O(n)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const ll modn = 1e9 + 7;
> const int maxn = 2e5;
> int t;
> ll n, a[maxn + 5],psum[maxn + 5], ans, inv, tot;
> 
> ll qpow(ll x,ll y)
> {
> 	ll res = 1;
> 	for(;y;y >>= 1)
> 	{
> 		if(y&1) res = ((res%modn) * (x%modn)) % modn;
> 		x = ((x%modn) * (x%modn))%modn;
> 	}
> 	return res%modn;
> }
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n;ans = 0;
> 		tot = n*(n - 1)/2;
> 		inv = qpow(tot, modn - 2)%modn;
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			cin >> a[i];
> 			psum[i] = (psum[i - 1] + a[i])%modn;
> 		}
> 		for(int i = 2;i <= n;i ++)
> 			ans = (ans%modn + (((psum[i - 1]%modn) * (a[i]%modn))%modn * (inv%modn))%modn)%modn;
> 		cout << ans%modn << endl;
> 	}
> 	return 0;
> }
> ```


---


#### [Problem - 2004D - Codeforces](https://codeforces.com/problemset/problem/2004/D)

- [x] [Problem - 2004D - Codeforces](https://codeforces.com/problemset/problem/2004/D)

#binary-search #math/classification #greedy 

有 $n$ 个城市排成一排，标号 1 到 n. 城市之间用传送门移动，传送门有 4 种颜色：蓝、绿、红、黄. 每座城市都有 2 个传送门. 当两个城市 $i,j$ 之间有相同的传送门时，可以互相抵达，花费 $|i-j|$. 有 $q$ 次询问，计算从城市 $x$ 到 城市 $y$ 的最小花费.

$1\leq t\leq 10^4,1\leq n,q \leq 2\cdot 10^5,1 \leq x, y\leq n$

$|x-y| \leqslant |x-k|+|y-k|$ 这是显然. 所以当 $x,y$ 可以互相抵达时，就直接通过传送门即可，花费 $|x-y|$

$x,y$ 不可以互相抵达时，尝试寻找中介. 在考虑中介的时候，我们知道，在当前讨论环境下，$x,y$ 所有的传送门一定包含了 4 种颜色，于是我们只需要找包含 $x$ 中一种颜色和 $y$ 中一种颜色的城市作为中介. 中介只需 1 个，多了有时反而不优.

在符合条件的中介中，最好的情况是落在 $x,y$ 中间，花费 $|x-y|$.

其次就是最靠近 $x$ 或最靠近 $y$ .

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int inf = 1e9 + 7;
> const int maxn = 2e5;
> int t, n, q, ty[maxn + 5], x, y, mid, ans;
> string pt[maxn + 5];
> // 1   2   3    4   5   6
> // BG, BR, BY, GR, GY, RY
> vector< int > med[10];
> string di[10] = {"","BG", "BR", "BY", "GR", "GY", "RY"};
> 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n >> q;
> 		for(int i = 1;i <= 6;i ++) med[i].clear();
> 		for(int i = 1;i <= n;i ++)
> 		{
> 			cin >> pt[i];
> 			for(int j = 1;j <= 6;j ++)
> 				if(pt[i] == di[j])
> 				{
> 					ty[i] = j;
> 					break;
> 				}
> 			med[ty[i]].push_back(i);
> 		}
> 		
> 		for(int qq = 1;qq <= q;qq ++)
> 		{
> 			cin >> x >> y;
> 			if(pt[x][0] == pt[y][0] || pt[x][0] == pt[y][1] || pt[x][1] == pt[y][0] || pt[x][1] == pt[y][1])
> 				cout << abs(x - y) << endl;
> 			else
> 			{
> 				mid = (x + y) >> 1;
> 				ans = inf;
> 				for(int i = 1;i <= 6;i ++)
> 				{
> 					if((pt[x][0] == di[i][0] || pt[x][0] == di[i][1] || pt[x][1] == di[i][0] || pt[x][1] == di[i][1])&&(pt[y][0] == di[i][0] || pt[y][0] == di[i][1] || pt[y][1] == di[i][0] || pt[y][1] == di[i][1]))
> 					{
> 					    auto lb = lower_bound(med[i].begin(),med[i].end(),x);
> 					    if(lb != med[i].end()) ans = min(abs(x - *lb) + abs(y - *lb), ans);
> 					    if(lb != med[i].begin())
> 					    {
> 					        lb --;
> 					        ans = min(abs(x - *lb) + abs(y - *lb), ans);
> 					    }
> 					    lb = lower_bound(med[i].begin(),med[i].end(),y);
> 					    if(lb != med[i].end()) ans = min(abs(x - *lb) + abs(y - *lb), ans);
> 					    if(lb != med[i].begin())
> 					    {
> 					        lb --;
> 					        ans = min(abs(x - *lb) + abs(y - *lb), ans);
> 					    }
> 					}
> 				}
> 				if(ans == inf) cout << -1 << endl;
> 				else cout << ans << endl;
> 			}
> 		}
> 		
> 	}
> 	return 0;
> }
> ```


#### [Problem - 2004C - Codeforces](https://codeforces.com/problemset/problem/2004/C)

 - [x] [Problem - 2004C - Codeforces](https://codeforces.com/problemset/problem/2004/C)
#greedy 

有 $n$ 张牌，Alice 和 Bob 轮流取，Alice 先手，每人每轮取 1 张，每张牌上有一个分数 $a_i$ ，最终游戏分数记为二者得分之差 $A-B$. Alice 想让游戏分数尽可能大，Bob 则想让游戏分数尽可能小，二者都采取最优策略. 现在 Bob 可以作弊，只能增加某些牌的分数，而且增加的总分数不超过 $k$ ，问采取最优作弊策略，Bob 可以让游戏分数最小到多少.

$1\leq t\leq 5000, 2\leq n \leq 2\cdot 10^5,0\leq k\leq 10^9,1\leq a_i\leq 10^9$

如果不作弊，那么两人均采用贪心策略，将牌从大到小排序，Alice 将取走排在奇数位的牌，Bob 将取走排在偶数位的牌.

考虑作弊，对某个牌加上一定分数后，其在牌堆中的位置可能会发生改变（在保证从大到小排序的前提下）. 作弊当然是希望让偶数位牌的分数总和尽可能大.

对偶数位牌加分，如果超过了前一张牌，那么这两张牌的位置会互换，Alice 总会拿走较大的一张. 并且根据贪心，我们可以知道 $A-B\geqslant 0$ ，所以我们能做的只能是让 $A-B$ 尽可能接近于 0. 同时，加同样的分数，加给在前面的偶数牌和加给在后面的偶数牌效果等价，所以我们只需要让偶数位置的牌尽可能接近它们的前一张牌（在奇数位）

时间复杂度 $O(n\log n + n)$ （最复杂反而是排序...）

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int t, n, k, a[maxn + 5], d, A, B; 
> int main()
> {
> 	cin >> t;
> 	for(int tt = 1;tt <= t;tt ++)
> 	{
> 		cin >> n >> k;
> 		for(int i = 1;i <= n;i ++)
> 			cin >> a[i];
> 		sort(a + 1,a + 1 + n);
> 		A = 0;B = 0;
> 		for(int i = n;i >= 2 && k > 0;i -= 2)
> 		{
> 			d = min(a[i] - a[i - 1],k);
> 			a[i - 1] += d;
> 			k -= d;			
> 		}
> 		for(int i = n;i >= 1;i -= 2)
> 		{
> 			A += a[i];
> 			B += a[i - 1];
> 		}
> 		cout << A - B << endl;
> 	}
> 	return 0;
> }
> ```


#### [Problem - 2002B - Codeforces](https://codeforces.com/problemset/problem/2002/B)

- [x] [Problem - 2002B - Codeforces](https://codeforces.com/problemset/problem/2002/B)
#game 

Alice 有一个 $n$ 排列 $[a_1,a_2,...,a_n]$ , Bob 也有一个 $n$ 排列 $[b_1,b_2,...b_n]$, 在每一轮游戏中，以下事件按顺序发生：

- Alice 在她的数组中，取走第一个或最后一个
- Bob 在他的数组中，取走第一个或最后一个

游戏进行 $n-1$ 轮，最后 Alice 剩下 $x$ ，Bob 剩下 $y$ ，如果 $x=y$ 那么 Bob 获胜，否则 Alice 获胜. 如果两个人都以最优策略游戏，谁会获胜.

$1\leq t\leq 10^4, 1\leq n\leq 3\cdot 10^5$

需要从中发现规律. Alice 拿走一个数 $x$ 的时候，Bob 数组中的 $x$ 就没有用了，所以 Bob 应该尽早去掉自己数组中的 $x$ . 而在这个时候，如果 $x$ 不在 Bob 数组的第一个或最后一个，那么 Bob 就必须去掉一个不为 $x$ 的数 $y$ ，而这时候，如果 Alice 数组中还有 $y$ 那么 Alice 可以只留下 $y$ ，Bob 必败.
所以只需要考察：Alice 的数组第一个和最后一个是否也在 Bob 的数组第一个或最后一个出现.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 3e5;
> int a[maxn + 5],b[maxn + 5];
> int n,la,ra,lb,rb;
> bool flag = 1;
> int t;
> 
> int main()
> {
>     cin >> t;
>     for(int tt = 1;tt <= t;tt ++)
>     {
>         cin >> n;flag = 1;
>         for(int i = 1;i <= n;i ++) cin >> a[i];
>         for(int i = 1;i <= n;i ++) cin >> b[i];
>         la = 1;ra = n;lb = 1;rb = n;
>         for(int i = 1;i <= n;i ++)
>         {
>             if(a[la] != b[lb] && a[la] != b[rb])
>             {
>                 flag = 0;break;
>             }
>             if(a[ra] != b[lb] && a[ra] != b[rb])
>             {
>                 flag = 0;break;
>             }
>             if(a[la] == b[lb]) la ++,lb ++;
>             else
>             {
>                 if(a[la] == b[rb]) la ++,rb --;
>                 else
>                 {
>                     if(a[ra] == b[lb]) ra --,lb ++;
>                     else
>                     {
>                         if(a[ra] == b[rb]) ra --,rb --;
>                     }
>                 }
>             }
>         }
>         if(flag) cout << "Bob" << endl;
>         else cout << "Alice" << endl;
>     }
>         
>     return 0;
> }
> ```
