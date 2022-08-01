---
mathjax: true
title: DP基本概念
date: 2021-10-28 11:35:32
categories:
- Differential Privacy
tags:
- DP
---
差分隐私（Differential Privacy）是由Dwork等人[^1]在2006年针对数据库中的隐私泄露问题提出的一种新颖的隐私定义。在该定义的约束下，对数据库中数据的查询结果对某一记录的变化是不敏感的，即单一的记录在数据集中与不在数据集中，对于查询结果的影响可以忽略不计。因此，一个记录因其加入导数据集中所产生的隐私泄露风险被控制在极小的、可接受的范围内，攻击者无法通过分析查询结果而得到精确的个体信息。
<!--more-->

# 差分隐私定义
## 基本定义

**定义 1[^2]**. 差分隐私.假设有一个随机算法$\mathcal{M}$,$\Omega$表示算法所有可能的输出结果构成的集合.对于任意的邻接数据集$D$和$D^{\prime}$,以及任意子集$S\in\Omega$，如果算法$\mathcal{M}$满足：
$$
Pr[\mathcal{M}(D)\in S] \leq \text{exp}(\epsilon)Pr[\mathcal{M}(D^{\prime}\in S)]+\delta
$$
则称算法$\mathcal{M}$满足$(\epsilon,\delta)-DP$,参数$\epsilon$被称为隐私预算.当$\delta=0$时，就称算法$\mathcal{M}$满足$\epsilon-DP$,事实上$\epsilon-DP$是十分严格的定义，也被称为完美差分隐私。但是通常我们会允许以一个极小的概率$\delta$来打破约束，因此$(\epsilon,\delta)-DP$也被称为近似差分隐私[^3].

## 敏感度

差分隐私保护可以通过在查询函数的返回值中添加适当的干扰噪声来实现。但是加入噪声过多会影响结果的可用性，过少则无法提供猪狗的安全报账。敏感度是决定家人噪声量大小的关键参数。**它是指删除数据集中任一记录对查询结果造成的最大改变**

### 全局敏感度

假设现在有一个查询函数$f:D\to R^d$，输入为一数据集$D$，输出为一$d$维实数向量.那么对于任意的邻接数据集$D$和$D^\prime$(邻接数据指的是两个数据集只在一条记录上有差别)，那么
$$
GS_{f} = \mathop{\text{max}}\limits_{D,D^{\prime}} \parallel f(D) - f(D^{\prime}) \parallel_1
$$
称为函数$f$的<font color='red'>全局敏感度</font>。其中$\parallel f(D) - f(D^{\prime}) \parallel$是$f(D)$和$f(D^\prime)$之间的1-范数距离。

函数的全局敏感度由函数本身决定，不同的函数会有不同的全局敏感度，一些函数具有较小的的全局铭感度（例如计数函数，其敏感度为1），因此只需要加入少许噪声即可掩盖因一个记录被删除对查询结果产生的影响，实现差分隐私保护。但是对于某些函数而言，例如<font color='red'>求平均值，求中位数等函数</font>，则往往需要较大的全局敏感度。

 以求中位数为例，设函数$f(D)=median(x_1,x_2,\cdots,x_n)$,其中$x_i(i=1,2,\cdots,n)$是区间$[a,b]$中的一个实数.不妨设$n$为奇数，且数据已经被排序，那么函数的返回值即为第$m=(n+1)/2$个数。再某种极端情况下，设$x_{1}= x_{2} = \cdots = x_{m} = a$且$x_{m+1} = x_{m+2} = \cdots = x_{n} = b$,那么从中删除一个数就可能使函数的返回值有$a$变成$b$，因此函数的全局敏感度为$b-a$,这可能是一个很大的值。

**基于全局敏感度的关联敏感度**
$$
\Delta GS_q = \text{max} \left( \sum \limits_{j=0} \left( k \parallel Q(D^j) - Q(D^{-j}) \parallel_1 \right) \right)
$$
其中$k$表示关联记录的数量。
### 关联敏感度

#### 关联程度

假设两个记录$r_i$和$r_j$之间存在相关关系。这也就是说他们之间的相关关系可以通过**关联程度**$\delta_{ij}\in [-1,1]$来表示。

如果$\delta_{ij}<0$, $r_i$ 和 $r_j$之间是**负相关**；如果$\delta_{ij}>0$, $r_i$ 和 $r_j$之间是**正相关**；如果$\delta_{ij}=0$, $r_i$ 和 $r_j$之间**没有关系**；如果$|\delta_{ij}|=1$, $r_i$ 和 $r_j$之间是**完全相关**。

我们可以将数据集中记录之间的相关程度通过一个关联矩阵$\Delta$表示,显然$\delta_{ii}=1$。
$$
\Delta=\left[
\begin{aligned}
	\delta_{11}	&& \delta_{12} && \cdots &&\delta_{1n} \\
		\delta_{21}	&& \delta_{22} && \cdots &&\delta_{2n} \\
		\cdots	&& \cdots && \cdots &&\cdots \\
		\delta_{n1}	&& \delta_{n2} && \cdots &&\delta_{nn} \\
	\end{aligned}
\right]
$$

#### 记录敏感度

