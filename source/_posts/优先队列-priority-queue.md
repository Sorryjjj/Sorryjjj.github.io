---
title: 优先队列 priority_queue
date: 2020-03-17 11:07:28
tags: algorithm
categories: algorithm
---

# 概念

队列的一种，其中优先队列的出队顺序是按照优先级来的，其中最重要的元素始终位于前面。

无论入队顺序，当前优先级最高（或最低）的元素优先出队，保持一种动态的有序性，对于不断改变有入队的操作，而又需要某种最大或最小的操作的问题是再合适不过。通常优先队列的实现是由最小堆或者最大堆完成的，并通过堆序保持队列的有序性，模拟队列的结构。

# 用法

```c++
#include<queue>
using namespace std;
priority_queue<结构类型> 队列名;
```

结构类型常用自定义结构体，并重载小于符号。

```c++
struct node {
  int x,y;
  bool operator < (const node &a) {
    return x < a.x;
  }
};
```

优先队列对于需要处理（大量）项目的算法以及反复需要识别哪个项目现在最大或最小——或者是定义“最重要”的算法非常有用。如：

- 事件驱动的模拟
- Dijkstra的图搜索算法
- 霍夫曼编码（若使用常规队列或者普通数组，需要反复扫描整个序列以查找目标项目。）

# 操作

假设有一优先队列q

```c++
q.size();//返回q里元素个数 
q.empty();//返回q是否为空，空则返回1，否则返回0 
q.push(k);//在q的末尾插入k 
q.pop();//删掉q的第一个元素 
q.top();//返回q的第一个元素
```

入队、出队、查找最小值或最大值

更改优先级：算法决定元素在队列中变得更重要

# 题目中的应用

## 题目来源

[POJ 1365 **Full Tank**](http://poj.org/problem?id=3635)

## 题意

给 n 个城市 m 条路，每个城市有个加油站，但是收费单价不同，现给你几个查询并给定汽车的装油量， 问你从 S 城市到 T 城市的最小费用。

## 思路

题意比较明确。给定N(N&lt;=1000)个点的图，要求从S到E花费最少。
其中每个点可以加油，给出每个点的油价。一个单位距离消耗一个单位油。车辆有最大储存油量。

可以把这道题目理解为是一个二维的最短路，其中这个“路”在这里并不是两点之间距离，而是两点之间花费值。

而我们在每一个点可以选择加油或者走向下一个点。因为加油量始终为整数，所以我们可以一次只加一单位油。设dp[u][f]表示在u节点，当前油量为f的最小花费(可以理解为从s到u的最短路)

那么之后的扩展有两个选择：

1.如果在城市u的顶点dp[u][f]加油更优（假如已经由之前某个城市更新了到这个城市的最小花费）,那么我们在u加一单位油。(这是dij求最短路的松弛操作，注意求同存异）

2.可以从城市u转移到城市v。并且由u转移到v更优（同样是松弛操作）。那么我们就更新v城市状态dp[v][f’],这里f’是u点的油量f减去u-v的路径长度。

## 代码

```c++
#include<iostream>
#include<cstdio>
#include<vector>
#include<cstring>
#include<queue>
using namespace std;

const int maxn=1e3+1e2;

int G[maxn][maxn]; //边权图
vector<int>link[maxn];
int p[maxn];    //油价
int dp[maxn][110];  //dp[i][j]表示在i点，油量为j时的最小花费
int n,m;


struct Node{  //状态
      int pos,fuel,cost;
      Node(){}
      Node(int p,int f, int c)
      {
          pos = p;
          fuel = f;
          cost = c;
      }
      bool operator < (  const Node& rhs )const{
            return rhs.cost < cost;
      }
};



int BFS(int s, int e, int cap)
{
      memset(dp, 127, sizeof dp);   //表示把所有的值预设为不可达状态，INF
      Node now(s,0,0);
      dp[s][0]=0;
      priority_queue<Node>q;
      q.push(now);
      while(!q.empty())
      {
        Node now=q.top(); q.pop();
        int u=now.pos;
        if( u == e )return now.cost;
        //试着加一升油
        if(now.fuel < cap && dp[u][now.fuel + 1]> dp[u][now.fuel]+p[u]  )
        {
          dp[u][now.fuel + 1]=dp[u][now.fuel] + p[u];
          Node rhs(u, now.fuel+1, now.cost + p[u]);
          q.push(rhs);
        }
        //u -> v
        for( int i=0;i<link[u].size();i++ )
        {
          int v=link[u][i];
          int v_fuel=now.fuel-G[u][v] * 1;
          if(v_fuel < 0)continue;
          if(dp[u][now.fuel] < dp[v][v_fuel])
          {
            dp[v][v_fuel] = dp[u][now.fuel];
            Node rhs(v, v_fuel, now.cost);
            q.push(rhs);
          }
        }
      }
      return -1;
}

int main()
{
      while(cin >> n >> m)
      {
        memset(G, 0, sizeof G);
        for( int i=0;i<n;i++ ) cin>>p[i];//顶点从0开始编号
        for( int i=1;i<=m;i++ )
        {
          int u,v,d;
          cin>>u>>v>>d;
          G[u][v]=G[v][u]=d;
          link[u].push_back(v);
          link[v].push_back(u);
        }
        int q; cin>>q;
        for( int i=1; i<=q;i++ )
        {
          int s,e,cap;
          cin>>cap>>s>>e;
          int ans=BFS(s,e,cap);   //从s到e，在油箱容量为c的条件下的最小油费
          if(ans == -1)cout<<"impossible\n";
          else cout << ans <<endl;
        }
      }
      return 0;
}
```

