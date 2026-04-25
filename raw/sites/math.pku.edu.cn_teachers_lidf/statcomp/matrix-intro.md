---
crawl_time: '2026-01-17 14:24:59'
framework: sphinx
title: 27 统计计算中的矩阵计算 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-intro.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 27 统计计算中的矩阵计算

## 27.1 矩阵计算的应用例子

统计模型和统计计算中广泛使用矩阵运算和线性方程组求解。
例如，如下的线性模型
\[\begin{align}
{\boldsymbol y} = { X}\boldsymbol \beta + \boldsymbol\varepsilon
\tag{27.1}
\end{align}\]
中，
\({\boldsymbol y}\)为\(n \times 1\)向量,
\(X\)为\(n \times p\)矩阵,
一般第一列元素全是1, 代表截距项；
\(\boldsymbol\beta\)为\(p \times 1\)未知参数向量;
\(\boldsymbol\varepsilon\)为\(n \times 1\)随机误差向量,
\(\boldsymbol\varepsilon\)的元素独立且方差为相等的\(\sigma^2\)(未知)。
参数\(\boldsymbol\beta\)的最小二乘估计是如下正规方程的解：
\[\begin{align}
X^T X \boldsymbol \beta = X^T \boldsymbol y,
\tag{27.2}
\end{align}\]
当\(X\)为列满秩矩阵时\(\boldsymbol\beta\)的最小二乘估计可以表示为
\[\begin{align}
\hat{\boldsymbol\beta} = (X^T X)^{-1} X^T \boldsymbol y ,
\tag{27.3}
\end{align}\]
因变量\(\boldsymbol y\)的拟合值（预报值）可以表示为
\[\begin{aligned}
\hat{\boldsymbol y} = X (X^T X)^{-1} X^T \boldsymbol y = H \boldsymbol y,
\end{aligned}\]
其中\(H = X (X^T X)^{-1} X^T\)是对称幂等矩阵。

再比如，在时间序列分析问题中需要对多元序列建模，
常用的一种模型是向量自回归模型，
\(p\)阶向量自回归模型可以写成
\[\begin{aligned}
\boldsymbol x\_t = \sum\_{j=1}^p A\_j \boldsymbol x\_{t-j} + \boldsymbol\varepsilon\_t,
\ t \in \mathbb Z
\end{aligned}\]
其中\(\boldsymbol x\_t\)是\(m\)元随机向量，
\(A\_1, A\_2, \dots, A\_p\)是\(m\times m\)矩阵，
\(\boldsymbol\varepsilon\_t\)是\(m\)元白噪声。

