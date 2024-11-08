---
title: Super Brief Introduction to Time Complexity
draft: false
tags:
  - material
  - time_complexity
---

[TOC]
### Preface?

I have thought about writing this tutorial in English. But in regard of my poor English proficiency, I would consider writing it half in English and half in Chinese.

Well then, let's start QwQ...

### Why do we need to take time into consideration

The problems of time limits have occurred in the early stage of computer's development. When CPU has not been developed as powerful as today, it took a really long time to run a programme and get an output. So it's natural to think about how to reduce the running time so as to get your output as quickly as possible. 

As for programme contest, many problems declare time limits for running your code and if your programme is still running when your running time has exceeded time limits, you will not get AC but TLE. 



### What does time mean to programme 

What determines the running time of a programme? Well, it's the number of  instructions a program executed. The running time is longer when your program needs to execute more instructions.

Since this tutorial is super brief, I'd like to give a not so rigorous saying of time complexity. According to experience, what we mainly care about is the approximation of running time and we take it as the time complexity of a programme.

Here we give a rough definition of the approximation of a programme's running time.

- the number of the total instructions of the most complex part of a programme matters.

Let's suppose the number is $K$. Now we introduce the notation of $O()$.

- we say $O(K)$ is the running time, namely the time complexity of a programme.



### What does $K$ exactly mean

I will show you some examples of calculating $K$.

##### Example 1

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int main()
{
    int a = 0;int b= 0 ;
    cin >> a >> b;
    cout << a + b;
    return 0;
}
```

Let's focus on function ```main()```  and we find that ```int a = 0;``` ```int b = 0;``` ```cin >> a >> b;``` ```cout << a + b;``` are the same as complex because they are all one-sentence codes.

so the complexity of this programme is $O(1)$.

- note: focus on functions .



##### Example 2

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int n;
int main()
{
    cin >> n;
    for(int i = 1;i <= n;i ++)
    {
        cout << i;
    }
    return 0;
}
```

The most complex part of this programme is the ```for``` block. It has $n$ instructions exactly so the time complexity is $O(n)$.

How about this?

```c++
int sum = 0;
for(int i = 1;i <= n;i ++)
{
    sum = sum + i;
    cout << sum << endl;
}
```

It seems like $O(2n)$ , right? but when $n$ comes to super big such as $10^7$ , $2$ is too small compared to $n$ so we often ignore constants in time complexity analysis. So we often agree that it's $O(n)$. Nevertheless, it's undeniable that we sometimes encounter problems where constants can't be ignored.

And how about this?

```c++
for(int i = 1;i <= n;i ++)
    pre[i] = pre[i - 1] + a[i];
for(int i = n;i >= 1;i --)
    suf[i] = suf[i + 1] + a[i];
```

It is also $O(n)$.

- note: focus on major block and often ignore constants.
- If there are some parts whose $K$ are $K_1,K_2,...,K_n$ respectively, you are allow to assume that $K$ of the whole programme is $\max_{i=1}^{n}\{K_i\}$



##### Example 3

```c++
for(int i = 1;i <= n;i ++)
{
    cnt = 0;
    for(int j = 1;j*j <= i;j ++)
    {
        if(i % j == 0)
        {
            if(j * j == i) cnt ++;
            else cnt += 2;
        }
    }
}
```

Here the outer ```for``` block for ```i``` will loop for $n$ times and the inner ```for``` block for ```j``` will loop for $\sqrt{i}$ times.

As for rough approximation, we reckon the time complexity is $O(n\sqrt{n})$ . Of course, you can make a more precisely approximation like this 
$$
O(\sum_{k=1}^n\sqrt{k})
$$
But it's not easy to calculate this sum while $n\sqrt{n}$ is easier to calculate. Later I'll talk about why we need to turn formula into number.

- note: when it comes to loop in loop, consider the multiplication principle of combinatorial mathematics.
- make rough approximation is enough.



### How does notation $O$ turn $K$ into time

We are talking about asymptotic complexity in this tutorial and notation $O$ is sometimes written as $\Theta$ or other symbols. And more formally, $O$ represents upper-bound complexity, $\Omega$ represents lower-bound complexity.

Anyway, we just accept the notation $O()$ as a function which turns the number of instructions $K$ into the running time of a programme.

Nowadays, contemporary PCs have a powerful performance and we have a table as below

时间限制只有 1s, 程序时间复杂度为 $O(n)$

|  $n$   |   效果   |
| :----: | :------: |
| $10^6$ | 游刃有余 |
| $10^7$ |   可以   |
| $10^8$ | 勉勉强强 |

Since we have a table as above, we can calculate $K$ to decide whether our programmes can give us a result in 1s or not.



