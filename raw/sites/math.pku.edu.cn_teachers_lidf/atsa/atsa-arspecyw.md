---
crawl_time: '2026-01-17 14:28:49'
framework: sphinx
title: 9 AR(\(p\))序列的谱密度和Yule-Walker方程 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 9 AR(\(p\))序列的谱密度和Yule-Walker方程

## 9.1 AR(\(p\))序列的谱密度

### 9.1.1 AR(\(p\))序列的自协方差

因为AR(\(p\))的平稳解为
\[
X\_t = A^{-1}(\mathscr B) \varepsilon\_t
= \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j}
\]
由线性平稳列性质知\(\{X\_t\}\)为零均值，自协方差函数为
\[\begin{align}
\gamma\_k = E(X\_{t+k} X\_t)
= \sigma^2 \sum\_{j=0}^\infty \psi\_j \psi\_{j+k},
\quad k=0, 1, \dots
\tag{9.1}
\end{align}\]

设\(1<\rho<\min\{|z\_j|\}\)，则\(\psi\_j = o(\rho^{-j})\), 有
\[\begin{align}
|\gamma\_k| \leq & \sigma^2 \sum\_{j=0}^\infty |\psi\_j| \cdot |\psi\_{j+k}|
\leq \sigma^2 (\sum\_{j=0}^\infty |\psi\_j|)
(\sum\_{l=k}^\infty |\psi\_l|) \\
\leq & c\_0 \sum\_{l=k}^\infty \rho^{-l}
\leq c\_1 \rho^{-k}
\tag{9.2}
\end{align}\]
即\(\{\gamma\_k\}\)负指数衰减。

\(\{X\_t\}\)序列前后的相关减小很快，
称为时间序列的**短记忆性**。
征根离单位圆越远\(\{\gamma\_k\}\)衰减越快。

### 9.1.2 AR(\(p\))的谱密度

由线性平稳列的谱密度公式得平稳解的谱密度
\[\begin{aligned}
f(\lambda) = \frac{\sigma^2}{2\pi} \left|
\sum\_{j=0}^\infty \psi\_j e^{ij\lambda} \right|^2
\end{aligned}\]
而\(\sum \psi\_j z^j = 1/A(z)\)所以
\[\begin{align}
f(\lambda) = \frac{\sigma^2}{2\pi}
\frac{1}{\left| A(e^{i\lambda}) \right|^2}
\tag{9.3}
\end{align}\]
\(f(\lambda)\)是一个恒正的偶函数。
如果\(A(z)\)有靠近单位圆的根\(\rho\_j e^{i\lambda\_j}\)则
\(|A(e^{i\lambda\_j})|\)会接近零，
造成谱密度在\(\lambda=\lambda\_j\)处有一个峰值。

### 9.1.3 谱密度的自协方差函数反演公式

谱密度的定义是满足
\[\begin{aligned}
\gamma\_k = \int\_{-\pi}^\pi e^{ik\lambda} f(\lambda) d\lambda
\end{aligned}\]
的非负可积函数。上式是一个Fourier级数系数的公式(差一个常数)。

在\(\{\gamma\_k\}\)满足一定条件下\(f(\lambda)\)必存在，
且可表成\(\{\gamma\_k\}\)的Fourier级数。

**定理9.1** 如果平稳序列\(\{X\_t\}\)的自协方差函数\(\{\gamma\_k\}\)
绝对可和:
\(\sum|\gamma\_k|<\infty\),
则\(\{X\_t\}\)有谱密度
\[\begin{align}
f(\lambda)=\frac{1}{2\pi} \sum\_{k=-\infty}^\infty \gamma\_k e^{-ik\lambda}.
\tag{9.4}
\end{align}\]

