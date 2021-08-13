---
title: zroi-8-10
date: 2021-08-10 08:43:45
tags: 笔记
---

# dp 优化方法
$$
f_i = \min_{i - l < j < i} \{f_j + p_j\} + p_i \qquad\text{单调队列优化} \\
f_i = \sum_{i - l < j < i} \{f_j + p_j\} + p_i \qquad\text{前缀和优化} \\
f_i = \min_{i - l < j < i} \{f_j + p_jw_i\} + p_i \qquad\text{斜率优化} \\
f_i = \sum_{1 \le j <i} \{f_j + g_{i - j}\} + p_i \qquad\text{分治FFT} \\
f_i = \min_{\cdots \ldots \ddots} \{f_j\}+ p_i \qquad\text{CDQ 分治} \\
f_i = \min_{j < i}\{f_j + w(j + 1, i)\}\qquad\text{决策单调性}
$$
还有WQS二分凸优化，长链剖分，线段树合并，矩阵优化DDP, 齐次线性递推，生成函数等。

<!-- more -->