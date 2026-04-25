---
crawl_time: '2026-01-17 14:29:16'
framework: sphinx
title: 23 非决定性平稳序列及其Wold表示 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 23 非决定性平稳序列及其Wold表示

## 23.1 非决定性平稳序列

对平稳序列,
考虑用所有的历史\(\{X\_t, \ t\leq n\}\)对\(X\_{n+1}\)进行最佳线性预测.
当预测误差是零时, \(X\_{n+1}\)的信息完全含在历史资料中.
这样的平稳序列被称为**决定性的**.
§[9.5](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#arspecyw-completelypred)中\(\Gamma\_{n+1}\)不满秩造成\(X\_t\)
可以被\(X\_{t-1}, \dots, X\_{t-n}\)完全线性预测，
是决定性平稳列的特例。

**最小序列**: 用\(\{X\_s: s \neq t \}\)预报\(X\_t\)误差不为零。
决定性序列不是最小序列。

实际问题中, 决定性平稳序列描述事物的发展没有新的信息出现.

如果用\(\{X\_t, \ t\leq n\}\)对\(X\_{n+1}\)做线性预测的误差不是零,
说明\(X\_{n+1}\)的信息不能由历史资料的线性组合及其极限完全确定,
我们称这种时间序列是**非决定性的**.

非决定性平稳序列描述事物的发展总伴随新的信息出现.

最小序列一定是非决定性的。

平稳序列的Wold定理表示告诉我们,
非决定性平稳序列总是可以分解成白噪声的单边滑动和加上一个决定性平稳序列.

从应用的角度讲, 非决定性平稳序列总是白噪声的单边滑动和加上一个离散谱序列.

### 23.1.1 最佳线性预测均方误差的极限

设\(\{X\_n:n\in \mathbb Z \}\)是零均值平稳序列. 记
\[\boldsymbol{X}\_{n, m}=(X\_{n},X\_{n-1},\cdots,X\_{n-m+1})^{T},\]
这里 \(n\)表示向量的第一个脚标, \(m\)表示向量的维数.

定义
\[
\hat{X}\_{n+1,m}=L(X\_{n+1} |\boldsymbol{X}\_{n,m}).
\]
从最佳线性预测的性质8知道
\(\sigma^2\_{1,m}=E(X\_{n+1}-\hat{X}\_{n+1,m})^2\)是\(m\)的单调减函数,
于是定义
\[
\sigma^2\_1 \stackrel{\triangle}{=}
\lim\_{m\rightarrow \infty}\sigma^2\_{1,m}< \infty.
\]

**定理23.1** \(\sigma^2\_1 \stackrel{\triangle}{=} \lim\_{m\rightarrow \infty}\sigma^2\_{1,m}\)与\(n\)无关.

**证明**:
设\(\boldsymbol{a}=(a\_1,a\_2,\dots,a\_m)^{T}\) 是预测方程[(22.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#eq:blpprop-blpgammeq0105)的解,
则\(\boldsymbol a\)和\(n\)无关.
由于
\[\begin{align}
Y\_n \stackrel{\triangle}{=} X\_{n+1}-\sum\_{j=1}^ma\_jX\_{n+1-j}
=X\_{n+1}- \hat{X}\_{n+1,m} , \quad n\in \mathbb Z,
\tag{23.1}
\end{align}\]
是平稳序列,
所以\(\sigma\_{1,m}^2=E Y\_n^2=E Y\_0^2\)与\(n\)无关.
最后\(\sigma^2\_1=\lim\_{m\to \infty} \sigma^2\_{1,m}\)与\(n\)无关.

○○○○○○

### 23.1.2 决定性与非决定性的严格定义

对充分大的\(m\), \(L(X\_{n+1}|\boldsymbol X\_{n,m})\)表示用充分多的历史对未来\(X\_{n+1}\)进行预测.
\(\sigma\_{1,m}^2\)表示的是预测的均方误差.
当\(m \to \infty\)时,
\(\sigma\_{1,m}^2 \to 0\) 说明\(X\_{n+1}\)
可以由所有历史\(X\_{n}, X\_{n-1},\dots\)进行完全预测.
当\(\sigma\_1^2 > 0\) 说明\(X\_{n+1}\)不可以由所有历史\(X\_{n}, X\_{n-1},\dots\)的线性组合以及极限进行完全预测.

**定义23.1** 设\(\{X\_t\}\)是零均值平稳序列.

* 如果\(\sigma^2\_1=0\), 称\(\{X\_t\}\)是**决定性平稳序列**;
* 如果\(\sigma^2\_1>0\),
  称\(\{X\_t\}\)是**非决定性平稳序列**,
  并且称\(\sigma^2\_1=\lim\_{m\rightarrow\infty}\sigma^2\_{1,m}\)
  为\(\{X\_t\}\)的**一步(线性)预测的均方误差**。

对于平稳序列\(\{X\_t\}\), 如果\(EX\_t=\mu\),
引入\(\{Z\_t\}=\{X\_t-\mu\}\)
和\(m\)维向量 \(\boldsymbol{\mu}\_m=(\mu,\dots,\mu)^T\).

按照最佳线性预测的定义[22.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#def:blpprop-blp-withmu),
\[\begin{aligned}
\hat{X}\_{n+1,m}
=& \mu+ L(X\_{n+1}-\mu |\boldsymbol{X}\_{n,m} - \boldsymbol{\mu}\_m) \\
=& \mu+ L(Z\_{n+1} |\boldsymbol{Z}\_{n,m})
=\mu+ \hat Z\_{n+1,m}.
\end{aligned}\]
于是
\[
E(Z\_{n+1} - \hat Z\_{n+1,m} )^2
= E(X\_{n+1} -\hat X\_{n+1,m})^2.
\]
即对\(X\)预报的均方误差等于对中心化得到的\(Z\)预报的均方误差。
因而, 当且仅当\(\{X\_t - \mu\}\)是决定性平稳序列时,
称\(\{X\_t\}\)是决定性平稳序列.
于是以后只需要讨论零均值的平稳序列.

### 23.1.3 可完全线性预测

设平稳列\(\{X\_t\}\)的\(n+1\)阶自协方差阵\(\Gamma\_{n+1}\)退化，
\(|\Gamma\_n|>0\)。
则\(X\_1,X\_2,\cdots,X\_{n+1}\)线性相关,
所以\(X\_{n+1}\)可以由\(X\_{n}, X\_{n-1},\dots,X\_1\)线性表示.
于是,
\(L(X\_{n+1}|X\_n,\dots,X\_1)=X\_{n+1}\).
当\(m \geq n\)时,
\[\begin{aligned}
L(X\_{n+1}|X\_n,\dots,X\_{n-m+1})=X\_{n+1},
\end{aligned}\]
即有\(\sigma^2\_{1, m}=0\),
\(\{X\_t\}\)是决定性平稳列。

最简单的决定性平稳列是\(X\_t \equiv \xi\),
\(\xi\)为随机变量。

### 23.1.4 离散谱序列

设零均值随机变量\(\xi\_j, \eta\_k (j,k=1,2,\dots,p)\) 两两正交,
满足
\[\begin{align}
E(\xi\_j^2)=E(\eta\_j^2)=\sigma^2\_j, \ j=1,2,\dots
\tag{23.2}
\end{align}\]
对确定的\(j\), 定义简单离散谱序列
\[\begin{align}
Z\_j(t) = \xi\_j \cos(t\lambda\_j) + \eta\_j\sin(t\lambda\_j),
\quad t\in \mathbb Z.
\tag{23.3}
\end{align}\]
可以证明\(\{Z\_j(t)\}\)是平稳序列。
事实上，易见\(E Z\_j(t) \equiv 0\)。
而
\[\begin{aligned}
E[Z\_j(t) Z\_j(s)] =&
E\xi\_j^2 \cos(\lambda\_j t) \cos(\lambda\_j s)
+ E\eta\_j^2 \sin(\lambda\_j t) \sin(\lambda\_j s) \\
=& \sigma\_j^2 \cos((t-s)\lambda\_j)
\end{aligned}\]
只依赖于\(t-s\)。

\(\{Z\_j(t)\}\)的每一次实现是周期函数,
由§2.3的定理3.7 知道\(\{Z\_j(t)\}\)的3阶自协方差矩阵是退化的,
因此[(23.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-nondet-discspecdef0203)是可完全线性预测的，
\(Z\_j(n)\)可以被\(Z\_j(n-1), Z\_j(n-2)\)完全线性预测，是决定性序列。

事实上容易证明
\[\begin{aligned}
& Z\_j(t)
= (2\cos\lambda\_j) Z\_j(t-1) - Z\_j(t-2)
\end{aligned}\]
定义离散谱序列
\[\begin{align}
Z\_t=\sum\_{j=1}^p Z\_j(t), \quad t \in \mathbb Z.
\tag{23.4}
\end{align}\]
这是\(p\)个简单离散谱序列的叠加.
由§[9.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#arspecyw-autocovposd)的定理[9.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-arspecyw.html#thm:arspecyw-discspec)知道由[(23.4)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-nondet-discspecsumdef0204)定义的离散谱序列也是决定性的.
\(Z\_{n}\)可以被\(Z\_{n-1}, Z\_{n-2}, \dots, Z\_{n-2p}\)完全线性预测。

### 23.1.5 纯非决定性

决定性与非决定性取决于一步线性预报误差是否为零。

对非决定性序列，
用\(\{X\_s, s \leq n \}\)预报\(X\_{n+k}\)的误差会随\(k\)增大而增大。
记
\[\begin{aligned}
\sigma\_{k,m}^2
=& E [ X\_{n+k} - L(X\_{n+k} | X\_n, X\_{n-1}, \dots, X\_{n-m+1})]^2
\end{aligned}\]
则\(\sigma\_{k,m}\)也是\(m\)的单调递减函数，与\(n\)无关。可定义
\[\begin{aligned}
\sigma\_k^2 =& \lim\_{m\to\infty} \sigma\_{k,m}^2
\end{aligned}\]
在极限意义下可以证明\(\sigma\_k^2 \geq \sigma\_{k-1}^2\):
\[\begin{align}
\sigma\_k^2
=& \lim\_{m \to \infty}
E(X\_{n+k} -
L(X\_{n+k}|X\_{n},X\_{n-1},\dots,X\_{n-m}))^2\\
=& \lim\_{m \to \infty}
E[X\_{n+k-1}-L(X\_{n+k-1}|X\_{n-1},X\_{n-2},\dots,X\_{n-1-m})]^2 \\
\geq& \lim\_{m \to \infty}
E[X\_{n+k-1}-L(X\_{n+k-1}|X\_{n},X\_{n-1},\dots,X\_{n-m-1})]^2\\
=& \lim\_{m \to \infty} \sigma\_{k-1,m+1}^2 =\sigma^2\_{k-1}.
\tag{23.5}
\end{align}\]

注意上面证明中没有说明\(\sigma\_{k,m}^2\)是\(k\)的增函数。
反例：AR(2)序列
\[\begin{aligned}
X\_t = \frac12 X\_{t-2} + \varepsilon\_t,
\quad \varepsilon\_t \sim \text{WN}(0,\sigma^2)
\end{aligned}\]
平稳解为
\[\begin{aligned}
X\_t = \sum\_{j=0}^\infty \left(\frac12 \right)^j \varepsilon\_{t-2j}
\end{aligned}\]
\[\begin{aligned}
\gamma\_0 =& \frac43 \sigma^2 \quad \gamma\_1 = 0 \\
\gamma\_2 =& \frac23 \sigma^2
\end{aligned}\]
\[\begin{aligned}
L(X\_t | X\_{t-1}) =& 0 \quad \sigma\_{1,1}^2 = \frac43 \sigma^2 \\
L(X\_t | X\_{t-2}) =& \frac12 X\_{t-2} \quad
\sigma\_{2,1}^2 = \sigma^2 < \sigma\_{1,1}^2
\end{aligned}\]

由最佳线性预测定义知
\[\begin{aligned}
\sigma\_{k,m}^2
=& E[X\_{n+k} - L(X\_{n+k} | X\_{n}, X\_{n-1}, \dots, X\_{n-m+1})]^2\\
\leq& E[X\_{n+k}-0]^2 = \gamma\_0
\end{aligned}\]
所以\(\sigma\_k^2 \leq \gamma\_0\)。
\(k\to\infty\)时如果\(\sigma\_k^2\to\gamma\_0\)
则最佳线性预测与用平均值0预测效果相同，没有作用。

**定义23.2** 设\(\{X\_t\}\)是非决定性的平稳序列.
如果\(\lim\_{k\to\infty}\sigma^2\_k=\gamma\_0\),
则称\(\{X\_t\}\)是**纯非决定性的**.

纯非决定性的平稳列不能作长期预报。
非决定性但不是纯非决定性的平稳列作长期预报是有意义的;
当然，决定性序列可以精确地长期预报。

对纯非决定性的平稳序列, 有如下的结果:
\[\begin{align}
\lim\_{k\rightarrow\infty}
\lim\_{m\rightarrow\infty}
E[L(X\_{n+k}|X\_{n},X\_{n-1},\dots,X\_{n-m+1})]^2=0.
\tag{23.6}
\end{align}\]

实际上,
记\(\hat{X}\_{n+k,m}=L(X\_{n+k}|X\_{n},X\_{n-1},\dots,X\_{n-m+1})\).
由投影的正交性得
\[
\sigma\_{k,m}^2 = E(X\_{n+k} - \hat{X}\_{n+k,m} )^2
= E X\_{n+k}^2 -E\hat{X}\_{n+k,m}^2.
\]
于是得到
\[\begin{align}
\lim\_{k\rightarrow\infty}\lim\_{m\rightarrow\infty}
E\hat{X}\_{n+k,m}^2
=\lim\_{k\rightarrow\infty}\lim\_{m\rightarrow\infty}
( \gamma\_0 -\sigma^2\_{k,m})
= \gamma\_0- \gamma\_0 =0.
\tag{23.7}
\end{align}\]

从[(23.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-nondet-pnd-dlim0207)也可看出,
对于纯非决定性的平稳序列做长期或超长期预测是不合适的.

## 23.2 Wold表示定理

### 23.2.1 线性闭包

设\(A\)为Hilbert空间\(H\)的子集，
记\(\mbox{sp}(A)\)或\(L\_A\)为\(A\)的所有有限线性组合构成的集合，
记\(\overline{\mbox{sp}}(A)\)或\(\bar L\_A\)为\(\mbox{sp}(A)\)的元素及其元素极限组成的集合，
记\(H\_A\)为包含\(A\)的最小的闭子空间。

**引理23.1** 设\(A\)为Hilbert空间\(H\)的子集
则
\[
H\_A = \overline{\mbox{sp}}(A)
\]
于是\(\forall \xi \in H\_A\)，
必存在\(\xi\_n \in \mbox{sp}(A), n=1,2,\dots\)
使得
\[\begin{aligned}
\| \xi\_n - \xi \| \to 0, \quad n\to\infty.
\end{aligned}\]

称\(\overline{\mbox{sp}}(A)\)为\(A\)的**线性闭包**，
或由\(A\)生成的子希尔伯特空间，
或由\(A\)张成的子希尔伯特空间。

**证明**:
易见\(\mbox{sp}(A) \subset \overline{\mbox{sp}}(A) \subset H\)。

首先，\(H\_A\)存在而且是\(H\)的闭子空间。
事实上，令
\[
H\_A = \bigcap\_{B\text{是}H \text{的闭子空间且} B \supset A} B
\]
因为\(H \supset A\)所以\(H\_A\)非空。
易见\(H\_A\)也是线性空间，且也是闭集，
所以\(H\_A\)是包含\(A\)的最小闭子空间。

易见\(\mbox{sp}(A)\)为\(H\)的子线性空间，
且由\(A \subset H\_A\)和\(H\_A\)是线性空间知\(\mbox{sp}(A) \subset H\_A\)。
因为\(H\_A\)是闭集所以
\(\overline{\mbox{sp}}(A) \subset H\_A\)。

另一方面，可以证明\(\overline{\mbox{sp}}(A)\)是闭子空间，
由\(H\_A\)的定义及\(A \subset \overline{\mbox{sp}}(A)\)得
\(H\_A \subset \overline{\mbox{sp}}(A)\)。

易见\(\overline{\mbox{sp}}(A)\)是\(H\)的线性子空间。
下面证明\(\overline{\mbox{sp}}(A)\)是闭集。

设\(\xi\_n \in \overline{\mbox{sp}}(A)\),
\(\xi \in H\)使得\(\lim\_{n\to\infty} \| \xi\_n - \xi \| = 0\),
只要证明\(\xi \in \overline{\mbox{sp}}(A)\)。
对\(\xi\_n\)，存在\(\eta\_n \in \mbox{sp}(A)\)使得
\[
\| \xi\_n - \eta\_n \| < \frac{1}{n}
\]
所以
\[\begin{aligned}
\| \eta\_n - \xi \|
\leq& \| \xi\_n - \eta\_n \| + \| \xi\_n - \xi \| \\
\leq& \frac{1}{n} + \| \xi\_n - \xi \| \\
\to& 0, \ (n\to\infty)
\end{aligned}\]
即\(\xi \in \overline{\mbox{sp}}(A)\)，
所以\(\overline{\mbox{sp}}(A)\)是闭子空间，
于是\(\overline{\mbox{sp}}(A) \supset H\_A\)，
从而\(H\_A = \overline{\mbox{sp}}(A)\)。证毕。

○○○○○○

### 23.2.2 无穷历史的线性预测

记\(H\_n\)为\(X\_n, X\_{n-1}, \dots\)生成的闭子空间（线性闭包）。
\(L(X\_{n+k} | X\_n, X\_{n-1}, \dots, X\_{n-m+1})\)当
\(m\to\infty\)时为\(L(X\_{n+k}|H\_n)\)(见后面的定理[23.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimproj))。

**定理23.2** 设\(Y \in L^2\), \(\xi \in H\_n\),
则\(\xi=L(Y|H\_n)\)的充分必要条件是
\[\begin{align}
Y-\xi \perp X\_j, \quad j=n,n-1,n-2,\dots
\tag{23.8}
\end{align}\]

**证明**:

**必要性**: 由定理[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-projperpcond)得到\(Y - \xi \perp H\_n\)所以有[(23.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-condthm)。

**充分性**: 记\(A = \{X\_n, X\_{n-1}, \dots \}\)，
则由[(23.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-condthm)可知\(Y - \xi \perp L\_A\)。
由引理[23.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#lem:wold-wold-closedsubset),
对\(\eta \in H\_n\)有\(\eta\_m \in L\_A\)使\(\eta\_m \to \eta\)，
由内积的连续性可得
\[\begin{aligned}
E((Y-\xi)\eta)
= \lim\_{m\to\infty} E((Y-\xi) \eta\_m) = 0
\end{aligned}\]
即\(Y-\xi \perp H\_n\)，
由定理[22.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-projperpcond)即得\(\xi=L(X\_{n+k}|H\_n)\)。

○○○○○○

\(H\_n\)为\(\{X\_s, s \leq n\}\)所张成的子Hilbert空间，
\(L(X\_{n+k}|H\_n)\)是一个投影。
下面的定理说明这个投影是有穷维最佳线性预测的极限。

**定理23.3** 设\(\boldsymbol{X}\_{n,m} = (X\_n, X\_{n-1}, \dots, X\_{n-m+1})^T\),
当\(m \to\infty\)时
\[\begin{align}
L(Y|\boldsymbol{X}\_{n,m}) \stackrel{\text{m.s.}}{\longrightarrow}
\hat Y \stackrel{\triangle}{=} L(Y|H\_n)
\tag{23.9}
\end{align}\]

**证明**:
记\(\hat Y\_m = L(Y|\boldsymbol{X}\_{n,m})\)。
先证明\(\{\hat Y\_m\}\)是\(H\_n\)中基本列。
显然\(\hat Y\_m \in H\_n\)，设当\(m \to\infty\)时
\[\begin{aligned}
\eta\_m^2 \stackrel{\triangle}{=} E(Y - \hat Y\_m)^2 \to \eta^2
\qquad\text{(注意单调性)}
\end{aligned}\]

对\(m,k\to\infty\)，
注意\(\hat Y\_m, \hat Y\_{m+k}\)都和\(Y - \hat Y\_{m+k}\)正交，得
\[\begin{aligned}
& \|\hat Y\_m - \hat Y\_{m+k} \|^2
= \| \hat Y\_m - Y + Y - \hat Y\_{m+k} \|^2 \\
=& \| \hat Y\_m - Y \|^2 + \| Y - \hat Y\_{m+k} \|^2
+ 2 \langle \hat Y\_m - Y, Y - \hat Y\_{m+k} \rangle \\
=& \eta\_m^2 + \eta\_{m+k}^2 - 2 \langle Y, Y - \hat Y\_{m+k} \rangle \\
=& \eta\_m^2 + \eta\_{m+k}^2
- 2 \langle Y - \hat Y\_{m+k}, Y - \hat Y\_{m+k} \rangle \\
=& \eta\_m^2 + \eta\_{m+k}^2 - 2 \eta\_{m+k}^2 \to 0
\end{aligned}\]
因此\(\{\hat Y\_m\}\)是\(H\_n\)的基本列，
在\(H\_n\)中存在唯一极限\(\xi\)。

由内积的连续性, 对任何\(X\_s, s \leq n\)有
\[\begin{aligned}
\langle X\_s, Y - \xi \rangle
= \lim\_{m \to \infty} \langle X\_s, Y - L(Y|\boldsymbol{X}\_{n,m}) \rangle = 0
\end{aligned}\]
由定理[23.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-projcond)得到\(\xi = L(Y|H\_n)\)。

○○○○○○

### 23.2.3 无穷历史最优线性预测方差

由内积连续性，
\[\begin{aligned}
\sigma\_1^2
\stackrel{\triangle}{=}& \lim\_{m\to\infty} \| X\_{n+1} - L(X\_{n+1} | \boldsymbol{X}\_{n,m}) \|^2 \\
=& \| X\_{n+1} - L(X\_{n+1} | H\_n) \|^2
= \| X\_1 - L(X\_1|H\_0) \|^2
\end{aligned}\]
最后一个等号是因为等号右边也可以写成有限自变量预报均方误差极限。

\(\sigma\_1^2 = 0 \Leftrightarrow X\_1 = L(X\_1 | H\_0)\)，
所以\(\sigma\_1^2 = 0 \Rightarrow X\_1 \in H\_0\)。
反之，如果\(X\_1 \in H\_0\)，
则\(E(X\_1 - X\_1)^2 = 0\)最小所以\(X\_1=L(X\_1|H\_0)\)。
即\(\sigma\_1^2 = 0 \Leftrightarrow X\_1 \in H\_0\)。
这时\(\{X\_t \}\)是决定性序列。
类似地，
\[\begin{aligned}
\sigma\_k^2
\stackrel{\triangle}{=}& \lim\_{m\to\infty} \| X\_{n+k} - L(X\_{n+k} | \boldsymbol{X}\_{n,m}) \|^2 \\
=& \| X\_{n+k} - L(X\_{n+k} | H\_n) \|^2
= \| X\_k - L(X\_k|H\_0) \|^2
\end{aligned}\]

**定理23.4** 设 \(\{X\_t\}\)是零均值平稳列，

(1) \(\{X\_t\}\)是决定性序列当且仅当对某个\(n\)有
\[\begin{align}
X\_{n+1} \in H\_n;
\tag{23.10}
\end{align}\]
并且如果[(23.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-infdimdet0214)对某个\(n\)成立则对所有\(n\)成立，
这时\(H\_n = H\_{n-1}, \forall n \in \mathbb Z\)。

(2) \(\{X\_t\}\)是纯非决定性的当且仅当对某个\(n\)，有
\[\begin{align}
\sigma\_k^2 = \|X\_{n+k} - L(X\_{n+k}|H\_n) \|^2
\to \gamma\_0,
\quad k \to \infty
\tag{23.11}
\end{align}\]
并且如果[(23.11)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-infdimpnd0215)对某个\(n\)成立则对所有\(n\)成立。

### 23.2.4 Wold表示定理

**定理23.5 (Wold表示定理)** 任一非决定性的零均值平稳列可以表示成
\[\begin{align}
X\_t = \sum\_{j=0}^\infty a\_j \varepsilon\_{t-j} + V\_t,
\quad t \in \mathbb Z
\tag{23.12}
\end{align}\]
其中

(1) \(\varepsilon\_t = X\_t - L(X\_t | X\_{t-1}, X\_{t-2}, \dots)\)
是零均值白噪声，满足
\[\begin{aligned}
& E \varepsilon\_t^2 = \sigma^2 > 0,
\quad a\_0 = 1 \\
& a\_j = E(X\_t \varepsilon\_{t-j}) / \sigma^2,\\
& \sum\_{j=0}^\infty a\_j^2 < \infty
\end{aligned}\]

(2) \(\{U\_t = \sum\_{j=0}^\infty a\_j \varepsilon\_{t-j}, \ t \in \mathbb Z\}\)
和\(\{V\_t\}\)都是平稳列且两者互相正交；

(3) 定义\(H\_\varepsilon(t) = \bar{\text{sp}}\{\varepsilon\_s: s \leq t\}\),
\(H\_U(t) = \bar{\text{sp}}\{U\_s: s \leq t\}\),
则\(\forall t\)
\[\begin{aligned}
H\_U(t) = H\_\varepsilon(t)
\end{aligned}\]

(4) \(\{U\_t\}\)是纯非决定性的平稳序列，
有谱密度
\[\begin{aligned}
f(\lambda) = \frac{\sigma^2}{2\pi}
\left| \sum\_{j=0}^\infty a\_j e^{ij\lambda} \right|^2
\end{aligned}\]

(5) \(\{V\_t\}\)是决定性的平稳序列。
对任何\(t, k \in \mathbb Z\)，
\(V\_t \in H\_{t-k}\)。

**定义23.3** 在Wold表示定理中

* (1) 称[(23.12)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-thmdecomp0216)是\(\{X\_t\}\)的**Wold表示**；
* (2) 称\(\{U\_t\}\)是\(\{X\_t\}\)的**纯非决定性部分**，
  称\(\{V\_t\}\)是\(\{X\_t\}\)的**决定性部分**；
* (3) 称\(\{a\_j\}\)是\(\{X\_t\}\)的**Wold系数**；
* (4) 称一步预测误差
  \(\varepsilon\_t = X\_t - L(X\_t | X\_{t-1}, X\_{t-2}, \dots)\)
  为\(\{X\_t\}\)的**新息序列**;
* (5) 称\(\sigma^2 = E \varepsilon\_t^2\)为
  **一步(线性)预测的均方误差**。

这里新息的意思是不能被历史线性预测的部分。
由Wold定理可知任何纯非决定性平稳序列可以表达为新息的单边滑动和。
事实上，任何白噪声的单边滑动和(系数平方可和)一定是纯非决定性的，
但其中的白噪声不一定恰好是新息。

#### 23.2.4.1 ARMA序列的Wold表示

设\(\{X\_t\}\)是ARMA(\(p,q\))序列，模型方程为
\[\begin{aligned}
A(\mathscr B) X\_t = B(\mathscr B) \varepsilon\_t,\quad t\in\mathbb Z,
\end{aligned}\]
设\(A^{-1}(z) B(z)\)有Taylor展开式
\[\begin{aligned}
\Psi(z) = A^{-1}(z) B(z) = \sum\_{j=0}^\infty \psi\_j z^j,
\quad |z| \leq 1
\end{aligned}\]
则
\[\begin{align}
X\_t = \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j},
\quad t \in \mathbb Z.
\tag{23.13}
\end{align}\]
下面证明\(\{X\_t\}\)是纯非决定性的平稳序列，
[(23.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-armawold0219)是\(\{X\_t\}\)的Wold表示，
\(\{\varepsilon\_t\}\)是\(\{X\_t\}\)的新息序列，
\(\{\psi\_j\}\)是\(\{X\_t\}\)的Wold系数。

我们只对比较容易的可逆ARMA的情况证明。
由[(23.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-armawold0219)看出\(X\_t \in H\_\varepsilon(t)\),
利用可逆性，
\(\varepsilon\_t = B^{-1}(\mathscr B) A(\mathscr B) X\_t \in H\_t\),
所以\(H\_t = H\_\varepsilon(t)\).
于是
\[\begin{aligned}
X\_t - \varepsilon\_t = \sum\_{j=1}^\infty \psi\_j \varepsilon\_{t-j}
\in H\_\varepsilon(t-1) = H\_{t-1}
\end{aligned}\]
来证\(X\_t - \varepsilon\_t = L(X\_t | H\_{t-1})\)。
只要证明\(X\_t - (X\_t - \varepsilon\_t) \perp H\_{t-1}\).

事实上，
由于\(\varepsilon\_t\)与\(\varepsilon\_{t-j}, j\geq 1\)正交可知
\(\varepsilon\_t \perp H\_\varepsilon(t-1) = H\_{t-1}\)。故
\[\begin{aligned}
X\_t - \varepsilon\_t =& L(X\_t | H\_{t-1}) \\
\varepsilon\_t =& X\_t - L(X\_t | H\_{t-1})
\end{aligned}\]
即\(\{\varepsilon\_t\}\)是\(\{X\_t\}\)的新息列，
\(\sigma^2 = E\varepsilon\_t^2\)是一步预测均方误差，
在[(23.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-armawold0219)两边同乘以\(\varepsilon\_{t-j}\)后取期望，
利用内积的连续性可得
\[\begin{aligned}
\psi\_j = \langle X\_t, \varepsilon\_{t-j} \rangle /\sigma^2
\end{aligned}\]
即\(\{\psi\_j\}\)是\(\{X\_t\}\)的Wold系数列。

○○○○○○

#### 23.2.4.2 Wold表示定理证明

取\(\varepsilon\_t = X\_t - L(X\_t|H\_{t-1})\),
\(H\_\varepsilon(t) = \overline{\mbox{sp}}\{\varepsilon\_s:\; s \leq t \}\),
易见\(\varepsilon\_t \in H\_t\)，
由\(H\_t\)的单调性可知
\(\varepsilon\_t \in H\_s, \forall t < s\)，因此
\(H\_\varepsilon(t) \subset H\_t\)。

来证明\(\{\varepsilon\_t\}\)是白噪声。

由定理[23.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimproj)
\[\begin{aligned}
L(X\_t|H\_{t-1}) =& \lim\_{m\to\infty}
L(X\_t | X\_{t-1}, \dots, X\_{t-m}) \quad(L^2)\\
=& \lim\_{m\to\infty} \boldsymbol{a}\_m^T \boldsymbol{X}\_{t-1,m}
\end{aligned}\]
其中\(\boldsymbol{a}\_m\)是\(m\)阶Y-W系数(预测方程的解)，
不依赖于\(t\)，
由内积的连续性
\[\begin{aligned}
E \varepsilon\_t^2 =& \lim\_{m\to\infty}
\| X\_t - L(X\_t | X\_{t-1}, \dots, X\_{t-m}) \|^2 \\
=& \lim\_{m\to\infty} (
\gamma\_0 - \boldsymbol{a}\_m^T \Gamma\_m \boldsymbol{a}\_m)
\quad\text{(与}t\text{无关)}\\
=& \lim\_{m\to\infty} \sigma\_{1,m}^2
= \sigma^2 > 0
\quad\text{(由非决定性定义)}
\end{aligned}\]
对\(s>t\)，\(\varepsilon\_s \perp H\_{s-1} \supset H\_t\)所以
\(\varepsilon\_s \perp \varepsilon\_t\), (\(s>t\)时)。即
\[\begin{aligned}
\{\varepsilon\_t\} \sim \text{WN}(0,\sigma^2), \quad \sigma^2>0
\end{aligned}\]

○○○

定义\(V\_t = X\_t - L(X\_t|H\_\varepsilon(t))\),
则\(V\_t \in H\_t\)。
来证明\(\{\varepsilon\_t\}\)和\(\{V\_t\}\)正交。

由投影性质，\(V\_t \perp H\_\varepsilon(t)\)，即
\(V\_t \perp \varepsilon\_s\), \(\forall s \leq t\)。
当\(s>t\)时，
注意\(\varepsilon\_s \perp H\_{s-1} \supset H\_t\)，
而\(V\_t \in H\_t\)所以\(\varepsilon\_s \perp V\_t\), \(\forall s>t\)。
于是\(\{\varepsilon\_t\}\)和\(\{V\_t\}\)正交。

○○○

来证明\(\{\varepsilon\_t\}\)和\(\{X\_t\}\)平稳相关。

当\(s>t\)时\(\varepsilon\_s \perp H\_t\)所以
\(\langle \varepsilon\_s, X\_t \rangle=0\), \(\forall s>t\)。

当\(s \leq t\)时，由定理[23.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimproj)，
\[\begin{aligned}
L(X\_t|H\_{t-1}) =& \lim\_{m\to\infty}
L(X\_t | \boldsymbol{X}\_{t-1,m}) \quad (L^2) \\
=& \lim\_{m\to\infty} \boldsymbol{a}\_m^T \boldsymbol{X}\_{t-1,m}
\end{aligned}\]
由内积的连续性, \(s \leq t\)时
\[\begin{aligned}
\langle X\_t, \varepsilon\_s \rangle
=& \lim\_{m\to\infty}
\langle X\_t, X\_s - \boldsymbol{a}\_m^T \boldsymbol{X}\_{s-1,m} \rangle \\
=& \gamma\_{t-s} -
\lim\_{m\to\infty}(a\_{m1}\gamma\_{t-s+1}
+ \dots + a\_{mm}\gamma\_{t-s+m})
\end{aligned}\]
只依赖于\(t-s\)。
所以\(\{\varepsilon\_t\}\)和\(\{X\_t\}\)平稳相关。

○○○

由\(\{\varepsilon\_t\}\)和\(\{X\_t\}\)平稳相关，
若定义
\[\begin{aligned}
a\_j =& \langle X\_t, \varepsilon\_{t-j} \rangle
/ \sigma^2 \quad (j \geq 0)
\end{aligned}\]
则\(a\_j\)与\(t\)无关。且
\[\begin{aligned}
a\_0 =& \langle X\_t, \varepsilon\_{t} \rangle / \sigma^2 \\
=& \langle \varepsilon\_t + L(X\_t|H\_{t-1}), \varepsilon\_{t} \rangle / \sigma^2 \\
=& \langle \varepsilon\_t, \varepsilon\_t \rangle / \sigma^2 = 1
\end{aligned}\]

○○○

令\(U\_t = L(X\_t|H\_\varepsilon(t))\),
则\(V\_t = X\_t - L(X\_t|H\_\varepsilon(t)) = X\_t - U\_t\),
\(X\_t = U\_t + V\_t\)。
来证明
\[\begin{align}
U\_t = \sum\_{j=0}^\infty a\_j \varepsilon\_{t-j}
\tag{23.14}
\end{align}\]

定义\(U\_{t,n} = L(X\_t | \varepsilon\_t, \varepsilon\_{t-1}, \dots, \varepsilon\_{t-n})\).
设\(U\_{t,n}=\sum\_{j=0}^n b\_j \varepsilon\_{t-j}\)，
由\(\{\varepsilon\_t\}\)与\(\{ X\_t \}\)平稳相关可知\(\{ b\_j \}\)与\(t\)无关。
对\(j=0,1,\dots,n\)
\[\begin{aligned}
\sigma^2 a\_j =& \langle X\_t, \varepsilon\_{t-j} \rangle
= \langle U\_{t,n} + (X\_t - U\_{t,n}), \varepsilon\_{t-j} \rangle \\
=& \langle U\_{t,n}, \varepsilon\_{t-j} \rangle
= \sigma^2 b\_j
\end{aligned}\]
即\(b\_j=a\_j\)，
\[\begin{aligned}
U\_{t,n} = L(X\_t | \varepsilon\_t, \varepsilon\_{t-1}, \dots, \varepsilon\_{t-n})
= \sum\_{j=0}^n a\_j \varepsilon\_{t-j}
\end{aligned}\]

注意\(U\_{t,n}\)是\(X\_t\)的投影所以
\(\|U\_{t,n}\|^2 \leq \|X\_t \|^2 = \gamma\_0\)，
所以
\[\begin{aligned}
\|U\_{t,n} \|^2 = \sigma^2 \sum\_{j=0}^n a\_j^2
\leq \gamma\_0 < \infty
\end{aligned}\]
故\(\sum\_{j=0}^\infty a\_j^2 < \infty\)，
\(\sum\_{j=0}^n a\_j \varepsilon\_{t-j}\)均方收敛到
\(\sum\_{j=0}^\infty a\_j \varepsilon\_{t-j}\)。
由定理[23.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimproj)知\(U\_{t,n}\)均方收敛到\(U\_t=L(X\_t | H\_\varepsilon(t))\)，
所以[(23.14)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-thmproofutdef)成立。

\(\{U\_t\}\)是线性平稳列，
其谱密度立即可得(结论(4)的第二部分)。

○○○

由于\(\{\varepsilon\_t\}\)与\(\{V\_t\}\)正交所以
\(V\_s \perp H\_\varepsilon(t), \forall s,t \in \mathbb Z\)，
而\(U\_t \in H\_\varepsilon(t)\)所以\(V\_s \perp U\_t\),
\(\{V\_t\}\)与\(\{U\_t\}\)正交。
由\(\{U\_t \}\)和\(\{V\_t \}\)正交，
\(X\_t = U\_t + V\_t\), \(\{ X\_t \}\)和\(\{ U\_t \}\)平稳可知
\(V\_t = X\_t - U\_t\)也是平稳列。

至此定理的(1)(2)已证明。

○○○

来证明第(3)条结论。
定义\(H\_U(t) = \bar{\text{sp}}\{U\_s:\; s\leq t\}\),
来证明\(H\_U(t) = H\_\varepsilon(t)\)。

显然\(U\_t \in H\_\varepsilon(t)\)所以\(H\_U(t) \subset H\_\varepsilon(t)\)。
只要证明\(H\_\varepsilon(t) \subset H\_U(t)\)。

注意\(\varepsilon\_t \in H\_t \subset \bar{\text{sp}}\{U\_s, V\_s:\; s\leq t\}\),
由引理[23.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#lem:wold-wold-closedsubset)，
存在\(\xi\_m \in L{\{V\_t, V\_{t-1}, \dots, V\_{t-m}\}}\),
\(\eta\_m \in L{\{U\_t, U\_{t-1}, \dots, U\_{t-m}\}}\),
使
\[\begin{aligned}
\| \xi\_m + \eta\_m - \varepsilon\_t \|^2 \to 0 \ (m\to\infty)
\end{aligned}\]
但前面已证明\(\{V\_t\}\)与\(\{\varepsilon\_t\}\)正交，也与\(\{U\_t\}\)正交，
所以
\[\begin{aligned}
& \| \xi\_m + \eta\_m - \varepsilon\_t \|^2 \\
=& \| \xi\_m \|^2 + \| \eta\_m - \varepsilon\_t \|^2 \\
\geq& \| \eta\_m - \varepsilon\_t \|^2
\end{aligned}\]
令\(m\to\infty\)得
\(\| \eta\_m - \varepsilon\_t \|^2 \to 0\)，
由引理[23.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#lem:wold-wold-closedsubset)知\(\varepsilon\_t \in H\_U(t)\)。
所以\(H\_\varepsilon(t) \subset H\_U(t)\),
\(H\_\varepsilon(t) = H\_U(t)\)。
结论(3)证毕。

○○○

来证明\(\{U\_t\}\)是纯非决定性的(结论(4))。
利用定理[22.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#thm:blpprop-proj-prop)(4)(5)
\[\begin{aligned}
& L(U\_{t+k}|H\_U(t)) = L(U\_{t+k} | H\_\varepsilon(t)) \\
=& L \left[ \sum\_{j=0}^{k-1} a\_j \varepsilon\_{t+k-j}
+ \sum\_{j=k}^\infty a\_j \varepsilon\_{t+k-j} | H\_\varepsilon(t) \right] \\
=& \sum\_{j=k}^\infty a\_j \varepsilon\_{t+k-j}
\end{aligned}\]
于是
\[\begin{aligned}
& \| U\_{t+k} - L(U\_{t+k}|H\_U(t)) \|^2
= \| \sum\_{j=0}^{k-1} a\_j \varepsilon\_{t+k-j} \|^2 \\
= & \sigma^2 \sum\_{j=0}^{k-1} a\_j^2
\to \sigma^2 \sum\_{j=0}^\infty a\_j^2 = E U\_t^2
\end{aligned}\]
按定义可知\(\{U\_t\}\)为纯非决定性的。

注意：这个证明对一般单边线性序列不适用。

○○○

已证明\(\{V\_t\}\)平稳，来证明\(\{V\_t\}\)是决定性的。
用定理[23.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimvar)。
注意\(\varepsilon\_{t-j} \in H\_{t-j}\)，
\[\begin{aligned}
V\_t =& X\_t - U\_t = X\_t - \varepsilon\_t - \sum\_{j=1}^\infty a\_j \varepsilon\_{t-j} \\
=& L(X\_t | H\_{t-1}) - \sum\_{j=1}^\infty a\_j \varepsilon\_{t-j}
\end{aligned}\]
其中\(L(X\_t | H\_{t-1}) \in H\_{t-1}\)，
\(\sum\_{j=1}^\infty a\_j \varepsilon\_{t-j} \in H\_\varepsilon(t-1) \subset H\_{t-1}\)，
所以\(V\_t \in H\_{t-1}\)。

注意\(H\_{t-1} \subset \bar{\text{sp}}\{U\_s, V\_s:\; s \leq t-1\}\),
由引理[23.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#lem:wold-wold-closedsubset)，
存在\(\xi\_m \in L\_{\{V\_{t-1}, \dots, V\_{t-m}\}}\),
\(\eta\_m \in L\_{\{U\_{t-1}, \dots, U\_{t-m}\}}\),
使
\[\begin{aligned}
\| \xi\_m + \eta\_m - V\_t \|^2 \to 0
\end{aligned}\]
已证明\(\{V\_t\}\)与\(\{U\_t\}\)正交所以\(\eta\_m\)与\(\xi\_m - V\_t\)正交，
于是
\[\begin{aligned}
& \| \xi\_m + \eta\_m - V\_t \|^2 = \| \xi\_m - V\_t \|^2 + \| \eta\_m \|^2 \\
\geq& \| \xi\_m - V\_t \|^2
\end{aligned}\]
故\(\| \xi\_m - V\_t \|^2 \to 0(m \to \infty)\)，
由引理[23.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#lem:wold-wold-closedsubset)知\(V\_t \in H\_V(t-1)=\bar{\text{sp}}\{V\_s:\; s\leq t-1\}\)。
由定理[23.4](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#thm:wold-wold-infdimvar)知\(\{V\_t\}\)为决定性的，
且\(H\_V(t) = H\_V(t-j), j\in \mathbb Z\)，
所以\(V\_t \in H\_V(t-j), \; t,j \in \mathbb Z\)。

○○○○○○

#### 23.2.4.3 关于新息的讨论

\(\varepsilon\_t = X\_t - L(X\_t | H\_{t-1})\)是
\(X\_t\)提供的比\(X\_{t-1}, X\_{t-2}, \dots\)多的信息(线性意义下)。

可以证明
\[\begin{align}
H\_t = \text{sp}(\varepsilon\_t) \oplus H\_{t-1}
\tag{23.15}
\end{align}\]
这样
\[\begin{aligned}
H\_t = \mathop\oplus\limits\_{j=0}^\infty \text{sp}(\varepsilon\_{t-j})
\oplus H\_{-\infty} ,
\end{aligned}\]
其中\(H\_{-\infty} = \mathop\cap\limits\_{t} H\_t\)。
\(V\_t \in H\_{-\infty}\)。

事实上，[(23.15)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-wold-innovht)右侧两项正交，
都是闭子空间，且都包含于\(H\_t\)，
则(参见[23.5.2](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#wold-app-orthdecomp))
\[
\text{sp}(\varepsilon\_t) \oplus H\_{t-1}
= \{ \alpha \varepsilon\_t + \xi: \alpha \in \mathbb R,
\xi \in H\_{t-1} \}
\]
是\(H\_t\)内的闭子空间,
\(\text{sp}(\varepsilon\_t) \oplus H\_{t-1} \subset H\_t\)。

来证\(H\_t \subset \text{sp}(\varepsilon\_t) \oplus H\_{t-1}\)。
由引理[23.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#lem:wold-wold-closedsubset)只要证明\(X\_s \in \text{sp}(\varepsilon\_t) \oplus H\_{t-1}, s \leq t\)。

当\(s < t\)时显然，对\(X\_t\)，因为
\[\begin{aligned}
X\_t = \varepsilon\_t + L(X\_t|H\_{t-1})
\end{aligned}\]
所以\(X\_t \in \text{sp}(\varepsilon\_t) \oplus H\_{t-1}\),
于是有
\[\begin{aligned}
H\_t = \text{sp}(\varepsilon\_t) \oplus H\_{t-1} .
\end{aligned}\]

○○○○○○

## 23.3 Kolmogorov公式

考虑多步预报的均方误差。
设\(\{X\_t\}\)是非决定性的平稳列，
由Wold表示定理(5)，\(V\_{t+n} \in H\_t\)，
所以用无穷长历史进行的最佳线性预测为
\[\begin{align}
L(X\_{t+n}|H\_t) =& L(U\_{t+n}|H\_t) + L(V\_{t+n}|H\_t) \\
=& L(\sum\_{j=0}^\infty a\_j \varepsilon\_{t+n-j} | H\_t) + V\_{t+n} \\
=& \sum\_{j=n}^\infty a\_j \varepsilon\_{t+n-j} + V\_{t+n}
\tag{23.16}
\end{align}\]
称\(L(X\_{t+n}|H\_t)\)是\(X\_{t+n}\)的\(n\)步预报，
由Wold分解公式知预报误差为
\[\begin{align}
X\_{t+n} - L(X\_{t+n}|H\_t) = \sum\_{j=0}^{n-1} a\_j \varepsilon\_{t+n-j}
\tag{23.17}
\end{align}\]
预报的均方误差为
\[\begin{align}
\sigma^2(n) = \sigma^2 \sum\_{j=0}^{n-1} a\_j^2
\tag{23.18}
\end{align}\]
\(n\to\infty\)时\(\sigma^2(n) \to E U\_t^2\).

**定理23.6 (Kolmogorov公式)** 设\(\{U\_t\}\)是非决定性平稳序列\(\{X\_t\}\)的
纯非决定性部分, \(f(\lambda)\)是\(\{U\_t\}\)的谱密度. 则有
\[\begin{align}
\sigma^2 = E[X\_t-L(X\_t|H\_{t-1})]^2 =
2\pi \exp\left(\frac{1}{2\pi}
\int^{\pi}\_{-\pi} \ln f(\lambda)d\lambda \right).
\tag{23.19}
\end{align}\]

公式[(23.19)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-kolmog-infpred0224)的证明需要较多解析函数的知识.
当\(\{U\_t\}\)是白噪声时, 公式[(23.19)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-kolmog-infpred0224)明显是成立的.

从Kolmogorov公式[(23.19)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-kolmog-infpred0224)看到,
如果\(\{X\_t\}\)是非决定性的, 则它的纯非决定性部分的谱密度\(f(\lambda)\)必是\(\ln\)可积的,
即
\[\begin{align}
\int^{\pi}\_{-\pi} \ln f(\lambda)d\lambda > -\infty.
\tag{23.20}
\end{align}\]

## 23.4 最佳预测和最佳线性预测相等的条件

设\(\{X\_t\}\)是平稳序列, 用
\(\mathscr F\_t=\sigma\{X\_t,X\_{t-1},\dots\}\)
表示由\(X\_t, X\_{t-1},\dots\)生成的\(\sigma\)-代数.
称条件数学期望
\[ E(X\_{t+k}| \mathscr F\_t) \]
是用全体历史\(\{X\_j: j \leq t\}\)对\(X\_{t+k}\)进行预测时的**最佳预测**.

最佳预测是均方误差最小的，
这是因为条件数学期望\(E(X\_{t+k}| \mathscr F\_t)\)是\(X\_{t-1}, X\_{t-2},\dots\)的函数,
二阶矩有限:
\[
E[E(X\_{t+k}| \mathscr F\_t)]^2 \leq E [E(X\_{t+k}^2| \mathscr F\_t)]
= EX\_{t+k}^2 < \infty.
\]
由概率论中数学期望性质可以证明对任意二阶矩有限的\(X\_t, X\_{t-1},\dots\)的函数\(\xi\)有
\[\begin{align}
E(X\_{t+k} - \xi)^2 \geq E[X\_{t+k} - E(X\_{t+k}|\mathscr F\_t)]^2
\tag{23.21}
\end{align}\]

事实上，对\(\xi \in \mathscr F\_t\)，
\[\begin{aligned}
& E\left[ (X\_{t+k} - \xi)^2 \right] \\
=& E \left\{ \left[
( X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) )
+ ( E(X\_{t+k}|\mathscr F\_t) - \xi ) \right]^2 \right\} \\
=& E \left\{ [ X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) ]^2 \right\}
+ E \left\{ [ E(X\_{t+k}|\mathscr F\_t) - \xi ) ]^2 \right\} \\
& + 2 E \left\{
( X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) )
( E(X\_{t+k}|\mathscr F\_t) - \xi )
\right\}
\end{aligned}\]
而交叉项
\[\begin{aligned}
& E \left\{
( X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) )
( E(X\_{t+k}|\mathscr F\_t) - \xi )
\right\} \\
=& E \left\{ E \left[
( X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) )
( E(X\_{t+k}|\mathscr F\_t) - \xi )
\,|\, \mathscr F\_t \right] \right\} \\
=& E \left\{
( E(X\_{t+k}|\mathscr F\_t) - \xi )
\; E \left[
X\_{t+k} - E(X\_{t+k}|\mathscr F\_t)
\,|\, \mathscr F\_t \right] \right\} \\
= 0
\end{aligned}\]
所以
\[\begin{aligned}
& E\left[ (X\_{t+k} - \xi)^2 \right] \\
=& E \left\{ [ X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) ]^2 \right\}
+ E \left\{ [ E(X\_{t+k}|\mathscr F\_t) - \xi ) ]^2 \right\} \\
\geq& E \left\{ [ X\_{t+k} - E(X\_{t+k}|\mathscr F\_t) ]^2 \right\}
\end{aligned}\]

最佳预测一般比最佳线性预测好，但是对纯非决定性序列如果其新息是独立序列则二者等价。

**定理23.7** 设平稳序列\(\{X\_t\}\)有Wold表示
\[\begin{align}
X\_{t}=\sum\_{j=0}^{\infty} a\_j \varepsilon\_{t-j}, \ \ t \in \mathbb Z.
\tag{23.22}
\end{align}\]
则
\[\begin{align}
L(X\_{t+n}|H\_t)=E(X\_{t+n}|\mathscr F\_t), \ n \geq 1, \ \ t\in \mathbb Z,
\tag{23.23}
\end{align}\]
成立的充分必要条件是
\[\begin{align}
E(\varepsilon\_{t+1}|\varepsilon\_{t},\varepsilon\_{t-1},\dots)
=0, \ \ t\in \mathbb Z.
\tag{23.24}
\end{align}\]

[(23.24)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#eq:wold-bestp-mardiff0229)的条件称为鞅差。
零均值独立白噪声列是鞅差的特例。

**推论23.1** 设ARMA(\(p,q\))序列\(\{X\_t\}\)中的新息\(\{\varepsilon\_t\}\)是独立白噪声,
则用全体历史\(\{X\_t,X\_{t-1},\dots\}\)对\(X\_{t+n}\)进行预测时,
最佳预测和最佳线性预测相等.

## 23.5 附录：补充

非决定性也称为非奇异，决定性序列称为奇异序列。
见谢衷洁《时间序列分析》P.82。
纯非决定性序列叫做正则序列，
见谢衷洁《时间序列分析》P.118第13题。

### 23.5.1 关于预测的分类

```
knitr::include_graphics("figs/forecast-class.png")
```

![时间序列预测分类](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/figs/forecast-class.png)

图23.1: 时间序列预测分类

### 23.5.2 正交直和分解

**定理**：设\(H\)为Hilbert空间，
\(H\_1\)和\(H\_2\)是\(H\)的闭子空间，
\(H\_1\)与\(H\_2\)正交，
\(M = H\_1 \oplus H\_2 = \{ x + y: x \in H\_1, y \in H\_2 \}\)，
则\(M\)是\(H\)的闭子空间，
且\(\forall \xi \in H\),
\[
L(\xi | M)
= L(\xi | H\_1) \oplus L(\xi | H\_2),
\]
其中\(\oplus\)表示求和，
且求和的两项正交。

**证明**：
对\(\xi, \eta \in M\)，
应有\(\xi\_1, \eta\_1 \in H\_1\),
\(\xi\_2, \eta\_2 \in H\_2\)使得\(\xi = \xi\_1 + \xi\_2\),
\(\eta = \eta\_1 + \eta\_2\)，
于是\(\xi + \eta = (\xi\_1 + \eta\_1) + (\xi\_2 + \eta\_2) \in M\)；
对\(\alpha \in \mathbb R\)，
\(\alpha \xi = (\alpha \xi\_1) + (\alpha \xi\_2) \in M\)，
所以\(M\)是\(H\)的子线性空间。

对\(M\)中的基本列\(\{ \xi\_n \}\)，
有分解\(\xi\_n = \xi\_{1,n} + \xi\_{2,n}\)，
\(\xi\_{1,n} \in H\_1\),
\(\xi\_{2,n} \in H\_2\),
\[\begin{aligned}
0 =& \lim\_{n,m \to \infty} \| \xi\_n - \xi\_m \|^2
= \lim\_{n,m \to \infty} \left( \| \xi\_{1,n} - \xi\_{1,m} \|^2
+ \| \xi\_{2,n} - \xi\_{2,m} \|^2 \right)
\end{aligned}\]
从而\(\{ \xi\_{1,n} \}\)是\(H\_1\)的基本列，
\(\{ \xi\_{2,n} \}\)是\(H\_2\)的基本列，
存在\(\xi\_1 \in H\_1\), \(\xi\_2 \in H\_2\)，
使得\(\lim \| \xi\_{1,n} - \xi\_1 \| = 0\),
\(\lim \| \xi\_{2,n} - \xi\_2 \| = 0\),
于是
\[
\lim\_{n\to\infty} \| \xi\_n - (\xi\_1 + \xi\_2) \|^2
= \lim\_{n\to\infty} \left( \| \xi\_{1,n} - \xi\_1 \|^2
+ \| \xi\_{2,n} - \xi\_2 \|^2 \right) = 0
\]
因\(\xi\_1 + \xi\_2 \in M\)，
所以\(M\)是\(H\)的闭子空间。

\(\forall \xi \in H\),
令\(\xi\_1 = L(\xi | H\_1)\),
\(\xi\_2 = L(\xi | H\_2)\),
则\(\xi\_1 + \xi\_2 \in M\)，
且
\[\begin{aligned}
\xi - (\xi\_1 + \xi\_2) =& (\xi - \xi\_1) + (-\xi\_2) \perp H\_1, \\
\xi - (\xi\_1 + \xi\_2) =& - \xi\_1 + (\xi - \xi\_2) \perp H\_2, \\
\end{aligned}\]
因此\(\xi - (\xi\_1 + \xi\_2)\)与\(M = H\_1 + H\_2\)正交，
从而\(\xi\_1 + \xi\_2 = L(\xi | M)\)，
且\(\xi\_1 \perp \xi\_2\)。
结论得证。

### 23.5.3 单边线性序列与Wold表示

单边线性序列一定是纯非决定性的，
但其中的白噪声不一定是新息，
所以表达式本身不一定是Wold表示。

如
\[\begin{aligned}
X\_t = \varepsilon\_t + 2 \varepsilon\_{t-1}
\end{aligned}\]
是纯非决定性序列，
其谱密度
\[\begin{aligned}
f(\lambda) = \frac{\sigma^2}{2\pi} | 1 + 2 e^{i\lambda} |^2
= \frac{4\sigma^2}{2\pi} | 1 + \frac12 e^{i\lambda} |^2
\end{aligned}\]
但\(\varepsilon\_t \neq X\_t - L(X\_t | H\_{t-1})\)。

令
\[
\eta\_t = (1 - \frac12 \mathscr B)^{-1} X\_t
\]
则\(\{ \eta\_t \}\)是\(\{ X\_t \}\)的系数绝对可和的线性滤波，
故平稳，
且\(\{ \eta\_t \}\)的谱密度为
\[
f\_\eta(\lambda) = | 1 - \frac12 e^{-i\lambda} |^{-2} f(\lambda)
= \frac{4\sigma^2}{2\pi}
\]
所以\(\{ \eta\_t \}\)是WN(0, \(4\sigma^2\))，
\[
X\_t = \eta\_t + \frac12 \eta\_{t-1}
\]
是可逆MA(1)模型。
由上面关于可逆ARMA模型的一般结论可知\(\eta\_t\)是\(\{ X\_t \}\)的新息，
而\(\varepsilon\_t\)与\(\eta\_t\)方差不同，
不会a.s.相等，
由新息的唯一性可知\(\{ \varepsilon\_t \}\)不是\(\{ X\_t \}\)的新息。
所以单边线性序列中的白噪声列不一定是新息。

### 23.5.4 离散谱序列可完全线性预测的直接证明

对
\[\begin{aligned}
Z\_j(t) =& \xi\_j \cos(t\lambda\_j) + \eta\_j \sin(t \lambda\_j),
\ t \in \mathbb Z,
\end{aligned}\]
有
\[\begin{aligned}
& 2 \cos\lambda\_j Z\_j(t-1) - Z\_j(t-2) \\
=& \xi\_j \{ 2\cos\lambda\_j \cos[(t-1)\lambda\_j] - \cos[(t-2)\lambda\_j] \} \\
& + \eta\_j \{ 2\cos\lambda\_j \sin[(t-1)\lambda\_j] - \sin[(t-2)\lambda\_j] \} \\
=& \xi\_j \{ \cos(t\lambda\_j) + \cos[(t-2)\lambda\_j] - \cos[(t-2)\lambda\_j] \} \\
& + \eta\_j \{ \sin(t\lambda\_j) + \sin[(t-2)\lambda\_j] - \sin[(t-2)\lambda\_j] \} \\
=& Z\_j(t)
\end{aligned}\]
所以
\[\begin{aligned}
L(Z\_j(t) | Z\_j(t-1), Z\_j(t-2)) = 2\cos\lambda\_j Z\_j(t-1) - Z\_j(t-2).
\end{aligned}\]
但是混合多个频率的离散谱序列的预测公式就没有这么容易。