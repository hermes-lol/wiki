---
crawl_time: '2026-01-17 14:29:04'
framework: sphinx
title: B 数学分析 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# B 数学分析

时间序列分析用到了数学分析、复分析、实变函数、泛函分析、测度论、概率论、随机过程、数理统计、多元统计分析中的一些结果。
这里对一些数学知识进行整理。

## B.1 极限

**定理B.1 (分部求和公式)** 若\(\{ x\_k, k=m, m+1, \dots, n \}\),
\(\{ y\_k, k=m, m+1, \dots, n+1 \}\)是数列，则
\[\begin{aligned}
\sum\_{k=m}^n x\_k (y\_{k+1} - y\_k)
=& [x\_n y\_{n+1} - x\_m y\_m]
- \sum\_{k=m+1}^n y\_k (x\_k - x\_{k-1}) \\
=& [x\_n y\_{n+1} - x\_m y\_m]
- \sum\_{k=m}^{n-1} y\_{k+1} (x\_{k+1} - x\_{k})
\end{aligned}\]

**证明**：
\[\begin{aligned}
\text{左边} =& \sum\_{k=m}^n x\_k y\_{k+1} - \sum\_{k=m}^n x\_k y\_k \\
\text{右边}
=& x\_n y\_{n+1} - x\_m y\_m
- \sum\_{k=m+1}^n x\_k y\_k + \sum\_{k=m+1}^n x\_{k-1} y\_k \\
=& x\_n y\_{n+1} + \sum\_{s=m}^{n-1} x\_s y\_{s+1}
- \sum\_{k=m}^n x\_k y\_k \\
= & \sum\_{s=m}^{n} x\_s y\_{s+1}
- \sum\_{k=m}^n x\_k y\_k \\
=& \text{左边}
\end{aligned}\]

○○○○○○

**定理B.2 (Kronecker引理)** 设\(\{ x\_n, n \in \mathbb N\_+ \}\)是复数列，
\(\sum\_{n=1}^\infty x\_n\)收敛到复数\(s\)。
实数列\(\{ b\_n \}\)满足
\(0 < b\_1 \leq b\_2 \leq \dots\)且\(\lim\_{n\to\infty} b\_n = \infty\)，
则
\[
\lim\_{n\to\infty} \frac{1}{b\_n} \sum\_{k=1}^n b\_k x\_k = 0 .
\]

**证明**：
记\(S\_n = \sum\_{j=1}^n x\_j\), \(S\_0 = 0\),
\(y\_{n+1} = S\_n\)。
由分部求和公式有
\[\begin{aligned}
& \sum\_{k=1}^n b\_k x\_k
= \sum\_{k=1}^n b\_k (S\_k - S\_{k-1})
= \sum\_{k=1}^n b\_k (y\_{k+1} - y\_{k}) \\
=& b\_n y\_{n+1} - b\_1 y\_1
- \sum\_{k=1}^{n-1} y\_{k+1} (b\_{k+1} - b\_{k}) \\
=& b\_n S\_n
- \sum\_{k=1}^{n-1} S\_{k} (b\_{k+1} - b\_{k})
\end{aligned}\]
于是
\[\begin{aligned}
\frac{1}{b\_n} \sum\_{k=1}^n b\_k x\_k
= S\_n - \frac{1}{b\_n} \sum\_{k=1}^{n-1} (b\_{k+1} - b\_k) S\_k
\end{aligned}\]
由于\(S\_n \to s\),
\(\forall \varepsilon>0\)，
存在\(N\)使得\(n \geq N\)时\(| S\_n - s | < \varepsilon/2\)。
将上式右边变成
\[\begin{aligned}
& S\_n - \frac{1}{b\_n} \sum\_{k=1}^{N-1} (b\_{k+1} - b\_k) S\_k
- \frac{1}{b\_n} \sum\_{k=N}^{n-1} (b\_{k+1} - b\_k) S\_k \\
=& S\_n - \frac{1}{b\_n} \sum\_{k=1}^{N-1} (b\_{k+1} - b\_k) S\_k
- \frac{1}{b\_n} \sum\_{k=N}^{n-1} (b\_{k+1} - b\_k) s
- \frac{1}{b\_n} \sum\_{k=N}^{n-1} (b\_{k+1} - b\_k) (S\_k - s) \\
=& S\_n - \frac{1}{b\_n} \sum\_{k=1}^{N-1} (b\_{k+1} - b\_k) S\_k
- \frac{b\_n - b\_N}{b\_n} s
- \frac{1}{b\_n} \sum\_{k=N}^{n-1} (b\_{k+1} - b\_k) (S\_k - s) \\
\end{aligned}\]
当\(n\to\infty\)时，
第一项和第三项分别趋于\(s\)和\(-s\)，可以消去；
第二项趋于0，
第四项的绝对值小于等于
\(\frac12\varepsilon \frac{b\_n - b\_N}{b\_n} \leq \frac12\varepsilon\)，
所以存在\(N\_2>N\)使得\(n > N\_2\)时四项之和绝对值小于\(\varepsilon\)。
Knonecker引理证毕。

○○○○○○

**定理B.3 (Stolz定理)** 设实数列\(\{ a\_n \}\)、\(\{ b\_n \}\)满足

