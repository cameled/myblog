---
title: Container With Most Water
date: 2021-02-27 22:27:43
tags: Algorithm
katex: false
mathjax: true
---

# 1. 概述

在[容器储水问题](https://leetcode.com/problems/container-with-most-water/)中,双指针法能够将复杂度降到{%mathjax%}O(n){%endmathjax%}。

本文是对双指针法有效性的证明，首先通过暴力枚举的方式列出S的解集A，然后证明双指针法遍历了整个解集A。

# 2. 暴力枚举法
假设{%mathjax%}x{%endmathjax%}取{%mathjax%}a,b (a < b){%endmathjax%}，则{%mathjax%}a{%endmathjax%}和{%mathjax%}b{%endmathjax%}之间容器的储水量为:

{%mathjax%}S(a, b) = (b - a) * min(y_a, y_b) (1){%endmathjax%}

通过对所有可能的{%mathjax%}S(a, b){%endmathjax%}取值进行枚举，得到{%mathjax%}S{%endmathjax%}的解集如下：
{%mathjax%}
A = \lbrace{S(0, 1), S(0, 2), S(0, 3) ... S(0, n), S(1, 2), S(1, 3) ... S(1, n) ... S(n-1, n)}\rbrace
{%endmathjax%}

# 3. 双指针法
双指针法的处理方式分为如下两种情况，

1. {% mathjax %}y_a < y_b {% endmathjax %}， 此时移动左侧指针；
2. {% mathjax %}y_a >= y_b {% endmathjax %}， 此时移动右侧指针。

下面我们将证明，通过上述规则能够完全遍历S的所有解。

## 3.1 {%mathjax%} y_a < y_b {%endmathjax%}

当{%mathjax%}y_a < y_b (a<b){%endmathjax%}，此时容器的有效高度为{%mathjax%}y_a{%endmathjax%}，容器的储水量为：

{%mathjax%}S(a, b) = (b - a) * y_a{%endmathjax%}

此时，对于解集{%mathjax%}A_{(a,b)} = \lbrace{S(a, a+1), S(a, a+2), ... S(a, b-1), S(a, b)}\rbrace{%endmathjax%}，最大值为{%mathjax%}S(a, b){%endmathjax%}。
上述结论的证明如下：
	令{%mathjax%}i \in \lbrace{a+1, a+2, ... b-1}\rbrace{%endmathjax%}；
	当{%mathjax%}y_i > y_a{%endmathjax%}时， {%mathjax%}S(a, i) = (i - a) * y_a{%endmathjax%}，因为{%mathjax%}i < b{%endmathjax%}，所以{%mathjax%}S(a, i) < S(a, b){%endmathjax%}。
	当{%mathjax%}y_i < y_a{%endmathjax%}时， {%mathjax%}S(a, i) = (i - a) * y_b{%endmathjax%}，因为{%mathjax%}i < b{%endmathjax%}且{%mathjax%}y_i < y_a < y_b{%endmathjax%}，所以{%mathjax%}S(a, i) < S(a, b){%endmathjax%}。
	所以{%mathjax%}S(a, b){%endmathjax%}为解集{%mathjax%}A_{(a,b)}{%endmathjax%}的最大值。

例如，对于{%mathjax%}S(0, b){%endmathjax%}, 当{%mathjax%}y_0 < y_b{%endmathjax%}时, 由上述结论，我们可以知道{%mathjax%}\lbrace{S(0, 1), S(0, 2), S(0, 3) ... S(0, b)}\rbrace{%endmathjax%}的最大值为{%mathjax%}S(0, b){%endmathjax%}。

通过上述分析，当{%mathjax%}y_a < y_b{%endmathjax%}时，我们可以移动左侧的指针，因为已经找到了与该指针相关的所有解的最大值。

## 3.2 {%mathjax%} y_a >= y_b {%endmathjax%}

同理，当{%mathjax%}y_a >= y_b{%endmathjax%}时，我们可以移动右侧的指针，因为已经找到了与该指针相关的所有解的最大值。


# 4. 总结
通过容器储水问题的双指针算法，我们发现通过消除无效和重复的求解过程，可以有效降低算法的复杂度。
