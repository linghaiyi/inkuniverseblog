---
title: zroi-8-12

date: 2021-08-12 19:05:27
tags: 赛后总结
---

### T1 如烟
<!-- more -->
很简单的最短路题目，思维上有点难
考虑$dp[x][y]$表示$(x,y)$是否是一个合法的点对，初始状态是
$dp[x][x] = 1(1\le x \leN)$， 然后考虑假如(x,y)是一个可行的点对，
那么一定存在一个x的前驱x’戒者y的前驱y’， 使得(x’,y)是一
个可行的点对，戒者(x,y’)是一个可行的点对。 亍是我们直接跑一遍递推，复杂度为$O(N^2)$

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 3000 + 10;
const int inf = 0x7fffffff;
struct Edge
{
    int ne, e;
} edge[N << 1], rdge[N << 1];
int h[N], num_edge = 1;
inline void add_edge(int f, int e)
{
    edge[++num_edge].ne = h[f];
    edge[num_edge].e = e;
    h[f] = num_edge;
}
int rh[N], num_rdge = 1;
inline void add_rdge(int f, int e)
{
    swap(f, e);
    rdge[++num_rdge].ne = rh[f];
    rdge[num_rdge].e = e;
    rh[f] = num_rdge;
}
int n, m, sc, tot;
bool vis[N];
vector<int> g;
void rfs(int u, int fa)
{
    int v;
    if (vis[u]) return;
    vis[u] = 1;
    g.push_back(u);
    for (int i = rh[u]; i; i = rdge[i].ne)
    {
        v = rdge[i].e;
        if (v == fa) continue;
        rfs(v, u);
    }
    return;
}
void dfs(int u, int fa)
{
    int v;
    if (vis[u])
        return;
    tot++;
    vis[u] = 1;
    for (int i = h[u]; i; i = edge[i].ne)
    {
        v = edge[i].e;
        if (v == fa) continue;
        dfs(v, u);
    }
    return;
}
int T, x, y, z;
int main()
{
    scanf("%d", &T);
    while (T--)
    {
        long long ans = 0;
        scanf("%d%d", &n, &m);
        memset(h, 0, sizeof(h));
        memset(rh, 0, sizeof(rh));
        num_edge = num_rdge = 1;
        for (int i = 1; i <= m; i++)
        {
            scanf("%d%d", &x, &y);
            add_edge(x, y);
            add_rdge(x, y);
        }
        for (int i = 1; i <= n; i++)
        {
            memset(vis, 0, sizeof vis);
            g.clear();
            rfs(i, 0);
            memset(vis, 0, sizeof vis);
            for (int j = 0; j < g.size(); j++)
            {
                tot = 0;
                dfs(g[j], 0);
                ans += tot;
            }
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```

### T2 星空
用并查集维护联通块， 然后数一下每个联通块
里面的边数，对亍每个联通块如果有偶数条边， 答案不变，否则答案+1
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000010;
int fa[N], siz[N], n, m, ans;
bool vis[N];
int get(int x)
{
    if (x == fa[x]) return fa[x];
    else
    {
        int root = get(fa[x]);
        fa[x] = root;
        return root;
    }
}
void merge(int x, int y)
{
    int fx = get(x), fy = get(y);
    if (fx == fy)
        siz[fx]++;
    else
    {
        fa[fx] = fy;
        siz[fy] += siz[fx] + 1;
    }
}
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
        fa[i] = i;
    for (int i = 1, a, b; i <= n; i++)
    {
        scanf("%d%d", &a, &b);
        merge(a, b);
    }
    for (int i = 1; i <= n; i++)
    {
        if (i == fa[i])
            ans += siz[get(i)] % 2;
    }
    printf("%d", ans);
    return 0;
}
```

### T3
重点是DP时考虑质因数分解
```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 100 + 10;
int n, k;
map<int, int> m[N];
int dfs(int x, int y)
{
    if (m[x].find(y) != m[x].end())
        return m[x][y];
    if (x == 1)
        return m[x][y] = y;
    m[x][y] = y;
    for (int i = 2; i <= 5; i++)
        m[x][y] = min(dfs(x - 1, y / i) + (y % i), m[x][y]);
    return m[x][y];
}
signed main()
{
    cin >> n >> k;
    printf("%lld", dfs(k, n));
    return 0;
}
```

### T4 倔强
考试的时候想了一个假做法。。。10分
求出每个位置能放的数字有哪些，把位置和点进行二分图匹配。因为可以将一段的数字从小到大重排，
有解当且仅当存在完美匹配。所以最暴力的做法是从小到大枚举所有格子，枚举这个格子填哪个数，填
完之后重新跑一遍二分图匹配，如果有解就可以填。稍加优化，每次只尝试增广一个点，可以使每次判
断复杂度变为 O(N ^ 4) ，总复杂度 O(n ^ 8)  
```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 100 + 10;
int n, k;
map<int, int> m[N];
int dfs(int x, int y)
{
    if (m[x].find(y) != m[x].end())
        return m[x][y];
    if (x == 1)
        return m[x][y] = y;
    m[x][y] = y;
    for (int i = 2; i <= 5; i++)
        m[x][y] = min(dfs(x - 1, y / i) + (y % i), m[x][y]);
    return m[x][y];
}
signed main()
{
    cin >> n >> k;
    printf("%lld", dfs(k, n));
    return 0;
}
```


PS: 8-11是~~快乐~~的acm, 就不更了