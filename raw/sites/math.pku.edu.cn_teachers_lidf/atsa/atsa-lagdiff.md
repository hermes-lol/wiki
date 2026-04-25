---
crawl_time: '2026-01-17 14:28:51'
framework: sphinx
title: 7 推移算子和常系数差分方程 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 7 推移算子和常系数差分方程

## 7.1 推移算子

对任何时间序列\(\{X\_t\}\)和无穷级数
\[
\psi(z)= \sum\_{j=-\infty}^\infty b\_j z^j
\]
只要级数
\[
\sum\_{j=-\infty}^\infty b\_j X\_{t-j}
\]
在某种意义下收敛(例如a.s.收敛, 依概率收敛, 均方收敛),
就定义
\[\begin{align}
\psi(\mathscr B)=& \sum\_{j=-\infty}^{\infty}b\_j\mathscr B^j, \\
\psi(\mathscr B) X\_t =& \sum\_{j=-\infty}^\infty
b\_j \mathscr B^j X\_{t}= \sum\_{j=-\infty}^\infty b\_j X\_{t-j}.
\tag{7.1}
\end{align}\]
并且称\(\mathscr B\)是时间\(t\)的向后推移算子或滞后算子,
简称为**推移算子**.

显然\(\mathscr B X\_t = X\_{t-1}\).
如果\(\{ X\_t \}\)是平稳列，
\(\mathscr B\)确实是Hilbert空间\(\bar L^2(X)\)上的一个算子，
也可以扩充到\(L^2\)空间上。
这里我们只给出它的简单性质。

* (1) 对和\(t\)无关的随机变量或者常数\(Y\), 有\(\mathscr B Y =Y\).
* (2) 对常数\(a\)，\(\mathscr B^{n}( a X\_t)=a \mathscr B^{n} X\_t =a X\_{t-n}\).
* (3) \(\mathscr B^{n+m} X\_t =\mathscr B^n(\mathscr B^m X\_t)=X\_{t-n-m}\).
* (4) 对多项式 \(\psi(z)=\sum\_{j=0}^{p}c\_jz^j\), 有
  \(\psi(\mathscr B) X\_t=\sum\_{j=0}^{p}c\_j X\_{t-j}\).
* (5) （交换律）对于多项式
  \(\psi(z)=\sum\_{j=0}^{p}c\_jz^j\) 和 \(\phi(z)= \sum\_{j=0}^{q}d\_jz^j\)
  的乘积\(A(z)=\psi(z) \phi(z)\), 有
  \[A(\mathscr B)X\_t=\psi(\mathscr B) [\phi(\mathscr B)X\_t] = \phi (\mathscr B) [\psi(\mathscr B)X\_t].\]
* (6) 对于时间序列 \(\{X\_t\}\), \(\{Y\_t\}\),
  多项式 \(\psi(z)=\sum\_{j=0}^{p}c\_jz^j\), 和随机变量\(U,V,W\), 有
  \[\psi(\mathscr B)(UX\_t+VY\_t+W)=U\psi(\mathscr B)X\_t+V\psi(\mathscr B)Y\_t + W\psi(1).\]

**性质证明**:

(1) 
对\(t \in \mathbb Z\),
定义\(X\_t = Y\),
\(\forall t\)，
对\(j \neq 1\)定义\(b\_j = 0\)。
由[(7.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0101)得
\[
\mathscr B Y = \mathscr B X\_t = \mathscr B X\_{t-1} = Y
\]

(2) 
令\(Y\_t = a X\_t\)，
由[(7.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0101)得到
\[
\mathscr B^n (a X\_t) = \mathscr B^n Y\_{t} = Y\_{t-n} = a X\_{t-n}
\]

(3) 
\[
\mathscr B^{n}[\mathscr B^m X\_t]
= \mathscr B^n X\_{t-m}
= X\_{t-m-n}
= \mathscr B^{n+m} X\_t
\]

(4) 
对\(j<0\)和\(j>p\)取\(b\_j=0\)，
由[(7.1)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0101)即可得
\[
\psi(\mathscr B) X\_t = \sum\_{j=0}^n b\_j X\_{t-j}
\]

(5) 
对于多项式
\(\psi(z)=\sum\_{j=0}^{p}c\_jz^j\)
和\(\phi(z)= \sum\_{j=0}^{q}d\_jz^j\)
的乘积\(A(z)=\psi(z) \phi(z)\),
记
\[
Z\_t = \phi(\mathscr B) X\_t = \sum\_{j=0}^q d\_j X\_{t-j}
\]
则
\[\begin{aligned}
\psi(\mathscr B) [\phi(\mathscr B) X\_t]
=& \psi(\mathscr B) Z\_t
= \sum\_{k=0}^p c\_k Z\_{t-k} \\
=& \sum\_{k=0}^p c\_k \sum\_{j=0}^k d\_j X\_{t-k-j} \\
=& \sum\_{k=0}^p \sum\_{j=0}^k c\_k d\_j X\_{t-k-j} \\
=& A(\mathscr B) X\_t
\end{aligned}\]
同理可证\(A(\mathscr B) X\_t = \psi(\mathscr B)[\phi(\mathscr B) X\_t]\)。

(6) 
略。

○○○○○○

如果\(\sum\_{j=-\infty}^\infty |\psi\_j| < \infty\)，
\(\{ X\_t \}\)为平稳列，
则
\[
\Psi(\mathscr B) X\_t
= \sum\_{j=-\infty}^\infty \psi\_j X\_{t-j}
\]
是平稳列\(\{ X\_t \}\)的线性滤波，
\(\{ \psi\_j \}\)为一个保时线性滤波器，
\(\Psi(\mathscr B) X\_t\)在a.s.和均方意义下收敛，
见[2.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#tsa-linser-filt-filt)和[5.1.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-Hilbert-station.html#Hilbert-space-L2sta)。
这时性质(5)(交换律)和性质(6)(线性性质)仍成立。

考虑无穷阶推移算子多项式的交换律。有如下定理：

**定理7.1 (线性滤波的交换率)** 设\(\{a\_k\}\), \(\{b\_j\}\)为两个绝对可和的实数列，则实数列
\[\begin{aligned}
d\_m = \sum\_{j=-\infty}^\infty a\_j b\_{m-j}
\end{aligned}\]
绝对可和（\(\{d\_m\}\)称为\(\{a\_k\}\)和\(\{b\_j\}\)的**离散卷积**），记
\[\begin{aligned}
A(z)=\sum\_k a\_k z^k,\quad B(z)=\sum\_j b\_j z^j,\quad
D(z)=\sum\_m d\_m z^m .
\end{aligned}\]

(1) 若\(\{y\_t\}\)为有界的数列：\(|y\_t|\leq M, t\in \mathbb Z\)，
则
\[\begin{aligned}
A(\mathscr B)[B(\mathscr B) y\_t] = B(\mathscr B)[A(\mathscr B) y\_t] = D(\mathscr B) y\_t
\end{aligned}\]

(2) 若\(\{X\_t\}\)为平稳列，则
\[\begin{aligned}
A(\mathscr B)[B(\mathscr B) X\_t] = B(\mathscr B)[A(\mathscr B) X\_t] = D(\mathscr B) X\_t, \ \text{a.s. 和} L^2 .
\end{aligned}\]

**证明** 
上述的\(\{a\_k\}\), \(\{b\_j\}\)绝对可和保证了\(\{d\_m\}\)也绝对可和：
\[\begin{aligned}
\sum\_{m=-\infty}^\infty |d\_m|
\leq& \sum\_{m=-\infty}^\infty \sum\_{j=-\infty}^\infty |a\_j| |b\_{m-j}| \\
=& \sum\_{j=-\infty}^\infty \sum\_{m=-\infty}^\infty |a\_j| |b\_{m-j}| \\
=& \sum\_{j=-\infty}^\infty |a\_j| \cdot \sum\_{m=-\infty}^\infty |b\_{m-j}| \\
=& \sum\_{j=-\infty}^\infty |a\_j| \cdot \sum\_{k=-\infty}^\infty |b\_k| < \infty
\end{aligned}\]

(1) 
\[\begin{aligned}
&\sum\_k \sum\_j |a\_k| |b\_j| |y\_{t-k-j}| \\
\leq& M(\sum\_k |a\_k|)(\sum\_j |b\_j|) < \infty
\end{aligned}\tag{\*1}
\]
所以
\[\begin{aligned}
& A(\mathscr B)B(\mathscr B) y\_t
= \sum\_k a\_k \sum\_j b\_j y\_{t-k-j} \\
=& \sum\_k \sum\_j a\_k b\_j y\_{t-k-j} \\
=& \sum\_k \sum\_m a\_k b\_{m-k} y\_{t-m} \quad
\text{(令$m=k+j,\ j=m-k$)} \\
=& \sum\_m \sum\_k a\_k b\_{m-k} y\_{t-m} \quad
\text{(无穷级数次序交换用到(\*1)式)} \\
=& \sum\_m d\_m y\_{t-m}
\end{aligned}\]
同样可证\(B(\mathscr B)A(\mathscr B) y\_t = D(\mathscr B)y\_t\)。

(2) 由推论[2.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/tsa-linser-filt.html#cor:linser-problim-suminfexp)，因为
\[\begin{aligned}
\sum\_k \sum\_j |a\_k| |b\_j| E |X\_{t-k-j}|
\leq \sqrt{\gamma\_0} (\sum\_k |a\_k|) (\sum\_j |b\_j|) < \infty,
\end{aligned}\]
由单调收敛定理有
\[\begin{aligned}
E \; \sum\_k \sum\_j |a\_k| |b\_j| |X\_{t-k-j}|
\leq \sqrt{\gamma\_0} (\sum\_k |a\_k|) (\sum\_j |b\_j|) < \infty,
\end{aligned}\]

所以\(A(\mathscr B)[B(\mathscr B) X\_t]\), \(B(\mathscr B)[A(\mathscr B) X\_t]\),
\(D(\mathscr B) X\_t\)都a.s.绝对收敛，
所以两重级数可以交换次序，
并且可以用换元法证明都等于\(D(\mathscr B) X\_t\)。

类似可证明\(A(\mathscr B)[B(\mathscr B) X\_t]\), \(B(\mathscr B)[A(\mathscr B) X\_t]\),
\(D(\mathscr B) X\_t\)都\(L^2\)收敛。
当同时a.s.收敛和\(L^2\)收敛时，
极限a.s.相等。

○○○○○○

**注：**
若所有\(a\_k=0(k<0)\), \(b\_j=0(j<0)\)，
则
\[\begin{aligned}
d\_m = \sum\_{j=0}^\infty a\_j b\_{m-j}
\end{aligned}\]
这时\(A(z)\), \(B(z)\)，\(D(z)\)在\(|z| \leq 1\)绝对一致收敛。

如果不满足这样的条件，
若\(a\_k, k<0\)和\(b\_j, j<0\)满足一些条件，
\(A(z)\)、\(B(z)\)、\(D(z)\)可以
在包含单位圆的圆环内解析。

比如, 如果存在\(0<\nu<1\)使得
\[\begin{aligned}
a\_k = o(\nu^{-k}), \quad k<0
\end{aligned}\]
则取\(\nu < \nu\_1\)，对\(\nu\_1 \leq |z| \leq 1\)有
\[\begin{aligned}
& \sum\_{k=-\infty}^0 |a\_k z^k | = \sum\_{k=-\infty}^0 |a\_k| \cdot |z|^k \\
\leq& \sum\_{k=-\infty}^0 |a\_k| \cdot \nu\_1^k
\leq \sum\_{k=-\infty}^0 c\_1 \nu\_{-k} \cdot \nu\_1^k \\
=& c\_1 \sum\_{k=-\infty}^0 c\_1 \left(\frac{\nu}{\nu\_1} \right)^{-k} < \infty
\end{aligned}\]
所以，\(A(z)\), \(B(z)\), \(D(z)\)在双边的时候也可以是有意义的。
定理[7.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#thm:linfil-commute)的证明不需要\(A(z)\)、\(B(z)\)、\(D(z)\)的级数收敛性。

考虑有限阶推移算子多项式的逆算子。有如下定理：

**定理7.2 (线性滤波的逆)** 设实系数多项式\(A(z)=\sum\_{j=0}^p \phi\_j z^j\)满足最小相位条件:
\[\begin{aligned}
A(z) \neq 0, \quad \forall |z| \leq 1
\end{aligned}\]
则存在\(\delta>0\)使
\[\begin{aligned}
A^{-1}(z) \stackrel{\triangle}{=} \frac{1}{A(z)} = \sum\_{j=0}^\infty \psi\_j z^j,
\quad |z| \leq 1 + \delta
\end{aligned}\]
其中\(\psi\_j = o((1+\delta)^{-j})\), \((j\to\infty)\)，
从而\(\sum\_{j=0}^\infty |\psi\_j| < \infty\)，
且有

(1) 若\(\{y\_t\}\)为有界数列，则
\[\begin{aligned}
y\_t = A^{-1}(\mathscr B) A(\mathscr B) y\_t = A(\mathscr B) A^{-1}(\mathscr B) y\_t
\end{aligned}\]

(2) 若\(\{X\_t\}\)为平稳列，则
\[\begin{aligned}
X\_t = A^{-1}(\mathscr B) A(\mathscr B) X\_t = A(\mathscr B) A^{-1}(\mathscr B) X\_t,
\quad \text{a.s.}
\end{aligned}\]

**证明**:
设多项式\(A(z)\)的根为\(z\_1, \dots, z\_p\)，取
\[\begin{aligned}
1 < 1 + \delta < \min\_{j} |z\_j|
\end{aligned}\]
则\(A(z)\)在\(|z|\leq 1+\delta\)无零点，
\(A^{-1}(z)\)在\(|z|\leq 1+\delta\)解析，
可以展开为Taylor级数
\[\begin{aligned}
A^{-1}(z) = \sum\_{j=0}^\infty \psi\_j z^j,
\quad |z| \leq 1+\delta
\end{aligned}\]

由于\(\sum\_{j=0}^\infty \psi\_j (1+\delta)^j\)收敛所以
\(\psi\_j (1+\delta)^j \to 0\)当\(j\to \infty\)，
即\(\psi\_j = o((1+\delta)^{-j})\ (j\to \infty)\)。
由此知\(\{\psi\_j\}\)绝对可和。定义\(\phi\_j=0\), 对
\(j<0\)或\(j>p\)，定义\(\psi\_j=0\)对\(j<0\)，注意到
\[\begin{aligned}
A(z) A^{-1}(z) = 1
\end{aligned}\]
令\(d\_0 = 1, d\_m = 0\)对\(m \neq 0\)，
由定理[7.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#thm:linfil-commute)可得所需结论。

○○○○○○

上面的定理也可以推广到双边无穷阶滤波的情形：

**定理7.3 (双边线性滤波的逆)** 设实系数多项式\(A(z)=\sum\_{j=-\infty}^\infty a\_j z^j\)满足:
\[\begin{align}
A(z) \neq 0, \quad \forall \alpha < |z| < \beta
\tag{7.2}
\end{align}\]
（其中\(0 < \alpha < 1 < \beta\)）
则存在\(\{ \psi\_j \}\)使得\(\psi\_j = o(\rho^{-|j|})\), \(\rho>1\), 且
\[\begin{aligned}
A^{-1}(z) \stackrel{\triangle}{=} \frac{1}{A(z)} = \sum\_{j=-\infty}^\infty \psi\_j z^j,
\quad \alpha < |z| < \beta
\end{aligned}\]
收敛。
有

(1) 
若\(\{y\_t\}\)为有界数列，则
\[\begin{aligned}
y\_t = A^{-1}(\mathscr B) A(\mathscr B) y\_t = A(\mathscr B) A^{-1}(\mathscr B) y\_t
\end{aligned}\]

(2) 
若\(\{X\_t\}\)为平稳列，则
\[\begin{aligned}
X\_t = A^{-1}(\mathscr B) A(\mathscr B) X\_t = A(\mathscr B) A^{-1}(\mathscr B) X\_t,
\quad \text{a.s.}
\end{aligned}\]

注意条件[(7.2)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-linfinv2sc1)保证了系数\(\{ a\_j \}\)绝对可和。
证明类似。

## 7.2 常系数齐次线性差分方程

给定 \(p\)个实数 \(a\_1,a\_2,\cdots,a\_p\), \(a\_p\neq 0\), 我们称
\[\begin{align}
X\_t-[a\_1 X\_{t-1}+ a\_2 X\_{t-2}+\dots+a\_p X\_{t-p}]=0,
\ \ t\in \mathbb Z,
\tag{7.3}
\end{align}\]
为\(p\)阶**齐次常系数线性差分方程**,
简称为齐次差分方程.
满足[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的实数列(或复数列)\(\{X\_t\}\)称为[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的解.
满足[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的实值(或复值)时间序列\(\{X\_t\}\)也称为[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的解.

[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的解\(\{X\_t\}\)
可以由它的\(p\)个初值\(X\_0,X\_1,\cdots,X\_{p-1}\)逐步递推得到:
\[\begin{aligned}
&X\_t=[a\_1 X\_{t-1}+ a\_2 X\_{t-2}+\dots+a\_p X\_{t-p}] , \ \ t \geq p, \\
&X\_{t-p} = \frac{1}{a\_p}[x\_t - a\_1 X\_{t-1} - \dots
- a\_{p-1}X\_{t-p+1} ], \ t-p<0
\end{aligned}\]

若初值是随机变量则递推得到的\(\{X\_t\}\)是时间序列。

用推移算子把差分方程写成
\[\begin{align}
A(\mathscr B)X\_t = 0, \ t \in \mathbb Z, \text{ 其中}\
A(z)=1-\sum\_{j=1}^p a\_j z^j.
\tag{7.4}
\end{align}\]diff-lagop0103)
\end{align}
\(A(z)\)称为差分方程(1.2)的特征多项式。

解有线性性质:
\(\{X\_t\}\), \(\{Y\_t\}\)是解则
\[ \xi X\_t + \eta Y\_t \]
也是解。
这样，
差分方程只要有非零解，
就有无穷多个解，
下面找出方程的所有的线性独立的解。

### 7.2.1 差分方程基础解

根据代数基本定理，
设实系数\(p\)次多项式\(A(z)\)有\(k\) 个互不相同的零点\(z\_1,z\_2,\cdots,z\_k\),
其中 \(z\_j\)是\(r(j)\)重零点，
\(\sum\_{j=1}^k r(j) = p\)。
可以证明对每一\(z\_j\)有
\[\begin{align}
A(\mathscr B) t^l z\_j^{-t}=0, \ l=0,1,\dots, r(j)-1
\tag{7.5}
\end{align}\]

**证明**:  
\[\begin{aligned}
A(z) =& \prod\_{j=1}^k (1 - z\_j^{-1} z)^{r(j)} \\
A(\mathscr B) =& \prod\_{j=1}^k (1 - z\_j^{-1} \mathscr B)^{r(j)}
\end{aligned}\]
只要证明
\[\begin{align}
(1 - z\_j^{-1} \mathscr B)^{l+1} (t^l z\_j^{-t}) = 0,
\ l=0,1,\dots,r(j)-1
\tag{7.6}
\end{align}\]
用归纳法。\(l=0\)时
\[\begin{aligned}
(1 - z\_j^{-1} \mathscr B) z\_j^{-t} = z\_j^{-t} - z\_j^{-1} z\_j^{-(t-1)} = 0
\end{aligned}\]
设对\(0 \leq l \leq m-1\)已经证明
\[\begin{aligned}
(1- z\_j^{-1} \mathscr B)^{l+1} t^l z\_j^{-t} = 0
\end{aligned}\]
则对\(l=m\)有
\[\begin{aligned}
& (1 - z\_j^{-1}\mathscr B)^{m+1} t^m z\_j^{-t} \\
=& (1 - z\_j^{-1}\mathscr B)^m (1 - z\_j^{-1}\mathscr B) \left( t^m z\_j^{-t} \right) \\
=& (1 - z\_j^{-1}\mathscr B)^m \left(
t^m z\_j^{-t} - z\_j^{-1} (t-1)^m z\_j^{-(t-1)} \right) \\
=& (1 - z\_j^{-1}\mathscr B)^m \left(t^m - (t-1)^m \right) z\_j^{-t} \\
=& (1 - z\_j^{-1}\mathscr B)^m \left(
c\_1 t^{m-1} + c\_2 t^{m-2} + \dots + c\_m \right) z\_j^{-t} \\
=& 0
\end{aligned}\]
于是[(7.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0105)成立，
从而[(7.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0104)成立。

○○○○○○

这样的基础解一共有\(p\)个，
组成了\(p\)个线性独立的解。
把基础解线性组合可以得到齐次线性差分方程的通解。
通解是方程的含有可取任意值的系数的解，
而且方程的每个解都包含在通解中。

### 7.2.2 齐次线性差分方程的通解

**定理7.4** 设\(A(z)\)有\(k\)个互不相同的零点\(z\_1,z\_2,\dots,z\_k\),
其中 \(z\_j\)是\(r(j)\)重零点. 则
\[\begin{align}
t^l z\_j^{-t}, \ \ l=0,1,\cdots,r(j)-1, \ \ j=1,2,\cdots,k
\tag{7.7}
\end{align}\]
是[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的 \(p\)个解;
而且, [(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的任何解\(\{X\_t\}\)都可以写成这\(p\)个解的线性组合:
\[\begin{align}
X\_t=\sum\_{j=1}^k\sum\_{l=0}^{r(j)-1}
U\_{l,j} t^l z\_j^{-t}, \ \ t\in \mathbb Z,
\tag{7.8}
\end{align}\]
其中的随机变量\(U\_{l,j}\)可以由\(\{X\_t\}\)的初值\(X\_0,X\_1,\cdots,X\_{p-1}\)惟一决定.
[(7.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0107)称为齐次线性差分方程[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的**通解**。

此定理关于时间序列叙述，实际上对差分方程的复数或实数列解也是成立的。

证明见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book))。

○○○○○○

[(7.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0107)中\(z\_j\)可以是复数。记
\[\begin{aligned}
U\_{l,j} =& V\_{l,j} e^{i\theta\_{l,j}}, \quad
z\_j = \rho\_j e^{i\lambda\_j} ,
\end{aligned}\]
则
\[\begin{aligned}
\text{Re}\left( U\_{l,j} t^l z\_j^{-t} \right)
=& \text{Re} \left(
V\_{l,j} t^l \rho\_j^{-t} e^{-i(\lambda\_j t - \theta\_{l,j})}
\right) \\
=& V\_{l,j} t^l \rho\_j^{-t} \cos(\lambda\_j t - \theta\_{l,j}) ,
\end{aligned}\]

差分方程[(7.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0102)的实值解可以表示为
\[\begin{aligned}
\sum\_{j=1}^k \sum\_{l=0}^{r(j)-1}
V\_{l,j} t^l \rho\_j^{-t} \cos(\lambda\_j t - \theta\_j), \quad
t \in \mathbb Z
\end{aligned}\]
\(\{V\_{l,j}, \theta\_{l,j}\}\)可以由初值\(X\_0, X\_1, \dots, X\_{p-1}\)
唯一决定。

### 7.2.3 通解的收敛性

如果差分方程的特征多项式\(A(z)\)的根都在单位圆外:
\(|z\_j| > 1, \quad j=1,2,\dots,k\),
或\(A(z) \neq 0, \quad \forall |z| \leq 1\),
取\(1 < \alpha < \min\{|z\_j|: j=1,\dots,k\}\)，则
\[\begin{aligned}
& t^l |z\_j|^{-t} = t^l (\alpha / |z\_j|)^t \alpha^{-t} = o(\alpha^{-t})
\end{aligned}\]
于是方程的任意解\(X\_t\)满足
\[\begin{align}
|X\_t| = o(\alpha^{-t}) \quad a.s., \ t \to \infty
\tag{7.9}
\end{align}\]
称\(X\_t\)以负指数阶收敛到零。

如果特征多项式有单位根\(z\_j = \exp(i\lambda\_j)\)，
则方程有一个周期解
\[\begin{aligned}
X\_t = a \cos(\lambda\_j t), \quad t \in \mathbb Z
\end{aligned}\]

如果单位圆内有根\(z\_j = \rho\_j \exp(i\lambda\_j), \rho\_j<1\)，
则方程有一个爆炸解（发散解）
\[\begin{aligned}
X\_t = a \left(\frac{1}{\rho\_j}\right)^t \cos(\lambda\_j t), \quad t \in \mathbb Z .
\end{aligned}\]

## 7.3 非齐次线性差分方程

设\(\{Y\_t\}\)为实值时间序列。
\[\begin{align}
A(\mathscr B) X\_t = Y\_t, \quad t \in \mathbb Z
\tag{7.10}
\end{align}\]
满足[(7.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0110)的时间序列\(\{X\_t\}\)称为[(7.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0110)的解。

如果有[(7.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-lagdiff.html#eq:lagdiff-lagop0110)的某个解(称为特解)\(\{X\_t^{(0)}\}\)，
则通解可以写成
\[\begin{align}
X\_t = X\_t^{(0)} + \sum\_{j=1}^k \sum\_{l=0}^{r(j)-1} U\_{l,j} t^l z\_j^{-t},
\quad t \in \mathbb Z .
\tag{7.11}
\end{align}\]

## 7.4 附录：复变函数复习

讨论推移算子多项式用到了一些复变函数知识，
这里进行复习。

若复变函数\(f(z)\)在复平面中点\(z\_0\)的某个邻域内的任一点都可导，
称\(f(z)\)在\(z\_0\)**解析**。
若\(f(z)\)在区域\(D\)内每一点解析，称\(f(z)\)在\(D\)内解析，
或称\(f(z)\)是\(D\)内一个**解析函数**。

**区域**\(D\)指\(D\)是连通开集。
\(D\)连通，即\(D\)中任意两个点之间都存在一条连续曲线。
复平面中的连续曲线，
就是从\([\alpha, \beta]\)到复数域的一个连续映射，
或平面上用参数方程表示的一条连续曲线。

若\(f(z), g(z)\)在区域\(D\)解析，则其和、差、积也在\(D\)内解析。
若\(g(z)\neq 0, z \in D\)，则\(f(z)/g(z)\)也在\(D\)内解析。
所以，多项式的倒数在不含分母零点的区域内解析。

把复变函数写成两个二元实函数：
\[\begin{aligned}
f(z) = f(x + iy) = u(x,y) + i v(x,y)
\end{aligned}\]
则\(f(z)\)解析当且仅当\(u(x,y)\)和\(v(x,y)\)都可微且满足柯西-黎曼条件
\[\begin{aligned}
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y},
\quad
\frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}
\end{aligned}\]
这时
\[\begin{aligned}
f'(z) =& \frac{\partial u}{\partial x} + i \frac{\partial v}{\partial x} \\
=& \frac{\partial v}{\partial y} - i \frac{\partial u}{\partial y}
\end{aligned}\]

解析函数必无穷阶可微。

**调和函数**:
二元实函数\(\varphi(x,y)\)在区域\(D\)内有二阶连续偏导且
\[\begin{aligned}
\frac{\partial^2 \varphi}{\partial x^2}
+ \frac{\partial^2 \varphi}{\partial y^2} = 0.
\end{aligned}\]
解析函数的实部和虚部都是调和函数。

**幂级数**:
复变级数
\[\begin{aligned}
\sum\_{k=0}^\infty a\_n z^n
\end{aligned}\]
称为幂级数。
如果
\[\begin{aligned}
R = \lim\_{n\to\infty} \frac{|a\_n|}{|a\_{n+1}|}
\end{aligned}\]
存在，则

* 当\(|z|<R\)时，幂级数绝对收敛；
* 当\(|z|>R\)时，幂级数发散；
* 当\(|z|=R\)时，幂级数可能收敛也可能发散。

\(R\)称为幂级数的收敛半径，
幂级数的收敛区域叫做收敛圆。

**泰勒级数**:
若函数\(f(z)\)在\(|z-b|<R\)内解析，
则在此范围可以展开成如下泰勒级数
\[\begin{aligned}
f(z) = \sum\_{k=0}^\infty a\_k (z-b)^k, \quad |z-b|<R
\end{aligned}\]
系数为
\[\begin{aligned}
a\_n = \frac{1}{2\pi i}\oint\_L \frac{f(\zeta)d\zeta}{(\zeta-b)^{n+1}}
= \frac{f^{(n)}(b)}{n!}
\end{aligned}\]
反之，这样的一个级数在\(z\_0\)收敛则在\(|z-b| < |z\_0-b|\)绝对收敛，解析。

**罗朗级数**:
含有负幂的级数
\[\begin{aligned}
\sum\_{n=-\infty}^{\infty} a\_n (z - b)^n
\end{aligned}\]
称为**罗朗级数**。
其中\(n \geq 0\)部分称作**解析部分**,
\(n<0\)部分称作**主要部分**。
解析部分的收敛范围是\(|z|<R\_1\),
主要部分的收敛范围是\(|z|>R\_2\)，
如果这两个区域相交，则罗朗级数在
\[\begin{aligned}
R\_2 < |z| < R\_1
\end{aligned}\]
内收敛。
若函数\(f(z)\)在环域\(R\_2 < |z-b| < R\_1\)内部为解析，
则\(f(z)\)在该环域内可展开为罗朗级数。

## 7.5 附录：用差分方程给出Fibonacci数列通解

Fibonacci数列定义为：
\[
F\_1 = F\_2 = 1,
\ F\_n = F\_{n-1} + F\_{n-2}, n=2,3,\dots
\]

这是一个齐次线性差分方程。
特征多项式
\[
1 - z - z^2 = 0,
\]
两个特征根为
\[
z\_1 = \frac{\sqrt{5}-1}{2},
\ z\_2 = - \frac{\sqrt{5} + 1}{2} .
\]
可求出通解，
注意到黄金分割比
\[
\frac{ \sqrt{5} - 1}{2}
= \left( \frac{1 + \sqrt{5}}{2} \right)^{-1}
\approx 0.618
\]
有
\[\begin{aligned}
F\_n =& c\_1 (\frac{\sqrt{5}-1}{2})^{-n} + c\_2 (- \frac{\sqrt{5} + 1}{2})^{-n} \\
=& c\_1 (\frac{\sqrt{5} + 1}{2})^n + c\_2 (-\frac{\sqrt{5}-1}{2})^n
\end{aligned}\]

将\(F\_1=F\_2=1\)代入，就得到通项公式
\[
F\_n = \frac{1}{\sqrt{5}} \left[
\left( \frac{1 + \sqrt{5}}{2} \right)^n
- \left( \frac{1 - \sqrt{5}}{2} \right)^n
\right]
\]

程序验证：

```
fib <- function(n){
  gr <- (sqrt(5) - 1)/2
  grinv <- 1/gr
  1/sqrt(5)*(grinv^n - (-gr)^n)
}
fib(1:10)
```

```
##  [1]  1  1  2  3  5  8 13 21 34 55
```

关于Fibonacci数列，有级数性质
\[
\sum\_{j=1}^{2n} F\_j =
\sum\_{k=1}^n (F\_{2k-1} + F\_{2k})
= F\_{2n+2} - F\_2 .
\]

## 7.6 附录：推移算子补充

设\(\{X\_t \}\)为时间序列，
\(\{ a\_j, j \in \mathbb Z \}\)是绝对可和实数列。
若\(\sup\_t E|X\_t| < \infty\)，
则
\[\begin{aligned}
\Psi(\mathscr B) X\_t = \sum\_{j=-\infty}^\infty a\_j X\_{t-j}
\end{aligned}\]
以概率1收敛。
如果进一步地\(\sup\_t E X\_t^2 < \infty\)则级数还均方收敛到同一极限。
(见([Brockwell and Davis 1987](#ref-BrockwellDavis1987:tstm-book)) §3.1)。
当\(\{ X\_t \}\)平稳时级数必以概率1收敛和均方收敛到同一极限，
结果也是平稳序列。

## 7.7 附录：常系数齐次线性微分方程

设\(a\_0, a\_1, \dots, a\_p\)为实常数，
\(a\_p \neq 0\)，
微分方程
\[
a\_0 y + a\_1 y' + \dots
+ a\_{p-1} y^{(p-1)} + a\_p y^{(p)}
= 0
\]
称为\(p\)阶常系数齐次线性微分方程。
这是与常系数齐次线性差分方程类似的方程。
定义其伴随方程(auxiliary equation)为
\[
a\_0 + a\_1 z + \dots + a\_{p-1} z^{p-1} + a\_p z^p = 0 .
\]

设伴随方程有\(k\)个不同的复根\(z\_j\), \(j=1,2,\dots,k\)，
设\(z\_j\)为\(r(j)\)重根，
\(\sum\_{j=1}^n r(j) = p\)。
则
\[
y = x^{s} e^{z\_j x},\ s=0,1,\dots, r(j)-1;\ j=1,2,\dots,k
\]
是微分方程的\(p\)个线性独立的解，
取其实部，
就构成微分方程的\(p\)个线性独立的实函数解，
通解为这\(p\)个基础解的线性组合。

## 7.8 附录：R中多项式求根

在R软件中，
用`polyroot()`函数求多项式的所有复根，
输入为多项式的升幂表示的系数作为R向量。
如：
\[
z^2 - 2 z + 1 = 0
\]

```
polyroot(c(1, -2, 1))
```

```
## [1] 1+0i 1-0i
```

又如：

\[
z^3 + z = 0
\]

```
polyroot(c(0, 1, 0, 1))
```

```
## [1] 0+0i 0+1i 0-1i
```

用`abs()`函数求复数模，
如：

```
abs(polyroot(c(0, 1, 0, 1)))
```

```
## [1] 0 1 1
```

### References

Brockwell, P. J., and R. A. Davis. 1987. *Time Series: Theory and Methods*. Springer-Verlag.