### Application of time complexity

Now we know something about estimating the running time of our programmes and you have a better understanding of your own codes.

According to ranges of problems' data, we can guess a possible correct algorithm or data structure and work towards that way to solve the problem.



### What's more...

Learn more about time complexity by searching materials online.

[复杂度 - OI Wiki](https://oiwiki.org/basic/complexity/)

Or reading books like 《算法导论》...

Even learn some techniques to lower time complexity such as fast read, $\rm{O_2}$ optimization ...

### Exercises

1. Read the code as below

   ```c++
   #include<iostream>
   #include<cstdio>
   using namespace std;
   int n,m;
   int main()
   {
       cin >> n >> m;
       //find 3 numbers whose sum equals to m
       for(int i = 1;i <= n;i ++)
           for(int j = i + 1;j <= n;j ++)
               for(int k = j + 1;k <= n;k ++)
                   if(i + j + k == m)
                   {
                       cout << i << ' ' << j << ' ' << k << endl;
       				break;
   				}
       cout << "-----------------------------------" << endl;
       //find 2 numbers whose product equals to m
       for(int i = 1;i <= n;i ++)
           for(int j = 1;j <= n;j ++)
               if(i * j == m)
                   cout << i << ' ' << j << endl;
       return 0;
   }
   ```

   (1) Estimate the time complexity of this programme.
   
   >[!hint]- Answer
   >$O(n^3)$

   (2) Do you have a better solution to reduce the time complexity? If you have, show your solution and the time complexity of your solution.
   
   >[!hint]- Hint
   >
   >Use datastructure or techniques.
   >
   >Can be reduced to $O(n^2)$

2. Read the code as below

   ```c++
   #include<iostream>
   #include<cstdio>
   using namespace std;
   long long modn = 998244353;
   int T;
   long long a,b,ans;
   int main()
   {
       cin >> T;//there are T cases
       while(T)
       {
           T --;
           cin >> a >> b;ans = 1;// calculate (a^b)mod 998244353
           for(int i = 1;i <= b;i ++)
               ans = (ans * a)%modn;
           cout << ans%modn << endl;
       }
       return 0;
   }
   ```

   $1\leqslant T\leqslant 10^5,1\leqslant a,b\leqslant 10^9$

   (1) Estimate the time complexity of this programme and judge whether it's a good solution with the time limit of 1s.
   
   > [!hint]- Answer
   > 
   > $O(Tb)$
   > 
   > $\max(Tb) = 10^{14} > 10^7$
   > 
   > Not a good solution.

   (2) Do you have a better solution? Tell me your solution and analyse the time complexity of your solution.
   
   > [!hint]- Hint
   > Quick power algorithms
   > 
   > $O(\log b)$

3. Read the code as below

   ```c++
   #include<iostream>
   #include<cstdio>
   using namespace std;
   const int maxn = 1e5;
   int a[maxn + 5],n,b,ans;
   
   void solve(int l,int r)
   {
       int mid = (l + r)/2;
       if(a[mid] > b)
       {
           if(mid == 1) ans = 0;
           else solve(l,mid);
       }
       else
       {
           if(mid == n) ans = mid;
           else
           {
               if(b > a[mid + 1]) solve(mid + 1,r);
               else ans = mid;
           }
       }
       return ;
   }
   
   int main()
   {
       cin >> n;
       for(int i = 1;i <= n;i ++)
           cin >> a[i];// read a[i]
       //guarantee that a[i] <= a[i + 1]
       cin >> b;
       //if we insert b into {a}
       //and maintain the property that a[i] <= a[i + 1]
       //where should b settle down
       solve(1,n);
       cout << "between "<<ans << " and " << ans + 1;
       return 0;
   }
   ```

   Estimate the time complexity.
   
   > [!hint]- Answer
   > $O(\log n)$

4. Read the code as below

   ```c++
   #include<iostream>
   #include<cstdio>
   
   using namespace std;
   const int maxn =1e5;
   int n,p[maxn + 5];
   bool vis[maxn + 5];
   
   void Getpremutations(int now)
   {
       if(now == n + 1)
       {
           for(int i = 1;i <= n;i ++)
               cout << p[i] << ' ';
          	return ;
   	}
       for(int i = 1;i <= n;i ++)
       {
           if(!vis[i])
           {
               p[now] = i;
               vis[i] = 1;
               Getpermutations(now + 1);
               vis[i] = 1;
               p[now] = 0;
           }
   	}
   }
   
   int main()
   {
       cin >> n;
       Getpermutations(1);
   	return 0;
   }
   ```

   Estimate the time complexity.
   
   >[!hint]- Answer
   > $O(n\cdot n!)$
