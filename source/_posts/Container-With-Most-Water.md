---
title: Container With Most Water
date: 2021-02-27 22:27:43
tags: Algorithm
katex: false
mathjax: true
---

# 1. 概述

在[容器储水问题](https://leetcode.com/problems/container-with-most-water/)中，双指针法能够将计算复杂度降到{%mathjax%}O(n){%endmathjax%}。本文是对双指针法的有效性思考，即证明通过双指针法可以求得最大储水量。

假设当{%mathjax%}x{%endmathjax%}取{%mathjax%}x_a，x_b (a < b){%endmathjax%}时，储水量最大，即{%mathjax%}S_{max} = (x_b - x_a) * min(y_a， y_b){%endmathjax%}。

由于{%mathjax%}min(y_a， y_b){%endmathjax%}的存在，{%mathjax%}S_{max}{%endmathjax%}分为两种情况：

1. {% mathjax %}y_a < y_b {% endmathjax %}， 即{% mathjax %}S_{max} = (x_b - x_a) * y_a{% endmathjax %}
2. {% mathjax %}y_a > y_b {% endmathjax %}， 即{% mathjax %}S_{max} = (x_b - x_a) * y_b{% endmathjax %}

# 2. {%mathjax%} y_a < y_b {%endmathjax%}

当{%mathjax%}y_a < y_b{%endmathjax%}，{%mathjax%}y_a{%endmathjax%}为{%mathjax%}S_{max}{%endmathjax%}的有效值。此时容器的有效高度在左侧，即{%mathjax%}S_{max}{%endmathjax%}的有效高度由左侧指针产生。

## 2.1 对左侧指针有效性的证明

对左侧指针有效性的证明分为两部分：

1. {%mathjax%}x_i <= x_a{%endmathjax%}
2. {%mathjax%}x_i > x_a{%endmathjax%}

### 2.1.1 {%mathjax%}i <= a{%endmathjax%}

当{%mathjax%}x_i <= x_a{%endmathjax%}时，令{%mathjax%}m = x_a - x_i (m >= 0){%endmathjax%}，

则{%mathjax%}S(i) = ((x_b - x_a) + m) * y_i{%endmathjax%}。

令{%mathjax%}w = x_b - x_a{%endmathjax%}，则{%mathjax%}S(i) = (w + m) * y_i{%endmathjax%};

又{%mathjax%}S(a){%endmathjax%}为容器储水最大值，则{%mathjax%}(w + m) * y_i < w * y_a{%endmathjax%}。

上式展开：

{%mathjax%}(w + 0) * y_a = w * y_a{%endmathjax%}
...
{%mathjax%}(w + a) * y_0 < w * y_a{%endmathjax%}

由上述展开可知，当{%mathjax%}x{%endmathjax%}取{%mathjax%}i{%endmathjax%}时，如果存在{%mathjax%}S(k) > S(i){%endmathjax%}， 则{%mathjax%}k > i{%endmathjax%}。

### 2.1.2 {%mathjax%}i > a{%endmathjax%}

对于这种情况，说明{%mathjax%}S_{max}{%endmathjax%}左侧指针已经通过了{%mathjax%}x_a{%endmathjax%}处。

### 2.1.3 总结

当{%mathjax%}y_i < y_b{%endmathjax%}时，对于{%mathjax%}i{%endmathjax%}，总是能期望存在{%mathjax%}k > i{%endmathjax%}， 其中{%mathjax%}S(k){%endmathjax%}为容器最大储水量。

## 2.2 {%mathjax%} y_a > y_b {%endmathjax%}

当{%mathjax%}y_a > y_i{%endmathjax%}时，对于{%mathjax%}i{%endmathjax%}，总是能期望存在{%mathjax%}k < i{%endmathjax%}， 其中{%mathjax%}S(k){%endmathjax%}为容器最大储水量。

# 3. 总结
通过归纳法，证明了双指针法在解决容器储水问题中的有效性。
