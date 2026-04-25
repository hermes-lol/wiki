---
crawl_time: '2026-01-17 14:29:07'
framework: sphinx
title: 29 条件异方差模型 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 29 条件异方差模型

## 29.1 资产收益率

设\(t\)为某个固定时间单位的个数（比如天数、月数、年数），
以天为例，
用\(P\_t\)表示某金融资产在第\(t\)天的价格。
令
\[
R\_t = \frac{P\_t - P\_{t-1}}{P\_{t-1}},
\]
称为第\(t\)天的**简单收益率**。
\(1 + R\_t\)称为毛收益率。
令
\[
r\_t = \log(1 + R\_t)
= \log \frac{P\_t}{P\_{t-1}}
= \log P\_t - \log P\_{t-1},
\]
称为第\(t\)天的**对数收益率**。

如果已知\(r\_1, \dots, r\_t\)，
则易见
\[
\frac{P\_t}{P\_0}
= \prod\_{j=1}^t \frac{P\_j}{P\_{j-1}}
= \prod\_{j=1}^t (1 + R\_j),
\]
而
\[
\log \frac{P\_t}{P\_0}
= \sum\_{j=1}^t \log \frac{P\_j}{P\_{j-1}}
= \sum\_{j=1}^t r\_j,
\]
所以对数收益率更容易进行数学推导。

## 29.2 ARCH模型

对于资产收益率序列\(\{r\_t \}\)，
如果它是高斯过程，
则最优线性预测就是最优预测，
通常只要建立一个ARMA这样的线性模型就足够了。
但是，
在金融市场中，
资产收益率常常不是正态分布的，
而且其条件分布\(X\_t | \mathscr F\_{t-1}\)的条件方差不是恒定的，
会随时间\(t\)变化，
金融资产的收益率会有“波动率聚集”现象，
即某一段时间的\(r\_t\)波动较大，
而另一端时间的\(r\_t\)波动较小，
所以有必要在对条件期望\(E(r\_t | \mathscr F\_{t-1})\)建模的同时对条件方差\(\text{Var}(r\_t | \mathscr F\_{t-1})\)建模。

设\(\{ \varepsilon\_t \}\)是对条件均值建模后的残差，
理论上，
这相当于Wold分解中的新息，
设其满足
\[
E(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)
= 0 .
\]
\(\{ \varepsilon\_t \}\)是二阶平稳的宽白噪声，
但这并不要求\(\{ \varepsilon\_t \}\)独立，
我们可以考虑能够表现波动率聚集的模型。
因为\(\varepsilon\_t^2\)代表了波动大小，
我们尝试对其建立如下的AR(1)模型：
\[
\varepsilon\_t^2
= \alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2 + \eta\_t,
\]
其中\(\alpha\_0 > 0\), \(\alpha\_1 \geq 0\)，
\(\{\eta\_t \}\)是独立同分布零均值白噪声列。
这样的模型中\(\varepsilon\_t^2\)和\(\varepsilon\_{t-1}^2\)是正相关的，
所以能够体现波动率聚集性质。
设\(E\varepsilon\_t^2 = \sigma^2\)，
则
\[
\sigma^2 = \alpha\_0 + \alpha\_1 \sigma^2 + 0,
\]
将\(\varepsilon\_t^2\)中心化，
得
\[
(\varepsilon\_t^2 - \sigma^2)
= \alpha\_1 (\varepsilon\_{t-1}^2 - \sigma^2) + \eta\_t,
\]
可见\(\varepsilon\_t^2 - \sigma^2\)满足一个AR(1)模型，
其中\(0 \leq \alpha\_1 < 1\)。
记\(A(z) = 1 - \alpha\_1 z\)，
则
\[
A(\mathscr B)(\varepsilon\_t^2 - \sigma^2)
= \eta\_t,
\]
所以\(\varepsilon\_t^2\)有平稳解
\[
\varepsilon\_t^2
= \sigma^2
+ A^{-1}(\mathscr B) \eta\_t
= \sigma^2
+ \sum\_{j=0}^\infty \alpha\_1^j \eta\_{t-j} .
\]
由此平稳解可见\(\eta\_t\)与\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots \}\)独立，
从而
\[
\sigma\_t^2
= \text{Var}(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)
= E(\varepsilon\_t^2 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)
= \alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2 .
\]
这就给出了条件方差的一个模型。
进一步地可以设
\[
\sigma\_t^2
= \alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2 .
\]
注意上式中已经没有\(\eta\_t\)。

显然，
这个模型中的系数应使得\(\alpha\_0 > 0\),
\(\alpha\_j \geq 0\), \(j=1,2,\dots,p\)，
且多项式\(A(z) = 1 - \alpha\_1 z - \dots - \alpha\_p z^p\)满足最小相位性。
注意对应的AR(\(p\))模型不能保证\(\varepsilon\_t^2\)非负，
但有下面的ARCH模型定义和下一节的平稳解结果。

