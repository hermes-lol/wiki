---
crawl_time: '2026-01-17 14:28:56'
framework: sphinx
title: 3 正态时间序列和随机变量的收敛性 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 3 正态时间序列和随机变量的收敛性

## 3.1 随机向量的数学期望和方差

矩阵随机变量\({\boldsymbol M} = (M\_{i,j})\_{m\times n}\)：
即矩阵每个元素都是一个随机变量。

矩阵随机变量的期望为每个元素取期望：
\[\begin{aligned}
E ({\boldsymbol M}) = (E M\_{i,j})\_{m\times n} = (\mu\_{ij})\_{m\times n}.
\end{aligned}\]

若\(A, B, C\)是常值矩阵，
\(C + A {\boldsymbol M} B\)有意义，则
\[\begin{aligned}
E (C + A {\boldsymbol M} B) = C + A \cdot E ({\boldsymbol M}) \cdot B
\end{aligned}\]

随机向量\({\boldsymbol X} = (X\_1, X\_2, \ldots, X\_n)^T\). 则协方差阵为
\[
\Sigma\_X = (\sigma\_{ij})\_{n\times n}
=\text{Var}({\boldsymbol X})
= E[({\boldsymbol X} - \mu) ({\boldsymbol X} - \mu)^T]
\]
其中\(\sigma\_{ii} = \text{Var}(X\_i)\)，
\(\sigma\_{ij} = \text{Cov}(X\_i, X\_j)\)。

\(\Sigma\_X\)对称非负定(半正定)。

\[
\Sigma\_X = E({\boldsymbol X} {\boldsymbol X}^T)
- E({\boldsymbol X}) E({\boldsymbol X})^T .
\]

若
\[\begin{align}
{\boldsymbol Y} = {\boldsymbol a} + B {\boldsymbol X} ,
\tag{3.1}
\end{align}\]
则有
\[\begin{align}
E {\boldsymbol Y} = {\boldsymbol a} + B E{\boldsymbol X}, \qquad
\text{Var}({\boldsymbol Y}) = B \Sigma\_X B^T.
\tag{3.2}
\end{align}\]

