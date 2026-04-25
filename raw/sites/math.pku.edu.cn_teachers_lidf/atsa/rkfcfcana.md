---
crawl_time: '2026-01-17 14:29:00'
framework: sphinx
title: E 实变函数和泛函分析 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/rkfcfcana.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# E 实变函数和泛函分析

参考：([郭懋正 2005](#ref-GuoMZ2005:RealFuncAna))

## E.1 集合运算

用\(\mathbb R\)表示实数域，
\(\overline{\mathbb R}\)表示\(\mathbb R \cup \{+\infty, -\infty\}\)。

对集合\(X\)，用\(2^X\)表示\(X\)的所有子集组成的集合（集合族），
称为\(X\)的**幂集**。

**集合上极限**：
\[
\varlimsup\_{n\to\infty} A\_n
= \bigcap\_{n=1}^{\infty} \bigcup\_{m=n}^{\infty} A\_m
= \{\omega:\; \omega \text{在无穷多个} A\_n \text{中出现} \}
\]

**集合下极限**：
\[
\varliminf\_{n\to\infty} A\_n
= \bigcup\_{n=1}^{\infty} \bigcap\_{m=n}^{\infty} A\_m
= \{\omega:\; \text{从某个} n\text{开始} \omega \text{属于后续所有的} A\_n \}
\]

**直积**（笛卡尔积）：
对集合\(A, B\)，
\(A \times B = \{(x,y): x \in A, y \in B \}\)。
类似可定义\(X\_1 \times X\_2 \dots \times X\_n\)，
\(X^n\)，\(X^T\)（其中\(T\)是\(\mathbb R\)的子集）。

**上确界**：对\(A \subset \mathbb R\)，
\(m\)是\(A\)的一个上界，
且对\(A\)的任意上界\(m'\)都有\(m \leq m'\)，
则称\(m\)为\(A\)的上确界，
记为\(\sup A\)。
如果\(A\)没有有限的上界则令\(\sup A = +\infty\)。
类似定义下确界。

**数列上极限**：
\[
\varlimsup\_{n\to\infty} a\_n
= \lim\_{n\to\infty} \sup\_{m\geq n} a\_m,
\]
可以等于\(\pm\infty\)。
类似定义下极限。
类似定义函数上极限和下极限。

集合中元素的个数，
分为三种情况：

* 有限个；
* 无限可数个，称为可列个；
* 无限不可数个。

\(\mathbb R^n=\{(x\_1, x\_2, \dots, x\_n): x\_i \in \mathbb R, i=1,2,\dots,n \}\)，
模为
\[
\| x \| = \sqrt{x\_1^2 + x\_2^2 + \dots + x\_n^2},
\]
两个点\(x,y\)的距离为\(d(x,y) = \| x - y \|\)。

序列\(\{ x\_n \} \subset \mathbb R^n\)收敛到极限\(x\)，定义为\(\lim\_{n\to\infty} \| x\_n - x \| = 0\)。

\(\mathbb R^n\)中的点集\(A\)中点的分类：

* **内点**\(x\)：\(x\)的一个邻域均属于\(A\)。
* **边界点**\(x\)：\(x \in A\)且\(x\)的任意一个邻域均与\(A^c\)都有非空交集。 其中，若\(x\)的一个邻域中仅有\(x\)属于\(A\)称\(x\)为孤立点。
* **聚点**\(x\)：存在\(x\_n \in A\)，\(x\_n \neq x\)使得\(\lim\_{n\to\infty} x\_n = x\)。

\(A\)与\(A\)的所有内点组成的集合的并集称为\(A\)的**闭包**，
记为\(\overline{A}\)。

\(\mathbb R^n\)中的**闭集**：\(A\)包含其所有的句点。
即\(A\)对极限运算封闭。
有限多个闭集的并集是闭集，
任意多个闭集的交集是闭集。
闭包是闭集。

\(\mathbb R^n\)中的**开集**：
所有点都是内点的集合。

生成的σ代数：设全集为\(X\),
\(\mathcal A\)是\(X\)的一些子集组成的集合族，
称包含\(\mathcal A\)的所有的σ代数的交集为\(\mathcal A\)生成的σ代数。

**Borel σ代数**：
\(\mathbb R^n\)中所有开集所生成的σ代数称为Borel σ代数，
记为\(\mathscr B^n\)，
其中的集合称为Borel集。
Borel集合的余集、可列并、可列交、上极限、下极限运算结果均是Borel集。

\(\mathbb R\)中任一非空开集是至多可数个互不相交的开区间的并集。
\(\mathbb R^n\)中任意非空开集是至多可数个互不相交的\(n\)为半开矩体的并集。
半开矩体是\([a\_1, b\_1) \times [a\_2, b\_2) \times \dots [a\_n, b\_n)\)这样的集合。

康拓集（Cantor set）：从\([0,1]\)中每次中间的一段，
保留端点，重复操作，极限情况下得到的集合。
是非空有界闭集，每个点都是聚点，没有内点，
无穷不可数。

函数\(f: \mathbb R^n \to \mathbb R\)连续，
当且仅当对任意\(\lambda \in \mathbb R\)，
\(\{ x: f(x) > \lambda \}\)都是开集；
当且仅当对任意\(\lambda \in \mathbb R\)，
\(\{ x: f(x) \leq \lambda \}\)都是闭集。

## E.2 序关系

**二元关系**：
设\(X\)和\(Y\)是两个集合，
\(E\)是\(X \times Y\)的子集，
称\(E\)是\(X \times Y\)的一个二元关系，
\(X \times X\)上的二元关系成为\(X\)的二元关系。
对\(x \in X, y \in Y\)，
如果\((x, y) \in E\)，
就称\(x\)和\(y\)满足二元关系\(E\)。

例如，
\(X = \mathbb R\)，
\(E = \{(x,y): x \leq y \}\)，
则二元关系\(E\)就是小于等于关系。

**序关系**：
设\(X\)是集合,
如果\(E\)为满足如下序公理的二元关系：

* `(1)` 自反性：\(\forall x \in X\), \(x\)与\(x\)自身满足\(E\)；
* `(2)` 反对称：如果\(a\)与\(b\)满足\(E\), \(b\)与\(a\)也满足\(E\)，则必有\(a=b\)；
* `(3)` 传递性：若\(a\)与\(b\)满足\(E\), \(b\)与\(c\)满足\(E\)，则\(a\)与\(c\)满足\(E\)。

就称\(E\)为\(X\)上的序关系，
\(x\)和\(y\)满足\(E\)，记为\(x \preceq y\)，
即\(x \preceq y\)当且仅当\((x,y) \in E\)。
称\((X, \preceq)\)为偏序空间，
简称序空间，
并将赋予了序关系的集合\(X\)称为有序集或偏序集。

\(x \preceq y\)可以等价地写成\(y \succeq x\)。
如果\(x \preceq y\)而且\(x \neq y\)，
可以写成\(x \prec y\)或\(y \succ x\)。

\(\leq\)和\(\geq\)是序关系，
\(=\)也是序关系。
\(<\)和\(>\)不是序关系。

**全序空间**：
给定偏序空间\((X, \preceq)\)，
如果\(\forall x, y \in X\)，
在\(x \preceq y\)和\(y \preceq x\)两者中至少有一个成立，
则称\((X, \preceq)\)是全序空间，
\(X\)是全序集合。

全序集合中任意两个元素\(x, y\)之间的关系必为\(x \prec y\), \(x=y\)，\(x \succ y\)三者之一，
这是实数比较\(\leq\)的自然推广。

一般的偏序则不需要任意两个元素可比，
典型代表是\(X\)为集合，
\(2^X\)为\(X\)的所有子集组成的集合族，
\(2^X\)中\(\subset\)是一个序关系，
但不构成全序，
两个子集可以没有包含关系。

对于偏序空间\((X, \preceq)\)，
设\(B \subset X\)，
如果\((B, \preceq)\)构成全序空间，
则称\(B\)为\(X\)的全序子集。
设\(A\)是\(X\)的全序子集，
且\(X\)中没有以\(A\)为真子集的全序子集，
就称\(A\)为\(X\)的最大全序子集。
这个定义不保证最大全序子集唯一。

**Zorn公理**：每个偏序集有最大全序子集。

给定偏序集\(X\)和全序子集\(B \subset X\)，
若\(a \in X\)使得对\(\forall x \in B\)，
都有\(x \preceq a\)，
则称\(a\)为\(B\)的一个上界。
若\(b \in B\)是\(B\)的一个上界，
称\(b\)为\(B\)的最大元。
对\(a \in X\)，
如果对\(X\)中任意能与\(a\)比较的\(x\)都有\(x \preceq a\)，
称\(a\)为\(X\)的一个极大元。

**Zorn引理**：
设\(X\)为非空偏序集，
若\(X\)的每个全序子集在\(X\)中有上界，
则\(X\)必有极大元。

## E.3 勒贝格测度

对\(\mathbb R^n\)中的开矩体
\[
I = [a\_1, b\_1] \times [a\_2, b\_2] \times \dots [a\_n, b\_n] ,
\]
定义其体积为
\[
|I| = \prod\_{i=1}^n (b\_i - a\_i) .
\]

设\(E \subset \mathbb R^n\)，
\(\{ I\_k \}\)是开矩体列，\(E \subset \bigcup\_{k=1}^\infty I\_k\)（称\(\{I\_k \}\)是\(E\)的一个覆盖），
则
\[
m^\*(E) = \inf \left\{ \sum\_{k=1}^{\infty} |I\_k| : \;
I\_k \text{是开矩体}, E \subset \bigcup\_{k=1}^\infty I\_k\right\}
\]
称为\(E\)的外测度。
外测度非负，单调，满足次可加性\(m^\*(E)(\bigcup\_{k=1}^\infty E\_k) \leq \sum\_{k=1}^\infty m^\*(E\_k)\)，
平移不变性\(m^\*(E + \{x\}) = m^\*(E)\), \(\forall x \in \mathbb R^n\)。

\(E + \{ x \} = \{ y + x: y \in E \}\)。

**勒贝格可测**：
设\(E \subset \mathbb R^n\)，
若对\(\mathbb R^n\)到任意子集\(T\)都有
\[
m^\*(T) = m^\*(T \cap E) + m^\*(T \cap E^c),
\]
则称\(E\)是勒贝格可测集，
简称为可测集，记为\(\mathscr M\)或\(\mathscr M\_n\)。
当\(E\)可测时记\(m(E) = m^\*(E)\)，
称为\(E\)的勒贝格测度。

所有开矩体可测，且\(m(I) = |I|\)。

所有可测集构成\(\mathbb R^n\)的一个σ代数。
Borel集都可测，即\(\mathscr B^n \subset \mathscr M^n\)，
若\(E\)可测，
必存在Borel集\(F, G\)使得\(F \subset E \subset G\)且\(m(F) = m(E) = m(G)\)，
所以可测集与Borel集可以近似等同看待。

设\(X\)是集合，
\(\mathscr F\)是\(X\)中子集组成的σ代数，
称\((X, \mathscr F)\)为**可测空间**；
如果集合函数\(\mu: \mathscr F \to [0, \infty]\)满足：

`(1)` \(\mu(\emptyset) = 0\);

`(2)` 若\(A\_n \in \mathscr F\), \(n=1,2,\dots\)互不相交，则有σ可加性
\[
\mu(\bigcup\_{n=1}^\infty A\_n) = \sum\_{n=1}^\infty \mu(A\_n),
\]
则称\(\mu\)为\((X, \mathscr F)\)上的一个测度，
称\((X, \mathscr F, \mu)\)为测度空间。

如果对任意的零测集\(A\)(满足\(\mu(A)=0\))，
其子集都可测，
称这样的测度空间是**完备**的。

如果\(\mu(X)<\infty\)，称\(\mu\)为有限测度。
如果\(X = \bigcup\_{n=1}^\infty A\_n\)，
其中\(\{ A\_n \}\)互不相交，
且\(\mu(A\_n) < \infty\)，
称\(\mu\)为σ有限测度。

测度对单独增集合列和单调减集合都有连续性：
\[
\lim\_{n\to\infty} \mu(A\_n)
= \mu(\lim\_{n\to\infty} \mu(A\_n) .
\]

关于\(x \in \mathbb R^n\)的一个论断\(S(x)\)在可测集\(E\)上几乎处处成立，
记为\(S(x) \text{ a.e.}[E]\)，
定义为存在集合\(E\_0 \subset E\)，\(m(E\_0) = 0\)，
使得\(S(x)\)对所有\(x \in E \backslash E\_0\)成立。
注意不需要\(S(x)\)在\(E\_0\)上都不成立，
所以不需要测度完备。
当\(E = \mathbb R^n\)时记\(S(x) \text{ a.e.}\)。

## E.4 勒贝格可测函数

考虑从\(\mathbb R^n\)到\(\overline{\mathbb R}\)的广义的函数，
并定义\(0 \times \infty = 0\)。

**可测函数**：
设\(E\)是\(\mathbb R^n\)中的可测集，
\(f: E \to \overline{\mathbb R}\)，
如果\(\forall t \in \mathbb R\)，
\(\{x \in \mathbb R^n: x \in E, f(x) > t \}\)都是可测集，
则称\(f\)为\(E\)上的（勒贝格）可测函数。
记\(\mathscr M(E)\)为\(E\)上的可测函数的全体。

可测与如下条件等价：\(\forall t \in \mathbb R\)，
\(\{x \in \mathbb R^n: x \in E, f(x) > t \}\)都可测；
\(\{x \in \mathbb R^n: x \in E, f(x) > t \}\)都可测；
\(\{x \in \mathbb R^n: x \in E, f(x) \geq t \}\)都可测；
\(\{x \in \mathbb R^n: x \in E, f(x) \leq t \}\)都可测。

**简单函数**：设\(E = \bigcup\_{i=1}^m E\_i\),
\(E\_i\)互不相交且可测，
\(\alpha\_i \in \mathbb R\)，
\[
\psi(x) = \sum\_{i=1}^m \alpha\_i I\_{E\_i}(x)
\]
称为\(E\)上的简单函数。
当每个\(E\_i\)是矩体时称为阶梯函数。
可测集\(E\)上的简单函数都可测。

可测集\(E\)上非负函数\(f: E \to \overline{\mathbb R}\)可测的充分必要条件是存在\(E\)上的非负简单函数列
\(\{ \psi\_k(x) \}\)使得\(\psi\_k(x) \leq \psi\_{k+1}(x)\), \(\forall x \in E\)，
且\(\lim\_{k\to\infty} \psi\_k(x) = f(x)\), \(\forall x \in E\)。

可测函数的运算：
若\(f, g\)是\(E\)上的可测函数，
\(c \in \mathbb R\)，
则\(c f(x)\)可测，\(|f(x)|\)可测，
\(f(x) g(x)\)可测；
\(f(x) + g(x)\)和\(f(x)/g(x)\)在其有定义的集合上可测（除去\(\infty - \infty\), 除以0等无定义的点）。

可测集\(E\)上的连续函数都是可测函数。

对可测集\(E\)上的可测函数列\(f\_k(x)\)，
\(\sup\_k f\_k(x)\), \(\inf\_k f\_k(x)\)，
\(\varlimsup\_k f\_k(x)\), \(\varliminf\_k f\_k(x)\)都可测；
如果\(\lim\_k f\_k(x) = f(x)\)存在则\(f(x)\)可测；
如果存在零测集\(E\_0\)使得
\[
\lim\_{k\to\infty} f\_k(x) = f(x),
\ x \in E \backslash E\_0,
\]
称\(f\_k(x)\)几乎处处收敛到\(f(x)\)，
这时\(f(x)\)可测。

可测集\(E\)上的可测函数\(f(x)\)可测，
当且仅当\(f^+(x)\)和\(f^-(x)\)都可测。
\[
f^+(x) = \max\{ f(x), 0 \},
\quad f^-(x) = \max\{ -f(x), 0 \} .
\]
如果\(f(x): \mathbb R \to \mathbb R\)连续，
\(g(x): E \to \mathbb R\)可测，\(E \subset \mathbb R^n\)可测，
则\(f(g(x))\)是\(E\)上可测函数。

如果可测集\(E\)上的函数\(f\)和\(g\)几乎处处相等，
则它们同时可测或同时不可测。

一般的测度空间\((X, \mathcal F, \mu)\)上的可测函数有类似的定义和性质。

## E.5 可测函数极限

**一致收敛**：
设\(E\)为点集，
对\(E \to \mathbb R\)的\(f\_k\)和\(f\)，
\(\forall \varepsilon > 0\),
存在\(K\)使得\(k \geq K\)时\(\forall x \in E\)有\(|f(x\_k) - f(x)| < \varepsilon\)。

**几乎一致收敛**：
设\(E\)为\(\mathbb R^n\)的可测集，
\(\forall \delta > 0\)，存在\(E\_{\delta} \subset E\)使得\(m(E \backslash E\_{\delta}) < \delta\)，
在\(E\_{\delta}\)上\(f\_k\)一致收敛到\(f\)。

一致收敛推出几乎一致收敛。

**点点收敛**：
设\(E\)为点集，
对\(E \to \mathbb R\)的\(f\_k\)和\(f\)，
若\(\lim\_{k\to\infty} f\_k(x) = f(x)\),
\(\forall x \in E\)，
称\(f\_k(x)\)在\(E\)上点点收敛到\(f(x)\)。
如果\(f\_k\)都可测，
则\(f\)也可测。

一致收敛推出点点收敛。

**几乎处处收敛**：
设\(E\)为\(\mathbb R^n\)中点集，
\(f(x), f\_k(x)\)是\(E \to \overline{\mathbb R}\)函数，
存在\(Z \subset E\)使得\(m(Z)=0\),
\(\forall x \in E \backslash Z\)有\(\lim\_k f\_k(x) = f(x)\)，
则称\(f\_k(x)\)几乎处处收敛到\(f(x)\)，
记为\(\lim\_k f\_k(x) = f(x)\), a.e.\([E]\)。
如果\(E\)可测，\(f\_k\)都可测，
则\(f\)也可测。

几乎一致收敛推出几乎处处收敛。
反过来，
如果\(m(E)<\infty\)，\(f\_k\)和\(f\)都可测且几乎处处有限，
则几乎处处收敛推出几乎一致收敛，
这是叶戈罗夫定理。

**依测度收敛**：
设\(E\)为可测集，
\(f, f\_k\)为\(E \to \overline{\mathbb R}\)函数，
可测且几乎处处有限，
\(\forall \varepsilon>0\)有
\[
\lim\_{k \to \infty} m(\{x \in E:\; |f\_k(x) - f(x)| > \varepsilon \}) = 0 .
\]

如果两个函数都是同一个函数列的依测度极限，
则这两个函数几乎处处相等。
即在几乎处处意义下依测度收敛的极限是唯一的。

**几乎处处收敛推出依测度收敛**：
设\(E\)可测，\(m(E)<\infty\)，
\(f\_k(x)\)是\(E\)上几乎处处有限的可测函数列，
\(f\_k(x)\)几乎处处收敛到\(f(x)\)，
则\(f(x)\)几乎处处有限且可测，
并且\(f\_k(x)\)依测度收敛到\(f(x)\)。

**依测度收敛有子列几乎处处收敛**（Riesz定理）：
设\(E\)可测，\(m(E)<\infty\)，
\(f\_k(x)\)是\(E\)上几乎处处有限的可测函数列，
\(f\_k(x)\)依测度收敛到几乎处处有限的可测函数\(f(x)\)，
则存在子列\(f\_{k\_i}(x)\)使得\(f\_{k\_i}(x)\)几乎处处收敛到\(f(x)\)。

依测度收敛当且仅当是依测度基本列：
\[
\lim\_{k,j\to\infty} m(\{x \in E:\; |f\_k(x) - f\_j(x)| > \varepsilon \}) = 0 .
\]

对可测集\(E \subset \mathbb R^n\)上的几乎处处有限的可测函数\(f\)，
必存在\(\mathbb R^n\)上的连续函数列\(\{ g\_k(x) \}\)使得\(g\_k(x)\)在\(E\)上几乎处处收敛到\(f(x)\)。

一般的测度空间\((X, \mathcal F, \mu)\)上的可测函数收敛性类似。

## E.6 勒贝格积分

对可测集\(E \subset \mathbb R^n\)的非负可测简单函数\(h(x):E \to \overline{\mathbb R}\):
\[
h(x) = \sum\_{j=1}^m \alpha\_j I\_{E\_j}(x),
\]
定义勒贝格积分为
\[
\int\_{E} h(x) \,dx
= \sum\_{j=1}^m \alpha\_j m(E\_j) .
\]

对可测集\(E \subset \mathbb R^n\)的非负可测函数\(f(x):E \to \overline{\mathbb R}\)，
定义
\[
\int\_{E} f(x) \,dx
= \sup \left\{ \int\_{E} h(x) \,dx :\;
h(x) \text{是} E \text{上非负可测简单函数，且} h(x) \leq f(x), \forall x \in E
\right\} .
\]
若\(\int\_{E} f(x) \,dx < \infty\)，
称\(f(x)\)在\(E\)上是勒贝格可积的，记为\(f \in L(E)\)。

如果\(E\)可测，\(A\)是\(E\)的可测子集，
\(f(x)\)是\(E\)上非负可测函数，
则
\[
\int\_{A} f(x) \,dx
= \int\_{E} f(x) I\_A(x) \,dx .
\]

Levi定理（非负单调收敛定理）：
设\(\{f\_k(x) \}\)是可测集\(E\)上的非负可测函数列，
\(f\_k(x) \leq f\_{k+1}(x)\)(\(\forall x \in E\))，
且\(\lim\_k f\_k(x) = f(x)\), \(\forall x \in E\)，
则
\[
\lim\_{k\to\infty} \int\_{E} f\_k(x) \,dx
= \int\_{E} f(x) \,dx .
\]

对可测集\(E\)上可测函数\(f(x)\)，
若\(f^+(x)\)和\(f^-(x)\)至少一个可积，
则定义
\[
\int\_{E} f(x) \,dx
= \int\_{E} f^+(x) \,dx
- \int\_{E} f^-(x) \,dx .
\]
如果\(f^+(x)\)和\(f^-(x)\)都可积，
则称\(f(x)\)在\(E\)上勒贝格可积，
记\(f(x) \in L(E)\)。

\(f(x)\)可积当且仅当\(|f(x)|\)可积，
且这时
\[
\left| \int\_{E} f(x) \,dx \right|
\leq \int\_{E} |f(x)| \,dx .
\]

设\(E\)是可测集：

* 如果\(f = g\) a.e.\([E]\)，
  \(f \in L(E)\)则\(g \in L(E)\)且\(\int\_{E} f(x) \,dx = \int\_{E} g(x) \,dx\)；
* 如果\(f(x)\)可测，而\(g(x)\)非负可积，\(f(x) \leq g(x)\), \(\forall x \in E\)，
  则\(f(x)\)可积且\(\left| \int\_{E} f(x) \,dx \right| \leq \int\_{E} g(x) \,dx\)；
* 若\(m(E) < \infty\)，
  则\(E\)上任意几乎处处有界的可测函数是可积的；
* 若\(f, g \in L(E)\),
  且\(f(x) \leq g(x)\), a.e.\([E]\)，
  则\(\int\_{E} f(x) \,dx \leq \int\_{E} g(x) \,dx\)；
* 若\(f, g \in L(E)\), \(\alpha, \beta \in \mathbb R\)，
  则（线性性质）
  \[
  \int\_{E} (\alpha f(x) + \beta g(x)) \,dx
  = \alpha \int\_{E} f(x) \,dx
  + \beta \int\_{E} g(x) \,dx .
  \]

如果\(m(E)=0\)，
则\(E\)上的可测函数\(f(x)\)都可积且\(\int\_{E} f(x) \,dx = 0\)。

若可测集\(E\)上的可测函数\(f(x)\)满足\(\int\_{E}|f(x)|\,dx < \infty\)，
则\(|f(x)| < \infty\), a.e.\([E]\)。

若可测集\(E\)上的可测函数\(f(x)\)满足\(\int\_{E}|f(x)|\,dx = 0\)，
则\(f(x) = 0\), a.e.\([E]\)。

积分的绝对连续性：
对可测集\(E\)上的可测函数\(f(x)\)，
\(\forall \varepsilon > 0\)，
必存在\(\delta > 0\)，
\(E\)的子集\(A\)只要满足\(m(A) < \delta\)就一定有
\[
\left| \int\_A f(x) \,dx \right|
\leq \int\_A |f(x)| \,dx < \varepsilon .
\]

可积函数的连续函数逼近：
对可测集\(E\)上的可测函数\(f(x)\)，
存在\(\mathbb R^n\)上有紧支集的连续函数列\(g\_k(x)\)使得
\[\begin{aligned}
&(1)\ \lim\_{k\to\infty} \int\_{E}|f(x) - g\_k(x)| \,dx = 0; \\
&(2)\ \lim\_{k\to\infty} g\_k(x) = f(x), \text{ a.e}[E] .
\end{aligned}\]

与黎曼积分的关系：
设\(f(x)\)为闭区间\([a,b]\)上的有界函数，
若\(f(x)\)在\([a,b]\)黎曼可积，
必勒贝格可积且积分相等；
\(f(x)\)在\([a,b]\)黎曼可积当且仅当\(f(x)\)的不连续点为零测集。

一般的测度空间\((X, \mathcal F, \mu)\)上也可以类似定义勒贝格积分。

## E.7 极限与积分的交换

非负单调收敛定理（Levi定理）：
若可测集\(E\)上的非负可测函数\(f\_k(x)\)满足\(f\_k(x) \leq f\_{k+1}(x)\),
\(\forall x, \forall k\)，
\(f(x) = \lim\_{k\to\infty} f\_k(x)\)，a.e.\([E]\)，则
\[
\lim\_{k\to\infty} \int\_E f\_k(x) \,dx
= \int\_E f(x) \,dx .
\]

勒贝格基本定理：
设\(\{ f\_k(x) \}\)是可测集\(E\)上的非负可测函数列，
则
\[
\sum\_{k=1}^\infty \int\_E f\_k(x) \,dx
= \int\_E \sum\_{k=1}^\infty f\_k(x) \,dx .
\]

Fatou引理：
设\(\{ f\_k(x) \}\)是可测集\(E\)上的非负可测函数列，
则
\[
\int\_E \varliminf\_{k \to \infty} f\_k(x) \,dx
\leq \varliminf\_{k \to \infty} \int\_E f\_k(x) \,dx .
\]

**控制收敛定理**：
设\(E\)为可测集，
\(\{ f\_k(x) \}\)是\(E\)上可测函数列，
\(f\_k(x)\)在\(E\)上几乎处处收敛到\(f(x)\)或者依测度收敛到\(f(x)\)，
若存\(E\)上非负可积函数\(g(x)\)使得\(f\_k(x) \leq g(x)\),
a.e. \([E]\)，
\(\forall k\)，
则\(f\_k(x)\)和\(f(x)\)在\(E\)上可积且
\[
\lim\_{k\to\infty} \int\_E f\_k(x) \,dx
= \int\_E f(x) \,dx .
\]

**有界收敛定理**：
若\(m(E)<\infty\)，
\(\{ f\_k(x) \}\)是\(E\)的可测函数列，
存在常数\(M\)使得
\[
|f\_k(x)| \leq M, \forall x \in E, \forall k,
\]
且\(f\_k(x)\)几乎处处或者依测度收敛到函数\(f(x)\)，
则\(f\_k(x)\)和\(f(x)\)可积且
\[
\lim\_{k\to\infty} \int\_E f\_k(x) \,dx
= \int\_E f(x) \,dx .
\]

逐项积分：
设\(E\)为\(\mathbb R^n\)的可测集，
\(f\_k(x) \in L(E)\)，
若
\[
\sum\_{k=1}^\infty \int\_E |f\_k(x)| \,dx < \infty,
\]
则级数\(\sum\_{k=1}^\infty f\_k(x)\)几乎处处收敛，
记为\(f(x)\),
则\(f(x) \in L(E)\)且
\[
\sum\_{k=1}^\infty \int\_E f\_k(x) \,dx
= \int\_E f(x) \,dx .
\]

参变积分：
设\(E\)为\(\mathbb R^n\)的可测集，
\(f(x,y): E \times [a,b] \to \overline{\mathbb R}\),
对每个\(y \in [a,b]\)，
\(f(\cdot, y)\)是\(E\)上的可积函数，
则
\[
\phi(y) = \int\_E f(x, y) \,dx
\]
是定义于\([a,b]\)的有限实值函数，
称为区间\([a,b]\)上的参变积分。

对参变积分：

`(1)` 若存在\(g(x) \in L(E)\)使得
\[
f(x,y) \leq g(x),
\ \forall x \in E, \ y \in [a,b] .
\]
且\(\lim\_{y \to y\_0} f(x, y)\)在\(E\)上a.e.收敛，
就有
\[
\lim\_{y \to y\_0} \phi(y)
= \int\_E \lim\_{y \to y\_0} f(x, y) \,dx ;
\]

`(2)` 若存在\(g(x) \in L(E)\)使得
\[
f(x,y) \leq g(x),
\ \forall x \in E, \ y \in [a,b] .
\]
且对a.e.\([E]\)的\(x\)函数\(f(x, \cdot)\)在\(y\_0\)处连续，
则\(\phi(y)\)在\(y\_0\)处连续；

`(3)` 若\(\frac{\partial f(x,y)}{\partial y}\)存在，
存在\(g(x) \in L(E)\)使得\(|\frac{\partial f(x,y)}{\partial y}| \leq g(x)\),
\(\forall x \in E, y \in [a,b]\)，则
\[
\phi'(y) = \int\_E \frac{\partial f(x,y)}{\partial y} \,dx .
\]

## E.8 重积分与累次积分

设\(A\)和\(B\)分别为\(\mathbb R^p\)和\(\mathbb R^q\)的可测集，
则\(A \times B\)为\(\mathbb R^{p+q}\)的可测集且
\[
m(A \times B) = m(A) m(B) .
\]

Tonelli定理：
设\(f(x,y): \mathbb R^{p+q} \to \overline{\mathbb R}\)非负可测，
则

`(1)` 对几乎处处的\(x \in \mathbb R^p\)，\(f(x, \cdot)\)是\(\mathbb R^q\)的非负可测函数；

`(2)` \(F\_f(x) = \int\_{\mathbb R^q} f(x, y) \,dy\)在\(\mathbb R^p\)上几乎处处有定义且非负可测；

`(3)` 重积分与累次积分相等，累次积分次序可交换：
\[
\int\_{\mathbb R^{p+q}} f(x, y) \,dx dy
= \int\_{\mathbb R^{p}} dx \int\_{\mathbb R^{q}} f(x, y)\,dy
= \int\_{\mathbb R^{q}} dy \int\_{\mathbb R^{p}} f(x, y)\,dx .
\]

Fubini定理：
设\(f(x,y): \mathbb R^{p+q} \to \overline{\mathbb R}\)可积，
则

`(1)` 对几乎处处的\(x \in \mathbb R^p\)，\(f(x, \cdot)\)是\(\mathbb R^q\)的可积函数；

`(2)` \(F\_f(x) = \int\_{\mathbb R^q} f(x, y) \,dy\)在\(\mathbb R^p\)上几乎处处有定义且可积；

`(3)` 重积分与累次积分相等，累次积分次序可交换：
\[
\int\_{\mathbb R^{p+q}} f(x, y) \,dx dy
= \int\_{\mathbb R^{p}} dx \int\_{\mathbb R^{q}} f(x, y)\,dy
= \int\_{\mathbb R^{q}} dy \int\_{\mathbb R^{p}} f(x, y)\,dx .
\]

注意，即使累次积分都存在且相等，
\(f(x,y)\)也不一定可积；
Fubini定理在使用时，
为了验证\(f(x,y)\)是否可积，
可以先用Tonelli定理计算\(|f(x,y)|\)的累次积分从而判断可积性。

## E.9 \(L^p\)空间

### E.9.1 定义

本节中设\(E\)为\(\mathbb R^n\)中的可测集。

设\(f(x)\)为\(E\)上的可测函数，
\(1 \leq p < \infty\)，
记
\[
\| f \|\_p
= \left( \int\_E |f(x)| \,dx \right)^{\frac{1}{p}} ,
\]
称\(\|f\|\_p\)为\(f\)的\(L^p\)范数或\(L^p\)模。令
\[
L^p(E) = \left\{f: f \text{为}E \text{上的可测函数且} \|f\|\_p < \infty \right\} ,
\]
称\(L^p(E)\)为\(E\)上的\(L^p\)空间。

设\(f(x)\)为\(E\)上的可测函数，
如果存在正数\(M\)使得\(|f(x)| \leq M\), a.e.\([E]\)，
称\(f(x)\)为本性有界的，取所有这样的\(M\)的下确界，
记为\(\|f\|\_\infty\)，
用\(L^\infty(E)\)表示\(E\)上所有的本性有界函数组成的集合。
对\(f \in L^\infty(E)\)，有
\[
\|f\|\_\infty = \inf\_{\triangle \subset E, \, m(\triangle)=0}
\sup\_{x \in E \backslash \triangle} |f(x)| .
\]
对一般的测度空间\((X, \mathscr F, \mu)\)，
也可以类似定义其\(L^p\)空间\(L^p(X, \mathscr F, \mu)\)。

下面设\(1 \leq p, q \leq \infty\)，
\(\frac{1}{p} + \frac{1}{q} = 1\)，
称\(p,q\)为共轭指数，\(p=1\)时\(q=\infty\)，
\(p=2\)时\(q=2\)，
\(1 < p < \infty\)时\(q = \frac{p}{p-1}\)。

**Hölder不等式**：
对\(f \in L^p(E)\), \(g \in L^q(E)\)，有
\[
\left| \int\_E f(x) g(x) \,dx \right|
\leq \int\_E |f(x) g(x)| \,dx
= \| f g \|\_1
\leq \|f\|\_p \, \|g\|\_q .
\]

当\(p=q=2\)时即Schwarz不等式。

若\(m(E)<\infty\)，
则对\(p\_1 < p\_2\)有\(\|f\|\_{p\_2} < \infty\)推出\(\|f\|\_{p\_1} < \infty\)，
\(\lim\_{p\to\infty} \| f \|\_p = \|f\|\_\infty\)。

### E.9.2 距离空间

\(L^p(E)\)对加法运算和数乘封闭，
构成实数域上的向量空间（线性空间）。

对一般的实数域上的向量空间（线性空间）\(X\)，
函数\(\| \cdot \|: X \to \mathbb R\)称为\(X\)的一个范数，如果它满足如下的三条：

* `(1)` 齐次性：\(\| \alpha x \| = |\alpha| \cdot \| x \|\),
  \(\forall \alpha \in \mathbb R, x \in X\)；
* `(2)` 三角不等式：\(\| x + y \| \leq \|x\| + \|y\|\),
  \(\forall x, y \in X\);
* `(3)` 正定性：\(\|x\| \geq 0\)，\(\forall x \in X\)，
  \(\|x\| = 0\)当且仅当\(x\)是向量空间\(X\)的零元素。

将\(L^p(E)\)中几乎处处相等的函数看成空间中的同一个元素，
则\(\| \cdot \|\)满足上述的范数的条件，
\(\|f\|\_p = 0\)当且仅当\(f(x) = 0\), a.e.\([E]\)。

**距离空间**
设\(X\)为实数域上的向量空间，
如果函数\(d: X \times X \to \mathbb R\)满足如下性质：

* `(1)` 正定性：\(d(x,y) \geq 0\), \(\forall x, y \in X\),
  \(d(x,y)=0\)当且仅当\(x=y\);
* `(2)` 对称性：\(d(x,y) = d(y,x)\), \(\forall x, y \in X\)；
* `(3)` 三角不等式：
  \(d(x, y) \leq d(x, z) + d(z, y)\), \(\forall x, y, z \in X\)。

对\(1 \leq p \leq \infty\)，
在\(L^p(E)\)中定义
\[
d(f, g) = \| f - g \|\_p,
\]
则\(L^p(E)\)构成距离空间。
有了距离以后就可以定义开集、闭集等拓扑，
定义序列极限。

对\(L^p(E)\)中的序列\(\{ f\_m \}\)和函数\(f\)，
如果
\[
\| f\_m - f \| \to 0,
\ m \to \infty,
\]
则称\(f\_m\)依\(L^p\)意义收敛到\(f\)，
记为\(f\_m \stackrel{L^p}{\rightarrow} f\)，
对\(p=2\)称为均方收敛。

\[
\| f\_m - f\_k \|\_p \to 0,
\ m, k \to \infty,
\]
称\(\{ f\_m \}\)为\(L^p(E)\)的基本列。
如果\(\{ f\_m \}\)收敛\(f \in L^p(E)\)，
\(\{ f\_m \}\)必为基本列；
反之，
如果\(\{ f\_m \}\)为基本列，
必存在\(f \in L^p(E)\)使得\(f\_m \stackrel{L^p}{\rightarrow} f\)。
称\(L^p(E)\)是完备的距离空间。
完备的距离空间中的聚点都在空间内。

如果考虑闭区间\([a,b]\)上黎曼可积函数组成的距离空间，
就不是完备的。
这说明勒贝格积分比黎曼积分更优秀。

\(L^p\)收敛有如下性质：

* `(1)` 唯一性：如果序列收敛到两个函数\(f\)和\(g\)则\(f=g\)，a.e.\([E]\)；
* `(2)` 如果\(f\_m \stackrel{L^p}{\rightarrow} f\)则\(\|f\_m\|\_p \to \|f\|\)；
* `(3)` 极限对加法和数乘封闭，若\(f\_m \stackrel{L^p}{\rightarrow} f\)，
  \(g\_m \stackrel{L^p}{\rightarrow} g\)，\(\alpha \in \mathbb R\)，
  则\(f\_m + g\_m \stackrel{L^p}{\rightarrow} f + g\),
  \(\alpha f\_m \stackrel{L^p}{\rightarrow} \alpha f\)。

### E.9.3 收敛之间的关系

**L^p收敛推出依测度收敛**：
对\(1 \leq p < \infty\)，
如果\(f\_m \stackrel{L^p}{\rightarrow} f\)，
则\(f\_m\)依测度收敛到\(f\)。

对\(1 \leq p \leq \infty\)，
若\(f\_m \stackrel{L^p}{\rightarrow} f\)，
则存在子列几乎处处收敛到\(f\)。

**控制收敛定理**：设\(1\leq p \leq \infty\),
\(f\_m, f\)为\(E\)上的可测函数，
\(f\_m\)几乎处处收敛到\(f\)或者依测度收敛到\(f\)，
且存在非负可测函数\(g \in L^p(E)\)使得
\[
| f\_m(x) | \leq g(x), \text{ a.e.}[E],
\]
则\(f\_m \stackrel{L^p}{\rightarrow} f\)。

### E.9.4 可分性

一个距离空间\((X,d)\)中，
如果\(F \subset G \subset X\)，
且对任意\(x \in G\)存在\(\{ x\_n \} \subset F\)使得\(d(x\_n, x) \to 0\)，
称\(F\)在\(G\)中稠密，
或称\(G\)中的元素可以用\(F\)中的元素逼近。
一个距离空间如果有可数的稠密子集，
称其为可分距离空间。

对\(f \in L^p(E)\)，
可以用简单可测函数逼近；
可以用\(\mathbb R^n\)中有紧支集的连续函数逼近；
可以用\(\mathbb R^n\)中有紧支集的阶梯函数逼近。

对\(1 \leq p < \infty\)，
\(L^p(E)\)有可数稠密子集，
从而是可分距离空间，
可以用紧支集阶梯函数组成可数稠密子集。

\(L^\infty(E)\)不一定可分。

## E.10 \(L^2\)内积空间

### E.10.1 定义

本节中设\(E\)是\(\mathbb R^n\)的可测集。

范数（模）仅能引出长度、距离、收敛的概念，
但是无法引出角度、正交（垂直）的概念。
\(L^2(E)\)的模与\(\mathbb R^n\)的模最接近，
也可以定义内积构成内积空间。
对\(f, g \in L^2(E)\)，定义
\[
\langle f, g \rangle
= \int\_E f(x) g(x) \,dx ,
\]
称为\(f\)和\(g\)的内积。
由Schwarz不等式可知内积是有限实数值：
\[
|\langle f, g \rangle|
\leq \|f\|\_2 \cdot \|g\|\_2 .
\]
对一般的实数域上的向量空间\(X\)，
函数\(K(x,y)：X \times X \to \mathbb R\)如果满足以下要求：

* `(1)` 双线性：\(K(x,y)\)分别关于\(x\)和\(y\)是线性函数；
* `(2)` 对称性：\(K(x,y) = K(y,x)\);
* `(3)` 正定性：\(K(x,x) \geq 0\), \(K(x,x)=0\)当且仅当\(x=0\)，

则称\(K(x,y)\)是\(X\)的一个**内积**，
记为\(\langle x, y \rangle\)。
\(L^2(E)\)中的\(\langle f, g \rangle\)满足内积的这三条要求。
定义了内积的向量空间称为**内积空间**。

从内积可以定义模\(\|x\| = \sqrt{\langle x, y \rangle}\)，
从模可以定义距离\(d(x,y) = \| x-y \|\)，
所以内积空间是距离空间。

### E.10.2 性质

简记\(\|f\|\_2\)为\(\|f\|\)。

**弱收敛**：
对\(f\_m, f \in L^2(E)\)，
如果对任意\(g \in L^2(E)\)都有
\[
\langle f\_m, g \rangle
\to \langle f, g \rangle,
\]
即\(\langle f\_m - f, g \rangle \to 0\)，
则称\(f\_m\)弱收敛到\(f\)。
\(L^2\)收敛必弱收敛；
如果\(f\_m\)弱收敛到\(f\)且\(\|f\_m\| \to \|f\|\)则\(f\_m \stackrel{L^p}{\rightarrow} f\)。

弱收敛可以用稠密子集判断：
对\(f\_m, f \in L^2(E)\)，如果\(\{ \| f\_m \|\}\)有界，
且存在\(L^2(E)\)的稠密子集\(G\)使得对任意\(g \in G\)都有
\(\langle f\_m, g \rangle \to \langle f, g \rangle\)，
则\(f\_m\)弱收敛到\(f\)。

**正交**：在内积空间中，如果\(\langle x, y \rangle = 0\)则称\(x\)与\(y\)正交（或垂直），
记为\(x \perp y\)。
对\(L^2(E)\)即
\[
\int\_E f(x) g(x) \,dx = 0 .
\]

如果\(f\)和\(g\)都是非负函数则内积等于0当且仅当\(f=g=0\), a.e.\([E]\)。

**标准正交系**：
设函数列\(\{\phi\_a \}\_{a \in A}\)中任意两个元素正交，
则称\(\{\phi\_a \}\_{a \in A}\)为正交系；
如果进一步还有\(\|\phi\_a\| = 0\), \(\forall a \in A\)，
称\(\{\phi\_a \}\_{a \in A}\)为标准正交系。

\(L^2(E)\)中的任意标准正交系必为可数集合。

例：\(L^2[-\pi, \pi]\)的一个标准正交系为三角函数列
\[
\frac{1}{\sqrt{2\pi}},
\frac{1}{\sqrt{\pi}} \cos x,
\frac{1}{\sqrt{\pi}} \sin x,
\frac{1}{\sqrt{\pi}} \cos 2 x,
\frac{1}{\sqrt{\pi}} \sin 2 x, \ldots
\]

设\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个标准正交系，
对\(f \in L^2(E)\)，
称
\[
c\_k = \langle f, \phi\_k \rangle
= \int\_E f(x) \phi\_k(x) \,dx
\]
为\(f\)（关于正交系\(\{\phi\_k\}\)）的傅立叶(Fourier)系数，
称
\[
\sum\_{k=1}^\infty c\_k \phi\_k(x)
\]
为\(f\)（关于正交系\(\{\phi\_k\}\)）的傅立叶级数，
记为\(f \sim \sum\_{k=1}^\infty c\_k \phi\_k(x)\)，
注意傅立叶级数不一定收敛，
即使收敛其极限也不一定等于\(f(x)\)。

Bessel不等式：在以上定义中
\[
\sum\_{k=1}^\infty c\_k^2 \leq \|f\|^2 .
\]

**Riesz-Fischer定理**：
设\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个标准正交系，
实数列\(\{ c\_k \}\)平方可和（即\(\sum\_{k=1}^\infty c\_k^2 < \infty\)），
则级数\(\sum\_{k=1}^\infty c\_k \phi\_k(x)\)在\(L^2(E)\)中收敛（均方收敛），
记极限为\(f\)，则\(c\_k = \langle f, \phi\_k \rangle\)，
且\(\|f\|^2 = \sum\_{k=1}^\infty c\_k^2\)。

设\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个标准正交系，
令
\[
G = \left\{ \sum\_{k=1}^\infty c\_k \phi\_k(x):\; \sum\_{k=1}^\infty c\_k^2 < \infty \right\},
\]
则\(G \subset L^2(E)\)；
对任意\(f \in L^2(E)\)，
令
\[
\tilde f = \sum\_{k=1}^\infty c\_k \phi\_k(x),
\]
其中\(\{ c\_k \}\)为\(f\)的傅立叶系数，
则\(\tilde f \in G\)且
\[
\| f - \tilde f \| = \min\_{g \in G} \| f - g \| .
\]

**标准正交基**：
设\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个标准正交系，
如果对任意\(f \in L^2(E)\)都存在\(k\)使得\(\langle f, \phi\_k \rangle \neq 0\)，
则称\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个**完全（完备）标准正交系**。
如果对任意\(f \in L^2(E)\)，
其傅立叶级数均收敛到\(f\)，
即
\[
f = \sum\_{k=1}^\infty \langle f, \phi\_k \rangle \; \phi\_k(x) ,
\]
则称\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个**标准正交基**。
对\(L^2(E)\)，
完全标准正交系是标准正交基。

\(L^2[-\pi,\pi]\)中三角函数列是标准正交基。

如果\(\{ \phi\_k(x) \}\_{k=1}^\infty\)为\(L^2(E)\)的一个标准正交系，
则以下条件等价：

* `(1)` \(\{ \phi\_k(x) \}\_{k=1}^\infty\)是标准正交基；
* `(2)` \(\{ \phi\_k(x) \}\_{k=1}^\infty\)的有限线性组合构成的集合在\(L^2(E)\)中稠密；
* `(3)` \(\{ \phi\_k(x) \}\_{k=1}^\infty\)是完全的；
* `(4)` Parseval等式成立：
  \[
  \| f \|^2 = \sum\_{k=1}^\infty \langle f, \phi\_k \rangle^2 ,
  \ \forall f \in L^2(E) ;
  \]
* `(5)` 内积等式：
  \[
  \langle f, g \rangle
  = \sum\_{k=1}^\infty \langle f, \phi\_k \rangle \cdot \langle g, \phi\_k \rangle,
  \ \forall f, g \in L^2(E) .
  \]

如果\(L^2(E)\)中有可数的线性无关的函数列，
使得其有限线性组合在\(L^2(E)\)中稠密，
可以用Gram-Schmidt正交化方法将其转化为标准正交基。

例：\(L^2[-\pi, \pi]\)上三角函数列是正交基；
\(L^2[-,1]\)的Legendre正交多项式是正交基；
\(L^2(-\infty, \infty)\)的Hermite多项式是标准正交基。

## E.11 卷积和傅立叶变换

### E.11.1 复值函数

傅立叶变换用复值函数表示比较方便，
将实值函数推广到复值函数。
设\(u(x), v(x)\)是\(\mathbb R^n\)的可测集\(E\)上的几乎处处有限的可测函数，
令\(f(x) = u(x) + i v(x)\)，
称\(f(x)\)为几乎处处有限的复值可测函数，
若\(\int\_E |f(x)| \,dx < \infty\)，
称\(f(x)\)在\(E\)上可积，
\(E\)上可积函数的全体仍记为\(L(E)\)，
\[
\left| \int\_E f(x) \,dx \right|
\leq \int\_E |f(x)| \,dx .
\]

\(L^2(E)\)由几乎处处有限的复值函数且
\[
\| f \|\_p = \left( \int\_E |f(x)|^p \,dx \right)^{1/p} < \infty
\]
的函数组成。其数乘用复数域。

对范数\(\|\cdot \|\_p\)，齐次性中的\(\alpha\)可以是复数；
对复数域上\(L^2(E)\)的内积，定义为：
\[
\langle f, g \rangle
= \int\_E f(x) \overline{g(x)} \,dx,
\]
满足如下性质：

* `(1)` 共轭双线性：关于第一自变量是线性函数，关于第二自变量是共轭线性的；
* `(2)` 共轭对称性：\(\langle f, g \rangle = \overline{\langle g, f \rangle}\)；
* `(3)` 正定性与实数域情形相同。

实数域上\(L^2(E)\)中的弱收敛性、正交性、傅立叶级数展开、标准正交基等性质都适用于复数域上的\(L^2(E)\)空间。
性质中的傅立叶系数\(\{ c\_k \}\)变成复数列，
\(\sum\_{k=1}^\infty c\_k^2\)改为\(\sum\_{k=1}^\infty |c\_k|^2\)，
稠密集\(G\)的定义中系数\(\{ c\_k \}\)也变成复数列，
有标准正交基时内积等式改为\(\langle f, g \rangle = \sum\_{k=1}^\infty \langle f, \phi\_k \rangle \; \overline{\langle g, \phi\_k \rangle}\)。

### E.11.2 卷积

**卷积**：
设\(f, g\)是\(\mathbb R^n\)上的实值或复值的可测函数，
如果积分
\[
\int\_{\mathbb R^n} f(x - y) g(y) \,dy
\]
存在，就称此积分为函数\(f\)和\(g\)的卷积，
记为\(f\*g(x)\)。
卷积有对称性：
\[
f\*g = g\*f .
\]

Yang不等式：
若\(1 \leq p \leq \infty\),
\(f \in L^p(\mathbb R^n)\)，
\(g \in L^1(\mathbb R^n)\)，
则\(f\*g \in L^p(\mathbb R^n)\)，且
\[
\| f\*g \|\_p \leq \| f \|\_p \| g \|\_1 .
\]
特别地，当\(f, g\)都可积时，
卷积存在且\(\| f\*g \|\_1 \leq \|f\|\_1 \|g\|\_1\)。
所以卷积运算在\(L(\mathbb R^n)\)空间中是封闭的运算。

平均连续性：
对\(1 \leq p < \infty\)和\(f \in L^p(\mathbb R^n)\)，有
\[
\lim\_{\Delta x \to 0} \| f(x + \Delta x) - f(x) \| = 0 .
\]

设\(f, g \in L^2(\mathbb R^n)\)，
则\(f\*g(x)\)是\(\mathbb R^n\)上的有界连续函数。

设\(1 \leq p < \infty\),
\(f \in L^p(\mathbb R)\)，
非负函数\(\phi \in L^1(\mathbb R^n)\)，
\(\| \phi \|\_1 = 1\)，
\(\phi\_{\varepsilon}(x) = \varepsilon^{-n} \phi(\frac{x}{\varepsilon})\)，
则
\[
\lim\_{\varepsilon\to 0} \| \phi\_{\varepsilon} \* f - f \|\_p = 0.
\]
这里\(\phi\_{\varepsilon} \* f \in L^p(\mathbb R^n)\)，是\(f\)的逼近。
如果\(f \in L^2(\mathbb R^n)\)，
\(\phi \in L^2(\mathbb R^n)\)且\(\|\phi\|\_1=1\)，
则\(\phi\_{\varepsilon} \* f \in L^2(\mathbb R^n)\)是连续有界函数，
\(\phi\_{\varepsilon} \* f\)可以看成是对不连续的\(f\)进行了局部平均，
变成了连续函数。

如果\(\phi\_{\varepsilon}(x)\)有任意阶偏导数，
则\(\phi\_{\varepsilon} \* f\)也有任意阶偏导数，
从而\(L^p(E)\)中的函数可以被有任意阶偏导数的函数逼近，
记\(C^\infty(\mathbb R^n)\)为\(\mathbb R^n\)中有任意阶偏导数的函数全体，
则\(C^\infty(\mathbb R^n)\)限制在\(E\)上的函数的集合在\(L^p(E)\)中稠密。

### E.11.3 傅立叶变换

**傅立叶变换**：对任意\(f \in L(\mathbb R^n)\)，
令
\[
\hat f(t) = (2\pi)^{-\frac{n}{2}} \int\_{\mathbb R^n}
f(x) e^{-it \cdot x} \,dx,
\ \forall t \in \mathbb R^n,
\]
其中\(t \cdot x = \sum\_{i=1}^n t\_i x\_i\)，
称\(\hat f\)是\(f\)的傅立叶(Fourier)变换，
可记为\(\mathcal F f\)，
显然\(\mathcal F\)是线性映射：
\[
\mathcal F (\alpha f + \beta g)
= \alpha \mathcal F f + \beta \mathcal F g .
\]

因为\(|e^{-i t \cdot x}|=1\)，
所以\(\hat f\)定义右侧的积分存在有限，
且
\[
|\hat f(t)| \leq (2\pi)^{-\frac{n}{2}} \| f \|\_1,
\ \forall t \in \mathbb R^n,
\]
由控制收敛定理可以证明\(\hat f(t)\)连续，
所以\(f \in L(\mathbb R^n)\)时\(\hat f\)为有界连续函数，
还可以证明
\[
\lim\_{t\to\infty} \hat f(t) = 0 .
\]

引入记号
\[
e\_t(x) = e^{it \cdot x},
\]
将其看成是\(x \in \mathbb R^n\)的函数。
对于多重指标\(\alpha = (\alpha\_1, \dots, \alpha\_n)\)，
其中\(\alpha\_1, \dots, \alpha\_n\)都是非负整数，
记\(|\alpha| = \sum\_{i=1}^n \alpha\_i\)，
记
\[\begin{aligned}
D^{\alpha} =& \left( \frac{\partial}{\partial x\_1} \right)^{\alpha\_1}
\cdots \left( \frac{\partial}{\partial x\_n} \right)^{\alpha\_n}, \\
D\_{\alpha} =& i^{-|\alpha|} D^{\alpha}
= \left(\frac{1}{i} \frac{\partial}{\partial x\_1} \right)^{\alpha\_1}
\cdots \left(\frac{1}{i} \frac{\partial}{\partial x\_n} \right)^{\alpha\_n},
\end{aligned}\]
对\(\mathbb R^n\)上的多项式
\[
P(z) = \sum\_{\alpha} c\_{\alpha} z^{\alpha}
= \sum\_{\alpha} c\_{\alpha} z\_1^{\alpha\_1}\cdots z\_n^{\alpha\_n},
\]
记
\[
P(D) = \sum\_{\alpha} c\_{\alpha} D\_{\alpha},
\quad P(-D) = \sum\_{\alpha} (-1)^{|\alpha|} c\_{\alpha} D\_{\alpha},
\]
则
\[
P(D) e\_t = P(t) e\_t .
\]

对\(f\)和\(x, y \in \mathbb R^n\)，
记平移算子
\[
\tau\_x f(y) = f(y-x) .
\]

\(L(\mathbb R^n)\)上的傅立叶变换有如下性质：

* `(1)` \(\mathcal F(\tau\_s f) = e\_{-s} \hat f\);
* `(2)` \(\mathcal F(e\_s f) = \tau\_s \hat f\);
* `(3)` \(\mathcal F(f\*g) = \hat f \hat g\);
* `(4)` 对\(\lambda>0\), \(\mathcal F(f(\frac{x}{\lambda}))(t) = \lambda^n \hat f(\lambda t)\).

引入记号：

* \(C(\mathbb R^n)\)为\(\mathbb R^n\)上连续函数的全体（实值或者复值）。
* \(C\_c(\mathbb R^n)\)为\(\mathbb R^n\)上有紧支集的连续函数的全体。
* \(C^\infty(\mathbb R^n)\)为\(\mathbb R^n\)上光滑函数的全体，记有任意阶偏导数的函数的全体。
* \(C\_c^\infty(\mathbb R^n)\)为\(\mathbb R^n\)上有紧支集的光滑函数的全体。

**速降函数**：
若\(f \in C^\infty(\mathbb R^n)\)，
对任意\(N=0,1,2,\dots\)，有
\[
\sup\_{|\alpha| \leq N} \sum\_{x \in \mathbb R^n}
(1 + |x|^2)^N | D^\alpha f(x)| < \infty,
\]
则称\(f\)为速降函数，
这些函数的全体记为\(\mathcal S(\mathbb R^n)\)。
速降函数的任意阶偏导数乘以任意多项式都可积。

\(\mathcal S(\mathbb R^n)\)在\(L^p(\mathbb R^n)\)（\(1 \leq p < \infty\)）中稠密。
\(C\_c^\infty(\mathbb R^n)\)在\(L^p(\mathbb R^n)\)（\(1 \leq p < \infty\)）中稠密。

算子\(\mathscr F\)是速降函数空间\(\mathcal F(\mathbb R^n)\)上一个一一的线性的满映射，
且保持\(L^2(\mathbb R^n)\)模不变。
对\(f, g \in \mathcal S(\mathbb R^n)\), 多项式\(P\), 多重指数\(\alpha\)，
\(\mathcal F\)有如下性质：

* `(1)` \(P \cdot f\), \(f \cdot g\)，\(f\*g\), \(D\_{\alpha} f\)都属于\(\mathcal S(\mathbb R^n)\)。
* `(2)` \(\mathcal F(P(D) f) = P \cdot \hat f\), \(\mathcal F(P \cdot f) = P(-D) \hat f\)；
* `(3)` \(\hat f \in \mathcal S(\mathbb R^n)\)。
* `(4)` \(\mathcal F(f \cdot g) = \hat f\*\hat g\)。

速降函数的逆傅立叶变换定理：

`(1)` 若\(f \in \mathcal S(\mathbb R^n)\)，则
\[
f(x) = (2\pi)^{-\frac{n}{2}} \int\_{\mathbb R^n} \hat f(t) e^{i x \cdot t} \,dt .
\]

`(2)` \(\mathcal F\)是\(\mathcal S(\mathbb R^n)\)一一的线性的满映射，且\(\mathcal F^4\)为恒等变换。

`(3)` 若\(f, \hat f \in L(\mathbb R^n)\)，则
\[
f(x) = (2\pi)^{-\frac{n}{2}} \int\_{\mathbb R^n} \hat f(t) e^{i x \cdot t} \,dt , \text{ a.e.}
\]

可以将\(\mathcal S(\mathbb R^n)\)的一一变换\(\mathcal F\)拓展到\(L^2(\mathbb R^n)\)：
存在\(L^2(\mathbb R^n)\)的一个一一的线性满映射\(\Psi\)，使得
\[
\Psi f = \mathcal F, \ \forall f \in \mathcal S(\mathbb R^n),
\]
且
\[
\| \Psi f \|\_2 = \| f \|\_2,
\ \forall f \in L^2(\mathbb R^n) ,
\]
\(\Psi^4\)是恒等映射，
称\(\Psi\)为\(L^2(\mathbb R^n)\)上的傅立叶变换。

对\(f, g \in L^2(\mathbb R^n)\)有
\[\begin{aligned}
\langle \Psi f, \Psi g \rangle
=& \langle f, g \rangle
= \langle \Psi^{-1} f, \Psi^{-1} g \rangle, \\
\langle f, \Psi g \rangle
=& \langle \Psi^{-1} f, g \rangle, \\
\int\_{\mathbb R^n} (\Psi f)(x) g(x) \,dx
=& \int\_{\mathbb R^n} f(x) (\Psi g)(x) \,dx .
\end{aligned}\]

注意\(\langle f, g \rangle = \int\_{\mathbb R^n} f(x) \overline{g(x)} \,dx\)。

## E.12 距离空间

### E.12.1 定义

设\(X\)为实数域上的向量空间，
如果函数\(d: X \times X \to \mathbb R\)满足如下性质：

* `(1)` 正定性：\(d(x,y) \geq 0\), \(\forall x, y \in X\),
  \(d(x,y)=0\)当且仅当\(x=y\);
* `(2)` 对称性：\(d(x,y) = d(y,x)\), \(\forall x, y \in X\)；
* `(3)` 三角不等式：
  \(d(x, y) \leq d(x, z) + d(z, y)\), \(\forall x, y, z \in X\)。

则称\(d(x,y)\)为\(X\)上一个距离，
\((X, d)\)称为一个距离空间。

从距离，
可以引出球邻域、余集、开集、闭集、矩体、Borel σ代数、稠密性等概念。
\(X\)的子集中\(\emptyset\)和全集既是开集又是闭集。

例：\(\mathbb R^n\)，欧式距离。

例：\(C[a,b]\)表示定义在\([a,b]\)上的连续函数全体，
定义距离为
\[
d(f, g) = \max\_{x \in [a,b]} |f(x) - g(x)| .
\]

例：\(C[0, \infty)\)表示定义在\([0, \infty)\)上所有连续函数构成的线性空间。
在其中定义距离
\[
\rho(f, g)
= \sum\_{n=1}^\infty \frac{1}{2^n}
\max\_{t \in [0, n]}(|f(t) - g(t)| \wedge 1) .
\]

例：\(L^p(E)\), \(1 \leq \infty\)，距离
\[
d(f, g) = \| f - g \|\_p .
\]

例：\(l^p\)空间（\(1 \leq p < \infty\)），其中元素为复数列(或实数列)\(\{ x\_k \}\)，满足
\[
\sum\_{k=1}^\infty x\_k^p < \infty .
\]

距离空间\((X, d)\)中的序列\(\{ x\_k \}\)和元素\(x\)如果满足
\[
d(x\_k, x) \to 0,
\ k \to \infty,
\]
称\(\{ x\_k \}\)**收敛**到\(x\)，
记为\(\lim\_{k\to\infty} x\_k = x\)，
称\(\{ x\_k \}\)为收敛列。
极限唯一。

收敛是按距离定义的，
比如在\(C[a,b]\)中收敛是函数的一致收敛。

如果\(\lim\_{k,m \to \infty} d(x\_m, x\_k) = 0\)，
称\(\{ x\_k \}\)为**基本列**，
如果\(X\)的所有基本列都收敛到\(X\)中，
则称\(X\)为完备的距离空间。

空间\(C[a,b]\)，\(L^p(E)\)，\(l^2\)是完备的；
\([a,b]\)上的多项式全体按\(\max\_{x \in [a,b]} |f(x) - g(x)|\)距离是不完备的，
\([a,b]\)上黎曼可积函数全体按\(L^1\)距离是不完备的。

对\(A \subset X\)，
如果存在元素互不相同的序列\(\{ x\_k \} \subset k\)使得\(\lim\_k x\_k = x\)，
称\(x\)为\(A\)的一个**聚点**或者极限点。

**同构映射**：给定距离空间\((X, d\_1)\)和\((Y, d\_2)\)，
设\(T\)是\(X\)到\(Y\)的映射，
如果\(\forall x\_1, x\_2 \in X\)都有\(d\_1(x\_1, x\_2) = d\_2(T x\_1, T x\_2)\)，
就称\(T\)是**等距映射**；
如果\(T\)还是满映射，
称\(X\)与\(Y\) **等距同构**。

两个等距同构的距离空间，
其一切与距离相联系的性质（开集、闭集、收敛、连续等）都是一样的，
可以视为是等价的。
当\(T\)是\(X\)到\(Y\)的等距映射时，
\(X\)与\(T X \subset Y\)等距同构，
可以将\((X, d\_1)\)看成是\((Y, d\_2)\)的子距离空间。

**稠密子集**：
设\((X, d)\)是一个距离空间，
集合\(E \subset X\)满足：
\(\forall x \in X\)，
存在\(\{ x\_m \} \subset E\)，
使得\(d(x\_m, x) \to 0\)，
就称\(E\)为\(X\)的稠密子集。

例：\(C[a,b]\)中多项式空间\(P[a,b]\)稠密。
\(L^p(E)\)中\(C\_c^\infty(E)\)(具有紧支集和任意阶偏导数的函数)稠密。

**距离空间的完备化**：
任给距离空间\((X\_0, d\_0)\)，
必存在一个完备距离空间\((X,d)\)，
使得\(X\_0\)与\(X\)的一个稠密子集\(X'\)等距同构，
且在等距映射之间不区分的意义下，
\((X,d)\)是唯一确定的，
这时\((X,d)\)称为\((X\_0, d\_0)\)的完备化空间。

例：在\(L^1\)距离下，\([a,b]\)上黎曼可积函数空间不完备，
完备化空间是\(L^1[a,b]\)；
\([a,b]\)上黎曼可积函数空间在\(L^1[a,b]\)中稠密。

### E.12.2 列紧性和可分性

对于\(\mathbb R^n\)空间，
元素个数无穷的有界子集一定有收敛子列，
一般的距离空间则不一定，
比如\(L^2[a,b]\)空间有可数的标准正交基，
有界但没有收敛子序列。

**有界**：
设\((X,d)\)为距离空间，
\(A \subset X\)，
如果存在\(x\_0 \in X\), \(r > 0\)，
使得\(A \subset B(x\_0, r) = \{x: d(x, x\_0) < r \}\)，
则称\(A\)为有界子集。

**列紧**：
设\((X,d)\)为距离空间，
\(A \subset X\)，
如果\(A\)中任意点列中有一个收敛子列，
极限取值于\(X\)中，
称\(A\)为列紧的。
如果这些收敛子列都收敛到\(A\)中的点，
称\(A\)为**自列紧**的。
如果\(X\)是列紧的，
则是自列紧的，
称\(X\)为列紧空间。

列紧空间必为完备距离空间，
其中任意子集都是列紧集，
任意闭子集都是自列紧集。

对\(\mathbb R^n\)，
有界集都是列紧集，
自列紧集等价于有界闭集。

距离空间中子集有界不一定列紧，
对完备距离空间，
列紧等价于如下的完全有界。

**完全有界**：
给定距离空间\((X,d)\)，\(M \subset X\)。
设\(N \subset M\), \(\varepsilon > 0\)，
若\(\forall x \in M\), 总存在\(y \in N\)使得\(x \in B(y, \varepsilon)\)，
则称\(N\)是\(M\)的一个\(\varepsilon\)网；
如果\(N\)还是一个有穷集，
称\(N\)是\(M\)的一个有穷\(\varepsilon\)网。
如果\(\forall \varepsilon\),
\(M\)都存在有穷\(\varepsilon\)网，
称\(M\)是**完全有界**的。

**Hausdorff定理**：对距离空间\((X,d)\)及\(M \subset X\)，

* `(1)` 如果\(M\)在\(X\)中列紧，则\(M\)完全有界；
* `(2)` 若\(X\)是完备空间，\(M\)完全有界，则\(M\)列紧。

**可分距离空间**：
距离空间如果存可数稠密子集，
就称为可分的。

完全有界的距离空间是可分的。
列紧必完全有界，所以列紧的距离空间是可分的。

例：\(C[a,b]\)和\(C[0, \infty)\)是完备可分距离空间。

**紧集**：
设\(M\)是距离空间\((X,d)\)的一个子集，
\(\Sigma = \{ G\_l, I \in I\}\)是\(X\)的开集族，
如果\(M \subset \bigcup\_{l \in I} G\_l\)，
则称\(\Sigma\)是\(M\)的一个开覆盖；
如果\(M\)的任意开覆盖\(\Sigma = \{ G\_l, I \in I\}\)中都有有限子覆盖，
即存在\(G\_{l\_1}, G\_{l\_2}, G\_{l\_m}\)使得\(M \subset \bigcup\_{i=1}^m G\_{l\_i}\)，
就称\(M\)为紧致集或紧集。

距离空间的紧集（紧致集）和自列紧集等价。

设\(M\)是一个紧距离空间（自列紧，也是紧致），
距离为\(\rho\)，
\(C(M)\)是\(M\)上定义的所有的实值或者复制的连续函数组成的集合，
定义
\[
d(f, g) = \max\_{x \in M} | f(x) - g(x) |,
\ \forall f, g \in C(M),
\]
则\(d(\cdot, \cdot)\)是距离，
且\(C(M)\)是完备距离空间。

Arzela-Ascoli定理：
集合\(F \subset C(M)\)是一个列紧集（紧致集）的充分必要条件为\(F\)一致有界且等度连续。

\(F \subset C(M)\)一致有界，
是存在\(K \in \mathbb R\)使得
\[
|f(x)| \leq K,
\ \forall x \in M, \ \forall f \in F .
\]

\(F \subset C(M)\)等度连续，
是\(\forall \varepsilon > 0\)，
\(\exists \delta = \delta(\varepsilon) > 0\)，
使得\(\forall f \in F\)，
以及\(x\_1, x\_2 \in M\)，
只要\(\rho(x\_1, x\_2) < \delta\)，
就有
\[
| f(x\_1) - f(x\_2) | < \varepsilon .
\]

\(F \subset M\)等度连续是其中每个函数都一致连续，
且其中的\(\delta\)不依赖于\(f \in F\)。

关于\(l^2\)中子集列紧的条件，
\(L^p(E)\)值子集列紧的条件，
见([郭懋正 2005](#ref-GuoMZ2005:RealFuncAna)) pp 217-218。

### E.12.3 连续映射

**连续映射**：
设\((X, d\_1)\)和\((Y, d\_2)\)是两个距离空间，
映射\(T : X \to Y\)，
\(\forall \varepsilon > 0\),
\(\exists \delta = \delta(x\_0, \varepsilon) > 0\)，
使得\(\forall x \in X\),
\[
d\_1(x, x\_0) < \delta
\Longrightarrow
d\_2(T x, T x\_0) < \varepsilon,
\]
就称\(T\)在点\(x\_0\)处连续；
如果\(T\)在\(X\)的所有点都连续，
称\(T\)是\(X\)上的连续映射。

\(T\)是\(X\)上的连续映射，
当且仅当\(\forall x\_0 \in X\)和\(\{x\_m \} \subset X\)，有
\[
\lim\_{m\to\infty} d\_1(x\_0, x\_m) = 0
\Longrightarrow
\lim\_{m\to\infty} d\_2(T x\_0, T x\_m) = 0 .
\]

**压缩映射**：
设\((X, d)\)为距离空间，
\(T\)是\(X\)到\(X\)的映射，
如果存在\(0 < a < 1\)，使得
\[
d(T x\_1, T x\_2) \leq a\, d(x\_1, x\_2),
\ \forall x\_1, x\_2 \in X,
\]
就称\(T\)是\(X\)上的一个压缩映射。

**压缩映射原理**（Banach不动点定理）：
设\((X,d)\)是一个完备距离空间，
\(T\)是\(X\)到自身的一个压缩映射，
则存在唯一的\(x^\*\)使得\(T x^\* = x^\*\)，
称\(x^\*\)为\(T\)的不动点。

例：\(X = [0,1]\)，
\(f(x)\)是\([0,1]\)上可微函数，满足
\[\begin{aligned}
& f(x) \in [0,1], \ \forall x \in [0,1], \\
& |f'(x)| \leq a < 1, \ \forall x \in [0,1],
\end{aligned}\]
则\(f\)是\(X\)上一个压缩映射。
存在\(x^\* \in [0,1]\)使得\(f(x^\*) = x^\*\)。

## E.13 Hilbert空间

### E.13.1 定义

记\(\mathbb K\)为实数域\(\mathbb R\)或者复数域\(\mathbb C\)。

**内积空间**：
设\(X\)是数域\(\mathbb K\)上的线性空间，
如果\(X\)上的一个二元函数\(g(x,y): X \times X \to \mathbb K\)满足：

* `(1)` \(g(\alpha x + \beta y, z) = \alpha g(x,z) + \beta g(y,z)\);
* `(2)` \(g(x, \alpha y + \beta z) = \overline{\alpha} g(x, y) + \overline{\beta} g(x,z)\);
* `(3)` \(g(x,x) \geq 0\), \(g(x,x)=0 \Leftrightarrow x = 0\);
* `(4)` \(g(x,y) = \overline{g(y,x)}\)

对任意\(x, y, z \in X\), \(\alpha, \beta \in \mathbb K\)成立，
则称\(g(x,y)\)为\(X\)上的一个内积，
称\((X, g(\cdot,\cdot))\)为一个内积空间。
如果仅有一个内积，经常简记为\(\langle x, y \rangle\)。

定义中(1)和(2)称为共轭双线性，
当\(\mathbb K = \mathbb R\)时称为双线性。
(3)称为正定性。
(4)称为对称性。
如果(3)中仅要求\(g(x,x) \geq 0\)，
就称\(g(x,y)\)为一个半内积，
称\(X, g(\cdot,\cdot)\)为半内积空间。

例1.
欧式空间\(\mathbb R^n\)定义内积
\[
\langle x, y \rangle
= \sum\_{i=1}^n x\_i y\_i .
\]

欧式空间\(\mathbb C^n\)定义内积
\[
\langle x, y \rangle
= \sum\_{i=1}^n x\_i \bar y\_i .
\]

例2.
\(l^2\)空间定义内积
\[
\langle x, y \rangle
= \sum\_{i=1}^\infty x\_i \bar y\_i .
\]

例3.
\(C^k(\bar \Omega)\)是\(\mathbb R^n\)中有界闭集\(\bar \Omega\)上有\(k\)阶连续偏导数的函数的全体，
定义内积
\[
\langle f, g \rangle
= \sum\_{|\alpha| \leq k} \partial^{\alpha} f(x) \overline{\partial^{\alpha} g(x)},
\ \forall f, g \in C^k(\bar \Omega) .
\]

例4.
设\((X, \mathscr F, \mu)\)为测度空间，
\(L^2(X, \mathscr F, \mu)\)是\((X, \mathscr F, \mu)\)所有平方可积的复值函数全体，
即其中的函数满足
\[
\| f \|\_2 = \left( \int\_X |f(x)|^2 \,dx \right)^{1/2} < \infty,
\]
两个函数在空间中相等定义为依\(\mu\)几乎处处相等。
定义内积
\[
\langle f, g \rangle
= \int\_X f(x) \overline{g(x)} \,dx,
\]
则\((X, \langle \cdot, \cdot \rangle)\)构成内积空间。

特别地，对概率测度空间\((\Omega, \mathscr F, P)\)，
\(L^2(\Omega, \mathscr F, P)\)为其中二阶矩有限的复值随机变量全体，
即
\[
L^2(\Omega, \mathscr F, P)
= \{\xi: \xi \text{是} (\Omega, \mathscr F, P) \text{上的复值随机变量，且}
E(|\xi|^2) < \infty \},
\]
这是一个线性空间，定义内积
\[
\langle \xi, \eta \rangle
= \int\_{\Omega} \xi(\omega) \overline{\eta(\omega)} \,dP(\omega)
= E(\xi \bar \eta),
\ \forall \xi, \eta \in L^2(\Omega, \mathscr F, P) .
\]

**内积导出的范数**：
对内积空间\((X, \langle \cdot, \cdot \rangle)\)，
定义
\[
\| x \| = \langle x, x \rangle^{1/2},
\ \forall x \in X,
\]
则\(\|\cdot\|\)是\(X\)上的范数，
称为内积\(\langle \cdot, \cdot \rangle\)导出的范数（模）。

**Cauchy-Schwarz不等式**：
设\(\|\cdot\|\)是内积\(\langle \cdot, \cdot \rangle\)导出的范数，
则
\[
| \langle x,y \rangle |
\leq \| x \| \, \| y \|,
\ \forall x, y \in X,
\]
且等号成立当且仅当\(x, y\)线性相关。

实际上，Cauchy-Schwarz不等式可以推广。

**共轭范数**：对内积空间\((X, \langle \cdot, \cdot \rangle)\)，
设\(\| \cdot \|\)是一个范数，
定义
\[
\| x \|^\* = \sup\_{\| y \| = 1} \langle x,y \rangle ,
\]
则\(\| \cdot \|^\*\)也是一个范数，
称为\(\| \cdot \|\)的共轭范数。

对\(p \geq 1, q \geq 1\)，若\(\frac{1}{p} + \frac{1}{q} = 1\)，
则\(\|\cdot \|\_p\)和\(\|\cdot \|\_q\)在\(\mathbb R^n\)中互为共轭范数，
特别地，\(\| \cdot \|\_2\)和\(\| \cdot \|\_2\)互为共轭范数。
另外，
\(\| \cdot \|\_1\)和\(\| \cdot \|\_{\infty}\)互为共轭范数。

**Hölder**不等式：
对内积空间\((X, \langle \cdot, \cdot \rangle)\)，
设\(p \geq 1, q \geq 1\)，\(\frac{1}{p} + \frac{1}{q} = 1\)，
则对任意\(x, y \in X\)，有
\[
| \langle x,y \rangle |
\leq \| x \|\_p \, \| y \|\_q,
\]
等号成立当且仅当\(|y\_i| = |x\_i|^{p-1}, x\_i y\_i \geq 0\), \(i=1,2,\dots,n\)。
证明见[C.2.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-prob.html#base-prob-momineq-Holder)。

**内积导出的距离**：
有了导出的范数，定义
\[
d(x,y) = \| x - y \|,
\ \forall x, y \in X,
\]
则\(d(\cdot, \cdot)\)是\(X\)上的距离，
称为内积\(\langle \cdot, \cdot \rangle\)导出的距离，
内积空间\((X, \langle \cdot, \cdot \rangle)\)也是距离空间。

**Hilbert空间**：
对内积空间\((X, \langle \cdot, \cdot \rangle)\)，
设由内积导出的距离为\(d(\cdot,\cdot)\)，
如果\((X, d)\)构成完备距离空间，
则称\((X, \langle \cdot, \cdot \rangle\)为Hilbert空间。

例1的欧式空间\(\mathbb R^n\)和\(\mathbb C^n\)是Hilbert空间。
例2的\(l^2\)是Hilbert空间。
例4的\(L^2(X, \mathcal F, \mu)\)是Hilbert空间，
特别地，
\(L^2(E)\)(\(E\)为\(\mathbb R^n\)中可测集)是Hilbert空间；
\(L^2(\Omega, \mathscr F, P)\)是Hilbert空间。

\(\mathbb R^n\)中有界闭集\(\bar\Omega\)上有\(k\)阶连续偏导数的函数全体\(C^k(\bar\Omega)\)不是完备距离空间。

**例**：
概率空间\((\Omega, \mathscr F, P)\)上所有二阶矩有限的实值随机变量的集合记作\(L^2\)，
\(L^2\)是Hilbert空间，
内积为\(<X, Y> = E(XY)\)。
0元素是a.s.等于零意义上，
两个元素相等也是a.s.相等意义上。
来证明完备性。

**完备性证明**:
设\(\{ X\_n \}\)为\(L^2\)的Cauchy列，
则存在\(\{ n \}\)的子序列\(n\_k\)，
使得当\(n, m \geq n\_k\)时，
\[
\| X\_n - X\_m \| \leq 2^{-3k}
\]
由切比雪夫不等式
\[
P(|X\_n - X\_m| \geq 2^{-k})
\leq 2^{2k} E(X\_n - X\_m)^2
\leq 2^{-k}
\]
由单调收敛定理得
\[\begin{aligned}
& E \sum\_{k=1}^\infty I[|X\_{n\_{k+1}} - X\_{n\_k} \geq 2^{-k}] \\
=& \sum\_{k=1}^\infty E I[|X\_{n\_{k+1}} - X\_{n\_k} \geq 2^{-k}] \\
=& \sum\_{k=1}^\infty P(|X\_{n\_{k+1}} - X\_{n\_k} \geq 2^{-k}) \\
\leq& \sum\_{k=1}^\infty 2^{-k} < \infty
\end{aligned}\]
从而
\[
\sum\_{k=1}^\infty I[|X\_{n\_{k+1}} - X\_{n\_k}| \geq 2^{-k}] < +\infty, \ \text{a.s.}
\]
存在\(\Omega^\* \subset \Omega\),
\(P(\Omega^\*)=1\)，
使得\(\forall \omega \in \Omega^\*\)，
存在\(K\_0\)使得\(k \geq k\_0\)时\(|X\_{n\_{k+1}} - X\_{n\_k}| < 2^{-k}\)。
于是\(k\)充分大时
\[
|X\_{n\_{k+m}} - X\_{n\_k}|
\leq \sum\_{j=1}^m | X\_{n\_{j+1}} - X\_{n\_j} |
\leq \sum\_{j=1}^m 2^{-(k + j)}
\leq 2^{-k + 1}
\]
于是对每个\(\omega \in \Omega^\*\)，
存在子序列\(X\_{n\_k}\)使得\(\{ X\_{n\_k} \}\)是实数基本列，存在极限\(X\)。
利用Fatou引理，
\[\begin{aligned}
E(X\_n - X)^2
=& E \lim\_{k\to\infty} [X\_n - X\_{n\_k}]^2 \\
\leq& \varliminf\_{k\to\infty} E(X\_n - X\_{n\_k})^2 \\
\to& 0, \ \text{当} n \to \infty
\end{aligned}\]
由三角不等式，
\[
\sqrt{E X^2}
= \| X \|
\leq \| X\_n - X \| + \| X\_n \| < \infty
\]
所以极限\(X \in L^2\)。
这就证明了\(L^2\)的完备性。
因为\(L^2\)中的元素是在a.s.相等意义下的，
所以\(X\)可以是a.s.可测的。

○○○○○○

**完备化**；
设内积空间\((X, \langle \cdot, \cdot \rangle)\)与导出的距离\(d\)不构成完备距离空间，
则存在Hilbert空间\(H\)，
使得\(H\)上由内积导出的距离与\(H\)一起成为\((X, d)\)的完备化距离空间，
且
\[
\langle x,y \rangle\_X
= \langle x, y \rangle\_H,
\ \forall x, y \in X.
\]

所以，不完备的内积空间可以完备化成为Hilbert空间。
\(\mathbb R\)上的Hilbert空间也可以嵌入到复数域上的Hilbert空间中。

例3的\(C^k(\bar\Omega)\)不完备，
其完备化的Hilbert空间记为\(H^k(\bar\Omega)\)，
称为索伯列夫（Sobolev）空间。

### E.13.2 正交

设\(H\)为Hilbert空间，
若\(<\boldsymbol x, \boldsymbol y>=0\)则称\(\boldsymbol x\)和\(\boldsymbol y\)正交，
记作\(\boldsymbol x \perp \boldsymbol y\)。
设\(A, B\)是\(H\)的非空子集，
如果\(\forall x \in A, y \in B\)有\(x \perp y\)，
则称\(A\)与\(B\)正交，记作\(A \perp B\)。

记
\[
\text{span}(A)
= \{ \sum\_{i=1}^n \alpha\_i x\_i: \alpha\_i \in K, x\_i \in A, i=1,2,\dots,n, n \geq 1 \},
\]
称\(\text{span}(A)\)为\(A\)的线性包。
如果\(x \perp A\)，
则\(x \perp \text{span}(A)\)。

记
\[
A^\perp = \{ x \in H: x \perp A \},
\]
称为\(A\)的正交补，
这是\(H\)的闭子空间，
也是子Hilbert空间。

如果\(x\_1, x\_2, \dots, x\_n\)在\(H\)中两两正交，
则
\[
\| x\_1 + x\_2 + \dots + x\_n \|^2
= \| x\_1 \|^2 + \| x\_2 \|^2 + \dots + \| x\_n \|^2 .
\]

设\(S\)是\(H\)的子线性空间，
如果\(S\)中的收敛序列的极限都在\(S\)中，
称\(S\)为\(H\)的**闭子空间**，也称为子Hilbert空间。

设\(S\)是\(H\)的闭子空间。
若\(\forall \boldsymbol z \in S\)都有\(\boldsymbol x \perp \boldsymbol z\)，
称\(\boldsymbol x\)与\(S\)正交，
记作\(\boldsymbol x \perp S\)。

记所有与\(S\)正交的元素组成集合为\(S^\perp\),
这也是\(H\)的闭子空间，称为\(S\)的**正交补空间**。
\(\boldsymbol x \perp S\)当且仅当\(\boldsymbol x \in S^\perp\)。

**正交集**：
设\(\mathcal E\)是Hilbert空间\(H\)的一个子集，
如果\(\forall x, y \in \mathcal E\)且\(x \neq y\)均有\(x \perp y\)，
称\(\mathcal E\)为正交集。
如果进一步\(\forall x \in \mathcal E\)有\(\| x \| = 1\)，
称\(\mathcal E\)为标准正交集。
如果\(\mathcal E^\perp = \{ 0 \}\)(其中0表示线性空间\(H\)的零元素)，
即不存在\(x \neq 0\)使得\(x \perp \mathcal E\)，
就称\(\mathcal E\)是完备的正交集。

根据Zorn引理可以证明：

**定理**：非零Hilbert空间\(H\)中必存在完备正交集。

**Bessel不等式**：
设\(\mathcal E = \{ e\_\alpha: \alpha \in \Lambda \}\)为Hilbert空间\(H\)的一个标准正交集，
其中\(\Lambda\)是一个集合，
则\(\forall x \in H\)，有
\[
\sum\_{\alpha \in \Lambda}
| \langle x, e\_\alpha \rangle |^2
\leq \| x \|^2 .
\]
上式左边的求和中非零项至多有可数个。

**正交基**：
设\(\mathcal E = \{ e\_\alpha: \alpha \in \Lambda \}\)为Hilbert空间\(H\)的一个标准正交集，
如果\(\forall x \in H\)，有
\[
x = \sum\_{\alpha \in \Lambda} \langle x, e\_\alpha \rangle e\_\alpha,
\]
则称\(\mathcal E\)为\(H\)的一个标准正交基，
简称为基，
其中的\(\{ \langle x, e\_\alpha \rangle: \alpha \in \Lambda \}\)称为\(x\)关于基\(\mathcal E\)的Fourier系数。
非零系数一定是至多可数的。

**定理**：
设\(\mathcal E = \{ e\_\alpha: \alpha \in \Lambda \}\)为Hilbert空间\(H\)的一个标准正交集，
则以下三条命题等价：

* `(1)` \(\mathcal E\)是标准正交基；
* `(2)` \(\mathcal E\)是完备的；
* `(3)` Parseval等式成立，即
  \[
  \| x \|^2 = \sum\_{\alpha \in \Lambda}
  | \langle x, e\_\alpha \rangle |^2 .
  \]

推论：非零Hilbert空间\(H\)必有完备正交基。

### E.13.3 投影

**定理E.1 (投影存在性)** 设\(H\)为Hilbert空间，
\(S\)是\(H\)的子Hilbert空间。

(1) 对\(\forall \boldsymbol y \in H\)，
存在唯一的\(\boldsymbol x \in S\)，
使得
\[
\| \boldsymbol y - \boldsymbol x \|
= \inf\_{\boldsymbol z \in S} \| \boldsymbol y - \boldsymbol z \|
\]
称\(\boldsymbol x\)是\(\boldsymbol y\)在闭子空间\(S\)上的**投影**,
记作\(\mathop{\mathrm{Proj}}\_{S} \boldsymbol y\)。

(2) 对\(\forall \boldsymbol y \in H\)，
\(\boldsymbol x \in S\)是\(\boldsymbol y\)在\(S\)上的投影当且仅当\(\boldsymbol y - \boldsymbol x \perp S\)。

**证明**: (1)
\(d = \inf\_{\boldsymbol z \in S} \| \boldsymbol y - \boldsymbol z \|\)必存在且\(d \geq 0\)。
存\(\{ \boldsymbol z\_n \} \subset S\)使得\(\| \boldsymbol z\_n - \boldsymbol y \| \to d\)(\(n\to\infty\))。
来证明\(\{ \boldsymbol z\_n \}\)是基本列。
利用恒等式
\[
\| \boldsymbol x - \boldsymbol y \|^2 + \| \boldsymbol x + \boldsymbol y \|^2
= 2 \| \boldsymbol x \|^2 + 2 \| \boldsymbol y \|^2
\]
可得
\[\begin{aligned}
\| \boldsymbol z\_n - \boldsymbol z\_m \|^2
=& \| (\boldsymbol z\_n - \boldsymbol y) - (\boldsymbol z\_m - \boldsymbol y) \|^2 \\
=& - \| \boldsymbol z\_n + \boldsymbol z\_m - 2 \boldsymbol y \|^2
+ 2 \| \boldsymbol z\_n - \boldsymbol y \|^2 + 2 \| \boldsymbol z\_m - \boldsymbol y \|^2 \\
=& -4 \| \frac{\boldsymbol z\_n + z\_m}{2} - \boldsymbol y \|^2
+ 2 \| \boldsymbol z\_n - \boldsymbol y \|^2 + 2 \| \boldsymbol z\_m - \boldsymbol y \|^2 \\
\leq& -4d
+ 2 \| \boldsymbol z\_n - \boldsymbol y \|^2 + 2 \| \boldsymbol z\_m - \boldsymbol y \|^2 \\
\to& 0 \ (n, m \to \infty)
\end{aligned}\]

所以\(\{ \boldsymbol z\_n \}\)是基本列，
存在\(\boldsymbol x \in H\)使得\(\| \boldsymbol z\_n - \boldsymbol x \| \to 0\)。
因为\(S\)是闭子空间所以\(\boldsymbol x \in S\)。由内积的连续性可得
\[
\| \boldsymbol y - \boldsymbol x \|
= \lim\_{n\to\infty} \| \boldsymbol y - \boldsymbol z\_n \|
= d
\]

再来证明唯一性。
如果有\(\boldsymbol x' \in S\)使得\(\| \boldsymbol y - \boldsymbol x \| = d\)，
则
\[\begin{aligned}
0 \leq& \| \boldsymbol x - \boldsymbol x' \|^2 \\
=& \| (\boldsymbol x - \boldsymbol y) - (\boldsymbol x' - \boldsymbol y) \|^2 \\
=& - \| \boldsymbol x + \boldsymbol x' - 2 \boldsymbol y \|^2
+ 2 \| \boldsymbol x - \boldsymbol y \|^2 + 2 \| \boldsymbol x' - \boldsymbol y \|^2 \\
=& -4 \| \frac{\boldsymbol x + \boldsymbol x'}{2} - \boldsymbol y \|^2
+ 2 \| \boldsymbol x - \boldsymbol y \|^2 + 2 \| \boldsymbol x' - \boldsymbol y \|^2 \\
\leq& -4d + 2d + 2d = 0
\end{aligned}\]
即\(\boldsymbol x' = \boldsymbol x\)。

(2) 先证明充分性。
设\(\boldsymbol x \in S\)使得\(\boldsymbol y - \boldsymbol x \perp S\)，
则\(\forall z \in S\)，
有
\[\begin{aligned}
\| \boldsymbol y - \boldsymbol z \|^2
=& \| (\boldsymbol y - \boldsymbol x) + (\boldsymbol x - \boldsymbol z) \|^2 \\
=& \| \boldsymbol y - \boldsymbol x \|^2 + \| \boldsymbol x - \boldsymbol z \|^2
+ 2 < \boldsymbol y - \boldsymbol x, \boldsymbol x - \boldsymbol z > \\
=& \| \boldsymbol y - \boldsymbol x \|^2 + \| \boldsymbol x - \boldsymbol z \|^2 \\
\geq& \| \boldsymbol y - \boldsymbol x \|^2
\end{aligned}\]
所以\(\boldsymbol x\)是\(\boldsymbol y\)在\(S\)的投影。

再来证明必要性。用反证法。
设\(\boldsymbol x \in S\)使得
\[
\| \boldsymbol y - \boldsymbol x \| = \inf\_{\boldsymbol z \in S} \| \boldsymbol y - \boldsymbol z \|
\]
如果\(\boldsymbol y - \boldsymbol x \perp S\)不成立，
则存在\(\boldsymbol z' \in S\)使得\(a = <\boldsymbol y - \boldsymbol x, \boldsymbol z'> \neq 0\)，
显然\(\boldsymbol z' \neq 0\)。
令
\[
\boldsymbol x' = \boldsymbol x + \frac{a}{\| \boldsymbol z' \|^2} \boldsymbol z
\]
则\(\boldsymbol x' \in S\)，且
\[\begin{aligned}
\| \boldsymbol y - \boldsymbol x' \|^2
=& \| (\boldsymbol y - \boldsymbol x) + (\boldsymbol x - \boldsymbol x') \|^2 \\
=& \| (\boldsymbol y - \boldsymbol x) - \frac{a}{\| \boldsymbol z' \|^2} \boldsymbol z' \|^2 \\
=& \| \boldsymbol y - \boldsymbol x \|^2
+ \frac{a^2}{\| \boldsymbol z' \|^4} \| \boldsymbol z' \|^2
- \frac{2 a}{\| \boldsymbol z' \|^2} <\boldsymbol y - \boldsymbol x, \boldsymbol z'> \\
=& \| \boldsymbol y - \boldsymbol x \|^2
- \frac{a^2}{\| \boldsymbol z' \|^2} \\
<& \| \boldsymbol y - \boldsymbol x \|^2
\end{aligned}\]
矛盾。定理证毕。

○○○○○○

定理说明，
如果需要用闭子空间\(S\)上的元素\(\boldsymbol x\)最优地逼近\(\boldsymbol y \in H\)，
\(\boldsymbol x = \mathop{\mathrm{Proj}}\_{S} \boldsymbol y\)是这个问题的唯一的解。
这里“最优逼近”是用\(\| \boldsymbol x - \boldsymbol y \|\)作为两个元素的距离时距离最小的近似。
最优逼近\(\boldsymbol x\)的条件也可以写成
\[
<\boldsymbol y - \boldsymbol x, \boldsymbol z> = 0,
\ \forall \boldsymbol z \in S .
\]

对Hilbert空间\(H\)和闭子空间\(S\)，
\(\mathop{\mathrm{Proj}}\_{S}\)是从\(H\)的\(S\)的一个线性映射。
记\(I\)为\(H\)上的恒等映射，
则\(\forall \boldsymbol y \in H\),
\[
\| \boldsymbol y \|^2
= \| \mathop{\mathrm{Proj}}\_{S} \boldsymbol y \|^2
+ \| (I - \mathop{\mathrm{Proj}}\_{S}) \boldsymbol y \|^2
\]
其中\((I - \mathop{\mathrm{Proj}}\_{S}) \boldsymbol y = \boldsymbol y - \mathop{\mathrm{Proj}}\_{S} \boldsymbol y\)。

**正交分解**：
\(\forall y \in H\)，存在唯一的分解
\[
\boldsymbol y = \boldsymbol y\_1 + \boldsymbol y\_2
= \mathop{\mathrm{Proj}}\_{S} \boldsymbol y
+ (I - \mathop{\mathrm{Proj}}\_{S}) \boldsymbol y
\]
其中\(\boldsymbol y\_1 \in S\),
\(\boldsymbol y\_2 \in S^\perp\)。
显然\(\boldsymbol y\_1 = \mathop{\mathrm{Proj}}\_{S} \boldsymbol y\)
和\(\boldsymbol y\_2 = (I - \mathop{\mathrm{Proj}}\_{S}) \boldsymbol y\)满足分解；
如果还有\(\boldsymbol x\_1 \in S\)和\(\boldsymbol x\_2 \in S^\perp\)
满足\(\boldsymbol y = \boldsymbol x\_1 + \boldsymbol x\_2\)，
则有
\[
[\boldsymbol x\_1 - \mathop{\mathrm{Proj}}\_{S} \boldsymbol y]
+ [\boldsymbol x\_2 - (I - \mathop{\mathrm{Proj}}\_{S}) \boldsymbol y]
= 0
\]
两边与\(\boldsymbol x\_1 - \mathop{\mathrm{Proj}}\_{S} \boldsymbol y\)作内积得
\[
\| \boldsymbol x\_1 - \mathop{\mathrm{Proj}}\_{S} \boldsymbol y \|^2 + 0 = 0
\]
所以\(\boldsymbol x\_1 = \mathop{\mathrm{Proj}}\_{S} \boldsymbol y\)，
即分解式唯一。
记这样的分解为
\[
\boldsymbol y = \boldsymbol y\_1 \oplus \boldsymbol y\_2,
\ \boldsymbol y\_1 \in S, \boldsymbol y\_2 \in S^\perp
\]

○○○○○○

还可以考虑更一般的距离问题。
考虑\(\mathbb R^n\)空间中的超平面
\[
H = \{\boldsymbol x \in \mathbb R^n:
\ \boldsymbol w^T \boldsymbol x + b = 0 \},
\]
其中\(\boldsymbol w\)是法向量，\(b\)是标量，
设\(p \geq 1\)，令
\[
d\_p(\boldsymbol y, H)
= \inf\_{\boldsymbol x \in H} \| \boldsymbol y - \boldsymbol x \|\_p,
\]
称为\(\boldsymbol y\)到超平面\(H\)的\(d\_p\)距离，
则可以证明
\[
d\_p(\boldsymbol y, H)
= \frac{| \boldsymbol w^T \boldsymbol x + b |}{\| \boldsymbol w \|\_q} .
\]

### E.13.4 Riesz表示定理

**线性泛函**：
从Hilbert空间\(H\)到数域\(\mathbb K\)的映射\(f\)称为一个泛函。
如果
\[
f(\alpha x + \beta y)
= \alpha f(x) + \beta f(y),
\ \forall x, y \in H,
\ \forall \alpha, \beta \in \mathbb K,
\]
则称\(f\)是\(H\)上的线性泛函。

对\(x\_0 \in H\)，
如果\(\| x\_n - x\_0 \| \to 0\)推出\(f(x\_n) \to f(x\_0)\)，
则称泛函\(f\)在点\(x\)处连续。
如果\(f\)在所有\(x \in H\)连续，
则称\(f\)为连续泛函。

Hilbert空间\(H\)的所有连续线性泛函组成的集合记为\(H^\*\)，
这是一个线性空间。

**定理**：设\(f\)是Hilbert空间\(H\)上的一个线性泛函，
则以下几条等价：

* `(1)` \(f\)连续；
* `(2)` \(f\)在某个点连续；
* `(3)` \(f\)在\(x=0\)处连续；
* `(4)` 存在常数\(c\)使得\(\forall x \in H\), \(|f(x)| \leq c \| x \|\)。

其中第(4)条成立时称\(f\)为有界线性泛函，
这时令
\[
\| f \| = \sup \{|f(x)|: \; \|x\| \leq 1 \}
\]
为\(f\)的模。
Hilbert空间上的有界线性泛函等价于连续线性泛函，
空间都是\(H^\*\)。

若\(f\)是\(H\)上的有界线性泛函（连续线性泛函），则
\[\begin{aligned}
\|f\| =& \sup \{|f(x)|: \; \|x\| \leq 1 \} \\
=& \sup \{|f(x)|: \; \|x\| = 1 \} \\
=& \sup \{\frac{|f(x)|}{\|x\|}: \; x \neq 0 \} \\
=& \inf \{c > 0:\; |f(x)| \leq c \|x\|, \forall x \in H \} .
\end{aligned}\]

设\(x\_0 \in H\)，
令
\[
f\_{x\_0}(x) = \langle x, x\_0 \rangle,
\ \forall x \in H,
\]
则\(f\_{x\_0}\)是一个线性泛函，
由
\[
|f\_{x\_0}(x)| \leq \|x\_0\| \, \| x \|,
\]
可知其为有界线性泛函，
且\(\| f \| \leq \| x\_0 \|\)，
又\(x\_0 \neq 0\)时
\[
f\_{x\_0}(\frac{x\_0}{\|x\_0\|})
= \langle \frac{x\_0}{\|x\_0\|}, x\_0 \rangle
= \| x\_0 \|
\]
所以\(\| f\_{x\_0} \| = \| x\_0 \|\)。

**Riesz表示定理**：
设\(f\)是Hilbert空间\(H\)上的有界线性泛函，
则存在唯一\(x\_f \in H\)，
使得
\[
f(x) = \langle x, x\_f \rangle ,
\ \forall x \in H ，
\]
且\(\|f\| = \| x\_f \|\)。

这样，从\(f \in H^\*\)到\(x\_f \in H\)的映射定义了一个一一满映射，
且为等距映射，
所以\(H^\*\)与\(H\)等距同构。

## E.14 Hilbert空间上的算子

**泛函**：
设\(\mathbb K\)为数域\(\mathbb R\)或者\(\mathbb C\)，
\(X\)和\(Y\)是\(\mathbb K\)上的线性空间，
\(D\)是\(X\)的子空间，
\(A: D \to Y\)是映射，
记\(x \in D\)的映射结果为\(A x\)，
\(D\)称为\(A\)的定义域，
有时记为\(D(A)\)。
\(R(A) = \{ A x: x \in D \}\)称为\(A\)的值域。
称\(A\)为一个算子。
如果
\[
A (\alpha x + \beta y)
= \alpha A x + \beta A y,
\ \forall x, y \in X, \ \alpha, \beta \in \mathbb K,
\]
则称\(A\)为线性算子。
当\(Y = \mathbb K\)时称\(A\)是数域\(\mathbb K\)上的线性泛函，
如实泛函或复泛函。

例：\(A\)为\(n\times m\)矩阵，
\(A x\), \(x \in \mathbb R^m\)是\(\mathbb R^m\)到\(\mathbb R^n\)的线性算子。

例：设\(\Omega\)是\(\mathbb R^n\)的开区域，
\(X = Y = C^\infty(\bar \Omega)\)，
\[
P(\partial) = \sum\_{|\alpha| \leq m}
a\_{\alpha}(x) \partial^{\alpha},
\]
其中\(a\_{\alpha}(x) \in C^\infty(\bar \Omega)\)，
定义
\[
A:\ u \in X \mapsto P(\partial) u,
\]
即
\[
(P(\partial) u)(x)
= \sum\_{|\alpha| \leq m}
a\_{\alpha}(x) \partial^{\alpha} u(x),
\]
则\(A\)是\(X\)到\(Y\)的一个线性算子。

如果\(X = Y = L^2(\Omega)\)，
\(D = C^m(\bar \Omega)\)，
则上面的微分多项式算子仍是\(X\)到\(Y\)的线性算子，
但定义域不是全空间\(X\)。

### E.14.1 连续和有界线性算子

设\(H, K\)是两个Hilbert空间，
\(D\)是\(H\)的线性子空间，
\(A\)是\(D \to K\)的线性算子，
按\(H\)中内积导出的距离，
如果\(A x\)在\(x = x\_0\)处连续，
称算子\(A\)则\(x = x\_0\)处连续，
如果\(A x\)在所有\(x \in D\)连续，
称\(A x\)是连续线性算子。

对于线性算子，
\(A x\)连续当且仅当\(Ax\)在\(x = 0\)连续。

**有界线性算子**：
设\(H, K\)是两个Hilbert空间，
称线性算子\(H \to K\)是有界线性算子，
如果存在常数\(c \geq 0\)使得
\[
\| Ax \|\_K
\leq c \| x \|\_H,
\ \forall x \in H.
\]
记\(\mathcal B(H,K)\)为这些有界线性算子的全体，
这是一个线性空间。

**定理**：
若\(A, B \in \mathcal B(H,K)\), \(\alpha \in \mathbb K\)，

* `(1)` \(A + B \in \mathcal B(H,K)\),
  且\(\| A + B \| \leq \|A\| + \|B\|\)；
* `(2)` \(\alpha A \in \mathcal B(H,K)\),
  且\(\| \alpha A \| = |\alpha| \, \|A\|\)；
* `(3)` 若还有\(C \in \mathcal(K,J)\)，
  则\(C A \in \mathcal B(H,J)\)，且\(\| CA \| \leq \|C\| \, \|A\|\)。

这说明\(\mathcal B(H,K)\)是定义了加法\(A + B\)和数乘\(\alpha A\)的线性空间，
且定义了复合运算\(CA\)。

如果\(A\)是\(H\)到\(K\)的线性算子，
则\(A\)连续当且仅当\(A\)有界。

### E.14.2 同构

设\(H\)和\(K\)为两个Hilbert空间，
如果存在线性算子\(U: H \to K\)使得
\[
\langle Ux, Uy \rangle\_K
= \langle x, y \rangle\_H,
\ \forall x, y \in H,
\]
则称\(U\)是\(H\)到\(K\)的保内积算子；
如果\(U\)还是满映射，
则称\(U\)是\(H\)到\(K\)上的同构算子，
此时\(H\)和\(K\)称为是**同构**的。

注意，
保内积算子一定是单映射：
对\(x, y \in H\)且\(x \neq y\)，
必有\(U x \neq U y\)，
否则
\[
\langle x-y, x-y \rangle\_K
\langle U(x-y), U(x-y) \rangle\_K
= \langle Ux - Uy, Ux - Uy \rangle\_K = 0
\]
推出\(x-y=0\)，矛盾。
所以同构映射是\(H\)到\(K\)的一一的满线性映射且保内积。

由内积定义模和距离，
设\(d\_H\)和\(d\_K\)分别为\(H\)和\(K\)中内积导出的距离，
对线性算子\(V: H \to K\)，
如果
\[
d\_K(Vx, Vy)
= d\_H(x, y),
\ \forall x, y \in H,
\]
则称\(V\)是\(H\)到\(K\)的保距算子。

注意
\[\begin{aligned}
d\_K(Vx, Vy)
=& \| Vx - V y \|\_K, \\
d\_H(x,y)
=& \| x - y \|\_H,
\end{aligned}\]
可以证明线性算子\(V\)为保距算子当且仅当
\[
\| V x \|\_K
= \| x \|, \ \forall x \in H .
\]
充分性显然，
对必要性，
只要取\(y=0\)即可。

保内积条件与保距条件也是等价的：
\[
\langle Ux, Uy \rangle\_K
= \langle x, y \rangle\_H,
\ \forall x, y \in H
\Longleftrightarrow
\| Ux \|\_K
= \| x \|\_H, \ \forall x \in H .
\]

保内积显然推出保距；
反之，如果保距条件成立，
则
\[\begin{aligned}
& \| Ux + Uy \|^2
= \| Ux \|^2 + \| Uy \|^2 + 2 \text{Re}(\langle Ux, Uy \rangle\_K) \\
=& \| x + y \|^2
= \|x\|^2 + \|y\|^2 + 2 \text{Re}(\langle x, y \rangle\_H),
\end{aligned}\]
因此
\[
\text{Re}(\langle Ux, Uy \rangle\_K)
= \text{Re}(\langle x, y \rangle\_H),
\ \forall x, y \in H.
\]
类似地，
\[\begin{aligned}
& \| iUx + Uy \|^2
= \| Ux \|^2 + \| Uy \|^2 + 2 \text{Re}(\langle iUx, Uy \rangle\_K) \\
=& \| ix + y \|^2
= \|x\|^2 + \|y\|^2 + 2 \text{Re}(\langle ix, y \rangle\_H),
\end{aligned}\]
但对\(z \in \mathbb C\)，
\(\text{Re}(iz) = - \text{Im}(z)\)，
所以
\[
\text{Im}(\langle Ux, Uy \rangle\_K)
= \text{Im}(\langle x, y \rangle\_H),
\ \forall x, y \in H,
\]
所以保距条件成立时保内积条件成立。

### E.14.3 可分Hilbert空间

**定理**
Hilbert空间\(H\)为可分的（有可数的稠密子集），
当且仅当存在至多可数的标准正交基\(\mathcal E\)。
若标准正交基个数为有限值\(N\)，
则\(H\)与\(\mathbb K^N\)同构，
否则与\(l^2\)同构。
映射为\(x \in \mapsto \{ \langle x, e\_i \rangle, i=1,2,\dots \}\),
其中\(\{ e\_i \}\)是\(H\)的标准正交基。

### E.14.4 共轭算子

若\(H\)和\(K\)是Hilbert空间，
\(u(x,z): H \times K \to \mathbb K\)称为共轭双线性形式，
如果\(u(x,z)\)关于\(x\)为线性函数，
关于\(z\)为共轭线性函数：
\[
u(x, \alpha z + \beta w)
= \bar\alpha u(x,z) + \bar\beta u(x,w),
\ \forall \alpha, \beta \in \mathbb K,
\ x \in H, z,w \in K .
\]

如果存在\(c \geq 0\)使得
\[
|u(x,z)| \leq c \, \|x\| \, \|z\|,
\forall x \in H, z \in K,
\]
则称\(u\)有界，称\(c\)是\(u\)的一个上界。

共轭双线性形式用来研究有界线性算子。
显然，
如果\(A \in \mathcal B(H,K)\)，
则\(u(x,z) = \langle A x, z \rangle\)是共轭双线性形式；
如果\(B \in \mathcal B(K, H)\)，
则\(v(x,z) = \langle x, B z \rangle\)是共轭双线性形式。

**定理E.2** 设\(u\)是\(H\times K\)的有界共轭双线性形式，
有上界\(c\)，
则存在唯一的\(A \in \mathcal B(H,K)\)和\(B \in \mathcal B(K, H)\)，
使得
\[
u(x,z) = \langle A x, z \rangle = \langle x, B z \rangle ,
\ \forall x \in H, z \in K,
\]
且\(\|A\| \leq c\), \(\|B\| \leq c\)。

上述定理基于Riesz表示定理。

**共轭算子**：
若\(A \in \mathcal B(H,K)\)，
则必存在唯一的\(B \in \mathcal B(K, H)\)使得
\[
\langle A x, z \rangle
= \langle x, B z \rangle ,
\ \forall x \in H, z \in K,
\]
称\(B\)为\(A\)的共轭算子，
记为\(A^\*\)。

**例（矩阵的共轭转置）**
设\(A\)为\(m \times n\)实矩阵，
定义\(A: x \in \mathbb R^n \mapsto A x \in \mathbb R^m\)，
\(Ax\)中的\(x\)看成是\(m\)维列向量，
则\(A\)是有界线性算子，有
\[\begin{aligned}
\langle A x, z \rangle
= z^T (A x)
= (A^T z)^T x
= \langle x, A^T z \rangle,
\ \forall x \in \mathbb R^n, z \in \mathbb R^m,
\end{aligned}\]
即算子\(A\)的共轭算子是由矩阵\(A^T\)生成的线性算子。

若\(A\)为\(m \times n\)复矩阵，
定义\(A: x \in \mathbb C^n \mapsto A x \in \mathbb C^m\)，
\(\mathbb C^n\)中内积定义为
\[
\langle x, y \rangle
= y^\* x,
\forall x, y \in \mathbb C^n,
\]
其中\(y\)看成列向量，
\(y^\*\)表示列向量\(y\)的共轭转置，
则\(A\)是有界线性算子，有
\[\begin{aligned}
\langle A x, z \rangle
= z^\* A x
= (A^\* z)^\* x
= \langle x, A^\* z \rangle,
\ \forall x \in \mathbb C^n, z \in \mathbb C^m,
\end{aligned}\]
即有界线性算子\(A\)的共轭算子是它的共轭转置矩阵对应的线性算子。

当\(m=n\)时，\(\mathbb C^n\)上的一个线性算子可以表示成\(n\times n\)矩阵\(A\)，
其共轭算子为\(A\)的共轭转置。
\(\mathcal B(H)\)上的共轭算子就是矩阵的共轭转置的推广。

**同构算子充要条件**：
设\(U \in \mathcal B(H,K)\)，
则\(U\)是\(H\)到\(K\)的同构，
当且仅当\(U\)可逆且\(U = U^\*\)。

通常考虑\(\mathcal B(H)\)中有界线性算子的共轭算子。
从\(A \in \mathcal B(H)\)到\(A^\*\)构成\(\mathcal B(H)\)到\(\mathcal B(H)\)的一个映射，
称为对合映射。

**定理**：
若\(A, B \in \mathcal B(H)\)，
\(\alpha, \beta \in \mathbb K\)，
则

* `(1)` \((\alpha A + \beta B)^\* = \bar\alpha A^\* + \bar\beta B^\*\);
* `(2)` \((AB)^\* = B^\* A^\*\)；
* `(3)` \((A^\*)^\* = A\)。

**定理**：若\(A \in \mathcal B(H)\)，
则
\[
\|A\| = \|A^\*\| = \| A^\* A \|^{1/2} .
\]

**例（积分算子）**
设\(E\)为\(\mathbb R^n\)中勒贝格可测集，
\(H = L^2(E)\)是\(E\)上所有平方可积函数组成的Hilbert空间，
\(k(x,y)\)是\(E \times E\)上平方可积函数，
称为积分核。
定义线性算子\(K\)如下
\[
K f(x)
= \int\_E k(x,y) f(y) \,dy,
\ \forall f \in L^2(E),
\]
称\(K\)为积分算子。
考虑\(L^2(E) \times L^2(E)\)的共轭双线性形式
\[
u(f,g) = \iint\_{E \times E} k(x,y) f(y) \overline{g(x)} \,dx \,dy,
\]
由Cauchy-Schwarz不等式知此共轭双线性形式有上界\(\|k\|\_2\)，
由定理[E.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/rkfcfcana.html#thm:base-hilbert-op-conj-bfb)可知\(K\)是有界线性算子，
\(\| K \| \leq \|k\|\_2\)。
此时\(K^\*\)也是积分算子，
对应的积分核为\(k^\*(x,y) = \overline{k(y,x)}\)。

对\(A \in \mathcal B(H)\)，
定义
\[
\text{ker}(A)
= \{ x \in H: A x = 0 \},
\]
这是\(H\)的子Hilbert空间，
称为\(A\)的核空间，
类似于方阵的0特征值对应的特征向量张成的线性子空间。

**定理** 对\(A \in \mathcal B(H)\)，
有
\[
\text{ker}(A) = R(A^\*),
\]
其中\(R(A^\*)\)表示\(A^\*\)的值域\(\{A^\* y: y \in H \}\)。

**自共轭算子、酉算子**：
对\(A \in \mathcal B(H)\)，
若\(A = A^\*\)，
称\(A\)是自共轭算子（或自伴算子、Hermite算子）。
若\(A A^\* = A^\* A\)，
则称\(A\)是正规算子。
如果\(A A^\* = A^\* A = I\)，
则称\(A\)是酉算子。

酉算子必为正规算子。
自共轭算子必为正规算子。

自共轭算子是实对称方阵和Hermite复矩阵的推广。
酉算子是实正交阵和复酉矩阵的推广。

设\(U\)是酉算子，即\(U U^\* = U^\* U = I\)，
则\(U\)可逆，
是一一的满映射，
且
\[
\langle x, y \rangle
= \langle U\* U x, y \rangle
= \langle U x, U y \rangle,
\]
所以酉算子还是保距（保内积）算子，
所以酉算子是\(H\)到自身的同构算子。

当\(H\)是复Hilbert空间时，
\(\forall A \in \mathcal B(H)\)，
\(B = (A + A^\*)/2\)，
\(C = (A - A^\*)/(2i)\)都是自共轭算子，
且\(A = B + i C\)，
\(B\)和\(C\)分别称为\(A\)的实部与虚部（注意\(B\)和\(C\)一般仍为复矩阵）。

**定理** 设\(H\)是复Hilbert空间，
\(A \in \mathcal B(H)\)，则

* `(1)` \(A\)是自共轭算子\(\Longleftrightarrow \langle Ax, x \rangle \in \mathbb R\),
  \(\forall x \in H\)；
* `(2)` \(A\)是正规算子\(\Longleftrightarrow \|Ax\| = \| A^\* x\|\),
  \(\forall x \in H\Longleftrightarrow A\)的实部与虚部可交换。

其中\(A\)的实部与虚部可交换，是指\(BC = CB\)。

**定理** 若\(A\)是自共轭算子，则
\[
\|A\|
= \sup\{ |\langle Ax, x \rangle| : \|x\|=1 \}
= \max(|m|, |M|),
\]
其中
\[
M = \sup\{ \langle Ax, x \rangle : \|x\|=1 \},
\ m = \inf\{ \langle Ax, x \rangle : \|x\|=1 \} .
\]

**推论**：
设\(A = A^\*\)，
\(\langle Ax, x \rangle = 0\)对\(\forall x \in H\)成立，
则\(A = 0\)，即\(A x = 0\), \(\forall x \in H\)。

### E.14.5 投影算子性质

**幂等算子**：对\(P \in \mathcal B(H)\)，
如果\(P^2 = P\)，
则称\(P\)为幂等算子。

设\(P\)为幂等算子，
则\((I-P)^2 = I - 2P + P^2 = I-P\)也是幂等算子。易证
\[
R(P) = \text{ker}(I-P),
\ \text{ker}(P) = R(I-P),
\]
投影算子的值域与核空间都是\(H\)的闭子空间。

**投影算子**：
对\(P \in \mathcal B(H)\)，
如果\(P^2 = P\)且\(P^\*=P\)（幂等而且自共轭的有界线性算子），
则称\(P\)为投影算子。

设\(H\)为Hilbert空间，
\(S\)是\(H\)的子Hilbert空间，
则\(P = \mathop{\mathrm{Proj}}\_{S}: H \to S\)是线性算子，
定义域为\(H\)，
值域为\(S\)，
是有界算子：
\[
\| P x \| \leq \| x \|,
\]
所以也是连续算子。
当\(S\)非零时（\(S\)不是仅由零元素组成），
\(\|P\| = 1\)。

因为对\(x \in H\)，
\(Px = \mathop{\mathrm{Proj}}\_{S} x \in S\)，
这时\(P(Px)=Px\)，
即\(P^2 = P\)，
\(P\)是幂等算子。

来证明\(P\)自共轭，只要证明
\[
\langle Px, y \rangle
= \langle x, Py \rangle ,
\forall x, y \in H.
\]
对\(x, y\)作正交分解
\[
x = x\_1 \oplus x\_2,
\ y = y\_1 \oplus y\_2,
\]
其中\(x\_1, y\_1 \in S\), \(x\_2, y\_2 \in S^\perp\)，
则
\[
\langle Px, y \rangle
= \langle x\_1, y\_1 + y\_2 \rangle
= \langle x\_1, y\_1 \rangle
= \langle x\_1 + x\_2, y\_1 \rangle
= \langle x, Py \rangle .
\]
所以，\(\mathop{\mathrm{Proj}}\_{S}\)是投影算子。

反过来，
设\(P\)是\(H\)的投影算子，
令\(S = R(P)\)，
则\(S\)是\(H\)的闭子空间，
对\(x = x\_1 \oplus x\_2 \in H\)，
其中\(x\_1 \in S\)，
\(x\_2 \in S^\perp\)，
只要证明\(P x = x\_1\)，
只要证明\(P x\_1 = x\_1\)，\(P x\_2 = 0\)。
事实上\(x\_1 \in S = R(P)\)故存在\(y\_1 \in H\)使得\(x\_1 = P y\_1\)，从而
\[\begin{aligned}
P x\_1 = P(P y\_1) = P^2 y\_1 = P y\_1 = x\_1 .
\end{aligned}\]
另外，
\[\begin{aligned}
\langle Px\_2, Px\_2 \rangle
= \langle x\_2, P(Px\_2) \rangle
= \langle x\_2, P^2 x\_2 \rangle
= \langle x\_2, P x\_2 \rangle = 0
\end{aligned}\]
（因为\(x\_2 \in S^\perp\)而\(P x\_2 \in S\)。）

\*\*定理（投影算子充要条件）
设\(P\)是Hilbert空间\(H\)的幂等算子（同时要求有界线性算子），
则以下条件等价：

* `(1)` \(P = P^\*\)（即投影算子）；
* `(2)` \(\text{ker}(P) = R(P)^\perp\);
* `(3)` \(P\)是c；
* `(4)` \(\|P\| = 1\);
* `(5)` 对所有\(H\)中的元素\(x\)有\(\langle Px, x \rangle \geq 0\)。

当上述任何一个条件成立时，
\(P\)都是投影算子，
而且是\(H\)到\(R(P)\)的正交投影。

\(\boldsymbol y \in S\)当且仅当\(\mathop{\mathrm{Proj}}\_{S} \boldsymbol y = \boldsymbol y\)。

\(\boldsymbol y \in S^\perp\)当且仅当\(\mathop{\mathrm{Proj}}\_{S} \boldsymbol y = \boldsymbol 0\)。

设\(S\)为闭子空间\(M\)的闭子空间，
则当\(\boldsymbol y \in M\)时
\(\mathop{\mathrm{Proj}}\_{S} \boldsymbol y \in S \subset M\)即
\(\mathop{\mathrm{Proj}}\_{S} \boldsymbol y \in M\)，
同时\((I - \mathop{\mathrm{Proj}}\_{S}) \boldsymbol y = \boldsymbol y - \mathop{\mathrm{Proj}}\_{S} \boldsymbol y \in M\)。
对一般的\(\boldsymbol y \in H\)，有
\[
\mathop{\mathrm{Proj}}\_{S} \boldsymbol y
= \mathop{\mathrm{Proj}}\_{S} \mathop{\mathrm{Proj}}\_{M} \boldsymbol y
\]

### References

郭懋正. 2005. *实变函数与泛函分析*. 北京大学出版社.