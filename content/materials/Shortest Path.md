---
title: Shortest Path
draft: false
tags:
  - material
  - graph
  - graph/shortest-path
---
### 最短路的基本性质

- ***所以在有负环的图中不存在最短路***
- 三角不等式性质：$\forall (u,v) \in E,\delta(s,v) \leq\delta(s,u)+w(u,v)$
- 最短路的最优子结构：两个结点点之间的一条最短路径包含其他的最短路径 ^311b6a

---

### Floyd

#### 性质

- Floyd算法可以求出**任意两点之间的最短路**
- 不能跑负权图
- 动态规划

#### 算法实现

1. 初始化 $f[a][b]=map[a][b],e(a,b)\in E$（$a,b$之间有一条边）
	&emsp;&emsp;&emsp;$f[x][y]=\infty,e(x,y)\notin E$（$x,y$之间没有边）
2. 枚举中间点 $k$ , 枚举起点$i$ , 枚举终点$j$
3. 松弛操作 $f[i][j]=min(f[i][j],f[i][k]+f[k][j])$

#### 时间复杂度

$O(n^3)$

```c++
void Floyd
{
	for(int k = 1;k <= n;k ++)
    	for(int i = 1;i <= n;i ++)
        	for(int j = 1;j <= n;j ++)
            	f[i][j] = min(f[i][j],f[i][k] + f[k][j]);
    return ;
}
```



---

### Dijkstra

#### 性质
- Dijkstra是用于求**单源最短路**的，也就是**一个起点，$n-1$个终点**
- 不能跑带负权的图
- 贪心算法

#### 算法实现 
1. 找离起点最近的白点$u$
2. 用松弛操作更新与$u$相邻的点$v$，$d(v)=min(d(v),d(u)+w(u,v))$，$w(u,v)$是$u$到$v$的边权
3. 给$u$打上标记，划为“蓝点”，不再访问
4. 回到步骤1

#### 时间复杂度

朴素 $O(n^2)$

优先队列优化 $O((n+m)\log m)$

```c++
void Dijkstra(int s)
{
    priority_queue<pair<int ,int > > q;
    bool vis[N];int dis[N];
    for(int i = 1;i <= n;i ++) vis[i] = 0,dis[i] = inf;
    q.push(make_pair(0,s));dis[s] = 0;
    for(;!q.empty();)
    {
        int u = q.top().second;
        q.pop();
        if(vis[u]) continue;
        vis[u] = 1;
        for(int i = head[u];i;i = e[i].nxt)
        {
            int v = e[i].to;
            if(dis[v] > dis[u] + e[i].val)
            {
                dis[v] = dis[u] + e[i].val;
                q.push(make_pair(-dis[v],v));
			}
        }
    }
    return ;
}
```



---

### Bellman-Ford

#### 性质
- 用于求单源最短路，~~爸爸~~本质是Bellman-Ford算法，请自行了解。
- 可以判断负环。
- 可以跑负权图。

#### 算法实现(队列优化)
1. 取队头，获得当前的位置$u$
2. 枚举所有出边，做一遍松弛操作
3. 如果某条边$e(u,v)$可以做松弛操作，检查$v$是否入队，如果没有，就将$v$入队了
4. 回到步骤1，直至队列取空

#### 时间复杂度

队列优化 最好情况下 $O(kn)$ ，其中 $k$ 为较小的常数，最坏 $O(nm)$

```c++
void SPFA(int s)
{
    queue<int > q;
    q.push(s);dis[s] = 0;
    inq[s] = 1;
    for(;!q.empty();)
    {
        u = q.front();
        q.pop();
        inq[u] = 0;
        for(int i = head[u];i;i = e[i].nxt)
        {
            v = e[i].to;
            if(dis[v] > dis[u] + e[i].val)
            {
                dis[v] = dis[u] + e[i].val;
                if(!inq[v])
                {
                    inq[v] = 1;
                    q.push(v);
                }
            }
        }
    }
    return ;
}
```

---

### 最短路的应用

- 严格次短路

  到某个顶点$v$的次短路要么是到其他顶点$u$的最短路再加上$u\rightarrow v$的边，要么是到$u$的最短路再加上$u\rightarrow v$的边
  
  即满足
  $$
  \begin{cases}
  dis2(v)=\min\{dis(u),dis2(u)\}+w(e_{u\rightarrow v}) \\ 
  dis2(v)>dis(v)
  \end{cases}
  $$
  更新的时候就记录次短路和最短路，最后输出次短路就好了

- 最短路计数

  将 DP 加载到最短路算法上

  1. 有一个$u$，$dis(u)+w(e_{u\rightarrow v}) < dis(v)$，此时到$v$的最短路需要更新，所以$dis(v)  = dis(u) + w(e_{u\rightarrow v}),f(v) = f(u)$，既然最短路更新了，那么最短路的数目也更新了。
  2. 有一个$u$，$dis(u)+w(e_{u\rightarrow v}) = dis(v)$，此时显然有$f(v)=f(v)+f(u)$

- 最短路上 DP

  可以先跑一遍最短路，然后根据距离确定 DP 的顺序。有时候会将图分层，像拓扑排序一样，每个结点都有一个类似于深度的数据。

- 建图，建立虚拟边或虚拟源点

  技巧1：点权转边权后可以使用最短路算法。

  技巧2：建立虚拟源点：建立一个虚拟源点，然后将所有点向它连边，如果新边边权全为 0 ，那么跑一遍最短路得到的 $dis(i)$ 表示结点 $i$ 到图中离它最近的结点的距离。根据题意可以巧妙设置新边边权，从而以一次最短路的时间计算所有结点的答案。

  技巧3：建一个与原图的边全部相反的图，可能有用。

  技巧4：建立等效边：当图中出现非常规计算路径长度的时候，可以考虑将所有非常规的路径长度计算出来，然后建立常规的等效边，然后就可以在图上跑最短路了

- 传递闭包

  意思就是计算一个表示结点之间能否到达的矩阵，Floyd

- 判负环

  SPFA 算法中如果某个点弹出队列的次数超过 $n -1$ 次，则存在负环

- 分层思想

  对于图中的边会随遍历过程而变化（例如走到某些特定结点后，就会新增边；或者经过不同的结点，相同的边的边权会改变）的图，我们可以考虑采用分层思想。主要步骤是对每一种状态都生成一张完整的图，形成了对应不同状态的层，然后抓住层与层之间的关系，建立层与层之间的边。
 ^712498
- 最小割问题可以将平面图转成对偶图然后跑最短路。
