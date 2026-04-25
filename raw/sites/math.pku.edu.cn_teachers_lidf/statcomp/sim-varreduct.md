---
crawl_time: '2026-01-17 14:25:15'
framework: sphinx
title: 14 方差缩减技术 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 14 方差缩减技术

随机模拟方法虽然有着适用性广、方法简单的优点，
但是又有精度低、计算量大的缺点,
一整套模拟算几天几夜也是常有的事情。
如果能成倍地减小随机模拟误差方差，
就可以有效地节省随机模拟时间，
有些情况下可以把耗时长到不具有可行性的模拟计算（比如几个月）缩短到可行（比如几天）。

前一节的关于定积分计算的重要抽样法、分层抽样法都是降低随机模拟误差方差的重要方法，
也可以用在一般的模拟问题中。
本节介绍一些其它的方差缩减技巧。
我们以随机变量\(X\)的期望\(\theta = EX\)的估计为例，
目标是降低\(\theta\)的估计量的渐近方差。

## 14.1 控制变量法

设要估计随机变量\(X\)的期望\(\theta = E X\),
从\(X\)中抽取\(N\)个独立样本值\(X\_1, X\_2, \dots, X\_n\)，
用样本平均值\(\bar X = \frac{1}{N} \sum\_{i=1}^N X\_i\)估计\(EX\)。
为了提高精度可以利用辅助信息。
设有另外的随机变量\(Y\)满足
\[\begin{aligned}
E Y = 0, \quad \text{Cov}(X,Y) < 0
\end{aligned}\]
令\(Z = X+Y\), 则
\[\begin{aligned}
E(Z) = \theta, \quad \text{Var}(Z)
=& \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y),
\end{aligned}\]
只要\(\text{Var}(Y) + 2\text{Cov}(X,Y) < 0\)则\(\text{Var}(Z) < \text{Var}(X)\)，
如果有\((X,Y)\)成对的抽样\((X\_i, Y\_i), i=1,2,\dots,n\),
令\(Z\_i = X\_i + Y\_i\),
则用\(\bar Z\)来估计\(\theta = E X = E Z\)的渐近方差就比用\(\bar X\)估计\(I\)的渐近方差减小了。

为了最好地利用\(Y\)与\(X\)的相关性，
令
\[\begin{aligned}
Z(b) = X + bY,
\end{aligned}\]
则
\[\begin{aligned}
E Z(b) =& E X = \theta, \\
\text{Var}(Z(b)) =& \text{Var}(X) + 2b\text{Cov}(X,Y) + b^2\text{Var}(Y),
\end{aligned}\]
求\(\text{Var}(Z(b))\)关于\(b\)的最小值点，
得
\[\begin{aligned}
b = -\text{Cov}(X,Y) / \text{Var}(Y)
= -\rho\_{X,Y}\sqrt{\text{Var}(X)/\text{Var}(Y)},
\end{aligned}\]
这时
\[\begin{aligned}
\text{Var}(Z(b))
= (1 - \rho\_{X,Y}^2) \text{Var}(X)
\leq \text{Var}(X),
\end{aligned}\]
可见只要能找到零均值随机变量\(Y\)使得\(\rho\_{X,Y} \neq 0\)就可以减小\(EX\)的估计方差。
\(Y\)和\(X\)的相关性越强，改善幅度越大。
这种减小随机模拟误差方差的方法叫做**控制变量法**。

实际中\(\rho\_{X,Y}\)和\(\text{Var}(X), \text{Var}(Y)\)可能是未知的，
可以先模拟一个小的样本估计\(\rho\_{X,Y}\)和\(\text{Var}(X), \text{Var}(Y)\)从而获得\(b\)的估计值。

控制变量法中要求控制变量\(Y\)与\(X\)相关且\(EY=0\)。
如果\(E Y \neq 0\)但\(EY= \mu\_Y\)已知，
只要用\(Y - \mu\_Y\)代替\(Y\)，
这需要能预先知道\(Y\)的期望值的真值。
另一种情况是\(EY=EX\)未知，\(Y\)与\(X\)相关，
这时令\(Z = \alpha X + (1-\alpha) Y\),
\(\alpha \in [0,1]\),
则\(EZ=EX=\theta\)，
可以求\(\alpha\)使得\(\text{Var}(Z)\)最小。
容易知道当\(\alpha = \text{Cov}(Y, Y-X)/\text{Var}(Y-X)\)时\(\text{Var}(Z)\)最小。

