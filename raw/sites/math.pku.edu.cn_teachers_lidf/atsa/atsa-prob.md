---
crawl_time: '2026-01-17 14:29:02'
framework: sphinx
title: C 概率论 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-prob.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# C 概率论

## C.1 测度与概率

**σ代数**:
设\(\Omega\)为集合，
\(\mathscr F\)是\(\Omega\)的子集组成的非空集合，
称为集类。如果\(\mathscr F\)满足以下条件：

1. 对“余”运算封闭：
   \(\forall A \in \mathscr F\)都有\(A^c \in \mathscr F\)；
2. 对“可数和”运算封闭：
   若\(\{ A\_n: n \geq 1 \} \subset \mathscr F\)，
   则\(\bigcup\_{n=1}^\infty A\_n \subset \mathscr F\)；

则称\(\mathscr F\)为σ域或者σ代数。
由定义易见\(\Omega \in \mathscr F\)，
\(\emptyset \in \mathscr F\)。
\(\{ \Omega, \emptyset \}\)构成最小的σ域。
\(\Omega\)的所有子集组成的集合构成最大的σ域。

称\((\Omega, \mathscr F)\)为可测空间。

若\(\Omega\)为实数域中的区间\(I\)，
包含其中的所有闭区间的最小的σ域称为\(I\)上的Borel σ域,
记作\(\mathscr B(I)\)。
实数域上的Borel σ域记作\(\mathscr B\)。

设\((\Omega, \mathscr F)\)为可测空间，
定义在\(\mathscr F\)上的实值函数\(\mu(\cdot)\)称为一个测度，
如果：

1. \(P(A) \geq 0\), \(\forall A \in \mathscr F\);
2. 对\(\{ A\_n: n \geq 1 \} \subset \mathscr F\)，
   如果\(\{ A\_n \}\)互不相交，
   则\(P(\bigcup\_{n=1}^\infty A\_n) = \sum\_{n=1}^\infty P(A\_n)\),

则称\(\mu(\cdot)\)是可测空间\((\Omega, \mathscr F)\)上的一个**测度**。

对\(\mathscr B\)或\(\mathscr B(I)\)，
定义测度\(\mu(\cdot)\)使得
\[
\mu([a,b]) = b - a,
\]
称\(\mu(\cdot)\)为Lebesgue（勒贝格）测度，
这是区间长度的推广。

在\(\mathscr F\)上定义函数\(P(\cdot)\)，
满足：

1. \(P(A) \geq 0\), \(\forall A \in \mathscr F\);
2. \(P(\Omega) = 1\);
3. 对\(\{ A\_n: n \geq 1 \} \subset \mathscr F\)，
   如果\(\{ A\_n \}\)互不相交，
   则\(P(\bigcup\_{n=1}^\infty A\_n) = \sum\_{n=1}^\infty P(A\_n)\)；

则称\(P(\cdot)\)是可测空间\((\Omega, \mathscr F)\)上的**概率测度**，
称\((\Omega, \mathscr F, P)\)为**概率空间**。
概率测度是满足\(P(\Omega)=1\)的测度。

设\(X = X(\omega)\)是定义在\(\Omega\)上的函数，
如果\(\forall x \in (-\infty, \infty)\)，
事件\(\{\omega: X(\omega) \leq x \} \in \mathscr F\)，
则称\(X\)为测度空间\((\Omega, \mathscr F)\)上的**可测函数**；
对概率空间\((\Omega, \mathscr F, P)\)，
称\(X\)为**随机变量**。
随机变量是可测空间\((\Omega, \mathscr F)\)到可测空间\((\mathbb R, \mathscr B)\)的可测映射。

对\(B \in \mathscr B\)，
令\(P\_X(B) = P(X \in B)\)，
则\((\mathbb R, \mathscr B, P\_X)\)构成概率空间，
称为随机变量\(X\)的样本概率空间。

随机变量\(X\)的**分布函数**为
\[
F(x) = P(X \leq x), \ x \in (-\infty, \infty)
\]
分布函数是单调不减右连续函数，
\(\lim\_{x \to -\infty} F(x) = 0\),
\(\lim\_{x \to \infty} F(x) = 1\)。
满足这样条件的函数必为某个随机变量的分布函数。

设\(g(\cdot)\)为Borel可测函数，
则
\[
E g(X)
= \int\_{-\infty}^\infty g(x) d F(x)
\]
只要右侧的积分存在。

## C.2 矩不等式

### C.2.1 高阶矩存在则低阶矩存在

