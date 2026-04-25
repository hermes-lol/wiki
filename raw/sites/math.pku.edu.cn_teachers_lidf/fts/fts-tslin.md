---
crawl_time: '2026-01-17 14:30:40'
framework: sphinx
title: 3 线性时间序列模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 3 线性时间序列模型

## 3.1 介绍

课程采用蔡瑞胸（Ruey S. Tsay）的《金融数据分析导论：基于R语言》([Tsay 2013](#ref-Tsay13:IAFD))
（An Introduction to Analysis of Financial Data with R）作为主要教材之一。
“线性时间序列模型”这一部分是教材的第二章和第三章的授课笔记，
本章讲授时间序列的线性模型，包括：

* 一些基本概念
* AR, MA, ARMA模型
* 单位根过程
* 指数平滑
* 季节模型
* 回归模型中误差项有序列相关的处理
* 长记忆的分数阶差分模型
* 模型比较
* 实例分析

### 3.1.1 例子：苹果公司2007年到2017年股票日收盘价

```
chartSeries(
  AAPL, type="line", TA=NULL,
  subset="2003/2017",
  major.ticks="years", minor.ticks=FALSE,
  theme="white", name="Apple"
  )
```

![苹果公司股票日收盘价](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-AAPL02-1.png)

图3.1: 苹果公司股票日收盘价

股价序列呈现缓慢的、非单调的上升趋势，
局部又有短暂的波动。

### 3.1.2 例子：可口可乐公司盈利季度数据

可口可乐公司每季度发布的每股盈利数据。
读入：

```
da <- read_table(
  "q-ko-earns8309.txt",
  col_types=cols(
    .default = col_double(),
    pends = col_date("%Y%m%d"),
    anntime = col_date("%Y%m%d")
    ) )
ko.Rqtr <- xts(da[["value"]], da[["pends"]])
```

时间序列图：

```
chartSeries(
  ko.Rqtr, type="line", TA=NULL,
  major.ticks="years", minor.ticks=FALSE,
  theme="white", name="Coca Kola Quarterly Return"
  )
```

![可口可乐季度盈利](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-KO02-1.png)

图3.2: 可口可乐季度盈利

序列仍体现出缓慢的、非单调的上升趋势，又有明显的每年的周期变化（称为季节性），
还有短期的波动。

下面用基本R的`plot()`作图并用不同颜色标出不同季节。

```
tmp.x <- year(index(ko.Rqtr)) + (quarter(index(ko.Rqtr))-1)/4
tmp.y <- c(coredata(ko.Rqtr))
plot(tmp.x, tmp.y, type="l", col="gray",
     xlab="year", ylab="Return")
cpal <- c("green", "red", "yellow", "black")
points(tmp.x, tmp.y, pch=16, 
       col=cpal[quarter(index(ko.Rqtr))])
legend("topleft", pch=16, col=cpal,
       legend=c("Spring", "Summer", "Autumn", "Winter"))
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-KO03-1.png)

用ggplot2包绘图：

```
da_b <- da %>%
  mutate(time = as.yearqtr(pends),
    qtr = factor(quarter(pends),
       levels = 1:4,
       labels=c("Spring", "Summer", "Autumn", "Winter")))
ggplot(data = da_b, mapping = aes(
  x = time, y = value)) +
  geom_point(mapping=aes(color=qtr)) +
  geom_line()
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-KO03b-1.png)

现在可以看出，每年一般冬季和春季最低，
夏季最高，秋季介于夏季和冬季之间。

### 3.1.3 例子：标普500指数月对数收益率

```
d <- read_table(
  "m-ibmsp-2611.txt",
  col_types=cols(.default=col_double(),
                 date=col_date(format="%Y%m%d")))
sp.rmon <- xts(log(1 + d[["sp"]]), d[["date"]])
```

```
chartSeries(
  sp.rmon, type="line", TA=NULL,
  major.ticks="auto", minor.ticks=FALSE,
  theme="white", name="S&P 500 Monthly Returns"
  )
```

![标普500月收益率](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-sp02-1.png)

图3.3: 标普500月收益率

收益率在0上下波动，除了个别时候基本在某个波动范围之内。

### 3.1.4 例子：美国国债3月期和6月期周利率

```
d <- read_table2(
  "w-tb3ms.txt",
  col_types=cols(.default=col_double()))
```

```
## Warning: `read_table2()` was deprecated in readr 2.0.0.
## ℹ Please use `read_table()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
x1 <- xts(d[["rate"]], make_date(d[["year"]], d[["mon"]], d[["day"]]))
d <- read_table2(
  "w-tb6ms.txt",
  col_types=cols(.default=col_double()))
x2 <- xts(d[["rate"]], make_date(d[["year"]], d[["mon"]], d[["day"]]))
tb36ms <- merge(x1, x2)
names(tb36ms) <- c("tb3ms", "tb6ms")
rm(d, x1, x2)
```

用xts包的`plot()`函数作图：

```
plot(tb36ms, type="l", grid.ticks.on="years")
```

![美国3月期和6月期国债周利率](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-tb02-1.png)

图3.4: 美国3月期和6月期国债周利率

聚焦到2004年的数据：

```
plot(tb36ms, type="l", subset="2004")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-tb02b-1.png)

