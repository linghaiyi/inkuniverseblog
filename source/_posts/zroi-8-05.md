---
title: ZROI-8-05
date: 2021-08-05 20:06:56
tags: 笔记
---

# 区间DP和树形DP
区间DP, 就是以区间作为状态的DP
树形DP, 就是以子树作为状态的DP

区间DP的一般套路
dp[l][r] 表示从l到r的状态值

树形DP的一般套路
dp[u][辅助数] 表示以 u 为根节点的子树，满足条件的状态值。

典型例题: