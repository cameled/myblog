---
title: Container With Most Water
date: 2021-02-27 22:27:43
tags: Algorithm
katex: false
mathjax: true
---

# 1. 概述

本文是对双指针法的有效性思考。

在[容器储水问题](https://leetcode.com/problems/container-with-most-water/)中，双指针法能够将复杂度降到{%mathjax%}O(n){%endmathjax%}。

假设{%mathjax%}x{%endmathjax%}取{%mathjax%}a，b (a < b){%endmathjax%}，则{%mathjax%}a{%endmathjax%}和{%mathjax%}b{%endmathjax%}之间容器的储水量为:

{%mathjax%}S(a， b) = (b - a) * min(y_a， y_b){%endmathjax%}

由于{%mathjax%}min(y_a， y_b){%endmathjax%}的存在，{%mathjax%}S(a， b){%endmathjax%}分为两种情况：

1. {% mathjax %}y_a < y_b {% endmathjax %}， 即{% mathjax %}S(a) = (b - a) * y_a{% endmathjax %}；
2. {% mathjax %}y_a >= y_b {% endmathjax %}， 即{% mathjax %}S(b) = (b - a) * y_b{% endmathjax %}。

# 2. {%mathjax%} y_a < y_b {%endmathjax%}

假设{%mathjax%}y_a < y_b (a<b){%endmathjax%}，此时容器的有效高度为{%mathjax%}y_a{%endmathjax%}，容器的储水量为：

{%mathjax%}S(a) = (b - a) * y_a{%endmathjax%}

令{%mathjax%}S_{max}(a){%endmathjax%}为{%mathjax%}{S(0), S(1), ..., S(a)}{%endmathjax%}中的最大值。

当{%mathjax%}a = 0{%endmathjax%}时，{%mathjax%} S(0)= (b - 0) * y_0{%endmathjax%}。
如果存在{%mathjax%}m{%endmathjax%}，使得{%mathjax%}S(m) > S(0){%endmathjax%}，则{%mathjax%}m > 0{%endmathjax%}。

当{%mathjax%}a = n （0 < n < b){%endmathjax%}时，{%mathjax%}S(n) = (b - n) * y_n{%endmathjax%}。
如果存在{%mathjax%}m{%endmathjax%}，使得{%mathjax%}S(m) > S_{max}(n){%endmathjax%}，则{%mathjax%}m > n{%endmathjax%}。

# 3. {%mathjax%} y_a >= y_b {%endmathjax%}

假设{%mathjax%}y_a >= y_b (a<b){%endmathjax%}，此时容器的有效高度为{%mathjax%}y_b{%endmathjax%}，容器的储水量为：

{%mathjax%}S(b) = (b - a) * y_b{%endmathjax%}

令{%mathjax%}S_{max}(b){%endmathjax%}为{S(b), S(b+1), ..., S(x_{max})}中的最大值。

当{%mathjax%}b = x_{max}{%endmathjax%}时，{%mathjax%} S(x_{max})= (x_{max} - a) * y_0{%endmathjax%}。
如果存在{%mathjax%}m{%endmathjax%}，使得{%mathjax%}S(m) > S(x_{max}){%endmathjax%}，则{%mathjax%}m < x_{max}{%endmathjax%}。

当{%mathjax%}b = n （0 < n < b){%endmathjax%}时，{%mathjax%}S(n) = (b - a) * y_n{%endmathjax%}。
如果存在{%mathjax%}m{%endmathjax%}，使得{%mathjax%}S(m) > S_{max}(n){%endmathjax%}，则{%mathjax%}m < n{%endmathjax%}。

# 4. 总结
单调情况已经在2,3节中讨论完毕。剩下的问题是，如何将上述两种情况合并，得出最终结论。
