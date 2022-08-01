---
mathjax: true
title: latex如何输出花体字母
date: 2021-09-17 20:04:11
tags:
- Latex
- 学习笔记
categories:
- Latex编辑
---



如何使用latex输出花体数学符号？

<!--more-->

# 引用的包

**\usepackage{amsthm,amsmath,amssymb}**

**\usepakage{mathrsfs}**

# 实现效果

| 代码        | 效果          |
| ----------- | ------------- |
| \mathcal{M} | $\mathcal{M}$ |
| \mathbb{M}  | $\mathbb{M}$  |
| \mathscr{M} | $\mathscr{M}$ |

# Latex 中的空格
|类型|代码|效果|说明|
|----|----|----|----|
|两个quad空格| a \qquad b|$a \qquad b$|两个M的宽度|
|quad空格| a \quad b|$a \quad b$|一个M的宽度|
|大空格|a\b|$a\ b$|1/3M的宽度|
|中等空格|a\;b|$a\;b$|2/7M的宽度|
|小空格|a\,b|$a\,b$|1/6M的宽度|
|紧贴|a\!b|$a\!b$|缩进1/6M宽度|

