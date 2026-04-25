---
crawl_time: '2026-01-17 14:28:55'
framework: sphinx
title: 4 严平稳序列及其遍历性 | 金融时间序列分析备课笔记
url: https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/strict-stationary.html
---

# [金融时间序列分析备课笔记](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/)

# 4 严平稳序列及其遍历性

## 4.1 严平稳

随机向量同分布是指其联合分布函数相同。

时间序列\(\{X\_t\}\)与\(\{Y\_t\}\)同分布，
当且仅当\(\forall n \in \mathbb N\_+\)和
\(t\_1, \ldots, t\_n \in \mathbb Z\),
\((X(t\_1), \ldots, X(t\_n))^T\)与\((Y(t\_1), \ldots, Y(t\_n))^T\)同分布。

**定义4.1 (严平稳列)** 时间序列\(\{X\_t\}\)对\(\forall n \in \mathbb N\_+\)和\(k \in \mathbb Z\)都有
\[
(X\_1, X\_2, \ldots, X\_n)^T \text{ 和 }
(X\_{1+k}, X\_{2+k}, \ldots, X\_{n+k})^T \text{ 同分布}.
\]
即分布平移不变，称\(\{X\_t \}\)为**严平稳**时间序列。

若\(\{X\_t \}\)严平稳，
对任多元函数 \(\phi(x\_1, x\_2, \ldots, x\_m)\) 令
\[\begin{aligned}
\{ Y\_t = \phi(X\_{t+1}, \ldots, X\_{t+m}), \ t \in \mathbb Z \}
\end{aligned}\]
则\(\{ Y\_t \}\)仍是严平稳列。

严平稳与宽平稳关系：

* 二阶矩有限的严平稳为宽平稳。
* 宽平稳一般不是严平稳。
* 正态平稳列既是宽平稳也是严平稳。
* 平稳序列\(=\)宽平稳序列\(=\)弱平稳序列。
* 严平稳序列\(=\)强平稳序列。

## 4.2 遍历性

时间序列一般只有一条轨道。
要用时间序列\(\{X\_t\}\)的一次实现 \(x\_1,x\_2,...x\_T\)推断\(\{X\_t(\omega), t\in\mathbb N, \omega\in\Omega \}\)的统计性质.
遍历性可以保证从一条轨道可以推断整体的统计性质。

如果严平稳序列是遍历的,
从它的一次实现 \(x\_1,x\_2,\dots, x\_T\) 就可以推断出这个严平稳序列的所有有限维分布:
\[\begin{aligned}
& F(x\_1,x\_2,...,x\_m) \\
=& P(X\_1\leq x\_1, X\_2\leq x\_2,...,X\_m \leq x\_m), \ \ m\in \mathbb N .
\end{aligned}\]
有遍历性的严平稳序列被称作**严平稳遍历序列**.

严平稳遍历的严格定义依赖于用测度论叙述的保测变换、不变集、不变随机变量概念，
详见王梓坤《随机过程通论》第197–204页（北京师范大学出版社，1996）。

**定理4.1 (遍历定理)** 如果 \(\{X\_t\}\)是严平稳遍历序列, 则有如下的结果:

1. 强大数律: 如果 \(E|X\_1|<\infty\) 则
   \[
   \lim\_{n\to \infty}\frac1n\sum\_{t=1}^n X\_t= EX\_1, \text{ a.s.}
   \]
2. 对任何多元函数 \(\phi(x\_1,x\_2,\cdots,x\_m)\),
   \[
   Y\_t=\phi(X\_{t+1},X\_{t+2},\cdots,X\_{t+m})
   \]
   是严平稳遍历序列.

**定理4.2** 如果\(\{\varepsilon\_t\}\)是独立同分布的\(\text{WN}(0,\sigma^2)\),
实数列\(\{a\_j\}\)平方可和,
则线性平稳序列
\[
X\_t=\sum\_{j=-\infty}^\infty a\_j\varepsilon\_{t-j}, \ \ t\in \mathbb Z,
\]
是严平稳遍历的。

这说明在独立同分布白噪声条件下线性平稳列满足严平稳遍历条件。
所以，
在许多教材中线性平稳列都假定独立同分布白噪声条件。

**例4.1** 对严平稳序列\(\{X\_t\}\),
设一条轨道为\(X\_1, \dots, X\_T\)，
给出有限维分布的强相合估计。

**解答**：
定义严平稳序列
\[\begin{aligned}
Y\_t =& I[X(t+t\_1)\leq y\_1, X(t+t\_2)\leq y\_2,\cdots,X(t+t\_m)\leq y\_m], \\
& t\in \mathbb Z.
\end{aligned}\]
这里\(I[A]\)是事件\(A\)的示性函数.
因为\(\{X\_t\}\)是遍历的,
由定理[4.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/strict-stationary.html#thm:strict-erg)的第2条知道\(\{Y\_t\}\)也是遍历的, 并且有界.
利用定理[4.1](https://www.math.pku.edu.cn/teachers/lidf/course/atsa/atsanotes/html/_atsanotes/strict-stationary.html#thm:strict-erg)的第1条(强大数律)得到
\[\begin{aligned}
&\lim\_{n\to \infty}\frac{1}{n}\sum\_{t=1}^n Y\_t = EY\_1 \\
=&P(X(t\_1)\leq y\_1, X(t\_2)\leq y\_2,\cdots,X(t\_m)\leq y\_m),
\text{ a.s.}
\end{aligned}\]

○○○○○○

这个例子说明, 在几乎必然的意义下,
严平稳遍历序列\(\{X\_t\}\)的每一次观测在观测长度趋于无穷时都可以决定\(\{X\_t\}\)的有限维分布.

**例4.2** 对严平稳序列\(\{X\_t\}\), 设其二阶矩有限，
设一条轨道为\(X\_1, \dots, X\_T\)，
给出相关函数\(E(X\_t X\_{t+k})\)的强相合估计。

**解答**：
这时\(Y\_t = X\_t X\_{t+k}\)也是严平稳遍历序列。
于是
\[
\lim\_{n\to\infty} \frac{1}{n} \sum\_{i=1}^n Y\_t
\to E(Y\_1) = E(X\_t X\_{t+k}) ,
\text{ a.s.}
\]

由此可知
\[
\lim\_{n\to\infty} \frac{1}{n} \sum\_{i=1}^{n-k} (X\_t - \bar X\_n)(X\_{t+k} - \bar X\_n)
\to \gamma\_k,
\text{ a.s.}
\]

○○○○○○

## 4.3 附录：随机过程知识

随机过程的分布由其所有有限维分布决定，
有限维分布族对次序交换保持一致，
对取边缘分布保持一致，
称为Kolmogorov相容性条件。

**定理4.3 (存在性定理)** 给定足标集和满足Kolmogorov相容性条件的有限维分布函数族，
必存在相应分布族的随机过程。

构造不唯一。
见王梓坤《随机过程论》。

**定理4.4 (正态过程存在性定理)** 设\(T\)为足标集，
\(a\_t\)为实值函数，
\(\sigma\_{s,t}\)为二元实值函数，对称，非负定，
则必存在正态过程\(\{\xi\_t, t \in T\}\)使其均值函数为\(a\_t\)，
自协方差函数为\(\sigma\_{s,t}\)。

见([谢衷洁 1990](#ref-Xie1990:tsabook))P5。

**复值正态分布**: 实部和虚部为联合正态分布。

### References

谢衷洁. 1990. *时间序列分析*. 北京大学出版社.