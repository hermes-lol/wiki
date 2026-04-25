---
crawl_time: '2026-01-17 14:29:17'
framework: sphinx
title: 22 最佳线性预测的基本性质 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 22 最佳线性预测的基本性质

对于时间序列进行统计分析的主要目的之一是解决时间序列的预测问题。
任何时间序列\(\{X\_t\}\)都可以按
\[
X\_t = T\_t + S\_t + R\_t
\]
分解成趋势项\(\{T\_t\}\)、季节项\(\{S\_t\}\) 和随机项\(\{R\_t\}\)的和。
趋势项和季节项都可以被当做非随机的时间序列处理,
它们的预测问题往往是简单的。
随机项\(\{R\_t\}\)一般是平稳序列。

于是,时间序列预测问题的重点应当是平稳序列。
本章主要讨论平稳序列的预测问题。
平稳序列的方差有限, 所以我们总是假设本章中的随机变量的方差有限。
由于平稳序列总是零均值平稳序列加上一个常数,
所以我们主要讨论零均值平稳序列的预测问题。

## 22.1 最佳线性预测

**定义22.1** 设 \(Y\) 和\(\boldsymbol{X} = (X\_1,\dots,X\_n)^T\)是均值为零, 方差有限的随机变量(向量).
如果\(\boldsymbol{a}\in {\mathbb R}^n\)使得对任何的\(\boldsymbol{b}\in {\mathbb R}^n\), 有
\[
E(Y-\boldsymbol{a}^{T}\boldsymbol{X})^2 \leq E(Y-\boldsymbol{b}^{T}\boldsymbol{X})^2.
\]
则称\(\boldsymbol{a}^{T}\boldsymbol{X}\)是用\(X\_1,X\_2,\dots,X\_n\)对\(Y\)进行预测的**最佳线性预测**,
记做\(L(Y|\boldsymbol{X})\)或\(\hat{Y}\). 于是
\[\begin{align}
\hat Y = L(Y|\boldsymbol{X})=\boldsymbol{a}^{T}\boldsymbol{X}.
\tag{22.1}
\end{align}\]

**定义22.2** 如果\(E Y=b,E\boldsymbol{X}=\boldsymbol{\mu}\), 定义
\[\begin{align}
L(Y|\boldsymbol{X})= L(Y-b|\boldsymbol{X}-\boldsymbol{\mu})+b,
\tag{22.2}
\end{align}\]
并称\(L(Y|\boldsymbol{X})\)
是用\(X\_1,X\_2,\cdots,X\_n\)对\(Y\)进行预测时的最佳线性预测.

以下总设随机变量均值为零。
用\(\Gamma = E(\boldsymbol{X} \boldsymbol{X}^T)\)表示\(\boldsymbol{X}\)的协方差阵。
用\(\Sigma\_{\boldsymbol{X}Y} = E(\boldsymbol{X} Y)\)表示\(\boldsymbol{X}\)和\(Y\)的协方差向量。

### 22.1.1 性质1

如果\(\boldsymbol{a}\in {\mathbb R}^n\), 使得
\[\begin{align}
\Gamma\boldsymbol{a}=\Sigma\_{\boldsymbol{X}Y},
\tag{22.3}
\end{align}\]
则
\[
L(Y|\boldsymbol{X})=\boldsymbol{a}^{T}\boldsymbol{X},
\]
并且有  
\[\begin{align}
E(Y-L(Y|\boldsymbol{X}))^2
= EY^2 - E[L(Y|\boldsymbol{X})]^2
= EY^2 -\boldsymbol{a}^T \Gamma\boldsymbol{a} .
\tag{22.4}
\end{align}\]