对\(0 < r\_1 < r\_2\)，
若\(E|X|^{r\_2} < \infty\),
必有\(E|X|^{r\_1} < \infty\)。
事实上，
\[\begin{aligned}
|x|^{r\_1} &
\begin{cases}
\leq 1, & |x| \leq 1 \\
\leq |x|^{r\_2}, & |x| > 1
\end{cases} \\
&\leq 1 + |x|^{r\_2}
\end{aligned}\]
所以\(E|X|^{r\_1} \leq 1 + E|X|^{r\_2}\)。

### C.2.2 \(C\_r\)不等式

记
\[
c\_r =
\begin{cases}
1, & 0 < r \leq 1 \\
2^{r-1}, & r > 1
\end{cases}
\]
则对随机变量\(X, Y\)有
\[
E|X + Y|^r
\leq c\_r (E|X|^r + E|Y|^r)
\]
称上述不等式为\(C\_r\)不等式。

下面证明。
对\(0 < r \leq 1\)，
\[
|a + b|^r
\leq (|a| + |b|)^r
\leq |a|^r + |b|^r
\]

事实上，
只要证明\((|a| + |b|)^r - |a|^r - |b|^r \leq 0\)，
不妨设\(a > 0\), \(b>0\),
只要证明\(f(x) = 1 - [x^r + (1 - x)^r] \geq 0\), \(x \in [0, 1]\)。
易见\(f(0) = f(1) = 0\),
\(f''(x) = r(1-r)(x^{r-2} + (1-x)^{r-2}) > 0, \forall 0 < x < 1\)，
所以\(f(x) \leq 0, 0 \leq x \leq 1\)。

