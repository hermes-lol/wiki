---
crawl_time: '2026-01-17 14:29:18'
framework: sphinx
title: 21 ARMA模型的参数估计 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 21 ARMA模型的参数估计

对于零均值化后的平稳观测数据\(x\_1,x\_2,\cdots,x\_N\),
如果拟合AR(\(p\))和MA(\(q\))模型的效果都不理想,
就要考虑ARMA(\(p,q\))模型的拟合.
这时可以假设\(x\_1,x\_2,\cdots,x\_N\)满足如下的可逆ARMA(\(p,q\))模型
\[\begin{align}
X\_t = \sum\_{j=1}^p a\_j X\_{t-j} + \varepsilon\_t +\sum\_{j=1}^q b\_j\varepsilon\_{t-j},
t=1,2,\dots
\tag{21.1}
\end{align}\]
其中\(\{\varepsilon\_t\}\) 是WN(0,\(\sigma^2\)),
未知参数\(\boldsymbol{a}=(a\_1,a\_2,\cdots,a\_p)^T\)和\(\boldsymbol{b} =(b\_1,b\_2,\cdots,b\_q)^T\)使得多项式
\[\begin{align}
A(z)= 1- \sum\_{j=1}^p a\_j z^j ,
\ \ B(z)=1+\sum\_{j=1}^q b\_j z^j
\tag{21.2}
\end{align}\]
互素, 并且满足
\[\begin{align}
A(z)B(z) \neq 0,\ |z|\leq 1.
\tag{21.3}
\end{align}\]

