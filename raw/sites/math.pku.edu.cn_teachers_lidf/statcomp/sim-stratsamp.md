---
crawl_time: '2026-01-17 14:25:16'
framework: sphinx
title: 13 分层抽样法 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-stratsamp.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 13 分层抽样法

用平均值法计算\(\int\_C h(\boldsymbol x) \,d\boldsymbol x\)，
若\(h(\boldsymbol x)\)在\(C\)内取值变化范围大则估计方差较大。
重要抽样法选取了与\(f(x)\)形状相似但是容易抽样的密度\(g(\boldsymbol x)\)作为试投密度，
大大提高了精度， 但是这样的\(g(\boldsymbol x)\)有时难以找到。

如果把\(C\)上的积分分解为若干个子集上的积分，
使得\(h(\boldsymbol x)\)在每个子集上变化不大， 分别计算各个子集上的积分再求和，
可以提高估计精度。 这种方法叫做**分层抽样法**。 这也是抽样调查中的重要技术。

**例13.1** 对函数
\[\begin{aligned}
h(x) = \begin{cases}
1 + \frac{x}{10}, & 0 \leq x \leq 0.5 \\
-1 + \frac{x}{10}, & 0.5 < x \leq 1
\end{cases}
\end{aligned}\]
求定积分
\[\begin{aligned}
I = \int\_0^1 h(x)\,dx,
\end{aligned}\]
可以得\(I\)的精确值为\(I = 0.05\)。
我们用平均值法和分层抽样法来估计\(I\)并比较精度。

在\((0,1)\)区间随机抽取\(N\)点用平均值法得\(\hat I\_2\)，其渐近方差为
\[\begin{aligned}
\text{Var}(\hat I\_2) = \frac{\text{Var}(h(U))}{N}
= \frac{143}{150N} \approx \frac{0.9533}{N} .
\end{aligned}\]

模拟的R程序:

```
h <- function(x) ifelse(x <= 0.5, 1 + 0.1*x, -1 + 0.1*x)
I.true <- 0.05
set.seed(1)
N <- 10000
eta <- h(runif(N))
I2 <- mean(eta)
sd2 <- sd(eta)
```

取\(N=10000\)，一次平均值法得到的估计和相对误差为

```
I2
## [1] 0.0594168
(I2 - I.true)/I.true
## [1] 0.1883359
```

相对误差为0.2，仅有一位有效数字。
估计值可报告为0.06。

估计的\(N\text{Var}(\hat I\_2)\)为

```
sd2^2
## [1] 0.9504117
```

理论值为0.9533。

把\(I\)拆分为\([0,0.5]\)和\([0.5,1]\)上的积分，即
\[\begin{aligned}
I = a + b = \int\_0^{0.5} h(x)\,dx + \int\_{0.5}^1 h(x)\,dx ,
\end{aligned}\]
对\(a\)和\(b\)分别用平均值法，得
\[\begin{aligned}
\hat a =& \frac{0.5}{N/2} \sum\_{i=1}^{N/2} h(0.5 U\_i)
= \frac{0.5}{N/2} \sum\_{i=1}^{N/2} (1 + 0.05 U\_i), \\
\hat b =& \frac{0.5}{N/2} \sum\_{i=(N/2)+1}^{N} h(0.5 + 0.5 U\_i)
= \frac{0.5}{N/2} \sum\_{i=(N/2)+1}^{N} (-1 + 0.05 + 0.05 U\_i), \\
\hat I\_5 =& \hat a + \hat b,
\end{aligned}\]
则分层抽样法结果\(\hat I\_5\)的渐近方差为
\[\begin{aligned}
\text{Var}(\hat I\_5) =&
\text{Var}(\hat a + \hat b)
= \text{Var}(\hat a) + \text{Var}(\hat b) \\
=& 0.25 \frac{\text{Var}(1 + 0.05U)}{N/2} +
0.25 \frac{\text{Var}(-0.95 + 0.05U)}{N/2}
= \frac{1/4800}{N},
\end{aligned}\]
分层后的估计方差远小于不分层的结果,
可以节省样本量约4500倍。

分层抽样法的一次模拟计算的R程序如下：

