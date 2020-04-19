---
title: public key encryption rsa
date: 2020-04-19 
tags: Encryption
---
## 简述

公钥加密技术最初由Diffie和Hellman于1976年提出。它是一种加密和解密使用不同密码的加密方式，通常也称为非对称加密。在对称加密中密码称为密钥；在非对称加密中则称作公钥和私钥。发展至今，已经出现了很多的公钥加密算法，稍后介绍的RSA就是其中的一种。

## RSA 加密算法
如上，RSA是一种非对称加密算法。它提供了一种可行的，加解密使用不同密码的方法。它的实现基于模运算，加解密的形式如下：

```
C = M ^ a mon n (M < n)
M = C ^ b mod n
```

假设上面是一个数学问题，如何使它成立呢？1978年，Ron Rivest,Adi Shamir,和Len Adleman运用这个问题提出了一种非对称加密算法（RSA即他们的名字首字母）。

以下不是推导，只是在谈一种使上面的加解密过程成立的情况。

```
已知问题：
	C = M ^ e mod n (M < n)
	M = C ^ d mod n
等价于：
	M = M ^ (e * d) mod n
```

现在问题变成证明下式成立:
```
	M = M ^ (e * d) mod n
```

这个式子成立吗？是的，某些条件下是成立的。

当e*d与n的欧拉函数φ(n)互为质数时,根据欧拉定理有：
```
	e * d = 1 mod φ（n)
	即 e * d = k * φ（n) + 1
```
所以现在问题变成证明下式成立:
```
	M = M ^ (k * φ（n) + 1) mod n
进一步推导：
	= (M * M ^ kφ(n)) mod n
	= (M * (M ^ φ(n)) ^ k ) mod n
```
现在问题成了证明下式成立：
```
	1 = (M ^ φ(n)) ^ k mod n
```
这个也好办，再次使用欧拉定理，因为n是我们自己选的，只要让n和它自己的欧拉函数互为质数就好了。

以上用反推法来说明RSA算法的合理性。因为先有公钥加密的概念，然后人们才开始找寻方法，找寻那种让使用不同密码的加解密成立的方法。

RSA算法就是其中的一种，它利用下式的存在来对信息进行分块加解密。
```
	C = M ^ e mod n
	M = C ^ d mod n
```