红色是6月期国债利率，
黑色是3月期国债。
一般6月期高，
但是有些时期3月期超过了6月期，如1980年：

```
plot(tb36ms, type="l", subset="1980")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-intro-tb02c-1.png)

## 3.2 平稳性

如图[3.3](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#fig:tslin-intro-sp02)那样的收益率数据基本呈现出在一个水平线（一般是0）上下波动，
且波动范围基本不变。
这样的表现是时间序列“弱平稳序列”的表现。
由弱平稳性，
可以对未来的标普500收益率预测如下：
均值在0左右，上下幅度在\(\pm 0.2\)之间。

弱平稳需要一阶矩和二阶矩有限。
记\(Ex\_t = \mu\)不变，
\(\gamma\_0 = \text{Var}(x\_t) = E(x\_t - \mu)^2\)不变。
某些分布是没有有限的二阶矩的，比如柯西分布，
这样的分布就不适用传统的线性时间序列理论。

稍后给出弱平稳的理论定义。

如图[3.2](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#fig:tslin-intro-KO02)这样的价格序列则呈现出水平的上下起伏，
如果分成几段平均的话，
各段的平均值差距较大。
这体现出非平稳的特性。

**时间序列**：设有随机变量序列
\(\{ x\_t, t=\dots, -2, -1, 0, 1, 2, \dots \}\)，
称其为一个时间序列。其中\(x\_t\)是一个随机变量，
也可以写成大写的\(X\_t\)。
时间序列\(\{ X\_t \}\)严格来说是一个二元的函数
\(X(t, \omega)\), \(t \in \mathbb Z\)(\(\mathbb Z\)表示所有整数组成的集合)，
\(\omega \in \Omega\)，
\(\Omega\)表示在一定的条件下所有可能的试验结果的集合。
经济和金融中的时间序列我们只能观察到其中某一个\(\omega\_0 \in \Omega\)对应的结果，
称为一条“轨道”。
而针对随机变量的许多理论性质都是在\(\omega \in \Omega\)上讨论的，
比如\(E X\_t = \int X\_t(\omega) P(d\omega)\)是\(X\_t(\omega)\)对\(\omega\in \Omega\)的平均。

为了能够用一条轨道的观测样本得到所有\(\omega\in\Omega\)的性质，
需要时间序列满足“遍历性”。

时间序列的样本：
设\(\{x\_t, t=1,2,\dots, T \}\)是时间序列中的一段。
仍将\(x\_t\)看成随机变量，也可以写成大写的\(X\_t\)。
如果有了具体数值，
那么样本就是一条轨道中的一段。

**自协方差函数**：
时间序列\(\{ X\_t \}\)中两个随机变量的协方差
\(\text{Cov}(X\_s, X\_t)\)叫做自协方差。
如果\(\text{Cov}(X\_s, X\_t) = \gamma\_{|t-s|}\)仅依赖于\(t-s\)，
则称
\[
\gamma\_k = \text{Cov}(X\_{t-k}, X\_t), k=0,1,2,\dots
\]
为时间序列\(\{X\_t \}\)的自协方差函数。
因为\(\text{Cov}(X\_s, X\_t) = \text{Cov}(X\_t, X\_s)\)，
所以\(\gamma\_{-k} = \gamma\_k\)。
易见\(\gamma\_0 = \text{Var}(X\_t)\)。

由Cauchy-Schwartz不等式，
\[
|\gamma\_k | = \left| E[ (X\_{t-k} - \mu) (X\_t - \mu)] \right|
\leq \left( E(X\_{t-k} - \mu)^2 \; E(X\_t - \mu)^2 \right)^{1/2} = \gamma\_0
\]

**弱平稳序列**(宽平稳序列，weakly stationary time series):
如果时间序列\(\{ X\_t \}\)存在有限的二阶矩且满足：

 (1) \(EX\_t = \mu\)与\(t\)无关；

 (2) \(\text{Var}(X\_t) = \gamma\_0\)与\(t\)无关;

 (3) \(\gamma\_k = \text{Cov}(X\_{t-k}, X\_t)\), \(k=1,2,\dots\)与\(t\)无关，

则称\(\{ X\_t \}\)为弱平稳序列。

适当条件下可以用时间序列的样本估计自协方差函数，
这是用一条轨道的信息推断所有实验结果\(\Omega\)，
估计公式为
\[
\hat\gamma\_k = \frac{1}{T} \sum\_{t=k+1}^T (x\_{t-k} - \bar x)(x\_t - \bar x),
k=0,1,\dots, T-1
\]
称\(\hat\gamma\_k\)为样本自协方差。
注意这里用了\(1/T\)而不是\(1/(T-k)\)，
用\(1/(T-k)\)在获得无偏性的同时会造成一些理论上的困难。

## 3.3 相关系数和自相关函数

### 3.3.1 相关系数

![IBM对标普500月度简单收益率](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-acf-ibmsp01-1.png)

图3.5: IBM对标普500月度简单收益率

图[3.5](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#fig:tslin-acf-ibmsp01)是IBM股票月度简单收益率对标普500收益率的散点图。
从图中看出，
两者有明显的正向相关关系。

两个随机变量\(X\)和\(Y\)的相关系数定义为
\[
\rho(X,Y) = \rho\_{xy} = \frac{\text{Cov}(X,Y)}{\sqrt{\text{Var}(X) \text{Var}(Y)}}
= \frac{E[(X-\mu\_x)(Y-\mu\_y)]}{\sqrt{E(X-\mu\_x)^2 E(Y-\mu\_y)^2}}
\]

如果有\((X,Y)\)的独立同分布样本\((x\_t, y\_t)\)，
\(t=1,2,\dots,T\)，
可估计相关系数为
\[
\hat\rho\_{xy}
= \frac{\sum\_{t=1}^T (x\_t - \bar x)(y\_t - \bar y)}
{\sqrt{\sum\_{t=1}^T (x\_t - \bar x)^2 \sum\_{t=1}^T (y\_t - \bar y)^2}}
\]

对于不独立的样本，
比如时间序列样本，
也可以计算相关系数，
其估计合理性需要一些模型假设。

对于联合分布非正态的情况，
有时相关系数不能很好地反映\(X\)和\(Y\)的正向或者负向的相关。
斯皮尔曼（Spearman）相关系数是计算\(X\)的样本的秩（名次）与\(Y\)的样本的秩之间的相关系数，
也称为Spearman rank correlation。

另一种常用的非参数相关系数是肯德尔tau(Kendall’s \(\tau\))系数，
反映了一致数对和非一致数对之间的差别。
对随机向量\((X, Y)\)，
设\((X\_1, Y\_1)\), \((X\_2, Y\_2)\)相互独立且联合分布与\((X,Y)\)联合分布相同，
定义\(X\)和\(Y\)的肯德尔tau系数为
\[
\tau = P\left[ (X\_1 - X\_2)(Y\_1 - Y\_2) > 0 \right]
- P\left[ (X\_1 - X\_2)(Y\_1 - Y\_2) < 0 \right]
\]
即两个观测的分量次序一致的概率减去分量次序相反的概率。
一致的概率越大，说明两个的正向相关性越强。

对IBM收益率与标普收益率数据计算这三种相关系数：

```
cor(d[,"sp"], d[,"ibm"])
```

```
## [1] 0.6395979
```

```
cor(d[,"sp"], d[,"ibm"], method="spearman")
```

```
## [1] 0.6065789
```

```
cor(d[,"sp"], d[,"ibm"], method="kendall")
```

```
## [1] 0.4328066
```

### 3.3.2 自相关函数与白噪声

设\(\{X\_t \}\)为弱平稳序列，
\(\{ \gamma\_k \}\)为自协方差函数。
则
\[
\rho(X\_{t-k}, X\_t)
= \frac{\text{Cov}(X\_{t-k}, X\_t)}{\sqrt{\text{Var}(X\_{t-k})\text{Var}(X\_{t})}}
= \frac{\gamma\_k}{\sqrt{\gamma\_0 \gamma\_0}}
= \frac{\gamma\_k}{\gamma\_0},
\ k=0,1,\dots, \ \forall t
\]
记\(\rho\_k = \gamma\_k / \gamma\_0\)，
这是\(X\_{t-k}\)与\(X\_t\)的相关系数且与\(t\)无关，
称\(\{ \rho\_k, k=0,1,\dots \}\)为时间序列\(\{ X\_t \}\)的自相关函数
（Autocorrelation function, ACF）。
\(\rho\_0=1\)。

如果弱平稳序列\(\{ X\_t \}\)满足\(\rho\_k=0\), \(k=1,2,\dots\)，
称\(\{ X\_t \}\)为**白噪声序列**。
如果随机变量序列\(\{ X\_t \}\)独立且期望和方差不随时间而变，
则\(\{ X\_t \}\)是白噪声序列，
称为独立白噪声。
如果独立白噪声还是同分布的，
称为独立同分布白噪声。

适当条件下\(\rho\_k\)可以从时间序列样本估计为
\[
\hat\rho\_k = \frac{\hat\gamma\_k}{\hat\gamma\_0},\ k=0,1,\dots
\]
\(\hat\rho\_0=1\)。
称\(\hat\rho\_k, k=1,2,\dots\)为**样本自相关函数**。

如果时间序列严平稳遍历，
则\(\hat\rho\_k\)是\(\rho\_k\)的强相合估计。

若\(\{ X\_t \}\)为有二阶矩的独立同分布随机变量列，
则
\(\hat\rho\_k\)(\(k>0\))渐近服从\(\text{N}(0, \frac{1}{T})\)。

如果\(\{\varepsilon\_t \}\)是零均值独立同分布白噪声，
\(q\)为非负整数，
\(\{ \psi\_j, j=0,1,\dots, q \}\)是数列，\(\psi\_0=1\)，
\[
X\_t = \mu + \sum\_{j=0}^q \psi\_j \varepsilon\_{t-j}, \ t \in \mathbb Z,
\]
则从\(\{ X\_t, t=1,\dots,T \}\)计算的ACF满足:
当\(k > q\)时，
\(\sqrt{T}\hat\rho\_k\)渐近服从\(\text{N}(0, 1 + 2 \sum\_{j=0}^q \rho\_j^2)\),
这称为Bartlett公式。
参见 ([何书元 2003](#ref-He-TSA03)) P.131 §4.2的例2.1。
原始文献：
MAURICE STEVENSON BARTLETT,
On the Theoretical Specification and Sampling Properties of Auto-Correlated Time Series,
Journal of the Royal Statistical Society (Supplement) 8 (1946), pp. 24-41.

在基本R软件中，
`acf(x)`可以估计时间序列`x`的自相关函数并对其前面若干项画图。

**例3.1** 例：CRSP的第10分位组合的月对数收益率，
1967-1到2009-12。
第10分位组合是NYSE、AMEX、NASDAQ市值最小的10%股票组成的投资组合，
每年都重新调整。

* CRSP是Center for Research in Security Prices,
  位于Chicago Booth。
* NYSE(The New York Stock Exchange, 纽约证券交易所),
* AMEX(American Stock Exchange, 美国证券交易所，在纽约华尔街附近)，
* NASDAQ(National Association of Securities Dealers Automated Quotations，纳斯达克，位于纽约)。

```
d <- read_table(
  "m-dec12910.txt", col_types=cols(
    .default=col_double(),
    date=col_date(format="%Y%m%d")))
