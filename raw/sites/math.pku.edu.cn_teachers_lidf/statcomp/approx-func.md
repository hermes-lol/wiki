---
crawl_time: '2026-01-17 14:25:06'
framework: sphinx
title: 21 函数逼近 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 21 函数逼近

在统计计算和其它科学计算中，
经常需要计算各种函数的值，
对函数进行逼近，
用数值方法计算积分、微分。

## 21.1 多项式逼近

数学中的超越函数如\(e^x\)、\(\ln x\)、\(\sin x\)在计算机中经常用泰勒级数展开来计算，
这就是用多项式来逼近函数。
多项式的高效算法见例[2.11](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/intro-error.html#exm:int-poly-qjs)。
数学分析中的Weirstrass定理表明，
闭区间上的连续函数可以用多项式一致逼近。
泰勒展开要求函数有多阶导数，
我们需要找到对更一般函数做多项式逼近的方法。

考虑如下的函数空间
\[\begin{aligned}
L^2[a,b] = \{ g(\cdot): g(x)\text{ 定义域为 }[a,b],
\text{ 且 }\int\_a^b g^2(x) w(x) dx < \infty \}
\end{aligned}\]
则\(L^2[a,b]\)是线性空间，在\(L^2[a,b]\)中定义内积
\[\begin{align}
<f, g> = \int\_a^b f(x) g(x) w(x) dx,
\tag{21.1}
\end{align}\]
其中\(w(x)\)是适当的权重函数，
则\(L^2[a,b]\)为Hilbert空间。
对\(g(x) \in L^2[a,b]\),
假设希望用\(n\)阶多项式\(f\_n(x)\)逼近\(g(x)\)，使得
\[\begin{align}
\| f\_n - g \|^2 = \int\_a^b | f\_n(x) - g(x) |^2 w(x) dx
\tag{21.2}
\end{align}\]
最小。
如何求这样的多项式？

用Gram-Schmidt正交化方法可以在\(L^2[a,b]\)中把多项式序列
\(\{1, x, x^2, \dots \}\)正交化为正交序列
\(\{ P\_0, P\_1, P\_2, \dots \}\)，
序列中函数彼此正交，且\(P\_k\)是\(k\)阶多项式,
称\(\{ P\_0, P\_1, P\_2, \dots \}\)为正交多项式。
设\(H\_n[a,b]\)为函数\(\{1, x, x^2, \dots, x^n \}\)的线性组合构成的线性空间，
则\(\{ P\_0, P\_1, \dots, P\_n \}\)构成\(H\_n[a,b]\)的正交基且
\(P\_n[a,b]\)是\(L^2[a,b]\)的子Hilbert空间，
使得加权平方距离[(21.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#eq:fun-dist-l2)最小的\(f\_n(x)\)是\(g(\cdot)\)在子空间\(H\_n[a,b]\)的投影，
记为\(\mathop{\mathrm{Proj}}\_{H\_n[a,b]}(g)\),
投影可以表示为\(\{ P\_0, P\_1, \dots, P\_n \}\)的线性组合
\[\begin{align}
\mathop{\mathrm{Proj}}\_{H\_n[a,b]}(g) = \sum\_{j=0}^n \frac{<g, P\_j>}{\| P\_j \|^2} P\_j.
\tag{21.3}
\end{align}\]

这样，只要预先找到\([a,b]\)上的多项式的正交基，
通过计算内积就可以很容易地找到使得[(21.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#eq:fun-dist-l2)最小的\(f\_n(x)\)。
对于\(L^2[a,b]\)中的任意函数\(g(x)\)有
\[\begin{aligned}
\lim\_{n\to\infty} \| \mathop{\mathrm{Proj}}\_{H\_n[a,b]}(g) - g \|^2 = 0,
\end{aligned}\]
于是有
\[\begin{aligned}
g = \lim\_{n\to\infty} \mathop{\mathrm{Proj}}\_{H\_n[a,b]}(g)
= \sum\_{j=0}^\infty \frac{<g, P\_j>}{\| P\_j \|^2} P\_j.
\end{aligned}\]

因为\(L^2[a,b]\)依赖于定义域\([a,b]\)和权重函数\(w(\cdot)\)，
所以正交多项式也依赖于\([a,b]\)和\(w(\cdot)\)。
针对定义域\([-1,1]\), \([0, \infty)\)和\((-\infty, \infty)\)
和几种不同的权重函数可以得到不同的正交多项式序列，
表[21.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#tab:orthpoly)列出了这些正交多项式的定义域、权重函数和名称。

表21.1:  正交多项式






| 函数空间 | 权函数 | 正交多项式 | 记号 |
| --- | --- | --- | --- |
| \(L^2[-1,1]\) | 1 | Legendre | \(P\_n(x)\) |
| \(L^2[-1,1]\) | \((1-x^2)^{-1/2}\)(边界加重) | Chebyshev I型 | \(T\_n(x)\) |
| \(L^2[-1,1]\) | \((1-x^2)^{1/2}\)(中心加重) | Chebyshev II型 | \(U\_n(x)\) |
| \(L^2[0,\infty)\) | \(\exp(-x)\) | Laguerre | \(L\_n(x)\) |
| \(L^2(-\infty, \infty)\) | \(\exp(-x^2)\) | Hermite | \(H\_n(x)\) |
| \(L^2(-\infty, \infty)\) | \(\exp(-x^2/2)\) | 修正Hermite | \(He\_n(x)\) |

Legendre多项式是权重函数\(w(x)=1\)的\(L^2[-1,1]\)中的正交多项式，
此权重函数在整个区间是等权重的，
内积为
\[\begin{aligned}
<f,g> = \int\_{-1}^{1} f(x)g(x) \, dx.
\end{aligned}\]
有一般公式
\[\begin{aligned}
P\_n(x) = \frac{1}{2^n n!} \frac{d^n}{dx^n} (x^2 - 1)^n ,
\end{aligned}\]
整个序列可以递推计算如下：
\[\begin{aligned}
P\_0(x) =& 1, \quad P\_1(x) = x, \quad P\_2(x) = (3x^2 - 1)/2, \\
P\_3(x) =& (5x^3 - 3x)/2, \quad P\_4(x) = (35x^4 -30x^2 + 3)/8, \\
(n+1)P\_{n+1}(x) =& (2n+1)x P\_n(x) - nP\_{n-1}(x),
\end{aligned}\]
且\(\| P\_n \|^2 = 2/(2n+1)\)。

Chebyshev I型多项式是权重函数为\(w(x) = (1-x^2)^{-1/2}\)的\(L^2[-1,1]\)中的正交多项式，
此权重函数强调两端，
内积为
\[\begin{aligned}
<f,g> = \int\_{-1}^{1} f(x)g(x) (1-x^2)^{-1/2} \, dx,
\end{aligned}\]
\(T\_n(x)\)有一般表达式\(\cos(n \cos^{-1}(x))\),
整个序列可以递推计算如下：
\[\begin{aligned}
T\_0(x) =& 1, \quad T\_1(x) = x, \quad T\_2(x) = 2x^2 - 1, \\
T\_3(x) =& 4x^3 - 3x, \quad T\_4(x) = 8x^4 - 8x^2 + 1, \\
T\_{n+1}(x) =& 2xT\_n(x) - T\_{n-1}(x)
\end{aligned}\]
且\(\| T\_0 \|^2 = \pi\), \(\| T\_n \|^2 = \frac{\pi}{2}\)(\(n>0\))。

Chebyshev II型多项式是权重函数为\(w(x) = (1-x^2)^{1/2}\)的\(L^2[-1,1]\)中的正交多项式，
此权重函数强调区间中间，
内积为
\[\begin{aligned}
<f,g> = \int\_{-1}^{1} f(x)g(x) (1-x^2)^{1/2} \, dx,
\end{aligned}\]
整个序列可以递推计算如下：
\[\begin{aligned}
U\_0(x) =& 1, \quad U\_1(x) = 2x, \quad U\_2(x) = 4x^2 - 1, \\
U\_3(x) =& 8x^3 - 4x, \quad U\_4(x) = 16x^4 - 12x^2 + 1, \\
U\_{n+1}(x) =& 2xU\_n(x) - U\_{n-1}(x),
\end{aligned}\]
且\(\| U\_n \|^2 = \pi/2\)。

Laguerre多项式是权重函数为\(w(x) = e^{-x}\)的\(L^2[0, \infty)\)中的正交多项式，
此权重函数强调区间左边，
内积为
\[\begin{aligned}
<f,g> = \int\_{0}^{\infty} f(x)g(x) e^{-x} \, dx,
\end{aligned}\]
整个序列可以递推计算如下：
\[\begin{aligned}
L\_0(x) =& 1, \quad L\_1(x) = 1-x, \quad L\_2(x) = (2 - 4x + x^2)/2, \\
L\_3(x) =& (6 - 18x + 9x^2 - x^3)/6, \\
L\_4(x) =& (24 - 96x + 72x^2 - 16x^3 + x^4)/24, \\
(n+1) L\_{n+1}(x) =& (2n+1-x)L\_n(x) - nL\_{n-1}(x),
\end{aligned}\]
且\(\| L\_n \|^2 = 1\)。

Hermite多项式是权重函数为\(w(x) = e^{-x^2}\)的\(L^2(-\infty, \infty)\)中的正交多项式，
此权重函数强调区间中央，
内积为
\[\begin{aligned}
<f,g> = \int\_{-\infty}^{\infty} f(x)g(x) e^{-x^2} \, dx,
\end{aligned}\]
整个序列可以递推计算如下：
\[\begin{aligned}
H\_0(x) =& 1, \quad H\_1(x) = 2x, \quad H\_2(x) = 4x^2 - 2, \\
H\_3(x) =& 8x^3 - 12x, \quad H\_4(x) = 16x^4 - 48x^2 + 12, \\
H\_{n+1}(x) =& 2x H\_n(x) - 2n H\_{n-1}(x),
\end{aligned}\]
且\(\| H\_n \|^2 = 2^n n! \pi^{1/2}\)。

修正Hermite 多项式与Hermite 多项式的区别仅仅是权重函数由\(e^{-x^2}\)改成了
\(w(x) = e^{-x^2/2}\)，内积为
\[\begin{aligned}
<f,g> = \int\_{-\infty}^{\infty} f(x)g(x) e^{-x^2/2} \, dx,
\end{aligned}\]
整个序列可以递推计算如下：
\[\begin{aligned}
He\_0(x) =& 1, \quad He\_1(x) = x, \quad He\_2(x) = x^2 - 1, \\
He\_3(x) =& x^3 - 3x, \quad He\_4(x) = x^4 - 6x^2 + 3, \\
He\_{n+1}(x) =& x He\_n(x) - n He\_{n-1}(x),
\end{aligned}\]
且\(\| He\_n \|^2 = n! \sqrt{2\pi}\)。

**例21.1** 用Julia程序生成正交多项式。

```
using Polynomials
function legendre(i)
  n = i-1
  p = Polynomial([-1,0,1])^n
  for i in 1 : n
    p = derivative(p)
  end
  return p / (2^n * factorial(n))
end

function laguerre(i)
  p = Polynomial([1])
  for j in 2 : i
    p = integrate(derivative(p) - p) + 1
  end
  return p
end

function hermite(i)
  p = Polynomial([1])
  x = Polynomial([0,1])
  for j in 2 : i
    p = x*p - derivative(p)
  end
  return p
end
```

统计计算中常常会遇到没有解析表达式的函数的计算问题，
比如，标准正态分布函数
\(\Phi(x) = \int\_{-\infty}^x \phi(u) du = \int\_{-\infty}^x \frac{1}{\sqrt{2\pi}} e^{-\frac12 u^2} du\)，
我们可以用数值积分方法把\(\Phi(x)\)计算到很高精度，
但是这样计算会极为耗时。
为此，
我们可以用Hermite多项式或修正Hermite多项式逼近\(\Phi(x)\)到一定的精度，
计算多项式系数时可以用数值积分计算，
这些系数计算可能耗时很多但是只要找到了逼近多项式就可以一劳永逸了。
当然，经过多年研究，
常见分布函数都已经有了很好的近似公式，比如
\[\begin{align}
\Phi(x) =& \begin{cases}
\frac12 + \phi(x) \sum\_{n=0}^\infty \frac{1}{(2n+1)!!} x^{2n+1}, & 0 \leq x \leq 3, \\
1 - \phi(x) \sum\_{n=0}^\infty (-1)^n \frac{(2n-1)!!}{x^{2n+1}}, & x>3
\end{cases}
\tag{21.4}
\end{align}\]
但是，在各种各样的统计模型中我们还是会遇到很多需要快速计算的函数，
这些函数很可能没有高阶导数，每计算一次花费很长时间，
我们可以用多项式逼近或插值方法给出近似公式，
然后每次用近似公式计算函数值。

有些统计方法需要估计一个函数，
这时可以用正交多项式级数表示这个函数，
只要估计级数展开所需的系数。

## 21.2 有理函数与连分式逼近

用正交多项式近似函数的好处是公式和计算简单，
容易计算展开系数，容易微分、积分。
但是多项式仅能逼近有限区间上的函数，
容易出现很强的波动，
尤其是在区间边界处,
而且多项式没有渐近线，没有无穷函数值的点，
很多函数，尤其是取值区间包含正负无穷的函数，
用正交多项式很难得到高精度。

有理函数能克服多项式的这些缺点。
分子和分母都是多项式的分式叫做有理函数。
逼近函数时，
在相同参数个数条件下有理函数经常比多项式逼近好。
有理函数可以从幂级数展开表示中解出。
有理函数在计算时可以化为连分式来计算，
连分式可以高效地计算。

连分式是像
\[\begin{aligned}
1 + \cfrac{1}{2 + \cfrac{1}{3 + \cfrac{1}{4}}} =
1 + \dfrac{1}{2} \genfrac{}{}{0pt}{}{}{+} \dfrac{1}{3} \genfrac{}{}{0pt}{}{}{+} \dfrac{1}{4}
\end{aligned}\]
这样的分式，等号右侧是对左侧的分式的简写。
一般地有
\[\begin{align}
R\_n =& b\_0 + \dfrac{a\_1}{b\_1 + \dfrac{a\_2}{b\_2 +
\genfrac{}{}{0pt}{}{}{\ddots +\dfrac{a\_n}{b\_n}}}}
= b\_0 + \frac{a\_1}{b\_1} \genfrac{}{}{0pt}{}{}{+} \frac{a\_2}{b\_2}
\genfrac{}{}{0pt}{}{}{+ \cdots +} \frac{a\_n}{b\_n}.
\tag{21.5}
\end{align}\]

计算连分式[(21.5)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#eq:fun-cfrac),
可以使用如下反向算法或正向算法，
正向算法适用于无穷连分式的计算。

| 连分数计算反向算法 |
| --- |
| \(z \leftarrow b\_n\) |
| **for** (\(k\) **in** \(n:1\)) { |
| \(\qquad\) \(z \leftarrow b\_{k-1} + \frac{a\_k}{z}\) |
| } |
| 输出\(R\_n \leftarrow z\) |

| 连分数计算正向算法 |
| --- |
| \(A\_0 \leftarrow b\_0\), \(B\_0 \leftarrow 1\) |
| \(A\_1 \leftarrow b\_1 b\_0 + a\_1\), \(B\_1 \leftarrow b\_1\) |
| **for**(\(k=2,\ldots,n\)) { |
| \(\qquad\) \(A\_k = b\_k A\_{k-1} + a\_k A\_{k-2}\) |
| \(\qquad\) \(B\_k = b\_k B\_{k-1} + a\_k B\_{k-2}\) |
| } |
| 输出\(R\_n = A\_n/B\_n\)，同时\(R\_k = A\_k / B\_k, k=1,2,\dots,n\) |

计算幂级数时，如果预先知道需要计算的项数，
可以把幂级数化为无穷连分数，
用连分数的正向算法计算:
\[\begin{align}
\sum\_{i=0}^\infty a\_i x^i
=& \frac{b\_0}{1} \genfrac{}{}{0pt}{}{}{-}
\frac{b\_1 x}{1 + b\_1 x} \genfrac{}{}{0pt}{}{}{-}
\frac{b\_2 x}{1 + b\_2 x} \genfrac{}{}{0pt}{}{}{-} \cdots
\tag{21.6}
\end{align}\]
其中\(b\_0 = a\_0\), \(b\_i = a\_i/a\_{i-1}(i>0)\).

有理分式可以化为连分式计算。
转化的一般规则是:

* 如果分子分母都有常数项，
  如\(\dfrac{1+2x+x^3}{1+2x}\)从分式中减去此常数项，
  使得分子以\(x\)为因子(如\(1 + \dfrac{x^3}{1 + 2x}\))。
* 如果分子中有\(x\)因子而分母中有常数项，则把分式变成分子只有\(x\)。
  (如\(\dfrac{x}{\dfrac{1+2x}{x^2}}\))
* 如果分母中有\(x\)因子而分子中有常数项(如\(\dfrac{1+2x}{x^2}\))，
  写成倒数形式(如\(\dfrac{1}{\dfrac{x^2}{1+2x}}\))再处理。
* 转换的想法是分子上只留常数项或一次项。

例如
\[\begin{aligned}
\dfrac{1+2x+x^3}{1+2x}
=& \frac{1}{1} + \left( \dfrac{1+2x+x^3}{1+2x} - \frac{1}{1} \right)
\ \text{(减去都有的常数项)} \\
=& 1 + \dfrac{x^3}{1+2x}
= 1 + \dfrac{x}{\dfrac{1+2x}{x^2}} \ \text{(分子仅留$x$)} \\
=& 1 + \dfrac{x}{\dfrac{1}{\dfrac{x^2}{1 + 2x}}}
\ \text{(分母有$x$但分子有常数加项时化为倒数)}\\
=& 1 + \dfrac{x}{\dfrac{1}{\dfrac{x}{2+\dfrac{1}{x}}}}
\ \text{(分子仅留$x$)} \\
=& 1 + \frac{x}{0} \genfrac{}{}{0pt}{}{}{+} \frac{1}{0}
\genfrac{}{}{0pt}{}{}{+} \frac{x}{2} \genfrac{}{}{0pt}{}{}{+} \frac{1}{x}.
\end{aligned}\]

对于一般有理分式
\[\begin{align}
g(x) = \frac{\sum\_{i=0}^\infty a\_{1i}x^i}
{\sum\_{i=0}^\infty a\_{0i}x^i}
\tag{21.7}
\end{align}\]
可化为
\[\begin{align}
g(x) = \frac{a\_{10}}{a\_{00}} \genfrac{}{}{0pt}{}{}{+}
\frac{a\_{20}x}{a\_{10}} \genfrac{}{}{0pt}{}{}{+}
\frac{a\_{30}x}{a\_{20}} \genfrac{}{}{0pt}{}{}{+} \cdots
\tag{21.8}
\end{align}\]
其中
\[\begin{aligned}
a\_{mi} = a\_{m-1,0}a\_{m-2,i+1} - a\_{m-2,0}a\_{m-1,i+1},
\ i=0,1,2,\dots; \ m=2,3,4,\dots
\end{aligned}\]

有理分式化为连分式还可以用如下公式：
\[\begin{align}
g(x) =& \frac{\sum\_{i=0}^\infty b\_{1i}x^i}
{\sum\_{i=0}^\infty b\_{0i}x^i}
= \frac{1}{d\_0} \genfrac{}{}{0pt}{}{}{+} \frac{x}{d\_1}
\genfrac{}{}{0pt}{}{}{+} \frac{x}{d\_2} \genfrac{}{}{0pt}{}{}{+} \cdots
\tag{21.9}
\end{align}\]
其中\(d\_m = \frac{b\_{m,0}}{b\_{m+1,0}}\),
\[\begin{aligned}
b\_{m+2,i} = b\_{m,i+1} - d\_m b\_{m+1,i+1},
\ i=0,1,2,\dots; m=0,1,2,\dots
\end{aligned}\]
例如，计算标准正态分布函数的[(21.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#eq:approx-func-pnorm-Taylor)可以用
[(21.9)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#eq:fun-rat-2cfrac2)化为如下连分式：
\[\begin{align}
\Phi(x) =& \begin{cases}
\dfrac{1}{2} + \dfrac{\phi(x) x}{1}
\genfrac{}{}{0pt}{}{}{-} \dfrac{x^2}{3}
\genfrac{}{}{0pt}{}{}{+} \dfrac{2 x^2}{5}
\genfrac{}{}{0pt}{}{}{-} \cdots
\genfrac{}{}{0pt}{}{}{+} \dfrac{(-1)^k k x^2}{2k+1}
\genfrac{}{}{0pt}{}{}{+} \cdots,
& 0 \leq x \leq 3, \\
1 - \dfrac{\phi(x)}{x}
\genfrac{}{}{0pt}{}{}{+} \dfrac{1}{x}
\genfrac{}{}{0pt}{}{}{+} \dfrac{2}{x}
\genfrac{}{}{0pt}{}{}{+} \cdots
\genfrac{}{}{0pt}{}{}{+} \dfrac{k}{x}
\genfrac{}{}{0pt}{}{}{+} \cdots,
& x > 3
\end{cases}.
\tag{21.10}
\end{align}\]

## 21.3 逼近技巧

大多数计算机语言都提供了\(\sqrt{x}\)、\(\ln x\)、\(e^x\)、\(\sin x\)等函数，
R、SAS等系统更提供了大多数概率密度、分布函数、分位数函数和随机数函数。
我们应该尽量利用这些经过很多人验证的函数实现,
但是，也不能盲目相信已有的计算代码。

函数的数学定义、计算公式和计算机算法这三者是有很大差别的。
计算机只能计算加减乘除，
即使有些计算公式已经是泰勒展开式这样的形式，
在实际编程计算时也要考虑浮点表示误差、误差积累等问题。
另外，一个常用函数的算法需要同时满足精确性和高效率，
算法的一点小改进就可能在反复调用中放大为很显著的效率改进。

以\(\sqrt{x}\)计算为例，
如果\(y^2\)的值和\(x\)差距在机器单位\(U\)以下，
则可以用\(y\)作为\(\sqrt{x}\)的值，
尽管这样的\(y\)可能有多个。
构造一个达到如此精度要求的高效率算法可能需要很多努力。
我们知道，泰勒展开、多项式逼近等一般只适用于小范围而不适用于无穷区间，
这时，“范围缩减”技术就可以帮我们把能够精确计算的范围扩大到全定义域。
对\(\sqrt{x}\)，
我们可以用\(x\_0=1\)处的泰勒展开精确计算\(x \in [\frac12, 1]\)区间的\(\sqrt{x}\):
\[\begin{aligned}
\sqrt{x} = 1 + \frac{1}{2} (x-1) - \frac{1}{8} (x-1)^2
+ \frac{1}{16}(x-1)^3 - \frac{5}{128}(x-1)^4 + \cdots
\end{aligned}\]
实际设计算法时要考虑这个公式中正负项互相抵消和误差积累问题。
用\(\sqrt{2} = \sqrt{4 \times \frac{1}{2}} = 2 \sqrt{\frac12}\)计算出\(\sqrt{2}\)，
对于一般的\(x\)，
都可以写成
\[\begin{aligned}
x = a 2^k, \ a \in [\frac12,1]
\end{aligned}\]
从而
\[\begin{aligned}
\sqrt{x} = \sqrt{a} \cdot 2^{k/2}
\end{aligned}\]
如果\(k\)是奇数，设\(k=2m+1\)，则
\(2^{k/2} = 2^m \sqrt{2}\)。

## 习题

#### 习题1

把Legendre多项式推广到\(L^2[a,b]\)中,
给出递推公式。

#### 习题2

设计计算\(L^2[a,b]\)上的Legendre多项式\(P\_n\)系数的算法并编写R程序。

#### 习题3

设计计算Laguerre多项式\(L\_n\)系数的算法并编写R程序。

#### 习题4

用Legendre正交多项式找到标准正态分布函数\(\Phi(x), x \in [0, 3]\)
的绝对误差在\(10^{-6}\)以下的近似公式并用R程序验证。
可以使用R中的`pnorm`函数作为已知的精确值。

#### 习题5

用Laguerre正交多项式方法找到标准正态分布函数\(\Phi(x), x \in [3, \infty)\)
的绝对误差在\(10^{-6}\)以下的近似公式并用R程序验证。

#### 习题6

证明连分式计算的正向算法。

#### 习题7

编写用连分式公式[(21.10)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-func.html#eq:fun-pnorm-cfrac)计算标准正态分布函数\(\Phi(x)\)的R程序，
要求绝对误差控制在\(10^{-10}\)以下。