---
crawl_time: '2026-01-17 14:25:30'
framework: sphinx
title: 32 广义逆矩阵 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 32 广义逆矩阵

## 32.1 广义逆的定义和性质

当\(A\)为满秩方阵时，
线性方程组\(A \boldsymbol x = \boldsymbol b\)有唯一解\(\boldsymbol x = A^{-1} \boldsymbol b\)。
当\(A\)为不满秩的方阵或长方形\(n \times m\)矩阵时，
\(A^{-1}\)不存在，
这时能否用类似逆矩阵的方式表示线性方程组\(A \boldsymbol x = \boldsymbol b\)的解？
\(A \boldsymbol x = \boldsymbol b\)可能有唯一解、无穷多个解或无解（无解时可以找最小二乘解），
用广义逆矩阵可以统一地给出这些问题的解。

一些统计计算问题的理论研究和数值计算也用到广义逆矩阵，
比如，
线性模型中参数最小二乘估计的表示,
典型相关分析中典型相关系数和典型变量的计算。

广义逆有多种定义，最常用的一种是加号逆。

**定义32.1 (加号逆和减号逆)** 设\(A\)为\(n\times m\)矩阵，
若\(m \times n\)矩阵\(G\)满足
\[\begin{aligned}
\text{i)} &\ AGA = A; \\
\text{ii)} &\ GAG = G; \\
\text{iii)} &\ (AG)^T = AG; \\
\text{iv)} &\ (GA)^T = GA,
\end{aligned}\]
则称矩阵\(G\)为矩阵\(A\)的**加号逆**或Moore-Penrose广义逆，
记作\(A^+\)。如果\(G\)满足定义中的第一个条件，则称\(G\)为
\(A\)的**减号逆**，记作\(A^-\)。

显然，如果\(A\)本身就是\(n\)阶可逆方阵，
则\(A^{-1}\)满足上述四个条件且是唯一的。
一般情况下有：

**定理32.1** \(n \times m\)矩阵\(A\)的加号逆存在且唯一。

**证明**:
若\(A\)不是零矩阵（所有元素都等于零的矩阵），
则\(A\)有奇异值分解\(A = V D U^T\)，
取\(G = U D^+ V^T\),
其中\(D^+\)是把对角阵\(D\)的主对角线中非零元素换成相应元素的倒数，
其它元素保持为零，
容易验证\(G\)是\(A\)的加号逆。
当\(A\)为零矩阵时，
\(m\times n\)的零矩阵是\(A\)的加号逆。

若\(A\_1^+\)和\(A\_2^+\)是\(n\times m\)矩阵\(A\)的两个加号逆，
则
\[\begin{align\*}
& A\_1^+ = A\_1^+ A A\_1^+ = A\_1^+ (A A\_1^+)^T
= A\_1^+ (A\_1^+)^T (A)^T
= A\_1^+ (A\_1^+)^T (A A\_2^+ A)^T \\
=& A\_1^+ (A\_1^+)^T A^T (A A\_2^+)^T
= A\_1^+ (A A\_1^+)^T A A\_2^+
= A\_1^+ A A\_1^+ A A\_2^+
= A\_1^+ A A\_2^+ \\
=& (A\_1^+ A)^T A\_2^+
= A^T (A\_1^+)^T A\_2^+
= (A A\_2^+ A)^T (A\_1^+)^T A\_2^+
= (A\_2^+ A)^T A^T (A\_1^+)^T A\_2^+ \\
=& (A\_2^+ A)^T(A\_1^+ A)^T A\_2^+
= A\_2^+ A A\_1^+ A A\_2^+
= A\_2^+ A A\_2^+
= A\_2^+
\end{align\*}\]
可见加号逆存在唯一。
证毕。

※※※※※