```
N <- 10000
set.seed(1)
eta1 <- h(runif(N/2, 0, 0.5))
a <- 0.5*mean(eta1)
eta2 <- h(runif(N/2, 0.5, 1))
b <- 0.5*mean(eta2)
I5 <- a+b
```

一次模拟的估计为：

```
I5
## [1] 0.0500084
```

\(N \text{Var}(\hat I\_5)\)的估计为

```
Nvar <- 0.5*var(eta1) + 0.5*var(eta2); Nvar
## [1] 0.0002117651
```

理论值是0.002083。

※※※※※

一般地，设积分\(I=\int\_C h(\boldsymbol x)\,d\boldsymbol x\)可以分解为\(m\)个不交的子集\(C\_j\)上的积分,
即
\[\begin{aligned}
I = \int\_C h(\boldsymbol x)\,d\boldsymbol x
= \int\_{C\_1} h(\boldsymbol x)\,d\boldsymbol x + \int\_{C\_2} h(\boldsymbol x)\,d\boldsymbol x
+ \dots + \int\_{C\_m} h(\boldsymbol x)\,d\boldsymbol x
\end{aligned}\]
在\(C\_j\)投\(n\_j\)个随机点\(X\_{ji} \sim \text{U}(C\_j)\), \(i=1,\ldots,n\_j\),
则\(I\)的\(m\)个部分可以分别用平均值法估计，由此得\(I\)的分层估计为
\[\begin{aligned}
\hat I\_5 = \sum\_{j=1}^m \frac{V(C\_j)}{n\_j} \sum\_{i=1}^{n\_j} h(X\_{ji})
\end{aligned}\]
记\(\sigma\_j^2 = \text{Var}(h(X\_{j1}))\),
划分子集时应使每一子集内\(h(\cdot)\)变化不大，即\(\sigma\_j^2\)较小。 这时
\[\begin{aligned}
\text{Var}(\hat I\_5) = \sum\_{j=1}^m \frac{V^2(C\_j) \sigma\_j^2}{n\_j}
\end{aligned}\]
若\(\sigma\_j^2\)可估计， 应取\(n\_j\)使
\[\begin{align}
n\_j \propto V(C\_j) \sigma\_j,
\tag{13.1}
\end{align}\]
即
\[\begin{aligned}
n\_j = N \frac{V(C\_j) \sigma\_j}{\sum\_{k=1}^m V(C\_k) \sigma\_k},
\ j=1,2,\dots,m
\end{aligned}\]
这样取的样本量\((n\_1, n\_2, \dots, n\_m)\)
在所有满足\(n\_1 + n\_2 + \dots + n\_m=N\)的取法中使得渐近方差最小。

在分层抽样法中，划分了子集后， 每一子集上的积分也可用重要抽样法计算。