如果\(\Gamma\)和\(\Sigma\_{\boldsymbol{X}Y}\)已知,
以\(\boldsymbol{a}\) 为未知数的线性方程组[(22.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpgammeq0105)被称为**预测方程**.

**证明：**
对任何\(\boldsymbol{b}\in {\mathbb R}^n\),
\[\begin{aligned}
&E(Y-\boldsymbol{b}^{T} \boldsymbol{X})^2\\
=& E[Y-\boldsymbol{a}^{T}\boldsymbol{X}
+(\boldsymbol{a}^{T}-\boldsymbol{b}^{T})\boldsymbol{X}]^2 \\
=& E(Y-\boldsymbol{a}^{T}\boldsymbol{X})^2
+ E[(\boldsymbol{a}^{T}-\boldsymbol{b}^{T})\boldsymbol{X}]^2
+ 2E[(\boldsymbol{a}^{T}-\boldsymbol{b}^{T})\boldsymbol{X}(Y-\boldsymbol{a}^{T}\boldsymbol{X})] \\
=& E(Y-\boldsymbol{a}^{T}\boldsymbol{X})^2
+ E[(\boldsymbol{a}^{T}-\boldsymbol{b}^{T})\boldsymbol{X}]^2
+ 2(\boldsymbol{a}^{T}-\boldsymbol{b}^{T})[E(\boldsymbol{X}Y)- E(\boldsymbol{X}\boldsymbol{X}^{T})\boldsymbol{a}] \\
=& E(Y-\boldsymbol{a}^{T}\boldsymbol{X})^2+E[(\boldsymbol{a}^{T}-\boldsymbol{b}^{T})\boldsymbol{X}]^2 \\
\geq & E(Y-\boldsymbol{a}^{T}\boldsymbol{X})^2.
\end{aligned}\]
所以, \(\boldsymbol{a}^{T}\boldsymbol{X}\)是\(Y\)的最佳线性预测.
利用[(22.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpgammeq0105)得到
\[\begin{aligned}
&E[Y-L(Y|\boldsymbol{X})]^2 =E(Y-\boldsymbol{a}^T \boldsymbol{X})^2\\
=& EY^2 + \boldsymbol{a}^T E(\boldsymbol{X} \boldsymbol{X}^T) \boldsymbol{a} - 2 \boldsymbol{a}^T E(\boldsymbol{X} Y) \\
=& EY^2 + \boldsymbol{a}^T \Gamma \boldsymbol{a} - 2 \boldsymbol{a}^T \Gamma \boldsymbol{a} \\
=&EY^2 - \boldsymbol{a}^T \Gamma \boldsymbol{a}.
\end{aligned}\]

注意：\(\boldsymbol{a}\)是预测方程的解等价于
\[\begin{aligned}
E((Y - \boldsymbol{a}^T \boldsymbol{X}) \boldsymbol{X})
= \Sigma\_{\boldsymbol{X}Y} - \Gamma \boldsymbol a = 0
\end{aligned}\]
即\(Y-\boldsymbol{a}^T \boldsymbol{X}\)与\(\boldsymbol{X}\)正交。
(注意\(\boldsymbol{a}^T \boldsymbol{X}\)是标量)

性质说明\(Y-\boldsymbol{a}^T \boldsymbol{X}\)与\(\boldsymbol{X}\)正交则
\(\boldsymbol{a}^T \boldsymbol{X}=L(Y|\boldsymbol{X})\)。

### 22.1.2 性质2

* (1) 如果\(\Gamma=E(\boldsymbol{X}\boldsymbol{X}^T)\)可逆,
  则\(\boldsymbol{a}={\Gamma}^{-1}E(\boldsymbol{X}Y)\)
  使得\(L(Y|\boldsymbol{X})=\boldsymbol{a}^T \boldsymbol{X}\)。
* (2) 预测方程 \(\Gamma\boldsymbol{a}=E(\boldsymbol{X}Y)\)总有解.
* (3) 如果\(\det(\Gamma)=0\)， 取正交矩阵\(A\)使得
  \[\begin{aligned}
  A \Gamma A^{T}
  = \text{diag}(\lambda\_1,\lambda\_2,\cdots,\lambda\_r,0,\dots,0), \quad
  \lambda\_j > 0,j=1,\dots,r.
  \end{aligned}\]
  定义\(\boldsymbol{Z}=A\boldsymbol{X}=(Z\_1,Z\_2,\dots,Z\_n)^{T}=(Z\_1,Z\_2,\dots,Z\_r,0,\dots,0)^{T}\)
  和\(\boldsymbol{\xi}=(Z\_1,Z\_2,\) \(\cdots,Z\_r)^{T}\),则
  \(E(\boldsymbol{\xi}\boldsymbol{\xi}^{T})\)正定,
  并且当取
  \[\begin{align}
  \boldsymbol{\alpha}=[E(\boldsymbol{\xi}\boldsymbol{\xi}^{T})]^{-1}E(\boldsymbol{\xi}Y)
  \tag{22.5}
  \end{align}\]
  时,
  \(L(Y|\boldsymbol X)=L(Y|\boldsymbol{\xi})=\boldsymbol{\alpha}^{T}\boldsymbol{\xi}\).

性质2的第二条说明最佳线性预测总存在，
而且总可以由预测方程的解表示。

性质2的第三条说明当第一条不成立时，
\(L(Y|\boldsymbol{X})\)可以通过\(\boldsymbol{X}\)
的基表示。

**证明**:
仅需证明\(\text{det}(\Gamma)=0\)时第三和第二条成立。

\[\begin{aligned}
E(\boldsymbol{Z}\boldsymbol{Z}^T)
=& E(A \boldsymbol{X}\boldsymbol{X}^T A^T)
= A \Gamma A^T \\
=& \text{diag}(\lambda\_1, \lambda\_2, \dots, \lambda\_r, 0, \dots, 0)
\end{aligned}\]
故\(Z\_{r+1}=\dots=Z\_{n}=0\)。
且\(E (\boldsymbol{\xi}\boldsymbol{\xi}^T) = \text{diag}(\lambda\_1,\dots,\lambda\_r)\)
是正定阵。当\(\boldsymbol{\alpha}\)按[(22.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpgammeqinv0107)定义时,有
\[
\left(
\begin{array}{lll}
\lambda\_1 & \cdots & 0 \\
& \ddots & \\
0 & \cdots & \lambda\_r
\end{array}
\right)
\boldsymbol{\alpha}=E(\boldsymbol{\xi}Y).
\]

注意\(A \Gamma A^T\)与\(\lambda\_1,\dots,\lambda\_r\)的关系，可以导出
\[\begin{align\*}
A \Gamma A^T \left(
\begin{array}{c} \boldsymbol{\alpha} \\ \boldsymbol{0} \end{array} \right)
=& \text{diag}(\lambda\_1,\dots,\lambda\_r, 0,\dots,0) \left(
\begin{array}{c} \boldsymbol{\alpha} \\ \boldsymbol{0} \end{array} \right) \\
=& \left(
\begin{array}{c} E(\boldsymbol{\xi}Y) \\ \boldsymbol{0} \end{array} \right)
= E\left( \left(
\begin{array}{c} \boldsymbol{\xi} \\ \boldsymbol{0} \end{array}\right) Y \right) \\
=& E(\boldsymbol{Z}Y) = E(A \boldsymbol{X} Y) = A E(\boldsymbol{X}Y)
= A \Sigma\_{\boldsymbol{X}Y}
\end{align\*}\]
两边同乘以\(A^T\),
记\(\boldsymbol{a} = A^T \left(\begin{array}{c} \boldsymbol{\alpha}\\ \boldsymbol{0} \end{array}\right)\)
有
\[\begin{aligned}
\Gamma \boldsymbol{a} = \Sigma\_{\boldsymbol{X}Y}
\end{aligned}\]
由性质1知
\[\begin{aligned}
L(Y|\boldsymbol{X}) = \boldsymbol{a}^T \boldsymbol{X} = (\boldsymbol{\alpha}^T, 0, \dots, 0) A \boldsymbol{X}
= \boldsymbol{\alpha}^T \boldsymbol{\xi}
\end{aligned}\]
这在证明了第三条的同时也证明了第二条。

### 22.1.3 性质3

尽管\(\boldsymbol{a}\)由\(\Gamma\boldsymbol{a}=E(\boldsymbol{X}Y)\)决定时可以不惟一,
但\(L(Y|\boldsymbol{X})\)总是(a.s.)惟一的.

**证明**:
预测方程总有解，
设\(\boldsymbol{a}\)为预测方程的一个解，
则由性质1对\(\forall \boldsymbol{b} \in \mathbb R^n\)有
\[\begin{aligned}
E(Y - \boldsymbol{b}^T \boldsymbol{X})^2
= E(Y - \boldsymbol{a}^T \boldsymbol{X})^2 + E((\boldsymbol{a}-\boldsymbol{b})^T \boldsymbol{X})^2
\end{aligned}\]
若还有\(\boldsymbol{b}\)使得\(L(Y|\boldsymbol{X}) = \boldsymbol{b}^T \boldsymbol{X}\)则
\(E(Y - \boldsymbol{b}^T \boldsymbol{X})^2 = E(Y - \boldsymbol{a}^T \boldsymbol{X})^2\)，
于是\(E((\boldsymbol{a}-\boldsymbol{b})^T \boldsymbol{X})^2 = 0\),
即\(\boldsymbol{b}^T \boldsymbol{X} = \boldsymbol{a}^T \boldsymbol{X}\)，a.s.

### 22.1.4 性质4

* (1) 如果\(E(\boldsymbol{X}Y)=0\)则\(L(Y|\boldsymbol{X})= 0\).
* (2) 如果\(Y=\sum\_{j=1}^nb\_jX\_j\)则\(L(Y|\boldsymbol{X})=Y\).

这是线性预测的两个极端：

* 因变量和自变量不相关时线性预测无效；
* 因变量为自变量线性组合时可以完全线性预测。

**证明**：

(1) \(\forall \boldsymbol{b}\)
\[\begin{aligned}
E(Y - \boldsymbol{b}^T \boldsymbol{X})^2
=& E Y^2 + \boldsymbol{b}^T \Gamma \boldsymbol{b}
- 2 \boldsymbol{b}^T E(\boldsymbol{X}Y) \\
=& E Y^2 + \boldsymbol{b}^T \Gamma \boldsymbol{b}
\geq E Y^2 = E(Y - 0)^2
\end{aligned}\]
所以\(L(Y|X) = 0\)。

(2) 这时\(Y = \boldsymbol{b}^T \boldsymbol{X}\),
\(E(Y - \boldsymbol{b}\boldsymbol{X})^2 = 0\)，
所以\(L(Y|\boldsymbol{X})= \boldsymbol{b}^T \boldsymbol{X} = Y\)。

### 22.1.5 性质5

设\(Y\_1,Y\_2,\cdots,Y\_m\)是随机变量,
\(b\_j\)是常数.
如果 \(Y=\sum\_{i=1}^m b\_i Y\_i\), 则
\[
L(Y|\boldsymbol{X})=\sum\_{j=1}^m b\_j L(Y\_j|\boldsymbol{X}).
\]

性质5说明求最佳线性预测的运算\(L(\cdot |\boldsymbol X)\)是一种线性运算.

**证明**:
设\(\boldsymbol{a}\_i\)为\(\Gamma \boldsymbol{a}\_i = E(\boldsymbol{X} Y\_i)\)的解
(\(i=1,2,\dots,m\))，
则\(L(Y\_i | \boldsymbol{X}) = \boldsymbol{a}\_i^T \boldsymbol{X}\)。
取\(\boldsymbol{a} = \sum\_{i=1}^m b\_i \boldsymbol{a}\_i\)，
则
\[\begin{aligned}
\Gamma \boldsymbol{a} =& \Gamma \left( \sum\_{i=1}^m b\_i \boldsymbol{a}\_i \right)
= \sum\_{i=1}^m b\_i (\Gamma \boldsymbol{a}\_i)
= \sum\_{i=1}^m b\_i E(\boldsymbol{X} Y\_i) \\
=& E\left(\boldsymbol{X} \sum\_{i=1}^m b\_i Y\_i \right)
= E(\boldsymbol{X} Y)
\end{aligned}\]
由性质1即知
\[\begin{aligned}
L(Y|\boldsymbol{X}) =& \boldsymbol{a}^T \boldsymbol{X} = \sum\_{i=1}^m b\_i \boldsymbol{a}\_i^T \boldsymbol{X}
= \sum\_{i=1}^m b\_i L(Y\_i | \boldsymbol{X})
\end{aligned}\]

### 22.1.6 性质6

设\(\boldsymbol{X}=(X\_1,X\_2,\cdots,X\_n)^{T}\),
\(\boldsymbol{Z}=(Z\_1,Z\_2,\cdots,Z\_m)^{T}\).
如果 \(E(\boldsymbol{X}\boldsymbol{Z}^{T})=0\)(\(\boldsymbol{X}\)与\(\boldsymbol{Z}\)不相关), 则有
\[
L(Y|\boldsymbol{X},\boldsymbol{Z})=L(Y|\boldsymbol{X})+L(Y|\boldsymbol{Z}).
\]
**证明**:
记\(\boldsymbol{\xi} = \left(\begin{array}{c} \boldsymbol{X} \\ \boldsymbol{Z} \end{array} \right)\)，
记\(\Sigma\_{XX}=E(\boldsymbol{X}\boldsymbol{X}^T)\), \(\Sigma\_{ZZ}=E(\boldsymbol{Z}\boldsymbol{Z}^T)\),
则
\[\begin{aligned}
\Sigma \stackrel{\triangle}{=}& E(\boldsymbol{\xi} \boldsymbol{\xi}^T)
= \left( \begin{array}{cc}
\Sigma\_{XX} & \boldsymbol{0} \\
\boldsymbol{0} & \Sigma\_{ZZ}
\end{array}\right)
\end{aligned}\]
设\(\boldsymbol{a}\), \(\boldsymbol{b}\)使得
\(\Sigma\_{XX} \boldsymbol{a} = E(\boldsymbol{X} Y)\),
\(\Sigma\_{ZZ} \boldsymbol{b} = E(\boldsymbol{Z} Y)\),
则\(L(Y|\boldsymbol{X})=\boldsymbol{a}^T \boldsymbol X\)，\(L(Y|\boldsymbol{Z})=\boldsymbol{b}^T \boldsymbol Z\),
取\(\boldsymbol{c} = \left(\begin{array}{c} \boldsymbol{a} \\ \boldsymbol{b} \end{array} \right)\)，
则
\[\begin{aligned}
\Sigma \boldsymbol{c} =&
\left(\begin{array}{c} \Sigma\_{XX} \boldsymbol{a} \\ \Sigma\_{ZZ}\boldsymbol{b} \end{array} \right)
= \left(\begin{array}{c} E(\boldsymbol{X}Y) \\ E(\boldsymbol{Z}Y) \end{array} \right)\\
=& E \left(
\left(\begin{array}{c} \boldsymbol{X} \\ \boldsymbol{Z} \end{array} \right) Y\right)
= E (\boldsymbol{\xi} Y)
\end{aligned}\]
由性质1
\[\begin{aligned}
L(Y | \boldsymbol{X},\boldsymbol{Z}) =& L(Y | \boldsymbol{\xi}) = \boldsymbol{c}^T \boldsymbol{\xi} \\
=& \boldsymbol{a}^T \boldsymbol{X} + \boldsymbol{b}^T \boldsymbol{Z}
= L(Y|\boldsymbol{X}) + L(Y|\boldsymbol{Z})
\end{aligned}\]

### 22.1.7 性质7

设\(\tilde Y=\boldsymbol{b}^{T}\boldsymbol{X}\)是\(\boldsymbol X\)的线性组合, 则
\(\tilde Y=L(Y|\boldsymbol X)\)的充分必要条件是
\[\begin{align\*}
E(X\_j (Y-\tilde Y))=0, \ 1\leq j \leq n. \tag{1.8}
\end{align\*}\]
即
\[\begin{align\*}
E(\boldsymbol{X}(Y - \boldsymbol{b}^T \boldsymbol{X})) = 0
\end{align\*}\]

\(L(Y|\boldsymbol{X})\)可以看成\(Y\)在\(\boldsymbol{X}\)张成的空间上的投影，
此性质即投影应满足的性质。
注意
\[\begin{align\*}
E(\boldsymbol{X}(Y - \boldsymbol{b}^T \boldsymbol{X}))
=& E(\boldsymbol{X}Y) - \Gamma \boldsymbol{b}.
\end{align\*}\]
即残差与自变量正交等价于系数\(\boldsymbol{b}\)满足预测方程。

**证明**:

**必要性**:
设\(\tilde Y\)为\(L(Y|\boldsymbol{X})\)，
由性质2知存在\(\boldsymbol{a}\)满足预测方程，
由性质1和性质3知
\(L(Y|\boldsymbol{X}) = \boldsymbol{a}^T \boldsymbol{X} = \boldsymbol{b}^T \boldsymbol{X}\)。
两边右乘以\(\boldsymbol{X}^T\)取期望得
\[\begin{aligned}
\boldsymbol{a}^T \Gamma = \boldsymbol{b}^T \Gamma
\end{aligned}\]
注意\(\Gamma \boldsymbol{a} = E(\boldsymbol{X}Y)\)所以由上式得
\(\Gamma \boldsymbol{b} = E(\boldsymbol{X}Y)\),
即条件成立。

**充分性**:
条件成立时\(\boldsymbol{b}\)是预测方程的解，
由性质1即知\(\tilde Y = \boldsymbol{b}^T \boldsymbol{X}\)是最佳线性预测。

### 22.1.8 性质8

如果
\[\begin{align\*}
\hat{Y}=& L(Y|X\_1,X\_2,\cdots,X\_n),\\
\tilde{Y}=& L(Y|X\_1,X\_2,\cdots,X\_{n-1}),
\end{align\*}\]
则有
\[
L(\hat Y | X\_1,X\_2,\cdots,X\_{n-1})= \tilde Y,
\]
并且有
\[\begin{align}
E(Y-\hat{Y})^2\leq E(Y-\tilde{Y})^2.
\tag{22.6}
\end{align}\]

[(22.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpmorebetter0109)表明在方差最小的意义下,
\(\hat Y\) 比\(\tilde Y\) 要好.  
这是由于 \(X\_1,X\_2,\) \(\cdots,X\_{n}\)中包含的信息比
\(X\_1,X\_2,\) \(\cdots,X\_{n-1}\)中包含的信息多的原因.

**证明**
\(Y\_0 \stackrel{\triangle}{=} L(\hat Y | X\_1,X\_2,\dots,X\_{n-1})\)
是\(X\_1,X\_2,\dots,X\_{n-1}\)的线性组合,
利用\(Y-\hat Y\), \(\hat Y - Y\_0\)都和
\(X\_1,\dots,X\_{n-1}\)正交, 得到
\[Y-Y\_0= (Y-\hat Y) +(\hat Y - Y\_0)\]
和\(X\_1,\dots,X\_{n-1}\)正交.
利用性质 7即知\(Y\_0=L(Y|X\_1,\dots,X\_{n-1})=\tilde Y\).

\(\hat Y\)是\(X\_1,\dots,X\_n\)对\(Y\)的最佳线性预测
而\(\tilde Y\)是\(X\_1,\dots,X\_n\)的一个线性组合所以有[(22.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpmorebetter0109)成立。

这个性质实际是投影的性质。

### 22.1.9 性质9(非零均值的最佳线性预测的意义)

如果\(EY=b\), \(E\boldsymbol{X}=\boldsymbol{\mu}\), 按定义
\(L(Y|\boldsymbol{X})=b + L(Y-b|\boldsymbol{X}-\boldsymbol{\mu})\)。
事实上对任何\(c\_0\in \mathbb R\), \(\boldsymbol{c} \in {\mathbb R}^n\),
\[\begin{align}
E[Y-L(Y|\boldsymbol{X})]^2
\leq E[Y-(c\_0 + \boldsymbol{c}^{T} \boldsymbol{X})]^2.
\tag{22.7}
\end{align}\]

**证明**
设\(L(Y|\boldsymbol{X}) = b + \boldsymbol{a}^T(\boldsymbol{X}-\boldsymbol{\mu})\), 则
\[\begin{aligned}
&E\left\{Y - c\_0 - \boldsymbol{c}^T \boldsymbol{X} \right\}^2 \\
=& E\left\{Y - b - \boldsymbol{a}^T(\boldsymbol{X}-\boldsymbol{\mu})
+ b + \boldsymbol{a}^T(\boldsymbol{X}-\boldsymbol{\mu}) \right.\\
& \left. - \left[ c\_0 + \boldsymbol{c}^T \boldsymbol{\mu}
+ \boldsymbol{c}^T (\boldsymbol{X} - \boldsymbol{\mu}) \right] \right\}^2\\
=& E\left\{
[ Y - b - \boldsymbol{a}^T (\boldsymbol{X}-\boldsymbol{\mu})]
+ (b - c\_0 - \boldsymbol{c}^T \boldsymbol{\mu})
+ (\boldsymbol{a}-\boldsymbol{c})^T(\boldsymbol{X}-\boldsymbol{\mu}) \right\}^2 \\
=& E[Y - b - \boldsymbol{a}^T (\boldsymbol{X}-\boldsymbol{\mu})]^2
+ (b - c\_0 - \boldsymbol{c}^T \boldsymbol{\mu})^2
+ (\boldsymbol{a}-\boldsymbol{c})^T \Gamma (\boldsymbol{a}-\boldsymbol{c}) \\
\geq& E[Y - b - \boldsymbol{a}^T (\boldsymbol{X}-\boldsymbol{\mu})]^2
= E[Y - L(Y|\boldsymbol{X})]^2
\end{aligned}\]

### 22.1.10 性质10

设\(\boldsymbol{X}\) 和 \(\boldsymbol{Z}\) 分别是\(m\)和\(n\)维向量,
如果有实矩阵\(A\), \(B\)使得\(\boldsymbol{X}= A\boldsymbol{Z}\),
\(\boldsymbol{Z}=B\boldsymbol{X}\),
则\(L(Y|\boldsymbol{X})=L(Y|\boldsymbol{Z})\).

如果\(\boldsymbol{X}\)和\(\boldsymbol{Z}\)能互相线性表示则利用其预报\(Y\)能达到的下界是相同的，
预报是一致的。

证明为习题。

### 22.1.11 性质总结

性质1、2、3、7说明\(L(Y|\boldsymbol{X})\)存在唯一，
且
\[\begin{aligned}
& \boldsymbol{a}^T \boldsymbol{X} = L(Y|\boldsymbol{X}) \\
\Longleftrightarrow &
\Sigma \boldsymbol{a} = \Sigma\_{\boldsymbol{X} Y} \\
\Longleftrightarrow &
E[X\_j (Y - \boldsymbol{a}^T \boldsymbol{X})] = 0, j=1,2,\dots,n .
\end{aligned}\]

性质1还给出了勾股定理：
\[
E(Y^2)
= E[L(Y|\boldsymbol{X})]^2 + E[Y - L(Y|\boldsymbol{X})]^2 .
\]

性质5说明\(L(\cdot | \boldsymbol{X})\)是定义在\(L^2\)空间的线性算子。

性质4说明如果\(Y\)与\(\boldsymbol{X}\)不相关则预报为\(EY\)，
如果\(Y\)已经是\(\boldsymbol{X}\)的线性组合则预报误差为0。

性质6说明如果两组自变量不相关，
则预报可以分别预报后求和；
性质10说明如果两组自变量线性等价（可以互相线性表示），
则预报相同。

性质8说明增加自变量可以使得预报均方误差减少。

### 22.1.12 例子

考虑§[13.7.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-examp42)的ARMA(4,2)序列的预测。
设已知ARMA(4,2)的参数。
\[\begin{aligned}
X\_t =& -0.9 X\_{t-1} - 1.4 X\_{t-2} - 0.7 X\_{t-3} - 0.6 X\_{t-4} \\
& + \varepsilon\_t + 0.5 \varepsilon\_{t-1} - 0.4 \varepsilon\_{t-2},
\ \varepsilon\_t \sim \text{WN}(0,1),
\end{aligned}\]

模拟生成观测到\(x\_1,\dots,x\_{21}\)，
用前14个观测值对最后的7个点作预测。

使用预测方程直接求解系数，
\(\Gamma\)使用理论值。
预测方程中\(\Sigma\_{\boldsymbol{X}Y}\)当\(Y=X\_{n+k}\)时
为
\[
\boldsymbol{g}\_k = E(\boldsymbol{X}\_n X\_{n+k})
= (\gamma\_{n+k-1}, \gamma\_{n+k-2}, \dots, \gamma\_k)^T
\]

最佳线性预测为
\[\begin{aligned}
\hat X\_{n+k} \stackrel{\triangle}{=} L(X\_{n+k} | \boldsymbol{X}\_n)
= (\Gamma\_n^{-1} \boldsymbol{g}\_k)^T \boldsymbol{X}\_n
= \boldsymbol{g}\_k^T \Gamma\_n^{-1} \boldsymbol{X}\_n
\end{aligned}\]
预测方差为
\[\begin{aligned}
\sigma^2(k) = \gamma\_0
- (\Gamma\_n^{-1} \boldsymbol{g}\_k)^T \Gamma\_n (\Gamma\_n^{-1} \boldsymbol{g}\_k)
= \gamma\_0 - \boldsymbol{g}\_k^T \Gamma\_n^{-1} \boldsymbol{g}\_k
\end{aligned}\]

设\(\{X\_t\}\)为正态平稳列，
则\(X\_{n+k}-\hat X\_{n+k}\)作为有限线性组合也是正态分布的。
\(X\_{n+k}-\hat X\_{n+k} \sim \text{N}(0, \sigma^2(k))\),
可以构造\(X\_{n+k}\)的置信区间(预测区间)：
\[\begin{aligned}
\Pr(|X\_{n+k} - \hat X\_{n+k}| / \sigma(k) \leq 1.96) = 0.95,
\quad k=1,2,\dots,m
\end{aligned}\]
见演示。

对真实数据需要用\(x\_1,x\_2,\dots,x\_N\)估计\(\hat\gamma\_k\),
然后用令\(\boldsymbol{x}\_n = (x\_{N-n+1}, x\_{N-n+2}, \dots, x\_N)^T\),
用\(\boldsymbol{x}\_n\)预报\(X\_{N+k}\), \(k=1,2,\dots\)。
数据先减去均值再估计并预测，预测值要把均值加回去。

下面是从ARMA模型参数计算Wold系数的R函数，
以及通过Wold系数计算理论自协方差函数的R函数。

```
## Wold coefficients for the ARMA model
arma.Wold <- function(n, a, b=numeric(0)){
  p <- length(a)
  q <- length(b)
  arev <- rev(a)
  psi <- numeric(n)
  psi[1] <- 1
  for(j in seq(n-1)){
    if(j <= q) bj=b[j]
    else bj=0
    psis <- psi[max(1, j+1-p):j]
    np <- length(psis)
    if(np < p) psis <- c(rep(0,p-np), psis)
    psi[j+1] <- bj + sum(arev * psis)
  }
  
  psi
}

## Calculate theoretical autocovariance function
## of ARMA model using Wold expansion
arma.gamma.by.Wold <- function(n, a, b=numeric(0), sigma=1){
  nn <- n + 100
  psi <- arma.Wold(nn, a, b)
  gam <- numeric(n)
  for(ii in seq(0, n-1)){
    gam[ii+1] <- sum(psi[1:(nn-ii)] * psi[(ii+1):nn])
  }
  gam <- (sigma^2) * gam
  gam
}
arma.gamma <- arma.gamma.by.Wold
```

下面的R程序模拟生成21个样本点，用开头的14个观测预测最后的7个观测（多步预测）。
使用模型的理论自协方差函数求解预测系数。

在结果图形中，绿色线是预测用到的观测，
红色点为预测值，绿色点为实际值，
上下两条红色虚线是逐点的预测区间。

```
## 对模拟ARMA(4,2)数据，
## 用理论协方差解预测方程进行Y-W预报
## 这应该和用Levinson公式得到的预测系数是一致的。
demo.pred.arma42 <- function(n=21){
  a <- c(-0.9, -1.4, -0.7, -0.6)
  b <- c(0.5, -0.4)
  ng <- 21

  x <- arima.sim(model=list(ar=a, ma=b), n=n)
  gams <- arma.gamma(ng, a, b, sigma=1)

  n.use <- 14  ## use n.use points in prediction
  m.pred <- 7  ## predit m.pred steps
  n.start <- n - m.pred - n.use + 1  ## which x are affected
  ## predict usging true ACV
  Ga <- matrix(0, nrow=n.use, ncol=n.use)
  for(ii in seq(n.use)) for(jj in seq(n.use)) {
    ind <- abs(ii-jj)+1
    Ga[ii,jj] <- gams[ind]
  }
  Gar <- solve(Ga)

  x.use <- x[(n.start+n.use-1):n.start] # reverse time order
  y.pred <- x
  y.pred[] <- NA
  errs <- numeric(m.pred)
  for(k in seq(m.pred)){
    g <- gams[(k+1):(k+n.use)]
    a <- Gar %*% g
    y.pred[n.start+n.use-1+k] <- sum(a * x.use)
    errs[k] <- gams[1] - g %*% Gar %*% g
  }
  lb <- y.pred[(n-m.pred+1):n]-1.96*errs
  ub <- y.pred[(n-m.pred+1):n]+1.96*errs
  yl <- range(c(lb,ub,x,y.pred), na.rm=T)
  plot(1:n, x, type="n",
       main="Prediction of ARMA(4,2)",
       xlab="t", ylab="y",
       ylim=yl)
  if(n.start > 1){
    lines(1:(n.start), x[1:n.start],
          lwd=2)  ## unused
  }
  lines(n.start:(n.start+n.use-1), x[n.start:(n.start+n.use-1)],
        col="green", lwd=2) ## used for predition
  lines((n-m.pred):n, x[(n-m.pred):n],
        type="b", col="green", lty=3, lwd=2)  ## true values
  lines((n-m.pred+1):n, y.pred[(n-m.pred+1):n],
        type="b", col="red", lwd=2)  ## predictons
  lines((n-m.pred+1):n, ub,
        type="l", col="red", lty=2)  ## upper bounds
  lines((n-m.pred+1):n, lb,
        type="l", col="red", lty=2)  ## lower bounds

  invisible()
}
set.seed(1)
demo.pred.arma42()
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/5013-blpprop_files/figure-html/blpprop-demo01-1.png)

## 22.2 Hilbert空间中的投影

下面说明最佳线性预测实际上是Hilbert 空间中的投影.

用\(L^2\)表示全体方差有限的随机变量构成的Hilbert空间(参见第[5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-Hilbert-station.html#atsa-Hilbert-station)章).
设\(H\)是\(L^2\)的闭子空间, \(Y\)属于\(L^2\).
可以证明\(H\)中存在惟一的\(\hat Y\)使得
\[\begin{align}
E(Y - \hat Y)^2 = \inf\_{\xi \in H} E(Y - \xi)^2
\tag{22.8}
\end{align}\]

**定义22.3** 如果\(H\)是\(L^2\)的闭子空间, \(Y \in L^2\),
\(\hat Y \in H\)使得[(22.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-projdef0111)成立,
则称\(\hat Y\)是\(Y\)在\(H\)上的投影.
记做\(P\_H(Y)\), 并且称\(P\_H\)是投影算子.

**定义22.4** 设\(Y \in L^2\), 如果对\(H\)中的任何\(\xi\),
\(E(Y\xi)=0\), 则称\(Y\)垂直于\(H\),
记作\(Y \perp H\).

### 22.2.1 投影存在唯一的证明

取\(Y\_n \in H\)使
\[\begin{aligned}
d = \inf\_{\xi\in H} E(Y-\xi)^2
= \lim\_{n\to\infty}E(Y - Y\_n)^2
\end{aligned}\]
则\((Y\_n+Y\_m)/2 \in H\),
并且当\(n,m \to \infty\)
\[\begin{align}
& E(Y\_n - Y\_m)^2\\
=& E[(Y\_n-Y) - (Y\_m -Y)]^2 \\
& + E[(Y\_n-Y) + (Y\_m -Y)]^2 \\
& - E[(Y\_n + Y\_m)-2Y]^2\\
=& 2E(Y\_n-Y)^2 + 2E(Y\_m -Y)^2 - 4 E[(Y\_n + Y\_m)/2-Y]^2\\
\leq& 2E(Y\_n-Y)^2 + 2E(Y\_m -Y)^2 -4d\\
\to & 2d +2d - 4d =0.
\tag{22.9}
\end{align}\]

于是, \(\{Y\_n\}\)是\(H\) 中的基本列,
从而有\(\hat Y \in H\) 使得\(Y\_n\)均方收敛到\(\hat Y\).
由内积的连续性知道
\[
E(Y-\hat Y)^2 = \lim\_{n\to \infty} E(Y - Y\_n)^2 =d.
\]
于是, \(\hat Y\)满足[(22.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-projdef0111).

如果又有\(\hat \xi\in H\) 也使得[(22.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-projdef0111)成立,
仿照[(22.9)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-projexonlypr0112)的推导得到
\[\begin{aligned}
&E(\hat Y - \hat \xi)^2\\
=& E[(\hat Y-Y) - (\hat \xi -Y)]^2 \\
& + E[(\hat Y-Y) + (\hat \xi -Y)]^2
- E[(\hat Y + \hat \xi)-2Y]^2\\
=& 2E(\hat Y-Y)^2 + 2E(\hat \xi -Y)^2
- 4 E[(\hat Y + \hat \xi)/2-Y]^2\\
\leq& 2d+2d -4d =0.
\end{aligned}\]
所以 \(\hat \xi=\hat Y\), a.s.

### 22.2.2 投影的垂直性(正交性)

**定理22.1** 设\(Y \in L^2\), \(\hat Y \in H\), 则
\(\hat Y =P\_H(Y)\)的充分必要条件是\((Y-\hat Y) \perp H\).

最佳线性预测性质7是定理[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-projperpcond)的特例。

**证明**
先证必要性.  
设\(\hat Y=P\_H(Y)\).
对\(\forall\xi \in H\), 我们证明
\[
a \stackrel{\triangle}{=} E[(Y-\hat Y) \xi]=0.
\]
无妨设\(E\xi^2=1\), 这时
\[\begin{aligned}
d \stackrel{\triangle}{=}& E(Y-\hat Y)^2
\leq E( Y- \hat Y - a \xi)^2 \\
=& E( Y- \hat Y)^2 +E(a\xi)^2 - 2 a E[(Y-\hat Y)\xi] \\
=& d + a^2 - 2 a^2 = d - a^2
\end{aligned}\]
由此得到\(a=0\).

来证明充分性。若\(\hat Y \in H\)使\(Y-\hat Y \perp H\)，
则对\(\forall \xi \in H\)有
\[\begin{aligned}
& E(Y - \xi)^2 = E(Y - \hat Y + \hat Y - \xi)^2 \\
=& E(Y - \hat Y)^2 + E(\hat Y - \xi)^2
+ 2 E[(Y-\hat Y)(\hat Y - \xi)] \\
=& E(Y - \hat Y)^2 + E(\hat Y - \xi)^2 \\
\geq& E(Y - \hat Y)^2
\end{aligned}\]
即\(\hat Y = P\_H(Y)\)。

### 22.2.3 最佳线性预报与投影的等价性

用\(L^2(\boldsymbol X)\)表示\(\boldsymbol X=(X\_1,X\_2,\cdots,X\_n)^T\)
的元素和常数\(1\) 生成的Hilbert空间.
它是\(X\_1,X\_2,..,X\_n\)和常数\(1\)的线性组合的全体(参见([何书元 2003](#ref-He-TSA03))第1章习题6.5).
设\(\boldsymbol{\mu} =(\mu\_1,\mu\_2,\cdots,\mu\_n)^T =E\boldsymbol{X}\).
对任何方差有限的随机变量\(Y\), 设\(EY=b\),
\(\hat Y=L(Y|\boldsymbol{X})\) 由[(22.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpdefwithmu0103)式定义. 则有
\[
Y- \hat Y =(Y-b)-L(Y-b|\boldsymbol{X}-\boldsymbol{\mu}).
\]
利用性质 7 知道
\[\begin{aligned}
&E[1 \cdot (Y-\hat Y)] = E(Y-b)-EL(Y-b|\boldsymbol{X}-\boldsymbol{\mu})=0,\\
&E[X\_i(Y-\hat Y)] = E[(X\_i-\mu\_i)(Y-\hat Y)] + \mu\_i E(Y-\hat Y) =0.
\end{aligned}\]
即得到\((Y-\hat Y)\)垂直于\(H \stackrel{\triangle}{=} L^2(\boldsymbol{X})\).
由定理[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-projperpcond)知道
\[
L(Y|\boldsymbol{X})=P\_H(Y).
\]

基于上述原因, 当\(H\)是\(\{X\_j:j\in T\}\)和常数\(1\)生成的Hilbert空间,
我们也用
\[
L(Y|1, X\_j, j \in T) \ \ \ \text{或} \ \ L(Y|H)
\]
表示\(P\_H(Y)\), 这里\(T\)是一个可列的指标集.

下面记\(\|\xi\| = \sqrt{E{\xi^2}}\),
\(\forall \xi\in L^2\)。
\(\|\xi\|\)是\(\xi\)的长度。

### 22.2.4 投影算子的性质

**定理22.2** 设 \(H\), \(M\)是\(L^2\)的闭子空间, \(X\),\(Y \in L^2\), \(a,b\)是常数.

* (1) \(L(aX+bY|H) = aL(X|H)+bL(Y|H)\).
  (对应于最佳线性预测性质5)
* (2) \(\|Y\|^2 = \|L(Y|H)\|^2 + \|Y-L(Y|H)\|^2\).
  (对应于最佳线性预测性质1的(1.6)式)
* (3) \(\|L(Y|H)\| \leq \|Y\|\).
* (4) \(Y\in H\) 的充分必要条件是\(L(Y|H)=Y\).
  (对应于最佳线性预测性质4第(2)条)
* (5) \(Y\)垂直于\(H\)的充分必要条件是\(L(Y|H)=0\),
  (对应于最佳线性预测性质4第(1)条)
* (6) 如果\(H\)是\(M\)的子空间, 则\(P\_H P\_M= P\_M P\_H = P\_H\), 并且对\(Y\in L^2\),
  \[
  \|Y - L(Y|M)\| \leq \| Y-L(Y|H)\|.
  \]
  (对应于最佳线性预测性质8)

**证明**

(1) 设\(Z = a L(X|H) + b L(Y|H)\)则\(Z \in H\)。由
\[\begin{aligned}
(aX + bY) - Z = a[X - L(X|H)] + b[Y - L(Y|H)]
\end{aligned}\]
看出\((aX + bY) - Z \perp H\)。所以
\[\begin{aligned}
L(aX + bY | H) = Z = a L(X|H) + b L(Y|H)
\end{aligned}\]
这说明投影是线性算子。

(2) 由于\(L(Y|H) \in H\)而\(Y - L(Y|H) \perp H\)所以
\[\begin{aligned}
\| Y \|^2 =& E[ (Y - L(Y|H)) + L(Y|H) ]^2 \\
=& E[Y - L(Y|H)]^2 + E[L(Y|H)]^2 \\
& + 2 E[(Y - L(Y|H)) L(Y|H)] \\
=& E[Y - L(Y|H)]^2 + E[L(Y|H)]^2 \\
=& \| Y - L(Y|H) \|^2 + \|L(Y|H)\|^2
\end{aligned}\]

(3) 由(2)直接得到。

(4) 必要性：\(Y\in H\)时取\(L(Y|H)=Y\)可得均方误差为0。

充分性: 若\(L(Y|H)=Y\)则由于投影必须属于\(H\)所以\(Y \in H\)。

(5) 必要性: 若\(Y \perp H\)则\(0 \in H, Y - 0 \perp H\)所以\(L(Y|H)=0\)。

充分性: 若\(L(Y|H)=0\)则由\(Y - L(Y|H) \perp H\)知\(Y \perp H\)。

(6) \(\forall Y \in L^2\)，
设\(\xi = P\_M(Y)\), \(\eta = P\_H(Y)\)，
来证\(P\_H(\xi) = \eta\)。
事实上，\(\eta \in H\)且
\[\begin{aligned}
\xi - \eta = (Y - \eta) - (Y - \xi)
\end{aligned}\]
其中\(Y - \eta\)和\(Y - \xi\)都与\(H\)垂直，
所以\(P\_H(\xi) = \eta\)，即\(P\_H P\_M = P\_H\)。
另外, \(H \subseteq M\)所以\(\eta \in M\)，
\(P\_M(\eta) = \eta\)，即\(P\_M P\_H = P\_H\)。

由\(P\_H(Y) \in M\)和\(P\_M(Y)\)的定义马上可得
\[\begin{aligned}
\|Y - P\_H(Y) \|^2 \geq \|Y - P\_M(Y)\|^2
\end{aligned}\]

## 22.3 最佳预测

最佳线性预测只用了自变量的线性函数而未考虑其他函数。
设
\[\begin{align}
M = \bar{\text{sp}}\{g(\boldsymbol{X}):
\ E g^2(\boldsymbol{X}) < \infty, g(\cdot)
\text{是可测函数} \}
\tag{22.10}
\end{align}\]
考虑用\(M\)中的元素逼近\(Y\)。

**定义22.5** 设\(M\)由[(22.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-bestpspace0114)定义.
用\(\boldsymbol{X}=(X\_1,X\_2,\dots,X\_n)^{T}\)对\(Y\)进行预测时,
称
\[\begin{align\*}
L(Y|M) \stackrel{\triangle}{=} P\_M(Y)
\tag{22.11}
\end{align\*}\]
为\(Y\)的最佳预测.

最佳预测\(L(Y|M)\)实际上是概率论中的条件数学期望\(E(Y|\boldsymbol X)\).

\(L^2(\boldsymbol{X})\)是\(M\)的子空间，
由定理[22.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-proj-prop)的(6)得
\[\begin{aligned}
\| Y - L(Y|M)\| \leq \| Y - L(Y|\boldsymbol{X})\|
\end{aligned}\]
在预测均方误差最小的意义下最佳预测比最佳线性预测好。

但是由于\(M\)要比\(L^2(\boldsymbol X)\)复杂很多,
实际计算最佳预测往往比计算最佳线性预测困难得多.

对于正态序列来讲, 最佳预测和最佳线性预测是一致的.

**定理22.3** 如果\((X\_1,X\_2,\dots,X\_n,Y)^T\)
服从联合正态分布\(\text{N}(\boldsymbol{\mu}, \Sigma)\),
\(M\)由[(22.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-bestpspace0114)定义, 则
\[\begin{align}
L(Y|M)=L(Y|X\_1,X\_2,\cdots,X\_n).
\tag{22.12}
\end{align}\]

**证明**
设\(\hat{Y}=L(Y|X\_1,X\_2,\dots,X\_n)\),
则\((Y-\hat{Y})\)与\(\boldsymbol{X}\)正交.
由于\(E(Y-\hat Y)=0\),
所以\(Y-\hat Y\)与\(\boldsymbol{X}\)不相关.
由正态分布的性质知道, \(Y-\hat Y\)与\(\boldsymbol{X}\)独立,
从而和\(M\)中的任何随机变量独立.
对任何\(\xi \in M\),
\(E[\xi(Y -\hat Y)]=(E\xi) E(Y-\hat Y)=0\),  
即 \(Y-\hat Y\)垂直于\(M\).
从\(\hat Y \in M\) 和定理[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-projperpcond)知道[(22.12)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-bestpnorm0116)成立.

### 22.3.1 例子

**例22.1** 设随机变量\(\varepsilon,\eta\)独立,
都服从标准正态分布\(N(0,1)\),
则\(E\eta^{4}=3\).
取\(X=\eta,Y=(3\varepsilon^2-\eta^2)\eta\).

则\(EX=EY=0\), \(E(XY)=E(3\varepsilon^2\eta^2-\eta^4)=0\).
从而\(L(Y|X)=0\).
计算
\[\begin{aligned}
E(Y|X) =& E(3\varepsilon^2 \eta - \eta^3|\eta) \\
=& 3\eta - \eta^3 = 3X - X^3
\end{aligned}\]
容易验证\(Y-(3\eta-\eta^3)=3(\varepsilon^2 - 1)\eta\)垂直于
\[
M=\bar{sp}\left\{g(X) :
E g^2(X) < \infty, g(x)\hbox{ 是可测函数 } \right \}.
\]
于是, 从\(3\eta-\eta^3\in M\) 知道: \(L(Y|M)=3X-X^3\).

## 22.4 附录：补充

### 22.4.1 正交直和投影

最佳线性预测的性质大都可以看成投影性质。
其中性质6可以扩充为：
设\(M,N\)是\(L^2\)的两个子Hilbert空间，
对\(\forall \xi\in M\), \(\forall \eta\in N\)，
有
\[\begin{aligned}
\langle \xi, \eta \rangle = 0
\end{aligned}\]
称\(M\)与\(N\)正交。定义
\[\begin{aligned}
M \oplus N = \{ \xi + \eta: \xi\in M, \eta\in N \}
\end{aligned}\]
则
\[\begin{aligned}
L(Y|M \oplus N) = L(Y|M) + L(Y|N)
\end{aligned}\]
用投影的残差正交性可以证明此性质。

### References

何书元. 2003. *应用时间序列分析*. 北京大学出版社.