**定义29.1 (ARCH模型)** 设\(\{v\_t \}\)是独立同分布零均值标准白噪声WN(0,1)，
非负常数\(\alpha\_0, \alpha\_1, \dots, \alpha\_p\)满足\(\alpha\_1 + \dots + \alpha\_p < 1\)且\(\alpha\_0>0\), \(\alpha\_p>0\)，
则如下模型
\[\begin{equation}
\begin{cases}
\varepsilon\_t = [\alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2 ]^{1/2} v\_t, \\
v\_t \text{与} \{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots \}
\text{相互独立},
\end{cases}
\tag{29.1}
\end{equation}\]
称为\(\text{ARCH}(p)\)模型。
如果\(\{\varepsilon\_t \}\)是严平稳白噪声且满足上述模型，
则称其为\(\text{ARCH}(p)\)序列。

记
\[
\sigma\_t
= [\alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2 ]^{1/2} .
\]
则模型[(29.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archmod1)可以表述为
\[\begin{equation}
\begin{cases}
\varepsilon\_t = \sigma\_t v\_t, \\
\sigma\_t
= [\alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2 ]^{1/2}, \\
v\_t \text{与} \{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots \}
\text{相互独立} .
\end{cases}
\tag{29.2}
\end{equation}\]

对ARCH(\(p\))序列，
来证明
\[
\sigma\_t^2
= \alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2
= \text{Var}(\varepsilon\_t
| \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots) .
\]
记\(\mathscr F\_{t-1} = \sigma(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots\})\)，
则\(v\_t\)与\(\mathscr F\_{t-1}\)独立而\(\sigma\_t^2\)关于\(\mathscr F\_{t-1}\)可测，
故\(v\_t\)与\(\sigma\_t\)独立。
于是，
\[\begin{aligned}
& \text{Var}(\varepsilon\_t | \mathscr F\_{t-1})
= E(\varepsilon\_t^2 | \mathscr F\_{t-1}) \\
=& E \left\{
\sigma\_t^2
v\_t^2
| \mathscr F\_{t-1}\right\}
= \sigma\_t^2
E(v\_t^2 | \mathscr F\_{t-1}) \\
=& \sigma\_t^2 E(v\_t^2)
= \sigma\_t^2 .
\end{aligned}\]

对ARCH(\(p\))序列，
设\(Ev\_t^4 < \infty\)，
\(E\varepsilon\_t^4 < \infty\)，
来证明\(\{ \varepsilon\_t^2 \}\)满足如下AR(\(p\))模型：
\[
(\varepsilon\_t^2 - \sigma^2)
= \alpha\_1 (\varepsilon\_{t-1}^2 - \sigma^2)
+ \cdots
+ \alpha\_p (\varepsilon\_{t-p}^2 - \sigma^2)
+ \eta\_t,
\]
其中\(\eta\_t\)为独立同分布零均值白噪声列。
事实上，
令
\[
\eta\_t = \varepsilon\_t^2 - \sigma\_t^2
= \sigma\_t^2(v\_t^2 - 1) ,
\]
注意\(v\_t\)与\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots\}\)独立，
\(v\_t\)与\(\sigma\_t\)独立，
所以有
\[\begin{aligned}
E(\eta\_t)
=& E(\sigma\_t^2 (v\_t^2 - 1))
= E(\sigma\_t^2) E(v\_t^2 - 1) = 0, \\
E(\eta\_t \eta\_{t+k})
=& E[\sigma\_t^2 (v\_t^2 - 1) \sigma\_{t+k}^2 (v\_{t+k}^2 - 1)]
= E(\sigma\_t (v\_t^2 - 1) \sigma\_{t+k}) E(v\_{t+k}^2 - 1)
= 0, \\
E(\eta\_t^2)
=& E[\sigma\_t^4(v\_t^2 - 1)^2]
= E[\sigma\_t^4] E[(v\_t^2 - 1)^2]
= E(\sigma\_1^4) E[(v\_1^2 - 1)^2] .
\end{aligned}\]
这里利用了\(\{\varepsilon\_t \}\)严平稳，
所以\(E\sigma\_t^4\)不依赖于\(t\)。
上式说明了\(\{\eta\_t \}\)是零均值白噪声列，
从而
\[
\varepsilon\_t^2
= \sigma\_t^2 + \eta\_t
= \alpha\_0 + \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2
+ \eta\_t,
\]
这是一个AR(\(p\))模型。

## 29.3 ARCH模型平稳解

**引理29.1** 设\(\alpha\_0, \alpha\_1, \dots\)是非负常数，
\(\{u\_t \}\)是独立同分布非负随机变量序列，
\(E(u\_t)=1\)。
如果\(\alpha\_0 > 0\)，
\(c = \sum\_{j=1}^\infty \alpha\_j < 1\)，
则有以下结论：

（1）有唯一的严平稳遍历序列\(\{Y\_t \}\)满足模型
\[\begin{equation}
\begin{cases}
Y\_t = \sigma\_t^2 u\_t,
\ \text{其中}
\sigma\_t^2 = \alpha\_0 + \sum\_{j=1}^\infty \alpha\_j Y\_{t-j} . \\
E(Y\_t) < \infty,
\ \{Y\_{t-1}, Y\_{t-2}, \dots\} \text{与} u\_t \text{独立} .
\end{cases}
\tag{29.3}
\end{equation}\]

