---
crawl_time: '2026-01-17 14:28:49'
framework: sphinx
title: 10 平稳序列的偏相关系数和Levinson递推公式 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 10 平稳序列的偏相关系数和Levinson递推公式

## 10.1 最优线性预测

### 10.1.1 有限个随机变量的最优线性预测

设\(X\_1, X\_2, \dots, X\_n, Y\)为随机变量。
考虑估计问题
\[\begin{aligned}
L(Y | X\_1,\dots, X\_n) \stackrel{\triangle}{=}
\mathop{\mathrm{argmin}}\_{\hat Y = a\_0 + a\_1 X\_1 + \dots + a\_n X\_n} E(Y - \hat Y)^2
\end{aligned}\]
称\(L(Y | X\_1, \dots, X\_n)\)为\(Y\)关于\(X\_1, \dots, X\_n\)的**最优线性预测**或者最优线性估计。
\(E(Y - \hat Y)^2 = \| Y - \hat Y \|^2\)称为预测的均方误差。
\(L(Y | X\_1, \dots, X\_n)\)实际是用\(X\_1, \dots, X\_n\)的带截距项的线性组合预测\(Y\)的均方误差最小的预测。

用\(\text{sp}(1, X\_1, \dots, X\_n)\)表示由\(1, X\_1, \dots, X\_n\)的线性组合组成的\(L^2\)的子空间。
由Hilbert空间的投影理论，
\(L(Y | X\_1, \dots, X\_n)\)是\(Y\)在\(\text{sp}(1, X\_1, \dots, X\_n)\)上的投影。

下面用求多元函数最小值的方法推导\(L(Y | X\_1, \dots, X\_n)\)的公式。
记\(\boldsymbol{X} = (X\_1,\dots,X\_n)^T\),
仅在\(\boldsymbol{X}\)协方差阵正定情况下给出结果。

令 \(\boldsymbol{\xi} = \boldsymbol{X} - E\boldsymbol{X}\),
\(\eta = Y - E Y\)。设
\(\Sigma\_{\boldsymbol{X}} \stackrel{\triangle}{=} \text{Var}(\boldsymbol{X}) = \text{Var}(\boldsymbol{\xi})\)正定。
记\(\Sigma\_{\boldsymbol{X},Y} = \text{Cov}(\boldsymbol{X}, Y)\).
对\(a\_0, a\_1, \dots, a\_n \in R\),
记\(\boldsymbol{a} = (a\_1, \dots, a\_n)^T\)，
有
\[\begin{aligned}
& E \left( Y - (a\_0 + a\_1 X\_1 + \dots + a\_n X\_n) \right)^2 \\
=& E(\eta - (a\_1 \xi\_1 + \dots a\_n \xi\_n))^2
+ (E Y - a\_0 - \boldsymbol{a}^T E \boldsymbol{X})^2
\end{aligned}\]
已知\(a\_1, \dots, a\_n\)后取\(a\_0 = E Y - \boldsymbol{a}^T E\boldsymbol{X}\)
就可以使上式后一项为零，所以不妨设\(E \boldsymbol X=0\), \(E Y=0\)。

这时
\[\begin{aligned}
g(\boldsymbol{a}) =& E(Y - (a\_1 X\_1 + \dots a\_n X\_n))^2 = E(Y - \boldsymbol{a}^T \boldsymbol{X})^2\\
=& \text{Var}(Y) + \boldsymbol{a}^T \Sigma\_{\boldsymbol{X}} \boldsymbol{a}
- 2 \boldsymbol{a}^T \Sigma\_{\boldsymbol{X},Y} , \\
\frac{\partial g(\boldsymbol{a})}{\partial \boldsymbol{a}} =&
2 \Sigma\_{\boldsymbol{X}} \boldsymbol{a} - 2 \Sigma\_{\boldsymbol{X},Y} , \\
\frac{\partial^2 g(\boldsymbol{a})}{\partial \boldsymbol{a} \partial \boldsymbol{a}^T} =&
2 \Sigma\_{\boldsymbol{X}} > 0 .
\end{aligned}\]
令\(\frac{\partial g(\boldsymbol{a})}{\partial \boldsymbol{a}} = 0\)求得
\[\begin{aligned}
\boldsymbol{a} = \Sigma\_{\boldsymbol{X}}^{-1} \Sigma\_{\boldsymbol{X}, Y}
\end{aligned}\]
因为海色阵\(\frac{\partial^2 g(\boldsymbol{a})}{\partial \boldsymbol{a} \partial \boldsymbol{a}^T}\)
正定所以上式为\(g(\boldsymbol{a})\)的唯一严格最小值点。

于是，
\[\begin{align}
L(Y | X\_1, X\_2, \dots, X\_n)
= E Y + \Sigma\_{Y, \boldsymbol{X}} \Sigma\_{\boldsymbol{X}}^{-1}(\boldsymbol{X} - E \boldsymbol{X})
\tag{10.1}
\end{align}\]

估计误差的最小值为
\[\begin{align}
E(\eta - \boldsymbol{a} \boldsymbol{\xi})^2
= \text{Var}(Y) - \Sigma\_{\boldsymbol{X},Y}^T \Sigma\_{\boldsymbol{X}}^{-1}
\Sigma\_{\boldsymbol{X},Y}
\tag{10.2}
\end{align}\]