dec <- xts(as.matrix(d[,-1]), d$date)
tclass(dec) <- "yearmon"
d10 <- ts(coredata(dec)[,"dec10"], start=c(1967,1), frequency=12)
plot(d10, main="CRSP Lower 10% Mothly Returns")
```

![CRSP第10分位组合月对数收益率](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-acf-dec01-1.png)

图3.6: CRSP第10分位组合月对数收益率

用`acf()`作时间序列的自相关函数图：

```
acf(d10)
```

![CRSP第10分位组合月对数收益率的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-acf-dec02-1.png)

图3.7: CRSP第10分位组合月对数收益率的ACF

`acf()`的返回值是一个列表，其中`lag`相当于\(k\)，
`acf`相当于\(\hat\rho\_k\)。
用`plot=FALSE`取消默认的图形输出。

ACF图中横轴上下两条水平下是在独立同分布白噪声假设下的加减两倍标准差，
即\(\pm \frac{2}{\sqrt{T}}\)。
如果独立同分布白噪声假设成立，
每个\(\hat\rho\_k\)有95%以上的概率落入这两条线之间。

ACF图\(k=0\)处总对应\(\hat\rho\_0=1\)。

上图的\(\hat\rho\_1\)和\(\hat\rho\_{12}\)都超出了界限
（因为是月度数目，横轴的单位是\(1/12\)为一个时间点）。
从此图可以认为此投资组合的收益率不是白噪声。

标准库stats的`acf()`作自相关函数图总是有\(\rho\_0 = 1\)，
这在其它系数很小时会使得其它系数显得很难分辨，
另外，当时间序列是月度或季度数据时，
横坐标是以年为单位。
所以，forecast包提供了一个类似功能的`Acf()`函数，
改进了上述缺点。如：

```
forecast::Acf(d10)
```

![CRSP第10分位组合月对数收益率的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-acf-dec02b-1.png)

图3.8: CRSP第10分位组合月对数收益率的ACF

TSA包的`acf()`函数也不做滞后0的点，
但是对于季节时间序列还是以年作为单位。

还可以用feasts包的`ACF()`函数与`autoplot()`来绘制ACF图，
也消除了上述`acf()`函数的缺点。
需要使用tsibble类型的时间序列。
如：

```
library(tsibble)
library(feasts)

