---
crawl_time: '2026-01-17 14:28:39'
framework: sphinx
title: 18 概率极限 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-problim.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 18 概率极限

## 18.1 几乎必然收敛

**定理18.1** 随机向量序列\(\boldsymbol\xi\_n\) a.s. 收敛到\(\boldsymbol\xi\)，
\(g(\cdot)\)为\(\mathbb R^n \to \mathbb R\)的连续函数，
则\(g(\boldsymbol\xi\_n)\) a.s. 收敛到\(g(\boldsymbol\xi)\)。

如果\(g(\cdot)\)定义是开集或者闭集\(G\)上的连续函数，
\(\boldsymbol\xi\_n\)和\(\boldsymbol\xi\)都取值于\(G\)，
则定理结论也成立。

## 18.2 依概率收敛

**定理18.2** 设\(\{ a\_n \}\)为实数列，则\(a\_n \stackrel{\mbox{P}}{\rightarrow} a\)
等价于\(\lim\_{n\to\infty} a\_n = a\)。

**证明**
充分性。设\(\lim\_n a\_n = a\)，则\(\forall \delta>0\),
\(\exists N\)使得\(n > N\)时\(|a\_n - a| \leq \delta\)。
所以
\[\begin{aligned}
\lim\_n P(|a\_n - a| > \delta) = \lim\_n 0 = 0.
\end{aligned}\]

必要性。
设\(a\_n \stackrel{\mbox{P}}{\rightarrow} a\)。
\(\forall \delta > 0\),
记\(p\_n = P(|a\_n - a| > \delta)\),
则\(\lim\_n p\_n = 0\)。
但是\(p\_n\)只能取0或者1，
所以\(\{ p\_n \}\)中只有有限个1，
所以存在\(N\)使得\(n>N\)时\(P(|a\_n - a| > \delta)=0\),
即\(n>N\)时\(|a\_n - a| \leq \delta\)，
即\(\lim\_n a\_n = a\)。

○○○○○○

**定理18.3** 设\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} \xi\),
\(\eta\_n \stackrel{\mbox{P}}{\rightarrow} \eta\),
则
\[\begin{aligned}
\xi\_n + \eta\_n \stackrel{\mbox{P}}{\rightarrow} \xi + \eta.
\end{aligned}\]

**证明**
任给定\(\varepsilon>0\)。
\[\begin{aligned}
& P(|(\xi\_n + \eta\_n) - (\xi+\eta)| \geq \varepsilon) \\
\leq& P(|\xi\_n - \xi| + |\eta\_n - \eta| \geq \varepsilon) \\
\leq& P(|\xi\_n - \xi| \geq \frac{\varepsilon}{2}
\text{ 或 } |\eta\_n - \eta| \geq \frac{\varepsilon}{2}) \\
\leq& P(|\xi\_n - \xi| \geq \frac{\varepsilon}{2})
+ P(|\eta\_n - \eta| \geq \frac{\varepsilon}{2}) \\
\to& 0, \quad(n\to\infty)
\end{aligned}\]

○○○○○○

**定理18.4** 设\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} \xi\),
\(a\)为常数,
则
\[\begin{aligned}
a \xi\_n \stackrel{\mbox{P}}{\rightarrow} a \xi.
\end{aligned}\]

**证明** \(a=0\)时显然。
当\(a \neq 0\)时，\(\forall \varepsilon>0\),
\[\begin{aligned}
&P(|a \xi\_n - a \xi| \geq \varepsilon) \\
=& P(|\xi\_n - \xi| \geq \frac{\varepsilon}{|a|}) \to 0
\quad(n\to\infty)
\end{aligned}\]

**引理18.1** 设\(\{\xi\_n \}\), \(\{ \eta\_n \}\)为两个随机序列，
\(a, b, c\)为常数，
若\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} \xi\),
\(\eta\_n \stackrel{\mbox{P}}{\rightarrow} \eta\),
则
\(a \xi\_n + b\eta\_n + c \stackrel{\mbox{P}}{\rightarrow} a\xi + b \eta + c\)。

**定理18.5** 设\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} a\),
\(a\)为常数，
函数\(g(\cdot)\)在\(a\)连续，则
\[\begin{aligned}
g(\xi\_n) \stackrel{\mbox{P}}{\rightarrow} g(a).
\end{aligned}\]

**证明** \(\forall \varepsilon>0\),
\(\exists \delta>0\)使得\(|x-a|<\delta\)时\(|g(x) - g(a)| < \varepsilon\)。
于是
\[\begin{aligned}
|g(x) - g(a)| \geq \varepsilon \Longrightarrow
|x-a| \geq \delta
\end{aligned}\]
于是
\[\begin{aligned}
P(|g(\xi\_n) - g(a)| \geq \varepsilon)
\leq P(|\xi\_n - a| \geq \delta) \to 0,
\quad(n\to\infty)
\end{aligned}\]

○○○○○○

例如，若\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} a\),
则
\[\begin{aligned}
\xi\_n^2 \stackrel{\mbox{P}}{\rightarrow}& a^2 \\
1/\xi\_n \stackrel{\mbox{P}}{\rightarrow}& 1/a \quad(\text{只要}a\neq 0)\\
\sqrt{\xi\_n} \stackrel{\mbox{P}}{\rightarrow}& \sqrt{a}
\quad(\text{只要} a\geq 0)
\end{aligned}\]

**定理18.6** 设\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} \xi\),
\(g(\cdot)\)为连续函数，则
\[\begin{aligned}
g(\xi\_n) \stackrel{\mbox{P}}{\rightarrow} g(\xi).
\end{aligned}\]

证明参考Tucker, H.G.(1967), A Graduate Course in Probability,
New York: Academic Press.

**定理18.7** 设\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} \xi\),
\(\eta\_n \stackrel{\mbox{P}}{\rightarrow} \eta\),
则
\[\begin{aligned}
\xi\_n \eta\_n \stackrel{\mbox{P}}{\rightarrow} \xi \eta.
\end{aligned}\]

**证明**

