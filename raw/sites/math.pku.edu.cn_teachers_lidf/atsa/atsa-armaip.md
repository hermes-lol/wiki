---
crawl_time: '2026-01-17 14:29:13'
framework: sphinx
title: 25 ARMA序列的递推预测 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 25 ARMA序列的递推预测

新息预测方法从理论上很完美，
对平稳列只需要知道自协方差函数列\(\{\gamma\_k\}\)，
还可以预测非平稳列。
但是对样本数据协方差需要估计。
注意新息预测系数中需要用到\(\gamma\_n\)，
但是如果样本量不够的话估计\(\gamma\_n\)会产生很大误差，
导致预测误差很大。
如果我们知道序列服从ARMA模型，
就可以用比较少的自协方差估计得到模型参数估计，
用模型参数来得到预报公式，
这样的预报结果受随机误差影响比较小。
另外，ARMA模型中的白噪声如果是独立白噪声，
最佳线性预报还是最佳预报。
这个结果虽然是对无穷长历史得到的但是样本量足够大时也近似成立。

## 25.1 AR序列的预测

### 25.1.1 AR序列的一步预测

设\(\{X\_t\}\)满足AR(\(p\))模型
\[\begin{align}
X\_t=\sum\_{j=1}^p a\_j X\_{t-j}+\varepsilon\_t, \quad
t\in \mathbb Z,
\tag{25.1}
\end{align}\]
其中\(\{\varepsilon\_t\}\)是零均值白噪声,
特征多项式
\[\begin{aligned}
A(z)=1 - \sum\_{j=1}^p a\_j z^j \neq 0, \quad
|z|\leq 1
\end{aligned}\]
考虑用\(\boldsymbol{X}\_n = (X\_1,X\_{2},\dots,X\_{n})^T\)预测\(X\_{n+1}\)的问题.
设\(\{\gamma\_n\}\)是\(\{X\_t\}\)的自协方差函数.

