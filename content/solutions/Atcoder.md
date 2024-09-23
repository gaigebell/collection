[toc]


## ABC372

#### [C - Count ABC Again (atcoder.jp)](https://atcoder.jp/contests/abc372/tasks/abc372_c)

#string #property 

给你一个长度为 $N$ 的字符串 $S$ . 回答 $Q$ 次询问.

第 $i$ 次询问形如下

- 给一个整数 $X_i$ 和字符 $C_i$ ，将 $S$ 中第 $X_i$ 个字符替换成 $C_i$ . 然后输出现在 $S$ 中子串 `ABC` 的个数.

$3\leq N \leq 2\times 10^5$

$1\leq Q \leq 2\times 10^5$

改变一个字符，只会影响 $[X_i - 2,X_i], [X_i-1,X_i+1],[X_i,X_i+2]$ 这 3 个区间. 

先统计一遍 `ABC` 的个数，然后询问时考察这 3 个区间，对应修改答案即可.

时间复杂度 $O(Q)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int N, Q, ans, x;
> string str;
> char ch[maxn + 5], cc;
> 
> int main()
> {
> 	cin >> N >> Q;
> 	cin >> str;
> 	for(int i = 0;i < str.size();i ++)
> 		ch[i + 1] = str[i];
> 	for(int i = 1;i + 2 <= N;i ++)
> 		if(ch[i] == 'A' && ch[i + 1] == 'B' && ch[i + 2] == 'C')
> 			ans ++;
> 	for(int qq = 1;qq <= Q;qq ++)
> 	{
> 		cin >> x >> cc;
> 		if(x > 2)
> 		{
> 			if(ch[x - 2] == 'A' && ch[x - 1] == 'B')
> 			{
> 				if(cc != 'C' && ch[x] == 'C') ans --;
> 				else
> 				{
> 					if(cc == 'C' && ch[x] != 'C') ans ++;
> 				}
> 			}
> 		}
> 		if(x + 2 <= N)
> 		{
> 			if(ch[x + 1] == 'B' && ch[x + 2] == 'C')
> 			{
> 				if(cc != 'A' && ch[x] == 'A') ans --;
> 				else
> 				{
> 					if(cc == 'A' && ch[x] != 'A') ans ++;
> 				}
> 			}
> 		}
> 		if(x > 1 && x < N)
> 		{
> 			if(ch[x - 1] == 'A' && ch[x + 1] == 'C')
> 			{
> 				if(cc != 'B' && ch[x] == 'B') ans --;
> 				else
> 					if(cc == 'B' && ch[x] != 'B') ans ++;
>  			}
> 		}
> 		ch[x] = cc;
> 		cout << ans << endl;
> 	}
> 	return 0;
> }
> ```


#### [D - Buildings (atcoder.jp)](https://atcoder.jp/contests/abc372/tasks/abc372_d)

#ds/segment_tree 

有 $N$ 个建筑排成一排，第 $i$ 个建筑高 $H_i$

对于每个 $i$ 找到满足以下条件的 $j\,(i < j\leq N)$ 的个数

- $i,j$ 之间没有建筑比 $j$ 高.

$1\leq N \leq 2\times 10^5$

$1\leq H_i \leq N$

$H_i \neq H_j (i \neq j)$

考虑某栋大楼 $j$ 对其他大楼答案的贡献. $j$ 对其后面的大楼没有贡献，只对其前面的大楼有贡献. 

$j$ 只对其前面比它矮的大楼有贡献，直到碰到一个比它高的大楼，就不再对更前面的大楼产生贡献.

于是 $j$ 会对一个区间的大楼的答案产生 +1 的贡献. 我们只需要找到在它前面第一个比它高的大楼.

使用权值线段树，大楼高度作为定义域，位置作为值域. 从前往后枚举大楼，先查询其高度以上的最大的位置在哪，然后用差分标记或线段树记录答案. 接着再将当前大楼加入权值线段树.

时间复杂度 $O(N\log N)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int N, h[maxn + 5], pre[maxn + 5],ans[maxn + 5];
> int tr[(maxn << 2) + 5];
> 
> int lson(int x)
> {
> 	return x<<1;
> }
> 
> int rson(int x)
> {
> 	return lson(x) + 1;
> }
> 
> void Modify(int k,int L,int R,int pos,int val)
> {
> 	if(L > pos || R < pos) return ;
> 	if(L == pos && L == R)
> 	{
> 		tr[k] = val;
> 		return ;
> 	}
> 	int mid = (L + R) >> 1;
> 	Modify(lson(k),L,mid,pos,val);
> 	Modify(rson(k),mid + 1,R,pos,val);
> 	tr[k] = max(tr[lson(k)],tr[rson(k)]);
> 	return ;
> }
> 
> int Query(int k,int L,int R,int l,int r)
> {
> 	if(L > r || R < l) return 0;
> 	if(l <= L && R <= r) return tr[k];
> 	int mid = (L + R) >> 1;
> 	int res = max(Query(lson(k),L,mid,l,r),Query(rson(k),mid + 1,R,l,r));
> 	return res;
> }
> 
> int main()
> {
> 	cin >> N;
> 	for(int i = 1;i <= N;i ++)
> 		cin >> h[i];
> 	Modify(1,1,N+1,N+1,1);
> 	for(int i = 2,j = 0;i <= N;i ++)
> 	{
> 		j = Query(1,1,N+1,h[i],N+1);
> 		pre[j] ++;
> 		pre[i] --;
> 		Modify(1,1,N+1,h[i],i);
> 	}
> 	for(int i = 1;i <= N;i ++)
> 		ans[i] = ans[i - 1] + pre[i];
> 		
> 	for(int i = 1;i <= N;i ++)
> 		cout << ans[i] << ' ';
> 	return 0;
> }
> ```

#### [E - K-th Largest Connected Components (atcoder.jp)](https://atcoder.jp/contests/abc372/tasks/abc372_e)

#ds/dsu 

有一张无向图，其中有 $N$ 个点，但是没有边.

给你 $Q$ 个询问，询问如下：

- 类型 1 格式 `1 u v`. 在 $u$ 和 $v$ 之间加 1 条边.
- 类型 2 格式 `2 u k`. 输出与 $u$ 联通的所有点中，第 $k$ 大的点. 如果少于 $k$ 个点，输出 -1

$1\leq N, Q\leq 2\times 10^5$

类型 1 $1\leq u < v \leq N$

类型 2 $1\leq v \leq N, 1\leq k \leq 10$

一个值得关注的点，在于数据范围 $1\leq k \leq 10$ . 也就是说，我们只需要维护一个连通块里面排名前 10 的点就可以了.

这里不必用图联通算法，使用并查集就可以确定节点在哪个连通块中.

当两个连通块合并的时候，两边排名前 10 的点组成的一定合并后排名前 20 的点，暴力排序后去掉最后 10 个，留下最大的 10 个即可.

时间复杂度: 合并 $O(N + 20\log 20)$ 路径压缩后时间复杂度更小一些, 回答 $O(1)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 2e5;
> int N, Q, ty, u, v;
> int fa[maxn + 5];
> vector< int > big[maxn + 5];
> 
> int findx(int x)
> {
>     if(fa[x] != x) fa[x] = findx(fa[x]);
>     return fa[x];
> }
> 
> bool judge(int x,int y)
> {
>     x = findx(x);
>     y = findx(y);
>     return x == y;
> }
> 
> void joint(int x,int y)
> {
>     x = findx(x);
>     y = findx(y);
>     fa[y] = x;
>     return ;
> }
> 
> bool cmp(int x,int y)
> {
>     return x > y;
> }
> 
> int main()
> {
>     cin >> N >> Q;
>     for(int i = 1;i <= N;i ++)
>     {
>         fa[i] = i;
>         big[i].push_back(i);
>     }
>     for(int qq = 1;qq <= Q;qq ++)
>     {
>         cin >> ty;
>         if(ty == 1)
>         {
>             cin >> u >> v;
>             if(!judge(u,v))
>             {
>                 int fu = findx(u);
>                 int fv = findx(v);
>                 for(int i = 0;i < big[fv].size();i ++)
>                     big[fu].push_back(big[fv][i]);
>                 sort(big[fu].begin(),big[fu].end(),cmp);
>                 for(;big[fu].size() > 10;) big[fu].pop_back();
>                 big[fv].clear();
>                 joint(u,v);
>             }
>         }
>         else{
>             cin >> u >> v;
>             int fu = findx(u);
>             //cout << "it belogs to " << fu << endl;
>             if(big[fu].size() < v) cout << -1 << endl;
>             else cout << big[fu][v - 1] << endl;
>         }
>     }
>     return 0;
> }
> ```



#### [F - Teleporting Takahashi 2 (atcoder.jp)](https://atcoder.jp/contests/abc372/tasks/abc372_f)

#graph #dp #optimization #key 

有一张有向图 $G$ , 有 $N$ 个顶点和 $N + M$ 条边，其中 $i$ 和 $i + 1$ （包含 $N$ 和 $1$）之间都有一条边. 剩下的 $M$ 条边从 $X_i$ 指向 $Y_i$ . Takahashi 在顶点 1 ，每次可以走 1 条边，问走了恰好 $K$ 次有多少种不同的走法.

$2\leq N \leq 2\times 10^5$

$0\leq M \leq 50$

$1\leq K \leq 2\times 10^5$

所有边各不相同

值得注意的是 $M\leq 50$. 考虑怎么利用这个条件.

**inline DP**

官方给了一种 inline DP 的做法. 大致意思就是，走到第 $i$ 个点的方案数 $f(i)$ 一定会贡献给 $f(i+1)$. 可以直接移动下标就不用对 $f$ 重新赋值了. 

那么这样的话，我们首先需要一个长度 $N+K$ 的 $f$.

从 $K + 1$ 开始，$[K+1,N+K]$ 这一段代表了初始的 $[1,N]$ 的 $f$ . 那么 $f(K + 1) = 1$ . 接着由于 $f(i)$ 要贡献给 $f(i + 1)$ . 我们直接取 $[K,N+K-1]$ 的 $f$ 代表走了 1 次的各个顶点的方案数. 这样以此类推， $[K + 1 - i,N + K - i]$ 表这一段的 $f$ 表示走了 $i$ 次的各个顶点的方案数.

至于另外 $M$ 条边产生的贡献，先记录下来到 $g$，然后分别加到下一轮对应下标的 $f$ 处便可完成转移. 这样 DP 的时间复杂度只有 $O(MK)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> const ll modn = 998244353;
> int N, M, K;
> ll f[maxn + maxn + 5], ans = 0;
> int ex_x[55], ex_y[55];
> map<int, ll> g;
> 
> 
> int main()
> {
>     cin >> N >> M >> K;
>     for(int i = 1;i <= M;i ++)
>         cin >> ex_x[i] >> ex_y[i];
>     M ++;
>     ex_x[M] = N;ex_y[M] = 1;
>     f[K + 1] = 1ll;
>     for(int i = K + 1;i > 1;i --)
>     {
>         for(int j = 1, u = 0, v = 0;j <= M;j ++)
>         {
>             u = i + ex_x[j] - 1;
>             v = ex_x[j];
>             g[v] = f[u];
>         }
>         for(int j = 1, u = 0, v = 0;j <= M;j ++)
>         {
>             u = ex_x[j];
>             v = i - 1 + ex_y[j] - 1;
>             f[v] = (f[v]%modn + g[u]%modn)%modn;
>         }
>     }
>     for(int i = 1;i <= N;i ++)
>         ans = (ans%modn + f[i]%modn)%modn;
>     cout << ans%modn << endl;
>     return 0;
> }
> ```

