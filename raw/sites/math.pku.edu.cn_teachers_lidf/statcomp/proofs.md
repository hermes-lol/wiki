---
crawl_time: '2026-01-17 14:25:18'
framework: sphinx
title: D 理论证明补充(*) | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/proofs.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# D 理论证明补充(`*`)

## D.1 对立变量的补充证明

**定理D.1** 设\(f(x\_1, \dots, x\_n)\)和\(g(x\_1, \dots, x\_n)\)是关于每个自变量单调不减的函数，
随机变量\(X\_1, \dots, X\_n\)相互独立，
记\(\boldsymbol X = (X\_1, \dots, X\_n)\)，
则下面的不等式当其中期望存在有限时成立：
\[\begin{align}
E[f(\boldsymbol X) g(\boldsymbol X)]
\geq E[f(\boldsymbol X)]\, E[g(\boldsymbol X)] .
\tag{D.1}
\end{align}\]

**证明**：
用数学归纳法。
当\(n=1\)时，对任意实数\(x, y\)有
\[
[f(x) - f(y)][g(x) - g(y)] \geq 0 .
\]
于是对任意随机变量\(X, Y\)有
\[
[f(X) - f(Y)][g(X) - g(Y)] \geq 0 .
\]
于是
\[
E \left\{ [f(X) - f(Y)][g(X) - g(Y)] \right\} \geq 0 .
\]
当期望存在有限时有
\[
E[f(X) g(X)] + E[f(Y) g(Y)]
\geq E[f(X) g(Y)] + E[f(Y) g(X)] .
\]
设\(X, Y\)独立同分布，且涉及的期望存在有限，
则
\[\begin{aligned}
E[f(X) g(X)] =& E[f(Y) g(Y)], \\
E[f(X) g(Y)] =& E[f(Y) g(X)] = E[f(X)] E[g(X)],
\end{aligned}\]
从而有
\[\begin{aligned}
E[f(X) g(X)] \geq E[f(X)] E[g(X)] .
\end{aligned}\]

设定理结论对\(n-1\)个自变量的情形成立,
设\(f(x\_1, \dots, x\_n)\)和\(g(x\_1, \dots, x\_n)\)是关于每个自变量单调不减的函数，
则
\[\begin{aligned}
& E[f(\boldsymbol X) g(\boldsymbol X) | X\_n = x] \\
=& E[f(X\_1, \dots, X\_{n-1}, x) g(X\_1, \dots, X\_{n-1}, x) | X\_n = x ] \\
=& E[f(X\_1, \dots, X\_{n-1}, x) g(X\_1, \dots, X\_{n-1}, x)] \quad(\text{由}X\_n\text{与}X\_1,\dots, X\_n\text{的独立性}) \\
\geq& E[f(X\_1, \dots, X\_{n-1}, x)] E[g(X\_1, \dots, X\_{n-1}, x)] \\
=& E[f(X\_1, \dots, X\_{n-1}, x) | X\_n = x] E[g(X\_1, \dots, X\_{n-1}, x) | X\_n = x]
\end{aligned}\]
所以有
\[\begin{aligned}
E[f(\boldsymbol X) g(\boldsymbol X) | X\_n]
\geq E[f(\boldsymbol X) | X\_n] E[g(\boldsymbol X) | X\_n] .
\end{aligned}\]

两边取期望得
\[\begin{aligned}
E[f(\boldsymbol X) g(\boldsymbol X)]
\geq E\left\{ E[f(\boldsymbol X) | X\_n] E[g(\boldsymbol X) | X\_n] \right\} .
\end{aligned}\]
注意到\(E[f(\boldsymbol X) | X\_n]\)和\(E[g(\boldsymbol X) | X\_n]\)分别是关于\(X\_n\)的一元函数，
且关于\(X\_n\)是增函数，
由\(n=1\)时已证明的结论可知
\[\begin{aligned}
& E\left\{ E[f(\boldsymbol X) | X\_n] E[g(\boldsymbol X) | X\_n] \right\} \\
\geq& E\left\{ E[f(\boldsymbol X) | X\_n] \right\} E\left\{ E[g(\boldsymbol X) | X\_n] \right\} \\
=& E[f(\boldsymbol X)] E[g(\boldsymbol X)] .
\end{aligned}\]
证毕。

※※※※※

**定理D.2** 设\(h(x\_1, \dots, x\_n)\)是关于每个自变量分别单调的函数，
\(U\_1, \dots, U\_n\)独立同U(0,1)分布，
则当\(h(U\_1, \dots, U\_n)\)二阶矩有限时
\[\begin{aligned}
\text{Cov}\left[
h(U\_1, \dots, U\_n),
h(1 - U\_1, \dots, 1 - U\_n)
\right] \leq 0.
\end{aligned}\]

**证明**：
因为自变量次序调整不影响结论，
所以不妨设\(h\)关于前\(r\)个分量分别是单调增的，
关于后\(n-r\)个分量分别是单调减的。
令
\[\begin{aligned}
f(x\_1, \dots, x\_n)
=& h(x\_1, \dots, x\_r, 1 - x\_{r+1}, \dots, 1 - x\_n), \\
g(x\_1, \dots, x\_n)
=& -h(1 - x\_1, \dots, 1 - x\_r, x\_{r+1}, \dots, x\_n),
\end{aligned}\]
则\(f(x\_1, \dots, x\_n)\)和\(g(x\_1, \dots, x\_n)\)是关于每个自变量单调不减的函数，
设\(V\_1, \dots, V\_n\)为独立同U(0,1)分布随机变量，
由定理[D.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/proofs.html#thm:prf-antith-base)可知
\[\begin{align}
& \text{Cov}[f(V\_1, \dots, V\_n), g(V\_1, \dots, V\_n)] \\
=& - \text{Cov}[h(V\_1, \dots, V\_r, 1 - V\_{r+1}, \dots, 1 - V\_n), \\
& \qquad \quad h(1 - V\_1, \dots, 1 - V\_r, V\_{r+1}, \dots, V\_n) ] \geq 0 ,
\tag{D.2}
\end{align}\]
设\(U\_1, \dots, U\_n\)独立同U(0,1)分布，
令\((V\_1, \dots, V\_n) = (U\_1, \dots, U\_r, 1 - U\_{r+1}, \dots, 1 - U\_n)\)，
则\(V\_1, \dots, V\_n\)独立同U(0,1)分布，从而由[(D.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/proofs.html#eq:prf-antith-main-pr01)式可知
\[\begin{aligned}
& \text{Cov}[h(U\_1, \dots, U\_r, U\_{r+1}, \dots, U\_n, \\
& \qquad \quad h(1-U\_1, \dots, 1-U\_r, 1-U\_{r+1}, \dots, 1 - U\_n) ]
\leq 0 .
\end{aligned}\]
证毕。

※※※※※