对于\(1\leq n \leq p-1\), 由最佳线性预测的性质1知道
\[\begin{aligned}
\hat X\_{n+1} =& L(X\_{n+1} | \boldsymbol{X}\_n)
=\boldsymbol{\gamma}\_n^T \Gamma\_n^{-1} \boldsymbol{X}\_n \\
=& a\_{n,1} X\_n + a\_{n,2} X\_{n-1} + \dots + a\_{n,n} X\_1
\end{aligned}\]
其中\(\Gamma\_n\)是\(\{X\_t\}\)的\(n\)阶自协方差矩阵,
\(\boldsymbol{\gamma}\_n = (\gamma\_n,\gamma\_{n-1},\dots,\gamma\_1){^T}\),
\((a\_{n,1}, a\_{n,2}, \dots, a\_{n,n})\)是\(n\)阶Yule-Walker系数。
预测的均方误差是
\[
E(X\_{n+1}-\hat X\_{n+1})^2 = \gamma\_0
- \boldsymbol{\gamma}\_n^T \Gamma\_n^{-1}\boldsymbol{\gamma}\_n.
\]
可以用Levinson递推公式计算线性组合系数系数和均方误差。
（见§[24.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#ipred-staipred)）

对于\(n\geq p\),
由于当\(k\geq 1\),
\(X\_t=\sum\_{j=0}^{\infty} \psi\_j \varepsilon\_{t-j}\)与\(\varepsilon\_{t+k}\)正交,
所以对 \(n\geq p\)
\[\begin{aligned}
\hat{X}\_{n+1} =& L(\varepsilon\_{n+1} + \sum\_{j=1}^p a\_j X\_{n+1-j}|\boldsymbol{X}\_n) \\
=& \sum\_{j=1}^p a\_j X\_{n+1-j}
\end{aligned}\]
由于\(X\_{n+1} - \hat X\_{n+1}\)与\(X\_n, \dots, X\_1\)正交，
可见\(n\geq p\)时
\[
\hat X\_{n+1} = L(X\_{n+1}|X\_n,X\_{n-1},\dots,X\_{n-p+1}).
\]

### 25.1.2 AR序列的多步预测

下面考虑用\(\boldsymbol{X}\_n\)预测\(X\_{n+k}(k\geq 1)\)的问题.
对于\(n\geq p\), 对\(k\)用归纳法容易证明(习题4.1)
\[
L(X\_{n+k}|\boldsymbol{X}\_n)
= L(X\_{n+k}|X\_n,X\_{n-1},\dots,X\_{n-p+1}).
\]
当\(n \geq p\)时，记
\[\begin{aligned}
\hat X\_{n,m} =
\begin{cases}
L(X\_m | \boldsymbol{X}\_n ), & m > n \\
X\_m, & m \leq n
\end{cases}
\end{aligned}\]
则
\[\begin{aligned}
& L(X\_{n+k}|\boldsymbol{X}\_n)
= L(\sum\_{j=1}^p a\_j X\_{n+k-j} + \varepsilon\_{n+k}|\boldsymbol{X}\_n) \\
=& L(\sum\_{j=1}^p a\_j X\_{n+k-j}|\boldsymbol{X}\_n)
= \sum\_{j=1}^p a\_j \hat X\_{n,n+k-j},\\
& k=1,2,\dots
\end{aligned}\]
可以递推计算\(\hat X\_{n,m}, m>n\)。

### 25.1.3 例：AR(1)预测

考虑AR(1)模型
\[
X\_n = a\_1 X\_{n-1} + \varepsilon\_n
\]
则对任何\(m\in \mathbb N\),
\[\begin{aligned}
L(X\_{n+1}|X\_n,X\_{n-1},\dots,X\_{n-m+1}) =& a\_1 X\_{n},\\
L(X\_{n+2}|X\_n,X\_{n-1},\dots,X\_{n-m+1}) =& a\_1 L(X\_{n+1}|X\_n) \\
=& a\_1^2 X\_n,\\
\cdots & \cdots\\
L(X\_{n+k}|X\_n,X\_{n-1},\dots,X\_{n-m+1}) = &a\_1^k X\_{n}.
\end{aligned}\]
当\(k\to {\infty}\), 利用\(|a\_1|<1\)和控制收敛定理得到
\[\begin{aligned}
& E[X\_{n+k}-L(X\_{n+k}|X\_n,X\_{n-1},\cdots, X\_{n-m})]^2 \\
=& E(X\_{n+k}-a\_1^k X\_n)^2
= E(X\_{n+k}^2) - a\_1^{2k} E( X\_n^2 ) \\
=& \gamma\_0 (1 - a\_1^{2k})
\to \gamma\_0
= E X\_0^2, \ \hbox{当 }\ k \to \infty.
\end{aligned}\]
这与AR(1)序列是纯非决定性的平稳序列有关.
实际上任何ARMA(\(p,q\))序列都是纯非决定性的(见§[23.2.4.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-wold.html#wold-wold-armawold)).

### 25.1.4 例：降雨量预测

平均降雨量为\(\bar{X}=540\)mm.
用\(X\_t,t=1,2,\dots\)表示该地区的逐年降雨量.
如果
\[
Y\_t=X\_t-\bar{X}
\]
满足AR(2)模型
\[
Y\_t = -0.54 Y\_{t-1} + 0.3 Y\_{t-2} + \varepsilon\_t,
\ \varepsilon\_t\sim \text{WN}(0,\sigma^2)
\]
给定观测\(X\_1=560\), \(X\_2=470\), \(X\_3=580\), \(X\_4=496\), \(X\_5=576\),
求\(X\_8\)的最佳线性预测.`

首先中心化得，
\[\begin{aligned}
& Y\_1=560-540=20, \ Y\_2=470-540=-70,\\
& Y\_3=580-540=40, \ Y\_4=496-540=-44,\\
& Y\_5=576-540=36.\\
\end{aligned}\]

用递推公式计算。
记\(\hat Y\_j=L(Y\_j | Y\_1,Y\_2,...,Y\_5)\), \(6\leq j \leq 8\), 则有  
\[\begin{aligned}
\hat{Y}\_6 =& -0.54\times 36 + 0.3\times (-44) = -32.64,\\
\hat{Y}\_7 =& -0.54 \hat{Y}\_6 + 0.3\times 36 = 28.43 \\
\hat{Y}\_8 =& -0.54 \hat Y\_7 + 0.3\hat Y\_6 = -25.14
\end{aligned}\]

最后加上平均值得
\[\begin{aligned}
\hat{X}\_6 &= 540-32.64=507.36\\
\hat{X}\_7 &= 540+28.43=568.43\\
\hat{X}\_8 &= 540-25.14=514.86.
\end{aligned}\]
在本例中, 特征多项式\(A(z)=1+0.54z-0.3z^2\)
有两个实根\(z\_1=-1.1355\),
\(z\_2=2.9355\).
最靠近单位圆的根的辐角是\(\pi\),
所以序列有周期\(T=2\)的特性.
预测数据也体现了围绕均值\(540\)上下交替变化的特性.

## 25.2 MA序列的预测

设\(\{\varepsilon\_t\}\)是\(\text{WN}(0,\sigma^2)\),
实系数多项式\(B(z)=1 + b\_1 z+ \dots + b\_q z^q\)在单位圆内无根:
\[
B(z)\neq 0, |z|<1
\]
满足MA(\(q\))模型
\[\begin{align}
X\_t = B(\mathscr B) \varepsilon\_t, \quad t\in \mathbb Z,
\tag{25.2}
\end{align}\]
的MA(\(q\))序列\(\{X\_t\}\)的自协方差函数\(\{\gamma\_k\}\)是\(q\)后截尾的.
假设\(\sigma^2\), \(b\_1, b\_2, \dots, b\_q\)已知,
我们考虑用\(X\_1, X\_2,\dots,X\_n\)预测\(X\_{n+k}\)的问题.

从[(24.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-ortheq0303)(新息预报)可以看出, 对\(n\geq 1\)
\[\begin{align}
L\_n = \overline{\mbox{span}}\{X\_1,X\_2,\cdots,X\_n\}
= \overline{\mbox{span}}\{{\hat \varepsilon}\_1,{\hat \varepsilon}\_2, \dots, {\hat \varepsilon}\_n\},
\tag{25.3}
\end{align}\]
这里\({\hat \varepsilon}\_n = X\_n - L(X\_n|\boldsymbol{X}\_{n-1})\)
是\(\{X\_t\}\)的逐步预测误差,
\(\boldsymbol X\_n = (X\_1,\dots,X\_n)^T\).

以下假定\(n \geq q\).
由于\(\{{\hat \varepsilon}\_t\}\)是正交序列,
并由\(q\)步截尾性可知\(X\_{n+1}\)与 \(L\_{n-q}\) 正交，
所以\(X\_{n+1}\)与\(\hat\varepsilon\_{n-q}, \hat\varepsilon\_{n-q-1}, \dots, \hat\varepsilon\_1\)正交，
根据最佳线性预测的性质6、4、10得到
\[\begin{align}
& L(X\_{n+1}|\boldsymbol{X}\_n)
= L(X\_{n+1} | {\hat \varepsilon}\_{n},\dots,{\hat \varepsilon}\_{1})
\quad \text{(性质10及(4.3))}\\
=& L(X\_{n+1}|{\hat \varepsilon}\_{n},\dots,{\hat \varepsilon}\_{n-q+1})
+ L(X\_{n+1}|{\hat \varepsilon}\_{n-q},\dots,{\hat \varepsilon}\_{1})
\quad \text{(性质6)}\\
=& L(X\_{n+1}|{\hat \varepsilon}\_{n},\dots,{\hat \varepsilon}\_{n-q+1})
\quad \text{(性质4)}\\
=& \sum\_{j=1}^q \theta\_{n,j} {\hat \varepsilon}\_{n+1-j}.
\quad \text{(新息预测公式)}
\tag{25.4}
\end{align}\]
预测的均方误差
\[\begin{align}
\nu\_{n} = E{\hat \varepsilon}\_{n+1}^2
=\gamma\_0 - \sum\_{j=1}^{q} \theta\_{n,j}^2 \nu\_{n-j}.
\tag{25.5}
\end{align}\]
这里的\(\{\theta\_{n,j}\}\), \(\nu\_n\)可以利用[(24.13)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-staiprediter0312)进行递推计算,
但注意\(\{\gamma\_k\}\)是\(q\)步截尾的。

## 25.3 ARMA序列的预测

对于满足ARMA(\(p,q\))模型
\[\begin{align}
A(\mathscr B) X\_t = B(\mathscr B)\varepsilon\_t,
\quad t\in \mathbb Z,
\tag{25.6}
\end{align}\]
的ARMA序列\(\{X\_t\}\),
定义\(m=\max(p,q)\)和
\[\begin{align}
Y\_t=\begin{cases}
X\_t/\sigma, & t=1,2,\dots,m ,\\
A(\mathscr B) X\_t/\sigma, & t=m+1,\dots.
\end{cases}
\tag{25.7}
\end{align}\]

则\(\{Y\_t\}\)由ARMA(\(p,q\))模型[(25.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armamod0407)的参数
\(\boldsymbol{\beta} =(a\_1, a\_2,\dots,a\_p,b\_1,b\_2,\dots,b\_q)^T\)
和标准白噪声\(\{\varepsilon\_t/\sigma\}\)决定,
从而不依赖\(\sigma\).
假设模型[(25.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armamod0407)中的参数已知,
我们考虑用\(X\_1,X\_2,\cdots,X\_n\)对\(X\_{n+k}\)进行逐步预测的问题.

从\(Y\_t\)的定义知道,
对\(t\geq 1\), \(Y\_t\in L\_t=\overline{\mbox{span}}\{X\_1,X\_2,\dots,X\_t\}\),
并且 \(X\_1,X\_2,\dots,X\_m \in \overline{\mbox{span}}\{Y\_1,Y\_2,\dots,Y\_m\}\).
容易看出, 对\(t> m\),
\[
X\_t = \sigma Y\_t + \sum\_{j=1}^p a\_j X\_{t-j}
\ \in \ \overline{\mbox{span}}\{Y\_1,Y\_2,\dots,Y\_t\}.
\]

于是再利用[(24.3)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-ortheq0303)(新息与原序列互相线性表示)得到
\[\begin{aligned}
L\_n =& \overline{\mbox{span}}\{X\_1,X\_2,\dots,X\_n\}\\
=& \overline{\mbox{span}}\{Y\_1,Y\_2,\dots,Y\_n\} = \overline{\mbox{span}}\{W\_1,W\_2,...,W\_n\},
\end{aligned}\]
其中 \(W\_t = Y\_t - L(Y\_t|\boldsymbol{Y}\_{t-1})\),
\(W\_1=Y\_1\) 是\(\{Y\_t\}\)的样本新息.

用\(\gamma\_k\)表示\(\{X\_t\}\)的自协方差函数,
取\(b\_0=1\), \(b\_j=0\), 当\(j>q\). 可以计算出
\[\begin{align}
E(Y\_s Y\_t)=\begin{cases}
\sigma^{-2}\gamma\_{t-s}, & 1\leq s\leq t\leq m ,\\
\sigma^{-2}[\gamma\_{t-s} - \sum\_{j=1}^p a\_j\gamma\_{t-s-j}],
\ \ & 1\leq s \leq m <t ,\\
\sum\_{j=0}^{q} b\_j b\_{j+t-s}, & t\geq s>m ,
\end{cases}
\tag{25.8}
\end{align}\]
其中的 \(\{\gamma\_k\}\)可用§[13.3](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-gamma)的[(13.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-gam-gampsi0210)和[(13.9)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#eq:armamod-gam-psiiter0211)计算.

定义\(\{X\_t\}\)的逐步预测误差
\[
Z\_t = X\_t - L(X\_t|\boldsymbol{X}\_{t-1}),
\ Z\_1=X\_1,
\]
则对\(1\leq t\leq m\),
\[\begin{aligned}
W\_t =& X\_t / \sigma - L(X\_t/\sigma |\boldsymbol{X}\_{t-1}) \\
=& \sigma^{-1} [X\_t-L(X\_t|\boldsymbol{X}\_{t-1})]
= \sigma^{-1} Z\_t
\end{aligned}\]

对\(t\geq m+1\),  
\[\begin{aligned}
W\_t =& \sigma^{-1}[X\_t - \sum\_{j=1}^p a\_j X\_{t-j}
- L(X\_t- \sum\_{j=1}^p a\_jX\_{t-j} | \boldsymbol{X}\_{t-1})]\\
=& \sigma^{-1} [X\_t-L(X\_t|\boldsymbol{X}\_{t-1})]
= \sigma^{-1} Z\_t
\end{aligned}\]
所以\(\{X\_t\}\)和\(\{Y\_t\}\)的预测误差之间总有以下的关系
\[\begin{align}
Z\_t= \sigma W\_t,
\ EZ\_t^2=\sigma^2 EW\_t^2,
\ t=1,2,\dots
\tag{25.9}
\end{align}\]

以下仍用\(\nu\_{t-1}\)表示 \(E W\_{t}^2\),
就有\(E Z\_t^2=\sigma^2 \nu\_{t-1}\).
对于\(1\leq n < m=\max(p,q)\),  
从逐步预测公式[(24.5)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-orthdec0305)得到
\[
\hat Y\_{n+1} = L(Y\_{n+1}|\boldsymbol{Y}\_n)
= \sum\_{j=1}^n \theta\_{n,j}W\_{n+1-j}.
\]
于是对\(1\leq n < m\),
\[\begin{aligned}
\hat X\_{n+1} =& L(X\_{n+1}|\boldsymbol{X}\_n)
= L(\sigma Y\_{n+1}|\boldsymbol{Y}\_n)\\
=& \sigma \hat Y\_{n+1}
= \sum\_{j=1}^n \theta\_{n,j}\sigma W\_{n+1-j}
= \sum\_{j=1}^n \theta\_{n,j}Z\_{n+1-j}.
\end{aligned}\]

对于\(n \geq m\), 利用ARMA序列的因果性即
\(E(X\_t\varepsilon\_{t+k})=0\), \(k=1,2,\dots,\)得到
\[
Y\_{n+1} = \sigma^{-1} B(\mathscr B)\varepsilon\_{n+1}
=\sigma^{-1}\sum\_{j=0}^q b\_j\varepsilon\_{n+1-j}
\]
与\(\{X\_{j}: 1\leq j\leq n-q\}\)正交,
从而与
\[
\overline{\mbox{span}}\{Y\_{j}: 1\leq j\leq n-q\}
= \overline{\mbox{span}} \{W\_{j}: 1\leq j\leq n-q \}
\]
中的任何随机变量正交.

利用最佳线性预测的性质6、4得到
\[
L(Y\_{n+1} | \boldsymbol{Y}\_n)
= \sum\_{j=1}^q \theta\_{n,j} W\_{n+1-j},
\ n \geq m=\max(p,q).
\]

于是对\(n \geq m\), 由\(Y\)和\(X\)的关系得到
\[\begin{aligned}
\hat X\_{n+1} =& L(\sum\_{j=1}^p a\_j X\_{n+1-j} + \sigma Y\_{n+1}|\boldsymbol{X}\_n)\\
=& \sum\_{j=1}^p a\_j X\_{n+1-j}
+ \sigma \sum\_{j=1}^q \theta\_{n,j} W\_{n+1-j} \\
=& \sum\_{j=1}^p a\_j X\_{n+1-j} + \sum\_{j=1}^q \theta\_{n,j}Z\_{n+1-j}.
\end{aligned}\]

总结上述推导得到:
\[\begin{align}
\hat X\_{n+1} = \begin{cases}
\sum\_{j=1}^n \theta\_{n,j} Z\_{n+1-j},
& 1\leq n < m,\\
\sum\_{j=1}^p a\_j X\_{n+1-j}
+ \sum\_{j=1}^q \theta\_{n,j} Z\_{n+1-j},
& \ n\geq m.
\end{cases}
\tag{25.10}
\end{align}\]

预测的均方误差仍然是\(EZ\_{n+1}^2=\sigma^2 \nu\_n\).
这里的\(\{\theta\_{n,j}\}\), \(\nu\_n=EW\_{n+1}^2\)可以利用[(25.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armacov0409)和[(24.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-coef0306)进行递推计算.
\(Z\_{n+1} = X\_{n+1} - \hat X\_{n+1}\)也可以递推计算。

因为\(\{Y\_t\}\)和\(\sigma\) 无关,
所以\(\theta\_{n,k}\), \(\{W\_n\}\)以及\(\nu\_{n-1}\)都是和\(\sigma^2\)无关的量.
它们只依赖于参数 \(\boldsymbol{a} = (a\_1,a\_2,\dots,a\_p)^T\)
和\(\boldsymbol{b} = (b\_1,b\_2,\dots,b\_q)^T\).
这个性质在研究ARMA模型的最大似然估计时将得到应用.

### 25.3.1 预报例子

考虑§[13.7.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armamod.html#armamod-examp42)和§[22.1.12](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#blpprop-blp-exa)的ARMA(4,2)模型。
\[\begin{aligned}
X\_t =& -0.9 X\_{t-1} - 1.4 X\_{t-2} - 0.7 X\_{t-3} - 0.6 X\_{t-4} \\
& + \varepsilon\_t + 0.5 \varepsilon\_{t-1} - 0.4 \varepsilon\_{t-2},
\ \varepsilon\_t \sim \text{WN}(0,1),
\end{aligned}\]

已计算自协方差函数
\(\gamma\_0, \gamma\_1, \dots, \gamma\_{20}\)，
由此用[(25.8)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armacov0409)计算出\(E(Y\_t Y\_S), (1\leq s,t \leq 21)\).
用§[24.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#ipred-genipred)的公式[(24.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-coef0306)计算出新息预报系数\(\theta\_{n,j}\)如下：

\[
\begin{array}{lrrrrrrr}
n& 1 & 2 &3 &4& \cdots& 19 & 20\\
\theta\_{n,1} & -0.226 & -0.4017 & -0.5705 & 0.1807 & \cdots & 0.4875 & 0.489 \\
\theta\_{n,2} & 0 & -0.6865 & -0.6353 & -0.1597 & \cdots & -0.3937 & -0.394 \\
\theta\_{n,3}& 0 & 0 & 0.3699 & -0.0000 & \cdots & -0.0001 & 0.000\\
\theta\_{n,4}& 0 & 0 & 0 & -0.0000 & \cdots & 0.0000 & - 0.000\\
\end{array}
\]

该模型的观测数据\(x\_1,\cdots,x\_{21}\)为：
\[\begin{array}{\*9c}
-0.4587 & 0.7125 & 1.9948 & -4.5285 & -0.7514 & 5.8782 & -0.1273 \\
-2.9223 & -0.7581 & 1.1422 & 2.1107 & -0.5640 & -2.4452 & -0.5105
\end{array}\]
利用公式[(25.10)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-armaip.html#eq:armaip-armaformu0411)和[(24.6)](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-ipred.html#eq:ipred-gp-coef0306)可以计算出逐步预测
\(\hat X\_{k+1}=L(X\_{k+1}|\boldsymbol X\_k)\)
和逐步预测的均方误差 \(\nu\_{k-1}= E W\_k^2\)如下:

\[
\begin{array}{rrr}
j & \hat X\_j & \nu\_{j-1} \\
1 & 0 & 6.670 \\
2 & 0.104 & 6.330 \\
3 & 0.070 & 2.505 \\
4 & -1.654 & 2.387 \\
5 & 0.232 & 1.268 \\
6 & 5.385 & 1.233 \\
7 & -1.788 & 1.142 \\
8 & -4.398 & 1.114 \\
9 & -0.837 & 1.086 \\
10 & 0.839 & 1.069 \\
11 & 2.259 & 1.056 \\
12 & -1.395 & 1.046 \\
13 & -2.354 & 1.038 \\
14 & 0.467 & 1.031 \\
15 & 2.585 & 1.026 \\
16 & 1.069 & 1.022 \\
17 & -1.668 & 1.018 \\
18 & 0.161 & 1.016 \\
19 & 5.725 & 1.013 \\
20 & -0.229 & 1.011 \\
21 & -3.468 & 1.010 \\
\end{array}\]

从以上数据看出\(\nu\_k\)收敛到\(\sigma^2=1\)的速度是较理想的(参见习题4.4).
由于\(Z\_k=X\_k-\hat X\_k\)服从正态分布\((0,\nu\_{k-1})\),
所以真值\(x\_t\)的置信度为0.95的置信下、上限分别是
\[
\hat X\_k - 1.96\sqrt{\nu\_{k-1}},
\ \ \hat X\_k + 1.96\sqrt{\nu\_{k-1}}.
\]

下面的R函数`ts.ipred.coef()`利用协方差阵\(\Gamma\_n\)（对非平稳列）或者自协方差函数列\(\{ \gamma\_k \}\)（对平稳列）求递推预测系数\(\{ \theta\_{nj} \}\)和方差\(\nu\_j\)，
`ts.ipred()`做递推预测。
这里还收录了计算ARMA模型Wold系数和理论自协方差函数的函数，
用Levinson递推求解预测系数的函数，
Levinson递推方法方法仅适用于平稳列。

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

### 非平稳时间序列新息递推预测的系数和一步预测误差方差计算。
### 输入：
###    Gam --- 如果序列非平稳，为自协方差矩阵n*n
###            如果序列平稳，为自协方差列\gamma_0, \gamma_1, \dots, \gamma_{n-1}
### 输出：
###   用新息预报方法对Y_1, Y_2, \dots, Y_n进行一步最佳线性预报的系数和均方误差。
###   theta -- (n-1)*(n-1)矩阵。第k行为预报Y_{k+1}的样本新息系数：
###      \hat Y_{k+1} = theta[k,1]*W_k + theta[k,2]*W_{k-1} + \dots + theta[k,k]*W_1
###      W_1=Y_1, W_k = Y_k - \hat Y_k, k=2,3,\dots,n
###   nu --- n个元素，nu[k]为预报Y_k的均方误差。
ts.ipred.coef <- function(Gam){
  if(!is.matrix(Gam)){
    n <- length(Gam)
    Gam <- outer(1:n, 1:n,
                 function(i,j) Gam[abs(i-j)+1])
  } else {
    n <- nrow(Gam)
  }
  ## X_1, X_2, \dots, X_{n-1}的自协方差矩阵为Gam

  ## 新息预报递推
  theta <- matrix(0, n-1, n-1)
  nu <- numeric(n)
  nu[1] <- Gam[1,1]
  theta[1,1] <- Gam[2,1] / Gam[1,1]
  nu[2] <- Gam[2,2] - theta[1,1]^2 * nu[1]
  for(k in 2:(n-1)){
    theta[k, k] <- Gam[k+1,0+1] / nu[1]
    for(j in 1:(k-1)){
      theta[k,k-j] <- (Gam[k+1,j+1]
                       - sum(theta[j, j:1]*theta[k,k:(k-j+1)]*nu[1:j])) /
                         nu[j+1]
    }
    nu[k+1] <- Gam[k+1,k+1] - sum(theta[k,k:1]^2 * nu[1:k])
  }
  list(theta=theta, nu=nu)
}

## 时间序列递推预报(新息预报)，假定已知自协方差列（平稳时）或自协方差阵（非平稳时）
## 输入:
##  x -- 时间序列。可以包含要预报的部分作为对比。
##  gams -- 向量或矩阵。向量时为自协方差列，矩阵时为自协方差阵
##  demean -- 是否减去均值再预报
##  end -- 递推预报是对每个时间点都进行预报，end是预报到那个时间点，
##         这受到gams大小的限制，只能预报到与自协方差阵阶数相同的时间点
## conf -- 计算预报区间的置信度
ts.ipred <- function(x, gams, demean=FALSE, end=length(x), conf=0.95){
  stationary <- !is.matrix(gams)
  res1 <- ts.ipred.coef(gams)
  theta <- res1$theta
  nu <- res1$nu

  if(demean) {xmean <- mean(x); x <- x - xmean}

  if(!stationary) end <- length(nu)
  W <- numeric(end)
  pred <- numeric(end)
  lb <- numeric(end)
  ub <- numeric(end)
  lam <- qnorm(1 - (1-conf)/2)

  pred[1] <- 0
  W[1] <- x[1]
  for(n in 1:(end-1)){
    pred[n+1] <- sum(theta[n, n:1]*W[1:n])
    W[n+1] <- x[n+1] - pred[n+1]
  }
  lb <- pred - lam*sqrt(nu[1:end])
  ub <- pred + lam*sqrt(nu[1:end])

  if(demean){
    pred <- xmean + pred
    lb <- xmean + lb
    ub <- xmean + ub
  }
  list(x=x, pred=pred, lb=lb, ub=ub, conf=conf)
}

### 对平稳列，已知自协方差列时用Levinson递推计算
### 逐个一步预报系数(Y-W系数)和一步预测误差方差
### 输入: gams[1:n] 为\gamma_k, k=0,1,\dots,n-1
### 输出：预报Y_1, Y_2, \dots, Y_{n}所需的系数和均方误差。
###   coef.YW -- (n-1)*(n-1)矩阵，第k行的1:k元素为
###         a_{k,1}, a_{k,2}, \dots, a_{k,k},
###         用来预报Y_{k+1}:
###         L(Y_{k+1} | Y_1, Y_2, \dots, Y_k)
###       = a_{k,1} Y_{k} + a_{k,2} Y_{k-1} + \dots + a_{k,k} Y_1
###       k=1,2,\dots,n-1
###       L(Y_1| ) = 0
###   sigmasq -- n向量，预报Y_1, Y_2, \dots, Y_n的一步预报均方误差
###       其1号元素是预报Y_1的均方误差，2号元素是预报Y_2的均方误差，
###       ..., n号元素是预报Y_n的均方误差。
Levinson.coef <- function(gams){
  n <- length(gams)

  ## ayw保存Y-W系数a[i,j], i=1,2,\dots,n-1, j=1,2,\dots,i
  ayw <- matrix(0, n-1, n-1)
  ## ss保存一步预报误差方差
  ss <- numeric(n)

  ss[1] <- gams[0+1] ## 预报Y_1的均方误差, 等于\gamma_0
  ayw[1,1] <- gams[1+1] / gams[0+1] ## 用Y_1预报Y_2的系数
  ss[2] <- ss[1] * (1 - ayw[1,1]^2) ## 用Y_1预报Y_2的均方误差
  if(n>2) for(k in 1:(n-2)){
    ## 用Y_1, \dots, Y_{k+1}预报Y_{k+2}的系数
    ayw[k+1,k+1] <- (gams[(k+1)+1] - sum(ayw[k,1:k] * gams[(k:1)+1])) / ss[k+1]
    ayw[k+1,1:k] <- ayw[k, 1:k] - ayw[k+1,k+1]*ayw[k, k:1]
    ## 用Y_1, \dots, Y_{k+1}预报Y_{k+2}的均方误差
    ss[k+2] <- ss[k+1] * (1 - ayw[k+1,k+1]^2)
  }

  list(coef.YW=ayw, sigmasq=ss)
}

### 平稳时间序列已知自协方差列，用Levinson递推计算Y-W系数并逐步预报。
### 输入：
###  x --- 序列X_1, \dots, X_n
###  gams --- 理论自协方差列\gamma_0, \gamma_1, \dots, \gamma_{m-1}, m <= n
###  max.lag --- 最多使用多少个历史值预报。缺省使用所有历史值，或理论自协方差列个数减一。
###  end --- 从时刻1预测到时刻end，缺省对每个时间点做一步预报。
###  demean --- 预报时是否中心化
###  conf --- 计算预测区间时的置信度。
### 输出：
###  x --- n个元素，原始序列
###  pred --- n个元素，每一步的预报值
###  lb, ub --- 对应的预测下限和上限。
sts.pred.levinson <- function(x, gams, max.lag=length(gams)-1,
                              end=length(x),
                              demean=FALSE, conf=0.95){
  n <- length(x)
  m <- length(gams)
  if(max.lag > m-1){
    msg <- paste('可用最大历史长度', max.lag,
                 '大于可用理论自协方差个数减一', m-1)
    stop(msg)
  }
  if(demean) {
    xmean <- mean(x)
    x[] <- x - xmean
  }
  
  pred <- numeric(end)
  lb <- numeric(end)
  ub <- numeric(end)
  lam <- qnorm(1 - (1-conf)/2)

  res1 <- Levinson.coef(gams)
  ayw <- res1$coef.YW ## (m-1)*(m-1)矩阵，第k行用于k个历史进行一步预报。
  ss <- res1$sigmasq  ## 长度为m的向量，第k个为用k-1个历史预报下一步的均方误差。
  
  pred[1] <- 0
  for(k in seq(end-1)){
    if(k <= max.lag) {
      pred[k+1] <- sum(ayw[k, 1:k] * x[k:1])
    } else {
      pred[k+1] <- sum(ayw[max.lag, 1:max.lag] * x[k:(k-max.lag+1)])
    }
  }
  if(demean) pred[] <- pred + xmean

  ss2 <- c(ss[1:min(end,max.lag+1)],
           rep(ss[max.lag+1], end-min(end,max.lag+1)))

  lb[] <- pred - lam*sqrt(ss2)
  ub[] <- pred + lam*sqrt(ss2)

  list(x=x, pred=pred, lb=lb, ub=ub, conf=conf)
}
```

下面的R函数生成ARMA(4,2)的长度为21的模拟数据，
但是用理论自协方差函数计算递推预测系数，
对最后的7个点作一步最佳线性预测，
计算预测区间，
并与直接用Levinson递推得到YW方程解的方法进行比较，结果一致。
注意这与§[22.1.12](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/atsa-blpprop.html#blpprop-blp-exa)的预测不太一样，
那里是基于前14个点对后7个点作多步预测。
一步预测误差一般低于多步预测误差。

```
## 从ARMA(4,2)的模拟样本用理论自协方差
## 和一般新息预报公式进行递推预报，
## 并与用Levinson递推公式解Y-W方程的一步预报对比。
demo.ipred.arma42 <- function(){
  a <- c(-0.9, -1.4, -0.7, -0.6)
  b <- c(0.5, -0.4)
  ng <- 21
  n <- 21
  gams <- arma.gamma(ng, a, b, sigma=1)
  
  x <- arima.sim(model=list(ar=a, ma=b), n=n)

  n.old <- 14  ## 不预报这些项
  m.pred <- 7  ## 对15--21进行一步预报

  ## 用理论自协方差函数计算递推预测系数
  res0 <- ts.ipred.coef(gams)
  ##cat("==== Iterative predition coefficients:\n")
  ##print(round(res0[["theta"]], 2))
  ##cat("==== Iterative predition MSEs:\n")
  ##print(round(res0[["nu"]], 2))
  ## 用理论自协方差函数计算YW方程解
  res0b <- Levinson.coef(gams)
  ##cat("==== Levinson YW solution predition coefficients:\n")
  ##print(round(res0b[["coef.YW"]], 2))
  ##cat("==== Levinson YW solution predition MSEs:\n")
  ##print(round(res0b[["sigmasq"]], 2))
  
  res1 <- ts.ipred(x, gams, demean=FALSE, end=n, conf=0.95)
  pred <- res1$pred
  lb <- res1$lb
  ub <- res1$ub

  res2 <- sts.pred.levinson(x=x, gams=gams, demean=FALSE, conf=0.95)

  cat("==== Iterative predition and Levinson YW predition:\n")
  print(round(cbind(x, pred, res2$pred, lb, res2$lb, ub, res2$ub), 4))

  yl <- range(c(x, pred, lb, ub))
  plot(1:n, x, type="n",
       main="Prediction of ARMA(4,2) By Innovation Method",
       xlab="t", ylab="y",
       ylim=yl)
  lines(1:n, x[1:n],
        type='b',
        col="black",
        lty=1, pch=2, 
        lwd=2)  ## 不预报的真实值
  lines((n.old+1):(n.old+m.pred), x[(n.old+1):(n.old+m.pred)],
        type='b',
        col="green",
        lty=1, pch=2,
        lwd=2) ## 预报部分的真实值
  lines((n.old+1):(n.old+m.pred), pred[(n.old+1):(n.old+m.pred)],
        type="b",
        col="red",
        lty=3, pch=3,
        lwd=2)  ## 预报值
  lines((n.old+1):(n.old+m.pred), lb[(n.old+1):(n.old+m.pred)],
        col="red", lty=3, lwd=1)  ## 预报下界
  lines((n.old+1):(n.old+m.pred), ub[(n.old+1):(n.old+m.pred)],
        col="red", lty=3, lwd=1)  ## 预报上界
  legend("top",
         col=c('green', 'red', 'red', 'red'),
         pch=c(2,3,NA,NA),
         lty=c(1,2,3,3),
         legend=c('True', 'Predict', 'LB', 'UB'))
  invisible()
}
set.seed(1)
demo.ipred.arma42()
```

```
## ==== Iterative predition and Levinson YW predition:
## Time Series:
## Start = 1 
## End = 21 
## Frequency = 1 
##          x    pred res2$pred      lb res2$lb      ub res2$ub
##  1  0.9736  0.0000    0.0000 -5.0622 -5.0622  5.0622  5.0622
##  2  3.3414 -0.2200   -0.2200 -5.1512 -5.1512  4.7111  4.7111
##  3 -4.1315 -2.0990   -2.0990 -5.2016 -5.2016  1.0037  1.0037
##  4 -3.9365 -0.7431   -0.7431 -3.7718 -3.7718  2.2855  2.2855
##  5  6.5983  6.1507    6.1507  3.9436  3.9436  8.3578  8.3578
##  6  1.0273  1.1262    1.1262 -1.0507 -1.0507  3.3032  3.3032
##  7 -2.8216 -5.1024   -5.1024 -7.1975 -7.1975 -3.0074 -3.0074
##  8  0.0599 -0.2545   -0.2545 -2.3236 -2.3236  1.8145  1.8145
##  9 -1.0723 -1.4527   -1.4527 -3.4955 -3.4955  0.5901  0.5901
## 10  2.6286  2.2890    2.2890  0.2620  0.2620  4.3160  4.3160
## 11 -0.2186  0.7958    0.7958 -1.2182 -1.2182  2.8099  2.8099
## 12 -2.9626 -3.3527   -3.3527 -5.3572 -5.3572 -1.3482 -1.3482
## 13  0.3625  2.3391    2.3391  0.3423  0.3423  4.3359  4.3359
## 14  2.8848  1.3270    1.3270 -0.6637 -0.6637  3.3178  3.3178
## 15  0.7091  0.5973    0.5973 -1.3885 -1.3885  2.5831  2.5831
## 16 -1.4901 -3.7039   -3.7039 -5.6857 -5.6857 -1.7221 -1.7221
## 17 -0.3880 -0.8703   -0.8703 -2.8487 -2.8487  1.1081  1.1081
## 18 -1.1331 -0.4252   -0.4252 -2.4008 -2.4008  1.5505  1.5505
## 19  2.2462  1.6478    1.6478 -0.3256 -0.3256  3.6211  3.6211
## 20  0.3857  1.3009    1.3009 -0.6705 -0.6705  3.2722  3.2722
## 21 -4.4308 -3.1497   -3.1497 -5.1194 -5.1194 -1.1800 -1.1800
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/5021-armaip_files/figure-html/armaip-ipred-arma42-1.png)

## 25.4 ARMA序列多步预测

设\(n>m\),
\[\begin{aligned}
& L(X\_{n+k+1} | \boldsymbol{X}\_n) \\
=& \begin{cases}
\sum\_{j=1}^p a\_j L(X\_{n+k+1-j} | \boldsymbol{X}\_n)
+ \sum\_{j=k+1}^q \theta\_{n+k,j} Z\_{n+k+1-j},
& 0 \leq k < q, n>m \\
\sum\_{j=1}^p a\_j L(X\_{n+k+1-j} | \boldsymbol{X}\_n),
& k \geq q, n>m.
\end{cases}
\end{aligned}\]

## 25.5 ARIMA序列预测

模型
\[
A(\mathscr B)(1-\mathscr B)^dX\_t=B(\mathscr B)\varepsilon\_t,
\ \ \varepsilon\_t \sim \text{WN}(0,\sigma^2), \ \ t\in N.
\]
则\(Y\_t=(1-\mathscr B)^d X\_t,\ t=d+1,d+2,\cdots,n\)
满足ARMA\((p,q)\)模型
\[
A(\mathscr B)Y\_t=B(\mathscr B)\varepsilon\_t, \ \ t \in N.
\]
考虑用\(X\_1,X\_2,\cdots,X\_n\)预测\(X\_{n+k}\)的问题.

首先利用ARMA序列的预测方法得到用\(Y\_{d+1},Y\_{d+2},\cdots,Y\_n\)预测
\[
Y\_{n+1},{Y}\_{n+2},\cdots,{Y}\_{n+k}
\]
时的最佳线性预测
\[
\tilde Y\_{n+j}=L(Y\_{n+j}|Y\_{d+1},Y\_{d+2},\cdots,Y\_{n}), \ j= 1,2,\cdots,k.
\]
再由公式
\[
(1-\mathscr B)^d \hat{X}\_t = \tilde {Y}\_t,\ t=n+1,n+2,\cdots,n+k
\]
得到近似的递推公式:
\[
\hat{X}\_{n+k}=\tilde {Y}\_{n+k}-\sum^{d}\_{j=1}C\_d^j(-1)^j\hat{X}\_{n+k-j}, \ k\geq 1,
\]
其中\(\hat{X}\_{n-j}=X\_{n-j}\), 当\(j\geq 0\).

## 25.6 附录：ARMA预报的补充

AR(\(p\))观测值为\(X\_1, X\_2, \dots, X\_n\)，假设已知理论参数，
进行逐步一步拟合和预报:
\[\begin{aligned}
\hat X\_t =& L(X\_t | X\_{t-1}, \dots, X\_1),
\ t=1,2,\dots,n \\
\hat X\_{n+k} =& L(X\_{t+k} | X\_1, X\_2, \dots, X\_n),
\ k=1,2,\dots
\end{aligned}\]

* 前\(p\)个用一般Levinson递推得到Y-W系数和方差；
* 后\(n-p\)个用AR系数预报;
* 从第\(n+1\)开始用递推预报。

### 25.6.1 思考：AR序列有限历史最佳线性预测多步预测的均方误差

多步预报的均方误差？
\[\begin{aligned}
& L(X\_{t+k} | X\_1, \dots, X\_n) \\
=& L(X\_{t+k} | X\_n, X\_{n-1}, \dots, X\_{n-p+1}
\end{aligned}\]
解方程
\[\begin{aligned}
\Gamma\_p \boldsymbol b = (\gamma\_k, \gamma\_{k+1}, \dots, \gamma\_{k+p-1})^T
\end{aligned}\]
得\(k\)步预报均方误差
\[\begin{aligned}
\sigma\_k^2 = \gamma\_0 - b\_1 \gamma\_{k} - b\_2 \gamma\_{k+1} - \dots - b\_p \gamma\_{k+p-1}
\end{aligned}\]
有没有简化公式？

### 25.6.2 MA模型的新息问题

MA(\(q\))序列满足自协方差函数\(q\)后截尾。
在零均值情况下，
\(X\_{n+1}\)与\(X\_1, X\_2, \dots, X\_{n-q}\)不相关，
即正交。
那么，
\(L(X\_{n+1} | X\_n, \dots, X\_1)\)能否表示为\(X\_{n}, X\_{n-1}, X\_{n-q+1}\)的线性组合？
如果这样，
MA的新息估计法就是不必要的。

以MA(1)为例。
模型为
\[
X\_t = \varepsilon\_t + b \varepsilon\_{t-1}
\]
其中\(|b|<1\), \(\{ \varepsilon\_t \}\)是WN(0, \(\sigma^2\))。

令
\[
\hat\varepsilon\_n = X\_n - L(X\_n | X\_1, \dots, X\_{n-1}),
\ n=1,2,\dots
\]
则\(\{ \hat\varepsilon\_t \}\)是正交随机变量列，
\(L(X\_1, \dots, X\_n) = L(\hat\varepsilon\_1, \dots, \hat\varepsilon\_n)\)。
于是由投影性质，
\[\begin{aligned}
\hat X\_{n+1}
=& L(X\_{n+1} | X\_1, \dots, X\_n)
= L(X\_{n+1} | \hat\varepsilon\_1, \dots, \hat\varepsilon\_n) \\
=& \sum\_{t=1}^n L(X\_{n+1} | \hat\varepsilon\_t)
\end{aligned}\]
但是，
因为对\(t \leq n-1\)，
\(X\_{n+1}\)与\(X\_t\)正交（不相关），
所以\(X\_{n+1}\)与\(L(X\_1, \dots, X\_{n-1})\)正交，
\(X\_{n+1}\)与\(L(\hat\varepsilon\_1, \dots, \hat\varepsilon\_{n-1})\)正交，
由投影性质可知\(t \leq n-1\)时\(L(X\_{n+1} | \hat\varepsilon\_t) = 0\)，
于是
\[
\hat X\_{n+1}
= L(X\_{n+1} | \hat\varepsilon\_n)
= \theta\_{n,n} \hat\varepsilon\_n
\]
其中\(\theta\_{n,j}\)是新息递推预报系数。

对MA(1)序列,
\(\gamma\_0 = \sigma^2(1 + b^2)\),
\(\gamma\_1 = \sigma^2 (2b)\)。
如果直接计算\(L(X\_{n+1} | X\_1, \dots, X\_n)\)，
需要求解线性方程组
\[
\left(\begin{array}{\*9c}
1+b^2 & 2b & 0 & \cdots & 0 & 0 & 0 \\
2b & 1+b^2 & 2b & \cdots & 0 & 0 & 0 \\
\vdots & \vdots & \vdots & & \vdots & \vdots & \vdots \\
0 & 0 & 0 & \cdots & 2b & 1+b^2 & 2b \\
0 & 0 & 0 & \cdots & 0 & 2b & 1+b^2
\end{array}\right)
\boldsymbol a\_n
=
\left(\begin{array}{\*9c}
2b \\ 0 \\ \vdots \\ 0 \\ 0
\end{array}\right)
\]
其中\(\boldsymbol a\_n = (a\_{n1}, \dots, a\_{nn})^T\)，
\(L(X\_{n+1} | X\_1, \dots, X\_n) = a\_{n1} X\_n + \dots + a\_{nn} X\_1\)。
\(\boldsymbol a\_n\)并不是只有\(a\_{n1}\)一个非零元素。

思考题：利用Levinson递推公式求出\(\boldsymbol a\_n\)的表达式。

下面用R程序对一些特殊\(b\)值求解\(\boldsymbol a\_n\)。

```
Levinson.coef <- function(gams){
  n <- length(gams)

  ## ayw保存Y-W系数a[i,j], i=1,2,\dots,n-1, j=1,2,\dots,i
  ayw <- matrix(0, n-1, n-1)
  ## ss保存一步预报误差方差
  ss <- numeric(n)

  ss[1] <- gams[0+1] ## 预报Y_1的均方误差, 等于\gamma_0
  ayw[1,1] <- gams[1+1] / gams[0+1] ## 用Y_1预报Y_2的系数
  ss[2] <- ss[1] * (1 - ayw[1,1]^2) ## 用Y_1预报Y_2的均方误差
  if(n>2) for(k in 1:(n-2)){
    ## 用Y_1, \dots, Y_{k+1}预报Y_{k+2}的系数
    ayw[k+1,k+1] <- (gams[(k+1)+1] - sum(ayw[k,1:k] * gams[(k:1)+1])) / ss[k+1]
    ayw[k+1,1:k] <- ayw[k, 1:k] - ayw[k+1,k+1]*ayw[k, k:1]
    ## 用Y_1, \dots, Y_{k+1}预报Y_{k+2}的均方误差
    ss[k+2] <- ss[k+1] * (1 - ayw[k+1,k+1]^2)
  }

  list(coef.YW=ayw, sigmasq=ss)
}

demo.ma1.yw <- function(b=0.5, maxn=10){
  gams <- c(1+b^2, 2*b, rep(0.0, maxn-1))
  res <- Levinson.coef(gams)
  print(round(res$coef.YW, 2))
}
demo.ma1.yw(b=0.5, maxn=10)
```

```
##        [,1]   [,2]  [,3]  [,4]   [,5]  [,6] [,7]  [,8]  [,9] [,10]
##  [1,]  0.80   0.00  0.00  0.00   0.00  0.00 0.00  0.00  0.00  0.00
##  [2,]  2.22  -1.78  0.00  0.00   0.00  0.00 0.00  0.00  0.00  0.00
##  [3,] -1.03   2.29 -1.83  0.00   0.00  0.00 0.00  0.00  0.00  0.00
##  [4,]  0.44   0.45 -1.00  0.80   0.00  0.00 0.00  0.00  0.00  0.00
##  [5,]  1.23  -0.54 -0.56  1.24  -0.99  0.00 0.00  0.00  0.00  0.00
##  [6,] 58.31 -71.89 31.55 32.45 -72.11 57.69 0.00  0.00  0.00  0.00
##  [7,] -0.02   1.02 -1.26  0.55   0.57 -1.26 1.01  0.00  0.00  0.00
##  [8,]  0.79   0.01 -0.81  0.99  -0.44 -0.45 1.00 -0.80  0.00  0.00
##  [9,]  2.17  -1.71 -0.03  1.75  -2.16  0.95 0.97 -2.16  1.73  0.00
## [10,] -1.09   2.36 -1.86 -0.03   1.90 -2.35 1.03  1.06 -2.35  1.88
```

输出结果是一个矩阵，
第\(n\)行是\(a\_{n,j}, j=1,2,\dots,n\)以及补充的0。
可以看出并不是只有\(a\_{n1}\)非零。