---
mathjax: true
title: 差分隐私之Composition Theorem
date: 2021-9-17
categories: 
- Differential Privacy
tags:
- DP
---



## 背景介绍

这里介绍了差分隐私中的Composition Theorem相关的定义

<!--more-->

### 差分隐私的定义

假设存在一个随机函数$\mathcal{M}$，使得在任意两个相邻的数据集 $D,D^{\prime}$（即$\Vert D-D^{\prime} \Vert_{1} \leq 1$）上得到的任意相同输出$\mathcal{S}$的概率满足
$$
\text{Pr}[\mathcal{M}(D)\in S] \leq  e^{\epsilon} \text{Pr}[\mathcal{M}(D^{\prime})\in S] + \delta
$$
则称该随机函数$\mathcal{M}$满足$(\epsilon,\delta)-DP$


### test
### Composition theorem探讨的问题

当$k$个随机函数，作用在同一个数据集上$\mathcal{M}_{1},\mathcal{M}_{2},\dots,\mathcal{M}_{k}$时，将这些随机函数称作一个Composition，记作$\mathcal{M}_{1:k}$。Composition Theorem探讨的是：假设每一个随机函数都满足$(\epsilon_{i},\delta_{i})-\text{DP}$，那么整个composition满足什么样的DP呢？

## 隐私损失

差分隐私（DP）的定义

