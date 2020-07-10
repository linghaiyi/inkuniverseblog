---
title: DP浅谈
date: 2020-06-21 21:25:19
tags: 笔记
---

闫氏DP分析法
![](https://gitee.com/inkuniverse/picture_bed/raw/master/img/2020-05-17T14_10_54.png)

# 树形dp

## 1.农场物语
<!--more-->
[P3574 POI2014](https://www.luogu.com.cn/problem/P3574)

![](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200621090519.png)

简单思路:
我们记$g(i)$为遍历i的子树的最短时间。

$f(i)$为遍历i的子树，且每人装好电脑的最短时间。

显然有，$f(i)>g(i)$

发现$f(i)-g(i)$这段时间是用来等待的。

参考代码

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
const int maxn = 500010;
int n,a[maxn],f[maxn],g[maxn],s[maxn];
//f[i]表示遍历时间+子树所有居民装好电脑的时间
//g[i]表示遍历时间
vector<int> G[maxn];

void dfs(int u,int fa)
{
	if(u != 1)f[u] = a[u];
	for(auto v:G[u])if(v != fa)dfs(v,u);
	int cnt = 0;
	for(auto v:G[u])if(v != fa)s[cnt++] = v;//保存儿子
	sort(s+1,s+cnt+1,[](int a,int b){return f[a]-g[a] > f[b]-g[b];});
	for(int i = 1;i <= cnt;i++)
	{
		f[u] = max(f[u],f[s[i]]+g[u]+1);
		g[u] += g[s[i]] + 2;//更新g[]
	}
}
int main(void)
{
	scanf("%d",&n);
	for(int i = 1;i <= n;i++)
		scanf("%d",a + i);
	for(int i = 1;i < n;i++)
	{
		int u,v;
		scanf("%d%d",&u,&v);
		G[u].push_back(v);
		G[v].push_back(u);
	}
	dfs(1,-1);//g[1]+a[1]是特判
	printf("%d",max(f[1],g[1]+a[1]));
	return 0;
}
```

[github raw下载](https://raw.githubusercontent.com/inkuniverse/OI-code/a12aeeb02e5ed5bdd7257b7b7d7fd1c70dec5185/OI-Code.cpp)

[github raw cdn下载](https://raw.staticdn.net/inkuniverse/OI-code/a12aeeb02e5ed5bdd7257b7b7d7fd1c70dec5185/OI-Code.cpp)

