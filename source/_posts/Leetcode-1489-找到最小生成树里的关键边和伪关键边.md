---
title: Leetcode.1489.找到最小生成树里的关键边和伪关键边
date: 2020-06-27 13:12:00
tags: 题解
---

![c++](https://img.shields.io/badge/lanuage-c++-blue) [![题解](https://img.shields.io/badge/type-题解-blue)](/tags/%E9%A2%98%E8%A7%A3/)

方法： 最小生成树算法。
<!--more-->

Steps: 
1. 使用最小生成树算法得到最小生成树的路径和minn。

2. 判断这条边是不是关键边：将这条边从路径中去除，然后利用最小生成树算法求路径和，如果路径和大于minn或者不连通，那么这条边就是关键边。

3. 判断这条边是不是伪关键边：首先调用上面的函数判断其是不是关键边，如果去除之后路径和不变，则说明其没在这里面（或可以被替代）。
   
4. 判断它可能会出现在某些最小生成树呢？只需要一开始将就这条边加入到最小生成树中，然后使用算法求路径和。如果路径和等于minn，则其就是伪关键边，否则就不是。

考虑边，所以使用Kruskal算法

```cpp
class Solution 
{
public:
    
    int p[110];
    
    
    int find(int a) 
    {
        if (a != p[a]) p[a] = find(p[a]);
        return p[a];
    }

    int work1(int n, vector<vector<int>>& edges, int k) 
    { // 不选第k条边的最小生成树的权重
        for (int i = 0; i < n; i ++ ) p[i] = i;
        int cost = 0, cnt = 0;
        for (auto& e:edges) 
        {
            if (e[3] == k) continue;  //  如果是第k条边，则跳过
            int f1 = find(e[1]), f2 = find(e[2]);
            if (f1 != f2) 
            {
                cost += e[0];
                cnt ++;
                if (cnt == n - 1) break;
                p[f1] = f2;
            }
        }
        if (cnt == n - 1) return cost;
        else return INT_MAX;
    }
    
    int work2(int n, vector<vector<int>>& edges, int k) 
    { // 一定选第k条边的最小生成树的权重
        for (int i = 0; i < n; i ++ ) p[i] = i;
        int cost = 0, cnt = 0;
        
        for (auto& e : edges) 
        {  // 先向第k条边加入到集合中
            if (e[3] == k) 
            {
                cost += e[0];
                cnt ++;
                p[e[1]] = e[2];
                break;
            }
        }
        
        for (auto& e: edges) 
        {
            int f1 = find(e[1]), f2 = find(e[2]);
            if (f1 != f2) 
            {
                cost += e[0];
                cnt ++;
                if (cnt == n - 1) break;
                p[f1] = f2;
            }
        }
        if (cnt == n - 1) return cost;
        else return INT_MAX;
    }
    
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) 
    {
            
        int m = edges.size();
        for (int i = 0; i < m; i ++ ) 
        {
            auto& e = edges[i];
            swap(e[0], e[2]);
            e.push_back(i);
        }
        sort(edges.begin(), edges.end());
        
        int minn = work1(n, edges, -1);
        vector<vector<int>> ans(2);
        for (int i = 0; i < m; i ++ ) 
        {
            if (work1(n, edges, i) > minn) ans[0].push_back(i);  // 判断是否为关键边
            else if (work2(n, edges, i) == minn) ans[1].push_back(i);  // 判断是否为伪关键边
        } 
        return ans;
        
    }
};
```