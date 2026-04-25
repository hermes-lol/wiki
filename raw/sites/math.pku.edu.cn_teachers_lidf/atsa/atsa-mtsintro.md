---
crawl_time: '2026-01-17 14:29:06'
framework: sphinx
title: 30 多元平稳序列介绍 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 30 多元平稳序列介绍

## 30.1 多维平稳序列的概念

沿时间变化的量经常有多个，
互相之间有相关性。
如第1章的北京地区洪涝灾害受灾面积\(X\_t\)和成灾面积\(Y\_t\)有正相关，
可以看成向量值时间序列
\[\begin{aligned}
(X\_t, Y\_t), \ t \in \mathbb N
\end{aligned}\]

向量值的时间序列称为多维（多元）时间序列。
仅介绍多维平稳序列。

**定义30.1 (多维平稳序列)** 称\(m\)维随机序列 \(\boldsymbol X\_t = (X\_{1t}, X\_{2t}, \dots, X\_{mt})^T\),
\(t \in \mathbb Z\)是**平稳序列**，
如果对任何\(t, n \in \mathbb Z\),

* (1) \(E \boldsymbol X\_t = \boldsymbol \mu = (\mu\_1, \mu\_2, \dots, \mu\_m)^T\)与\(t\)无关；
* (2) \(\Gamma(n) = E[(\boldsymbol X\_{t+n} - \boldsymbol \mu)(\boldsymbol X\_t - \boldsymbol \mu)^T]\)
  与\(t\)无关。

这时称\(\{\Gamma(n), n \in \mathbb Z \}\)为平稳序列\(\{ \boldsymbol X\_t \}\)的自协方差函数（矩阵）。

定义
\[\begin{align}
\gamma\_{jk}(n) =& E[(X\_{j,t+n} - \mu\_j) (X\_{kt} - \mu\_k)]
\tag{30.1}
\end{align}\]
则
\[\begin{align}
\Gamma(n) =& (\gamma\_{jk}(n))\_{j,k=1,2,\dots,m}.
\tag{30.2}
\end{align}\]

定义自相关系数
\[\begin{align}
\rho\_{jk}(n) = \frac{\gamma\_{jk}(n)}{\sqrt{ \gamma\_{jj}(0) \gamma\_{kk}(0) }}
\tag{30.3}
\end{align}\]
称
\[\begin{aligned}
R(n) = (\rho\_{jk}(n))\_{j,k=1,2,\dots,m}
\end{aligned}\]
为\(\{ \boldsymbol X\_t \}\)的自相关系数（矩阵）。

把\(\{ \boldsymbol X\_t \}\)标准化得
\[\begin{aligned}
\boldsymbol Y\_t =& \left( \frac{X\_{1t} - \mu\_1}{\sqrt{\gamma\_{11}(0)}},
\frac{X\_{2t} - \mu\_2}{\sqrt{\gamma\_{22}(0)}}, \dots,
\frac{X\_{mt} - \mu\_m}{\sqrt{\gamma\_{mm}(0)}} \right)^T
\end{aligned}\]
则\(\{ R(n) \}\)是平稳序列\(\{ \boldsymbol Y\_t \}\)的自协方差函数。

**定理30.1** 对任何\(n \in \mathbb Z\),

* (1) \(\Gamma(-n) = [\Gamma(n)]^T\);
* (2) \(|\gamma\_{jk}(n)| \leq [ \gamma\_{jj}(0) \gamma\_{kk}(0) ]^{1/2}\);
* (3) 非负定性：对任何实向量
  \(\boldsymbol\alpha\_1, \boldsymbol\alpha\_2, \dots, \boldsymbol\alpha\_n \in \mathbb R^m\),
  \[\begin{aligned}
  \sum\_{k=1}^n \sum\_{j=1}^n \boldsymbol\alpha\_k^T \Gamma(k-j) \boldsymbol\alpha\_j \geq 0.
  \end{aligned}\]

这些性质\(\{R(n)\}\)也满足。

**证明**:
(1) 
\[\begin{aligned}
\Gamma(-n) =& E\left[(\boldsymbol X\_{t-n} - \boldsymbol \mu)(\boldsymbol X\_t - \boldsymbol \mu)^T\right] \\
=& \left\{ E \left[ (\boldsymbol X\_t - \boldsymbol \mu) (\boldsymbol X\_{t-n} - \boldsymbol \mu)^T
\right]\right\}^T \\
=& \left[ \Gamma(n) \right]^T
\end{aligned}\]

(2) 这就是Schwarz不等式。

(3) 记\(\boldsymbol Y\_t = \boldsymbol X\_t - \boldsymbol \mu\), 则
\[\begin{aligned}
& E \left[ \sum\_{j=1}^n \boldsymbol\alpha\_j^T \boldsymbol Y\_j \right]^2 \\
=& E \sum\_{k=1}^n \sum\_{j=1}^n (\alpha\_k^T \boldsymbol Y\_k) (\alpha\_j^T \boldsymbol Y\_j) \\
=& \sum\_{k=1}^n \sum\_{j=1}^n \boldsymbol\alpha\_k^T E[\boldsymbol Y\_k \boldsymbol Y\_j^T] \boldsymbol\alpha\_j \\
= & \sum\_{k=1}^n \sum\_{j=1}^n \boldsymbol\alpha\_k^T \Gamma(k-j) \boldsymbol\alpha\_j \geq 0
\end{aligned}\]

○○○○○○

**定义30.2 (平稳相关)** 设\(\{X\_t \}\)和\(\{Y\_t \}\)是两个平稳列，
如果\(\text{Cov}(X\_s, Y\_t) = r\_{s-t}\)对任意\(s, t\)成立，
则称\(\{X\_t \}\)和\(\{Y\_t \}\)**平稳相关**。

**例30.1 (多维白噪声)** 若\(m\)维平稳序列\(\{\boldsymbol X\_t \}\)满足
\[\begin{aligned}
E \boldsymbol X\_1 = \boldsymbol \mu, \quad \Gamma(n) = Q \delta\_n
\end{aligned}\]
就称\(\{\boldsymbol X\_t \}\)是**\(m\)维白噪声**，
简记为WN\((\boldsymbol\mu, Q)\)。

这时\(\forall k, j\), 只要\(n \neq 0\)，就有
\[\begin{aligned}
E[(X\_{kt} - \mu\_k)(X\_{j,t+n} - \mu\_j)] = 0
\end{aligned}\]
另外，每个分量是一维白噪声。
特别地，当\(Q\)是对角阵时，
各分量是\(m\)个互不相关的白噪声。

○○○○○○

**例30.2 (多维线性平稳列)** 设\(\{ \boldsymbol\varepsilon\_t, t\in \mathbb Z \}\)是\(m\)维WN(0,\(Q\)),
\(Q = E[\boldsymbol\varepsilon\_t \boldsymbol\varepsilon\_t^T]\)。
如果实矩阵列\(\{ C\_j \}\)满足
\[\begin{aligned}
\sum\_{j=-\infty}^\infty C\_j Q C\_j^T < \infty
\end{aligned}\]
就称
\[\begin{align}
\boldsymbol X\_t =& \sum\_{j=-\infty}^\infty C\_j \boldsymbol\varepsilon\_{t-j}, \ t \in \mathbb Z
\tag{30.4}
\end{align}\]
为**\(m\)维平稳线性序列**。

