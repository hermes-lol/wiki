---
crawl_time: '2026-01-17 14:29:01'
framework: sphinx
title: D 线性代数 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-linalg.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# D 线性代数

## D.1 行列式

对\(n\)阶方阵\(A\)，
行列式为
\[
\text{det}(A)
= \sum\_{j\_1 j\_2 \dots j\_n} (-1)^{\tau(j\_1 j\_2 \dots j\_n)} a\_{1 j\_1} a\_{2 j\_2} \dots a\_{n j\_n} .
\]
其中的求和对所有的\(n!\)个\((1,2,\dots,n)\)的全排列\((j\_1, j\_2, \dots, j\_n)\)进行，
\(\tau(j\_1 j\_2 \dots j\_n)\)是排列\(j\_1 j\_2 \dots j\_n\)的逆序数，即
\[
\tau(j\_1 j\_2 \dots j\_n)
= \#\{(j\_i, j\_k): 1 \leq i < k \leq n, j\_i > j\_k \} .
\]

行列式可以看成是关于\(n^2\)个自变量的\(n\)次多项式函数。

对\(n\)阶方阵\(A\)的元素\(a\_{ij}\)，
将第\(i\)行和第\(j\)列删去后得到的\(n-1\)阶行列式\(M\_{ij}\)称为\(a\_{ij}\)的余子式，
而\(A\_{ij} = (-1)^{i+j} M\_{ij}\)称为\(a\_{ij}\)的代数余子式。
行列式按一行或一列展开的公式为
\[\begin{aligned}
\text{det}(A)
=& \sum\_{k=1}^n a\_{ik} A\_{ik}, \ \forall i \in \{1,2,\dots,n\} \\
=& \sum\_{l=1}^n a\_{lj} A\_{lj}, \ \forall j \in \{1,2,\dots,n\} .
\end{aligned}\]

**范德蒙(Vandrmonde)行列式**：
方阵\(A\)元素为\(a\_{ij} = c\_j^{i-1}\),
则
\[
\text{det}(A) = \prod\_{i < j} (c\_j - c\_i) .
\]

**克莱姆(Cramer)法则**：
设\(A\)为\(n\)阶方阵，\(\boldsymbol b\)为\(n\)维向量，
方程组\(A \boldsymbol x = \boldsymbol b\)当且仅当\(\text{det}(A) \neq 0\)时存在唯一解，
且第\(j\)个未知数的解等于将\(A\)的第\(j\)列替换成\(\boldsymbol b\)后的矩阵行列式，
除以\(\text{det}(A)\)的结果。
这样，\(n\)个方程、\(n\)个未知数的方程组在\(\text{det}(A) \neq 0\)时的解都是有理分式形式，
分子和分母的多项式次数为\(n\)。

**伴随矩阵**：
对方阵\(A\)，
设元素\(a\_{ij}\)的代数余子式为\(A\_{ij}\)，
令矩阵\(A^\* = (A\_{ij})\_{n\times n}^T\)，
称\(A^\*\)为\(A\)的伴随矩阵，
有
\[
A A^\* = A^\* A = \text{det}(A) I,
\]
当\(A\)可逆时有
\[
A^{-1} = \frac{1}{\text{det}(A)} A^\* .
\]

## D.2 线性空间和内积空间

**实数域上的线性空间**
某个集合\(H\)如果定义了如下的加法运算“\(+\)”和数乘运算“\(\cdot\)”，使得
\[\begin{aligned}
(1) & \text{若}\boldsymbol x, \boldsymbol y \in H,
\text{则}\boldsymbol x + \boldsymbol y \in H, \text{且} \\
& \boldsymbol x + \boldsymbol y = \boldsymbol y + \boldsymbol x \\
& (\boldsymbol x + \boldsymbol y) + \boldsymbol z
= \boldsymbol x + (\boldsymbol y + \boldsymbol z),
\text{其中} \boldsymbol z \in H \\
& \text{存在零元素} \boldsymbol 0, \text{使得}
\boldsymbol x + \boldsymbol 0 = \boldsymbol x, \forall \boldsymbol x \in H \\
& \forall \boldsymbol x \in H, \text{存在负元素} \boldsymbol z
\text{使得} \boldsymbol x + \boldsymbol z = \boldsymbol 0 \\
(2) & \text{对标量} \alpha, \beta \in \mathbb R,
\text{向量} \boldsymbol x, \boldsymbol y \in H,
\text{数乘结果} \alpha \cdot \boldsymbol x \in H, \text{且} \\
& (\alpha + \beta) \cdot \boldsymbol x
= \alpha \cdot \boldsymbol x + \beta \cdot \boldsymbol x \\
& \alpha \cdot (\boldsymbol x + \boldsymbol y)
= \alpha \cdot \boldsymbol x + \alpha \cdot \boldsymbol y \\
& (\alpha \beta) \cdot \boldsymbol x = \alpha \cdot (\beta \cdot \boldsymbol x) \\
& 1 \cdot \boldsymbol x = \boldsymbol x
\end{aligned}\]
则称\(H\)为实数域\(\mathbb R\)上的线性空间。