分层抽样法也可以用在求随机变量函数期望的问题中。
设\(X\)为随机变量，要求\(X\)的函数\(h(X)\)的数学期望\(\theta = E h(X)\)。
假设存在离散型随机变量\(Y\)，\(p\_j = P(Y=y\_j), j=1,2,\dots,m\),
在\(Y=y\_j\)条件下可以从\(X\)的条件分布抽样， 则
\[\begin{align}
E[h(X)] = E\{E[h(X)|Y]\}
= \sum\_{j=1}^m E[h(X) | Y=y\_j] p\_j,
\end{align}\]
如果在\(Y = y\_j\)条件下生成\(X\)的\(N\_j = N p\_j\)个抽样值，
设为\(X\_i^{(j)}, i=1,2,\dots, N\_j\),
则可以用\(\frac{1}{N\_j} \sum\_{i=1}^{N\_j} h(X\_i^{(j)})\)估计
\(E[h(X) | Y=y\_j]\)， 估计\(\theta\)为
\[\begin{align}
\hat\theta = \sum\_{j=1}^m \frac{1}{N\_j} \sum\_{i=1}^{N\_j} h(X\_i^{(j)}) p\_j
= \frac{1}{N} \sum\_{j=1}^m \sum\_{i=1}^{N p\_j} h(X\_i^{(j)}),
\tag{13.2}
\end{align}\]
这是\(\theta\)的无偏和强相合估计， 且估计方差
\[\begin{align}
\text{Var}(\hat\theta)
=& \frac{1}{N^2} \sum\_{j=1}^m N p\_j \text{Var}[h(X) | Y=y\_j] \nonumber \\
=& \frac{1}{N} \sum\_{j=1}^m \text{Var}[h(X) | Y=y\_j] p\_j
\tag{13.3} \\
=& \frac{1}{N} E \{ \text{Var}[h(X) | Y] \}
\leq \frac{1}{N} \text{Var}[h(X)],
\end{align}\]
比直接用平均值法估计\(E h(X)\)的方差小。 这里用到了条件方差的性质
\[\begin{align}
\text{Var}(X)
= E \left[ \text{Var}(X|Y) \right] + \text{Var} \left[ E(X|Y) \right] \geq E \left[ \text{Var}(X|Y) \right],
\end{align}\]
如果\(Y\)与\(X\)独立则\(E(X|Y)=EX\), \(\text{Var} \left[ E(X|Y) \right] = 0\),
这时分层抽样法比平均值法没有改进。 从[(13.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-stratsamp.html#eq:sim-int-stra-cve0)可以看出，
如果第\(j\)层样本的函数\(h(X\_i^{(j)}), i=1,2,\dots,Np\_j\)的样本方差为\(S\_j^2\),
则\(\text{Var}(\hat\theta)\)的一个无偏估计是
\[\begin{align\*}
\widehat{\text{Var}(\hat\theta)} = \frac{1}{N} \sum\_{j=1}^m S\_j^2 p\_j.
\end{align\*}\]

公式[(13.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-stratsamp.html#eq:sim-int-stra-hat0)取第\(j\)层样本数\(N\_j = N p\_j\)，
仅考虑了\(Y\)的取值分布，而未考虑\(X | Y=y\_j\)的条件分布情况。
类似于[(13.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-stratsamp.html#eq:sim-int-stra-nj0)，
应该对\(\text{Var}[h(X) | Y=y\_j]\)较大的层取更多的样本。
使得估计方差最小的分层样本量分配满足\(N\_j \propto p\_j \sqrt{\text{Var}[h(X)|Y=y\_j]}\),
即
\[\begin{align}
N\_j = N \frac{ p\_j \sqrt{\text{Var}[h(X)|Y=y\_j]}}{\sum\_{k=1}^m p\_k \sqrt{\text{Var}[h(X)|Y=y\_k]}}.
\tag{13.4}
\end{align}\]
在\(\text{Var}[h(X)|Y=y\_j]\)未知的时候，
可以预先抽取一个小的样本估计\(\text{Var}[h(X)|Y=y\_j]\),
然后按估计的最优\(N\_j\)分配各层的样本量。 采用[(13.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-stratsamp.html#eq:sim-int-str-ss3)的分层样本量后，
\[\begin{align\*}
\hat\theta =& \sum\_{j=1}^m \frac{1}{N\_j} \sum\_{i=1}^{N\_j} h(X\_i^{(j)}) p\_j, \\
\text{Var}(\hat\theta) =& \sum\_{j=1}^m \frac{p\_j^2 \text{Var}[h(X)|Y=y\_j]}{N\_j},
\end{align\*}\]
于是\(\text{Var}(\hat\theta)\)的估计为
\[\begin{aligned}
\widehat{\text{Var}(\hat\theta)} =& \sum\_{j=1}^m \frac{p\_j^2 S\_j^2}{N\_j},
\end{aligned}
\]
其中\(S\_j^2\)是第\(j\)层样本函数\(\{ h(X\_i^{(j)}), i=1,2,\dots,N\_j \}\)的样本方差。

分层抽样法的本质是把\(X\)的值相近的抽样分入一层，
使得同层的\(X\)条件方差较小，从而减小估计方差。

**例13.2** 设\(U \sim \text{U}(0,1)\), 用分层抽样法估计\(\theta = E h(U) = \int\_0^1 h(x) \,dx\)。

令\(Y = \text{ceiling}(mU)\),
即当且仅当\(\frac{j-1}{m} < U \leq \frac{j}{m}\)时\(Y=j\)， \(j=1,2,\dots,m\),
可以按照\(Y\)分层抽样估计\(\theta\):
\[\begin{aligned}
\theta =& E[h(U)] = \sum\_{j=1}^m E[h(U) | Y=j] \, P(Y=j) \\
=& \frac{1}{m} \sum\_{j=1}^m E[h(U) | Y=j],
\end{aligned}\]
易见\(Y=j\)条件下\(U\)服从\((\frac{j-1}{m}, \frac{j}{m})\)上的均匀分布，
设\(U\_1, U\_2, \dots, U\_n\)是U(0,1)的独立抽样，则用分层抽样法取每层\(N\_j=1\)估计\(\theta = E h(U)\)为
\[\begin{aligned}
\hat\theta =& \frac{1}{m} \sum\_{j=1}^m h\left( \frac{j-1 + U\_j}{m} \right).
\end{aligned}\]

※※※※※

## 习题

#### 习题1

设\(\sigma\_j, j=1,2,\dots, m\)为\(m\)个正实数,
\[\begin{aligned}
f(\alpha\_1, \alpha\_2, \dots, \alpha\_m)
=& \frac{\sigma\_1^2}{\alpha\_1} + \frac{\sigma\_2^2}{\alpha\_2}
+ \dots + \frac{\sigma\_m^2}{\alpha\_m},
\ (\alpha\_1, \dots, \alpha\_m) \in (0,1)^m,
\end{aligned}\]
则在\(\alpha\_1 + \alpha\_2 + \dots + \alpha\_m = 1\)条件下
\(f(\alpha\_1, \alpha\_2, \dots, \alpha\_m)\)的最小值点为
\[\begin{aligned}
\alpha\_j = \frac{\sigma\_j}{\sum\_{k=1}^m \sigma\_k},
\ j=1,2,\dots,m
\end{aligned}\]
最小值为\((\sigma\_1 + \dots + \sigma\_m)^2\)。

#### 习题2

考虑定积分
\[\begin{aligned}
I = \int\_{-1}^1 e^x dx = e - e^{-1}.
\end{aligned}\]

1. 用随机模拟方法计算定积分\(I\),
   分别用随机投点法、平均值法、重要抽样法和分层抽样法计算。
2. 设估计结果为\(\hat I\),
   如果需要以95%置信度保证计算结果精度在小数点后三位小数，
   这四种方法分别需要计算多少次被积函数值？
3. 用不同的随机数种子重复以上的估计\(B\)次，
   得到\(\hat I\_j, j=1,2,\dots,B\),
   由此估计\(\hat I\)的抽样分布方差，
   与2的结果进行验证。
4. 称
   \[\begin{aligned}
   \mbox{MAE}(\hat I) = E | \hat I - I|
   \end{aligned}\]
   为\(\hat I\)的平均绝对误差。
   从3得到的\(\hat I\_j, j=1,2,\dots,B\)中估计\(\mbox{MAE}(\hat I)\)。
   比较这四种积分方法的平均绝对误差大小。

#### 习题3

设\(h(x) = \frac{e^{-x}}{1 + x^2}\), \(x \in (0, 1)\),
用重要抽样法计算积分\(I = \int\_0^1 h(x) \,dx\)，
分别采用如下的试抽样密度:
\[\begin{aligned}
f\_1(x) =& 1, \ x \in (0, 1), \\
f\_2(x) =& e^{-x}, \ x \in (0, \infty), \\
f\_3(x) =& \frac{1}{\pi (1 + x^2)}, \ x \in (-\infty, \infty), \\
f\_4(x) =& (1 - e^{-1})^{-1} e^{-x}, \ x \in (0, 1), \\
f\_5(x) =& \frac{4}{\pi (1 + x^2)}, \ x \in (0, 1).
\end{aligned}\]

1. 作\(h(x)\)和各试抽样密度的图形，比较其形状。
2. 取样本点个数\(N=10000\),
   分别给出对应于不同试抽样密度的估计\(\hat I\_k\), \(k=1,2,3,4,5\),
   以及\(\text{Var}(\hat I\_k)\)的估计。
3. 分析\(\text{Var}(\hat I\_k)\)的大小差别的原因。
4. 把\((0,1)\)区间均分为10段，在每一段内取\(N=1000\)个样本点用平均值法计算积分值，
   把各段的估计求和得到\(I\)的估计\(\hat I\_6\)，估计其方差。
5. 用例[13.2](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-stratsamp.html#exm:strat-grid)的分层抽样方法计算积分的估计\(\hat I\_7\)，
   估计\(\text{Var}(\hat I\_7)\)并与前面的结果进行比较。