(1) \(\{b\_n \}\)严格单调递增；

(2) \(\lim\_{n\to\infty} b\_n = +\infty\);

(3) \(\lim\_{n\to\infty} \frac{a\_{n+1} - a\_{n}}{b\_{n+1} - b\_n} = L\)
有意义，\(L\)为有限实数、\(+\infty\)或\(-\infty\)；

则有

\[
\lim\_{n\to\infty} \frac{a\_n}{b\_n}
= \lim\_{n\to\infty} \frac{a\_{n+1} - a\_{n}}{b\_{n+1} - b\_n} = L
\]

这是与微积分中洛必达法则类似的数列极限定理。

**证明**：
当\(L\)为有限实数时，
由条件(3)和条件(1)可知，
\(\forall \epsilon>0\),
\(\exists N\_1 > 0\)，
当\(n > N\_1\)时
\[
\left| \frac{a\_{n+1} - a\_{n}}{b\_{n+1} - b\_n} - L \right|
< \epsilon
\]
从而
\[
L - \epsilon < \frac{a\_{n+1} - a\_{n}}{b\_{n+1} - b\_n}
< L + \epsilon
\]
\[
(L - \epsilon)(b\_{n+1} - b\_n)
< a\_{n+1} - a\_{n}
< (L + \epsilon)(b\_{n+1} - b\_n)
\quad (\*)
\]

由条件(2)，
\(\exists N\_2>N\_1\)，
当\(n > N\_2\)时
\(b\_n > \epsilon > 0\)。

当\(n>N\_2\)时，
从\(N\_2+1\)到\(n\)对(\*)式累加，
有
\[
(L-\epsilon)(b\_{n+1} - b\_{N\_2+1})
< a\_{n+1} - a\_{N\_2+1}
< (L+\epsilon)(b\_{n+1} - b\_{N\_2+1})
\]
于是
\[
L-\epsilon
< \frac{a\_{n+1} - a\_{N\_2+1}}{b\_{n+1} - b\_{N\_2+1}}
< L+\epsilon
\]
由\(b\_{n+1} > \epsilon > 0\)，得
\[
L-\epsilon
< \frac{\frac{a\_{n+1}}{b\_{n+1}} - \frac{a\_{N\_2+1}}{b\_{n+1}}}{1 - \frac{b\_{N\_2+1}}{b\_{n+1}}}
< L+\epsilon
\]
令\(n\to\infty\)，
因为
\[
\lim\_{n\to\infty} \frac{a\_{N\_2+1}}{b\_{n+1}} = 0,
\quad
\lim\_{n\to\infty} \frac{b\_{N\_2+1}}{b\_{n+1}} = 0
\]
所以存在\(N\_3 > N\_2\)，
\(n > N\_3\)时
\[
|L+\epsilon| \cdot
\left| \frac{b\_{N\_2+1}}{b\_{n+1}} \right| < \epsilon,
\quad
|L-\epsilon| \cdot \left| \frac{b\_{N\_2+1}}{b\_{n+1}} \right| < \epsilon,
\quad
\left| \frac{a\_{N\_2+1}}{b\_{n+1}} \right| < \epsilon
\]
于是
\[
L - 2\epsilon
< \frac{a\_{n+1}}{b\_{n+1}} - \frac{a\_{N\_2+1}}{b\_{n+1}}
< L + 2\epsilon
\]
\[
L - 3\epsilon
< \frac{a\_{n+1}}{b\_{n+1}}
< L + 3\epsilon
\]
即
\[
\left| \frac{a\_{n+1}}{b\_{n+1}} - L \right| < 3\epsilon
\]
即
\[
\lim\_{n\to\infty} \frac{a\_n}{b\_n} = L .
\]

\(L\)为无穷时的证明略。

○○○○○○

**推论B.1** 如果数列\(a\_n \to 0\)(\(n\to\infty\))，
则
\[
\lim\_{n\to\infty} \frac{1}{n} \sum\_{i=1}^n a\_i = 0
\]

**证明**：
由Stolz定理，记\(S\_n = \sum\_{i=1}^n a\_i\)，则
\[\begin{aligned}
\lim\_{n\to\infty} \frac{1}{n} \sum\_{i=1}^n a\_i
=& \lim\_{n\to\infty} \frac{S\_n}{n} \\
=& \lim\_{n\to\infty} \frac{S\_n - S\_{n-1}}{n - (n-1)} \\
=& \lim\_{n\to\infty} a\_n = 0
\end{aligned}\]

○○○○○○

## B.2 微积分

