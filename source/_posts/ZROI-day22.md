---
title: ZROI-day22
date: 2021-08-08 10:47:42
tags: 笔记
---

# 数位DP
<!-- more -->
要求的数与数的每个数位相关是用数位DP求解

### T1 Windy 数
#### 题目背景
Windy 定义了一种 Windy 数
#### 题目描述
不含前导零且相邻两个数字之差至少为 2 的正整数被称为 windy 数。windy 想知道，在 a 和 b 之间，包括 a 和 b ，总共有多少个 Windy 数？
<!-- ![](https://i.loli.net/2021/08/08/quhatbOfiZmeyUv.png) -->
##### 样例输入1
```
1 10
```
##### 样例输出1
```
9
```
##### 样例输入2
```
25 50
```
##### 样例输出2
```
20
```

考虑：$a$ 到 $b$ 之间的 windy 数就是 $1$ 到 $b$ 之间的windy数减去 $1$ 到 $a-1$ 的 windy 数。

问题就转换为两个求 $1$ 到 $n$ 之间的 windy 数的数量的问题。

代码实现
```cpp
int main(void)
{
    int a, b;
    cin >> a >> b;
    cout << get(b) - get(a - 1);
}
```

这里 get 函数的作用就是传入一个正整数 $x$, 返回 $1$ 到 $x$ 之间 windy 数的数量。

那么 get 函数怎么实现呢？

令 $n$ 为所求的数

### Part 1
首先，我们考虑位数小于 $n$ 的个数
我们枚举位数，此时每个位都可以随便取。

考虑令 dp[i][j] 表示共有 $i$ 位数，且末尾是 $j$ 的windy 数的个数。

容易发现递推式子是:
$$
dp[i][j] = \sum_{|j - k| >= 2, j \text{满足条件} } dp[i - 1][k].
$$

答案求法
```cpp
for (int i = 1; i < nums.size(); i ++ ) // 答案小于 a.size() 位，随便取
    for (int j = 1; j <= 9; j ++ )
        res += f[i][j];
```

### Part 2

卡位的话，特殊处理一下，注意前导零

### 最终
```cpp

#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
const int N = 15;
int f[N][N];
void init()
{
    //f[i][j] 表示以 j 为开头，共 i 个字母的 Windy 数.
    for(int i = 0; i <= 9; i++) f[1][i] = 1;
    for(int i = 2;i < N;i++)
        for(int j = 0; j <= 9; j++)
            for(int k = 0; k <= 9;k++)
                if(abs(j - k) >= 2) 
                {
                    f[i][j] += f[i - 1][k];
                    // printf("f[%d][%d] = %d\n", i, j, f[i][j]);
                }
}

int dp(int n)
{
    if(!n) return 0;
    
    vector<int> nums;
    while(n) nums.push_back(n % 10), n /= 10;

    int res = 0, last = -2;
    
    // 答案是 a.size() 位
    for(int i = nums.size() - 1; i >= 0; i--)
    {
        int x = nums[i];
        // 因为不能有前导零，所以i在第一次枚举的时候j不能等于0
        
        //处理该位 < x 的情况
        for(int j = i == nums.size() - 1; j < x; j++)
            if(abs(j - last) >= 2) res += f[i+1][j];
        
        //处理该位 = x 的情况
        if(abs(last - x) < 2) break;
        
        last = x;

        // 如果原数不是windy数就已经break掉了，所以这里一定是windy数
        if(!i) res++;
    }
    
    for (int i = 1; i < nums.size(); i ++ ) // 答案小于 a.size() 位，随便取
        for (int j = 1; j <= 9; j ++ )
            res += f[i][j];
    return res;
}

int main(void)
{
    init();
    int l, r;
    scanf("%d%d", &l, &r);
    cout << dp(r) - dp(l - 1) << endl;
}
```