

  

#### [3260. 找出最大的 N 位 K 回文数](https://leetcode.cn/problems/find-the-largest-palindrome-divisible-by-k/description/)
#dp #string/palindrome 

类似题目：[[Atcoder#[D - I Hate Non-integer Number (atcoder.jp)](https //atcoder.jp/contests/abc262/tasks/abc262_d)]]

观察数据范围，不支持 $O(n^2)$ 及以上的做法，看到 $1\leqslant k\leqslant 9$ 从这里入手，发现余数一定不超过 10. 于是考虑有关 $k$ 的做法.

比如动态规划，让转移方程与 $k$ 有关，一种想法是 $f(i,r)$ 表示满足前 $i$ 位组成的数字模 $k$ 为 $r$ 的条件下第 $i$ 位的数字.

考虑怎样转移，由于 DP 是有阶段性、最优子结构，考察数字拆解和组合在模运算下的性质，想到数字拆解成 $a_0 + a_1\times10+a_2\times 100+\cdots+a_n\times 10^n$ . 想到模运算在加法、乘法下运算顺序并不影响
结果，于是可以枚举某一位数字，然后对对应的 $f(i,r)$ 更新，让 $f(i,r)$ 尽可能大.

接着考虑到是回文数，最低位数字和最高位数字一样，那么从最中间开始往两边 DP，并记录下更新的过程，$f(1,0)$ 则为最高位和最低为数字，再回溯回去就能得到答案.

- 时间复杂度: $O(k^2n)$
- 空间复杂度: $O(nk)$

```C++ []
class Solution {
public:
    string largestPalindrome(int n, int k) {
        int rem[100005];
        string ans = "";
        string half = "";
        rem[1] = 1;
        for(int i = 2;i <= n;i ++)
            rem[i] = (rem[i - 1] * 10) % k;
        int f[100005][10],rec[100005][10];
        int m = n/2;
        for(int i = 0;i <= 9;i ++)
            for(int j = 1;j <= m;j ++)
                f[j][i] = -1;
        if(n&1)
        {
            m ++;
            for(int i = 0;i <= 9;i ++)
                f[m][i] = -1;
            for(int i = 0;i <= 9;i ++)
                f[m][(i*rem[m])%k] = i;
        }
        else
        {
            for(int i = 0;i <= 9;i ++)
                f[m][(((i*rem[m])%k)+((i*rem[n-m+1])%k))%k] = i;
        }
        for(int pos = m - 1;pos >= 1;pos --)
        {
            for(int r = 0, st = 0;r < k;r ++)
            {
                if(f[pos + 1][r] > -1)
                {
                    if(pos == 1) st = 1;
                    for(int d = st;d <= 9;d ++)
                    {
                        int nr = (r+((((d*rem[pos])%k)+((d*rem[n-pos+1])%k))%k))%k;
                        if(d > f[pos][nr])
                        {
                            f[pos][nr] = d;
                            rec[pos][nr] = r;
                        }
                    }
                }
            }
        }
        int rm = 0;
        for(int i = 1;i <= m;i ++)
        {
            half += char(f[i][rm] + '0');
            rm = rec[i][rm];
            //for(int j = 0;j < k;j ++) cout << f[i][j] << ' ';
            //cout << endl;
        }
        ans = half;
        half = "";
        if(n&1) m --;
        for(int i = m - 1;i >= 0;i --)
            half += ans[i];
        ans += half;
        return ans;
    }
};

```