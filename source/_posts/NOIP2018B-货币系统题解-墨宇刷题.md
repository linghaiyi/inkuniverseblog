---
title: '[NOIP2018B]货币系统题解-墨宇刷题'
date: 2020-07-06 15:38:23
tags: 题解
---

核心思想：硬币问题，小集表示大集

# 题目描述
<!--more-->
![](https://gitee.com/inkuniverse/picture_bed/raw/master/img/20200706155007.png)

# 思路

细想后发现，{3,9,10,6}变成了 {3,10},是不是因为9=3+3+3,6=3+3?

哦，只要大集里的数可以被其他小数表示出来，这个数就可以被删掉了。

于是，奇怪的算法诞生了！

# 算法

如果我们枚举，两重循环，会发现，9=3+3+3这种不可以。

~~难道要我dfs?~~

###递推！

记
$$
f[x]=\left\{
\begin{aligned}
0&\quad 没凑出\\
1&\quad 凑出了\\
2&\quad 本来就有，没有凑出。
\end{aligned}
\right.
$$

易证
$$
f[i+a[j]] = 1 \quad(f[j] > 0,a[]是原数)
$$

~~f[i]可以被凑出，f[i+a[j]]当然也可以被凑出~~

参考代码：

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int maxn = 110;
int f[25100], T, n, a[maxn];
int ans;
int main(void)
{
    scanf("%d", &T);
    while (T--)
    {
        memset(f,0,sizeof f);

        ans = 0;
        scanf("%d", &n);
        for (int i = 1; i <= n; i++)
        {
            scanf("%d", a + i);
            f[a[i]] = 2;
        }
        sort(a+1,a+n+1);
        for(int i = 1;i <= a[n];i++)
            if(f[i] > 0) for(int j = 1;j <= n;j++)
                if(i + a[j] <= a[n])f[i+a[j]] = 1; else break;
        
        for(int i = 1;i <= a[n];i++)
            if(f[i] == 2)ans++;
        cout<<ans<<endl;
    }
}
```

[便捷下载1](https://raw.githubusercontent.com/inkuniverse/OI-code/ded21cf62cfbfc89ed5fb80e14c1bd88f80b91cf/OI-Code.cpp)
[便捷下载2](https://raw.staticdn.net/inkuniverse/OI-code/ded21cf62cfbfc89ed5fb80e14c1bd88f80b91cf/OI-Code.cpp)