**例14.1** 设要估计\(I = \int\_0^1 e^t \,dt\)。
当然，可以得到积分真值为\(e-1\)，
这里用来演示控制变量法的优势。

设\(U \sim \text{U}(0,1)\)，\(X = e^U\),
则\(I = E e^U = EX\),
可以用平均值法估计I为
\[\begin{aligned}
\hat I\_1 = \frac{1}{N} \sum\_{i=1}^N e^{U\_i}.
\end{aligned}\]
其方差为
\[\begin{aligned}
\text{Var}(\hat I\_1) = \frac{1}{N} \text{Var}(e^{U}) = \frac{1}{N}(-\frac12 e^2 + 2e - \frac32)
\approx \frac{0.2420}{N}.
\end{aligned}\]
令\(Y = U-\frac12\), 则\(EY=0\), \(X\)与\(Y\)正相关，
可以计算出\(\text{Cov}(X, Y) \approx 0.14086\),
\(\text{Var}(Y)=1/12\)(更复杂的问题中可能需要从一个小的随机抽样中近似估计)，
于是\(b = -\text{Cov}(X,Y) / \text{Var}(Y) = -1.690\),
对\(Z(b) = e^U - 1.690 (U - \frac12)\)有
\[
\text{Var}(Z(b))
= [1 - \rho\_{X,Y}^2]\text{Var}(X)
= (1 - 0.9919^2) \text{Var}(X)
= 0.016 \text{Var}(X) = 0.0039,
\]
用控制变量法估计\(I\)为
\[\begin{aligned}
\hat I\_2
= \frac{1}{N} \sum\_{i=1}^N \left[ e^{U\_i} - 1.690 (U\_i - \frac12) \right].
\end{aligned}\]
\(\hat I\_1\)的方差比控制变量法\(\hat I\_2\)的方差大60倍以上。

※※※※※

**例14.2 (系统可靠性估计(\*))** 考虑由\(n\)个部件组成的一个系统，
用\(S\_i\)表示第\(i\)个部件是否正常工作，
1表示正常工作，0表示失效。
设\(S\_i \sim \text{B}(1, p\_i)\)且各\(S\_i\)相互独立。
用\(Y\)表示系统是否工作正常，1表示工作正常，
0表示系统失效。
设\(Y\)为\(S\_1, S\_2, \dots, S\_n\)的函数\(\phi(S\_1, S\_2, \dots, S\_n)\)且\(\phi\)关于每个\(S\_i\)是单调不减，
称\(\phi\)为系统的结构函数。
令\(R = P(Y=1) = E \phi(S\_1, S\_2, \dots, S\_n)\)，
称\(R\)为系统可靠度。

例如，
\(\phi(s\_1, s\_2, \dots, s\_n) = \prod\_{i=1}^n s\_i\),
则当且仅当所有部件正常工作时系统才正常工作，
这样的系统称为串联系统，
这时系统可靠度为
\[\begin{aligned}
R = P(S\_1=1, S\_2=1, \dots, S\_n=1) = \prod\_{i=1}^n P(S\_i=1) = p\_1 p\_2 \dots p\_n.
\end{aligned}\]

在系统比较简单的情况下(如串联、并联)，
可以给出用\(p\_1, p\_2, \dots, p\_n\)表示\(R\)的表达式。
但是更复杂的系统则很难写出\(R\)的表达式，
这时可以用随机模拟方法估计\(R\)。

记\(\boldsymbol S = (S\_1, S\_2, \dots, S\_n)\),
\(X = \phi(\boldsymbol S)\),
对\(\boldsymbol S\)独立抽取\(N\)个点\(\boldsymbol S^{(j)} = (S\_1^{(j)}, S\_2^{(j)}, \dots, S\_n^{(j)}), j=1,2,\dots,N\),
\(R\)可以用平均值法估计为
\[\begin{align}
\hat R\_1 =& \frac{1}{N} \sum\_{j=1}^N \phi(\boldsymbol S^{(j)}).
\end{align}\]

令\(Y = \sum\_{i=1}^n (S\_i - p\_i)\),
则\(E Y = 0\), \(Y\)与\(X\)正相关。
用一个小的抽样先近似估计\(\text{Cov}(X,Y), \text{Var}(Y)\)得到\(b\)的近似值，
可以得到方差缩减的估计量
\[\begin{align\*}
\hat R\_2 = \frac{1}{N} \sum\_{j=1}^N \left[ \phi(\boldsymbol S^{(j)}) + b \sum\_{i=1}^n (S\_i^{(j)} - p\_i) \right].
\end{align\*}\]

