---
crawl_time: '2026-01-17 14:20:40'
framework: rbook
title: 30 R初等统计分析 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 30 R初等统计分析

这一部分讲授如何用R进行统计分析，
包括基本概括统计和探索性数据分析，
置信区间和假设检验，
回归分析与各种回归方法，
广义线性模型，
非线性回归与平滑，
判别树和回归树，
等等。

主要参考书：

* ([Venables and Ripley 2002](#ref-MASS4ed))
* ([Kabacoff 2012](#ref-R-in-action))

## 30.1 概率分布

R中与`xxx`分布有关的函数包括：

* `dxxx(x)`，
  即`xxx`分布的分布密度函数(PDF)或概率函数(PMF)\(p(x)\)。
* `pxxx(q)`，
  即`xxx`分布的分布函数(CDF)\(F(q)=P(X \leq q)\)。
* `qxxx(p)`，
  即`xxx`分布的分位数函数\(q(p)\), \(p \in (0,1)\)，
  对连续型分布，\(q(p) = F^{-1}(p)\)，
  即\(F(x)=p\)的解\(x\)。
* `rxxx(n)`，
  即`xxx`的随机数函数，可以生成\(n\)个`xxx`的随机数。

`dxxx(x)`函数可以加选项`log=TRUE`，
用来计算`\ln p(x)`，
这在计算对数似然函数时有用，
比`log(dxxx(x))`更精确。

`pxxx(q)`可以加选项`lower.tail=FALSE`，
变成计算\(P(X>q) = 1 - F(q)\)。

`qxxx(p)`可以加选项`lower.tail=TRUE`，
表示求\(P(X>x)=p\)的解\(x\)；
可以加选项`log.p=TRUE`，
表示输入的\(p\)实际是\(\ln p\)。

这些函数都可以带有自己的分布参数，
有些分布参数有缺省值，
比如正态分布的缺省参数值为零均值单位标准差。

具体的分布类型可以在R命令行用`?Distributions`查看列表。
常用的分布密度有：

* 离散分布有`dbinom`二项分布,
  `dpois`泊松分布，
  `dgeom`几何分布，
  `dnbinom`负二项分布，
  `dmultinom`多项分布，
  `dhyper`超几何分布。
* 连续分布有
  `dunif`均匀分布，
  `dnorm`正态分布，
  `dchisq`卡方分布，
  `dt` t分布(包括非中心t)，
  `df` F分布，
  `dexp`指数分布，
  `dweibull` 威布尔分布，
  `dgamma` 伽马分布，
  `dbeta` 贝塔分布，
  `dlnorm` 对数正态分布，
  `dcauchy` 柯西分布,
  `dlogis` 逻辑斯谛分布。

R的扩展包提供了更多的分布，
参见R网站的如下链接：

* <https://cran.r-project.org/web/views/Distributions.html>

## 30.2 最大似然估计

R函数`optim()`、`nlm()`、`optimize()`可以用来求函数极值，
因此可以用来计算最大似然估计。
`optimize()`只能求一元函数极值。

### 30.2.1 一元正态分布参数最大似然估计

正态分布最大似然估计有解析表达式。
作为示例，
用R函数进行数值优化求解。

对数似然函数为：

\[
\ln L(\mu,\sigma^2) = -\frac{n}{2}\ln(2\pi)
-\frac{n}{2}\ln\sigma^2
- \frac{1}{2\sigma^2} \sum(X\_i - \mu)^2
\]

定义R的优化目标函数为上述对数似然函数去掉常数项以后乘以\(-2\)，
求其最小值点。
目标函数为：

```
objf.norm1 <- function(theta, x){
  mu <- theta[1]
  s2 <- exp(theta[2])
  n <- length(x)
  res <- n*log(s2) + 1/s2*sum((x - mu)^2)
  res
}
```

其中\(\theta\_1\)为均值参数\(\mu\)，
\(\theta\_2\)为方差参数\(\sigma^2\)的对数值。
`x`是样本数值组成的R向量。

可以用`optim`函数来求极小值点。下面是一个模拟演示:

```
norm1d.mledemo1 <- function(n=30){
  mu0 <- 20
  sigma0 <- 2
  set.seed(1)
  x <- rnorm(n, mu0, sigma0)

  theta0 <- c(0,0)
  ores <- optim(theta0, objf.norm1, x=x)
  print(ores)
  theta <- ores$par
  mu <- theta[1]
  sigma <- exp(0.5*theta[2])
  cat('真实mu=', mu0, ' 公式估计mu=', mean(x), 
      ' 数值优化估计mu=', mu, '\n')
  cat('真实sigma=', sigma0, 
      '公式估计sigma=', sqrt(var(x)*(n-1)/n), 
      ' 数值优化估计sigma=', sigma, '\n')
}
norm1d.mledemo1()
```

```
## $par
## [1] 20.166892  1.193593
## 
## $value
## [1] 65.83709
## 
## $counts
## function gradient 
##      115       NA 
## 
## $convergence
## [1] 0
## 
## $message
## NULL
## 
## 真实mu= 20  公式估计mu= 20.16492  数值优化估计mu= 20.16689 
## 真实sigma= 2 公式估计sigma= 1.817177  数值优化估计sigma= 1.816291
```

也可以用`nlm()`函数求最小值点，
下面是正态分布最大似然估计的另一个演示：

```
norm1d.mledemo2 <- function(){
  set.seed(1)
  n <- 30
  mu <- 20
  sig <- 2
  z <- rnorm(n, mean=mu, sd=sig)
  neglogL <- function(parm) {
      -sum( dnorm(z, mean=parm[1],
                  sd=exp(parm[2]), log=TRUE) )
  }

  res <- nlm(neglogL, c(10, log(10)))
  print(res)
  sig2 <- exp(res$estimate[2])
  cat('真实mu=', mu, ' 公式估计mu=', mean(z), 
      ' 数值优化估计mu=', res$estimate[1], '\n')
  cat('真实sigma=', sig, 
      '公式估计sigma=', sqrt(var(z)*(n-1)/n), 
      ' 数值优化估计sigma=', sig2, '\n')
  invisible()
}
norm1d.mledemo2()
```

```
## $minimum
## [1] 60.48667
## 
## $estimate
## [1] 20.1649179  0.5972841
## 
## $gradient
## [1] 1.444576e-05 9.094805e-06
## 
## $code
## [1] 2
## 
## $iterations
## [1] 28
## 
## 真实mu= 20  公式估计mu= 20.16492  数值优化估计mu= 20.16492 
## 真实sigma= 2 公式估计sigma= 1.817177  数值优化估计sigma= 1.817177
```

函数`optim()`缺省使用Nelder-Mead单纯型搜索算法，
此算法不要求计算梯度和海色阵，
算法稳定性好，
但是收敛速度比较慢。
可以用选项`method="BFGS"`指定BFGS拟牛顿法。
这时可以用`gr=`选项输入梯度函数，
缺省使用数值微分计算梯度。
如：

```
norm1d.mledemo1b <- function(n=30){
  mu0 <- 20
  sigma0 <- 2
  set.seed(1)
  x <- rnorm(n, mu0, sigma0)

  theta0 <- c(1,1)
  ores <- optim(theta0, objf.norm1, x=x, method="BFGS")
  print(ores)
  theta <- ores$par
  mu <- theta[1]
  sigma <- exp(0.5*theta[2])
  cat('真实mu=', mu0, ' 公式估计mu=', mean(x), 
      ' 数值优化估计mu=', mu, '\n')
  cat('真实sigma=', sigma0, 
      '公式估计sigma=', sqrt(var(x)*(n-1)/n), 
      ' 数值优化估计sigma=', sigma, '\n')
}
norm1d.mledemo1b()
```

```
## $par
## [1] 20.164916  1.194568
## 
## $value
## [1] 65.83704
## 
## $counts
## function gradient 
##       41       17 
## 
## $convergence
## [1] 0
## 
## $message
## NULL
## 
## 真实mu= 20  公式估计mu= 20.16492  数值优化估计mu= 20.16492 
## 真实sigma= 2 公式估计sigma= 1.817177  数值优化估计sigma= 1.817177
```

注意使用数值微分时，
参数的初值的数量级应该与最终结果相差不大，
否则可能不收敛，
比如上面程序取初值\((0,0)\)就会不收敛。

`optim()`函数支持的方法还有`"CG"`是一种共轭梯度法，
此方法不如BFGS稳健，
但是可以减少存储量使用，
对大规模问题可能更适用。
`"L-BFGS-B"`是一种区间约束的BFGS优化方法。
`"SANN"`是模拟退火算法，
有利于求得全局最小值点，
速度比其它方法慢得多。

函数`optimize()`进行一元函数数值优化计算。
例如，在一元正态分布最大似然估计中，
在对数似然函数中代入\(\mu = \bar x\)，目标函数（负2倍对数似然函数）为
\[
h(\sigma^2) = \ln(\sigma^2) + \frac{1}{\sigma^2} \sum (X\_i - \bar X)^2
\]
下面的例子模拟计算\(\sigma^2\)的最大似然估计：

```
norm1d.mledemo3 <- function(n=30){
  mu0 <- 20
  sigma0 <- 2
  set.seed(1)
  x <- rnorm(n, mu0, sigma0)

  mu <- mean(x)
  ss <- sum((x - mu)^2)/length(x)
  objf <- function(delta, ss) log(delta) + 1/delta*ss
  ores <- optimize(objf, lower=0.0001,
                   upper=1000, ss=ss)
  delta <- ores$minimum
  sigma <- sqrt(delta)

  print(ores)
  cat('真实sigma=', sigma0, 
      '公式估计sigma=', sqrt(var(x)*(n-1)/n), 
      ' 数值优化估计sigma=', sigma, '\n')
}
norm1d.mledemo3()
```

```
## $minimum
## [1] 3.30214
## 
## $objective
## [1] 2.194568
## 
## 真实sigma= 2 公式估计sigma= 1.817177  数值优化估计sigma= 1.817179
```

## 30.3 单样本均值的检验和置信区间

R的ggstatsplot扩展包提供了将假设检验结果图示化的功能。

### 30.3.1 大样本情形

设总体\(X\)期望为\(\mu\)，方差为\(\sigma^2\)。
\(X\_1, X\_2, \dots, X\_n\)是一组样本。

在大样本时，令
\[
Z = \frac{\bar x - \mu}{S / \sqrt{n}} \stackrel{\bullet}{\sim} \text{N}(0,1)
\]
其中\(\stackrel{\bullet}{\sim}\)表示近似服从某分布。
\(\bar x\)为样本均值，\(S\)为样本标准差。
\(\sigma\)未知且大样本时，
\(\mu\)近似\(1-\alpha\)置信区间为
\[
\bar x \pm \text{qnorm}(1-\alpha/2) S / \sqrt{n}
\]

用`BSDA::z.test(x, sigma.x=sd(x), conf.level=1-alpha)`可以计算如上的近似置信区间。

对于假设检验问题
\[\begin{aligned}
& H\_0: \mu = \mu\_0 \longleftrightarrow H\_a: \mu \neq \mu\_0 \\
& H\_0: \mu \leq \mu\_0 \longleftrightarrow H\_a: \mu > \mu\_0 \\
& H\_0: \mu \geq \mu\_0 \longleftrightarrow H\_a: \mu < \mu\_0
\end{aligned}\]
定义检验统计量
\[
Z = \frac{\bar x - \mu\_0}{S / \sqrt{n}}
\]
当\(\mu=\mu\_0\)时\(Z\)近似服从N(0,1)分布。
可以用`BSDA::z.test(x, mu=mu0, sigma.x=sd(x))`进行双侧Z检验。
加选项`alternative="greater"`作右侧检验，
加选项`alternative="less"`作左侧检验。

如果上述问题中标准差\(\sigma\)为已知，
应该在\(Z\)的公式用\(\sigma\)替换\(S\)，即
\[
Z = \frac{\bar x - \mu\_0}{\sigma / \sqrt{n}}
\]
计算\(\mu\)的置信区间程序为`BSDA::z.test(x, sigma.x=sigma0)`，
计算\(Z\)检验的程序为`BSDA::z.test(x, mu=mu0, sigma.x=sigma0)`，
仍可用选项`alternative=`指定双侧、右侧、左侧。

### 30.3.2 小样本正态情形

如果样本量较小（小于30），
则需要假定总体服从正态分布N(\(\mu, \sigma^2\))。

当\(\sigma\)已知时，
有
\[
Z = \frac{\bar x - \mu\_0}{\sigma / \sqrt{n}} \sim \text{N}(0,1)
\]
计算\(\mu\)的置信区间程序为`BSDA::z.test(x, sigma.x=sigma0)`，
计算\(Z\)检验的程序为`BSDA::z.test(x, mu=mu0, sigma.x=sigma0)`。

可以自己写一个计算Z检验的较通用的R函数：

```
z.test.1s <- function(
  x, n=length(x), mu=0, sigma=sd(x), alternative="two.sided"){
  z <- (mean(x) - mu) / (sigma / sqrt(n))
  if(alternative=="two.sided"){   # 双侧检验
    pvalue <- 2*(1 - pnorm(abs(z)))
  } else if(alternative=="less"){ # 左侧检验
    pvalue <- pnorm(z)
  } else if(alternative=="greater"){ # 右侧检验
    pvalue <- 1 - pnorm(z)
  } else {
    stop("alternative unknown!")
  }
  
  c(stat=z, pvalue=pvalue)
}
```

这个函数输入样本`x`和\(\sigma=\sigma\_0\)，
或者输入\(\bar x\)为`x`和\(\sigma=\sigma\_0\)，
输出检验统计量值和p值。

如果\(\sigma\)未知，应使用t统计量检验
\[
T = \frac{\bar x - \mu}{S / \sqrt{n}}
\]
当\(\mu=\mu\_0\)时\(T \sim t(n-1)\)。
如果`x`保存了样本，
用`t.test(x, conf.level=1-alpha)`计算\(\mu\)的置信区间；
用`t.test(x, mu=mu0, alternative="two.side")`计算\(H\_0: \mu=\mu\_0\)的双侧检验，
称为单样本t检验。

如果已知的\(\bar x\)和\(S\)而非具体样本数据，
可以自己写一个R函数计算t检验：

```
t.test.1s <- function(
  x, n=length(x), sigma=sd(x), mu=0, 
  alternative="two.sided"){
  tstat <- (mean(x) - mu) / (sigma / sqrt(n))
  if(alternative=="two.sided"){   # 双侧检验
    pvalue <- 2*(1 - pt(abs(tstat), n-1))
  } else if(alternative=="less"){ # 左侧检验
    pvalue <- pt(tstat, n-1)
  } else if(alternative=="greater"){ # 右侧检验
    pvalue <- 1 - pt(tstat, n-1)
  } else {
    stop("Alternative unknown!")
  }
  
  c(stat=tstat, pvalue=pvalue)
}
```

这个函数允许输入\(\bar x\)到`x`中，输入\(n\)，输入\(S\)到`sigma`中，
然后计算t检验。

### 30.3.3 统计假设检验显著性的解释

统计假设检验如果拒绝了\(H\_0\)，
也称“结果显著”、“有显著差异”等。
这只代表样本与零假设的差距足够大，
可以以比较小的代价（第一类错误概率）
排除这样的差距是由于随机样本的随机性引起的，
但是并不表明这样的差距是有科学意义或者有现实意义的。

尤其是超大样本时，
完全没有实际意义的差距也会检验出来是统计显著的。

这时，可以设定一个“有实际意义的差距”\(\delta\)，
当差距超过\(\delta\)时才认为有实际差距，如：
\[
H\_0: |\mu - \mu\_0| \leq \delta
\longleftrightarrow |\mu - \mu\_0| > \delta
\]

统计假设检验如果不拒绝\(H\_0\)，
也称为“结果不显著”、“没有显著差异”，
通常不认为\(H\_0\)就是对的，
看法是没有充分理由推翻\(H\_0\)。

进一步收集更多的数据，
有可能就会发现显著差异。

例如，\(H\_0\)是嫌疑人无罪，
最后不拒绝\(H\_0\)，
只能是没有充分证据证明有罪，
如果后来收集到了关键的罪证，
就还可以再判决嫌疑人有罪。

### 30.3.4 置信区间与双侧检验的关系

设总体为正态分布\(\text{N}(\mu, \sigma^2)\),
\(\sigma\)已知。
随机抽取了\(n\)个样品，计算得\(\bar x\)。
\(\mu\)的\(1-\alpha\)置信区间为
\[ \bar x \pm z\_{\alpha/2} \sigma/\sqrt{n} \]

置信区间理论依据：
\[
P\left(\frac{|\bar x - \mu|}{\sigma/\sqrt{n}}
\leq z\_{\alpha/2} \right) = 1 - \alpha \tag{\*}
\]

对双侧检验
\[ H\_0: \mu = \mu\_0 \longleftrightarrow H\_a: \mu \neq \mu\_0 \]
当
\[ \frac{|\bar x - \mu\_0|}{\sigma/\sqrt{n}} \leq z\_{\alpha/2}\]
时接受\(H\_0\)。

接受\(H\_0\)的条件，就是\(\mu\_0\)满足(\*)式的大括号中的条件，
即当且仅当\(\mu\_0\)落入\(\mu\)的\(1-\alpha\)置信区间时接受\(H\_0\)。

其它检验也与置信区间有类似关系，
但是单侧检验对应于单侧置信区间。

### 30.3.5 单样本均值比较例子

载入假设检验部分示例程序所用数据：

```
load("data/hyptest-data.RData")
```

#### 30.3.5.1 咖啡标重的单侧检验

美国联邦贸易委员会定期检查商品标签是否与实际相符。
Hiltop Coffee大罐咖啡标签注明含量为3磅。
每罐具体多一些或少一些并不要紧，
只要总体平均值不少于3磅，
消费者利益就未受侵害。

只有在有充分证据时才应该做出分量不够的判决进而对厂家进行处罚。
所以零假设是\(H\_0: \mu \geq 3\)，
对立假设是\(H\_a: \mu < 3\)。

收集数据进行统计判决后，
如果不拒绝\(H\_0\)，
就不需要采取行动；
如果拒绝了\(H\_0\)，
就说明有足够证据认为厂商灌装分量不足，
应进行处罚。

随机抽取了36件样品，
计算平均净重\(\bar x\)作为总体均值\(\mu\)的估计。

如果\(\bar x < 3\)，
则\(H\_0\)有理由被怀疑。

\(\bar x\)比\(\mu\_0 = 3\)少多少，
才让我们愿意拒绝\(H\_0\)?
做出拒绝\(H\_0\)的决定可能会犯第一类错误，
如果差距很小，
这样的差距很可能是随机抽样的随机性引起的，
拒绝\(H\_0\)有很大的机会犯错。

必须先确定一个能够容忍的第一类错误概率。
调查员认为，可以容忍1%的错误拒绝\(H\_0\)的机会
（不该处罚时给出了处罚）
以换得能够在厂商分量不足时正确地做出处罚决定的机会。
即选检验水平\(\alpha=0.01\)。

当\(H\_0\)实际成立时，
边界\(\mu=\mu\_0=3\)处犯第一类错误的概率最大
（这里最不容易区分），
所以计算第一类错误概率只要在边界处计算。

统计假设检验第一步是确定零假设与对立假设，
第二步是确定检验水平\(\alpha\)。
第三步是抽取随机样本，计算检验统计量。

以往经验证明净重的标准差是\(\sigma=0.18\)，
且净重的分布为正态分布。

则\(\bar x\)的抽样分布为\(\text{N}(\mu, \sigma^2/n)\)，
\(\bar x\)的标准误差为\(\text{SE}(\bar x) = 0.18 / \sqrt{36} = 0.03\)。

令
\[ z = \frac{\bar x - \mu\_0}{\text{SE}(\bar x)} \]
则\(H\_0\)成立且\(\mu\)取边界处的\(\mu\_0\)时\(z \sim \text{N}(0,1)\)。

当\(\bar x\)比\(\mu\_0=3\)小很多时应拒绝\(H\_0\)。
即检验统计量\(z\)很小时拒绝\(H\_0\)。

如果\(z = -1\)，
标准正态分布取-1或比-1更小的值的概率是0.1586
(用R程序`pnorm(-1)`计算)，
这个概率不小，
不小于\(\alpha=0.01\)，
这时不应该拒绝\(H\_0\)。

如果\(z = -2\)，
标准正态分布取-2或比-2更小的值的概率是0.0228，
(用R程序`pnorm(-2)`计算)，
这个概率已经比较小了，
但还不小于预先确定的第一类错误概率\(\alpha=0.01\)，
这时不应该拒绝\(H\_0\)。

如果\(z = -3\)，
标准正态分布取-3或比-3更小的值的概率是0.0013，
(用R程序`pnorm(-3)`计算)，
这个概率已经很小了，
也小于预先确定的第一类错误概率\(\alpha=0.01\)，
这时应该拒绝\(H\_0\)。

对左侧检验问题\(H\_0: \mu \geq \mu\_0 \longleftrightarrow H\_a: \mu < \mu\_0\),
若\(Z\)表示标准正态分布随机变量，
\(z\)是检验统计量的值，在\(\mu=\mu\_0\)时\(z\)的抽样分布是标准正态分布，
可以计算抽样分布中取到等于\(z\)以及比\(z\)更小的值的概率\(P(Z \leq z)\)，
称为检验的p值。

p值越小，说明\(\bar x\)比\(\mu\_0\)小得越多，
拒绝\(H\_0\)也越有充分证据。
当且仅当p值小于检验水平\(\alpha\)时拒绝\(H\_0\)。

p值小于\(\alpha\)，说明如果\(H\_0\)真的成立，
出现\(\bar x\)那么小的检验统计量值的概率也很小，
小于\(\alpha\)，所以第一类错误概率小于等于\(\alpha\)。

一般地，
p值是从样本计算的一个在假定零假设成立情况下，
检验统计量取值更倾向于对立假设成立的概率。
p值越小，拒绝\(H\_0\)越有依据。

设咖啡标称重量问题中抽取了36件样品，测得\(\bar x = 2.92\)磅，
计算得
\[
z = \frac{\bar x - \mu\_0}{\sigma/\sqrt{n}}
= \frac{2.92 - 3}{0.18/\sqrt{36}} = -2.67
\]

p值为\(P(Z \leq -2.67)\)(\(Z\)是标准正态分布随机变量)，
用R计算得`pnorm(-2.67)`\(=0.0038 \leq \alpha=0.01\)，
拒绝\(H\_0\)，
应对该厂商进行处罚。

已经预先确定了检验水平\(\alpha=0.01\)就不能在看到检验结果后更改检验水平。
但是，
报告出p值后，
其他的人可以利用这样的信息，
不同的人或利益方可能有不同的检验水平的选择，
这通过同一个p值都可以给出需要的结果。

p值也称为“**观测显著性水平**”，
因为它是能够拒绝\(H\_0\)的最小的可选检验水平，
选取比p值更小的检验水平就不能拒绝\(H\_0\)了。

用自定义的`z.test.1s()`计算：

```
z.test.1s(x=2.92, mu=3, n=36, sigma=0.18, alternative="less")
```

```
##         stat       pvalue 
## -2.666666667  0.003830381
```

p值为0.004，在0.01水平下拒绝原假设，
认为咖啡平均重量显著地低于标称的3磅。

设`Coffee`数据框的`Weight`是这36个样本值，
也可以将程序写成：

```
z.test.1s(Coffee[["Weight"]], mu=3, sigma=0.18, alternative="less")
```

```
##         stat       pvalue 
## -2.666666667  0.003830381
```

或

```
BSDA::z.test(Coffee[["Weight"]], mu=3, sigma.x=0.18, alternative="less")
```

```
## 
##  One-sample z-Test
## 
## data:  Coffee[["Weight"]]
## z = -2.6667, p-value = 0.00383
## alternative hypothesis: true mean is less than 3
## 95 percent confidence interval:
##        NA 2.969346
## sample estimates:
## mean of x 
##      2.92
```

#### 30.3.5.2 高尔夫球性能的双侧检验

美国高尔夫协会对高尔夫运动器械有一系列的规定。
MaxFlight生产高品质的高尔夫球，
平均的飞行距离是295码。

生产线有时会有波动，
当生产的球平均飞行距离不足295码时，
用户会感觉不好用；
当生产的球平均飞行距离超过295码时，
美国高尔夫协会禁止使用这样的球。

该厂商定期抽检生产线上下来的产品，
每次抽取50只。

当平均飞行距离与295码没有显著差距时，
不需要采取措施；
如果有显著差距，就需要调整生产线。

所以取\(H\_0: \mu = 295\),
\(H\_a: \mu \neq 295\)。
选检验水平\(\alpha=0.05\)。

随机抽取\(n\)个样品，
在可控条件下测试出每个的飞行距离，
计算出平均值\(\bar x\)。

当\(\bar x\)比\(\mu\_0\)(这里是295)小很多或者
\(\bar x\)比\(\mu\_0\)大很多时拒绝\(H\_0\)，
否则不拒绝\(H\_0\)。

设已知总体标准差\(\sigma=12\),
且飞行距离服从正态分布。

取统计量
\[z = \frac{\bar x - \mu\_0}{\sigma/\sqrt{n}}\]
在\(H\_0\)成立时，
\(z\)的抽样分布为标准正态分布。

* 设测得\(\bar x = 297.6\)，超过了\(\mu\_0=295\)。

\[
z = = \frac{\bar x - \mu\_0}{\sigma/\sqrt{n}}
= \frac{297.6 - 295}{12/\sqrt{50}} = 1.53
\]

\(H\_0\)成立时\(z\)抽样分布为标准正态分布，
设\(Z\)是标准正态分布随机变量，
\(Z\)取到1.53或者比1.53更倾向于反对\(H\_0\)的值概率？
这包括\(P(Z \geq 1.53)\)，
还应包括\(P(Z \leq -1.53)\)。

所以p值是\(P(|Z| \geq |z|)\)，
R中计算为`2*(1 - pnorm(abs(z)))`。
p值等于0.1260，
超过检验水平，
不拒绝\(H\_0\)，
不需要调整生产线。

用自定义的`z.test.1s()`检验：

```
z.test.1s(GolfTest[['Yards']], mu=295, sigma=12, alternative="two.sided")
```

```
##      stat    pvalue 
## 1.5320647 0.1255065
```

或用`BSDA::z.test()`:

```
BSDA::z.test(GolfTest[['Yards']], mu=295, sigma.x=12, alternative="two.sided")
```

```
## 
##  One-sample z-Test
## 
## data:  GolfTest[["Yards"]]
## z = 1.5321, p-value = 0.1255
## alternative hypothesis: true mean is not equal to 295
## 95 percent confidence interval:
##  294.2738 300.9262
## sample estimates:
## mean of x 
##     297.6
```

p值为0.13，在0.05水平下不拒绝\(H\_0\)，
生产的球的平均飞行距离与标准的295码之间没有显著差异。

#### 30.3.5.3 Heathrow机场打分的检验

某旅行杂志希望根据乘机进行商务旅行的意见给跨大西洋门户机场评分。
乘客评分取0到10分，超过7分算作是服务一流的机场。
在每个机场随机选取60位商务旅行乘客打分评价。
英国Heathrow机场的打分样本\(\bar x = 7.25\), \(S = 1.052\)。
英国Heathrow机场可以算是服务一流吗？

由于随机样本的随机性，
要判断的是\(\mu > 7\)是否成立，
这里\(\bar x = 7.25\)并不能很确定第说\(\mu > 7\)成立。

因为将机场评为一流需要有充分证据，
所以将对立假设\(H\_a\)设为\(\mu > 7\)，
然后取\(H\_0: \mu \leq 7\)。

取检验水平\(\alpha=0.05\)。

计算检验统计量
\[
t = \frac{\bar x - \mu\_0}{S / \sqrt{n}}
= \frac{7.25 - 7}{1.052/\sqrt{60}} = 1.84
\]
设\(T\)是服从\(t(n-1)\)分布的随机变量，
p值为\(P(T \geq 1.84)\)。
用R软件计算为`1 - pt(1.84, 60-1)`\(=0.0354\)。
p值小于\(\alpha\)，拒绝\(H\_0\)，
认为平均评分显著高于7，
可以认为Heathrow机场是服务一流。

从原始样本数据用`t.test()`检验：

```
t.test(AirRating[["Rating"]], mu=7, alternative="greater")
```

```
## 
##  One Sample t-test
## 
## data:  AirRating[["Rating"]]
## t = 1.8414, df = 59, p-value = 0.03529
## alternative hypothesis: true mean is greater than 7
## 95 percent confidence interval:
##  7.023124      Inf
## sample estimates:
## mean of x 
##      7.25
```

用自定义的`t.test.1s()`和样本统计量计算：

```
t.test.1s(x=7.25, sigma=1.052, n=60, mu=7, alternative="greater")
```

```
##       stat     pvalue 
## 1.84077155 0.03534255
```

#### 30.3.5.4 玩具厂商订货量的假设检验

玩具厂商Holiday Toys通过超过1000家玩具商店售货。
为了准备即将到来的冬季销售季节，
销售主管需要确定今年某新款玩具的生产量。
凭经验判断平均每家商店需要40件。

为了确定，随机抽取了25家商店，
提供了新款玩具的特点、进货价和建议销售价，
让他们给出订货量估计。
目的是判断按每家40件来生产是否合适。

因为多生产或少生产都不合适，
所以取零假设为\(H\_0: \mu=40\)。
对立假设为\(H\_a: \mu \neq 40\)。
只有拒绝\(H\_0\)时才需要更改生产计划。

取检验水平\(\alpha=0.05\)。
样本计算得\(n=25\)，\(\bar x = 37.4\), \(S = 11.79\)。

检验统计量
\[
t = \frac{\bar x - \mu\_0}{S / \sqrt{n}}
= \frac{37.4 - 40}{11.79 / \sqrt{25}} = -1.10
\]

设\(T\)为服从t\((n-1)\)分布的随机变量，
p值为\(2 P(|T| > |-1.10|)\)，
用R计算为`2*(1 - pt(abs(-1.10), 25-1))`\(=0.2822\)。

不拒绝\(H\_0\)，所以不需要更改生产计划。

从原始样本数据用`t.test()`检验：

```
t.test(Orders[["Units"]], mu=40, alternative="two.sided")
```

```
## 
##  One Sample t-test
## 
## data:  Orders[["Units"]]
## t = -1.1026, df = 24, p-value = 0.2811
## alternative hypothesis: true mean is not equal to 40
## 95 percent confidence interval:
##  32.5334 42.2666
## sample estimates:
## mean of x 
##      37.4
```

用自定义的`t.test.1s()`和样本统计量计算：

```
t.test.1s(x=37.4, sigma=11.79, n=25, mu=40, alternative="two.sided")
```

```
##       stat     pvalue 
## -1.1026293  0.2811226
```

利用ggstatsplot图示：

```
library(ggstatsplot)
gghistostats(
  data              = Orders,
  x                 = Units,
  title      = "玩具生产订货量研究", 
  test.value = 40
)
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics_files/figure-html/stat-base-hyptest-mu1s-ex04c-1.png)

### 30.3.6 单样本均值比较样本量和功效计算

检验的功效，
是指对立假设成立时检验拒绝\(H\_0\)的概率\(1-\beta\)，
其中\(\beta\)是第二类错误，
即当对立假设成立时错误地接受\(H\_0\)的概率。
需要足够大的样本量才能使得检验能够发现实际存在的显著差异。
因为要计算功效需要一个确定的对立假设下的分布，
而不是一族分布，
所以需要指定要检验的具体零假设和对立假设分布，
或者指定一个效应大小值。

设\(H\_0: \mu=\mu\_0\)，
对立假设是双侧或者单侧，
为了能够在\(\mu=\mu\_1 \neq \mu\_0\)时拒绝\(H\_0\)的概率达到指定的功效\(1 - \beta = 0.80\)，
设检验水平为\(\alpha\)，
\(|\mu\_1 - \mu\_0| = \delta\)，
需要的样本量为可以用R的`power.t.test()`计算，如：

```
power.t.test(delta = 2.5, sd = 1.5, 
  type = "one.sample",
  alternative = "two.sided",
  sig.level=0.05, 
  power=0.80)
```

```
## 
##      One-sample t test power calculation 
## 
##               n = 5.04919
##           delta = 2.5
##              sd = 1.5
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
```

其中`delta`是要发现的均值差距下限，
`sd`是样本标准差，
如果未知需要先用一个小样本进行估计，
`sig.level`是检验水平，
`power`是要求达到的功效。
可以用`alternative`参数选择双侧（`two.sided`）或单侧（`one.sided`）。
结果表明所求样本量为\(6\)。

样本量和功效是互相决定的。
前面的例子是给定要达到的功效，
求样本量。
也可以输入样本量，
求可达到的功效，如：

```
power.t.test(delta = 2.5, sd = 1.5, 
  type = "one.sample",
  alternative = "two.sided",
  sig.level=0.05, 
  n=10)
```

```
## 
##      One-sample t test power calculation 
## 
##               n = 10
##           delta = 2.5
##              sd = 1.5
##       sig.level = 0.05
##           power = 0.9964471
##     alternative = two.sided
```

所以在单样本t检验中，
若标准差为\(\sigma=1.5\)，
零假设和对立假设下的均值差为\(| \mu\_1 - \mu\_0 | = 2.5\)，
检验水平\(\alpha = 0.05\)，
作双侧t检验，
则样本量\(n=10\)时功效可达到\(99.64\%\)。

实际上，
要发现的均值之间的差别依赖于标准差，
而比值\(\frac{| \mu\_1 - \mu\_0 |}{\sigma}\)则完全决定了要发现的均值差别，
样本量与功效的关系在比值\(\frac{| \mu\_1 - \mu\_0 |}{\sigma}\)给定时也完全确定。
所以，
称比值
\[
d = \frac{| \mu\_1 - \mu\_0 |}{\sigma}
\]
为要检验的“效应大小”(effect size)。
上面例子中\(d = 2.5/1.5 = 1.67\)，
属于较大的效应，
所以很容易检出，
只需要较小的样本量就可以达到足够的功效。

([Cohen 1988](#ref-Cohen1988:PowerAna))给出了单样本t检验时常用的效应大小：

* 小(small): \(0.2\);
* 中(medium): \(0.5\);
* 大(large): \(0.8\)。

要发现的效应越小，
就需要越大的样本量。
要发现的效应很大时，
较小的样本量就可以获得较高的功效。

R扩展包pwr提供了功效与样本量计算问题。
比如，
单样本t检验，
要检验中等效应大小，
取检验水平\(0.05\)，
为了达到\(0.80\)的功效，
可计算如：

```
library(pwr)
cohen.ES(test = "t", size = "medium")
```

```
## 
##      Conventional effect size from Cohen (1982) 
## 
##            test = t
##            size = medium
##     effect.size = 0.5
```

```
pwr.t.test(
  type = "one.sample", 
  alternative="two.sided",
  d = 0.5,
  sig.level = 0.05,
  power = 0.80 )
```

```
## 
##      One-sample t test power calculation 
## 
##               n = 33.36713
##               d = 0.5
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
```

结果表明，
样本量要达到\(n=34\)。

也可以输入样本量，
计算可达到的功效：

```
pwr.t.test(
  type = "one.sample", 
  alternative="two.sided",
  d = 0.5,
  sig.level = 0.05,
  n = 30 )
```

```
## 
##      One-sample t test power calculation 
## 
##               n = 30
##               d = 0.5
##       sig.level = 0.05
##           power = 0.7539647
##     alternative = two.sided
```

单样本双侧t检验，
为了检验中等效应大小，
样本量\(n=30\)对应的功效是\(75\%\)。

**例30.1** 以希思罗机场打分数据为例。

\(H\_0\)下\(\mu = 7.0\),
实际数据的\(\bar x = 7.25\)，
标准差用样本标准差\(S = 1.052\)代替，
样本量\(n = 60\)，
计算\(\bar x - \mu\_0 = 7.25 - 0.25\)这样的均值差，
其效应大小为
\[
d = \frac{\bar x - \mu\_0}{S}
= \frac{7.25 - 7.0}{1.052}
= 0.2376
\]
这个效应比较小，
对应于较大的样本量或者较小的功效。

计算这样的效应大小的功效，
取检验水平\(0.05\):

```
pwr.t.test(
  type = "one.sample", 
  alternative="greater",
  d = (7.25 - 7.0)/1.052,
  sig.level = 0.05,
  n = 60 )
```

```
## 
##      One-sample t test power calculation 
## 
##               n = 60
##               d = 0.2376426
##       sig.level = 0.05
##           power = 0.5693621
##     alternative = greater
```

功效只有\(57\%\)。
计算需要多大样本量可达到\(80\%\)功效：

```
pwr.t.test(
  type = "one.sample", 
  alternative="greater",
  d = (7.25 - 7.0)/1.052,
  sig.level = 0.05,
  power = 0.80 )
```

```
## 
##      One-sample t test power calculation 
## 
##               n = 110.8416
##               d = 0.2376426
##       sig.level = 0.05
##           power = 0.8
##     alternative = greater
```

样本量要达到\(n=111\)。

功效随样本量变化曲线图示：

```
pwr.t.test(
  type = "one.sample", 
  alternative="greater",
  d = (7.25 - 7.0)/1.052,
  sig.level = 0.05,
  power = 0.80 ) |>
  plot()
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics_files/figure-html/stat-base-hyptest-mu1d-power23-1.png)

## 30.4 均值比较

### 30.4.1 独立两样本Z检验

假设有两个独立的总体\(X, Y\)，例如，男性与女性的寿命。
要比较两个总体的期望\(\mu\_1=EX\)和\(\mu\_2=EY\)。
设两个总体的方差分别为\(\sigma\_1^2\)和\(\sigma\_2^2\)。
可以对均值作双侧、左侧、右侧检验。
比如，双侧检验为
\[
H\_0: \mu\_1 = \mu\_2 \longleftrightarrow H\_a: \mu\_1 \neq \mu\_2
\]

如果两个总体都服从正态分布，
而且分别的方差\(\sigma\_1^2\)和\(\sigma\_2^2\)已知，
设\(X\)的样本量为\(n\_1\)的样本得到样本均值\(\bar x\)，
设\(Y\)的样本量为\(n\_2\)的样本得到样本均值\(\bar y\)，
取统计量
\[
Z = \frac{\bar x - \bar y}
{\sqrt{\frac{\sigma\_1^2}{n\_1} + \frac{\sigma\_2^2}{n\_2}}}
\]
其中分母是分子的标准误差。
当\(\mu\_1 = \mu\_2\)时\(Z \sim \text{N}(0,1)\)。

如果分布不一定是正态分布但是样本量足够大，
可以令
\[
Z = \frac{\bar x - \bar y}
{\sqrt{\frac{S\_x^2}{n\_1} + \frac{S\_y^2}{n\_2}}}
\]
当\(\mu\_1 = \mu\_2\)时\(Z\)近似服从\(\text{N}(0,1)\)。

检验问题还可以变成\(\mu\_1 - \mu\_2\)与某个\(\delta\)的比较，如
\[
H\_0: \mu\_1 \leq \mu\_2 - \delta \longleftrightarrow H\_a: \mu\_1 > \mu\_2 - \delta
\]
这个对立假设是“不次于”的结论。

可以写一个大样本方差未知或已知，
小样本独立两正态总体方差已知情况做Z检验的R函数：

```
z.test.2s <- function(
  x, y, n1=length(x), n2=length(y), delta=0,
  sigma1=sd(x), sigma2=sd(y), alternative="two.sided"){
  z <- (mean(x) - mean(y) - delta) / sqrt(sigma1^2 / n1 + sigma2^2 / n2)
  if(alternative=="two.sided"){   # 双侧检验
    pvalue <- 2*(1 - pnorm(abs(z)))
  } else if(alternative=="less"){ # 左侧检验
    pvalue <- pnorm(z)
  } else if(alternative=="greater"){ # 右侧检验
    pvalue <- 1 - pnorm(z)
  } else {
    stop("alternative unknown!")
  }
  
  c(stat=z, pvalue=pvalue)
}
```

函数允许输入两组样本到`x`和`y`中，
输入\(\sigma\_1\)和\(\sigma\_2\)到`sigma1`和`sigma2`中，
这时不输入`sigma1`和`sigma2`则从样本中计算样本统计量代替。
也允许输入\(\bar x\)和\(\bar y\)到`x`和`y`中，
输入\(\sigma\_1\)和\(\sigma\_2\)到`sigma1`和`sigma2`中，
或者输入\(S\_x\)和\(S\_y\)到`sigma1`和`sigma2`中，
输入\(n\_1\)到`n1`中，
输入\(n\_2\)到`n2`中，
计算独立两样本均值比较的\(Z\)检验。

### 30.4.2 独立两样本t检验

如果样本是小样本，但两个总体分别服从正态分布，
两个总体的方差未知，
但已知\(\sigma\_1^2 = \sigma\_2^2\)。
这时\(\sigma\_1^2 = \sigma\_2^2\)可以统一估计为
\[
S\_p^2 = \frac{1}{n\_1 + n\_2 - 2}\left( (n\_1 - 1)S\_x^2 + (n\_2 - 1)S\_y^2 \right)
\]
取检验统计量
\[
T = \frac{\bar x - \bar y}{S\_p \sqrt{\frac{1}{n\_1} + \frac{1}{n\_2}}}
\]
当\(\mu\_1 = \mu\_2\)时\(T \sim \text{t}(n\_1 + n\_2 - 2)\)。

当`x`和`y`存放了两组独立样本时，
可以用`t.test(x, y, var.equal=TRUE)`计算两样本t检验，
用`alternative=`选项指定双侧、右侧、左侧检验。

可以自己写一个这样的R函数作两样本t检验，
允许只输入统计量而非具体样本值：

```
t.test.2s <- function(
  x, y, n1=length(x), n2=length(y),
  sigma1=sd(x), sigma2=sd(y), delta=0,
  alternative="two.sided"){
  sp <- sqrt(1/(n1+n2-2) * ((n1-1)*sigma1^2 + (n2-1)*sigma2^2))
  t <- (mean(x) - mean(y) - delta) / (sp * sqrt(1 / n1 + 1 / n2))
  if(alternative=="two.sided"){   # 双侧检验
    pvalue <- 2*(1 - pt(abs(t), n1+n2-2))
  } else if(alternative=="less"){ # 左侧检验
    pvalue <- pt(t, n1+n2-2)
  } else if(alternative=="greater"){ # 右侧检验
    pvalue <- 1 - pt(t, n1+n2-2)
  } else {
    stop("alternative unknown!")
  }
  
  c(stat=t, pvalue=pvalue)
}
```

如果两个独立正态总体的方差不相等，样本量不够大，
可以用如下的在\(H\_0\)下近似服从t分布的检验统计量：
\[
T = \frac{\bar x - \bar y}{\sqrt{ \frac{S\_x^2}{n\_1} + \frac{S\_y^2}{n\_2} }}
\]
自由度为
\[
m = \frac{\left( S\_x^2 / n\_1 + S\_y^2 / n\_2 \right)^2}
{ \frac{\left( S\_x^2 / n\_1\right)^2}{n\_1 - 1}
+ \frac{\left(S\_y^2 / n\_2 \right)^2}{n\_2 - 1} }
\]
这个检验称为Welch两样本t检验，或Satterthwaite 两样本t检验
当`x`和`y`存放了两组独立样本时，
可以用`t.test(x, y, var.equal=FALSE)`计算Welch两样本t检验。

如果方差是否相等不易判断，
可以直接选用Welch检验。

因为大样本时t分布与标准正态分布基本相同，
所以非正态总体大样本Z检验也可以用`t.test()`计算。

### 30.4.3 成对均值比较问题

设总体包含两个\(X, Y\)分量，
比如，
一组病人的服药前血压与服药后血压。
这两个变量是相关的，
不能用独立两总体的均值比较方法比较\(\mu\_1=EX\)与\(\mu\_2=EY\)。

若\(\xi = X - Y\)服从正态分布，
则可以对\(\xi\)进行均值为零的单样本t检验。
这样的检验称为成对均值的t检验。

若`x`和`y`分别为\(X\)和\(Y\)的样本值，
可以用`t.test(x, y, paired=TRUE)`计算计算成对均值t检验。

### 30.4.4 均值比较的例子

#### 30.4.4.1 顾客平均年龄差别比较

HomeStyle是一个连锁家具店，
两家分店一个在城区，一个在郊区。
经理发现同一种家具有可能在一个分店卖的很好，
另一个分店则不好。
怀疑是顾客人群的人口学差异（年龄、性别、种族等）。

独立地抽取两个分店的顾客样本，比较平均年龄。
设城内的分店顾客年龄\(\sigma\_1=9\)已知，
郊区的分店顾客年龄\(\sigma\_2=10\)已知。

城内调查了\(n\_1=36\)位顾客，年龄的样本平均值\(\bar x=40\);
郊区调查了\(n\_2=49\)位顾客，年龄的样本平均值\(\bar y=35\)。
\(\bar x - \bar y = 5\)。

作双侧检验，水平\(\alpha=0.05\)。

计算\(Z\)统计量：
\[
Z = \frac{40 - 35}{\sqrt{ \frac{9^2}{36} + \frac{10^2}{49} }} = 2.4138
\]

p值为\(P(|V| \geq |2.4138|)\)，其中\(V\)为标准正态分布随机变量。
用R计算为`2*(1 - pnorm(abs(2.4138)))`=0.016，
两个分店的顾客年龄有显著差异，
城里的顾客平均年龄显著地高。

基于样本统计量用自定义的`z.test.2s()`计算：

```
z.test.2s(n1=36, x=40, sigma1=9, 
          n2=49, y=35, sigma2=10,
          alternative="two.sided")
```

```
##       stat     pvalue 
## 2.41379310 0.01578742
```

#### 30.4.4.2 两个银行营业所顾客平均存款比较

某银行要比较两个营业所的支票账户的平均存款额。
作双侧检验，水平0.05。没有方差信息时可以直接选用Welch检验。

设第一个营业所随机抽查了\(n\_1=28\)个支票账户，
均值为\(\bar x=1025\)(美元)，样本标准差为\(S\_1=150\)，
第二个营业所随机抽查了\(n\_2=22\)个支票账户，
均值为\(\bar y=910\),
样本标准差为\(S\_2=125\)。

从原始数据出发计算检验：

```
t.test(CheckAcct[[1]], CheckAcct[[2]], 
  var.equal = FALSE, alternative = "two.sided")
```

```
## 
##  Welch Two Sample t-test
## 
## data:  CheckAcct[[1]] and CheckAcct[[2]]
## t = 2.956, df = 47.805, p-value = 0.004828
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##   36.77503 193.25224
## sample estimates:
## mean of x mean of y 
## 1025.0000  909.9864
```

p值为0.005，在0.05水平下显著，
两个营业所的账户平均余额有显著差异。

用ggstatsplot包图示：

```
d <- rbind(
  data.frame(place = names(CheckAcct)[1], 
    checkings = CheckAcct[[1]]),
  data.frame(place = names(CheckAcct)[2], 
    checkings = CheckAcct[[2]]))
library(ggstatsplot)
ggbetweenstats(
  data  = d,
  x     = place,
  y     = checkings,
  title = "两个营业所支票账户存款比较"
)
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics_files/figure-html/stat-base-hyptest-mu2d-ex02b-1.png)

#### 30.4.4.3 两种工具软件的比较

考虑开发信息系统的两种工具软件的比较。
新的软件声称比旧的软件能加快进度。
设使用旧软件时平均项目完成时间为\(\mu\_1\)，
使用新软件时为\(\mu\_2\)。

检验：
\[
H\_0: \mu\_1 \leq \mu\_2 \longleftrightarrow
H\_a: \mu\_1 > \mu\_2
\]
(对立假设是新软件的工期更短)
取0.05水平。

```
t.test(SoftwareTest[['Current']], SoftwareTest[['New']],
       var.equal = FALSE, alternative = 'greater')
```

```
## 
##  Welch Two Sample t-test
## 
## data:  SoftwareTest[["Current"]] and SoftwareTest[["New"]]
## t = 2.2721, df = 21.803, p-value = 0.01665
## alternative hypothesis: true difference in means is greater than 0
## 95 percent confidence interval:
##  9.514309      Inf
## sample estimates:
## mean of x mean of y 
##       325       286
```

p值为0.017, 使用新的工具软件的平均项目完成时间显著地少于使用旧工具软件。

#### 30.4.4.4 两种工艺所需时间的比较

工厂希望比较同一生产问题两种工艺各自需要的时间，
将选用时间较短的工艺。
可以一组工人用工艺1，一组工人用工艺2，
随机分组。
两组之间工人的个体差异会使得差异的比较误差较大。

所以，让同一个工人分别采用两种工艺，
次序先后随机。
这样不会引入个体差异的影响，
精度较高。
使用成对检验。
用双侧检验，水平0.05。

```
t.test(Matched[["Method 1"]], Matched[["Method 2"]], 
       paired=TRUE, alternative = "two.sided")
```

```
## 
##  Paired t-test
## 
## data:  Matched[["Method 1"]] and Matched[["Method 2"]]
## t = 2.1958, df = 5, p-value = 0.07952
## alternative hypothesis: true mean difference is not equal to 0
## 95 percent confidence interval:
##  -0.05120834  0.65120834
## sample estimates:
## mean difference 
##             0.3
```

### 30.4.5 均值比较的样本量和功效计算

针对假定两组方差相等的独立两样本t检验，
设已知合并的标准差估计\(\sigma\)，
要检验的两组均值差的下界为\(\delta\)，
可以用`power.t.test()`估计需要的样本量，如：

```
power.t.test(
  delta = 2.5, sd = 1.5, 
  type = "two.sample",
  alternative = "two.sided",
  sig.level=0.05, 
  power=0.90)
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 8.649245
##           delta = 2.5
##              sd = 1.5
##       sig.level = 0.05
##           power = 0.9
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

这里输出的样本量是两组各自的样本量，
假定两组样本量相等。
结果表明两组都至少要有\(n=9\)个观测。

对于成对t检验，
只要将`type`输入为`"paired"`，
因为成对t检验实际上是对两个变量的差值进行检验，
所以`sigma`参数输入的是差值的标准差。

pwr包可以用来计算功效和样本量。
对于独立两样本比较问题，
定义要检验的效应大小：
\[
d = \frac{\mu\_2 - \mu\_1}{\sigma},
\]
其中\(\mu\_1\), \(\mu\_2\)是两组的均值，
\(\sigma\)是两组共同的标准差，
\(d\)为效应大小。
给定检验水平、单双侧、效应大小，
功效与样本量可以互相唯一决定。

常用的\(d\)值有：

* 小：\(d=0.2\);
* 中: \(d=0.5\);
* 大: \(d=0.8\)。

**例30.2** 考虑两个银行营业所顾客平均存款比较问题，
用独立两样本t检验。

以估计的均值为两组均值，
估计的合并标准差为\(\sigma\)，
计算效应大小：

```
x <- na.omit(CheckAcct[[1]])
y <- na.omit(CheckAcct[[2]])
d <- (mean(y) - mean(x)) / sd(c(x, y)); d
```

```
## [1] -0.7681087
```

计算两组各\(n = (28+22)/2 = 25\)个观测时的功效：

```
pwr.t.test(
  type="two.sample",
  alternative="two.sided",
  d = -0.7681,
  sig.level = 0.05,
  n = 25 )
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 25
##               d = 0.7681
##       sig.level = 0.05
##           power = 0.7583484
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

功效为\(76\%\)。

计算功效达到\(80\%\)所需样本量：

```
pwr.t.test(
  type="two.sample",
  alternative="two.sided",
  d = -0.7681,
  sig.level = 0.05,
  power = 0.80 )
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 27.60138
##               d = 0.7681
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

功效\(80\%\)需要每组有\(28\)个观测。

但是，在这里我们用了观测到的两组均值的差作为我们要发现的差别。
实际上，
我们可能需要发现更小一些的效应，这时需要更大的样本量。
比如，
指定Cohen规定的“中等”效应：

```
cohen.ES(test = "t", size = "medium")
```

```
## 
##      Conventional effect size from Cohen (1982) 
## 
##            test = t
##            size = medium
##     effect.size = 0.5
```

```
pwr.t.test(
  type="two.sample",
  alternative="two.sided",
  d = 0.5,
  sig.level = 0.05,
  power = 0.80 )
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 63.76561
##               d = 0.5
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

要发现中等的效应，
需要将样本量增大到每组各64个观测。

当要比较的两组的样本量不同时可以用`pwr.t2n.test()`计算功效，
输入`n1`和`n2`两个样本量。
比如，\(n\_1 = 28\), \(n\_2=22\)，
比较中等的均值差异效应：

```
pwr.t2n.test(
  alternative="two.sided",
  d = 0.5,
  sig.level = 0.05,
  n1 = 28, n2 = 22 )
```

```
## 
##      t test power calculation 
## 
##              n1 = 28
##              n2 = 22
##               d = 0.5
##       sig.level = 0.05
##           power = 0.4052476
##     alternative = two.sided
```

功效仅有\(41\%\)。

### 30.4.6 多元均值置信域

待补充。

## 30.5 单个比例的假设检验和置信区间

### 30.5.1 方法

设要比较的是总体的某个比例，
如选民中对候选人甲的支持率。
这时总体是两点分布总体，
支持为1， 不支持为0，参数为\(p=P(X=1)\)。
其它的比例问题类似，
\(p\)是“成功概率”。

关于单总体的比例，也有双侧、右侧、左侧检验问题：
\[\begin{aligned}
& H\_0: p=p\_0 \longleftrightarrow H\_a: p \neq p\_0 \\
& H\_0: p \leq p\_0 \longleftrightarrow H\_a: p > p\_0 \\
& H\_0: p \geq p\_0 \longleftrightarrow H\_a: p < p\_0
\end{aligned}\]

在R中，
大样本情况下可以用`prop.test(x, n, p=p0)`作单个比例的检验，
`n`是样本量，`x`是样本中符合条件（如支持）的个数，
检验使用近似卡方统计量，
用`alternative`选项选择双侧、右侧、左侧检验。
这个函数也给出比例\(p\)的近似\(1-\alpha\)置信区间，
用`conf.level`选项指定置信度。

大样本时还有
\[
\frac{\hat p - p}{\sqrt{p(1-p)/n}}
\]
近似服从标准正态分布，
可以据此计算Z检验。
自定义的R函数如下：

```
prop.test.1s <- function(x, n, p=0.5, alternative="two.sided"){
  phat <- x/n
  zstat <- (phat - p)/sqrt(p*(1-p)/n)
  if(alternative=="two.sided"){   # 双侧检验
    pvalue <- 2*(1 - pnorm(abs(zstat)))
  } else if(alternative=="less"){ # 左侧检验
    pvalue <- pnorm(zstat)
  } else if(alternative=="greater"){ # 右侧检验
    pvalue <- 1 - pnorm(zstat)
  } else {
    stop("alternative unknown!")
  }
  
  c(stat=zstat, pvalue=pvalue)
}
```

因为比例问题本质上是一个二项分布推断问题，
所以R还提供了`binom.test(x, n, p=p0)`函数进行精确检验并计算精确置信区间，
但其结果略保守。
用法与`prop.test()`类似。

### 30.5.2 高尔夫培训女生比例检验例子

橡树溪高尔夫培训机构的女性学员比例较少，只有20%。
为了增加女性学员，设计了促销措施。一个月后调查，
希望证明女性学员比例增加了。

设女性学员比例为\(p\)，
\[
H\_0: p \leq 0.20 \longleftrightarrow H\_a: p > 0.20
\]

取检验水平0.05。
抽查了400名学员，其中100名是女性学员，\(\hat p = 0.25\)。

用`prop.test()`检验：

```
prop.test(100, 400, p=0.20, alternative = "greater")
```

```
## 
##  1-sample proportions test with continuity correction
## 
## data:  100 out of 400, null probability 0.2
## X-squared = 5.9414, df = 1, p-value = 0.007395
## alternative hypothesis: true p is greater than 0.2
## 95 percent confidence interval:
##  0.2149649 1.0000000
## sample estimates:
##    p 
## 0.25
```

检验p值为0.0074，小于检验水平0.05，
所以女性学员比例已经显著地高于过去的20%了。

用自定义的大样本Z检验函数`prop.test.1s()`:

```
prop.test.1s(100, 400, p=0.20, alternative = "greater")
```

```
##        stat      pvalue 
## 2.500000000 0.006209665
```

检验p值为0.0062，与基于卡方检验的`prop.test()`结果略有差别。

用基于二项分布的`binom.test()`检验:

```
binom.test(100, 400, p=0.20, alternative = "greater")
```

```
## 
##  Exact binomial test
## 
## data:  100 and 400
## number of successes = 100, number of trials = 400, p-value = 0.008595
## alternative hypothesis: true probability of success is greater than 0.2
## 95 percent confidence interval:
##  0.2146019 1.0000000
## sample estimates:
## probability of success 
##                   0.25
```

p值为0.0086。

### 30.5.3 单个比例检验的样本量和功效

pwr包的`pwr.p.test()`函数可以进行单个比例假设检验的样本量和功效计算。
该函数用参数`h`指定要检验的比例差异大小，
这个比例差异的效应大小用`ES.h()`函数进行计算。
比如，\(H\_0: p = 0.20\), \(H\_a: p = 0.25\)，
则比例差异的效应大小为：

```
ES.h(p1 = 0.25, p2 = 0.20)
```

```
## [1] 0.1199023
```

效应大小的计算公式为(([Cohen 1988](#ref-Cohen1988:PowerAna)))：
\[\begin{align}
h = 2 \arcsin \sqrt{p\_1} - 2 \arcsin \sqrt{p\_2} .
\tag{30.1}
\end{align}\]

比例差异的常用值为：

* 小(small): \(h = 0.2\);
* 中(medium): \(h = 0.5\);
* 大(large): \(h = 0.8\)。

公式[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics.html#eq:stat-base-hyptest-prop1s-power-h)定义的比例差异效应大小与\(p\_1\)和\(p\_2\)的具体值有关系。
比如，
同样是\(p\_1 - p\_2 = 0.05\)：

```
ES.h(p1 = 0.10, p2 = 0.05)
```

```
## [1] 0.1924743
```

这个效应比\(0.20\)和\(0.25\)的差异效应大了许多。
公式[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics.html#eq:stat-base-hyptest-prop1s-power-h)考虑到了两个比例的相对差距，
而不仅是绝对差距。

给定效应大小、检验水平后，
可以给定样本量计算功效，
如：

```
pwr.p.test(
  h = ES.h(p1 = 0.25, p2 = 0.20),
  sig.level = 0.05,
  alternative = "greater",
  n = 400)
```

```
## 
##      proportion power calculation for binomial distribution (arcsine transformation) 
## 
##               h = 0.1199023
##               n = 400
##       sig.level = 0.05
##           power = 0.774333
##     alternative = greater
```

功效为\(77\%\)。
计算达到\(80\%\)功效所需的样本量：

```
pwr.p.test(
  h = ES.h(p1 = 0.25, p2 = 0.20),
  sig.level = 0.05,
  alternative = "greater",
  power = 0.80)
```

```
## 
##      proportion power calculation for binomial distribution (arcsine transformation) 
## 
##               h = 0.1199023
##               n = 430.044
##       sig.level = 0.05
##           power = 0.8
##     alternative = greater
```

所需样本量为\(431\)。

还可以图示功效与样本量的关系曲线：

```
pwr.p.test(
  h = ES.h(p1 = 0.25, p2 = 0.20),
  sig.level = 0.05,
  alternative = "greater",
  power = 0.80) |>
  plot()
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics_files/figure-html/stat-base-hyptest-prop1s-power14-1.png)

## 30.6 两个比例的比较

### 30.6.1 方法

设有\(X\)和\(Y\)两个独立的总体，
如男性和女性，
比较\(p\_1 = P(X=1)\)和\(p\_2 = P(Y=1)\)。

检验问题包括：
\[\begin{aligned}
H\_0: p\_1 = p\_2 & \longleftrightarrow H\_\text{a}: p\_1 \neq p\_2 \\
H\_0: p\_1 \leq p\_2 & \longleftrightarrow H\_\text{a}: p\_1 > p\_2 \\
H\_0: p\_1 \geq p\_2 & \longleftrightarrow H\_\text{a}: p\_1 < p\_2
\end{aligned}\]

在大样本情况下，
可以用R函数
`prop.test(c(nsucc1, nsucc2), c(n1,n2), alternative=...)`
进行独立两样本比例的比较检验，
其中`n1`和`n2`是两个样本量，
`nsucc1`和`nsucc2`是两个样本中符合特征的个数（个数除以样本量即样本中的比例），
`alternative`的选择也是`"two.sided"`、`"less"`、`"greater"`。

还可以比较\(p\_1 - p\_2\)与某个\(\delta\)。
设两个总体中分别抽取了\(n\_1\)和\(n\_2\)个样本点，
样本比例分别为\(\hat p\_1\)和\(\hat p\_2\)。
估计共同的样本比例
\[
\hat p = \frac{n\_1 \hat p\_1 + n\_2 \hat p\_2}{n\_1 + n\_2}
\]
当\(\delta=0\)时取统计量
\[
Z = \frac{\hat p\_1 - \hat p\_2}
{\sqrt{\hat p (1 - \hat p)
\left( \frac{1}{n\_1} + \frac{1}{n\_2} \right)}}
\]
当\(p\_1 = p\_2\)时\(Z\)在大样本情况下近似服从标准正态分布。
当\(\delta\neq 0\)时取统计量
\[
Z = \frac{\hat p\_1 - \hat p\_2 - \delta}
{\sqrt{\frac{\hat p\_1(1 - \hat p\_1)}{n\_1} + \frac{\hat p\_2(1 - \hat p\_2)}{n\_2}}}
\]
当\(p\_1 - p\_2 = \delta\)时\(Z\)在大样本情况下近似服从N(0,1)分布。

可以定义Z检验R函数为：

```
prop.test.2s <- function(x, n, delta=0.0, alternative="two.sided"){
  phat <- sum(x)/sum(n)
  p <- x / n
  if(delta==0.0){
    zstat <- (p[1] - p[2])/sqrt(phat*(1-phat)*(1/n[1] + 1/n[2]))
  } else {
    zstat <- (p[1] - p[2] - delta)/sqrt(p[1]*(1-p[1])/n[1] + p[2]*(1-p[2])/n[2])
  }
  if(alternative=="two.sided"){   # 双侧检验
    pvalue <- 2*(1 - pnorm(abs(zstat)))
  } else if(alternative=="less"){ # 左侧检验
    pvalue <- pnorm(zstat)
  } else if(alternative=="greater"){ # 右侧检验
    pvalue <- 1 - pnorm(zstat)
  } else {
    stop("alternative unknown!")
  }
  
  c(stat=zstat, pvalue=pvalue)
}
```

独立两总体比例比较的小样本情形意义不大，
可考虑使用Fisher精确检验，
R函数为`fisher.test()`。

### 30.6.2 报税代理分理处的错误率比较

某个代理报税的机构希望比较下属的两个分理处申报纳税返还的出错率。
各自随机选取取了\(n\_1=250\)和\(n\_2=300\)件申报，
第一个分理处错了35件，错误率\(\hat p\_1 = 0.14\);
第二个分理处错了27件，错误率\(\hat p\_2 = 0.09\)。

作双侧检验，水平0.10。

用`prop.test()`:

```
prop.test(c(35,27), c(250,300), alternative = "two.sided")
```

```
## 
##  2-sample test for equality of proportions with continuity correction
## 
## data:  c(35, 27) out of c(250, 300)
## X-squared = 2.9268, df = 1, p-value = 0.08712
## alternative hypothesis: two.sided
## 95 percent confidence interval:
##  -0.007506845  0.107506845
## sample estimates:
## prop 1 prop 2 
##   0.14   0.09
```

p值0.087，有显著差异，第一处的错误率高。
因为比例需要较大样本量，
所以样本量不太大时，
有人用0.10这样的检验水平。

用自定义的`prop.test.2s()`:

```
prop.test.2s(c(35,27), c(250,300), alternative = "two.sided")
```

```
##       stat     pvalue 
## 1.84618928 0.06486473
```

使用Fisher精确检验：

```
fisher.test(
  rbind(c(35, 250-35),
        c(27, 300-27)), 
  alternative = "two.sided")
```

```
## 
##  Fisher's Exact Test for Count Data
## 
## data:  rbind(c(35, 250 - 35), c(27, 300 - 27))
## p-value = 0.07809
## alternative hypothesis: true odds ratio is not equal to 1
## 95 percent confidence interval:
##  0.9345221 2.9208940
## sample estimates:
## odds ratio 
##   1.644457
```

`fisher.test()`要求输入\(2 \times 2\)矩阵，
两行分别为两组的成功数和失败数。
上面的检验结果p值为0.078，
在0.10水平下两个比例有显著差异。

### 30.6.3 比例比较的样本量

如果预先近似估计了两个总体的比例分别约为\(p\_1\)和\(p\_2\)，
则检验两个比例有无显著差距所需的样本量可以利用`power.prop.test()`计算，
如：

```
power.prop.test(
  power = 0.80, 
  sig.level = 0.05,
  p1 = 0.15, p2 = 0.30 )
```

```
## 
##      Two-sample comparison of proportions power calculation 
## 
##               n = 120.4719
##              p1 = 0.15
##              p2 = 0.3
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

输出的样本量是每组需要的样本量。
结果表明每组需要\(n=121\)个观测。

也可以给定样本量计算功效，如：

```
power.prop.test(
  n = 100, 
  sig.level = 0.05,
  p1 = 0.15, p2 = 0.30 )
```

```
## 
##      Two-sample comparison of proportions power calculation 
## 
##               n = 100
##              p1 = 0.15
##              p2 = 0.3
##       sig.level = 0.05
##           power = 0.7222795
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

功效为\(72\%\)。

pwr包的`pwr.2p.test()`函数可以进行比例比较的样本量和功效计算。
如：

```
pwr.2p.test(
  h = ES.h(p1 = 0.30, p2 = 0.15),
  power = 0.80, 
  sig.level = 0.05)
```

```
## 
##      Difference of proportion power calculation for binomial distribution (arcsine transformation) 
## 
##               h = 0.3638807
##               n = 118.5547
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: same sample sizes
```

需要各自\(119\)个观测。

计算中等比例差异效应的样本量：

```
pwr.2p.test(
  h = 0.5,
  power = 0.80, 
  sig.level = 0.05)
```

```
## 
##      Difference of proportion power calculation for binomial distribution (arcsine transformation) 
## 
##               h = 0.5
##               n = 62.79088
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: same sample sizes
```

需要各自\(63\)个观测值。
注意需要检测的比例差异效应越大，
需要的样本量就越小。

实际上，
当要比较的两个样本样本量不同时，
可以用`pwr.2p2n.test()`函数来计算功效，
将`n=`选项替换成`n1=`和`n2=`选项。
比如，
\(n\_1=250\), \(n\_2=300\)，
检验小的比例差异效应：

```
pwr.2p2n.test(
  h = 0.2,
  n1 = 250, n2 = 300,
  sig.level = 0.05)
```

```
## 
##      difference of proportion power calculation for binomial distribution (arcsine transformation) 
## 
##               h = 0.2
##              n1 = 250
##              n2 = 300
##       sig.level = 0.05
##           power = 0.6463766
##     alternative = two.sided
## 
## NOTE: different sample sizes
```

也可以给定功效以及`n1`，求`n2`:

```
pwr.2p2n.test(
  h = 0.2,
  n1 = 250, 
  power = 0.8,
  sig.level = 0.05)
```

```
## 
##      difference of proportion power calculation for binomial distribution (arcsine transformation) 
## 
##               h = 0.2
##              n1 = 250
##              n2 = 912.1748
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: different sample sizes
```

\(n\_2\)为\(913\)。

## 30.7 方差的假设检验和置信区间

### 30.7.1 单总体方差的假设检验和置信区间

方差和标准差是总体的重要数字特征，
在金融应用中标准差代表波动率，
在工业生产中标准差也是重要的质量指标，
如机床加工精度可以用标准差表示。

对总体方差\(\sigma^2\)可以做双侧、左侧、右侧检验，
与某个\(\sigma\_0^2\)比较。

设总体为正态总体，检验问题：
\[\begin{aligned}
H\_0:& \sigma^2 = \sigma\_0^2 \longleftrightarrow
H\_\text{a}: \sigma^2 \neq \sigma\_0^2 \\
H\_0:& \sigma^2 \geq \sigma\_0^2 \longleftrightarrow
H\_\text{a}: \sigma^2 < \sigma\_0^2 \\
H\_0:& \sigma^2 \leq \sigma\_0^2 \longleftrightarrow
H\_\text{a}: \sigma^2 > \sigma\_0^2
\end{aligned}\]

可以用如下的自定义函数计算单正态总体方差的检验：

```
var.test.1s <- function(x, n=length(x), var0, alternative="two.sided"){
  if(length(x)==1){ # 输入的是方差
    varx <- x
  } else {
    varx <- var(x)
  }
  xi <- (n-1)*varx/var0
  
  if(alternative=="less"){
    pvalue <- pchisq(xi, n-1)
  } else if (alternative=="right"){
    pvalue <- pchisq(xi, n-1, lower.tail=FALSE)
  } else if(alternative=="two.sided"){
    pvalue <- 2*min(pchisq(xi, n-1), pchisq(xi, n-1, lower.tail=FALSE))
  }
  
  c(statistic=xi, pvalue=pvalue)
}
```

可以输入样本数据到`x`中，
输入\(\sigma\_0^2\)到`var0`中，
进行检验；
也可以输入\(S^2\)到`x`中，输入样本量\(n\)到`n`中，
输入\(\sigma\_0^2\)到`var0`中，
进行检验。

### 30.7.2 独立两总体方差的比较

设两个独立的正态总体的方差分别为\(\sigma\_1^2\), \(\sigma\_2^2\)，
有双侧、左侧、右侧检验：
\[\begin{aligned}
H\_0:& \sigma\_1^2 = \sigma\_2^2 \longleftrightarrow
H\_\text{a}: \sigma\_1^2 \neq \sigma\_2^2 \\
H\_0:& \sigma\_1^2 \geq \sigma\_2^2 \longleftrightarrow
H\_\text{a}: \sigma\_1^2 < \sigma\_2^2 \\
H\_0:& \sigma\_1^2 \leq \sigma\_2^2 \longleftrightarrow
H\_\text{a}: \sigma\_1^2 > \sigma\_2^2
\end{aligned}\]

设两个总体分别抽取\(n\_1\)和\(n\_2\)个样本点，
样本方差分别为\(S\_x^2\)和\(S\_y^2\)。

检验统计量
\[
F = \frac{S\_x^2}{S\_y^2}
\]
在\(\sigma\_1^2 = \sigma\_2^2\)时\(F\)服从\(F(n\_1 - 1, n\_2 - 1)\)分布。

如果输入`x`, `y`为两个样本的具体值，
可以用`var.test(x, y, alternative=...)`检验，
其中`alternative`的选择是`"two.sided"`、`"less"`、`"greater"`。

`var.test()`假定总体都服从正态分布。
R中`mood.test()`是一种不要求正态分布假定的检验方法。
`bartlett.test()`检验多个独立正态总体方差是否全相等。

### 30.7.3 方差检验例子

**例**：
某车间生产铜丝，
生产一向比较稳定。
为了检查生产工艺是否受控，
从产品中随机抽取10根铜丝检查其折断力，
得数据如下：
\[
578, 572, 570, 568, 572,
570, 570, 572, 596, 584.
\]
问：从样本看，
能否认为折断力指标的标准差保持在以往的\(8.0\)水平？

用前面的自定义函数进行方差的卡方检验，
作双侧检验：

```
var.test.1s(x = c(
  578, 572, 570, 568, 572,
  570, 570, 572, 596, 584),
  var0 = 64,
  alternative = "two.sided")
```

```
##  statistic     pvalue 
## 10.6500000  0.6009287
```

检验p值为\(0.60\)，
在\(0.05\)水平下不拒绝零假设，
可认为生产工艺水平受控。

**例**：
某车间要比较在两种不同温度下生产的针织品断裂强度。
设两种温度下分别抽取的产品样品断裂强度如下：
\[
\begin{matrix}
\text{70℃} &
20.5, 18.8, 19.8, 20.9, 21.5, 19.5, 21.0, 21.2 \\
\text{80℃} &
17.7, 20.3, 20.0, 18.8, 19.0, 20.1, 20.2, 19.1
\end{matrix}
\]
问：两种温度下断裂强度的方差有误显著差异？

用`var.test()`：

```
var.test(c(
  20.5, 18.8, 19.8, 20.9, 21.5, 19.5, 21.0, 21.2), c(
  17.7, 20.3, 20.0, 18.8, 19.0, 20.1, 20.2, 19.1),
  alternative = "two.sided")
```

```
## 
##  F test to compare two variances
## 
## data:  c(20.5, 18.8, 19.8, 20.9, 21.5, 19.5, 21, 21.2) and c(17.7, 20.3, 20, 18.8, 19, 20.1, 20.2, 19.1)
## F = 1.069, num df = 7, denom df = 7, p-value = 0.9322
## alternative hypothesis: true ratio of variances is not equal to 1
## 95 percent confidence interval:
##  0.214011 5.339386
## sample estimates:
## ratio of variances 
##           1.068966
```

检验\(p\)值为\(0.93\)，
在\(0.05\)水平下不拒绝零假设，
认为两种温度下的断裂强度的方差没有显著差异。

## 30.8 拟合优度检验

### 30.8.1 各类比例相等的检验

设类别变量\(X\)有\(m\)个不同类别，
抽取\(n\)个样本点后，
各类的频数分别为\(f\_1, f\_2, \dots, f\_m\)。

零假设为所有类的比例相同；
对立假设为各类比例不完全相同。

在零假设下，每个类的期望频数为\(n/m\)。

检验统计量为
\[
\chi^2 = \sum\_{i=1}^m \frac{(f\_i - \frac{n}{m})^2}{\frac{n}{m}}
\]
当\(\chi^2\)超过某一临界值时拒绝零假设。
在\(H\_0\)成立且大样本情况下\(\chi^2\)统计量近似服从\(\chi^2(m-1)\)分布
（自由度为组数减1）。

设`x`中包含各类的频数，
R函数`chisq.test(x)`作各类的总体比例相等的拟合优度卡方检验。

**例**：
投掷一枚骰子1000次，
一点到六点的出现次数分别为：
168, 159, 168, 180, 167, 158，
问这枚骰子是否均匀？

**解答**：
利用`chisq.test()`函数，得

```
chisq.test(c(168, 159, 168, 180, 167, 158))
```

```
## 
##  Chi-squared test for given probabilities
## 
## data:  c(168, 159, 168, 180, 167, 158)
## X-squared = 1.892, df = 5, p-value = 0.8639
```

卡方统计量等于1.892, 零假设下服从\(\chi^2(5)\)分布，
p值为0.8639，
结果不显著，
不拒绝零假设，
认为六个面的概率相等，
骰子是均匀的。

```
library(ggstatsplot)
ggpiestats(
  data         = data.frame(face=1:6, 
                 counts=c(168, 159, 168, 180, 167, 158)),
  x            = face,
  counts       = counts,
  title        = "Dice equality"
)
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-basics_files/figure-html/stat-base-hyptest-gof-onepcteq02-1.png)

### 30.8.2 各类比例为指定值的检验

设分类变量\(X\)的各类每类有一个总体比例的假设，
希望作为零假设检验。

设共有\(m\)个类，在零假设中，
各类的比例分别为\(p\_1, p\_2, \dots, p\_m\)。

取了\(n\)个样本点组成的样本，
各类的频数分别为\(f\_1, f\_2, \dots, f\_m\)，
期望频数分别为\(np\_1, np\_2, \dots, np\_m\)。

检验统计量为
\[
\chi^2 = \sum\_{i=1}^m \frac{(f\_i - n p\_i)^2}{n p\_i}
\]
在\(H\_0\)下成立且大样本情况下\(\chi^2\)近似服从\(\chi^2(m-1)\)，
自由度为组数减1。
因为是大样本检验法，
每个组的期望频数不能低于5，
否则可以合并较小的类。

设`x`中包含各类的频数，
`p`中包含\(H\_0\)假定的各类的概率，
R函数`chisq.test(x, p)`作各类的总体比例为指定概率的拟合优度卡方检验。

**例**: 市场占有率的检验例子

某类产品的当前市场占比为公司A占30%, 公司B占50%，公司C占20%。
近期C公司推出了新产品代替本公司的老产品，
公司C委托一个咨询公司调查分析新产品是否导致了市场占有率的变动。
抽查了200位消费者，
购买三个公司的产品的人数分配为：
\[
(48, 98, 54) .
\]

设\(p\_1, p\_2, p\_3\)分别为三个公司现在的市场占有率，零假设为:
\[ H\_0: p\_1 = 0.30, \ p\_2 = 0.50, \ p\_3 = 0.20 \]
取检验水平0.05。

卡方统计量
\[
\chi^2 = \frac{(48 - 200\times 0.3)^2}{200\times 0.3}
+ \frac{(98 - 200\times 0.5)^2}{200\times 0.5}
+ \frac{(54 - 200\times 0.2)^2}{200\times 0.2} = 7.34
\]

p值\(=P(\chi^2(3-1) > 7.34) = 0.02548\)，
拒绝零假设，
认为市场占有率有变化。

用`chisq.test()`计算：

```
chisq.test(c(48, 98, 54), p=c(0.3, 0.5, 0.2))
```

```
## 
##  Chi-squared test for given probabilities
## 
## data:  c(48, 98, 54)
## X-squared = 7.34, df = 2, p-value = 0.02548
```

p值为0.025，在0.05水平下显著，
可以认为市场占有率有显著变化。

注意，
因为卡方检验本身也是利用了大样本近似分布，
所以p值结果本身就是近似地，
一般仅使用一到两位有效数字，
不要报告过多有效数字。

### 30.8.3 带有未知参数的单分类变量的拟合优度假设检验

如果\(H\_0\)下的概率有未知参数，
先用最大似然估计法估计未知参数，
然后将未知参数代入计算各类的概率，
从而计算期望频数，
计算卡方统计量。
求\(p\)值时自由度要改为“组数减1减去未知参数个数”。

**例**：
设某次选举有5位候选人，
其中呼声最高的是甲和乙。
问：甲、乙的支持率相等吗？

这不能用独立的两个比例的比较来解决，
因为这两个比例是互斥的，
不属于独立情况。

将类别简化为：甲，乙，其他。
零假设为“甲、乙的支持率都等于\(p\)，
其他三位候选人的支持率为\(1-2p\)，\(p\)未知”；
对立假设是甲、乙的支持率不相等。

假设调查了1000位选民，
其中300名支持甲，200名支持乙，500名支持其他三位候选人。

在假定零假设成立的情况下先用最大似然估计法估计未知参数\(p\)：
\(\hat p = (300 + 200)/1000/2 = 0.25\)。
这样，三个类的概率估计为\((0.25, 0.25, 0.50)\)。

然后，计算检验统计量：

```
chisq.test(c(300, 200, 500), c(0.25, 0.25, 0.50))
```

```
## Warning in chisq.test(c(300, 200, 500), c(0.25, 0.25, 0.5)): Chi-squared
## approximation may be incorrect
```

```
## 
##  Pearson's Chi-squared test
## 
## data:  c(300, 200, 500) and c(0.25, 0.25, 0.5)
## X-squared = 3, df = 2, p-value = 0.2231
```

其中的\(p\)值所用的自由度错误，我们需要自己计算p值：

```
res <- chisq.test(c(300, 200, 500), c(0.25, 0.25, 0.50))
```

```
## Warning in chisq.test(c(300, 200, 500), c(0.25, 0.25, 0.5)): Chi-squared
## approximation may be incorrect
```

```
c(statistic=res$statistic, 
  pvalue=pchisq(res$statistic, res$parameter - 1, 
                lower.tail=FALSE))
```

```
## statistic.X-squared    pvalue.X-squared 
##          3.00000000          0.08326452
```

检验\(p\)值为0.08而非原来的0.22。
在0.05水平下不显著，
结论为两个候选人的支持率没有显著差异。

### 30.8.4 拟合优度检验的功效和样本量

pwr包可以计算拟合优度检验的功效和样本量。

#### 30.8.4.1 市场占有率例子

考虑前面的市场占有率的例子。
零假设为：
\[
H\_0: p\_1 = 0.30, \ p\_2 = 0.50, \ p\_3 = 0.20.
\]
设要计算功效的对立假设的具体情况是:
\[
H\_1: p\_1 = 0.24, \ p\_2 = 0.49, \ p\_3 = 0.27.
\]
据此先计算卡方检验的效应，
使用`ES.w1()`函数：

```
null <- c(0.30, 0.50, 0.20)
alt <- c(0.24, 0.49, 0.27)
ES.w1(null, alt)
```

```
## [1] 0.1915724
```

卡方检验效应大小的常用取值为：

* 小：\(0.1\);
* 中: \(0.3\);
* 大: \(0.5\)。

所以\(0.19\)属于中小规模的效应，需要较大样本量。
计算功效大小：

```
pwr.chisq.test(
  w = ES.w1(null, alt),
  N = 200,
  df = 3-1,
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.1915724
##               N = 200
##              df = 2
##       sig.level = 0.05
##           power = 0.677598
## 
## NOTE: N is the number of observations
```

功效为\(68\%\)。
计算达到\(80\%\)功效所需样本量：

```
pwr.chisq.test(
  w = ES.w1(null, alt),
  power = 0.80,
  df = 4-1,
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.1915724
##               N = 297.0726
##              df = 3
##       sig.level = 0.05
##           power = 0.8
## 
## NOTE: N is the number of observations
```

样本量为\(297\)。

计算中等大小的效应以及\(80\%\)功效所需样本量：

```
cohen.ES(test = "chisq", size = "medium")
```

```
## 
##      Conventional effect size from Cohen (1982) 
## 
##            test = chisq
##            size = medium
##     effect.size = 0.3
```

```
pwr.chisq.test(
  w = 0.3,
  power = 0.80,
  df = 4-1,
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.3
##               N = 121.1396
##              df = 3
##       sig.level = 0.05
##           power = 0.8
## 
## NOTE: N is the number of observations
```

中等的效应大小只需要\(122\)个观测。

#### 30.8.4.2 候选人例子

考虑前面的5个候选人的例子。
将第3、4、5位候选人合并为一类。
零假设为:
\[
H\_0: p\_1 = p\_2 = p,
p\_3 = 1 - 2p.
\]
其中\(p\)是未知参数。
为了计算功效，
需要给出对立假设下的一个具体分布。
这里仅指定要检验中等的效应。
于是功效为：

```
pwr.chisq.test(
  w = 0.3,
  N = 1000,
  df = 3-1-1,
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.3
##               N = 1000
##              df = 1
##       sig.level = 0.05
##           power = 1
## 
## NOTE: N is the number of observations
```

功效为\(100\%\)，
所以中等大小的效应用\(1000\)个样本点过于充裕了。

如果按实际数据作为对立假设下的实际分布，
对应的效应大小为：

```
null <- c(0.25, 0.25, 0.50)
alt <- c(0.30, 0.30, 0.50)
ES.w1(null, alt)
```

```
## [1] 0.1414214
```

这也对应于较小的效应，
意味着较低的功效或者较大的样本量。
比如，
功效为：

```
pwr.chisq.test(
  w = ES.w1(null, alt),
  N = 1000,
  df = 3-1-1,
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.1414214
##               N = 1000
##              df = 1
##       sig.level = 0.05
##           power = 0.9940005
## 
## NOTE: N is the number of observations
```

因为样本量很大，所以功效达到了\(99\%\)。
计算达到\(80\%\)功效所需样本量：

```
pwr.chisq.test(
  w = ES.w1(null, alt),
  power = 0.80,
  df = 3-1-1,
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.1414214
##               N = 392.443
##              df = 1
##       sig.level = 0.05
##           power = 0.8
## 
## NOTE: N is the number of observations
```

需要的样本量为\(393\)。

## 30.9 检验分布类型

vcd包提供了一个goodfit函数，
可以用来拟合指定的某种理论分布(包括泊松、二项、负二项分布），
并检验服从该理论分布的零假设。

例如，我们生成一组速率参数为2的泊松随机数，
检验其分布是否泊松分布：

```
library(vcd)
```

```
## Loading required package: grid
```

```
set.seed(101)
datax <- rpois(100, 2)
summary(goodfit(datax, "poisson"))
```

```
## 
##   Goodness-of-fit test for poisson distribution
## 
##                       X^2 df  P(> X^2)
## Likelihood Ratio 4.289456  5 0.5085374
```

检验其是否服从二项分布，取二项分布试验数为10：

```
summary(goodfit(datax, "binomial", par = list(size = 10)))
```

```
## 
##   Goodness-of-fit test for binomial distribution
## 
##                       X^2 df  P(> X^2)
## Likelihood Ratio 5.052451  5 0.4095126
```

与二项分布也相容。
在检验分布类型时，
这种两种分布都不能拒绝的情况是常见的。

## 30.10 列联表独立性卡方检验

### 30.10.1 检验方法

设分类变量\(X\)有\(m\)个类，
分类变量\(Y\)有\(k\)个类，
**随机**抽取了样本量为\(n\)的样本，
设\(X\)的第\(i\)类与\(Y\)的第\(j\)类交叉的频数为\(f\_{ij}\)。

零假设为\(X\)与\(Y\)相互独立，
即行变量与列变量相互独立。

交叉频数列成列联表形式，第\(i\)行第\(j\)列为\(f\_{ij}\)。
共有\(mk\)个单元格，
将这\(mk\)个单元格的频数值看成是来自有\(mk\)个类别的多项分布的样本的频数结果。
注意，
如果样本不是随机抽取，
而是按照\(X\)或者安装\(Y\)分类观察或者试验得到的，
结果就不能看作是来自\(mk\)个类别的多项分布样本，
这里的方法就不适用。

第\(i\)行的行和为\(f\_{i\cdot}\),
\(X\)的第\(i\)类的百分比为\(r\_{i} = f\_{i\cdot}/n\);

第\(j\)列的列和为\(f\_{\cdot j}\),
\(Y\)的第\(j\)类的百分比为\(c\_{j} = f\_{\cdot j}/n\)。

当\(X\), \(Y\)独立时，
\((i,j)\)格子的期望频数为
\(E\_{ij} = n \times r\_i \times c\_j\)。

列联表独立性检验统计量

\[
\chi^2 = \sum\_{i=1}^m \sum\_{j=1}^k \frac{(f\_{ij} - E\_{ij})^2}{E\_{ij}}
\]

其中\(E\_{ij} = n r\_i c\_j\)，
\(r\_i\)为\(X\)第\(i\)类的百分比，
\(c\_j\)为\(Y\)第\(j\)类的百分比。

在\(H\_0\)下\(\chi^2\)近似服从\((m-1)(k-1)\)自由度的卡方分布。
设统计量值为\(c\)，
p值为\(P(\chi^2((m-1)(k-1)) > c)\)。

如果`x`, `y`分别是两个变量的原始观测值，
`chisq.test(x, y)`可以做列联表独立性检验。
如果`x`保存了矩阵格式的列联表，
矩阵行名是\(X\)各个类的名称，
矩阵列名是\(Y\)各个类的名称，
则`chisq.test(x)`可以做列联表独立性检验。

列联表卡方检验法的检验统计量在零假设下的卡方分布是大样本情况的近似分布。
如果每个变量仅有两个类，每个类的期望频数不能少于5;
如果有多个单元格，期望频数少于5的单元格的个数不能超过20%，
否则应该合并较小的类。

如果列联表的数据不是来自于总数\(n\)固定的随机抽样，
而是按照\(X\)变量分组抽取的，
这时零假设就变成了\(m\)个独立的总体的多项分布是否相同的问题，
这个问题恰好也可以使用上面的卡方统计量，
近似分布的自由度不变。
如果数据按\(Y\)变量分组抽取也一样。

### 30.10.2 列联表独立性卡方检验例子

#### 30.10.2.1 性别与啤酒种类的独立性检验

Alber’s Brewery of Tucson, Arizona是啤酒制造与销售商。
有三类啤酒产品：淡啤酒，普通啤酒，黑啤酒。

了解不同顾客喜好有利于制定更精准的销售策略。
希望了解男女顾客对不同类型的偏好有没有显著差异，
实际就是检验性别与啤酒类型偏好的独立性。

随机抽取了150位顾客，
得到如下的列联表：

```
ctab.beer <- rbind(c(
  20, 40, 20),
  c(30,30,10))
colnames(ctab.beer) <- c("Light", "Regular", "Dark")
rownames(ctab.beer) <- c("Male", "Female")
addmargins(ctab.beer)
```

```
##        Light Regular Dark Sum
## Male      20      40   20  80
## Female    30      30   10  70
## Sum       50      70   30 150
```

列联表独立性检验：

```
chisq.test(ctab.beer)
```

```
## 
##  Pearson's Chi-squared test
## 
## data:  ctab.beer
## X-squared = 6.1224, df = 2, p-value = 0.04683
```

在0.05水平下认为啤酒类型偏好与性别有关。
男性组的偏好分布、女性组的偏好分布、所有人的偏好分布：

```
tab2 <- round(prop.table(addmargins(ctab.beer, 1), 1), 3)
rownames(tab2)[3] <- "All"
tab2
```

```
##        Light Regular  Dark
## Male   0.250   0.500 0.250
## Female 0.429   0.429 0.143
## All    0.333   0.467 0.200
```

可以看出男性中对淡啤酒偏好偏少，
女性中对淡啤酒偏好偏多。

### 30.10.3 功效与样本量计算

在上述啤酒种类偏好的例子中，
假设要在\(0.01\)水平下检验出中等的效应大小，
样本量\(N=150\)，
计算其功效如下：

```
cohen.ES(test = "chisq", size = "medium")
```

```
## 
##      Conventional effect size from Cohen (1982) 
## 
##            test = chisq
##            size = medium
##     effect.size = 0.3
```

```
pwr.chisq.test(
  w = 0.3,
  N = 150, 
  df = (2-1)*(3-1),
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.3
##               N = 150
##              df = 2
##       sig.level = 0.05
##           power = 0.9185674
## 
## NOTE: N is the number of observations
```

功效为\(92\%\)。

如果明确指定了对立假设，
则可以用`ES.w2()`计算所对应的效应大小。
比如，
零假设是性别与啤酒种类独立，
而对立假设是性别与偏好的如下概率分布：
\[
\begin{matrix}
\text{Gender} \;\backslash \; \text{Beer}
& \text{Light} & \text{Regular} & \text{Dark} \\
\text{Male} & 2/15 & 4/15 & 2/15 \\
\text{Female} & 3/15 & 3/15 & 1/15
\end{matrix}
\]
就需要按此计算要检验的效应大小：

```
dist_tab <- matrix(
  c(2, 4, 2,  
    3, 3, 1)/15,
  byrow=TRUE, ncol=3,
  dimnames = list(
    c("Male", "Female"),
    c("Light", "Regular", "Dark")))
dist_tab
```

```
##            Light   Regular       Dark
## Male   0.1333333 0.2666667 0.13333333
## Female 0.2000000 0.2000000 0.06666667
```

```
ES.w2(dist_tab)
```

```
## [1] 0.2020305
```

计算此效应大小对应的功效：

```
pwr.chisq.test(
  w = ES.w2(dist_tab),
  N = 150, 
  df = (2-1)*(3-1),
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.2020305
##               N = 150
##              df = 2
##       sig.level = 0.05
##           power = 0.5932776
## 
## NOTE: N is the number of observations
```

因为要发现的效应更小，
所以达到的功效(\(59\%\))较低。
这说明，
如果啤酒偏好数据中反映的比例是真实情况的话，
实际上有\(41\%\)的概率会不拒绝零假设。

计算达到\(80\%\)功效所需的样本量：

```
pwr.chisq.test(
  w = ES.w2(dist_tab),
  power = 0.80, 
  df = (2-1)*(3-1),
  sig.level = 0.05)
```

```
## 
##      Chi squared power calculation 
## 
##               w = 0.2020305
##               N = 236.0499
##              df = 2
##       sig.level = 0.05
##           power = 0.8
## 
## NOTE: N is the number of observations
```

需要的样本量为\(236\)。

## 30.11 非参数检验

常用的有独立两样本比较的Wilcoxon秩和检验，
单样本的符号秩检验和符号检验等，

### 30.11.1 单样本的符号秩检验

`wilcox.test(x, mu)`进行符号秩检验，
适用于非正态总体，
检验\(H\_0: \mu = \mu\_0\)。

例如，
Heathrow机场打分是否在7分以上的检验：

```
wilcox.test(AirRating[["Rating"]], mu=7, alternative="greater")
```

```
## Warning in wilcox.test.default(AirRating[["Rating"]], mu = 7, alternative =
## "greater"): cannot compute exact p-value with ties
```

```
## Warning in wilcox.test.default(AirRating[["Rating"]], mu = 7, alternative =
## "greater"): cannot compute exact p-value with zeroes
```

```
## 
##  Wilcoxon signed rank test with continuity correction
## 
## data:  AirRating[["Rating"]]
## V = 457, p-value = 0.0469
## alternative hypothesis: true location is greater than 7
```

p值为0.047，
在0.05水平下拒绝原假设，
认为打分显著超过7分。

非参数检验一般功效较低，
这里的p值0.047就比用t检验得到的0.035的p值大一些。
p值越大，
越不容易得到显著的检验结果。

### 30.11.2 独立两样本比较的Wilcoxon秩和检验

`wilcox.test(x, y)`可以对独立两组进行位置参数的比较，
这是一个非参数检验。

例如，
对两个银行营业所顾客平均存款的比较：

```
wilcox.test(CheckAcct[[1]], CheckAcct[[2]], 
  alternative = "two.sided")
```

```
## Warning in wilcox.test.default(CheckAcct[[1]], CheckAcct[[2]], alternative =
## "two.sided"): cannot compute exact p-value with ties
```

```
## 
##  Wilcoxon rank sum test with continuity correction
## 
## data:  CheckAcct[[1]] and CheckAcct[[2]]
## W = 447, p-value = 0.00679
## alternative hypothesis: true location shift is not equal to 0
```

p值为0.007，
小于0.05，
两个营业所的顾客平均存款有显著差异。

### 30.11.3 成对比较的Wilcoxon配对检验

对成对比较问题，
可以计算两个变量的差进行符号秩检验，
称为Wilcoxon配对检验。

例如，同一组工人比较两种工艺的时间的问题：

```
wilcox.test(Matched[["Method 1"]], Matched[["Method 2"]], 
       paired=TRUE, alternative = "two.sided")
```

```
## Warning in wilcox.test.default(Matched[["Method 1"]], Matched[["Method 2"]], :
## cannot compute exact p-value with zeroes
```

```
## 
##  Wilcoxon signed rank test with continuity correction
## 
## data:  Matched[["Method 1"]] and Matched[["Method 2"]]
## V = 14, p-value = 0.1056
## alternative hypothesis: true location shift is not equal to 0
```

p值为0.11，
超过0.05，
认为两种工艺时间的差异不显著。

### References

Cohen, J. 1988. *Statistical Power Analysis for the Behavioral Sciences*. 2nd ed. Routledge. https://doi.org/<https://doi.org/10.4324/9780203771587>.

Kabacoff, Robert I. 2012. *R语言实战*. 人民邮电出版社.

Venables, W. N., and B. D. Ripley. 2002. *Modern Applied Statistics with s*. 4th Ed. Springer.