对\(r > 1\)，
有
\[
(a + b)^r \leq 2^{r-1}(|a|^r + |b|^r)
\]
事实上，不妨设\(a>0, b>0\)，只要证明
\((a + b)^r - 2^{r-1}(a^r + b^r) < 0\),
只要证对\(0 \leq x \leq 1\)，
\(f(x) = 1 - 2^{r-1}(x^r + (1-x)^r) \leq 0\)。
易见\(f(0)<0, f(1)<0\)，
\(f''(x) = -2^{r-1} r (r-1) [x^{r-2} + (1-x)^{r-2}] < 0, x \in (0,1)\)，
\(f(x)\)是凹函数，
有唯一的最大值点，
令\(f'(x)=0\)得\(x=\frac12\)为最大值点，
而\(f(\frac12)=0\)，
所以\(f(x) \leq 0\), \(x \in [0,1]\)。

利用\(C\_r\)记号则有如下\(c\_r\)不等式：
\[
(a + b)^r \leq c\_r(|a|^r + |b|^r)
\]
从而对随机变量\(X, Y\)有
\[
E|X + Y|^r
\leq c\_r (E|X|^r + E|Y|^r)
\]

### C.2.3 Hölder不等式

对\(p>1\), \(q>1\), \(\frac{1}{p} + \frac{1}{q} = 1\)，
随机变量\(X, Y\)，
有如下的Hölder不等式:
\[
E|XY|
\leq (E|X|^p)^{\frac{1}{p}} (E|Y|^q)^{\frac{1}{q}}
\]
记\(\| X \|\_p = (E|X|^p)^{\frac{1}{p}}\)，
不等式可以写成
\[
E|XY| \leq \| X \|\_p \; \| Y \|\_q
\]
特别地，
当\(p=q=2\)时为如下的Schwarz不等式：
\[
E|XY| \leq \sqrt{EX^2 EY^2}
\]

证明可以利用如下的Young不等式：

设\(p>1\), \(q>1\), \(\frac{1}{p} + \frac{1}{q} = 1\),
\(a>0\), \(b>0\)，
有
\[
ab \leq \frac{1}{p} a^p + \frac{1}{q} b^q
\]
等号成立当且仅当\(a^p = b^q\)。

**Young不等式证明**:
\(\frac{1}{q} = 1 - \frac{1}{p} = \frac{p-1}{p}\),
\(q = \frac{p}{p-1}\)。
固定\(b>0\)，
令
\[
f(a) = \frac{1}{p} a^p + \frac{1}{q} b^q - ab
\]
只要证明\(f(a) \geq 0\)且等号成立当且仅当\(a = b^{\frac{1}{p-1}}\)。
求导得
\[
f'(a) = a^{p-1} - b
\]
令\(f'(a) = 0\)解得\(a^\* = b^{\frac{1}{p-1}}\)。
对\(a = a^\*\)有
\[\begin{aligned}
f(a^\*) =& \frac{1}{p} b^{\frac{p}{p-1}} + \frac{p-1}{p} b^{\frac{p}{p-1}} - b^{\frac{1}{p-1}} b \\
=& 0
\end{aligned}\]
注意到\(a<a^\*\)时\(f'(a) < 0\), \(a > a^\*\)时\(f'(a)>0\)，
所以\(a=a^\*\)是\(f(a)\)的唯一的严格最小值点，
故
\(f(a) > f(a^\*) = 0\)，对\(a \neq a^\*\)，
而\(f(a^\*)=0\)是唯一的一个取等号的点。
Young不等式证毕。

**Hölder不等式证明**：

对\(p>1\), \(q>1\), \(\frac{1}{p} + \frac{1}{q} = 1\)，
取
\[
a = \frac{|X|}{(E|X|^p)^{\frac{1}{p}}},
b = \frac{|Y|}{(E|Y|^q)^{\frac{1}{q}}}
\]
利用Young不等式有
\[
\frac{|XY|}{(E|X|^p)^{\frac{1}{p}} (E|Y|^q)^{\frac{1}{q}}}
\leq \frac{1}{p} \frac{|X|^p}{E|X|^p)}
+ \frac{1}{q} \frac{|Y|^q}{E|Y|^q}
\]
所以
\[\begin{aligned}
|XY|
\leq& \frac{1}{p} |X|^p (E|X|^p)^{\frac{1}{p} - 1} (E|Y|^q)^{\frac{1}{q}}
+ \frac{1}{q} |Y|^q (E|Y|^q)^{\frac{1}{q} - 1} (E|X|^p)^{\frac{1}{p}}
\end{aligned}\]
两边取期望得
\[\begin{aligned}
E|XY|
\leq& \frac{1}{p} (E|X|^p)^{\frac{1}{p}} (E|Y|^q)^{\frac{1}{q}}
+ \frac{1}{q} (E|Y|^q)^{\frac{1}{q}} (E|X|^p)^{\frac{1}{p}} \\
=& (E|X|^p)^{\frac{1}{p}} (E|Y|^q)^{\frac{1}{q}}
\end{aligned}\]
得证。

### C.2.4 Minkowski不等式

对\(p \geq 1\)，
记\(\| X \|\_p = (E|X|^p)^{\frac{1}{p}}\)，
有
\[
\| X + Y \|\_p \leq \| X \|\_p + \| Y \|\_p
\]

**证明**

\(p=1\)时\(E|X + Y| \leq E|X| + E|Y|\)显然。
对\(p>1\)，利用Hölder不等式，有
\[\begin{aligned}
\| X+Y \|\_p^p
=& ( E|X + Y|^p )^{\frac{1}{p}}
\leq E \left( |X| \cdot |X+Y|^{p-1} \right)
+ E \left( |Y| \cdot |X+Y|^{p-1} \right) \\
\leq& \| X \|\_p \left( E|X+Y|^{(p-1)q} \right)^{\frac{1}{q}}
+ \| Y \|\_p \left( E|X+Y|^{(p-1)q} \right)^{\frac{1}{q}} \\
\end{aligned}\]
注意到\(\frac{1}{q} = \frac{p-1}{p}\), \(q = \frac{p}{p-1}\),
\((p-1)q = p\)，
所以
\[
\| X+Y \|\_p^p
\leq \left( \| X \|\_p + \| Y \|\_p \right) \|X+Y\|\_p^{\frac{p}{q}}
\]
而
\[
p - \frac{p}{q} = p (1 - \frac{1}{q}) = p \frac{1}{p} = 1
\]
所以
\[
\| X+Y \|\_p \leq \| X \|\_p + \| Y \|\_p
\]
成立。

### C.2.5 Jensen不等式

**定理C.1** 设\(g(\cdot)\)为凸函数，
\(X\)为随机变量，
\(Eg(X)\)和\(EX\)存在，则
\[
g(EX) \leq E g(X)
\]
等号成立当且仅当存在常数\(c\)使得\(X=c\), a.s.

证明略。

### C.2.6 \(\log E|X|^p\)是\(p\)的凸函数

对\(p\geq 0\)，
\(g(p) = \log E|X|^p\)是\(p\)的凸函数。

利用Schwarz不等式，
设\(0 \leq p\_1 < p\_2\)，
有
\[\begin{aligned}
\left( E |X|^{\frac{p\_1 + p\_2}{2}} \right)^2
=& \left\{ E\left( |X|^{\frac{p\_1}{2}} |X|^{\frac{2\_1}{2}} \right) \right\}^2 \\
\leq& E |X|^{p\_1} \cdot E |X|^{p\_2}
\end{aligned}\]
取对数得
\[
2 \log E |X|^{\frac{p\_1 + p\_2}{2}}
\leq \log E |X|^{p\_1} + \log E E |X|^{2\_1}
\]
即
\[
g(\frac12 p\_1 + \frac12 p\_2)
\leq \frac12 g(p\_1) + \frac12 g(p\_2)
\]
易见\(g(p)\)关于\(p\)连续，
所以\(g(p) = \log E|X|^p\)是\(p\geq 0\)的凸函数。

## C.3 可测函数极限

随机变量是概率空间\((\Omega, \mathscr F, P)\)上的可测函数。
设\(X = X(\omega)\)是\((\Omega, \mathscr F, P)\)上的可测函数，
允许取\(+\infty\)和\(-\infty\)值。
称\(X\) **a.s.有限**，如果
\(P(X = \pm \infty)=0\)。

设\(X(\omega)\)和\(Y(\omega)\)是\(\Omega\)上的函数，
如果存在零概率事件\(N \in \mathscr F\)，
使得\(\{\omega: X(\omega) \neq Y(\omega) \} \subset N\)，
称\(X\)和\(Y\) **a.s.相等**或者a.e.相等（几乎处处相等）。

如果\(Y(\omega)\)是\(\Omega\)上的函数，
\(X(\omega)\)是随机变量，
使得\(Y\)与\(X\) a.s.相等，
则称\(Y\) **a.s.可测**或几乎处处可测。
注意这时\(\{\omega:\; Y(\omega) \neq X(\omega) \}\)包含于零概率集合但是本身不一定是可测集，
所以几乎处处可测不一定可测，
\(Y\)不一定是随机变量(\((\Omega, \mathscr F)\)可测函数)，
但是在等价性的角度来看可以看成是随机变量。

设\(X\_n\)是随机变量序列且\(P(X\_n = \pm\infty)=0\)，
\(X(\omega)\)是\(\Omega\)上的函数，
若存在零概率集合\(N \in \mathscr F\)使得
\(\{ \omega:\; X\_n(\omega) \not\to X(\omega) \} \subset N\)，
称\(X\_n\)几乎处处收敛到\(X\)，
或者a.s.收敛到\(X\),
\(X\)一定是几乎处处可测的。
事实上，令
\[
\tilde X(\omega) = \begin{cases}
\lim\_n X\_n(\omega), & \omega \notin N \\
0, & \omega \in N
\end{cases}
\]
则\(\tilde X\)可测且与\(X\)几乎处处相等。

如果\(\forall \omega \in \Omega\)，
\(X\_n(\omega) \to X(\omega)\)，
\(X\_n\)可测，
则\(X\)可测。

如果将几乎处处相等的\(\Omega\)上的函数看成同一个函数，
则随机变量序列的几乎处处极限如果存在就是唯一的。
当\(\mathscr F\)对\(P\)是完全的，
几乎处处极限\(X\)是可测的（即随机变量）。

所谓完全(完备)测度，
是指如果\(N \in \mathscr F\)使得\(P(N)=0\)，
则对任何\(A \subset N\)都有\(A \in \mathscr F\)，
称\(\mathscr F\)对测度\(P\)是完全的。

参见朱成熹《测度论基础》，科学出版社1986。

## C.4 多元期望和方差阵的性质

设\(A, B, C\)为矩阵，\(\boldsymbol M\)为随机矩阵，有
\[\begin{aligned}
E( A M B + C) = A E(M) B + C .
\end{aligned}\]

设\(\boldsymbol a, \boldsymbol b\)为实值向量，
\(A, B\)为实值矩阵，\(\boldsymbol X, \boldsymbol Y, \boldsymbol Z\)为实值随机向量，则
\[\begin{aligned}
\text{Var}(\boldsymbol a^T \boldsymbol X) =& \boldsymbol a^T \text{Var}(\boldsymbol X) \boldsymbol a \\
\text{Var}(A \boldsymbol X + \boldsymbol b) =& A \text{Var}(\boldsymbol X) A^T \\
\text{Cov}(\boldsymbol X + \boldsymbol Y, \boldsymbol Z)
=& \text{Cov}(\boldsymbol X, \boldsymbol Z)
+ \text{Cov}(\boldsymbol Y, \boldsymbol Z) \\
\text{Var}(\boldsymbol X + \boldsymbol Y)
=& \text{Var}(\boldsymbol X) + \text{Var}(\boldsymbol Y)
+ \text{Cov}(\boldsymbol X, \boldsymbol Y) + \text{Cov}(\boldsymbol Y, \boldsymbol X) \\
\text{Cov}(A \boldsymbol X, B \boldsymbol Y) =& A \text{Cov}(\boldsymbol X, \boldsymbol Y) B^T .
\end{aligned}\]

○○○○○○