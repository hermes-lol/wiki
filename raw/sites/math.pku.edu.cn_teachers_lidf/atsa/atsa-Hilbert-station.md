---
crawl_time: '2026-01-17 14:28:53'
framework: sphinx
title: 5 Hilbert空间中的平稳序列 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-Hilbert-station.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 5 Hilbert空间中的平稳序列

## 5.1 Hilbert空间

本章中设\(\{X\_t\}\)是平稳序列。
\(X\_t\)可以用线性组合\(\sum\_{j=1}^k a\_j x\_{t-j}\)预测。
应该推广到用无穷的线性组合\(\sum\_{j=1}^\infty a\_j x\_{t-j}\)预测。
这就需要研究这样的无穷线性组合的性质。

在线性代数的欧式空间理论中，
一个向量\(\boldsymbol y\)到一个子空间\(S\)的最短距离，
是\(\boldsymbol y\)与\(\boldsymbol y\)在\(S\)上的投影向量的距离。
将预测误差按均方误差度量，
也可以得到类似结论，
但是需要考虑无穷维空间的问题。

所有二阶矩有限的随机变量组成的一个无穷维函数空间\(L^2\)，
所有\(\{X\_t\}\)及其有限和无穷线性组合也构成一个无穷维函数空间，
是\(L^2\)的子空间。
下面从有限线性组合讲起。

### 5.1.1 平稳列导出的线性空间

设\(\{X\_t\}\)是平稳序列。令
\[
L^2(X) = \left\{
\left. \sum\_{j=1}^k a\_j X(t\_j) \right|
a\_j \in \mathbb R, t\_j \in \mathbb Z,
1 \leq j \leq k, k \in \mathbb N\_+
\right\}
\]
\(\forall X, Y, Z \in L^2(X), a, b \in \mathbb R\)有

1. \(X+Y =Y+X\in L^2(X)\), \((X+Y)+Z=X+(Y+Z)\);
2. \(0 \in L^2(X)\), \(X+0=X\), \(X+(-X)=0 \in L^2(X)\);
3. \(a(X+Y)=aX+aY\in L^2(X)\), \((a+b)X=aX+bX\), \(a(bX)=(ab)X\).

即\(L^2(X)\)是一个线性空间。

### 5.1.2 将线性空间推广为Hilbert空间

令\(L^2 = \{Y: E Y^2 < \infty \}\)，
易见\(L^2\)是线性空间，
\(L^2(X)\)是\(L^2\)空间的子空间。

在\(L^2\)中定义内积\(\langle X,Y\rangle =E(XY)\)，
则
\[\begin{aligned}
& \langle X,Y\rangle =\langle Y,X\rangle \\
& \langle aX+bY,Z\rangle =a\langle X,Z\rangle +b\langle Y,Z\rangle
\end{aligned}\]
\(\langle X,X\rangle \geq 0\),
并且 \(\langle X,X\rangle =0\)当且仅当 \(X=0\) , a.s.,
所以 \(L^2\) 又是内积空间.
\(L^2\)中的0元素，
定义为a.s.等于0的随机变量。

内积有Schwarz不等式(习题1.6.2)
\[
|\langle X,Y \rangle | \leq [\langle X,X \rangle
\langle Y,Y \rangle ]^{1/2} .
\]

我们还需要对无穷线性组合的封闭性。需要极限的概念。

由内积定义模(范数)
\[
\| X \| = (\langle X, X\rangle )^{1/2}
\]
Schwarz不等式可以写成
\[
|\langle X,Y \rangle | \leq \|X \| \cdot \| Y \| .
\]
由Schwarz不等式可以证明模满足三角不等式
\[
\| X + Y \| \leq \| X \| + \| Y \| .
\]

由模定义距离
\[
\| X-Y \| = (\langle X-Y,X-Y\rangle )^{1/2} .
\]
则\(\|X-Y\| = \|Y-X\| \geq 0\),
且\(\|X-Y\|=0\)当且仅当\(X=Y\), a.s..
距离满足三角不等式：
\[
\|X-Y\| \leq \|X-Z\| + \|Z-Y\| .
\]

对有限维线性空间，定义了内积已经足够，
因为有限维内积空间与\(\mathbb R^n\)同构。
对无穷维空间，要考虑极限问题。