**contract graph**

官方还给了另一种做法，就是将度数为 2 的点全部去掉，相当于压缩单向无分叉的路径. 

因为在没有分叉的点 $f(i)$ ，只是负责将 $f(i-1)$ 传递给 $f(i + 1)$ ，一定有 $f(i) = f(i - 1)$.

那么这样的话，剩下的最多也就只有 100 条边. 时间复杂度应该是 $O(2MK)$

> [!important] Key
> 
> - 利用条件 $M\leq 50$
> - 循环轮换的性质怎么利用
> - 开开眼界


---

## ABC371

#### [A - Jiro (atcoder.jp)](https://atcoder.jp/contests/abc371/tasks/abc371_a)

有三兄弟 $A,B,C$ ，给你他们之间的大小关系 $S_{AB}, S_{AC}, S_{BC}$ （要么是 `<`, 要么是 `>`）

找到老二.

穷举所有情况即可知道每种情况对应的答案

#### [B - Taro (atcoder.jp)](https://atcoder.jp/contests/abc371/tasks/abc371_b)

有 $N$ 个家庭 $M$ 个婴儿，按出生时间顺序输入，只有每个家庭第一个出生的男婴才能叫 Taro. 判断每位婴儿是否叫 Taro

对所有家庭第一个出现的男婴输出 `YES` ，然后打上标记，此后的本家庭男婴均为 `NO`.

女婴直接输出 `NO` 即可.

#### [C - Make Isomorphic (atcoder.jp)](https://atcoder.jp/contests/abc371/tasks/abc371_c)

#graph #enumerate 

给两个简单无向图 $G$ 和 $H$ ，两张图都有 $N$ 个点. 你可以对 $H$ 进行任意次以下操作

- 选一对整数 $(i,j)$ 满足 $1\leq i < j\leq N$ . 花费 $A_{i,j}$ 元，如果在 $H$ 中 $i,j$ 之间没有边，那么就加上这条边，否则就去掉这条边.

找到使得 $G$ 和 $H$ 同构的最小花费. 

$1\leq N\leq 8$

让 $G$ 不动，设顶点集为 ${\cal G} = \{1,2,3,...,N\}$. 考虑 $H$ 的顶点所有可能的排列，${\cal H} = \{p_1,p_2,...,p_N\}$. 然后让 $G$ 中第 $i$ 个顶点和第 $j$ 个顶点对应第 $p_i$ 个顶点和第 $p_j$ 个顶点. 那么方便来说就要考虑用邻接矩阵. $g(i,j) \neq h(p_i,p_j)$ 就需要产生花费. 枚举所有可能的情况，取花费总和最小的作为答案.

时间复杂度 $O(N!\cdot N^2)$

代码中包含了 `next_permutation` 的使用

> [!code]- Code
> ```cpp
> #include <bits/stdc++.h>
> 
> using namespace std;
> int N, u, v, MG, MH, ans = 1000000007;
> bool G[10][10], H[10][10];
> int cost[10][10], p[10];
> 
> 
> int main()
> {
>     cin >> N;
>     cin >> MG;
>     for(int i = 1;i <= MG;i ++)
>     {
>         cin >> u >> v;
>         G[u][v] = 1;
>         G[v][u] = 1;
>     }
>     cin >> MH;
>     for(int i = 1;i <= MH;i ++)
>     {
>         cin >> u >> v;
>         H[u][v] = 1;
>         H[v][u] = 1;
>     }
>     for(int i = 1;i <= N;i ++)
>     {
>         for(int j = i + 1;j <= N;j ++)
>         {
>             cin >> cost[i][j];
>             cost[j][i] = cost[i][j];
>         }
>     }
>     for(int i = 1;i <= N;i ++) p[i] = i;
>     do
>     {
>         int sum = 0;
>         for(int i = 1;i <= N;i ++)
>         {
>             for(int j = 1;j < i;j ++)
>             {
>                 if(G[i][j] != H[p[i]][p[j]])
>                 {
>                     sum += cost[p[i]][p[j]];
>                 }
>             }
>         }
>         ans = min(ans, sum);
>     } while (next_permutation(p + 1,p + 1 + N));
>     cout << ans << endl;
>     return 0;
> }
> ```


#### [D - 1D Country (atcoder.jp)](https://atcoder.jp/contests/abc371/tasks/abc371_d)

#prefix_sum #binary-search 


有 $N$ 个村庄在一条线上，第 $i$ 个村庄在 $X_i$, 有 $P_i$ 个村民.

回答 $Q$ 次询问，每次询问问在坐标 $L_i,R_i$ 之间，生活着多少村民.

$1\leq N, Q\leq 2\times 10^5$

$-10^9\leq X_1 < X_2 < ... < X_N \leq 10^9$

$1\leq P_i \leq 10^9$

二分出最靠近 $L_i,R_i$ 的村庄 $l,r$ .

$S_r - S_{l-1}$ 即为答案. $S_i$ 表示前 $i$ 个村庄的总人口.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> ll N, Q, LQ, RQ;
> ll x[maxn + 5],p[maxn + 5],sum[maxn + 5], lp, rp;
> 
> int bisearch(ll pos,bool ty)
> {
>     int L = 1;int R = N;int mid = 0;int res = 0;
>     if(ty) res = N;
>     else res = 1;
>     for(;L <= R;)
>     {
>         mid = (L + R) >> 1;
>         if(ty)
>         {
>             if(pos >= x[mid])
>             {
>                 res = mid;
>                 L = mid + 1;
>             }
>             else R = mid - 1;
>         }
>         else{
>             if(pos <= x[mid])
>             {
>                 res = mid;
>                 R = mid - 1;
>             }
>             else L = mid + 1;
>         }
>     }
>     return res;
> }
> 
> int main()
> {
>     cin >> N;
>     for(int i = 1;i <= N;i ++)
>         cin >> x[i];
>     for(int i = 1;i <= N;i ++)
>     {
>         cin >> p[i];
>         sum[i] = sum[i - 1] + p[i];
>     }
>     cin >> Q;
>     for(int i = 1;i <= Q;i ++)
>     {
>         cin >> LQ >> RQ;
>         if(RQ < x[1] || LQ > x[N]) cout << 0 << endl;
>         else{
>             lp = bisearch(LQ,0);
>             rp = bisearch(RQ,1);
>             cout << sum[rp] - sum[lp - 1] << endl;
>         }
>         
>     }
>     
> 
>     return 0;
> }
> ```



#### [E - I Hate Sigma Problems (atcoder.jp)](https://atcoder.jp/contests/abc371/tasks/abc371_e)

#ds/segment_tree #key 

给一个长度为 $N$ 的整数序列 $A = (A_1,A_2,...,A_N)$

定义 $f(l,r)$ 为在 $(A_{l},A_{l+1},...,A_{r})$ 中不同元素的个数.

求

$$
\sum_{i=1}^{N}\sum_{j=i}^{N} f(i,j)
$$

$1\leq N \leq 2\times 10^5$

$1\leq A_i \leq N$


考虑一个整数序列 $(A_1,A_2,...,A_{i-1})$ 加入一个新元素 $A_i$ ，$\forall \, 1\leq l\leq i,\,f(l,i)$ 会怎么变化.

假设里 $A_i$ 最近的相同元素为 $A_k$， 那么当 $A_i$ 加进来的时候

- $\forall\,k<l\leq i,\,f(l,i) = f(l,i-1)+1$

- $\forall\, 1\leq l\leq k,\,f(l,i) = f(l,i - 1)$

于是我们可以用 $g(j)$ 表示 $(A_j,...,A_i)$ 的不同元素的个数. 对 $[k+1,i]$ 区间的 $g(j)$ 都加 1. 其他自然不需变. 

然后每次新加一个元素就对应操作，然后对答案加上全体和. 用线段树维护 $g(j)$ 的区间和.

时间复杂度 $O(N\log N)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> ll N, lst[maxn + 5], a[maxn + 5];
> ll ans, tr[(maxn << 2) + 5], tag[(maxn << 2) + 5];
> 
> int lson(int x)
> {
>     return x << 1;
> }
> 
> int rson(int x)
> {
>     return lson(x) + 1;
> }
> 
> void Pushdown(int k,int L,int mid,int R)
> {
>     tag[lson(k)] += tag[k];
>     tag[rson(k)] += tag[k];
>     tr[lson(k)] += tag[k]*(mid - L + 1);
>     tr[rson(k)] += tag[k]*(R - mid);
>     tag[k] = 0;
>     return ;
> }
> 
> void Add(int k,int L,int R,int l,int r)
> {
>     if(L > r || R < l) return ;
>     if(l <= L && R <= r)
>     {
>         tag[k] ++;
>         tr[k] += (R - L + 1);
>         return ;
>     }
>     int mid = (L + R) >> 1;
>     Pushdown(k,L,mid,R);
>     Add(lson(k),L,mid,l,r);
>     Add(rson(k),mid + 1,R,l,r);
>     tr[k] = tr[lson(k)] + tr[rson(k)];
>     return ;
> }
> 
> ll Query(int k,int L,int R,int l,int r)
> {
>     if(L > r || R < l) return 0;
>     if(l <= L && R <= r) return tr[k];
>     int mid = (L + R) >> 1;
>     Pushdown(k,L,mid,R);
>     ll res = Query(lson(k),L,mid,l,r) + Query(rson(k),mid + 1,R,l,r);
>     tr[k] = tr[lson(k)] + tr[rson(k)];
>     return res;
> }
> 
> int main()
> {
>     cin >> N;
>     for(int i = 1;i <= N;i ++)
>         cin >> a[i];
>     for(int i = 1;i <= N;i ++)
>     {
>         Add(1,1,N,lst[a[i]] + 1,i);
>         ans += Query(1,1,N,1,i);
>         lst[a[i]] = i;
>     }
> 
>     cout << ans << endl;
>     return 0;
> }
> ```