由于谱密度是实值函数, 所以[(9.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw0304)还可以写成
\[
f(\lambda)=\frac{1}{2\pi}\sum\_{k=-\infty}^\infty
\gamma\_k \cos(k\lambda)
= \frac{1}{2\pi} \left[
\gamma\_0 + 2\sum\_{k=1}^\infty \gamma\_k \cos(k\lambda)\right].
\]

**证明**:
因为\(\{\gamma\_k\}\)绝对可和所以[(9.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw0304)右边绝对一致收敛,
\(f(\lambda)\)连续。
于是积分与级数可交换：
\[\begin{aligned}
\int\_{-\pi}^\pi f(\lambda) e^{ij\lambda}\ d\lambda
=\frac{1}{2\pi}\sum\_{k=-\infty}^\infty \gamma\_k
\int\_{-\pi}^\pi e^{-i(k-j)\lambda} \ d\lambda
=\gamma\_j.
\end{aligned}\]

还要验证\(f(\lambda)\)非负。
若\(X\_1, \dots, X\_N\)为\(\{X\_t\}\)的观测值，
\[\begin{aligned}
I\_N(\lambda) = \frac{1}{2\pi N} \left|
\sum\_{j=1}^N X\_j e^{ij\lambda} \right|^2,
\quad \lambda \in [-\pi,\pi]
\end{aligned}\]
称为\(X\_1,\ldots, X\_N\)的**周期图**。
令\(f\_N(\lambda) = E I\_N(\lambda)\),
则\(f\_N(\lambda) \geq 0\)，于是
\[\begin{aligned}
0 \leq& f\_N(\lambda)
= \frac{1}{2\pi N}
\sum\_{k=1}^N \sum\_{j=1}^N \gamma\_{k-j} e^{-i(k-j)\lambda} \\
=& \frac{1}{2\pi N} \sum\_{m=1-N}^{N-1} (N-|m|)\gamma\_{m} e^{-im\lambda} \\
=& \frac{1}{2\pi} \sum\_{m=1-N}^{N-1}\gamma\_{m} e^{-im\lambda}
-\frac{1}{2\pi N} \sum\_{m=1-N}^{N-1}|m|\gamma\_{m} e^{-im\lambda}.
\end{aligned}
\tag{\*}
\]
由Kronecker引理知后一项趋于0, 于是
\[\begin{aligned}
f(\lambda) = \lim\_{N\to \infty} f\_N(\lambda) \geq 0.
\end{aligned}\]

○○○○○○

**附注**：(\*)式的二重求和的简化

\[\begin{aligned}
& \frac{1}{2\pi N}
\sum\_{k=1}^N \sum\_{j=1}^N \gamma\_{k-j} e^{-i(k-j)\lambda} \\
=& \frac{1}{2\pi N}
\sum\_{k=1}^N \sum\_{m=k-N}^{k-1} \gamma\_{m} e^{-im\lambda}
\quad (\text{令}m = k-j)
\end{aligned}\]
交换\(m\)与\(k\)的求和次序。因为关于\(m\)的条件为
\[\begin{aligned}
k - N \leq m \leq k - 1
\end{aligned}\]
所以\(k \leq m+N\), \(k \geq m+1\)，
求和变为
\[\begin{aligned}
\frac{1}{2\pi N}
\sum\_{m=1-N}^{N-1} \sum\_{k=\max(m+1,1)}^{k=\min(m+N,N)}
\gamma\_{m} e^{-im\lambda}
\end{aligned}\]
因为\(m \geq 0\)时\(k\)的求和从\(m+1\)到\(N\)有\(N-m\)项，
\(m<0\)时\(k\)的求和从\(1\)到\(m+N=N-|m|\)有\(N-|m|\)项，所以求和变为
\[\begin{aligned}
\frac{1}{2\pi N}
\sum\_{m=1-N}^{N-1} (N - |m|) \gamma\_{m} e^{-im\lambda}
\end{aligned}\]

**附注**：Kronecker引理：
设复数或实数级数\(\sum\_{j=1}^n x\_j\)收敛，
非负实数列\({b\_n}\)单调不减趋于\(+\infty\)，
则
\[
\lim\_{n\to\infty} \frac{1}{b\_n} \sum\_{j=1}^n b\_j x\_j = 0 .
\]

见[B.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#thm:Kroneckerlemma)。

○○○○○○

**推论9.1** AR(\(p\))的平稳解序列\(\{X\_t\}\)有谱密度
\[\begin{aligned}
f(\lambda)=\frac{1}{2\pi}\sum\_{k=-\infty}^\infty \gamma\_k e^{-ik\lambda}
=\frac{\sigma^2}{2\pi}\frac{1}{|A(e^{i\lambda})|^2}.
\end{aligned}\]

## 9.2 Yule-Walker方程

### 9.2.1 白噪声列与平稳解的关系

\(A(\mathscr B) X\_t = \varepsilon\_t\)的平稳解为
\[\begin{aligned}
X\_t = A^{-1}(\mathscr B) \varepsilon\_t
= \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j}
\end{aligned}\]
对\(k \geq 1\)由控制收敛定理或内积的连续性得
\[\begin{aligned}
E(X\_t \varepsilon\_{t+k} )
= \sum\_{j=0}^\infty \psi\_j E(\varepsilon\_{t-j} \varepsilon\_{t+k})
= 0
\end{aligned}\]
即\(X\_t\)与未来的输入不相关。

如果\(\{\varepsilon\_t\}\)是独立白噪声则\(X\_t\)与未来的输入独立。

### 9.2.2 AR序列的等价定义

设\(a\_1, \dots, a\_p\)为实数，
\(\{ \varepsilon\_t \}\)为WN(\(0, \sigma^2\)),
若平稳列\(\{ X\_t, t \in \mathbb Z \}\)满足
\[
X\_t = a\_1 X\_{t-1} + \dots + a\_p x\_{t-p} + \varepsilon\_t,
\ t \in \mathbb Z,
\]
且\(\varepsilon\_{t+k}, k=1,2,\dots\)与\(\{ X\_s: s \leq t \}\)互不相关，
则称\(\{ X\_t \}\)为AR(\(p\))序列。

可以看出，两个定义是等价的。
由前面的推导可知原始定义可以导出等价定义中的性质。
反之，如果等价定义成立，
\(A(z)=1 - a\_1 z - \dots - a\_p z^p\)一定满足最小相位性。

### 9.2.3 Yule-Walker方程

在AR(\(p\))模型
\[
X\_t = a\_1 X\_{t-1} + \dots + a\_p X\_{t-p} + \varepsilon\_{t-p}
\]
两边同时乘以\(X\_{t-k}\)(\(k \geq 1\))后取期望，
有
\[
E(X\_t X\_{t-k}) = a\_1 E(X\_{t-1} X\_{t-k}) + \dots + a\_p E(X\_{t-p} X\_{t-k}) + E(\varepsilon\_t X\_{t-k})
\]
即有
\[
\gamma\_k = a\_1 \gamma\_{k-1} + \dots + a\_p \gamma\_{k-p} + 0
\]

所以，对\(k \geq 1\)，有递推式
\[
\gamma\_k = a\_1 \gamma\_{k-1} + \dots + a\_p \gamma\_{k-p} .
\]

\(k \geq 1\)时，\(\gamma\_k\)满足齐次线性差分方程
\[
A(\mathscr B) \gamma\_k = 0 .
\]

将\(k=1,2,\dots,p\)的方程写成
\[\begin{aligned}
\gamma\_1 =& a\_1 \gamma\_0 + a\_2 \gamma\_1 + \dots + a\_p \gamma\_{p-1} \\
\gamma\_2 =& a\_1 \gamma\_1 + a\_2 \gamma\_0 + \dots + a\_p \gamma\_{p-2} \\
\vdots =& \vdots \\
\gamma\_p =& a\_1 \gamma\_{p-1} + a\_2 \gamma\_{p-2} + \dots + a\_p \gamma\_0
\end{aligned}\]
写成矩阵形式：
\[\begin{aligned}
\left(\begin{matrix}
\gamma\_1 \\
\gamma\_2 \\
\gamma\_3 \\
\vdots \\
\gamma\_p
\end{matrix}\right)
=&
\left(\begin{matrix}
\gamma\_0 & \gamma\_1 & \cdots & \gamma\_{p-1} \\
\gamma\_1 & \gamma\_0 & \cdots & \gamma\_{p-2} \\
\gamma\_2 & \gamma\_1 & \cdots & \gamma\_{p-3} \\
\vdots & \ddots & \ddots & \vdots \\
\gamma\_{p-1} & \gamma\_{p-2} & \cdots & \gamma\_0
\end{matrix}\right)
\left(\begin{matrix}
a\_1 \\
a\_2 \\
\vdots \\
a\_p
\end{matrix}\right)
\end{aligned}\]
记
\[\begin{aligned}
\boldsymbol{\gamma}\_p =&
\left(\begin{matrix}
\gamma\_1 \\
\gamma\_2 \\
\gamma\_3 \\
\vdots \\
\gamma\_p
\end{matrix}\right),
\ \Gamma\_p =
\left(\begin{matrix}
\gamma\_0 & \gamma\_1 & \cdots & \gamma\_{p-1} \\
\gamma\_1 & \gamma\_0 & \cdots & \gamma\_{p-2} \\
\gamma\_2 & \gamma\_1 & \cdots & \gamma\_{p-3} \\
\vdots & \ddots & \ddots & \vdots \\
\gamma\_{p-1} & \gamma\_{p-2} & \cdots & \gamma\_0
\end{matrix}\right),
\ \boldsymbol{a}\_p =
\left(\begin{matrix}
a\_1 \\
a\_2 \\
\vdots \\
a\_p
\end{matrix}\right)
\end{aligned}\]
有
\[\begin{aligned}
& \Gamma\_p \boldsymbol{a}\_p = \boldsymbol{\gamma}\_p
\end{aligned}\]

对\(\gamma\_0\)，
\[\begin{aligned}
E[X\_t \varepsilon\_t]
=& E[ (a\_1 X\_{t-1} + \dots + a\_p X\_{t-p} + \varepsilon\_t) \varepsilon\_t ] \\
=& 0 + \sigma^2 = \sigma^2 .
\end{aligned}\]
在\(X\_t = a\_1 X\_{t-1} + \dots + a\_p X\_{t-p} + \varepsilon\_t\)两边同时乘以\(X\_t\)后取期望，得  
\[\begin{aligned}
\gamma\_0 =& a\_1 \gamma\_1 + \dots + a\_p \gamma\_p + E[X\_t \varepsilon\_t] \\
=& a\_1 \gamma\_1 + \dots + a\_p \gamma\_p + \sigma^2 .
\end{aligned}\]

上式另一证明为：
\[\begin{aligned}
\gamma\_0 =& E X\_t^2
= E \left( \sum\_{j=1}^p a\_j X\_{t-j} + \varepsilon\_t
\right)^2 \\
=& E \left(\sum\_{j=1}^p a\_j X\_{t-j}\right)^2
+ E \varepsilon\_t^2 \\
=& \boldsymbol{a}\_p^T \Gamma\_p \boldsymbol{a}\_p + \sigma^2 \\
=& \boldsymbol{a}\_p^T \boldsymbol{\gamma}\_p + \sigma^2 \\
=& a\_1 \gamma\_1 + a\_2\gamma\_2 + \dots + a\_p \gamma\_p + \sigma^2
\end{aligned}\]

于是
\[\begin{aligned}
\gamma\_k =& a\_1 \gamma\_{k-1} + a\_2\gamma\_{k-2} + \dots + a\_p \gamma\_{k-p}, \ k \geq 1 \\
\gamma\_0 =& a\_1 \gamma\_1 + a\_2\gamma\_2 + \dots + a\_p \gamma\_p + \sigma^2 .
\end{aligned}\]
用推移算子写成
\[\begin{aligned}
A(\mathscr B) \gamma\_k =& 0, \ k \geq 1 \\
A(\mathscr B) \gamma\_0 =& \sigma^2 .
\end{aligned}\]

对\(n \geq p\)，令
\[\begin{aligned}
\boldsymbol{\gamma}\_n =&
\left(\begin{matrix}
\gamma\_1 \\
\gamma\_2 \\
\gamma\_3 \\
\vdots \\
\gamma\_n
\end{matrix}\right),
\ \boldsymbol{a}\_n =
\left(\begin{matrix}
a\_1 \\
a\_2 \\
\vdots \\
a\_p \\
0 \\
\vdots \\
0
\end{matrix}\right)
\end{aligned}\]
仍有
\[\begin{align}
\Gamma\_n \boldsymbol{a}\_n = \boldsymbol{\gamma}\_n
\tag{9.5}
\end{align}\]
在节[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#blpprop-blp)将可以看到，
对于一般的平稳列\(\{X\_t\}\)，
\(\boldsymbol{a}\_n\)是用\(X\_{t-1}, \dots, X\_{t-n}\)预测\(X\_t\)时的最优线性预测系数。

为了证明[(9.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw-matn)，
对\(n \geq p\),
把\(X\_t, X\_{t+1}, \dots, X\_{t+n-1}\)的递推式写成矩阵形式得
\[\begin{align}
& \left( \begin{array}{l} X\_t\\
X\_{t+1}\\
\vdots \\
X\_{t+n-1}
\end{array} \right) \\
= &
\left( \begin{array}{llll}
X\_{t-1} & X\_{t-2} & \dots &X\_{t-n}\\
X\_{t} & X\_{t-1} & \dots &X\_{t+1-n}\\
\vdots & \vdots & & \vdots \\
X\_{t+n-2} & X\_{t+n-3} & \dots & X\_{t-1}\\
\end{array} \right)
\boldsymbol{a}\_n +
\left( \begin{array}{l}
\varepsilon\_t\\
\varepsilon\_{t+1}\\
\vdots \\
\varepsilon\_{t+n-1}
\end{array} \right),
\tag{9.6}
\end{align}\]
在[(9.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw0306)两边同时乘上\(X\_{t-1}\)后取数学期望, 利用\(X\_t\)与未来输入的不相关性就可以证明[(9.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw-matn)式。

**定理9.2 (Yule-Walker方程)** AR\((p)\)序列的自协方差函数满足
\[\begin{align}
\boldsymbol{\gamma}\_n = \Gamma\_n\boldsymbol{a}\_n, \quad
\gamma\_0 = \boldsymbol{\gamma}\_n^T \boldsymbol{a}\_n + \sigma^2,
\quad n\geq p,
\tag{9.7}
\end{align}\]
即
\[\begin{align}
\gamma\_k
=& a\_1 \gamma\_{k-1} + a\_2 \gamma\_{k-2} + \dots + a\_p \gamma\_{k-p},
\quad k\geq 1 \tag{9.8} \\
A(\mathscr B) \gamma\_k =& 0, \quad k \geq 1 \\
A(\mathscr B) \gamma\_0 =& \gamma\_0 - a\_1 \gamma\_1 - a\_2 \gamma\_2 - \dots
- a\_p \gamma\_p = \sigma^2
\tag{9.9}
\end{align}\]
特别地，当\(n=p\)时
\[\begin{align}
& \Gamma\_p \left(\begin{array}{c}
a\_1 \\ a\_2 \\ \dots \\ a\_p \end{array} \right)
= \left(\begin{array}{c}
\gamma\_1 \\ \gamma\_2 \\ \dots \\ \gamma\_p
\end{array} \right)
\tag{9.10}
\end{align}\]

记\(\phi\_0=1, \phi\_1 = -a\_1, \dots, \phi\_p=-a\_p\),
则\(A(z) = \sum\_{j=0}^p \phi\_j z^j\)，
AR模型可写成\(\sum\_{j=0}^p \phi\_j X\_{t-j} = \varepsilon\_t\)。

Yule-Walker方程可写成
\[\begin{aligned}
\left(\begin{array}{cccc}
\gamma\_0 & \gamma\_1 & \cdots & \gamma\_p \\
\gamma\_1 & \gamma\_0 & \cdots & \gamma\_{p-1} \\
\vdots & \vdots & & \vdots \\
\gamma\_p & \gamma\_{p-1} & \cdots & \gamma\_0
\end{array}\right)
\left(\begin{array}{c}
\phi\_0 \\ \phi\_1 \\ \vdots \\ \phi\_p
\end{array}\right)
=
\left(\begin{array}{c}
\sigma^2 \\ 0 \\ \vdots \\ 0
\end{array}\right)
\end{aligned}\]

## 9.3 自协方差函数的周期性

对\(k<0\)定义\(\psi\_k=0\)。

**推论9.2** AR(\(p\))序列的自协方差函数\(\{\gamma\_k\}\)满足和
AR(\(p\))模型\(A(\mathscr B) X\_t = \varepsilon\_t\)相应的差分方程
\[\begin{aligned}
\gamma\_k - (a\_1 \gamma\_{k-1} + a\_2 \gamma\_{k-2}
+ \dots + a\_p \gamma\_{k-p}) = \sigma^2 \psi\_{-k},
\quad k \in \mathbb Z .
\end{aligned}\]

**证明**:
\(k \geq 0\)时即定理结论。对\(k<0\)，
\[\begin{aligned}
& \gamma\_k - (a\_1 \gamma\_{k-1} + a\_2 \gamma\_{k-2}
+ \dots + a\_p \gamma\_{k-p}) \\
=& E\left[ X\_{t-k} ( X\_t - \sum\_{j=1}^p a\_j X\_{t-j} ) \right] \\
=& E(X\_{t-k} \varepsilon\_t)
= E \left[ \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-k-j}
\varepsilon\_t \right]
= \sigma^2 \psi\_{-k} .
\end{aligned}\]

○○○○○○

设\(A(z)\)有\(p\)个互异根\(z\_j=\rho\_j e^{i\lambda\_j}, j=1,\dots,p\)，
可以证明（略）
\[\begin{align}
\gamma\_t =& A^{-1}(\mathscr B) \sigma^2 \psi\_{-t} \\
=& \sigma^2 \sum\_{j=1}^p c\_j A^{-1}(z\_j^{-1}) z\_j^{-t} \\
=& \sigma^2 \sum\_{j=1}^p A\_j \rho\_j^{-t} \cos(\lambda\_j t + \theta\_j),
\quad t \geq 0 .
\tag{9.11}
\end{align}\]
可见如果\(\{z\_j\}\)中有靠近单位圆的复根则\(\{\gamma\_k\}\)
的衰减振荡特性会显现出来。

**例9.1** AR(4)模型1:
\[\begin{aligned}
z\_1,z\_2 = 1.09e^{\pm i\pi/3}, \quad
z\_3,z\_4 = 1.098e^{\pm i 2\pi/3}
\end{aligned}\]
周期为\(2\pi/(\pi/3)=6\)和\(2\pi/(2\pi/3)=3\)。

AR(4)模型2:
\[\begin{aligned}
z\_1,z\_2 = 1.264e^{\pm i\pi/3}, \quad
z\_3,z\_4 = 1.273e^{\pm i 2\pi/3}
\end{aligned}\]

AR(4)模型3:
\[\begin{aligned}
z\_1,z\_2 = 1.635e^{\pm i\pi/3}, \quad
z\_3,z\_4 = 1.647e^{\pm i 2\pi/3}
\end{aligned}\]

**程序演示**:

```
library(polynom)
demo.ar.roots <- function(){
  n <- 1024
  rtlis <- list(c(complex(mod=1.09, arg=pi/3*c(1,-1)),
                  complex(mod=1.098, arg=pi*2/3*c(1,-1))),
                c(complex(mod=1.264, arg=pi/3*c(1,-1)),
                  complex(mod=1.273, arg=pi*2/3*c(1,-1))),
                c(complex(mod=1.635, arg=pi/3*c(1,-1)),
                  complex(mod=1.647, arg=pi*2/3*c(1,-1))),
                complex(mod=1.02, arg=pi/6*c(1,-1)),
                complex(mod=1.02, arg=pi/2*c(1,-1)),
                c(complex(mod=1.05, arg=pi/6*c(1,-1)),
                  complex(mod=1.05, arg=pi/2*c(1,-1))))
  tits <- c("AR(4): 1.09exp(+-i pi/3), 1.098exp(+-i 2pi/3)",
            "AR(4): 1.264exp(+-i pi/3), 1.273exp(+-i 2pi/3)",
            "AR(4): 1.635exp(+-i pi/3), 1.647exp(+-i 2pi/3)",
            "AR(2): mod=1.02 arg=+-pi/6",
            "AR(2): mod=1.02 arg=+-pi/2",
            "AR(4): mod=1.05 arg=+-pi/6,+-pi/2")
  tits <- c(
    expression(paste("AR(4):", 
                     list(1.09*e^{phantom(.) %+-% i*frac(pi,3)}, 
                          1.098*e^{phantom(.) %+-% i*frac(2*pi,3)}) )),
    expression(paste("AR(4):", 
                     list(1.264*e^{phantom(.) %+-% i*frac(pi,3)}, 
                          1.273*e^{phantom(.) %+-% i*frac(2*pi,3)}) )),
    expression(paste("AR(4):", 
                     list(1.635*e^{phantom(.) %+-% i*frac(pi,3)}, 
                          1.647*e^{phantom(.) %+-% i*frac(2*pi,3)}) )),
    expression(paste("AR(2):", 
                     list(1.02*e^{phantom(.) %+-% i*frac(pi,6)}) )),
    expression(paste("AR(2):", 
                     list(1.02*e^{phantom(.) %+-% i*frac(pi,2)}) )),
    expression(paste("AR(4):", 
                     list(1.05*e^{phantom(.) %+-% i*frac(pi,6)}, 
                          1.05*e^{phantom(.) %+-% i*frac(pi,2)}) ))
    )
  oldpar <- par(mfrow=c(3,1), mar=c(2,2,0,0), 
                mgp=c(2, 0.5, 0), oma=c(0,0,2,0))
  on.exit(par(oldpar))
  for(ii in seq(along=rtlis)){
    rt = rtlis[[ii]]
    y <- ar.gen(n, rt, sigma=1.0, by.roots=TRUE,
                plot.it=FALSE)
    plot(window(y, 1, 60))
    acf(y)
    ##spectrum(y, taper=0.2)
    ar.true.spectrum(attr(y, "a"), title="")
    mtext(tits[ii], side=3, outer=TRUE)
  }
}
demo.ar.roots()
```

```
## Warning in polynomial(p): 强制改变时丢弃了虚数部分
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2018-arspecyw_files/figure-html/arspecyw-rootsdemo01-1.png)

```
## Warning in polynomial(p): 强制改变时丢弃了虚数部分
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2018-arspecyw_files/figure-html/arspecyw-rootsdemo01-2.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2018-arspecyw_files/figure-html/arspecyw-rootsdemo01-3.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2018-arspecyw_files/figure-html/arspecyw-rootsdemo01-4.png)

```
## Warning in polynomial(p): 强制改变时丢弃了虚数部分
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2018-arspecyw_files/figure-html/arspecyw-rootsdemo01-5.png)![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/2018-arspecyw_files/figure-html/arspecyw-rootsdemo01-6.png)

## 9.4 自协方差函数的正定性

AR(\(p\))平稳解唯一故自协方差函数可被自回归系数和白噪声方差唯一决定。
反之，
若\(\Gamma\_p\)正定则根据Yule-Walker方程可以从
\(\gamma\_0, \gamma\_1, \dots, \gamma\_p\)解出\(a\_1, \dots, a\_p, \sigma^2\):
\[\begin{align}
\boldsymbol{a}\_p = \Gamma\_p^{-1} \boldsymbol{\gamma}\_p, \quad
\sigma^2 = \gamma\_0 - \boldsymbol{\gamma}\_p^T \Gamma\_p^{-1} \boldsymbol{\gamma}\_p.
\tag{9.12}
\end{align}\]

**定理9.3** 设\(\Gamma\_n\)是实值平稳序列\(\{X\_t\}\)的\(n\)阶自协方差矩阵,
\(\gamma\_0>0\)。

(1) 如果\(\{X\_t\}\)的谱密度\(f(\lambda)\)存在,
则对\(n\geq 1\)，\(\Gamma\_n\)正定；

(2) 如果\(\lim\_{k \to \infty} \gamma\_k = 0\)，
则对\(n \geq 1\)，\(\Gamma\_n\)正定。

**引理9.1** 对实平稳列\(\{X\_t\}\)，设其自协方差阵为\(\Gamma\_n\), \(n \in \mathbb N\)；
设其谱函数为\(F(\lambda)\)。
\(\forall \boldsymbol{b}=(b\_1, \dots, b\_n) \in \mathbb C^n\)有
\[\begin{aligned}
\boldsymbol{b}^\* \Gamma\_n \boldsymbol{b} = \int\_{-\pi}^\pi
\left| \sum\_{j=1}^n b\_j e^{ij\lambda} \right|^2 dF(\lambda) .
\end{aligned}\]
若\(\{X\_t\}\)有谱密度\(f(\lambda)\)则
\[\begin{aligned}
\boldsymbol{b}^\* \Gamma\_n \boldsymbol{b} = \int\_{-\pi}^\pi
\left| \sum\_{j=1}^n b\_j e^{ij\lambda} \right|^2 f(\lambda) d\lambda .
\end{aligned}\]

**引理证明：**

\[\begin{aligned}
& \boldsymbol{b}^\* \Gamma\_n \boldsymbol{b}
= \sum\_{j=1}^n \sum\_{k=1}^n
\bar b\_j b\_k \gamma\_{k-j} \\
=& \sum\_{j=1}^n \sum\_{k=1}^n \bar b\_j b\_k
\int\_{-\pi}^\pi e^{i(k-j)\lambda} dF(\lambda) \\
=& \int\_{-\pi}^\pi \sum\_{j=1}^n \sum\_{k=1}^n \bar b\_j b\_k
e^{-ij\lambda} e^{ik\lambda} dF(\lambda) \\
=& \int\_{-\pi}^\pi
\overline{\left( \sum\_{j=1}^n b\_j e^{ij\lambda} \right)}
\left( \sum\_{k=1}^n b\_k e^{ik\lambda} \right) \,dF(\lambda)\\
=& \int\_{-\pi}^\pi
\left| \sum\_{j=1}^n b\_j e^{ij\lambda} \right|^2 dF(\lambda) .
\end{aligned}\]

**定理证明：**

(1) 对\(\boldsymbol{b} = (b\_1, \dots, b\_n)^T\)，
\(\sum\_{j=1}^n b\_j z^{j-1}\)
至多有\(n-1\)个零点。\(\gamma\_0 = \int\_{-\pi}^\pi f(\lambda) d\lambda > 0\),
于是
\[\begin{aligned}
\boldsymbol{b}^T \Gamma\_n \boldsymbol{b}
= \int\_{-\pi}^\pi \left| \sum\_{j=1}^n b\_j e^{ij\lambda} \right|^2
f(\lambda) d \lambda > 0 .
\end{aligned}\]

(2) 用反证法。
设\(\Gamma\_n\)正定,
\(\det(\Gamma\_{n+1})=0\)和\(EX\_t=0\)
(非零均值情况只要减去均值).  
定义
\[
\boldsymbol{X}\_n=(X\_1,X\_{2},\dots, X\_n)^T
\]
对任何实向量
\(\boldsymbol{b}=(b\_1,b\_2,\dots,b\_n)^T \neq 0\) 有
\[
E(\boldsymbol{b}^T \boldsymbol{X}\_n)^2
=\boldsymbol{b}^T \Gamma\_n \boldsymbol{b} >0,
\]
且由\(|\Gamma\_{n+1}|=0\)知存在
\(\boldsymbol{a} =(a\_1,a\_2,\dots,a\_{n+1})^T \neq 0\), \(a\_{n+1}\neq 0\)使得
\[
E(\boldsymbol{a}^T \boldsymbol{X}\_{n+1})^2
= \boldsymbol{a} ^T \Gamma\_{n+1} \boldsymbol{a}
=0.
\]
于是
\[
\boldsymbol{a}^T \boldsymbol{X}\_{n+1} = a\_1 X\_1 + a\_2 X\_2 + \dots + a\_{n+1} X\_{n+1}=0
\]
a.s.成立, \(X\_{n+1}\)可以由\(\boldsymbol{X}\_n\)线性表示:
\[\begin{aligned}
X\_{n+1} = -\frac{a\_n}{a\_{n+1}} X\_{n}
- \frac{a\_{n-1}}{a\_{n+1}} X\_{n-1} - \dots
- \frac{a\_1}{a\_{n+1}} X\_{1}, \quad
\text{a.s.},
\end{aligned}\]

利用\(\{X\_t\}\)的平稳性知道
\[\begin{aligned}
X\_t = -\frac{a\_n}{a\_{n+1}} X\_{t-1}
- \frac{a\_{n-1}}{a\_{n+1}} X\_{t-2} - \dots
- \frac{a\_1}{a\_{n+1}} X\_{t-n}, \quad
\text{a.s.}, \quad t \in \mathbb Z .
\end{aligned}\]

递推知对任何\(k\geq 1\), \(X\_{n+k}\) 可以由\(X\_1,X\_2,\dots,X\_n\)线性表示,
即有实向量
\(\boldsymbol{\alpha} \stackrel{\triangle}{=} \boldsymbol{\alpha}^{(k)} \stackrel{\triangle}{=} (\alpha\_1^{(k)}, \dots, \alpha\_n^{(k)})^T\)
使得
\[
X\_{n+k}=(\boldsymbol{\alpha}^{(k)})^T \boldsymbol{X}\_n.
\]

\(X\_{n+k}\)被\(\boldsymbol{X}\_n\)线性表示，
说明\(X\_{n+k}\)与\(X\_1,\dots,X\_n\)有强的相关，
而定理假设是\(\gamma\_k \to 0\)，
又说明\(X\_{n+k}\)与\(X\_1,\dots,X\_n\)的相关性要趋于零，
这就会有矛盾，下面把矛盾严格表述。

用 \(0 < \lambda\_1 \leq \lambda\_2 \dots \leq \lambda\_n\)
表示\(\Gamma\_n\)的特征值,
则有正交矩阵 \(B\)使得
\[
B \Gamma\_n B^T = \Lambda
= \text{diag}(\lambda\_1, \lambda\_2,\dots,\lambda\_n).
\]  
用\(|\boldsymbol{\alpha}^{(k)}|\)
表示\(\boldsymbol{\alpha}^{(k)}\)的欧氏模, 则有
\[\begin{aligned}
\gamma\_0 =& E X\_{n+k}^2
= E((\boldsymbol{\alpha}^{(k)})^T \boldsymbol{X}\_n)^2
= (\boldsymbol{\alpha}^{(k)})^T \Gamma\_n \boldsymbol{\alpha}^{(k)} \\
=& ( (\boldsymbol{\alpha}^{(k)})^T B^T)
(B \Gamma\_n B^T)
(B \boldsymbol{\alpha}^{(k)} )\\
=& ( (\boldsymbol{\alpha}^{(k)})^T B^T)
\Lambda
(B \boldsymbol{\alpha}^{(k)} )\\
\geq& \lambda\_1 (B \boldsymbol{\alpha}^{(k)})^T
(B \boldsymbol{\alpha}^{(k)} )
= \lambda\_1 |\boldsymbol{\alpha}^{(k)}|^2.
\end{aligned}\]
即有\(|\boldsymbol{\alpha}^{(k)} | \leq \sqrt{\gamma\_0/\lambda\_1} < \infty\).

另一方面
\[\begin{aligned}
\gamma\_0 =& E( (\boldsymbol{\alpha}^{(k)})^T \boldsymbol{X}\_n \cdot X\_{n+k})
= (\boldsymbol{\alpha}^{(k)})^T E(\boldsymbol{X}\_n X\_{n+k} ) \\
=& (\boldsymbol{\alpha}^{(k)})^T
(\gamma\_{n+k-1}, \gamma\_{n+k-2}, \dots,\gamma\_{k})^T\\
\leq & |\boldsymbol{\alpha}^{(k)}|
\left(\sum\_{j=0}^{n-1}\gamma\_{j+k}^2\right)^{1/2}\\
\leq & (\gamma\_0/\lambda\_1)^{1/2}
\left(\sum\_{j=0}^{n-1}\gamma\_{k+j}^2\right)^{1/2}\\
\to & 0. \quad (\text{当}k\to \infty)
\end{aligned}\]
这与\(\gamma\_0>0\)矛盾, 故 \(\det(\Gamma\_{n+1})=0\)不成立.

○○○○○○

**推论9.3** 系数平方可和的线性平稳序列的自协方差阵总是正定的。

这是上面的定理与定理[6.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-spectrum.html#thm:spec-linser)的推论。

对平稳列\(\{X\_n \}\)的自协方差函数\(\{\gamma\_j, j \in \mathbb Z\}\)，
如果\(\{X\_n \}\)自协方差阵总是正定的，
则称自协方差函数\(\{\gamma\_j, j \in \mathbb Z\}\)是正定序列。

**定理9.4** 设实平稳列\(\{ X\_t \}\)的谱函数\(F(\lambda)\)是阶梯函数。
如果\(F(\lambda)\)恰好有\(n\)个跳跃点，
则\(\Gamma\_n\)正定而\(\Gamma\_{n+1}\)退化。
如果\(F(\lambda)\)有无穷个跳跃点，
则对任意\(n \geq 1\)，\(\Gamma\_n\)都是正定的。

**证明**：
\(\forall \boldsymbol b = (b\_1, \dots, b\_n)^T\),
\(\lambda\)的函数
\[
g(\lambda) = \sum\_{k=1}^n b\_k e^{i k \lambda}
\]
是函数
\[
h(z) = \sum\_{k=1}^n b\_k z^k
= z \sum\_{j=0}^{n-1} b\_{j+1} z^j
\]
在\(z = e^{i\lambda}\)的值，
所以\(g(\lambda)\)至多有\(n-1\)个零点。
当\(F(\lambda)\)的跳跃点个数\(\geq n\)时，
\[\begin{aligned}
\boldsymbol b^T \Gamma\_n \boldsymbol b
=& \int\_{-\pi}^{\pi}
\left| \sum\_{k=1}^n b\_k e^{i k \lambda} \right|^2 \, d F(\lambda) \\
=& \int\_{-\pi}^{\pi} |g(\lambda)|^2 \, d F(\lambda)
\end{aligned}\]
关于\(d F(\lambda)\)的积分等于跳跃高度乘以跳跃点处被积函数然后求和，
这里被积函数只有至多\(n-1\)个零点而跳跃点有\(n\)个以上，
所以求和至少有一项非零，故积分为正值，
即\(\Gamma\_n\)正定。

如果\(G(\lambda)\)恰好有如下的\(n\)个跳跃点：
\[
-\pi < \lambda\_1 < \dots < \lambda\_n \leq \pi
\]
定义复数\(b\_0, b\_1, \dots, b\_n\)为
\[
\sum\_{k=0}^n b\_k e^{i k \lambda}
= \prod\_{j=1}^n (1 - e^{i (\lambda - \lambda\_j)})
\]
则\(b\_0 = 1\)，
令\(\boldsymbol b = (b\_0, b\_1, \dots, b\_n)^T \neq \boldsymbol 0\)，
当\(G(\cdot)\)是\(n/2\)个频率的离散谱序列时，
因为频率是相反数成对出现的，
所以\(\boldsymbol b\)为实向量。
对\(\boldsymbol b\)有
\[
\boldsymbol b^\* \Gamma\_{n+1} \boldsymbol b
= \int\_{-\pi}^{\pi} \left|
\sum\_{k=0}^n b\_k e^{i k \lambda} \right|^2 \, d F(\lambda)
\]
因为函数\(\sum\_{k=0}^n b\_k e^{i k \lambda}\)在\(F(\lambda)\)的\(n\)个跳跃点处都等于零，
所以上述积分为零，
如果\(\boldsymbol b\)为实向量，
则\(\Gamma\_{n+1}\)退化。

如果\(\boldsymbol b\)为复向量，
对零均值平稳列\(\{ X\_t \}\)有
\[
\text{Var}(\sum\_{j=0}^n b\_j X\_j) =
E \left| \sum\_{j=0}^n b\_j X\_j \right|^2 =
\boldsymbol b^\* \Gamma\_{n+1} \boldsymbol b
= 0
\]
取\(\boldsymbol a = \text{Re}(\boldsymbol b)\)，
则\(\boldsymbol a^T \Gamma\_{n+1} \boldsymbol a = 0\)，
且\(a\_0 = b\_0 = 1\)故\(\boldsymbol a \neq \boldsymbol 0\)，
\(\Gamma\_{n+1}\)退化。
定理证毕。

○○○○○○

## 9.5 时间序列的可完全预测性

有限个频率的离散谱序列的轨道具有周期性，
可以用有限个历史值的线性组合无误差地预报整个序列。

对于方差有限的随机变量\(Y\_1, Y\_2,\cdots,Y\_n\),
如果有不全为零的常数\(b\_1,\dots,b\_n\)使得
\[
\text{Var} \large(\sum\_{j=1}^n b\_j Y\_{j} \large) =0,
\]
则称随机变量\(Y\_1, Y\_2,\cdots,Y\_n\) 是**线性相关**的,
否则称为**线性无关**的.

线性相关时, 存在常数\(b\_0\)使得\(\sum\_{j=1}^n b\_j Y\_{j} = b\_0\)
a.s.成立.  
并且当\(b\_n\neq 0\)时,
\(Y\_n\)可以由\(Y\_1, Y\_2,\cdots,Y\_{n-1}\)线性表示:
\[\begin{aligned}
Y\_n = a\_0 + a\_1 Y\_{n-1} + \dots + a\_{n-1} Y\_1
\end{aligned}\]
这时我们称\(Y\_n\)可以由\(Y\_1, Y\_2,\cdots,Y\_{n-1}\)
**完全线性预测**.

对于平稳序列\(\{X\_t\}\)，\(X\_{t-1}, \dots, X\_{t-n}\)的一个带截距的线性组合为
\(b\_1 X\_{t-1} + \dots b\_n X\_{t-n} - b\_0\)，这\(n\)个变量线性无关当且仅当
\[\begin{aligned}
& \text{Var}(\sum\_{j=1}^n b\_j X\_{t-j} - b\_0)
= \text{Var}(\sum\_{j=1}^n b\_j X\_{t-j}) \\
=& \boldsymbol{b} \Gamma\_n \boldsymbol{b} > 0
\end{aligned}\]
即\(\Gamma\_n\)正定。

反之，若\(\Gamma\_n\)正定而\(\Gamma\_{n+1}\)不满秩，
则\(X\_t\)可以被\(X\_{t-1}, \dots, X\_{t-n}\)完全线性预测。

线性平稳列不能完全线性预测。

有限个频率成分的离散谱序列可完全线性预测。