**内积**:
实数域\(\mathbb R\)上的线性空间\(H\)
中向量\(\boldsymbol x\)和\(\boldsymbol y\)的实值二元函数
\(<\boldsymbol x, \boldsymbol y>\)称为一个内积，
如果满足如下条件：
\[\begin{aligned}
(1) & <\boldsymbol x, \boldsymbol y> = <\boldsymbol y, \boldsymbol x>,
\forall \boldsymbol x, \boldsymbol y \in H \\
(2) & <\boldsymbol x + \boldsymbol y, \boldsymbol z>
= <\boldsymbol x, \boldsymbol z>
+ <\boldsymbol y, \boldsymbol z>,
\forall \boldsymbol x, \boldsymbol y,
\boldsymbol z \in H \\
(3) & <\alpha \boldsymbol x, \boldsymbol y>
= \alpha <\boldsymbol x, \boldsymbol y>,
\forall \alpha \in \mathbb R,
\boldsymbol x, \boldsymbol y \in H \\
(4) & <\boldsymbol x, \boldsymbol x> \geq 0,
\forall \boldsymbol x \in H \\
(5) & <\boldsymbol x, \boldsymbol x>=0
\Longleftrightarrow \boldsymbol x = \boldsymbol 0 .
\end{aligned}\]
定义了内积的线性空间称为**内积空间**。
\(n\)维欧式空间\(\mathbb R^n\)是内积空间，
内积空间是\(\mathbb R^n\)的推广。

从内积可以导出向量的模（长度、范数）：
\[
\| \boldsymbol x \|
= \sqrt{<\boldsymbol x, \boldsymbol x>}
\]
\(\| \boldsymbol x \| = 0\)当且仅当\(\boldsymbol x = \boldsymbol 0\)。

实数域上的内积空间的内积和内积对应的模总满足Cauchy-Schwarz不等式：
\[
|<\boldsymbol x, \boldsymbol y>|
\leq \| \boldsymbol x \| \; \| \boldsymbol y \|
\]
等号成立当且仅当\(\boldsymbol y = \alpha \boldsymbol x\)或
\(\boldsymbol x = \beta \boldsymbol y\)。
证明参见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book)) P.44 §2.1式(2.1.4)的证明。

内积导出的模满足三角不等式：
\[
\| \boldsymbol x + \boldsymbol y \|
\leq \| \boldsymbol x \| + \| \boldsymbol y \|
\]
这可以用Cauchy-Schwarz不等式证明。

**范数（模）**的定义：
实数域\(\mathbb R\)上的线性空间\(H\)上的实值函数\(\| \bullet \|\)称为一个范数（模），
如果满足如下条件：
\[\begin{aligned}
(1) & \| \boldsymbol x \| \geq 0, \forall \boldsymbol x \in H \\
(2) & \| \boldsymbol x \| = 0 \Longleftrightarrow
\boldsymbol x = \boldsymbol 0, \forall \boldsymbol x \in H \\
(3) & \| \boldsymbol x + \boldsymbol y \|
\leq \| \boldsymbol x \| + \| \boldsymbol y \|,
\forall \boldsymbol x, \boldsymbol y \in H \\
(4) & \| \alpha \boldsymbol x \| = |\alpha| \; \| \boldsymbol x \|,
\forall \alpha \in \mathbb R, \boldsymbol x \in H
\end{aligned}\]
定义了范数（模）的线性空间称为度量空间或者赋范空间。

由内积导出的模满足以上的一般范数定义。

在定义了范数以后，
可以定义空间中的元素极限。
对\(\boldsymbol x\_n\), \(\boldsymbol x\)，
称\(\lim\_{n\to\infty} \boldsymbol x\_n = \boldsymbol x\)，
如果\(\lim\_{n\to\infty} \| \boldsymbol x\_n - \boldsymbol x \| = 0\)。

如果\(H\)是内积空间，
内积有如下的连续性：
若\(\boldsymbol x\_n, \boldsymbol y\_n, \boldsymbol x, \boldsymbol y \in H\)，
且\(\lim\_{n\to\infty} x\_n = \boldsymbol x\),
\(\lim\_{n\to\infty} y\_n = \boldsymbol y\),
则
\[\begin{aligned}
(1) & \| \boldsymbol x\_n \| \to \| \boldsymbol x \|, \ n \text{当}\to \infty \\
(2) & <\boldsymbol x\_n, \boldsymbol y\_n>
\to <\boldsymbol x, \boldsymbol y>, \ \text{当}n \to \infty
\end{aligned}\]
证明与前面关于\(L^2\)的证明相同。

○○○○○○

### References

Brockwell, P. J., and R. A. Davis. 1987. *Time Series: Theory and Methods*. Springer-Verlag.