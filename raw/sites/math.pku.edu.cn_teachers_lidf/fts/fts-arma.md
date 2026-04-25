---
crawl_time: '2026-01-17 14:30:36'
framework: sphinx
title: 6 ARMA模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-arma.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 6 ARMA模型

## 6.1 ARMA模型的概念

AR模型有偏自相关函数截尾性质；
MA模型有相关函数截尾性质。
有些因果线性时间序列有与AR和MA类似的表现，
但是不能在低阶实现偏自相关函数截尾或者自相关函数截尾。

ARMA模型结合了AR和MA模型，
在对数据拟合优度相近的情况下往往可以得到更简单的模型，
而且不要求偏自相关函数截尾也不要求相关函数截尾。

ARMA(1,1)模型为
\[
X\_t = \phi\_0 + \phi\_1 X\_{t-1}
+ \varepsilon\_t + \theta\_1 \varepsilon\_{t-1} ,
\]
或
\[
X\_t - \phi\_1 X\_{t-1}
= \phi\_0 + \varepsilon\_t + \theta\_1 \varepsilon\_{t-1} ,
\]
其中\(|\phi\_1|<1\), \(|\theta\_1|<1\),
\(-\phi\_1 \neq \theta\_1\)。
\(\{\varepsilon\_t \}\)是独立同分布零均值白噪声列,
\(\varepsilon\_t\)与\(X\_{t-1}, X\_{t-2}, \dots\)独立。

一般的ARMA(\(p,q\))类似。

AR(\(p\))可以看成ARMA(\(p\), 0),
MA(\(q\))可以看成是ARMA(0, \(q\))。

在ARMA(1,1)的系数条件中，
\(|\phi\_1|<1\)是平稳解条件，
\(|\theta\_1|<1\)是可逆性条件，
\(-\phi\_1 \neq \theta\_1\)是为了模型不至于退化：
模型也可以写成
\[
(1 - \phi\_1 B) X\_t
= \phi\_0 + (1 + \theta\_1 B) \varepsilon\_t .
\]
如果不加\(-\phi\_1 \neq \theta\_1\)条件，
两边的滞后算子的多项式就可以消去。

## 6.2 ARMA模型的性质

形式地，
\[\begin{aligned}
\frac{1}{1 - \phi\_1 z} =& \sum\_{j=0}^\infty \phi\_1^j z^j, \\
\frac{1}{1 - \phi\_1 B} =& \sum\_{j=0}^\infty \phi\_1^j B^j, \\
\frac{1 + \theta\_1 z}{1 - \phi\_1 z}
=& 1 + (\phi\_1 + \theta\_1) \sum\_{j=1}^\infty \phi\_1^{j-1} z^j, \\
\frac{1 + \theta\_1 B}{1 - \phi\_1 B}
=& 1 + (\phi\_1 + \theta\_1) \sum\_{j=1}^\infty \phi\_1^{j-1} B^j,
\end{aligned}\]
于是

\[\begin{align}
X\_t =& \frac{1}{1 - \phi\_1 B} \left\{ \phi\_0 + (1 + \theta\_1 B) \varepsilon\_t \right\} \nonumber\\
=& \frac{\phi\_0}{1 - \phi\_1} + \frac{1 + \theta\_1 B}{1 - \phi\_1 B}\varepsilon\_t \nonumber\\
=& \frac{\phi\_0}{1 - \phi\_1} + \varepsilon\_t
+ (\phi\_1 + \theta\_1) \sum\_{j=1}^\infty \phi\_1^{j-1} \varepsilon\_{t-j} .
\tag{6.1}
\end{align}\]

这是因果型线性时间序列，是弱平稳的，满足上述ARMA(1,1)模型方程。
\[\begin{aligned}
E X\_t
=& \frac{\phi\_0}{1 - \phi\_1}, \\
\text{Var}(X\_t)
=& \sigma^2 \left(
1 + (\phi\_1+\theta\_1)^2 \sum\_{j=1}^\infty (\phi\_1^2)^{j-1}
\right)
= \sigma^2
\frac{1 + \theta\_1^2 + 2\phi\_1 \theta\_1}{1 - \phi\_1^2} .
\end{aligned}\]