（2）满足[(29.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-lem1)的严平稳遍历序列可表示成
\[\begin{equation}
Y\_t = \alpha\_0 \sum\_{n=1}^\infty A\_t(n),
\ E(Y\_t) = \frac{\alpha\_0}{1 - c},
\ t \in \mathbb N .
\tag{29.4}
\end{equation}\]
其中\(A\_t(0) = u\_t\)，对\(n \geq 1\)，有
\[\begin{equation}
A\_t(n)
= \sum\_{i\_1, i\_2, \dots, i\_n \geq 1}
\alpha\_{i\_1} \alpha\_{i\_2} \dots \alpha\_{i\_n}
u\_t u\_{t-i\_1} u\_{t - (i\_1 + i\_2)} \dots u\_{t - (i\_1 + \dots + i\_n)} ;
\tag{29.5}
\end{equation}\]
若进一步假设\(c \sqrt{E(u\_t^2)} < 1\)，
则\(E(Y\_t^2) < \alpha\_0 E(u\_t^2) / (1 - c \sqrt{E(u\_t^2)})^2\).

参见Fan J Q, Yao Q W.
Nonlinear time series: non-parametric and parametric methods.
Springer-Verlag. 2005.
证明见([何书元 2023](#ref-HeSY:TSA2023))附录1.5.

**定理29.1** 对于ARCH模型[(29.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archmod2),
设\(c = \sum\_{j=1}^p \alpha\_j\),
\(u\_t = v\_t^2\),
\(A\_t(n)\)按[(29.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-lem3)定义，
其中当\(j>p\)时\(\alpha\_j = 0\)。则

（1）存在严平稳遍历白噪声\(\{\varepsilon\_t\}\)满足ARCH模型[(29.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archmod2),
其中
\[\begin{equation}
\varepsilon\_t
= \left[ \alpha\_0 \sum\_{n=0}^\infty A\_t(n) \right]^{1/2}
u\_t,
\ E \varepsilon\_t = 0,
\ E \varepsilon\_t^2 = \frac{\alpha\_0}{1 - c} ;
\tag{29.6}
\end{equation}\]

（2）如果\(\{e\_t \}\)也是模型[(29.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archmod2)的严平稳解，
则\(e\_t^2 = \varepsilon\_t^2\), a.s.;

（3）如果\(c \sqrt{E(v\_1^4)} < 1\),
则
\[
E(\varepsilon\_t^4)
< \alpha\_0^2 E(v\_1^4) / \left[1 - c \sqrt{E(v\_1^4)} \right]^2 .
\]

**证明**：（1）取\(u\_t = v\_t^2\)，
则\(\{u\_t \}\)是独立同分布非负随机变量序列且\(E(u\_t)=1\)。
令\(Y\_t\)为[(29.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-lem2)的定义，
则\(\{Y\_t \}\)符合引理[29.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#lem:atsa-arch-archso-lem)中的结论(1)，
即
\[\begin{equation}
\begin{cases}
Y\_t = \left(\alpha\_0 + \sum\_{j=1}^p \alpha\_j Y\_{t-j} \right) u\_t, \\
E(Y\_t) < \infty,
\quad u\_t \text{与} \{Y\_{t-1}, Y\_{t-2}, \dots\} \text{独立} .
\end{cases}
\tag{29.7}
\end{equation}\]
取
\[\begin{equation}
\varepsilon\_t
= \left(\alpha\_0 + \sum\_{j=1}^p \alpha\_j Y\_{t-j} \right)^{1/2}
v\_t,
\tag{29.8}
\end{equation}\]
则\(\varepsilon\_t^2 = Y\_t\)，
从而可知\(E(\varepsilon\_t^2)<\infty\)。
由定理[4.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/strict-stationary.html#thm:strict-erg)可知\(\{\varepsilon\_t \}\)是严平稳遍历序列。
将[(29.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-thmved)中的\(Y\_t\)替换成\(\varepsilon\_t^2\)，
可知\(\{\varepsilon\_t\}\)满足如下模型
\[
\varepsilon\_t
= \left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{t-j}^2 \right)^{1/2}
v\_t .
\]

根据\(Y\_t\)的定义可以看出\(v\_t\)与\(\{Y\_{t-1}, Y\_{t-2}, \dots\}\)独立，
而\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots\}\)仅依赖于\(\{v\_{t-1}, Y\_{t-1}, Y\_{t-2}, \dots\}\)，
所以\(v\_t\)与\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots\}\)独立。

只要证明\(\{\varepsilon\_t\}\)是白噪声列。
前面已证明\(E(\varepsilon\_t^2)<\infty\)，于是
\[\begin{aligned}
E(\varepsilon\_t)
=& E \left[ \left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{t-j}^2 \right)^{1/2}
v\_t \right] \\
=& E \left[ \left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{t-j}^2 \right)^{1/2}
\right]
E(v\_t) \\
=& 0 . \\
E(\varepsilon\_t^2)
=& E(Y\_t)
= \frac{\alpha\_0}{1 - c}
= \frac{\alpha\_0}{1 - \sum\_{j=1}^p \alpha\_j} . \\
E(\varepsilon\_s \varepsilon\_t)
=& E \left[ \left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{s-j}^2 \right)^{1/2}
v\_s
\left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{t-j}^2 \right)^{1/2}
v\_t
\right] \\
=& E \left[ \left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{s-j}^2 \right)^{1/2}
v\_s
\left(\alpha\_0 + \sum\_{j=1}^p
\alpha\_j \varepsilon\_{t-j}^2 \right)^{1/2}
\right] E(v\_t) \\
=& 0 \quad(s < t) .
\end{aligned}\]
即\(\{\varepsilon\_t \}\)是零均值白噪声列，
且是严平稳遍历序列，
满足ARCH模型。

（2）如果\(\{e\_t \}\)也是模型[(29.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-lem2)的严平稳解，
则\(\{Y\_t = e\_t^2 \}\)是模型[(29.7)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-thmp1)的唯一严平稳解，
根据\(\varepsilon\_t\)定义可知\(e\_t^2 = \varepsilon\_t^2\)，a.s.

（3）
由引理[29.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#lem:atsa-arch-archso-lem)结论(3)，
若\(c \sqrt{E(v\_1^4)} < 1\),
即\(c \sqrt{E(u\_t^2)} < 1\), 则
\[
E(\varepsilon\_t^4)
= E(Y\_t^2)
< \alpha\_0^2 E(u\_t^2) / \left[ 1 - c \sqrt{E(u\_t^2)} \right]^2
= \alpha\_0^2 E(v\_1^4) / \left[ 1 - c \sqrt{E(v\_1^4)} \right]^2 .
\]

---

由定理[29.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#thm:atsa-arch-archso)结论(3)可知，
当\(c \sqrt{E(v\_1^4)} < 1\),
则\(E(\varepsilon\_t^4) < \infty\)，
根据上一节的讨论可知这时\(\{\varepsilon\_t^2 \}\)满足如下AR(\(p\))模型：
\[\begin{equation}
\varepsilon\_t^2
= \alpha\_0
+ \alpha\_1 \varepsilon\_{t-1}^2
+ \dots
+ \alpha\_p \varepsilon\_{t-p}^2
+ \eta\_t,
\tag{29.9}
\end{equation}\]
其中\(\eta\_t = \varepsilon\_t^2 - \sigma\_t^2 = \sigma\_t^2(v\_t^2 - 1)\)是零均值白噪声列,
\[
\text{Var}(\eta\_t)
= E(\eta\_t^2)
= E(\sigma\_t^4) E[(v\_t^2 - 1)^2] .
\]

对随机变量\(\xi\)，
若\(E(\xi^4) < \infty\)，
则定义
\[
\kappa\_{\xi}
= \frac{E(X- E(X))^4}{[\text{Var(X)}]^2},
\]
称\(\kappa\_{\xi}\)为\(\xi\)的**峰度**。
对正态分布的\(\xi\)，
有\(\kappa\_{\xi} = 3\)，
称\(\kappa\_{\xi} - 3\)为\(\xi\)的**超额峰度**。
这个指标度量了随机变量分布的厚尾性，
其样本的表现是异常值比正态分布更多。
ARCH模型主要用于金融资产收益率模型，
金融资产收益率常常体现出厚尾分布特性，
而ARCH模型一般是厚尾的。

**例29.1** 设\(\{\varepsilon\_t \}\)是ARCH模型[(29.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archmod2)的严平稳解，
\(\kappa\_{\varepsilon}\)是\(\varepsilon\_t\)的峰度，
\(\kappa\_v\)是\(v\_t\)的峰度，则：

（1）\(\kappa\_{\varepsilon} \geq \kappa\_v\)，
且等号成立当且仅当\(\sigma\_t^2\)为常数值，
即没有条件异方差性的情形；

（2）当\(E(v\_1^4) < 1\), \(E(\varepsilon\_t^4)<1\)时，
作为AR(\(p\))序列的\(\{\varepsilon\_t^2\}\)序列的自相关系数都是非负的；
特别地当\(\alpha\_j > 0\)时\(\rho\_{kj} > 0\), \(k=1,2,\dots\)。

**证明**：
（1）
\(\varepsilon\_t = \sigma\_t v\_t\)，
\(E(\varepsilon\_t) = 0\)，
\(E(v\_t^2) = 1\)，
\[
E(\varepsilon\_t^2)
= E(\sigma\_t^2 v\_t^2)
= E(\sigma\_t^2) E(v\_t^2)
= E(\sigma\_t^2).
\]
从而
\[\begin{aligned}
E(\varepsilon\_t^4)
=& E(\sigma\_t^4 v\_t^4)
= E(\sigma\_t^4) E(v\_t^4) \\
\geq& [E(\sigma\_t^2)]^2 E(v\_t^4), \\
\kappa\_{\varepsilon}
=& \frac{E(\varepsilon\_t^4)}{[E(\varepsilon\_t^2)]^2} \\
\geq& \frac{[E(\sigma\_t^2)]^2 E(v\_t^4)}{[E(\sigma\_t^2)]^2} \\
=& E(v\_t^4) = \kappa\_{v} .
\end{aligned}\]
等式成立的条件是等式\(E(\sigma\_t^4) = [E(\sigma\_t^2)]^2\)，
这当且仅当\(\sigma\_t^2 = c\), a.s.

（2）
这时\(E(v\_t^4)<\infty\)，\(E(\varepsilon\_t^4) < \infty\)，
所以\(\{\varepsilon\_t^2 \}\)满足AR(\(p\))模型[(29.9)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-archso-ar)，
令\(A(z) = 1 - \alpha\_1 z - \dots - \alpha\_p z^p\)，
有
\[
\varepsilon\_t^2 - E(\varepsilon\_t^2)
= A^{-1}(\mathscr B)(\eta\_t)
= \sum\_{j=0}^\infty c\_j \eta\_{t-j} .
\]
记\(s = \sum\_{j=1}^p \alpha\_j t^j\)，
则
\[
A^{-1}(t)
= \frac{1}{1 - s}
= \sum\_{k=0}^\infty s^k
= \sum\_{k=0}^\infty (\sum\_{j=1}^p \alpha\_j t^j)^k,
\ t \in (0, 1] .
\]
则\(\{c\_j \}\)满足
\[
\sum\_{j=0}^\infty c\_j t^j
= \sum\_{k=0}^\infty (\sum\_{j=1}^p \alpha\_j t^j)^k,
\ t \in (0, 1].
\]
可见\(c\_j \geq 0\), \(j=0,1,2,\dots\)，
故\(\{\varepsilon\_t^2 \}\)的协方差函数
\[
\gamma\_{\varepsilon\_t^2}(k)
= \sigma\_{\eta}^2 \sum\_{n=0}^\infty c\_n c\_{n+k} \geq 0 .
\]
即\(\{\varepsilon\_t^2 \}\)的相关系数都是非负的。
如果对某个\(j\_0\)满足\(\alpha\_{j\_0} > 0\)，
则\(\sum\_{k=0}^\infty (\sum\_{j=1}^p \alpha\_j t^j)^k\)中\(t^{k j\_0}\)系数为正值，
从而\(c\_{k j\_0} > 0\)，这时
\[\begin{aligned}
\gamma\_{\varepsilon\_t^2}(k j\_0)
\geq&
\sigma\_{\eta}^2
\sum\_{m=0}^\infty c\_{m j\_0} c\_{m j\_0 + k j\_0}
= \sigma\_{\eta}^2
\sum\_{m=0}^\infty c\_{m j\_0} c\_{(m+k) j\_0}
> 0 .
\end{aligned}\]

---

上面的例子说明ARCH模型对应的\(\{\varepsilon\_t \}\)序列通常是厚尾分布的，
而且\(\{\varepsilon\_t^2 \}\)都是正相关的。

如果金融收益率序列\(\{r\_t \}\)是平稳列，
有Wold分解，
其新息序列为\(\{\varepsilon\_t \}\)，
设其满足鞅差条件：
\[
E(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)
= 0,
\]
则可设\(\{\varepsilon\_t \}\)满足ARCH(\(p\))模型，
这个模型保证了波动率聚集效应（即\(\{\varepsilon\_t^2 \}\)序列相关性为正相关），
厚尾分布，
条件方差随时间变化，
称为ARCH效应。
对实际数据建模时，
可以对平稳数据先建立ARMA模型，
将这样的模型看做是关于条件期望\(E(r\_t | r\_{t-1}, r\_{t-2}, \dots)\)的模型，
然后将模型的残差看做是新息序列\(\{\varepsilon\_t \}\)，
可以通过检验\(\{\varepsilon\_t^2 \}\)是否白噪声列来检验是否具有ARCH效应，
如果有ARCH效应则对\(\{\varepsilon\_t \}\)建立ARCH(\(p\))模型，
作为条件方差\(\text{Var}(r\_t | r\_{t-1}, r\_{t-2}, \dots)\)的模型。

## 29.4 ARCH模型参数估计

办法是设定\(v\_t\)的分布，
称为“条件分布”，
然后定义似然函数，
进行最大似然估计。
对\(v\_t\)可以考虑使用正态分布、t分布等类型，
为了进行模型选择，
可以计算AIC准则值，
以及对拟合残差进行残差诊断。

## 29.5 GARCH模型

ARCH模型容易理解，
有严平稳遍历解，
但是在实际数据建模时，
往往需要比较高阶才能良好拟合。
为此，
类似于从AR模型推广到ARMA模型，
将ARCH模型推广为如下的GARCH模型（广义自回归条件异方差模型）。
仍设\(\{r\_t \}\)为平稳的资产收益率，
\(\{\varepsilon\_t \}\)为其新息，
满足\(E(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots) = 0\)。

**定义29.2 (GARCH模型)** 设\(\{v\_t \}\)为独立同分布WN(0,1)列，非负常数\(\alpha\_i\), \(\beta\_j\)满足条件
\[\begin{equation}
\sum\_{i=1}^p \alpha\_i + \sum\_{j=1}^q \beta\_j < 1,
\quad \alpha\_0 \alpha\_p \beta\_q > 0 .
\tag{29.10}
\end{equation}\]
称
\[\begin{equation}
\begin{cases}
\varepsilon\_t = \sigma\_t v\_t, \\
\sigma\_t^2 = \alpha\_0
+ \sum\_{i=1}^p \alpha\_i \varepsilon\_{t-i}^2
+ \sum\_{j=1}^q \beta\_j \sigma\_{t-j}^2, \\
v\_t \text{与} \{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots \} \text{独立} .
\end{cases}
\tag{29.11}
\end{equation}\]
为GARCH(\(p,q\))模型，
其中\(\sigma\_t^2 = \text{Var}(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)\)。
如果\(\{\varepsilon\_t \}\)是严平稳白噪声，
满足模型[(29.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def)，
则称\(\{\varepsilon\_t \}\)是GARCH(\(p,q\))序列。

下面讨论模型[(29.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def)的严平稳解存在性。
记\(h = \max(p,q)\)，
当\(i>p\)时定义\(\alpha\_i=0\),
当\(j>q\)时定义\(\beta\_j = 0\)。
定义两个多项式
\[
\alpha(t)
= \sum\_{i=1}^p \alpha\_i t^i,
\quad
\beta(t)
= \sum\_{j=1}^q \beta\_j t\_j ,
\ t \in [-1, 1] .
\]
由[(29.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def-coef)，
可以展开\(\frac{1}{1 - \beta(t)}\)为
\[
\frac{1}{1 - \beta(t)}
= \sum\_{k=0}^\infty [\beta(t)]^k,
\]
于是
\[\begin{equation}
[1 - \beta(t)]^{-1} \alpha(t)
= \alpha(t) \sum\_{k=0}^\infty [\beta(t)]^k
= \sum\_{n=1}^\infty c\_n t^n .
\tag{29.12}
\end{equation}\]
因为\(\alpha(t)\)和\(\beta(t)\)都是非负系数的多项式，
所以\(\sum\_{k=0}^\infty [\beta(t)]^k\)也是非负系数的，
从而\(c\_n\)都非负，且存在正值。

由[(29.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def-coef)可知\(\alpha(1) + \beta(1) < 1\)，
所以
\[\begin{equation}
0 < c = \sum\_{n=1}^\infty c\_n
= \frac{\alpha(1)}{1 - \beta(1)} < 1 .
\tag{29.13}
\end{equation}\]

**定理29.2** 在GARCH(\(p,q\))模型中，
记\(u\_t = v\_t^2\)，
\(\{c\_n \}\), \(c\)分别由[(29.12)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-cn)和[(29.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-c)定义。
引入
\[\begin{equation}
\begin{cases}
A\_t(0) = u\_t, \\
A\_t(n) = \sum\_{i\_1, i\_2, \dots, i\_n \geq 1}
c\_{i\_1} c\_{i\_2} \dots c\_{i\_n}
u\_{t} u\_{t - i\_1} u\_{t - i\_2} \dots u\_{t - i\_n} .
\end{cases}
\tag{29.14}
\end{equation}\]
则有以下结果：

（1）GARCH模型[(29.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def)有如下的严平稳遍历解
\[\begin{equation}
\begin{cases}
\varepsilon\_t
= \left[ \alpha\_0 \sum\_{n=0}^\infty A\_t(n) \right]^{1/2}
v\_t , \\
E(\varepsilon\_t) = 0,
\quad
E(\varepsilon\_t^2) = \frac{\alpha\_0}{
1 - \sum\_{i=1}^p \alpha\_i - \sum\_{j=1}^q \beta\_j} .
\end{cases}
\tag{29.15}
\end{equation}\]

（2）如果\(\{e\_t \}\)也是[(29.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def)的严平稳解，
则\(e\_t^2 = \varepsilon\_t^2\), a.s.

（3）如果\(c \sqrt{E(v\_1^4)} < 1\)，
则\(E(\varepsilon\_t^4) < \alpha\_0^2 E(v\_1^4) / (1 - c \sqrt{E(v\_1^4)})^2\) .

**证明**：
（1）
令\(c\_0 = \alpha\_0 / (1 - \beta(1))\)。
\(\{c\_n \}\), \(\{u\_t \}\)满足引理[29.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#lem:atsa-arch-archso-lem)的条件，
所以模型
\[
\begin{cases}
Y\_t = \left(c\_0 + \sum\_{j=1}^\infty c\_j Y\_{t-j} \right) u\_t, \\
u\_t \text{与} \{ Y\_{t-1}, Y\_{t-2}, \dots \} \text{独立}
\end{cases}
\]
的唯一的严平稳遍历解是
\[
Y\_t = c\_0 \sum\_{n=0}^\infty A\_t(n) .
\]
并且\(E(Y\_t) = c\_0 / (1 - c) < \infty\).
定义
\[\begin{equation}
\varepsilon\_t
= \left(
c\_0 + \sum\_{j=1}^\infty c\_j Y\_{t-j}
\right)^{1/2}
v\_t,
\tag{29.16}
\end{equation}\]
则\(\varepsilon\_t^2 = Y\_t\)，
由定理[4.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/strict-stationary.html#thm:strict-erg)可知\(\{\varepsilon\_t \}\)是严平稳遍历序列，
由\(A\_t(n)\)定义可以看出\(v\_t\)与\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots\}\)独立，
于是
\[
E(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)
= E\left(
c\_0 + \sum\_{j=1}^\infty c\_j \varepsilon\_{t-j}^2
\right)^{1/2}
E(v\_t)
= 0,
\]
又
\[\begin{aligned}
\sigma\_t^2
=& \text{Var}(\varepsilon\_t | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots) \\
=& E(\varepsilon\_t^2 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots) \\
=& E(Y\_t^2 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots) \\
=& \left(
c\_0 + \sum\_{j=1}^\infty c\_j \varepsilon\_{t-j}^2
\right)
E(v\_t^2 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots) \\
=& \left(
c\_0 + \sum\_{j=1}^\infty c\_j \varepsilon\_{t-j}^2
\right)
E(v\_t^2) \\
=& c\_0 + \sum\_{j=1}^\infty c\_j \varepsilon\_{t-j}^2 .
\end{aligned}\]
由定理[4.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/strict-stationary.html#thm:strict-erg)可知\(\{\sigma\_t \}\)也是严平稳遍历序列。
上式表明ARCH(\(p,q\))模型是一种无穷阶的ARCH模型。
用推移算子可以将上式写成
\[
\sigma\_t^2
= c\_0 + [1 - \beta(\mathscr B)]^{-1} \alpha(\mathscr B)
\varepsilon\_t^2 .
\]
由\(c\_0 = \frac{\alpha\_0}{1 - \beta(1)} = [1 - \beta(\mathscr B)]^{-1} \alpha\_0\)可见
\[
\sigma\_t^2
= [1 - \beta(\mathscr B)]^{-1} \alpha\_0
+ [1 - \beta(\mathscr B)]^{-1} \alpha(\mathscr B)
\varepsilon\_t^2
= [1 - \beta(\mathscr B)]^{-1}
(\alpha\_0 + \alpha(\mathscr B) )
\varepsilon\_t^2 .
\]
在上式两边作用\(1 - \beta(\mathscr B)\)得
\[
[1 - \beta(\mathscr B)] \sigma\_t^2
= \alpha\_0 + \alpha(\mathscr B)
\varepsilon\_t^2 ,
\]
可写成
\[
\sigma\_t^2 = \alpha\_0
+ \alpha(\mathscr B)
\varepsilon\_t^2
+ \beta(\mathscr B) \sigma\_t^2,
\]
即
\[
\sigma\_t^2 = \alpha\_0
+ \alpha\_1 \varepsilon\_{t-1}^2
+ \dots + \alpha\_p \varepsilon\_{t-p}^2
+ \beta\_1 \sigma\_{t-1}^2
+ \dots + \beta\_q \sigma\_{t-q}^2 .
\]
即严平稳遍历序列\(\{\varepsilon\_t \}\)满足GARCH模型[(29.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def)，
\[
E(\varepsilon\_t^2)
= E(Y\_t)
= \frac{c\_0}{1 - c}
= \frac{\alpha\_0 / (1 - \beta(1))}{1 - \frac{\alpha(1)}{1 - \beta(1)}}
= \frac{\alpha\_0}{1 - \beta(1) - \alpha(1)} .
\]
与定理[29.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#thm:atsa-arch-archso)证明类似可见\(\{\varepsilon\_t \}\)是白噪声列。

（2）和（3）的证明与定理[29.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#thm:atsa-arch-archso)证明类似。

---

令\(h = \max(p,q)\)，
\[
A(z) = 1 - \alpha(z) - \beta(z),
\quad
B(z) = 1 - \beta(z),
\]
则
\[
A(z) = 1 - \sum\_{j=1}^h (\alpha\_j + \beta\_j) z^j,
\quad
B(z) = 1 - \sum\_{j=1}^q \beta\_j z^j .
\]
条件[(29.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def-coef)保证\(A(t)\)满足最小相位性，
\(B(t)\)满足可逆性。
事实上，
记\(a\_j = \alpha\_j + \beta\_j\)，
则\(a\_j \geq 0\)，
\(\sum\_{j=1}^h a\_j = \sum\_{j=1}^p \alpha\_j + \sum\_{j=1}^q \beta\_j < 1\)。
于是对任意复数\(z\)满足\(|z| \leq 1\)，
有
\[
\left|
\sum\_{j=1}^h a\_j z^j
\right|
\leq
\sum\_{j=1}^h a\_j |z|^j
\leq \sum\_{j=1}^h a\_j
< 1,
\]
所以\(1 - \sum\_{j=1}^h a\_j z^j\)在\(|z| \leq 1\)时没有零点，
即\(A(z)\)满足最小相位条件，
类似可知\(B(z)\)满足可逆条件。

**命题29.1** 如果\(E(v\_1^4) < \infty\), \(E(\varepsilon\_1^4) < \infty\)，
则GARCH(\(p,q\))序列\(\{ \varepsilon\_t^2 \}\)满足如下ARMA(\(p,q\))模型：
\[\begin{equation}
A(\mathscr B) \varepsilon\_t^2
= \alpha\_0 + B(\mathscr B) \eta\_t,
\tag{29.17}
\end{equation}\]

\[
\]
其中\(\{\eta\_t \}\)是严平稳的零均值白噪声列。

**证明**：
令\(\eta\_t = \varepsilon\_t^2 - \sigma\_t^2\)，
则\(\{\eta\_t \}\)为严平稳列，
\(\varepsilon\_t^2 = \sigma\_t^2 + \eta\_t\)，
代入到[(29.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-def)中可得
\[\begin{aligned}
\varepsilon\_t^2
=& \sigma\_t^2 + \eta\_t \\
=& \alpha\_0 + \alpha(\mathscr B) \varepsilon\_t^2
+ \beta(\mathscr B) \sigma\_t^2
+ \eta\_t \\
=& \alpha\_0 + \alpha(\mathscr B) \varepsilon\_t^2
+ \beta(\mathscr B)(\varepsilon\_t^2 - \eta\_t)
+ \eta\_t \\
=& \alpha\_0
+ [\alpha(\mathscr B) + \beta(\mathscr B)] \varepsilon\_t^2
+ \eta\_t
- \beta(\mathscr B) \eta\_t \\
=& \alpha\_0
+ [1 - A(\mathscr B)] \varepsilon\_t^2
+ B(\mathscr B) \eta\_t,
\end{aligned}\]
这就是模型[(29.17)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arch.html#eq:atsa-arch-garch-arma-mod)，
且满足最小相位条件和可逆性条件。
只要再证明\(\{\eta\_t \}\)是零均值白噪声列。
\[
\eta\_t
= \varepsilon\_t^2 - \sigma\_t^2
= \sigma\_t^2 (v\_t^2 - 1),
\]
由\(v\_1\)四阶矩有限和\(\varepsilon\_1\)四阶矩有限可知\(\eta\_t\)二阶矩有限，
由\(\{\eta\_t \}\)严平稳可知\(\{\eta\_t \}\)宽平稳。
由\(v\_t\)与\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots \}\)独立，
\(\sigma\_t\)由\(\{\varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots \}\)决定，
可知\(v\_t\)与\(\sigma\_t\)独立，
从而
\[\begin{aligned}
E(\eta\_t)
=& E(\sigma\_t^2) E(v\_t^2 - 1)
= 0, \\
E(\eta\_t \eta\_{t+k})
=& E(\sigma\_t^2 (v\_t^2 - 1) \sigma\_{t+k}^2) E(v\_{t+k}^2 - 1)
= 0 .
\end{aligned}\]
由于\(E(\varepsilon\_1^4) < \infty\), \(E(v\_1^4) < \infty\)，
必有\(E(\sigma\_1^4) < \infty\)，
这是因为
\[
\sigma\_t^2 = E(\varepsilon\_t^2 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots),
\]
由条件Jensen不等式有
\[
\sigma\_t^4
= [E(\varepsilon\_t^2 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots)]^2
\leq E(\varepsilon\_t^4 | \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots),
\text{ a.s.},
\]
从而
\(E(\sigma\_t^4) \leq E(\varepsilon\_t^4) < \infty\)。
由\(\{\sigma\_t \}\)的严平稳性可知
\[
E(\eta\_t^2)
= E(\sigma\_t^4) E[(v\_t^2 - 1)^2]
< \infty,
\]
且不依赖于\(t\)，
从而\(\{\eta\_t \}\)是严平稳的零均值白噪声列。

---

GARCH序列也具有厚尾性。

**命题29.2** 设\(\{\varepsilon\_t \}\)是GARCH(\(p,q\))序列，
\(\kappa\_{\varepsilon}\)是\(\varepsilon\_t\)的峰度，
\(\kappa\_v\)是\(v\_t\)的峰度，则

（1）\(\kappa\_{\varepsilon} \geq \kappa\_v\)，
等号成立当且仅当\(\sigma\_t\)为常数(a.s.);

（2）如果\(E(v\_1^4) < \infty\),
\(E(\varepsilon\_1^4) < \infty\)，
则\(\{ \varepsilon\_t^2 \}\)的自相关系数\(\rho\_k = \text{Corr}(\varepsilon\_t^2, \varepsilon\_{t+k}^2)\)都非负，
且当\(\alpha\_j > 0\)时所有的\(\rho\_{kj} > 0\)，
\(k=1,2,\dots\)。

证明略。

### References

———. 2023. *应用时间序列分析*. 2nd ed. 北京大学出版社.