**定理B.4 (微积分基本定理)** (1) 若\(f(x)\)是定义在\([a,b]\)上的Riemann可积函数且在\(x=x\_0\)处连续，
则函数
\[\begin{aligned}
F(x) = \int\_a^{x} f(t)dt, \quad x \in [a,b]
\end{aligned}\]
在\(x=x\_0\)处可微且\(F'(x\_0)=f(x\_0)\)。

(2) 若\(f(x)\)是定义在\([a,b]\)上的可微函数，
\(f'(x)\)在\([a,b]\)上是Riemann可积函数，
则\(f(x)\)是其导函数的不定积分：
\[\begin{aligned}
\int\_a^x f'(t) dt = f(x) - f(a), \quad x \in [a,b]
\end{aligned}\]

对Lebesgue积分也有类似结论。

**定理B.5 (Lebesgue定理)** 若\(f(x)\)是定义在\([a,b]\)上的单调上升实值函数，
则\(f(x)\)的不可微点集为零测集且有
\[\begin{aligned}
\int\_a^b f'(x) dx \leq f(b) - f(a)
\end{aligned}\]

**勒贝格积分与黎曼积分关系**

黎曼积分是按照\(x\)的区间进行分割，
当细分小区间长度趋于零时的极限（如果存在）。
勒贝格积分是按照函数值\(y\)的区间进行分割，
用简单函数的积分逼近一般函数的积分。

**定理B.6** 对闭区间\([a,b]\)上的有界函数\(f\)，
如果黎曼可积，
则\(f\)必为Borel可测函数且勒贝格可积，
积分值相等。

**定理B.7** 对闭区间\([a,b]\)上的有界函数\(f\)，
\(f\)黎曼可积的充分必要条件是\(f\)在\([a,b]\)中的不连续点组成的集合为勒贝格零测集。

**推论**：
\([a,b]\)上仅有有限个不连续点的函数是黎曼可积的，
也是勒贝格可积的，
两种积分相等。

**定义B.1 (有界变差函数)** 设\(f(x)\)是定义在\([a,b]\)上的实值函数，
作分划\(\Delta\_t\):
\(a=x\_0 < x\_1 < \dots < x\_n = b\)
以及相应的和
\[\begin{aligned}
\nu\_\Delta = \sum\_{i=1}^n | f(x\_i) - f(x\_{i-1}) |
\end{aligned}\]
令
\[\begin{aligned}
\bigvee\_a^b(f) = \sup \{ \nu\_\Delta: \Delta \text{为$[a,b]$的任一分划} \}
\end{aligned}\]
并称它为\(f\)在\([a,b]\)上的全变差。若
\[\begin{aligned}
\bigvee\_a^b(f) < +\infty
\end{aligned}\]
则称\(f(x)\)是\([a,b]\)上的有界变差函数，
其全体记为\(BV([a,b])\)。

有界变差函数有界，
\(BV([a,b])\)构成一个线性空间。

## B.3 数值级数

设\(\{ a\_n, n = 1,2,\dots \}\)为实数列，
考虑\(\sum\_{n=1}^{\infty} a\_n\)。
称
\[
S\_n = \sum\_{i=1}^n a\_i
\]
为部分和序列。
如果\(S\_n\)有实数值极限\(S\)，
则称级数\(\sum\_{n=1}^{\infty} a\_n\)收敛到\(S\)；
如果\(S\_n\)极限为\(+\infty\)或\(-\infty\)，
则称级数\(\sum\_{n=1}^{\infty} a\_n\)发散到\(+\infty\)或\(-\infty\)；
如果\(S\_n\)极限不存在，
则称级数\(\sum\_{n=1}^{\infty} a\_n\)发散。

如果\(\sum\_{n=1}^{\infty} |a\_n|\)收敛到有限值，
则称级数\(\sum\_{n=1}^{\infty} a\_n\)绝对收敛，
绝对收敛推出收敛。

如果级数\(\sum\_{n=1}^{\infty} a\_n\)收敛，
则\(\lim\_{n \to \infty} a\_n = 0\)。

对正项级数\(\sum\_{n=1}^{\infty} a\_n\)和\(\sum\_{n=1}^{\infty} b\_n\)，
如果\(\lim\_{n\to\infty} \frac{a\_n}{b\_n}\)为有限的非零实数值，
即\(a\_n\)和\(b\_n\)同阶，
则两个级数同时收敛或者同时发散。

**达朗倍尔判别法**：
设\(\sum\_{n=1}^{\infty} a\_n\)是正项级数，
若
\[
\varlimsup\_{n\to\infty} \frac{a\_{n+1}}{a\_n} = r < 1,
\]
则\(\sum\_{n=1}^{\infty} a\_n\)收敛；
若
\[
\varliminf\_{n\to\infty} \frac{a\_{n+1}}{a\_n} = r > 1,
\]
则\(\sum\_{n=1}^{\infty} a\_n\)发散；
\(r=1\)时不能判断。

**哥西判别法**：
设\(\sum\_{n=1}^{\infty} a\_n\)是正项级数，
若
\[
\varlimsup\_{n\to\infty} a\_n^{1/n} = \rho,
\]
则当\(\rho<1\)时级数收敛，
当\(\rho>1\)时级数发散。

如果级数\(\sum\_{n=1}^{\infty} a\_n\)绝对收敛，
则任意改变求和次序，
级数仍绝对收敛，
且收敛到相同值；
否则，
改变求和次序可能发散或收敛到不同的结果。

**二重级数**：
对数列\(\{a\_{ij}, i=1,2,\dots, j=1,2,\dots\}\),
令\(S\_i = \sum\_{j=1}^\infty a\_{ij}\)，
若每个\(S\_i\)收敛，
且\(\sum\_{i=1}^\infty S\_i\)收敛，
则级数\(\sum\_{i=1}^\infty \sum\_{j=1}^\infty a\_{ij}\)收敛到\(\sum\_{i=1}^\infty S\_i\)。

如果其中\(\sum\_{i=1}^\infty \sum\_{j=1}^\infty |a\_{ij}| < \infty\)，
则可交换次序
\[
\sum\_{i=1}^\infty \sum\_{j=1}^\infty a\_{ij}
= \sum\_{j=1}^\infty \sum\_{i=1}^\infty a\_{ij} .
\]

**级数乘法**：
设级数\(\sum\_{n=1}^{\infty} a\_n\)和\(\sum\_{n=1}^{\infty} b\_n\)至少有一个绝对收敛，
则
\[
\left( \sum\_{n=1}^{\infty} a\_n \right)
\left( \sum\_{n=1}^{\infty} b\_n \right)
= \sum\_{n=1}^{\infty} c\_n ,
\]
其中
\[
c\_n = \sum\_{i=1}^{n} a\_i b\_{n+1-i} .
\]

常用求和公式：

\[\begin{aligned}
1+2+3+\dots+n =& \sum\_{k=1}^n k = \frac{1}{2} n (n+1) . \\
1^2 + 2^2 + 3^2 + \dots + n^2 =& \sum\_{k=1}^n k^2
= \frac{1}{6} n(n+1)(2n+1) .\\
1^3 + 2^3 + 3^3 + \dots + n^3 =& \sum\_{k=1}^n k^3
= \left( \frac{1}{2} n (n+1) \right)^2 .\\
1^4 + 2^4 + 3^4 + \dots + n^4 =& \sum\_{k=1}^n k^4
= \frac{1}{30} n(n+1)(2n+1)(3n^2+3n-1) .
\end{aligned}\]

## B.4 函数项级数

设\(f\_n(x)\)是定义在区间\(I\)上的函数，
\(n=1,2,\dots\)，称\(\{ f\_n(x), n=1,2,\dots \}\)为函数序列。
如果在\(I\)的一个非空子集\(I\_1\)上
\[
\lim\_{n\to\infty} f\_n(x) = f(x), \ \forall x \in I\_1,
\]
则称\(f(x)\)在\(I\_1\)上是函数序列的极限函数。

考虑函数级数\(\sum\_{n=1}^\infty u\_n(x)\)，
\(u\_n(x)\)是区间\(I\)上的函数，
如果其部分和序列
\[
S\_n(x) = \sum\_{i=1}^n u\_i(x)
\]
在\(I\_1 \subset I\)中收敛到极限函数\(S(x)\)，
则称级数\(\sum\_{n=1}^\infty u\_n(x)\)在\(I\_1\)中收敛到\(S(x)\)。

使得级数\(\sum\_{n=1}^\infty u\_n(x)\)收敛的点\(x\)称为收敛点，
否则称为发散点。
所有收敛点组成的集合称为收敛区域，
所有发散点组成的集合称为发散区域。

如果\(\sum\_{n=1}^\infty |u\_n(x)|\)在\(I\_1\)中收敛，
则称\(\sum\_{n=1}^\infty u\_n(x)\)在\(I\_1\)中绝对收敛，
绝对收敛推出收敛。

级数\(\sum\_{n=1}^\infty u\_n(x)\)与极限\(\lim\_{n\to\infty} \sum\_{i=1}^n u\_i(x)\)是同一问题。
对其中函数的微分、积分、极限等操作能否与求和或者极限运算交换次序？
在一致收敛条件下可以。

**一致收敛**：
设\(f\_n(x)\)在区间\(I\_1\)有极限函数\(f(x)\)，
如果任给\(\epsilon>0\)，都存在一个不依赖于\(x\)的正整数\(N\)，
当\(n \geq N\)时，
对任意\(x \in I\_1\)都有
\[
|f\_n(x) - f(x)| < \epsilon,
\]
则称\(f\_n(x)\)在区间\(I\_1\)一致收敛到\(f(x)\)。
类似地，
如果级数的部分和序列一致收敛，
则称级数一致收敛。

\(f\_n(x)\)在区间\(I\_1\)一致收敛到\(f(x)\)，
当且仅当
\[
\lim\_{n\to\infty} \sup\_{x \in I\_1} |f\_n(x) - f(x)| = 0 .
\]

对函数级数\(\sum\_{n=1}^\infty u\_n(x)\)，
如果\(\sum\_{i=n+1}^\infty u\_i(x)\)一致收敛到0，
则函数级数一致收敛。

**极限次序交换**：
设函数\(f\_n(x)\), \(n=1,2,\dots\)定义在\([a,b]\)上，
\(x\_0 \in [a,b]\)且\(f\_n(x)\)在\([a,b] \backslash \{x\_0\}\)上一致收敛到\(f(x)\)，
设
\(\lim\_{x \to x\_0} f\_n(x)\)存在，
则
\[
\lim\_{x\to x\_0} \lim\_{n\to \infty} f\_n(x)
= \lim\_{n\to \infty} \lim\_{x\to x\_0} f\_n(x) .
\]

**极限与求和号交换次序**：
设函数\(u\_n(x)\), \(n=1,2,\dots\)定义在\([a,b]\)上，
\(x\_0 \in [a,b]\)且\(\sum\_{n=1}^\infty u\_n(x)\)在\([a,b] \backslash \{x\_0\}\)上一致收敛到\(S(x)\)，
设
\(\lim\_{x \to x\_0} u\_n(x)\)存在，
则
\[
\lim\_{x \to x\_0} \sum\_{n=1}^\infty u\_n(x)
= \sum\_{n=1}^\infty \lim\_{x \to x\_0} u\_n(x) .
\]

如果\(f\_n(x)\)是闭区间\([a,b]\)上的连续函数，
\(f\_n(x)\)在\([a,b]\)上一致收敛到\(f(x)\)，
则\(f(x)\)也是闭区间\([a,b]\)上的连续函数。

如果\(u\_n(x)\)是闭区间\([a,b]\)上的连续函数，
\(\sum\_{n=1}^\infty u\_n(x)\)在在\([a,b]\)上一致收敛到\(S(x)\)，
则\(S(x)\)也是闭区间\([a,b]\)上的连续函数。

如果\(u\_n(x)\)是开区间\((a,b)\)上的连续函数，
\(\sum\_{n=1}^\infty u\_n(x)\)在\((a,b)\)内每一个闭区间上都一致收敛到\(S(x)\)，
则\(S(x)\)也是开区间\((a,b)\)上的连续函数。

**积分号下取极限**：
设\(f\_n(x)\)是闭区间\([a,b]\)上的连续函数，
\(f\_n(x)\)在\([a,b]\)上一致收敛到\(f(x)\)，
则
\[
\lim\_{n\to\infty} \int\_a^b f\_n(x) \,dx
= \int\_a^b \lim\_{n\to\infty} f\_n(x) \,dx .
\]

**积分与求和号交换次序**：
设\(u\_n(x)\)是闭区间\([a,b]\)上的连续函数，
级数\(\sum\_{n=1}^\infty u\_n(x)\)在在\([a,b]\)上一致收敛到\(S(x)\)，
则
\[
\int\_a^b \sum\_{n=1}^\infty u\_n(x) \,dx
= \sum\_{n=1}^\infty \int\_a^b u\_n(x) \,dx .
\]

**微分与求和号交换次序**：
设\(u\_n(x)\)在闭区间\([a,b]\)上可微，
\(\sum\_{n=1}^\infty u\_n'(x)\)一致收敛，
且\(\sum\_{n=1}^\infty u\_n(x)\)至少在某一个点\(x\_0\)上收敛，
则\(\sum\_{n=1}^\infty u\_n(x)\)在\([a,b]\)上一致收敛，
且
\[
\left( \sum\_{n=1}^\infty u\_n(x) \right)'
= \sum\_{n=1}^\infty u\_n'(x) .
\]

## B.5 幂级数

形如
\[
\sum\_{n=0}^\infty a\_n (x - x\_0)^n
\]
的函数项级数称为幂级数，
其中\(x\_0\)是任意给定实数，
\(\{ a\_n \}\)是实数列。
实际上只要考虑
\[\begin{align}
\sum\_{n=0}^\infty a\_n x^n .
\tag{B.1}
\end{align}\]

幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)的收敛区域只有如下三种情况：

1. 整个实数轴；
2. 关于原点对称的有限区间\((-R, R)\)，可含端点；
3. 只在\(x=0\)处收敛。

令
\[
\rho = \varlimsup\_{n\to\infty} |a\_n|^{1/n},
\]
当\(0 \leq \rho < \infty\)时，
幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)在\(|x| < \frac{1}{\rho}\)绝对收敛；
当\(0 < \rho < \infty\)，\(|x|>\frac{1}{\rho}\)时，
幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)发散。
称\(R = \frac{1}{\rho}\)为幂级数的收敛半径，
\((-R, R)\)为幂级数的收敛区间。
当\(\rho=0\)时，收敛区间是\((-\infty, \infty)\)；
当\(\rho=\infty\)时，
仅在\(x=0\)处收敛。
收敛区间端点处是否收敛不确定。

若幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)在\(x = x\_1 \neq 0\)处收敛，
则它在\(|x| < |x\_1|\)处绝对收敛；
如果级数在\(x = x\_0\)处发散，则它在\(|x| > |x\_0|\)处也发散。

如果
\[
\lim\_{n\to\infty} \frac{|a\_{n+1}|}{|a\_n|} = l,
\]
则幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)的收敛半径\(R = 1/l\)（包括\(l=0\)和\(l=\infty\)的情况）。

设幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)的收敛半径\(R>0\)，
则对任意\(0 < r < R\)，级数在\([-r, r]\)上一致收敛，
称为在\((-R, R)\)内闭一致收敛。

幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)在收敛区间\((-R, R)\)内是连续函数。

**幂级数微分**：
幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)在收敛区间\((-R, R)\)内可微，
且微分与求和号可交换：
\[
\left(\sum\_{n=0}^\infty a\_n x^n \right)'
= \sum\_{n=1}^\infty n a\_n x^{n-1} ,
\]
右边的级数与[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)有相同的收敛半径。

**幂级数积分**：
设幂级数[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)收敛半径\(R > 0\)，
则积分号与求和号可交换：
\[
\int\_0^x \sum\_{n=0}^\infty a\_n t^n \,dt
= \sum\_{n=0}^\infty \frac{a\_n}{n+1} x^{n+1} ,
\]
右边的级数与[(B.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mathanal.html#eq:spec-mathanal-powser-ps0f)有相同的收敛半径。

**泰勒展开**：
设函数\(f(x)\)在\(I = (x\_0 - \delta, x\_0 + \delta)\)上有任意阶导数，
且存在正常数\(M\)使得
\[
| f^{(n)}(x) | \leq M^n ,
\ \forall x \in I, \ n=1,2,\dots,
\]
则对\(x \in I\)有
\[
f(x) = \sum\_{n=0}^\infty \frac{f^{(n)}(x\_0)}{n!} (x - x\_0)^n .
\]

## B.6 傅立叶级数

考虑复数域上的希尔伯特空间
\(L^2[-\pi, \pi] = (L^2[-\pi, \pi], \mathscr B, U)\),
其中\(\mathscr B\)是\([-\pi,\pi]\)上的Borel集组成的\(\sigma\)域，
\(U\)是\([-\pi,\pi]\)上的Lebegue测度。
定义内积为
\[\begin{aligned}
<f, g> = E(f \bar g)
= \frac{1}{2\pi} \int\_{-\pi}^\pi f(x) \bar g(x) dx.
\end{aligned}\]
这时\(\{ e\_n = e^{inx}, n \in \mathbb Z \}\)构成标准正交基。
如果\(f \in L^2[-\pi, \pi]\)且
\[\begin{aligned}
< f, e\_j > = 0, \ \forall j \in \mathbb Z
\end{aligned}\]
则
\[\begin{aligned}
f(x) = 0, \ \text{a.e.}
\end{aligned}\]

对\(f \in L^2[-\pi, \pi]\),
令
\[\begin{aligned}
S\_n f = \sum\_{j=-n}^n <f, e\_j> e\_j,
\end{aligned}\]
其中
\[\begin{aligned}
<f, e\_j> = \frac{1}{2\pi} \int\_{-\pi}^\pi e^{-ijx} f(x) dx
\end{aligned}\]
叫做\(f\)的Fourier系数，
Fourier系数列必平方可和。
\(S\_n f\)叫做\(f\)的\(n\)阶Fourier逼近，
\(S\_n f\)是\(f\)在
\(\overline{\mbox{sp}}\{e\_j, |j| \leq n \}\)上的投影。

\(S\_n f\)均方极限存在且等于\(f\)。
\(S\_n f\)的极限写成函数级数
\[\begin{aligned}
S f = \sum\_{j=-\infty}^\infty <f, e\_j> e\_j.
\end{aligned}\]

\[\begin{aligned}
L^2[-\pi, \pi] = \overline{\mbox{sp}}\{ e\_j, j \in \mathbb Z \}.
\end{aligned}\]

\[\begin{aligned}
\| f \|^2 = \sum\_{j=-\infty}^\infty | < f, e\_j > |^2.
\end{aligned}\]

\[\begin{aligned}
<f, g> = \sum\_{j=-\infty}^\infty <f, e\_j> \cdot \overline{<g, e\_j>}.
\end{aligned}\]

若\(f(x)\)是以\(2\pi\)为周期的连续函数，
则任给\(\varepsilon>0\)，
存在三角多项式
\[\begin{aligned}
T\_n(x) = \frac{a\_0}{2} + \sum\_{j=1}^{n} \left\{
a\_j \cos(jx) + b\_j \sin(jx) \right\}
\end{aligned}\]
使得
\[\begin{aligned}
|f(x) - T\_n(x)| < \varepsilon,\ \forall x \in (-\infty,\infty)
\end{aligned}\]
事实上，
\[\begin{aligned}
n^{-1}(S\_0 f + S\_1 f + \dots S\_{n-1} f) \to f
\end{aligned}\]
在\([-\pi,\pi]\)一致收敛\((n\to\infty)\)。

若\(f(x)\)是以\(2\pi\)为周期的连续函数，
且\(f' \in L^2[-\pi,\pi]\)，
则\(S\_n f\)不仅均方收敛到\(f\)，
而且绝对一致收敛到\(f\)。
(见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book))§2.8, §2.11)。

对于以\(2\pi\)为周期的函数\(f(x)\)，
如果在\([-\pi, \pi]\)上可积（有瑕点时绝对可积），
则可以计算
\[\begin{aligned}
a\_n =& \frac{1}{\pi} \int\_{-\pi}^{\pi} f(x) \cos(nx) dx,
\ n=0, 1, 2, \dots \\
b\_n =& \frac{1}{\pi} \int\_{-\pi}^{\pi} f(x) \sin(nx) dx,
\ n=1, 2, \dots
\end{aligned}\]
并形式地写出函数级数
\[\begin{aligned}
\frac{a\_0}{2} + \sum\_{n=1}^{\infty} \left\{
a\_n \cos(nx) + b\_n \sin(nx) \right\}
\end{aligned}\]
但不能保证级数收敛且收敛到\(f(x)\)。

如果\(f(x)\)在\(x=x\_0\)处满足\(\alpha\)级(\(0<\alpha \leq 1\))李普希兹条件：
\[\begin{aligned}
| f(x\_0 \pm t) - f(x\_0) | \leq L t^\alpha, \ 0<t\leq \delta
\end{aligned}\]
(其中\(L>0, \delta>0\))，
则\(f(x)\)的傅立叶级数在\(x\_0\)处收敛到\(f(x)\)。

若\(f(x)\)在\([a,b]\)逐段可微（除了有限个点外可微，在这些点上有左右导数），
则其傅立叶级数在每个\(x=x\_0\)处均收敛到
\[\begin{aligned}
S\_0 = \frac{f(x\_0+0) + f(x\_0-0)}{2}
\end{aligned}\]
当然，除去不可微的有限个点之外都收敛到\(f(x\_0)\)。

若对点\(x\_0\)存在\(h>0\)使得\(f(x)\)在\([x\_0 - h, x\_0]\)和\([x\_0, x\_0 + h]\)分别单调,
则\(f(x)\)的傅立叶级数在\(x\_0\)收敛到
\[\begin{aligned}
\frac{f(x\_0+0) + f(x\_0-0)}{2}
\end{aligned}\]

若\(f(x)\)逐段单调，则其傅立叶级数对任意\(x\_0\)均收敛到
\[\begin{aligned}
\frac{f(x\_0+0) + f(x\_0-0)}{2}
\end{aligned}\]

若\(f(x)\)在区间\([-\pi,\pi]\)上平方可积，
则\(\forall \varepsilon>0\)，
存在三角多项式\(T(x)\)使得
\[\begin{aligned}
\int\_{-\pi}^\pi | f(x) - T(x) |^2 dx < \varepsilon
\end{aligned}\]

若\(f(x)\)在区间\([-\pi,\pi]\)上黎曼可积或在广义积分意义下平方可积，
设\(S\_n(f,x)\)为其傅立叶级数的部分和，
则
\[\begin{aligned}
\lim\_{n\to\infty} \int\_{-\pi}^\pi
| f(x) - S\_n(f,x) |^2 dx = 0
\end{aligned}\]

## B.7 参变积分

**定理B.8 (参变积分连续性（一）)** 设二元函数\(f(x,y)\)是\([a,b] \times [\alpha, \beta]\)上的连续函数，
则
\[
g(x) = \int\_{\alpha}^{\beta} f(x, y) \,dy
\]
是\([a,b]\)上的连续函数。

**推论(积分号下取极限)** 在定理条件下对\(x\_0 \in [a,b]\)有
\[
\lim\_{x\to x\_0} \int\_{\alpha}^{\beta} f(x, y) \,dy
= \int\_{\alpha}^{\beta} \lim\_{x\to x\_0} f(x, y) \,dy .
\]

如果是广义积分或者瑕积分则需要更强的条件。

**定理B.9 (参变积分连续性（二）)** 设二元函数\(f(x,y)\)是\([a,b] \times [\alpha, \beta]\)上的连续函数，
\(\phi(x)\), \(\psi(x)\)是\([a, b]\)上的连续函数且取值于\([\alpha,\beta]\)，
则
\[
g(x) = \int\_{\phi(x)}^{\psi(x)} f(x, y) \,dy
\]
是\([a,b]\)上的连续函数。

**定理B.10 (积分号下求导)** 设\(f(x,y)\)和\(\frac{\partial f(x,y)}{\partial x}\)都是\([a,b] \times [\alpha, \beta]\)上的连续函数，
则
\[
g(x) = \int\_{\alpha}^{\beta} f(x, y) \,dy
\]
在\([a,b]\)上可微，且
\[
g'(x) = \frac{\partial }{\partial x} \int\_{\alpha}^{\beta} f(x, y) \,dy
= \int\_{\alpha}^{\beta} \frac{\partial f(x,y)}{\partial x} \,dy .
\]

**定理B.11 (参变积分求导)** 设\(f(x,y)\)和\(\frac{\partial f(x,y)}{\partial x}\)都是\([a,b] \times [\alpha, \beta]\)上的连续函数，
\(\phi(x)\), \(\psi(x)\)是\([a, b]\)上的可微函数且取值于\([\alpha,\beta]\)，
则
\[
g(x) = \int\_{\phi(x)}^{\psi(x)} f(x, y) \,dy
\]
在\([a,b]\)上可微，且
\[\begin{aligned}
g'(x) =& \frac{\partial }{\partial x} \int\_{\phi(x)}^{\psi(x)} f(x, y) \,dy \\
=& \int\_{\phi(x)}^{\psi(x)} \frac{\partial f(x,y)}{\partial x} \,dy
+ f(x, \psi(x)) \psi'(x) - f(x, \phi(x)) \phi'(x) .
\end{aligned}\]

## B.8 向量和矩阵的微分

### B.8.1 关于向量的微分

对\(f : \mathbb R^p \rightarrow \mathbb R\),
记\(\frac{\partial f(\boldsymbol x)}{\partial \boldsymbol x}\)
为\(f\)的\(p\)个一阶偏导数组成的列向量，
称为\(f\)的梯度，
记一阶偏导数组成的行向量为
\(\frac{\partial f(\boldsymbol x)}{\partial \boldsymbol x^T}\)。

记\(\frac{\partial^2 f(\boldsymbol x)}{\partial \boldsymbol x \partial \boldsymbol x^T}\)
为\(f\)的二阶偏导数组成的\(p \times p\)矩阵，
称为\(f\)的海色阵(Hessian)。

设\(\boldsymbol a\)为\(p\)维列向量，\(A\)为\(p \times p\)对称阵，
则
\[\begin{align\*}
& \frac{\partial (\boldsymbol a^T \boldsymbol x )}{\partial \boldsymbol x} = \boldsymbol a,
\quad \frac{\partial (\boldsymbol x^T \boldsymbol a )}{\partial \boldsymbol x} = \boldsymbol a, \\
& \frac{\partial (\boldsymbol x^T A \boldsymbol x)}{\partial \boldsymbol x}
= 2 A \boldsymbol x, \\
& \frac{\partial^2 (\boldsymbol x^T A \boldsymbol x)}{\partial \boldsymbol x \partial \boldsymbol x^T}
= 2 A .
\end{align\*}\]

### B.8.2 关于矩阵的微分

设\(f(\boldsymbol X)\)是以矩阵\(\boldsymbol X = (x\_{ij})\_{m \times n}\)为自变量的实值函数，
关于各矩阵元素可导，
记\(\frac{\partial f(\boldsymbol X)}{\partial \boldsymbol X}\)
表示\(f\)关于每个元素\(x\_{ij}\)的偏导数组成的矩阵，
即
\[
\left( \frac{\partial f(\boldsymbol X)}{\partial x\_{ij}} \right)\_{m \times n} .
\]

性质：

对\(\boldsymbol X\_{m\times n}\),
\[\begin{aligned}
& \frac{\partial f(\boldsymbol X)}{\partial \boldsymbol X^T}
= \left( \frac{\partial f(\boldsymbol X)}{\partial \boldsymbol X} \right)^T . \\
\end{aligned}\]

对\(\boldsymbol X\_{m\times n}\)和\(\boldsymbol A\_{n \times m}\)，
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol X \boldsymbol A)}{\partial \boldsymbol X}
= \frac{\partial \text{tr}(\boldsymbol A \boldsymbol X)}{\partial \boldsymbol X}
= \boldsymbol A^T,
\end{aligned}\]
对\(\boldsymbol X\_{m\times n}\)和\(\boldsymbol A\_{m \times n}\)，
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol X^T \boldsymbol A)}{\partial \boldsymbol X}
= \frac{\partial \text{tr}(\boldsymbol A \boldsymbol X^T)}{\partial \boldsymbol X}
= \boldsymbol A . \\
\end{aligned}\]