由方差的表达式可知\(|\phi\_1|<1\)是平稳性的必要条件。

求自协方差函数和自相关函数。
因为协方差与均值无关，
而\(\phi\_0\)只影响到均值，
所以求协方差与自相关函数时可以假定\(\phi\_0=0\)。
对
\[\begin{align}
X\_t - \phi\_1 X\_{t-1}
= \varepsilon\_t + \theta\_1 \varepsilon\_{t-1} ,
\tag{6.2}
\end{align}\]
在[(6.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-arma.html#eq:arma-arma11eq2)两边乘以\(\varepsilon\_t\)，
注意\(\varepsilon\_t\)与\(X\_{t-1}\)还有\(\varepsilon\_{t-1}\)独立，
有
\[
E(X\_t \varepsilon\_t)
= \sigma^2 .
\tag{\*}
\]

在[(6.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-arma.html#eq:arma-arma11eq2)两边乘以\(X\_{t-1}\)并取期望得
\[
\gamma\_1 - \phi\_1 \gamma\_0
= \theta\_1 E(\varepsilon\_{t-1} X\_{t-1}) .
\]
由(\*)可知\(E(\varepsilon\_t X\_t) = \sigma^2\),
所以\(E(\varepsilon\_{t-1} X\_{t-1}) = \sigma^2\)，
\[
\gamma\_1 - \phi\_1 \gamma\_0 = \sigma^2 \theta\_1,
\]
\[
\gamma\_1
= \sigma^2
\frac{(\phi\_1 + \theta\_1)(1 + \theta\_1\phi\_1)}{1 - \phi\_1^2} .
\]

在[(6.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-arma.html#eq:arma-arma11eq2)两边乘以\(X\_{t-k}\)(\(k\geq 2\))并取期望，得
\[
\gamma\_k - \phi\_1 \gamma\_{k-1} = 0,
\quad
\gamma\_k = \phi\_1 \gamma\_{k-1} = \phi\_1^{k-1} \gamma\_1 .
\]
所以ARMA(1,1)的自相关函数为
\[
\rho\_k = \begin{cases}
\dfrac{(\phi\_1 + \theta\_1)(1 + \phi\_1 \theta\_1)
}{1 + 2\phi\_1 \theta\_1 + \theta\_1^2}, & k=1, \\
\phi\_1 \rho\_{k-1} = \phi\_1^{k-1} \rho\_1, & k \geq 2 .
\end{cases}
\]

所以ARMA(1,1)的ACF与AR(1)的ACF很相似，
但是从\(k=2\)处才开始负指数衰减。
与AR类似，
自相关函数不能有限步截尾。

ARMA(1,1)的偏自相关函数与MA(1)的偏自相关函数类似，
但负指数衰减从\(k=2\)开始，
也不能在有限步截尾。

总之，
ARMA(1,1)的平稳性条件与AR(1)相同，
自相关函数与偏自相关函数均不能有限步截尾
(设\(\phi\_1\neq 0\), \(\theta\_1 \neq 0\))。

## 6.3 一般ARMA模型

一般ARMA模型为
\[
X\_t
= \phi\_0 + \phi\_1 X\_{t-1} + \dots + \phi\_p X\_{t-p}
+ \varepsilon\_t
+ \theta\_1 \varepsilon\_{t-1} + \dots
+ \theta\_q \varepsilon\_{t-q} .
\]
其中\(\{ \varepsilon\_t \}\)为独立同分布零均值白噪声列，
\(\varepsilon\_t\)与\(X\_{t-1}, X\_{t-2}, \dots\)独立。
\(1 - \phi\_1 z - \dots - \phi\_p z^p\)称为特征多项式，
特征多项式的根都在单位园外，这是平稳性条件。
一般要求\(1 + \theta\_1 z + \dots + \theta\_q z^q\)的根也都在单位圆外，
这个条件称为可逆性条件。
两个多项式没有公共根，
否则同一模型可能会有不同的表示。

平稳解的均值为
\[
EX\_t
= \frac{\phi\_0}{1 - \phi\_1 - \dots - \phi\_p} .
\]

## 6.4 ARMA模型辨识

可以逐个从低阶模型尝试，
\(p+q\)越小越好，
找到AIC最小的选择，
用精确最大似然或者条件最大似然方法估计参数。
对残差进行白噪声检验以验证模型是否充分。

R的forecast包提供了一个`auto.arima()`函数，
可以自动进行模型选择。
TSA包提供了一个`armasubsets()`函数用于模型选择。

**例6.1** 考虑3M公司股票从1946年2月到2008年12月的月对数收益率，
共有755个观测。

```
d <- read_table(
  "m-3m4608.txt",
  col_types=cols(.default=col_double(),
                 date=col_date(format="%Y%m%d")))
mmm <- xts(log(1 + d[["rtn"]]), d$date)
rm(d)
tclass(mmm) <- "yearmon"
ts.3m <- ts(coredata(mmm), start=c(1946,2), frequency=12)
head(ts.3m)
```

```
##          Series 1
## [1,] -0.081125460
## [2,]  0.018421282
## [3,] -0.105360516
## [4,]  0.190518702
## [5,]  0.005114897
## [6,]  0.073743834
```

```
plot(ts.3m, main="3M Monthly Log Return")
abline(h=0, col="gray")
```

![3M公司月对数收益率](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2023-IAFD-arma_files/figure-html/arma-3m-exm01-1.png)

图6.1: 3M公司月对数收益率

ACF图形：

```
forecast::Acf(ts.3m, main="")
```

![3M公司月对数收益率的ACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2023-IAFD-arma_files/figure-html/arma-3m-exm02-1.png)

图6.2: 3M公司月对数收益率的ACF

ACF很接近于白噪声。

PACF图形：

```
pacf(ts.3m, main="")
```

![3M公司月对数收益率的PACF](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2023-IAFD-arma_files/figure-html/arma-3m-exm03-1.png)

图6.3: 3M公司月对数收益率的PACF

PACF也比较接近于白噪声但是有比较多的超出界限的值，
尽管超出量不大。

用forecasts包的`auto.arima()`函数定阶：

```
forecast::auto.arima(
  ts.3m, max.p = 6, max.q = 6, 
  max.P = 1, max.Q = 1)
```

```
## Series: ts.3m 
## ARIMA(3,0,1)(1,0,1)[12] with non-zero mean 
## 
## Coefficients:
##          ar1      ar2      ar3      ma1    sar1     sma1    mean
##       0.0453  -0.0285  -0.0837  -0.1124  0.5319  -0.4435  0.0103
## s.e.  0.3146   0.0417   0.0387   0.3147  0.2885   0.3049  0.0023
## 
## sigma^2 = 0.003998:  log likelihood = 1016.63
## AIC=-2017.25   AICc=-2017.06   BIC=-1980.24
```

`auto.arima()`函数选择了一个季节ARMA模型，
记为ARIMA\((3,0,1)(1,0,1)\_{12}\)。
其中的\((1,0,1)\_{12}\)部分属于季节模型部分，
将在[8](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-seas.html#fts-seas)讲授。
参数`max.p`, `max.q`, `max.P`, `max.Q`分别指定要考虑的模型各个最大阶数。

使用`forecast::Arima()`估计模型：

```
forecast::Arima(ts.3m, order = c(3, 0, 1),
  seasonal = list(
    order = c(1, 0, 1), period = 12))
```

```
## Series: ts.3m 
## ARIMA(3,0,1)(1,0,1)[12] with non-zero mean 
## 
## Coefficients:
##          ar1      ar2      ar3      ma1    sar1     sma1    mean
##       0.0453  -0.0285  -0.0837  -0.1124  0.5319  -0.4435  0.0103
## s.e.  0.3146   0.0417   0.0387   0.3147  0.2885   0.3049  0.0023
## 
## sigma^2 = 0.003998:  log likelihood = 1016.63
## AIC=-2017.25   AICc=-2017.06   BIC=-1980.24
```

`forecast::Arima()`是基本R中`stats::arima()`函数的改进版本，
使用状态空间模型卡尔曼滤波来进行精确最大似然估计。
状态空间模型能够自然地处理缺失值问题，
所以我们测试有缺失时的情况：

```
ts.3m.miss <- ts.3m
ts.3m.miss[301:350] <- NA
plot(ts.3m.miss)
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/2023-IAFD-arma_files/figure-html/arma-3m-exm08-1.png)

```
forecast::Arima(ts.3m.miss, order = c(3, 0, 1),
  seasonal = list(
    order = c(1, 0, 1), period = 12))
```

```
## Series: ts.3m.miss 
## ARIMA(3,0,1)(1,0,1)[12] with non-zero mean 
## 
## Coefficients:
##          ar1      ar2      ar3      ma1    sar1     sma1    mean
##       0.1638  -0.0369  -0.0977  -0.2174  0.7152  -0.6368  0.0107
## s.e.  0.2187   0.0400   0.0407   0.2176  0.2258   0.2481  0.0024
## 
## sigma^2 = 0.003879:  log likelihood = 960.01
## AIC=-1904.02   AICc=-1903.82   BIC=-1867
```

对中间有一部分数据缺失的时间序列，
`forecast::Arima`也成功进行了ARMA模型建模估计。

## 6.5 ARMA模型预测

ARMA(\(p,q\))的预测与AR和MA的预测都有关系。
对AR部分仍是递推地向前预测，
对MA部分，
需要估计新息\(\varepsilon\_t\)的值。

超前一步预测：
\[
\hat x\_h(1)
= \phi\_0 + \sum\_{j=1}^p \phi\_j x\_{h+1-j}
+ \sum\_{j=1}^q \theta\_j \hat\varepsilon\_{h+1-j} .
\]
其中\(\hat\varepsilon\_i = E(\varepsilon\_i | X\_1, \dots, X\_h)\)。

对超前\(k\)步预测有
\[
\hat x\_h(k) = \phi\_0 + \sum\_{j=1}^p \phi\_j \hat x\_{h+k-j}
+ \sum\_{j=1}^q \theta\_j \hat\varepsilon\_{h+k-j} .
\]
其中\(h+k-j \leq h\)时\(\hat x\_{h+k-j} = x\_{h+k-j}\),
\(h+k-j>h\)时\(\hat\varepsilon\_{h+k-j}=0\)。
预测误差为\(e\_h(k) = x\_{h+k} - \hat x\_{h+k}\)。

## 6.6 ARMA模型的三种表示

设ARMA(\(p,q\))模型的系数满足平稳性条件与可逆性条件。

第一种表示：
\[
(1 - \phi\_1 B - \dots - \phi\_p B^p) X\_t
= \phi\_0
+ (1 + \theta\_1 B + \dots + \theta\_q B^q) \varepsilon\_t .
\]

### 6.6.1 ARMA模型的MA表示

用滞后算子的性质，
令\(P(z) = 1 - \sum\_{j=1}^p \phi\_p z^j\),
\(Q(z) = 1 + \sum\_{j=1}^q \theta\_q z^j\)，
则
\[
\Psi(z)
= \frac{Q(z)}{P(z)}
= \sum\_{j=0}^\infty \psi\_j z^j,
\]
其中\(\{ \psi\_j \}\)绝对可和（实际上，\(j\to\infty\)时\(\psi\_j\)以负指数速度衰减）。
所以
\[
X\_t = \mu + \Psi(B) \varepsilon\_t
= \mu
+ \sum\_{j=0}^\infty
\psi\_j \varepsilon\_{t-j},
\ t \in \mathbb Z .
\]
这称为ARMA模型的MA表示或者Wold表示。
\(\mu=EX\_t = \phi\_0/P(1) = \phi\_0 / (1 - \phi\_1 - \dots - \phi\_p)\)。
\(\psi\_0=1\)。
系数\(\{ \psi\_j, j=0,1,\dots \}\)称为ARMA模型的脉冲响应函数或者Wold系数。
\(\psi\_j\)是脉冲响应函数的意义是，
如果\(\epsilon\_t=1\)，
它将给\(X\_{t+j}\)施加一个\(\psi\_j\)的增量影响。

\(\psi\_j\)随\(j\to+\infty\)以负指数速度衰减，
这样的性质是合理的，
即一个时刻发生的事情的影响应该随历史向前推进而迅速降低。

MA表示对估计多步预测的均方误差有影响。
理论上

\[\begin{align}
E(X\_{h+k} | \mathscr F\_h)
= \mu + \sum\_{i=0}^\infty \psi\_{i+k} \varepsilon\_{h-i} .
\tag{6.3}
\end{align}\]

其中\(\mathscr F\_h\)是包含\(\{ X\_s, s \leq t \}\)的最小\(\sigma\)代数，
表示到时刻\(h\)为止的观测信息，
\(E(\varepsilon\_{h+k} | \mathscr F\_h)=0\), \(k \geq 1\)。
于是理论的预测误差为
\[
e\_h(k)=
X\_{h+k} - E(X\_{h+k} | \mathscr F\_h)
= \sum\_{j=0}^{k-1} \psi\_j \varepsilon\_{h+k-j} .
\]
均方误差为
\[
\text{Var}(e\_h(k))
= \sigma^2 \sum\_{j=0}^{k-1} \psi\_j^2 .
\]

因为\(\{ \psi\_j \}\)绝对可和，
所以[(6.3)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/fts-arma.html#eq:arma-wold-predk)中的级数当\(k\to\infty\)时趋于0，
于是长期预测值趋于\(\mu\)，
这称为均值反转。
当\(k\to\infty\)是多步预测的均方误差趋于
\(\sigma^2 \sum\_{j=0}^{\infty} \psi\_j^2 = \text{Var}(X\_t)\)，
即用均值作为预测的均方误差。

### 6.6.2 ARMA模型的AR表示

当平稳性与可逆性条件都成立时，
令
\[
\pi(z) = \frac{P(z)}{Q(z)},
\]
则
\[
\pi(z) = \sum\_{j=0}^\infty \pi\_j z^j,
\]
\(\{ \pi\_j \}\)绝对可和，
当\(j\to\infty\)时\(\pi\_j\)以负指数速度收敛到0。
\(\pi\_0=1\)。
于是ARMA模型有如下的AR表示

\[
\pi(B) X\_t
= \frac{\phi\_0}{Q(1)} + \varepsilon\_t .
\]
或
\[
X\_t
= \tilde\phi\_0
- \pi\_1 X\_{t-1} - \pi\_2 X\_{t-2} - \dots
+ \varepsilon\_t .
\]
其中\(\tilde\phi\_0 = \phi\_0/Q(1) = \phi\_0 / (1 + \theta\_1 + \dots + \theta\_q)\)。
这称为ARMA的AR表示或者长阶自回归形式。
这说明平稳可逆ARMA序列可以用自回归模型近似表示。
\(\pi\_j\)称为ARMA模型的\(\pi\)权重。

## 6.7 附录

文献和历史：

* EVGENIJ EVGENIEVICH SLUTZKY和GEORGE UDNY YULE在19世纪初提出了MA模型和AR模型。
* HERMAN WOLD(1938),
  A Study in the Analysis of Stationary Time Series,
  Almquist and Wicksell, Stockholm. 博士论文。
  完善了线性时间序列模型。
* GEORGE E.P. BOX and GWILYM M. JENKINS(1970),
  Time Series Analysis: Forecasting and Control,
  Holden Day, San Francisco et al.; 2nd enlarged edition 1976.
  本书给出了应用线性时间序列模型的理论与方法。
* 下面的文献提出，
  一元时间序列模型短期预测常常优于大量经济变量的联立方程的预测：
  CLIVE W.J. GRANGER and PAUL NEWBOLD(1975),
  Economic Forecasting: The Atheist’s Viewpoint, in:
  G.A. RENTON (ed.), Modelling the Economy, Heinemann, London
  pp. 131 – 148.