可见，统计模型中广泛使用矩阵作为模型表达工具，
统计计算中有大量的矩阵计算。
我们需要研究稳定、高效的矩阵计算方法。
例如，按照[(27.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-intro.html#eq:linreg-beta-inv)计算\(\hat{\boldsymbol\beta}\)从理论上很简单，
但需要计算逆矩阵，实际计算量比较大，而且当\(X\)的各列之间有近似线性关系时算法不稳定；
如果把[(27.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-intro.html#eq:linreg-beta-eq)看成是线性方程组来求解，
或者直接考虑最小化\(\| \boldsymbol y - X \boldsymbol \beta \|^2\)的问题，
则可以找到各种快速且高精度的计算方法。

有许多编程语言有成熟的矩阵计算软件包，
比如FORTRAN和C语言的LAPACK程序包([E. and others 1999](#ref-lapack-ug))、IMSL([Inc. 1985](#ref-imsl))程序包。
在常用统计软件系统一般内建了高等矩阵计算功能，
比如R软件(见[A](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/appendix-rintro.html#appendix-rintro))、Julia(见[B](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/jintro.html#jintro))、Python的scipy模块、SAS软件中的IML模块、MATLAB软件，等等。
我们可能不需要再去自己编写矩阵乘法、解线性方程组这些基础的计算程序，
但是还是要了解这里面涉及的算法，
这样遇到高强度、高维数等复杂情形下的矩阵计算问题才能给出稳定、高效的解决方案。
对于反复使用的矩阵运算，
1%的速度提升也是难能可贵的；
对于高阶矩阵，
应该尽可能少产生中间结果矩阵，
尽可能把输入和输出保存在同一存储位置。

**例27.1** 设\(A\)为\(n\times n\)矩阵，\(\boldsymbol x\)为\(n\)维列向量，
计算矩阵乘法\(A \boldsymbol x\)需要\(n^2\)次乘法和\(n^2\)次加法。
如果\(A\)有特殊结构\(A = I\_n + \boldsymbol u \boldsymbol v^T\)，
其中\(\boldsymbol u\)和\(\boldsymbol v\)是\(n\)维列向量，则
\[\begin{aligned}
A \boldsymbol x = \boldsymbol x + (\boldsymbol v^T \boldsymbol x) \boldsymbol u
\end{aligned}\]
只需要\(2n\)次乘法和\(2n\)次加法，
并且矩阵\(A\)也不需要保存\(n^2\)个元素，
而只需\(\boldsymbol u\)和\(\boldsymbol v\)的\(2n\)个元素。
若\(A\)是上三角矩阵或下三角矩阵，
则\(A \boldsymbol x\)只需要\(\frac12 n(n+1)\)次乘法和加法，
计算量比一般的\(A\)减少一半。

在R语言中，
用`matrix`函数定义一个矩阵，
用`cbind`和`rbind`进行横向和纵向合并，
用`t(A)`表示\(A\)的转置，
用`A %*% B`表示矩阵\(A\)和\(B\)相乘。
R语言的矩阵是多维数组中的二维数组。

Julia语言也支持多维数组，
矩阵就是二维数组。

## 27.2 矩阵记号与特殊矩阵

在本书中我们用黑体小写字母表示向量，
且缺省为列向量，
如\(\boldsymbol a\), \(\boldsymbol v\)，
用\(a\_i\)表示\(\boldsymbol a\)的第\(i\)元素。
用\(\mathbb R^n\)表示所有\(n\)维实值向量组成的\(n\)维欧式空间,
用\((\boldsymbol a, \boldsymbol b)\)表示\(\boldsymbol a^T \boldsymbol b\)，
称为\(\boldsymbol a, \boldsymbol b\)的内积，
记\(\| \boldsymbol a \| = (\boldsymbol a, \boldsymbol a)^{1/2}\)，
称为\(\boldsymbol a\)的欧式模。
用大写字母表示矩阵，如\(A\), \(M\)，
用\(a\_{ij}\)表示\(A\)的第\(i\)行第\(j\)列元素，
用\(\boldsymbol a\_{\cdot j}\)表示\(A\)的第\(j\)列组成的列向量，
用\(\boldsymbol a\_{i \cdot}\)表示\(A\)的第\(i\)行组成的行向量。
用\(A^T\)表示\(A\)的转置,
\(\det(A)\)表示\(A\)的行列式。
另外，一些特殊的矩阵定义如下：

* 设\(\boldsymbol e\_i\)为\(n\)维列向量，如果其第\(i\)个元素为1，其它元素为0，
  称\(\boldsymbol e\_i\)为\(n\)维**单位向量**。
* 记\(\boldsymbol 1\)为元素都是1的列向量。
* 用\(I\_n\)表示\(n\)阶单位阵，用\(I\)表示单位阵。
* 若矩阵\(A\)的元素满足\(a\_{ij}=0, \forall i<j\)，
  称\(A\)为**下三角矩阵**。
* 若矩阵\(A\)的元素满足\(a\_{ij}=0, \forall i>j\)，
  称\(A\)为**上三角矩阵**。
* 若矩阵\(A\)的元素满足\(a\_{ij}=0, \forall i \neq j\)，
  称\(A\)为**对角矩阵**。
* 若矩阵\(A\)的元素满足\(a\_{ij}=0, \forall |i - j| > 1\)，
  称\(A\)为**三对角矩阵**。
* 若实方阵\(A\)满足\(A^T A = I\)，则称\(A\)为**正交阵**。
* 若\(A\)为\(n\)阶实对称矩阵，对任意\(n\)维非零实数向量\(\boldsymbol x\)有
  \(\boldsymbol x^T A \boldsymbol x > 0\)，
  称\(A\)为**正定阵**。
* 若\(A\)为\(n\)阶实数对称矩阵，
  对任意\(n\)维实数向量\(\boldsymbol x\)有
  \(\boldsymbol x^T A \boldsymbol x \geq 0\)，
  称\(A\)为**非负定阵**或**半正定阵**。
* 若\(P(i,j)\)是把\(I\_n\)的第\(i\)行和第\(j\)行交换位置后得到的\(n\)阶方阵，
  称\(P(i,j)\)为**基本置换阵**。
  \(P(i,j) A\)把\(A\)的第\(i\)、\(j\)两行互换,
  \(A P(i,j)\)把\(A\)的第\(i\)、\(j\)两列互换。
  \(P(i,j) P(i,j) = I\_n\)。
* 设\(\boldsymbol \pi=(k\_1, k\_2, \dots, k\_n)^T\)
  是由\((1,2,\dots,n)\)的一个排列组成的\(n\)维向量，
  方阵\(P=(\boldsymbol e\_{k\_1}, \boldsymbol e\_{k\_2}, \dots, \boldsymbol e\_{k\_n})\),
  称\(P\)为**置换阵**。
  \(P\)是一个正交阵，
  对任意\(m \times n\)矩阵\(A\),
  \(A P\)是把\(A\)的各列按照\(k\_1, k\_2, \dots, k\_n\)次序重新排列得到的矩阵。
* 设\(A\)为\(n \times m\)矩阵(\(n \geq m\))，
  称\(\mu(A) = \{ \boldsymbol y \in \mathbb R^n: \boldsymbol y = A \boldsymbol x, \boldsymbol x \in \mathbb R^m \}\)
  为由\(A\)的列向量张成的线性子空间。
* 设\(n\)阶实对称矩阵\(P\)满足\(P^2=P\)，称\(P\)为**对称幂等矩阵**，
  \(P\)是\(\mathbb R^n\)到\(\mu(P)\)的**(正交)投影矩阵**，
  对任意\(\boldsymbol x \in \mathbb R^n\), \(P \boldsymbol x\)与\(\boldsymbol x - P \boldsymbol x = (I - P)\boldsymbol x\)正交。
  若\(A\)为\(n \times m\)列满秩矩阵,
  则\(P = A (A^T A)^{-1} A^T\)是\(\mathbb R^n\)到\(\mu(A)\)的正交投影矩阵。

**例27.2** 考虑如下置换：

\[\begin{align}
\left(
\begin{matrix}
1 & 2 & 3 & 4 \\
4 & 3 & 1 & 2
\end{matrix}
\right)
\tag{27.4}
\end{align}\]

表示将1换成4，
2换成3，
3换成1，
4换成2。

对应的置换阵，用Julia表示:

```
P = eye(4)[:, [4,3,1,2]]
P * [1;2;3;4]

## 4-element Array{Float64,1}:
##  3.0
##  4.0
##  2.0
##  1.0
```

用\(P\)左乘一个列向量，
将第1行换到了第4行，
第2行换到了第3行，
第3行换到了第1行，
第4行换到了第2行。

以上的置换变换的逆变换是将[(27.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-intro.html#eq:matrix-intro-perm-exa1)的两行调换，
然后按照第一行排序：

\[\begin{align}
\left(
\begin{matrix}
1 & 2 & 3 & 4 \\
3 & 4 & 2 & 1
\end{matrix}
\right)
\tag{27.5}
\end{align}\]

```
Prev = eye(4)[:, [3,4,2,1]]
Prev * [3;4;2;1]

## 4-element Array{Float64,1}:
##  1.0
##  2.0
##  3.0
##  4.0
```

实际上，\(P\)是正交阵，所以\(P^{-1}=P^T\)：

```
maximum(Prev - P')
## 0.0
```

## 27.3 矩阵微分

对\(f : \mathbb R^p \rightarrow \mathbb R\),
记\(\nabla f(\boldsymbol x)\)
为\(f\)的\(p\)个一阶偏导数组成的列向量，
称为\(f\)的梯度。
记\(\nabla^2 f(\boldsymbol x)\)为\(f\)的二阶偏导数组成的\(p \times p\)矩阵，
称为\(f\)的海色阵(Hessian)。
\(\nabla^2 f(\boldsymbol x)\)的第\(i\)行第\(j\)列元素为
\(\frac{\partial^2 f(\boldsymbol x)}{\partial x\_i \partial x\_j}\)。

设\(\boldsymbol a\)为\(p\)维列向量，\(A\)为\(p \times p\)对称阵，
则
\[\begin{aligned}
& \nabla (\boldsymbol a^T \boldsymbol x ) = \boldsymbol a,
\quad \nabla (\boldsymbol x^T \boldsymbol a ) = \boldsymbol a, \\
& \nabla (\boldsymbol x^T A \boldsymbol x) = 2 A \boldsymbol x, \quad
\nabla^2 (\boldsymbol x^T A \boldsymbol x) = 2 A .
\end{aligned}\]

如果\(A\)是\(p\times p\)矩阵，则
\[\begin{align\*}
& \nabla (\boldsymbol x^T A \boldsymbol x)
= (A + A^T) \boldsymbol x, \quad
\nabla^2 (\boldsymbol x^T A \boldsymbol x)
= A + A^T
\end{align\*}\]

对\(f : \mathbb R^p \rightarrow \mathbb R^q\),
设\(\boldsymbol y = f(\boldsymbol x)\),
记\(q \times p\)个一阶偏导数\(\frac{\partial y\_i}{\partial x\_j}\)组成的\(q \times p\)矩阵为
\(\frac{\partial f(\boldsymbol x)}{\partial \boldsymbol x}\)，
称为\(f\)的Jacobi矩阵，
当\(q=1\)时，
Jacobi矩阵是行向量，
是梯度向量（列向量）的转置。

若\(\boldsymbol y = f(\boldsymbol x) = A \boldsymbol x\),
其中\(A\)是\(q \times p\)矩阵，
则\(\frac{\partial f(\boldsymbol x)}{\partial \boldsymbol x} = A\)。

对\(f : \mathbb R^{n \times m} \to \mathbb R\)，
设\(y = f(A)\),
其中\(A\)是\(n \times m\)矩阵，
定义\(\frac{\partial f(A)}{\partial A}\)
为矩阵\(\left(\frac{\partial y}{\partial a\_{ij}} \right)\_{n \times m}\)。
若\(f(A) = \boldsymbol a^T A \boldsymbol b\)，
其中\(\boldsymbol a\)和\(\boldsymbol b\)是列向量，
则\(\frac{\partial f(A)}{\partial A} = \boldsymbol a \boldsymbol b^T\)。

## 27.4 矩阵期望

这里列出概率论中关于随机向量和随机矩阵的几个基本公式。
设\(\boldsymbol X\)为\(n\)元随机向量，\(\boldsymbol Y\)为\(m\)随机向量，
\(\boldsymbol M\)为\(m \times n\)随机矩阵，
\(A, B, C\)为普通非随机的实数矩阵。
\(\boldsymbol a, \boldsymbol b\)为非随机的向量。
\(E\boldsymbol X\)是\(\boldsymbol X\)的各个分量的期望组成的列向量，
\(\text{Var}(\boldsymbol X)\)表示\(\boldsymbol X\)的协方差阵，
其\((i,j)\)元素为\(X\_i\)和\(X\_j\)的协方差，当\(i=j\)时为\(X\_i\)的方差。
\(\text{Cov}(\boldsymbol X, \boldsymbol Y)\)是一个\(n \times m\)矩阵，
其\((i,j)\)元素为\(\text{Cov}(X\_i, Y\_j)\)。
有如下常用公式:
\[\begin{aligned}
E(A \boldsymbol M B + C) =& A E(\boldsymbol M) B + C\\
\text{Var}(\boldsymbol X) =& E\left[ (\boldsymbol X - E \boldsymbol X) (\boldsymbol X - E \boldsymbol X)^T \right] \\
\text{Var}(A \boldsymbol X + \boldsymbol a) =& A \text{Var}(\boldsymbol X) A^T \\
\text{Cov}(\boldsymbol X, \boldsymbol Y)
=& E \left[(\boldsymbol X - E\boldsymbol X)
(\boldsymbol Y - E\boldsymbol Y)^T \right] \\
\text{Cov}(A \boldsymbol X + \boldsymbol a,
B \boldsymbol Y + \boldsymbol b)
=& A \text{Cov}(\boldsymbol X, \boldsymbol Y) B^T .
\end{aligned}\]

## 习题

#### 习题1

设\(A\)为\(n\times m\)矩阵，
\(\boldsymbol x\)为\(m\)维向量，
为了计算\(A \boldsymbol x\)，
可以逐个计算结果的\(n\)个元素，
也可以把这个乘法看成是对\(A\)的各列用\(\boldsymbol x\)作为线性组合系数作线性组合，
逐次把\(A\)的第\(j\)列与\(x\_j\)相乘累加到结果向量中。
写出这两种算法，尽可能减少不必要的存储。
计算这两种算法所需的乘法和加法的次数。
如果使用C语言或FORTRAN语言来实现这两种算法，
在\(n, m\)很大时，两种算法会有不同的计算效率，
设法验证。

### 参考文献

E., Anderson., and ten others. 1999. “LAPACK Users’ Guide.” SIAM. 1999. <http://www.netlib.org/lapack/lug/lapack_lug.html>.

Inc., IMSL. 1985. “IMSL Quality Mathematical and Statistical Fortran Subroutines for Ibm Personal Computers.” *IEEE Micro* 5 (2): 97.