当\(|\Gamma\_n|=0\)时（协方差阵不满秩时），
最优线性估计也存在，但有无穷多个(详见[22](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#atsa-blpprop))。

### 10.1.2 平稳列的最优线性预测

设\(\{X\_t\}\)为零均值平稳列。考虑用\(X\_1,\ldots,X\_n\)的线性组合预测\(X\_{n+1}\)。
设\(\Gamma\_n>0\)，则
\[\begin{aligned}
& L(X\_{n+1} | X\_n, X\_{n-1}, \dots, X\_1) \\
=& \left[
\text{Var}(
\left(
\begin{array}{c}
X\_n\\ \vdots \\ X\_1
\end{array}
\right) )^{-1}
\text{Cov}(
\left(
\begin{array}{c}
X\_n\\ \vdots \\ X\_1
\end{array}
\right),
X\_{n+1})
\right]^T
\left(
\begin{array}{c}
X\_n\\ \vdots \\ X\_1
\end{array}
\right) \\
=& (\Gamma\_n^{-1}
\left( \begin{array}{c}
\gamma\_1\\ \gamma\_2 \\ \dots\\ \gamma\_n \end{array} \right)
)^T
\left( \begin{array}{c}
X\_n \\ \dots \\ X\_1 \end{array} \right) \\
\stackrel{\triangle}{=}& a\_{n1} X\_{n} + a\_{n2} X\_{n-1} + \dots + a\_{nn} X\_1 \\
\stackrel{\triangle}{=}& \boldsymbol{a}\_n^T (X\_n, X\_{n-1}, \dots, X\_1)^T
\end{aligned}\]

Yule-Walker方程:
\[\begin{aligned}
\Gamma\_n \left(\begin{array}{c}
a\_{n1} \\ \vdots \\ a\_{nn} \end{array} \right)
= \left( \begin{array}{c}
\gamma\_1\\ \vdots\\ \gamma\_n \end{array} \right)
\end{aligned}\]
简记为
\[\begin{aligned}
\Gamma\_n \boldsymbol a\_n = \boldsymbol \gamma\_n
\end{aligned}\]
可以证明（见[22](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#atsa-blpprop)），
Y-W方程总有解，
当且仅当\(\Gamma\_n\)满秩时解是唯一的。
Y-W方程的解\(\boldsymbol{a}\_n\)称为\(\{ X\_t \}\)或\(\{\gamma\_k\}\)的\(n\)阶Yuler-Walker系数。

\[\begin{aligned}
L(X\_{n+1} | X\_n, \dots, X\_1)
= \boldsymbol{a}\_n^T (X\_n, \dots, X\_1)^T .
\end{aligned}\]

由平稳性
\[\begin{aligned}
L(X\_{t} | X\_{t-1}, \dots, X\_{t-n})
= \boldsymbol{a}\_n^T (X\_{t-1}, \dots, X\_{t-n})^T .
\end{aligned}\]

最小的线性预测方差为
\[\begin{aligned}
\sigma\_n^2
\stackrel{\triangle}{=} & E(X\_{n+1} - (a\_{n1} X\_{n} + \dots a\_{nn} X\_1))^2 \\
=& \text{Var}(X\_{n+1}) - \boldsymbol{\gamma}\_n^T \Gamma\_n \boldsymbol{\gamma}\_n \\
=& \gamma\_0 - \boldsymbol{\gamma}\_n^T \boldsymbol{a}\_n \\
=& \gamma\_0 - a\_{n1}\gamma\_1 - \dots - a\_{nn}\gamma\_n .
\end{aligned}\]
由平稳性
\[\begin{aligned}
E(X\_t - (a\_{n1} X\_{t-1} + \dots a\_{nn} X\_{t-n}))^2
= \sigma\_n^2 .
\end{aligned}\]

## 10.2 最小相位性

如果\(\{\gamma\_k\}\)是某AR(\(p\))序列的自协方差函数，
则\(p\)阶的Yuler-Walker方程解出的Yule-Walker系数就是
AR模型的自回归系数，所以满足如下的最小相位性：
\[\begin{aligned}
A(z) = 1 - \sum\_{j=1}^p a\_j z^j \neq 0, \quad \text{对}|z|\leq 1
\end{aligned}\]

对于一般的平稳列有如下定理。

**定理10.1 (Y-W系数的最小相位性)** 如果实数列\(\gamma\_k, k=0, 1, \dots, n\)使得
\[\begin{aligned}
\Gamma\_{n+1} \stackrel{\triangle}{=} \left(
\begin{array}{cccc}
\gamma\_0 & \gamma\_1 & \cdots & \gamma\_n \\
\gamma\_1 & \gamma\_0 & \cdots & \gamma\_{n-1} \\
\vdots & \vdots & & \vdots \\
\gamma\_n & \gamma\_{n-1} & \cdots & \gamma\_0
\end{array}
\right) > 0
\end{aligned}\]
则解出的\(n\)阶Yuler-Walker系数\(\boldsymbol{a}\_n\)满足如下最小相位条件：
\[\begin{aligned}
1 - \sum\_{j=1}^n a\_{nj} z^j \neq 0, \quad |z| \leq 1.
\end{aligned}\]

最小相位性就是以\(\boldsymbol{a}\_n\)为系数的AR(\(p\))模型能表示成因果性线性平稳列的充分必要条件。

一般的线性平稳列的自协方差列正定，
所以其任意\(n\)阶Yuler-Walker系数都满足最小相位条件。

## 10.3 Levinson递推公式

**定理10.2 (Levinson递推)** 如果\(\Gamma\_{n+1}\)正定，则\(\gamma\_k, k=0,1, \dots, n\)的
\(1,2,\dots,n, n+1\)阶Yuler-Walker系数
\(\{a\_{ij}, i=1,\dots,n+1, j=1, \dots, i \}\)
和均方误差\(\sigma\_k^2\)可以如下递推计算：
\[\begin{align}
\sigma\_0^2 =& \gamma\_0 \\
a\_{1,1} =& \gamma\_1 / \gamma\_0 \\
\sigma\_k^2 =& \sigma\_{k-1}^2 (1 - a\_{k,k}^2) \\
a\_{k+1,k+1} =&
\frac{\gamma\_{k+1} - a\_{k,1} \gamma\_k - a\_{k,2} \gamma\_{k-1}
- \dots - a\_{k,k} \gamma\_1}{
\gamma\_0 - a\_{k,1} \gamma\_1 - a\_{k,2} \gamma\_2 - \dots
- a\_{k,k} \gamma\_k } \\
a\_{k+1,j} =& a\_{k,j} - a\_{k+1,k+1} a\_{k,k+1-j}, \quad 1 \leq j \leq k
\tag{10.3}
\end{align}\]
其中
\[\begin{align}
\sigma\_k^2 = E (X\_{k+1} - (a\_{k,1} X\_{k} + \dots + a\_{k,k} X\_1))^2
\tag{10.4}
\end{align}\]
是用\(X\_k, X\_{k-1}, \dots, X\_1\)预测\(X\_{k+1}\)的均方误差。

### 10.3.1 Levinson公式的记忆方法

回忆§[9.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#arspecyw-yw)中的[(9.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw-dag1)和[(9.9)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw-dag2)
\[\begin{align}
& \gamma\_k - \sum\_{j=1}^p a\_j \gamma\_{k-j} = 0,
\quad k \geq 1 ,\tag{10.5}\\
& \sigma^2 = \gamma\_0 - \sum\_{j=1}^p a\_j \gamma\_{0-j} .
\tag{10.6}
\end{align}\]
在[(10.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:arpacfyw-argamma1)中将\(k\)替换成\(k+1\)得
\[
\gamma\_{k+1} - \sum\_{j=1}^p a\_j \gamma\_{k+1-j} = 0 .
\]
在Levinson递推的\(a\_{k+1,k+1}\)递推公式中可以将分子看成上式左边\(a\_j = a\_{k,j}\), \(p=k\)的情形，
将分母看成是[(10.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:arpacfyw-arsigma1)中\(p=k\), \(a\_j=a\_{k,j}\)的情形。

\(a\_{k+1,j}, 1\leq j \leq k\)的公式可以写成如下的矩阵形式
\[\begin{aligned}
\left(\begin{array}{c}
a\_{k+1,1} \\ \vdots \\ a\_{k+1,k}
\end{array}\right)
=
\left(\begin{array}{c}
a\_{k,1} \\ \vdots \\ a\_{k,k}
\end{array}\right)
- a\_{k+1,k+1}
\left(\begin{array}{c}
a\_{k,k} \\ \vdots \\ a\_{k,1}
\end{array}\right) .
\end{aligned}\]

关于\(\sigma\_k^2\)，
记\(\boldsymbol X\_k = (X\_k, X\_{k-1}, \dots, X\_1)^T\)，有:

\[\begin{aligned}
\sigma\_k^2 =& E[X\_{k+1} - \boldsymbol a\_k^T \boldsymbol X\_k]^2 \\
=& E[(X\_{k+1} - \boldsymbol a\_k^T \boldsymbol X\_k) X\_{k+1}]
- E[(X\_{k+1} - \boldsymbol a\_k^T \boldsymbol X\_k) \boldsymbol a\_k^T \boldsymbol X\_k] \\
=& \gamma\_0 - \boldsymbol a\_k^T (\gamma\_1 \ \ldots \gamma\_k)^T
- 0 \quad(\text{根据Y-W方程})\\
=& \gamma\_0 - a\_{k,1}\gamma\_1 - \dots - a\_{k,k} \gamma\_k
\end{aligned}\]
这是\(a\_{k+1,k+1}\)的递推公式的分母，
所以\(a\_{k+1,k+1}\)的递推公式也可以写成
\[\begin{aligned}
a\_{k+1,k+1} = \frac{\gamma\_{k+1} - a\_{k,1} \gamma\_k - a\_{k,2} \gamma\_{k-1}
- \dots - a\_{k,k} \gamma\_1}{\sigma\_k^2}
\end{aligned}\]
注意\(\sigma\_k^2\)是用\(k\)个历史值预报第\(k+1\)个的最小均方误差线性预测的均方误差。

### 10.3.2 Levinson递推的计算顺序

用Levinson递推公式计算各阶Yuler-Walker系数和
\[\begin{aligned}
\sigma\_k^2 =& E[X\_{k+1} - a\_{k,1} X\_k - a\_{k,2} X\_{k-1} - \dots - a\_{k,k} X\_1]^2
\end{aligned}\]
次序应为

* 初值(不用历史资料预报\(X\_1\))：
  \[\begin{aligned}
  \sigma\_0^2 =& E[X\_1 - 0]^2 = \gamma\_0
  \end{aligned}\]
* \(k+1=1\)(用\(X\_1\)预报\(X\_2\)):
  \[\begin{aligned}
  a\_{1,1} =& \gamma\_1 / \gamma\_0 \\
  \sigma\_1^2 =& E[X\_2 - a\_{1,1} X\_1]^2 \\
  =& \sigma\_0^2 (1 - a\_{1,1}^2)
  \end{aligned}\]
* \(k+1=2\)(用\(X\_1, X\_2\)预报\(X\_3\)):
  \[\begin{aligned}
  a\_{2,2} =& \frac{\gamma\_2 - a\_{1,1} \gamma\_1}{\sigma\_1^2} \\
  a\_{2,1} =& a\_{1,1} - a\_{2,2} a\_{1,1} \\
  \sigma\_2^2 =& E[X\_3 - a\_{2,1} X\_2 - X\_{2,2} X\_1]^2 \\
  =& \sigma\_1^2 ( 1 - a\_{2,2}^2)
  \end{aligned}\]
* \(k+1=3\)(用\(X\_1, X\_2, X\_3\)预报\(X\_4\)):
  \[\begin{aligned}
  a\_{3,3} =& \frac{\gamma\_3 - a\_{2,1} \gamma\_2 - a\_{2,2} \gamma\_1}{\sigma\_2^2} \\
  a\_{3,1} =& a\_{2,1} - a\_{3,3} a\_{2,2} \\
  a\_{3,2} =& a\_{2,2} - a\_{3,3} a\_{2,1} \\
  \sigma\_3^2 =& E[ X\_4 - a\_{3,1}X\_3 - a\_{3,2} X\_2 - a\_{3,3} X\_3]^2 \\
  =& \sigma\_2^2 ( 1- a\_{3,3}^2)
  \end{aligned}\]
* ………………

计算次序为
\[
\begin{array}{c|ccccc|c}
\hline
k & a\_{k,j} & & & & & \sigma\_{k}^2 \\
\hline
0 & & & & & & \sigma\_0^2 \\
1 & a\_{1,1} & & & & & \sigma\_1^2 \\
2 & a\_{2,2} & a\_{2,1} & & & & \sigma\_2^2 \\
3 & a\_{3,3} & a\_{3,1} & a\_{3,2} & & & \sigma\_3^2 \\
4 & a\_{4,4} & a\_{4,1} & a\_{4,2} & a\_{4,3} & & \sigma\_4^2 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \ddots & \vdots
\end{array}
\]

## 10.4 偏相关系数

**定义10.1 (偏相关系数)** 如果\(\Gamma\_n\)正定，称\(a\_{n,n}\)为\(\{X\_t\}\)或
\(\{\gamma\_k\}\)的\(n\)阶偏(自)相关系数。
序列\(\{a\_{n,n}, n=1,2,\dots\}\)称为\(\{X\_t\}\)或
\(\{\gamma\_k\}\)的\(n\)阶偏(自)相关函数。

偏自相关是\(X\_1\)和\(X\_{n+1}\)之间的如下意义下的**偏相关系数**：
\[\begin{aligned}
a\_{n,n} = \text{Corr}[&X\_1 - L(X\_1 | X\_2, \dots, X\_n), \\
& X\_{n+1} - L(X\_{n+1} | X\_2, \dots, X\_n) ]
\end{aligned}\]
即\(a\_{n,n}\)为\(X\_1\)和\(X\_{n+1}\)扣除\(X\_2,\dots,X\_n\)的线性影响后的相关系数。

设\(\{X\_t\}\)是AR(\(p\))序列。其自协方差函数正定。
由Yule-Walker方程[(9.7)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#eq:arspecyw0309)知其\(n\)阶(\(n\geq p\))Y-W系数为
\[\begin{align}
\boldsymbol{a}\_n =& (a\_1, \dots, a\_p, 0, \dots, 0)^T \\
=& (a\_{n,1}, a\_{n,2}, \dots, a\_{n,n})^T, \quad
n \geq p
\tag{10.7}
\end{align}\]
其偏相关系数满足
\[\begin{align}
a\_{n,n} = \begin{cases}
a\_p \neq 0, \quad & n=p \\
0, & n > p
\end{cases}
\tag{10.8}
\end{align}\]
称此性质为AR(\(p\))序列的相关系数\(p\)后截尾。

反之，如果一个零均值平稳列偏相关系数\(p\)后截尾，
则它必是AR(\(p\))序列（见下面的定理）。

偏相关截尾条件隐含要求自协方差列正定。

**定理10.3 (AR序列的偏相关函数条件)** 设零均值平稳列\(\{X\_t\}\)的自协方差函数\(\{\gamma\_k\}\)是正定序列，
则\(\{X\_t\}\)是AR(\(p\))序列的充分必要条件是，它的偏相关系数\(\{a\_{n,n}\}\)
\(p\)后截尾。

**证明**：

只要证明充分性。
记\((a\_{p,1}, \dots, a\_{p,p})=(a\_1,\dots,a\_p)\)，
令\(\varepsilon\_t = X\_t - \sum\_{j=1}^p a\_j X\_{t-j}\)，
只要证明\(\{\varepsilon\_t\}\)是白噪声。
最小相位性由定理[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-minphase)给出。

记\(\boldsymbol{a}\_p = (a\_{p,1}, \dots, a\_{p,p})=(a\_1,\dots,a\_p)\)，
由Levinson公式和\(a\_{p+k,p+k}=0\)(\(k>0\))得
\[\begin{aligned}
a\_{p+1,j} =& a\_{p,j} - a\_{p+1,p+1} a\_{p,p+1-j} = a\_j,
\quad & 1\leq j \leq p \\
a\_{p+k,j} =& a\_{p+k-1,j} = \dots = a\_{p,j} = a\_j,
& k \geq 2, 1 \leq j \leq p \\
a\_{p+k,j} =& a\_{p+k-1, j} = 0
& p < j \leq p+k
\end{aligned}\]
即\(n\geq p\)时
\[\begin{aligned}
\boldsymbol{a}\_n = (a\_{n,1}, a\_{n,2}, \dots, a\_{n,n})
= (a\_1, a\_2, \dots, a\_p, 0, \dots, 0)
\end{aligned}\]

注意\(\boldsymbol{a}\_n\)是Y-W方程的解，即
\[\begin{aligned}
\left(\begin{array}{cccc}
\gamma\_0 & \gamma\_1 & \cdots & \gamma\_{n-1} \\
\gamma\_1 & \gamma\_0 & \cdots & \gamma\_{n-2} \\
\vdots & \vdots & & \vdots \\
\gamma\_{n-1} & \gamma\_{n-2} & \cdots & \gamma\_0
\end{array}
\right)
\left(\begin{array}{c}
a\_1 \\ a\_2 \\ \vdots \\ a\_p \\ 0 \\ \vdots \\ 0
\end{array}
\right)
= \left(\begin{array}{c}
\gamma\_1 \\ \gamma\_2 \\ \vdots \\ \gamma\_n
\end{array}
\right)
\end{aligned}\]
可写成
\[\begin{aligned}
\gamma\_k =& a\_1 \gamma\_{k-1} + a\_2 \gamma\_{k-2} + \dots + a\_p \gamma\_{k-p} \\
=& \sum\_{j=1}^p a\_j \gamma\_{k-j}, \quad k\geq 1
\end{aligned}
\tag{\*}
\]

由定理[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-minphase)知\(A(z)=1 - \sum\_{j=1}^p a\_j z^j\)满足最小相位条件。

令
\[\begin{aligned}
\varepsilon\_t = X\_t - \sum\_{j=1}^p a\_j X\_{t-j}, \quad t \in \mathbb Z
\end{aligned}\]
则\(\{\varepsilon\_t\}\)是平稳序列，满足\(E\varepsilon\_t=0\),
\(\text{Var}(\varepsilon\_t)=\sigma\_p^2 > 0\)
(因为\(\{\gamma\_k\}\)为正定序列所以\(\{X\_t\}\)不是可完全线性预测的)。

下面只要证明\(\{\varepsilon\_t\}\)是白噪声。
\(\forall t > s\)有
\[\begin{aligned}
E(\varepsilon\_t X\_s) =&
E\left[ \left( X\_t - \sum\_{j=1}^p a\_j X\_{t-j} \right)
X\_s \right] \\
=& \gamma\_{t-s} - \sum\_{j=1}^p a\_j \gamma\_{t-s-j} \\
=& 0 \qquad\text{(由(\*))}
\end{aligned}\]
所以\(t>s\)时
\[\begin{aligned}
E(\varepsilon\_t \varepsilon\_s)
= E \left[ \varepsilon\_t
\left(X\_s - \sum\_{j=1}^p a\_j X\_{s-j} \right) \right] = 0
\end{aligned}\]
即\(\{\varepsilon\_t\}\)是\(\text{WN}(0,\sigma\_p^2)\)，且
\(a\_1, a\_2, \dots, a\_p\)满足最小相位条件。证毕。

○○○○○○

## 10.5 本节内容的应用意义

* 有了观测样本\(x\_1, x\_2, \dots, x\_N\)可以估计样本自协方差函数:
  \[\begin{aligned}
  \hat\gamma\_k = \frac{1}{N} \sum\_{t=1}^{N-k}
  (x\_t - \bar x)(x\_{t+k} - \bar x)
  \end{aligned}\]
* 有了\(\{\hat\gamma\_k\}\)可以计算各阶偏相关系数的估计\(\{a\_{k,k}\}\)。
* 如果发现样本偏相关系数呈现截尾性则可以拟合AR模型。
* 定理[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-minphase)保证当\(\hat\Gamma\_{p+1}\)
  正定时得到的模型系数满足最小相位条件。
* 最小相位条件保证系统是稳定的，预测有意义。
* 真实模型为AR(\(p\))时\(\hat\Gamma\_{p+1}\) a.s.正定。

## 10.6 附录：最优线性预测的Hilbert空间投影意义

设\(X\_1, X\_2, \dots, X\_n, Y\)为随机变量。
考虑估计问题
\[\begin{aligned}
L(Y | X\_1,\dots, X\_n) \stackrel{\triangle}{=}
\mathop{\mathrm{argmin}}\_{\hat Y = a\_0 + a\_1 X\_1 + \dots + a\_n X\_n} E(Y - \hat Y)^2
\end{aligned}\]
称\(L(Y | X\_1, \dots, X\_n)\)为\(Y\)关于\(X\_1, \dots, X\_n\)的**最优线性估计**。

上述最优线性预测问题等价于在\(L^2\)的闭子空间\(\text{sp}(1, X\_1, \dots, X\_n)\)求一个与\(Y\)距离最近的元素，
根据Hilbert空间投影的性质可知，
\(L(Y | X\_1, \dots, X\_n)\)是\(Y\)在\(\text{sp}(1, X\_1, \dots, X\_n)\)的投影。
\(L(Y | X\_1, \dots, X\_n)\)满足的条件是
\[
Y - L(Y | X\_1, \dots, X\_n) \perp \text{sp}(1, X\_1, \dots, X\_n)
\]
设\(L(Y | X\_1, \dots, X\_n) = a\_0 + a\_1 X\_1 + \dots + a\_n X\_n\)，则
\[
Y - a\_0 + a\_1 X\_1 + \dots + a\_n X\_n \perp 1, X\_1, \dots, X\_n
\]
也既是
\[\begin{aligned}
& E (Y - a\_0 + a\_1 X\_1 + \dots + a\_n X\_n) = 0 \\
& E X\_j (Y - a\_0 + a\_1 X\_1 + \dots + a\_n X\_n) = 0, j=1,\dots,n
\end{aligned}\]
记\(\boldsymbol a = (a\_1, \dots, a\_n)^T\),
\(\boldsymbol X = (X\_1, \dots, X\_n)^T\)，
\(\Sigma\_{XX} = \text{Var}(\boldsymbol X)\),
\(\Sigma\_{XY} = \text{Cov}(\boldsymbol X, Y)\)。
估计问题可以写成
\[
L(Y | \boldsymbol X) \stackrel{\triangle}{=}
\mathop{\mathrm{argmin}}\_{\hat Y = a\_0 + \boldsymbol a^T \boldsymbol X} E(Y - \hat Y)^2
\]
\(a\_0\)和\(\boldsymbol a\)的充分必要条件是
\[\begin{aligned}
& a\_0 = EY - \boldsymbol a^T E \boldsymbol X \\
& E [(Y - a\_0 - \boldsymbol a^T \boldsymbol X) \boldsymbol X^T] = 0
\end{aligned}\]

将第一式代入到第二式中，得
\[\begin{aligned}
E [(Y - EY - \boldsymbol a^T (\boldsymbol X - E\boldsymbol X)) \boldsymbol X^T] = 0
\end{aligned}\]
即
\[\begin{aligned}
\text{Cov}(Y, \boldsymbol X) =& \boldsymbol a^T \text{Var}(\boldsymbol X) \\
\text{Var}(\boldsymbol X) \boldsymbol a =& \text{Cov}(Y, \boldsymbol X) \\
\Sigma\_{XX} \boldsymbol a =& \Sigma\_{XY}
\end{aligned}\]
由投影的存在性知\(\boldsymbol a\)必有解，
且\(\Sigma\_{XX}>0\)时
\[
\boldsymbol a = \Sigma\_{XX}^{-1} \Sigma\_{XY}
\]
当\(|\Sigma\_{XX}|=0\)时\(\boldsymbol a\)有无穷多解，
但是得到的最佳线性预测都是同一个。

## 10.7 附录：最小相位性定理证明

来证明定理[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-minphase)的Y-W系数最小相位性。

用\(I\_n\)表示\(n\)阶单位阵，
用\(\boldsymbol 0\_n\)表示元素都是0的\(n\)阶方阵。
由平稳性，
\((X\_{m}, X\_{m-1}, \dots, X\_{m-n})\)与
\((X\_{n+1}, X\_n, \dots, X\_1)\)有相同的协方差阵\(\Gamma\_{n+1}\)，
所以用\(X\_{m-1}, \dots, X\_{m-n}\)对\(X\_m\)作最优线性预测的公式为
\[\begin{align}
\hat X\_m \stackrel{\triangle}{=}
L(X\_m | X\_{m-1}, \dots, X\_{m-n})
= \sum\_{j=1}^n a\_{nj} X\_{m-j}
\tag{10.9}
\end{align}\]
定义
\[\begin{align}
V\_m = X\_m - \hat X\_m
= X\_m - \sum\_{j=1}^n a\_{nj} X\_{m-j}
\tag{10.10}
\end{align}\]
则由\(\Gamma\_{n+1}>0\)可知
\(E V\_m^2 \stackrel{\triangle}{=} \sigma\_n^2 > 0\)。

由最优线性预测的性质（或者\(L^2\)中投影算子的性质）可知
\[
E(V\_m X\_{m-j}) = 0, j=1,2,\dots,n
\]

引入
\[
\boldsymbol Y\_m
=\left(\begin{array}{cc}
X\_m \\
X\_{m-1} \\
\vdots \\
X\_{m-n+1}
\end{array}\right)
\quad
\boldsymbol V\_m = \left(\begin{array}{cc}
V\_m \\
0 \\
\vdots \\
0
\end{array}\right)
\]
\[
A =
\left(\begin{array}{ccccccc}
a\_{n1}\ a\_{n2}\ \cdots a\_{n,n-1}\ & a\_{nn} \\
I\_{n-1} & 0
\end{array}\right)
\]
则有
\[\begin{align}
\boldsymbol Y\_m - A \boldsymbol Y\_{m-1} = \boldsymbol V\_m
\tag{10.11}
\end{align}\]
这称为AR模型的马氏扩张。
于是有
\[\begin{align}
&\left(\begin{array}{cc}
\sigma\_n^2 & 0 \nonumber \\
0 & \boldsymbol 0\_{n-1}
\end{array}\right)
= E (\boldsymbol V\_m \boldsymbol V\_m^T) \nonumber \\
=& E[ \boldsymbol V\_m (\boldsymbol Y\_m - A \boldsymbol Y\_{m-1})^T] \nonumber \\
=& E \left[ \boldsymbol V\_m \boldsymbol Y\_m^T \right]
- E[ \boldsymbol V\_m \boldsymbol Y\_{m-1}^T ] A^T \nonumber \\
=& E \left[ \boldsymbol V\_m \boldsymbol Y\_m^T \right]
\quad (\text{利用} E(V\_m X\_{m-j} = 0)) \nonumber \\
=& E \left[ (\boldsymbol Y\_m - A \boldsymbol Y\_{m-1})
\boldsymbol Y\_m^T \right] \nonumber \\
=& E(\boldsymbol Y\_m \boldsymbol Y\_m^T)
- A E(\boldsymbol Y\_{m-1} \boldsymbol Y\_m^T) \nonumber \\
=& \Gamma\_n - A E \left[ \boldsymbol Y\_{m-1}
(A \boldsymbol Y\_{m-1} + \boldsymbol V\_m)^T \right] \nonumber \\
=& \Gamma\_n - A E(\boldsymbol Y\_{m-1} \boldsymbol Y\_{m-1}^T) A^T
- A E( \boldsymbol Y\_{m-1} \boldsymbol V\_{m}^T) \nonumber \\
=& \Gamma\_n - A \Gamma\_n A^T
\quad (\text{再次利用} E(V\_m X\_{m-j} = 0))
\tag{10.12}
\end{align}\]

设复数\(z\_0\)使得\(\text{det}(I\_n - z\_0 A)=0\)，
方程即
\[\begin{aligned}
& \text{det}\left(\begin{array}{\*9c}
1 - a\_{n1} z\_0 & - a\_{n2} z\_0 & - a\_{n3} z\_0 & \cdots & -a\_{n,n-1} z\_0 & - a\_{nn} z\_0 \\
-z\_0 & 1 & 0 & \cdots & 0 & 0 \\
0 & -z\_0 & 1 & \cdots & 0 & 0 \\
\vdots & \vdots & \vdots & \ddots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & -z\_0 & 1
\end{array}\right) \\
=& 1 - a\_{n1} z\_0 - a\_{n2} z\_0^2 - \dots - a\_{nn} z\_0^n = 0
\end{aligned}\]
当\(z\_0 = 0\)时行列式等于1，
所以如果\(z\_0\)是方程的根则\(z\_0 \neq 0\)。
要证明最小相位性，
只要证明所有使得上面方程成立的\(z\_0\)都满足\(|z\_0| > 1\)。

因为行列式\(\text{det}(I\_n - z\_0 A)=0\)，
必存在非零复向量
\(\boldsymbol \alpha^\* = (\alpha\_1, \alpha\_2, \dots, \alpha\_n)\)
使得
\[
\boldsymbol \alpha^\* (I\_n - z\_0 A) = 0
\]
即
\[
\boldsymbol \alpha^\* A = z\_0^{-1} \boldsymbol \alpha^\*
\]
由矩阵\(A\)的结构，
这可以写成
\[\begin{equation}
\begin{cases}
a\_{n1} \alpha\_1 + \alpha\_2 = z\_0^{-1} \alpha\_1 \\
a\_{n2} \alpha\_1 + \alpha\_3 = z\_0^{-1} \alpha\_2 \\
\vdots \\
a\_{n,n-1} \alpha\_1 + \alpha\_{n} = z\_0^{-1} \alpha\_{n-1} \\
a\_{nn} \alpha\_1 = z\_0^{-1} \alpha\_{n}
\end{cases}
\tag{10.13}
\end{equation}\]
由此可知\(\alpha\_1 \neq 0\)，
如果不然，
则由[(10.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:pacflev-app-minphase-proof5)
递推可得\(0 = \alpha\_1 = \alpha\_2 = \dots = \alpha\_n = 0\)，
这与\(\boldsymbol\alpha^\*\)非零矛盾。

利用\(\sigma\_n^2 = E V\_m^2 > 0\)和[(10.12)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:pacflev-app-minphase-proof4)
及\(\boldsymbol\alpha^\* A = z\_0^{-1} \boldsymbol\alpha^\*\)有
\[\begin{aligned}
0 <& \alpha\_1 \sigma\_n^2 \alpha\_1^\* \\
=& \boldsymbol\alpha^\*
\left(\begin{array}{cc}
\sigma\_n^2 & 0 \nonumber \\
0 & \boldsymbol 0\_{n-1}
\end{array}\right)
\boldsymbol\alpha \\
=& \boldsymbol\alpha^\* \Gamma\_n \boldsymbol\alpha
- \boldsymbol\alpha^\* A \Gamma\_n A^T \boldsymbol\alpha \\
=& \boldsymbol\alpha^\* \Gamma\_n \boldsymbol\alpha
- |z\_0|^{-2} \boldsymbol\alpha^\* \Gamma\_n \boldsymbol\alpha \\
=& (1 - |z\_0|^{-2}) \boldsymbol\alpha^\* \Gamma\_n \boldsymbol\alpha
\end{aligned}\]
由\(\Gamma\_n\)的正定性知\(\boldsymbol\alpha^\* \Gamma\_n \boldsymbol\alpha > 0\)，
所以\(1 - |z\_0|^{-2} > 0\)，
即有\(|z\_0| > 1\)。
定理证毕。

○○○○○○

## 10.8 附录：AR序列的等价定义

**定理10.4 (AR序列的等价定义)** 设\(a\_1, \dots, a\_p\)为实数，\(a\_p \neq 0\)，
\(A(z) = 1 - a\_1 z - \dots a\_p z^p\),
\(\{ \varepsilon\_t \}\)为WN(0, \(\sigma^2\))，
零均值平稳列\(\{ X\_t, t \in \mathbb Z \}\)
满足
\[
A(\mathscr B) X\_t = \varepsilon\_t,
\ t \in \mathbb Z
\]
如果\(\varepsilon\_{t+k}, k=1,2,\dots\)与\(\{ X\_s, s \leq t \}\)互不相关，
则\(A(z)\)的根都在单位圆外，
从而\(\{X\_t \}\)为AR(\(p\))序列。

**证明**
\(L(X\_t | X\_{t-1}, \dots, X\_{t-p})\)即\(X\_t\)到
\(\text{sp}(X\_{t-1}, \dots, X\_{t-p})\)的投影，
由投影的线性性质及\(\varepsilon\_t\)与\(X\_{t-1}, \dots, X\_{t-p}\)正交可得
\[
L(X\_t | X\_{t-1}, \dots, X\_{t-p})
= a\_1 X\_{t-1} + \dots + a\_p X\_{t-p} + 0
\]
可知用\(X\_{t-1}, \dots, X\_{t-p}\)预测\(X\_t\)的Y-W系数为\((a\_1, \dots, a\_p)\)，
又
\[
E(X\_t - L(X\_t | X\_{t-1}, \dots, X\_{t-p}))^2
= E \varepsilon\_t^2 = \sigma^2 > 0
\]
所以\(X\_t, X\_{t-1}, \dots, X\_{t-p}\)线性无关，
有\(\Gamma\_{p+1} > 0\)。
由定理[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-minphase)可知\(A(z)\)满足最小相位性，
因此\(\{ X\_t \}\)是AR(\(p\))序列。

○○○○○○

## 10.9 Levinson递推公式证明

来证明定理[10.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-levinson)。

\(a\_{1,1} = \gamma\_1 / \gamma\_0\)显然，
设对\(k\)(\(k \leq n-1\))已知
\[\begin{aligned}
a\_{k+1,k+1} =&
\frac{\gamma\_{k+1} - a\_{k,1} \gamma\_k - a\_{k,2} \gamma\_{k-1}
- \dots - a\_{k,k} \gamma\_1}{
\gamma\_0 - a\_{k,1} \gamma\_1 - a\_{k,2} \gamma\_2 - \dots
- a\_{k,k} \gamma\_k } \\
a\_{k+1,j} =& a\_{k,j} - a\_{k+1,k+1} a\_{k,k+1-j}, \quad 1 \leq j \leq k
\end{aligned}\]

引入正交阵
\[
T = \begin{pmatrix}
0 & \cdots & 0 & 1 \\
0 & \cdots & 1 & 0 \\
\vdots & & \vdots & \vdots \\
1 & \cdots & 0 & 0
\end{pmatrix},
\]
则\(T A\)的结果是将\(A\)的各行颠倒次序，
\(A T\)的结果是将\(A\)的各列颠倒次序，
且\(T^T = T^{-1} = T\)。
由\(\Gamma\_k\)的结构易见
\[
T \Gamma\_k T = \Gamma\_k,
\quad \boldsymbol g\_k = T \boldsymbol \gamma\_k
= (\gamma\_k, \dots, \gamma\_1)^T .
\]
再引入
\[\begin{aligned}
\Gamma\_{k+1}
=& \begin{pmatrix}
\Gamma\_k & \boldsymbol g\_k \\
\boldsymbol g\_k^T & \gamma\_0
\end{pmatrix}
= \begin{pmatrix}
\Gamma\_k & T \boldsymbol \gamma\_k \\
\boldsymbol \gamma\_k^T T & \gamma\_0
\end{pmatrix}, \\
\boldsymbol a\_{k+1}^{[1:k]}
=& (a\_{k+1,1}, \dots, a\_{k+1,k})^T, \\
\boldsymbol a\_{k+1} =&
\begin{pmatrix}
\boldsymbol a\_{k+1}^{[1:k]} \\
a\_{k+1,k+1}
\end{pmatrix}, \\
\boldsymbol \gamma\_{k+1} =&
\begin{pmatrix}
\boldsymbol\gamma\_k \\
\gamma\_{k+1}
\end{pmatrix} . \\
\end{aligned}\]
利用上述记号将\(k+1\)阶Y-W方程写成
\[
\begin{pmatrix}
\Gamma\_k & \boldsymbol g\_k \\
\boldsymbol g\_k^T & \gamma\_0
\end{pmatrix}
\begin{pmatrix}
\boldsymbol a\_{k+1}^{[1:k]} \\
a\_{k+1,k+1}
\end{pmatrix}
= \begin{pmatrix}
\boldsymbol\gamma\_k \\
\gamma\_{k+1}
\end{pmatrix} .
\]
分别写成
\[\begin{align}
\Gamma\_k \boldsymbol a\_{k+1}^{[1:k]}
+ a\_{k+1,k+1} \boldsymbol g\_k =& \boldsymbol\gamma\_k,
\tag{10.14} \\
\boldsymbol g\_k^T \boldsymbol a\_{k+1}^{[1:k]}
+ a\_{k+1,k+1} \gamma\_0 =& \gamma\_{k+1} .
\tag{10.15}
\end{align}\]
由[(10.14)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:pacflev-app-levpr5)得
\[\begin{align}
\boldsymbol a\_{k+1}^{[1:k]}
=& \Gamma\_k^{-1} \boldsymbol\gamma\_k
- a\_{k+1,k+1} \Gamma\_k^{-1} \boldsymbol g\_k \\
=& \Gamma\_k^{-1} \boldsymbol\gamma\_k
- a\_{k+1,k+1} \Gamma\_k^{-1} T \boldsymbol \gamma\_k \\
=& \boldsymbol a\_k
- a\_{k+1,k+1} (T \Gamma\_k T)^{-1} T \boldsymbol \gamma\_k \\
=& \boldsymbol a\_k
- a\_{k+1,k+1} T \Gamma\_k^{-1} \boldsymbol \gamma\_k \\
=& \boldsymbol a\_k
- a\_{k+1,k+1} T \boldsymbol a\_k .
\tag{10.16}
\end{align}\]
将[(10.16)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:pacflev-app-levpr7)代入[(10.15)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:pacflev-app-levpr6)，得
\[
\boldsymbol g\_k^T (\boldsymbol a\_k - a\_{k+1,k+1} T \boldsymbol a\_k)
+ a\_{k+1,k+1} \gamma\_0 = \gamma\_{k+1},
\]
即
\[
\boldsymbol g\_k^T \boldsymbol a\_k
- a\_{k+1,k+1} \boldsymbol g\_k^T T \boldsymbol a\_k
+ a\_{k+1,k+1} \gamma\_0 = \gamma\_{k+1},
\]
化简得
\[\begin{aligned}
a\_{k+1,k+1}(\gamma\_0 - \boldsymbol a\_k^T \boldsymbol \gamma\_k)
=& \gamma\_{k+1} - \boldsymbol a\_k^T \boldsymbol g\_k, \\
a\_{k+1,k+1}
=& \frac{\gamma\_{k+1} - \boldsymbol a\_k^T \boldsymbol g\_k}{
\gamma\_0 - \boldsymbol a\_k^T \boldsymbol \gamma\_k} \\
=& \frac{\gamma\_{k+1} - a\_{k+1,1} \gamma\_k - \dots - a\_{k+1,k} \gamma\_1}{
\gamma\_{0} - a\_{k+1,1} \gamma\_1 - \dots - a\_{k+1,k} \gamma\_k} .
\end{aligned}\]
这就证明了\(a\_{k+1,k+1}\)的公式，
而[(10.16)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#eq:pacflev-app-levpr7)就是\(a\_{k+1,j}\), \(1 \leq j \leq k\)的公式。

再来证明\(\sigma\_k\)的公式。
\(\sigma\_0^2 = \gamma\_0\)显然，
记
\[
\boldsymbol X\_k = (X\_k, X\_{k-1}, \dots, X\_1)^T,
\]
则按定义有
\[\begin{aligned}
\sigma\_k^2
=& E[X\_{k+1} - \boldsymbol a\_k^T \boldsymbol X\_k]^2 \\
=& E[(X\_{k+1} - \boldsymbol a\_k^T \boldsymbol X\_k) X\_{k+1}]
- E[(X\_{k+1} - \boldsymbol a\_k^T \boldsymbol X\_k) \boldsymbol X\_k^T \boldsymbol a\_k] \\
=& \gamma\_0 - \boldsymbol a\_k^T \boldsymbol\gamma\_k
- (\boldsymbol\gamma\_k^T - \boldsymbol a\_k^T \Gamma\_k) \boldsymbol a\_k\\
=& \gamma\_0 - \boldsymbol a\_k^T \boldsymbol\gamma\_k - 0 \boldsymbol a\_k\\
=& \gamma\_0 - \boldsymbol a\_k^T \boldsymbol\gamma\_k .
\end{aligned}\]
于是关于\(\sigma\_{k+1}^2\)有
\[\begin{aligned}
\sigma\_{k+1}^2
=& \gamma\_0 - \boldsymbol a\_{k+1} \boldsymbol\gamma\_{k+1} \\
=& \gamma\_0
- (\boldsymbol [\boldsymbol a\_{k+1}^{[1:k]}]^T, a\_{k+1,k+1})
\begin{pmatrix}
\boldsymbol\gamma\_k \\
\gamma\_{k+1}
\end{pmatrix} \\
=& \gamma\_0
- [\boldsymbol a\_{k+1}^{[1:k]}]^T \boldsymbol\gamma\_k
- a\_{k+1,k+1} \gamma\_{k+1} \\
=& \gamma\_0 - \boldsymbol a\_k^T \boldsymbol\gamma\_k
+ a\_{k+1,k+1} \boldsymbol a\_k^T T \boldsymbol\gamma\_k
- a\_{k+1,k+1} \gamma\_{k+1} \\
=& \sigma\_k^2
- a\_{k+1,k+1}(\gamma\_{k+1} - \boldsymbol a\_k^T \boldsymbol g\_k) \\
=& \sigma\_k^2
- a\_{k+1,k+1}[a\_{k+1,k+1}(\gamma\_0 - \boldsymbol a\_k^T \boldsymbol g\_k)] \\
=& \sigma\_k^2 - a\_{k+1,k+1}^2 \sigma\_k^2
= \sigma\_k^2(1 - a\_{k+1,k+1}^2) .
\end{aligned}\]
得证。

## 10.10 附录：离散谱序列的预测

考虑离散谱序列
\[
X\_t = A \cos(\omega t) + B \sin(\omega t),
t \in \mathbb Z,
\]
其中\(0<\omega<\pi\),
\(A, B\)是互不相关的零均值随机变量，
\(\text{Var}(A)=\text{Var}(B)=\sigma^2\)。
这是零均值平稳列，
自协方差函数为
\[
\gamma\_k = \sigma^2 \cos(k \omega), k=0,1,2, \dots
\]
考虑最优线性预测问题。

### 10.10.1 直接求解Y-W方程

为了用\(X\_1\)预测\(X\_2\)，
只要求解
\[
\gamma\_0 a\_{11} = \gamma\_1
\]
即可得\(a\_{11} = \cos\omega\)，
\[
L(X\_2|X\_1) = a\_{11} X\_1 = \cos\omega \cdot X\_1
\]
预测的均方误差为
\[\begin{aligned}
\sigma\_2^2
=& E(X\_2 - \cos\omega \cdot X\_1)^2 \\
=& \sigma^2 (\cos 2\omega - \cos\omega \cos\omega)^2
+ \sigma^2 (\sin 2\omega - \cos\omega \sin\omega)^2 \\
=& \sigma^2 ( \sin^4 \omega + \cos^2 \omega \sin^2 \omega) \\
=& \sigma^2 \sin^2 \omega > 0
\end{aligned}\]

考虑用\(X\_2, X\_1\)预测\(X\_3\)的问题。
\[
\Gamma\_2 = \left(\begin{array}{cc}
1 & \cos\omega \\
\cos\omega & 1
\end{array}\right)
\]
\(|\Gamma\_2| = 1 - \cos^2 \omega = \sin^2\omega > 0\)，
所以\(\Gamma\_2 > 0\)。
求解方程

\[
\left(\begin{array}{cc}
1 & \cos\omega \\
\cos\omega & 1
\end{array}\right)
\left(\begin{array}{c}
a\_{21} \\
a\_{22}
\end{array}\right)
=
\left(\begin{array}{c}
\cos\omega \\
\cos 2\omega
\end{array}\right)
\]

即
\[\begin{aligned}
& a\_{21} + \cos\omega \cdot a\_{22} = \cos\omega \\
& \cos\omega \cdot a\_{21} + a\_{22} = \cos 2\omega \\
& a\_{22} = \cos 2\omega - \cos\omega \cdot a\_{21} \\
& a\_{21} + \cos\omega(\cos 2\omega - \cos\omega \cdot a\_{21}) = \cos\omega \\
& a\_{21}+ \cos\omega \cos 2\omega - \cos^2 \omega \cdot a\_{21} = \cos\omega \\
& \sin^2 \omega \cdot a\_{21} = \cos\omega(1 - \cos 2\omega)
= 2 \cos\omega \sin^2 \omega \\
& a\_{21} = 2 \cos\omega \\
& a\_{22} = \cos 2\omega - \cos\omega \cdot a\_{21}
= \cos 2\omega - \cos\omega \cdot 2 \cos\omega
= -1
\end{aligned}\]
其中用到三角函数公式\(\cos 2\omega = 2 \cos^2 \omega - 1 = 1 - 2 \sin^2 \omega\)。

于是\(X\_3\)的最优线性预测为
\[
\hat X\_3
= a\_{21} X\_2 + a\_{22} X\_1
= 2 \cos\omega \cdot X\_2 - X\_1
\]

预测的均方误差为

\[\begin{aligned}
\sigma\_2^2 =& E(X\_3 - 2 \cos\omega \cdot X\_2 + X\_1)^2 \\
=& \sigma^2 \left( \cos 3\omega - 2 \cos\omega \cos 2\omega + \cos\omega \right)^2 \\
& + \sigma^2 \left( \sin 3\omega - 2 \cos\omega \sin 2\omega + \sin\omega \right)^2 \\
=&0
\end{aligned}\]
所以离散谱序列可完全线性预测。
对任意\(t\)，
为了用\(X\_{t-1}, \dots, X\_{t-p}\)预测\(X\_t\)，
当\(p \geq 2\)时有
\[
L(X\_t | X\_{t-1}, \dots, X\_{t-p})
= 2 \cos\omega \cdot X\_{t-1} - X\_{t-2}
\]
且\(L(X\_t | X\_{t-1}, \dots, X\_{t-p}) = X\_t\)，
预测误差为零。

事实上，\(\Gamma\_3\)为
\[
\left(\begin{array}{ccc}
1 & \cos\omega & \cos 2\omega \\
\cos\omega & 1 & \cos\omega \\
\cos 2\omega & \cos\omega & 1
\end{array}\right)
\]
记\(b = \cos\omega\)，
则\(\cos 2\omega = 2 b^2 - 1\)，
行列式为

\[\begin{aligned}
& \left|\begin{array}{ccc}
1 & b & 2b^2 - 1 \\
b & 1 & b \\
2 b^2 - 1 & b & 1
\end{array}\right| \\
=& \left|\begin{array}{ccc}
1 & b & 2b^2 - 1 \\
0 & 1 - b^2 & 2b - 2b^2 \\
0 & 2b - 2b^2 & -4b^4 + 4b^2
\end{array}\right| \\
=& (1-b^2)^2
\left|\begin{array}{cc}
1 & 2b \\
2b & 4b^2
\end{array}\right| \\
=& 0
\end{aligned}\]

### 10.10.2 用Levinson递推求解Y-W方程

\(\gamma\_0 = \sigma^2\),
\(\gamma\_1 = \sigma^2 \cos\omega\),
\(\gamma\_2 = \sigma^2 \cos 2\omega\)。

按照Levinson递推公式的递推次序依次计算：
\[\begin{aligned}
\text{初值}: & \\
\sigma\_0^2 =& \gamma\_0 = \sigma^2 \\
k+1=1: & \\
a\_{11} =& \frac{\gamma\_1}{\gamma\_0} = \cos\omega \\
\sigma\_1^2 =& E[ X\_2 - L(X\_2 | X\_1) ]^2 \\
=& \sigma\_0^2 [ 1 - a\_{11}^2 ] = \sigma^2 \sin^2 \omega \\
k+1=2: & \\
a\_{22} =& \frac{\gamma\_2 - a\_{11} \gamma\_1}{\sigma\_1^2} \\
=& \frac{\cos 2\omega - \cos\omega \cos\omega}{\sin^2 \omega} \\
=& -1 \\
a\_{21} =& a\_{11} - a\_{22} a\_{11} \\
=& \cos\omega [ 1 - (-1) ] = 2 \cos\omega \\
\sigma\_2^2 =& E[ X\_3 - L(X\_3 | X\_2, X\_1) ]^2 \\
=& \sigma\_1^2 [ 1 - a\_{22}^2 ] \\
=& \sigma^2 \sin^2 \omega [1 - (-1)^2] \\
=& 0
\end{aligned}\]

## 10.11 附录：Y-W方程反解讨论

Y-W方程中如果\(\Gamma\_{p+1}>0\)则
\(a\_1, \dots, a\_p, \sigma^2\)唯一确定,
其中\(a\_1, \dots, a\_p\)满足最小相位条件
（见定理[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#thm:arpacfyw-minphase)），
\(\sigma^2>0\)。

反过来，
如果\(a\_1, \dots, a\_p\)满足最小相位条件，
\(\sigma^2>0\),
Y-W方程中的\(\gamma\_0, \gamma\_1,\dots, \gamma\_p\)
是否唯一确定？

参考：谢衷洁《时间序列分析》P.189定理4.4。
定理说明，给定某平稳列的前\(p+1\)个自协方差
\(\gamma\_0, \gamma\_1,\dots, \gamma\_p\),
必存在AR\((p)\)序列使其前\(p+1\)个自协方差函数等于
这\(p+1\)个，模型参数由Y-W解出。
见习题6.1.2。
另外，该参考书定理4.5说明在前\(p+1\)个自协方差函数等于给定的这
\(p+1\)个的所有平稳列中，AR(\(p\))模型的一步预测误差达到最大，
从而信息量最大。

更一般地，对非可完全线性预测平稳列\(\{X\_t\}\)的自协方差列
\(\{\gamma\_k \}\)，有各阶Y-W方程：
\[\begin{align\*}
\Gamma\_n \boldsymbol a\_n =& \boldsymbol\gamma\_n \\
\gamma\_0 - \boldsymbol a\_n^T \boldsymbol\gamma\_n =& \sigma\_n^2
\end{align\*}\]
其中\(\boldsymbol\gamma\_n = (\gamma\_1, \dots, \gamma\_n)^T\)。
假设\(\boldsymbol a\_n = (a\_{n1}, a\_{n2}, \dots, a\_{nn})^T\)
和\(\sigma\_n^2>0\)给定，
\(\boldsymbol a\_n\)满足最小相位条件，
则满足上述Y-W方程的\(\gamma\_0, \gamma\_1, \dots, \gamma\_n\)是否唯一确定？

对\(n=1\)，
显然
\[\begin{align\*}
\gamma\_0 =& \frac{\sigma^2}{1-a\_1^2} \\
\gamma\_1 =& a\_1 \gamma\_0
\end{align\*}\]
唯一。

对\(n=2\)，方程为
\[\begin{align\*}
\gamma\_0 a\_1 + \gamma\_1 a\_2 =& \gamma\_1 \\
\gamma\_1 a\_1 + \gamma\_0 a\_2 =& \gamma\_2 \\
\gamma\_0 - a\_1 a\_1 \gamma\_1 - a\_2 \gamma\_2 =& \sigma^2
\end{align\*}\]
消元得
\[\begin{align\*}
\gamma\_0 =& \sigma^2 / \left(
1 - a\_2^2 - \frac{(1+a\_2) a\_1^2}{1-a\_2} \right) \\
\gamma\_1 =& \frac{a\_1}{1-a\_2} \gamma\_0 \\
\gamma\_2 =& a\_1 \gamma\_1 + a\_2 \gamma\_0
\end{align\*}\]

对\(n=3\)，因为\(\gamma\_0>0\)，\(\gamma\_k = \rho\_k \gamma\_0\)，
所以如果能由\(a\_1, a\_2, a\_3\)决定\(\rho\_1, \rho\_2, \rho\_3\)则
可由
\[\begin{align\*}
\sigma\_3^2 = \gamma\_0 - a\_1 \gamma\_1 - a\_2 \gamma\_2 - - a\_3 \gamma\_3
= \gamma\_0 (1 - a\_1 \rho\_1 - a\_2 \rho\_2 - a\_3 \rho\_3)
\end{align\*}\]
解出\(\gamma\_0\)。
把
\[\begin{align\*}
\left(\begin{array}{ccc}
\gamma\_0 & \gamma\_1 & \gamma\_2 \\
\gamma\_1 & \gamma\_0 & \gamma\_1 \\
\gamma\_2 & \gamma\_1 & \gamma\_0
\end{array}\right)
\left(\begin{array}{c}
a\_1 \\ a\_2 \\ a\_3
\end{array}\right)
= \left(\begin{array}{c}
\gamma\_1 \\ \gamma\_2 \\ \gamma\_3
\end{array}\right)
\end{align\*}\]
两边除以\(\gamma\_0\)并写成关于\(\rho\_1, \rho\_2, \rho\_3\)的方程，
得
\[\begin{align\*}
\left(\begin{array}{ccc}
a\_2 - 1 & 0 & a\_3 \\
a\_1 + a\_3 & -1 & 0 \\
a\_2 & a\_1 & -1
\end{array}\right)
\left(\begin{array}{c}
\rho\_1 \\ \rho\_2 \\ \rho\_3
\end{array}\right)
= -\left(\begin{array}{c}
a\_1 \\ a\_2 \\ a\_3
\end{array}\right)
\end{align\*}\]
很难判别此三元一次方程组的系数矩阵是否满秩。
可以计算其行列式为
\[\begin{align\*}
a\_1^2 a\_3 + a\_1 a\_3^2 + a\_2 a\_3 + a\_2 - 1
\end{align\*}\]
这个矩阵可能有不满秩的情况，例如当
\[\begin{align\*}
A(z) = 1 - 1.8z + 1.775789z^2 - 0.9z^3
\end{align\*}\]
时，此矩阵行列式为零，且\(A(z)\)满足最小相位条件。