---
crawl_time: '2026-01-17 14:29:22'
framework: sphinx
title: 2 线性平稳序列和线性滤波 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 2 线性平稳序列和线性滤波

## 2.1 有限运动平均

线性平稳序列是白噪声的线性组合得到的序列。
最简单的线性平稳序列是有限运动平均。

设\(\{\varepsilon\_t\}=\{\varepsilon\_t: \ t \in \mathbb Z\}\)
是\(\text{WN}(0,\sigma^2)\).
对于非负整数\(q\)和常数
\(a\_0,a\_1,\ldots,a\_q\)(\(a\_0 \neq 0, a\_q \neq 0\)),
我们称
\[
X\_t = \sum\_{j=0}^q a\_j \varepsilon\_{t-j}
= a\_0\varepsilon\_t +a\_1\varepsilon\_{t-1}+\cdots
+ a\_q\varepsilon\_{t-q}, \ \ t \in \mathbb Z
\]
是白噪声\(\{\varepsilon\_t\}\)的(有限)运动平均或滑动平均,
简称为 MA (Moving Average).

MA的平稳性:

\[\begin{aligned}
E X\_t =& 0 , \\
E (X\_{t+k} X\_t) =& \begin{cases}
\sigma^2 \sum\_{j=0}^{q-k} a\_j a\_{j+k}, & 0 \leq k \leq q, \\
0, & k > q
\end{cases}
\end{aligned}\]

可见\(\{X\_t\}\)平稳。
\(\gamma\_k = 0\), \(\forall k>q\), 称这样的序列为\(q\)相关的。

## 2.2 线性平稳序列

### 2.2.1 期望与极限交换次序

随机变量有限平均到随机变量无穷级数的推广需要概率论的极限理论。

**定理2.1 (单调收敛定理)** 如果非负随机变量序列\(\{\xi\_n \}\)单调不减:
\(0\leq \xi\_1 \leq \xi\_2 \leq \cdots\),
则当 \(\xi\_n \to \xi\) a.s. 时,
有\(E\xi =\lim\_{n\to \infty} E\xi\_n\).

这里的随机变量是广义随机变量，允许取\(+\infty\)值。

对于任何时间序列 \(\{Y\_t\}\), 利用单调收敛定理得到
\[\begin{aligned}
E\left[ \sum\_{t=-\infty}^{\infty} |Y\_t| \right]=&
\lim\_{n\to \infty} E \left[ \sum\_{t=-n}^{n} |Y\_t| \right] \\
=& \lim\_{n\to \infty}\sum\_{t=-n}^{n} E|Y\_t|
= \sum\_{t=-\infty}^{\infty} E |Y\_t|.
\end{aligned}\]

**定理2.2 (控制收敛定理)** 如果随机变量序列\(\{\xi\_n\}\)满足\(|\xi\_n|\leq \xi\_0\) a.s. 和
\(E|\xi\_0|< \infty\),
则当\(\xi\_n \to \xi\), a.s.时,
\(E|\xi|<\infty\) 并且
\(E\xi\_n \to E\xi\).