这时可以证明[(30.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-def-mlinserdef0105)中每个分量都是均方收敛的，并且
\[\begin{align}
E \boldsymbol X\_t =& 0, \\
\Gamma(n) =& E[\boldsymbol X\_{t+n} \boldsymbol X\_t^T] = \sum\_{j=-\infty}^\infty C\_{j+n} Q C\_j^T
\tag{30.5}
\end{align}\]

**例30.3 (多维MA模型)** 设\(\{ \boldsymbol\varepsilon\_t \}\)是\(m\)维WN(0,\(Q\))。
如果\(B\_1, B\_2, \dots, B\_q\)是\(m \times m\)实矩阵，
满足
\[\begin{aligned}
\mbox{det}(I\_m + B\_1 z + B\_2 z^2 + \dots + B\_q z^q) \neq 0,
\ |z| < 1
\end{aligned}\]
就称
\[\begin{aligned}
\boldsymbol X\_t = \boldsymbol\varepsilon\_t + B\_1 \boldsymbol\varepsilon\_{t-1}
+ \dots + B\_q \boldsymbol\varepsilon\_{t-q},
\ t \in \mathbb Z
\end{aligned}\]
为一个**\(m\)维MA(\(q\))序列**。

若进一步要求
\[\begin{aligned}
\mbox{det}(I\_m + B\_1 z + B\_2 z^2 + \dots + B\_q z^q) \neq 0,
\ |z| \leq 1
\end{aligned}\]
则称\(\{ \boldsymbol X\_t \}\)为一个可逆的MA(\(q\))序列。

○○○○○○

矩阵系数多项式：

设\(A\_1, A\_2, \dots, A\_p, B\_1, B\_2, \dots, B\_q\)是\(p+q\)个\(m\times m\)实矩阵，记
\[\begin{aligned}
A(z) =& I\_m - A\_1 z - A\_2 z^2 - \dots - A\_p z^p \\
B(z) =& I\_m + B\_1 z + B\_2 z^2 + \dots + B\_q z^q
\end{aligned}\]
形如\(A(z), B(z)\)的多项式被称为**矩阵系数多项式**，
实际是\(m^2\)维向量值函数，
每个分量是多项式。

如果对任意满足
\[\begin{aligned}
A(z)= C(z) A\_1(z), \ B(z) = C(z) B\_1(z)
\end{aligned}\]
的\(m \times m\)矩阵系数多项式\(C(z)\)必有\(\mbox{det}(C(Z))=\)常数，
就称\(A(z)\)和\(B(z)\)是**左互素**的。

**例30.4 (多维ARMA的不可识别性)** 设\(\{ \boldsymbol\varepsilon\_t \}\)是WN(0,\(Q\))。
称\(m\)维平稳序列\(\{ \boldsymbol X\_t \}\)满足\(m\)维平稳可逆的ARMA(\(p,q\))模型，
如果对任何\(t \in \mathbb Z\)，
\[\begin{align}
\boldsymbol X\_t = \sum\_{j=1}^p A\_j \boldsymbol X\_{t-j} + \varepsilon\_t
+ \sum\_{j=1}^q B\_j \varepsilon\_{t-j}
\tag{30.6}
\end{align}\]
其中多项式\(A(z), B(z)\)满足
\[\begin{aligned}
(1) & A(z), B(z) \text{左互素}; \\
(2) & \mbox{det}(A(z)B(z)) \neq 0, \ |z| \leq 1.
\end{aligned}\]

用推移算子把模型方程写成
\[\begin{align}
A(\mathscr B) \boldsymbol X\_t = B(\mathscr B) \boldsymbol\varepsilon\_t, \ t \in \mathbb Z
\tag{30.7}
\end{align}\]

对于一维ARMA，自协方差函数可以唯一决定模型参数；
对于多维ARMA不能。

在模型[(30.7)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-def-marmamodlagop0108)中，
如果\(\mbox{det}(A(z)) = c\)是常数，
则\(A^{-1}(z)\)仍然是一个矩阵多项式，并且
\(\mbox{det}(A^{-1}(z))=c^{-1}\)。于是
\[\begin{aligned}
\boldsymbol X\_t = A^{-1}(\mathscr B) B(\mathscr B) \varepsilon\_t = B\_1(\mathscr B) \varepsilon\_t,
\ t \in \mathbb Z
\end{aligned}\]
其中\(B\_1(z)=A^{-1}(z)B(z)\)是矩阵系数多项式，满足
\[\begin{aligned}
\mbox{det}(B\_1(z)) = c^{-1} \mbox{det}(B(z)) \neq 0,
\ |z| \leq 1
\end{aligned}\]
就有了两个模型。
所以ARMA模型参数是不唯一的。

○○○○○○

## 30.2 多维平稳序列的均值和自协方差函数的估计

### 30.2.1 均值的估计

设\(\{ \boldsymbol X\_t \}\)是\(m\)维平稳序列,
\(\boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_N\)是观测值。
均值\(\boldsymbol\mu\)的估计为
\[\begin{aligned}
\hat{\boldsymbol\mu}\_N = (\hat\mu\_1, \hat\mu\_2, \dots, \hat\mu\_m)^T
= \frac{1}{N} \sum\_{t=1}^N \boldsymbol X\_t.
\end{aligned}\]

由分量的相应结论可得：

**定理30.2** 如果\(\{ \boldsymbol X\_t \}\)的每个分量序列
\(\{ X\_{jt}: t\in\mathbb Z \}\)都是严平稳遍历序列，则当\(N\to\infty\)时，
\[\begin{aligned}
\hat{\boldsymbol\mu}\_N \to \boldsymbol\mu, \ \text{a.s.}
\end{aligned}\]

**定理30.3** 如果自协方差函数\(\Gamma(n) \to 0\),
当\(n \to\infty\)，则
\[\begin{aligned}
E|\hat{\boldsymbol\mu}\_N - \hat{\boldsymbol\mu}|^2 \to 0,
\ \text{当$N\to\infty$时}
\end{aligned}\]
其中\(|\hat{\boldsymbol\mu}\_N - {\boldsymbol\mu}|^2 = \sum\_{j=1}^m (\hat\mu\_j - \mu\_j)^2\)。

这是均方收敛，推出依概率收敛，
即\(\hat{\boldsymbol\mu}\_N\)是\(\boldsymbol\mu\)的相合估计。

**证明**：
\[\begin{aligned}
E |\hat{\boldsymbol\mu}\_N - \hat{\boldsymbol\mu}|^2 =& \sum\_{j=1}^m E (\hat\mu\_j - \mu\_j)^2 \\
=& \sum\_{j=1}^m E \left(
\frac{1}{N} \sum\_{t=1}^N X\_{jt} - \mu\_j \right)^2 \\
=& \sum\_{j=1}^m \frac{1}{N^2} E \left(
\sum\_{t=1}^N (X\_{jt} - \mu\_j) \right)^2 \\
=& \sum\_{j=1}^m \frac{1}{N^2}
\sum\_{l=1}^N \sum\_{k=1}^N \gamma\_{jj}(l-k) \\
=& \sum\_{j=1}^m \frac{1}{N^2} \sum\_{k=1-N}^{N-1}(N - |k|) \gamma\_{jj}(k) \\
\leq& \sum\_{j=1}^m \frac{1}{N} \sum\_{k=1-N}^{N-1} |\gamma\_{jj}(k)|
\to 0, \ (N\to\infty)
\end{aligned}\]

○○○○○○

**定理30.4** 如果
\[\begin{aligned}
\sum\_{k=0}^\infty | \gamma\_{jj}(k) | < \infty, \ j=1,2,\dots,m
\end{aligned}\]
则当\(N\to\infty\)时
\[\begin{aligned}
N E | \hat{\boldsymbol\mu}\_N - \boldsymbol\mu |^2
\to \sum\_{j=1}^m \sum\_{k=-\infty}^\infty \gamma\_{jj}(k).
\end{aligned}\]

**证明** 按定理[30.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#thm:mtsintro-est-estmu-l2thm)的证明，有
\[\begin{aligned}
& N E | \hat{\boldsymbol\mu}\_N - \boldsymbol\mu |^2 \\
=& \sum\_{j=1}^m \frac{1}{N} \sum\_{k=1-N}^{N-1}(N - |k|) \gamma\_{jj}(k) \\
=& \sum\_{j=1}^m \sum\_{k=1-N}^{N-1} \gamma\_{jj}(k)
- \sum\_{j=1}^m \frac{1}{N} \sum\_{k=1-N}^{N-1} |k| \gamma\_{jj}(k) \\
\to& \sum\_{j=1}^m \sum\_{k=-\infty}^{\infty} \gamma\_{jj}(k).
\end{aligned}\]

○○○○○○

**定理30.5** 设\(\{\boldsymbol\varepsilon\_t \}\)是\(m\)维独立同分布的
WN(\(\boldsymbol 0, Q\))，
\(m \times m\)矩阵\(C\_n = (c\_{j,k}(n))\)的每个元素\(\{c\_{j,k}(n)\}\)对\(n\in\mathbb Z\)绝对可和，
\(m\)维平稳序列\(\{ \boldsymbol X\_t \}\)为线性平稳列
\[\begin{aligned}
\boldsymbol X\_t =& \boldsymbol\mu
+ \sum\_{j=-\infty}^\infty C\_j \boldsymbol\varepsilon\_{t-j}, \ t \in \mathbb Z
\end{aligned}\]
如果
\[\begin{aligned}
\Sigma =& \left( \sum\_{j=-\infty}^\infty C\_j \right) Q
\left( \sum\_{j=-\infty}^\infty C\_j^T \right) \neq 0
\end{aligned}\]
则
\[\begin{aligned}
\sqrt{N}(\hat{\boldsymbol\mu}\_N - \boldsymbol\mu) \stackrel{\mbox{d}}{\longrightarrow}
\text{N}(0,\Sigma)
\end{aligned}\]

见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book))。

