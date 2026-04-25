---
crawl_time: '2026-01-17 14:25:35'
framework: sphinx
title: 29 特殊线性方程组求解(*) | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 29 特殊线性方程组求解(`*`)

对于特殊的系数矩阵，
比如稀疏的、带状的、巨大的系数矩阵，
需要利用其特殊结构以提高运算效率或减轻对存储空间要求。

## 29.1 带状矩阵

如果矩阵\(A\)的元素\(a\_{ij}\)满足
\(a\_{ij}=0\)对\(i > j+p\)和\(j > i+q\)，
则称\(A\)为**带状矩阵**，
称\(p\)为下带宽，\(q\)为上带宽。
储存带状矩阵时可以排除零元素，仅保存其它元素，
这样每行至多有\(p+q+1\)个元素，
整个矩阵只保存\(n(p+q+1)\)个元素。
带状矩阵如果保存在一个\(n \times (p+q+1)\)矩阵中，
其\(a\_{ij}\)元素在新的存储矩阵的\((i, j - (i-p-1))\)位置。

线性方程组\(A \boldsymbol x = \boldsymbol b\)的系数矩阵如果是带状矩阵，
用列主元的高斯消元法求解会在交换行时失去带状结构。
如果\(A\)有LU分解\(A = L U\)，
易见下三角矩阵\(L\)是一个下带宽\(p\)的带状单位下三角矩阵，
上三角矩阵\(U\)是上带宽\(q\)的带状上三角矩阵。

\(p=q=1\)的带状矩阵叫做三对角矩阵，
只需要把主对角线、下副对角线、上副对角线分别保存在三个\(n\)维向量中。
三对角矩阵如果有LU分解，必为如下形式:
\[\begin{align}
M =& \left(\begin{array}{\*6c}
b\_1 & c\_1 \\
a\_2 & b\_2 & c\_2 \\
& \ddots&\ddots&\ddots\\
& & a\_{n-1} & b\_{n-1} & c\_{n-1} \\
& & & a\_{n} & b\_n
\end{array}\right) \label{eq:mat-band-tri} \\
=& \left(\begin{array}{\*5c}
1 \\
l\_2 & 1 \\
& \ddots&\ddots\\
& & l\_{n-1} & 1 \\
& & & l\_n & 1
\end{array}\right)
\left(\begin{array}{\*5c}
d\_1 & c\_1 \\
& d\_2 & c\_2 \\
& & \ddots & \ddots \\
& & & d\_{n-1} & c\_{n-1} \\
& & & & d\_n
\end{array}\right)
\tag{29.1}
\end{align}\]
所以只要求解\(l\_2, \dots, l\_n\)和\(d\_1, \dots, d\_n\)，
输入\(\boldsymbol a, \boldsymbol b, \boldsymbol c\)三个向量后，
求解得到\(l\_2, \dots, l\_n\)可以放在\(a\_2, \dots, a\_n\)的存储空间中，
\(d\_1, \dots, d\_n\)放在\(b\_1, \dots, b\_n\)的存储空间中，
有如下的递推算法:

| 三对角矩阵递推算法 |
| --- |
| 输入\(\boldsymbol a, \boldsymbol b, \boldsymbol c\) |
| **for**(\(i\) **in** \(2:n\)) { |
| \(\qquad\) \(a\_i \leftarrow a\_i / b\_{i-1}\) |
| \(\qquad\) \(b\_i \leftarrow b\_i - a\_{i} c\_{i-1}\) |
| } |
| 输出\((a\_2,\dots,a\_n)\)作为\((l\_2, \dots, l\_n)\), \(\boldsymbol b\)作为\(\boldsymbol d\) |

三对角的线性方程组在LU分解后，
用两次回代法解方程也可以利用\(L\)和\(U\)的带状结构简化算法。

