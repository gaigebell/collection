---
title: "Untitled"
draft: false
tags:
  - solution
---
 


- [ ] [Problem - 2004B - Codeforces](https://codeforces.com/problemset/problem/2004/B)
- [ ] [Problem - 2004A - Codeforces](https://codeforces.com/problemset/problem/2004/A)
- [ ] [Problem - 1997D - Codeforces](https://codeforces.com/problemset/problem/1997/D)
- [ ] [Problem - 1997C - Codeforces](https://codeforces.com/problemset/problem/1997/C)
- [ ] [Problem - 1997B - Codeforces](https://codeforces.com/problemset/problem/1997/B)
- [ ] [Problem - 1997A - Codeforces](https://codeforces.com/problemset/problem/1997/A)





### [Problem - 2008C - Codeforces](https://codeforces.com/problemset/problem/2008/C)
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


### [Problem - 2008D - Codeforces](https://codeforces.com/problemset/problem/2008/D)
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
### [Problem - 2008E - Codeforces](https://codeforces.com/problemset/problem/2008/E)
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

### [Problem - 2008F - Codeforces](https://codeforces.com/problemset/problem/2008/F)
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

### [Problem - 2004D - Codeforces](https://codeforces.com/problemset/problem/2004/D)
- [x] [Problem - 2004D - Codeforces](https://codeforces.com/problemset/problem/2004/D)
#binary-search #math/classification #greedy 
有 $n$ 个城市排成一排，标号 1 到 n. 城市之间用传送门移动，传送门有 4 种颜色：蓝、绿、红、黄. 每座城市都有 2 个传送门. 当两个城市 $i,j$ 之间有相同的传送门时，可以互相抵达，花费 $|i-j|$. 有 $q$ 次询问，计算从城市 $x$ 到 城市 $y$ 的最小花费.
$1\leq t\leq 10^4,1\leq n,q \leq 2\cdot 10^5,1 \leq x, y\leq n$

$|x-y| \leqslant |x-k|+|y-k|$ 这是显然. 所以当 $x,y$ 可以互相抵达时，就直接通过传送门即可，花费 $|x-y|$

$x,y$ 不可以互相抵达时，尝试寻找中介. 在考虑中介的时候，我们知道，在当前讨论环境下，$x,y$ 所有的传送门一定包含了 4 种颜色，于是我们只需要找包含 $x$ 中一种颜色和 $y$ 中一种颜色的城市作为中介. 中介只需 1 个，多了有时反而不优.
在符合条件的中介中，最好的情况是落在 $x,y$ 中间，花费 $|x-y|$.
其次就是最靠近 $x$ 或最靠近 $y$ .

```c++
#include<bits/stdc++.h>

using namespace std;
const int inf = 1e9 + 7;
const int maxn = 2e5;
int t, n, q, ty[maxn + 5], x, y, mid, ans;
string pt[maxn + 5];
// 1   2   3    4   5   6
// BG, BR, BY, GR, GY, RY
vector<int > med[10];
string di[10] = {"","BG", "BR", "BY", "GR", "GY", "RY"};

int main()
{
	cin >> t;
	for(int tt = 1;tt <= t;tt ++)
	{
		cin >> n >> q;
		for(int i = 1;i <= 6;i ++) med[i].clear();
		for(int i = 1;i <= n;i ++)
		{
			cin >> pt[i];
			for(int j = 1;j <= 6;j ++)
				if(pt[i] == di[j])
				{
					ty[i] = j;
					break;
				}
			med[ty[i]].push_back(i);
		}
		
		for(int qq = 1;qq <= q;qq ++)
		{
			cin >> x >> y;
			if(pt[x][0] == pt[y][0] || pt[x][0] == pt[y][1] || pt[x][1] == pt[y][0] || pt[x][1] == pt[y][1])
				cout << abs(x - y) << endl;
			else
			{
				mid = (x + y) >> 1;
				ans = inf;
				for(int i = 1;i <= 6;i ++)
				{
					if((pt[x][0] == di[i][0] || pt[x][0] == di[i][1] || pt[x][1] == di[i][0] || pt[x][1] == di[i][1])&&(pt[y][0] == di[i][0] || pt[y][0] == di[i][1] || pt[y][1] == di[i][0] || pt[y][1] == di[i][1]))
					{
					    auto lb = lower_bound(med[i].begin(),med[i].end(),x);
					    if(lb != med[i].end()) ans = min(abs(x - *lb) + abs(y - *lb), ans);
					    if(lb != med[i].begin())
					    {
					        lb --;
					        ans = min(abs(x - *lb) + abs(y - *lb), ans);
					    }
					    lb = lower_bound(med[i].begin(),med[i].end(),y);
					    if(lb != med[i].end()) ans = min(abs(x - *lb) + abs(y - *lb), ans);
					    if(lb != med[i].begin())
					    {
					        lb --;
					        ans = min(abs(x - *lb) + abs(y - *lb), ans);
					    }
					}
				}
				if(ans == inf) cout << -1 << endl;
				else cout << ans << endl;
			}
		}
		
	}
	return 0;
}
```


### [Problem - 2004C - Codeforces](https://codeforces.com/problemset/problem/2004/C)

 - [x] [Problem - 2004C - Codeforces](https://codeforces.com/problemset/problem/2004/C)

#greedy 
有 $n$ 张牌，Alice 和 Bob 轮流取，Alice 先手，每人每轮取 1 张，每张牌上有一个分数 $a_i$ ，最终游戏分数记为二者得分之差 $A-B$. Alice 想让游戏分数尽可能大，Bob 则想让游戏分数尽可能小，二者都采取最优策略. 现在 Bob 可以作弊，只能增加某些牌的分数，而且增加的总分数不超过 $k$ ，问采取最优作弊策略，Bob 可以让游戏分数最小到多少.

$1\leq t\leq 5000, 2\leq n \leq 2\cdot 10^5,0\leq k\leq 10^9,1\leq a_i\leq 10^9$

如果不作弊，那么两人均采用贪心策略，将牌从大到小排序，Alice 将取走排在奇数位的牌，Bob 将取走排在偶数位的牌.
考虑作弊，对某个牌加上一定分数后，其在牌堆中的位置可能会发生改变（在保证从大到小排序的前提下）. 作弊当然是希望让偶数位牌的分数总和尽可能大.
对偶数位牌加分，如果超过了前一张牌，那么这两张牌的位置会互换，Alice 总会拿走较大的一张. 并且根据贪心，我们可以知道 $A-B\geqslant 0$ ，所以我们能做的只能是让 $A-B$ 尽可能接近于 0. 同时，加同样的分数，加给在前面的偶数牌和加给在后面的偶数牌效果等价，所以我们只需要让偶数位置的牌尽可能接近它们的前一张牌（在奇数位）

时间复杂度 $O(n\log n + n)$ （最复杂反而是排序...）

```C++
#include<bits/stdc++.h>

using namespace std;
const int maxn = 2e5;
int t, n, k, a[maxn + 5], d, A, B; 
int main()
{
	cin >> t;
	for(int tt = 1;tt <= t;tt ++)
	{
		cin >> n >> k;
		for(int i = 1;i <= n;i ++)
			cin >> a[i];
		sort(a + 1,a + 1 + n);
		A = 0;B = 0;
		for(int i = n;i >= 2 && k > 0;i -= 2)
		{
			d = min(a[i] - a[i - 1],k);
			a[i - 1] += d;
			k -= d;			
		}
		for(int i = n;i >= 1;i -= 2)
		{
			A += a[i];
			B += a[i - 1];
		}
		cout << A - B << endl;
	}
	return 0;
}
```


### [Problem - 2002B - Codeforces](https://codeforces.com/problemset/problem/2002/B)

- [x] [Problem - 2002B - Codeforces](https://codeforces.com/problemset/problem/2002/B)

#game 

Alice 有一个 $n$ 排列 $[a_1,a_2,...,a_n]$ , Bob 也有一个 $n$ 排列 $[b_1,b_2,...b_n]$, 在每一轮游戏中，以下事件按顺序发生：
- Alice 在她的数组中，取走第一个或最后一个
- Bob 在他的数组中，取走第一个或最后一个
游戏进行 $n-1$ 轮，最后 Alice 剩下 $x$ ，Bob 剩下 $y$ ，如果 $x=y$ 那么 Bob 获胜，否则 Alice 获胜. 如果两个人都以最优策略游戏，谁会获胜.
$1\leq t\leq 10^4, 1\leq n\leq 3\cdot 10^5$

需要从中发现规律. Alice 拿走一个数 $x$ 的时候，Bob 数组中的 $x$ 就没有用了，所以 Bob 应该尽早去掉自己数组中的 $x$ . 而在这个时候，如果 $x$ 不在 Bob 数组的第一个或最后一个，那么 Bob 就必须去掉一个不为 $x$ 的数 $y$ ，而这时候，如果 Alice 数组中还有 $y$ 那么 Alice 可以只留下 $y$ ，Bob 必败.
所以只需要考察：Alice 的数组第一个和最后一个是否也在 Bob 的数组第一个或最后一个出现.

```C++
#include<bits/stdc++.h>

using namespace std;
const int maxn = 3e5;
int a[maxn + 5],b[maxn + 5];
int n,la,ra,lb,rb;
bool flag = 1;
int t;

int main()
{
    cin >> t;
    for(int tt = 1;tt <= t;tt ++)
    {
        cin >> n;flag = 1;
        for(int i = 1;i <= n;i ++) cin >> a[i];
        for(int i = 1;i <= n;i ++) cin >> b[i];
        la = 1;ra = n;lb = 1;rb = n;
        for(int i = 1;i <= n;i ++)
        {
            if(a[la] != b[lb] && a[la] != b[rb])
            {
                flag = 0;break;
            }
            if(a[ra] != b[lb] && a[ra] != b[rb])
            {
                flag = 0;break;
            }
            if(a[la] == b[lb]) la ++,lb ++;
            else
            {
                if(a[la] == b[rb]) la ++,rb --;
                else
                {
                    if(a[ra] == b[lb]) ra --,lb ++;
                    else
                    {
                        if(a[ra] == b[rb]) ra --,rb --;
                    }
                }
            }
        }
        if(flag) cout << "Bob" << endl;
        else cout << "Alice" << endl;
    }
        
    return 0;
}
```