在定理[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#thm:linser-problim-monot)和定理[2.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#thm:linser-problim-contr)条件下均有，
\[\begin{aligned}
\lim\_{n \to \infty} E \xi\_n = E \lim\_{n \to \infty} \xi\_n.
\end{aligned}\]
即期望与极限可以交换次序。

**推论2.1** 如果随机变量序列\(\{Y\_n\}\)满足\(\sum\_{i=-\infty}^\infty E|Y\_i|\leq \infty\)，则
\(\sum\_{i=-\infty}^\infty Y\_i\) a.s.收敛,
\(E|\sum\_{i=-\infty}^\infty Y\_i| < \infty\)且
\[\begin{aligned}
E \sum\_{n=-\infty}^\infty Y\_n
= \sum\_{n=-\infty}^\infty E Y\_n
\end{aligned}\]

**证明**
令
\[
\eta = \sum\_{i=-\infty}^\infty |Y\_i|
\]
则\(\eta\)是随机变量，
由单调收敛定理知\(E\eta < \infty\),
\(\eta < \infty\), a.s.。

令
\[
\xi\_n = \sum\_{i=-n}^n Y\_i
\]
则
\[
|\xi\_n| \leq \eta,
\]
记事件\(A = \{ \eta < \infty \}\)，
则\(P(A) = 1\)，
对\(\omega \in A\),
有\(\sum\_{i=-\infty}^\infty |Y\_i(\omega)| < \infty\)所以\(\sum\_{i=-\infty}^\infty Y\_i(\omega)\)收敛，
\(\lim\_{n\to\infty} \xi\_n(\omega)\)收敛，记为\(\xi(\omega)\)；
对\(\omega \notin A\), 定义\(\xi(\omega) = 0\)，
则\(\xi\)是随机变量，
且
\[
\lim\_{n\to\infty} \xi\_n = \xi, \text{ a.s.}
\]
记
\[
\xi = \sum\_{i=-\infty}^\infty \xi\_i .
\]
由控制收敛定理
\[
\lim\_{n\to\infty} E \xi\_n = E \xi,
\]
而
\[
\lim\_{n\to\infty} E \xi\_n
= \lim\_{n\to\infty} E \sum\_{i=-n}^n Y\_i
= \lim\_{n\to\infty} \sum\_{i=-n}^n E Y\_i
= \sum\_{i=-\infty}^\infty E Y\_i,
\]
于是有
\[
E \sum\_{i=-\infty}^\infty \xi\_i = \sum\_{i=-\infty}^\infty E \xi\_i .
\]
○○○○○○

如果实数列 \(\{a\_j\}\) 满足
\[
\sum\_{j=-\infty}^{\infty} |a\_j| < \infty,
\]
则称\(\{a\_j\}\)是**绝对可和**的. 记\(\{a\_j\} \in l\_1\).

注意: \(\{a\_j\} \in l\_1\)则\(\{a\_j\} \in l\_2\)
(即\(\sum\_j a\_j^2 < \infty\)).
反之不一定成立。

### 2.2.2 线性序列定义

对于绝对可和的实数列\(\{a\_j\}\),
定义零均值白噪声\(\{\varepsilon\_t\}\)的无穷滑动和如下
\[
X\_t=\sum\_{j=-\infty}^{\infty} a\_j \varepsilon\_{t-j},
\ \ t \in \mathbb Z.
\]
则\(\{X\_t\}\)是平稳序列。
\(E X\_t = 0\),
\[\begin{align}
\gamma\_k = \sigma^2 \sum\_{j=-\infty}^\infty a\_j a\_{j+k}, k \in \mathbb Z .
\tag{2.1}
\end{align}\]

令\(c\_k = \sum\_{j=-\infty}^\infty a\_j a\_{j+k}\), \(k \in \mathbb Z\)。

### 2.2.3 线性序列的a.s.收敛性

作为无穷和\(\{X\_t\}\)有没有定义？
由Schwarz不等式，
\[
E|\varepsilon\_t|
= E(|\varepsilon\_t| \cdot 1)
\leq \sqrt{E \varepsilon\_t^2 \cdot 1}
= \sigma,
\]
所以
\[
\sum\_{j=-\infty}^\infty E |a\_j \varepsilon\_{t-j}|
= \sum\_{j=-\infty}^\infty |a\_j| E|\varepsilon\_{t-j}|
\leq \sigma \sum\_{j=-\infty}^\infty |a\_j| < \infty,
\]
由推论[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#cor:linser-problim-suminfexp)可知\(\sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j}\) a.s.收敛。

### 2.2.4 线性序列的L1收敛性

级数的余项的\(L\_1\)模
\[\begin{aligned}
& E \left|
\sum\_{|j|>N} a\_j \varepsilon\_{t-j}
\right|
\leq \sum\_{|j|>N} |a\_j| E |\varepsilon\_{t-j}|
\leq \sigma \sum\_{|j|>N} |a\_j| \to 0
\end{aligned}\]
即线性平稳列在\(L\_1\)意义下收敛。

### 2.2.5 线性序列的平稳性

由推论[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#cor:linser-problim-suminfexp)可知
\[\begin{aligned}
E \sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j}
&= \sum\_{j=-\infty}^\infty a\_j E \varepsilon\_{t-j} = 0
\end{aligned}\]

由Schwarz不等式知\(E|\epsilon\_{t-j} \epsilon\_{t+k-l}| \leq \sigma^2\), 于是
\[
\sum\_{j=-\infty}^\infty \sum\_{l=-\infty}^\infty
E |a\_j a\_l \epsilon\_{t-j} \epsilon\_{t+k-l}|
\leq \sigma^2 \sum\_{j=-\infty}^\infty \sum\_{l=-\infty}^\infty |a\_j|\; |a\_l|
= \sigma^2 \left( \sum\_{j=-\infty}^\infty |a\_j| \right)^2 < \infty,
\]
由推论[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#cor:linser-problim-suminfexp)，
\[\begin{aligned}
E X\_t X\_{t+k} &=
E \sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j}
\sum\_{l=-\infty}^\infty a\_l \varepsilon\_{t+k-l} \\
&= \sum\_{j=-\infty}^\infty \sum\_{l=-\infty}^\infty
a\_j a\_l E(\varepsilon\_{t-j} \varepsilon\_{t+k-l}) \\
&= \sigma^2 \sum\_{j=-\infty}^\infty a\_j a\_{j+k}
\end{aligned}\]
即\(\{X\_t\}\)是平稳序列, \(E X\_t=0\)，
\(\gamma\_k = \sigma^2 \sum\_{j=-\infty}^\infty a\_j a\_{j+k}\).

### 2.2.6 线性序列的L2收敛性

设\(\{a\_j\} \in l\_2\)，即\(\sum\_j a\_j^2 < \infty\)，则
\[\begin{aligned}
X\_t=\sum\_{j=-\infty}^{\infty} a\_j \varepsilon\_{t-j}\ (L^2)
\end{aligned}\]
也是平稳序列。期望为零，自协方差函数同上。

\(X\_t\)定义的无穷级数是\(L^2\)收敛的。
证明需要应用Hilbert空间性质。
见[5.1.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-Hilbert-station.html#Hilbert-space-L2sta)。

注意\(\{a\_j \} \in l\_1 \Longrightarrow \{a\_j \} \in l\_2\)。

### 2.2.7 线性序列的自协方差函数收敛性

当\(\{a\_j\} \in l\_1\)时
\[
\sum\_{k=-\infty}^{\infty} |\gamma\_k| < \infty
\]
事实上，
\[\begin{aligned}
& \sum\_{k=-\infty}^{\infty} |\gamma\_k|
\leq \sigma^2 \sum\_{k=-\infty}^{\infty}
\sum\_{j=-\infty}^{\infty} |a\_j a\_{j+k}| \\
& = \sigma^2 \sum\_{j=-\infty}^{\infty} |a\_j|
\sum\_{k=-\infty}^{\infty} |a\_{j+k}|
= \sigma^2 (\sum\_{j=-\infty}^{\infty}|a\_j|)^2 < \infty
\end{aligned}\]

**定理2.3** 当\(\{a\_j\} \in l\_2\)时, 自协方差函数 \(\lim\_{k\to \infty}\gamma\_k =0\).

**证明**: 利用Cauchy不等式
\(|\sum a\_j b\_j | \leq \left( \sum a\_j^2 \sum b\_j^2 \right)^{1/2}\)
得到
\[\begin{aligned}
|\gamma\_k| =& \sigma^2 \left| \sum\_{j=-\infty}^{\infty} a\_j a\_{j+k} \right| \\
\leq& \sigma^2 \sum\_{|j| \leq k/2} | a\_j a\_{j+k} |
+ \sigma^2 \sum\_{|j| > k/2} | a\_j a\_{j+k} | \\
\leq& \sigma^2 \left[ \sum\_{j=-\infty}^{\infty} a\_j^2 \sum\_{|j| \leq k/2} a\_{j+k}^2
\right]^{1/2} +
\sigma^2 \left[ \sum\_{|j| > k/2} a\_j^2 \sum\_{j=-\infty}^{\infty} a\_j^2
\right]^{1/2} \\
\leq& 2\sigma^2 \left[ \sum\_{j=-\infty}^{\infty} a\_j^2 \right]^{1/2}
\left[ \sum\_{|j| \geq k/2} a\_j^2 \right]^{1/2}
\to 0 \quad (k \to \infty)
\end{aligned}\]

○○○○○○

线性序列的应用：

线性序列描述了自协方差函数衰减到零的时间序列。
只要样本自协方差函数衰减到零就可以用线性序列来描述。

单边线性序列：

\[
X\_t = \sum\_{j={\mathbf 0}}^\infty a\_j \varepsilon\_{t-j},
\ t \in \mathbb Z
\]
称为单边运动平均(MA)，或单边无穷滑动和。
这样的\(X\_t\)有因果性：\(X\_t\)只受\(s \leq t\)的\(\varepsilon\_s\)影响而不受
\(t\)时刻以后的\(\varepsilon\_s\)影响。
\[
\gamma\_k = \begin{cases}
\sum\_{j=0}^\infty a\_j a\_{j+k},\ & k \geq 0 \\
\gamma\_{-k} \ & k < 0
\end{cases}
\]

## 2.3 时间序列的线性滤波

对序列\(\{X\_t \}\)进行滑动求和：
\[
Y\_t = \sum\_{j=-\infty}^\infty h\_j X\_{t-j}, \ \ t\in \mathbb Z
\]
称为对\(\{X\_t\}\)进行**线性滤波**。
其中绝对可和的\(\{h\_j\}\)称为一个**保时线性滤波器**。
如果输入信号\(\{X\_t\}\)是平稳列则输出\(\{Y\_t\}\)也是平稳列。
\(\{ Y\_t \}\)的收敛性可以用推论[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#cor:linser-problim-suminfexp)得出。

关于线性滤波的性质，
在[7.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#lagdiff-lagop)中还有进一步讨论。

当\(\{ X\_t \}\)平稳时，由推论[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#cor:linser-problim-suminfexp)，
\[
\mu\_Y = E Y\_t
= \sum\_{j=-\infty}^\infty h\_j E X\_{t-j}
= \mu\_X \sum\_{j=-\infty}^\infty h\_j
\]

\[\begin{aligned}
\gamma\_Y(n) &= \text{Cov}(Y\_{n+1}, Y\_1) \\
&= \sum\_{j, k=-\infty}^\infty h\_j h\_k E[(X\_{n+1-j}-\mu)(X\_{1-k}-\mu)] \\
&= \sum\_{j, k= -\infty}^\infty h\_j h\_k \gamma\_{n+k-j}
\end{aligned}\]

**例2.1 (矩形窗滤波器)** 取
\[
h\_j = \begin{cases}
\frac{1}{2M+1}, \ & |j| \leq M \\
0, & |j|>M.
\end{cases}
\]
则
\[
Y\_t = \frac{1}{2M+1} \sum\_{j=-M}^M x\_{t-j}
\]
是\({X\_t}\)的滑动平均，
这种滤波称为**矩形窗滤波器**。
可以平滑\(\{X\_t\}\)，抑制高频信号。
高频信号表现是粗糙和复杂的曲线，低频信号表现为缓慢和光滑的变化。

○○○○○○

**例2.2 (余弦波信号的滤波)** 设
\[
X\_t = S\_t + \varepsilon\_t
= b\cos(\omega t+U) +\varepsilon\_t, \ \ t \in \mathbb Z
\]
其中\(U \sim\)U(\(0, 2\pi\)), \(\{\varepsilon\_t\}\)零均值平稳,
\(U\)与\(\{\varepsilon\_t\}\)独立。
信号\(\{S\_t\}\)方差\(b^2/2\)，
噪声\(\{\varepsilon\_t\}\)方差\(\sigma^2\),
信号与方差之比称为**信噪比**，
等于\(b^2/(2\sigma^2)\)。
用矩形窗滤波。

\[\begin{aligned}
Y\_t &= \frac{1}{2M+1} \sum\_{j=-M}^M X\_{t-j} \\
&= \frac{b\sin[\omega (M+0.5)]}{(2M+1)\sin(\omega /2)}
\cos(\omega t+U) + \eta\_t \\
\eta\_t &= \frac{1}{2M+1}\sum\_{j=-M}^M \varepsilon\_{t-j}
\end{aligned}\]

除了\(\eta\_t\)项之外，结果与原始信号相比只有幅度有了成比例变化。
上式的化简可以用复数的极坐标表示来推导有关三角函数求和的公式。
\(\text{Var}(\eta\_t) = \frac{\sigma^2}{2M+1}\).

新的信噪比为
\[
\frac{b^2}{2\sigma^2}
\frac{\sin^2[\omega (M+0.5)]}{(2M+1)\sin^2(\omega /2)}.
\]
特别当\(\omega(M+0.5) = \pi/2\)时信噪比为
\[
\frac{b^2 \omega}{2\pi \sigma^2 \sin^2(\omega /2)}
> \frac{2b^2}{\pi \omega \sigma^2}
= \frac{b^2}{2\sigma^2} \cdot \frac{4}{\pi\omega}
\]
信噪比至少增大到\(4/(\pi\omega)\)倍，
\(\omega\)越小信噪比提高越多。
\(\omega\)越小，\(M\)应越大。

演示：

```
demo.mafilt <- function(b=3, M=3){
  n <- 100
  ##om <- pi/7
  om <- pi/12
  sigma <- 1.0
  eps <- rnorm(n, 0, sigma)
  sn0 <- b^2 / (2*sigma^2)
  tt <- seq(n)
  u <- runif(1)
  signal <- b * cos(om*tt + 2*pi*u)
  y <- signal + eps
  filt <- rep(1/(2*M+1), 2*M+1)
  yf <- filter(y, filt, method="convolution")
  rg <- range(c(y, yf))
  sn <- round(b^2 / (2*sigma^2), 3)
  plot(tt, y, main=paste("MA filter: SN=", sn, sep=""),
       type="l", xlab='time', ylab='y',
       xlim=c(0,120),
       ylim=c(-b*1.5,b*1.5))
  lines(tt, signal, col="green", lwd=2)
  lines(tt, yf, col="red", lwd=2)
  legend("topright", lty=c(1,1,1), lwd=c(1,2,2),
         col=c("black", "green", "red"),
         legend=c("序列", "信号", "滤波"))
}
demo.mafilt()
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/1015-linser_files/figure-html/linser-filt-mafilt01-1.png)

○○○○○○

## 2.4 附录：补充证明

### 2.4.1 不独立的白噪声示例

考虑如下的模型。
设\(\{ \varepsilon\_t \}\)为iid 标准正态分布随机变量列，
\(\alpha\_0>0\),
\(0 < \alpha\_1 < 1\)。
令
\[
a\_t = \sigma\_t \varepsilon\_t, \quad
\sigma\_t^2 = \alpha\_0 + \alpha\_1 a\_{t-1}^2
\]

令\(\mathscr F\_t = \sigma\{ \varepsilon\_t, \varepsilon\_{t-1}, \dots \}\)，
则\(\varepsilon\_t \in \mathscr F\_t\),
\(\varepsilon\_t\)与\(\mathscr F\_{t-1}\)独立。
应可证明\(\sigma\_t \in \mathscr F\_{t-1}\),
\(a\_t \in \mathscr F\_t\),
\(\mathscr F\_t = \sigma \{ a\_t, a\_{t-1}, \dots \}\)
(严格证明?)

于是
\[\begin{aligned}
E a\_t =& E[ E(\sigma\_t \varepsilon\_t | \mathscr F\_{t-1}) ] \\
=& E[ \sigma\_t E( \varepsilon\_t | \mathscr F\_{t-1}) ] \\
=& E[ \sigma\_t E(\varepsilon\_t )] = 0
\end{aligned}\]

对\(k=1,2,\dots\)，
\[\begin{aligned}
E (a\_t a\_{t-k}) =& E[ E(\sigma\_t \sigma\_{t-k} \varepsilon\_t \varepsilon\_{t-k} | \mathscr F\_{t-1}) ] \\
=& E[ \sigma\_t \sigma\_{t-k} \varepsilon\_{t-k} E( \varepsilon\_t | \mathscr F\_{t-1}) ] \\
=& E[ \sigma\_t \sigma\_{t-k} \varepsilon\_{t-k} E(\varepsilon\_t )] = 0
\end{aligned}\]

所以\(\{ a\_t \}\)是不相关列，
而
\[\begin{aligned}
\text{Var}(a\_t | a\_{t-1}, a\_{t-2}, \dots)
=& E(a\_t^2 | a\_{t-1}, a\_{t-2}, \dots) \\
=& E(\sigma\_t^2 \varepsilon\_t^2 | a\_{t-1}, a\_{t-2}, \dots) \\
=& \sigma\_t^2 E ( \varepsilon\_t^2 | a\_{t-1}, a\_{t-2}, \dots) \\
=& \sigma\_t^2 = \alpha\_0 + \alpha\_1 a\_{t-1}^2
\end{aligned}\]
可见\(a\_t\)与\(a\_{t-1}\)不独立。

如果模型中的\(\{ a\_t \}\)平稳，则\(E a\_t^2 = E a\_{t-1}^2\)，
有
\[\begin{aligned}
E(a\_t^2) =& E[ E(a\_t^2 | \mathscr F\_{t-1}) ] \\
=& E[ E(\sigma\_t^2 \varepsilon\_t^2 | \mathscr F\_{t-1}) ] \\
=& E[ \sigma\_t^2 E(\varepsilon\_t^2 | \mathscr F\_{t-1}) ] \\
=& E[ \sigma\_t^2 E(\varepsilon\_t^2) ] \\
=& E[ \sigma\_t^2 ]
= \alpha\_0 + \alpha\_1 E a\_{t-1}^2
\end{aligned}\]
解得
\[
\text{Var}(a\_t) = E(a\_t^2) = \frac{\alpha\_0}{1 - \alpha\_1}
\]
但是\(a\_t\)平稳的证明不显然。

### 2.4.2 三角级数求和的简化推导

\[\begin{aligned}
& \sum\_{j=-M}^M b \cos(\omega (t-j) + U) \\
=& \Re\left\{ \sum\_{j=-M}^M b \exp\{ i [\omega(t-j) + U] \} \right\} \\
=& \Re\left\{ b e^{i(\omega t + U)} \sum\_{j=-M}^M e^{-i \omega j} \right\}
\end{aligned}\]
其中
\[\begin{aligned}
& \sum\_{j=-M}^M e^{-i \omega j}
= \frac{e^{i\omega M} - e^{-i \omega(M+1)}}{1 - e^{-i \omega}} \\
=& \frac{\left( e^{i\omega M} - e^{-i \omega(M+1)} \right)
(1 - e^{i \omega})}{ | 1 - e^{-i \omega} |^2 } \\
=& \frac{e^{i\omega M} - e^{-i\omega(M+1)} - e^{i\omega(M+1)} + e^{-i\omega M}}
{(1 + \cos\omega)^2 + \sin^2 \omega} \\
=& \frac{2 \cos(M\omega) - 2 \cos[(M+1)\omega]}{2 + 2 \cos\omega} \\
=& \frac{-4 \cos[(2M+1)\omega/2] \sin(-\frac12 \omega)}
{e \sin^2 \frac{\omega}{2}} \\
=& \frac{\sin[(M + \frac12)\omega]}{ \sin\frac{\omega}{2}}
\end{aligned}\]
所以
\[\begin{aligned}
& \sum\_{j=-M}^M b \cos(\omega (t-j) + U)
= b\cos(\omega t + U)
\frac{\sin[(M + \frac12)\omega]}{ \sin\frac{\omega}{2}}
\end{aligned}\]