以上定理证明中给出了用奇异值分解计算加号逆的方法。
另一种计算加号逆的方法是利用“满秩分解”。
设\(A\)为非零的\(n \times m\)实矩阵，
设\(\text{rank}(A)=r\),
则存在\(n \times r\)的满秩矩阵\(B\)和\(r \times m\)的满秩矩阵\(C\)使得\(A = BC\),
这称为\(A\)的**满秩分解**(见习题3)。
这时，\(A\)的加号逆可表示为
\[\begin{align}
A^+ = C^T (C C^T)^{-1} (B^T B)^{-1} B^T.
\end{align}\]

加号逆也是减号逆，
所以减号逆一定存在，
但是当\(A\)不是满秩方阵时\(A\)的减号逆有无穷多个。

容易看出，
当且仅当\(A\)为满秩方阵时\(A^+ = A^{-1}\)。
当且仅当\(A\)为\(n \times m\)列满秩阵时
\(A^+ A = I\_m\),
这时\(A^+ = (A^T A)^{-1} A^T\)；
当且仅当\(A\)为\(n \times m\)行满秩阵时
\(A A^+ = I\_n\)，
这时\(A^+ = A^T (A A^T)^{-1}\)。

**定理32.2** 加号逆有如下的性质:
\[\begin{align\*}
\text{i)}\,&\,
(A^+)^+ = A; \\
\text{ii)}\,&\,
(A^T)^+ = (A^+)^T; \\
\text{iii)}\,&\,
(\lambda A)^+ = \lambda^{-1} A^+, \ \forall \lambda \neq 0;\\
\text{iv)}\,&\,
\text{rank}(A^+) = \text{rank}(A)
= \text{rank}(A A^+)
= \text{rank}(A^+ A); \\
\text{v)}\,&\,
(A^T A)^+ = A^+ (A^+)^T; \\
\text{vi)}\,&\,
A^+ = (A^T A)^+ A^T = A^T (A A^T)^+;\\
\text{vii)}\,&\,
A A^+ \text{和} A^+ A \text{都是对称幂等矩阵}; \\
\text{viii)}\,&\,
\text{若$A$是对称幂等矩阵，则 $A^+ = A$。}
\end{align\*}\]

证明留给读者。

**定理32.3** 若\(n \times m\)矩阵\(A\)是列满秩矩阵，
有\(QR\)分解\(A=QR\),
\(Q\)为\(n \times m\)矩阵使得\(Q^T Q=I\),
\(R\)为\(m\)阶满秩上三角方阵，
则\(A^+ = R^{-1} Q^T\)。

**证明**:
令\(G = R^{-1} Q^T\)，则
\[
\begin{aligned}
AGA =& QR R^{-1}Q^T QR = QR = A \\
GAG =& R^{-1} Q^T QR R^{-1} Q^T = R^{-1}Q^T = G \\
G A =& R^{-1}Q^T QR = I\_m \text{ 对称} \\
A G =& QR R^{-1} Q^T = Q Q^T \text{ 对称}
\end{aligned}
\]
证毕。

## 32.2 叉积阵(`*`)

设矩阵\(A\)为\(n \times m\)，
矩阵\(A^T A\)经常出现在统计计算中，
比如线性模型的正规方程，
协方差阵估计，等等。
\(A^T A\)是对称半正定矩阵。
\(A^T A\)与\(A\)之间有密切的联系。
\(\text{rank}(A^T A) = \text{rank}(A)\),
\(A = 0\)当且仅当\(A^T A = 0\)，
当\(\text{rank}(A)=m \leq n\)时\(A^T A\)是对称正定阵。

若\(G\)是\(A^T A\)的一个减号逆，
则\(G^T\)也是\(A^T A\)的减号逆（注意对称阵的减号逆不一定对称）：
\[
(A^T A) G^T (A^T A)
= [ (A^T A) G (A^T A)]^T
= (A^T A)^T = (A^T A)
\]

\(A^T A\)的广义逆与\(A\)的广义逆密切相关。

**定理32.4** 对\(A^T A\)的任一个减号逆\((A^T A)^-\)，
\((A^T A)^- A^T\)是\(A\)的减号逆。

