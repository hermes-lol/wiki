---
crawl_time: '2026-01-17 14:25:07'
framework: sphinx
title: 20 序贯重要抽样(*) | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-sis.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 20 序贯重要抽样(`*`)

MCMC是目前广泛使用的随机模拟方法，
其中Gibbs抽样方法（见§[19.3](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-mcmc.html#sim-Gibbs)）
在确定了各个分量的条件分布后可以轮流产生各个分量的抽样。
序贯重要抽样方法是重要抽样法（见[12](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-impsamp.html#sim-impsamp)）的一种推广，
其做法与Gibbs抽样方法有些相似，
也是每次从一个分量抽样。

回顾多维随机变量抽样的条件分布法（见§[7.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/rng-multi.html#rng-multi-cond)）。
设随机向量\(\boldsymbol X\)的密度
\(\pi(\boldsymbol x) = \pi(x\_1, x\_2, \ldots, x\_n)\)可以逐次地分解为条件密度乘积
\[\pi(x\_1, x\_2, \ldots, x\_n)
= \pi(x\_1) \pi(x\_2|x\_1) \pi(x\_3|x\_1,x\_2) \cdots \pi(x\_n|x\_1,\ldots,x\_{n-1})
\]
则可以用条件分布法产生\(\boldsymbol X\)的抽样。
当数据是时间序列或者可以依次增加对\(\pi\)的信息(比如\(\pi\)是Bayes后验分布)时这种方法很自然。
但是，条件分布\(\pi(x\_t|x\_1,\ldots,x\_{t-1})\)可能是难以得到的或难以抽样的。
为此，采用重要抽样思想：
取一系列辅助分布\(\pi\_t(\boldsymbol x\_t) = \pi\_t(x\_1,\ldots,x\_t), t=1,\ldots,n\)，
使其近似于\((X\_1,\ldots,X\_t)\)的分布，
\(\pi\_n = \pi\)，
各\(\pi\_t\)可以分别有一个未知的归一化常数。
对\(t=1, 2,\dots,n\)用重要抽样法逐次抽取\(X\_1, X\_2, \dots, X\_n\)，
使得\((X\_1, \dots, X\_t)\)是关于\(\pi\_t\)适当加权的样本。
抽取\(X\_t\)时一般是给定\(X\_1, \dots, X\_{t-1}\)后从一个试抽样分布
\(g\_t(x\_t|\boldsymbol x\_{t-1})\)抽取。
适当计算权重就可以得到关于\(\pi\)适当加权的抽样\(\boldsymbol X\_n = (X\_1, X\_2, \dots, X\_n)\)。

这种抽样方法叫做**序贯重要抽样**（sequential importance sampling，SIS），
算法如下:

\[\begin{aligned}
\quad& \text{置} t \leftarrow 1 \\
& \text{从} g\_1(\cdot) \text{抽取} X\_1, \text{置} W\_1 \leftarrow \pi\_1(X\_1)/g\_1(X\_1) \\
& \text{for}(t\ \ \text{in}\ 2:n) \{ \\
& \qquad \text{从} g\_t(x\_t | X\_1, \dots, X\_{t-1}) \text{抽取} X\_t, \text{记} \boldsymbol X\_t = (X\_1, \dots, X\_{t-1}, X\_t) \\
& \qquad \text{计算步进权重} U\_t \leftarrow \dfrac{\pi\_t(\boldsymbol{X}\_t)}
{\pi\_{t-1}(\boldsymbol{X}\_{t-1}) g\_t(X\_t | \boldsymbol{X}\_{t-1})} \\
& \qquad \text{令} W\_t \leftarrow W\_{t-1} U\_t \\
& \} \\
& \text{输出} (\boldsymbol X\_n, W\_n) \text{为关于} \pi(\boldsymbol x) \text{适当加权的样本}
\end{aligned}\]

SIS一般同时独立进行\(N\)组，得到\(\{(\boldsymbol{X}\_n^{(j)}, W\_n^{(j)}) \}\_{j=1}^N\)，
每一组称为一个“**流**”或一个“**粒子**”。

按照SIS步骤，有
\[\begin{eqnarray\*}
& X\_1 \sim g\_1(x\_1), &
w\_1 = \frac{\pi\_1(X\_1)}{g\_1(X\_1)} \\
& X\_2 \sim g\_2(x\_2|x\_1), &
U\_2 = \frac{\pi\_2(\boldsymbol{X}\_2)}{\pi\_1(X\_1) g\_2(X\_2|X\_1)}, \\
& &
W\_2 = W\_1 U\_2 = \frac{\pi\_2(\boldsymbol{X}\_2)}{g\_1(X\_1) g\_2(X\_2|X\_1)} \\
& X\_3 \sim g\_3(X\_3|\boldsymbol{X}\_2), &
U\_3 = \frac{\pi\_3(\boldsymbol{X}\_3)}{\pi\_2(\boldsymbol{X}\_2) g\_3(X\_3|\boldsymbol{X}\_2)},\\
&& W\_3 = \frac{\pi\_3(\boldsymbol{X}\_3)}{g\_1(X\_1) g\_2(X\_2|X\_1) g\_3(X\_3|\boldsymbol{X}\_2)}\\
& \cdots\cdots & \\
& X\_n \sim g\_n(x\_n|\boldsymbol{X}\_{n-1}), &
W\_n = \frac{\pi\_n(\boldsymbol{X}\_n)}
{g\_1(X\_1) g\_2(X\_2|X\_1) \cdots g\_n(X\_n|\boldsymbol{X}\_{n-1})} .
\end{eqnarray\*}\]
记
\[g\_t(\boldsymbol{x}\_t)
= g\_1(x\_1) g\_2(x\_2|x\_1) \cdots g\_t(x\_t|\boldsymbol{x}\_{t-1})
\]
则\(\boldsymbol{X}\_t \sim g\_t(\cdot)\)而
\[
W\_t = \frac{\pi\_t(\boldsymbol{X}\_t)}{g\_t(\boldsymbol{X}\_t)},
\]
因此\((\boldsymbol{X}\_t, W\_t)\)关于\(\pi\_t\)适当加权(\(t=1,\ldots,n\))。
注意\(\pi\_n = \pi\)故这样得到的\((\boldsymbol{X}\_n, W\_n)\)关于\(\pi(\cdot)\)适当加权。

试抽样分布的一种常见取法为\(g\_t(x\_t|\boldsymbol{x}\_{t-1}) = \pi\_t(x\_t|\boldsymbol{x}\_{t-1})\)。

## 20.1 非线性滤波平滑

SIS方法可以用在很多统计模型的计算中,
作为示例，
考虑如下的非线性滤波平滑问题。

设不可观测的“状态”\(X\_t\)服从如下状态方程
\[
X\_t \sim q\_t(\cdot | X\_{t-1}, \theta), \ t=1,2,\dots,n
\]
\(X\_t\)的信息可以反映在可观测的\(Y\_t\)中，其关系满足如下观测方程
\[
Y\_t \sim f\_t(\cdot | X\_t, \phi), \ t=1,2,\dots,n
\]
已知\(\boldsymbol{Y}\_n = (Y\_1, \ldots, Y\_n)=\boldsymbol y\_n\)和\(\theta, \phi\)
后估计\(\boldsymbol X = (X\_1,\ldots,X\_n)\)的问题称为滤波平滑问题,
只需要求后验分布
\(\pi(\boldsymbol{x}\_n) = p\_{\boldsymbol X\_n | \boldsymbol Y\_n}(\boldsymbol{x}\_n | \boldsymbol{y}\_n)\)。
如果能从\(\pi\)大量抽样\(\boldsymbol{X}^{(j)}, j=1,\ldots,N\)
则可以用随机模拟方法对\(\boldsymbol X\_n = (X\_1, \dots, X\_n)\)的后验分布进行推断。
下面用SIS方法产生\(\pi\)的样本。

记
\[
\pi\_t(\boldsymbol{x}\_t)
= p\_{\boldsymbol X\_t | \boldsymbol Y\_t}(\boldsymbol{x}\_t | \boldsymbol{y}\_t), \ t=1,2,\dots,n
\]
注意
\[\begin{align}
\pi\_t(\boldsymbol{x}\_t) \propto
f\_t(y\_t|x\_t)q\_t(x\_t|x\_{t-1}) \pi\_{t-1}(\boldsymbol{x}\_{t-1})
\tag{20.1}
\end{align}\]
取试抽样分布\(g\_t(x\_t | \boldsymbol{x}\_{t-1}) = q\_t(x\_t | x\_{t-1})\),
对\(t=1\), \(\pi\_1(x\_1) = p\_{X\_1|Y\_1}(x\_1 | y\_1) \propto f\_1(y\_1 | x\_1) p(x\_1)\),
其中\(p(x\_1)\)为\(X\_1\)的分布密度，
抽取\(X\_1 \sim g\_1(x\_1)\), 如果可能应取\(g\_1(x\_1) = \pi\_1(x\_1)\)。

产生关于\(\pi\)适当加权的\(\boldsymbol X\_n\)抽样的SIS步骤如下:

\[\begin{aligned}
\quad& \text{置} t \leftarrow 1, \text{从} g\_1(x\_1) \text{抽取} X\_1, \text{置} W\_1 \leftarrow \pi\_1(X\_1)/g\_1(X\_1) \\
& \text{for}(t \text{ in } 2:n) \{ \\
& \qquad \text{从} q\_t(x\_t|X\_{t-1}) \text{抽取} X\_t, \text{记} \boldsymbol X\_t = (X\_1, \dots, X\_{t-1}, X\_t) \\
& \qquad \text{令步进权重} U\_t \leftarrow f\_t(y\_t | X\_t) \\
& \qquad \text{令} W\_t \leftarrow W\_{t-1} U\_t \\
& \} \\
& \text{输出} (\boldsymbol X\_n, W\_n) \text{作为关于} \pi(\boldsymbol x) \text{适当加权的样本}
\end{aligned}\]

这种方法相当于用状态方程前进一步获得下一分量的抽样，
用同时刻的观测值\(y\_t\)的似然作为步进权重。
这样，\(t\)步以后得到的\(\boldsymbol{X}\_t\)服从
\[
g\_t(\boldsymbol{x}\_t) = g\_{t-1}(\boldsymbol{x}\_{t-1}) q\_t(x\_t|x\_{t-1})
\]
其中\(g\_1(x\_1) \propto f\_1(y\_1 | x\_1)\)。
于是
\[\begin{eqnarray\*}
\frac{\pi\_t(\boldsymbol{x}\_t)}{g\_t(\boldsymbol{x}\_t)} &=&
\frac{f\_t(y\_t | x\_t) q\_t(x\_t | x\_{t-1}) \pi\_{t-1}(\boldsymbol{x}\_{t-1})}
{g\_{t-1}(\boldsymbol{x}\_{t-1}) q\_t(x\_t|x\_{t-1})} \\
&=& f\_t(y\_t | x\_t) \frac{\pi\_{t-1}(\boldsymbol{x}\_{t-1})}{g\_{t-1}(\boldsymbol{x}\_{t-1})}, \\
W\_t &=& f\_t(y\_t | X\_t) W\_{t-1} = \frac{\pi\_t(\boldsymbol{X}\_t)}{g\_t(\boldsymbol{X}\_t)},
\end{eqnarray\*}\]
可见\((\boldsymbol{X}\_t, W\_t)\)关于\(\pi\_t\)适当加权,
\((\boldsymbol X\_n, W\_n)\)关于\(\pi\_n(\boldsymbol x) = p\_{\boldsymbol X\_n | \boldsymbol Y\_n}(\boldsymbol x\_n | \boldsymbol y\_n)\)适当加权。

设第\(t\)步的抽样为\(\boldsymbol X\_t^{(i)}, i=1,\dots,N\),
对应权重为\(W\_t^{(i)}, i=1,\dots, n\)。
以上的方法在抽取\((X\_1, \dots, X\_n)\)时不考虑\((y\_1, \dots, y\_n)\)的具体取值，
这样的试抽样分布虽然很容易抽样，
但是效果很差，
权重\(\{ W\_t^{i}, i=1,\dots,N \}\)随着\(t\)的增加会把变得差异很大，
以至于\(N\)个流中只有极少数流能起作用。

由[(20.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-sis.html#eq:sim-filter-pi)可见
\[\begin{aligned}
\pi\_t( x\_t | \boldsymbol x\_{t-1})
= p\_{X\_t | \boldsymbol X\_{t-1}, Y\_t}(x\_t | \boldsymbol x\_{t-1})
\propto f\_t(y\_t|x\_t)q\_t(x\_t|x\_{t-1}),
\end{aligned}\]
如果能取试抽样分布\(g\_t(x\_t) \propto f\_t(y\_t|x\_t)q\_t(x\_t|x\_{t-1})\)
则每次抽取\(X\_t\)都利用了同期的观测值\(y\_t\)的信息，
会大大改善抽样效率。
更进一步，
设\(\pi\_{t+1}(\boldsymbol x\_{t+1})\)关于\(\boldsymbol x\_t\)的边缘分布为\(\pi\_{t,t+1}(\boldsymbol x\_t)\),
如果在第\(t\)步能从\(\pi\_{t,t+1}(\boldsymbol x\_t)\)
的条件分布\(p(x\_t|\boldsymbol x\_{t-1})\)抽样，
就可以利用\(y\_t, y\_{t+1}\)的信息，
抽样效率可以进一步改善。

另外一种改进的办法是再抽样，
增加权重大的流，舍弃权重小的流。

## 20.2 再抽样

如果试抽样分布选取不适当，
最后的权重可能会差别很大，
体现在权重\(\{ W\_t^{(i)}, i=1,\dots,N \}\)的样本变异系数很大，
称为权重偏斜严重。
这时，权重小的流基本不起作用。
出现这样的情况时，
可以把一些权重太小的流舍弃而增加权重大的流，
这样的技术称为**再抽样**。

### 20.2.1 简单随机再抽样

设进行SIS时在时刻\(t\)已经得到了\(N\)个部分的流
\(\{ \boldsymbol X\_t^{(i)}, i=1,\dots, N \}\)以及相应的权重
\(\{ W\_t^{(i)}, i=1,\dots, N \}\)。
可以在每一步都按照权重对流再抽样，
也可以仅当权重偏斜严重时才进行再抽样。
如果在第\(t\)步再抽样，
只要以正比于\(W\_t^{(i)}\)的概率从
\(\{ \boldsymbol X\_t^{(j)}, j=1,\dots, N \}\)中抽取\(\boldsymbol X\_t^{(i)}\),
独立有放回地抽取\(N\)个，
记作\(\{ \boldsymbol X\_t^{\*(i)}, i=1,\dots, N \}\),
并把权重调整为相等的\(W\_t^{\*(i)} = \frac{1}{N} \sum\_{j=1}^N W\_t^{(j)}\),
\(i=1,\dots,N\)。
这样的方法称为**简单随机再抽样**。
这样再抽样后各个流不再是独立的。

### 20.2.2 剩余再抽样

为了达到以上的简单随机抽样的效果，
还可以把大权重的的流直接复制多份，
小权重的流仅以一定比例保留，其它舍弃。
这种方法称为**剩余再抽样**(residual resampling)，
其计算量更小而且模拟误差更小。
算法描述如下。
如果在第\(t\)步需要再抽样，
则计算\(\bar W\_t = \frac{1}{N} \sum\_{j=1}^N W\_t^{(j)}\),
然后按如下做法从\(N\)个流中重新抽取\(N\)个。
首先，对\(i=1,\dots,N\),
直接保留\(k\_i = [W\_t^{(i)}/\bar W\_t]\)份\(\boldsymbol X\_t^{(i)}\)(\([\cdot]\)表示向下取整)；
其次，令\(N\_r = N - \sum\_{i=1}^N k\_i\)为缺额个数，
随机有放回地按照正比于\(\frac{W\_t^{(i)}}{\bar W\_t} - k\_i\)(取整后的小数部分)
的概率从\(\{ \boldsymbol X\_t^{(j)}, j=1,\dots,N \}\)中抽取\(\boldsymbol X\_t^{(i)}\),
共抽取\(N\_r\)个。
这样得到了\(N\)个新的流,
记作\(\{ \boldsymbol X\_t^{\*(i)}, i=1,\dots, N \}\),
并调整其权重为相等的\(W\_t^{\*(i)} = \bar W\_t\),
\(i=1,\dots,N\)。
这种方法的第一步把权重大的流直接复制了若干份，
第二步对按剩余的权重再抽取到满\(N\)个流为止。

### 20.2.3 舍选控制再抽样

简单随机再抽样和剩余再抽样都使得结果各个流不再独立。
另外一种想法是采用§[12](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-impsamp.html#sim-impsamp)中介绍的舍选控制方法，
对权重小的流从头重新抽样并适当调整权重。
首先，
设定若干个要执行再抽样的时间点\(0 < t\_1 < \dots < t\_k \leq n\),
以及相应的权重阈值\(c\_1, \dots, c\_k\)。
在\(t=t\_j\)时，
进行舍选控制再抽样。
若流\(i\)的权重\(W\_t^{(i)} \geq c\_j\),
则保留此流\(\boldsymbol X\_t^{(i)}\)和权重\(W\_t^{(i)}\)不变；
若流\(i\)的权重\(W\_t^{(i)} < c\_j\),
则以概率\(W\_t^{(i)} / c\_j\)保留此流，
如果决定保留，则修改其权重为\(c\_j\)，
如果决定舍弃此流，
则从\(t=1\)重新生成这个流，
同样也需要经过\(t\_1, \dots, t\_j\)处的舍选判断，
如果被舍弃就还从\(t=1\)重新开始，直到被接受。

### 20.2.4 部分舍选控制再抽样

如果从头开始重新抽样的情况发生比较多则模拟的效率会比较低，
为此可以采用如下的部分舍选控制再抽样的SIS方法。

首先，
设定若干个要执行再抽样的时间点\(0 < t\_1 < \dots < t\_k \leq n\),
以及相应的权重阈值\(c\_1, \dots, c\_k\)。
在\(t=t\_j\)时，
进行部分舍选控制再抽样。
若流\(i\)的权重\(W\_t^{(i)} \geq c\_j\),
则保留此流\(\boldsymbol X\_t^{(i)}\)和权重\(W\_t^{(i)}\)不变。
若流\(i\)的权重\(W\_t^{(i)} < c\_j\),
则以概率\(W\_t^{(i)} / c\_j\)保留此流,
如果决定保留，则修改其权重为\(c\_j\)。
如果决定舍弃此流，
则不是从头重新生成这个流，
而是按照概率正比于\(W\_{t\_{j-1}}^{(s)}\)
从\(\{ \boldsymbol X\_{t\_{j-1}}^{(s)}, s=1,\dots, N \}\)
中随机抽取一个替换原来的\(\boldsymbol X\_{t\_{j-1}}^{(i)}\),
用\(t\_{j-1}\)时的权重\(\{ W\_{t\_{j-1}}^{(s)} \}\)的平均值\(\bar W\_{t\_{j-1}}\)代替原来的权重\(W\_{t\_{j-1}}^{(i)}\)，
然后继续按照SIS标准步骤将此流经\(t=t\_{j-1}+1, \dots, t\_j\)延伸得到新的\(\boldsymbol X\_{t\_j}^{(i)}\)
和权重\(W\_{t\_j}^{(i)}\)，然后再进行\(t\_j\)处的舍选控制，
如果被舍弃就再从\(t\_{j-1}\)处随机再抽样后继续，
直到在\(t\_j\)处被接受。

关于序贯重要抽样更详细的讨论参见 Liu ([2001](#ref-Liu-Strategies2001)) 。

## 习题

#### 习题1

在调相通讯中，考虑如下的状态空间模型
\[\begin{aligned}
X\_t =& \phi\_1 X\_{t-1} + \eta\_t, \ \eta\_t \sim N(0, \sigma\_\eta^2), \ t=1,2,\dots,n \\
y\_t =& A \cos(f t + X\_t) + \varepsilon\_t, \ \eta\_t \sim N(0, \sigma\_\varepsilon^2), \ t=1,2,\dots,n \\
\end{aligned}\tag{\*}\]
其中\(\phi\_1 = 0.6, \sigma\_\eta^2=1/6, A = 320, f = 1.072\times 10^7, \sigma\_\varepsilon^2 = 1\),
\((y\_1, \dots, y\_n)\)为观测值，\((X\_1, \dots, X\_n)\)为不可观测的随机变量。

1. 设\(X\_0=0\)，\(n=128\)，模拟生成\((X\_t, y\_t), t=1,2,\dots,n\)。
2. 设计SIS算法产生关于已知\(y\_1, \dots, y\_n\)条件下\(X\_1, \dots, X\_n\)
   的条件分布的适当加权样本，共生成\(N=10000\)组,
   试抽样采用从(\*)向前一步的方法。
3. 考察以上得到的权重\(\{ W\_i \}\)的分布情况。
4. 在SIS抽样的每一步进行剩余再抽样；
5. 根据后验均值方法利用上述改进的抽样估计\((X\_1, \dots, X\_n)\);
6. 对每个\(X\_t\)，计算上述后验估计的标准误差；
7. 独立地重复\(M=400\)次估计过程，
   从\(M\)次不同的后验估计计算新的估计标准误差,
   与6得到的结果进行比较。

### 参考文献

Liu, Jun S. 2001. *Monte Carlo Strategies in Scientific Computing*. Springer.