> [!important] Key
> 
> $O(N^3)$ 肯定是超时的，考察变化过程中的规律，减少运算是关键
> 
> 类似差分的思想，$f(l,i) - f(l,i-1)=1$
> 
> 还有对“种类”的理解，第一次出现才是新种类，才会对答案造成影响.
> 
> 类似的思想与题目
> 
> - [[Binary Index Tree - BIT#离线解题技巧]]

**官方做法**

#combinatorics/complementary_set

答案将等于

$$
\begin{aligned}
&\sum_{i=1}^N\sum_{j=i}^N\sum_{k=1}^N[(A_i,...,A_j)包含k]\\[2ex]
=&\sum_{k=1}^{N}\sum_{i=1}^N\sum_{j=i}^N[(A_i,...,A_j)包含k]\\[2ex]
=&\sum_{k=1}^{N}包含k的(A_i,...,A_j)的数量\\[2ex]
=&\sum_{k=1}^{N}\left(子序列总数-不包含k的(A_i,...,A_j)的数量\right)
\end{aligned}
$$

设 $A_x=k$ 中的 $x$ 从小到大为 $x_1,x_2,...x_{C_k}$ 其中 $C_k$ 为 $k$ 的个数，不包含 $k$ 的 $(A_i,...,A_j)$ 的数量为 $cnt(0,x_1)+cnt(x_1,x_2)+\cdots+cnt(x_{C_k},N+1)$

计算组合数 

$$
cnt(x_i,x_{i + 1}) = \binom{x_{i+1}-x_i - 1}{2} + x_{i+1}-x_i-1
$$

时间复杂度为 $O(1)$

总复杂度为 $O(N)$

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> ll N, a[maxn + 5];
> ll ans, res, tot;
> vector< ll > pos[maxn + 5];
> 
> int main()
> {
>     cin >> N;
>     for(int i = 1;i <= N;i ++)
>         pos[i].push_back(0);
> 
>     for(int i = 1;i <= N;i ++)
>     {
>         cin >> a[i];
>         pos[a[i]].push_back(i);
>     }
>     
>     for(int i = 1;i <= N;i ++)
>         pos[i].push_back(N + 1);
> 
>     tot = N * (N - 1) / 2 + N;
> 
>     for(int i = 1;i <= N;i ++)
>     {
>         res = 0;
>         for(int j = 1;j < pos[i].size();j ++)
>         {
>             res = res + (pos[i][j] - pos[i][j - 1] - 1) * (pos[i][j] - pos[i][j - 1] - 2) / 2 + (pos[i][j] - pos[i][j - 1] - 1);
>         }
>         ans += (tot - res);
>     }
> 
>     cout << ans << endl;
>     return 0;
> }
> ```


---

## ABC370
#### [A - Raise Both Hands (atcoder.jp)](https://atcoder.jp/contests/abc370/tasks/abc370_a)

给两个数 $L,R$ ，$L=1$ 时输出 `Yes` ， $R = 1$ 时输出 `No` . 如果 $L,R$ 同时为 1 或 0 输出 `Invalid` .

$L,R$ 只可能是 0 或 1 .

读懂题的时候就知道怎么做了.

#### [B - Binary Alchemy (atcoder.jp)](https://atcoder.jp/contests/abc370/tasks/abc370_b)

#simulation 

有 $N$ 种元素，分别是 $1,2,...,N$ . 

元素之间可以结合，$i$ 和 $j$ 结合就会变成元素 $A_{\max(i,j),\min(i,j)}$ 

从元素 1 开始，按顺序与 $1,2,...,N$ 结合，求最终得到的元素.

$1\leq N\leq 100,1\leq A_{i,j} \leq N$

设第 $i$ 次结合得到的元素为 $x_i$

$$
x_i = A_{\max(i,x_{i-1}),\min(i,x_{i-1})}
$$

$x_0 = 1$. 直接模拟即可.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> int N,ans;
> int A[105][105];
> 
> int main()
> {
>   cin >> N;
>   for(int i =1 ;i <= N;i ++)
>     for(int j = 1;j <= i;j ++)
>       cin >> A[i][j];
>   ans = 1;
>   for(int i = 1;i <= N;i ++)
>   {
>     ans = A[max(ans,i)][min(ans,i)];
>   }
>   cout << ans;
>   return 0;
> }
> ```

#### [C - Word Ladder (atcoder.jp)](https://atcoder.jp/contests/abc370/tasks/abc370_c)

#greedy #string 

给两个只有小写字母的字符串 $S$ 和 $T$，  它们长度相同.

让 $X$ 为一个空数组，重复下列步骤直到 $S = T$ ：

- 改变 $S$ 中的一个字符，并将 $S$ 追加入数组 $X$ 中.

找到长度最小的数组 $X$ . 如果有多个答案，输出字典序最小的答案.

$1\leq |S| = |T| \leq 100$

相等的字符不需要改变，所以 $X$ 的长度就是这俩字符串有着不同字符的位置的个数. 现在考虑怎样构造字典序最小的答案.

$S,T$ 中相等的字符不会产生影响，考虑不同字符带来的影响.

需要改变的字符只有以下两种情况.
1. $S_i < T_i$
2. $S_i > T_i$

第二种情况使得 $S$ 的字典序变小，所以我们应当先对第二种情况进行操作. 在对第二种情况进行操作的时候，我们应当按它们在 $S$ 中的出现顺序进行操作.

处理完第二种情况之后，再去处理第一种情况. 这时由于这些字符字典序要变大，所以我们尽可能先让后面的发生改变，位于前面的字符尽可能晚地发生改变，所以我们应当逆着它们在 $S$ 中出现顺序的进行操作.

以上贪心策略即可构造出字典序最小的答案. 代码写得又臭又长 QwQ.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> const int maxn = 100;
> string S,T;
> bool vis[maxn + 5];
> int n;
> vector< int > rec;
> char ch[maxn + 5];
> 
> int main()
> {
>   cin >> S >> T;
>   n = S.size();
>   for(int i = 0;i < n;i ++)
>   {
>     ch[i] = S[i];
>     if(S[i] != T[i])
>     {
>       if(S[i] > T[i])
>       {
>         rec.push_back(i);
>         vis[i + 1] = 1;
>       }
>       else vis[i + 1] = 0;
>     }
>     else vis[i + 1] = 1;
>   }
>   for(int i = n - 1;i >= 0;i --)
>     if(vis[i + 1] == 0)
>       rec.push_back(i);
>   cout << rec.size() << endl;
>   for(int i = 0;i < rec.size();i ++)
>   {
>     ch[rec[i]] = T[rec[i]];
>     for(int j = 0;j < n;j ++)
>       cout << ch[j];
>     cout << endl;
>   }
>   return 0;
> }
> ```

---

## ABC369

#### [A - 369 (atcoder.jp)](https://atcoder.jp/contests/abc369/tasks/abc369_a)
#math 
给两个整数 $A, B$ ，找一个 $x$ 使得这 3 个数可以排成等差数列. 问这样的 $x$ 有多少个.
$1\leq A,B \leq 100$

不妨令 $A\leq B$
1. $x,A,B$ ：$x = A-(B-A)$
2. $A,x,B$ ：$x = \dfrac{A+B}{2}$
3. $A,B,x$ ：$x = B+(B-A)$

第2种要求 $A+B$ 模 2 为 0. 如果 $A=B$ 则 3 种情况下的 $x$ 是相同的.

#### [B - Piano 3 (atcoder.jp)](https://atcoder.jp/contests/abc369/tasks/abc369_b)
#simulation 
Takahashi 将在钢琴上按顺序按下 $N$ 个琴键，第 $i$ 次按下琴键 $A_i$ ，如果此时 $S_i = \text L$ 那么用左手弹下，如果 $S_i = \text R$ 那么用右手弹下. 最开始疲劳程度为 0 ，当他的一只手从琴键 $x$ 移动到 $y$ 时，疲劳值增加 $|y-x|$ . 找到弹这个谱子的最小疲劳值.
$1\leq N\leq 100,1\leq A_i\leq 100$

Takahashi 只能按顺序弹，所以直接计算左手累积的疲劳值加上右手累积的疲劳值即可

#### [C - Count Arithmetic Subarrays (atcoder.jp)](https://atcoder.jp/contests/abc369/tasks/abc369_c)
#math #combinatorics 
给一个有 $N$ 个正整数的序列 $A = (A_1,A_2,...,A_N)$. 计算满足以下要求的整数对 $(l,r)$ 的数量：
$1\leq l\leq r\leq N$ 使得 $(A_l,A_{l + 1},...,A_{r})$ 是等差数列
$1\leq N\leq 2\times 10^5,1\leq A_i \leq 10^9$

🤓☝️知道如果一个序列 $a_1,...,a_n$ 是等差数列，那么 $\forall 1\leq l\leq r\leq n, a_l,...,a_r$ 都是等差数列. 于是我们考虑找到最长的那些等差数列 $(l_1,r_1),(l_2,r_2),...,(l_m,r_m)$ ，显然 $r_1\leq l_2,r_2\leq l_3,...,r_i\leq l_{i + 1}$ ，那么答案的一部分就是
$$
\sum_{i = 1}^m \dfrac{(r_i - l_i + 1)(r_i - l_i)}{2}
$$
这里意思是说，根据上面的性质，我们只要看从 $(l,r)$ 里选两个端点，这俩端点之间的序列一定是等差数列，所以就是计算从 $r-l+1$ 个点里选两个的组合数 $\binom{r-l+1}{2}$ 也就是上面那个形式.
然后再加上长度为 1 的数列个数就是答案了.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> int N,a[maxn + 5],d[maxn + 5];
> ll ans,L,R;
> 
> int main()
> {
>   cin >> N;
>   for(int i = 1;i <= N;i ++)
>     cin >> a[i];
>   if(N == 1)
>   {
>     cout << 1;
>     return 0;
>   }
>   ans = N;
>   for(int i = 2;i <= N;i ++)
>     d[i] = a[i] - a[i - 1];
>   L = 1;R = 2;
>   for(int i = 3;i <= N;i ++)
>   {
>     if(d[i] == d[i - 1])
>     {
>       R = i;
>     }
>     else
>     {
>       ans += (R-L+1)*(R-L)/2;
>       L = i - 1;
>       R = i;
>     }
>   }
>   ans += (R-L+1)*(R-L)/2;
>   cout << ans;
>   return 0;
> }
> ```
> 
> 
#### [D - Bonus EXP (atcoder.jp)](https://atcoder.jp/contests/abc369/tasks/abc369_d)
#dp 
Takahashi 将按顺序遇到 $N$ 只怪物，第 $i$ 只怪物有力量值 $A_i$.
对于每只怪物，他可以选择放走或者打败它.
每次行动得到的经验值情况如下：
- 如果放走这只怪物，他将获得 0 经验值
- 如果他击败这只怪物，这只怪物有力量值 $X$ ，那么他将获得 $X$ 经验值.
- 如果这只怪物是第偶数个被击败的，那么他将额外再获得 $X$ 经验值.
求他可以获得的最大经验值.
$1\leq N\leq 2\times 10^5,1\leq A_i \leq 10^9$

每次面对怪物都有两种选择，将每次面对怪物视为一个阶段，选择作为决策，同时还需要知道击败的这只怪物是第奇数个被击败还是第偶数个被击败. 考虑 DP.
设 $f(i,0/1)$ 表示遇到第 $i$ 只怪物，最后一个被击败的怪物是第偶数个/第奇数个时的最大经验值.

- 放走这只怪物
$$
f(i,0) = \max(f(i,0),f(i-1,0))
$$
$$
f(i,1) = \max(f(i,1),f(i - 1,1))
$$
- 击败这只怪物
$$
f(i,0) = \max(f(i,0),f(i - 1,1) + A_i + A_i)
$$
$$
f(i,1) = \max(f(i,1),f(i - 1,0) + A_i)
$$

$\max\{f(N,0),f(N,1)\}$ 即为答案.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> int N;
> ll a[maxn + 5],f[maxn + 5][5];
> 
> int main()
> {
>   cin >> N;
>   for(int i = 1;i <= N;i ++)
>     cin >> a[i];
>   f[1][1] = a[1];
>   f[1][0] = 0;
>   for(int i = 2;i <= N;i ++)
>   {
>     f[i][1] = max(f[i - 1][0] + a[i],f[i - 1][1]);
>     f[i][0] = max(f[i - 1][1] + a[i] + a[i],f[i - 1][0]);
>   }
>   cout << max(f[N][1],f[N][0]);
>   return 0;
> }
> ```
> 
> 
#### [E - Sightseeing Tour (atcoder.jp)](https://atcoder.jp/contests/abc369/tasks/abc369_e)
#graph/shortest-path/floyd #brute-force #dfs
有 $N$ 个小岛和 $M$ 个连接两个小岛的双向桥. 没有自环，但是两座岛之间有多个桥. 第 $i$ 个桥连接小岛 $U_i$ 和 $V_i$ ，花费时间 $T_i$.
给 $Q$ 个询问，回答这些询问，第 $i$ 个询问内容如下：
现在给 $K_i$ 个不同的桥，$B_{i,1},B_{i,2},...,B_{i,K_i}$， 求出必须经过这些桥的从小岛 $1$ 到小岛 $N$ 的最短用时
你可以以任意方向或顺序穿过这些桥.
$2\leq N\leq 400$
$N-1\leq M\leq 2\times 10^5$
$1\leq U_i < V_i \leq N$
$1\leq T_i \leq 10^9$
$1\leq Q\leq 3000$
$1\leq K_i \leq 5$
$1\leq B_{i,1} < B_{i,2} < ... < B_{i,K_i} \leq M$

题目看着好复杂 TwT... 🤓☝️ $N\leq 400$ ! $K_i \leq 5$ ! $Q\leq 3000$ ! 不妨先用最简单粗暴的方法做一做！
因为这个必须通过的桥最多只有 5 座，那么通过的顺序一共有 $K!=120$ 种，然后又可以从任意方向通过，又有 $2^K=32$ 种通过方式，总共就是 $3840$ 种方式. 而 $Q\leq 3000$ ，那么直接枚举所有可能性是不会超时的. $N\leq 400$ ，可以用 Floyd 先求出所有小岛之间的最短用时，然后枚举所有情况，比如某种情况是 $V_0 = 1,U_1,V_1,U_2,V_2,...,U_K,V_K,U_{K+1}=N$ 那么用时就是 $\sum dis(U_i,V_{i - 1}) + \sum T_i$
在所有情况中取最小值即可.

> [!code]- Code 
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 2e5;
> const ll inf = 1e15 + 7;
> int N,M,Q,K;
> struct bridge{
>   int from;
>   int to;
>   ll ti;
> }b[maxn + 5];
> int a[10],rec[20];
> bool vis[10];
> ll dis[505][505];
> 
> ll solve(int now)
> {
>   ll res = inf;
>   if(now == K + 1)
>   {
>     return dis[rec[K+K]][N];
>   }
>   else
>   {
>      for(int i = 1;i <= K;i ++)
>     {
>       if(!vis[i])
>       {
>         vis[i] = 1;
>         rec[now + now - 1] = b[a[i]].from;
>         rec[now + now] = b[a[i]].to;
>         res = min(res,dis[rec[now+now-2]][rec[now + now - 1]] + b[a[i]].ti + solve(now + 1));
>         rec[now + now - 1] = b[a[i]].to;
>         rec[now + now] = b[a[i]].from;
>         res = min(res,dis[rec[now + now - 2]][rec[now + now - 1]] + b[a[i]].ti + solve(now + 1));
>         vis[i] = 0;
>         rec[now + now - 1] = rec[now + now] = 0;
>       }
>     }
>     return res;
>   }
>   
> }
> 
> int main()
> {
>   cin >> N >> M;
>   for(int i = 1;i <= N;i ++)
>     for(int j = 1;j <= N;j ++)
>     {
>       if(i == j) dis[i][j] = 0;
>       else dis[i][j] = inf;
>     }
>   for(int i = 1;i <= M;i ++)
>   {
>     cin >> b[i].from >> b[i].to >> b[i].ti;
>     dis[b[i].from][b[i].to] = dis[b[i].from][b[i].to] = min(dis[b[i].from][b[i].to],b[i].ti);
>   }
>   for(int k = 1;k <= N;k ++)
>     for(int i = 1;i <= N;i ++)
>       for(int j = i + 1;j <= N;j ++)
>         dis[i][j] = dis[j][i] = min(dis[i][j],dis[i][k] + dis[k][j]);
>   cin >> Q;
>   for(int qq = 1;qq <= Q;qq ++)
>   {
>     cin >> K;rec[0] = 1;
>     for(int i = 1;i <= K;i ++)
>       cin >> a[i];
>     cout << solve(1) << endl;
>   }
>   return 0;
> }
> ```
> 
> 


---

#### [E - Apple Baskets on Circle (atcoder.jp)](https://atcoder.jp/contests/abc270/tasks/abc270_e)
#simulation #sort #binary-search 
有 $N$ 个篮子围成一圈，第 $i$ 个篮子有 $A_i$ 个苹果，从第 1 个篮子开始，如果当前篮子有苹果就吃 1 个，无论有没有吃上苹果，都到下一个篮子去，重复以上步骤. 当恰好吃了 $K$ 个苹果的时候，每个篮子里还剩多少苹果
$1\leq N \leq 10^5, 0\leq A_i \leq 10^12, 1\leq K \leq 10^12, \sum A_i \geq K$

考虑到每个篮子前都会吃 1 个苹果的情况，这个时候所有篮子都有苹果，吃 1 圈就吃掉 $N$ 个苹果，所以当出现空篮子的时候，一定是最少苹果的篮子变空，而这时已经吃了 $A_{min}\times N$ 个苹果. 接着只要把空篮子排除在外，问题又转化为上述情况. 
于是可以考虑先将篮子按苹果的多少排个序. 然后可以吃 $A_{min}\times N + A_{min2}\times(N-1) + \cdots$ 个苹果，直至还要吃的苹果数不够吃空最少苹果的篮子，然后特殊处理，最终还要吃的苹果不够吃1圈剩下还有苹果的篮子，这个时候按顺序排序，逐个逐个吃光要吃的苹果数即可.

> [!code]- Code
> ```cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 1e5;
> int N,pos;
> ll K,acc,m;
> struct basket{
>   ll val;
>   int id;
> }a[maxn + 5];
> 
> bool cmp2(basket x, basket y)
> {
>   return x.id < y.id;
> }
> 
> bool cmp(basket x, basket y)
> {
>   if(x.val != y.val) return x.val < y.val;
>   return x.id < y.id;
> }
> 
> ll bisearch(ll nn,ll R,ll tot)
> {
>   ll L = 0;ll mid = 0;ll res = 0;
>   for(;L <= R;)
>   {
>     mid = (L + R) >> 1;
>     if(mid * nn <= tot)
>     {
>       res = mid;
>       L = mid + 1;
>     }
>     else R = mid - 1;
>   }
>   return res;
> }
> 
> int main()
> {
>   cin >> N >> K;
>   for(int i = 1;i <= N;i ++)
>   {
>     cin >> a[i].val;
>     a[i].id = i;
>   }
>   sort(a + 1,a + 1 + N, cmp);
>   for(ll i = 1, lf = 0;i <= N;i ++)
>   {
>     a[i].val -= acc;
>     lf = N - i + 1;
>     if(lf*a[i].val >= K)
>     {
>       m = bisearch(lf,a[i].val,K);
>       acc += m;
>       a[i].val -= m;
>       for(int j = i + 1;j <= N;j ++) a[j].val -= acc;
>       K -= lf*m;
>       break;
>     }
>     else
>     {
>       K -= lf * a[i].val;
>       acc += a[i].val;
>       a[i].val = 0;
>     }
>   }
>   sort(a + 1,a + 1 + N,cmp2);
>   for(int i = 1;i <= N;i ++)
>     if(a[i].val > 0 && K > 0)
>     {
>       a[i].val --;
>       K --;
>     }
>   for(int i = 1;i <= N;i ++)
>     cout << a[i].val << ' ';
>   return 0;
> }
> ```
> 

#### [D - I Hate Non-integer Number (atcoder.jp)](https://atcoder.jp/contests/abc262/tasks/abc262_d)
#dp #combinatorics 
给一个长度为 $N$ 的数组 $A = (a_1,...,a_N)$, 有 $2^N - 1$ 种方式从 $A$ 中选出一个或更多数. 其中有多少种方案，选出的数的平均数是整数？答案模 998244353
$1\leq N \leq 100, 1\leq a_i \leq 10^9$

考虑到求 $(a_1+...+a_m)\mod m$ 的方案数，这类型的运算满足分配律，可以考察所有组合下对所有可能的模数取模得到的结果的方案数.
具体地说，设 $f(i,j,k,m)$ 表示前 $i$ 个数选了 $j$ 个，模数为 $k$ 结果为 $m$ 的方案数. “前 $i$ 个数选 $j$ 个”明显是求组合选择方案数的经典方法，“模数为 $k$ 结果为 $m$” 则是根据模运算和加法运算结合的性质而想到的. 此时转移方程呼之欲出.
$$
f(i,j,k,(m+a_i)\bmod k) = f(i-1,j,k,(m+a_i)\bmod k) + f(i-1,j-1,k,m)
$$
$O(N^4)$ 不满，可过
类似的题目 [[Leetcode Record#[3260. 找出最大的 N 位 K 回文数](https //leetcode.cn/problems/find-the-largest-palindrome-divisible-by-k/description/)]]

> [!code]- Code
> ```Cpp
> #include<bits/stdc++.h>
> 
> using namespace std;
> typedef long long ll;
> const int maxn = 100;
> const ll modn = 998244353;
> int N;
> ll a[maxn + 5];
> ll f[maxn + 5][maxn + 5][maxn + 5][maxn + 5],ans;
> 
> int main()
> {
>   cin >> N;
>   for(int i = 1;i <= N;i ++) cin >> a[i];
>   // initialization
>   for(int i = 0;i <= N;i ++)
>     for(ll k = 1;k <= N;k ++)
>       f[i][0][k][0] = 1;
> //DP
>   for(int i = 1;i <= N;i ++)
>   {
>     for(int j = 1;j <= i;j ++)
>     {
>       for(ll k = 1;k <= N;k ++)
>       {
>         for(ll m = 0, p = 0;m < k;m ++)
>         {
>           p = (m + a[i])%k;
>           f[i][j][k][p] = (f[i - 1][j - 1][k][m]%modn + f[i - 1][j][k][p]%modn)%modn;
>         }
>       }
>     }
>   }
>   for(int j = 1;j <= N;j ++)
>   {
>     ans = (ans%modn + f[N][j][j][0]%modn)%modn;
>     //cout << f[N][j][j][0] << endl;
>   }
>   cout << ans%modn;
>   return 0;
> }
> ```
> 

### AGC024
#greedy
#### B - Backfront

题意：给一个 $1\sim N$ 的排列 $P$ ，通过以下操作将这个排列排序，使所有元素从小到大排列。

- 选一个序列中的元素，将它移到序列的开头或者结尾。

问最少要操作多少次才能将序列排好序。

搬运 Editorial

考虑不用执行操作就已经排好序的子序列满足什么条件：

- 如果 $x_i < x_{i + 1}$ 那么，要满足 $x_i$ 在 $x_{i + 1}$ 右边。
- 满足 $x_i + 1 = x_{i + 1}$

于是我们可以找出最长的满足以上条件的子序列，然后剩下的元素全部操作一遍就可以将序列排好序。

证明考虑贪心，

### ABC184
#probabilty/expectation
#### D - increment of coins（简单期望）

题意：有三种硬币，分别有 $A,B,C$ 个，每次随机选出一个硬币，然后放回两个相同种类的硬币，直至某种硬币达到 100 个，问期望操作次数。

设 $f(a,b,c)$ 表示三种硬币已经分别有 $a,b,c$ 个，到完成目标还需要的操作次数。

显然
$$
f(a,b,c) = \dfrac{a}{a+b+c}\times f(a + 1,b,c) + \dfrac{b}{a+b+c}\times f(a,b+1,c) + \dfrac{c}{a+b+c}\times f(a,b,c+1)
$$
答案即为 $f(A,B,C)$

### AGC046 

#### B - Extension (DP)
#dp
题意：给出 $A\times B$ 的网格，所有网格涂成白色，通过以下两种操作扩展为 $C\times D$ ：

1. 向上扩展一行，在新增格子中选择一个涂成黑色，其它涂成白色。
2. 向右扩展一列，在新增的格子中选择一个涂成黑色，其它涂成白色。

问最后能得到多少种不同的 $C\times D$ 的网格。

考虑已经拓展到 $i\times j$ ，有两种情况拓展到 $i\times j$ ：

1. $(i - 1)\times j\rightarrow i\times j$ ，上一次向上拓展，方案数 $f(i-1,j)\times j$ .
2. $i\times(j-1)\rightarrow i\times j$ ，上一次向右拓展，方案数 $f(i,j - 1)\times i$ .

但是会出现算重的情况，第一种情况中拓展前最右列只有一个黑色格子的情况与第二种情况种拓展前最顶行只有一个黑子格子的情况在拓展后会重复计算，于是考虑去重。

![算重](C:\Users\47328\Desktop\信息学\notes\AGC046B.png)

不难发现，只要减去一次这些情况的方案数即可，第一种情况中拓展前最右列只有一个黑色格子的情况与第二种情况种拓展前最顶行只有一个黑子格子的情况总和是 $(i - 1)\times(j - 1)\times f(i-1,j-1)$ .

于是转移方程为：
$$
f(i,j) = f(i-1,j)\times j+f(i,j-1)\times i-(i-1)\times(j - 1)\times f(i-1,j-1)
$$




### ARC097

#### D - Equals (并查集 性质)
#ds/dsu
#property 
题意：给一个 $1$ 到 $N$ 的整数排列，再给出 $M$ 对整数对，第 $i$ 对整数对对应着操作：将排列中第 $x_i$ 个数和第 $y_i$ 个数交换。求出按任意顺序执行任意次操作后，最多能使多少 $p_i=i$.

建立图论模型，将所有整数对转化为边，构建一个无向图，然后我们发现一个明显的结论：

当 $p_i$ 与 $i$ 不在同一连通块中时，不可能做到 $p_i=i$ ，于是继续往连通方面思考。

发现当 $p_i$ 与 $i$ 在同一连通块中时，一定可以做到 $p_i = i$ ，推广结论可知统计有多少 $p_i$ 与 $i$ 在同一连通块中即可。



### ARC125

#### B - Squares (代数 性质)
#algebra
#property

题意：给一个整数 $N$ ，找出有多少对整数 $(x,y)$ 满足以下条件：

- $1\leqslant x,y \leqslant N$ .
- $x^2 - y$ 是一个平方数。（$0$ 也是平方数）



自然地列出式子 $x^2 - y = k^2$ ，然后变式得到 $(x+k)(x-k) = y$ ，我们令 $p = (x + k),q = (x-k)$ ，那么问题转化为找出满足以下性质的整数对 $p,q$ ：

1. $1\leqslant y = pq \leqslant N$ .
2. $x= \dfrac{p+q}{2}$ 是个整数。
3. $1\leqslant x=\dfrac{p+q}{2}\leqslant N$ .
4. $k \geqslant 0 \Leftrightarrow q\leqslant p$ .

于是我们可以枚举 $q$ ，那么 $p$ 的范围就是 $q\leqslant p\leqslant \dfrac{N}{q}$ ，又因为 $\dfrac{p+q}{2}$ 是个整数，所以 $p,q$ 的奇偶性相同，于是我们可以 $O(1)$ 算出对于每个 $q$ ，$p$ 的个数为 
$$
\left\lceil\dfrac{\lfloor\dfrac{N}{q}\rfloor-q+1}{2}\right\rceil
$$
由于 $pq\leqslant N$ ，可以知道枚举 $q$ 不超过 $\sqrt N$ 次，所以时间复杂度 $O(\sqrt N)$ .



### ARC128

#### A - Gold and Silver (性质)
#trick #property  #greedy #dp 
题意：最开始手上有 $1\,{\rm g}$ 金子，每一天都有一个汇率 $A_i$ ，第 $i$ 天可以做交易：

- 如果手上有 $x\,{\rm g}$ 金子，那么全部换成 $(x\times A_i)\,{\rm g}$ 银子。
- 如果手上有 $x\,{\rm g}$ 银子，那么全部换成 $\dfrac{x}{A_i}\,{\rm g}$ 金子。

求出每一天的交易方案使得最后一天得到的金子最多。

这种交易问题常见的 trick 是：这一天不进行交易等价于在同一天中买入后卖出。

于是考虑如果有一种最优情况是第 $i$ 天将金子换成银子，第 $j$ 天将银子换成金子，那么实际上可以看作：
$$
x\times\dfrac{A_i }{ A_{i + 1}}\times\dfrac{A_{i + 1}}{A_{i+2}}\times\cdots\times\dfrac{A_{j-1}}{A_j}
$$
显然根据贪心，可以知道当 $A_i > A_{i + 1}$ 的时候就进行交易即可。



当然也可以考虑 DP ，但是由于精度问题，要用另一个 trick ：将乘除转为加减
$$
\log\dfrac{a}{b} = \log a - \log b,\log ab = \log a + \log b
$$
于是我们将其转化为对数运算。

设 $f(i)$ 表示第 $i$ 天可以获得的最多的金子， $g(i)$ 表示第 $i$ 天可以获得的最多的银子，直接 $f$ 与 $g$ 之间互相转移即可：
$$
f(i) = \max\{f(i-1),g(i-1)+\log A_i\}\\
g(i) = \max\{g(i - 1),f(i - 1) - \log A_i\}
$$
记录 DP 过程的转移顺序即可。这个 trick 本身其实也受精度的影响，正确性不如上一个 trick 稳定。







### ABC224

#### D - 8 Puzzle on Graph (图论 最短路 暴力)
#graph/shortest-path  #brute-force
题意：有 $9$ 个结点，有标号 $ 1\sim 8$ 的棋子放在其中的 $8$ 个结点上，给出结点之间的无向边，每次可以将一个棋子从一个结点移到另一个空结点上，问最少要操作多少次才能使棋子 $i$ 在结点 $i$ 上。

考虑其状态的表示，我们可以用字符串表示当前棋盘的状态，令字符串的第 $i$ 位表示结点 $i$ 上的棋子编号，令 $9$ 表示结点为空。于是要求从给出的状态 $s$ 转移到状态 $123456789$ 的最少步数。

我们发现，这样表示状态，其可能的方案数有 $9!=362880$ 种，状态数比较小，所以可以暴力搜索。

用 ```map``` 存下每个状态对应的最小操作数，然后用类似最短路的思路做即可。



#### E - Integers on Grid (DP 优化)
#dp #optimization 
题意：给一个 $H\times W$ 的网格，其中有 $N$ 个格子的值为 $A(i,j)$  ，其余的格子的值均为 $0$.你有一个棋子，最开始放在某一个格子 $(i,j)$ 上，这个棋子可以移动到另一个格子 $(x,y)$ 当且仅当：

- $A(i,j) < A(x,y)$.
- $(i,j)$ 和 $(x,y)$ 在同一列或者同一行.

现在问若最开始将棋子放在第 $i$ 个有值的格子上，最多能移动多少步，对所有的 $i=1,2,\ldots,N$ 求出这个答案。

考虑 DP ，设 $f(i)$ 表示从格子 $i$ 开始最多能移动的步数。由于棋子只能从值大的格子移到值小的格子，所以在一开始将有值的格子按值从大到小排好序，我们就可以进行线性 DP.
$$
f(i) = \max\{f(j)|j<i,A_j>A_i,x_i=x_j \;{\rm or}\;y_i=y_j\}
$$
不难发现，这样 DP 是 $O(N^2)$ 的，我们需要对其进行优化。我们可以将每一行和每一列的最大步数记下来，转移的时候就可以做到 $O(1)$ 转移了，然后再在适当的时候，对每一行和每一列的最大步数更新，这样就能够保证答案的正确性，并且得到一个优秀复杂度的算法。



#### F - Problem where +s Separate Digits (拆分贡献 转化为期望与概率)
#probabilty/expectation 
题意：给一个只包含数字的字符串 $S$ ，可以在任意两个数字之间加 ```+``` ，可以加任意次，使 $S$ 组成一个表达式 $T$ ，现在求对于所有 $T$ 的值的和。

显然，我们可以单独考虑每一位数字对答案的贡献。一位数字可能作为个位出现，也可能作为百位或千位或更高位出现，那么我们要计算在所有可能的 $T$ 中，对于第 $i$ 位数字 $S_i$ ，它作为个位、百位、千位...的方案数有多少。

我们可以将其转换为概率期望问题，对于所有的 $T$ 我们等概率地得到一个 $T'$ ，问 $T'$ 的值的期望是多少，当我们求出这个期望 $E$ 的时候，由期望的定义可知，答案即为 $E\times 2^{Len(S)-1}$ .

现在考虑从末尾往开头第 $i$ 位作为一个数中某一位的情况与其概率：

- 作为个位，只要在 $i$ 与 $i + 1$ 间加 ```+``` 即可，概率 $\dfrac{1}{2}$ .

- 作为十位，$i$ 与 $i + 1$ 间没有 ```+``` ，$i+1$ 与 $i+2$ 间有 ```+``` ，概率 $\dfrac{1}{4}$ .
- $\cdots\cdots$
- 贡献系数为 $10^{i-2}$ ，概率 $\dfrac{1}{2^{i-1}}$ .
- 贡献系数为 $10^{i-1}$ ，$i$ 后面没有 ```+```，概率与贡献系数为 $10^{i-2}$ 的情况一样，是 $\dfrac{1}{2^{i-1}}$ .

那么这个数对期望的贡献就是 
$$
S_i\times\sum_{j=1}^{i-1} \left(10^{j-1}\times\dfrac{1}{2^j}\right)\times10^{i-1}\times\dfrac{1}{2^{i-1}}
$$
后面那一大块贡献系数，有前缀性质，可以边计算贡献边计算系数，时间复杂度 $O(N)$ .



### ABC227

#### D - Project Planning (二分答案)
#binary-search
题意：有 $N$ 个部门，第 $i$ 个部门的人数为 $A_i$ ，一个项目需要恰好 $K$ 个来自不同部门的人完成，问最多可以完成多少项目。

考虑贪心，我们可以采用如下贪心策略：每次为一个项目选人的时候，都从可选人数最多的 $K$ 个部门选择。

但是如果我们逐次进行上述操作，那么时间复杂度将很大。我们发现，答案具有单调性，于是考虑二分答案。二分一个答案 $P$ ，考虑能否完成 $P$ 个项目，对部门的人数进行分类讨论：

1. $A_i < P$ ，部门 $i$ 最多派出 $A_i$ 人，参与 $A_i$ 个不同的项目。
2. $A_i \geqslant P$ ，部门 $i$ 最多派出 $P$ 人，参与这 $P$ 个不同的项目。

于是只要判断 $\sum\min\{A_i,P\}$ 与 $P\times K$ 的关系即可知道能否完成 $P$ 个项目。 







### ABC226

#### A - Round decimals (3:39)

题意：给一个三位小数的浮点数，输出离它最近的整数。

因为给出的数是非负的，所以小数部分大于 $0.5$ 就输出整数部分 + 1，否则输出整数部分。

#### B - Counting Arrays (17:20)

题意：给出 $N$ 个数组，问有多少个不同的数组。

将数组转成字符串，每个元素间加空格，然后排个序统计一下就水过了。

#### C - Martial artist (10:51)
#graph 
题意：有 $N$ 个动作可以学，第 $i$ 个动作用时 $T_i$ ，学第 $i$ 个动作前要先学 $A_{i,1},A_{i,2},\cdots ,A_{i,K_i}$ 并且 $A_{i,j}<i,1\leqslant j \leqslant K_i$，问从 $0$ 开始学，学完第 $N$ 个动作至少要多少时间。

建立图论模型，不难发现，最后形成一个 DAG 并且最终汇聚在 $N$ 结点，于是将所有边反向，转化为 $N$ 单源的图，从 $N$ 开始 DFS 将所有遍历到的结点的时间加起来即为答案。

#### D - Teleportation (44:13)
#greedy #algebra 
题意：有 $N$ 个城镇，坐标为 $(x_i,y_i)$ ，在坐标 $(x,y)$ 使用一次咒语 $(a,b)$ 可以使你传送到 $(x + a,y + b)$ ，现在问咒语集合中至少要有多少个咒语，使得对于每对城镇 $i,j(i\neq j)$ ，可以从集合中选出一个咒语，反复运用若干次后可以从 $i$ 传送到 $j$.

贪心，要令集合大小最小，就要让其中的咒语的适用性尽可能强。我们可以将每对城镇间的路径用向量表示，如果有多个向量方向相同，显然只要将它们的单位向量作为咒语就可以用一个咒语走过这些路径。

于是求出所有向量的坐标表示，并求出它们的单位向量，在所有单位向量中去重后，就是答案集合。

#### E - Just one (72:49) (图论 性质)
#graph #property 
题意：给出一张 $N$ 个结点和 $M$ 条边的无向图，没有自环和重边，问有多少种方案钦定所有边的方向使得每一个点出度为 1.

找性质，先从简单情况开始研究。图论题可以考虑研究基础的图论模型，比如链、树、环等。

考虑一条链的情况，发现方案数为 0 ，总有一个点出度为 0 .

考虑一棵树的情况，发现方案数为 0 .

考虑一个简单环的情况，发现方案数为 2 ，只要令环中的所有边同向即合法。

然后研究一下基础模型组合的情况，就可以发现性质：当一个连通块有且仅有一个简单环的时候，有 2 种钦定边的方案使这个连通块合法。

证明：显然，只有当图中的边数与点数相等的时候，每个点才能恰好被分配一条出边。如果边数大于点数，那么一定存在一个点出度为 2 .

于是得证。当整个图中出现非法连通块的时候，方案数就为 0 ；如果全部连通块合法，那么总方案数就是 $2^k$ 其中 $k$ 为连通块的个数。



#### F - Score of Permutations





#### G - The baggage (贪心)



#### H - Random Kth Max (期望)



### ABC225 (Virtual Participation)

#### A - Distinct Strings (5:18)

题意：给 3 个小写字母，问能组合成多少种字符串。

显然答案与有多少个字母相同有关。

1. 全部相同时答案为 1
2. 有两个相同时答案为 3
3. 互不相同时答案为 6

暴力特判输出即可。

#### B - Star or Not (8:12)
#graph 
题意：给出一个图，问其是否星星图（菊花图），星星图的定义为有一个结点直接连接其它所有结点。

根据定义，花蕊的性质就是度为 $N-1$ ，检查所有结点中是否存在即可。

#### C - Calendar Validator (25:13)

题意：给一个 $10^{100}\times 7$ 的矩阵 $A$ ，$A_{i,j}=(i-1)\times7+j$，再给一个矩阵 $B$ ，问矩阵 $B$ 在不旋转的情况下，是否是 $A$ 的子矩阵。

可以先确定 $B$ 中的一个元素，然后推得是 $A$ 的子矩阵的 $B'$ ，然后对比 $B$ 和 $B'$ 即可。

推出 $B$ 的左上角可能在 $A$ 的位置 $(i,j)$ ，然后枚举 $B$ 中的每一个元素，判断其是否与计算推理出来的值相同即可。

#### D - Play Train (44:03)
#simulation #ds/list
题意：有 $N$ 辆玩具车，给出 $Q$ 次操作和询问，一共有三种类型：

1. ```1 x y``` 令 $x$ 的尾部与 $y$ 的头部相连。保证 $x\neq y$ ，$x$ 的尾部和 $y$ 的头部在此操作前没有与其它车相连，在此操作前 $x$ 和 $y$ 属于不同列车组。
2. ```2 x y ``` 令 $x$ 的尾部与 $y$ 的头部分离。保证 $x\neq y$ ，此操作前 $x$ 的尾部与 $y$ 的头部相连。
3. ```3 x ``` 从头至尾地输出 $x$ 所在的列车组的所有玩具车编号。

模拟链表操作即可，对每个玩具车记录前驱 ${\rm pre}(x)$ 和后继 ${\rm nxt}(x)$ 。输出的时候，从 $x$ 开始一直访问前前驱，然后倒序输出，再一直访问后继顺序输出即可做到 $O(n)$.

#### E - フ/7 (贪心)
#greedy 
题意：在一个平面上，给出若干个"7"形状的图案，问以最优方式删除一些"7"，最多有多少个"7"可以在原点完全可视。"7"由两条线段 $AB,BC$ 组成，其中 $A(x_i - 1,y_i),B(x_i,y_i),C(x_i,y_i-1)$ .一个"7"在原点完全可视，当且仅当四边形 $OABC$ 不与任何其它图案"7"相交。

![7](C:\Users\47328\Desktop\信息学\notes\7.png)

如图所示，红色的图案即为"7"，并且可以知道，图案 1 不是完全可视的，因为其与原点组成的四边形与图案 2 相交。

我们可以尝试发掘两个图案互不遮挡的条件。

可以发现，一个图案将遮挡从原点到图案的两条边所在的两条射线所夹的区域，令靠近 $y$ 轴的直线斜率为 $k_1$ ，靠近 $x$ 轴的直线斜率为 $k_2$ ，于是对于两个图案 $p,q$ 有：

- 两个图案互不遮挡当且仅当 $k_{1p}\leqslant k_{2q}$ 或 $k_{1q}\leqslant k_{2p}$

![7(2)](C:\Users\47328\Desktop\信息学\notes\7(2).png)

所以我们可以用区间表达"7"，一个图案"7"所占有的区间就是 $[k_2,k_1]$ ，于是问题转化为求最多互不相交区间。

经典贪心，按右端点从小到大排序，每次先选右端点小的即可得到答案。

比较斜率的 Trick (高中解析几何基础) : $k_1 = \dfrac{y_1}{x_1},k_2=\dfrac{y_2}{x_2},k_1 < k_2 \Leftrightarrow y_1x_2<y_2x_1$

#### F - String Cards (贪心 DP)
#greedy  #dp 
题意：给出 $N$ 张卡牌，每张卡牌上面有一个字符串，要求从中选 $K$ 个，并自由排列组合后，使得字典序最小的方案。字典序相同时长度小的字符串字典序更小。

[官方题解](https://atcoder.jp/contests/abc225/editorial/2850)很详细，这里搬运翻译一下，并稍作批注：

介绍 4 种错解，并举出它们的反例。

##### 错解 1

> 将 $(S_1,\ldots ,S_N)$ 排序，使得 $S_i\leqslant S_{i + 1}$.
>
> 将前 $K$ 个字符串首尾相连作为答案.

反例：

```
2 2
b
ba
```

正确答案 ```bab``` ，程序输出 ```bba```.

显然没有考虑到字符串长度不一的条件。

##### 错解 2 (考场上就想到这...)

> 将 $(S_1,\ldots ,S_N)$ 排序，使得 $S_i+S_{i+1}\leqslant S_{i+1}+S_{i}$.
>
> 将前 $K$ 个字符串首尾相连作为答案.

反例：

```
2 1
b
ba
```

正确答案 ```b``` ，程序输出 ```ba```.

没有考虑到作为结尾的字符串后面不需再加字符串。

##### 错解 3

> 将 $(S_1,\ldots ,S_N)$ 排序，使得 $S_i+S_{i+1}\leqslant S_{i+1}+S_{i}$.
>
> 设 $f(i,j)$ 表示在前 $i$ 个字符串中选出 $j$ 个首尾相连的最小字典序的字符串，
>
> 转移方程 $f(i,j) =\min\{f(i-1,j),f(i-1,j-1)+S_i\}$
>
> 答案为 $f(N,K)$.

反例：

```
3 2
baa
ba
b
```

正确答案 ```baab``` ，程序输出 ```baaba```.

同样是没有考虑到作为结尾的字符串后面不需再加字符，从前往后考虑总会使得正确答案是程序输出的前缀，于是在字符串长度的差异上，程序输出字典序总大于正确答案。

##### 错解 4

> 将 $(S_1,\ldots ,S_N)$ 排序，使得 $S_i+S_{i+1}\leqslant S_{i+1}+S_{i}$.
>
> 设 $f(i,j)$ 表示在前 $i$ 个字符串中选出 $j$ 个首尾相连的最小字典序的字符串，
>
> 转移方程 
> $$
> f(i,j) =\min\{f(i-1,j),\min_{k<i}\{f(i-1,j-1)+S_i\}\}
> $$
> 答案为 $f(N,K)$.

反例：

```
4 3
bbaba
bba
b
b
```

正确答案 ```bbababb``` ，程序输出 ```bbababbab```.

还是老问题，转移过程中增加参与转移的状态并不能消除无法考虑到结尾字符串后面不需再加字符串的问题，还是会使输出成为答案的前缀。

##### 正解

将 $(S_1,\ldots ,S_N)$ 排序，使得 $S_i+S_{i+1}\leqslant S_{i+1}+S_{i}$ . 这一步是必须的，因为它保证了这些字符串从前往后选或者从后往前选都一定能取得最佳方案，这样排序的原理就是贪心中的相邻交换法，我们发现错解 1 之所以错是因为令两个长度不一的字符串比较，但它们组合起来以后的情况与两个独立的字符串的情况完全不同，所以正解排序通过比较两个长度相同的字符串解决了这一问题。（原题解有详细证明）这一步排序提供了一种 DP 的顺序，消除了原问题的后效性，为 DP 做铺垫。

然后从上面的错解，我们知道，后面几种看似十分接近正解的解法，问题都出现在不能选择到最优的作为结尾的字符串，所以为什么不考虑先从后面开始选呢？设 $f(i,j)$ 表示在 $(S_i,\ldots ,S_N)$ 中选出 $j$ 个字符串首尾相连得到的最小字典序的字符串。那么对于枚举到一个新的字符串，我们有两种决策，一种是抛弃它，另一种是将其作为先前最优解的开头，于是有：
$$
f(i,j) = \min\{f(i+1,j),S_i + f(i+1,j-1)\}
$$
好了，现在可以总结一下这道题了。



```cpp
#include<iostream>
#include<cstdio>
#include<string>
#include<cstring>
#include<algorithm>

using namespace std;
int N,K,tot;
string s[55],f[55][55],inf;

string smin(string s1,string s2)
{
	int len1 = s1.size();int len2 = s2.size();
	for(int i = 0;i < min(len1,len2);i ++)
		if(s1[i] != s2[i]) return (s1[i] < s2[i] ? s1 : s2);
	return (len1 < len2 ? s1 : s2);
}

bool cmp(string s1,string s2)
{
	string s3 = s1 + s2;
	string s4 = s2 + s1;
	int len = s3.size();
	for(int i = 0;i < len;i ++)
		if(s3[i] != s4[i]) return s3[i] < s4[i];
	return s1.size() < s2.size();
}

int main()
{
	scanf("%d%d",&N,&K);
	for(int i = 1;i <= N;i ++) cin >> s[i],tot += s[i].size();
	sort(s + 1,s + 1 + N,cmp);
	for(int i = 1;i <= tot;i ++) inf += "z";
	for(int i = 0;i <= N + 1;i ++)
		for(int j = 1;j <= N;j ++)
			f[i][j] = inf;
	for(int i = N,ii = 1;i >= 1;i --,ii ++)
		for(int j = 1;j <= min(ii,K);j ++)
		{
			f[i][j] = smin(f[i + 1][j],s[i] + f[i + 1][j - 1]);
//			printf("f(%d,%d) : ",i,j);cout << f[i][j] << endl;
		}
	cout << f[1][K];
	return 0;
}
```

#### G - X (网络流 最小割)
#network-flow/minimum-cut
题意：给一个 $H$ 行 $W$ 列的网格，每一个正方形网格 $(i,j)$ 权值为 $A_{i,j}$ ，现在想给若干个网格画上 X ，即连上两条对角线，其它网格不能画任何东西，问最大得分是多少。定义得分为画上 X 的网格的权值和减去一个常数 $C$ 与最少笔画数的乘积，即 ${\rm score} = \sum A_{i,j} - C\times n$.

翻译一下题解并稍作批注

官方题解通过题意转化与简化操作类型，将原问题转化为一个最小割问题。

首先要考虑用于画 X 的最小线段数怎么求。

- 对于网格 $(i,j)$ ，当它的左上角 $(i-1,j-1)$ 或右下角 $(i+1,j+1)$ 有画了 X 的网格时，它可以少画一笔。
- 对于网格 $(i,j)$ ，当它的左下角 $(i - 1,j + 1)$ 或右上角 $(i + 1,j - 1)$ 有花了 X 的网格时，它可以少画一笔。

然后可以将以上条件等价为 ：

- 对于网格 $(i,j)$ ，当它的左上角没有画了 X 的网格，那么它就要多画一笔。
- 对于网格 $(i,j)$ ，当它的左下角没有画了 X 的网格，那么它就要多画一笔。

于是画 X 的最小线段数 $n=n_1+n_2$ ，$n_1$ 为满足以上第一种情况的网格数， $n_2$ 为满足以上第二种情况的网格数。

然后就将问题转化为：

> 对于每个网格，可以选择画或者不画，如果画了，就得到分数 $A_{i,j}$ ，不画就不得分。
>
> 同时，对于每一个画了的网格，如果左上角的网格没有画，就使答案减去 $C$ ，如果右上角的网格没有画，也使答案减去 $C$ .求最大得分。 

求最大得分不好求，那就考虑求最小代价，将问题转化为：

> 对于每个网格，可以选择画或者不画，如果画了，就没有增加代价，如果不画，就增加代价 $A_{i,j}$.
>
> 同时，对于每一个画了的网格，如果左上角的网格没有画，就增加代价 $C$ ，如果右上角的网格没有画，也增加代价 $C$ .求最小代价。

于是可以通过建立网络流模型，将上述问题转化为最小割问题。

考虑怎么建图。考虑我们的图是怎么样的，对于不画的格子，只能增加代价 $A_{i,j}$ ，并且不会有更多的代价增加，所以割断这一类边后，不需要再割这个格子的其它边。而对于画了的格子，代价为 $A_{i,j}$ 的边一定不被割掉，会有代价为 $0$ 的边，还会有代价 $C$ 的边，当其左上角和右上角的格子都画了 X 时，就会割掉代价为 $0$ 的边，否则割掉一条或两条代价为 $C$ 的边。

于是建立一个源点 $S$ ，一个汇点 $T$ ，按一下方案建图：

1.  源点 $S$ 向分别所有网格连一条代价为 $A_{i,j}$ 的边。
2. 对于所有左上角没有网格的网格，分别向汇点连一条代价为 $C$ 的边。
3. 对于所有左下角没有网格的网格，分别向汇点连一条代价为 $C$ 的边。
4. 对于所有左上角有网格的网格，向汇点连一条代价为 $0$ 的边，向左上角的网格连一条代价为 $C$ 的边。
5. 对于所有左下角有网格的网格，向汇点连一条代价为 $0 $ 的边，向左下角的网格连一条代价为 $C$ 的边。

于是求出最小代价后，用总价值减去最小代价即为答案 ${\rm Ans} = \sum A_{i,j} - {\rm mincost}$

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<string>
#include<queue>

using namespace std;
typedef long long ll;
const ll inf = 0xfffffffff;
ll A[105][105],C,sum;
int H,W,cnt = 1,tot,map[105][105];
int head[50005],p[50005],dep[50005];

struct edge{
	int nxt;
	int to;
	ll cap;
}e[500005];

void addedge(int from,int to,ll val)
{
	cnt ++;
	e[cnt].nxt = head[from];
	e[cnt].to = to;
	e[cnt].cap = val;
	head[from] = cnt;
	cnt ++;
	e[cnt].nxt = head[to];
	e[cnt].to = from;
	e[cnt].cap = 0;
	head[to] = cnt;
	return ;
}

bool bfs()
{
	memset(dep,-1,sizeof(dep));
	queue<int > q;int u = 0;
	q.push(tot + 1);dep[tot + 1] = 1;
	for(;!q.empty();)
	{
		u = q.front();
		q.pop();
		for(int i = head[u],v = 0;i;i = e[i].nxt)
		{
			v = e[i].to;
			if(e[i].cap > 0 && dep[v] < 0)
			{
				q.push(v);
				dep[v] = dep[u] + 1;
			}
		}
	}
	return dep[tot + 2] > 0;
}

ll dfs(int now,int t,ll flow)
{
	if(now == t || flow == 0) return flow;
	ll res = 0;
	for(int &i = p[now];i;i = e[i].nxt)
	{
		int v = e[i].to;
		if(e[i].cap > 0 && dep[v] > dep[now])
		{
			ll fl = dfs(v,t,min(flow,e[i].cap));
			res += fl;
			flow -= fl;
			e[i].cap -= fl;
			e[i ^ 1].cap += fl;
			if(flow == 0) break;
		}	
	}
	return res;
}

void Dinic()
{
	ll ans = 0;
	while(bfs())
	{
		memcpy(p,head,sizeof(p));
		ans += dfs(tot + 1,tot + 2,inf);
	}
	printf("%lld",sum - ans);
}

int main()
{
	scanf("%d%d%lld",&H,&W,&C);
	for(int i = 1;i <= H;i ++)
		for(int j = 1; j<= W;j ++)
		{
			scanf("%lld",&A[i][j]);
			tot ++;map[i][j] = tot;
			sum += A[i][j];
		}
	for(int i = 1;i <= H;i ++)
	{
		for(int j = 1;j <= W;j ++)
		{
			addedge(tot + 1,map[i][j],A[i][j]);
			if(i - 1 < 1 || j - 1 < 1) addedge(map[i][j],tot + 2,C);
			else addedge(map[i][j],map[i - 1][j - 1],C),addedge(map[i][j],tot + 2,0);
			if(i + 1 > H || j - 1 < 1) addedge(map[i][j],tot + 2,C);
			else addedge(map[i][j],map[i + 1][j - 1],C),addedge(map[i][j],tot + 2,0);
		}
	}
	Dinic();
	
	return 0;
} 
```

#### H - Social Distance 2 (组合数学)
#combinatorics
题意：有 $N$ 张椅子，每张椅子只能坐一个人，有 $M$ 个人将坐在这其中的 $M$ 张椅子上，定义得分为
$$
\prod_{i=1}^{M-1}(B_{i + 1}-B_i)
$$
其中 $B=(B_1,B_2,\ldots ,B_M)$ 是每一个人所做的椅子的编号。

已经有 $K$ 个人坐在编号为 $A_1,A_2,\ldots,A_K$ 的椅子上了，计算剩下的 $M-K$ 个人就座的所有方案的得分总和。 

翻译官方题解并稍作批注。

在题目要求区分这 $M-K$ 个还没有座位的人，我们不妨暂时忽略他们具体是谁，在计算出一些结果后再乘上排列数即可。

考虑计算以下几种值：

> $f_1(n,k)$ 表示有人已经坐在这 $n$ 张椅子中最左端的椅子和最右端的椅子，并且还有 $k$ 个人要就座所有方案的得分总和。
>
> $f_2(n,k)$ 表示有人已经坐在这 $n$ 张椅子中最左端的椅子，并且还有 $k$ 个人要就座所有方案的得分总和。
>
> $f_3(n,k)$ 表示 $k$ 个人坐在 $n$ 张空椅子上的所有方案的得分总和。

原文介绍了生成函数和组合计数两种方法计算 $f_1$ ，本文只搬运其介绍组合计数的部分。

不会了...咕咕...