※※※※※

## 14.2 对立变量法

控制变量法需要知道控制变量\(Y\)的期望值真值，
并精确或近似知道\(\text{Cov}(X,Y)\)和\(\text{Var}(Y)\)。
对立变量法的要求比较简单。

模拟中经常使用均匀分布随机数\(U\)变换产生的随机数\(X = g(U)\)。
下面的定理说明，
如果变换\(g(\cdot)\)是单调的，
则随机变量\(Y = g(1-U)\)就是与\(X\)负相关的。
注意\(g(1-U)\)与\(g(U)\)同分布所以\(EY = EX\)。

**定理14.1** 设\(g\)为单调函数，\(U \sim\) U(0,1),
则\(\text{Cov}(g(U), g(1-U)) \leq 0\)。

**证明**：
\(\forall u\_1, u\_2 \in [0,1]\)，由\(g\)单调可知
\[\begin{align\*}
\left( g(u\_1) - g(u\_2) \right) \left( g(1-u\_1) - g(1-u\_2) \right) \leq 0
\end{align\*}\]
设\(U\_2\) 服从 U(0,1)且与\(U\)独立，令\(X\_1 = g(U), Y\_1 = g(1 - U)\),
\(X\_2 = g(U\_2), Y\_2 = g(1 - U\_2)\), 则\(X\_1, Y\_1, X\_2, Y\_2\)的分布相同，且
\[\begin{align\*}
& E(X\_1 - X\_2) (Y\_1 - Y\_2) \\
=& \text{Cov}(X\_1 - X\_2, Y\_1 - Y\_2) \\
=& \text{Cov}(X\_1, Y\_1) + \text{Cov}(X\_2, Y\_2) - \text{Cov}(X\_1, Y\_2) - \text{Cov}(X\_2, Y\_1) \\
=& 2 \text{Cov}(X\_1, Y\_1)
\end{align\*}\]
注意\((X\_1 - X\_2)(Y\_1 - Y\_2) \leq 0\)所以\(\text{Cov}(X\_1, Y\_1) \leq 0\)，
即\(\text{Cov}(g(U), g(1-U)) \leq 0\)。证毕。

