---
crawl_time: '2026-01-17 14:31:04'
framework: sphinx
title: 22 随机波动率模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-stochvol.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 22 随机波动率模型

本章内容来自自([Tsay 2013](#ref-Tsay13:IAFD))§4.13和§4.14内容。

## 22.1 随机波动率模型

前面的波动率方程中\(\sigma\_t^2 = \text{Var}(a\_t | F\_{t-1})\)都是被\(\sigma\_{t-1}, \dots\)和
\(a\_{t-1}, \dots\)完全决定。
另一种方法是假定\(\sigma\_t^2\)的模型本身有新息，
这样的模型称为随机波动率(Stochastic Volatility, SV)模型。
模型写成
\[\begin{aligned}
a\_t = \sigma\_t \varepsilon\_t,
\quad
(1 - \alpha\_1 B - \dots - \alpha\_m B^m) \ln \sigma\_t^2 = \alpha\_0 + v\_t .
\end{aligned}\]
其中\(\sigma\_t^2\)取对数是为了取消系数必须为非负的限制。
\(\{ \varepsilon\_t \}\)独立同标准正态分布，
\(\{ v\_t \}\)独立同N(\(0, \sigma\_v^2\))分布，
\(\{ \varepsilon\_t \}\)和\(\{ v\_t \}\)相互独立。
\(\alpha\_i\)为常数，
特征多项式\(1 - \alpha\_1 z - \dots - \alpha\_m z^m\)根都在单位圆外。
记\(\xi\_t = \ln\sigma\_t^2\)，
则\(\{ \xi\_t \}\)是一个严平稳AR(\(m\))序列。

加入\(v\_t\)新息后，
收益率\(r\_t\)的一个新息\(a\_t\)就包含了\(\varepsilon\_t\)和\(v\_t\)两个新息，
这增加了模型的自由度，
但是使得从\(r\_t\)数据估计模型参数变得更加困难，
需要使用Kalman滤波或者随机模拟方法计算拟似然估计。

当\(m=1\)时，有
\[\begin{aligned}
\ln\sigma\_t^2 \sim& \text{N}\left( \frac{\alpha\_0}{1-\alpha\_1},
\frac{\sigma\_v^2}{1 - \alpha\_1^2} \right)
= \text{N}(\mu\_h, \sigma\_h^2) , \\
E a\_t^2 =& \exp\left( \mu\_h + \frac12 \sigma\_h^2 \right) , \\
E a\_t^4 =& 3 \exp\left( 2 \mu\_h^2 + 2 \sigma\_h^2 \right) , \\
\rho(a\_t^2, a\_{t-i}^2)
=& \frac{e^{\sigma\_h^2 \alpha\_1^i} - 1}{3 e^{\sigma\_h^2} - 1} .
\end{aligned}\]

SV模型经常在拟合上有所改善，
但是波动率的样本外预测时好时坏。

## 22.2 长记忆随机波动率模型

对资产收益率的实证分析发现，
收益率本身没有长记忆性，
但是其平方序列或者绝对值序列的ACF往往衰减很慢。
前面GARCH类模型的建模中\(\sigma\_{t-1}^2\)的系数很接近于1，
也提示有长记忆。

下面对1962年到2003年标普500指数和IBM股票的日对数收益率序列的绝对值作ACF，
可以看到长记忆现象存在。

```
da <- read_table(
  "d-ibmvwewsp5-6203.txt",
  col_types=cols(
    .default=col_double(),
    date=col_date(format="%Y%m%d")))
xts.ibm <- xts(log(1 + da[,-1]), da[["date"]])
ibm <- coredata(xts.ibm)[,"ibm"]
sp5 <- coredata(xts.ibm)[,"sp5"]
```

标普500指数日对数收益率绝对值的ACF：

```
np <- 200; nt <- length(sp5)
tmpa <- acf(abs(sp5), lag.max=np, main="", plot=FALSE)
plot(seq(np), tmpa$acf[2:(np+1)], type="h", 
  xlab="Lag", ylab="acf", ylim=c(-0.05, 0.3))
abline(h=c(2,-2)/sqrt(nt), lty=2, col="blue")
```

![标普500指数日对数收益率绝对值的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/3035-stochvol_files/figure-html/stochvol-lm-ibmsp-acf01-1.png)

图22.1: 标普500指数日对数收益率绝对值的ACF

IBM股票日对数收益率绝对值的ACF：

```
np <- 200; nt <- length(sp5)
tmpa <- acf(abs(ibm), lag.max=np, main="", plot=FALSE)
plot(seq(np), tmpa$acf[2:(np+1)], type="h", 
  xlab="Lag", ylab="acf", ylim=c(-0.05, 0.3))
abline(h=c(2,-2)/sqrt(nt), lty=2, col="blue")
```

![IBM股票日对数收益率绝对值的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/3035-stochvol_files/figure-html/stochvol-lm-ibmsp-acf02-1.png)

图22.2: IBM股票日对数收益率绝对值的ACF

简单的长记忆随机波动率(LMSV)模型可以写成
\[\begin{aligned}
a\_t = \sigma\_t \varepsilon\_t,
\quad \sigma\_t = \sigma e^{\frac12 u\_t},
\quad (1 - B)^d u\_t = \eta\_t .
\end{aligned}\]
其中\(\sigma>0\)，
\(\{\varepsilon\_t \}\)和\(\{\eta\_t\}\)是两个相互独立的独立同分布高斯白噪声列，
\(\varepsilon\_t \sim \text{N}(0,1)\),
\(\eta\_t \sim \text{N}(0, \sigma\_\eta^2)\),
\(0<d<0.5\)。
长记忆来源于分数差分\((1-B)^d\)，
这使得\(u\_t\)的ACF以负幂速度衰减而非负指数速度衰减。

对LMSV有
\[\begin{aligned}
\ln a\_t^2
=& \ln(\sigma\_t^2 \varepsilon\_t^2)
= \ln \sigma^2 + u\_t + \ln\varepsilon\_t^2 \\
=& (\ln \sigma^2 + E \ln\varepsilon\_t^2)
+ u\_t + (\ln\varepsilon\_t^2 - E \ln\varepsilon\_t^2) \\
=& \mu + u\_t + e\_t .
\end{aligned}\]
其中\(u\_t\)是一个长记忆的平稳高斯时间序列，
\(e\_t\)是一个非高斯的独立同分布白噪声列。

LMSV估计比较复杂，
分数参数\(d\)可以用拟最大似然估计法或者回归方法估计。
标普500指数成份股日收益率平方的对数序列的\(d\)估计的中位数是0.38。
同一行业的股票的长记忆成分往往相同。

### B 参考文献

———. 2013. *金融数据分析导论：基于R语言*. 机械工业出版社.