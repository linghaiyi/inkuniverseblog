---
title: zroicspday3T2
date: 2021-09-12 09:13:21
tags:
---

# CSP 7 连测 day3 T2 总结
PS: 此题就是点灯游戏的最小步数问题

算法很简单，由于第一行确定后，下面的怎么放就确定了
于是，就只要枚举第一行，就可以确定以后的行，然后判断有没有成功就可以了。
时间复杂度 $O(2 ^ n n ^ 2)$ , $n \le 17$ 因此可以过

当时在赛场上面没有考虑到点当前行会影响下一行，只得了10分.

满分代码:
```cpp
#include <bits/stdc++.h>
using namespace std;
const int dx[5] = {1, -1, 0, 0, 0}, dy[5] = {0, 0, 1, -1, 0};
int n, m, ans, sum, a[20][20], b[20][20];
char s[20];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
    {
        scanf("%s", s + 1);
        for (int j = 1; j <= m; j++)
            if (s[j] == 'X')
                a[i][j] = 1;
    }
    ans = 1e9 + 7;
    for (int i = 0; i < (1 << m); i++)
    {
        sum = 0;
        for (int j = 1; j <= n; j++)
            for (int k = 1; k <= m; k++)
                b[j][k] = a[j][k];
        for (int j = 1; j <= m; j++)
            if (i & (1 << (j - 1)))
            {
                sum++;
                for (int i = 0; i < 5; i++)
                {
                    int xx = 1 + dx[i], yy = j + dy[i];
                    if (xx < 1 || xx > n || yy < 1 || yy > m)
                        continue;
                    b[xx][yy] ^= 1;
                }
            }
        for (int j = 2; j <= n; j++)
            for (int k = 1; k <= m; k++)
                if (b[j - 1][k])
                {
                    sum++;
                    for (int i = 0; i < 5; i++)
                    {
                        int xx = j + dx[i], yy = k + dy[i];
                        if (xx < 1 || xx > n || yy < 1 || yy > m)
                            continue;
                        b[xx][yy] ^= 1;
                    }
                }
        bool tmp = 1;
        for (int j = 1; j <= n; j++)
        {
            for (int k = 1; k <= m; k++)
                if (b[j][k])
                {
                    tmp = 0;
                    break;
                }
            if (!tmp)
                break;
        }
        if (tmp)
            ans = min(ans, sum);
    }
    printf("%d\n", ans);
    return 0;
}
```