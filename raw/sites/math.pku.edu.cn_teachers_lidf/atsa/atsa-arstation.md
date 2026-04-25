---
crawl_time: '2026-01-17 14:28:50'
framework: sphinx
title: 8 自回归模型及其平稳性 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 8 自回归模型及其平稳性

## 8.1 特例: AR(1)

\[\begin{align}
X\_t = a X\_{t-1} + \varepsilon\_t, \quad t \in \mathbb Z, \quad
\{\varepsilon\_t\} \sim \text{WN}(0,\sigma^2)
\tag{8.1}
\end{align}\]

从初值\(X\_0\)出发。\(a\)越小，初值影响减小越快。
\(|a|\)接近于1时，初值和前面的\(\varepsilon\_{t-j}\)影响减小越慢，
序列振荡。

只要\(|a|<1\)，序列最终可以稳定下来。称系统[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)是**稳定的**。

如果\(a=\pm 1\)则序列振荡越来越大，呈爆炸型。

\(|a|>1\)时序列也不能稳定。
\(|a| \geq 1\)时称[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)是**非稳定的**.

```
ar1.gen <- function(n, a, sigma=1.0, 
                   plot.it=FALSE, n0=1000,
                   x0=numeric(length(a))){
  n2 <- n0 + n
  eps <- rnorm(n2, 0, sigma)
  x2 <- filter(eps, a, method="recursive", side=1, init=x0)
  x <- x2[(n0+1):n2]
  x <- ts(x)
  attr(x, "model") <- "AR(1)"
  attr(x, "a") <- a
  attr(x, "sigma") <- sigma
  if(plot.it) plot(x)
  x
}

demo.ar1 <- function(){
  as <- c(0.1, 0.3, 0.5, 0.6, 0.7, 0.8, 0.9, 0.95, 0.99, 1.0, 1.001)
  x0 <- 10
  n <- 500
  for(a in as){
    for(a1 in c(a, -a)){
      x <- ar1.gen(n=n, a=a1, sigma=0.2,
                   n0=0, x0=x0)
      plot(x, main=paste("AR(1): x0=10, a=", a1, sep=""),
           ylim=c(-12, 15))
      abline(h=0, col="red")
    }
  }
}
set.seed(106)
demo.ar1()
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-1.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-2.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-3.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-4.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-5.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-6.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-7.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-8.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-9.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-10.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-11.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-12.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-13.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-14.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-15.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-16.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-17.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-18.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-19.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-20.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-21.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2015-arstation_files/figure-html/ar1sim01-22.png)

### 8.1.1 AR(1)的差分方程及平稳解

\(A(z) = 1 - a z\)是差分方程[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)的特征多项式。
\(z\_1 = \frac{1}{a}\)是特征根。
稳定的充分必要条件是\(|a|<1\)，或\(|z\_1|>1\)，
即特征根都在单位圆外。

当\(|a|<1\)时下面的线性序列有定义:
\[\begin{align}
X\_t = \sum\_{j=0}^\infty a^j \varepsilon\_{t-j}, \quad t \in \mathbb Z
\tag{8.2}
\end{align}\]

\[\begin{aligned}
a X\_{t-1} + \varepsilon\_t
=& a \sum\_{j=0}^\infty a^j \varepsilon\_{t-1-j}
+ \varepsilon\_t \\
=& \sum\_{i=1}^\infty a^i \varepsilon\_{t-i} + \varepsilon\_t
\quad (i = j+1) \\
=& X\_t
\end{aligned}\]
于是平稳序列[(8.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0202)是非齐次差分方程[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)的解，
称为**平稳解**。

[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)的通解为
\[\begin{align}
X\_t = \sum\_{j=0}^\infty a^j \varepsilon\_{t-j},
+ \xi a^t, \quad t \in \mathbb Z
\tag{8.3}
\end{align}\]
当\(t \to \infty\)时[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)的所有解a.s.收敛到平稳解[(8.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0202)。
收敛速度是负指数速度\(|a|^t\)。
平稳解可以看成系统[(8.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0201)处于稳定状态的情况。
特征根\(\frac{1}{a}\)离单位圆越远，稳定性越好。

## 8.2 一般AR(\(p\))

**定义8.1** 如果\(\{\varepsilon\_t\}\)是白噪声\(\text{WN}(0,\sigma^2)\),
实数\(a\_1,a\_2,\dots,a\_p\) \((a\_p\neq 0)\)
使得多项式\(A(z)\)的零点都在单位圆外:
\[\begin{align}
A(z)=1-\sum\_{j=1}^p a\_j z^j \neq 0, \ |z|\leq 1,
\tag{8.4}
\end{align}\]
则称\(p\)阶差分方程
\[\begin{align}
X\_t=\sum\_{j=1}^p a\_j X\_{t-j} +\varepsilon\_t, \ \ t\in \mathbb Z,
\tag{8.5}
\end{align}\]
是一个\(p\)阶自回归模型, 简称为**AR(\(p\))模型**.

满足 AR\((p)\)模型[(8.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0205)的平稳时间序列\(\{X\_t\}\)
称为[(8.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0205)的**平稳解**,
也称作AR(\(p\))序列.  
称\(\boldsymbol a = (a\_1,a\_2,\cdots,a\_p)^T\) 是AR\((p)\)模型的自回归系数.
称条件[(8.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0204)是**稳定性条件**或**最小相位条件**.
\(A(z)\)称为模型[(8.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0205)的特征多项式。
模型可用推移算子写成
\[\begin{align}
A(\mathscr B) X\_t = \varepsilon\_t, \quad t \in \mathbb Z
\tag{8.6}
\end{align}\]

## 8.3 平稳解和通解

### 8.3.1 AR(\(p\))的平稳解

由定理[7.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#thm:linfil-inverse)可知，
\[
A^{-1}(z)
= \sum\_{j=0}^\infty \psi\_j z^j,
\]
展开序列在\(|z| \leq \rho\)收敛，
其中\(\rho>1\)小于特征多项式的每一个根的模，
\(\psi\_j = o(\rho^{-j})\)。
于是
\[
Y\_t = A^{-1}(\mathscr B) \varepsilon\_t
= \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j}
\]
是线性平稳列，
有
\[
A(\mathscr B) Y\_t
= A(\mathscr B) A^{-1}(\mathscr B) \varepsilon\_t
= \varepsilon\_t,
\]
从而\(\{Y\_t \}\)是AR(\(p\))模型的平稳解。
反之，
如果\(\{X\_t \}\)是平稳解，
则根据定理[7.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#thm:linfil-inverse)，
\[
A^{-1}(\mathscr B) [A(\mathscr B) X\_t] = A^{-1}(\mathscr B) \varepsilon\_t = Y\_t,
\]
即有\(X\_t = Y\_t\)。
所以
\[\begin{align}
X\_t = \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j},
\tag{8.7}
\end{align}\]
这是AR(\(p\))模型[(8.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0205)的唯一的平稳解。
\(\{\psi\_j\}\)称为平稳序列\(\{X\_t\}\)的Wold系数，
以负指数速度衰减，
且特征根离单位圆越远，
衰减速度越快。

**定理8.1** (1) 由[(8.7)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0209)定义的时间序列\(\{X\_t\}\)
是AR(\(p\))模型[(8.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arstation.html#eq:arstation0205)的唯一(a.s.意义)平稳解；

(2) AR(\(p\))的模型的通解有如下形式
\[\begin{align}
Y\_t= \sum\_{j=0}^\infty \psi\_j\varepsilon\_{t-j} +
\sum\_{j=1}^k \sum\_{l=0}^{r(j)-1} U\_{l,j} t^l z\_j^{-t}, \quad
t\in \mathbb Z.
\tag{8.8}
\end{align}\]

### 8.3.2 Wold系数的递推公式

记\(a\_0=-1\)则\(A(z)=-\sum\_{j=0}^p a\_j z^j\)，
\[\begin{aligned}
1 = A(z) A^{-1}(z) = -\sum\_{m=0}^\infty
\left(\sum\_{j=0}^p a\_j \psi\_{m-j}\right) z^m
\end{aligned}\]
故\(\psi\_0=1\), \(\sum\_{j=0}^p a\_j \psi\_{m-j} = 0\), \(m>0\)。
于是
\[\begin{aligned}
\begin{cases}
\psi\_0 = 1, \\
\psi\_m = \sum\_{j=1}^p a\_j \psi\_{m-j}, & m=1, 2, \dots \\
\psi\_m =0, & m<0
\end{cases}
\end{aligned}\]
用推移算子表示为
\[
\psi\_m =0, m<0;
\quad \psi\_0 = 1;
\quad
A(\mathscr B) \psi\_m = 0, m \geq 1 .
\]

### 8.3.3 通解与平稳解的关系

AR(\(p\))的通解\(\{Y\_t\}\)与平稳解有如下关系
\[\begin{aligned}
|Y\_t - X\_t| = \left| \sum\_{j=1}^k \sum\_{l=0}^{r(j)-1} U\_{l,j} t^l z\_j^{-t} \right|
= o(\rho^{-t}), \text{a.s.}, \ t \to \infty
\end{aligned}\]
其中\(1 < \rho < \min\{|z\_j|\}\)。

根离单位园越远，稳定下来的速度越快。
可以用此事实作为模拟产生AR(\(p\))序列的理论基础。

### 8.3.4 AR序列的模拟

取\(x\_{1-p}=\dots=x\_{0}=0\)，生成\(\{\varepsilon\_t\} \sim \text{WN}(0,\sigma^2)\).
迭代得到\(x\_t = \varepsilon\_t + \sum\_{j=0}^p a\_j x\_{t-j}, j=1,2,\dots, n\_0+n\)。
取\(y\_t = x\_{t+n\_0}, t=1,2,\dots,n\).

白噪声列可以用正态分布随机数来生成。

\(n\_0\)取50即可，但特征根接近单位圆时要取大的\(n\_0\)。