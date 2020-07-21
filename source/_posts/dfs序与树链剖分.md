---
title: dfs序与树链剖分
date: 2020-07-21 05:08:08
tags: 笔记
---

# 核心知识点
树链剖分主要可以解决一类树上的值问题，相当于把树上问题转换成了链上的问题。
<!--more-->

# dfs序
![例题, https://yugu-cloud-disk.oss-cn-shanghai.aliyuncs.com/live/yugu20stg6-cdsfcesf/slide.pdf?OSSAccessKeyId=LTAI4FsiWjpNs1epYQp3d1Ag&Expires=1595346103&Signature=0ChdqkRUjdEa%2FXxmb7GSYTg3Dzo%3D page 98](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200721175943.png)

如果操作只涉及子树，那么我们只需要使用**点在dfs序的位置**内就可以完成操作了。
## 概念
dfs序，即遍历一个树的顺序。
那么，先根还是后根呢？答案是，先根（想一想，为啥呢
## 性质
每个点的子树，对应 DFS 序上一段连续的区间。<br/>
进入点 x 的时间为 dfnx，离开 x 的子树的时间为 $\rm dfn_x + size_x -1$<br/>
点 x 的子树在 DFS 序上对应的区间为 $\rm [dfn_x, dfn_x + size_x − 1]$.<br/>
按照 DFS 序维护点的相关信息，子树操作变为序列区间操作。~~(win!)~~

<p align="right">--Powered by dalao cdsfcesf</p>

## 具体实现

对一棵树进行 DFS。每访问到一个新的节点，记录下它的编号。</br>
可以得到一个长为 n 的排列，称为这棵树的一个 DFS 序。</br>
同时得到点 x 在 DFS 序中的位置编号$dfn_x$


```cpp
vector<int> G[maxn];
int dfn[maxn],size[maxn],cnt,fa[maxn],a[maxn],arr[maxn];
void dfs(int u)//用于初始化
{
    dfn[u] = cnt;size[u] = 1;a[cnt++] = arr[u];
    for(auto v : G[u])
        if(v != fa[u])
            fa[v] = u,dfs(v),size[u] += size[v];
}
void add(int u,int p)//u和他的所有子节点加上p
{
    add(dfn[u],dfn[u] + size[u] - 1, p);//调用线段树add
}
```

<textarea width=100% height=60px>Your note here</textarea>

# 树链剖分

在学习他之前，我们先来了解几个概念。

## 基本概念

1. 重链和轻链

    先看看这个树：![烂树](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200721192127.png)
    哦不,是这个：![好树](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200721191924.png) 
    对于一个节点u,他的孩子v中，size[v]最大的v叫儿子，其他叫女儿。（陈旧观念严重）<br/>
    （好吧，官方的叫法为轻儿子和重儿子）<br/>
    他与儿子的连边即为重边。
    重边的连边即为重链。
    在从根到子节点，或从任意一点到他的子节点的路径上，重链的数量不会多于$\log(n)$条。<br/>轻边数量的级别也是 $O(\log n)$。<br/>下面给出证明：
    > 即得易见平凡，仿照上例显然。留作习题答案略，读者自证不难。

2. LCA
   
    LCA就是最近公共祖先的意思。
    >若 $y$ 在 $x$ 到根的路径上，且 $x \neq y$ ,那么 $y$ 是 $x$ 的祖先<br/>
    >$x$ 的所有祖先是 $x$ 到根的路径上除 $x$ 之外的所有点。<br/>
    >求点 $z$ 满足 $z$ 是 $x$ 和 $y$ 共同的祖先，且深度最大。则称 $z$ 为 $x$ 和 $y$ 的最近公共祖先 (LCA)。

    比如说这个图：
    ![](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200721194809.png)
    其中，10 12的公共祖先为3 <br/>
    13 14的公共祖先为4 <br/>
    7 8的公共祖先为4 <br/>

## 树链剖分具体实现

1. 函数部分
    需要使用两遍dfs,
    首先 DFS 一次，求出每个点的子树大小和重儿子。
    再进行一次 DFS 记录 DFS 序。到达一个点时，先访问重儿子，
    再访问其余儿子。
    对一条重链，链上的点在 DFS 序中一定相邻。
    因此一条重链对应 DFS 序中一段区间。
    可用数据结构按照 DFS 序维护相关信息。
2. 变量部分
   DFS 两次，求出重要的量：
    $\rm son_x$：$x$ 的重儿子
    $\rm dfn_x$：$x$ 在 DFS 序中的编号
    $\rm top_x$：$x$ 所在重链的链顶
    若 $(x, y)$ 是重边，则 $top_y$ = $topx$；
    若 $(x, y)$ 是轻边，则 $top_y$ = $y$。
3. 参考代码:
   ```cpp
   void dfs1(int x)
    {
    	siz[x] = 1;
    	for(auto y:G[x])
    		if(y!=fa[x])fa[y]=x,d[y] = d[x] + 1,dfs1(y,x),siz[x]+=siz[y];
    	int mx = 0;
    	for(auto y:G[x])
    		if(y!=fa[x]) if(mx<siz[y])mx=siz[y],son[x] = y;
    }
    void dfs2(int x,int pa)
    {
    	dfn[x] = ++cnt;
    	if(son[x])
    		top[son[x]] = top[x],dfs2(son[x],x);
            //传递
    	for(auto y:G[x])if(y!=fa[x] && y!=son[x])top[y] = y,dfs2(y,x);
    }
    ```

## 树链剖分解决实际问题

如，求lca.
```cpp
int lca(int x,int y)
{
	while(top(x) != top(y)){
		if(d[top[x]] < d[top[y]])swap(x,y);
		x = fa[top[x]];
	}
	return (d[x] < d[y])?x:y;
}
```
练习：
   <https://www.luogu.com.cn/problem/P3384>