### 30.2.2 自协方差函数的估计

设\(\{ \boldsymbol X\_t \}\)是\(m\)维平稳序列,
\(\boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_N\)是观测值。
自协方差函数\(\Gamma(n)\)的估计为
\[\begin{aligned}
\begin{cases}
\hat\Gamma(n) = \frac{1}{N}
\sum\_{t=1}^{N-n} ( \boldsymbol X\_{t+n} - \hat{\boldsymbol\mu}\_N)
( \boldsymbol X\_{t} - \hat{\boldsymbol\mu}\_N)^T,
& 0 \leq n \leq N-1 \\
\hat\Gamma(-n) = \hat\Gamma^T(n),
& 1 \leq n \leq N-1
\end{cases}
\end{aligned}\]
是一维情况的推广，具有良好统计性质。

若\(\hat\gamma\_{jk}(n)\)为\(\hat\Gamma(n)\)的第\((j,k)\)元素，
则相关系数估计为
\[\begin{align}
\hat\rho\_{jk}(n) = \frac{\hat\gamma\_{jk}(n)}
{\sqrt{\hat\gamma\_{jj}(0) \hat\gamma\_{kk}(0)}}
\tag{30.8}
\end{align}\]
自相关系数矩阵的估计为
\[\begin{align}
\hat R(n) = (\hat\rho\_{jk}(n)).
\tag{30.9}
\end{align}\]

利用严平稳序列的遍历定理可得：

**定理30.6** 在定理[30.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#thm:mtsintro-est-mu-clt)的条件下，
对固定的\(n\)，当\(N\to\infty\)时
\[\begin{aligned}
\hat\Gamma(n) \to \Gamma(n),\ \text{a.s.},
\quad
\hat R(n) \to R(n), \ \text{a.s.}
\end{aligned}\]

关于\(\hat R(n)\)的渐近分布有(见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book))定理11.2.2)：

**定理30.7** 设\(\{ Z\_{1t} \}\)和\(\{ Z\_{2t} \}\)都是一维独立同分布的零均值白噪声，
彼此相互独立，
\(\{a\_j \}\)和\(\{ b\_j \}\)是绝对可和的实数列，
\[\begin{aligned}
X\_{1t} =& \sum\_{j=-\infty}^\infty a\_j Z\_{1,t-j}, \ t \in \mathbb N\_+, \\
X\_{2t} =& \sum\_{j=-\infty}^\infty b\_j Z\_{2,t-j}, \ t \in \mathbb N\_+,
\end{aligned}\]
则

* (1) 对\(k \geq 0\), 当\(N \to \infty\)时，
  \[\begin{aligned}
  \sqrt{N}\hat\rho\_{12}(k) \stackrel{\text{d}}{\longrightarrow}
  \text{N}(0,\sigma\_{11})
  \end{aligned}\]
* (2) 对\(h, k \geq 0\)且\(h \neq k\)，
  \[\begin{aligned}
  \sqrt{N}( \hat\rho\_{12}(h), \hat\rho\_{12}(k))
  \stackrel{\text{d}}{\longrightarrow}
  \text{N}(0, \Sigma)
  \end{aligned}\]
  其中
  \[\begin{aligned}
  \Sigma =& \left(\begin{array}{cc}
  \sigma\_{11} & \sigma\_{12} \\
  \sigma\_{12} & \sigma\_{11}
  \end{array}\right) \\
  \sigma\_{11} =& \sum\_{j=-\infty}^\infty \rho\_{11}(j) \rho\_{22}(j) \\
  \sigma\_{12} =& \sum\_{j=-\infty}^\infty \rho\_{11}(j) \rho\_{22}(j+k-h).
  \end{aligned}\]

## 30.3 VAR模型

**定义30.3** 设\(\{ \boldsymbol\varepsilon\_t \}\)是\(m\)维WN(0,\(Q\)),
\(A\_1, A\_2, \dots, A\_p\)是\(m \times m\)实矩阵，使得
\[\begin{align}
\det\left( I\_m - \sum\_{j=1}^p A\_j z^j \right) \neq 0,
\ |z| \leq 1
\tag{30.10}
\end{align}\]
称如下的模型
\[\begin{align}
\boldsymbol X\_t = \sum\_{j=1}^p A\_j \boldsymbol X\_{t-j} + \boldsymbol\varepsilon\_t,
\ t \in \mathbb Z
\tag{30.11}
\end{align}\]
是一个\(m\)维AR(\(p\))模型；
如果平稳序列\(\{ \boldsymbol X\_t \}\)满足\(m\)维AR(\(p\))模型，
就称\(\{ \boldsymbol X\_t \}\)是一个\(m\)维的AR(\(p\))序列。