**证明**:
由定理[32.2](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#thm:ginv-props)性质vi),
\[
A^+ = (A^T A)^+ A^T,
\quad
A = A A^+ A = A (A^T A)^+ A^T A,
\]
所以
\[
\begin{aligned}
& A[(A^T A)^- A^T] A \\
=& A A^+ A \cdot [(A^T A)^- A^T] A \\
=& A [(A^T A)^+ A^T] A \cdot [(A^T A)^- A^T] A \\
=& A (A^T A)^+ \cdot [ (A^T A) (A^T A)^- (A^T A)] \\
=& A (A^T A)^+ \cdot (A^T A) \\
=& A [(A^T A)^+ A^T] A \\
=& A A^+ A = A .
\end{aligned}
\]

定理证毕。

※※※※※

## 32.3 正交投影(`*`)

设\(\mathbb R^n\)为\(n\)维欧式空间，
即\(\boldsymbol x = (x\_1, x\_2, \dots, x\_n)^T\), \(x\_i \in \mathbb R\),
\(i=1,2,\dots,n\)这样的元素组成的集合并定义了加法和数乘，
又定义了内积
\[
(\boldsymbol x, \boldsymbol y) = \boldsymbol x^T \boldsymbol y =
\sum\_{i=1}^n x\_i y\_i
\]
当\((\boldsymbol x, \boldsymbol y) = 0\)时称\(\boldsymbol x\)和\(\boldsymbol y\)正交，
记作\(\boldsymbol x \perp \boldsymbol y\)。

设\(A\)为\(n \times m\)实数矩阵，
记\(A \in \mathbb R^{n \times m}\)。
称
\[
\mu(A) = \{ A\boldsymbol \beta \in \mathbb R^n:\ \boldsymbol\beta \in \mathbb R^m \}
\]
为\(A\)的各列张成的线性子空间。
\(\mathbb R^n\)的线性子空间是\(\mathbb R^n\)的子集且加法和数乘在此子集中封闭。
记
\[
\mu(A)^{\perp} = \{ \boldsymbol z \in \mathbb R^n:\ A^T \boldsymbol z = \boldsymbol 0 \}
\]
这是\(\mathbb R^n\)中所有与\(A\)的各列都正交的向量组成的集合，
也是\(\mathbb R^n\)的子空间。

**定理32.5** 设\(A \in \mathbb R^{n \times m}\)，
则对任意\(\boldsymbol y \in \mathbb R^n\)，
有\(\hat{\boldsymbol y} = A A^+ \boldsymbol y \in \mu(A)\),
\(\boldsymbol z = (I - A A^+) \boldsymbol y \in \mu(A)^{\perp}\)，使得
\[
\begin{cases}
\boldsymbol y = \hat{\boldsymbol y} + \boldsymbol z, \\
\hat{\boldsymbol y} \in \mu(A), \ \boldsymbol z \in \mu(A)^{\perp}, \ \hat{\boldsymbol y} \perp \boldsymbol z .
\end{cases}
\]

**证明**:
易见\(\hat{\boldsymbol y} \in \mu(A)\)，
而
\[
\begin{aligned}
A^T \boldsymbol z
=& A^T \boldsymbol y - A^T A A^+ \boldsymbol y
= A^T \boldsymbol y - A^T (A A^+)^T \boldsymbol y \\
=& A^T \boldsymbol y - A^T (A^+)^T A^T \boldsymbol y
= A^T \boldsymbol y - A^T (A^T)^+ A^T \boldsymbol y \\
=& A^T \boldsymbol y - A^T \boldsymbol y = 0
\end{aligned}
\]
所以\(\boldsymbol z \in \mu(A)^{\perp}\)，证毕。

※※※※※

**定义32.2** 设\(A \in \mathbb R^{n\times m}\)，
\(\boldsymbol y \in \mathbb R^n\)，
称\(\hat{\boldsymbol y} = A A^+ \boldsymbol y\)为\(\boldsymbol y\)在子空间\(\mu(A)\)中的**正交投影**。

正交投影满足\(\boldsymbol y = \hat{\boldsymbol y} + (\boldsymbol y - \hat{\boldsymbol y})\),
其中\(\hat{\boldsymbol y} \in \mu(A)\)，
而\(\boldsymbol y - \hat{\boldsymbol y} \in \mu(A)^{\perp}\)。

**定理32.6** \(\boldsymbol y \in \mu(A)\)当且仅当\(A A^+ \boldsymbol y = \boldsymbol y\)；
\(\boldsymbol y \in \mu(A)^{\perp}\)当且仅当
\(A A^+ \boldsymbol y = \boldsymbol 0\)。

**证明**:
若\(\boldsymbol y \in \mu(A)\)，
则存在\(\boldsymbol\beta\)使得\(\boldsymbol y = A\boldsymbol\beta\)，
于是
\[
A A^+ \boldsymbol y = A A^+ A \boldsymbol\beta = A \boldsymbol\beta = \boldsymbol y .
\]
反之，若\(A A^+ \boldsymbol y = \boldsymbol y\),
记\(\boldsymbol\beta = A^+ \boldsymbol y\)，
则\(\boldsymbol y = A \boldsymbol\beta \in \mu(A)\)。

若\(\boldsymbol y \in \mu(A)^{\perp}\)，
即\(A^T \boldsymbol y = \boldsymbol 0\)，
则
\[
A A^+ \boldsymbol y
= (A A^+)^T \boldsymbol y
= (A^+)^T A^T \boldsymbol y = \boldsymbol 0 .
\]
反之，
若\(A A^+ \boldsymbol y = \boldsymbol 0\)，
则
\[
A^T \boldsymbol y
= (A A^+ A)^T \boldsymbol y
= A^T (A A^+)^T \boldsymbol y
= A^T A A^+ \boldsymbol y = \boldsymbol 0 .
\]
证毕。

※※※※※

另外，\(\hat{\boldsymbol y}\)还是\(\mu(A)\)中与\(\boldsymbol y\)距离最近的元素：

**定理32.7** 设\(A \in \mathbb R^{n\times m}\)，
\(\boldsymbol y \in \mathbb R^n\)，
\(\hat{\boldsymbol y} = A A^+ \boldsymbol y\)是\(\boldsymbol y\)在\(\mu(A)\)中的正交投影，
则
\[
\| \boldsymbol y - \hat{\boldsymbol y} \|^2
\leq \| \boldsymbol y - A \boldsymbol\beta \|,
\ \forall \boldsymbol\beta \in \mathbb R^m .
\]

**证明**:
\[
\| \boldsymbol y - A \boldsymbol\beta \|^2
= \| (\boldsymbol y - \hat{\boldsymbol y}) + (\hat{\boldsymbol y} - A \boldsymbol\beta) \|^2
\]
易见
\(\hat{\boldsymbol y} - A \boldsymbol\beta \in \mu(A)\)
而\(\boldsymbol y - \hat{\boldsymbol y} \in \mu(A)^{\perp}\)，
所以
\[
\| \boldsymbol y - A \boldsymbol\beta \|^2
= \| \boldsymbol y - \hat{\boldsymbol y} \|^2 + \| \hat{\boldsymbol y} - A \boldsymbol\beta \|^2
\geq \| \boldsymbol y - \hat{\boldsymbol y} \|^2
\]
证毕。

※※※※※

称\(A A^+\)是\(\mathbb R^n\)向\(\mu(A)\)投影的正交投影阵。

**定义32.3** 设\(P\)为\(n\)阶实对称矩阵，
若\(P = P^2\)，
则称\(P\)为**对称幂等阵**。

\(A A^+\)是对称幂等阵：
\[
(A A^+)^2 = (A A^+ A) A^+ = A A^+ .
\]

**定理32.8** 若\(P\)是对称幂等阵，
则\(P\)是\(\mathbb R^n\)向\(\mu(P)\)的正交投影阵。

**证明**:
要证明\(P = P P^+\)，事实上
\[\begin{aligned}
P P^+
=& P P^+ P P^+
= P P^+ P P P^+ \\
=& P P^+ P (P P^+)^T
= P P^+ P (P^+)^T P^T \\
=& P P^+ P (P^T)^+ P
= P P^+ P P^+ P \\
=& P P^+ P = P
\end{aligned}\]

证毕。

※※※※※

**定理32.9** 设\((A^T A)^-\)是\(A^T A\)的任一个减号逆，
则
\[
A (A^T A)^- A^T = A A^+
\]
即\(A (A^T A)^- A^T\)是向\(A\)的各列张成的线性空间\(\mu(A)\)的正交投影阵，
不依赖于减号逆的选择。

**证明**:
\[
\begin{aligned}
A (A^T A)^- A^T
=& A A^+ A (A^T A)^- (A A^+ A)^T \\
=& A (A^T A)^+ A^T A (A^T A)^- [A (A^T A)^+ A^T A]^T \\
=& A (A^T A)^+ A^T A (A^T A)^- A^T A (A^T A)^+ A^T \\
=& A (A^T A)^+ [A^T A (A^T A)^- A^T A] (A^T A)^+ A^T \\
=& A (A^T A)^+ A^T A (A^T A)^+ A^T \\
=& A (A^T A)^+ A^T
= A A^+
\end{aligned}
\]

证毕。

※※※※※

## 32.4 线性方程组通解(`*`)

利用广义逆可以讨论线性方程组解的表示。

**定理32.10** 系数矩阵为\(n\times m\)矩阵的线性方程组\(A \boldsymbol x = \boldsymbol b\)有解的充分必要条件为
\[\begin{align}
A A^+ \boldsymbol b = \boldsymbol b.
\tag{32.1}
\end{align}\]
在方程组有解时，
对\(A\)的任何一个减号逆\(A^-\),
\(\boldsymbol x = A^- \boldsymbol b\)是方程组的解，
且方程组的通解为
\[\begin{align}
\boldsymbol x = A^+ \boldsymbol b + (I - A^+ A) \boldsymbol y, \ \forall \boldsymbol y \in \mathbb R^m.
\tag{32.2}
\end{align}\]

**证明**:
若[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-solve-cond)成立，则\(\boldsymbol x = A^+ \boldsymbol b\)是一个解。
反之，若\(\boldsymbol x\)满足\(A \boldsymbol x = \boldsymbol b\),
则
\[\begin{align\*}
\boldsymbol b =& A \boldsymbol x = A A^+ A \boldsymbol x = A A^+ \boldsymbol b
\end{align\*}\]
成立，即[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-solve-cond)成立。
另外，方程有解当且仅当\(\boldsymbol b \in \mu(A)\)，
这当且仅当\(A A^+ \boldsymbol b = \boldsymbol b\)。

设\(A^-\)是\(A\)的任意一个减号逆，
若\(\boldsymbol x = A^- \boldsymbol b\)，
则
\[\begin{aligned}
A \boldsymbol x =& A A^- \boldsymbol b
= A A^- (A A^+ \boldsymbol b) \quad(\text{由解的存在性}) \\
=& (A A^- A) A^+ \boldsymbol b
= A A^+ \boldsymbol b = \boldsymbol b \quad(\text{由解的存在性}).
\end{aligned}\]
即当解存在时\(A^- b\)是方程组的解。

令\(\boldsymbol x\)由 [(32.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lineq-gensolexp) 定义，
容易验证它是一个解。
反之，若\(\boldsymbol x\)满足\(A \boldsymbol x = \boldsymbol b\)，
则
\[\begin{align\*}
\boldsymbol x = A^+ \boldsymbol b + \boldsymbol x - A^+ \boldsymbol b
= A^+ \boldsymbol b + \boldsymbol x - A^+ A \boldsymbol x
= A^+ \boldsymbol b + (I - A^+ A) \boldsymbol x,
\end{align\*}\]
满足通解形式。
实际上，通解 [(32.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lineq-gensolexp) 中的\(A^+\)也可以替换成\(A\)的任何一个减号逆。
证毕。

※※※※※

**定理32.11** 当线性方程组\(A \boldsymbol x = \boldsymbol b\)有解时，
\(\boldsymbol x = A^+ \boldsymbol b\)是所有解中唯一的长度最小的解。

**证明**:
有解时通解中
\(A^+ \boldsymbol b \perp (I - A^+ A) \boldsymbol y\)：
\[
\boldsymbol y^T (I - A^+ A)^T A^+ \boldsymbol b
= \boldsymbol y^T (I - A^+ A) A^+ \boldsymbol b
= \boldsymbol y^T (A^+ \boldsymbol b - A^+ A A^+ \boldsymbol b)
= \boldsymbol y^T (A^+ \boldsymbol b - A^+ \boldsymbol b) = \boldsymbol 0,
\]
所以
\[
\| A^+ \boldsymbol b + (I - A^+ A) \boldsymbol y \|^2
= \| A^+ \boldsymbol b \|^2 + \| (I - A^+ A) \boldsymbol y \|^2
\geq \| A^+ \boldsymbol b \|^2 .
\]
证毕。

※※※※※

## 32.5 最小二乘问题通解

广义逆可以用来分析回归分析和线性模型问题中最小二乘解的结构。
设\(X\)为\(n \times m\)矩阵(\(n > m\))，
则当\(X\)列满秩时矩阵
\(P = X (X^T X)^{-1} X^T\)是对称幂等矩阵，
可以把向量\(\boldsymbol y\)正交投影到\(X\)的各列张成的线性空间\(\mu(X)\)中,
这时最小二乘问题
\[\begin{align}
\min\_{\boldsymbol\beta \in \mathbb R^m} \| \boldsymbol y - X \boldsymbol\beta \|\_2^2
\tag{32.3}
\end{align}\]
有唯一解\(\hat{\boldsymbol\beta} = (X^T X)^{-1} X^T \boldsymbol y\)。
对一般情况有如下结论。

**定理32.12** 设\(X\)为\(n \times m\)矩阵(\(n > m\)),
则最小二乘问题[(32.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lsprob)的所有的最小二乘解可以写成
\[\begin{align}
\hat{\boldsymbol\beta} = X^+ \boldsymbol y + (I - X^+ X) \boldsymbol z, \ \forall \boldsymbol z \in \mathbb R^m.
\tag{32.4}
\end{align}\]
在这些最小二乘解中\(\boldsymbol\beta\_0 = X^+ \boldsymbol y\)是唯一的长度最短的解。

**证明**:
令\(P = X X^+\)，则\(P\)是对称幂等矩阵，
\(P \boldsymbol y\)是向量\(\boldsymbol y\)到\(X\)的各列张成的线性子空间\(\mu(X)\)的正交投影,
且
\[\begin{align\*}
X \hat{\boldsymbol\beta} = X X^+ \boldsymbol y + (X - X X^+ X) \boldsymbol z = X X^+ \boldsymbol y = P \boldsymbol y,
\end{align\*}\]
于是
\[\begin{align\*}
\| \boldsymbol y - X \boldsymbol\beta \|\_2^2
=& \| \boldsymbol y - X \hat{\boldsymbol\beta} + X(\hat{\boldsymbol\beta} - \boldsymbol\beta) \|^2 \\
=& \| \boldsymbol y - P \boldsymbol y + X(\hat{\boldsymbol\beta} - \boldsymbol\beta) \|^2 \\
=& \| \boldsymbol y - P \boldsymbol y \|^2 + \| X(\hat{\boldsymbol\beta} - \boldsymbol\beta) \|^2 \quad \text{(因为$P\boldsymbol y$是正交投影)} \\
=& \| \boldsymbol y - X \hat{\boldsymbol\beta} \|^2 + \| X(\hat{\boldsymbol\beta} - \boldsymbol\beta) \|^2 \\
\geq& \| \boldsymbol y - X \hat{\boldsymbol\beta} \|^2,
\end{align\*}\]
所以[(32.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lsgsol)是最小二乘问题[(32.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lsprob)的解,
等号成立当且仅当\(X(X^+ \boldsymbol y - \boldsymbol\beta)=0\)。

若\(\tilde{\boldsymbol\beta}\)是最小二乘问题[(32.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lsprob)的解，
则\(X(X^+ \boldsymbol y - \tilde{\boldsymbol\beta})=0\),
于是\(\tilde{\boldsymbol\beta}\)是线性方程组\(X \tilde{\boldsymbol\beta} = X X^+ \boldsymbol y\)的解，
由定理\(\ref{thm:mat-ginv-linsol}\)可知存在\(\boldsymbol z\)使得
\[\begin{align\*}
\tilde{\boldsymbol\beta}
= X^+ (X X^+ \boldsymbol y) + (I - X^+ X)\boldsymbol z
= X^+ \boldsymbol y + (I - X^+ X)\boldsymbol z.
\end{align\*}\]

设\(\hat{\boldsymbol\beta}\)为最小二乘解[(32.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-ginv.html#eq:mat-ginv-lsgsol)，则
\[\begin{align\*}
\| \hat{\boldsymbol\beta} \|\_2^2
=& \| X^+ \boldsymbol y \|^2 + \| (I - X^+ X) \boldsymbol z \|^2
+ 2 \boldsymbol z^T (I - X^+ X) X^+ \boldsymbol y \\
=& \| X^+ \boldsymbol y \|^2 + \| (I - X^+ X) \boldsymbol z \|^2 \\
\geq& \| X^+ \boldsymbol y \|^2,
\end{align\*}\]
等号成立当且仅当\((I - X^+ X) \boldsymbol z=0\)即\(\hat{\boldsymbol\beta} = X^+ \boldsymbol y\)。

定理证毕。

※※※※※

## 习题

#### 习题1

设\(n \times m\)非零矩阵\(A\)的有奇异值分解\(A = V D U^T\)，
证明\(U D^+ V^T\)为其加号逆,
其中\(D^+\)是把对角阵\(D\)的主对角线中非零元素换成相应元素的倒数，
其它元素保持为零。

#### 习题2

设\(A\)为\(n\)阶实对称方阵，
\(B\)为\(n\)阶正定阵。
写出用Cholesky分解的方法求解广义特征值问题
\(A \boldsymbol\alpha = \lambda B \boldsymbol\alpha\)的算法，
并用编写程序实现该算法。

#### 习题3

设\(A\)为非零的\(n \times m\)实矩阵，
且\(\text{rank}(A)=r\),
证明存在\(n \times r\)的满秩矩阵\(B\)和\(r \times m\)的满秩矩阵\(C\)使得\(A = BC\)。

#### 习题4

设\(n\times m\)非零的实矩阵\(A\)有满秩分解
\(A=BC\),
证明\(A\)的加号逆可表示为
\[\begin{align\*}
A^+ = C^T (C C^T)^{-1} (B^T B)^{-1} B^T.
\end{align\*}\]

#### 习题5

设\(X\)为\(n \times p\)矩阵（\(n>p\)），
\(\boldsymbol y\)为\(n\)维向量，
证明正规方程\(X^T X \boldsymbol\beta = X^T \boldsymbol y\)的解\(\boldsymbol\beta\)中长度最小的一个为
\(X^+ \boldsymbol y\)。

#### 习题6

证明加号逆的性质i)—viii)。

#### 习题7

对\(n \times m\)矩阵\(A\)，
令\(P = A (A^T A)^+ A^T\),
证明\(P=A A^+\), 且\(P\)是对称幂等阵。
记\(\mu(A) = \{ A \boldsymbol x: \ \boldsymbol x \in \mathbb R^m \}\),
证明对任意\(\boldsymbol y \in \mathbb R^n\)有\(P \boldsymbol y \in \mu(A)\)且
\[\begin{align\*}
\boldsymbol z^T (\boldsymbol y - P\boldsymbol y) = 0, \ \forall \boldsymbol z \in \mu(A).
\end{align\*}\]

#### 习题8

设\(A\)为\(n \times m\)矩阵，
若线性方程组\(A \boldsymbol x = \boldsymbol b\)有解，
证明\(\boldsymbol x\_0 = A^+ \boldsymbol b\)是所有解中唯一的长度最小的解。