对于一个关联矩阵$\Delta$，和一个查询集$Q$，记录$r_i$的记录敏感度为
$$
CS_i = \sum \limits_{j=0}^{n} \mid \delta_{ij} \mid (\parallel Q(D^j) - Q(D^{-j}) \parallel_1)
$$
记录敏感度表示的是删除一个记录后对数据集$D$中的所有记录的影响程度。这个定义同时考虑了关联记录的数量和关联程度。显然，如果$D$中的记录全部都是独立的，那么$CS_i$也就等于全局铭感度(因为$\delta_{ij}=0(i\neq j)$)

#### 关联敏感度

对于一个查询$Q$，关联敏感度取决于最大的记录敏感度。
$$
CS_q = \mathop{\text{max}} \limits_{i \in q} (CS_i)
$$
其中 $q$ 是响应查询 $Q$ 的所有记录的记录集。
相关敏感度与查询 $Q$ 相关。 它列出了所有响应 $Q$ 的记录 $q$，并选择最大的记录敏感度作为相关敏感度。 当查询只覆盖独立或弱相关记录时，相关敏感性不会带来额外的噪音。

# 皮尔逊系数(Pearson's correlation coefficient)
## 相关系数(Correlation coefficient)
$$
\rho_{XY} = \frac{\text{Cov}(X,Y)}{\sqrt{D(X)}\sqrt{D(Y)}} = \frac{E((X-E(X))(Y-E(Y)))}{\sqrt{D(X)}\sqrt{D(Y)}}
$$

其中，$E$为数学期望或均值，$D$为方差，开根号为标准差，$E((X-E(X))(Y-E(Y)))$称为随机变量$X$与的协方差，记为$\text{Cov}(X,Y)$，即$\text{Cov}(X,Y) = E((X-E(X))(Y-E(Y)))$，而两个变量之间的协方差和标准差的商则称为随机变量$X$与$Y$的相关系$\rho_{XY}$

相关系数衡量随机变量X与Y相关程度的一种方法，相关系数的取值范围是[-1,1]。相关系数的绝对值越大，则表明$X$与$Y$相关度越高。当$X$与$Y$线性相关时，相关系数取值为1（正线性相关）或-1（负线性相关）。

具体的，如果有两个变量：$X$、$Y$，最终计算出的相关系数的含义可以有如下理解：
当相关系数为0时，X和Y两变量无关系。
当$X$的值增大（减小），$Y$值增大（减小），两个变量为正相关，相关系数在0.00与1.00之间。
当$X$的值增大（减小），$Y$值减小（增大），两个变量为负相关，相关系数在-1.00与0.00之间。
\par 通常情况下通过以下相关系数取值范围判断变量的相关强度：
0.8-1.0     极强相关
0.6-0.8     强相关
0.4-0.6     中等程度相关
0.2-0.4     弱相关
0.0-0.2     极弱相关或无相关
这个相关系数也称作**皮尔森相关系数$r$**。
皮尔逊系数的定义：
两个变量之间的皮尔逊相关系数定义为两个变量之间的协方差和标准差的商：
$$
\rho_{XY} = \frac{\text{Cov}(X,Y)}{\delta_X \delta_Y} = \frac{E((X-\mu_X)(Y-\mu_Y))}{\delta_X \delta_Y}
$$
随机变量$X$的标准差计算公式为：
$$
	\delta_X = \sqrt{E((X-E(X))^2)} = \sqrt{E(X^2) - (E(X))^2}
$$
$$
	\rho_{XY} = \frac{\sum \limits_{i=1}^{N} (X_i - \bar{X})(Y_i - \bar{Y})}{\sqrt{\sum \limits_{i=1}^{N} (X_i - \bar{X})^{2}}\sqrt{\sum \limits_{i=1}^{N} (Y_i - \bar{Y})^{2}}}
$$
**注**：皮尔逊相关系数的适用范围
当两个变量的标准差都不为零时，相关系数才有定义，皮尔逊相关系数适用于：
(1) 两个变量之间是线性关系，都是连续数据。
(2) 两个变量的总体是正态分布，或接近正态的单峰分布。
(3) 两个变量的观测值是成对的，每对观测值之间相互独立

# References

[^1]:C. Dwork, “Differential Privacy,” in *Automata, Languages and Programming*, vol. 4052, M. Bugliesi, B. Preneel, V. Sassone, and I. Wegener, Eds. Berlin, Heidelberg: Springer Berlin Heidelberg, 2006, pp. 1–12. doi: [10.1007/11787006_1](https://doi.org/10.1007/11787006_1).

[^2]:C. Dwork and A. Roth, “The Algorithmic Foundations of Differential Privacy,” *FNT in Theoretical Computer Science*, vol. 9, no. 3–4, pp. 211–407, 2013, doi: [10.1561/0400000042](https://doi.org/10.1561/0400000042).
[^3]:A. Beimel, K. Nissim, and U. Stemmer, “Private Learning and Sanitization: Pure vs. Approximate Differential Privacy,” in *Approximation, Randomization, and Combinatorial Optimization. Algorithms and Techniques*, vol. 8096, P. Raghavendra, S. Raskhodnikova, K. Jansen, and J. D. P. Rolim, Eds. Berlin, Heidelberg: Springer Berlin Heidelberg, 2013, pp. 363–378. doi: [10.1007/978-3-642-40328-6_26](https://doi.org/10.1007/978-3-642-40328-6_26).