[22.4](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-interp.html#approx-interp-spline)中求自然样条函数的未知参数\(M\_1, M\_2, \dots, M\_n\)的线性方程组就是三对角的。

带状的正定阵的Cholesky分解的结果也是带状的，
可以简化计算。

**例29.1** 设\(\{ \varepsilon\_t, t \in \mathbb Z \}\)是随机变量序列，
其中\(\mathbb Z\)表示所有整数组成的集合。
\(E \varepsilon\_t \equiv 0\),
\(\text{Var}(\varepsilon\_t) \equiv \delta > 0\),
\(\text{Cov}(\varepsilon\_t, \varepsilon\_s)=0\)对\(s \neq t\)，
则称\(\{ \varepsilon\_t, t \in \mathbb Z \}\)是白噪声列。
若\(0 < |b| < 1\)，
\[\begin{align}
X\_t = \varepsilon\_t + b \varepsilon\_{t-1}, \ t \in \mathbb Z,
\end{align}\]
称\(\{ X\_t \}\)为MA(1)序列。
讨论其方差结构。

设各\(\varepsilon\_t\)服从N(0,\(\delta\))分布。
若有\(\{ X\_t \}\)的观测\(X\_1, X\_2, \dots, X\_n\)，
则其联合分布完全依赖于\(b, \delta\)。
令\(\boldsymbol X = (X\_1, X\_2, \dots, X\_n)^T\)，
则\(E \boldsymbol X = \boldsymbol 0\),
\(\text{Var}(\boldsymbol X) = \Sigma\),
\(\Sigma\)是一个三对角正定矩阵，
\(\sigma\_{ii} = \delta(1 + b^2)\),
\(\sigma\_{i+1,i} = \sigma\_{i,i+1} = \delta b\)。
为了求\(b, \delta\)的最大似然估计，
注意到样本\(\boldsymbol X = \boldsymbol x\)的对数似然函数为
\[\begin{align}
\ln L(b, \delta) = -\frac{n}{2} \ln(2\pi) - \frac12 \ln \det(\Sigma)
- \frac12 \boldsymbol x^T \Sigma^{-1} \boldsymbol x,
\tag{29.2}
\end{align}\]
计算似然函数涉及给定参数\((b, \delta)\)后求相应的\(\Sigma\)的行列式以及计算二次型
\(\boldsymbol x^T \Sigma^{-1} \boldsymbol x\)，
只要对\(\Sigma\)作Cholesky分解\(\Sigma = L L^T\),
\(L\)也是带状的，只有主对角线和下副对角线存在非零元素，
\(L\)满秩，这时\(\det(\Sigma) = [\det(L)]^2 = (l\_{11} l\_{22} \dots l\_{nn})^2\),
\(\boldsymbol x^T \Sigma^{-1} \boldsymbol x = (L^{-1} \boldsymbol x)^T (L^{-1} \boldsymbol x)\)，
只要用回代法解得\(\boldsymbol y = L^{-1} \boldsymbol x\)则\(\boldsymbol x^T \Sigma^{-1} \boldsymbol x = \boldsymbol y^T \boldsymbol y\)。

## 29.2 Toeplitz矩阵

在时间序列分析中，
若\(\{ \xi\_t, t \in\mathbb Z \}\)是平稳时间序列，
则\(\boldsymbol \xi = (\xi\_1, \xi\_2, \dots, \xi\_n)^T\)有协方差阵
\(\Gamma\_n = \big(\gamma(|i-j|) \big)\_{n\times n}\),
其中\(\gamma(k) = \text{Cov}(\xi\_1, \xi\_{k+1})\)叫做\(\{ \xi\_t \}\)的协方差函数。
这样形式的矩阵叫做Toeplizt矩阵，形式为
\[\begin{align}
\Gamma\_n = \left(\begin{array}{\*4c}
\gamma(0) & \gamma(1) & \cdots & \gamma(n-1) \\
\gamma(1) & \gamma(0) & \cdots & \gamma(n-2) \\
\vdots & \vdots & \ddots & \vdots \\
\gamma(n-1) & \gamma(n-2) & \cdots & \gamma(0)
\end{array}\right)
\end{align}\]

在很一般的条件下\(\Gamma\_n\)正定。在本小节中假定\(\Gamma\_n\)正定。
如果要求解如下形式的方程，
\[\begin{align}
\Gamma\_n \boldsymbol a^{(n)} = \boldsymbol\gamma^{(n)},
\tag{29.3}
\end{align}\]
其中\(\boldsymbol a^{(n)} = (a\_{n1}, a\_{n2}, \dots, a\_{nn})^T\),
\(\boldsymbol\gamma^{(n)} = (\gamma(1), \gamma(2), \dots, \gamma(n))^T\),
可以用一个Levinson递推算法求解，
只需要\(O(n^2)\)次运算(见([何书元 2003](#ref-He-TSA03)) 2.4.3小节)。
\(\boldsymbol a^{(n)}\)叫做时间序列\(\{ \xi\_t \}\)的\(n\)阶Yule-Walker系数，
可用在AR建模或最优线性预报中。

在另外一些问题中可能需要对一般的\(\boldsymbol y^{(n)}\)求解
\[\begin{align}
\Gamma\_n \boldsymbol x^{(n)} = \boldsymbol y^{(n)},
\tag{29.4}
\end{align}\]
其中\(\boldsymbol x^{(n)} = (x\_{n1}, x\_{n2}， \dots, x\_{nn})^T\),
\(\boldsymbol y^{(n)} = (y\_1, y\_2, \dots, y\_n)^T\),
比如计算\(\boldsymbol \xi\)的似然函数时。
[(29.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-yw)是[(29.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-ywg)的特例。
下面利用Toeplitz矩阵的特殊结构构造[(29.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-ywg)的高效算法。

首先考虑Y-W方程[(29.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-yw)的求解。
令\(P\_n\)表示把\((1,2,\dots,n)\)排列为\((n,n-1,\dots,1)\)的置换阵，
则\(P\_n P\_n = I\_n\), \(P\_n \Gamma\_n^{-1} = \Gamma\_n^{-1} P\_n\),
\(P\_n \Gamma\_n^{-1} P\_n = \Gamma\_n^{-1}\)。
记\(\boldsymbol a\_\*^{(n+1)} = (a\_{n+1,1}, a\_{n+1,2}, \dots, a\_{n+1,n})^T\),
则\(n+1\)阶的Y-W方程可以写成如下分块形式
\[\begin{align}
\left(\begin{array}{cc}
\Gamma\_n & P\_n \boldsymbol\gamma^{(n)} \\
\boldsymbol\gamma^{(n)T} P\_n & \gamma(0)
\end{array}\right)
\left(\begin{array}{c}
\boldsymbol a\_\*^{(n+1)} \\ a\_{n+1,n+1}
\end{array}\right)
=
\left(\begin{array}{c}
\boldsymbol\gamma^{(n)} \\ \gamma(n+1)
\end{array}\right)
\end{align}\]
利用矩阵消元的想法把上面的第\(n+1\)个方程中的\(\boldsymbol\gamma^{(n)T} P\_n\)变成零，
只要用\(-\boldsymbol\gamma^{(n)T} P\_n \Gamma\_n^{-1}\)左乘前\(n\)个方程然后加到第\(n+1\)个方程即可，
这时有
\[\begin{align}
\Gamma\_n a\_\*^{(n+1)} + a\_{n+1,n+1} P\_n \gamma^{(n)} =& \boldsymbol\gamma^{(n)}, \\
\left(\gamma(0) - \boldsymbol\gamma^{(n)T} P\_n \Gamma\_n^{-1} P\_n \boldsymbol\gamma^{(n)} \right) a\_{n+1,n+1}
=& \gamma(n+1) - \boldsymbol\gamma^{(n)T} P\_n \Gamma\_n^{-1} \boldsymbol\gamma^{(n)},
\end{align}\]
于是
\[\begin{align}
a\_{n+1,n+1} =& \frac{\gamma(n+1) - \boldsymbol\gamma^{(n)T} P\_n \boldsymbol a^{(n)}}
{\gamma(0) - \boldsymbol\gamma^{(n)T} \boldsymbol a^{(n)}}
= \frac{\gamma(n+1) - a\_{n1} \gamma(n) - \dots - a\_{nn} \gamma(1)}
{\gamma(0) - a\_{n1} \gamma(1) - \dots - a\_{nn} \gamma(n)}, \\
\boldsymbol a\_\*^{(n+1)} =& \boldsymbol a^{(n)} - a\_{n+1,n+1} P\_n \boldsymbol a^{(n)}.
\end{align}\]

为求解[(29.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-ywg)，类似对\(n+1\)阶的方程进行分块消元，分块形式为
\[\begin{align}
\left(\begin{array}{cc}
\Gamma\_n & P\_n \boldsymbol\gamma^{(n)} \\
\boldsymbol\gamma^{(n)T} P\_n & \gamma(0)
\end{array}\right)
\left(\begin{array}{c}
\boldsymbol x\_\*^{(n+1)} \\ x\_{n+1,n+1}
\end{array}\right)
=
\left(\begin{array}{c}
\boldsymbol y^{(n)} \\ y\_{n+1}
\end{array}\right)
\end{align}\]
其中\(\boldsymbol x\_\*^{(n+1)}\)是\(\boldsymbol x^{(n+1)}\)的前\(n\)个元素组成的列向量。
消去第\(n+1\)个方程中的\(\boldsymbol\gamma^{(n)T} P\_n\)，得
\[\begin{align}
x\_{n+1,n+1} =& \frac{y\_{n+1} - \boldsymbol\gamma^{(n)T} P\_n \boldsymbol x^{(n)}}
{ \gamma(0) - \boldsymbol\gamma^{(n)T} \boldsymbol a^{(n)}}
= \frac{y\_{n+1} - x\_{n1} \gamma(n) - \dots - x\_{nn} \gamma(1)}
{ \gamma(0) - a\_{n1} \gamma(1) - \dots - a\_{nn} \gamma(n)}, \\
\boldsymbol x\_\*^{(n+1)} =& \boldsymbol x^{(n)} - x\_{n+1,n+1} P\_n \boldsymbol a^{(n)}.
\end{align}\]
为求解\(\boldsymbol x^{(n)}\)需要同时计算\(\boldsymbol a^{(n)}\)，
总共只需要\(O(n^2)\)次运算。

## 29.3 稀疏系数矩阵方程组求解

系数矩阵为带状矩阵时解方程组计算量可以从\(O(n^3)\)降低到\(O(n)\)。
更一般的稀疏矩阵的非零元素分布不一定有固定的规律，
而且在求解消元过程中可能变得不再稀疏。
现代统计模型中经常有数千自变量的情形，
涉及的矩阵经常是稀疏矩阵，
减少不必要的存储并加速运算可以使这样的问题求解变得可行或者更加高效。
在经典的统计问题中，
有许多个因素的试验的设计阵是稀疏矩阵。

稀疏矩阵可以只保存非零元素与其所在的行列位置。
在设计存储方案时，
要考虑到如何使用此矩阵，
比如，仅用来计算\(A \boldsymbol x\)这样的矩阵和向量的乘法，
要求解线性方程组\(A \boldsymbol x = \boldsymbol b\),
矩阵是否会添加、删除元素或修改元素值，
访问矩阵时需要按行访问还是按列访问，等等。

下面假设对稀疏矩阵\(A\)主要需要计算\(\boldsymbol y \leftarrow A \boldsymbol x\)这样的矩阵和向量的乘法。
如果\(A, \boldsymbol x, \boldsymbol y\)都可以保存到内存中，不需要修改\(A\)的内容，
则可以把\(A\)的非零元素存入一个长度为\(m\)的数组\(\boldsymbol a\)(\(m\)是\(A\)的非零元素个数),
把非零元素的行号存入长度为\(m\)的数组\(\boldsymbol r\), 列号存入数组\(\boldsymbol c\)，
即\(a\_{ij} \neq 0\)保存在\(\boldsymbol a\)的第\(k\)元素中，
则\(r\_k=i\), \(c\_k=j\)。乘法\(\boldsymbol y \leftarrow A \boldsymbol x\)的伪代码为:

| 稀疏矩阵乘以向量的算法 |
| --- |
| 输入\(\boldsymbol a, \boldsymbol r, \boldsymbol c, \boldsymbol x\) |
| \(\boldsymbol y \leftarrow \boldsymbol 0\) |
| **for**(\(k\) **in** \(1:m\)) { # 计算与第\(k\)个\(A\)非零元素有关的乘法 |
| \(\qquad\) \(i \leftarrow r\_k, \ j \leftarrow c\_k\) |
| \(\qquad\) \(y\_i \leftarrow y\_i + a\_k x\_j\) |
| } |
| 输出\(\boldsymbol y\) |

以上算法稍作修改就可以适用于\(A\)的所有非零元素无法同时调入内存，
需要分批调入的情形。

如果\(A\)可能被修改，
就要设计存储方法使得能迅速定位元素。
比如，
可以把\(A\)的非零元素按行存储为链表，
然后保存每行的非零元素个数，
并把每行的非零元素所在的列号保存为链表。
这样可以在一行中迅速找到某个元素的值,
也可以删除非零元素、添加非零元素、修改元素值。

## 29.4 用迭代法求解线性方程组

用消元法求解\(A \boldsymbol x = \boldsymbol b\)需要\(O(n^3)\)次运算,
可以在有限步结束。
迭代算法每次给出解\(\boldsymbol x\)的一个近似，
下次迭代对此近似进行改进，
每次迭代仅需要\(O(n^2)\)次运算，
当达到需要的精度时停止迭代。
如果迭代很快收敛，
这种方法可以比消元法更快求解。
如果\(A\)是稀疏矩阵，
消元法会在消元过程中使矩阵变得稠密，
而迭代法则可以保持使用稀疏矩阵，
从而减少计算量。

Jacobi方法是求解线性方程组的一种迭代算法。
对线性方程组\(A \boldsymbol x = \boldsymbol b\),
首先把\(A\)分解为\(A = L + D + U\),
其中\(L\)仅有\(A\)的严格下三角部分，\(D\)为\(A\)的主对角部分，
\(U\)仅有\(A\)的严格上三角部分。
设迭代的第\(k\)步已经得到了近似解\(\boldsymbol x^{(k)}\),
则在第\(k+1\)步时对\(i=1,2,\dots,n\)计算
\[\begin{align}
x\_i^{(k+1)} =& \frac{b\_i - \sum\_{j \neq i} a\_{ij} x\_j^{(k)}}{a\_{ii}}
\tag{29.5}
\end{align}\]
写成向量形式为
\[\begin{align}
\boldsymbol x^{(k+1)} =& D^{-1} \left[ \boldsymbol b - (L + U) \boldsymbol x^{(k)} \right].
\tag{29.6}
\end{align}\]
在迭代收敛时，[(29.6)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-linmisc-jacobi2)变成
\[\begin{align}
D \boldsymbol x = \boldsymbol b - L \boldsymbol x - U \boldsymbol x
\Longleftrightarrow
(L + D + U) \boldsymbol x = \boldsymbol b
\end{align}\]
即\(A \boldsymbol x = \boldsymbol b\)。

显然，Jacobi方法要求\(A\)的主对角线元素都不等于零。
为了使得迭代收敛，
即\(\lim\_{k\to\infty} \| \boldsymbol x^{(k)} - \boldsymbol x \| = 0\),
注意到第\(k+1\)步的误差为
\[\begin{align\*}
\left( \boldsymbol x^{(k+1)} - \boldsymbol x \right)
=& -D^{-1}(L + U)\left( \boldsymbol x^{(k)} - \boldsymbol x \right)
= C^{k+1}(\boldsymbol x^{(0)} - \boldsymbol x),
\end{align\*}\]
其中\(C = D^{-1}(L + U)\)，
所以迭代收敛的充分条件为所有\(a\_{ii} \neq 0\)且\(\| C \| < 1\)，这时
\[\begin{align\*}
\| \boldsymbol x^{(k+1)} - \boldsymbol x \| \leq \| C \|^{k+1} \| \boldsymbol x^{(0)} - \boldsymbol x \|
\to 0 \quad(k \to \infty).
\end{align\*}\]
其中\(\| C \|\)是\(C\)的任意一种矩阵范数。
可以证明，
在所有\(a\_{ii} \neq 0\)时，
Jacobi算法收敛的充分必要条件为\(\| C \|\_2 < 1\)
(参见 ([关治 and 陆金甫 1998](#ref-Guan-Lu98)) §6.1)。

在某些问题中以上收敛性条件可以得到验证。
为了判断迭代是否已经收敛，一般预先指定一个精度\(\epsilon\),
当\(\| \boldsymbol x^{(k+1)} - \boldsymbol x^{(k)} \| < \epsilon\)时停止迭代。
由于舍入误差的影响，
\(\epsilon\)只要取为问题需要的精度即可，
取过小的\(\epsilon\)有可能会造成算法无法停止。

以上的Jacobi方法在第\(k+1\)步利用\(\boldsymbol x^{(k)}\)得到整个\(\boldsymbol x^{(k+1)}\)。
另外一种迭代方法叫做Gauss-Seidel迭代，
每次仅更新一个分量，而且更新后的分量值马上在下一步中就可以利用。
迭代公式为
\[\begin{align}
x\_i^{(k+1)}
=& \frac{b\_i - \sum\_{j<i} a\_{ij} x\_j^{(k+1)} - \sum\_{j>i} a\_{ij} x\_j^{(k)}}{a\_{ii}},
\tag{29.7}
\end{align}\]
写成向量形式为
\[\begin{align\*}
(L + D) \boldsymbol x^{(k+1)} = \boldsymbol b - U \boldsymbol x^{(k)}
\Longleftrightarrow
\boldsymbol x^{(k+1)} = (L + D)^{-1} (\boldsymbol b - U \boldsymbol x^{(k)} ),
\end{align\*}\]
这种方法收敛的充分必要条件为\(L+D\)可逆且\(\| (L+D)^{-1} U \|\_2 < 1\)。
特别地，当\(A\)为对称正定阵时Gauss-Seidel迭代必定收敛
(参见 ([关治 and 陆金甫 1998](#ref-Guan-Lu98)) §6.2)。

对于Gauss-Seidel方法，
一种改进的方法是\(x\_i^{(k+1)}\)取为[(29.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-js)与\(x\_i^{(k)}\)的加权平均，
这种方法称为超松弛(SOR)迭代法，
某些特殊的系数矩阵可以找到最优的加权因子使得收敛速度达到最优。

当\(A\)为对称正定阵时，
\(A \boldsymbol x = \boldsymbol b\)的解等价于
\(f(\boldsymbol x) = \frac12 \boldsymbol x^T A \boldsymbol x - \boldsymbol b^T \boldsymbol x\)的最小值点，
可以用函数最优化方法如共轭梯度法求解。

## 习题

#### 习题1

设方阵\(A\)满秩，
\(a\_{ij}=0\)对\(i>j+p\)和\(j > i+q\),
若\(A\)有\(LU\)分解\(A = LU\)，
证明当\(i > j+p\)时\(l\_{ij}=0\),
当\(j > i+q\)时\(u\_{ij}=0\)。

---

#### 习题2

证明[29.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#mat-band)关于三对角矩阵的递推算法。

---

#### 习题3

编写用LU分解方法求解三对角矩阵的算法程序。
要在程序中设置分母等于零时快速算法失败的预防措施。

---

#### 习题4

对例[29.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#exm:mat-ma1-logl),
编写输入\(b\)和\(\delta\)计算对数似然函数[(29.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-ma1-logl)的高效算法程序,
尽可能不占用额外存储空间。

---

#### 习题5

编写算法程序，
输入时间序列自协方差函数值\((\gamma(0), \gamma(1), \dots, \gamma(n))\)和
实数值\((y\_1, y\_2, \dots, y\_n)\)，
用递推算法求解方程[(29.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-yw)和[(29.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solspe.html#eq:mat-ywg),
输出\(\boldsymbol a^{(n)}\)和\(\boldsymbol x^{(n)}\)。
注意避免中间结果不必要的存储。

---

### 参考文献

何书元. 2003. *应用时间序列分析*. 北京大学出版社.

关治, and 陆金甫. 1998. *数值分析基础*. 高等教育出版社.