as_tsibble(d10) |>
  ACF(value) |>
  autoplot() +
  labs(title="CRSP第10分位组合月对数收益率的ACF")
```

![CRSP第10分位组合月对数收益率的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-acf-dec02d-1.png)

图3.9: CRSP第10分位组合月对数收益率的ACF

○○○○○

### 3.3.3 用单个自相关系数作白噪声检验

如果\(\{ X\_ t \}\)是独立同分布白噪声，
则\(\hat\rho\_k\)(\(k\geq 1\))近似\(\text{N}(0, 1/T)\)。
若\(H\_0\)是序列为白噪声，
取统计量

\[
t = \sqrt{T} \hat\rho\_k
\]

如果\(|t| > \text{qnorm}(1-\alpha/2)\)，
则拒绝白噪声零假设。
实际中常取\(\alpha=0.05\),
\(\text{qnorm}(1-\alpha/2) \approx 2\)，
当\(\hat\rho\_1\)超出\(\pm 2 /\sqrt{T}\)则拒绝\(H\_0\)，
有多个\(\hat\rho\_k\)超出\(\pm 2/\sqrt{T}\)也可拒绝\(H\_0\)，
有一个\(t\)统计量值很大（比如超出\(\pm 3\)）也可拒绝\(H\_0\)。

在判断\(\{ X\_t \}\)是否\(X\_t = \mu + \sum\_{j=0}^q \psi\_j \varepsilon\_{t-j}\)
这样的模型时，
根据Bartlet公式，
可取
\[
t = \frac{\hat\rho\_k}{\sqrt{\frac{1}{T} \left( 1 + 2\sum\_{j=1}^{k-1} \hat\rho\_j^2 \right)}},\ k>q
\]

当\(t\)超出\(\text{qnorm}(1-\alpha/2)\)时拒绝这样的模型。

**例3.2** 这是例[3.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#exm:tslin-crsp01) 的继续。
有研究者认为小市值股票倾向于在每年的一月份有正的收益率。

为此，用\(H\_0: \rho\_{12} = 0\)对\(H\_a: \rho\_{12} \neq 0\)的检验来验证。
如果一月份有取正值的倾向，
则相隔12个月的值会有正相关。

计算\(\hat\rho\_{12}\)的值：

```
tmp1 <- acf(d10, plot=FALSE)
r12 <- tmp1$acf[abs(tmp1$lag-12/12)<1E-10]
r12
```

```
## [1] 0.130411
```

计算\(t\)统计量的值，检验p值：

```
t12 <- sqrt(tmp1$n.used)*r12; t12
```

```
## [1] 2.962369
```

```
pv <- 2*(1 - pnorm(abs(t12))); pv
```

```
## [1] 0.003052812
```

\(p\)值小于0.05,
拒绝了\(H\_0: \rho\_{12}=0\)，
这个检验的结果支持一月份效应的存在性。

○○○○○

### 3.3.4 Ljung-Box白噪声检验

为了检验时间序列样本是否来自白噪声序列，
可以检验\(\rho\_k=0, k=1,2,\dots\)的零假设。
前面检验单个\(\rho\_k\)的做法如果针对多个进行检验就有多重检验的第一类错误增大的问题。

Box和Pierce([G. Box and Pierce 1970](#ref-Box-Pierce1970))提出了混成统计量(Portmanteau statistic)
\[
Q\_\*(m) = T \sum\_{j=1}^m \hat\rho\_j^2
\]

用来检验

\[
H\_0: \rho\_1 = \dots = \rho\_m = 0
\longleftrightarrow
H\_a: \text{不全为零}
\]

在\(\{X\_t \}\)是独立白噪声序列条件下，
\(Q\_\*(m)\)近似服从\(\chi^2(m)\)分布。
给定检验水平\(\alpha\)，
当\(Q\_\*(m) > \text{qchisq}(1-\alpha, m)\)时拒绝\(H\_0\)，
否定白噪声假设。
如果检验的序列是线性时间序列估计的残差序列，
则卡方自由度应改为\(m\)减去估计的系数个数。

Ljung和Box([Ljung and Box 1978](#ref-Ljung-Box1978))对此检验方法进行了改进。
统计量改为

\[
Q(m) = T(T+2) \sum\_{j=1}^m \frac{\hat\rho\_j^2}{T-j}
\]
在独立同分布白噪声假设下仍近似服从\(\chi^2(m)\)分布。
当\(Q(m) > \text{qchisq}(1-\alpha, m)\)时拒绝\(H\_0\)，
否定白噪声假设。
这个检验称为Ljung-Box白噪声检验。
如果检验的序列是线性时间序列估计的残差序列，
则卡方自由度应改为\(m\)减去估计的系数个数。
比如，
对ARMA(\(p,q\))模型建模的残差作白噪声检验，
卡方自由度应改为\(m - (p+q)\)。

在R软件中，
`Box.test(x, type="Ljung-Box")`执行Ljung-Box白噪声检验。
`Box.test(x, type="Box-Pierce")`执行Box-Pierce混成检验。
用`fitdf=`指定要减去的自由度个数。

R的portes包提供了多种一元和多元白噪声检验功能。

**例3.3** 检验IBM股票月收益率是否白噪声。

考虑IBM股票从1926-01到2011-09的月度收益率数据，
简单收益率和对数收益率分别考虑。

读入数据：

```
d <- read_table(
  "m-ibmsp-2611.txt", col_types=cols(
    .default=col_double(),
    date=col_date(format="%Y%m%d")))