以下仍然用 \(\hat \gamma\_k\)表示由[(19.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estar.html#eq:estar-hatgamma0102)定义的样本自协方差函数.
我们先设\(p,q\)是已知的.

## 21.1 ARMA(\(p,q\))模型的矩估计方法

利用§[13.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-iden)的[(13.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-iden-ywmat0215)知道
ARMA(\(p,q\))序列的自协方差函数满足延伸的Yule-Walker方程
\[\begin{align}
\left ( \begin{array}{llll}
\gamma\_{q+1}\\
\gamma\_{q+2}\\
\vdots \\
\gamma\_{q+p}
\end{array} \right )
= \left ( \begin{array}{llll} \gamma\_{q} \ & \gamma\_{q-1} \ &\cdots& \gamma\_{q-p+1}\\
\ \gamma\_{q+1}\ & \gamma\_{q} \ &\cdots& \gamma\_{q-p+2}\\
\vdots & \vdots & \cdots & \vdots \\
\ \gamma\_{q+p-1}\ & \gamma\_{q+p-2} \ &\cdots& \gamma\_q\\
\end{array} \right )
\left ( \begin{array}{llll}
a\_1\\
a\_2\\
\vdots \\
a\_p
\end{array} \right )
\tag{21.4}
\end{align}\]
这是参数\(\boldsymbol a\) 的估计方程,
从它得到\(\boldsymbol a\)的矩估计
\[\begin{align}
\left ( \begin{array}{llll}
\hat a\_1\\
\hat a\_2\\
\vdots \\
\hat a\_p
\end{array} \right )
= \left ( \begin{array}{llll} \hat \gamma\_{q} \ &\hat \gamma\_{q-1} \ &\cdots&\hat \gamma\_{q-p+1}\\
\hat \gamma\_{q+1}\ & \hat \gamma\_{q} \ &\cdots&\hat \gamma\_{q-p+2}\\
\vdots & \vdots & \cdots & \vdots \\
\hat \gamma\_{q+p-1}\ &\hat \gamma\_{q+p-2} \ &\cdots&\hat \gamma\_q\\
\end{array} \right ) ^{-1}
\left ( \begin{array}{llll}
\hat \gamma\_{q+1}\\
\hat \gamma\_{q+2}\\
\vdots \\
\hat \gamma\_{q+p}
\end{array} \right ).
\tag{21.5}
\end{align}\]

利用§[13.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-iden)的定理[13.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#thm:armamod-arsolu-inv)知道[(21.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-extyw0304)中的\(p\times p\)矩阵\(\Gamma\_{p,q}\)是可逆的.
用\(\hat \Gamma\_{p,q}\)表示[(21.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-momesteq0305)中的\(p\times p\)矩阵.
当ARMA(\(p,q\))模型中的白噪声\(\{\varepsilon\_t\}\)独立同分布时,
\(\hat\gamma\_k\) a.s.收敛到\(\gamma\_k\).
于是当\(N\to \infty\),
\[
\det(\hat \Gamma\_{p,q})\to \det(\Gamma\_{p,q}) \neq 0.
\]
所以当\(N\)充分大后, [(21.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-momesteq0305)中的
矩阵 \(\hat \Gamma\_{p,q}\)也是可逆的.
这时矩估计是惟一的.
还可以看出,
在上述的条件下,
矩估计[(21.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-momesteq0305)是强相合的:
\[\begin{align}
\lim\_{N\to \infty} \hat a\_j =a\_j, \ a.s., \ 1\leq j\leq p.
\tag{21.6}
\end{align}\]

这种相合性要求大样本，
在中小样本情形有很大可能误差较大，
且不一定满足最小相位条件。
在中小样本，
矩估计不是一种实用的方法，
即使作为最大似然估计的初值也不太合适。

下面估计MA\((q)\)部分的参数.
由于
\[
z\_t \stackrel{\triangle}{=} x\_t- \sum\_{j=1}^p a\_j x\_{t-j},
\ t=p+1,p+2,\dots,N
\]
满足MA(\(q\))模型
\[
z\_t=B(\mathscr B)\varepsilon\_t,
\]
所以得到\(\hat a\_1, \hat a\_2,\dots,\hat a\_p\)后,
\[\begin{align}
y\_t = x\_t - \sum\_{j=1}^p \hat a\_j x\_{t-j},
\ t=p+1,p+2,\dots,N.
\tag{21.7}
\end{align}\]
是一个MA(\(q\))序列的近似观测数据. 它的样本自协方差函数由
\[\begin{align}
\hat \gamma\_y(k)
= \sum\_{j=0}^p \sum\_{l=0}^p \hat a\_j \hat a\_l \hat \gamma\_{k+j-l},
k=0,1,\cdots, q
\tag{21.8}
\end{align}\]
定义, 其中\(\hat a\_0=-1\).
现在将[(21.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-momest-sidegamma0308)看成一个MA\((q)\)序列的样本自协方差函数,
利用第[20](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#atsa-estma)章的方法就可以估计出MA\((q)\)部分的参数\(\boldsymbol b\)和\(\sigma^2\).

矩估计法，其中MA部分用逆相关函数法的R程序：

```
## 用矩估计法估计ARMA，其中MA部分用逆相关函数法
## gms输入$\gamma_k$, $k=0,1,\dots,p+q$。
arma.moment.racv <- function(x, p=1, q=1){
  n <- length(x)
  pmax <- round(sqrt(n))
  if(pmax < p+q) pmax <- p+q
  ## 计算样本自协方差函数
  gms <- c(acf(x, lag.max=pmax+p, type="covariance", plot=FALSE)$acf)

  ## 求解AR部分
  Gpq <- matrix(0, nrow=p, ncol=p)
  for(ii in seq(p)) for(jj in seq(p)){
    Gpq[ii,jj] <- gms[abs(q + ii - jj)+1]
  }
  gs <- gms[(q+1+1):(q+p+1)]
  a <- solve(Gpq, gs)
  aa <- c(-1, a)

  ## 从已有的自协方差估计与AR参数估计，计算MA部分的$\gamma_j, j=0,1,\dots,pmax$。
  gys <- numeric(pmax+1)
  for(k in seq(0, pmax)){
    gys[k+1] <- sum(c(outer(0:p,0:p,
                            function(ii,jj) aa[ii+1] * aa[jj+1]
                            * gms[abs(k+jj-ii)+1])))
  }

  ## 从MA部分的自协方差估计，拟合长阶自回归模型
  res1 <- ar.yw(gys)

  ## 求MA部分的逆相关函数
  gamr <- ar_racv(res1$ar, res1$var.pred)[1:(q+1)]
  
  ## 利用逆相关函数为输入求解YW方程，得到MA参数估计
  res2 <- ar.yw(gamr)
  b <- -res2$ar
  sig2 <- 1/res2$var.pred
  ##sig2 <- gys[1] / sum(c(1, b)^2)
  
  list(ar=a, ma=b, var.pred=sig2)
}
## 从AR模型参数计算逆相关函数
ar_racv <- function(a, sig2){
  p <- length(a)
  a <- c(-1, a)
  a2 <- c(a, numeric(p+1))
  res <- numeric(p+1)
  for(k in seq(0, p, by=1)){
    res[k+1] <- sum(a * a2[(k+1):(k+p+1)])
  }
  res/sig2
}
```

## 21.2 ARMA(\(p,q\))模型的自回归逼近法

如果ARMA模型中已知\(\{ \varepsilon\_t \}\)则\(a\_1,\dots, a\_p\),
\(b\_1,\dots,b\_q\)可以看成是回归系数。
\(\varepsilon\_t\)作为一步预报误差，
可以用样本新息估计。
但是样本新息直接计算困难，
所以可以拟合长阶自回归模型，
用自回归模型的残差作为一步预报误差的估计。

首先为数据建立AR模型.
取自回归阶数的上界\(P\_0=[ \sqrt{N}]\).
这里\([a]\)表示 \(a\)的整数部分.
采用AIC定阶方法得到 AR模型的阶数估计\(\hat p\)
和自回归系数的估计
\[
(\hat \phi\_1, \hat \phi\_2,\dots,\hat \phi\_{\hat p}).
\]
计算残差
\[
\hat \varepsilon\_t = x\_t-\sum\_{j=1}^{\hat p} \hat \phi\_j x\_{t-j},
\ \ t= \hat p +1, \hat p +2,\dots, N.
\]
然后写出近似的ARMA(\(p,q\))模型
\[
x\_t = \sum\_{j=1}^p a\_j x\_{t-j} + \hat \varepsilon\_t + \sum\_{j=1}^q b\_j
\hat \varepsilon\_{t-j}, \ t= L+1, L+2,\dots, N.
\]
\(L=\max(\hat p, p, q)\), \(a\_j,b\_k\)是待定参数.
最后对目标函数
\[\begin{align}
Q(\boldsymbol{a},\boldsymbol{b})
= \sum\_{t=L+1}^N \left( x\_t -
\sum\_{j=1}^p a\_j x\_{t-j} - \sum\_{j=1}^q b\_j \hat \varepsilon\_{t-j}
\right)^2
\tag{21.9}
\end{align}\]
极小化,
得到最小二乘估计 \((\hat a\_1, \dots, \hat a\_p\),
\(\hat b\_1,\dots\hat b\_q)\).
\(\sigma^2\)的最小二乘估计由下式定义.
\[
\hat \sigma^2
= \frac{1}{N-L} Q(\hat a\_1,\dots,\hat a\_p,
\hat b\_1,\dots,\hat b\_q).
\]

## 21.3 正态时间序列的似然函数

设\(\{X\_t\}\)是零均值正态时间序列,
对\(n\geq 1\), \(\boldsymbol{X}\_n = (X\_1,X\_2,..,X\_n)^T\)
的协方差矩阵\(\Gamma\_n\)正定.
采用§[24.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#ipred-staipred)的记号可以得到最佳线性预测
\[
\hat X\_{n} \stackrel{\triangle}{=} L(X\_{n}|\boldsymbol{X}\_{n-1})
= \sum\_{j=1}^{n-1} \theta\_{n-1,n-j} Z\_{j},
\]
其中 \(Z\_j=X\_j-\hat X\_j\), \(Z\_1=X\_1\).
于是
\[\begin{aligned}
X\_n =& \hat X\_n + Z\_n
= \sum\_{j=1}^{n-1} \theta\_{n-1,n-j}Z\_{j} + Z\_n \\
=& \sum\_{j=1}^{n} \theta\_{n-1,n-j} Z\_{j}
\qquad (\theta\_{n-1,0} \stackrel{\triangle}{=} 1) \\
=& (\theta\_{n-1,n-1},\theta\_{n-1,n-2},\cdots,\theta\_{n-1,1}, 1)
\cdot \boldsymbol{Z}\_n,
\end{aligned}\]
其中\(\boldsymbol Z\_n = (Z\_1,Z\_2,\cdots,Z\_n)^T\).

为了方便引入下三角矩阵
\[
C=\left( \begin{array}{lllllll} &1 &0 &0 &\cdots &0 \\
&\theta\_{1,1} &1 &0 &\cdots & 0 \\
&\theta\_{2,2} & \theta\_{2,1} &1 &\cdots & 0 \\
&\vdots v\cdots & \vdots & \cdots &\vdots \\
&\theta\_{n-1,n-1} \ &\theta\_{n-1,n-2} \ &\theta\_{n-1,n-3} \ &\cdots\ &1
\end{array} \right)
\]
则有
\[
\boldsymbol{X}\_n= C \boldsymbol{Z}\_n.
\]
由于
\[
r\_{k-1} \stackrel{\triangle}{=} EZ\_k^2,
\ \ k=1,2,...,n-1
\]
是预测的均方误差
(原来新息预报中\(r\_{k-1}\)用\(\nu\_{k-1}\)表示，
但是下面\(\nu\_{k-1}\)要表示对\(X\_t\)作变换后的序列\(Y\_t\)的样本新息方差),
所以用\(Z\_1,Z\_2,\dots,Z\_{n}\)的正交性得到
\[
D \stackrel{\triangle}{=} E(\boldsymbol{Z}\_n \boldsymbol{Z}\_n^T)
= \hbox{diag}(r\_0,r\_1,\dots, r\_{n-1}).
\]
由此得到\(\boldsymbol{X}\_n\)的协方差矩阵:
\[\begin{aligned}
& \Gamma\_n = E(C\boldsymbol{Z}\_n \boldsymbol{Z}\_n^T C^T) = C D C^T,\\
& \det(\Gamma\_n) = \det(D) = r\_0 r\_1 \cdots r\_{n-1}, \\
& \boldsymbol{X}\_n^T \Gamma\_n^{-1} \boldsymbol{X}\_n
= \boldsymbol{Z}\_n^T C^T (CDC^T)^{-1} C \boldsymbol{Z}\_n
= \sum\_{j=1}^n Z\_j^2 / r\_{j-1}.
\end{aligned}\]
由于\(\boldsymbol{X}\_n\)的分布由\(\Gamma\_n\)决定,
而\(r\_j\), \(\theta\_{k,j}\) 都是\(\Gamma\_n\)的函数,
所以可得基于\(\boldsymbol{X}\_n\)的似然函数
\[\begin{align}
L(\Gamma\_n) =& \frac{1}{(2\pi)^{n/2}[\det(\Gamma\_n)]^{1/2}}
\exp \left(-\frac{1}{2} \boldsymbol{X}\_n^T \Gamma^{-1}\_n \boldsymbol{X}\_n\right )\cr
=& \frac{1}{(2\pi)^{n/2}(r\_0 r\_1 \cdots r\_{n-1})^{1/2}}
\exp\left(-\frac{1}{2} \sum\_{j=1}^n Z\_j^2/r\_{j-1}\right).\cr
\tag{21.10}
\end{align}\]

## 21.4 ARMA模型的最大似然估计

### 21.4.1 似然函数

设\(\{X\_t\}\)是满足ARMA模型[(21.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-mod0301)的平稳序列.
采用§[25.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#armaip-arma)中符号,
利用§[25.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#armaip-arma)的[(25.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armaformu0411)得到逐步预报的递推公式
\[\begin{align}
\hat X\_{k+1} = \begin{cases}
\sum\_{j=1}^k \theta\_{k,j} Z\_{k+1-j}, & 1\leq k < m, \\
\sum\_{j=1}^p a\_j X\_{k+1-j}
+ \sum\_{j=1}^q \theta\_{k,j} Z\_{k+1-j}, & \ k \geq m.
\end{cases}
\tag{21.11}
\end{align}\]
这里\(m=\max(p,q)\),
\[\begin{align}
Z\_{k} =& X\_k - \hat X\_k
= X\_k - L(X\_k|X\_{k-1},X\_{k-2},\dots,X\_1),
\tag{21.12}
\\
r\_{k-1} =& EZ\_k^2
= E(X\_k - \hat X\_{k})^2
= \sigma^2 \nu\_{k-1}.
\tag{21.13}
\end{align}\]

\(\theta\_{k,j}, \nu\_k\)
可以通过[(25.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armacov0409)和[(24.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-coef0306)递推计算。
而[(25.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armacov0409)中的\(\sigma^{-2} \gamma\_k\)
由§[13.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-gamma)的[(13.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-gam-gampsi0210), [(13.9)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-gam-psiiter0211)计算，
即用ARMA自协方差的Wold系数表达式，
Wold系数可由模型参数递推计算。
它们都是和\(\sigma^2\)无关的量,
由ARMA模型[(21.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-mod0301)的参数
\[\begin{align}
\boldsymbol \beta =(a\_1,a\_2,\dots,a\_p,b\_1,b\_2,\dots,b\_q)^T
\tag{21.14}
\end{align}\]
惟一决定.
从而\(\hat X\_{k}\) 也是和\(\sigma^2\)无关的量,
仅由观测数据\(X\_1,X\_2,\dots, X\_{k-1}\)和\(\boldsymbol{\beta}\)决定.
将[(21.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-mle-ipred0311)和[(21.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-mle-orthdec0313)代入[(21.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-normlikf0310)就得到基于观测数据\(X\_1,X\_2,\cdots,X\_N\)的似然函数
\[\begin{align}
L(\boldsymbol{\beta}, \sigma^2)
= \frac{\exp\Big(-\frac{1}{2\sigma^2}
\sum\_{k=1}^N Z\_k^2/\nu\_{k-1}\Big)}
{(2\pi)^{N/2}(\sigma^{2N}\nu\_0\nu\_1\cdots \nu\_{N-1})^{1/2}}
\tag{21.15}
\end{align}\]

引入
\[\begin{align}
S(\boldsymbol \beta)= \sum\_{k=1}^N Z\_k^2/\nu\_{k-1}.
\tag{21.16}
\end{align}\]
忽略常数项后, 可以得到对数似然函数
\[\begin{align}
\ln L(\boldsymbol \beta,\sigma^2)
= -\frac{N}{2}\ln \sigma^2
-\frac12 \ln (\nu\_0\nu\_1\cdots \nu\_{N-1})
-\frac{1}{2\sigma^2} S(\boldsymbol{\beta}).
\tag{21.17}
\end{align}\]

利用
\[
\frac{\partial}{\partial \sigma^2} \ln L(\boldsymbol \beta, \sigma^2)
= -\frac{N}{2\sigma^2} + \frac{S(\boldsymbol{\beta})}{2\sigma^4}
= 0
\]
得到
\[
\sigma^2 = \frac{1}{N} S(\boldsymbol{\beta}).
\]
将上式代入[(21.17)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-mle-armalnlik0317)得到
\[\begin{align}
l(\boldsymbol{\beta}) \stackrel{\triangle}{=}& -\frac{2}{N}\ln L(\boldsymbol{\beta}, S(\boldsymbol{\beta})/N) - 1\\
=& \frac{1}{N}\ln(\nu\_0\nu\_1\cdots \nu\_{N-1})
+ \ln [S(\boldsymbol{\beta})/N].
\tag{21.18}
\end{align}\]
通常称\(l(\boldsymbol{\beta})\)是约化似然函数.
可以看出, \(l(\boldsymbol{\beta})\)的最小值点
\[\begin{align}
\hat \beta = (\hat a\_1, \hat a\_2, \dots, \hat a\_p,
\hat b\_1,\hat b\_2,\dots,\hat b\_q)^T
\tag{21.19}
\end{align}\]
是\(\boldsymbol{\beta}\)的最大似然估计, 而
\[\begin{align}
\hat \sigma^2 = \frac{1}{N}S(\hat \beta)
\tag{21.20}
\end{align}\]
是\(\sigma^2\)的最大似然估计.

### 21.4.2 最大似然估计的计算

在计算 \(l(\boldsymbol{\beta})\)的最小值点时,
可采用一般的最优化方法.
要计算\(l(\boldsymbol{\beta})\)函数值，
通过[(25.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armacov0409)和[(24.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-coef0306)递推计算出
\(\theta\_{k,j}\),
\(\nu\_j\) 和 \(Z\_k=X\_k - \hat X\_k\),
然后计算出\(l(\boldsymbol \beta )\).

为了加快搜索的速度和提高估计的精度,
初始值\(\boldsymbol{\beta}^{(0)}\)
应当选择在\(l(\boldsymbol{\beta})\)的最小值附近.

实际计算中,
初始值应当选成在§[21.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#estarma-moment)中定义的矩估计或[21.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#estarma-arapprox)中定义的自回归逼近估计.
为了得的估计模型的合理性和可逆性,
还应当选择初始值\(\boldsymbol{\beta}^{(0)}\)
使得\(A(z), B(z)\)在闭单位圆内没有零点.

### 21.4.3 最大似然估计与最小二乘估计

由于当\(k \to \infty\),
用§[23.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#wold-wold)定理[23.2.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#wold-wold-infdimproj)得到
\[\begin{aligned}
\nu\_k =& E(X\_{k+1}-\hat X\_{k+1})^2/\sigma^2 \\
=& E[X\_n - L(X\_n|X\_{n-1},X\_{n-2},\dots,X\_{n-k})]^2/\sigma^2\\
\to& E[X\_n-L(X\_n|X\_{n-1},X\_{n-2},\dots)]^2/\sigma^2 \\
=& E\varepsilon\_n^2/\sigma^2=1.
\end{aligned}\]
所以 \(N\to \infty\) 时,
\[
\frac{1}{N}\ln (\nu\_0\nu\_1\cdots \nu\_{N-1})\to 0.
\]
于是, 对较大的\(N\),
\(l(\boldsymbol{\beta})\)和\(S(\boldsymbol{\beta})\)的最小值点近似相等.
于是也可以用\(S(\boldsymbol{\beta})\)
的最小值点\(\tilde \beta\)作为\(\boldsymbol{\beta}\)的估计.
通常也称\(\tilde{\beta}\)是\(\boldsymbol{\beta}\)的最小二乘估计,
相应的白噪声方差\(\sigma^2\)的估计定义成
\[\begin{align}
\tilde\sigma^2 = \frac{1}{N-p-q} S(\tilde \beta).
\tag{21.21}
\end{align}\]

### 21.4.4 最大似然估计的极限分布

可以证明最小二乘估计\(\tilde\beta\)和最大似然估计\(\hat\beta\)有相同的极限分布,
\(\tilde\sigma^2\)和\(\hat\sigma^2\)有相同的极限分布.

**定理21.1** 如果\(\{X\_t\}\)是平稳可逆的ARMA(\(p,q\))序列,
白噪声是独立同分布序列,
\(E\varepsilon\_t^4<\infty\). 则当
\(N\to \infty\) 时,
\[
\sqrt{N}(\boldsymbol{\hat\beta} -\boldsymbol{\beta})
\]
依分布收敛到正态分布\(N(0, V(\boldsymbol{\beta}))\), 其中
\[\begin{align}
V(\boldsymbol{\beta})=\begin{cases}
\sigma^2 \left( \begin{array}{ll}
E(\boldsymbol{U} \boldsymbol{U}^T) \ & E(\boldsymbol{U} \boldsymbol{V}^T) \\
E(\boldsymbol{V} \boldsymbol{U}^T) \ & E(\boldsymbol{V} \boldsymbol{V}^T)
\end{array}
\right)^{-1} ,
\quad & p>0, q>0, \\
\sigma^2 \Big( E(\boldsymbol{U} \boldsymbol{U}^T) \Big)^{-1},
\quad & q=0, p>0 ,\\
\sigma^2\Big ( E(\boldsymbol{V} \boldsymbol{V}^T) \Big )^{-1},
\quad & p=0, q>0.
\end{cases}
\tag{21.22}
\end{align}\]
\(\boldsymbol{U}=(U\_p,U\_{p-1},\dots,U\_1)^T\),
\(\boldsymbol{V}=(V\_q,V\_{q-1},\dots,V\_1)^T\),  
\(\{U\_t\}\)和\(\{V\_t\}\)分别是
AR(\(p\))和AR(\(q\))序列，
分别满足\(A(\mathscr B)U\_t=\varepsilon\_t\)和\(B(\mathscr B)V\_t=\varepsilon\_t\).

极限分布的利用:

用\(v\_{jj}\)表示\(V(\boldsymbol{\beta})\)的第\((j,j)\)的元素, 则
\[
\sqrt{N}( \hat \beta\_j - \beta\_j)
\]
依分布收敛到正态分布\(N(0, v\_{jj})\).
在实际问题中, 真值\(V(\boldsymbol{\beta})\)是未知的,
通常用估计量\(V(\hat \beta)\)代替.
于是可以用\(V(\hat \beta)\)的第\((j,j)\)的元\(\hat v\_{jj}\)作为\(v\_{jj}\)的近似.
这样, 当\(N\)较大时, 利用
\[
P[ \sqrt{N}|\hat \beta\_j - \beta\_j|/ \sqrt{v\_{jj}} \leq 1.96 ]
\approx 0.95
\]
就可以在置信水平0.95下得到\(\beta\_j\)的近似置信区间
\[
[\ \hat \beta\_j -1.96 \sqrt{\hat v\_{jj}/N},\quad
\hat \beta\_j +1.96 \sqrt{\hat v\_{jj}/N}\ ].
\]

## 21.5 例子：ARMA(4,2)的估计与模拟分析

对于ARMA(4,2)模型
\[\begin{align}
& X\_t + 0.9 X\_{t-1} + 1.4 X\_{t-2} + 0.7 X\_{t-3} + 0.6 X\_{t-4} \\
=& \varepsilon\_t + 0.5\varepsilon\_{t-1} - 0.4\varepsilon\_{t-2},
\quad t\in \mathbb Z,
\tag{21.23}
\end{align}\]
的模拟计算,  
其中\(\{\varepsilon\_t\}\)是标准正态\(\text{WN}(0,1)\).

```
demo.arma.est <- function(n=300, m=100){
  a <- -c(0.9, 1.4, 0.7, 0.6)
  b <- c(0.5, -0.4)
  sig <- 1.0
  
  ests <- matrix(0, nrow=m, ncol=7)
  for(ii in seq(m)){
    x <- arima.sim(
      model=list(ar=a, ma=b), n=n,
      rand.gen=function(n, ...) rnorm(n, 0, sig))
    ## 用R的arima函数估计模型参数。最大似然估计法。
    fit.mle <- arima(x, order=c(4,0,2), 
      include.mean=FALSE)
    ests[ii,] <- c(coef(fit.mle), fit.mle$sigma2)
  }
  res <- matrix(0, nrow=4, ncol=7)
  rownames(res) <- c("真实参数", "估计均值", 
                     "估计标准差", 
                     "估计根均方误差"
                     )
  colnames(res) <- c("a1", "a2", "a3", "a4", "b1", "b2", "s2")
  res[1,] <- c(a, b, sig^2)
  res[2,] <- apply(ests, 2, mean)
  res[3,] <- apply(ests, 2, sd)
  res[4,] <- sqrt(1/m*( (m-1)*res[3,]^2 + m*(res[2,]-res[1,])^2))
  cat("\n\n==== ARMA(4,2)模型模拟: 样本量=", n, " 重复", m, "次估计比较\n")
  print(round(res, 4))

  invisible(res)
}
set.seed(1)
demo.arma.est(m=400)
```

```
## 
## 
## ==== ARMA(4,2)模型模拟: 样本量= 300  重复 400 次估计比较
##                     a1      a2      a3      a4     b1      b2     s2
## 真实参数       -0.9000 -1.4000 -0.7000 -0.6000 0.5000 -0.4000 1.0000
## 估计均值       -0.8964 -1.3941 -0.6952 -0.5950 0.4995 -0.4043 0.9822
## 估计标准差      0.0639  0.0781  0.0748  0.0550 0.0770  0.0781 0.0839
## 估计根均方误差  0.0639  0.0782  0.0748  0.0551 0.0769  0.0781 0.0857
```

将这里的最大似然估计结果与教材P.223结果比较可以看出，
用最大似然估计可以大大改善MA部分的参数估计精度。

## 21.6 ARMA模型的检验

在得到了ARMA(\(p,q\))模型的参数估计
\[
(\hat a\_1,\dots, \hat a\_{p}), \ (\hat b\_1,\cdots, \hat b\_q)
\]
后, 对模型进行检验是十分必要的.

首先要检验模型的平稳性和合理性,
即要检验估计的参数满足[(21.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-rootcond0303).

然后对取定的初值
\[
x\_0=x\_{-1}=\dots=x\_{-p+1}= \hat \varepsilon\_0=\dots=\hat \varepsilon\_{-q+1}=0
\]
递推计算模型的残差
\[\begin{align}
\hat \varepsilon\_t = x\_t - \sum\_{l=1}^{p} \hat a\_l x\_{t-l}
+ \sum\_{j=1}^{ q} \hat b\_j \hat \varepsilon\_{t-j},
\ t=1,2,\dots
\tag{21.24}
\end{align}\]
取\(m =O(N^{1/3})\)和\(m>\max(p,q)\). 如果残差
\[
\hat \varepsilon\_t, \quad t= m, m+1,.., N
\]
可以通过白噪声的检验,
就认为模型合适. 否则寻找其他的模型.

在实际工作中, 参数\((p,q)\)是未知的.
但是根据数据的性质有时可以知道 阶数的大约范围.
可以在这个范围内对每一对\((p,q)\)建立ARMA\((p,q)\)模型,
如果一个模型可以通过检验, 就把 这个模型留做备用.

如果不能确定阶数的范围,
可以采用从\(p+q=1, p+q=2,\dots\)开始由低阶到高阶的依次搜寻的方法.
然后在所有备用的模型中选出\(p+q\)最小的一个模型.

如果\(p+q\)不能惟一决定 \((p,q)\),
可以取\(p\)较大的一个.
也可以在所有的备用模型中,
采用下面的AIC定阶方法, 最后确定一个模型.

## 21.7 ARMA模型的定阶方法

和AR模型的定阶方法相似,
给定ARMA(\(p,q\))模型的阶数
\((p,q)\)的一个估计\((k,j)=(\hat p, \hat q)\).
无论这个估计是怎样得到的,
按前面的方法都可以估计出ARMA(\(k,j\))模型的参数.
用\(\hat \sigma^2=\hat \sigma^2(k,j)\)表示白噪声方差\(\sigma^2\)的估计.
一般来讲, 希望\(\hat \sigma^2\)的取值越小越好.
因为\(\hat\sigma^2\)越小表示模型拟合的越精确.
但是，较小的残差方差\(\hat\sigma^2\)通常对应于较大的阶数\(k\), \(j\).
这样, 过多追求拟合的精度,
或说过分追求较小的残差方差
\(\hat\sigma^2\)会导致较大的\(\hat p\)和\(\hat q\),
从而导致较多的待估参数.
其结果会使建立的模型关于数据过于敏感,
从而降低模型的稳健性.

另一方面，参数过多的模型拟合较好但是对新数据的解释能力差。
AIC定阶准则就是为了克服参数过多的弊病而提出的。

ARMA模型的AIC定阶方法和AR模型的AIC定阶方法是相同的.
如果已知\(p\)的上界\(P\_0\)和\(q\)的上界\(Q\_0\),
对于每一对\((k,j)\), \(0\leq k\leq P\_0\),
\(0\leq j \leq Q\_0\) 计算AIC函数
\[\begin{align}
\mbox{AIC}(k,j) = \ln ( \hat \sigma^2(k,j)) +\frac{2(k+j)}{N}.
\tag{21.25}
\end{align}\]
AIC(\(k,j\))的最小值点\((\hat p, \hat q)\)称为\((p,q)\)的AIC定阶.
如果最小值不惟一, 应先取\(k+j\)最小的, 然后取\(j\)最小的.
一般AIC定阶并不是相合的.
也就是说,
如果平稳序列的观测\(\{x\_t\}\)确实是来自一个ARMA\((p,q)\)模型时,
AIC定阶\((\hat p, \hat q)\) 并不依概率收敛到真正的\((p,q)\).
但是, 和\(AR(p)\)模型的AIC定阶相似,
这种不相容性并不能否定AIC定阶的实用性.
因为一方面实际数据并不会真正满足某一个ARMA\((p,q)\)模型,
所以真正的阶数并不存在, 采用ARMA模型只是对数据的一种处理方法.
另一方面, AIC定阶有时会高估阶数, 但是并不会高估出很多.
这比低估阶数要好,
低估了阶数往往会造成更大的误差和模型的选择不当.

将[(21.25)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-order-aic0325)中的\(2(k+j)/N\)改为\((k+j)\ln N /N\)就得到BIC(\(k,j\))定阶.

## 21.8 ARMA模型的谱密度估计

如果得到了ARMA(\(p,q\)) 模型的参数估计, 模型的检验也已经通过.
可以利用
\[\begin{align}
\hat f(\lambda) = \frac{\hat \sigma^2}{2\pi}
\frac{ | 1 + \sum\_{j=1}^{\hat q} \hat b\_j e^{ij\lambda}|^2}
{|1 - \sum\_{j=1}^{\hat p} \hat a\_j e^{ij\lambda}|^2}
\tag{21.26}
\end{align}\]
作为谱密度的估计.

这是因为如果观测数据确实是ARMA(\(p,q\))序列[(21.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#eq:estarma-mod0301)时,
它的谱密度是
\[
f(\lambda) = \frac{\sigma^2}{2\pi}
\frac{| 1+ \sum\_{j=1}^{ q} b\_j e^{ij\lambda}|^2}
{|1- \sum\_{j=1}^p a\_j e^{ij\lambda}|^2.}
\]
不难看出,
如果\(\hat p, \hat q\), \(\hat a\_1,...,\hat a\_p\),
\(\hat b\_1, \hat b\_2,\cdots,\hat b\_q\)和
\(\hat\sigma^2\)分别是
\(p, q\), \(a\_1,...,a\_p\), \(b\_1,b\_2,..,b\_q\) 和
\(\sigma^2\)的相合估计,
则 \(\hat f(\lambda)\) 是 \(f(\lambda)\)的相合估计.

通常对谱密度进行估计只是为了解时间序列的频率特性.
后面有一章将主要介绍谱密度的估计方法.

### 21.8.1 谱密度估计示例

考虑§[21.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estarma.html#estarma-examp42sim)那里的ARMA(4,2)模型。
模拟生成300个观测数据值，
用最大似然法估计参数，
然后做出真实谱密度与估计的谱密度的对比图形。

```
arma.spec <- function(a, b, sigma, ngrid=201){
  p <- length(a)
  q <- length(b)
  freqs <- seq(from=0, to=pi, length=ngrid)
  spec1 <- numeric(ngrid)
  spec2 <- numeric(ngrid)
  for(ii in seq(ngrid)){
    spec1[ii] <- 1 + sum(complex(mod=b, arg=freqs[ii]*seq(q)))
    spec2[ii] <- 1 - sum(complex(mod=a, arg=freqs[ii]*seq(p)))
  }
  spec = sigma^2 / (2*pi) * abs(spec1)^2 / abs(spec2)^2
  
  list(frequency=freqs, spec=spec)
}
demo.arma.spec.sim <- function(n=300, ngrids=201){
  a <- -c(0.9, 1.4, 0.7, 0.6)
  b <- c(0.5, -0.4)
  sig <- 1.0
  
  x <- arima.sim(
    model=list(ar=a, ma=b), n=n,
    rand.gen=function(n, ...) rnorm(n, 0, sig))
  ## 用R的arima函数估计模型参数。最大似然估计法。
  fit.mle <- arima(x, order=c(4,0,2), 
    include.mean=FALSE)

  sres1 <- arma.spec(a=a, b=b, sigma=sig)
  freqs <- sres1$frequency
  spec <- sres1$spec
  plot(freqs, spec, type='l', lwd=2,
       main="ARMA(4,2) 谱密度估计",
       xlab="frequency", ylab="spectrum",
       axes=FALSE)
  axis(2)
  axis(1, at=(0:6)/6*pi,
       labels=c(0, expression(pi/6),
         expression(pi/3), expression(pi/2),
         expression(2*pi/3), expression(5*pi/6), expression(pi)))
  box()

  ## 添加估计值
  sres2 <- arma.spec(
    a=coef(fit.mle)[1:4],
    b = coef(fit.mle)[5:6],
    sigma=fit.mle$sigma2)
  lines(freqs, sres2$spec, lty=2)
  
  ## 图例
  legend("topleft", lty=c(1,2), lwd=c(2,1), legend=c("真实", "估计"))
  invisible()
}
set.seed(1)
demo.arma.spec.sim()
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/4035-estarma_files/figure-html/estarma-spec-sim-1.png)

## 21.9 ARIMA模型的参数估计

对于不平稳的数据，
常用的模型是ARIMA(\(p,d,q\))。
其中\(d\)取为使得数据\(\{X\_t \}\)在差分后平稳的差分阶数。
差分后是否平稳，
可以使用各种单位根检验方法。
一旦确定了\(d\)的值，
令\(y\_t = (1 - \mathscr B)^d X\_t\)，
以\(\{y\_t \}\)为观测数据建立ARMA(\(p,q\))模型即可。

## 21.10 季节ARIMA模型的参数估计

对于月度、季度频率的数据，
在建模中还要考虑固定的季节变化的影响。
如果数据本身平稳，
可以采用如下的模型：
\[
A(\mathscr B) A\_s(\mathscr B^s) X\_t
= B(\mathscr B) B\_s(\mathscr B^s) \varepsilon\_t,
\]
其中\(s\)是数据的频率，
比如月度数据\(s=12\)，
季度数据\(s=4\)。
这样的模型可以产生系数系数的模型，
能够用比较少的模型参数将季节性纳入模型。

如果数据不平稳，
则数据中可能含有单位根，
包括\((1-\mathscr B)^d\)这样的根，
和\((1-\mathscr B^s)^D\)这样的根，
模型可以表示为
\[
A(\mathscr B) A\_s(\mathscr B^s)
(1-\mathscr B)^d (1-\mathscr B^s)^D X\_t
= B(\mathscr B) B\_s(\mathscr B^s) \varepsilon\_t .
\]
将这样的模型记为\(\text{ARIMA}(p,d,q)(P,D,Q)\_s\)。
为了建模，
也只需要通过平稳性检验得到差分阶\(d\)和\(D\)的值，
令
\[
y\_t = (1-\mathscr B)^d (1-\mathscr B^s)^D X\_t,
\]
则\(\{y\_t\}\)服从平稳的季节ARMA模型，
仍可以用最大似然方法估计参数。

例如，
实际中常用的一个模型是\(\text{ARIMA}(0,1,1)(0,1,1)\_s\)模型，
称为航空模型。
其模型为
\[
(1-\mathscr B) (1-\mathscr B^s) X\_t
= (1 - \theta \mathscr B) (1 - \Theta \mathscr B^s)
\varepsilon\_t .
\]