\[\begin{aligned}
\xi\_n \eta\_n =& \frac{1}{2}\left[
\xi\_n^2 + \eta\_n^2 - (\xi\_n - \eta\_n)^2 \right]
\end{aligned}\]
其中\(\xi\_n^2 \stackrel{\mbox{P}}{\rightarrow} \xi^2\),
\(\eta\_n^2 \stackrel{\mbox{P}}{\rightarrow} \eta^2\),
\(\xi\_n - \eta\_n \stackrel{\mbox{P}}{\rightarrow} \xi - \eta\),
\((\xi\_n - \eta\_n)^2 \stackrel{\mbox{P}}{\rightarrow} (\xi - \eta)^2\),
所以
\[\begin{aligned}
\xi\_n \eta\_n \stackrel{\mbox{P}}{\rightarrow}&
\frac{1}{2}\left[
\xi^2 + \eta^2 - (\xi-\eta)^2 \right] = \xi \eta
\end{aligned}\]

○○○○○○

## 18.3 依分布收敛

**定理18.8** 设\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} \xi\),
则\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\)。

**证明**
设\(\xi\_n \sim F\_n(\cdot)\), \(\xi \sim F(\cdot)\),
设\(x\)为\(F(\cdot)\)的一个连续点。
对任意\(\epsilon>0\),
\[\begin{aligned}
F\_n(x) =& P(\xi\_n \leq x) \\
=& P\left( \xi\_n \leq x, \ |\xi\_n - \xi| < \epsilon \right)
+ P\left( \xi\_n \leq x, \ |\xi\_n - \xi| \geq \epsilon \right) \\
\leq& P(\xi \leq x+\epsilon) + P(|\xi\_n - \xi| \geq \epsilon)
\end{aligned}\]
于是
\[\begin{aligned}
\varlimsup\_{n\to\infty} F\_n(x) \leq F(x+\epsilon).
\end{aligned}\]
另一方面，
\[\begin{aligned}
P(\xi\_n>x)
=& P(\xi\_n > x, \ |\xi\_n - \xi| < \epsilon)
+ P(\xi\_n > x, \ |\xi\_n - \xi| \geq \epsilon) \\
\leq& P(\xi > x - \epsilon) + P(|\xi\_n - \xi| \geq \epsilon), \\
\varlimsup\_{n\to\infty} \left[ 1 - F\_n(x) \right]
\leq& 1 - F(x - \epsilon), \\
\varliminf\_{n\to\infty} F\_n(x)
\geq& F(x - \epsilon),
\end{aligned}\]
总之有
\[\begin{aligned}
F(x-\epsilon) \leq \varliminf\_{n\to\infty} F\_n(x)
\leq \varlimsup\_{n\to\infty} F\_n(x) \leq F(x+\epsilon),
\end{aligned}\]
令\(\epsilon \to 0+\)则可知
\[\begin{aligned}
\lim\_{n\to\infty} F\_n(x) = F(x).
\end{aligned}\]

○○○○○○

**定理18.9** 若\(a\)是常数，\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} a\),
则\(\xi\_n \stackrel{\mbox{P}}{\rightarrow} a\)。

**证明**
记
\[\begin{aligned}
F(x) =& \begin{cases}
1, & x \geq a \\
0, & x < a
\end{cases}
\end{aligned}\]
则
\[\begin{aligned}
P(\xi\_n \leq x) \to F(x), \ \forall x \neq a.
\end{aligned}\]

\(\forall \delta>0\),
\[\begin{aligned}
& P(|\xi\_n - a| > \delta) \\
=& P(\xi\_n > a + \delta) + P(\xi\_n < a - \delta) \\
=& 1 - P(\xi\_n \leq a + \delta) + P(\xi\_n < a - \delta) \\
\leq& 1 - P(\xi\_n \leq a + \delta) + P(\xi\_n \leq a - \delta) \\
\to& 0 \quad(n \to \infty)
\end{aligned}\]

○○○○○○

依分布收敛不一定依概率收敛。
比如，设\(X \sim \text{N}(0,1)\)，
则\(-X\)与\(X\)同分布。
令
\[\begin{aligned}
X\_n = \begin{cases}
X, & \text{$n$为偶数}, \\
-X, & \text{$n$为奇数},
\end{cases}
\end{aligned}\]
则\(\{ X\_n \}\)依分布收敛到\(X\)，
但是不依概率收敛到\(X\)。

概率质量函数(PMF)的收敛性与分布函数收敛性不同。
例如，取\(\xi\_n = 2 + \frac{1}{n}\)，
则\(\xi\_n\)的PMF为
\[\begin{aligned}
p\_n(x) = \begin{cases}
1, & x = 2+\frac{1}{n}, \\
0, & \text{其它}
\end{cases},
\end{aligned}\]
且
\[\begin{aligned}
\lim\_n p\_n(x) = 0, \ x \in (-\infty, \infty),
\end{aligned}\]
但是\(\xi\_n\)的分布函数趋于\(\xi=2\)的分布函数。

如果\(\xi\_n\)的密度函数\(p\_n(x) \to p(x)\)，
\(p(x)\)为\(\xi\)的密度函数，\(p\_n(x)\)有可积的上界，
则根据控制收敛定理可知，
\(\xi\_n\)依分布收敛到\(\xi\)。

**定理18.10** 设\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\),
\(\eta\_n \stackrel{\mbox{P}}{\rightarrow} 0\),
则
\[\begin{aligned}
\xi\_n + \eta\_n \stackrel{\mbox{d}}{\rightarrow} \xi.
\end{aligned}\]

**证明**
设\(x\_0\)是\(\xi\)的分布函数\(F(x)\)的连续点。
对于\(\delta>0\)，由
\[\begin{aligned}
& P(\xi\_n + \eta\_n \leq x\_0) \\
=& P(\xi\_n + \eta\_n \leq x\_0,\ \eta\_n \leq -\delta)
+ P(\xi\_n + \eta\_n \leq x\_0,\ \eta\_n > -\delta) \\
\leq& P(\eta\_n \leq -\delta) + P(\xi\_n \leq x\_0 + \delta) \\
\leq& P(|\eta\_n| \geq \delta) + P(\xi\_n \leq x\_0 + \delta)
\end{aligned}\]
可知
\[\begin{align}
& P(\xi\_n + \eta\_n \leq x\_0) - F(x\_0) \\
\leq& [ P(\xi\_n \leq x\_0 + \delta) - F(x\_0 + \delta) ]
+ [ F(x\_0 + \delta) - F(x\_0) ]
+ P(|\eta\_n| \geq \delta)
\tag{18.1}
\end{align}\]