由[(3.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-lintran01ev),
对\(Y = {\boldsymbol \alpha}^T {\boldsymbol X}\), 有
\[\begin{aligned}
0 \leq \text{Var}(Y) = \text{Var}({\boldsymbol \alpha}^T {\boldsymbol X})
= {\boldsymbol \alpha}^T \text{Var}({\boldsymbol X}) {\boldsymbol \alpha} .
\end{aligned}\]
由此可以证明随机向量协方差阵非负定（半正定）。

设随机向量
\(\boldsymbol X=(X\_1, X\_2, \dots, X\_n)^T\)，
\(\boldsymbol Y=(Y\_1, Y\_2, \dots, Y\_m)^T\)，
则两个随机向量的协方差阵为
\[
\text{Cov}(\boldsymbol X, \boldsymbol Y)
= E \big[ (\boldsymbol X - E \boldsymbol X)
(\boldsymbol Y - E \boldsymbol Y)^T \big] .
\]
这是一个\(n \times m\)矩阵，
其\((i,j)\)元素为\(\text{Cov}(X\_i, Y\_j)\)。
有恒等式
\[
\text{Cov}(\boldsymbol X, \boldsymbol Y)
= E \big( \boldsymbol X \boldsymbol Y^T \big)
- (E \boldsymbol X) (E \boldsymbol Y)^T .
\]

设\(\boldsymbol \mu\), \(\boldsymbol \nu\)为非随机的向量，
\(A\), \(B\)为非随机的矩阵，
则
\[
\text{Cov}(\boldsymbol\mu + A \boldsymbol X,
\boldsymbol\nu + B \boldsymbol Y)
= A \text{Cov}(\boldsymbol X, \boldsymbol Y) B^T .
\]

## 3.2 多元正态分布

**定义3.1 (多元正态分布)** 称随机向量 \({\boldsymbol Y} =(Y\_1,Y\_2,\cdots,Y\_m)^T\)
服从\(m\)元(或多元，或多维，或\(m\)维)正态分布,
如果存在\(m\)维常数列向量\({\boldsymbol\mu}\),
\(m \times n\)常数矩阵\(B\)和iid的标准正态随机变量\(X\_1,X\_2,\ldots,X\_n\)使得
\[
{\boldsymbol Y}={\boldsymbol \mu} + B {\boldsymbol X} .
\]

这时
\[\begin{aligned}
E{\boldsymbol Y} =& {\boldsymbol \mu}, \\
\Sigma =& \text{Var}({\boldsymbol Y}) = B B^T .
\end{aligned}\]

每个\(X\_j\)的特征函数为
\[\begin{aligned}
E\left( e^{it X\_j} \right) = e^{-t^2/2}
\end{aligned}\]
随机向量\(\boldsymbol X = (X\_1, X\_2, \dots, X\_n)^T\)的特征函数为
\[\begin{aligned}
\phi\_{\boldsymbol X}(\boldsymbol t)
=& E e^{i \boldsymbol t^T \boldsymbol X}
= E \prod\_{j=1}^n e^{i t\_j X\_j} \\
=& \prod\_{j=1}^n E e^{i t\_j X\_j}
= \prod\_{j=1}^n e^{-t\_j^2/2}
= e^{-\boldsymbol t^T \boldsymbol t / 2}
\end{aligned}\]
其中 \(\boldsymbol t = (t\_1, t\_2, \dots, t\_n)^T\)。

于是, \({\boldsymbol Y}\)的特征函数为
\[\begin{aligned}
\phi\_{\boldsymbol Y}(\boldsymbol t) =& E e^{i \boldsymbol t^T \boldsymbol Y} \\
=& E e^{i(\boldsymbol t^T \boldsymbol \mu + \boldsymbol t^T B \boldsymbol X)} \\
=& e^{i \boldsymbol t^T \boldsymbol \mu} E e^{i (\boldsymbol t^T B) \boldsymbol X} \\
=& e^{i \boldsymbol t^T \boldsymbol \mu} e^{-(\boldsymbol t^T B) (\boldsymbol t^T B)^T / 2} \\
& (\text{注意} E e^{i \boldsymbol s^T \boldsymbol X} = e^{-\boldsymbol s^T \boldsymbol s / 2},
\text{令} \boldsymbol s^T = (\boldsymbol t^T B))
\\
=& \exp\left[ i {\boldsymbol t}^T {\boldsymbol \mu}
- \frac{1}{2} {\boldsymbol t}^T B B^T {\boldsymbol t} \right] \\
=& \exp\left[ i {\boldsymbol t}^T {\boldsymbol \mu}
- \frac{1}{2} {\boldsymbol t}^T \Sigma {\boldsymbol t} \right].
\end{aligned}\]
这是多维正态分布的等价定义。

**定义3.2 (多元正态分布等价定义)** 称随机向量 \({\boldsymbol Y} =(Y\_1,Y\_2,\cdots,Y\_m)^T\)服从\(m\)元正态分布,
若其特征函数为
\[\begin{align}
\phi\_{\boldsymbol Y}(\boldsymbol t) =& E e^{i \boldsymbol t^T \boldsymbol Y}
= \exp\left[ i {\boldsymbol t}^T {\boldsymbol \mu}
- \frac{1}{2} {\boldsymbol t}^T \Sigma {\boldsymbol t} \right].
\tag{3.3}
\end{align}\]
其中\(\boldsymbol \mu\)为\(m\)为常数列向量，
\(\Sigma\)为\(m\times m\)非负定阵。

多维正态分布记为\(\boldsymbol Y \sim\)N(\({\boldsymbol \mu}, \Sigma\))，
其中\(\boldsymbol \mu = E(\boldsymbol Y)\)，
\(\Sigma = \text{Var}(\boldsymbol Y)\)。
\(\boldsymbol Y\)的分布完全由\(\boldsymbol\mu, \Sigma\)决定，
与定义[3.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#def:norlim-mnormdist-0)中矩阵\(B\)的选择无关。

当\(\Sigma>0\)(正定)时，\(\boldsymbol Y\)有密度
\[
p(\boldsymbol y) = (2\pi)^{-\frac{n}{2}} |\Sigma|^{-\frac{1}{2}}
\exp\left\{ -\frac12 (\boldsymbol y - \boldsymbol\mu)^T \Sigma^{-1}
(\boldsymbol y - \boldsymbol\mu) \right\}
\]

若\(|\Sigma|=0\)，
则\(Y\)的分量由两部分\(\boldsymbol Y\_1\)和\(\boldsymbol Y\_2\)组成，
\(\text{Var} (\boldsymbol Y\_1)>0\)，
\(\boldsymbol Y\_2\)为\(\boldsymbol Y\_1\)的线性组合。(可递推证明)

**定理3.1** \({\boldsymbol Y} = (Y\_1, Y\_2,\dots, Y\_n)^T \sim \text{N}({\boldsymbol\mu}, \Sigma)\)
的充分必要条件是:
对任何\({\boldsymbol a}=(a\_1,a\_2,\cdots,a\_n)^T \in {\mathbb R}^n\)，
\[\begin{align}
W = {\boldsymbol a}^T \boldsymbol Y \ \sim \text{N}
({\boldsymbol a}^T {\boldsymbol\mu}, {\boldsymbol a}^T \Sigma {\boldsymbol a}).
\tag{3.4}
\end{align}\]

定理[(3.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-mnormcond1)说明多维正态分布的任意线性组合是一元正态分布。
但是，这里的一元正态分布是推广的\(\text{N}(\mu, \sigma^2)\)，
允许\(\sigma^2=0\)。

**证明**:

**必要性**：
由[(3.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-mnorm-charfun01)得\(W\)的特征函数为
\[\begin{align}
\phi(t) =& E \exp(itW) \nonumber \\
=& E \exp[it {\boldsymbol a}^T {\boldsymbol Y}] \nonumber \\
=& E \exp[i(t {\boldsymbol a}^T) {\boldsymbol Y}] \nonumber \\
=& \exp\left[ it {\boldsymbol a}^T {\boldsymbol\mu}
- \frac12 t^2 {\boldsymbol a}^T \Sigma {\boldsymbol a} \right]
\tag{3.5}
\end{align}\]
这是一元正态分布的特征函数，所以
\(W \sim \text{N}({\boldsymbol a}^T {\boldsymbol\mu}, {\boldsymbol a}^T \Sigma {\boldsymbol a})\)。

**充分性**：

若[(3.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-mnormcond1)成立，
则[(3.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-mnorm-prcond01)成立，
取\(t=1\)，
对任意\(\boldsymbol a\)有
\[\begin{aligned}
E \exp(i {\boldsymbol a}^T {\boldsymbol Y}) =&
\exp\left( i {\boldsymbol a}^T {\boldsymbol\mu}
- \frac12 {\boldsymbol a}^T \Sigma {\boldsymbol a} \right).
\end{aligned}\]
即\(\boldsymbol Y\)的特征函数为[(3.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-mnorm-charfun01),
于是\(\boldsymbol Y\)服从多维正态分布。

○○○○○○

## 3.3 正态平稳序列

**定义3.3** 对于时间序列 \(\{X\_t\}\), 如果对任何 \(n \geq 1\)
和 \(t\_1,t\_2,\cdots\), \(t\_n \in \mathbb Z\), 有
\((X(t\_1), X(t\_2),\ldots,X(t\_n))\)服从多元正态分布,
则称\(\{X\_t\}\)是**正态时间序列**.
特别当\(\{X\_t\}\)还是平稳序列时, 又称为**正态平稳列**.

\(\{X\_t: t \in \mathbb N\_+ \}\) 是正态时间序列
\(\Longleftrightarrow\)
对任何正整数\(m\), \((X\_1, X\_2, \ldots, X\_m)\)服从\(m\)维正态分布。

\(\{X\_t: t \in {\mathbb Z} \}\) 是正态时间序列
\(\Longleftrightarrow\)
对任何正整数\(m\), \((X\_{-m}, X\_{-m+1}, \ldots, X\_m)\)服从\(2m+1\)维正态分布.

正态分布对线性运算的封闭性为其理论研究提供了便利。
另外，正态分布和线性模型之间有一种内在的联系。

## 3.4 概率极限

设\(\xi\_n \sim F\_n(x)\), \(\xi \sim F(x)\)。
如果在\(F\)的每个连续点\(x\) 有 \(F\_n(x) \to F(x)\),
则称\(\xi\_n\)**依分布收敛**到\(\xi\),
记做\(\xi\_n \stackrel{d}{\to} \xi\)。

如果对任取\(\epsilon>0\)
有 \(P(|\xi\_n-\xi|\geq \epsilon) \to 0\),
则称\(\xi\_n\)**依概率收敛**到\(\xi\),
或称\(\xi\_n\)相合于\(\xi\),
或\(\xi\_n\)弱收敛到\(\xi\),
记做\(\xi\_n \stackrel{\text{P}}{\to} \xi\)。

如果 \(E|\xi\_n -\xi| \to 0\), 则称\(\xi\_n\) \(L^1\)收敛到 \(\xi\)
(很少用)。

如果 \(E|\xi\_n -\xi|^2 \to 0\), 则称\(\xi\_n\) **\(L^2\)收敛**到 \(\xi\),
或称\(\xi\_n\) **均方收敛**到 \(\xi\),
记做\(\xi\_n \to \xi\ (L^2)\)。

对\(p>0\)，
如果\(E|\xi\_n|^p\)和\(E|\xi|^p\)都有限，
且\(E|\xi\_n - \xi|^p \to 0\)，
则称称\(\xi\_n\) **\(L^p\)收敛**到 \(\xi\)。
因为\(0 < p < q\)时
\(E |X|^p \leq 1 + E |X|^q\)，
所以\(L^q\)收敛推出\(L^p\)收敛。

如果
\[\begin{aligned}
P(\lim\_{n\to\infty} \xi\_n = \xi) = 1
\end{aligned}\]
则称\(\xi\_n\) **a.s.收敛**到\(\xi\)。

**定理3.2** \(L^2\)收敛 \(\Rightarrow\) \(L^1\)收敛
\(\Rightarrow\) 依概率收敛 \(\Rightarrow\) 依分布收敛。

证明略。

**定理3.3** a.s.收敛 \(\Rightarrow\) 依概率收敛 \(\Rightarrow\) 依分布收敛。

证明略。

**定理3.4** 若\(\xi\_n\)依概率收敛到\(\xi\)，
则存在子序列\(\{ n\_k \}\)使得\(\xi\_{n\_k}\) a.s. 收敛到\(\xi\)。

证明略。

**定理3.5** 依概率极限如果存在，
就a.s.唯一。

**证明**

设随机变量序列\(\{ \xi\_n \}\)依概率收敛到\(\xi\)和\(\eta\)。
则\(\forall \epsilon > 0\)，
\[
\begin{aligned}
\lim\_{n\to\infty} P(|\xi\_n - \xi| > \epsilon) =& 0, \\
\ \lim\_{n\to\infty} P(|\xi\_n - \eta| > \epsilon) =& 0 .
\end{aligned}
\]
于是
\[
\begin{aligned}
& P(|\xi - \eta| > 2\epsilon) \\
=& P(|(\xi\_n - \eta) - (\xi\_n - \xi)| > 2\epsilon) \\
\leq& P(|\xi\_n - \eta| + |\xi\_n - \xi| > 2\epsilon) \\
\leq& P(|\xi\_n - \eta| > \epsilon \text{ 或 } |\xi\_n - \xi| > \epsilon) \\
\leq& P(|\xi\_n - \eta| > \epsilon) + P(|\xi\_n - \xi| > \epsilon) \\
\to& 0 \ (n \to \infty) .
\end{aligned}
\]
于是
\[
\begin{aligned}
P(\xi \neq \eta) =& P(|\xi - \eta| > 0) \\
=& P\left( \bigcup\_{n=1}^\infty \left\{ |\xi - \eta| > \frac{1}{n} \right\} \right) \\
\leq& \sum\_{n=1}^\infty P\left( |\xi - \eta| > \frac{1}{n} \right) \\
=& 0 .
\end{aligned}
\]
即\(\xi = \eta\), a.s.，证毕。

**推论3.1** 以概率1收敛极限、\(L^2\)极限、\(L^1\)极限、依概率极限如果存在，
则a.s.唯一；
如果这些极限中的几个同时存在，
则极限也a.s.相等。

**定理3.6** \(\xi\_n\)依分布收敛到\(\xi\)，
当且仅当对任意\(\mathbb R\)上的一元有界实值连续函数\(f(\cdot)\)都有
\[
E f(\xi\_n) \to E f(\xi), \ n \to \infty .
\]

由此，也称依分布收敛为**弱收敛**。
证明略。

**定理3.7** \(\xi\_n\)依分布收敛到\(\xi\)，
当且仅当对任意\(\forall t \in \mathbb R\)
\[
E e^{it\xi\_n} \to E e^{it \xi}, \ n \to\infty .
\]

即依分布收敛等价于特征函数收敛。
证明略。

**定理3.8** \(\xi\_n\)依分布收敛到常数\(c\)，
当且仅当\(\xi\_n\)依概率收敛到常数\(c\)。

证明略。

**定理3.9** 随机向量\(\boldsymbol{\xi}\_n\) a.s. (或者\(L^p\)、依概率)
收敛到随机向量\(\boldsymbol{\xi}\)，
当且仅当对应的分量a.s.（或者\(L^p\)、依概率)收敛关系成立。

证明略。

**定理3.10** 如果正态序列
\(\xi\_n \sim \text{N}(\mu\_n, \sigma\_n^2), n \in \mathbb N\)
依分布收敛到随机变量 \(\xi\), 则极限
\[
\lim \mu\_n = \mu, \ \lim \sigma\_n^2 = \sigma^2
\]
存在，且
\(\xi \sim \text{N}(\mu, \sigma^2)\).

证明参见王梓坤《随机过程论》P.18。

**定理3.11** \(\{\varepsilon\_t\}\)是正态\(\text{WN}(0,\sigma^2)\)序列，
实数列\(\{a\_j\}\)绝对可和，则线性序列
\[ X\_t = \sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j} \]
是零均值正态平稳列，自协方差函数为
\[\begin{align}
\gamma\_k = \sigma^2 \sum\_{j=-\infty}^\infty a\_j a\_{j+k},
\ k \in \mathbb Z .
\tag{3.6}
\end{align}\]

当\(\{a\_j\} \in l\_2\)时结论仍成立。

**证明**:

由§[2.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#tsa-linser-filt-linser)知
\(\{ X\_t \}\)是零均值平稳列，
自协方差函数为[(3.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#eq:norlim-norser-gammak)。

只要证明\(\{ X\_t \}\)是正态序列，
只要证明\(\forall m \in \mathbb N\_+\),
\(\boldsymbol X = (X\_{-m}, \dots, X\_0, \dots, X\_m)^T\)服从多元正态分布。
要使用定理[3.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#thm:norlim-mnormcond)（多元正态与一元正态关系）
和定理[3.10](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#thm:norlim-norserlimdist)（一元正态分布的依分布极限仍为正态分布）。

记\(\Sigma = (\gamma\_{|i-j|})\_{i,j=-m, \dots, m}\)，
来证明\(\boldsymbol X \sim \text{N}(0, \Sigma)\)。
记
\[\begin{aligned}
\eta\_t(n) = \sum\_{j=-n}^n a\_j \varepsilon\_{t-j}, \ t = -m, \dots, m
\end{aligned}\]
这是\(X\_t\)的部分和。由控制收敛定理可知
\[\begin{aligned}
E|\eta\_t(n) - X\_t| \leq& \sum\_{|j|>n} |a\_j| \sigma \to 0 (n \to \infty)
\end{aligned}\]

对\(\forall \boldsymbol b = (b\_{-m}, \dots, b\_0, \dots, b\_m)^T \in \mathbb R^{2m+1}\),
记
\[\begin{aligned}
Y =& \boldsymbol b^T \boldsymbol X = \sum\_{t=-m}^m b\_t X\_t \\
\eta(n) =& \sum\_{t=-m}^m b\_t \eta\_t(n)
\end{aligned}\]
则当\(n\to\infty\)时
\[\begin{aligned}
E|\eta(n) - Y| \leq \sum\_{t=-m}^m |b\_t| \cdot E|\eta\_t(n) - X\_t| \to 0
\end{aligned}\]
即\(\eta(n) \stackrel{L\_1}{\to} Y\),
于是\(\eta(n) \stackrel{d}{\to} Y\)，
由定理[3.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#thm:norlim-mnormcond)知\(\eta(n)\)服从正态分布，
由定理[3.10](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#thm:norlim-norserlimdist)知\(Y\)服从正态分布，
易见\(EY=0\),
\[\begin{aligned}
\text{Var}(Y)
= \text{Var}(\sum\_{t=-m}^m b\_t X\_t)
= \boldsymbol b^T \Sigma \boldsymbol b
\end{aligned}\]
即有\(Y \sim \text{N}(0, \boldsymbol b^T \Sigma \boldsymbol b)\)，
从而由定理[3.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-normallim.html#thm:norlim-mnormcond)可知\(\boldsymbol X\)服从多元正态分布，
从而\(\{ X\_t \}\)为正态序列。

○○○○○○

### 3.4.1 一些反例

**例3.1** a.s.收敛推出依概率收敛，
但是反之不然。给出反例。

设\(\Omega=[0,1]\)，
\(P(\cdot)\)为\([0,1]\)上的勒贝格测度。
令\(f\_{mk} = I\_{[\frac{k-1}{m}, \frac{k}{m}]}\),
\(k=1,2,\dots,m\),
\(m=1,2,\dots\)。
将\(\{ f\_{mk} \}\)排序为
\(f\_{11}\), \(f\_{21}\), \(f\_{22}\),
\(f\_{31}\), \(f\_{32}\), \(f\_{33}\), \(\ldots\)，
记这个随机变量序列为\(X\_n(\omega)\)(\(\omega \in [0,1]\))。
则对任意\(\epsilon \in (0,1)\)，
\[
P(|X\_n(\omega)| > \epsilon)
= P(X\_n(\omega) = 1)
\to 0 \ (n\to\infty) .
\]
即\(X\_n\)依概率收敛到0。
但是对任意\(\omega \in [0,1]\)，
总有无数个\(n\)使得\(X\_n(\omega)=1\)，
从而\(X\_n\)不a.s.收敛到0。

○○○○○○

**例3.2** \(L\_1\)收敛和\(L\_2\)收敛都推出依概率收敛，
但是反之不然。给出反例。

设\(\Omega=[0,1]\)，
\(P(\cdot)\)为\([0,1]\)上的勒贝格测度。
令\(X\_n(\omega) = n^2 I\_{[0, \frac{1}{n}]}(\omega)\),
则对任意\(\epsilon \in (0,1)\)有
\[
P(|X\_n - 0| > \epsilon)
= P(X\_n \neq 0) = \frac{1}{n}
\to 0, \ n \to \infty
\]
即\(X\_n\)依概率收敛到0。
但是，
\[
E|X\_n - 0| = E X\_n
= \int\_0^1 X\_n(\omega) \, d\omega
= n^2 \cdot \frac{1}{n} = n
\to \infty \ (n\to\infty)
\]
所以\(X\_n\)不\(L\_1\)收敛到0,
也不\(L\_2\)收敛到0。

○○○○○○

**例3.3** 依概率收敛推出依分布收敛，
但是反之不然。给出反例。

设\(X, X\_1, X\_2, \dots\)独立同N(0,1)分布。
则\(X\_n\)的分布函数\(F\_n(x)\)与\(X\)的分布函数\(F(x)\)处处相等，
当然有\(\lim\_{n\to\infty} F\_n(x) = F(x)\)，
对任意\(x \in (-\infty, \infty)\)成立，
即\(X\_n\)依分布收敛到\(X\)。
但是对任意\(\epsilon>0\)，
因为\(X\_n - X \sim \text{N}(0, 2)\)，
所以
\[
P(|X\_n - X| > \epsilon)
= 2(1 - \Phi(\epsilon/\sqrt{2}))
\]
为正常数，
因此\(X\_n\)不能依概率收敛到\(X\)。
上式中\(\Phi(\cdot)\)表示标准正态分布函数。

○○○○○○

## 3.5 补充

### 3.5.1 联合密度

**性质**：
若\(\boldsymbol Y\)服从多元正态分布\(N(\boldsymbol\mu, \Sigma)\)且\(\Sigma\)正定，
则
\(\boldsymbol Y\)有联合密度
\[
p(\boldsymbol y) = (2\pi)^{-\frac{n}{2}} |\Sigma|^{-\frac{1}{2}}
\exp\left\{ -\frac12 (\boldsymbol y - \boldsymbol\mu)^T \Sigma^{-1}
(\boldsymbol y - \boldsymbol\mu) \right\}
\]

**证明**:

当\(\Sigma\)为对称正定阵时，
由线性代数知识可知\(\Sigma\)有特征值分解
\(\Sigma = U \Lambda U^T\)，
其中\(\Lambda = \text{diag}(\lambda\_1, \dots, \lambda\_n)\)，
\(\lambda\_j > 0, j=1,2,\dots,n\)，
\(U\)为正交阵\(U^T U = I\_n\)。
令\(\Lambda^{-1/2} = \text{diag}(\lambda\_1^{-1/2}, \dots, \lambda\_n^{-1/2})\)，
\(\Sigma^{-1/2} = U \Lambda^{-1/2} U^T\)，
令\(\boldsymbol X = \Sigma^{-1/2} (\boldsymbol Y - \boldsymbol\mu)\)，
则\(\boldsymbol Y = \boldsymbol\mu + \Sigma^{1/2} \boldsymbol X\),
\(\boldsymbol X\)的特征函数为
\[\begin{aligned}
\phi(\boldsymbol t) =& E \exp\left\{ i \boldsymbol t^T \boldsymbol X \right\} \\
=& E \exp\left\{ i \boldsymbol t^T \Sigma^{-1/2} \boldsymbol Y \right\}
\cdot \exp\left\{ -i \boldsymbol t^T \Sigma^{-1/2} \boldsymbol\mu \right\} \\
=& \exp\left\{ i \boldsymbol t^T \Sigma^{-1/2} \boldsymbol\mu
- \frac12 \boldsymbol t^T \Sigma^{-1/2} \Sigma \Sigma^{-1/2} \boldsymbol t \right\}
\cdot \exp\left\{ -i \boldsymbol t^T \Sigma^{-1/2} \boldsymbol\mu \right\} \\
=& \exp\left\{ -\frac12 \boldsymbol t^T \boldsymbol t \right\}
\end{aligned}\]
这说明\(\boldsymbol X\)为\(n\)维标准正态分布随机向量，
于是\(\boldsymbol X\)的联合密度函数为
\(p\_{\boldsymbol X}(\boldsymbol x) = (2\pi)^{-n/2} \exp\{ -\frac12 \boldsymbol x^T \boldsymbol x \}\)。
从\(\boldsymbol X\)到\(\boldsymbol Y\)的变换的逆变换为
\(\boldsymbol X = \Sigma^{-1/2} (\boldsymbol Y - \boldsymbol\mu)\)，
这是\(\mathbb R^n\)上的一一变换，
逆变换的Jacobi行列式为\(|\Sigma^{-1/2}|=|\Sigma|^{-1/2}\)。
由随机向量的变换的密度公式可得\(\boldsymbol Y\)的密度为
\[\begin{aligned}
p\_{\boldsymbol Y}(\boldsymbol y)
=& p\_{\boldsymbol X}(\Sigma^{-1/2}(\boldsymbol y - \boldsymbol\mu)) \cdot |\Sigma|^{-1/2} \\
=& (2\pi)^{-n/2} |\Sigma|^{-1/2} \exp\left\{ -\frac12 (\boldsymbol y - \boldsymbol\mu)^T
\Sigma^{-1} (\boldsymbol y - \boldsymbol\mu) \right\}
\end{aligned}\]

○○○○○○

**性质**：对\(\boldsymbol\mu \in \mathbb R^n\)和\(n\)阶对称非负定阵\(\Sigma\)，
设\(\Sigma\)的秩为\(m \leq n\)，
则存在列满秩矩阵\(B\_{n\times m}\)和\(m\)元的标准多元正态分布随机向量\(\boldsymbol X\)
使得\(\boldsymbol Y = \boldsymbol\mu + B \boldsymbol X\)
服从多元正态分布\(\text{N}(\boldsymbol\mu, \Sigma)\)分布。

**证明**：

由线性代数知识，\(\Sigma\)有如下的特征值分解：
\[
\Sigma = U \text{diag}(\lambda\_1, \dots, \lambda\_m, 0, \dots, 0) U^T,
\]
其中\(\lambda\_1 \geq \dots \geq \lambda\_m > 0\)是\(\Sigma\)的正特征值，
\(U\)为\(n\)阶正交阵，
记\(U = (U\_1\ U\_2)\)，
其中\(U\_1\)是\(U\)的前\(m\)列组成的矩阵，
记\(\Lambda\_1 = \text{diag}(\lambda\_1, \dots, \lambda\_m)\),
\(\Lambda\_1^{1/2} = \text{diag}(\lambda\_1^{1/2}, \dots, \lambda\_m^{1/2})\),
则
\[
\Sigma = (U\_1\ U\_2)
\left(\begin{array}{cc}
\Lambda\_1 & 0 \\
0 & 0
\end{array}\right)
\left(\begin{array}{c}
U\_1^T \\
U\_2^T
\end{array}\right)
= U\_1 \Lambda\_1 U\_1^T
\]
令\(B = U\_1 \Lambda\_1^{1/2}\)，
则\(B B^T = \Sigma\)，
于是若\(\boldsymbol X\)服从\(m\)元的标准多元正态分布，
则
\(\boldsymbol Y = \boldsymbol\mu + B \boldsymbol X\)
服从多元正态分布\(\text{N}(\boldsymbol\mu, B B^T)\)即
\(\text{N}(\boldsymbol\mu, \Sigma)\)。

○○○○○○

### 3.5.2 二元正态分布

二元正态分布的协方差阵为
\[
\Sigma = \left(\begin{array}{cc}
\sigma\_1^2 & \rho \sigma\_1 \sigma\_2 \\
\rho \sigma\_1 \sigma\_2 & \sigma\_2^2
\end{array}\right)
\]
行列式\(|\Sigma| = \sigma\_1^2 \sigma\_2^2 (1 - \rho^2)\),
\(|\Sigma| = 0\)当且仅当\(\rho = \pm 1\)。
\(|\rho| < 1\)时有联合密度
\[\begin{aligned}
p\_{\boldsymbol Y}(\boldsymbol y) =&
\frac{1}{2 \pi \sigma\_1 \sigma\_2 \sqrt{1 - \rho^2}}
\exp\left\{ - \frac{1}{2(1-\rho^2)} \left[
\left( \frac{y\_1 - \mu\_1}{\sigma\_1} \right)^2
+ \left( \frac{y\_2 - \mu\_2}{\sigma\_2} \right)^2 \right. \right. \\
& \left. \left. - 2 \rho \left( \frac{y\_1 - \mu\_1}{\sigma\_1} \right) \left( \frac{y\_2 - \mu\_2}{\sigma\_2} \right)
\right] \right\}
\end{aligned}\]

### 3.5.3 正态条件分布

设
\[\begin{aligned}
\boldsymbol X = \left(\begin{array}{c}
\boldsymbol X\_1 \\ \boldsymbol X\_2
\end{array}\right)
\sim \text{N}(\boldsymbol\mu, \Sigma),
\ \boldsymbol \mu = \left(\begin{array}{c}
\boldsymbol \mu\_1 \\ \boldsymbol \mu\_2
\end{array}\right)
\ \Sigma = \left(\begin{array}{cc}
\Sigma\_{11} & \Sigma\_{12} \\
\Sigma\_{21} & \Sigma\_{22}
\end{array}\right)
\end{aligned}\]
则\(\boldsymbol X\_2 = \boldsymbol x\_2\)条件下\(\boldsymbol X\_1\)的条件分布为
\[\begin{aligned}
\text{N}\big(\boldsymbol\mu\_1 + \Sigma\_{12}\Sigma\_{22}^{-1}(\boldsymbol x\_2 - \boldsymbol\mu\_2),
\; \Sigma\_{11} - \Sigma\_{12} \Sigma\_{22}^{-1} \Sigma\_{21} \big).
\end{aligned}\]
条件方差不依赖于\(\boldsymbol x\_2\)的值。

令
\[\begin{aligned}
\boldsymbol X\_{1\cdot 2} =& \boldsymbol X\_1 - E(\boldsymbol X\_1 | \boldsymbol X\_2)
= \boldsymbol X\_1 - \boldsymbol\mu\_1 + \Sigma\_{12}\Sigma\_{22}^{-1}(\boldsymbol X\_2 - \boldsymbol\mu\_2), \\
\Sigma\_{11\cdot 2} =& \Sigma\_{11} - \Sigma\_{12} \Sigma\_{22}^{-1} \Sigma\_{21}
\end{aligned}\]
则\((\boldsymbol X\_2, \boldsymbol X\_{1\cdot 2})\)独立，
\(\boldsymbol X\_{1\cdot 2} \sim \text{N}(\boldsymbol 0, \Sigma\_{11\cdot 2})\)。

### 3.5.4 多元正态分布等价定义证明

如果随机向量\(\boldsymbol Z\)有特征函数
\[\begin{aligned}
\phi(\boldsymbol t) = \exp(i \boldsymbol t^T \boldsymbol\mu - \frac12 \boldsymbol t^T \Sigma \boldsymbol t),
\end{aligned}\]
其中\(\Sigma\)是\(n\)阶非负定矩阵，
则存在分量独立同标准正态分布的随机向量\(\boldsymbol\varepsilon\)和常数矩阵\(B\)使得
\(\boldsymbol Z = \boldsymbol\mu + B \boldsymbol\varepsilon\)。

**证明**：
令
\[\begin{aligned}
\boldsymbol Y = \boldsymbol Z - \boldsymbol\mu
\end{aligned}\]
则\(\boldsymbol Y\)的特征函数为
\[\begin{aligned}
\phi\_{\boldsymbol Y}(\boldsymbol t) =& E \exp\left[ i\boldsymbol t^T \boldsymbol Y \right]
= \exp \left[ -\frac12 \boldsymbol t^T \Sigma \boldsymbol t \right]
\end{aligned}\]
设\(\text{rank}(\Sigma)=m\leq n\),
做特征值分解
\[\begin{aligned}
& \Sigma = P^T \Lambda P, \ P^T P = I\_n, \\
& \Lambda = \text{diag}(\lambda\_1, \lambda\_2, \dots, \lambda\_m,
0, \dots, 0)
\end{aligned}\]
(其中\(\lambda\_j>0, j=1,2,\dots,m\))。
令
\[\begin{aligned}
A =& \text{diag}(\lambda\_1^{-\frac{1}{2}}, \lambda\_2^{-\frac{1}{2}},
\dots, \lambda\_m^{-\frac{1}{2}}, 1, \dots, 1) \\
\boldsymbol W =& A P \boldsymbol Y
\end{aligned}\]
则
\[\begin{aligned}
\boldsymbol Y = P^T A^{-1} \boldsymbol W
\stackrel{\triangle}{=} D \boldsymbol W,
\end{aligned}\]
其中
\[\begin{aligned}
\text{Var}(\boldsymbol W)
= \text{Var}(A P \boldsymbol Y) = A P \Sigma P^T A = A \Lambda A
= \text{diag}(1,1,\dots, 1, 0, \dots, 0)
\end{aligned}\]
所以
\[\begin{aligned}
\boldsymbol W = \left(\begin{array}{c}
\boldsymbol \varepsilon \\ \boldsymbol 0 \end{array}\right)
\end{aligned}\]
其中\(\boldsymbol \varepsilon\)为\(m\)维。记
\[\begin{aligned}
G = \left( I\_m \ \ \boldsymbol 0 \right)\_{m\times n}
\end{aligned}\]
则\(\boldsymbol \varepsilon = G \boldsymbol W = G A P \boldsymbol Y\),
\(\boldsymbol \varepsilon\)的特征函数为
\[\begin{aligned}
\phi\_{\boldsymbol \varepsilon}(\boldsymbol t)
=& E \exp\left[ i \boldsymbol t^T G A P \boldsymbol Y \right]
= \phi\_{\boldsymbol Y}(P^T A G^T \boldsymbol t) \\
=& \exp\left[ -\frac12 \boldsymbol t^T GAP\Sigma P^T A G^T \boldsymbol t \right]
= \exp\left[ -\frac12 \boldsymbol t^T \boldsymbol t \right]
\end{aligned}\]
即\(\boldsymbol \varepsilon \sim \text{N}(0, I\_m)\)。
则
\[\begin{aligned}
\boldsymbol Y =& D \boldsymbol W
= \left(D\_1 \ D\_2 \right)
\left( \begin{array}{c}
\boldsymbol \varepsilon \\ 0 \end{array}\right) \\
=& D\_1 \boldsymbol \varepsilon
\stackrel{\triangle}{=} B \boldsymbol \varepsilon \\
\boldsymbol Z =& \boldsymbol \mu + B \boldsymbol \varepsilon .
\end{aligned}\]
证毕。