定理[14.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#thm:sim-anti-lem)可以推广到如下情形。

**定理14.2** 设\(h(x\_1, x\_2, \dots, x\_n)\)是关于每个自变量单调的函数，
\(U\_1, U\_2, \dots, U\_n\)相互独立同U(0,1)分布，
则\(\text{Cov}(h(U\_1, U\_2, \dots, U\_n),\ h(1-U\_1, 1-U\_2, \dots, 1-U\_n)) \leq 0\)。

证明见附录[D.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/proofs.html#proofs-antith)。

对均匀随机数\(U\)最常见的变换是逆变换\(X = F^{-1}(U)\)。
下面的定理给出了提高\(I = EX\)估计精度的方法。

**定理14.3 (对立变量法)** 设\(F(x)\)为连续分布函数，
\(U \sim \text{U}(0,1)\)，\(X = F^{-1}(U)\)，
\(Y = F^{-1}(1-U)\)，\(Z = \frac{X+Y}{2}\)，则
\(X\)与\(Y\)同分布\(F(x)\)且\(\text{Cov}(X,Y) \leq 0\)，
\[\begin{align\*}
\text{Var}(Z) \leq \frac{1}{2} \text{Var}(X)
\end{align\*}\]

**证明**:
因为\(U\)和\(1-U\)同分布所以\(X = F^{-1}(U)\)和\(Y=F^{-1}(1-U)\)同分布。
由定理[14.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#thm:sim-anti-lem),
令\(g(\cdot) = F^{-1}(\cdot)\)
可知\(\text{Cov}(X, Y) = \text{Cov}(g(U), g(1-U)) \leq 0\)，
从而
\[\begin{align\*}
\text{Var}(Z) =& \frac{\text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y)}{4} \\
=& \frac{\text{Var}(X) + \text{Cov}(X,Y)}{2}
\leq \frac{1}{2} \text{Var}(X)
\end{align\*}\]
证毕。

※※※※※

根据定理[14.3](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#thm:sim-anti)的结论，
为了估计\(I = EX\)，
产生\(U\_1, U\_2, \dots, U\_N\)后用
\[\begin{aligned}
Z\_i = \frac{1}{2}\left(F^{-1}(U\_i) + F^{-1}(1 - U\_i) \right),
\ i=1,2,\dots,N
\end{aligned}\]
的样本平均值估计\(I\)可以提高精度,
在不增加抽样个数的条件下把估计的随机误差方差降低至少一半。
利用定理[14.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#thm:sim-anti-lem)或[14.2](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#thm:sim-anti-lem-multi)提高随机模拟精度的方法叫做**对立变量法**。
对立变量法不需要计算\(X\)和\(Y\)的方差及协方差的值，
比控制变量法更简便易行。

**例14.3** 再次考虑例[14.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#exm:sim-vd-inte)的问题，估计\(I = \int\_0^1 e^t \,dt\)。
下面用对立变量法改善原始的平均值法\(\hat I\_1\)的估计方差。

设\(U \sim \text{U}(0,1)\)，\(X = e^U\),
令\(Y = e^{1-U}\), 用对立变量法估计\(I\)为
\[\begin{aligned}
\hat I\_3 =& \frac{1}{N} \sum\_{i=1}^{N} \frac{e^{U\_i} + e^{1 - U\_i}}{2},
\end{aligned}\]
方差为
\[\begin{aligned}
\text{Var}(\hat I\_3) =& \frac{1}{N/2} \frac{ \text{Var}(e^U) + \text{Cov}(e^U, e^{1-U})}{2}
= \frac{1}{N} (-\frac34 e^2 + \frac52 e - \frac54)
\approx \frac{0.0039}{N},
\end{aligned}\]
\(\hat I\_1\)的方差比对立变量法估计\(\hat I\_3\)的方差大至少60倍，
而\(\hat I\_3\)的方差和例[14.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#exm:sim-vd-inte)中控制变量法估计量\(\hat I\_2\)的方差相近。

※※※※※

对立变量法和控制变量法是类似做法，
一般不能结合使用。

**例14.4 (可靠性估计的对立变量法(\*))** 再次考虑例[14.2](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#exm:sim-sysrel-control)的可靠度估计问题。
用对立变量法改善估计方差。

设\(\{ U\_k \}\)为标准均匀分布随机数列，
取
\[\begin{aligned}
S\_i^{(j)} = \begin{cases}
1 & \text{当} U\_{n(j-1)+i} \leq p\_i, \\
0 & \text{其它}
\end{cases}
\end{aligned}\]
则\(S\_i^{(j)}\)是\(U\_{n(j-1)+i}\)的单调非增函数。
\(R\)用平均值法估计为
\[\begin{aligned}
\hat R\_1 =& \frac{1}{N} \sum\_{j=1}^N \phi(S\_1^{(j)}, S\_2^{(j)}, \dots, S\_n^{(j)}).
\end{aligned}\]

利用对立变量法，
令\(h(U\_1, U\_2, \dots, U\_n) = \phi(S\_1, S\_2, \dots, S\_n)\),
则\(h\)关于每个自变量是单调非增函数，
于是
\[\begin{aligned}
\text{Cov}(h(U\_1, U\_2, \dots, U\_n), \ h(1 - U\_1, 1 - U\_2, \dots, 1 - U\_n)) \leq 0,
\end{aligned}\]
估计系统可靠度\(R\)为
\[\begin{aligned}
\hat R\_3 = \frac{1}{N} \sum\_{j=1}^N
\frac{h(U\_1^{(j)}, U\_2^{(j)}, \dots, U\_n^{(j)}) + h(1 - U\_1^{(j)}, 1 - U\_2^{(j)}, \dots, 1 - U\_n^{(j)})}{2}
\end{aligned}\]
就能比\(\hat R\_1\)的误差方差至少降低\(\frac12\)。

※※※※※

注意实际应用中定理[14.2](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#thm:sim-anti-lem-multi)中的\(U\_1, U\_2, \dots, U\_n\)独立同U(0,1)分布的要求可以推广。
例如，
设\(X\_i \sim \text{N}(\mu\_i, \sigma\_i^2)\)相互独立，
要估计\(\theta = Eh(X\_1, X\_2, \dots, X\_n)\),
其中\(h\)关于每个自变量单调不减，
则\(h(2\mu\_1 - X\_1, 2\mu\_2 - X\_2, \dots, 2\mu\_n - X\_n)\)
与\(h(X\_1, X\_2, \dots, X\_n)\)同分布，
对
\[\begin{aligned}
Z = \frac{h(X\_1, X\_2, \dots, X\_n) + h(2\mu\_1 - X\_1, 2\mu\_2 - X\_2, \dots, 2\mu\_n - X\_n)}{2}
\end{aligned}\]
抽样用平均值估计\(\theta\)可以比仅对\(h(X\_1, X\_2, \dots, X\_n)\)抽样得到的估计量的方差缩减一半以上。
事实上，
令\(f(x\_1, \dots, x\_n) = h(x\_1, \dots, x\_n)\),
\(g(x\_1, \dots, x\_n) = -h(2\mu\_1 - x\_1, \dots, 2\mu\_n - x\_n)\)，
由附录[D.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/proofs.html#proofs-antith)定理[D.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/proofs.html#thm:prf-antith-base)可知\(h(X\_1, X\_2, \dots, X\_n)\)与\(h(2\mu\_1 - X\_1, 2\mu\_2 - X\_2, \dots, 2\mu\_n - X\_n)\)负相关。

## 14.3 条件期望法

进行统计估计时，如果有额外的相关信息，
利用这样的信息可以提高估计精度。
比如，对随机变量\(Y\)，如果\(Y\)服从某种模型，
在估计\(I = E Y\)时应当尽量利用模型信息。

设变量\(X\)与\(Y\)不独立，
根据Rao-Blackwell不等式:
\[\begin{align\*}
\text{Var}\left\{ \text{E}(Y | X) \right\} \leq \text{Var}(Y)
\end{align\*}\]
又
\[\begin{align\*}
\text{E} \left\{ \text{E}(Y | X) \right\} = \text{E} Y = I
\end{align\*}\]
所以，对\(Z = \text{E}(Y | X)\)抽样,
用\(Z\)的样本平均值来估计\(I=\text{E} Y\)比直接用\(Y\)的样本平均值的精度更高。
这种改善随机模拟估计精度的方法叫做**条件期望法**，
或Rao-Blackwell方法。

**例14.5** 设\(X\sim p(x)\)，\(\varepsilon \sim \text{N}(0, \sigma^2)\)且与\(X\)独立，
\[\begin{align\*}
Y = \psi(X) + \varepsilon,
\end{align\*}\]
估计\(I = \text{E} Y\)。

可以用条件分布抽样法对二元随机向量\(\boldsymbol Z=(X,Y)\)抽样产生\(Y\)的样本。
从\(p(\cdot)\)抽样得\(X\_1, X\_2, \dots, X\_N\),
独立地从N(0,\(\sigma^2\))抽样得
\(\varepsilon\_1, \varepsilon\_2, \dots, \varepsilon\_N\)，
令
\[\begin{align\*}
Y\_i = \psi(X\_i) + \varepsilon\_i,
\ i=1,2,\dots, N
\end{align\*}\]
然后用\(Y\_1, Y\_2, \dots, Y\_N\)的样本平均值估计\(EY\):
\[\begin{align\*}
\hat I\_1 = \frac{1}{N} \sum\_{i=1}^N Y\_i,
\end{align\*}\]
估计方差为
\[\begin{align\*}
\text{Var}(\hat I\_1) = \frac{\text{Var}(Y\_1)}{N}
= \frac{\text{Var}(\psi(X\_1))}{N} + \frac{\sigma^2}{N}.
\end{align\*}\]

另一方面，注意\(E(Y | X) = \psi(X)\)，
也可以只对\(X\)抽样然后用条件期望法估计\(EY\):
\[\begin{align\*}
\hat I\_2 = \frac{1}{N} \sum\_i \psi(X\_i),
\end{align\*}\]
估计方差为
\[\begin{align\*}
\text{Var}(\hat I\_2) = \frac{\text{Var}(\psi(X\_1))}{N}
< \text{Var}(\hat I\_1).
\end{align\*}\]

这个例子演示了条件期望法可以缩减误差方差的原因：
对\(Y\)抽样分成两步进行：第一步对\(X\)抽样，
第二步利用\(Y|X\)的条件分布对\(Y\)抽样。
所以使用\(Y\)的样本估计\(EY\)包含了第二步对\(Y\)抽样的随机误差，
而使用\(X\)的函数\(E(Y|X)=\psi(X)\)的抽样来估计\(EY\)则避免了第二步对\(Y\)抽样引起的随机误差。

※※※※※

**例14.6 (圆周率估计改进(\*))** 考虑例[10.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-intro.html#exm:sim-intro-pisim)中用随机模拟方法估计\(\pi\)的改进问题。

设\((X, Y)\)服从正方形\(D = [-1,1]^2\)上的均匀分布，
令\(\eta = 1\)表示\((X,Y)\)落入单位圆\(C = \{(x,y) \,:\, x^2 + y^2 \leq 1 \}\),
\(\eta=0\)表示未落入单位圆,
则\(\eta \sim \text{B}(1, \pi/4)\)。
设\((X\_i, Y\_i), i=1,2,\dots,N\)是\((X,Y)\)的独立重复抽样，
\(\eta\_i\)表示\(X\_i^2 + Y\_i^2 \leq 1\)是否发生，
则\(\pi\)的估计为
\[\begin{aligned}
\hat \pi\_1 = \frac{4}{N} \sum\_{i=1}^N \eta\_i,
\end{aligned}\]
方差为\(\text{Var}(\hat\pi\_1) = \pi(4-\pi)/N \approx \frac{2.6968}{N}\)。

用\(\zeta = E(\eta | X)\)的样本代替\(\eta\)来估计\(E \eta = \pi/4\)，则由
\[\begin{aligned}
E(\eta | X=x) =& P(X^2 + Y^2 \leq 1 \,|\, X=x)
= P(Y^2 \leq 1 - x^2) \\
=& P(-\sqrt{1 - x^2} \leq Y \leq \sqrt{1 - x^2}) \\
=& \sqrt{1 - x^2} \quad(\text{注意 }Y \sim \text{U}(-1,1))
\end{aligned}\]
可知\(\zeta = \sqrt{1 - X^2}\)。其方差为
\[\begin{aligned}
\text{Var}(\zeta) =& E(1-X^2) - (E\zeta)^2 = \frac23 - \frac{\pi^2}{16},
\end{aligned}\]
估计\(\pi\)为
\[\begin{aligned}
\hat\pi\_2 = \frac{4}{N} \sum\_{i=1}^N \sqrt{1 - X\_i^2},
\end{aligned}\]
方差为\(\text{Var}(\hat\pi\_2) \approx 0.7971/N\)，
\(\text{Var}(\hat\pi\_1) / \text{Var}(\hat\pi\_2) \approx 3.4\)。

另外，\(\zeta\)仅依赖于\(X^2\)，
容易发现若\(U \sim \text{U}(0,1)\)则\(X^2\)和\(U^2\)同分布，
所以可取\(\xi = \sqrt{1 - U^2}\),
其中\(U \sim \text{U}(0,1)\)。
这时，函数\(h(u) = \sqrt{1 - u^2}\)是\(u \in (0,1)\)的单调函数，
可以利用对立变量法，构造\(\pi\)的估计量为
\[\begin{aligned}
\hat\pi\_3 =&
\frac{4}{N} \sum\_{i=1}^N \frac{\sqrt{1 - U\_i^2} + \sqrt{1 - (1 - U\_i)^2}}{2},
\end{aligned}\]
其中\(U\_1, U\_2, \dots, U\_N\)为\(U(0,1)\)随机数，
则\(\text{Var}(\hat\pi\_3) \approx 0.11/N\),
\(\text{Var}(\hat\pi\_1) / \text{Var}(\hat\pi\_3) \approx 25\)。

也可以用对立变量法来改进\(\hat\pi\_2\)。
令\(W = U^2 - \frac{1}{3}\),
则\(EW = 0\)且\(W\)与\(\zeta\)负相关。
可以先进行一个小规模的模拟估计\(\text{Cov}(\zeta, W)\)和\(\text{Var}(W)\)得到
\(b = -\text{Cov}(\zeta, W)/\text{Var}(W)\)的近似值，
用\(\zeta(b) = \zeta + b W\)代替\(\zeta\)进行抽样，
可以减小\(\hat\pi\_2\)的方差。

※※※※※

**例14.7** 在标准化重要抽样法中应用条件期望法减小方差。

设随机变量\(\boldsymbol Y \sim f(\boldsymbol y)\),
为了估计\(\theta = E h(\boldsymbol Y)\),
经常使用标准化重要抽样法
\[\begin{aligned}
\hat\theta\_1 = \frac{\sum\_{i=1}^N W\_i h(\boldsymbol X\_i)}{\sum\_{i=1}^N W\_i},
\end{aligned}\]
其中\(\boldsymbol X\_i\)为试抽样密度\(g(\boldsymbol x)\)的样本，
权重\(W\_i = f(\boldsymbol X\_i)/g(\boldsymbol X\_i)\)。
如果\(\boldsymbol x = (\boldsymbol u, \boldsymbol v)\),
\(h(\boldsymbol x) = h\_1(\boldsymbol u)\),
则只要对\(\boldsymbol X=(\boldsymbol U, \boldsymbol V)\)的分量\(\boldsymbol U\)
抽样得到\(\boldsymbol U\_i, i=1,\dots,N\)及新的权重
\(\tilde W\_i = f\_{\boldsymbol U}(\boldsymbol U\_i) / g\_{\boldsymbol U}(\boldsymbol U\_i)\),
其中\(f\_{\boldsymbol U}(\boldsymbol u) = \int f(\boldsymbol u, \boldsymbol v) \,d\boldsymbol v\),
\(g\_{\boldsymbol U}(\boldsymbol u) = \int g(\boldsymbol u, \boldsymbol v) \,d\boldsymbol v\),
则可估计\(\theta\)为
\[\begin{aligned}
\hat\theta\_2 = \frac{\sum\_{i=1}^N \tilde W\_i h\_1(\boldsymbol U\_i)}{\sum\_{i=1}^N \tilde W\_i},
\end{aligned}\]
这时
\[\begin{aligned}
\text{Var}(\tilde W\_i) \leq \text{Var}(W\_i), \ i=1,2,\dots,N.
\end{aligned}\]
事实上，
\[\begin{aligned}
\frac{f\_{\boldsymbol U}(\boldsymbol u)}{g\_{\boldsymbol U}(\boldsymbol u)}
=& \int \frac{f(\boldsymbol u, \boldsymbol v)}{g\_{\boldsymbol U}(\boldsymbol u)} \,dv
= \int \frac{f(\boldsymbol u, \boldsymbol v)}{g\_{\boldsymbol U}(\boldsymbol u) g\_{\boldsymbol V|\boldsymbol U}(\boldsymbol v|\boldsymbol u)}
g\_{\boldsymbol V|\boldsymbol U}(\boldsymbol v|\boldsymbol u) \,d\boldsymbol v \nonumber\\
=& \int \frac{f(\boldsymbol u, \boldsymbol v)}{g(\boldsymbol u, \boldsymbol v)}
g\_{\boldsymbol V|\boldsymbol U}(\boldsymbol v|\boldsymbol u) \,d\boldsymbol v
= E\_g \left\{ \left. \frac{f(\boldsymbol U, \boldsymbol V)}{g(\boldsymbol U, \boldsymbol V)} \right|
\boldsymbol U= \boldsymbol u \right\}
\end{aligned}\]
于是
\[\begin{aligned}
\text{Var}\_g\left\{ \frac{f(\boldsymbol U, \boldsymbol V)}{g(\boldsymbol U, \boldsymbol V)} \right\}
\geq \text{Var}\_g\left\{
E\_g\left[ \left. \frac{f(\boldsymbol U, \boldsymbol V)}{g(\boldsymbol U, \boldsymbol V)} \right| \boldsymbol U \right] \right\}
= \text{Var}\_g\left\{ \frac{f\_{\boldsymbol U}(\boldsymbol U)}{g\_{\boldsymbol U}(\boldsymbol U)} \right\}.
\end{aligned}\]

※※※※※

## 14.4 随机数复用

在统计研究中，经常需要比较若干种统计方法的性能，
如偏差、方差、覆盖率等。
除了努力获取理论结果以外，
可以用随机模拟方法进行比较：
重复生成\(N\)组随机样本，
对每个样本同时用不同统计方法计算结果，
最后从\(N\)组结果比较不同方法的性能。
这样比较时，
并没有对每种方法单独生成\(N\)组样本，
而是每个样本同时应用所有要比较的方法。
这样不仅减少了计算量，
而且在比较时具有更高的精度。

**例14.8** 对正态分布总体\(X \sim N(\mu, \sigma^2)\),
如果有样本\(X\_1, X\_2, \dots, X\_n\),
估计\(\sigma^2\)有两种不同的公式:
\[\begin{aligned}
\hat\sigma\_1^2 =& \frac{1}{n-1} \sum\_{i=1}^n (X\_i - \bar X)^2,
\hat\sigma\_2^2 = \frac{1}{n} \sum\_{i=1}^n (X\_i - \bar X)^2.
\end{aligned}\]
希望比较两个估计量的偏差和均方误差:
\[\begin{aligned}
b\_1 =& E\hat\sigma\_1^2 - \sigma^2, \quad b\_2 = E\hat\sigma\_2^2 - \sigma^2, \\
s\_1 =& E(\hat\sigma\_1^2 - \sigma^2)^2, \quad s\_2 = E(\hat\sigma\_2^2 - \sigma^2)^2.
\end{aligned}\]

当然，这个问题很简单，可以得到偏差和均方误差的理论值：
\[\begin{aligned}
b\_1 =& 0, \quad b\_2 = -\frac{1}{n} \sigma^2, \\
s\_1 =& \frac{2 \sigma^4}{n-1}, \quad
s\_2 = \frac{(2n-1)\sigma^4}{n^2},
\quad
s\_1 - s\_2 = \frac{(3n-1) \sigma^4}{n^2(n-1)}.
\end{aligned}\]
我们用随机模拟来作比较。
重复地生成\(N\)组样本\((X\_1^{(j)}, X\_2^{(j)}, \dots, X\_n^{(j)})\), \(j=1,2,\dots,N\)。
对每组样本分别计算\(\hat\sigma\_{1j}^2 = \frac{1}{n-1} \sum\_{i=1}^n (X\_i^{(j)} - \bar X^{(j)})^2\)
和\(\hat\sigma\_{2j}^2 = \frac{1}{n} \sum\_{i=1}^n (X\_i^{(j)} - \bar X^{(j)})^2\)，
得到偏差和均方误差的估计
\[\begin{aligned}
\hat b\_1 =& \frac{1}{N} \sum\_{j=1}^N \hat\sigma\_{1j}^2 - \sigma^2,
\quad
\hat b\_2 = \frac{1}{N} \sum\_{j=1}^N \hat\sigma\_{2j}^2 - \sigma^2, \\
\hat s\_1 =& \frac{1}{N} \sum\_{j=1}^N (\hat\sigma\_{1j}^2 - \sigma^2)^2,
\quad
\hat s\_2 = \frac{1}{N} \sum\_{j=1}^N (\hat\sigma\_{2j}^2 - \sigma^2)^2.
\end{aligned}\]
容易看出，
两种方法使用相同的模拟样本得到的偏差、均方误差的估计精度与每种方法单独生成模拟样本得到的估计精度相同。

但是，如果要估计\(\Delta s = s\_1 - s\_2\)，
利用相同的样本的估计精度更好。
一般地，
\[\begin{align}
\text{Var}(\hat s\_1 - \hat s\_2) = \text{Var}(\hat s\_1) + \text{Var}(\hat s\_2) - 2 \text{Cov}(\hat s\_1, \hat s\_2).
\tag{14.1}
\end{align}\]
如果每种方法使用不同的样本，则[(14.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#eq:sim-rng-rep-ex1)变成
\[\begin{align}
\text{Var}(\hat s\_1 - \hat s\_2)
= \frac{1}{N}\left[ \text{Var}( (\hat\sigma\_1^2 - \sigma^2)^2 )
+ \text{Var}( (\hat\sigma\_2^2 - \sigma^2)^2 ) \right].
\tag{14.2}
\end{align}\]
如果两种方法利用相同的样本计算，
则[(14.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#eq:sim-rng-rep-ex1)变成
\[\begin{align}
\text{Var}(\hat s\_1 - \hat s\_2)
=& \frac{1}{N}\left[ \text{Var}( (\hat\sigma\_1^2 - \sigma^2)^2 )
+ \text{Var}( (\hat\sigma\_2^2 - \sigma^2)^2 ) \right. \\
& \left. - 2 \text{Cov}( (\hat\sigma\_1^2 - \sigma^2)^2, (\hat\sigma\_2^2 - \sigma^2)^2 )
\right].
\tag{14.3}
\end{align}\]
而\((\hat\sigma\_1^2 - \sigma^2)^2\)与\((\hat\sigma\_2^2 - \sigma^2)^2\)明显具有强正相关，
所以两种方法针对相同样本计算时\(\hat s\_1 - \hat s\_2\)的方差比两种方法使用单独样本时的方差要小得多。
这说明重复利用相同的随机数或样本往往可以提高比较的精度。
可以计算出[(14.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#eq:sim-rng-rep-ex2)约为\(16 n^{-2} / N\), [(14.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#eq:sim-rng-rep-ex2)约为\(104 n^{-3} / N\)。

※※※※※

## 习题

#### 习题1

用随机模拟法计算二重积分\(\int\_0^1 \int\_0^1 e^{(x+y)^2} \,dy\,dx\),
用对立变量法改善精度。

#### 习题2

用R程序实现例[14.8](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-varreduct.html#exm:sim-varreduct-normal-s2)的模拟。
取\(\mu=0\), \(\sigma=1\),
\(N=100000\), \(n=5, 10, 30\)，
列出各偏差、均方误差和\(\hat s\_1 - \hat s\_2\)的值。