对\(\boldsymbol X\_{m\times n}\),
\(\boldsymbol A\_{p\times m}\),
\(\boldsymbol B\_{n\times p}\),
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol A \boldsymbol X \boldsymbol B)}{\partial \boldsymbol X}
= \frac{\partial \text{tr}(\boldsymbol B \boldsymbol A \boldsymbol X)}{\partial \boldsymbol X}
= \boldsymbol A^T \boldsymbol B^T . \\
\end{aligned}\]

对\(\boldsymbol X\_{m\times n}\)和对称阵\(\boldsymbol A\_{n\times n}\),
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol X \boldsymbol A \boldsymbol X^T)}{\partial \boldsymbol X}
= 2 \boldsymbol X \boldsymbol A .
\end{aligned}\]

对\(\boldsymbol X\_{m\times n}\),
\(\boldsymbol A\_{n\times m}\),
\(\boldsymbol B\_{n\times m}\),
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol X \boldsymbol A \boldsymbol X \boldsymbol B)}{\partial \boldsymbol X}
= \boldsymbol B^T \boldsymbol X^T \boldsymbol A^T
+ \boldsymbol A^T \boldsymbol X^T \boldsymbol B^T .
\end{aligned}\]

对\(\boldsymbol X\_{m\times n}\),
\(\boldsymbol A\_{n\times n}\),
\(\boldsymbol B\_{m\times m}\),
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol X \boldsymbol A \boldsymbol X^T \boldsymbol B)}{\partial \boldsymbol X}
= \boldsymbol B^T \boldsymbol X \boldsymbol A^T
+ \boldsymbol B \boldsymbol X \boldsymbol A .
\end{aligned}\]

对\(\boldsymbol X\_{m\times n}\),
\(\boldsymbol B\_{m\times m}\),
\[\begin{aligned}
& \frac{\partial \text{tr}(\boldsymbol X^T \boldsymbol X \boldsymbol B)}{\partial \boldsymbol X}
= \boldsymbol X ( \boldsymbol B + \boldsymbol B^T) .
\end{aligned}\]

对可逆的\(m\times m\)矩阵\(\boldsymbol X\)，有
\[\begin{aligned}
\frac{\partial \log \text{det}(\boldsymbol X)}{\partial \boldsymbol X}
=& (\boldsymbol X^T)^{-1}, \\
\frac{\partial \text{det}(\boldsymbol X^{-1})}{\partial \boldsymbol X}
=& -\frac{1}{\text{det}(\boldsymbol X)} (\boldsymbol X^T)^{-1}, \\
\frac{\partial \text{det}(\boldsymbol X^{-1})}{\partial \boldsymbol X^{-1}}
=& -\text{det}(\boldsymbol X) \boldsymbol X^T . \\
\end{aligned}\]

○○○○○○

### References

Brockwell, P. J., and R. A. Davis. 1987. *Time Series: Theory and Methods*. Springer-Verlag.