ibm <- ts(d[["ibm"]], start=c(1926,1), frequency=12)
```

读入的是简单收益率的月度数据。
作ACF图：

```
forecast::Acf(ibm)
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-LB-IBM02-1.png)

从ACF来看月度简单收益率是白噪声。

作Ljung-Box白噪声检验，
分别取\(m=12\)和\(m=24\)：

```
Box.test(ibm, lag=12, type="Ljung-Box")
```

```
## 
##  Box-Ljung test
## 
## data:  ibm
## X-squared = 13.098, df = 12, p-value = 0.362
```

```
Box.test(ibm, lag=24, type="Ljung-Box")
```

```
## 
##  Box-Ljung test
## 
## data:  ibm
## X-squared = 35.384, df = 24, p-value = 0.0629
```

在0.05水平下均不拒绝零假设，
支持IBM月度简单收益率是白噪声的零假设。

从简单收益率计算对数收益率，
并进行LB白噪声检验：

```
Box.test(log(1 + ibm), lag=12, type="Ljung-Box")
```

```
## 
##  Box-Ljung test
## 
## data:  log(1 + ibm)
## X-squared = 12.814, df = 12, p-value = 0.3827
```

```
Box.test(log(1 + ibm), lag=24, type="Ljung-Box")
```

```
## 
##  Box-Ljung test
## 
## data:  log(1 + ibm)
## X-squared = 34.506, df = 24, p-value = 0.07607
```

