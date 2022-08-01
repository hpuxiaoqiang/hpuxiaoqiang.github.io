---
mathjax: true
title: Pailler 算法
date: 2021-09-21 20:28:51
tags:
- 同态加密
categories:
- 密码学基础
---

**Paillier 算法：**基于合数剩余问题的公钥密码体制，具有加法同态和混合乘法同态的特性。

<!--more-->

## 算法加密与解密过程

### 密钥生成阶段

选着两个较为接近(一般是同bit量级)的大素数$p$和$q$，要求满足$gcd(pq,(p-1)(q-1))=1$。令$n=pq$，$p-1$和$q-1$的最小公倍数为$\lambda=lcm(p-1,q-1)$（或者为$\lambda=\varphi(n)$）,随机选择一个整数$g \in Z_{n^2}^*$，且要求$ \mu = (L(g^ \lambda \;\text{mod}\;n^2))^{-1}$存在，其中$L(x)=(x-1)/n$。因此公钥为$(n,g)$，私钥为$\lambda$。

### 加密阶段

设$m \in Z_n$是明文，随机选者一个整数$r \in Z_n^*$，且满足$gcd(r,n)=1$，则密文为$c=E(m,r)=g^m·r^n \;\text{mod}(n^2)$

### 解密阶段

$m=D(c)=L((c^\lambda \; \text{mod}\;n^2)· (\mu  \;\text{mod}\;n))$

## 算法验证

因为要保证$ \mu = (L(g^ \lambda \; \text{mod }\; n^2))^{-1}$的存在，也就是$L(g^\lambda \; \text{mod} \; n^2)$在模$n$下的逆存在，因此可以得出$gcd(g,p)=1$且$gcd(g,q)=1$。

若$g$为$p$或$q$的倍数，则有$gcd(g,n^2)=p \; or \; q$,则$g^ \lambda \equiv 1 \; mod \; n^2$。(根据欧拉定理)，那么$L(g^ \lambda \;\text{mod}\;n^2)=0$,则$L(g^ \lambda \; \text{mod}\;n^2)$在模$n$下的逆也就不存在了。
因为

$$(p-1)|\lambda,(q-1)|\lambda$$
所以

$$\lambda = k_1(p-1)=k_2(q-1)$$
根据费马定理可得

$$g^\lambda = g^{k_1(p-1)} \equiv 1 \; \text{mod} \; p$$

同理

$$g^\lambda = g^{k_2(q-1)} \equiv 1 \; \text{mod} \; q$$

所以

$$(g^\lambda -1 )|pq$$

因此有

$$g^\lambda \equiv 1 \; \text{mod}\;n$$

所以

$$g^\lambda \;\text{mod} \; n^2 \equiv 1 \; \text{mod}\;n$$

即

$$g^\lambda (modn^2)=k_gn+1,k<n$$

所以

$$L(g^\lambda \;\text{mod} \; n^2))=k_g$$

又因为

$$gcd(r,n)=1$$

故有

$$r^\lambda \equiv 1 \; \text{mod}\;n$$
所以

$$r^\lambda \; \text{mod} \; n^2 \equiv 1 \; \text{mod}\;n$$
即

$$r^\lambda \;\text{mod}\; n^2=k_rn+1,k<n$$

并且有

$$1+kn \equiv 1+kn \; \text{mod} \; n^2$$

$$(1+kn)^2 \equiv 1+2kn+(kn)^2 \equiv 1+2kn \; \text{mod} \; n^2$$

$$(1+kn)^2 \equiv 1+3kn+3(kn)^2+(kn)^3 \equiv 1+3kn \; \text{mod} \; n^2$$

根据数学归纳法可以得出

$$(1+kn)^m \equiv 1+knm \; \text{mod} \; n^2$$

所以

$$g^{\lambda m}=(1+k_gn)^m \equiv k_gnm+1\; \text{mod} \; n^2$$

$$r^{\lambda n}=(1+k_rn)^m \equiv k_rn^2+1 \equiv 1 \; \text{mod} \; n^2$$

所以

$$L(g^{m\lambda}*r^{n\lambda }(mod\quad n^2))=L(K_gnm+1)=mk_g$$

又因为

$$\mu = {L(g^\lambda \;\text{mod} \; n^2)}^{-1}={k_g}^{-1}$$

故

$$L(g^{m\lambda}*(r^{n\lambda }\;\text{mod}\; n^2)*(\mu \;\text{mod}\;n)) = mk_g*{k_g}^{-1}=m$$

