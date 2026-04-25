---
crawl_time: '2026-01-17 14:29:20'
framework: sphinx
title: 20 MA模型的参数估计 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 20 MA模型的参数估计

## 20.1 MA(1)模型的参数估计

考虑MA(1)
\[\begin{aligned}
X\_t = \varepsilon\_t + b \varepsilon\_{t-1},
\quad t=1,2,\dots
\end{aligned}\]
其中\(|b|<1\)。
\(\gamma\_0=E X\_t^2 = \sigma^2(1 + b^2)\),
\(\gamma\_1 = \sigma^2 b\).
\(\rho\_1 = \frac{b}{1+b^2}\), 关于\(b\)的方程为
\[\begin{aligned}
\rho\_1 b^2 - b + \rho\_1 = 0
\end{aligned}\]
当\(|\rho\_1|\leq 0.5\)时\(b\)才有实解:
\[\begin{aligned}
b = \frac{1 - \sqrt{1 - 4\rho\_1^2}}{2\rho\_1}
\end{aligned}\]
(另一个解使\(|b|>1\)，抛弃)
用\(\hat\rho\_1 = \hat\gamma\_1 / \hat\gamma\_0\)代替理论值
可得\(b\)的矩估计
\[\begin{aligned}
\hat b = \frac{1 - \sqrt{1 - 4\hat\rho\_1^2}}{2\hat\rho\_1}
\end{aligned}\]

