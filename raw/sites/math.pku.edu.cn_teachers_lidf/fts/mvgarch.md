---
crawl_time: '2026-01-17 14:30:55'
framework: sphinx
title: 29 多元波动率模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/mvgarch.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 29 多元波动率模型

参考：

* ([Tsay 2014](#ref-Tsay14:MTSA)) 第7章。

## 29.1 介绍

设\(\boldsymbol z\_t\)是多元平稳序列，
令\(\mathscr F\_t = \sigma(\boldsymbol z\_t, \boldsymbol z\_{t-1}, \dots)\)。
令\(\Sigma\_t = \text{Var}(\boldsymbol z\_t | \mathscr F\_{t-1})\)，
称为\(\{\boldsymbol z\_t \}\)序列的波动率矩阵。
多元波动率建模困难有：维数增长过快，
\(k\)维的序列，
\(\Sigma\_t\)有\(k(k+1)/2\)个自由参数；
需要保证\(\Sigma\_t\)正定，
这就不能将\(\Sigma\_t\)看成一个普通向量建模。

将\(\boldsymbol z\_t\)分解为
\[
\boldsymbol z\_t
= \boldsymbol \mu\_t + \boldsymbol a\_t,
\]
其中\(\boldsymbol\mu\_t = E(\boldsymbol z\_t | \mathscr F\_{t-1})\)，
\(\boldsymbol a\_t\)是新息列。
\(\boldsymbol a\_t\)是序列不相关的，
但并不一定是独立的。

设
\[
\boldsymbol a\_t
= \Sigma\_t^{1/2} \boldsymbol\epsilon\_t,
\]
其中\(\{\epsilon\_t\}\)独立同\((\boldsymbol 0, I)\)分布，
设其四阶矩有限。

可以将\(\Sigma\_t\)用其特征值分解\(\Sigma\_t = P\_t \Lambda\_t P\_t^T\)表示，
其中\(\Lambda\_t\)是\(\Sigma\_t\)特征值组成的对角阵，
\(P\_t\)是各个单位特征向量组成的正交阵；
也可以用Cholesky分解\(\Sigma\_t = L\_t L\_t^T\)或者LDL分解\(\Sigma\_t = L\_t D\_t L\_t^T\)表示。

仍用均值方程表示\(\boldsymbol\mu\_t\)的模型，
用波动率方程表示\(\Sigma\_t\)的模型。

## 29.2 ARCH效应检验

设\(\boldsymbol a\_t\)为\(k\)维的新息序列。
类似于一元情形，
可以检验\(\boldsymbol a\_t^2\)是否白噪声。
可以用Ljung-Box统计量
\[
Q\_k^\*(m)
= T^2 \sum\_{i=1}^m
\frac{1}{T-i}
\boldsymbol b\_i^T
(\hat{\boldsymbol\rho}\_0^{-1}
\otimes \hat{\boldsymbol\rho}\_0^{-1})
\boldsymbol b\_i,
\]
其中\(m\)是滞后阶数，
\(\hat{\boldsymbol\rho}\_j\)是\(\boldsymbol a\_t^2\)的滞后\(j\)的互相关系数矩阵估计，
\(\boldsymbol b\_i = \text{vec}(\hat{\boldsymbol\rho}\_i^T)\)，
vec表示将矩阵按列拉直为向量，
\(\otimes\)是矩阵的Kronecher积。
如果没有ARCH效应，
\(Q\_k^\*(m)\)渐近服从\(\chi^2(k^2 m)\)分布。

另一种类似方法是定义
\[
e\_t
= \boldsymbol a\_t^T \Sigma^{-1}
\boldsymbol a\_t - k,
\]
检验\(e\_t\)是否白噪声。
\(\Sigma\)用估计值代替，
计算\(e\_t\)的自相关系数\(\hat\rho\_i\)，
令
\[
Q^\*(m)
= T(T+2)
\sum\_{i=1}^m \frac{1}{T-i} \hat\rho\_i^2 ,
\]
无ARCH效应时\(Q^\*(m)\)渐近服从\(\chi^2(m)\)分布。

因为金融数据经常有厚尾分布，
使得上述近似在中小样本效果不好，
可以计算\(e\_t\)序列的秩统计量\(R\_t\)，
然后计算\(R\_t\)的自相关系数\(\tilde\rho\_i\)，
使用统计量
\[
Q\_R(m)
= \sum\_{i=1}^m \frac{[\tilde\rho\_i - E \tilde\rho\_i]^2}{
\text{Var}(\tilde\rho\_i)},
\]
其中
\[\begin{aligned}
E \tilde\rho\_i
=& -(T-i)[T(T-1)], \\
\text{Var}(\tilde\rho\_i)
=& \frac{5T^4 - (5i+9)T^3 + 9(i-2)T^2
+ 2i(5i+8)T + 16i^2}{
5(T-1)^2 T^2 (T+1)},
\end{aligned}\]
无ARCH效应时\(Q\_R(m)\)渐近服从\(\chi^2(m)\)分布。

模拟研究发现基于\(e\_t\)的秩统计量的分布近似较好。

MTS包的`MarchTest()`函数进行多元的ARCH效应检验。

## 29.3 模型最大似然估计

设模型的参数为\(\boldsymbol\theta\)。
如果\(\boldsymbol\epsilon\_t\)为标准多元正态分布，
则似然函数为(相差一个常数项)
\[
\ell(\boldsymbol\theta)
= \sum\_{t=1}^T \ell\_t(\boldsymbol\theta),
\quad
\ell\_t(\boldsymbol\theta)
= -\frac{1}{2}[\log(|\Sigma\_t|)
+ \boldsymbol a\_t^T \Sigma\_t^{-1} \boldsymbol a\_t] .
\]
其中\(\Sigma\_t\)的形式依赖于具体的多元条件异方差模型。

也可以针对\(\boldsymbol\epsilon\_t\)的其它分布计算似然函数，
另一种方法是，
即使\(\boldsymbol\epsilon\_t\)不服从标准多元正态分布，
但似然函数仍按上面正态情形计算，
这种方法属于拟最大似然估计(QMLE, quasi MLE)。
在适当正则性条件下，
QMLE估计服从中心极限定理，
收敛到真实参数值。

## 29.4 指数加权平均模型

考虑\(\Sigma\_t\)的如下简单模型：
\[
\hat\Sigma\_t
= \lambda \hat\Sigma\_{t-1}
+ (1-\lambda)
\hat{\boldsymbol a}\_{t-1}
\hat{\boldsymbol a}\_{t-1}^T .
\]
称其为多元条件异方差的指数加权平均(EWMA)模型。
\(0 < \lambda < 1\)代表了\(\hat\Sigma\_t\)的持续性。
\(\lambda\)可以预先取定，
对金融数据\(\hat\lambda \approx 0.96\)常常比较合适，
也可以用QMLE方法估计\(\lambda\)值。
\(\hat\Sigma\_t\)的初值\(\hat\Sigma\_0\)可以采用\(\hat{\boldsymbol a}\_t\)的样本协方差阵。
这种模型很简单，
能保证\(\hat\Sigma\_t\)正定，
但是模型拟合效果不够好。

**例29.1** 考虑CRSP百分之一，百分之二，百分之五资产组合从1961年1月到2011年9月的对数收益率建模。

读入数据：

```
da <- readr::read_table("m-dec125910-6111.txt")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   date = col_double(),
##   dec1 = col_double(),
##   dec2 = col_double(),
##   dec5 = col_double(),
##   dec9 = col_double(),
##   dec10 = col_double()
## )
```

```
rtn <- log(1 + as.matrix(da[,c("dec1", "dec2", "dec5")]))
```

用VAR(1)作为均值模型：

```
library(MTS)
mod1 <- VAR(rtn)
```

```
## Constant term: 
## Estimates:  0.006376978 0.007034631 0.007342962 
## Std.Error:  0.001759562 0.001950008 0.002237004 
## AR coefficient matrix 
## AR( 1 )-matrix 
##        [,1]  [,2]     [,3]
## [1,] -0.194 0.224  0.00836
## [2,] -0.232 0.366 -0.04186
## [3,] -0.313 0.452  0.00238
## standard error 
##       [,1]  [,2]  [,3]
## [1,] 0.108 0.160 0.101
## [2,] 0.120 0.177 0.111
## [3,] 0.138 0.204 0.128
##   
## Residuals cov-mtx: 
##             [,1]        [,2]        [,3]
## [1,] 0.001814678 0.001859113 0.001962277
## [2,] 0.001859113 0.002228760 0.002420858
## [3,] 0.001962277 0.002420858 0.002933081
##   
## det(SSE) =  1.712927e-10 
## AIC =  -22.45809 
## BIC =  -22.39289 
## HQ  =  -22.43273
```

```
at <- mod1$residuals
```

对VAR(1)的多元残差序列作多元条件异方差检验：

```
MarchTest(at)
```

```
## Q(m) of squared series(LM test):  
## Test statistic:  244.7878  p-value:  0 
## Rank-based Test:  
## Test statistic:  215.215  p-value:  0 
## Q_k(m) of squared series:  
## Test statistic:  176.9811  p-value:  1.252294e-07 
## Robust Test(5%) :  155.2633  p-value:  2.347499e-05
```

结果表明存在条件异方差性。

建立多元条件异方差的EWMA模型，
参数`lambda`取负值表示用QMLE估计\(\lambda\)值：

```
mod1b <- EWMAvol(at, lambda = -0.1)
```

```
## 
## Coefficient(s):
##         Estimate  Std. Error  t value Pr(>|t|)    
## lambda   0.96427     0.00549    175.6   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

提取估计的\(\hat\Sigma\_t\)序列，
并进行模型诊断：

```
Sigma_t <- mod1b$Sigma.t
mod1c <- MCHdiag(at, Sigma_t)
```

```
## Test results:  
## Q(m) of et: 
## Test and p-value:  59.5436 4.421175e-09 
## Rank-based test: 
## Test and p-value:  125.0929 0 
## Qk(m) of epsilon_t: 
## Test and p-value:  189.5403 4.518401e-09 
## Robust Qk(m):  
## Test and p-value:  228.234 5.57332e-14
```

模型诊断结果说明多元条件异方差EWMA模型并不合适。

## 29.5 DCC模型

### 29.5.1 模型分解

直接对\(\Sigma\_t\)建模有参数个数过多问题。
一种简化模型是动态条件相关(DCC, dynamic conditional correlation)模型。
设\(\Sigma\_t = (\sigma\_{ij,t})\_{k \times k}\)。
令\(D\_t\)是\(\Sigma\_t\)对应的标准差组成的对角阵，
即\(D\_t = \text{diag}(\sqrt{\sigma\_{11,t}}, \sqrt{\sigma\_{22,t}}, \dots, \sqrt{\sigma\_{kk,t}})\)，
则\(\Sigma\_t\)对应的相关系数矩阵为
\[
\rho\_t = D\_t^{-1} \Sigma\_t D\_t^{-1} .
\]
\(\rho\_t\)有\(k(k-1)/2\)个自由参数。
DCC将条件方差阵的建模分解为关于方差\(\sigma\_{ii,t}\)的建模，
与关于相关系数的建模两步。
\(\sigma\_{ii,t}\)的部分可以用一元的GARCH模型建模，
这样得到的基于第\(i\)分量的历史的条件方差，
而不是基于所有历史的条件方差，
会有细微的理论差别。

令\(\boldsymbol\eta\_t = (\eta\_{1t}, \dots, \eta\_{kt})^T\),
其中\(\eta\_{it} = a\_{it} / \sqrt{\sigma\_{ii,t}}\)是新息各个分量的标准化。
则\(\rho\_t\)是\(\boldsymbol\eta\_t\)的条件方差阵。
考虑\(\rho\_t\)的模型。

### 29.5.2 两种条件相关系数模型

Engle(2002)提出了\(\rho\_t\)的如下模型：
\[\begin{aligned}
\rho\_t =& J\_t Q\_t J\_t, \\
Q\_t =& (1 - \theta\_1 - \theta\_2) \bar Q
+ \theta\_1 Q\_{t-1}
+ \theta\_2 \boldsymbol\eta\_{t-1} \boldsymbol\eta\_{t-1}^T, \\
J\_t =& \text{diag}(q\_{11,t}^{-1/2}, \dots, q\_{kk,t}^{-1/2}) .
\end{aligned}\]
其中\(\bar Q\)是\(\boldsymbol\eta\_t\)的无条件方差阵，
\(\theta\_1>0\), \(\theta\_2>0\)，\(\theta\_1 + \theta\_2 < 1\)。
模型决定了\(Q\_t\)是正定阵。
\(J\_t\)仅用于将\(Q\_t\)转换成相关系数矩阵。
模型仅有两个参数\(\theta\_1\)和\(\theta\_2\)。

Tse和Tsui(2002)提出了\(\rho\_t\)的另一种模型：
\[
\rho\_t
= (1 - \theta\_1 - \theta\_2) \bar\rho
+ \theta\_1 \rho\_{t-1}
+ \theta\_2 \psi\_{t-1},
\]
其中\(\psi\_{t-1}\)是\(\hat{\boldsymbol\eta}\_{t-1}, \dots, \hat{\boldsymbol\eta}\_{t-m}\)的样本相关系数矩阵。
Engle的模型相当于\(m=1\)的情形。
Tse和Cui的模型不需要关于\(\rho\_t\)重新转换为相关系数矩阵，
其做法保证了模型得到\(\rho\_t\)是相关系数矩阵，
但需要选择\(m\)的取值。\(m\)越大，
\(\rho\_t\)的变化越光滑。

DCC模型的\(\rho\_t\)部分仅包含两个未知参数，
这既是其优点，
也是一个弱点。
优点是模型容易估计；
弱点是模型的局限性较大，
而且也不容易验证数据是否满足这种模型。

### 29.5.3 建模步骤

一种实用的DCC建模步骤如下：

1. 用VAR(\(p\))作为均值模型建模，
   获得残差\(\hat{\boldsymbol a}\_t\)作为条件方差模型的输入。
2. 用一元GARCH模型对\(\boldsymbol a\_t\)的每一个分量分别建立条件方差模型。
   设估计的条件方差为\(\hat h\_{it}\)，
   令\(\hat\sigma\_{ii,t} = \hat h\_{it}\)。
3. 计算标准化残差\(\hat\eta\_{it} = \hat a\_{it} / \sqrt{\hat\sigma\_{ii,t}}\)，
   然后对\(\hat{\boldsymbol\eta}\_t\)建立关于\(\hat\rho\_t\)的DCC模型。

\(\boldsymbol\eta\_t\)的条件分布可以是标准多元正态分布，
也可以是标准多元t分布。

### 29.5.4 例子

考虑1961年到2011年IBM股票、标普指数、可口可乐股票的月度对数收益率序列。
设\(\boldsymbol z\_t\)为此3元时间序列。
均值方程取为常数，\(\hat{\boldsymbol\mu}\_t = \hat{\boldsymbol\mu}\)。
新息序列为\(\hat{\boldsymbol a}\_t = \boldsymbol z\_t - \hat{\boldsymbol\mu}\_t\)。

```
da <- readr::read_table("m-ibmspko-6111.txt")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   date = col_double(),
##   ibm = col_double(),
##   sp = col_double(),
##   ko = col_double()
## )
```

```
rtn <- log(1 + as.matrix(da[,c("ibm", "sp", "ko")]))
hat_mu <- colMeans(rtn); hat_mu
```

```
##         ibm          sp          ko 
## 0.007727740 0.005023909 0.010595213
```

使用MTS包建模，
首先用`dccPre`建立各个分量的一元GARCH(1,1)条件方差模型，
条件分布取为正态分布：

```
library(MTS)
mod2 <- dccPre(rtn, include.mean=TRUE)
```

```
## Sample mean of the returns:  0.00772774 0.005023909 0.01059521 
## Component:  1 
## Estimates:  0.000419 0.126738 0.788309 
## se.coef  :  0.000162 0.035404 0.055645 
## t-value  :  2.593451 3.579734 14.16673 
## Component:  2 
## Estimates:  9e-05 0.127725 0.836053 
## se.coef  :  4.1e-05 0.03084 0.031723 
## t-value  :  2.20126 4.141593 26.35484 
## Component:  3 
## Estimates:  0.000256 0.098706 0.830358 
## se.coef  :  8.5e-05 0.022361 0.03344 
## t-value  :  3.015323 4.414111 24.83091
```

结果相当于
\[\begin{aligned}
\sigma\_{11,t}
=& 0.000419 + 0.1267 a\_{1,t-1}^2 + 0.7883 \sigma\_{11,t-1}; \\
\sigma\_{22,t}
=& 0.00009 + 0.1277 a\_{2,t-1}^2 + 0.8361 \sigma\_{22,t-1}; \\
\sigma\_{33,t}
=& 0.000256 + 0.0987 a\_{3,t-1}^2 + 0.8304 \sigma\_{33,t-1} .
\end{aligned}\]

然后，
计算标准化的\(\hat{\boldsymbol\eta}\_t = (\hat\eta\_{1t}, \hat\eta\_{2t}, \hat\eta\_{3t})^T\)序列，
\(\hat\eta\_{jt} = \hat a\_{jt} / \sqrt{\sigma\_{ii,t}}\)，
以及一元的条件方差估计：

```
std_innov <- mod2$sresi
vol_marg <- mod2$marVol
```

用`dccFit()`函数以标准化\(\hat{\boldsymbol\eta}\_t\)序列为输入估计关乎\(\hat\rho\_t\)的DCC模型，
采用Tse和Tsui的模型，
取\(m=k+1=4\)，
条件分布取多元t分布：

```
mod2b <- dccFit(std_innov)
```

```
## Estimates:  0.8087993 0.04027417 7.959072 
## st.errors:  0.1491731 0.02259899 1.135866 
## t-values:   5.421883 1.782122 7.007054
```

估计的模型公式为：

\[
\hat\rho\_t
= (1 - 0.8088 - 0.0403) \bar\rho
+ 0.8088 \hat\rho\_{t-1} + 0.0403 \psi\_{t-1} .
\]
t分布自由度估计为\(7.96\)。

`mod1b$rho.t`给出了拟合的\(\hat\rho\_t\)序列，
用一个\(T \times k^2\)矩阵存放，
每一行是一个\(\hat\rho\_t\)的按列拉直向量。

### 29.5.5 其它软件包

rmgarch包的`dccspec()`和`dccfit()`可以用来建立DCC模型。
此包已转为tsmarch包。

```
library(rmgarch)
```

```
## Warning: package 'rmgarch' was built under R version 4.4.3
```

```
## Loading required package: rugarch
```

```
## Loading required package: parallel
```

```
## 
## Attaching package: 'rugarch'
```

```
## The following object is masked from 'package:purrr':
## 
##     reduce
```

```
## The following object is masked from 'package:stats':
## 
##     sigma
```

```
## 
## Attaching package: 'rmgarch'
```

```
## The following objects are masked from 'package:xts':
## 
##     first, last
```

```
## The following objects are masked from 'package:dplyr':
## 
##     first, last
```

## 29.6 BEKK模型

Engle and Kroner(1995) 提出了BEKK模型，
此模型参数个数较多所以比较灵活。
设\(\boldsymbol a\_t\)为\(k\)元的新息列，
\(\Sigma\_t\)的BEKK(1,1)波动率模型为
\[
\Sigma\_t
= A\_0 A\_0^T
+ A\_1 \boldsymbol a\_{t-1} \boldsymbol a\_{t-1}^T A\_1^T
+ B\_1 \Sigma\_{t-1} B\_1,
\]
其中\(A\_0\)是下三角阵使得\(A\_0 A\_0^T\)正定，
\(A\_1\)和\(B\_1\)是\(k\)阶方阵。
此模型包含\(2k^2 + \frac{1}{2}k(k+1)\)个参数，
保证\(\Sigma\_t\)正定。
当参数矩阵\(A\_0, A\_1, B\_1\)满足一定条件时模型平稳且\(\boldsymbol a\_t\)有无条件方差阵。

**例29.2** 考虑IBM股票和标普指数月对数收益率，
时间从1961年1月到2011年12月。

读入数据：

```
da <- readr::read_table("m-ibmsp-6111.txt")
rtn <- log(1 + as.matrix(da[,c("ibm", "sp")]))
```

估计BEKK(1,1)模型：

```
library(MTS)
mod3a <- BEKK11(
  rtn, include.mean=TRUE,
  ini.estimates = c(
    0, 0, 
    0.01, 0.01, 0.01,
    0.1, 0.02, 0.02, 0.1,
    0.9, 0.01, 0.01, 0.9
  ))
```

```
Initial estimates:  0.00772774 0.005023909 ...
Lower limits:  ...
Upper limits:  ...
Log likelihood function:  2003.801 

Coefficient(s):
        Estimate  Std. Error  t value   Pr(>|t|)    
mu1   0.00789936  0.00253413  3.11719 0.00182585 ** 
mu2   0.00567206  0.00154833  3.66335 0.00024894 ***
A011  0.01395530  0.00414066  3.37031 0.00075084 ***
A021  0.00786456  0.00240824  3.26568 0.00109201 ** 
A022  0.00700592  0.00185071  3.78553 0.00015338 ***
A11   0.17749121  0.06341185  2.79902 0.00512575 ** 
A21  -0.04544205  0.03203754 -1.41840 0.15607403    
A12   0.20272882  0.08670987  2.33801 0.01938657 *  
A22   0.38911355  0.05346647  7.27771 3.3951e-13 ***
B11   0.97064393  0.02568654 37.78804 < 2.22e-16 ***
B21   0.01109264  0.01184700  0.93633 0.34910580    
B12  -0.07812127  0.03118842 -2.50482 0.01225149 *  
B22   0.90034700  0.02156710 41.74632 < 2.22e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

MTS的`BEKK11`函数设计较差，
很可能不收敛或者结果依赖于初值。

模型诊断，检验残差是否多元白噪声：

```
Sigma_t <- mod3a$Sigma.t
at <- scale(rtn, center=TRUE, scale=FALSE)
MCHdiag(at, Sigma_t)
```

```
Test results:  
Q(m) of et: 
Test and p-value:  5.049123 0.8878725 
Rank-based test: 
Test and p-value:  17.30134 0.0679564 
Qk(m) of epsilon_t: 
Test and p-value:  30.91185 0.8482728 
Robust Qk(m):  
Test and p-value:  49.71055 0.1396908
```

从模型诊断看拟合效果较好。

BEKK模型有较好的理论性质，
能保证\(\Sigma\_t\)正定。
但是，
因为其参数个数较多，
当\(k\)较大时往往效果不好，
仅\(k=2,3\)时有较好效果。
参数中可能有些元素与0没有显著差异，
也没有办法合理地剔除。
MTS包的`BEKK11`函数运行也不够稳定。

## 29.7 基于Cholesky分解的模型

### 29.7.1 基本思路

这里用的是Cholesky分解的思想而不是直接利用\(\Sigma\_t\)的Cholesky分解。
设新息\(\boldsymbol a\_t\)为\(k\)元时间序列，
有\(E\boldsymbol a\_t = \boldsymbol 0\)，
对各个分量进行正交化：
\[\begin{equation}
\begin{aligned}
b\_{1t} =& a\_{1t}, \\
b\_{2t} =& a\_{2t} - \beta\_{21,t} b\_{1t}, \\
b\_{3t} =& a\_{3t} - \beta\_{31,t} b\_{1t} - \beta\_{32,t} b\_{2t} , \\
& \vdots \\
b\_{kt} =& a\_{kt} - \beta\_{k1,t} b\_{1t}
- \dots - \beta\_{k,k-1,t} b\_{k-1,t}
\end{aligned}
\tag{29.1}
\end{equation}\]

除了第一个方程以外其他方程中的\(\beta\)都是取\(Eb\_{it}^2\)最小的估计值，
这使得\(b\_{1t}, b\_{2t}, \dots, b\_{kt}\)正交。
这种做法类似于线性代数中线性无关向量组的Gram-Schmidt正交化过程。

记\(\boldsymbol b\_t = (b\_{1t}, \dots, b\_{kt})^T\)，
以上的正交化过程可以写成
\[
\boldsymbol b\_t = \beta\_t \boldsymbol a\_t,
\]
其中\(\beta\_t\)是一个单位下三角阵（主对角线元素都等于1的下三角阵），
从而\(\boldsymbol a\_t = \beta\_t^{-1} \boldsymbol b\_t\)。

设\(\boldsymbol b\_t\)的条件方差阵为\(\Sigma\_{b,t}\)，
则\(\Sigma\_{b,t}\)是对角阵，
且
\[\begin{equation}
\Sigma\_t = \beta\_t^{-1} \Sigma\_{b,t} \beta\_t^{-T} .
\tag{29.2}
\end{equation}\]
这实际是正定阵\(\Sigma\_t\)的LDL分解，
有些教材也LDL分解称为Cholesky分解。
因为\(\Sigma\_{b,t}\)是对角阵，
且\(\text{det}(\Sigma\_t) = \prod\_{i=1}^k \sigma\_{b,i,t}\)，
其中\(\sigma\_{b,i,t} = \text{Var}(b\_{it} | F\_{t-1})\)，
所以分解后的似然函数也比较容易计算。

Cholesky方法的多元波动率模型做法如下：

* 对系数\(\beta\_{ij,t}\)，
  使用变系数回归方法估计（比如建立状态空间模型），
  得\(\beta\_t\)序列，
  以及\(\boldsymbol b\_t\)序列；
* 对\(\boldsymbol b\_t\)的每个分量使用一元的波动率模型如一元GARCH模型；
* 用[(29.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/mvgarch.html#eq:mvgarch-chol-decomp)得到\(\Sigma\_t\)序列。

### 29.7.2 一种具体做法

下面给出一种具体的做法步骤。

* 对模型[(29.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/mvgarch.html#eq:mvgarch-chol-idea-orth)，
  利用递推最小二乘法进行参数估计，
  设第\(i\)个方程的参数估计记为\(\hat{\boldsymbol\beta}\_{i,t}\)，
  是\(i-1\)维向量（\(i=2,\dots,k\)）。
  递推最小二乘估计是随着时间\(t\)增加利用新观测值更新\(\hat{\boldsymbol\beta}\_{i,t}\)估计值的算法，
  需要利用开始的若干个时间点的数据计算初始估计。
* 对\(\hat{\boldsymbol\beta}\_{i,t}\)，
  利用EWMA方法取\(\lambda=0.96\)进行平滑估计。
  具体说来，
  设\(\hat{\boldsymbol\beta}\_{i,t}\)关于\(t\)的均值为\(\hat{\boldsymbol\mu}\_i\)，
  令\(\hat{\boldsymbol\beta}\_{i,t}^\* = \hat{\boldsymbol\beta}\_{i,t} - \hat{\boldsymbol\mu}\_i\)，
  然后取
  \[
  \tilde{\boldsymbol\beta}\_{it}^\*
  = \lambda \tilde{\boldsymbol\beta}\_{i,t-1}^\*
  + (1-\lambda) \hat{\boldsymbol\beta}\_{i,t-1}^\*,
  \ t=2,3,\dots,
  \]
  初值取\(\tilde{\boldsymbol\beta}\_{i,1}^\* = \hat{\boldsymbol\beta}\_{i,1}^\*\)，
  取平滑结果为\(\tilde{\boldsymbol\beta}\_{it} = \tilde{\boldsymbol\beta}\_{it}^\* + \hat{\boldsymbol\mu}\_i\)。
* 令\(\hat b\_{1t}=a\_{1t}\)，
  用上一步获得的\(\tilde{\boldsymbol\beta}\_{it}\)计算\(\hat b\_{it}\)。
* 对每个\(\hat b\_{it}\)序列拟合一元GARCH模型，
  记拟合值为\(\hat\sigma^2\_{b,i,t}\)。
* 用估计的\(\tilde{\boldsymbol\beta}\_{it}\)和\(\hat\sigma^2\_{b,i,t}\)计算\(\hat\Sigma\_t\)。

### 29.7.3 计算实例

考虑[29.5.4](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/mvgarch.html#mvgarch-dcc-exa)中使用的IBM、标普、可口可乐数据建模。

```
da <- readr::read_table("m-ibmspko-6111.txt")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   date = col_double(),
##   ibm = col_double(),
##   sp = col_double(),
##   ko = col_double()
## )
```

```
rtn <- log(1 + as.matrix(da[,c("ibm", "sp", "ko")]))
hat_mu <- colMeans(rtn); hat_mu
```

```
##         ibm          sp          ko 
## 0.007727740 0.005023909 0.010595213
```

利用前三年的数据作为递推最小二乘法的初始数据。

```
library(MTS)
library(fGarch)
```

```
## NOTE: Packages 'fBasics', 'timeDate', and 'timeSeries' are no longer
## attached to the search() path when 'fGarch' is attached.
## 
## If needed attach them yourself in your R script by e.g.,
##         require("timeSeries")
```

```
## 
## Attaching package: 'fGarch'
```

```
## The following object is masked from 'package:TTR':
## 
##     volatility
```

```
mod4 <- MCholV(rtn)
```

```
## Sample means:  0.007728 0.005024 0.010595 
## Estimation of the first component 
## Estimate (alpha0, alpha1, beta1):  0.000356 0.117514 0.810289 
## s.e.                            :  0.000157 0.037004 0.057991 
## t-value                         :  2.262887 3.175751 13.97267 
## Component  2  Estimation Results (residual series): 
## Estimate (alpha0, alpha1, beta1):  6.4e-05 0.099156 0.858354 
## s.e.                            :  3.1e-05 0.027785 0.037238 
## t-value                         :  2.034528 3.568616 23.05076 
## Component  3  Estimation Results (residual series): 
## Estimate (alpha0, alpha1, beta1):  0.000173 0.117506 0.818722 
## s.e.                            :  6.2e-05 0.028651 0.038664 
## t-value                         :  2.808075 4.101297 21.17521
```

上面仅显示了三个正交化的\(\hat b\_{it}\)序列的分别的GARCH(1,1)模型估计结果，
但保存的`mod4`中包含变量`Sigma.t`是估计的\(\hat\Sigma\_t\)。
保存为\(T' \times 9\)矩阵，
\(T'\)是扣除了用于RLS标准化以后\(T\)剩余的观测时间点个数。

进行模型诊断：

```
at <- scale(rtn[-(1:36),], center=TRUE, scale=FALSE)
MCHdiag(at, mod4$Sigma.t)
```

```
## Test results:  
## Q(m) of et: 
## Test and p-value:  15.94979 0.1010789 
## Rank-based test: 
## Test and p-value:  21.99727 0.01511849 
## Qk(m) of epsilon_t: 
## Test and p-value:  123.7687 0.01057302 
## Robust Qk(m):  
## Test and p-value:  95.49625 0.3259657
```

```
MCHdiag(at, mod4$Sigma.t, m=5)
```

```
## Test results:  
## Q(m) of et: 
## Test and p-value:  5.717053 0.3347315 
## Rank-based test: 
## Test and p-value:  5.579834 0.3492711 
## Qk(m) of epsilon_t: 
## Test and p-value:  59.83697 0.06842153 
## Robust Qk(m):  
## Test and p-value:  58.97872 0.07891491
```

虽然有一些结果显著，
但是相对于其它参数太少的模型拟合效果还是好得多。

## 29.8 其它多元波动率模型介绍

一类方法是考虑对\(\boldsymbol a\_t\)正交化，
求\(M\)使得\(\boldsymbol b\_t = M \boldsymbol a\_t\)的各个分量正交，
这样可以对\(\boldsymbol b\_t\)每个分量使用一元波动率模型，
再利用\(\boldsymbol a\_t\)和\(\boldsymbol b\_t\)的方差阵关系获得\(\Sigma\_t\)估计。

另一类方法基于Copula，
分别描述各分量的边缘变化，
并用Copula表示各分量之间的关系。

Hu and Tsay(2014) 提出了Principal Volatility Components模型。

### B 参考文献

———. 2014. *Multivariate Time Series Analysis with r and Financial Applications*. John Wiley & Sons, Inc.