---
crawl_time: '2026-01-17 14:28:45'
framework: sphinx
title: 13 自回归滑动平均模型 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 13 自回归滑动平均模型

## 13.1 ARMA(\(p,q\))模型及其平稳解

**定义13.1 (ARMA模型)** 设\(\{\varepsilon\_t\}\) 是 \(\text{WN}(0,\sigma^2)\),
实系数多项式\(A(z)\) 和 \(B(z)\)没有公共根,
满足 \(b\_0=1,\ a\_p b\_q\neq 0\) 和
\[\begin{align}
A(z)=& 1-\sum^{p}\_{j=1} a\_j z^j\neq 0 , |z| \leq 1, \\
B(z)=& \sum^{q}\_{j=0} b\_j z^j\neq 0 ,\; |z|< 1,
\tag{13.1}
\end{align}\]
就称差分方程：
\[\begin{align}
X\_t = \sum^{p}\_{j=1} a\_j X\_{t-j} + \sum^{q}\_{j=0} b\_j \varepsilon\_{t-j},
\quad t \in \mathbb Z,
\tag{13.2}
\end{align}\]
是一个**自回归滑动平均模型**,
简称为ARMA(\(p,q\))模型.
称满足[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的平稳序列\(\{X\_t\}\)为**平稳解**或**ARMA(\(p,q\))序列**.

模型写成
\[\begin{align}
A(\mathscr B) X\_t = B(\mathscr B) \varepsilon\_t, \quad t \in \mathbb Z
\tag{13.3}
\end{align}\]
\(A^{-1}(z) B(z)\)在\(|z| < \rho\) 解析(\(1 < \rho < \min\{|z\_j|\}\),
\(\{z\_j\}\)为\(A(z)\)的所有根)，
可以Taylor展开
\[\begin{align}
\Psi(z) \stackrel{\triangle}{=}& A^{-1}(z) B(z) = \sum\_{j=0}^\infty \psi\_j z^j,
\quad |z| \leq \rho
\tag{13.4}
\end{align}\]

易见\(\psi\_j = o(\rho^{-j})\),
\[
A^{-1}(\mathscr B) B(\mathscr B)\varepsilon\_t = \Psi(\mathscr B)\varepsilon\_t
= \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j}
\]
是线性平稳列。两边用\(A(\mathscr B)\)作用，根据[7](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#atsa-lagdiff)中线性滤波的逆的定理[7.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#thm:linfil-inverse)知
\[\begin{aligned}
A(\mathscr B) \Psi(\mathscr B)\varepsilon\_t
= A(\mathscr B) A^{-1}(\mathscr B) B(\mathscr B) \varepsilon\_t
= B(\mathscr B) \varepsilon\_t
\end{aligned}\]
即\(\Psi(\mathscr B)\varepsilon\_t\)是ARMA(\(p,q\))模型[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的解。

反之，若\(\{Y\_t\}\)是[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的一个平稳解，
在[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)两边作用\(A^{-1}(\mathscr B)\)即得
\[\begin{aligned}
A^{-1}(\mathscr B) A(\mathscr B) Y\_t = Y\_t
= A^{-1}(\mathscr B) B(\mathscr B) \varepsilon\_t = \Psi(\mathscr B)\varepsilon\_t
\end{aligned}\]
即
\[\begin{align}
X\_t = A^{-1}(\mathscr B) B(\mathscr B) \varepsilon\_t = \Psi(\mathscr B)\varepsilon\_t
\tag{13.5}
\end{align}\]
是ARMA(\(p,q\))模型[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的唯一平稳解。

称[(13.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-sta-only0206)中的\(\{\psi\_j\}\)为\(\{X\_t\}\)的Wold系数。

**定理13.1** 由[(13.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-sta-only0206)定义的平稳序列\(\{X\_t\}\) 是 ARMA(\(p,q\))模型[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的惟一平稳解.

模型[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的任意解可以写成
\[\begin{align}
Y\_t = X\_t + \sum\_{j=1}^k \sum\_{l=0}^{r(j)-1}
V\_{l,j} t^l \rho\_j^{-t} \cos(\lambda\_j t - \theta\_{l,j}),
t \in \mathbb Z,
\tag{13.6}
\end{align}\]
其中\(\{X\_t\}\)为平稳解[(13.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-sta-only0206)，
\(z\_1,z\_2,\dots,z\_k\)为\(A(z)\)的全体互不相同的零点,
\(z\_j = \rho\_j e^{i\lambda\_j}\)
有重数\(r(j)\).
随机变量\(V\_{j,l}, \theta\_{l,j}\)
由\(Y\_0-X\_0,Y\_1-X\_1,\dots,Y\_{p-1}-X\_{p-1}\)惟一决定.

## 13.2 ARMA模型的模拟生成

[(13.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-sta-gensolu0207)中的\(\{ Y\_t \}\)与\(\{X\_t \}\)当\(t \to \infty\)时无限接近：
\[\begin{align}
|Y\_t - X\_t| \leq \sum\_{j=1}^k \sum\_{l=0}^{r(j)-1}
|V\_{l,j}| t^l \rho\_j^{-t} ,
t \to \infty
\tag{13.7}
\end{align}\]
可以据此模拟ARMA模型:
取初值\(Y\_{-(p-1)} = \dots = Y\_{-1} = Y\_0 = 0\)，递推得
\[\begin{aligned}
Y\_t = \sum\_{j=1}^p a\_j Y\_{t-j} + \sum\_{j=0}^q b\_j \varepsilon\_{t-j},
t=1,2,\dots, m+n
\end{aligned}\]
当\(m\)较大时取后一段\(Y\_t, t=m+1,m+2,\dots,m+n\)作为ARMA(\(p,q\))模型的模拟数据。
当\(A(z)\)有靠近单位圆的根时\(m\)要取得较大。

## 13.3 ARMA(\(p,q\))序列的自协方差函数

### 13.3.1 用Wold系数表示

\(\{\gamma\_k\}\)可由Wold系数表示:
\[\begin{align}
\gamma\_k = \sigma^2 \sum\_{j=0}^\infty \psi\_j \psi\_{j+k},
\quad k=0,1,2,\dots
\tag{13.8}
\end{align}\]
由于\(\psi\_j = o(\rho^{-j}), j\to\infty\)，
由[(13.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-gam-gampsi0210)可得\(\gamma\_k = o(\rho^{-j}), j\to\infty\)。

### 13.3.2 Wold系数递推公式

记\(b\_j=0, j<0\)或\(j>q\), \(b\_0=1\)；
\(\psi\_j=0, j<0\)。
由参数\(\boldsymbol{a}\_p = (a\_1, \dots, a\_p)^T\),
\(\boldsymbol{b}\_p = (b\_1, \dots, b\_q)^T\)计算\(\{\psi\_j\}\)时可以递推
\[\begin{align}
\psi\_j = \begin{cases}
1, \quad & j=0, \\
b\_j + \sum\_{k=1}^p a\_k \psi\_{j-k}, & j=1,2,\dots
\end{cases}
\tag{13.9}
\end{align}\]

**证明**:

记\(A(z) = 1 - \sum\_{j=1}^p a\_j z^j = \sum\_{j=0}^p \phi\_j z^j\)。
注意
\[\begin{aligned}
A(z)\Psi(z) =&
\sum\_{k=0}^p \phi\_k z^k \sum\_{j=0}^\infty \psi\_j z^j \\
=& \sum\_{j=0}^\infty \sum\_{k=0}^p \phi\_k \psi\_{j-k} z^j = B(z)
\end{aligned}\]
比较系数得
\[\begin{aligned}
\sum\_{k=0}^p \phi\_k \psi\_{j-k} =& b\_j, \quad j \geq 1 \\
\psi\_j =& \sum\_{k=1}^p a\_k \psi\_{j-k} + b\_j, \quad j \geq 1
\end{aligned}\]
即[(13.9)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-gam-psiiter0211)成立。

○○○○○○

## 13.4 ARMA(\(p,q\))模型的可识别性

我们将证明: 由ARMA(\(p,q\))模型的自协方差函数\(\{\gamma\_k\}\)可以决定ARMA(\(p,q\))
模型的参数
\[\begin{aligned}
(\boldsymbol{a}^T, \boldsymbol{b}^T, \sigma^2)
= (a\_1, \dots, a\_p, b\_1, \dots, b\_q, \sigma^2)
\end{aligned}\]

**引理13.1** 设\(\{X\_t\}\)是[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-def-eq0202)的平稳解.
如果又有白噪声\(\{\eta\_t\}\)和实系数多项式\(C(\mathscr B)\),\(D(\mathscr B)\)使得
\[C(\mathscr B)X\_t = D(\mathscr B)\eta\_t, \ \ t\in \mathbb Z,\]
成立, 则\(C(z)\)的阶数\(\geq p\), \(D(z)\)的阶数\(\geq q\).

这主要因为我们要求多项式\(A(z)\)和\(B(z)\)互素。
证明略。

### 13.4.1 ARMA序列的Y-W方程

ARMA模型的平稳解为
\[\begin{aligned}
X\_t = \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j}
\end{aligned}\]
所以
\[\begin{aligned}
E (\varepsilon\_{t+k} X\_t) = 0, \quad k>0
\end{aligned}\]

类似AR模型可推导ARMA模型的Y-W方程:
在模型方程两边同乘以\(X\_{t-k}\)求期望得
\[\begin{aligned}
E(X\_t X\_{t-k}) =&
\sum\_{j=1}^p a\_j E(X\_{t-j} X\_{t-k})
+ \sum\_{j=0}^q b\_j E(\varepsilon\_{t-j} X\_{t-k})\\
\text{即} & \\
\gamma\_k =& \sum\_{j=1}^p a\_j \gamma\_{k-j}
+ \sum\_{j=0}^q b\_j
E(\varepsilon\_{t-j} \sum\_{l=0}^\infty \psi\_l \varepsilon\_{t-k-l}) \\
=& \sum\_{j=1}^p a\_j \gamma\_{k-j}
+ \sum\_{j=0}^q b\_j \psi\_{j-k} \sigma^2,
\quad k \in \mathbb Z
\end{aligned}\]

当\(k>q\)时\(\psi\_{j-k}=0, j=0,1,\dots,q\)，上式为
\[\begin{aligned}
\gamma\_k =& \sum\_{j=1}^p a\_j \gamma\_{k-j},
\quad k \geq q+1
\end{aligned}\]

总之
\[\begin{align}
\gamma\_k - \sum\_{j=1}^p a\_j \gamma\_{k-j}
= \begin{cases}
\sigma^2 \sum\_{j=\max(0,k)}^q b\_j \psi\_{j-k}, \ & k<q \\
\sigma^2 b\_q, & k=q \\
0 & k>q
\end{cases}
\tag{13.10}
\end{align}\]

对\(k>q\)的Y-W方程可以写成矩阵形式：
\[\begin{align}
\left[
\begin{array}{c}
\gamma\_{q+1} \\ \gamma\_{q+2} \\ \vdots \\ \gamma\_{q+p}
\end{array}
\right]
=
\left[
\begin{array}{cccc}
\gamma\_{q} & \gamma\_{q-1} & \cdots & \gamma\_{q-p+1} \\
\gamma\_{q+1} & \gamma\_{q} & \cdots & \gamma\_{q-p+2} \\
\vdots & \vdots & & \vdots \\
\gamma\_{q+p-1} & \gamma\_{q+p-2} & \cdots & \gamma\_{q} \\
\end{array}
\right]
\left[
\begin{array}{c}
a\_1 \\ a\_2 \\ \vdots \\ a\_p
\end{array}
\right]
\tag{13.11}
\end{align}\]

对比AR(\(p\))的Y-W方程，相当于\(\Gamma\_p\)的\((i,j)\)元素写成\(\gamma\_{i-j}\)后
给所有\(\gamma\_{\cdot}\)的下标都加上\(q\)。

把系数矩阵记为\(\Gamma\_{p,q}\):
\[\begin{aligned}
\Gamma\_{p,q} =&(\gamma\_{|q+i-j|})\_{i,j=1,2,\dots,p} \\
=&
\left[
\begin{array}{cccc}
\gamma\_{q} & \gamma\_{q-1} & \cdots & \gamma\_{q-p+1} \\
\gamma\_{q+1} & \gamma\_{q} & \cdots & \gamma\_{q-p+2} \\
\vdots & \vdots & & \vdots \\
\gamma\_{q+p-1} & \gamma\_{q+p-2} & \cdots & \gamma\_{q} \\
\end{array}
\right]
\end{aligned}\]
只要\(\Gamma\_{p,q}\)可逆则可解出\(a\_1,\dots, a\_p\)。

解出\(a\_1,\dots,a\_p\)后令
\[\begin{aligned}
Y\_t = A(\mathscr B) X\_t = B(\mathscr B)\varepsilon\_t, \quad t \in \mathbb Z
\end{aligned}\]
则\(\{Y\_t\}\)是一个MA(\(q\))序列，其自协方差函数为\(q\)步截尾，且
\[\begin{aligned}
\gamma\_y(k) =& E(Y\_t Y\_{t-k}) \\
=& \sum\_{j=0}^p \sum\_{l=0}^p \phi\_j \phi\_l E(X\_{t-j} X\_{t-l}) \\
=& \sum\_{j=0}^p \sum\_{l=0}^p \phi\_j \phi\_l \gamma\_{k+l-j},
\quad 0\leq k \leq q
\end{aligned}\]
可以用§[12](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#atsa-mamod)的方法唯一解出\(b\_1,\dots, b\_q, \sigma^2\)。

于是，只要\(\Gamma\_{p,q}\)可逆,
则ARMA(\(p,q\))序列的自协方差函数和
ARMA(\(p,q\))模型的参数\((\boldsymbol{a}\_p^T, \boldsymbol{b}\_q^T, \sigma^2)\)
相互惟一决定。

### 13.4.2 ARMA模型中AR部分的参数求解

如果\(\Gamma\_{p,q}\)可逆则由[(13.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-iden-ywmat0215)可以解出\(a\_1, \dots, a\_p\)。

**定理13.2** 设\(\{\gamma\_k\}\)为ARMA(\(p,q\))序列\(\{X\_t\}\)
的自协方差函数列,则\(m\geq p\)时\(\Gamma\_{m,q}\)可逆。

**证明:**

用反证法然后由引理[13.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#lem:armamod-iden-cancel)导出矛盾。

设\(\Gamma\_{m,q}\)(\(m\times m\)矩阵)不满秩，
则存在\(\boldsymbol{\beta}=(\beta\_0,\beta\_1, \dots, \beta\_{m-1})^T \neq 0\)使得
\(\Gamma\_{m,q} \boldsymbol{\beta}=0\)，即
\[\begin{align}
\sum\_{l=0}^{m-1} \beta\_l \gamma\_{q+k-l} = 0,
\quad k=0,1,\dots, m-1
\tag{13.12}
\end{align}\]
注意当\(k\geq m\)时\(q+k-l > q\)，所以这时
\(\gamma\_{q+k-l} = \sum\_{j=1}^p a\_j \gamma\_{q+k-l-j}\)，
所以取\(k=m\)有
\[\begin{aligned}
\sum\_{l=0}^{m-1} \beta\_l \; \gamma\_{q+k-l}
=& \sum\_{l=0}^{m-1} \beta\_l \sum\_{j=1}^p a\_j \gamma\_{q+k-l-j} \\
=& \sum\_{j=1}^p a\_j \sum\_{l=0}^{m-1} \beta\_l \; \gamma\_{q+(k-j)-l} \\
=& 0 \quad (\text{由(13.12)及}0 \leq k-j = m-j \leq m-1)
\end{aligned}\]

递推得上式当\(k>m\)时也成立。因此
\[\begin{aligned}
\sum\_{l=0}^{m-1} \beta\_l \; \gamma\_{q+k-l} = 0, \quad k \geq 0.
\end{aligned}\]

令\(Y\_t = \sum\_{l=0}^{m-1} \beta\_l X\_{t-l}\)则\(\{Y\_t\}\)是零均值平稳列，
利用
\[\begin{aligned}
E(Y\_t X\_{t-q-k}) = \sum\_{l=0}^{m-1} \beta\_l \gamma\_{q+k-l} = 0,
\quad k \geq 0
\end{aligned}\]
可知\(\{Y\_t\}\)的自协方差\(q-1\)步截尾:
\[\begin{aligned}
E(Y\_t Y\_{t-q-k}) = 0, \quad k \geq 0
\end{aligned}\]
所以\(\{Y\_t\}\)是MA(\(q'\))(\(q' \leq q-1\))序列，
存在\(\{\eta\_t\} \sim \text{WN}(0,s^2)\)使得
\[\begin{aligned}
\sum\_{l=0}^{m-1} \beta\_l X\_{t-l} = \sum\_{j=0}^{q'} d\_j \eta\_{t-j}
\end{aligned}\]
与引理[13.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#lem:armamod-iden-cancel)矛盾。

○○○○○○

### 13.4.3 ARMA模型的一个充分条件

**定理13.3** 设零均值平稳序列\(\{X\_t\}\)有自协方差函数\(\{\gamma\_k\}\).
又设实数\(a\_1,a\_2,\cdots,a\_p\)
\((a\_p\neq 0)\) 使得\(A(z)=1-\sum\_{j=1}^p a\_j z^j\) 满足最小相位条件,
另外
\[\begin{align}
\gamma\_k - \sum^{p}\_{j=1} a\_j\gamma\_{k-j}=\begin{cases}
c \neq 0 , \ & k=q, \\
0, & k>q,
\end{cases}
\tag{13.13}
\end{align}\]
则\(\{X\_t\}\)是一个ARMA\((p',q')\)序列,
其中\(p'\leq p , \ q' \leq q\). \

**证明**:
设\(Y\_t=A(\mathscr B) X\_t = X\_t-\sum^{p}\_{j=1}a\_j X\_{t-j}\).
则\(\{Y\_t\}\)是零均值平稳序列, 满足
\[
E(Y\_t X\_{t-k})=\gamma\_k -\sum^{p}\_{j=1} a\_j \gamma\_{k-j}
=\begin{cases}
c \neq 0, & k=q ,\\
0, & k>q.
\end{cases}
\]
所以有
\[\begin{aligned}
\gamma\_y(k) =& E(Y\_t Y\_{t-k})
=E\big [ Y\_t (X\_{t-k} - \sum\_{j=1}^p a\_j X\_{t-k-j}) \big]\\
=& \begin{cases}
c \neq 0, \ & k=q ,\\
0, & k>q.
\end{cases}
\end{aligned}\]
说明\(\{Y\_t\}\)的自协方差函数是\(q\)后截尾的.

由定理[12.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#thm:mamod-truncthm)知道, \(\{Y\_t\}\)为一个MA\((q)\)序列,
即存在单位圆内没有根的\(q\)阶实系数多项式\(B(z)\)使得\(B(0)=b\_0=1\)
和
\[\begin{align}
A(\mathscr B) X\_{t}=Y\_t = B(\mathscr B)\varepsilon\_t,
\quad t\in \mathbb Z,
\tag{13.14}
\end{align}\]
其中\(\{\varepsilon\_t\}\) 是 \(\text{WN}(0,\sigma^2)\).

如果\(A(z)\)和\(B(z)\)没有公因子,
上述模型就是所需要的ARMA(\(p,q\))模型.
否则设公因子是\(C(z)\), 则有
\(A(z)=C(z)A'(z)\), \(B(z)=C(z)B'(z).\)
这时[(13.14)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-suffcond-proof-matrunc0220)变成
\[
C(\mathscr B)A'(\mathscr B)X\_t =C(\mathscr B)B'(\mathscr B)\varepsilon\_t.
\]
两边乘\(C^{-1}(\mathscr B)\)(显然\(C(z)\)也满足最小相位条件)
后得到所需ARMA(\(p',q'\))模型:
\[
A'(\mathscr B)X\_t =B'(\mathscr B)\varepsilon\_t.
\]

○○○○○○

## 13.5 ARMA序列的谱密度

由于ARMA序列的\(\{\gamma\_k\}\)绝对可和，以及平稳解的线性序列表达式，
可得ARMA(\(p,q\))序列(2.6)有谱密度
\[\begin{align}
f(\lambda) =& \frac{1}{2\pi}
\sum\_{k=-\infty}^\infty \gamma\_k e^{-ik\lambda} \\
=& \frac{\sigma^2}{2\pi} \left|
\sum\_{j=0}^\infty \psi\_j e^{ij\lambda} \right|^2
= \frac{\sigma^2}{2\pi} \left|
\frac{B(e^{i\lambda})}{A(e^{i\lambda})} \right|^2
\tag{13.15}
\end{align}\]
形如[(13.15)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-den0221)的谱密度被称为**有理谱密度**.

## 13.6 可逆ARMA序列

**定义13.2** 在ARMA\((p,q)\)模型的定义 [13.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-def) 中,
如果进一步要求\(B(z)\)在单位圆上无根:
\[\begin{align}
B(z)=1 + \sum^{q}\_{j=1} b\_j z^j \neq 0, |z|\leq 1
\tag{13.16}
\end{align}\]
则称ARMA(\(p,q\))模型[(13.16)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-invdef0222)为可逆的ARMA模型,
称相应的平稳解为可逆的ARMA(\(p,q\))序列.

从定理[12.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-mamod.html#thm:mamod-minser-speccond)(最小序列的谱密度条件）知道可逆的ARMA(\(p,q\))序列是最小序列.

对于可逆的ARMA(\(p,q\))模型[(13.16)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-invdef0222)，
由于\(B^{-1}(z)A(z)\)在\(\{z: |z|\leq\rho\}\) \((\rho>1)\) 内解析,
所以有Taylor展式：
\[\begin{align}
B^{-1}(z)A(z)=\sum^{\infty}\_{j=0} \varphi\_j z^j,\ \ |z|\leq\rho,
\tag{13.17}
\end{align}\]
其中\(|\varphi\_j|=o(\rho^{-j})\),当\(j\to\infty\),从而可以定义
\(B^{-1}(\mathscr B)A(\mathscr B)=\sum^{\infty}\_{j=0}\varphi\_j \mathscr B^j\).
在[(13.17)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-invTaylor0223)两边乘以\(B^{-1}(\mathscr B)\), 得到：
\[\begin{align}
\varepsilon\_t =B^{-1}(\mathscr B)A(\mathscr B)X\_t
=\sum^{\infty}\_{j=0} \varphi\_j X\_{t-j},
\ \ t\in \mathbb Z.
\tag{13.18}
\end{align}\]
[(13.18)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-invfilt0224)是[(13.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-sta-only0206)的逆转形式,
表明可逆ARMA(\(p,q\))序列和它的噪声序列可以相互线性表示.

○○○○○○

## 13.7 ARMA模型例子

先给出一些有关的自定义R函数。

```
## ARMA theoretical spectrum given ARMA coefficients
arma.true.spectrum <- function(a, b, ngrid=256, sigma=1,
                               tit="True ARMA Spectral Density",
                               plot.it=TRUE){
  p <- length(a)
  q <- length(b)
  freqs <- seq(from=0, to=pi, length=ngrid)
  spec1 <- numeric(ngrid)
  spec2 <- numeric(ngrid)
  for(ii in seq(ngrid)){
    spec1[ii] <- 1 + sum(complex(mod=b, arg=freqs[ii]*seq(q)))
    spec2[ii] <- 1 - sum(complex(mod=a, arg=freqs[ii]*seq(p)))
  }
  spec = sigma^2 / (2*pi) * abs(spec1)^2 / abs(spec2)^2
  if(plot.it){
    plot(freqs, spec, type='l',
         main=tit,
         xlab="frequency", ylab="spectrum",
         axes=FALSE)
    axis(2)
    axis(1, at=(0:6)/6*pi,
         labels=c(0, expression(pi/6),
           expression(pi/3), expression(pi/2),
           expression(2*pi/3), expression(5*pi/6), expression(pi)))
  }
  box()
  invisible(list(frequencies=freqs, spectrum=spec,
       ar.coefficients=a, ma.coefficients=b,
       sigma=sigma))
}

## simulate ARMA(p, q) model.
## a is vector a_1, \dots, a_p
## b is vector b_1, \dots, b_q
## Model is
##   X_t = a_1 X_{t-1} + \dots + a_p X_{t-p}
##         + \epsilon_t + b_1 \epsilon_{t-1} + \dots + b_q \epsilon_{t-q}
## \Var(\epsilon_t) = sigma^2
arma.gen <- function(n, a, b, sigma=1.0,
                     by.roots.ar=FALSE, by.roots.ma=FALSE,
                     plot.it=FALSE, n0=1000,
                     x0=numeric(length(a))){
  n2 <- n0 + n
  p <- length(a)
  ## first generate n0+n MA(q) series
  eps <- ma.gen(n2, b, sigma=sigma,
                by.roots=by.roots.ma,
                plot.it=FALSE)
  b <- attr(eps, "b")
  if(by.roots.ar){
    require(polynom)
    cf <- Re(c(poly.calc(a)))
    cf <- cf / cf[1]
    a <- -cf[-1]
  }

  ##set.seed(1)
  x2 <- filter(eps, a, method="recursive", side=1, init=x0)
  x <- x2[(n0+1):n2]
  x <- ts(x)
  attr(x, "model") <- "ARMA"
  attr(x, "a") <- a
  attr(x, "b") <- b
  attr(x, "sigma") <- sigma
  if(plot.it) plot(x)
  x
}

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

## Solve ARMA parameters given autocovariance functions
arma.solve <- function(gms, p, q){
  Gpq <- matrix(0, nrow=p, ncol=p)
  for(ii in seq(p)) for(jj in seq(p)){
    Gpq[ii,jj] <- gms[abs(q + ii - jj)+1]
  }
  gs <- gms[(q+1+1):(q+p+1)]
  a <- solve(Gpq, gs)
  aa <- c(-1, a)

  gys <- numeric(q+1)
  for(k in seq(0, q)){
    gys[k+1] <- sum(c(outer(0:p,0:p,
                            function(ii,jj) aa[ii+1] * aa[jj+1]
                            * gms[abs(k+jj-ii)+1])))
  }

  res <- ma.solve(gys)
  b <- res$b
  sigma <- sqrt(res$s2)

  list(a=a, b=b, sigma=sigma)
}

## simulate MA(q) model.
## a is vector a_1, \dots, a_q
## Model is X_t = eps_t + a_1 eps_{t-1} + \dots + a_q eps_{t-q}
## \Var(\epsilon_t) = sigma^2
## This version uses the filter function.
ma.gen <- function(n, a, sigma=1.0, by.roots=FALSE,
                   plot.it=FALSE){
  if(by.roots){
    require(polynom)
    cf <- Re(c(poly.calc(a)))
    cf <- cf / cf[1]
    a <- cf
  }
  q <- length(a)
  n2 <- n+q
  eps <- rnorm(n2, 0, sigma)
  x2 <- filter(eps, c(1,a), method="convolution", side=1)
  x <- x2[(q+1):n2]
  x <- ts(x)
  attr(x, "model") <- "MA"
  attr(x, "b") <- a
  attr(x, "sigma") <- sigma
  if(plot.it) plot(x)
  x
}

## Given \gamma_0, \gamma_1, \dots, \gamma_q,
## Solve MA(q) coefficients b_1, \dots, b_q, \sigma^2
## Using Li Lei's algorithm.
## Input: gms -- \gamma_0, \gamma_1, \dots, \gamma_q
ma.solve <- function(gms, k=100){
  q <- length(gms)-1
  if(q==1){
    rho1 <- gms[2] / gms[1]
    b <- (1 - sqrt(1 - 4*rho1^2))/(2*rho1)
    s2 <- gms[1] / (1 + b^2)
    return(list(b=b, s2=s2))
  }
  
  A <- matrix(0, nrow=q, ncol=q)
  for(j in seq(2,q)){
    A[j-1,j] <- 1
  }
  cc <- numeric(q); cc[1] <- 1
  gamma0 <- gms[1]
  gammas <- numeric(q+k)
  gammas[1:(q+1)] <- gms
  gamq <- gms[-1]
  Gammak <- matrix(0, nrow=k, ncol=k)
  for(ii in seq(k)){
    for(jj in seq(k)){
      Gammak[ii,jj] <- gammas[abs(ii-jj)+1]
    }
  }
  
  Omk <- matrix(0, nrow=q, ncol=k)
  for(ii in seq(q)){
    for(jj in seq(k)){
      Omk[ii,jj] <- gammas[ii+jj-1+1]
    }
  }
  PI <- Omk %*% solve(Gammak, t(Omk))

  s2 <- gamma0 - c(t(cc) %*% PI %*% cc)
  b <- 1/s2 * c(gamq - A %*% PI %*% cc)
  return(list(b=b, s2=s2))
}
```

### 13.7.1 ARMA(4,2)例子

ARMA(4,2):
\[\begin{align}
\begin{aligned}
a\_1 = -0.9, \quad & a\_2 = -1.4, \\
a\_3=-0.7, \quad & a\_4=-0.6; \\
b\_1=0.5, \quad & b\_2=-0.4.
\end{aligned}
\tag{13.19}
\end{align}\]

\(A(z)\)的根为\(1.1380e^{\pm 2.2062i}\),
\(1.1344e^{\pm 1.4896i}\),
\(B(z)\)的两个实根为\(2.3252, -1.0752\)。
此时间序列有两个频率成分，对应于AR部分的特征根的辐角。

模拟生成长度\(n=80\)的样本（结果见图[13.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#fig:armamod-examp-arma42)）：

```
set.seed(1)
n <- 80
a <- c(-0.9, -1.4, -0.7, -0.6)
b <- c(0.5, -0.4)

x <- arma.gen(n, a, b, plot.it=F)

ts.plot(x, main="ARMA(4,2) Series")
```

![ARMA(4,2)模拟数据](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3015-armamod_files/figure-html/armamod-examp-arma42-1.png)

图13.1: ARMA(4,2)模拟数据

对模拟数据估计ACF并作图（结果见图[13.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#fig:armamod-examp-arma42b)）：

```
acf(x)
```

![ARMA(4,2)数据的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3015-armamod_files/figure-html/armamod-examp-arma42b-1.png)

图13.2: ARMA(4,2)数据的ACF

模型的理论谱密度（结果见图[13.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#fig:armamod-examp-arma42c)）：

```
arma.true.spectrum(a, b)
```

![ARMA(4,2)数据的理论谱密度](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3015-armamod_files/figure-html/armamod-examp-arma42c-1.png)

图13.3: ARMA(4,2)数据的理论谱密度

模型的理论自协方差函数（结果见图[13.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#fig:armamod-examp-arma42d)）：

```
ng <- 21
gams <- arma.gamma(ng, a, b, sigma=1)
plot(0:(ng-1), gams, type="h",
     main="Theoretical gamma by Wold",
     xlab="k", ylab=expression(gamma[k]))
abline(h=0)
```

![ARMA(4,2)数据的理论自协方差函数](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/3015-armamod_files/figure-html/armamod-examp-arma42d-1.png)

图13.4: ARMA(4,2)数据的理论自协方差函数

```
cat("\n====== Autocovariances ========\n")
```

```
## 
## ====== Autocovariances ========
```

```
names(gams) <- 0:(ng-1)
print(round(gams, 4))
```

```
##       0       1       2       3       4       5       6       7       8       9 
##  6.6708 -1.5078 -4.5792  2.4672  1.2433 -0.4630 -0.3035 -1.4293  1.2894  1.3309 
##      10      11      12      13      14      15      16      17      18      19 
## -1.8203 -0.2699  1.0861 -0.1239 -0.1279 -0.3097 -0.1071  0.6939 -0.1810 -0.5477 
##      20 
##  0.3249
```

下面从理论自协方差函数反解模型参数。

```
cat("\n===== Solve ARMA from ACV ======\n")
```

```
## 
## ===== Solve ARMA from ACV ======
```

```
res <- arma.solve(gams, p=4, q=2)
print(res)
```

```
## $a
## [1] -0.9 -1.4 -0.7 -0.6
## 
## $b
## [1]  0.4999999 -0.4000000
## 
## $sigma
## [1] 1
```

用样本中估计的自协方差函数反解模型参数并与用理论值反解的结果对比：

```
gams2 <- acf(x, type="covariance", plot=FALSE)$acf
res2 <- arma.solve(gams2, p=4, q=2)
print(res2)
```

```
## $a
## [1] -1.0102537 -1.5340719 -0.8871690 -0.7297601
## 
## $b
## [1] 0.63530816 0.03771917
## 
## $sigma
## [1] 1.358952
```

○○○○○○

### 13.7.2 ARMA(2,2)例子

设\(\{X\_t\} \sim\) ARMA(2,2), 已知
\[\begin{aligned}
(\gamma\_0,\gamma\_1,\cdots,\gamma\_4)
=(4.61,\ -1.06,\ 0.29,\ 0.69,\ -0.12)
\end{aligned}\]
要反解ARMA参数。

```
gams3 <- c(4.61, -1.06, 0.29, 0.69, -0.12)
res3 <- arma.solve(gams3, p=2, q=2)
print(res3)
```

```
## $a
## [1]  0.08939301 -0.62648682
## 
## $b
## [1] -0.3334025  0.8157936
## 
## $sigma
## [1] 2.002966
```

```
z1 <- polyroot(c(1, -res3[["a"]]))
cat("AR roots: ", Mod(z1[1]), "exp(+- i ", Arg(z1[1]), ")", "\n")
```

```
## AR roots:  1.263409 exp(+- i  1.514296 )
```

```
z2 <- polyroot(c(1, res3[["b"]]))
cat("MA roots: ", Mod(z2[1]), "exp(+- i ", Arg(z2[1]), ")", "\n")
```

```
## MA roots:  1.107159 exp(+- i  1.385167 )
```

解出的模型为
\[
X\_t = 0.0894 X\_{t-1} - 0.6265 X\_{t-2}
+ \varepsilon\_t - 0.3334 \varepsilon\_{t-1} + 0.8158 \varepsilon\_{t-2},
\ \{ \varepsilon\_t \} \sim \text{WN}(0, 2.0030^2)
\]

AR部分的特征根为\(1.26e^{\pm i 1.51}\),
MA部分的特征根为\(1.11 e^{\pm i 1.39}\)。

○○○○○○

## 13.8 附录：ARAM(1,1)例子

对ARMA(1,1)
\[
X\_t = a X\_{t-1} + \varepsilon\_t + b \varepsilon\_{t-1}
\]
由Wold系数递推公式得
\(\psi\_0=1\),
\[\begin{aligned}
\psi\_1 =& b + a \psi\_{1-1} = a + b \\
\psi\_j =& a \psi\_{j-1} = \cdots = a^{j-1} \psi\_1 = a^{j-1}(a + b),
\ j=2,3,\dots
\end{aligned}\]
于是ARMA(1,1)的平稳解可以写成
\[
X\_t = \varepsilon\_t
+ (a + b)\sum\_{j=1}^\infty a^{j-1} \varepsilon\_{t-j}
\]

从Wold系数计算协方差函数得
\[\begin{aligned}
\gamma\_0 =& \sigma^2 \sum\_{j=0}^\infty \psi\_j^2
= \sigma^2\left\{ 1 + \sum\_{j=1}^\infty (a + b)^2 a^{2(j-1)} \right\} \\
=& \sigma^2\left\{ 1 + \frac{(a + b)^2}{1 - a^2} \right\}
= \sigma^2 \frac{1 + 2 a b + b^2}{1 - a^2} \\
\gamma\_1 =& \sigma^2 \sum\_{j=0}^\infty \psi\_j \psi\_{j+1}
= \sigma^2 \left\{ \psi\_1 + \sum\_{j=1}^\infty a (a + b)^2 a^{2(j-1)} \right\} \\
=& \sigma^2 \frac{(a+b)(1+ab)}{1 - a^2}
= \sigma^2 \frac{a + b + a^2 b + a b^2}{1 - a^2} \\
\gamma\_k =& \sigma^2 \sum\_{j=0}^\infty \psi\_j \psi\_{j+k}
= \sigma^2 a \sum\_{j=0}^\infty \psi\_j \psi\_{j+k-1} = a \gamma\_{k-1} = \cdots \\
=& a^{k-1} \gamma\_1
= a^{k-1} \sigma^2 \frac{(a+b)(1+ab)}{1 - a^2}
\end{aligned}\]

也可以利用YW方程计算协方差函数。
对\(k=0,1,2,\dots\)，
模型方程两边同乘以\(X\_{t-k}\)取期望，
因为\(E \varepsilon\_t X\_{t-k}=0\)对\(k\geq 1\)，
有
\[\begin{aligned}
\gamma\_0 =& a \gamma\_1 + E(X\_t \varepsilon\_t) + b E(X\_t \varepsilon\_{t-1}) \\
\gamma\_1 =& a \gamma\_0 + 0 + b E(X\_{t-1} \varepsilon\_{t-1}) \\
\gamma\_k =& a \gamma\_{k-1} + 0 + b \cdot 0, k=2,3,\dots
\end{aligned}\]
由Wold表示可知\(E(X\_{t} \varepsilon\_{t-j}) = \sigma^2 \psi\_j\)，
所以
\[\begin{aligned}
\gamma\_0 =& a \gamma\_1 + \sigma^2[1 + b(a+b)] \\
\gamma\_1 =& a \gamma\_0 + \sigma^2 b
\end{aligned}\]
将第二式代入第一式就可求解\(\gamma\_0, \gamma\_1\)为
\[\begin{aligned}
\gamma\_0 =& \sigma^2 \frac{1 + 2ab + b^2}{1 - a^2}, \\
\gamma\_1 =& \sigma^2 \frac{a+b + a^2 b + a b^2}{1 - a^2} \\
\gamma\_k =& a \gamma\_{k-1} = a^{k-1} \gamma\_1, k=2,3,\dots
\end{aligned}\]

自相关函数为
\[\begin{aligned}
\rho\_1 =& \frac{(a+b)(1 + ab)}{1 + 2ab + b^2} \\
\rho\_k =& a \rho\_{k-1} = a^{k-1} \rho\_1, k=2,3,\dots
\end{aligned}\]

下面求出ARMA(1,1)的偏相关函数前几项。
\[
a\_{11}=\rho\_1
= \frac{(a+b)(1 + ab)}{1 + 2ab + b^2}
\]
\(a\_{22}\)满足方程
\[
\left(\begin{matrix}
\gamma\_0 & \gamma\_1 \\
\gamma\_1 & \gamma\_0
\end{matrix}\right)
\left(\begin{matrix}
a\_{21} \\ a\_{22}
\end{matrix}\right)
=
\left(\begin{matrix}
\gamma\_1 \\ \gamma\_2
\end{matrix}\right)
\]
两边除以\(\gamma\_0\)得
\[
\left(\begin{matrix}
\rho\_0 & \rho\_1 \\
\rho\_1 & \rho\_0
\end{matrix}\right)
\left(\begin{matrix}
a\_{21} \\ a\_{22}
\end{matrix}\right)
=
\left(\begin{matrix}
\rho\_1 \\ \rho\_2
\end{matrix}\right)
\]
其中\(\rho\_0=1\), \(\rho\_2 = a \rho\_1\)，即
\[
\left(\begin{matrix}
1 & \rho\_1 \\
\rho\_1 & 1
\end{matrix}\right)
\left(\begin{matrix}
a\_{21} \\ a\_{22}
\end{matrix}\right)
=
\left(\begin{matrix}
\rho\_1 \\ \rho\_2
\end{matrix}\right)
\]
由Cramer法则，
\[
a\_{22} = \frac{\left| \begin{matrix}
1 & \rho\_1 \\
\rho\_1 & a \rho\_1
\end{matrix}\right|}{\left| \begin{matrix}
1 & \rho\_1 \\
\rho\_1 & 1
\end{matrix}\right|}
= \frac{\rho\_1(a - \rho\_1)}{1 - \rho\_1^2}
\]
类似地，
\[\begin{aligned}
a\_{33} =&
\frac{\left| \begin{matrix}
1 & \rho\_1 & \rho\_1 \\
\rho\_1 & 1 & \rho\_2 \\
\rho\_2 & \rho\_1 & \rho\_3
\end{matrix}\right|}{\left| \begin{matrix}
1 & \rho\_1 & \rho\_2 \\
\rho\_1 & 1 & \rho\_1 \\
\rho\_2 & \rho\_1 & 1
\end{matrix}\right|}
= \frac{\rho\_1 (a - \rho\_1)^2}{1 + 2 a \rho\_1^3 - \rho\_1^2(2 + a^2)}
\end{aligned}\]

○○○○○○

## 13.9 附录：ARMA模型的谱条件

ARMA模型的谱表示:
\[\begin{aligned}
X\_t = \int\_{-\pi}^\pi e^{it\lambda} \frac{B(e^{-i\lambda})}{A(e^{-i\lambda})}
d Z\_\varepsilon(\lambda)
\end{aligned}\]
其中\(\frac{B(z)}{A(z)}\)叫做ARMA模型的极大解析函数。

AMRA模型的谱密度:

设\(A(\cdot)\), \(B(\cdot)\)根都在单位圆外，
\(\{\varepsilon\_t\}\)是WN(\(0,\sigma^2\)), 则
平稳列\(\{X\_t\}\)是可逆ARMA模型
\[\begin{aligned}
A(\mathscr B) X\_t = B(\mathscr B) \varepsilon\_t
\end{aligned}\]
的充分必要条件是它有谱密度
\[\begin{aligned}
f(\lambda) = \frac{\sigma^2}{2\pi}
\left| \frac{B(e^{-i\lambda})}{A(e^{-i\lambda})} \right|^2
\end{aligned}\]
见([谢衷洁 1990](#ref-Xie1990:tsabook)) P.76定理2.4。

**证明**

必要性教材中已证明。设充分性条件成立，
这时设
\[\begin{aligned}
\frac{A(z)}{B(z)} = \sum\_{j=0}^\infty d\_j z^j,
\ |z| \leq \rho\_2 \ (\rho\_2>1)
\end{aligned}\]
令
\[\begin{aligned}
\eta\_t = \sum\_{j=0}^\infty d\_j X\_{t-j}
\end{aligned}\]
则
\[\begin{aligned}
A(\mathscr B) X\_t = B(\mathscr B)\eta\_t
\end{aligned}\]
且\(\{\eta\_t \}\)的谱密度为
\[\begin{aligned}
f\_\eta(\lambda) =& \left|\sum\_{j=0}^\infty d\_j e^{-ij\lambda} \right|^2 \cdot
\frac{\sigma^2}{2\pi} \left| \frac{B(e^{-i\lambda})}{A(e^{-i\lambda})} \right|^2 \\
=& \left| \frac{A(e^{-i\lambda})}{B(e^{-i\lambda})} \right|^2 \cdot
\frac{\sigma^2}{2\pi} \left| \frac{B(e^{-i\lambda})}{A(e^{-i\lambda})} \right|^2 \\
=& \frac{\sigma^2}{2\pi}
\end{aligned}\]
即\(\{\eta\_t \}\)为白噪声，
所以\(\{X\_t \}\)为可逆ARMA序列。

○○○○○○

## 13.10 附录：ARMA模型与Hilbert空间

参考([谢衷洁 1990](#ref-Xie1990:tsabook)) P.78。

考虑可逆ARMA模型
\[\begin{aligned}
A(\mathscr B) X\_t = B(\mathscr B) \varepsilon\_t
\end{aligned}\]
设
\[\begin{aligned}
H\_X =& {\mathcal L}\{X\_t: t \in \mathbb Z \} \\
H\_\varepsilon =& {\mathcal L}\{\varepsilon\_t: t \in \mathbb Z \} \\
H\_X(t) =& {\mathcal L}\{X\_s: s \leq t, \ s \in \mathbb Z \} \\
H\_\varepsilon(t) =& {\mathcal L}\{\varepsilon\_s: s \leq t, \ s \in \mathbb Z \}
\end{aligned}\]
其中\(\mathcal L\)表示线性闭包为Hilbert空间。
则\(\varepsilon\_t \in H\_X(t)\),
\(\varepsilon\_t \in H\_X(t) \ominus H\_X(t-1)\),
\(\{\varepsilon\_t/\sigma, t \in \mathbb Z \}\)是\(H\_X\)的一组完备标准正交基。
把\(X\_t\)用标准正交基展开成Wold表示
\[\begin{aligned}
X\_t = \sigma \sum\_{j=0}^\infty \psi\_j \frac{\varepsilon\_{t-j}}{\sigma}
\end{aligned}\]
展开的系数\(\sigma\psi\_j\)是\(H\_X\)中\(X\_t\)对正交基\(\{\varepsilon\_t/\sigma, t \in \mathbb Z \}\)
的广义傅立叶系数
\[\begin{aligned}
\sigma\psi\_j = < X\_t, \varepsilon\_{t-j}/\sigma >, \ j=0,1,2,\dots
\end{aligned}\]

可逆ARMA模型中的白噪声\(\varepsilon\_t\)是新息(Wold序列)：
\[\begin{aligned}
\varepsilon\_t = X\_t - L(X\_t | H\_X(t-1))
\end{aligned}\]
(参考([谢衷洁 1990](#ref-Xie1990:tsabook)) P.81定理2.6).

设可逆ARMA模型Wold表示为
\[\begin{aligned}
X\_t = \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j}
\end{aligned}\]
逆表示为
\[\begin{aligned}
\varepsilon\_{t} = \sum\_{j=0}^\infty d\_j X\_{t-j}
\end{aligned}\]
则\(\{d\_j \}\)可解为
\[\begin{aligned}
d\_0 =& 1 \\
d\_j =& - \sum\_{k=1}^j \psi\_k d\_{j-k}, \ j=1,2,\dots
\end{aligned}\]
这是因为
\[\begin{aligned}
\left(\sum\_{k=0}^\infty d\_k z^k \right)
\left(\sum\_{l=0}^\infty \psi\_l z^l \right) = 1
\end{aligned}\]
即
\[\begin{aligned}
\sum\_{j=0}^\infty \left(\sum\_{k=0}^j \psi\_k d\_{j-k} \right) z^j = 1
\end{aligned}\]

### References

谢衷洁. 1990. *时间序列分析*. 北京大学出版社.