如果\(\{\varepsilon\_t\}\)是独立同分布的白噪声,
由§[16.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#estacv-cons)定理[16.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#thm:estacv-consistency)知道\(\hat\rho\_1\)是\(\rho\_1\)的强相合估计:
\(\lim\_{ N\to \infty} \hat \rho\_1=\rho\_1\) , a.s., 于是
\[
\lim\_{N\to \infty} \hat b = \frac{1-\sqrt{1-4 \rho^2\_1} }{2 \rho\_1}
=b, \ a.s.,
\]
所以\(\hat b\)是\(b\)的强相合估计.
实际上利用§[16.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#estacv-cons)定理[16.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#thm:estacv-clt)还可以证明,
当\(N \to \infty\)时, \(\sqrt{N}( \hat b - b)\)
依分布收敛到正态分布([27]).
\[
\text{N}\left(0, \frac{1+b^2+b^4+b^6+b^8}{(1- b^2)^2}\right).
\]

## 20.2 MA模型的矩估计及其计算

考虑可逆MA(\(q\))模型:
\[\begin{align}
X\_t = \varepsilon\_t + \sum\_{j=1}^q b\_j \varepsilon\_{t-j},
\quad t \in \mathbb Z,
\tag{20.1}
\end{align}\]
\(\{\varepsilon\_t\}\sim\text{WN}(0,\sigma^2)\), 系数满足可逆条件：
\[\begin{align}
B(z) = 1 + \sum\_{j=1}^q b\_j z^j \neq 0,
\quad |z|\leq 1,
\tag{20.2}
\end{align}\]
假定\(q\)已知，估计\(\boldsymbol{b} = (b\_1,\dots, b\_q)^T\)和\(\sigma^2\)。

参数与自协方差函数关系为
\[\begin{align}
\gamma\_k = \sigma^2(b\_0 b\_k + b\_1 b\_{k+1}
+ \dots + b\_{q-k}b\_q),
\quad 0 \leq k \leq q.
\tag{20.3}
\end{align}\]
(其中\(b\_0=1\))

从样本估计出\(\hat\gamma\_0, \hat\gamma\_1, \dots, \hat\gamma\_q\)后，
可以用解非线性方程组的方法求解\(\boldsymbol{b}\)和\(\sigma^2\)，
但不能保证解唯一，也不能保证可逆性条件。
求解方法包括线性迭代方法和Newton-Raphson迭代方法。

还可以根据§[12.8](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#mamod-coefiter)计算矩估计。
由MA(\(q\))的\(q\)步截尾性，
可定义
\[\begin{aligned}
\tilde \gamma\_k = \begin{cases}
\hat\gamma\_k, & 0 \leq k \leq q,\\
0, & k > q,
\end{cases}
\end{aligned}\]
用\(\{\tilde\gamma\_k\}\)作为\(\{\gamma\_k\}\)的估计代入§[12.8](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#mamod-coefiter)的计算公式。
记
\[\begin{align}
\begin{array}{llr}
A=\left(
\begin{array}{llllll}
0 & 1 & 0 & \cdots & 0 & 0 \\
0 & 0 & 1 & \cdots & 0 & 0 \\
\cdots & \cdots & & \cdots & \cdots & \\
0 & 0 & 0 & \cdots & 0 & 1 \\
0 & 0 & 0 & \cdots & 0 & 0
\end{array} \right )\_{q\times q}, &
C=\left( \begin{array}{c}
1 \\ 0 \\ \vdots \\0
\end{array}
\right)\_{q \times 1} \\ \\
\Omega\_k=\left( \begin{array}{llll}
\tilde\gamma\_1 & \tilde\gamma\_2 & \cdots & \tilde\gamma\_k \\
\tilde\gamma\_2 & \tilde\gamma\_3 & \cdots & \tilde\gamma\_{k+1} \\
\cdots & \cdots & \cdots & \cdots \\
\tilde\gamma\_q & \tilde\gamma\_{q+1} & \cdots & \tilde\gamma\_{q+k-1}
\end{array}
\right), &
\boldsymbol{\gamma}\_q=\left( \begin{array}{c}
\tilde\gamma\_1 \\ \tilde\gamma\_2 \\ \vdots \\ \tilde\gamma\_q
\end{array}
\right) &
\end{array}
\tag{20.4}
\end{align}\]
矩估计为
\[\begin{align}
(\hat b\_1, \dots, \hat b\_q)^T
= \frac{1}{\hat\sigma^2}(\boldsymbol\gamma\_q - A \hat\Pi C),
\quad
\hat\sigma^2 = \hat\gamma\_0 - C^T \hat\Pi C
\tag{20.5}
\end{align}\]
其中
\[
\hat\Pi = \lim\_{k\to\infty} \hat\Omega\_k \tilde\Gamma\_k^{-1} \hat\Omega\_k
\]

**定理20.1** 如果模型[(20.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-mod0202)中的\(\{\varepsilon\_t\}\)是独立同分布的\(\text{WN}(0,\sigma^2)\),
则几乎必然地当\(N\)充分大后由[(20.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-momest-matlim0206)计算的
\(\hat b\_1, \hat b\_2,\dots,\hat b\_q\)满足可逆条件[(20.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-cpoly0203).

**证明**
由于当\(N\to \infty\),\(\hat \gamma\_k\to \gamma\_k\) , a.s., 所以
\[\begin{aligned}
\hat f(\lambda) =& \frac{1}{2\pi}\sum\_{k=-q}^{q}
\hat \gamma\_k e^{-ik\lambda} \\
\to&
f(\lambda) = \frac{1}{2\pi}\sum\_{k=-q}^{q}
\gamma\_k e^{-ik\lambda}, \ \text{a.s.}, \ \hbox{当}\ N\to \infty
\end{aligned}\]
在\([-\pi, \pi]\)上一致成立.
于是当\(N\)充分大后\(\hat f(\lambda)\)恒正.
利用§[12.6](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#mamod-truncthm)的引理[12.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#lem:mamod-truncthm-lem12)知道有惟一的\((b\_1',b\_2',\dots,b\_q')\)满足可逆性条件[(20.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-cpoly0203),
并且使得
\[
\hat f(\lambda)
=\frac{\sigma^2\_0}{2\pi}
|1+\sum\_{j=1}^q b\_j'e^{-ij\lambda}|^2
\]
是
\[
Y\_t = e\_t+\sum\_{j=1}^q b\_j'e\_{t-j},
\ \ \{e\_t\} \sim \text{WN}(0,\sigma^2\_0)
\]
的谱密度.
这时, \(\tilde \gamma\_k = E(Y\_t Y\_{t+k})\).
再利用§[12.8](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#mamod-coefiter)知道
\[
(\hat b\_1, \hat b\_2,\dots,\hat b\_q)
=(b\_1',b\_2',\cdots,b\_q').
\]

○○○○○○

从定理[20.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#thm:estma-matlim)的证明知道这样得到的
\((\hat b\_1, \hat b\_2,\cdots,\hat b\_q)\)满足
\[
1+\sum\_{j=1}^q\hat b\_j z^j \neq 0, \ |z|\leq 1
\]
的充分条件是
\[
\sum\_{k=-q}^{q} \hat \gamma\_k e^{-ik\lambda}>0,
\ \lambda \in [-\pi, \pi].
\]

### 20.2.1 矩估计法的R程序

```
# Given \gamma_0, \gamma_1, \dots, \gamma_q,
## Solve MA(q) coefficients b_1, \dots, b_q, \sigma^2
## Using Li Lei's algorithm.
## Input: gms -- \gamma_0, \gamma_1, \dots, \gamma_q
ma.solve <- function(gms, k=100){
  q <- length(gms)-1
  if(q==1){
    rho1 <- gms[2] / gms[1]
    b <- (1 - sqrt(1 - 4*rho1^2))/(2*rho1)
    s2 <- gms[1] / (1 + b^2)
    return(list(b=b, s2=s2))
  }
  
  A <- matrix(0, nrow=q, ncol=q)
  for(j in seq(2,q)){
    A[j-1,j] <- 1
  }
  cc <- numeric(q); cc[1] <- 1
  gamma0 <- gms[1]
  gammas <- numeric(q+k)
  gammas[1:(q+1)] <- gms
  gamq <- gms[-1]
  Gammak <- matrix(0, nrow=k, ncol=k)
  for(ii in seq(k)){
    for(jj in seq(k)){
      Gammak[ii,jj] <- gammas[abs(ii-jj)+1]
    }
  }
  
  Omk <- matrix(0, nrow=q, ncol=k)
  for(ii in seq(q)){
    for(jj in seq(k)){
      Omk[ii,jj] <- gammas[ii+jj-1+1]
    }
  }
  PI <- Omk %*% solve(Gammak, t(Omk))

  s2 <- gamma0 - c(t(cc) %*% PI %*% cc)
  b <- 1/s2 * c(gamq - A %*% PI %*% cc)
  return(list(b=b, s2=s2))
}
```

**例20.1** 考虑如下MA(2)模型：
\[
X\_t = \varepsilon\_t - 0.36 \varepsilon\_{t-1} + 0.85 \varepsilon\_{t-2},
\ \varepsilon\_t \sim \text{WN}(0, 2^2)
\]
对样本量\(N=100, 300\)重复产生样本\(M=400\)组，
进行参数估计，
计算估计参数的平均值、标准差、根均方误差。

```
demo.ma2.mom <- function(m=400){
  b <- c(-0.36, 0.85)
  sig <- 2.0
  ests.100 <- matrix(0, nrow=m, ncol=3)
  ests.300 <- matrix(0, nrow=m, ncol=3)
  for(ii in seq(m)){
    x <- arima.sim(
      model=list(ma=b), n=300,
      rand.gen=function(n, ...) rnorm(n, 0, sig))
    ## 样本量100的结果
    gms1 <- c(acf(x[1:100], type="covariance", lag.max=2, plot=FALSE)$acf)
    res1 <- ma.solve(gms1)
    ## 样本量300的结果
    gms2 <- c(acf(x[1:300], type="covariance", lag.max=2, plot=FALSE)$acf)
    res2 <- ma.solve(gms2)
    ests.100[ii,] <- c(res1$b, res1$s2)
    ests.300[ii,] <- c(res2$b, res2$s2)
  }
  res <- matrix(0, nrow=7, ncol=3)
  rownames(res) <- c("真实参数", "样本量100估计均值", "样本量300估计均值",
                     "样本量100估计标准差", "样本量300估计标准差",
                     "样本量100估计根均方误差", "样本量300估计根均方误差"
                     )
  colnames(res) <- c("b1", "b2", "s2")
  res[1,] <- c(b, sig^2)
  res[2,] <- apply(ests.100, 2, mean)
  res[3,] <- apply(ests.300, 2, mean)
  res[4,] <- apply(ests.100, 2, sd)
  res[5,] <- apply(ests.300, 2, sd)
  res[6,] <- sqrt(1/m*( (m-1)*res[4,]^2 + m*(res[2,]-res[1,])^2))
  res[7,] <- sqrt(1/m*( (m-1)*res[5,]^2 + m*(res[3,]-res[1,])^2))
  print(round(res, 4))

  invisible(res)
}
demo.ma2.mom(m=400)
```

```
##                              b1     b2      s2
## 真实参数                -0.3600 0.8500  4.0000
## 样本量100估计均值       -0.4583 1.0040  6.4688
## 样本量300估计均值       -0.2798 0.3969  4.3447
## 样本量100估计标准差      2.1513 6.0264 31.4712
## 样本量300估计标准差      1.6003 5.0242 10.3337
## 样本量100估计根均方误差  2.1509 6.0209 31.5286
## 样本量300估计根均方误差  1.6003 5.0384 10.3265
```

可以看出矩估计法的误差太大，
在中小样本情形无法接受。
经检查，
如果输入的是理论自协方差函数，
求解的结果是完全正确的，
所以并不是程序问题，
而是矩估计法的问题。
极大似然估计可以获得较满意的估计精度。

## 20.3 MA模型参数估计的逆相关函数法

因为AR的Yule-Warker估计能保证最小相位性所以想到把MA变成一个AR再估计。

**定义20.1** 设平稳序列\(\{X\_t\}\)有恒正的谱密度\(f(\lambda)\). 通常称
\[\begin{align}
f\_y(\lambda) =\frac{1}{4\pi^2 f(\lambda)}.
\tag{20.6}
\end{align}\]
为\(\{X\_t\}\)的**逆谱密度**.
称
\[
\gamma\_y(k)
\stackrel{\triangle}{=} \int\_{-\pi}^{\pi}
e^{ik\lambda} f\_y(\lambda) \; d \lambda
\]
为\(\{X\_t\}\)的**逆相关函数**或**逆自相关函数**.
严格来说应该叫做**逆自协方差函数**。

设\(\{X\_t\}\)为可逆MA(\(q\))序列。其谱密度为
\[\begin{aligned}
f(\lambda) = \frac{\sigma^2}{2\pi} |B(e^{i\lambda})|^2
\end{aligned}\]
逆谱密度为
\[\begin{align}
f\_y(\lambda) = \frac{\sigma^{-2}}{2\pi} |B(e^{i\lambda})|^{-2}
\tag{20.7}
\end{align}\]
\(f\_y\)为如下AR(\(q\))序列的谱密度
\[\begin{align}
Y\_t = -\sum\_{j=1}^q b\_j Y\_{t-j} + e\_t, \quad t\in \mathbb Z
\tag{20.8}
\end{align}\]
其中\(\{e\_t\}\sim\text{WN}(0,\sigma^{-2})\)。

\(\{Y\_t\}\)的自协方差函数
\[\begin{align}
\gamma\_y(k) = \int\_{-\pi}^\pi e^{ik\lambda} f\_y(\lambda)\;d\lambda,
\quad k=0,1,2,\dots
\tag{20.9}
\end{align}\]
是\(\{X\_t\}\)的逆相关函数。

只要能估计\(\{\gamma\_y(k)\}\)，设估计为\(\{\hat\gamma\_y(k) \}\)，
就可以用Y-W方法估计\(\{Y\_t\}\)的参数\(b\_1,\dots,b\_q\)和\(\sigma^{-2}\)：
\[\begin{align}
& \left(\begin{array}{cccc}
\hat\gamma\_y(0) & \hat\gamma\_y(1) & \cdots & \hat\gamma\_y(q-1) \\
\hat\gamma\_y(1) & \hat\gamma\_y(0) & \cdots & \hat\gamma\_y(q-2) \\
\vdots & \vdots & \ddots & \vdots \\
\hat\gamma\_y(q-1) & \hat\gamma\_y(q-2) & \cdots & \hat\gamma\_y(0)
\end{array}\right)
\left(\begin{array}{c}
-\hat b\_1 \\
-\hat b\_2 \\
\vdots \\
-\hat b\_q
\end{array}\right)
= \left(\begin{array}{c}
\hat\gamma\_y(1) \\
\hat\gamma\_y(2) \\
\vdots \\
\hat\gamma\_y(q)
\end{array}\right)
\tag{20.10}
\\
& \hat\sigma^{-2} = \hat\gamma\_y(0) + \hat b\_1 \hat\gamma\_y(1) + \dots + \hat b\_q \hat\gamma\_y(q)
\tag{20.11}
\end{align}\]

从而得到MA(\(q\)) 序列\(\{X\_t\}\)的参数估计，
且系数满足可逆性条件。

**引理20.1** 如果 \((a\_1,a\_2,\dots,a\_p)\) 和 \(\sigma^2\)分别是AR\((p)\)模型
\[
X\_t=\sum\_{j=1}^p a\_j X\_{t-j} + \varepsilon\_t, \ \ t\in \mathbb Z,
\]
的自回归系数和白噪声\(\{\varepsilon\_t\}\)的方差, 则
\(\{X\_t\}\)的逆相关函数为
\[
\gamma\_y(k)
= \begin{cases}
\frac{1}{\sigma^2} \sum\_{j=0}^{p-k}a\_j a\_{j+k},
&0 \leq k \leq p, \ \ a\_0\stackrel{\triangle}{=} -1, \\
0, \ &k>p.
\end{cases}
\]

**证明**
\(\{X\_t\}\)有谱密度
\[
f(\lambda) =\frac{\sigma^2}{2\pi}|A(e^{i\lambda})|^{-2},
\ \hbox{ 其中 }
\ A(z)=1-\sum\_{j=1}^p a\_j z^j,
\]
和逆谱密度
\[
f\_y(\lambda)=\frac{1}{ 4\pi^2 f(\lambda)}
= \frac{1}{2\pi\sigma^2}|A(e^{i\lambda})|^2,
\]
于是有逆相关函数
\[\begin{aligned}
\gamma\_y(k)
= \int\_{-\pi}^{\pi} e^{ik\lambda} f\_y(\lambda) d \lambda
= \begin{cases}
\frac{1}{\sigma^2}
\sum\_{j=0}^{p-k} a\_j a\_{j+k}, & 0 \leq k \leq p, \\
0, &k>p.
\end{cases}
\end{aligned}\]

○○○○○○

满足[(20.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-mod0202)的可逆MA\((q)\)序列\(\{X\_t\}\)可以写成无穷阶自回归的形式
\[\begin{align}
X\_t - \sum\_{j=1}^{\infty} a\_j X\_{t-j} =\varepsilon\_t,
\quad t=1,2,..,
\tag{20.12}
\end{align}\]
这里的回归系数\(\{a\_j\}\) 由 \(1/B(z)\) 在单位圆内的Taylor级数决定:
\[
\frac1{B(z)} = 1 - \sum\_{j=1}^\infty a\_j z^j ,
\ |z|\leq 1.
\]
由于当 \(j\to \infty\), \(a\_j\)是以负指数阶收敛到\(0\)的,
所以对较大的正整数 \(p\)
可以将无穷阶自回归模型[(20.12)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-ma2infar0213)写成近似的长阶(\(p\)阶)自回归的形式
\[\begin{align}
X\_t\approx \sum\_{j=1}^p a\_j X\_{t-j} + \varepsilon\_t,
\ t=1,2,..,
\tag{20.13}
\end{align}\]
利用引理[20.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#lem:estma-racv-maracv)知道\(\{X\_t\}\)的逆相关函数\(\gamma\_y(k)\)满足
\[\begin{align}
\gamma\_y(k) \approx
\frac1{\sigma^2} \sum\_{j=0}^{p-k} a\_j a\_{j+k},
\ 0\leq k \leq q,
\ a\_0\stackrel{\triangle}{=} -1.
\tag{20.14}
\end{align}\]

用样本\(x\_1,x\_2,\dots,x\_N\)计算逆相关函数\(\hat\gamma\_y(k)\)和\(\hat b\_1,\dots,\hat b\_q, \hat\sigma^2\)的方法如下。

(1) 首先利用\(\{x\_t\}\)的样本自协方差函数\(\hat \gamma\_k\)建立一个AR\((p\_N)\)模型,
这里\(p\_N\)可以是AR模型的AIC定阶,
也可以取作\(K\ln (N)\)的整数部分, \(K\)是一个正数.

(2) 对\(p\equiv p\_N\)解样本Yule-Walker方程[(19.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estar.html#eq:estar-yweq0103), [(19.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estar.html#eq:estar-yweqsig0104),
得到样本Yule-Walker 系数
\[
\hat{\boldsymbol a} = (\hat a\_{1}, \hat a\_{2},\dots,\hat a\_{p})^T\
\hbox{ 和 } \ \hat\sigma^2\_p.
\]

(3) 计算样本逆相关函数
\[
\hat \gamma\_y(k)
= \frac{1}{\hat \sigma^2\_p} \sum \_{j=0}^{p-k}
\hat a\_{j} \hat a\_{j+k},
\ k=0,1,2,\dots,q, \ \hat a\_{0} \stackrel{\triangle}{=} -1.
\]

(4) 利用样本Yule-Walker方程[(20.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-racvyweq0211)和[(20.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-racvywsig0212)计算出 MA\((q)\)系数的估计
\(\boldsymbol{\hat b}\_q=(\hat b\_1, \hat b\_2,..,\hat b\_q)^T\)
和 \(\hat \sigma^2\).

这种办法是用一个长阶自回归来近似\(\{X\_t\}\)，
自回归序列的逆自相关函数很容易计算，这样得到逆相关函数。

逆相关函数法估计MA参数的R程序：

```
## 用输入的$\gamma_0, \dots, \gamma_p$求解AR(p)模型参数
ar.yw <- function(gam){
  p <- length(gam) - 1
  G <- matrix(0, p, p)
  for(i in 1:p){
    for(j in 1:p){
      G[i,j] = gam[1 + abs(i-j)]
    }
  }
  gri <- gam[-1]
  a <- solve(G, gri)
  sig2 <- sum(gam * c(1, -a))
  list(ar = a, var.pred = sig2)
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

## 用长阶自回归方法和逆相关函数方法估计MA模型
ma.solve.racv <- function(x, q=1){

  ## 拟合长阶自回归，用来估计逆自协方差函数
  n <- length(x)
  plar <- round(sqrt(n))
  if(plar < q) plar <- q
  
  mod1 <- ar(x, aic = FALSE, order.max = plar, method="yule-walker")
  a <- mod1$ar
  sigsq <- mod1$var.pred
  gamr <- ar_racv(a, sigsq)[1:(q+1)]
  res2 <- ar.yw(gamr)
  b <- -res2$ar
  sig2 <- 1/res2$var.pred
  
  list(ma = b, var.pred = sig2)
}
```

## 20.4 MA模型参数估计的新息方法

用新息预报公式可以计算\(\{X\_t\}\)的样本新息。
\[\begin{aligned}
\hat\varepsilon\_{t+1} =& X\_{t+1} - \hat X\_{t+1}
\stackrel{\triangle}{=} X\_{t+1} - L(X\_{t+1} | X\_1,\dots,X\_{t}) \\
=& X\_{t+1} - L(X\_{t+1} | \hat\varepsilon\_1,\dots,\hat\varepsilon\_{t}) \\
=& X\_{t+1} - \sum\_{j=1}^t \theta\_{t,j} \hat\varepsilon\_{t+1-j},
\quad t=1,2,\dots
\end{aligned}\]
其中\(\hat X\_1 \stackrel{\triangle}{=} 0\)，\(\{\theta\_{t,j}\}\)可递推计算，
预报误差\(\nu\_t = E \hat\varepsilon\_{t+1}^2\)可递推计算。
对MA(\(q\))序列\(\{X\_t\}\)上述新息预报公式中当\(t\geq q, j>q\)时\(\theta\_{t,j}=0\),
新息预报公式变成
\[\begin{align}
\hat X\_{t+1} = \sum\_{j=1}^q \theta\_{t,j} \hat\varepsilon\_{t+1-j},
\quad t\geq q
\tag{20.15}
\end{align}\]

对MA(\(q\))序列\(\{X\_t\}\)，其白噪声项\(\{\varepsilon\_t\}\)是新息, \(t\to\infty\)时:
\[\begin{aligned}
\varepsilon\_{t+1} =& X\_{t+1} - L(X\_{t+1} | X\_t, X\_{t-1}, X\_{t-2}, \dots) \\
\approx& X\_{t+1} - L(X\_{t+1} | X\_t, X\_{t-1}, X\_{t-2}, \dots, X\_1) \\
=& \hat\varepsilon\_{t+1}
\end{aligned}\]
这是因为§[23.2.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#wold-wold-infdimproj)定理[23.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimproj)，
无穷长历史最佳线性预测是有限历史预测的极限。
于是\(t\)较大时
\[\begin{aligned}
X\_t =& \varepsilon\_t + b\_1 \varepsilon\_{t-1} + \dots + b\_q \varepsilon\_{t-q} \\
\approx& \hat\varepsilon\_t + b\_1 \hat\varepsilon\_{t-1}
+ \dots + b\_q \hat\varepsilon\_{t-q} \\
=& X\_t - \hat X\_t + b\_1 \hat\varepsilon\_{t-1}
+ \dots + b\_q \hat\varepsilon\_{t-q}
\end{aligned}\]
说明
\[\begin{aligned}
\hat X\_t \approx b\_1 \hat\varepsilon\_{t-1}
+ \dots + b\_q \hat\varepsilon\_{t-q}
\end{aligned}\]
上式与[(20.15)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-innov-maqpred0216))的新息预报公式比较可知当\(t\)较大时
可以用\(\hat b\_j = \theta\_{t,j}\)估计\(b\_j\)，
用\(\nu\_t\)估计\(\sigma^2\)。
这种估计称为**新息估计**。

MA参数的新息估计算法如下:

* 给定观测数据\(x\_1,x\_2,\dots,x\_N\),
  取\(m=o(N^{1/3})\).
* 计算样本自协方差函数
  \(\hat\gamma\_0, \hat \gamma\_1, \cdots, \hat \gamma\_m\).
* \(\boldsymbol{b}\) 和 \(\sigma^2\)的新息估计
  \[\begin{align}
  (\hat b\_1, \hat b\_2,\dots,\hat b\_q)
  = (\hat \theta\_{m,1}, \hat
  \theta\_{m,2},\dots,\hat \theta\_{m,q}),
  \ \ \hat \sigma^2 = \hat \nu\_m.
  \tag{20.16}
  \end{align}\]
  由下面的新息预报递推公式得到.
  \[\begin{align}
  \begin{cases}
  \hat \nu\_0=\hat \gamma\_0, &\\
  \hat \theta\_{n,n-k}
  = \hat \nu\_k^{-1}[\hat \gamma\_{n-k} -
  \sum\_{j=0}^{k-1} \hat \theta\_{k,k-j}
  \hat \theta\_{n,n-j}\hat \nu\_j],
  & 0\leq k \leq n-1,\\
  \hat \nu\_n=\hat \gamma\_0-\sum\_{j=0}^{n-1}
  \hat \theta^2\_{n,n-j}\hat \nu\_j,
  & 1\leq n \leq m,
  \end{cases}
  \tag{20.17}
  \end{align}\]
  其中\(\sum\_{j=0}^{-1}(\cdot)\stackrel{\triangle}{=} 0\).
* 递推次序是
  \[
  \hat \nu\_0; \ \hat \theta\_{1,1}, \hat \nu\_1;
  \ \hat \theta\_{2,2}, \hat \theta\_{2,1}, \hat \nu\_2;
  \hat \theta\_{3,3},\hat \theta\_{3,2}, \hat \theta\_{3,1}, \hat \nu\_3;
  \cdots.
  \]

下面的定理与推论给出了新息估计方法的相合性。

**定理20.2** 设\(\{X\_t\}\)是可逆ARMA(\(p,q\))序列, 满足
\[
A(\mathscr B)X\_t=B(\mathscr B)\varepsilon\_{t}, \ \ t\in \mathbb Z.
\]
\(\{\psi\_j\}\)是\(B(z)/A(z)\)的Taylor级数系数.
如果\(\{\varepsilon\_t\}\)是4阶矩有限的独立同分布\(\text{WN}(0,\sigma^2)\),
正整数列\(m=m(N)<N\) 满足当\(N\to \infty\) 时,
\(m\to \infty\) 和 \(m=o(N^{1/3})\).
则对任何正整数\(q\), 当\(N\to \infty\)
\[
\sqrt{N}(\hat \theta\_{m,1}- \psi\_1,
\hat \theta\_{m,2}- \psi\_2, \dots,\hat\theta\_{m,q} - \psi\_q)
\]
依分布收敛到\(q\)维正态分布\(N(0,A)\),
其中\(q\times q\)矩阵
\[
A=(a\_{i,j}), \ \ a\_{i,j}=
\sum\_{k=1}^{\min(i,j)}\psi\_{i-k}\psi\_{j-k}.
\]
并且\(\hat \nu\_m\) 依概率收敛到\(\sigma^2\).

参见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book))。

**推论20.1** 对于MA(\(q\))序列\(\{X\_t\}\),
当模型[(20.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-mod0202)中的白噪声是4阶矩有限的独立同分布序列时,
新息估计[(20.16)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-innov-coefipred0217)是相合估计,
当\(N\)趋于无穷时,
\[
\sqrt{N}(\hat b\_{1}- b\_1, \hat b\_2 - b\_2,\cdots,\hat b\_q - b\_q)
\]
依分布收敛到\(q\)维正态分布\(N(0,A)\), 其中\(q\times q\)矩阵
\[
A=(a\_{i,j}),
\ \ a\_{i,j}= \sum\_{k=1}^{\min(i,j)}b\_{i-k}b\_{j-k},
\ \ b\_0\stackrel{\triangle}{=} 1.
\]
并且\(\hat \nu\_m\) 依概率收敛到\(\sigma^2\).

注意需要\(m\to\infty\)时新息方法得到的参数估计才是相合的，
不能用\(\theta\_{q,1},\dots,\theta\_{q,q}\)作为\(b\_1,\dots,b\_q\)的估计。
实际中，\(m\)也不能取得太大，因为新息预测的递推公式中用到\(\hat\gamma\_{m-1}\)。

## 20.5 MA模型的定阶方法

由于MA(\(q\))序列的特征是自相关系数\(q\)后截尾,
所以当样本自相关系数 \(\hat\rho\_k = \hat \gamma\_k/ \hat \gamma\_0\)
从某一点\(\hat q\)后变得很小时,
可以\(\hat q\)作为\(q\)的估计.
从§[16.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#estacv-clt)的定理[16.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#thm:estacv-clt)及§[16.3.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estacv.html#estacv-clt-matest)知道,
对于\(m>q\), \(\sqrt{N} \hat \rho\_m\)依分布收敛到期望为0,
方差为
\[
1 + 2\rho\_1^2 + 2\rho\_2^2+ \dots + 2\rho\_q^2
\]
的正态分布.
这样由样本自相关系数\(\hat \rho\_k,
k=1,2,\cdots,m\)的图形可以大致得到\(q\)的估计,
同时也可以判断采用MA(\(q\))模型的合理与否.

还可以用AIC定阶方法: 如果根据问题
的背景或数据的特性能够判定MA(\(q\))模型阶数\(q\)的上界是\(Q\_0\).
对于\(m=0,1,2,\dots,Q\_0\)按前述的方法逐个拟合MA(\(m\))模型.
白噪声方差\(\sigma^2\)的估计量记做\(\hat \sigma^2\_m\).
定义AIC函数
\[
\text{AIC}(m) = \ln (\hat \sigma^2\_m) + 2m/N,
\quad m=0,1,\dots,Q\_0.
\]
这里 \(N\)是样本个数.
AIC(m)的最小值点\(\hat q\) (如不惟一, 应取小的)
称为MA(\(q\))模型的AIC定阶.

## 20.6 MA模型的拟合检验

从观测数据\(x\_1,x\_2,\cdots,x\_N\)
得到模型的参数估计\(\hat q\), \(\hat b\_1,\hat b\_2,\dots,\hat b\_{\hat q}\)
和\(\hat \sigma^2\)后, 取
\[\begin{aligned}
\hat \varepsilon\_{1-\hat q} & =
\hat \varepsilon\_{2-\hat q} =\dots=\hat \varepsilon\_0 =0,\\
y\_t =& x\_t-\bar x\_N,\\
\hat \varepsilon\_t =& y\_t - \sum\_{j=1}^{\hat q}
\hat b\_j \hat \varepsilon\_{t-j},
\ t=1,2,\dots,N.
\end{aligned}\]
对\(L=O(N^{1/3})\),
如果\(\{\hat \varepsilon\_t: t=L, L+1,\cdots,N\}\)
能够通过白噪声检验, 就认为模型的选择合适.
否则改变\(\hat q\)的取值,
拟合新的MA模型或改用其他的模型,
例如改用ARMA模型等.

## 20.7 MA谱密度估计

如果从数据得到了MA 模型的参数估计, 模型的检验也已经通过,
可以利用
\[
\hat f(\lambda)
= \frac{\hat \sigma^2}{2\pi}
| 1+ \sum\_{j=1}^{\hat q}
\hat b\_j e^{ij\lambda}|^2
\]
作为所关心的平稳序列的谱密度的估计.
这是因为如果观测数据确实是MA\((q)\)序列[(20.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-estma.html#eq:estma-mod0202)时,
它的谱密度是
\[
f(\lambda) = \frac{\sigma^2}{2\pi}
| 1+ \sum\_{j=1}^{q} b\_j e^{ij\lambda}|^2.
\]
不难看出,
如果\(\hat q\), \(\hat b\_1, \hat b\_2,\dots,\hat b\_q\)和
\(\hat\sigma^2\)分别是\(q\), \(b\_1,b\_2,..,b\_q\) 和 \(\sigma^2\)的相合估计,
则\(\hat f(\lambda)\) 是 \(f(\lambda)\)的相合估计.

### References

Brockwell, P. J., and R. A. Davis. 1987. *Time Series: Theory and Methods*. Springer-Verlag.