在0.05水平下不拒绝零假设。

○○○○○

Box-Pierce检验和Ljung-Box检验受到\(m\)取值的影响，
建议采用\(m \approx \ln T\)，
且序列为季度、月度这样的周期序列时，
\(m\)应取为周期的整数倍。

**例3.4** 这是例[3.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#exm:tslin-crsp01)和例[3.2](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#exm:tslin-crsp02)的继续。
对CRSP最低10分位的资产组合的月简单收益率作白噪声检验。

此组合的收益率序列的ACF:

```
forecast::Acf(d10)
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2020-IAFD-tslin_files/figure-html/tslin-LB-dec04-1.png)

针对\(m=12\)和\(m=24\)作Ljung-Box白噪声检验：

```
Box.test(d10, type="Ljung-Box", lag=12)
```

```
## 
##  Box-Ljung test
## 
## data:  d10
## X-squared = 41.06, df = 12, p-value = 4.789e-05
```

```
Box.test(d10, type="Ljung-Box", lag=24)
```

```
## 
##  Box-Ljung test
## 
## data:  d10
## X-squared = 56.246, df = 24, p-value = 0.0002122
```

在0.05水平下均拒绝零假设，
认为CRSP最低10分位的投资组合的月度简单收益率不是白噪声。

○○○○○

有效市场假设认为收益率是不可预测的，
也就不会有非零的自相关。
但是，股价的决定方式和指数收益率的计算方式等可能会导致在观测到的收益率序列中有自相关性。
高频金融数据中很常见自相关性。

常见的白噪声检验还有TREVOR S. BREUSCH (1978)
和LESLIE G. GODFREY (1978)提出的拉格朗日乘子法检验（LM检验）。
零假设为白噪声，
对立假设为AR、MA或者ARMA。
参见：

* TREVOR S. BREUSCH(1978),
  Testing for Autocorrelation in Dynamic Linear Models,
  Australian Economic Papers 17, pp. 334 – 355
* LESLIE G. GODFREY(1978),
  Testing Against General Autoregressive and Moving Average Error Models When Regressors Include Lagged Dependent Variables,
  Econometrica 46 , S. 1293 – 1302

## 3.4 线性时间序列

设\(\{ X\_t \}\)是独立同分布的二阶矩有限的随机变量，
称\(\{ X\_t \}\)为**独立同分布白噪声**(white noise)。
最常用的白噪声一般假设均值为零。
如果\(\{ X\_t \}\)独立同\(\text{N}(0, \sigma^2)\)分布，
称\(\{ X\_t \}\)为高斯(Gaussian)白噪声或正态白噪声。

白噪声序列的自相关函数为零（\(\rho\_0=1\)除外）。

实际应用中如果样本自相关函数近似为零
（ACF图中都位于控制线之内或基本不超出控制线），
则可认为该序列是白噪声的样本。

R的`Box.test()`函数提供了Box-Pierce检验和Ljung-Box检验功能。
R的portes包提供了多种一元和多元白噪声检验功能。

如：IBM月度收益率可以认为是白噪声（见例[3.3](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#exm:tslin-ibm-wntest01)）；
CRSP最低10分位投资组合月度收益率不是白噪声（见例[3.4](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#exm:tslin-crsp03)）。

设\(\{ \varepsilon\_t \}\)是零均值独立同分布白噪声，
\(\text{Var}(\varepsilon\_t)=\sigma^2\),
数列\(\{ \psi\_j \}\)满足\(\sum\_j \psi\_j^2 < \infty\)，
\(\psi\_0=1\),
令
\[\begin{align}
X\_t = \mu + \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j},
t \in \mathbb Z
\tag{3.1}
\end{align}\]
则称\(\{ X\_t \}\)是（因果）线性时间序列，
\(\varepsilon\_t\)代表了在时刻\(t\)增加的变动信息，
称为**新息**(innovation)或者扰动(shock)。
因果线性时间序列满足新息\(\varepsilon\_{t+j}\)(\(j\geq 1\))与历史的\(X\_t, X\_{t-1}\)独立。
而且，
\(\{ X\_{t-1}, X\_{t-2}, \dots \}\)与
\(\{ \varepsilon\_{t-1}, \varepsilon\_{t-2}, \dots\}\)可以互相线性地表示。

这个定义可以放宽到\(\{ \varepsilon\_t \}\)是宽平稳的不相关列（宽白噪声）的情形。
这时，称[(3.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-tslin.html#eq:tslin-wnlints-lints-def)为\(\{X\_t \}\)的**Wold分解**，
称\(\{ X\_t \}\)为**纯非决定性序列**。
参见:

* 何书元(2003), 应用时间序列分析. 北京大学出版社. 第5章。
* HERMAN WOLD(1938),
  A Study in the Analysis of Stationary Time Series,
  Almquist and Wicksell, Stockholm .(博士论文)

许多弱平稳时间序列是线性时间序列，
后面讲到的AR模型、MA模型、ARMA模型都属于线性时间序列。
另外的许多弱平稳时间序列可以被线性时间序列近似。
非平稳的时间序列不是线性时间序列。

\(\{\psi\_j \}\)称为模型的\(\psi\)权重。
易见
\[
E X\_t = \mu,
\quad
\text{Var}(X\_t) = \sigma^2 \sum\_{j=0}^\infty \psi\_j^2,
\]
自协方差函数为
\[
\begin{aligned}
\gamma\_{k}
=& \text{Cov}(X\_t, X\_{t-k})
= E \left[ \left( \sum\_{i=0}^\infty \psi\_i \varepsilon\_{t-i} \right)
\left( \sum\_{i=0}^\infty \psi\_i \varepsilon\_{t-i-l} \right)\right] \\
=& E \left( \sum\_{i,j=0}^\infty \psi\_i \psi\_j \varepsilon\_{t-i} \varepsilon\_{t-j-k} \right)
= \sigma^2 \sum\_{i,j=0}^\infty \psi\_i \psi\_j \delta\_{i-j-k} \\
=& \sigma^2 \sum\_{j=0}^\infty \psi\_j \psi\_{j+k}
\end{aligned}
\]
其中\(\delta\_k\)当\(k=0\)时为1，
当\(k\neq 0\)时为0。

自相关函数为
\[
\rho\_k = \frac{\gamma\_k}{\gamma\_0}
= \frac{\sum\_{j=0}^\infty \psi\_j \psi\_{j+k}}{1 + \sum\_{j=1}^\infty \psi\_j^2},
\ k \geq 0
\]

线性时间序列模型满足\(\psi\_j \to 0\), \(j \to \infty\)，
所以历史上的扰动的影响会逐渐消失。
另外\(\rho\_k \to 0\), \(k \to \infty\)，
所以相距较远的观测之间的相关性很小。

不是所有的弱平稳时间序列都有这样的性质。
非平稳序列更是不需要满足这些性质。

## 3.5 附录：补充知识

### 3.5.1 严平稳

对时间序列\(\{ X\_t, t \in \mathbb Z \}\)，
如果对任意的\(t \in \mathbb Z\)和正整数\(n\), \(k\)，
\((X\_t, \dots, X\_{t+n-1})\)总是与
\((X\_{t+k}, \dots, X\_{t+n-1+k})\)同分布，
则称\(\{ X\_t \}\)为**严平稳**时间序列。

如果严平稳时间序列\(\{X\_t \}\)有二阶矩，
则它也是宽平稳的。

如果宽平稳时间序列\(\{X\_t \}\)的任意有限维分布都服从广义正态分布，
则\(\{X\_t \}\)也是严平稳的。

如果\(\{ \varepsilon\_t \}\)为独立同分布零均值白噪声，
方差为\(\sigma^2\)，
\(\{ \psi\_j \}\)绝对可和或者平方可和，
则线性时间序列
\[
X\_t = \mu + \sum\_{j=0}^\infty \psi\_j \varepsilon\_{t-j},
t \in \mathbb Z
\]
同时宽平稳和严平稳的时间序列。

### 3.5.2 严平稳遍历性

时间序列建模，
是从理论上无穷长度的随机变量序列\(\{ X\_t(\omega), t=\dots, -1, 0, 1, \dots , \omega \in \Omega \}\)
中获得一段观测值，
称为一条**轨道**，
如\(\{X\_1(\omega\_0), X\_2(\omega\_0), \dots, X\_T(\omega\_0) \}\)，
其中\(\omega\_0\)是概率空间\(\Omega\)中的一个结果点。
虽然\(T\)可以越来越大，
但是我们还是仅仅观测到结果空间中的一个点，
希望据此推断所有\(\omega \in \Omega\)的性质。
这需要序列\(\{ X\_t \}\)具有**遍历性**。

许多序列没有遍历性。
例如，
\(X\_t \equiv \xi\)，
其中\(\xi\)为U(0,1)随机变量，
则每条轨道是一个常数，
但不同轨道的常数值不同，
从一条轨道无法推断所有结果。
有如，设随机变量\(U\)服从\(\text{U}(0, 2\pi)\)，
\(A>0\), \(f \in (0, 2\pi)\)为常数，则时间序列
\[
X\_t = A \cos(f t + U), \ t \in \mathbb Z
\]
是宽平稳时间序列，
但其每条轨道是一条正弦曲线上的离散点，
不同轨道的相位不同，
从一条轨道无法推断所有轨道的分布情况。

如果从时间序列的一条轨道就可以推断出它的所有有限维分布，
就称其为严平稳遍历的。
这里不给出遍历性的严格定义，
仅给出一些严平稳遍历的充分条件。
可以证明，
宽平稳的正态时间序列是严平稳遍历的，
上述由零均值独立同分布白噪声产生的线性序列是严平稳遍历的。

严平稳遍历的性质：

**定理**:
如果\(\{ X\_t \}\)是严平稳遍历序列，
则

(1) 强大数律: 如果\(E|X\_1|<\infty\)则
\[
\lim\_{T\to\infty} \frac{1}{T} \sum\_{t=1}^T X\_t = E X\_1, \ \text{a.s.}
\]

(2) 对任意函数\(\phi(x\_1, x\_2, \dots, x\_m)\)，
\(Y\_t = \phi(X\_t, X\_{t-1}, \dots, X\_{t+m-1})\)也是严平稳遍历序列。

### B 参考文献

Box, GEP, and D. Pierce. 1970. “Distribution of Residual Autocorelations in Autoregressive-Integrated Moving Average Time Series Models.” *J. Of American Stat. Assoc.* 65: 1509–26.

Ljung, G., and GEP Box. 1978. “On a Measure of Lack of Fit in Time Series Models.” *Biometrika* 66: 67–72.

———. 2013. *金融数据分析导论：基于R语言*. 机械工业出版社.

何书元. 2003. *应用时间序列分析*. 北京大学出版社.