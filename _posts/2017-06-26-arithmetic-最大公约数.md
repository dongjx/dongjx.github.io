---
layout:     post
title:      Arithmetic:最大公约数
subtitle:   
date:       2017-06-26
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - arithmetic
---
一个很基础的算法，看过无数次的我...老年痴呆晚期

---

## 最大公约数
辗转相除法， 又名欧几里德算法（Euclidean algorithm）乃求两个正整数之最大公因子的算法。
__设两数为a、b(a>b)，求a和b最大公约数(a，b)的步骤如下：用a除以b，得a÷b=q......r1(0≤r1)。若r1=0，则(a，b)=b；若r1≠0，则再用b除以r1，得b÷r1=q......r2 (0≤r2）.若r2=0，则(a，b)=r1，若r2≠0，则继续用r1除以r2，……如此下去，直到能整除为止。其最后一个余数为0的除数即为(a, b)的最大公约数。
例如：a=25,b=15，a/b=1......10,b/10=1......5,10/5=2.......0,最后一个余数为0d的除数就是5, 5就是所求最大公约数__

### 证明一：
设a,b的最大公约数是c, r是a%b, k是a/b的商, 即a/b=k …r, 则设a=mc, b=nc
则 r=a-bk=mc-nck=(m-nk)c， b=nc => c是r, b的因数
假设 n与m-nk不互质：
    设n, m-nk的公约数d， 则m-nk = xd, n=yd(d>1) => m= xd+nk = xd+ynd => a=(x+yn)cd, b=ycd
    则cd是a, b的公约数, 则cd>c
    由于c是a,b 最大公约数，所以 不存在cd > c，所以假设不成立，n与m-nk互质
所以gcd(b, r)的最大公约数是c => gcd(a, b)=gcd(b,r)

### 证明二：
设a,b的最大公约数是c
1. 证明Rn-1同时整除a,b.
     因为Rn=0, 所以 Rn-1 可以整除Rn-2
     Rn-2 = Kn * Rn-1
     => Rn-3 = Kn-1* Rn-2 + Rn-1 = Kn-1* Kn* Rn-1 + Rn-1 = (Kn-1*Kn + 1) * Rn-1
     => Rn-4 = Kn-2 * Rn-3 + Rn-2 = Kn-2*(Kn-1* Kn* Rn-1 + Rn-1) + Kn * Rn-1 = (Kn-2*Kn-1*Kn + 1 + Kn) Rn-1  
    ….
   
     则 Rn-1可以整除Rn-3, 同理可得 Rn-1 可整除 Rn-2, Rn-3… R0
     则R0 = xRn-1, R1=yRn-1
     => b= K1 * R0 + R1 = (K1*x + y) Rn-1 = zRn-1 则 b= zRn-1
     => a=K0b + R0 = (K0 * Z + x) Rn-1
    则 Rn-1是a, b的公约数，Rn-1 <= c
2. 证明c能整除Rn-1
    设a=mc, b=nc
    => R0= a-kb = mc-knc=(n-kn)c, 所以c可以整除R0
    => R1=b - K1R0 = nc - K1*(n-kn)c, 所以c可以整除R1
    => R2=R0 - R1* K2 = (n-kn)c - nc - K1*(n-kn)c * K2, 所以c可以整除R2
    ...
    所以 c可以整除Rn-1,  所以c <= Rn-1

由一、二可得 c等于Rn-1
所以 a,b的最大公约数是 Rn-1

## 最小公倍数
`a * b / 最大公约数`

    




