---
crawl_time: '2026-01-17 14:28:44'
framework: sphinx
title: 14 广义ARMA模型和ARIMA模型介绍 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 14 广义ARMA模型和ARIMA模型介绍

## 14.1 广义ARMA模型

设\(A(z)=1-\sum^{p}\_{j=1}a\_j z^j\),
\(B(z)=1+\sum^{q}\_{j=1}b\_jz^j\)
是两个没有公共根的实系数多项式,
\(a\_p b\_q \neq 0\), \(\{\varepsilon\_t\}\) 是WN\((0,\sigma^2)\).
如果不对\(A(z)\), \(B(z)\)的根做任何其他限制,
则称差分方程：
\[\begin{align}
A(\mathscr B)X\_t=B(\mathscr B)\varepsilon\_t, \ \ t \in \mathbb Z,
\tag{14.1}
\end{align}\]
为**广义ARMA\((p,q)\)模型**,
称满足[(14.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-genarmaeq0301)的\(\{X\_t\}\)为广义ARMA\((p,q)\)序列.

如果\(A(z)\)在单位圆上有根, 可以证明[(14.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-genarmaeq0301)没有平稳解.

如果\(A(z)\)在单位圆上没有根,
则有\(0 < \rho\_1<1<\rho\_2\)使得复变函数\(B(z)/A(z)\)在圆环
\[\begin{align}
D=\{z : \ \rho\_1 \leq |z| \leq \rho\_2\}
\tag{14.2}
\end{align}\]
内解析.
于是\(B(z)/A(z)\)有Laurent级数展开
\[\begin{align}
A^{-1}(z)B(z) = \sum\_{j=-\infty}^\infty c\_j z^j, \ z\in D ,
\tag{14.3}
\end{align}\]
存在\(\rho > 1\)使\(c\_j = o(\rho^{-|j|}), j \to \pm \infty\)。

这样, 从[(14.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-genarmaeq0301)可以得到惟一的平稳解
\[\begin{align}
X\_t=A^{-1}(\mathscr B)B(\mathscr B)\varepsilon\_t= \sum\_{j=-\infty}^{\infty}c\_j \varepsilon\_{t-j},
\ \ t\in \mathbb Z.
\tag{14.4}
\end{align}\]

如果\(A(z)\)在单位圆内有根,
由[(14.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-genarma-stasolu0304)定义的平稳序列是白噪声的双边无穷滑动和。
这个平稳序列某种意义下是不合理的,
因为\(t\)时的观测受到了\(t\)以后的干扰的影响.

再由差分方程的理论知道,
这时[(14.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-genarmaeq0301)的其他解都随着时间的增加而加速振荡.
为此, 人们把这时的广义ARMA模型称为“爆炸模型”。

## 14.2 ARIMA\((p,d,q)\)模型

ARIMA\((p,d,q)\)模型是AR部分有单位特征根（即1）的广义ARMA模型，
\(d\)为单位特征根的重数。
除了单位根以外，要求AR部分根都在单位圆外，MA部分单位圆内没有根。

ARIMA\((p,d,q)\)序列是\(d\)阶差分后服从ARMA(\(p,q\))模型的非平稳时间序列。
设\(d\)是一个正整数, 如果
\[\begin{align}
Y\_t = (1-\mathscr B)^d X\_t =
\sum\_{k=0}^d C\_{d}^k (-1)^k X\_{t-k}, \ \ t\in \mathbb Z,
\tag{14.5}
\end{align}\]
是一个ARMA\((p,q)\)序列,
其模型MA部分的特征多项式没有等于1的特征根，
则称\(\{X\_t\}\)是一个求和自回归滑动平均\((p,d,q)\)序列.
简称为ARIMA\((p,d,q)\)序列,  
其中\(C\_{d}^k\)是二项式系数.

**例14.1** ARIMA(\(p,1,q\))。

这时\(d=1\),
\[\begin{aligned}
Y\_t=(1-\mathscr B)X\_t=X\_t-X\_{t-1}
\end{aligned}\]
为一个ARMA\((p,q)\) 序列.

给定初值\(X\_0\)后, 有
\[\begin{align}
X\_t & = X\_{t-1} + Y\_t\\
& = X\_{t-2} + Y\_{t-1} + Y\_t\\
& = \cdots \\
& = X\_0 + Y\_1 + Y\_2 + \cdots + Y\_t\\
& = X\_0 + \sum^{t}\_{j=1} Y\_j, \ t \geq 1.
\tag{14.6}
\end{align}\]
[(14.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-arima-diff1-0307)是
\[\begin{aligned}
(1 - \mathscr B) X\_t = Y\_t
\end{aligned}\]
的解，也是其通解。

如果\(\{ Y\_t \}\)是相应的ARMA(\(p,q\))模型的通解，
则[(14.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-arima-diff1-0307)也是相应的ARIMA(\(p,1,q\))模型的通解。
所以,求和ARIMA\((p,1,q)\)序列不是平稳序列.

对正整数\(d\), 求和ARIMA\((p,d,q)\)序列也都不是平稳序列.

○○○○○○

**例14.2** ARIMA(\(p,2,q\))。

这时\(d=2\),
\[\begin{aligned}
Y\_t=(1-\mathscr B)^2 X\_t=X\_t - 2X\_{t-1} + X\_{t-2}
\end{aligned}\]
为一个ARMA\((p,q)\) 序列.

给定初值\(X\_{-1}, X\_0\)后, 有
\[\begin{aligned}
X\_t - X\_{t-1} = X\_{t-1} - X\_{t-2} + Y\_t,\ t \geq 1
\end{aligned}\]
两边对\(t=1,2,\dots,n\_1\)求和得
\[\begin{aligned}
X\_{n\_1} - X\_0 = X\_{n\_1-1} - X\_{-1} + \sum\_{i=1}^{n\_1} Y\_i
\end{aligned}\]
整理后得
\[\begin{aligned}
X\_{n\_1} - X\_{n\_1-1} = X\_0 - X\_{-1} + \sum\_{i=1}^{n\_1} Y\_i
\end{aligned}\]

两边再对\(n\_1=1,2,\dots,t\)求和，得
\[\begin{aligned}
X\_t - X\_0 = t(X\_0 - X\_{-1}) + \sum\_{n\_1=1}^t \sum\_{i=1}^{n\_1} Y\_i, \ t \geq q
\end{aligned}\]
或
\[\begin{align}
X\_t =& X\_0 + t(X\_0 - X\_{-1}) + \sum\_{n\_1=1}^t \sum\_{i=1}^{n\_1} Y\_i, \ t \geq q
\tag{14.7}
\end{align}\]

○○○○○○

一般地，设\(\{X\_t\}\)为ARIMA(\(p,d,q\))序列，\((1-\mathscr B)^d X\_t = Y\_t\)，
模型通解为
\[\begin{aligned}
X\_t = C\_0 + C\_1 t + \dots + C\_{d-1} t^{d-1}
+ \sum\_{n\_{d-1}=1}^t \dots \sum\_{n\_1=1}^{n\_2} \sum\_{j=1}^{n\_1} Y\_j.
\end{aligned}\]

实际问题中有许多数据经过一或两次差分后会稳定下来.
差分运算是对数据进行预处理的常用方法之一.

## 14.3 单位根过程

ARIMA(\(p,1,q\))模型称为单位根过程，相应的时间序列被称为单位根序列。

单位根过程与有一个AR部分特征根\(|z\_j|>1\)
但\(|z\_j|\)十分接近于1的平稳ARMA序列很难区分。

单位根过程与带有线性趋势的模型不同。
单位根过程数据没有固定走势，可以称为随机趋势。
带有线性趋势的模型减去线性趋势后就平稳了，
单位根过程减去任何线性趋势都不平稳。

ARIMA(\(0, 1, 0\))模型是最简单的单位根过程，
也可以称为随机游动。
随机游动\(p\_t = p\_{t+1} + \varepsilon\_t\)与固定趋势加扰动
\(Y\_t = a + bt + X\_t\)(其中\(\{ X\_t \}\)平稳)都能呈现出缓慢的趋势变化。
区别在于：

* 随机游动的方差是线性增长的，
  固定趋势的观测值方差不变；
* 随机游动的扰动的影响是永久的，
  固定趋势的扰动的影响仅在一个时刻（如果扰动\(X\_t\)是白噪声）
  或者很短时间（如果是扰动\(X\_t\)是线性时间序列）；
* 随机游动的趋势没有固定方向，
  固定趋势的变化形状是固定的；
* 固定趋势模型\(Y\_t\)减去一个固定的回归函数\(Y = a + bt\)就可以变成平稳列，
  随机游动减去任意的非随机函数都不能变平稳，
  可以用差分运算变成平稳。

下面用随机模拟方法演示随机游动（单位根过程）与固定趋势模型。
设随机游动模型为
\[
Y\_t = Y\_{t-1} + \varepsilon\_t, \ \varepsilon\_t \text{iid N}(0,1)
\]
固定趋势模型为
\[
Y\_t = 10 + 0.01 t + \varepsilon\_t, \ \varepsilon\_t \text{iid N}(0,1)
\]

随机游动的模拟：

```
set.seed(3)
plot(cumsum(rnorm(500)), type="l")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3018-arima_files/figure-html/arima-urdemo-01-1.png)

```
set.seed(4)
plot(cumsum(rnorm(500)), type="l")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3018-arima_files/figure-html/arima-urdemo-01-2.png)

```
set.seed(9)
plot(cumsum(rnorm(500)), type="l")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3018-arima_files/figure-html/arima-urdemo-01-3.png)

单位根过程的水平是随机变化的，也没有固定的方向。

固定趋势模型的模拟：

```
set.seed(1)
plot(10 + 0.01*(1:500) + rnorm(500), type="l")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3018-arima_files/figure-html/arima-urdemo-02-1.png)

固定趋势模型的水平是有规律的。

## 14.4 分数差分ARFIMA(\(p,d,q\))模型

ARMA序列自协方差函数负指数衰减，是短记忆的。
离散谱序列自协方差不衰减到0，是长记忆的。

其它的平稳序列如何区分长记忆还是短记忆？若存在\(d<\frac12\), 使得
\[\begin{aligned}
\gamma\_k \sim k^{2d-1}, \ k\to \infty
\end{aligned}\]
则称相应的序列为**长记忆序列**。
即\(\gamma\_k \sim \frac{1}{k^\alpha}, \alpha>0\),
\(\gamma\_k\)以幂函数速度趋于零。

对于\(d\neq 0\) , \(d\in (-0.5,0.5)\), \((1-z)^{-d}\)有Taylor展开公式
\[\begin{align}
(1-z)^{-d} = \sum\_{j=0}^\infty \pi\_j z^j, \ |z|\leq 1,
\tag{14.8}
\end{align}\]
其中
\[\begin{aligned}
\pi\_j = \frac{\Gamma(j+d)}{\Gamma(d) \Gamma(j+1)}
= \frac{d(d+1)\dots (d+j-1)}{j!},
\quad j=0,1,\dots
\end{aligned}\]
用Stirling公式
\[\begin{aligned}
\Gamma(x) \sim \sqrt{2\pi} e^{1-x} (x-1)^{x-0.5}, \ x\to+\infty
\end{aligned}\]
可证明
\[\begin{aligned}
\pi\_j \sim j^{d-1}, \ j\to+\infty
\end{aligned}\]
所以\(\{\pi\_j\}\)平方可和。

定义线性平稳序列
\[\begin{align}
X\_t=(1-\mathscr B)^{-d}\varepsilon\_t = \sum\_{j=0}^\infty \pi\_j \varepsilon\_{t-j},
\ \ t\in \mathbb Z.
\tag{14.9}
\end{align}\]
则\(\{X\_t\}\)是模型
\[\begin{align}
(1-\mathscr B)^dX\_t=\varepsilon\_t, \ \ t\in \mathbb Z,
\tag{14.10}
\end{align}\]
的惟一平稳解.
人们称[(14.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-arfima-mod0317)是ARFIMA\((0,d,0)\)模型.

谱密度为
\[\begin{align}
f(\lambda) =& \frac{\sigma^2}{2\pi}
\left| \sum\_{j=0}^\infty \pi\_j e^{ij\lambda} \right|^2
= \frac{\sigma^2}{2\pi}
\left| 1 - e^{i\lambda} \right|^{-2d} \\
=& \frac{\sigma^2}{2\pi}
\frac{1}{\left| 2 \sin(\lambda/2) \right|^{2d}},
\quad \lambda \in [-\pi,\pi]
\tag{14.11}
\end{align}\]

对ARFIMA(0,\(d\),0)序列,
\[\begin{align}
\gamma\_k =& \int\_{-\pi}^\pi e^{ik\lambda} f(\lambda)d\lambda
= \frac{\sigma^2}{\pi} \int\_0^\pi
\frac{\cos(k\lambda)}{[2\sin(\lambda/2)]^{2d}} d\lambda \\
=& \frac{\sigma^2\Gamma(1-2d)\Gamma(k+d)}
{\Gamma(k-d+1)\Gamma(d) \Gamma(1-d)}, \ k\in \mathbb N\_+
\tag{14.12} \\
\rho\_k =& \frac{\gamma\_k}{\gamma\_0}
= \frac{\Gamma(k+d) \Gamma(k-d)}
{\Gamma(k+1-d) \Gamma(d)}
\end{align}\]

对\(k=1\)有
\[\begin{aligned}
d = \frac{\rho\_1}{1 + \rho\_1}
\end{aligned}\]
由Sterling公式可证明
\[\begin{align}
\gamma\_k \sim k^{2d-1}
\frac{\sigma^2 \Gamma(1-2d)}{\Gamma(d)\Gamma(1-d)},
\ \hbox{ 当} \ k \to \infty.
\tag{14.13}
\end{align}\]
即长记忆。

当\(d \in (-0.5, 0)\)时
\[\begin{aligned}
\sum\_{k=0}^\infty |\gamma\_k| < \infty.
\end{aligned}\]

当\(d \in (0,0.5)\)时
\[\begin{aligned}
\sum\_{k=0}^\infty |\gamma\_k| = \infty.
\end{aligned}\]

用Levinson递推公式和归纳法可得ARFIMA(0,\(d\),0)的偏相关函数满足
\[\begin{aligned}
a\_{k,k} = \frac{d}{k-d}, k=1,2,\dots
\end{aligned}\]
也可以用于\(d\)的估计。

类似可定义ARFIMA(\(p,d,q\))模型。
一般ARFIMA(\(p,d,q\))的讨论略过。

## 14.5 附录：ARIMA不平稳证明

ARIMA(\(p,d,q\))模型没有平稳解。

当\(d=1\)时，
问题为，\(\{\xi\_t \}\)是ARMA(\(p,q\))序列,
MA部分的特征多项式没有等于1的根，
\(\{ X\_t \}\)满足
\[\begin{align}
X\_t - X\_{t-1} = \xi\_t
\tag{14.14}
\end{align}\]
来证明[(14.14)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arima.html#eq:arima-app-nonsta-diff1)没有平稳解。
这个结论不能推广到对任意的平稳列\(\{\xi\_t \}\)都成立，
因为取\(\{ X\_t \}\)为白噪声列，\(\xi\_t = X\_t - X\_{t-1}\)是一个
MA(1)序列，这时\(\{ X\_t \}\)平稳。

归纳地，如果结论对\(d=1\)成立，则\(d=2\)时，
若\(X\_t\)是如下ARIMA(\(p,2,q\))的平稳解：
\[\begin{aligned}
A(\mathscr B)(1-\mathscr B)^2 X\_t = B(\mathscr B) \varepsilon\_t
\end{aligned}\]
令\(Y\_t = X\_t - X\_{t-1}\)，则\(Y\_t\)是如下ARIMA(\(p,1,q\))的平稳解：
\[\begin{aligned}
A(\mathscr B)(1-\mathscr B) Y\_t = B(\mathscr B) \varepsilon\_t
\end{aligned}\]
由归纳法假设这不可能。
所以只要证明ARIMA(\(p,1,q\))没有平稳解则
ARIMA(\(p,d,q\))都没有平稳解。

**情形1**:

当\(\{\xi\_t \}\)为WN(\(0,\sigma^2\))时，
因
\[\begin{aligned}
X\_t - X\_0 = \sum\_{k=1}^t \xi\_t
\end{aligned}\]
所以
\[\begin{aligned}
\text{Var}(X\_t - X\_0) =& t \sigma^2
\end{aligned}\]
因此\(\{ X\_t \}\)不平稳。

**情形2**:

设\(X\_t\)满足模型
\[
A(\mathscr B) X\_t = \varepsilon\_t
\]
其中\(\{ \varepsilon\_t \}\)为白噪声列，
\(A(1)=0\)，
则可分解\(A(z) = (1 - z)A\_1(z)\)，
若有平稳列\(X\_t\)使得\(A(\mathscr B) X\_t = \varepsilon\_t\)，
令\(Y\_t = A\_1(\mathscr B) X\_t\)，
\(Y\_t\)也平稳，且\(Y\_t - Y\_{t-1} = \varepsilon\_t\)，
由情形1推出矛盾。

**情形3**

当\(\{\xi\_t \}\)是可逆ARMA(\(p,q\))序列时,
设其自协方差函数为\(\{\gamma\_k\}\)，
这时\(\{\xi\_t \}\)有连续谱密度
\(f(\lambda), \lambda \in [-\pi,\pi]\)，
且有正下界\(c>0\)。
于是
\[\begin{aligned}
&\text{Var}(X\_t - X\_0) = \text{Var}(\sum\_{k=1}^t \xi\_t)
= \sum\_{k=1}^t \sum\_{j=1}^t \gamma\_{k-j} \\
=& \sum\_{k=1}^t \sum\_{j=1}^t \int\_{-\pi}^{\pi}
e^{i(k-j)\lambda} f(\lambda) d\lambda
= \int\_{-\pi}^{\pi} \left| \sum\_{k=1}^t e^{ik\lambda} \right|^2
f(\lambda) d\lambda \\
\geq& c \int\_{-\pi}^{\pi} \left| \sum\_{k=1}^t e^{ik\lambda} \right|^2
d\lambda
= c \sum\_{k=1}^t \sum\_{j=1}^t \int\_{-\pi}^{\pi}
e^{i(k-j)\lambda} d\lambda \\
=& c \sum\_{k=1}^t 2\pi \quad\text{(仅$k-j=0$时积分不为零)}
= 2\pi c t
\end{aligned}\]
所以\(\{ X\_t \}\)不平稳。

**情形4**

当\(\{ \xi\_t \}\)为一般的ARMA(\(p,q\))时：
\[\begin{aligned}
A(\mathscr B) \xi\_t = B(\mathscr B) \varepsilon\_t
\end{aligned}\]
设其自协方差函数为\(\{\gamma\_k\}\)，
这时\(\{X\_t \}\)满足如下广义ARMA模型：
\[\begin{aligned}
A(\mathscr B)(1-\mathscr B) X\_t = B(\mathscr B) \varepsilon\_t
\end{aligned}\]
按问题条件知\(B(1) \neq 0\)。
于是
\[\begin{aligned}
&\text{Var}(X\_t - X\_0) = \text{Var}(\sum\_{k=1}^t \xi\_t)
= \sum\_{k=1}^t \sum\_{j=1}^t \gamma\_{k-j} \\
=& \sum\_{k=1-t}^{t-1} (t - |k|) \gamma\_k \\
=& t \sum\_{k=1-t}^{t-1} \gamma\_k - \sum\_{k=1-t}^{t-1} |k| \gamma\_k
\end{aligned}\]
令\(t \to +\infty\), 注意到ARMA序列\(\{\xi\_t\}\)的自协方差函数\(\{\gamma\_k\}\)负指数衰减
所以\(\sum\_{k=-\infty}^\infty |k| \gamma\_k\)绝对收敛，而
\[\begin{aligned}
\sum\_{k=-\infty}^\infty \gamma\_k = 2\pi f(0)
= 2\pi \frac{|B(1)|^2}{|A(1)|^2} > 0
\end{aligned}\]
所以\(\text{Var}(X\_t - X\_0) \to +\infty, \ t \to +\infty\)，
\(\{ X\_t \}\)非平稳。

对于AR部分特征多项式单位圆上有复根的情况，
复根必共轭成对出现，且重数必为偶数重，
否则不可能组成实系数多项式。
这种情形的具体证明有待补充。