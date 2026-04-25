---
crawl_time: '2026-01-17 14:29:15'
framework: sphinx
title: 24 时间序列的递推预测 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 24 时间序列的递推预测

§[10.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-pacflev.html#pacflev-bestlinpred)已经用Y-W方程给出了非决定性平稳序列的预测公式，
这里我们进行更深入的讨论。

* 非决定性平稳序列可以根据自协方差函数用Levinson递推得到预测系数和预测方差；
* 对AR(\(p\))模型只要使用自回归系数预测；
* 对MA模型和ARMA模型则只能递推得到预测系数。

这里研究可以化简MA模型和ARMA模型预测的公式,
以及非平稳时也可用的递推公式。
假设自协方差函数已知，
实际中可以用样本自协方差函数代替。

## 24.1 一般时间序列的递推预测

### 24.1.1 递推预测的正交分解

设\(\{Y\_t\}\)是方差有限的零均值时间序列,
对任何正整数\(n\), 用
\[
L\_n=\overline{\text{span}}\{Y\_1,Y\_2,\dots,Y\_n\}
\]
表示\(Y\_1,\dots,Y\_n\)的线性组合的全体.
定义\(\boldsymbol{Y}\_n = (Y\_1,Y\_2,\dots,Y\_n)^T\),
\[\begin{align}
\hat Y\_1=0,
\ \hat Y\_{n}=L(Y\_n|\boldsymbol{Y}\_{n-1}),
\ n=2,\dots.
\tag{24.1}
\end{align}\]
引入预测误差\(W\_n\)及其方差\(\nu\_{n-1}\)如下:
\[\begin{align}
W\_n= Y\_n-\hat Y\_n,
\ \ \nu\_{n-1}= EW\_n^2.
\ \ n=1,2,\cdots .
\tag{24.2}
\end{align}\]
由最佳线性预测的性质 7 知道\(W\_n\)和\(L\_{n-1}\)中的任何随机变量正交,
并且\(W\_n \in L\_n\).
于是
\(\{W\_n\}\)是一个正交序列, 满足
\[
E(W\_n W\_k)=\nu\_{n-1} \delta\_{n-k}.
\]
这里\(\delta\_t\)是Kronecker 函数.

用
\[
M\_n \stackrel{\triangle}{=} \overline{\text{span}}\{W\_1,W\_2,\dots,W\_n\}
\]
表示\(W\_1,W\_2,\cdots,W\_n\)的线性组合全体.
则\(M\_n \subset L\_n\).

对\(n\in \mathbb N\),
我们用归纳法证明\(Y\_n\in M\_n\).

首先\(Y\_1=W\_1 \in M\_1\).
如果对\(k\leq n\)已经证明\(Y\_k\in M\_k\),
注意\(\hat Y\_{n+1}=L(Y\_{n+1}| \boldsymbol{Y}\_n) \in M\_n\), 于是
\[
Y\_{n+1}=(Y\_{n+1}-\hat Y\_{n+1})
+ \hat Y\_{n+1}= W\_{n+1} + \hat Y\_{n+1} \in M\_{n+1}.
\]
这就证明了对\(n\in \mathbb N\), \(Y\_n \in M\_n\)成立.

于是得到
\[\begin{align}
L\_n =& \overline{\text{span}}\{Y\_1,Y\_2,\dots,Y\_n\} \\
=& \overline{\text{span}}\{W\_1,W\_2,\dots,W\_n\}=M\_n, \\
& n = 1,2,\dots
\tag{24.3}
\end{align}\]

§[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#blpprop-blp)性质10和[(24.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-ortheq0303)式告诉我们用
\(\boldsymbol{W}\_n =(W\_1,W\_2,\dots,W\_{n})^T\)
对\(Y\_{n+1}\)进行预测和用
\(\boldsymbol{Y}\_n =(Y\_1,Y\_2,\dots,Y\_{n})^T\) 对\(Y\_{n+1}\)
进行预测是等价的.
由于\(\{W\_t\}\)是正交序列,
所以用\(\boldsymbol{W}\_n\)对\(Y\_{n+1}\)进行预测有很多的方便.
类似于在正交基上的投影，可以直接计算坐标。

### 24.1.2 递推预测定理

**定理24.1** 设\(\{Y\_t\}\)是零均值时间序列(不要求平稳!).
如果\((Y\_1,Y\_2,\dots,Y\_{m+1})^T\)的协方差矩阵
\[\begin{align}
\Big( E(Y\_s Y\_t)\Big )\_{1\leq s,t \leq m+1}
\tag{24.4}
\end{align}\]
正定,
则最佳线性预测
\[\begin{align}
\hat Y\_{n+1} \stackrel{\triangle}{=}& L(Y\_{n+1}|\boldsymbol Y\_n),
\ n=1,2,\dots,m \nonumber \\
=& \sum\_{j=1}^n \theta\_{n,j} W\_{n+1-j}
= \sum\_{k=0}^{n-1} \theta\_{n,n-k} W\_{k+1}
\tag{24.5} \\
= & \theta\_{n,1} W\_n + \theta\_{n,2} W\_{n-1}
+ \dots + \theta\_{n,n}W\_1 \\
=& \theta\_{n,n} W\_1 + \theta\_{n,n-1} W\_2 + \dots + \theta\_{n,1} W\_n
\nonumber
\end{align}\]
其中的系数\(\{\theta\_{n,j}\}\)
和预测的均方误差\(\nu\_n=EW\_{n+1}^2\)满足如下的递推公式.
\[\begin{align}
\begin{cases}
& \nu\_0=EY\_1^2, \\
& \theta\_{n,n-k}
=\left[ E(Y\_{n+1}Y\_{k+1})
- \sum\_{j=0}^{k-1} \theta\_{k,k-j}\theta\_{n,n-j}\nu\_j \right] / \nu\_k, \\
& \qquad\qquad 0\leq k \leq n-1,\\
&\nu\_n = EY\_{n+1}^2
- \sum\_{k=0}^{n-1}\theta\_{n,n-k}^2\nu\_k,
\end{cases}
\tag{24.6}
\end{align}\]
其中约定\(\sum\_{j=0}^{-1}(\cdot)\stackrel{\triangle}{=} 0\).

递推的顺序是
\[
\begin{array}{lllll}
\nu\_0 \\
\theta\_{1,1} & \nu\_1 \\
\theta\_{2,2} & \theta\_{2,1} & \nu\_2 \\
\theta\_{3,3} & \theta\_{3,2} & \theta\_{3,1} & \nu\_3 \\
\vdots & \vdots & \vdots & \vdots & \ddots
\end{array}
\]
从协方差可以递推计算系数\(\{\theta\_{n,k} \}\)和\(\{\nu\_n\}\)，
并递推计算
\[\begin{aligned}
\hat Y\_1 =& 0, & W\_1 =& Y\_1 - \hat Y\_1 \\
\hat Y\_2 =& \theta\_{1,1} W\_1, & W\_2 =& Y\_2 - \hat Y\_2 \\
\hat Y\_3 =& \theta\_{2,2} W\_1 + \theta\_{2,1} W\_2, & W\_3 =& Y\_3 - \hat Y\_3 \\
\hat Y\_4 =& \theta\_{3,3} W\_1 + \theta\_{3,2} W\_2 + \theta\_{3,1} W\_3,
& W\_4 =& Y\_4 - \hat Y\_4 \\
\cdots & \cdots \cdots & \cdots & \cdots \cdots
\end{aligned}\]

**证明**

从自协方差矩阵[(24.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-varmat0304)的正定性知道\(\nu\_n=EW\_{n+1}^2 >0\).
以下设\(0\leq k\leq n-1\).
在[(24.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-orthdec0305)两边同乘\(W\_{k+1}\)后求数学期望,
由\(\{W\_k\}\)的正交性得
\[\begin{align}
E(\hat Y\_{n+1}W\_{k+1})
=& \theta\_{n,n-k}\nu\_k.
\tag{24.7}
\end{align}\]
利用\(W\_{n+1}=Y\_{n+1}-\hat Y\_{n+1}\)和\(W\_{k+1}\)垂直,
得
\[\begin{align}
E(Y\_{n+1} W\_{k+1})= E(\hat Y\_{n+1}W\_{k+1})
= \theta\_{n,n-k} \nu\_k
\tag{24.8}
\end{align}\]

注意到
\[
\hat Y\_{k+1}
=\sum\_{j=1}^{k} \theta\_{k,j}W\_{k+1-j}
=\sum\_{j=0}^{k-1} \theta\_{k,k-j}W\_{j+1},
\]
于是利用[(24.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-YWE0307p)可得
\[\begin{aligned}
\theta\_{n,n-k}=& E(Y\_{n+1}W\_{k+1})/\nu\_k \\
=& E\left[ Y\_{n+1}(Y\_{k+1} - \sum\_{j=0}^{k-1} \theta\_{k,k-j}W\_{j+1})
\right] / \nu\_k \\
=& \left[ E(Y\_{n+1}Y\_{k+1}) - \sum\_{j=0}^{k-1} \theta\_{k,k-j}E(Y\_{n+1} W\_{j+1})
\right] / \nu\_k \\
=& [E(Y\_{n+1}Y\_{k+1})
- \sum\_{j=0}^{k-1}\theta\_{k,k-j}\theta\_{n,n-j}\nu\_j]/\nu\_k.
\end{aligned}\]

最后, 利用 \(\nu\_n=EW^2\_{n+1}= EY\_{n+1}^2- E\hat Y\_{n+1}^2\)和[(24.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-orthdec0305)得到预测的均方误差公式:
\[
\nu\_n = EY\_{n+1}^2 - \sum\_{j=1}^{n}\theta\_{n,j}^2\nu\_{n-j}
= EY\_{n+1}^2 - \sum\_{j=0}^{n-1}\theta\_{n,n-j}^2\nu\_j.
\]

○○○○○○

### 24.1.3 多步预报问题

下面考虑用\(\{Y\_1,Y\_2,\cdots,Y\_n\}\)预测\(Y\_{n+k+1}\)的问题.
设\((Y\_1,Y\_2,\dots,Y\_{n+k+1})^T\)的自协方差矩阵正定.
仍记\(\hat Y\_{n+k+1} = L(Y\_{n+k+1}|\boldsymbol{Y}\_{n+k})\),
用\(W\_{j}\)表示预测误差\(Y\_j - L(Y\_j|\boldsymbol{Y}\_{j-1})\),
则有
\[\begin{align}
\hat Y\_{n+k+1} = \sum \_{j=1}^{n+k}\theta\_{n+k,j} W\_{n+k+1-j}.
\tag{24.9}
\end{align}\]
注意, 对\(j \geq 0\), \(W\_{n+j+1}\)垂直于\(L\_n\),
\(W\_{n-j} \in L\_n\).
根据§[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#blpprop-blp)中最佳线性预测的性质4、5、8或定理[22.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-proj-prop)得到
\[\begin{align}
&L(Y\_{n+k+1}|\boldsymbol{Y}\_n) = L(\hat Y\_{n+k+1}|\boldsymbol{Y}\_n) \nonumber \\
=& L [\sum\_{j=1}^{n+k} \theta\_{n+k,j} W\_{n+k+1-j}\ \Big| \ \boldsymbol{W}\_n ]
\nonumber \\
=&L [\sum \_{j=k+1}^{n+k} \theta\_{n+k,j} W\_{n+k+1-j} \ \big | \ \boldsymbol W\_n ]
\nonumber \\
=&\sum \_{j=k+1}^{n+k}\theta\_{n+k,j} W\_{n+k+1-j}.
\tag{24.10}
\\
=& \sum\_{j=0}^{n-1} \theta\_{n+k,n+k-j} W\_{j+1} \nonumber
\end{align}\]

由投影的正交性, 得到预测的均方误差
\[\begin{align}
& E[Y\_{n+k+1} - L(Y\_{n+k+1}|\boldsymbol{Y}\_n)]^2 \nonumber \\
=& EY\_{n+k+1}^2-E[L(Y\_{n+k+1}|\boldsymbol{Y}\_n)]^2 \nonumber \\
=& EY\_{n+k+1}^2
- \sum \_{j=k+1}^{n+k} \theta\_{n+k,j}^2 \nu\_{n+k-j},
\tag{24.11}
\\
=& EY\_{n+k+1}^2 - \sum\_{j=0}^{n-1} \theta\_{n+k, n+k-j}^2 \nu\_j^2 \nonumber
\end{align}\]
其中的系数\(\theta\_{n+k,j}\)和预测的均方误差
\(\nu\_{n+k-j}\)可用递推公式[(24.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-coef0306)计算,
只不过因为\(Y\_{n+1}, \dots, Y\_{n+k}\)未知所以\(W\_{n+1}, \dots, W\_{n+k}\)不能计算。

## 24.2 正态时间序列的区间预测

如果\(\{Y\_t\}\)是正态时间序列, 则\(\hat Y\_{n+1}\)也是最佳预测.
\(W\_{n+1}=Y\_{n+1}-\hat Y\_{n+1}\)
作为\(Y\_1,Y\_2,\cdots,Y\_{n+1}\)的线性组合服从正态分布\(N(0,\nu\_n)\).
利用
\[
P \left( |Y\_{n+1}-\hat Y\_{n+1}|/\sqrt{\nu\_n} \leq 1.96 \right) = 0.95
\]
可以得到\(Y\_{n+1}\)的置信度为0.95的置信区间(预测区间)
\[
[\hat Y\_{n+1} -1.96\sqrt{\nu\_n}, \ \hat Y\_{n+1} + 1.96\sqrt{\nu\_n}]
\]

## 24.3 平稳序列的递推预测

设\(\gamma\_k=E(X\_{t+k}X\_t)\) 是零均值平稳序列\(\{X\_t\}\)的自协方差函数,
\(\Gamma\_n\)是\(\{X\_t\}\)的\(n\)阶自协方差矩阵.
设\(\boldsymbol{X}\_n =(X\_1,X\_2,...,X\_n)^T\),
\(Z\_n=X\_n - L(X\_n|\boldsymbol{X}\_{n-1})\), 可以把定理[24.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#thm:ipred-ipredthm)改述如下.

**推论24.1** 设\(\{X\_t\}\)是零均值平稳序列, 对任何\(n \in \mathbb N\), 自协方差矩阵\(\Gamma\_n\)正定.
则最佳线性预测
\[\begin{align}
\hat X\_{n+1} \stackrel{\triangle}{=}& L(X\_{n+1}|\boldsymbol{X}\_n) \nonumber \\
=& \sum\_{j=1}^n \theta\_{n,j} Z\_{n+1-j},
\tag{24.12} \\
=& \sum\_{j=0}^{n-1} \theta\_{n,n-j} Z\_{j+1}
\ n=1,2,\dots \nonumber
\end{align}\]
其中的系数\(\{\theta\_{n,j}\}\)
和预测的均方误差\(\nu\_n=EZ\_{n+1}^2\)满足如下的递推公式:
\[\begin{align}
\begin{cases}
& \nu\_0=\gamma\_0 \\
&\theta\_{n,n-k} =
[\gamma\_{n-k} - \sum\_{j=0}^{k-1}
\theta\_{k,k-j}\theta\_{n,n-j}\nu\_j]/\nu\_k, \\
& \qquad\qquad\qquad \ 0\leq k\leq n-1,\\
& \nu\_n = \gamma\_0 - \sum\_{j=0}^{n-1} \theta\_{n,n-j}^2\nu\_j,
\end{cases}
\tag{24.13}
\end{align}\]
其中\(\sum\_{j=0}^{-1}(\cdot)\stackrel{\triangle}{=} 0\),
递推的顺序是
\[
\begin{array}{lllll}
\nu\_0 \\
\theta\_{1,1} & \nu\_1 \\
\theta\_{2,2} & \theta\_{2,1} & \nu\_2 \\
\theta\_{3,3} & \theta\_{3,2} & \theta\_{3,1} & \nu\_3 \\
\vdots & \vdots & \vdots & \vdots & \ddots
\end{array}
\]

由于预测误差\(Z\_n=X\_n-L(X\_{n}|\boldsymbol{X}\_{n-1})\) 和\(\boldsymbol{X}\_{n-1}\)正交,
所以是不被\(\boldsymbol X\_{n-1}\)的线性组合包含的信息.
基于这个原因, 人们又称\(Z\_n\)是**样本新息**.
从§[23.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#wold-wold)的讨论知道,
\[
\nu\_n=E[X\_1-L(X\_1|X\_0,X\_{-1},\cdots,X\_{-n+1})]^2 \to \sigma^2, \ (n\to \infty)
\]
这里
\(\sigma^2\)是用全体历史\(X\_t,X\_{t-1},\dots\)预测\(X\_{t+1}\)时的均方误差.
\(\sigma^2>0\)表示\(\{X\_t\}\)是非决定性的.