记
\[\begin{aligned}
A(z) = I\_m - \sum\_{j=1}^p A\_j z^j
\end{aligned}\]
模型为
\[\begin{align}
A(\mathscr B) \boldsymbol X\_t = \boldsymbol\varepsilon\_t, \ t \in \mathbb Z
\tag{30.12}
\end{align}\]
用\(A(z)\)的伴随矩阵表示\(A^{-1}(z)\)后，
可知\(A^{-1}(z)\)的每个元素在\(|z| \leq 1\)有泰勒展式，
于是
\[\begin{align}
A^{-1}(z) = \sum\_{j=0}^\infty C\_j z^j, \ |z| \leq 1.
\tag{30.13}
\end{align}\]
其中\(C\_n=(c\_{jk}(n))\), \(\{c\_{jk}(n), n \in \mathbb N\_+ \}\)以负指数速度趋于零。
类似一维情况可得
\[\begin{align}
\boldsymbol X\_t =& A^{-1}(\mathscr B) \boldsymbol\varepsilon\_t
= \sum\_{j=0}^\infty C\_j \boldsymbol\varepsilon\_{t-j},
\ t \in \mathbb Z
\tag{30.14}
\end{align}\]
是模型[(30.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-mod0302)的唯一平稳解。

**例30.5 (一维AR的马氏化)** 将一维AR(\(p\))序列表示成VAR。

一维AR(\(p\))
\[\begin{align}
X\_t =& \sum\_{j=1}^p a\_j X\_{t-j} + \varepsilon\_t,
\ t \in \mathbb Z,
\tag{30.15} \\
\{ \varepsilon\_t \} & \text{为WN}(0,\sigma^2)
\end{align}\]
令
\[\begin{aligned}
\boldsymbol X\_t =& (X\_t, X\_{t-1}, \dots, X\_{t-p+1})^T \\
\boldsymbol \varepsilon\_t =& (\varepsilon\_t, 0, \dots, 0)^T \\
A =& \left(\begin{array}{ccccc}
a\_1 & a\_2 & \cdots & a\_{p-1} & a\_p \\
1 & 0 & \cdots & 0 & 0 \\
0 & 1 & \cdots & 0 & 0 \\
\vdots & \vdots & & \vdots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{array}\right)
\end{aligned}\]
则得到\(p\)维AR(1)模型：
\[\begin{align}
\boldsymbol X\_t = A \boldsymbol X\_{t-1} + \boldsymbol\varepsilon\_t,
\ t \in \mathbb Z.
\tag{30.16} \\
\end{align}\]
其中
\[\begin{aligned}
& \det(I\_p - Az) \\
=& \det\left(\begin{array}{ccccc}
1-a\_1z & -a\_2z & \cdots & -a\_{p-1}z & -a\_pz \\
-z & 1 & \cdots & 0 & 0 \\
0 & -z & \cdots & 0 & 0 \\
\vdots & \vdots & & \vdots & \vdots \\
0 & 0 & \cdots & -z & 1
\end{array}\right) \\
=& 1 - a\_1 z - a\_2 z^2 - \dots - a\_p z^p \neq 0,
\ |z| \leq 1.
\end{aligned}\]
计算上面行列式时，按\(j=2, 3, \dots, p\)的顺序把第\(j\)列乘以\(z^{j-1}\)后加到第一列，
则第一列只有\((1,1)\)元素非零。

○○○○○○

### 30.3.1 多维AR序列的自协方差函数

由平稳解[(30.14)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-stasolu0305)可知
\[\begin{aligned}
E[ \boldsymbol X\_t \boldsymbol\varepsilon\_{t+n}^T] = 0, n \geq 1.
\end{aligned}\]
这体现出模型有因果性。
于是对\(n\geq 0\)
\[\begin{align}
\Gamma(n) =& E(\boldsymbol X\_{t+n} \boldsymbol X\_t^T) \\
=& E \left[ \left( \sum\_{j=1}^p A\_j \boldsymbol X\_{t+n-j} + \boldsymbol\varepsilon\_{t+n} \right)
\boldsymbol X\_t^T \right] \\
=& \sum\_{j=1}^p A\_j \Gamma(n-j) + E(\boldsymbol\varepsilon\_{t+n} \boldsymbol X\_t^T)
\tag{30.17}
\end{align}\]
从而
\[\begin{align}
A(\mathscr B) \Gamma(n) = 0, \ n \geq 1.
\tag{30.18}
\end{align}\]

另外
\[\begin{align}
E(\boldsymbol\varepsilon\_t \boldsymbol X\_t^T)
= E \left( \sum\_{j=0}^\infty \boldsymbol\varepsilon\_t \boldsymbol\varepsilon\_{t-j}^T C\_j^T
\right)
= Q C\_0^T = Q
\tag{30.19}
\end{align}\]

总之有\(m\)维情况下自协方差函数矩阵的Yule-Walker方程
\[\begin{align}
\begin{cases}
\Gamma(0) = \sum\_{j=1}^p A\_j \Gamma(-j) + Q, &\\
\Gamma(n) = \sum\_{j=1}^p A\_j \Gamma(n-j), & n \geq 1 \\
\end{cases}
\tag{30.20}
\end{align}\]

把[(30.20)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-yweqall0311)的第二式写成分块矩阵形式：
\[\begin{align}
\left(\begin{array}{c}
\Gamma^T(1) \\ \Gamma^T(2) \\ \vdots \\ \Gamma^T(n)
\end{array}\right)
=
\left(\begin{array}{cccc}
\Gamma(0) & \Gamma(1) & \cdots & \Gamma(p-1) \\
\Gamma(-1) & \Gamma(0) & \cdots & \Gamma(p-2) \\
\vdots & \vdots & & \vdots \\
\Gamma(-p+1) & \Gamma(-p+2) & \cdots & \Gamma(0)
\end{array}\right)
\left(\begin{array}{c}
A\_1^T \\ A\_2^T \\ \vdots \\ A\_p^T
\end{array}\right)
\tag{30.21}
\end{align}\]
系数矩阵是向量
\[\begin{aligned}
(\boldsymbol X\_p^T, \boldsymbol X\_{p-1}^T, \dots, \boldsymbol X\_1^T)^T
\end{aligned}\]
的协方差矩阵，如果它正定则\(A\_1, A\_2, \dots, A\_p\)和\(Q\)可以由
\[\begin{aligned}
\Gamma(0), \Gamma(1), \dots, \Gamma(p)
\end{aligned}\]
唯一决定，且满足最小相位条件[(30.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-minpha0301)。

### 30.3.2 多维AR序列的参数估计

#### 30.3.2.1 Y-W方法

在Y-W方程[(30.20)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-yweqall0311)中，
把\(\Gamma(n)\)用样本自协方差函数\(\hat\Gamma(n)\)代替，
得到样本Y-W方程，可解得模型的Y-W估计
\[\begin{aligned}
(\hat A\_1, \hat A\_2, \dots, \hat A\_p, \hat Q).
\end{aligned}\]
解法有类似一维时Levinson递推那样的递推公式，
见([谢衷洁 1990](#ref-Xie1990:tsabook))。
只要Y-W方程中的系数矩阵正定，
则Y-W估计满足最小相位条件[(30.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-minpha0301)。

#### 30.3.2.2 最小二乘

把观测值\(\boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_N\)满足的模型写成
\[\begin{align}
\boldsymbol X\_t^T = \sum\_{j=1}^p \boldsymbol X\_{t-j}^T A\_j^T + \boldsymbol\varepsilon\_t^T,
\ t=p+1, p+2, \dots, N
\tag{30.22}
\end{align}\]
引入
\[\begin{aligned}
\boldsymbol X\_n =& \left(\begin{array}{cccc}
\boldsymbol X\_p^T & \boldsymbol X\_{p-1}^T & \cdots & \boldsymbol X\_1^T \\
\boldsymbol X\_{p+1}^T & \boldsymbol X\_p^T & \cdots & \boldsymbol X\_2^T \\
\vdots & \vdots & & \vdots \\
\boldsymbol X\_{n-1}^T & \boldsymbol X\_{n-2}^T & \cdots & \boldsymbol X\_{n-p}
\end{array}\right)\_{(n-p)\times mp} \\
\boldsymbol Y\_n =& \left(\begin{array}{c}
\boldsymbol X\_{p+1}^T \\ \boldsymbol X\_{p+2}^T \\ \vdots \\ \boldsymbol X\_n^T
\end{array}\right)\_{(n-p)\times m}
\ \boldsymbol E\_n = \left(\begin{array}{c}
\boldsymbol \varepsilon\_{p+1}^T \\ \boldsymbol \varepsilon\_{p+1}^T \\ \vdots \\ \boldsymbol \varepsilon\_n^T
\end{array}\right)\_{(n-p)\times m} \\
\boldsymbol A =& \left(\begin{array}{c}
\boldsymbol A\_1^T \\ \boldsymbol A\_2^T \\ \vdots \\ \boldsymbol X\_p^T
\end{array}\right)\_{mp\times m}
\end{aligned}\]
可以把[(30.22)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-lsmodeq0313)写成
\[\begin{aligned}
\boldsymbol Y\_n = \boldsymbol X\_n \boldsymbol A + \boldsymbol E\_n
\end{aligned}\]
求\(A\_1, A\_2, \dots, A\_p\)的最小二乘估计\(\hat{\boldsymbol A}\)，
就是求\(\hat{\boldsymbol A}\)使得
\[\begin{aligned}
S(\boldsymbol A)
= (\boldsymbol Y\_n - \boldsymbol X\_n \boldsymbol A)^T
(\boldsymbol Y\_n - \boldsymbol X\_n \boldsymbol A)
\end{aligned}\]
最小。
这里\(S(\boldsymbol A)\)是\(m \times m\)矩阵。对两个对称矩阵\(A\)和\(B\)，
当且仅当\(A-B\)非负定且不等于零矩阵时称\(A>B\)。
\(\hat{\boldsymbol A}\)满足方程
\[\begin{aligned}
(\boldsymbol X\_n^T \boldsymbol X\_n) \hat{\boldsymbol A} = \boldsymbol X\_n^T \boldsymbol Y\_n
\end{aligned}\]
当\((\boldsymbol X\_n^T \boldsymbol X\_n)\)可逆时
\[\begin{aligned}
\hat{\boldsymbol A} = (\boldsymbol X\_n^T \boldsymbol X\_n)^{-1} \boldsymbol X\_n^T \boldsymbol Y\_n
\end{aligned}\]

事实上，若\(\hat{\boldsymbol A}\)满足下式
\[\begin{aligned}
(\boldsymbol X\_n^T \boldsymbol X\_n) \hat{\boldsymbol A} = \boldsymbol X\_n^T \boldsymbol Y\_n
\end{aligned}\]
则对任何与\(\hat{\boldsymbol A}\)同阶的\(\boldsymbol B\)，有
\[\begin{aligned}
& (\boldsymbol Y\_n - \boldsymbol X\_n \boldsymbol B)^T(\boldsymbol Y\_n - \boldsymbol X\_n \boldsymbol B) \\
=& (\boldsymbol Y\_n - \boldsymbol X\_n \hat{\boldsymbol A} + \boldsymbol X\_n \hat{\boldsymbol A} - \boldsymbol X\_n \boldsymbol B )^T \\
& \cdot (\boldsymbol Y\_n - \boldsymbol X\_n \hat{\boldsymbol A} + \boldsymbol X\_n \hat{\boldsymbol A} - \boldsymbol X\_n \boldsymbol B ) \\
=& (\boldsymbol Y\_n - \boldsymbol X\_n \hat{\boldsymbol A})^T(\boldsymbol Y\_n - \boldsymbol X\_n \hat{\boldsymbol A})
+ (\boldsymbol X\_n (\hat{\boldsymbol A} - \boldsymbol B))^T (\boldsymbol X\_n (\hat{\boldsymbol A} - \boldsymbol B)) \\
\geq& (\boldsymbol Y\_n - \boldsymbol X\_n \hat{\boldsymbol A})^T(\boldsymbol Y\_n - \boldsymbol X\_n \hat{\boldsymbol A})
\end{aligned}\]

### 30.3.3 VAR的预测

例[30.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#exm:mtsintro-var-markov)证明了一维AR(\(p\))模型可以化为\(p\)维AR(1)模型。
进一步地，任何一个\(m\)维AR(\(p\))模型可以写成一个\(mp\)维的AR(1)模型。
考虑\(m\)维AR(\(p\))模型
\[\begin{align}
\boldsymbol X\_t = \sum\_{j=1}^p A\_j \boldsymbol X\_{t-j} + \boldsymbol\varepsilon\_t,
\ t \in \mathbb Z
\tag{30.23}
\end{align}\]
记
\[\begin{aligned}
\boldsymbol Y\_t =& \left(\begin{array}{c}
\boldsymbol X\_t \\ \boldsymbol X\_{t-1} \\ \vdots \\ \boldsymbol X\_{t-p+1}
\end{array}\right)\_{mp \times 1}, \quad
\boldsymbol \eta\_t = \left(\begin{array}{c}
\boldsymbol \varepsilon\_t \\ \boldsymbol 0 \\ \vdots \\ \boldsymbol 0
\end{array}\right)\_{mp \times 1} \\
\boldsymbol A =& \left(\begin{array}{ccccc}
A\_1 & A\_2 & \cdots & A\_{p-1} & A\_p \\
I\_m & \boldsymbol 0 & \cdots & \boldsymbol 0 & \boldsymbol 0 \\
\boldsymbol 0 & I\_m & \cdots & \boldsymbol 0 & \boldsymbol 0 \\
\vdots & \vdots & & \vdots & \vdots \\
\boldsymbol 0 & \boldsymbol 0 & \cdots & I\_m & \boldsymbol 0
\end{array}\right)\_{mp \times mp}
\end{aligned}\]
则\(m\)维AR(\(p\))模型[(30.23)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-var-varmp0314)可以写成\(mp\)维AR(1):
\[\begin{aligned}
\boldsymbol Y\_t = \boldsymbol A \boldsymbol Y\_{t-1} + \boldsymbol \eta\_t, \ t \in \mathbb Z
\end{aligned}\]
其中，系数矩阵\(\boldsymbol A\)满足
\[\begin{align}
& \det(I\_p - \boldsymbol A z) \\
=& \det\left(\begin{array}{ccccc}
I\_p - A\_1z & -A\_2z & \cdots & -A\_{p-1}z & -A\_pz \\
-I\_p z & I\_p & \cdots & 0 & 0 \\
0 & -I\_p z & \cdots & 0 & 0 \\
\vdots & \vdots & & \vdots & \vdots \\
0 & 0 & \cdots & -I\_p z & I\_p
\end{array}\right) \\
=& \det(I\_p - A\_1 z - A\_2 z^2 - \dots - A\_p z^p) \neq 0,
\ |z| \leq 1.
\tag{30.24}
\end{align}\]
且\(\{ \boldsymbol \eta\_t \}\)为\(mp\)维零均值白噪声。

于是，只需要研究多维AR(1)的预报问题。
对\(m\)维随机向量\(\boldsymbol Y=(Y\_1, Y\_2, \dots, Y\_m)^T\)定义
\[\begin{aligned}
L(\boldsymbol Y | \boldsymbol Z)
= [ L(Y\_1 | \boldsymbol Z), L(Y\_2 | \boldsymbol Z), \dots, L(Y\_m | \boldsymbol Z) ]
\end{aligned}\]
其中\(L(Y\_k | \boldsymbol Z)\)是\(\boldsymbol Z\)对\(Y\_k\)的最佳线性预测。
设\(\{ \boldsymbol X\_t \}\)是\(m\)维AR(1)序列，满足
\[\begin{aligned}
\boldsymbol X\_t = A \boldsymbol X\_{t-1} + \boldsymbol \varepsilon\_t, \ t \in \mathbb Z.
\end{aligned}\]
考虑用\(\boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n\)对\(\boldsymbol X\_{n+k}\)进行最佳线性预测。

利用平稳解的因果性，
即\(E(\boldsymbol X\_t \boldsymbol \varepsilon\_{n+k})=0\)(\(k \geq 1\))可得
\[\begin{aligned}
& L(\boldsymbol X\_{n+1} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& L(A \boldsymbol X\_n + \boldsymbol\varepsilon\_{n+1} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& L(A \boldsymbol X\_n | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& A \, L(\boldsymbol X\_n | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& A \boldsymbol X\_n
\end{aligned}\]
对\(k \geq 1\)有
\[\begin{aligned}
& L(\boldsymbol X\_{n+k} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& L(A \boldsymbol X\_{n+k-1} + \boldsymbol\varepsilon\_{n+k} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& L(A \boldsymbol X\_{n+k-1} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& A \, L(\boldsymbol X\_{n+k-1} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& \cdots = A^k \boldsymbol X\_n
\end{aligned}\]

总之有
\[\begin{aligned}
& L(\boldsymbol X\_{n+k} | \boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_n) \\
=& L(\boldsymbol X\_{n+k} | \boldsymbol X\_n) = A^k \boldsymbol X\_n,
\ n,k \in \mathbb N\_+
\end{aligned}\]

## 30.4 多维平稳序列的谱分析

### 30.4.1 多维平稳序列的谱函数

设\(\{ \boldsymbol X\_t = (X\_{1t}, X\_{2t})^T: \ t \in \mathbb Z \}\)是一个2维零均值平稳序列。
对给定复数\(z\)定义
\[\begin{aligned}
Y\_t = X\_{1t} + z X\_{2t}, \ t \in \mathbb Z
\end{aligned}\]
易见\(E Y\_t = 0\),
\[\begin{aligned}
\gamma\_z(k) \stackrel{\triangle}{=}& E(Y\_{t+k} \bar Y\_t)
= E[ (X\_{1,t+k} + z X\_{2,t+k}) (X\_{1t} + \bar z X\_{2t}) ] \\
=& \gamma\_{11}(k) + z \gamma\_{21}(k) + \bar z \gamma\_{12}(k) + |z|^2 \gamma\_{22}(k)
\end{aligned}\]
都不依赖于\(t\)，所以\(\{ Y\_t \}\)是一个复值平稳序列。
设\(\{ Y\_t \}\)有谱函数\(F\_z\), 则对\(n \in \mathbb Z\),
\[\begin{aligned}
\gamma\_z(n)
= \int\_{-\pi}^\pi e^{in\lambda} d F\_z(\lambda),
\ F\_z(-\pi) = 0.
\end{aligned}\]

当\(z=\pm 1, \pm i\)时得
\[\begin{aligned}
\gamma\_1(n) =& \gamma\_{11}(n) + \gamma\_{21}(n)
+ \gamma\_{12}(n) + \gamma\_{22}(n) \\
\gamma\_{-1}(n) =& \gamma\_{11}(n) - \gamma\_{21}(n)
- \gamma\_{12}(n) + \gamma\_{22}(n) \\
\gamma\_{i}(n) =& \gamma\_{11}(n) + i \gamma\_{21}(n)
- i \gamma\_{12}(n) + \gamma\_{22}(n) \\
\gamma\_{-i}(n) =& \gamma\_{11}(n) - i \gamma\_{21}(n)
+ i \gamma\_{12}(n) + \gamma\_{22}(n)
\end{aligned}\]
可以解出
\[\begin{aligned}
\gamma\_{12}(n) =& \frac14[ \gamma\_1(n) - \gamma\_{-1}(n)
+ i \gamma\_i(n) - i \gamma\_{-i}(n)] \\
\gamma\_{21}(n) =& \frac14[ \gamma\_1(n) - \gamma\_{-1}(n)
- i \gamma\_i(n) + i \gamma\_{-i}(n)]
\end{aligned}\]

用\(F\_{11}(\lambda)\)和\(F\_{22}(\lambda)\)分别表示\(\{ X\_{1t} \}\)和
\(\{ X\_{2t} \}\)的谱函数，并引入
\[\begin{aligned}
F\_{12}(\lambda) =& \frac14[ F\_1(\lambda) - F\_{-1}(\lambda)
+ i F\_i(\lambda) - i F\_{-i}(\lambda)] \\
F\_{21}(\lambda) =& \frac14[ F\_1(\lambda) - F\_{-1}(\lambda)
- i F\_i(\lambda) + i F\_{-i}(\lambda)]
\end{aligned}\]
引入矩阵函数
\[\begin{align}
\boldsymbol F(\lambda) =& \left(\begin{array}{cc}
F\_{11}(\lambda) & F\_{12}(\lambda) \\
F\_{21}(\lambda) & F\_{22}(\lambda)
\end{array}\right),
\ \lambda \in [-\pi, \pi]
\tag{30.25}
\end{align}\]
则有
\[\begin{align}
\Gamma(n) =& E(\boldsymbol X\_{t+n} \boldsymbol X\_t^T)
= \int\_{-\pi}^\pi e^{in\lambda} d \boldsymbol F(\lambda) \\
=& \left(\begin{array}{cc}
\int\_{-\pi}^\pi e^{in\lambda} d F\_{11}(\lambda) &
\int\_{-\pi}^\pi e^{in\lambda} d F\_{12}(\lambda) \\
\int\_{-\pi}^\pi e^{in\lambda} d F\_{21}(\lambda) &
\int\_{-\pi}^\pi e^{in\lambda} d F\_{22}(\lambda)
\end{array}\right),
\ n \in \mathbb Z
\tag{30.26}
\end{align}\]

称\(\boldsymbol F(\lambda)\)为2维平稳序列\(\{ \boldsymbol X\_t \}\)的**谱函数矩阵**。
由于矩阵\(\boldsymbol F(\lambda)\)的每个元素都是\([-\pi, \pi]\)上分布函数的线性组合，
所以都是有界变差右连续函数。
另外\(\boldsymbol F(\lambda)\)还是Hermite矩阵：
\[\begin{aligned}
\boldsymbol F^\*(\lambda) = \boldsymbol F(\lambda)
\end{aligned}\]
其中星号表示共轭转置。

还可证明\(\boldsymbol F(\lambda)\)关于\(\lambda\)单调不减，
即\(\forall \lambda\_1 < \lambda\_2, \lambda\_1, \lambda\_2 \in [-\pi, \pi]\),
\(\boldsymbol F(\lambda\_2) - \boldsymbol F(\lambda\_1)\)非负定。

当\(\boldsymbol F(\lambda)\)的每个元素的实部和虚部都是连续函数，
并且除去有限个点外导函数连续时，称
\[\begin{align}
\boldsymbol f(\lambda) =& \left(\begin{array}{cc}
f\_{11}(\lambda) & f\_{12}(\lambda) \\
f\_{21}(\lambda) & f\_{22}(\lambda)
\end{array}\right) \\
=& \left(\begin{array}{cc}
F\_{11}'(\lambda) & F\_{12}'(\lambda) \\
F\_{21}'(\lambda) & F\_{22}'(\lambda)
\end{array}\right),
\ \lambda \in [-\pi, \pi]
\tag{30.27}
\end{align}\]
为\(\{ \boldsymbol X\_t \}\)的**谱密度矩阵**,
称\(f\_{12}(\lambda)\)是\(\{ X\_{1t} \}\)和\(\{ X\_{2t} \}\)的**互谱密度**。

因为\(\boldsymbol F(\lambda)\)是Hermite矩阵，并且单调不减，
所以\(\boldsymbol f(\lambda)\)是Hermite非负定的。
这时
\[\begin{align}
\Gamma(n) =& \int\_{-\pi}^\pi e^{in\lambda} \boldsymbol f(\lambda) d\lambda \\
\stackrel{\triangle}{=}& \left(\begin{array}{cc}
\int\_{-\pi}^\pi e^{in\lambda} f\_{11}(\lambda) d\lambda &
\int\_{-\pi}^\pi e^{in\lambda} f\_{12}(\lambda) d\lambda \\
\int\_{-\pi}^\pi e^{in\lambda} f\_{21}(\lambda) d\lambda &
\int\_{-\pi}^\pi e^{in\lambda} f\_{22}(\lambda) d\lambda
\end{array}\right),
\ n \in \mathbb Z
\tag{30.28}
\end{align}\]

**定理30.8** 设\(m\)维平稳序列\(\{ \boldsymbol X\_t \}\)有自协方差函数\(\{\Gamma(n) \}\)，
则存在唯一的\(m \times m\)函数矩阵
\[\begin{aligned}
\boldsymbol F(\lambda) = (F\_{jk}(\lambda)), \ \lambda \in [-\pi, \pi],
\end{aligned}\]
使得

* (1) \(\Gamma(n) = \int\_{-\pi}^\pi e^{in\lambda} d \boldsymbol F(\lambda)\),
  \(n \in \mathbb Z\);
* (2) \(\boldsymbol F^\*(\lambda) = \boldsymbol F(\lambda)\),
  \(\boldsymbol F(-\pi) = \boldsymbol 0\),
  当\(\lambda\_1 < \lambda\_2\)时\(\boldsymbol F(\lambda\_2) - \boldsymbol F(\lambda\_1)\)非负定；
* (3) \(\boldsymbol F(\lambda)\)中的每个元素\(F\_{jk}(\lambda)\)是有界变差和右连续的。

\(\boldsymbol F(\lambda)\)叫做\(\{ \boldsymbol X\_t \}\)的**谱函数矩阵**。

如果\(\boldsymbol F(\lambda)\)的每个元素\(F\_{jk}(\lambda)\)的实部和虚部都是连续函数，
且除去有限个点外导函数连续，就称
\[\begin{aligned}
\boldsymbol f(\lambda) = (f\_{jk}(\lambda)) \stackrel{\triangle}{=} (F\_{jk}'(\lambda))
\end{aligned}\]
为\(\{ \boldsymbol X\_t \}\)的**谱密度矩阵**。
这时
\[\begin{align}
\Gamma(n) =& \int\_{-\pi}^\pi e^{in\lambda} \boldsymbol f(\lambda) d \lambda \\
=& \left( \int\_{-\pi}^\pi e^{in\lambda} \boldsymbol f\_{jk}(\lambda) d \lambda \right)\_{m \times m},
\ n \in \mathbb Z
\tag{30.29}
\end{align}\]

称实变复值函数是绝对连续函数，
如果其实部和虚部都是绝对连续函数。
当\(\boldsymbol F(\lambda)\)的每个元素都是绝对连续函数时，
\(\boldsymbol f(\lambda) = (F\_{jk}'(\lambda))\)就是\(\{ \boldsymbol X\_t \}\)的**谱密度矩阵**。

**定理30.9** 设\(m\)维平稳序列\(\{ \boldsymbol X\_t \}\)有自协方差函数
\(\Gamma(n) = (\gamma\_{jk}(n))\),
如果
\[\begin{aligned}
\sum\_{n=-\infty}^\infty | \gamma\_{jk}(n) | < \infty,
\ j,k=1,2,\dots, m
\end{aligned}\]
则
\[\begin{aligned}
\boldsymbol f(\lambda) =& \frac{1}{2\pi}
\sum\_{n=-\infty}^\infty \Gamma(n) e^{-in\lambda},
\ \lambda \in [-\pi, \pi]
\end{aligned}\]
是\(\{ \boldsymbol X\_t \}\)的**谱密度矩阵**。

### 30.4.2 多维平稳序列的谱表示

设\(\{ \boldsymbol X\_t = (X\_{1t}, X\_{2t}, \dots, X\_{mt})^T\)
是\(m\)维零均值平稳序列，
则其每个分量\(\{ X\_j(t) \}\)是一位零均值平稳序列，有谱表示
\[\begin{align}
X\_{jt} =& \int\_{-\pi}^\pi e^{it\lambda} d Z\_j(\lambda),
\ t \in \mathbb Z
\tag{30.30}
\end{align}\]
中\(\{ Z\_j(\lambda) \}\)是\([-\pi, \pi]\)上右连续的正交增量过程。

记
\[\begin{align}
\boldsymbol Z(\lambda) =& (Z\_1(\lambda), Z\_2(\lambda), \dots, Z\_m(\lambda))^T,
\ \lambda \in [-\pi, \pi],
\tag{30.31}
\end{align}\]
有
\[\begin{align}
\boldsymbol X\_t =& \int\_{-\pi}^\pi e^{in\lambda} d \boldsymbol Z(\lambda)
\ \lambda \in [-\pi, \pi].
\tag{30.32}
\end{align}\]
这称为\(m\)维平稳序列\(\{ \boldsymbol X\_t \}\)的**谱表示**。

[(30.32)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-spec-ranintmd0408)中的\(\boldsymbol Z(\lambda)\)有下列性质：

* (1) 正交增量性：
  对\(-\pi \leq \lambda\_1 < \lambda\_2 \leq \lambda\_3 < \lambda\_4 \leq \pi\),
  \[\begin{aligned}
  E\left[ (\boldsymbol Z(\lambda\_2) - \boldsymbol Z(\lambda\_1))
  (\boldsymbol Z(\lambda\_4) - \boldsymbol Z(\lambda\_3))^\* \right] = 0.
  \end{aligned}\]
* (2) 右连续性：当\(\delta \downarrow 0\)时
  \[\begin{aligned}
  E\left[ (\boldsymbol Z(\lambda + \delta) - \boldsymbol Z(\lambda))
  (\boldsymbol Z(\lambda + \delta) - \boldsymbol Z(\lambda))^\* \right] \to 0.
  \end{aligned}\]
* (3) \(\boldsymbol F(\lambda) = E[ \boldsymbol Z(\lambda) \boldsymbol Z^\*(\lambda)]\)
  是\(\{ \boldsymbol X\_t \}\)的谱函数矩阵，满足
  \[\begin{aligned}
  \boldsymbol F(\lambda\_2) - \boldsymbol F(\lambda\_1)
  =& E\left[ (\boldsymbol Z(\lambda\_2) - \boldsymbol Z(\lambda\_1))
  (\boldsymbol Z(\lambda\_2) - \boldsymbol Z(\lambda\_1))^\* \right],
  \ \lambda\_1 < \lambda\_2
  \end{aligned}\]

满足上述(1), (2), (3)的\(m\)维随机过程\(\{ \boldsymbol Z(\lambda) \}\)叫做**右连续的正交增量过程**。

**定理30.10** 对\(m\)维零均值平稳序列\(\{ \boldsymbol X\_t \}\)，
有右连续的正交增量过程 \(\{ \boldsymbol Z(\lambda) \}\)使得
\(\boldsymbol Z(-\pi) = \boldsymbol 0\)和[(30.32)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#eq:mtsintro-spec-ranintmd0408)成立。
如果正交增量过程\(\{ \boldsymbol\xi(\lambda) \}\)也满足上述的条件，
则
\[\begin{aligned}
P(\boldsymbol\xi(\lambda) = \boldsymbol Z(\lambda)) = 1, \ \lambda \in [-\pi, \pi].
\end{aligned}\]

**例30.6 (多维线性平稳序列的谱密度)** 设\(\{ \boldsymbol \varepsilon\_t \}\)是\(m\)维WN(0, \(Q\)),
\(Q = E[\boldsymbol\varepsilon\_t \boldsymbol\varepsilon\_t^T]\),
\(\{ A\_j \}\)是一列\(m \times m\)实值矩阵，满足
\[\begin{aligned}
\sum\_{j=-\infty}^\infty A\_j Q A\_j^T < \infty,
\end{aligned}\]
则
\[\begin{aligned}
\boldsymbol X\_t = \sum\_{j=-\infty}^\infty A\_j \boldsymbol\varepsilon\_{t-j},
\ t \in \mathbb Z
\end{aligned}\]
是\(m\)维零均值平稳序列。

自协方差函数为
\[\begin{aligned}
\Gamma(n) = E[ \boldsymbol X\_{t+n} \boldsymbol X\_n ]
= \sum\_{j=-\infty}^\infty A\_{j+n} Q A\_j^T,
\ n \in \mathbb Z
\end{aligned}\]

本例中，对两个实矩阵\(A, B\)，
用\(A \leq B\)表示\(A\)和\(B\)的对应元素比较全部成立小于等于关系，
用\([A]\)表示把\(A\)的所有元素都取绝对值组成的矩阵。这时必有
\[
[AB] \leq [A] [B]
\]
如果要求\(\sum\_{j=-\infty}^\infty [A\_j] < \infty\),
则
\[\begin{aligned}
\sum\_{n=-\infty}^\infty [\Gamma(n)]
=& \sum\_{n=-\infty}^\infty \left[\sum\_{j=-\infty}^\infty A\_{j+n} Q A\_j^T \right] \\
\leq & \sum\_{n=-\infty}^\infty \sum\_{j=-\infty}^\infty [A\_{j+n}] [Q] [A\_j^T] \\
\leq& \left(\sum\_{n=-\infty}^\infty [A\_n] \right)
[Q] \left(\sum\_{j=-\infty}^\infty [A\_j^T] \right)
< \infty
\end{aligned}\]
从而\(\{ \boldsymbol X\_t \}\)有谱密度
\[\begin{aligned}
\boldsymbol f(\lambda)
=& \frac{1}{2\pi} \sum\_{n=-\infty}^\infty \Gamma(n) e^{-in\lambda} \\
=& \frac{1}{2\pi} \sum\_{n=-\infty}^\infty \sum\_{j=-\infty}^\infty
A\_{j+n} Q A\_j^T e^{-in\lambda} \\
=& \frac{1}{2\pi} \left( \sum\_{k=-\infty}^\infty A\_k e^{-ik\lambda} \right)
Q \left( \sum\_{j=-\infty}^\infty A\_j e^{-ij\lambda} \right)^\*
\end{aligned}\]

○○○○○○

**例30.7 (VARMA序列谱密度)** 考虑平稳可逆的\(m\)维ARMA(\(p,q\))模型的谱密度。模型为
\[\begin{aligned}
A(\mathscr B) \boldsymbol X\_t = B(\mathscr B) \boldsymbol\varepsilon\_t, \ t \in \mathbb Z,
\end{aligned}\]

\(\det(A(z)) A^{-1}(z)\)是\(A(z)\)的伴随矩阵，
仍是矩阵系数多项式。
于是\(A^{-1}(z) B(z)\)的每个元素是有理多项式。
将\(A^{-1}(z) B(z)\)的每个元素进行泰勒展开后得\(A^{-1}(z) B(z)\)泰勒展开式
\[\begin{aligned}
A^{-1}(z) B(z) = \sum\_{j=0}^\infty C\_j z^j,
\ |z| \leq 1.
\end{aligned}\]
其中的系数矩阵满足
\[\begin{aligned}
\sum\_{j=0}^\infty [C\_j] < \infty.
\end{aligned}\]

由例[30.6](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mtsintro.html#exm:mtsintro-spec-linserint)，平稳解
\[\begin{aligned}
\boldsymbol X\_t = A^{-1}(\mathscr B) B(\mathscr B) \boldsymbol\varepsilon\_t
= \sum\_{j=0}^\infty C\_j \boldsymbol\varepsilon\_{t-j},
\ t \in \mathbb Z
\end{aligned}\]
有谱密度矩阵
\[\begin{aligned}
\boldsymbol f(\lambda)
=& \frac{1}{2\pi} \left( \sum\_{k=-\infty}^\infty C\_k e^{-ik\lambda} \right)
Q \left( \sum\_{j=-\infty}^\infty C\_j e^{-ij\lambda} \right)^\*
\end{aligned}\]

○○○○○○

**例30.8 (二次相干函数)** 设二维零均值平稳序列\(\{ \boldsymbol X\_t \}\)有谱表示
\[\begin{aligned}
\boldsymbol X\_t = \int\_{-\pi}^\pi e^{it\lambda} d
\left(\begin{array}{c}
Z\_1(\lambda) \\ Z\_2(\lambda)
\end{array}\right)
\end{aligned}\]

取\(\lambda \in [-\pi, \pi]\), 对充分小的正数\(\Delta\lambda\)，
定义
\[\begin{aligned}
\Delta Z\_k(\lambda) = Z\_k(\lambda + \Delta\lambda) - Z\_k(\lambda),
\ k=1,2
\end{aligned}\]
则有
\[\begin{aligned}
& \boldsymbol F(\lambda + \Delta\lambda) - F(\lambda) \\
=& E\left[
\left(\begin{array}{c}
\Delta Z\_1(\lambda) \\ \Delta Z\_2(\lambda)
\end{array}\right)
(\Delta \bar Z\_1(\lambda) , \Delta \bar Z\_2(\lambda))
\right] \\
=& \left(\begin{array}{cc}
E|\Delta Z\_1(\lambda)|^2 &
E[\Delta Z\_1(\lambda) \Delta \bar Z\_2(\lambda)] \\
E[\Delta \bar Z\_1(\lambda) \Delta Z\_2(\lambda)] &
E|\Delta Z\_2(\lambda)|^2
\end{array}\right)
\end{aligned}\]

如果\(\boldsymbol F'(\lambda)\)连续，则有
\[\begin{aligned}
d \boldsymbol F(\lambda) =& \boldsymbol f(\lambda) d\lambda
= \left(\begin{array}{cc}
E|d Z\_1(\lambda)|^2 &
E[d Z\_1(\lambda) d \bar Z\_2(\lambda)] \\
E[d \bar Z\_1(\lambda) d Z\_2(\lambda)] &
E|d Z\_2(\lambda)|^2
\end{array}\right)
\end{aligned}\]
\(d Z\_1(\lambda)\)和\(d Z\_2(\lambda)\)的相关系数为
\[\begin{aligned}
\rho\_{12}(\lambda) =&
\frac{E[d Z\_1(\lambda) d \bar Z\_2(\lambda)]}
{\sqrt{E|d Z\_1(\lambda)|^2 E|d Z\_2(\lambda)|^2}} \\
=& \frac{f\_{12}(\lambda) d\lambda}
{\sqrt{f\_{11}(\lambda) d\lambda \cdot f\_{22}(\lambda) d\lambda}} \\
=& \frac{f\_{12}(\lambda)}{\sqrt{f\_{11}(\lambda) f\_{22}(\lambda)}}
\end{aligned}\]
其中规定\(0/0=0\)。

通常称
\[\begin{aligned}
K\_{12}^2(\lambda) = |\rho\_{12}(\lambda)|^2
= \frac{|f\_{12}(\lambda)|^2}{f\_{11}(\lambda) f\_{22}(\lambda)}
\end{aligned}\]
为\(\{ \boldsymbol X\_t, t \in \mathbb Z \}\)的**二次相干函数**。
\(K\_{12}^2(\lambda)\)描述了\(\{ X\_{1t} \}\)和\(\{ X\_{2t} \}\)
在角频率\(\lambda\)处的线性相关性强弱。
如果\(K\_{12}^2(\lambda)=1\),
说明\(\{ X\_{1t} \}\)和\(\{ X\_{2t} \}\)在角频率\(\lambda\)处线性相关。

○○○○○○

**例30.9 (线性滤波的二次相干函数)** 设平稳序列\(\{ X\_{1t} \}\)有谱密度\(f(\lambda)\),
\(\{ h\_j \}\)是绝对可和线性滤波器，
输出过程为
\[\begin{aligned}
X\_{2t} = \sum\_{j=-\infty}^\infty h\_j X\_{1, t-j}, \ t \in \mathbb Z
\end{aligned}\]

则\(\{X\_{2t}\}\)有谱密度（见定理[6.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-spectrum.html#thm:spec-filt)）
\[\begin{aligned}
f\_{22}(\lambda) =& |h(\lambda)|^2 f(\lambda)
\end{aligned}\]
其中
\[\begin{aligned}
h(\lambda) = \sum\_{j=-\infty}^\infty h\_j e^{-ij\lambda}.
\end{aligned}\]

计算得
\[\begin{aligned}
\gamma\_{12}(n) =& E(X\_{1, t+n} X\_{2t}) \\
=& \sum\_{j=-\infty}^\infty h\_j E(X\_{1,t+n} X\_{1,t-j}) \\
=& \sum\_{j=-\infty}^\infty h\_j \gamma\_{11}(n+j) \\
=& \sum\_{j=-\infty}^\infty h\_j
\int\_{-\pi}^\pi e^{i(n+j)\lambda} f(\lambda) d\lambda \\
=& \int\_{-\pi}^\pi e^{in\lambda} h(-\lambda) f(\lambda) d\lambda
\end{aligned}\]
所以\(\{ X\_{1t} \}\)和\(\{ X\_{2t} \}\)的互谱密度为
\[\begin{aligned}
f\_{12}(\lambda) = h(-\lambda) f(\lambda),
\ \lambda \in [-\pi, \pi].
\end{aligned}\]
于是2维平稳序列\(\boldsymbol X\_t = (X\_{1t}, X\_{2t})^T\)有退化的谱密度矩阵
\[\begin{aligned}
\boldsymbol f(\lambda) =& \left(\begin{array}{cc}
f(\lambda) & h(-\lambda)f(\lambda) \\
h(\lambda)f(\lambda) & |h(\lambda)|^2 f(\lambda)
\end{array}\right)
\end{aligned}\]
这时二次相干函数
\[\begin{aligned}
K^2(\lambda) \equiv 1, \ \forall \lambda \in [-\pi, \pi].
\end{aligned}\]

○○○○○○

### 30.4.3 谱密度矩阵的估计

二维零均值平稳序列\(\{ \boldsymbol X\_t \}\)的观测值\(\boldsymbol X\_1, \boldsymbol X\_2, \dots, \boldsymbol X\_N\)的周期图定义为
\[\begin{aligned}
I\_N(\lambda) =& \frac{1}{2\pi N}
\left( \sum\_{j=1}^N \boldsymbol X\_j e^{-ij\lambda} \right)
\left( \sum\_{j=1}^N \boldsymbol X\_j e^{-ij\lambda} \right)^\*.
\end{aligned}\]
周期图\(I\_n(\lambda)\)的值是\(2 \times 2\)方阵，
可以对周期图平滑得到谱密度矩阵估计。

设\(\lambda\_j = 2j\pi / N\), 对\(\lambda \in [0, \pi]\),
用\(g(N, \lambda)\)表示\(\{ \lambda\_j: 0 \leq j \leq N/2 \}\)中距离\(\lambda\)最近的\(\lambda\_j\)（若左右两个距离相等取左边一个）。
取\(M\_N = O(\sqrt{N})\)且\(\lim\_{N \to\infty} M\_N = \infty\)。
设\(W\_N(k)\)为满足下列条件的实值权函数：
\[\begin{aligned}
(1) & W\_N(k) = W\_N(-k), \ W\_N(k) \geq 0, \ |k| \leq M\_N; \\
(2) & \sum\_{|k| \leq M\_N} W\_N(k) = 1; \\
(3) & \lim\_{N \to\infty} \sum\_{|k| \leq M\_N} W\_N^2(k) = 0.
\end{aligned}\]
谱密度矩阵的平滑周期图估计为
\[\begin{aligned}
\hat{\boldsymbol f}(\lambda) =& \sum\_{|k| \leq M\_N} W\_N(k)
I\_N(g(N,\lambda) + \lambda\_k), \ \lambda \in [0,\pi], \\
\hat{\boldsymbol f}(\lambda) =& [\hat{\boldsymbol f}(-\lambda) ]^T,
\ \lambda \in [-\pi, 0].
\end{aligned}\]

设
\[\begin{aligned}
\hat{\boldsymbol f}(\lambda) =& \left(\begin{array}{cc}
\hat f\_{11} & \hat f\_{12} \\
\hat f\_{21} & \hat f\_{22}
\end{array}\right)
\end{aligned}\]
二次相干函数\(K\_{12}^2(\lambda)\)的估计定义为
\[\begin{aligned}
\hat K\_{12}^2(\lambda) =&
\frac{| \hat f\_{12}(\lambda) |^2}
{\hat f\_{11}(\lambda) \hat f\_{22}(\lambda)}.
\end{aligned}\]

## 30.5 附录：补充

### 30.5.1 多维ARMA不可辨识的例子

只要\(\mbox{det}(A(z))\)为不依赖于\(z\)的非零常数，
则多维ARMA模型参数不能从\(\{ \Gamma(n) \}\)唯一确定。

例如，考虑2维\(VAR(1)\)。
模型为
\[
\boldsymbol X\_t = A \boldsymbol X\_{t-1} + \boldsymbol\varepsilon\_t
\]
其中
\[
A = \left(\begin{array}{cc}
a\_{11} & a\_{12} \\
a\_{21} & a\_{22}
\end{array}\right)
\]
特征多项式的行列式为
\[\begin{aligned}
\mbox{det}(I - A z)
=& \left| \begin{array}{cc}
1 - a\_{11} z & -a\_{12}z \\
-a\_{21}z & 1 - a\_{22}z
\end{array}\right | \\
=& 1 - (a\_{11} + a\_{22}) z + (a\_{11} a\_{22} - a\_{12} a\_{21}) z^2
\end{aligned}\]
令\(z\)和\(z^2\)系数等于零，
则
\[
a\_{22} = -a\_{11},
\ a\_{12} a\_{21} = - a\_{11}^2
\]
取\(a\_{11} \neq 0\),
\(a\_{22} = -a\_{11}\),
取\(a\_{12} \neq 0\)，
取\(a\_{21} = - \frac{a\_{11}^2}{a\_{12}}\)则有\(\mbox{det}(I - A z)=1\)不依赖于\(z\)。
这时
\[\begin{aligned}
(I - A z)^{-1}
=& \left( \begin{array}{cc}
1 - a\_{22} z & a\_{21}z \\
a\_{12}z & 1 - a\_{11}z
\end{array}\right) \\
=& I + \left( \begin{array}{cc}
- a\_{22} & a\_{21} \\
a\_{12} & - a\_{11}
\end{array}\right) z
\end{aligned}\]
令
\[
B = \left( \begin{array}{cc}
- a\_{22} & a\_{21} \\
a\_{12} & - a\_{11}
\end{array}\right)
\]
则模型可以写成
\[
\boldsymbol X\_t
= (I - A \mathscr B)^{-1} \boldsymbol\varepsilon\_t
= (I + B \mathscr B) \boldsymbol\varepsilon\_t
\]
又可以写成一个2维MA(1)模型。

○○○○○○

### References

Brockwell, P. J., and R. A. Davis. 1987. *Time Series: Theory and Methods*. Springer-Verlag.

谢衷洁. 1990. *时间序列分析*. 北京大学出版社.