另一方面，
\[\begin{aligned}
& P(\xi\_n + \eta\_n \leq x\_0) \\
\geq& P(\xi\_n + \eta\_n \leq x\_0,\ \eta\_n \leq \delta) \\
\geq& P(\xi\_n + \delta \leq x\_0,\ \eta\_n \leq \delta) \\
=& P(\xi\_n + \delta \leq x\_0 )
- P(\xi\_n + \delta \leq x\_0,\ \eta\_n > \delta) \\
\geq& P(\xi\_n \leq x\_0 - \delta) - P(\eta\_n > \delta) \\
\geq& P(\xi\_n \leq x\_0 - \delta) - P(|\eta\_n| > \delta)
\end{aligned}\]
于是
\[\begin{align}
& P(\xi\_n + \eta\_n \geq x\_0) - F(x\_0) \\
\geq& P(\xi\_n \leq x\_0 - \delta) - F(x\_0 - \delta)
+ F(x\_0 - \delta) - F(x\_0) - P(|\eta\_n| > \delta)
\tag{18.2}
\end{align}\]

任给定\(\varepsilon>0\)，取\(\delta\_1>0\)足够小使得
\[\begin{aligned}
F(x\_0 + \delta\_1) - F(x\_0) < \frac{\varepsilon}{3}
\end{aligned}\]
且\(x\_0 + \delta\_1\)是\(F(x)\)的连续点（由Lebesgue定理，
单调函数几乎处处可微，从而在任意小的区间上都有连续点）。
由于
\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\),
\(\eta\_n \stackrel{\mbox{P}}{\rightarrow} 0\),
存在\(n\_1\)使得\(\forall n \geq n\_1\)，
\[\begin{aligned}
P(\xi\_n \leq x\_0 + \delta) - F(x\_0 + \delta) <& \frac{\varepsilon}{3} \\
P(|\eta\_n| \geq \delta) <& \frac{\varepsilon}{3}
\end{aligned}\]
由[(18.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-problim.html#eq:arest-app-limindist-sump01)式,
当\(n \geq n\_1\)时
\[\begin{aligned}
P(\xi\_n + \eta\_n \leq x\_0) - F(x\_0) < \varepsilon
\end{aligned}\]

再取\(\delta\_2 > 0\)使
\[\begin{aligned}
F(x\_0) - F(x\_0 - \delta\_2) < \frac{\varepsilon}{3}
\end{aligned}\]
且\(x\_0 - \delta\_2\)是\(F(x)\)的连续点，存在\(n\_2 \geq n\_1\)使得
\(\forall n \geq n\_2\)有
\[\begin{aligned}
P(\xi\_n \leq x\_0 - \delta) - F(x\_0 - \delta) >& - \frac{\varepsilon}{3} \\
P(|\eta\_n| \geq \delta) <& \frac{\varepsilon}{3}
\end{aligned}\]
由[(18.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-problim.html#eq:arest-app-limindist-sump02)可得当\(n \geq n\_2\)时
\[\begin{aligned}
P(\xi\_n + \eta\_n \leq x\_0) - F(x\_0) > -\varepsilon
\end{aligned}\]
于是当\(n \geq n\_2\)时
\[\begin{aligned}
|P(\xi\_n + \eta\_n \leq x\_0) - F(x\_0)| < \varepsilon
\end{aligned}\]
即有
\[\begin{aligned}
\lim\_{n\to\infty} P(\xi\_n + \eta\_n \leq x\_0) = F(x\_0)
\end{aligned}\]
即
\[\begin{aligned}
\xi\_n + \eta\_n \stackrel{\mbox{d}}{\rightarrow} \xi.
\end{aligned}\]

○○○○○○

**定理18.11** 设\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\),
\(\eta\_n \stackrel{\mbox{P}}{\rightarrow} 1\),
则\(\eta\_n \xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\)。

证明与定理[18.10](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-problim.html#thm:estar-problim-bydist-sum)类似。

**定理18.12** 设\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\),
\(g(\cdot)\)是定义在\(\xi\)的支撑集上的连续函数，
则
\[\begin{aligned}
g(\xi\_n) \stackrel{\mbox{d}}{\rightarrow} g(\xi).
\end{aligned}\]

证明略。例如，\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \text{N}(0,1)\)，
则\(\xi\_n^2 \stackrel{\mbox{d}}{\rightarrow} \chi^2(1)\)。

**定理18.13** 设
\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\),
\(A\_n \stackrel{\mbox{P}}{\rightarrow} a\),
\(B\_n \stackrel{\mbox{P}}{\rightarrow} b\),
则
\[\begin{aligned}
A\_n + B\_n \xi\_n \stackrel{\mbox{d}}{\rightarrow} a + b\xi.
\end{aligned}\]

证明略。

**定理18.14** 设取正整数整数值的随机变量序列\(\{N\_n \}\)满足\(N\_n \leq N\_{n+1}, \forall n\)，
\(\xi\_n\)依分布收敛到\(\xi\)。

(1) 如果\(\{ N\_n \}\)与\(\{ \xi\_n \}\)相互独立，
则\(\xi\_{N\_n}\)依分布收敛到\(\xi\)；

(2) 如果存在\(c>0\)使得\(N\_n / n \to c\), a.s.，
则\(\xi\_{N\_n}\)依分布收敛到\(\xi\)。

其中的第二条称为Anscombe定理。
证明略。

**定理18.15** 设随机变量序列\(\{ \xi\_n \}\)依分布收敛到\(\xi\)，
\(p>0\)，
存在非负随机变量\(\eta\)使得\(E\eta<\infty\)且\(|\xi\_n| \leq \eta, \forall n\)，
则\(E \xi\_n^p \to E \xi^p\)。

**定理18.16** 设随机变量序列\(\{ \xi\_n \}\)依分布收敛到\(\xi\)，
\(p>0\)，
\(E \xi\_n^p\)满足如下的一致可积性条件：
\[
\lim\_{c \to \infty} \sup\_{n \geq 1}
E \left\{ |\xi\_n|^p \boldsymbol 1\_{\{ |\xi\_n|^p > c \}} \right\} = 0
\]
则\(E \xi\_n^p \to E \xi^p\)。

证明略。

**定理18.17** 设\(\{ F\_1, F\_2, \dots, \}\)是分布函数列，
\(F\_n\)依分布收敛到\(F\)，
若对某一个\(\delta>0\)和\(p\_0 > 0\)，
数列\(\{ \int\_R |x|^{p\_0+\delta} d F\_n(x): n \geq 1 \}\)有界,
则对任意\(p \in [0, p\_0]\)和整数\(k \in (0, p\_0]\)，
当\(n \to \infty\)时有
\[\begin{aligned}
\int\_R x^k d F\_n(x) \to& \int\_R x^k d F(x) ,
\quad
\int\_R |x|^p d F\_n(x) \to& \int\_R |x|^p d F(x)
\end{aligned}\]

见朱成熹《测度论基础》（科学出版社1983年）P126的推论。
即二阶矩有界加依分布收敛推出一阶矩收敛；
三阶矩有界加依分布收敛推出二阶矩收敛。

反例：设\(X\_n \sim 2 n \text{U}(0, \frac{1}{n})\)，
则\(X\_n \to 0\), a.s.。
\(EX\_n \equiv 1\)而\(E 0 = 0\)。

依分布收敛可以推广到更一般的情形。
设\((S, \rho)\)是距离空间，
\(\mathscr B(S)\)是包含其中的开集的最小\(\sigma\)代数，
若\(Q\_n\)和\(Q\)是\((S, \mathscr B(S))\)中的概率测度，
满足
\[
\lim\_{n\to\infty} \int\_S g(s) \,dQ\_n(s)
= \int\_S g(s) \,dQ(s)
\]
对任意定义在\(S\)上的实值连续有界函数\(g\)成立，
则称\(Q\_n\)弱收敛到\(Q\)。

\(X\)是概率空间\((\Omega, \mathscr F, P)\)到可测空间\((S, \mathscr B(S))\)的可测函数，
称为随机元。
\(X\)导出了\(S\)中的测度\(Q(A) = P(X^{-1}(A))\)。
若\(P(X\_n^{-1}(\cdot))\)弱收敛到\(P(X^{-1}(\cdot))\)，
则称\(X\_n\)弱收敛到\(X\)（或依分布收敛）。

## 18.4 概率母函数

若\(X\)为取非负整数值的离散型随机变量，
\(P(X=j) = p\_j\),
则
\[
P(t) = E t^X = \sum\_{j=0}^\infty t^j p\_j
\]
在\(t \in [-1, 1]\)一致绝对收敛，
称\(P(t)\)为\(X\)的概率母函数。

**定理18.18** 设取值为非负整数的随机变量\(X\)有概率母函数\(P(t) = E t^X\), \(t \in [-1,1]\),
和分布列\(\{ p\_j \}\)，
则分布列与概率母函数相互唯一决定。

分布列决定概率母函数显然；
反之的证明略。
参见([李贤平 2010](#ref-Lixp2010:Prob))节4.4, ([何书元 2006](#ref-Hesy2006:Prob))节5.1。

\[
\begin{aligned}
EX =& P'(1) \\
E(X(X-1)) =& P''(1) \\
\text{Var}(X) =& EX^2 - (EX)^2 = P''(1) + P'(1) - [P'(1)]^2
\end{aligned}
\]

## 18.5 矩母函数

对随机变量\(X\)，
如果存在\(h>0\)使得
\[
M(t) = E e^{t X}
\]
在\(t \in (-h, h)\)存在，
则称\(M(t)\)为\(X\)的**矩母函数**。
条件也可以放松到在\(t \in [0, h)\)存在，
或者在\(t \in (-h, 0]\)存在。

矩母函数存在时，
\[
\begin{aligned}
\frac{d}{dt} M(t) =& E (X e^{t X}), \quad \frac{d}{dt} M(0) = EX \\
\frac{d^2}{dt^2} M(t) =& E (X^2 e^{t X}), \quad \frac{d^2}{dt^2} M(0) = E(X^2) \\
& \cdots\cdots \\
\frac{d^n}{dt^n} M(t) =& E (X^n e^{t X}), \quad \frac{d^n}{dt^n} M(0) = E(X^n)
\end{aligned}
\]
这也是“矩母函数”的名称来源。

**定理18.19** 设随机变量\(X\)有矩母函数\(M(t) = E e^{t X}\), \(t \in (-h, h)\),
和分布函数\(F(x)\)，
则分布函数与矩母函数相互唯一决定。

分布函数决定矩母函数显然，
矩母函数决定分布函数的证明略。

**定理18.20** 设\(\xi\_n\)有矩母函数\(M\_n(t) = E e^{t \xi\_n}\), \(t \in (-h, h)\),
\(\xi\)有矩母函数\(M(t) = E e^{t \xi}\), \(t \in [-h\_1, h\_1]\),
\(0 < h\_1 \leq h\),
若\(\lim\_{n\to\infty} M\_n(t) = M(t)\), \(\forall t \in [-h\_1, h\_1]\),
则\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\)。

证明略。

## 18.6 特征函数

概率母函数与矩母函数存在时都可以决定分布，
对于研究独立随机变量和的分布有好的性质，
但都不是对所有随机变量存在的。
特征函数具有相同的优良性，
而且对于所有的随机变量都存在，
只不过用到复数，
数学上较复杂。

对随机变量\(X\)，
定义
\[
\phi(t) = E e^{iX}
= E \cos (tX) + i\, E \sin (tX),
\ t \in \mathbb R
\]
称\(\phi(t)\)为\(X\)的**特征函数**。
特征函数存在唯一。

**定理18.21 (逆转公式)** 如果\(X\)的分布函数\(F(x)\)在\(a, b\)连续，则
\[
F(b) - F(a) = \frac{1}{2\pi} \lim\_{T \to \infty}
\int\_{-T}^T \frac{e^{ita} - e^{itb}}{it} \phi(t) \, dt
\]

证明略。

**定理18.22** 随机变量的分布函数与特征函数相互唯一决定。

特征函数的性质：

**定理18.23** 设\(X\)为随机变量, \(\phi(t) = E e^{itX}\)，则

(1) \(\phi(0) = 1\), \(|\phi(t)| \leq 1\), \(\phi(-t) = \overline{\phi(t)}\)。

(2) \(\phi(t)\)在\((-\infty, \infty)\)一致连续。

(3) 如果\(E X^k\)存在，则
\[
\phi^{(k)}(t) = i^k E(X^k e^{itX}),
\quad \phi^{(k)}(0) = i^k E (X^k)
\]

(4) 非负定性：对任意复数\(a\_1, a\_2, \dots, a\_n\)，
和实数\(t\_1, t\_2, \dots, t\_n\)，都有
\[
\sum\_{k=1}^n \sum\_{j=1}^n \phi(t\_k - t\_j) a\_k \bar a\_j \geq 0
\]

(5) 设随机变量\(X\_1, X\_2, \dots, X\_n\)相互独立，
如果\(X\_j\)的特征函数为\(\phi\_j(t)\), 令\(Y = \sum\_{j=1}^n X\_j\)，
则\(Y\)的特征函数为
\[
\phi\_Y(t) = \prod\_{j=1}^n \phi\_j(t) .
\]

见([何书元 2006](#ref-Hesy2006:Prob)) 节5.2。

正态分布N(\(\mu\), \(\sigma^2\))的特征函数为
\[
\phi(t) = \exp\{ i \mu t - \frac12 \sigma^2 t^2 \} .
\]

**定理18.24 (连续性定理)** 设\(\xi\_n\)的特征函数为\(\phi\_n(t)\)，
\(\xi\)的特征函数为\(\phi(t)\)，
则\(\xi\_n \stackrel{d}{\to} \xi\)的充分必要条件是
\[
\lim\_{n\to\infty} \phi\_n(t) = \phi(t), \ \forall t \in (-\infty, \infty)
\]

证明略。

对随机向量\(\boldsymbol X\),
定义其特征函数为
\[
\phi(\boldsymbol t)
= E e^{i \boldsymbol t^T \boldsymbol X},
\ \boldsymbol t \in \mathbb R^n
\]
特征函数有定义。

**定理18.25 (随机向量特征函数性质)** 设\(\boldsymbol X = (X\_1, \dots, X\_n)^T\)，
\(\boldsymbol X\)的特征函数为\(\phi(\boldsymbol t)\),
分量\(X\_j\)的特征函数为\(\phi\_j(t)\)。则

(1) \(\phi(\boldsymbol t)\)与\(\boldsymbol X\)的分布函数相互唯一决定；

(2) \(X\_1, X\_2, \dots, X\_n\)相互独立当且仅当
\[
\phi(\boldsymbol t) = \phi\_1(t\_1) \phi\_2(t\_2) \dots \phi\_n(t\_n),
\ \forall \boldsymbol t = (t\_1, t\_2, \dots, t\_n) \in \mathbb R^n .
\]

(3) 设\(\{\boldsymbol\xi\_k \}\)为随机向量序列，
\(\boldsymbol\xi\_k\)的特征函数为\(\phi\_k(\boldsymbol t)\)，
如果\(\phi\_k(\boldsymbol t)\)收敛到在\(\boldsymbol t = \boldsymbol 0\)连续的函数\(g(\boldsymbol t)\),
则\(g(\boldsymbol t)\)是某个随机向量\(\boldsymbol\xi\)的特征函数，
且对任意\(\boldsymbol a \in \mathbb R^n\)，
有\(\boldsymbol a^T \boldsymbol\xi\_k \stackrel{\mbox{d}}{\longrightarrow} \boldsymbol a^T \boldsymbol\xi\)。

## 18.7 中心极限定理

**定理18.26** 设\(\{ \xi\_n \}\)为独立同分布的随机变量序列，
\(\mu = E\xi\_1\), \(\sigma^2 = \text{Var}(\xi\_1) < \infty\)，
\(\bar \xi\_n = \frac{1}{n} \sum\_{i=1}^n \xi\_i\)，
则
\[
\frac{\xi\_n - \mu}{\sigma / \sqrt{n}}
\stackrel{\mbox{d}}{\longrightarrow}
\mbox{N}(0,1) .
\]

**定理18.27** 设\(\{ \xi\_n \}\)为独立同分布的随机变量序列，
\(\mu = E\xi\_1\), \(\sigma^2 = \text{Var}(\xi\_1) < \infty\)，
\(\bar \xi\_n = \frac{1}{n} \sum\_{i=1}^n \xi\_i\)，
设\(\hat\sigma\_n^2\)依概率收敛到\(\sigma^2\)，
则
\[
\frac{\xi\_n - \mu}{\hat\sigma\_n / \sqrt{n}}
\stackrel{\mbox{d}}{\longrightarrow}
\mbox{N}(0,1) .
\]

## 18.8 依概率有界

设\(\{\xi\_n\}\)是随机变量序列，
如果对任意\(\varepsilon>0\)，
存在正数\(M\)，使得
\[\begin{aligned}
\sup\_n P(|\xi\_n|>M) \leq \varepsilon
\end{aligned}\]
就称随机变量序列\(\{\xi\_n\}\)是**依概率有界的**，
记做\(\xi\_n = O\_p(1)\)。
对单个的随机变量\(\xi\)，
\(\forall \varepsilon>0\)，
显然存在\(M>0\)使得\(P(|\xi| > M) \leq \varepsilon\)，
一个序列的依概率有界是要求对整个序列，
给定\(\varepsilon>0\)后有共同的\(M\)使得\(P(|\xi\_n| > M) \leq \varepsilon\)同时成立。

设\(\{c\_n\}\)是非零常数列，
如果\(\{\xi\_n / c\_n \} = O\_p(1)\)，
就称\(\xi\_n = O\_p(c\_n)\)。
设随机变量序列\(\eta\_n \neq 0\)，
若\(\{\xi\_n / \eta\_n\} = O\_p(1)\)则称
\(\xi\_n = O\_p(\eta\_n)\)。

依概率有界的**等价定义**：
称\(\{ \xi\_n \}\)依概率有界，
若\(\forall \varepsilon>0\),
\(\exists M>0\)和\(N\)使得当\(n \geq N\)时
\[\begin{aligned}
P(|\xi\_n| \leq M) \geq 1 - \varepsilon.
\end{aligned}\]
事实上，当原定义条件成立时显然此等价定义的条件也成立。
若此等价定义条件成立，
则对\(n \geq N\)有
\[\begin{aligned}
P(|\xi\_n| \leq M) \geq 1 - \varepsilon,
\quad P(|\xi\_n| > M) < \varepsilon.
\end{aligned}\]
对\(j=1,2,\dots,N\)，
存在\(M\_j>0\)使得
\[\begin{aligned}
P(|\xi\_j| > M\_j) < \varepsilon
\end{aligned}\]
令\(M' = \max(M, M\_1, M\_2, \dots, M\_N)\)，
则
\[\begin{aligned}
P(|\xi\_n| > M') <& \varepsilon,
\ \forall n \in \mathbb N\_+, \\
\sup\_{n \in \mathbb N\_+} P(|\xi\_n| > M') \leq& \varepsilon,
\end{aligned}\]
满足原定义。

○○○○○○

若\(\xi\_n \stackrel{\text{Pr}}{\to} 0 \ (n\to\infty)\)
则记\(\xi\_n = o\_p(1)\)。
设\(\{c\_n\}\)是非零常数列，
如果\(\{\xi\_n / c\_n \} = o\_p(1)\)，
就称\(\xi\_n = o\_p(c\_n)\)。
设随机变量序列\(\eta\_n \neq 0\)，
若\(\{\xi\_n / \eta\_n \} = o\_p(1)\)则称\(\xi\_n = o\_p(\eta\_n)\)。

**定理18.28** 若\(\xi\_n = o\_p(c\_n)\)则\(\xi\_n = O\_p(c\_n)\)。

**证明**
按依概率收敛定义，
\(\forall \delta>0\),
\(\forall \varepsilon>0\)，
存在\(N\)使\(n>N\)时
\[\begin{aligned}
P( |\xi\_n / c\_n| > \delta) < \varepsilon
\end{aligned}\]
取\(M \geq \delta\)使得
\[\begin{aligned}
P(\max\_{1 \leq n \leq N}|\xi\_n / c\_n| > M) < \varepsilon,
\end{aligned}\]
则
\[\begin{aligned}
P(|\xi\_n / c\_n| > M) < \varepsilon,
\quad n=1,2,\dots
\end{aligned}\]
即\(\xi\_n/c\_n = O\_p(1)\).

○○○○○○

**定理18.29** 如果\(\xi\_n = O\_p(1)\)而\(c\_n \to \infty\)则
\(\xi\_n / c\_n = o\_p(1)\)。

**证明**
\(\forall \varepsilon > 0\), \(\forall \delta>0\)，
\(\exists M > 0\)使
\[\begin{aligned}
P(|\xi\_n| > M) < \varepsilon,
\ n \in \mathbb N\_+.
\end{aligned}\]
\(\exists N\)使\(n > N\)时\(c\_n > M / \delta\), 于是
\[\begin{aligned}
P \left (\left| \frac{\xi\_n}{c\_n} \right| > \delta \right)
=& P(|\xi\_n| > |c\_n| \delta)
\leq P(|\xi\_n| > M) < \varepsilon.
\end{aligned}\]

○○○○○○

**定理18.30** 对随机变量\(\xi\)，\(\xi=O\_p(1)\).

**定理18.31** 若存在非负随机变量\(\xi\)使得\(|\xi\_n| \leq \xi\) a.s.则\(\xi\_n=O\_p(1)\)。

**证明**
由\(P(|\xi\_n| \leq \xi)=1\)，
\(\forall \varepsilon>0\),
\(\exists M>0\)使\(P(\xi > M)<\varepsilon\)，
于是\(P(|\xi\_n|>M) \leq P(\xi > M) < \varepsilon\),
\(\forall n \in \mathbb N\_+\)。

○○○○○○

**定理18.32** 若存在常数\(d>0\), \(c>0\)使得使得\(\sup\_{n} E|\xi\_n|^d \leq c\),
则\(\xi\_n=O\_p(1)\)。

**证明**
由马尔可夫不等式，
对任意\(M>0\)有
\[
P(|\xi\_n| > M)
\leq \frac{E|\xi\_n|^d}{M^d}
\to 0 \ (M \to\infty)
\]
所以对任意\(\epsilon>0\)，
存在\(M>0\)使得
\[
P(|\xi\_n| > M)
< \epsilon, \ \forall n
\]

○○○○○○

**定理18.33** 若\(\{\xi\_n\}\)同分布，则\(\xi\_n=O\_p(1)\)。

**证明**
\(\forall \varepsilon>0\), \(\exists M>0\)使
\(P(|\xi\_1|>M)<\varepsilon\)。
由同分布性知 \(P(|\xi\_t|>M)=P(|\xi\_1|>M)<\varepsilon\)。

○○○○○○

**定理18.34** \[\begin{aligned}
O\_p(1) \pm O\_p(1) =& O\_p(1) \\
O\_p(1) \cdot O\_p(1) =& O\_p(1) \\
O\_p(1) \pm o\_p(1) =& O\_p(1) \\
O\_p(1) \cdot o\_p(1) =& o\_p(1).
\end{aligned}\]

**证明**
仅证明\(O\_p(1) \cdot o\_p(1) = o\_p(1)\)。
设\(\xi\_n = O\_p(1)\),
\(\eta\_n = o\_p(1)\)。
对任意给定的\(\delta>0\)和\(\epsilon>0\),
存在\(M\), 使得
\[\begin{aligned}
\sup\_n P(|\xi\_n| > M) < \epsilon.
\end{aligned}\]
于是
\[\begin{aligned}
& \varlimsup\_{n\to\infty} P( |\xi\_n \eta\_n| > \delta) \\
\leq& \varlimsup\_{n\to\infty} P( |\xi\_n \eta\_n| > \delta, |\xi\_n| \leq M)
+ \varlimsup\_{n\to\infty} P( |\xi\_n \eta\_n| > \delta, |\xi\_n| > M) \\
\leq& \varlimsup\_{n\to\infty} P( |\eta\_n| > \frac{\delta}{M} )
+ \varlimsup\_{n\to\infty} P( |\xi\_n| > M) \\
\leq& \epsilon,
\end{aligned}\]
即\(\lim\_n P( |\xi\_n \eta\_n| > \delta) = 0\),
\(\xi\_n \eta\_n = o\_p(1)\)。

○○○○○○

**定理18.35** 若\(\xi\_n\)依概率收敛到\(\xi\)则\(\{\xi\_n\} = O\_p(1)\)。

**证明**
\(\xi\_n = \xi + (\xi\_n - \xi) = O\_p(1) + o\_p(1)\)。

○○○○○○

**定理18.36** 设\(\xi\_n \stackrel{\mbox{d}}{\rightarrow} \xi\),
则\(\xi\_n = O\_p(1)\)。

**证明**
设\(\xi\)分布函数为\(F(x)\)，
\(\forall \varepsilon>0\)，
存在\(M>0\)且\(M\)和\(-M\)为\(F(x)\)的连续点，使得
\[\begin{aligned}
\varliminf\_{n\to\infty} P(|\xi\_n| \leq M)
=& \lim\_{n\to\infty} P(\xi\_n \leq M) - \varliminf\_{n\to\infty} P(\xi\_n < -M) \\
\geq& \lim\_{n\to\infty} P(\xi\_n \leq M) - \lim\_{n\to\infty} P(\xi\_n \leq -M) \\
=& F(M) - F(-M) \geq 1 - \frac{\varepsilon}{2} > 1 - \varepsilon
\end{aligned}\]
于是\(\exists N\)，当\(n \geq N\)时
\[\begin{aligned}
\inf\_{m\geq n} P(|\xi\_m| \leq M) \geq& 1-\varepsilon \\
P(|\xi\_n| \leq M) \geq& 1-\varepsilon
\end{aligned}\]
由\(O\_p\)等价定义可知\(\xi\_n = O\_p(1)\)。

○○○○○○

**定理18.37** 设\(\xi\_n = O\_p(1)\)，
\(\eta\_n / \xi\_n = o\_p(1)\),
则\(\eta\_n = o\_p(1)\)。

**证明**

\[
\eta\_n = o\_p(1) \cdot O\_p(1) = o\_p(1) .
\]
也可以直接证明如下。

\(\forall \delta > 0\),
\(\forall \varepsilon > 0\)。
由\(\xi\_n = O\_p(1)\)可知存在\(M>0\)使得
\[\begin{aligned}
P(|\xi\_n| > M) < \frac{\varepsilon}{2},
\ \forall n \in \mathbb N\_+.
\end{aligned}\]
由\(\eta\_n / \xi\_n = o\_p(1)\)可知存在\(N>0\)使得\(n > N\)时
\[\begin{aligned}
P(|\eta\_n / \xi\_n| > \delta / M) < \frac{\varepsilon}{2},
\end{aligned}\]
于是\(n > N\)时
\[\begin{aligned}
& P(|\eta| > \delta) \\
=& P(|\eta| > \delta,\ |\xi\_n| \leq M) + P(|\eta| > \delta,\ |\xi\_n| > M) \\
\leq& P(|\eta\_n / \xi\_n| > \delta / M) + P(|\xi\_n| > M) \\
<& \varepsilon
\end{aligned}\]
即\(\eta\_n = o\_p(1)\)。

○○○○○○

## 18.9 Delta方法

**定理18.38** 设随机变量序列\(\{ \xi\_n \}\)有极限分布
\[\begin{aligned}
\sqrt{n}(\xi\_n - \theta) \stackrel{\text{d}}{\to} \text{N}(0, \sigma^2)
\end{aligned}\]
函数\(g(x)\)在\(\theta\)处可微，
\(g'(\theta) \neq 0\)。
则
\[\begin{aligned}
\sqrt{n}(g(\xi\_n) - g(\theta)) \stackrel{\text{d}}{\to} \text{N}(0, \sigma^2 (g'(\theta))^2).
\end{aligned}\]

**证明**
由泰勒公式
\[\begin{aligned}
g(\xi\_n) = g(\theta) + g'(\theta) (\xi\_n - \theta) + \eta\_n
\end{aligned}\]
其中
\[\begin{aligned}
\eta\_n =& h(\xi\_n - \theta) \\
h(x) =& g(x + \theta) - g(\theta) - g'(\theta) x \\
h'(0) =& 0
\end{aligned}\]
由后面的引理[18.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-problim.html#lem:estar-problim-delta-lemdiriv)可知
\(\eta\_n = o\_p(|\xi\_n - \theta|)\)，于是
\[\begin{aligned}
\sqrt{n}(g(\xi\_n) - g(\theta))
= \sqrt{n} g'(\theta) (\xi\_n - \theta) + o\_p(\sqrt{n} |\xi\_n - \theta|)
\end{aligned}\]
前一项依分布收敛到\(\text{N}(0, \sigma^2 (g'(\theta))^2)\),
后一项中\(\sqrt{n} |\xi\_n - \theta| = O\_p(1)\)所以是\(o\_p(1)\)的，
于是结果可得。

**引理18.2** 设函数\(h(x)\)在\(x=0\)处可微，\(h'(0)=0\)。
若\(\xi\_n = o\_p(1)\)则\(h(\xi\_n) = o\_p(|\xi\_n|)\)。

**引理证明**
\(\forall \varepsilon>0\), \(\forall \delta>0\)，
由\(h'(0)=0\)可知存在\(\delta\_1 > 0\)使得对任意\(0 < |x| \leq \delta\_1\)有
\[\begin{aligned}
\left| \frac{h(x)}{x} \right| < \delta
\end{aligned}\]
于是
\[\begin{aligned}
& P \left(\left| \frac{h(\xi\_n)}{\xi\_n} \right| > \delta \right) \\
=& P \left(\left| \frac{h(\xi\_n)}{\xi\_n} \right| > \delta, \ |\xi\_n| \leq \delta\_1 \right)
+ P \left(\left| \frac{h(\xi\_n)}{\xi\_n} \right| > \delta, \ |\xi\_n| > \delta\_1 \right) \\
=& P \left( \left| \frac{h(\xi\_n)}{\xi\_n} \right| > \delta, \ |\xi\_n| > \delta\_1 \right) \\
\leq& P(|\xi\_n| > \delta\_1)
\to 0 \quad (n \to \infty)
\end{aligned}\]

○○○○○○

**引理18.3** \[\begin{aligned}
\lim\_{n\to\infty} \left(1 + \frac{b}{n} + o(\frac{1}{n}) \right)^{cn}
= e^{bc}.
\end{aligned}\]

## 18.10 随机向量的极限

**定义18.1** 设\(\{ \boldsymbol\xi\_n \}\)为随机向量序列，
\(\boldsymbol\xi\)为随机向量，如果对任意\(\delta>0\)都有
\[\begin{aligned}
\lim\_{n\to\infty} P( | \boldsymbol\xi\_n - \boldsymbol\xi | > \delta ) = 0,
\end{aligned}\]
则称\(\boldsymbol\xi\_n\)依概率收敛到\(\boldsymbol\xi\)，
记作\(\boldsymbol\xi\_n \stackrel{\text{P}}{\to} \boldsymbol\xi\)。

定义中\(| \boldsymbol\xi\_n - \boldsymbol\xi |\)两边的竖线代表欧式长度。

**定理18.39** 设\(\boldsymbol\xi\_n = (\xi\_{n1}, \dots, \xi\_{nm})^T\),
\(\boldsymbol\xi = (\xi\_{1}, \dots, \xi\_{m})^T\),
则\(\boldsymbol\xi\_n \stackrel{\text{P}}{\to} \boldsymbol\xi\)
当且仅当\(\xi\_{nj} \stackrel{\text{P}}{\to} \xi\_j\), \(j=1,\dots,m\)。

**定义18.2** 设\(\{ \boldsymbol\xi\_n \}\)为随机向量序列，
\(\boldsymbol\xi\_n\)分布函数为\(F\_n(\boldsymbol x)\),
\(\boldsymbol\xi\)为随机向量，
有分布函数\(F(\boldsymbol x)\),
如果对\(F(\cdot)\)的任意连续点\(\boldsymbol x\)均有
\[\begin{aligned}
\lim\_{n\to\infty} F\_n(\boldsymbol x) = F(\boldsymbol x),
\end{aligned}\]
则称\(\boldsymbol\xi\_n\)依分布收敛到\(\boldsymbol\xi\)或依分布收敛到\(F(\cdot)\)，
记作\(\boldsymbol\xi\_n \stackrel{\text{d}}{\to} \boldsymbol\xi\)
或\(\boldsymbol\xi\_n \stackrel{\text{d}}{\to} F(\cdot)\)。

**定理18.40** 设\(\{ \boldsymbol\xi\_n \}\)为随机向量序列，
\(\boldsymbol\xi\)为随机向量，
\(\boldsymbol\xi\_n \stackrel{\text{d}}{\to} \boldsymbol\xi\),
函数\(g(\boldsymbol x)\)是定义于\(\boldsymbol\xi\)的支撑集上的连续函数，
则\(g(\boldsymbol\xi\_n)\)依分布收敛于\(g(\boldsymbol\xi)\)。

推论：
随机向量依分布收敛，
则相应分量依分布收敛。

**定理18.41** 设\(\boldsymbol\xi\_n\)有矩母函数\(M\_n(\boldsymbol t) = E e^{\boldsymbol t^T \boldsymbol\xi\_n}\),
\(\boldsymbol\xi\)有矩母函数\(M(t) = E e^{\boldsymbol t^T \boldsymbol\xi}\),
若\(\lim\_{n\to\infty} M\_n(\boldsymbol t) = M(\boldsymbol t), \, \| t \| \leq h\)(\(h>0\)),
则\(\boldsymbol\xi\_n \stackrel{\mbox{d}}{\rightarrow} \boldsymbol\xi\)。

**定理18.42 (随机向量的中心极限定理)** 设独立同分布随机向量序列\(\{ \boldsymbol\xi\_n \}\)具有共同的期望\(\boldsymbol\mu\)和协方差阵\(\Sigma\)，
\(\Sigma\)正定，
设共同的矩母函数\(M(\boldsymbol t)\)在\(\boldsymbol 0\)的一个开邻域存在，
令
\[\begin{aligned}
\boldsymbol\eta\_n = \frac{1}{\sqrt{n}} \sum\_{i=1}^n (\boldsymbol\xi\_i - \boldsymbol\mu)
= \sqrt{n}(\bar{\boldsymbol\xi} - \boldsymbol\mu),
\end{aligned}\]
则\(\boldsymbol\eta\_n\)依分布收敛到\(\text{N}\_m(\boldsymbol 0, \Sigma)\)分布。

**定理18.43** 设\(m\)维随机向量序列\(\{ \boldsymbol\xi\_n \}\)渐近\(\text{N}\_m(\boldsymbol\mu, \Sigma)\)分布，
\(A, \boldsymbol b\)为非随机的矩阵和向量，
则\(A \boldsymbol\xi\_n + \boldsymbol b\)渐近\(\text{N}\_m(A\boldsymbol\mu + \boldsymbol b, A \Sigma A^T)\)分布。

**定理18.44** 设\(m\)维随机向量序列\(\{ \boldsymbol\xi\_n \}\)满足
\[\begin{aligned}
\sqrt{n}(\boldsymbol\xi\_n - \boldsymbol\mu\_0) \stackrel{\mbox{d}}{\rightarrow}
\text{N}\_m(\boldsymbol 0, \Sigma),
\end{aligned}\]
设\(\boldsymbol g(\boldsymbol x)\)为一个\(\mathbb R^m\)到\(\mathbb R^k\)的变换(\(k \leq m\)),
把各个一阶偏导数组成一个矩阵
\[\begin{aligned}
B = \left( \frac{\partial g\_i(\boldsymbol x)}{\partial x\_j} \right)\_{
\substack{i=1,\dots,k;\\ j=1,\dots,m}},
\end{aligned}\]
设在\(\boldsymbol\mu\_0\)的某个邻域内，\(B\)的各个元素连续且\(B\)不等于零矩阵，
记\(B\)在\(\boldsymbol x= \boldsymbol\mu\_0\)处的值为\(B\_0\)，则
\[\begin{aligned}
\sqrt{n}(\boldsymbol g(\boldsymbol\xi\_n) - \boldsymbol g(\boldsymbol\mu\_0))
\stackrel{\mbox{d}}{\rightarrow}
\text{N}\_m(\boldsymbol 0, B\_0 \Sigma B\_0^T).
\end{aligned}\]

### References

———. 2006. *概率论*. 北京大学出版社.

李贤平. 2010. *概率论基础*. 第三版. 高等教育出版社.