**定义5.1** 对\(\xi\_n \in L^2\), \(\xi\_0 \in L^2\):

1. 如果\(\lim\_{n\to \infty}\| \xi\_n - \xi\_0\| = 0\),
   则称\(\xi\_n\)在\(L^2\)中(或均方)收敛到\(\xi\_0\), 记做
   \(\xi\_n \stackrel{\text{m.s.}}{\to} \xi\_0\)或
   \(\xi\_n \stackrel{L^2}{\to} \xi\_0\).
2. 如果当\(n,\ m \to \infty\)时,
   \(\| \xi\_n-\xi\_m\| \to 0\), 则称\(\{\xi\_n\}\)是\(L^2\)中的基本列
   或Cauchy列.

**定理5.1** 如果\(\{\xi\_n\}\)是\(L^2\)中的基本列,
则(在a.s.的意义下)有惟一的\(\xi \in L^2\)使得
\(\xi\_n \stackrel{\text{m.s.}}{\to} \xi\).

证明略。（同学们自学）

**定义5.2 (完备的内积空间)** 每个基本列都有极限在空间内的内积空间称为**完备**的内积空间，
又称**Hilbert空间**。

由定理[5.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-Hilbert-station.html#thm:Hilsta-caulim)，
\(L^2\)是Hilbert空间。

用\(\bar L^2(X)\)表示\(L^2\)中包含\(L^2(X)\)的最小闭子空间，
则\(\bar L^2(X)\) 是Hilbert空间，
称为由平稳序列\(\{X\_t\}\)生成的Hilbert空间。

### 5.1.3 内积的连续性

**定理5.2 (内积的连续性)** 在内积空间中,
如果 \(\| \xi\_n - \xi\| \to 0\),
\(\| \eta\_n - \eta \| \to 0\) 则有

1. \(\| \xi\_n\| \to \| \xi \|\),
2. \(\langle \xi\_n, \eta\_n \rangle \to \langle \xi,\eta\rangle\).

注意：由第2条当然有
\[\begin{aligned}
\langle \xi\_n, \eta \rangle \to \langle \xi, \eta \rangle
\end{aligned}\]

**证明**:

\[\begin{alignat\*}{2}
& (1)\ & & \Big|\, \| \xi\_n \| - \| \xi \| \, \Big| \\
& & \leq& \| \xi\_n - \xi \| \to 0 \ (n\to\infty)\\
& (2)\ & &
\Big| \langle \xi\_n, \eta\_n \rangle - \langle \xi, \eta \rangle \Big| \\
& &
=& \Big| \langle \xi\_n, \eta\_n - \eta \rangle +
\langle \xi\_n - \xi, \eta \rangle \Big| \\
& & \leq& \Big| \langle \xi\_n, \eta\_n - \eta \rangle \Big| +
\Big| \langle \xi\_n - \xi, \eta \rangle \Big| \\
& & \leq& \|\xi\_n\| \cdot \|\eta\_n - \eta\| +
\|\xi\_n - \xi\| \cdot \|\eta \| \\
& & \to& 0 . \ (n\to\infty)
\end{alignat\*}\]

**例5.1 (n维欧式空间)** \(\mathbb R^n\)是线性空间，定义内积
\(\langle \boldsymbol{a}, \boldsymbol{b} \rangle = \boldsymbol{a}^T \boldsymbol{b}\)
则为内积空间。

\(\mathbb R^n\)是完备的内积空间。
\(| \boldsymbol{a} | = \sqrt{\boldsymbol{a}^T \boldsymbol{a}}\)
为欧氏模。
此空间有限维：由\(n\)个元素（\(n\)维向量）组成基。

### 5.1.4 平稳列的有限维欧式空间

设\(\{X\_t\}\)是零均值平稳列，\(\boldsymbol{X} = (X\_1,\dots,X\_n)\)。
令\(L\_n = \text{sp}\{ X\_1, \dots, X\_n \} = \{\boldsymbol{a}^T \boldsymbol{X}: \boldsymbol{a} \in \mathbb R^n \}\),
则\(L\_n\)是Hilbert空间，
称为由\(\boldsymbol{X}\)生成的Hilbert空间。

\(L\_n\)是线性空间和内积空间易验证，下面证明其完备性。

先设 \(\{X\_t\}\)是标准白噪声WN\((0,1)\)。
对任何线性组合\(\xi\_k = \boldsymbol a\_k^T \boldsymbol X\), 只要
\[
\| \xi\_k-\xi\_m\| ^2
= \| \boldsymbol{a}^T\_k \boldsymbol X - \boldsymbol{a}^T\_m \boldsymbol{X} \| ^2
=(\boldsymbol{a}\_k - \boldsymbol{a}\_m)^T (\boldsymbol a\_k - \boldsymbol a\_m)\to 0,
\]
由例[5.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-Hilbert-station.html#exm:hilsta-Rn)知道有\(\boldsymbol{a} \in \mathbb R^n\) 使得
\[
| \boldsymbol a\_k -\boldsymbol a |\to 0
\]
当\(k \to \infty\),
取 \(\xi = \boldsymbol a ^T\boldsymbol X\) 时,
\[
\| \xi\_k - \xi \|^2 =(\boldsymbol a\_k - \boldsymbol a)^T(\boldsymbol a\_k - \boldsymbol a )\to 0.
\]
可见\(\{X\_t\}\)是标准白噪声WN\((0,1)\)时\(L\_n\) 是完备的.

对一般的零均值平稳序列,
可以设协方差阵\(\Gamma=E(\boldsymbol X \boldsymbol X^T)\)的秩是\(m\), \(m\leq n\).
对\(\Gamma\)做特征值分解得(注意这是随机向量的常用手法)
\[\begin{aligned}
\Gamma =& P^T \Lambda P \\
\Lambda =& \text{diag}\{ \lambda\_1, \dots, \lambda\_m, 0, \dots, 0 \}
\end{aligned}\]
令
\[\begin{aligned}
A =& \text{diag}\{ \lambda\_1^{-1/2}, \dots, \lambda\_m^{-1/2},
1, \dots, 1 \} \stackrel{\triangle}{=} \Lambda^{-1/2} \\
\boldsymbol Y =& A P \boldsymbol X, \quad \boldsymbol X = P^T A^{-1} \boldsymbol Y
\end{aligned}\]
则
\[\begin{aligned}
\text{Var}(\boldsymbol Y) =& A P \text{Var}(\boldsymbol X) P^T A = A P P^T \Lambda P P^T A \\
=& \text{diag}\{ 1,\dots,1, 0, \dots, 0 \}
\end{aligned}\]
因此\(Y\_1, \dots, Y\_m\)是某零均值白噪声列的某段。
\(\boldsymbol X\)的线性组合即\(Y\_1,\dots, Y\_m\)的线性组合。
因此\(L\_n\)是完备的。

○○○○○○

### 5.1.5 \(L^2\)意义下的线性序列

考虑\(L^2\)中的零均值白噪声列\(\{\varepsilon\_t\}\),
设\(\text{Var}(\varepsilon\_t)=\sigma^2\).
设\(\{a\_j\} \in l\_2\).
令
\[
\xi\_n(t) = \sum\_{j=-n}^n a\_j \varepsilon\_{t-j}, \ t \in \mathbb Z
\]
则\(\xi\_n(t) \in L^2\)。
对\(m<n\)，当\(m \to \infty\)时
\[\begin{aligned}
\| \xi\_n - \xi\_m \|^2
=& \| \sum\_{m<|j|\leq n} a\_j \varepsilon\_{t-j} \|^2 \\
=& \sigma^2 \sum\_{m<|j|\leq n} a\_j^2
\leq \sigma^2 \sum\_{|j| > m} a\_j^2 \to 0
\end{aligned}\]
由\(L^2\)完备性知存在\(X\_t \in L^2\)使得
\(\xi\_n(t) \stackrel{\text{m.s.}}{\to} X\_t\)。

记\(\xi\_n(t)\)在\(L^2\)中的极限\(X\_t\)为
\[
X\_t = \sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j}, \ t \in \mathbb Z
\]
来证明\(\{ X\_t \}\)平稳。

由\(L^2\)中内积连续性得
\[\begin{aligned}
E X\_t = \lim\_{n\to\infty} \langle \xi\_n(t), 1 \rangle
= \lim\_n E \xi\_n(t) = 0
\end{aligned}\]
以及
\[\begin{aligned}
E X\_t X\_{t+k} =& \lim\_n \langle \xi\_n(t), \xi\_n(t+k) \rangle \\
=& \lim\_n \langle \sum\_{j=-n}^n a\_j \varepsilon\_{t-j},
\sum\_{l=-n}^n a\_l \varepsilon\_{t+k-l} \rangle \\
=& \lim\_n \sum\_{j=-n}^n \sum\_{l=-n}^n a\_j a\_l \delta\_{k-l+j} \sigma^2 \\
=& \sigma^2 \sum\_{j=-\infty}^\infty a\_j a\_{j+k}
\end{aligned}\]
这就证明了\(\{X\_t \}\)为平稳列。

○○○○○○

## 5.2 复值时间序列

设\(X, Y\)为随机变量，
\(Z = X + i Y\)称为复值随机变量。
\(E Z = E X + i E Y\).
\[
\text{Cov}(Z\_1, Z\_2)
= E\left( (Z\_1 - E Z\_1) (Z\_2 - E Z\_2)^\* \right)
\]
(\(Z^\*\)表示\(Z\)的共轭转置)

对于两个复随机向量\(\boldsymbol Z\_1\), \(\boldsymbol Z\_2\)，
\[
\text{Cov}(\boldsymbol Z\_1, \boldsymbol Z\_2)
= E \left(
(\boldsymbol Z\_1 - E \boldsymbol Z\_1) (\boldsymbol Z\_2 - E \boldsymbol Z\_2)^\* \right)
\]

\(E |Z|^2 = E X^2 + E Y^2 < \infty\)时称\(Z\)是二阶矩有限的复随机变量。
所有二阶矩有限复随机变量的集合\(H\)在定义内积\(\langle X,Y \rangle = E(X Y^\*)\)后构成Hilbert空间。

复值随机变量的序列\(\{Z\_n\}\)称为**复值时间序列**.
若\(E Z\_n = \mu\),
\(\text{Cov}(Z\_n, Z\_m) = \gamma\_{n-m}\)，
\(\forall n,m \in \mathbb Z\),
则称\(\{Z\_n\}\)是**复值平稳序列**。

\[
\gamma\_{-k} = \gamma\_k^\* .
\]

若复值零均值平稳列\(\{\varepsilon\_t \}\)满足
\[\begin{aligned}
\text{Cov}(\varepsilon\_n, \varepsilon\_m)=\sigma^2 \delta\_{n-m},
\quad \forall n,m \in \mathbb Z
\end{aligned}\]
则称\(\{\varepsilon\_t\}\)为**复值零均值白噪声**。

**例5.2** 设\(Y \sim \text{U}[-\pi, \pi]\)。
定义
\[\begin{aligned}
\varepsilon\_n=e^{i n Y}, \ n\in \mathbb Z.
\end{aligned}\]
则
\[\begin{aligned}
E \varepsilon\_n =&\delta\_n \\
E (\varepsilon\_n \bar \varepsilon\_m)
=&\frac{1}{2\pi}\int\_{-\pi}^{\pi} e^{i(n-m)y} \, dy \\
=& \delta\_{n-m}
\end{aligned}\]
即\(\{\varepsilon\_t\}\)为复值白噪声(需要将\(\varepsilon\_0\)定义为\(0\))。

对于平方可和的实数列\(\{a\_j\}\), 定义
\[\begin{aligned}
Z\_n = \sum\_{j=-\infty}^{\infty} a\_j \varepsilon\_{n-j}
= \sum\_{j=-\infty}^{\infty} a\_je^{i(n-j)Y}, \ \ n\in \mathbb Z.
\end{aligned}\]
由内积的连续性得到
\[\begin{aligned}
E Z\_n =& \sum\_{j=-\infty}^{\infty} a\_j E \varepsilon\_{n-j} = a\_n \\
E (Z\_n \bar Z\_m)
=& \sum\_{j=-\infty}^{\infty} \sum\_{k=-\infty}^{\infty} a\_j a\_k
E(\varepsilon\_{n-j} \varepsilon\_{m-k}) \\
=& \sum\_{j=-\infty}^{\infty} \sum\_{k=-\infty}^{\infty} a\_j a\_k \delta\_{n-m-j+k} \\
=& \sum\_{k=-\infty}^{\infty} a\_k a\_{k+n-m}
\end{aligned}\]
因为均值非常数，
所以\(\{Z\_n \}\)不是复值平稳列。

另一方面，
\[\begin{aligned}
& E (Z\_n \bar Z\_m) \\
=& E \left[ \sum\_{j=-\infty}^{\infty} a\_j e^{i(n-j)Y}
\sum\_{k=-\infty}^{\infty} a\_k e^{-i(m-k)Y} \right] \\
=& \frac{1}{2\pi} \int\_{-\pi}^{\pi}
\left( \sum\_{j=-\infty}^{\infty} a\_j e^{i(n-j)y} \right)
\left( \sum\_{k=-\infty}^{\infty} a\_k e^{-i(m-k)y} \right) \; dy \\
=& \frac{1}{2\pi} \int\_{-\pi}^{\pi}
\left|\sum\_{j=-\infty}^{\infty} a\_j e^{-ijy} \right|^2 e^{i(n-m)y} \; dy
\end{aligned}\]
即
\[\begin{aligned}
\sum\_{j=-\infty}^{\infty} a\_j a\_{j+k}
= \frac{1}{2\pi}\int\_{-\pi}^{\pi}
\left|\sum\_{j=-\infty}^{\infty} a\_je^{-ijy} \right|^2 e^{iky}\; dy.
\end{aligned}\]

定义
\[\begin{aligned}
f(\lambda) =& \frac{\sigma^2}{2\pi}
\left| \sum\_{j=-\infty}^{\infty} a\_j e^{-ij\lambda} \right|^2
= \frac{\sigma^2}{2\pi} \left| \sum\_{j=-\infty}^{\infty} a\_j e^{ij\lambda} \right|^2
\end{aligned}\]
就得到公式
\[\begin{align}
\sigma^2 \sum\_{j=-\infty}^\infty a\_j a\_{j+k}
= \int\_{-\pi}^{\pi}f(y) e^{iky}\, dy
\tag{5.1}
\end{align}\]

○○○○○○

## 5.3 附录：补充知识

### 5.3.1 绝对可和系数的线性序列与L2的线性序列

系数绝对可和的线性平稳列是系数平方可和的线性平稳列的特例。
系数绝对可和的线性平稳列的极限是a.s.极限，
设\(\sum\_j |a\_j| < \infty\),
\[\begin{aligned}
X\_t = \sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j},
\ \text{a.s.}
\end{aligned}\]
而
\[\begin{aligned}
Y\_t = \sum\_{j=-\infty}^\infty a\_j \varepsilon\_{t-j},
\ (L^2)
\end{aligned}\]
记
\[\begin{aligned}
\xi\_n = \sum\_{j=-n}^n a\_j \varepsilon\_{t-j},
\end{aligned}\]
则
\[\begin{aligned}
\xi\_n \to X\_t, \ \text{a.s.},
\quad \xi\_n \to Y\_t, \ (L^2)
\end{aligned}\]
这时
\[\begin{aligned}
\xi\_n \stackrel{\text{Pr}}{\to} X\_t,
\quad \xi\_n \stackrel{\text{Pr}}{\to} Y\_t,
\end{aligned}\]
由测度论可知同一序列如果有两个依概率的极限
则这两个极限a.s.相等。

### 5.3.2 随机过程的均方连续性

**定义5.3 (均方连续)** \[\begin{aligned}
\lim\_{h \to 0} \| \xi\_{t\_0 + h} - \xi\_{t\_0} \| = 0
\end{aligned}\]
称\(\{\xi\_t \}\)在\(t=t\_0\)**均方连续**，
如果对所有\(t\_0 \in \mathbb R\)都成立则称称\(\{\xi\_t \}\)在\(\mathbb R\)上均方连续。

均方连续不一定轨道连续。
若\(\{ \xi\_t \}\)是平稳过程(连续时)，则
\(\{ \xi\_t \}\)在\(\mathbb R\)上均方连续
\(\Longleftrightarrow\) \(\{ \xi\_t \}\)在\(t=0\)均方连续
\(\Longleftrightarrow\) \(\gamma(\tau)\)在\(\mathbb R\)连续
\(\Longleftrightarrow\) \(\gamma(\tau)\)在\(\tau=0\)连续。