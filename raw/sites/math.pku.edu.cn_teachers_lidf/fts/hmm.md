---
crawl_time: '2026-01-17 14:30:49'
framework: sphinx
title: 34 隐马氏模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 34 隐马氏模型

## 34.1 介绍

隐马氏模型(HMM)类似于状态空间模型，
但是其状态遵从一个马氏链，
一般是离散状态的。
此模型也有广泛应用，
比如生物研究、模式识别、金融建模等。

参考：

* ([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)): Hidden Markov Models for Time Series - An Introduction Using R. 2nd ed., 2016, CRC Press.

### 34.1.1 混合分布

隐马氏模型的观测值变量在简单情况下边缘分布服从独立混合分布。
设\(\delta\_1, \dots, \delta\_m\)是加权平均系数，
\(p\_j(x)\), \(j=1,2,\dots,m\)是\(m\)个密度（或概率质量函数），
令
\[
p(x) = \sum\_{j=1}^m \delta\_j p\_j(x),
\]
则\(p(x)\)是一个密度（或概率质量函数），
其分布称为独立混合分布或者简称为混合分布。
设\(X\_j \sim p\_j\), \(X \sim p\)，
则
\[
E(X)
= \sum\_{j=1}^m \delta\_j E(X\_j) .
\]
且
\[
E(X^k)
= \sum\_{j=1}^m \delta\_j E(X\_j^k) .
\]

**例34.1** 考虑1900年到2006年全世界每年大地震（震级7级以上）次数的时间序列，
这是计数数据，
但是有连续较多年份和连续较少年份，
所以又序列相关，
不能用简单的泊松分布拟合。

数据来自：
<http://www.hmms-for-time-series.de/first/data/>

```
da_quakes <- readr::read_table(
  "hmm-earthquakes.txt",
  col_names = c("year", "quakes"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   year = col_double(),
##   quakes = col_double()
## )
```

```
maj_quakes <- ts(da_quakes[["quakes"]], start=1900)
plot(maj_quakes)
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7050-hmm_files/figure-html/hmm-intro-mix-quake-data-1.png)

拟合单个泊松分布的演示：

```
da_quakes |>
  count(quakes) |>
  right_join(tibble(quakes=0:41)) |>
  mutate(n = coalesce(n, 0)) |>
  arrange(quakes) -> da_fquakes
```

```
## Joining with `by = join_by(quakes)`
```

```
lam_quakes <- mean(da_quakes[["quakes"]])
plot(da_fquakes[["quakes"]], 
  da_fquakes[["n"]] / sum(da_fquakes[["n"]]), 
  type="h",
  xlab="quakes",
  ylab="prob")
points(da_fquakes[["quakes"]], 
  dpois(da_fquakes[["quakes"]], lam_quakes), pch=16)
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7050-hmm_files/figure-html/hmm-intro-mix-quake-pois-1.png)

上图中竖线是观测比例，
圆点是用单个泊松分布拟合的概率。
拟合是不充分的。
另外，
数据不是独立观测，其ACF图形：

```
forecast::Acf(maj_quakes)
```

```
## Registered S3 method overwritten by 'quantmod':
##   method            from
##   as.zoo.data.frame zoo
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7050-hmm_files/figure-html/hmm-intro-mix-quake-pois-acf-1.png)

**例34.2** 设\(J\)是随机变量，
\(P(J=1)=\delta\_1\), \(P(J=2)=\delta\_2=1-\delta\_1\)，
\(P(Y | J)\)服从参数为\(\lambda\_J\)的泊松分布，
则\(Y\)是两个泊松分布的混合分布。
用混合泊松分布拟合大地震个数数据。

似然函数为
\[
L(\lambda\_1, \lambda\_2, \delta\_1 | y\_1, \dots, y\_n)
= \prod\_{i=1}^n
\left(\delta\_1 p(y\_i | \lambda\_1)
+ (1 - \delta\_1) p(y\_i | \lambda\_2) \right) .
\]
这个似然函数很容易计算，
但是难以最大化。
此似然函数是\(n\)项的乘积，
而每一项都是两项的和，
取对数后，
其中的两项的和仍会使得求导结果变得复杂。
可以用数值方法求最大值，R的`nlm()`函数可以进行计算，
或者用EM算法，可以用flexmix包或者自己编程。

注意要求\(0 < \delta\_1 < 1\), \(\lambda\_j > 0\)。
可以取\(\text{logit}(\delta\_1)\)和\(\log\lambda\_j\)作为参数，
将约束优化转化成无约束优化。
对更一般的\(m \geq 2\)，
令
\[\begin{aligned}
\lambda\_j =& e^{\eta\_j},
j=1,\dots,m; \\
\delta\_j
=& \frac{e^{\tau\_j}}{1 + \sum\_{s=2}^m e^{\tau\_s}} ,
\ j=2,\dots,m .
\end{aligned}\]

对大地震个数数据，
编写如下混合泊松分布估计程序：

```
# 将无约束的$2m-1$个参数，
# 转换为$\lambda_1, \dots, \lambda_m$, $delta_1, \dots, \delta_m$
w2n <- function(wpar){
  m <- (length(wpar) + 1)/2
  lams <- exp(wpar[1:m])
  deltas <- exp(c(0, wpar[(m+1):(2*m-1)]))
  deltas <- deltas / sum(deltas)
  list(lambda = lams, delta = deltas)
}

# 将原始的$\lambda_1, \dots, \lambda_m$, $delta_1, \dots, \delta_m$,
# 转换为$2m-1$个无约束参数
n2w <- function(lambda, delta){
  c(log(lambda),
    log(delta[-1] / (1 - sum(delta[-1])))  )
}

# 负对数似然函数
# 输入的是无约束的$2m-1$个参数
mllk <- function(wpar, x){
  pars <- w2n(wpar)
  -sum(log(
    outer(x, pars$lambda, dpois) %*% pars$delta  ))
}
```

对地震个数数据的估计：

```
# 两个泊松的混合，参数估计
mllk_quakes <- \(wpar) mllk(wpar, da_quakes[["quakes"]])
nlmr_1 <- nlm(mllk_quakes,
  n2w(c(10, 20), c(0.5, 0.5)))$estimate |>
  w2n()
```

```
## Warning in nlm(mllk_quakes, n2w(c(10, 20), c(0.5, 0.5))): NA/Inf replaced by
## maximum positive value
## Warning in nlm(mllk_quakes, n2w(c(10, 20), c(0.5, 0.5))): NA/Inf replaced by
## maximum positive value
```

```
print(nlmr_1)
```

```
## $lambda
## [1] 15.77711 26.83990
## 
## $delta
## [1] 0.6757257 0.3242743
```

估计结果为\(\delta\_1 = 0.6757\),
\(\lambda\_1 = 15.78\),
\(\lambda\_2 = 26.84\)。
拟合图形：

```
plot(da_fquakes[["quakes"]], 
  da_fquakes[["n"]] / sum(da_fquakes[["n"]]), 
  type="h",
  xlab="quakes",
  ylab="prob")
points(da_fquakes[["quakes"]], 
  c(outer(da_fquakes[["quakes"]], nlmr_1$lambda, dpois) %*% 
    nlmr_1$delta), pch=16)
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7050-hmm_files/figure-html/hmm-intro-mix-pois-quakes-est2b-1.png)

○○○○○○

如果是连续分布的混合，
则似然函数可以趋于无穷。
比如，
两个正态密度的混合，
如果对某一个\(y\_i\)取期望参数为\(y\_i\)，
则\(\sigma\_1 \to 0\)时似然函数趋于无穷。
这可以将“等于”\(y\_i\)时的密度函数值，
变成以数据的观测精度，
取\(y\_i\)所在的区间的概率值，
则似然函数不会趋于无穷。

### 34.1.2 马氏链

设\(\{C\_t, t=1,2,\dots \}\)是一个离散时间随机过程，
取值于离散状态集合\(S\)，
满足
\[
P(C\_{t+1} = i\_{t+1} | C\_t = i\_t, \dots, C\_1 = i\_1)
= P(C\_{t+1} = i\_{t+1} | C\_t = i\_t),
\]
对任意\(t\), \(i\_j\), \(j=1,\dots,t+1\)成立，
则称\(\{C\_t \}\)是一个马氏链，
称\(P(C\_{t+1} = j | C\_t = i)\)为转移概率，
如果转移概率都不依赖于\(t\)，
则称马氏链为时齐的(homogeneous)，
记转移概率为\(\gamma\_{ij}\)，
记\(P(C\_{s+t} = j | C\_s = i)\)为\(\gamma\_{ij}(t)\)。
矩阵\(\Gamma(t)\)为\(t\)步转移概率矩阵，
有
\[
\Gamma(s+t) = \Gamma(s) \Gamma(t) .
\]

设状态空间为\(S = \{1,2,\dots, m\}\)，
则\(\Gamma = \Gamma(1)\)是一步转移概率矩阵，
\((i,j)\)元素为\(\gamma\_{ij}\)。
满足
\[
\Gamma \boldsymbol 1 = \boldsymbol 1 .
\]
定义向量
\[
\boldsymbol u(t)
= (P(C\_t=1), \dots, P(C\_t=m))^T,
\]
称为\(C\_t\)的无条件分布或者边缘分布，
称\(\boldsymbol u(1)\)为初始分布。
有
\[
[\boldsymbol u(t+1)]^T
= [\boldsymbol u(t)]^T \Gamma .
\]

若存在分布\(\boldsymbol\delta=(\delta\_1, \dots, \delta\_m)^T\)，
\(\delta\_i \in [0, 1]\)，使得
\[
\boldsymbol\delta^T \Gamma = \boldsymbol\delta^T,
\ \boldsymbol\delta^T \boldsymbol 1 = 1 .
\]
则\(\boldsymbol\delta\)称为平稳分布。
初始分布为平稳分布的马氏链每一步的无条件分布都是平稳分布。
不可约的有限状态马氏链必有平稳分布，
每一个概率严格为正；
在不可约、非周期、有限状态时，
马氏链有唯一的极限分布，
极限分布是唯一的平稳分布。

如何从一条轨道估计转移概率矩阵？
这只要估计\(\Gamma\)的每一行，
每一行构成一个概率分布，
对第\(i\)行，
找到所有从\(i\)到\(j\)的转移，
计算频数，
然后转换成比例即得\(\gamma\_{ij}\)的估计值。
设\(f\_{ij}\)表示\(n-1\)次转移中从\(i\)转移到\(j\)的次数，
则估计\(\gamma\_{ij}\)为
\[
\hat\gamma\_{ij}
= \frac{f\_{ij}}{\sum\_{k=1}^m f\_{ik}} .
\]

以上估计是由条件似然函数得到的最大似然估计，
条件似然函数以初始状态为条件。
如果马氏链平稳，
还可以写出完全似然函数，
求最大似然估计，
可以用数值算法，
特殊情况下有解析表达式。
比如，
设\(\{C\_t \}\)是取值于\(\{0, 1\}\)的马氏链，
设\(f\_{00}=0\), \(f\_{01}=1\),
\(f\_{11}>0\)。
令\(c = f\_{10} + (1-c\_1)\)，
\(d=f\_{11}\)，
则最大似然估计为
\[
\hat\gamma\_{10}
= \frac{-(1+d) + [(1+d)^2 + 4c(c+d-1)]^{1/2}}{2(c+d-1)} .
\]

## 34.2 隐马氏链定义和性质

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.29第2章。

### 34.2.1 隐马氏链定义

设\(\{C\_t \}\)为马氏链，
\(\{X\_t \}\)为随机过程，
\(X\_t\)在\(X\_1, \dots, X\_{t-1}, C\_1, \dots, C\_t\)下的条件分布等于\(X\_t\)在\(C\_t\)下的条件分布，
则称\(\{ X\_t \}\)服从隐马氏模型。
实际上状态空间模型也是这样的隐马氏模型，
但状态空间模型中的状态方程一般不是离散状态马氏链。

若马氏链\(\{C\_t \}\)的状态空间仅有\(m\)个值，
则称模型为\(m\)状态HMM。
隐马氏过程的其它一些名称为隐马氏过程，
马氏相依混合，
马氏机制转换模型(Markov-switch model，尤其是\(X\_t\)本身也有运动方程时)，
受马氏机制控制的模型，
马氏混合模型，
潜在马氏模型(latent Markov model，见于纵向数据模型)。

设\(p\_i(x)\)表示\(X\_t\)在\(C\_t=i\)条件下的分布，
离散分布时为概率质量函数，
连续分布时为概率密度函数。

### 34.2.2 边缘分布

讨论\(X\_t\)的分布，
以及\((X\_t, X\_{t+k})\)这样的联合分布。
假定马氏链\(\{C\_t \}\)时齐并给出结果，
也给出平稳条件下的结果。
仅对\(X\_t\)分布离散进行推导，
结果也适用于分布连续情形。

#### 34.2.2.1 一元分布

设\(X\_1, \dots, X\_T\)为离散取值的观测值序列，
令\(u\_i(t) = P(C\_t = i)\)，
则
\[\begin{aligned}
P(X\_t = x)
=& \sum\_{i=1}^m P(C\_t = i) P(X\_t = x | C\_t = i) \\
=& \sum\_{i=1}^m u\_i(t) p\_i(x) \\
=& (u\_1(t), \dots, u\_m(t)) \;
(p\_1(x), \dots, p\_m(x))^T \\
\stackrel{\triangle}{=}&
[\boldsymbol u(t)]^T
\text{diag} (p\_1(x), \dots, p\_m(x))
\boldsymbol 1 \\
\stackrel{\triangle}{=}&
[\boldsymbol u(t)]^T P(x) \boldsymbol 1 .
\end{aligned}\]
注意\([\boldsymbol u(t)]^T = [\boldsymbol u(1)]^T \Gamma^{t-1}\)，
所以
\[
P(X\_t = x)
= [\boldsymbol u(1)]^T \Gamma^{t-1}
P(x) \boldsymbol 1 .
\]

以上仅设\(\{C\_t \}\)为\(m\)状态时齐马氏链。
若进一步知道\(\{C\_t \}\)平稳，
平稳分布为\(\boldsymbol\delta\)，
则\(\boldsymbol\delta^T \Gamma = \boldsymbol\delta^T\)，
\(\boldsymbol u(1) = \boldsymbol\delta\)，
这时
\[
P(X\_t = x)
= \boldsymbol\delta^T P(x) \boldsymbol 1 .
\]

#### 34.2.2.2 二元分布

\[\begin{aligned}
& P(X\_t, X\_{t+k}, C\_t, C\_{t+k}) \\
=& P(X\_{t+k} | X\_t, C\_t, C\_{t+k})
\cdot P(X\_t | C\_t, C\_{t+k})
\cdot P(C\_{t+k} | C\_t) \cdot P(C\_t) \\
=& P(C\_t) P(C\_{t+k} | C\_t)
P(X\_t | C\_t)
P(X\_{t+k} | C\_{t+k}) .
\end{aligned}\]
关于\(C\_t\)和\(C\_{t+k}\)的所有可能取值对上式求和就得到\((X\_t, X\_{t+k})\)的联合分布。
\[\begin{aligned}
& P(X\_t = v, X\_{t+k} = w) \\
=& \sum\_{i=1}^m \sum\_{j=1}^m
u\_i(t) p\_i(v) \gamma\_{ij}(k) p\_j(w) \\
=& \boldsymbol u(t)^T P(v) \Gamma^k P(w) \boldsymbol 1 .
\end{aligned}\]

若马氏链平稳，
则\(\boldsymbol u(t) \equiv \boldsymbol\delta\)，
有
\[
P(X\_t = v, X\_{t+k} = w)
= \boldsymbol\delta^T P(v) \Gamma^k P(w) \boldsymbol 1 .
\]

类似地，
时齐、平稳情况下
\[
P(X+t = v, X\_{t+k} = w, X\_{t+k+l} = z)
= \boldsymbol\delta^T
P(v) \Gamma^k P(w)
\Gamma^l P(z)
\boldsymbol 1 .
\]

#### 34.2.2.3 矩

\[\begin{aligned}
E(X\_t)
=& \sum\_{i=1}^m u\_i(t) E(X\_t | C\_t = i) \\
=& \sum\_{i=1}^m \delta\_i E(X\_t | C\_t = i) \quad(\text{平稳时}) .
\end{aligned}\]

平稳时，
\[\begin{aligned}
E(g(X\_t))
=& \sum\_{i=1}^m \delta\_i E(g(X\_t) | C\_t = i), \\
E(g(X\_t, X\_{t+k}))
=& \sum\_{i=1}^m \sum\_{j=1}^m
E(g(X\_t, X\_{t+k}) | C\_t, C\_{t+k})
\delta\_i \gamma\_{ij}(k) .
\end{aligned}\]

如果\(g(x, y) = g(x) g(y)\)，
注意在\((C\_t, C\_{t+k})\)条件下\(X\_t\)与\(X\_{t+k}\)独立，
所以
\[\begin{aligned}
E(g(X\_t, X\_{t+k}))
=& \sum\_{i=1}^m \sum\_{j=1}^m
E(g(X\_t)) | C\_t)
E(g(X\_{t+k}) | C\_{t+k})
\delta\_i \gamma\_{ij}(k) .
\end{aligned}\]

上面的推导可以用来计算方差、协方差。
若\(m=2\)，
\(X\_t | C\_t = i\)服从Poisson(\(\lambda\_i\))分布，
则
\[\begin{aligned}
E(X\_t) =& \delta\_1 \lambda\_1 + \delta\_2 \lambda\_2 . \\
\text{Var}(X\_t)
=& E(X\_t)
+ \delta\_1 \delta\_2 (\lambda\_2 - \lambda\_1)^2
\geq E(X\_t) . \\
\text{Cov}(X\_t, X\_{t+k})
=& \delta\_1 \delta\_2 (\lambda\_2 - \lambda\_1)^2
(1 - \gamma\_{12} - \gamma\_{21})^k .
\end{aligned}\]
ACF是负指数速度衰减的。

### 34.2.3 似然函数

设隐马氏模型的观测值序列有\(T\)个，
记\(\boldsymbol X^{(t)} = (X\_1, \dots, X\_t)^T\)，
\(\boldsymbol x^{(t)} = (x\_1, \dots, x\_t)^T\)。
\((x\_1, \dots, x\_T)\)的似然函数，
即\(P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)})\)，
需要将\(P(X\_1=x\_1, \dots, X\_T=x\_T, C\_1=c\_1, \dots, C\_T=c\_T)\)中的每个\(C\_t\)项关于\(c\_t\)求和，
共\(T\)重求和，每一重为\(m\)项的和，
所以计算量为\(m^T\)乘以每一项的计算量，
而每一项都是\(2T\)项的乘积，
所以表面上看似然函数的计算量达到\(O(T m^T)\)，
\(T\)较大时计算不可行；
但实际上一般有计算量\(O(T m^2)\)的算法。

#### 34.2.3.1 两状态隐马氏模型的似然函数

设\(\{C\_t \}\)是两状态马氏链，
转移矩阵为
\[
\Gamma = \begin{pmatrix}
\frac{1}{2} & \frac{1}{2} \\
\frac{1}{4} & \frac{3}{4}
\end{pmatrix} .
\]
设\(X\_t | C\_t=i \sim b(1, p\_i)\)，
\(p\_1 = \frac{1}{2}\), \(p\_2 = 1\)。
这样的隐马氏模型称为伯努利隐马氏模型。
\(\{C\_t \}\)的平稳分布为
\[
\boldsymbol\delta = (\frac{1}{3}, \frac{2}{3})^T .
\]
为了计算\(P(X\_1=1, X\_2=1, X\_3=1)\)，
需要写出\(P(X\_1=1, X\_2=1, X\_3=1, C\_1=i, C\_2=j, C\_3=k)\)，
这是\(2 \times 3 = 6\)项的乘积；
然后关于\(i, j, k\)求和，
有\(2^3\)个求和项。
\[
P(X\_1=1, X\_2=1, X\_3=1)
= \sum\_{i=1}^2 \sum\_{j=1}^2 \sum\_{k=1}^2
\delta\_i p\_i(1) \gamma\_{ij}
p\_j(1) \gamma\_{jk}
p\_k(1) .
\]

注意
\[\begin{aligned}
& P(X\_1=1, X\_2=1, X\_3=1, C\_1=i, C\_2=j, C\_3=k) \\
=& P(X\_1=1, X\_2=1, X\_3=1) \\
& \cdot P(C\_1=i, C\_2=j, C\_3=k | X\_1=1, X\_2=1, X\_3=1),
\end{aligned}\]
所以如果模型参数已知，
要求\((C\_1, C\_2, C\_3)\)的后验最大估计，
只要求\(P(X\_1=1, X\_2=1, X\_3=1, C\_1=i, C\_2=j, C\_3=k)\)的最大值点。

可以写成矩阵形式：
\[
P(X\_1=1, X\_2=1, X\_3=1)
= \boldsymbol\delta^T P(1)
\Gamma P(1)
\Gamma P(1)
\boldsymbol 1 .
\]

从这个例子还可以给出一个反例：
\[
P(X\_3 = 1 | X\_2 = 1, X\_1 = 1)
\neq P(X\_3 = 1 | X\_2 = 1) .
\]
所以隐马氏模型的观测值序列一般不是马氏链。
但也可能有是马氏链的情形。

#### 34.2.3.2 一般隐马氏模型的似然函数

设隐马氏模型\(C\_t\)为\(m\)状态的，
转移矩阵\(\Gamma\)，
初始分布\(\boldsymbol\delta\)，
\(\boldsymbol\delta\)常常是平稳分布但非必要。
设观测值\(x\_1, \dots, x\_T\)给定，
要计算似然函数。
似然函数值可以用矩阵表示为
\[\begin{align}
L\_T
= \boldsymbol\delta^T P(x\_1)
\Gamma P(x\_2)
\Gamma P(x\_3) \cdots
\Gamma P(x\_T)
\boldsymbol 1 .
\tag{34.1}
\end{align}\]
其中\(P(x) = \text{diag}((p\_1(x), \dots, p\_m(x)))\),
\(p\_j(x) = P(X\_t=x | C\_t=j)\)，
不依赖于\(t\)的值。

若\(\boldsymbol\delta\)还是平稳分布，
则
\[\begin{align}
L\_T
= \boldsymbol\delta^T
\Gamma P(x\_1)
\Gamma P(x\_2)
\Gamma P(x\_3) \cdots
\Gamma P(x\_T)
\boldsymbol 1 .
\tag{34.2}
\end{align}\]

用[(34.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-defp-lik-gen-mat1)和[(34.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-defp-lik-gen-mat2)这样的矩阵形式可以递推地计算，
\[\begin{aligned}
\boldsymbol\alpha\_1^T
=& \boldsymbol\delta^T P(x\_1); \\
\boldsymbol\alpha\_t^T
=& \boldsymbol\alpha\_{t-1}^T
\Gamma P(x\_t),
\ t=2,3,\dots, T . \\
L\_T =& \boldsymbol\alpha\_T^T \boldsymbol 1 .
\end{aligned}\]
递推计算量为\(O(T m^2)\)量级。

若\(\boldsymbol\delta\)为平稳分布，
则递推公式为
\[\begin{aligned}
\boldsymbol\alpha\_0^T
=& \boldsymbol\delta^T; \\
\boldsymbol\alpha\_t^T
=& \boldsymbol\alpha\_{t-1}^T
\Gamma P(x\_t),
\ t=1,2,\dots, T . \\
L\_T =& \boldsymbol\alpha\_T^T \boldsymbol 1 .
\end{aligned}\]
称各个\(\boldsymbol\alpha\_t\)为向前概率(forward probabilities)。
可以证明\(\boldsymbol\alpha\_t\)的第\(j\)元素等于\(P(\boldsymbol X^{(t)} = \boldsymbol x^{(t)}, C\_t = j)\)。

实际编程计算时，
迭代相乘很容易造成向下溢出，
所以应计算对数值。

递推算法的另一种推导方式是定义
\[
\alpha\_t(j)
= P(\boldsymbol X^{(t)} = \boldsymbol x^{(t)}, C\_t = j),
\ j=1,2,\dots, m,
\]
令\(\boldsymbol\alpha\_t = (\alpha\_t(1), \dots, \alpha\_t(m))^T\)，
然后证明
\[
\boldsymbol\alpha\_t^T
= \boldsymbol\alpha\_{t-1}^T \Gamma P(x\_t),
\]
则显然有\(L\_T = \boldsymbol\alpha\_T^T \boldsymbol 1\)。

#### 34.2.3.3 观测值有缺失值的情形

如果\(x\_1, \dots, x\_T\)中某些值缺失，
比如\(x\_{s}\)缺失，
则在计算\((X\_1, \dots, X\_T, C\_1, \dots, C\_T)\)的联合概率时，
只要去掉相应的\(P(x\_s | C\_s)\)项即可。
于是，
在\(L\_T\)的递推计算公式中，
若\(x\_s\)缺失，
则只要将\(P(x\_s)\)替换成单位阵\(I\)即可。

#### 34.2.3.4 观测值有区间删失的情形

设\(x\_s\)的观测值仅知道\(x\_s \in [a\_s, b\_s]\)。
则在\(L\_T\)的\(m\)重求和计算公式中，
只要将\(P(X\_s = x\_s | C\_s = c\_s)\)替换成\(P(X\_s \in [a\_s, b\_s] | C\_s = c\_s)\)，
在矩阵形式的公式中只要将\(\text{diag}(P(X\_s = x\_s | C\_s = j), j=1,2,\dots,m)\)替换成\(\text{diag}(P(X\_s \in [a\_s, b\_s] | C\_s = j), j=1,2,\dots,m)\)。
注意若\(a\_s=-\infty\)，\(b\_s=\infty\)，
则\(x\_s\)为缺失值，
这时\(P(X\_s \in [a\_s, b\_s] | C\_s = c\_s)=1\)，
与\(x\_s\)缺失时将\(P(x\_s)\)替换成单位阵\(I\)的做法是一致的。

## 34.3 直接最大化似然函数的方法

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.47第3章。

因为似然函数\(L\_T\)的计算量有\(O(T m^2)\)的算法，
所以随着\(T\)的增长计算量是线性增长的，
可以用数值算法直接对\(L\_T\)求最大值点。
这其中要注意几个问题：

* \(L\_T\)本质上是\(T\)项相乘，
  会发生数值计算向下溢出，
  即很多小于1的数相乘趋近于0；
* 未知参数可能有约束；
* 似然函数可能存在多个局部极大值点。

这一节讨论上述问题的解决，
构造MLE的通用算法，
给出参数估计的标准误差算法。
最大似然估计也可以用EM算法，
在下一节介绍。

### 34.3.1 调整似然函数值的量纲

在观测\(X\_t\)为离散分布时，
\(\boldsymbol\alpha\_t\)是多个乘积的求和，
每个乘积都是多个概率的乘积，
当\(t\)较大时趋于零，
会发生数值向下溢出（即被保存成0）。
如果\(X\_t\)为连续分布，
则\(\boldsymbol\alpha\_t\)计算会向上溢出。

因为算法是矩阵乘积，
所以实际计算既有乘积又有加法，
不能简单地计算对数解决问题。

一种办法是给出\(\log(p + q)\)的近似计算公式。
设\(p>q\)，
记\(\tilde p = \log(p)\), \(\tilde q = \log q\)，
注意
\[\begin{aligned}
\log(p+q)
=& \log(p) + \log(1 + \frac{q}{p}) \\
=& \tilde p + \log(1 + e^{\tilde q - \tilde p}),
\end{aligned}\]
可以给出\(\log(1 + e^x)\)在\(x < 0\)的近似算法，
这只要对一些\(x\)值预先计算并保存，
然后用插值法近似就可以。

另一种方法是在递推的每一步对\(\boldsymbol\alpha\_t\)进行缩放使其元素绝对值处于较正常的取值范围。
每一个\(\boldsymbol\alpha\_t\)都缩放为元素和等于1，
并记住元素和的取值以利于恢复原始的\(\boldsymbol\alpha\_t\)值。
对\(t=0,1,\dots, T\)，
令
\[
\boldsymbol\phi\_t
= (\boldsymbol\alpha\_t^T \boldsymbol 1)^{-1}
\boldsymbol\alpha\_t,
\ w\_t = \boldsymbol\alpha\_t^T \boldsymbol 1 .
\]
则\(\boldsymbol\alpha\_t = w\_t \boldsymbol\phi\_t\)，
递推变成（平稳分布情形下）
\[\begin{align}
w\_0 =& \boldsymbol\alpha\_0^T \boldsymbol 1
= \boldsymbol\delta^T \boldsymbol 1 = 1, \\
\boldsymbol\phi\_0^T =& \boldsymbol\delta^T, \\
w\_t \boldsymbol\phi\_t^T
=& w\_{t-1} \boldsymbol\phi\_{t-1}^T
\Gamma P(x\_t);
\tag{34.3}\\
L\_T =& \boldsymbol\alpha\_T^T \boldsymbol 1
= w\_T \boldsymbol\phi\_T^T \boldsymbol 1 = w\_T.
\end{align}\]
所以
\[
L\_T
= w\_T
= \prod\_{t=1}^T \frac{w\_t}{w\_{t-1}}.
\]
由[(34.3)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-dirml-sca-01)可知
\[
w\_t = w\_{t-1} \boldsymbol\phi\_{t-1}^T
\Gamma P(x\_t) \boldsymbol 1 .
\]
于是
\[
\log(L\_T)
= \sum\_{t=1}^T
\log(\boldsymbol\phi\_{t-1}^T
\Gamma P(x\_t) \boldsymbol 1) .
\]

\(w\_t\)也可以取为\(\boldsymbol\alpha\_t\)的元素的最大值，
或者平均值。

如果\(\boldsymbol\delta\)只是初始分布而不是平稳分布，
则递推可以从\(w\_1 = \boldsymbol\delta^T P(x\_1) \boldsymbol 1\),
\(\boldsymbol\phi\_1 = w\_1^{-1} \boldsymbol\delta^T P(x\_1)\)开始计算。
下面的R程序是\(\boldsymbol\delta\)为初始分布的对数似然函数计算，
并设\(X\_t | C\_t\)的分布是泊松分布。

```
logL <- function(
  delta,    # 初始分布，长度m的向量
  Gam,      # 转移概率矩阵, m*m
  lambda,   # 各个泊松分布的参数，长度m的向量
  x         # 观测值，长度为T的向量
  ){
  # $\alpha_1$:
  alpha <- delta*dpois(x[1], lambda)
  sa <- sum(alpha)
  # $\log(L_T)$初值
  lscale <- log(sa)
  # $\phi_1$:
  alpha <- alpha / sa
  for(t in 2:length(x)){
    alpha <- (alpha %*% Gam) * dpois(x[t], lambda)
    sa <- sum(alpha)
    lscale <- lscale + log(sa)
    alpha <- alpha / sa
  }
  
  return(lscale)
}
```

### 34.3.2 参数值约束

考虑观测值条件分布为泊松分布的情形。
在平稳情形下，
待估计参数包括转移概率矩阵\(\Gamma\)和各个泊松分布参数\(\boldsymbol\lambda\)。
\(\Gamma\)的各个元素非负且各行的和等于1；
\(\boldsymbol\lambda\)的各个元素为正值。

R函数`constrOptim`支持线性约束下的优化问题，
但是用了内点罚函数法，
在参数值靠近约束边界时速度会很慢。
如果能通过对参数进行一些变换，
将约束问题变成无约束问题，
就可以利用成熟可靠的无约束优化算法求解。

对泊松参数\(\lambda\_i\)，
只要变成\(\eta\_i = \log(\lambda\_i)\)。
得到关于\(\eta\_i\)的最大值点\(\hat\eta\_i\)后，
只要取\(\hat\lambda\_i = e^{\hat\eta\_i}\)。

\(\Gamma\)矩阵有\(m^2\)个元素，
但只有\(m(m-1)\)个自由参数。
设用只有\(m(m-1)\)个变量的矩阵\(G\)表示\(\Gamma\)，
\(G\)的第\((i,j)\)元素记为\(\tau\_{ij}\),
但主对角线元素\(\tau\_{ii}\)设定为0。
设\(g(\cdot)\)是一个\(\mathbb R \to \mathbb R^+\)的严格单调增函数，比如
\[
g(x) = e^{x},
\text{ 或 }
g(x) = \begin{cases}
e^x, & x \leq 0, \\
1 + x, & x > 0 .
\end{cases}
\]
令
\[
\nu\_{ij}
= \begin{cases}
g(\tau\_{ij}), & i \neq j, \\
1, & i = j .
\end{cases}
\]
然后令
\[
\gamma\_{ij}
= \frac{\nu\_{ij}}{\sum\_{s=1}^m \nu\_{is}} .
\]
比如，
\(g(x)=e^x\)时，
\[
\gamma\_{ij}
= \frac{e^{\tau\_{ij}}}{1 + \sum\_{s \neq i}^m e^{\tau\_{is}}} ,
i \neq j;
\quad \gamma\_{ii} = 1 - \sum\_{j \neq i} \gamma\_{ij} .
\]
反向的变换为
\[
\tau\_{ij} = \log \frac{\gamma\_{ij}}{\gamma\_{ii}},
i \neq j;
\quad \tau\_{ii} = 0 .
\]

称\(\eta\_i\)和\(\tau\_{ij}\)为工作参数，
\(\lambda\_i\)和\(\gamma\_{ij}\)为自然参数。

对每一组工作参数，都可以计算对应的自然参数从而计算似然函数值；
求得关于工作参数的最大值点后，
再求出对应的自然参数即可得到最大似然估计。

例如，
观测条件分布为泊松分布的自然参数和工作参数之间的转换程序：

```
# 观测条件分布为泊松的HMM，自然参数转换为工作参数
pois_HMM_pn2pw <- function(
  m,              # 状态个数
  lambda,         # 各个泊松分布的m个参数
  Gam,            # 转移概率矩阵
  delta=NULL,     # 初始分布
  stationary=TRUE # 平稳时不需要初始分布参数
  ){
  # 泊松分布参数转换为工作参数
  loglam <- log(lambda)
  
  # 转移概率矩阵转换为工作参数，其中对角线元素无用
  taua <- log(Gam / diag(Gam))
  # 仅取矩阵的非对角元素，按列拉直
  tau <- c(taua)[!c(diag(m))]
  
  if(stationary){
    wdelta <- NULL
  } else {
    # 用$\log(\delta_i / \delta_1)$, i=2,\dots,m表示
    wdelta <- log(delta[-1] / delta[1])
  }
  
  c(loglam, tau, wdelta)
}

# 观测条件分布为泊松的HMM，工作参数转换为自然参数
pois_HMM_pw2pn <- function(
    m,              # 状态个数
    parvec,         # 保存了所有工作参数的向量
    stationary=TRUE # 平稳时不需要初始分布参数
    ){
  # 泊松分布参数
  lambda <- exp(parvec[1:m])
  
  # 状态转移矩阵
  gam <- diag(m)
  gam[!gam] <- exp(parvec[(m+1):(m*m)])
  gam <- gam / rowSums(gam)
  if(stationary){
    delta <- solve(t(diag(m) - gam + 1), rep(1, m))
  } else {
    delta <- c(1, exp(parvec[(m*m+1):length(parvec)]))
    delta <- delta / sum(delta)
  }
  
  list(lambda=lambda, Gamma=gam, delta=delta)
}
```

### 34.3.3 局部极大值点

似然函数可能有多个局部极大值点。
即使使用EM算法也不能避免收敛到局部极大值点而不是全局最大值点。
可以运行多次算法，
每次从不同初值出发，
比较最后的结果是否相同。

### 34.3.4 迭代初值

对于泊松分布参数，
可以用数据的统计量变换为初值；
比如，
两状态时，
用均值的两个倍数；
三个状态时，
用四分之一分位数、中位数、四分之三分位数，
以此类推。

转移概率矩阵则难以获得合理的初值。
一种选择是取\(\Gamma\)的非对角元素为同一个值，
比如\(0.01\), \(0.05\)等。
这还可以使得平稳分布为离散均匀分布。

好的初值可以使算法更稳定。

### 34.3.5 似然函数无界问题

若观测值条件分布为连续分布，
似然函数在某些参数组合处可能没有上界。
这时可以将\(X\_t = x\_t\)看做是\(X\_t\)属于\(x\_t\)的测量精度所决定的区间，
变成区间删失问题，
这时的似然函数变成了概率。

### 34.3.6 地震数据建模应用

考虑前面的大地震个数时间序列数据建模。
用观测条件分布泊松，
状态空间为两状态，
用前面所述方法计算似然函数，用`nlm()`优化。

从工作参数计算似然函数的程序：

```
# 基于工作参数计算负对数似然函数
pois_HMM_mllk <- function(
    parvect,  # 工作参数
    x,        # 观测值向量
    m,        # 状态个数
    stationary=TRUE,
    ...){
  if(m==1) {
    # m==1时仅有lambda参数
    return(
      -sum(dpois(
        x,exp(parvect),log=TRUE)))
  }
  
  n <- length(x)
  # 自然参数
  pn <- pois_HMM_pw2pn(
    m, parvect, stationary=stationary)
  # $\alpha_1$:
  foo <- pn$delta*dpois(x[1],pn$lambda)
  # $w_1$:
  sumfoo   <- sum(foo)
  lscale   <- log(sumfoo)
  # $\phi_1$
  foo      <- foo/sumfoo
  for (i in 2:n)  {
    if(!is.na(x[i])){
      P <- dpois(x[i], pn$lambda)
    } else {
      P<-rep(1,m)
    }
    # $\alpha_i$:
    foo    <- (foo %*% pn$Gamma) * P
    # $w_i$:
    sumfoo <- sum(foo)
    lscale <- lscale + log(sumfoo)
    # $\phi_i$:
    foo    <- foo/sumfoo
  }
  
  # 负对数似然函数值
  mllk <- -lscale
  return(mllk)
}

# 计算泊松HMM最大似然估计直接优化方法
pois_HMM_mle <- function(
    x,        # 观测值
    m,        # 状态个数
    lambda0,  # 各条件泊松分布参数
    gamma0,   # 初始转移概率矩阵
    delta0=NULL, #　初始分布，若平稳则不需要
    stationary=TRUE,
    ...){
  # 生成工作参数初值
  parvect0  <- pois_HMM_pn2pw(
    m, lambda0, gamma0, delta0, 
    stationary=stationary)
  # 优化
  mod <- nlm(
    pois_HMM_mllk,
    parvect0, x=x, m=m,
    stationary=stationary)
  # 从求解得到的工作参数转化为自然参数
  pn <- pois_HMM_pw2pn(
    m=m, mod$estimate,
    stationary=stationary)
  # 负对数似然函数值
  mllk <- mod$minimum
  # 工作参数个数
  np <- length(parvect0)
  AIC <- 2*(mllk + np)
  # 有效观测个数
  n <- sum(!is.na(x))
  BIC <- 2*mllk + np*log(n)
  
  list(
    m=m,
    lambda=pn$lambda,
    Gamma=pn$Gamma,
    delta=pn$delta,
    code=mod$code,
    mllk=mllk,
    AIC=AIC,
    BIC=BIC)
}
```

对大地震个数时间序列数据建模，
用两状态泊松HMM：

```
res <- pois_HMM_mle(
  da_quakes[["quakes"]],
  m=2,
  lambda0 = c(10,20),
  gamma0 = rbind(
    c(0.9, 0.1),
    c(0.1, 0.9)  ),
  stationary = TRUE )
```

```
## Warning in nlm(pois_HMM_mllk, parvect0, x = x, m = m, stationary = stationary):
## NA/Inf replaced by maximum positive value
```

\(\boldsymbol\lambda\):

```
res$lambda
```

```
## [1] 15.47223 26.12535
```

\(\Gamma\):

```
res$Gamma
```

```
##           [,1]       [,2]
## [1,] 0.9340397 0.06596032
## [2,] 0.1285095 0.87149054
```

\(\boldsymbol\delta\):

```
res$delta
```

```
## [1] 0.6608197 0.3391803
```

这里只给出了\(m=2\)的结果。
还可以考虑\(m=3,4\)。
\(m\)越大，
概率转移矩阵中越容易出现0概率值。
这是因为在样本量有限的情况下，
状态个数越多，
某些转移被观测到很少次数的出现可能性就越大。

### 34.3.7 标准误差和置信区间

可以用对数似然函数的海色阵来估计参数的方差阵。
但是，对HMM估计来说，
参数经常出现在取值区间边界，
使得海色阵方法不适用，
可以用bootstrap方法计算标准误差，
但计算速度很慢。

#### 34.3.7.1 海色阵方法

在一定正则性条件下，
HMM的参数最大似然估计是相合估计，渐近正态分布，
渐近有效。
因此大样本下计算标准误差后可以计算参数的近似置信区间。
但这些应用条件经常会不满足。

海色阵是关于负对数似然函数的工作参数的二阶偏导数矩阵：
\[
H = -(\frac{\partial^2 \ell}{\partial\phi\_i \phi\_j}) .
\]
需要求关于自然参数的二阶偏导数。
设各自然参数为\(\theta\_j\)，
\[
G = - (\frac{\partial^2 \ell}{\partial\theta\_i \theta\_j}),
\]
Jacobi
\[
M = (\frac{\partial \theta\_j}{\partial \phi\_i}),
\]
则
\[
H = M G M^T,
\ G^{-1} = M^T H^{-1} M .
\]
在最小值点处计算\(G^{-1}\)，
可以估计自然参数的协方差阵。

有文献提出了计算对数似然函数的迭代算法，
同时可以计算梯度和海色阵，
这样还可以用Levenberg-Marquardt算法进行优化计算。

#### 34.3.7.2 bootstrap方法

这里使用参数bootstrap方法，
用基于\(\hat\Theta\)的模型模拟关于\(\Theta\)的数据参数估计分布。

估计\(\hat\Theta\)的算法：

1. 对原始数据求最大似然估计\(\hat\Theta\)。
2. 1. 模拟生成真实参数为\(\hat\Theta\)的HMM模型数据，
      观测长度取为与原始数据相同，
      称为bootstrap样本。
   2. 从bootstrap样本估计模型参数，记为\(\hat\Theta^\*\)。
   3. 将(a)和(b)重复B次，每一次记录参数估计值\(\hat\Theta^\*\)。
3. 用\(B\)个\(\hat\Theta^\*\)的样本协方差阵，
   作为\(\hat\Theta\)的协方差阵估计。

在生成bootstrap样本后，
对某个\(\theta\_i\)，
可以用它的bootstrap样本的\(\alpha/2\)和\(1-\alpha/2\)分位数作为近似\(1-\alpha\)置信区间。

下面是对泊松HMM模型生成bootstrap样本的模拟程序。
利用了parallel包使用单个计算机的多个CPU核心进行并行计算。

```
# 对大地震个数时间序列，拟合三状态泊松HMM，
# 并计算bootstrap标准误差
res3 <- pois_HMM_mle(
  da_quakes[["quakes"]],
  m=3,
  lambda0 = c(15,18,23),
  gamma0 = rbind(
    c(0.9, 0.05, 0.05),
    c(0.05, 0.9, 0.05),
    c(0.05, 0.05, 0.9)),
  stationary = TRUE )

# 从估计的MLE，产生B组bootstrap样本
library(parallel)
cpucl <- makeCluster(8)
clusterExport(cpucl, c(
  "res3", "da_quakes",
  "pois_HMM_mle",
  "pois_HMM_generate_sample",
  "pois_HMM_mllk",
  "pois_HMM_pn2pw",
  "pois_HMM_pw2pn"))
simlis <- parLapply(
  cpucl,
  1:1000,
  \(k) {
    set.seed(100+k)
    xx <- pois_HMM_generate_sample(
      length(da_quakes[["quakes"]]),
      res3)
    pois_HMM_mle(
      xx,
      m=3,
      lambda0 = res3$lambda,
      gamma0 = res3$Gamma,
      stationary = TRUE )
  }
)
stopCluster(cpucl)

# save(res3, simlis, file="hmm-quakes-boot.RData")
```

上述模拟中有若干个模拟计算平稳分布时出错，
导致估计参数可能有异常值。

计算bootstrap置信区间：

```
# lambda参数的模拟值矩阵
da_lambda <- simlis |> 
  map("lambda") |> 
  reduce(rbind) 
# $\lambda$的估计值和90%近似bootstrap置信区间：
tab_lambda <- data.frame(
  res3$lambda,
  t(apply(da_lambda, 2, \(x) quantile(x, c(0.05, 0.95))))) |> 
  setNames(c("estimate", "90% LB", "90% UB"))
tab_lambda
```

```
##   estimate   90% LB   90% UB
## 1 13.14573 11.90044 14.72184
## 2 19.72101 17.21585 22.04908
## 3 29.71438 20.16680 33.67770
```

```
# Gamma参数的模拟值
da_Gamma <- simlis |> 
  map("Gamma") |> 
  map(c) |>
  reduce(rbind) 
# Gamma 估计值：
res3$Gamma
```

```
##              [,1]       [,2]       [,3]
## [1,] 9.546239e-01 0.02444264 0.02093345
## [2,] 4.976683e-02 0.89936731 0.05086586
## [3,] 1.889245e-08 0.19664254 0.80335744
```

```
# 90% LB
da_Gamma |>
  apply(2, \(x) quantile(x, 0.05)) |>
  matrix(3,3) |>
  round(digits=4)
```

```
##        [,1]   [,2]   [,3]
## [1,] 0.8040 0.0000 0.0000
## [2,] 0.0136 0.6482 0.0000
## [3,] 0.0000 0.0531 0.4298
```

```
# 90% UB
da_Gamma |>
  apply(2, \(x) quantile(x, 0.95)) |>
  matrix(3,3) |>
  round(digits=4)
```

```
##        [,1]   [,2]   [,3]
## [1,] 0.9888 0.1345 0.1259
## [2,] 0.2275 0.9664 0.1904
## [3,] 0.0000 0.5702 0.9469
```

```
# delta参数的模拟数据
da_delta <- simlis |> 
  map("delta") |> 
  reduce(rbind) 
# $\delta$的估计值90%近似bootstrap置信区间：
tab_delta <- data.frame(
  res3$delta,
  t(apply(
    da_delta, 2, 
    \(x) quantile(x, c(0.05, 0.95))))) |> 
  setNames(c("estimate", "90% LB", "90% UB"))
tab_delta
```

```
##    estimate     90% LB    90% UB
## 1 0.4436404 0.12927050 0.7504726
## 2 0.4044996 0.12714475 0.6623415
## 3 0.1518600 0.03552257 0.3636175
```

## 34.4 用EM算法求最大似然估计

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.65第4章。

在HMM中EM算法称为Baum-Welch算法，
这个算法不要求马氏链平稳，
把初始分布也作为未知参数进行估计。
如果马氏链平稳，
则需要修改Baum-Welch算法

### 34.4.1 向前概率和向后概率

在前面计算似然函数时，用过
\[\begin{align}
\boldsymbol\alpha\_1^T =&
\boldsymbol\delta^T P(x\_1), \\
\boldsymbol\alpha\_t^T
=& \boldsymbol\delta^T P(x\_1)
\Gamma P(x\_2)
\dots
\Gamma P(x\_t)
= \boldsymbol\alpha\_{t-1}^T
\Gamma P(x\_t) ,
\ t=2,3,\dots, T .
\tag{34.4}
\end{align}\]
其中\(\boldsymbol\delta\)为初始分布，
即\(C\_1\)的分布。
称\(\boldsymbol\alpha\_t\)为向前概率。
本节要证明\(\boldsymbol\alpha\_t\)的第\(j\)元素
\[\begin{align}
\alpha\_t(j)
= P(X\_1=x\_1, \dots, X\_t=x\_t, C\_t = j),
\ j=1,2,\dots, m, \ t=1,\dots, T .
\tag{34.5}
\end{align}\]

定义向后概率\(\boldsymbol\beta\_t\):
\[\begin{align}
\boldsymbol\beta\_t
= \Gamma P(x\_{t+1})
\Gamma P(x\_{t+2}) \cdots
\Gamma P(x\_{T})
\boldsymbol 1 ,
t=T, T-1, \dots, 1,
\tag{34.6}
\end{align}\]
其中\(\boldsymbol\beta\_T = \boldsymbol 1\).
可反向递推
\[
\boldsymbol\beta\_t
= \Gamma P(x\_{t+1}) \boldsymbol\beta\_{t+1},
t=T-1, T-2, \dots, 1 .
\]
本节要证明\(\boldsymbol\beta\_t\)的第\(j\)元素为
\[\begin{align}
\beta\_t(j)
= P(X\_{t+1}=x\_{t+1}, \dots, X\_T=x\_T | C\_t = j) ,
\ j=1,\dots,m, \ t=T-1, T-2,\dots, 1 .
\tag{34.7}
\end{align}\]
联系[(34.5)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-fpj)和[(34.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-bpj)可得
\[
\alpha\_t(j) \beta\_t(j)
= P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)}, C\_t = j) .
\]
关于\(j\)求和可得
\[
\boldsymbol\alpha\_t^T \boldsymbol\beta\_t
= \sum\_{j=1}^m \alpha\_t(j) \beta\_t(j)
= P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)})
= L\_T .
\]

向前概率\(\boldsymbol\alpha\_t\)要利用前\(t\)个样本观测值向前递推计算，
所以称为向前概率；
向后概率\(\boldsymbol\beta\_t\)要用到后\(T-t\)个样本观测值向后递推计算，
所以称为向后概率。

#### 34.4.1.1 向前概率

从定义可知\(\boldsymbol\alpha\_1^T = \boldsymbol\delta^T P(x\_1)\),
对\(t=1,2,\dots, T-1\)有
\[
\boldsymbol\alpha\_{t+1}^T
= \boldsymbol\alpha\_t^T \Gamma P(x\_{t+1}),
\]
从而
\[
\alpha\_{t+1}(j)
= \left(\sum\_{i=1}^m \alpha\_t(i) \gamma\_{ij} \right)
p\_j(x\_{t+1}) .
\]

可以证明
\[\begin{aligned}
&P(\boldsymbol X^{(t+1)}=\boldsymbol x^{(t+1)},
C\_t = i, C\_{t+1} = j) \\
=& P(\boldsymbol X^{(t)}=\boldsymbol x^{(t)},
C\_t=i)
P(C\_{t+1}=j | C\_{t}=i)
P(X\_{t+1}=x\_{t+1} | C\_{t+1}=j) \\
=& P(\boldsymbol X^{(t)}=\boldsymbol x^{(t)},
C\_t=i)
\gamma\_{ij}
p\_j(x\_{t+1}) .
\end{aligned}\]
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.341式(B.1)。
易见\(\boldsymbol\alpha\_1(j) = P(C\_1=j, X\_1=x\_1)\)，
用数学归纳法，
设[(34.5)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-fpj)对\(1\)到\(t\)成立，
则上式变成
\[
P(\boldsymbol X^{(t+1)}=\boldsymbol x^{(t+1)},
C\_t = i, C\_{t+1} = j)
= \alpha\_t(i) \gamma\_{ij} p\_j(x\_{t+1}),
\]
关于\(i\)求和有
\[
P(\boldsymbol X^{(t+1)}=\boldsymbol x^{(t+1)},
C\_{t+1} = j)
= \left( \sum\_{i=1}^m \alpha\_t(i) \gamma\_{ij} \right) p\_j(x\_{t+1})
= \alpha\_{t+1}(j) .
\]
由数学归纳法可知对\(t=1,2,\dots, T\)，
\[
\alpha\_t(j)
= P(\boldsymbol X^{(t)}=\boldsymbol x^{(t)}, C\_t = j),
\ j=1,2,\dots,m .
\]
即[(34.5)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-fpj)，
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.66命题2。

#### 34.4.1.2 向后概率

注意\(\boldsymbol\beta\_T = \boldsymbol 1\),
\[
\boldsymbol\beta\_t
= \Gamma P(x\_{t+1}) \boldsymbol\beta\_{t+1},
\ t=T-1, T-2, \dots, 1 .
\]
上式的第\(i\)项为
\[
\beta\_t(i)
= \sum\_{j=1}^m \gamma\_{ij} p\_j(x\_{t+1}) \beta\_{t+1}(j) .
\]
可以证明对\(t=T-1, \dots, 2, 1\)有
\[
\beta\_t(j)
= P(X\_{t+1}=x\_{t+1}, \dots, X\_T = x\_T | C\_t = j),
\ j=1,2,\dots, m ,
\]
若\(P(C\_t=j) > 0\)，
即[(34.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-bpj)式成立，
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.67命题3。

证明方法是数学归纳法对\(t=T-1,\dots, 1\)反向证明，
要用到
\[\begin{aligned}
&P(X\_{t+1}=x\_{t+1} | C\_{t+1}=j)
P(X\_{t+2}=x\_{t+2}, \dots, X\_{T}=x\_{T} | C\_{t+1}=j) \\
=& P(X\_{t+1}=x\_{t+1}, \dots, X\_{T}=x\_{T} | C\_{t+1}=j),
\ j=1,2,\dots,m, \\
& P(X\_{t+1}=x\_{t+1}, \dots, X\_{T}=x\_{T} | C\_{t+1}=j) \\
=& P(X\_{t+1}=x\_{t+1}, \dots, X\_{T}=x\_{T} | C\_{t+1}=j, C\_t=i),
i, j = 1, 2, \dots, m .
\end{aligned}\]
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.342 (B.5)，P.343 (B.6)式。
归纳地，
设[(34.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-bpj)对\(\beta\_{t+1}(j)\)成立。
则
\[\begin{aligned}
& P(X\_{t+1} = x\_{t+1}, X\_{t+2} = x\_{t+2}, \dots, X\_{T} = x\_{T}
| C\_t = i) \\
=& \sum\_{j=1}^m P(X\_{t+1} = x\_{t+1}, X\_{t+2} = x\_{t+2}, \dots, X\_{T} = x\_{T},
C\_{t+1} = j
| C\_t = i) \\
=& \sum\_{j=1}^m P(X\_{t+1} = x\_{t+1}, X\_{t+2} = x\_{t+2}, \dots, X\_{T} = x\_{T} |
C\_{t+1} = j, C\_t = i)
P(C\_{t+1} = j | C\_t = i) \\
=& \sum\_{j=1}^m P(X\_{t+1} = x\_{t+1}, X\_{t+2} = x\_{t+2}, \dots, X\_{T} = x\_{T} |
C\_{t+1} = j) \gamma\_{ij} \\
=& \sum\_{j=1}^m P(X\_{t+1} = x\_{t+1} | C\_{t+1} = j)
P(X\_{t+2} = x\_{t+2}, \dots, X\_{T} = x\_{T} | C\_{t+1} = j)
\gamma\_{ij} \\
=& \sum\_{j=1}^m p\_j(x\_{t+1}) \beta\_{t+1}(j) \gamma\_{ij} \\
=& \beta\_t(i) .
\end{aligned}\]
由数学归纳法可知[(34.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-bpj)对\(t=T-1, T-2,\dots, 1\)成立。

#### 34.4.1.3 向前概率与向后概率的性质

对\(t=1,2,\dots, T\)有
\[\begin{align}
\alpha\_t(i) \beta\_t(i)
= P(X\_1=x\_1, \dots, X\_T=x\_T, C\_t=i),
\ i=1,2,\dots,m .
\tag{34.8}
\end{align}\]
从而似然函数
\[\begin{align}
L\_T
= P(X\_1=x\_1, \dots, X\_T=x\_T)
= \sum\_{i=1}^m \alpha\_t(i) \beta\_t(i)
= \boldsymbol\alpha\_t^T \boldsymbol\beta\_t,
\ t=1,2,\dots, T .
\tag{34.9}
\end{align}\]

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.68命题4。
结果在EM算法中使用，
也用于局部解码。
但为了计算\(L\_T\)不需要向后概率，
只需要用
\[
L\_T = \boldsymbol\alpha\_T^T \boldsymbol\beta\_T
= \boldsymbol\alpha\_T^T \boldsymbol 1 .
\]

关于\(L\_T = \boldsymbol\alpha\_t^T \boldsymbol\beta\_t\)，
也可以从\(\boldsymbol\alpha\_t\)和\(\boldsymbol\beta\_t\)的定义直接得到：
\[\begin{aligned}
& \boldsymbol\alpha\_t^T \boldsymbol\beta\_t \\
=& \left[ \boldsymbol\delta^T P(x\_1)
\Gamma P(x\_2)
\dots
\Gamma P(x\_t) \right] \cdot
\left[ \Gamma P(x\_{t+1})
\Gamma P(x\_{t+2}) \cdots
\Gamma P(x\_{T})
\boldsymbol 1 \right] \\
=& L\_T .
\end{aligned}\]
见[(34.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-defp-lik-gen-mat1)。

从[(34.9)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-prop-lik)可以看出，
为了计算似然函数，
可以在\(t=1,2,\dots,T\)这\(T\)个位置计算，
都得到似然函数值，
但最方便的是原来[(34.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-defp-lik-gen-mat1)的方法，
即在\(t=T\)处计算，
这只需要使用向前概率的递推计算公式，
得到\(\boldsymbol\alpha\_T\)后有\(L\_T = \boldsymbol\alpha\_T^T\boldsymbol 1\)，
不需要计算向后概率。

为了进行EM计算，
还需要用到如下的性质，
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.69命题5。
对\(t=1,2,\dots, T\)有
\[\begin{align}
P(C\_t=j | \boldsymbol X^{(T)}=\boldsymbol x^{(T)})
= \alpha\_t(j) \beta\_t(j) / L\_T .
\tag{34.10}
\end{align}\]
对\(t=2,\dots,T\)有
\[\begin{align}
P(C\_{t-1}=j, C\_{t}=k |
\boldsymbol X^{(T)}=\boldsymbol x^{(T)})
= \alpha\_{t-1}(j)
\gamma\_{jk}
p\_k(x\_t)
\beta\_t(k)
/ L\_T .
\tag{34.11}
\end{align}\]

[(34.10)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-prop-ct)从[(34.9)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-prop-lik)立即可得。
为了证明[(34.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-prop-ct1ct)，
需要用到\(t<T\)时
\[\begin{aligned}
& P(X\_1=x\_1, \dots, X\_T=x\_T, C\_t=i, C\_{t+1}=j) \\
=& P(X\_1=x\_1, \dots, X\_t=x\_t, C\_t=i)
P(C\_{t+1}=j | C\_t=i) \\
& P(X\_{t+1}=x\_{t+1}, \dots, X\_T=x\_T | C\_{t+1}=j) .
\end{aligned}\]
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R))P.342 (B.4)式；
和\(t=0,1,\dots,T-1\)时
\[\begin{aligned}
&P(X\_{t+1}=x\_{t+1} | C\_{t+1}=j)
P(X\_{t+2}=x\_{t+2}, \dots, X\_{T}=x\_{T} | C\_{t+1}=j) \\
=& P(X\_{t+1}=x\_{t+1}, \dots, X\_{T}=x\_{T} | C\_{t+1}=j),
\ j=1,2,\dots,m.
\end{aligned}\]
见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R))P.342 (B.5)式。

### 34.4.2 EM算法

EM算法经常用于模型中含有不可观测的随机变量（称为缺失数据）的情形，
HMM中的状态\(C\_t\)就是不可观测随机变量。
事实上，
Baum等人针对HMM提出的算法早于Dempster, Laird和Rubin在1977年提出的EM算法。

#### 34.4.2.1 一般EM算法

EM算法是用于最大似然估计的迭代算法，
适用于有缺失数据的情形，
完全数据的似然函数容易计算与最大化，
而带有缺失数据的似然函数则不容易计算与最大化。
设\(\boldsymbol\theta\)是要估计的未知参数，
EM算法在E步、M步反复迭代，
直到某种收敛准则满足：

* E步：设第\(k\)步的参数估计为\(\boldsymbol\theta^{(k)}\)，
  设观测数据已知以及分布参数为\(\boldsymbol\theta^{(k)}\)，
  关于完全数据对数似然函数中的缺失数据部分求条件期望，
  得到关于参数\(\boldsymbol\theta\)的函数，
  此函数仅依赖于\(\boldsymbol\theta\)和观测值，
  不依赖于缺失数据。
* M步：对E步得到的关于\(\boldsymbol\theta\)的函数求最大值点，
  作为\(\boldsymbol\theta^{(k+1)}\)。

迭代会收敛到似然函数的一个稳定点，
但不一定是全局最大值点。

#### 34.4.2.2 HMM的EM算法

设未知的\(C\_t\)值为\(c\_t\), \(t=1,2,\dots,T\)，
这是模型中的缺失数据。
记
\[
u\_j(t)
= I\_{\{ c\_t = j\}},
\]
这里\(I\_A\)表示\(A\)成立时取1，不成立时取0。
记
\[
v\_{jk}(t)
= I\_{\{ c\_{t-1}=j, c\_t=k \}} .
\]

完全数据\((c\_1, \dots, c\_T, x\_1, \dots, x\_T)\)的对数似然函数为
\[\begin{aligned}
\text{CDLL}
=& \log P(X\_1 = x\_1, \dots, X\_T=x\_T, c\_1=c\_1, \dots, C\_T=c\_T) \\
=& \log \left\{
P(C\_1=c\_1) P(X\_1=x\_1 | C\_1=c\_1) \right. \\
& P(C\_2=c\_2 | C\_1=c\_1) P(X\_2=x\_2 | C\_2=c\_2) \\
& \cdots \\
& \left.
P(C\_T=c\_T | C\_{T-1}=c\_{T-1}) P(X\_T=x\_T | C\_T=c\_T)
\right\} \\
=& \log\left\{
\delta\_{c\_1} p\_{c\_1}(x\_1)
\gamma\_{c\_1 c\_2} p\_{c\_2}(x\_2)
\cdots
\gamma\_{c\_{T-1} c\_T} p(c\_T)(x\_T)
\right\} \\
=& \log\left\{
\delta\_{c\_1} p\_{c\_1}(x\_1)
\prod\_{t=2}^T [\gamma\_{c\_{t-1} c\_t} p\_{c\_t}(x\_t)]
\right\} \\
=& \log \delta\_{c\_1}
+ \sum\_{t=2}^T \log\gamma\_{c\_{t-1} c\_t}
+ \sum\_{t=1}^T \log p\_{c\_t}(x\_t) .
\end{aligned}\]
其中\(c\_1, \dots, c\_T\)是缺失数据，
在E步需要根据当前参数估计值对应的条件分布将其积分掉。

用示性函数\(u\_j(t)\)和\(v\_{jk}(t)\)将CDLL写成：
\[\begin{align}
\text{CDLL}
=& \sum\_{j=1}^m u\_j(1) \log\delta\_j \\
& + \sum\_{j=1}^m \sum\_{k=1}^m
\left( \sum\_{t=2}^T v\_{jk}(t) \right) \log\gamma\_{jk} \\
& + \sum\_{j=1}^m \sum\_{t=1}^T
u\_j(t) \log p\_j(x\_t) \\
=& A\_1 + A\_2 + A\_3 .
\tag{34.12}
\end{align}\]

这里\(\boldsymbol\delta\)是模型的初始分布，
即\(C\_1\)的分布，
不要求平稳。

为什么要用示性函数\(u\_j(t)\)和\(v\_{jk}(t)\)将CDLL改写成[(34.12)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-em-emalg-hmm-cdll)这样的形式？
这是因为[(34.12)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-em-emalg-hmm-cdll)中每一个求和项都变成了仅依赖于隐藏状态值，
不依赖于未知参数的0-1值随机变量，
乘以不依赖于隐藏状态但依赖于未知参数的部分，
这样可以在EM算法的E步中，
仅对0-1随机变量部分求条件期望，
是线性的。

这里马氏链不要求平稳，
从参数估计角度来看，
仅仅为\(t=1\)一个时间点估计一组参数\(\boldsymbol\delta\)是不合算的，
而且\(\boldsymbol\delta\)的估计会是以概率1取某一个状态。
所以在不要求平稳时，
可以对每一个\(C\_1=i\)的值\(i\)，
求给定\(C\_1=i\)条件下的条件最大似然估计，
然后关于\(i\)求最大值。
如果要求平稳，
则不需要单独估计\(\boldsymbol\delta\)。

后面的例子中还是估计了\(\boldsymbol\delta\)，
用来演示这样做的效果。

在HMM的EM算法的E步，
设当前参数为\(\boldsymbol\delta^{(s)}\),
\(\boldsymbol\lambda^{(s)}\)，
\(\Gamma^{(s)}\)，
统一记为\(\boldsymbol\theta^{(s)}\)，
设观测值\(\boldsymbol x^{(T)}=(x\_1, \dots, X\_T)^T\)已知，
在这样的条件下求\(u\_j(t)\)和\(v\_{jk}(t)\)的条件期望：
\[\begin{aligned}
\hat u\_j(t)
=& P(C\_t = j | \boldsymbol x^{(T)}, \boldsymbol\theta^{(s)}) \\
=& \alpha\_t(j) \beta\_t(j) / L\_T(\boldsymbol\theta^{(s)}), \\
\hat v\_{jk}(t)
=& P(C\_{t-1}=j, C\_t=k
| \boldsymbol x^{(T)}, \boldsymbol\theta^{(s)}) \\
=& \alpha\_{t-1}(j) \gamma\_{jk} p\_k(x\_t) \beta\_t(k)
/ L\_T(\boldsymbol\theta^{(s)}) .
\end{aligned}\]
其中\(\gamma\_{jk}\)实际为\(\gamma\_{jk}^{(s)}\)，
向前概率\(\alpha\_t(j)\)按当前参数\(\boldsymbol\theta^{(s)}\)向前递推计算，
并设初始分布为\(\boldsymbol\delta^{(s)}\),
向后概率\(\beta\_t(j)\)按当前参数\(\boldsymbol\theta^{(s)}\)向后递推计算。
这样计算概率的依据见式[(34.10)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-prop-ct)和[(34.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-fpbp-prop-ct1ct)。

在HMM的EM算法的M步，
将CDLL中的\(u\_j(t)\)和\(v\_{jk}(t)\)分别替换成其当前的条件期望值\(\hat u\_j(t)\)和\(\hat v\_{jk}(t)\)，
然后关于自变量\(\boldsymbol\theta\)求最大值，
记最大值点为\(\boldsymbol\theta^{(s+1)}\)，
就可以回到E步重复。

考察CDLL的[(34.12)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-em-emalg-hmm-cdll)可以看出，
代入条件期望后，
\(A\_1 = \sum\_{j=1}^m \hat u\_j(1) \log\delta\_j\)仅依赖于初始分布\(\boldsymbol\delta\)；
\(A\_2 = \sum\_{j=1}^m \sum\_{k=1}^m \left( \sum\_{t=2}^T \hat v\_{jk}(t) \right) \log\gamma\_{jk}\)仅依赖于转移概率矩阵\(\Gamma\)；
\(A\_3 = \sum\_{j=1}^m \sum\_{t=1}^T \hat u\_j(t) \log p\_j(x\_t)\)仅依赖于观测值条件分布的参数，
在泊松HMM中即\(\boldsymbol\lambda\)。
所以只要将这三部分分别求最大值。

对\(A\_1\)，
根据KL距离的理论：
若\((p\_1, \dots, p\_m)\)和\((q\_1, \dots, q\_m)\)是\(\{1, 2, \dots, m \}\)上的两个离散分布，
则
\[
D(p || q)
= \sum\_{j=1}^m p\_j \log\frac{p\_j}{q\_j} \geq 0 .
\]
由此可知仅在\(\delta\_j = \delta\_{j}^{(s+1)} = \hat u\_j(1)\)时达到最大值。

同理，
对\(A\_2\)，
可以看出只要关于\(\Gamma\)的每一行求解，
记\(\hat f\_{jk} = \sum\_{t=2}^T \hat v\_{jk}(t)\)，
则\(\gamma\_{jk}^{(s+1)} = \hat f\_{jk} / \sum\_{l=1}^m \hat f\_{jl}\)。

\(A\_3\)部分的最大化涉及到观测值条件分布\(p\_j(t)\)的形式，
在泊松和正态两种情形下可以写出解的表达式。

从\(A\_2\)部分求解看出，
得到\(\hat v\_{jk}(t)\)后，
在求最大值时仅用到其关于\(t=2,\dots,T\)的和\(\hat f\_{jk}\)。
在利用向前、向后算法计算\(\boldsymbol\alpha\_t\)和\(\boldsymbol\beta\_t\)时要注意数值计算向下溢出和向上溢出的可能性，
计算\(\hat v\_{jk}(t)\)以及对其求和也存在溢出的可能性，
设计算法时要注意避免溢出。

#### 34.4.2.3 泊松HMM的EM算法

设条件分布\(p\_j(x)\)为Poisson(\(\lambda\_j\))分布，
则
\[
p\_j(x\_t) = \frac{\lambda\_j^{x\_t}}{x\_t!} e^{-\lambda\_j},
\]
于是
\[
A\_3 = \sum\_{j=1}^m \sum\_{t=1}^T
\hat u\_j(t) [-\lambda\_j + x\_t \log\lambda\_j]
+ \text{常数项},
\]
令\(\partial A\_3 / \partial\lambda\_j = 0\)得
\[
0 = \sum\_{j=1}^m \sum\_{t=1}^T
\hat u\_j(t) (-1 + \frac{x\_t}{\lambda\_j}),
\]
解得
\[
\lambda\_j^{(s+1)}
= \frac{\sum\_{t=1}^T \hat u\_j(t) x\_t}{\sum\_{t=1}^T \hat u\_j(t)} .
\]

#### 34.4.2.4 正态HMM的EM算法

设\(p\_j(x)\)表示\(\text{N}(\mu\_j, \sigma\_j^2)\)。
则可解得
\[\begin{aligned}
\mu\_j^{(s+1)}
=& \frac{\sum\_{t=1}^T \hat u\_j(t) x\_t}{
\sum\_{t=1}^T \hat u\_j(t)}, \\
\sigma\_j^2
=& \frac{\sum\_{t=1}^T \hat u\_j(t) (x\_t - \mu\_j^{(s+1)})^2}{
\sum\_{t=1}^T \hat u\_j(t)} .
\end{aligned}\]

#### 34.4.2.5 HMM平稳时的EM算法

若HMM的状态马氏链平稳，
则\(\boldsymbol\delta\)可以由\(\Gamma\)决定，
比如求解
\[
\boldsymbol\delta
= (I - \Gamma^T + \boldsymbol 1 \boldsymbol 1^T)^{-1}
\boldsymbol 1.
\]
但是，这时\(A\_1\)中的\(\boldsymbol\delta\)由\(A\_2\)中的\(\Gamma\)决定，
一般不能给出\(\Gamma\)的封闭表达式，
只能数值求解最大值，
这是平稳时用EM方法的一个缺点。

### 34.4.3 EM算法应用实例

#### 34.4.3.1 泊松HMM参数估计的EM算法程序

不假定马氏链平稳，
设观测值的条件分布为泊松分布，
向前概率\(\log\boldsymbol\alpha\_t\), \(t=1,2,\dots,T\)的计算程序，
输出对数值，保存为\(m \times T\)矩阵：

```
# 向前概率$\boldsymbol\alpha_t$的对数值的计算
# 不假定马氏链平稳
pois_HMM_lforward <- function(
    x,    # 观测数据，长度n
    mod){ # 模型自然参数
  n <- length(x) # 观测数据长度
  # 矩阵，每一列保存一个$\boldsymbol\alpha_t$:
  lalpha <- matrix(NA, mod$m, n)
  
  # $\boldsymbol\alpha_1$，不要求平稳情形
  foo <- mod$delta * dpois(x[1],mod$lambda)
  sumfoo <- sum(foo)
  lscale <- log(sumfoo)
  foo <- foo/sumfoo # 归一化，避免数值向下溢出
  # $t=1$时的$\boldsymbol\alpha_t$的对数值:
  lalpha[,1]    <- lscale + log(foo) 
  
  for (i in 2:n)  {
    foo <- (foo %*% mod$Gamma) * dpois(x[i], mod$lambda)
    sumfoo <- sum(foo)
    lscale <- lscale + log(sumfoo)
    foo <- foo/sumfoo # $\boldsymbol\alpha_i$的归一化
    lalpha[,i] <- log(foo) + lscale
  }
  
  return(lalpha)
}
```

向后概率\(\log\boldsymbol\beta\_t\), \(t=1,2,\dots,T\)的计算程序，
输出对数值，保存为\(m \times T\)矩阵：

```
# 向后概率$\boldsymbol\beta_t$的对数值的计算
# 不假定马氏链平稳
pois_HMM_lbackward <- function(
    x,      # 观测数据，长度n
    mod) {  # 模型自然参数
  n <- length(x) # 观测长度
  m <- mod$m     # 状态个数
  # 矩阵，每一列是一个$\boldsymbol\beta_t$值的对数变换：
  lbeta      <- matrix(NA,m,n)
  
  # 初值$\boldsymbol\beta_t = $\boldsymbol 1$:
  lbeta[,n] <- rep(0,m)
  foo <- rep(1/m,m) # 归一化，避免向下溢出
  lscale <- log(m)  # 用来保存当前归一化所用分母的对数值
  for (i in (n-1):1){
    foo <- mod$Gamma %*% (dpois(x[i+1],mod$lambda) * foo)
    lbeta[,i] <- log(foo) + lscale
    sumfoo <- sum(foo)
    foo <- foo/sumfoo # 归一化，避免向下溢出
    lscale <- lscale + log(sumfoo)
  }
  
  return(lbeta)
}
```

泊松HMM模型用EM进行参数估计的程序，
不假定马氏链平稳：

```
# 计算泊松HMM最大似然估计EM算法
# 不假定马氏链平稳
# 不需要工作参数
pois_HMM_EM <- function(
    x,        # 观测值
    m,        # 状态个数
    lambda0,  # 各条件泊松分布参数
    gamma0,   # 初始转移概率矩阵
    delta0=rep(1/m, m), #　初始分布
    ...){

  tol <- 1E-5     # 对数似然函数计算精度
  max_iter <- 100 # 最大迭代次数
  n <- length(x) # 观测长度$T$
  mod <- list(
    m=m,
    delta=delta0, 
    lambda=lambda0, 
    Gamma=gamma0)
  logL0 <- -Inf
  iter <- 0
  
  ## EM迭代
  repeat{
    iter <- iter + 1
    
    # E步，和$\hat v_{jk}(t)$
    ## 计算向前概率和向后概率的对数值
    lalpha <- pois_HMM_lforward(x, mod)
    lbeta <- pois_HMM_lbackward(x, mod)
    
    ## 计算$\log L_T = \log(\boldsymbol\alpha_T^T \boldsymbol 1)$:
    sc_alpha_T <- mean(lalpha[,n])
    alpha_T_sc <- exp(lalpha[,n] - sc_alpha_T)
    logL <- sc_alpha_T + log(sum(alpha_T_sc))
    
    ## 计算$\hat u_j(t)$对数值, $m \times n$矩阵
    ## lu[j,t]为$\log \hat u_j(t)$
    lu <- lalpha + lbeta - logL
    
    ## 计算$\hat v_{jk}(t)$关于$t=2,\dots,T$的和的对数值$\log\hat f_{jk}$
    lfmat <- matrix(0, m, m)
    for(j in 1:m) for(k in 1:m){
      lalphaj_sc <- lalpha[j, 1:(n-1)]
      sc_alphaj <- mean(lalphaj_sc)
      lalphaj_sc[] <- lalphaj_sc - sc_alphaj
      
      lbetak_sc <- lbeta[k, 2:n]
      sc_betak <- mean(lbetak_sc)
      lbetak_sc[] <- lbetak_sc - sc_betak
      
      ldpk_sc <- dpois(x[2:n], mod$lambda[k], log=TRUE)
      sc_dpk <- mean(ldpk_sc)
      ldpk_sc[]  <- ldpk_sc - sc_dpk
      
      lfmat[j, k] <- log(sum(exp(
        lalphaj_sc 
        + ldpk_sc
        + lbetak_sc
      ) ) ) + sc_alphaj + sc_betak + sc_dpk +
        log(mod$Gamma[j,k]) - logL
    }
    
    # M步
    ## $\boldsymbol\delta$:
    mod$delta <- exp(lu[,1])
    
    ## $\Gamma$的每一行分别计算
    for(j in 1:m){
      fj_sc <- exp(lfmat[j,] - mean(lfmat[j,]))
      mod$Gamma[j,] <- fj_sc / sum(fj_sc)
    }
    
    ## $\boldsymbol\lambda$: 
    for(j in 1:m){
      uj_sc <- exp(lu[j,] - mean(lu[j,]))
      mod$lambda[j] <- sum(uj_sc*x) / sum(uj_sc)
    }

    if(iter < max_iter && logL > -Inf && abs(logL - logL0) < tol){
      break
    } else {
      logL0 <- logL
    }
  }
  
  mod$converged <- iter < max_iter
  mod$logLik <- logL
  mod$iter <- iter

  mod
}
```

#### 34.4.3.2 逐年大地震次数用泊松HMM的EM算法建模

两状态模型估计：

```
pois_HMM_EM(
  da_quakes[["quakes"]],
  m=2,
  lambda0 = c(10,20),
  gamma0 = rbind(
    c(0.9, 0.1),
    c(0.1, 0.9)  ) )
```

```
## $m
## [1] 2
## 
## $delta
## [1] 1.000000e+00 3.755168e-91
## 
## $lambda
## [1] 15.41865 26.01369
## 
## $Gamma
##           [,1]       [,2]
## [1,] 0.9283438 0.07165622
## [2,] 0.1189387 0.88106134
## 
## $converged
## [1] TRUE
## 
## $logLik
## [1] -341.8787
## 
## $iter
## [1] 38
```

三状态模型估计：

```
pois_HMM_EM(
  da_quakes[["quakes"]],
  m=3,
  lambda0 = c(15, 18, 23),
  gamma0 = rbind(
    c(0.9, 0.05, 0.05),
    c(0.05, 0.9, 0.05),
    c(0.05, 0.05, 0.9) ) )
```

```
## $m
## [1] 3
## 
## $delta
## [1] 1.000000e+00 1.914166e-32 1.250111e-85
## 
## $lambda
## [1] 13.13378 19.71213 29.70823
## 
## $Gamma
##              [,1]       [,2]       [,3]
## [1,] 9.393205e-01 0.03205032 0.02862918
## [2,] 4.038284e-02 0.90644789 0.05316927
## [3,] 2.553212e-13 0.19023966 0.80976034
## 
## $converged
## [1] TRUE
## 
## $logLik
## [1] -328.5275
## 
## $iter
## [1] 19
```

## 34.5 观测值预测、状态估计

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.81第5章。

在给定观测值后进行最大似然估计，
然后可以对缺失观测值进行估计，
预测观测值，
估计马氏链状态，等等。
这都是基于条件分布的计算。

本节不要求马氏链平稳，
\(\boldsymbol\delta\)是\(t=1\)时的状态\(C\_1\)的分布。

### 34.5.1 观测值的条件分布

记\(\boldsymbol x^{(-t)}\)表示在\(\boldsymbol x^{(t)} = (x\_1, \dots, x\_T)^T\)中删去\(x\_t\)后的向量。
\(\boldsymbol X^{(-t)}\)含义类似。
考虑\(X\_t\)在\(\boldsymbol x^{(-t)}\)条件下的条件分布，
这可以用来填补缺失值。

要计算
\[
P(X\_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})
= \frac{P(X\_t=x, \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})}{
P(\boldsymbol X^{(-t)} = \boldsymbol x^{(-t)}) },
\]
分子就是似然函数，
但\(t\)时刻的\(P(x\_t)\)用\(P(x)\)替换；
分母就是当\(x\_t\)缺失时的似然函数，
见[34.2.3.3](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#hmm-defp-lik-miss)，
只要在计算时将\(P(x\_t)\)用单位阵\(I\)替换。
记\(B\_t = \Gamma P(x\_t)\),
则
\[\begin{align}
& P(X\_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)}) \\
=& \frac{\boldsymbol\delta^T P(x\_1) B\_2 \dots B\_{t-1}
\Gamma P(x) B\_{t+1} \dots B\_T \boldsymbol 1}{
\boldsymbol\delta^T P(x\_1) B\_2 \dots B\_{t-1}
\Gamma I B\_{t+1} \dots B\_T \boldsymbol 1 } \\
& \propto \boldsymbol\alpha\_{t-1}^T
\Gamma P(x) \boldsymbol\beta\_t ,
\ t=2,3,\dots, T .
\tag{34.13}
\end{align}\]
对\(t=1\)有
\[\begin{align}
& P(X\_1 = x | \boldsymbol X^{(-1)} = \boldsymbol x^{(-1)}) \\
=& \frac{\boldsymbol\delta^T P(x) B\_2 \dots B\_{T}
\boldsymbol 1}{
\boldsymbol\delta^T I B\_2 \dots B\_{T} \boldsymbol 1 } \\
& \propto \boldsymbol\delta^T
P(x) \boldsymbol\beta\_1 .
\tag{34.14}
\end{align}\]

这些条件概率实际是\(m\)个观测值条件分布的混合。
[(34.13)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-dec-conddist-main)右侧为行向量\(\boldsymbol\alpha\_{t-1}^T \Gamma\)，乘以对角阵\(P(x)\)，再乘以列向量\(\boldsymbol\beta\_t\)，
所以是一个单重求和的二次型形式：
\[
P(X\_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})
= \rho\_t
\sum\_{j=1}^m d\_j(t) p\_j(x),
\ t=2,3,\dots, T,
\]
其中\(d\_j(t)\)是\(\boldsymbol\alpha\_{t-1}^T \Gamma\)的第\(j\)元素乘以 \(\beta\_j(t)\)，
\(\rho\_t\)为比例常数。
对\(t=1\)，
只要取\(d\_j(1) = \delta\_j \beta\_j(1)\)。
因此条件分布\(P(X\_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})\)是将\(x\_t=x | C\_t\)的各个条件分布\(p\_j(x)\)用系数\(d\_j(t)\)进行混合得到的混合分布。
将比例常数与\(d\_j(t)\)合并写成\(w\_j(t)\)，则有
\[\begin{align}
P(X\_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})
= \sum\_{j=1}^m w\_j(t) p\_j(x),
\ t=1,2,\dots, T .
\tag{34.15}
\end{align}\]
其中
\[
w\_j(t) = \frac{d\_j(t)}{\sum\_{k=1}^m d\_k(t)} .
\]

条件分布可以用来插补缺失值，
或者进行异常值检测。

计算以上条件概率的程序：

```
## 对泊松HMM，不假定马氏链平稳，计算如下条件概率：
## $P(X_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})$
## 输入xc为要计算的x值组成的向量
## 输入x为观测值向量
## 输入mod为自然参数列表
## 输出结果为矩阵，每一行对应一个时间t
pois_HMM_conditional <- function(
    xc,
    x,
    mod){
  n <- length(x) # 观测长度
  m <- mod$m     # 马氏链状态个数
  nxc <- length(xc) # 要计算条件概率的自变量值x的个数
  # dxc: 每一行对应一个时间t
  dxc <- matrix(NA_real_, nrow=n, ncol=nxc)
  # 每一列保存一组$p_j(x), j=1,\dots,m$的值
  Px <- matrix(NA_real_, nrow=m, ncol=nxc)
  for(j in 1:nxc) {
    Px[,j] <- dpois(xc[j], mod$lambda)
  }
  
  # 向前概率$\boldsymbol\alpha_t$对数值, $t=1,\dots,T$
  # 保存为$m \times T$矩阵
  la <- pois_HMM_lforward(x, mod)
  # 向后概率$\boldsymbol\beta_t$对数值, $t=1,\dots,T$
  # 保存为$m \times T$矩阵
  lb <- pois_HMM_lbackward(x, mod)
  # 下面是针对$t=1$的更改
  la <- cbind(log(mod$delta),la)
  # lafact, lbfact用于防止数值计算溢出
  lafact <- apply(la,2,max)
  lbfact <- apply(lb,2,max)
  for (i in 1:n) {
    # foo: 各$w_j(t)$系数，$j=1,\dots,m$，当$t=i$时的值
    if(i==1){
      foo <- exp(la[,1]-lafact[1]) * 
        exp(lb[,1]-lbfact[1])
    } else {
      foo <- (exp(la[,i]-lafact[i]) %*% mod$Gamma) * 
        exp(lb[,i]-lbfact[i])
    }
    foo <- foo/sum(foo)
    # dxc: 输出的时间点$t = i$时多个自变量值处的条件概率值，
    # 第i列对应于时间$t=i$
    dxc[i,]  <- foo %*% Px
  }
  
  return(dxc)
}
```

### 34.5.2 观测值的预测分布

预测分布指条件概率\(P(X\_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})\)，
可以看成是\(X\_{T+1}, \dots, X\_{T+h}\)缺失情况下的计算。
这时
\[
P(X\_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})
= \frac{P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)},
X\_{T+h}=x)}{
P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)})} ,
\]
分母即似然函数\(L\_T\)，
分子即观测\(X\_1, \dots, X\_{T+h}\)中\(X\_{T+1}, \dots, X\_{T+h-1}\)缺失，
而\(X\_{T+h}=x\)时的似然函数，
这用有缺失值情形下似然函数计算公式即可，
就是将缺失的\(x\_t\)对应的\(P(x\_t)\)用单位阵\(I\)代替。
表达式为
\[\begin{aligned}
& P(X\_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)}) \\
=& \frac{\boldsymbol\delta^T P(x\_1) B\_2 \dots, B\_T
\Gamma^h P(x) \boldsymbol 1}{
\boldsymbol\delta^T P(x\_1) B\_2 \dots, B\_T
\boldsymbol 1} \\
=& \frac{\boldsymbol\alpha\_T^T \Gamma^h P(x) \boldsymbol 1}{
\boldsymbol\alpha\_T^T \boldsymbol 1} .
\end{aligned}\]
记
\[
\boldsymbol\phi\_t
= \frac{\boldsymbol\alpha\_t}{\boldsymbol\alpha\_t^T \boldsymbol 1},
\]
即将\(\boldsymbol\alpha\_t\)归一化为和等于1，
则
\[\begin{align}
P(X\_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})
= \boldsymbol\phi\_T^T \Gamma^h P(x) \boldsymbol 1 .
\tag{34.16}
\end{align}\]

联合分布概率的预测也可以类似地计算。

由[(34.16)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-dec-fcdist-phi)，
预测分布概率\(P(X\_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})\)是观测条件分布\(p\_j(x)\)的混合分布，
\[\begin{align}
P(X\_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})
= \sum\_{j=1}^m \xi\_j(h) p\_j(x),
\tag{34.17}
\end{align}\]
其中\(\xi\_j(h)\)是行向量\(\boldsymbol\phi\_T^T \Gamma^h\)的第\(j\)元素。

设观测条件分布为泊松，
不假定马氏链平稳，
预测分布概率计算程序如下：

```
## 对泊松HMM，不假定马氏链平稳，计算如下预测分布概率：
## $P(X_{T+h} = x | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})$
## 输入xf为要计算的x值组成的向量
## 输入h为预测步数，对1:h每一步都计算
## 输入x为观测值向量
## 输入mod为自然参数列表
## 输出结果为矩阵，每一行对应一个预测步数
pois_HMM_forecast <- function(
    xf,
    h = 1,
    x,
    mod){
  n <- length(x)    # 观测长度
  nxf <- length(xf) # 需要计算取值条件概率的自变量x的个数
  # dxf为输出的预测条件概率矩阵，
  # 每一行代表一个h值的各个不同x的条件概率值
  dxf <- matrix(0, nrow=h, ncol=nxf)
  foo <- mod$delta * dpois(x[1], mod$lambda)
  sumfoo <- sum(foo)
  lscale <- log(sumfoo)
  foo <- foo/sumfoo
  for(i in 2:n){
    foo <- (foo %*% mod$Gamma) * dpois(x[i], mod$lambda)
    sumfoo <- sum(foo)
    lscale <- lscale + log(sumfoo)
    foo <- foo/sumfoo
  }
  # 现在foo保存了$\boldsymbol\phi_T$，即$\boldsymbol\alpha_T$归一化结果
  for(i in 1:h) {
    foo    <- foo %*% mod$Gamma
    for(j in 1:mod$m){
      dxf[i,] <- dxf[i,] + 
        foo[j] * dpois(xf,mod$lambda[j])
    } 
  }
  
  return(dxf)
}
```

因为有了预测分布，
所以可以用预测分布的最大概率对应的\(x\)（分布的众数值）作为点预测值，
可以计算预测区间。

当\(h \to \infty\)时，
马氏链会趋近于平稳分布\(\boldsymbol\delta^\*\)，
条件概率也趋近于平稳分布下的独立混合概率
\[
\sum\_{j=1}^m \delta\_j^\* p\_j(x) .
\]

### 34.5.3 解码

给定观测值\(\boldsymbol x^{(T)}\)，
希望得到\(C\_t\)的条件分布，
从而可以用条件分布的众数值作为\(C\_t\)的预测值，称为“解码”，
也给出\(C\_t\)的预测区间。
也可以计算联合条件分布。

术语“局部”解码指预测单个的\(C\_t\)值，
“全局”解码指预测\(C\_1, \dots, C\_T\)序列。

#### 34.5.3.1 状态的条件分布与局部解码

考虑向前概率\(\boldsymbol\alpha\_t\)和向后概率\(\boldsymbol\beta\_t\)，
有
\[
\alpha\_t(j) \beta\_t(j)
= P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)},
C\_t = j),
\]
见[34.4.1.3](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#hmm-em-fpbp-prop)。
由此可得状态的条件概率分布
\[\begin{align}
P(C\_t = j | \boldsymbol X^{(T)} = \boldsymbol x^{(T)})
=& \frac{P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)},
C\_t = j)}{P(\boldsymbol X^{(T)} = \boldsymbol x^{(T)})} \\
=& \frac{\alpha\_t(j) \beta\_t(j)}{L\_T} ,
\ j=1,2,\dots,m .
\tag{34.18}
\end{align}\]

\(L\_T\)的计算方法参见[34.3.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#hmm-dirml-sca)的归一化方法。
计算\(\alpha\_t(j) \beta\_t(j)\)也需要注意防止数值溢出，
可以计算对数值并归一化处理。

为了估计\(C\_t\)的值，
只要取
\[
j\_t^\*
= \text{argmax}\_j
P(C\_t = j | \boldsymbol X^{(T)} = \boldsymbol x^{(T)}) .
\]
这称为“局部解码”。

泊松HMM单个状态条件概率计算程序，
不要求马氏链平稳：

```
# 泊松HMM单个状态条件概率计算程序，
# 不要求马氏链平稳
# 输出$m \times T$的矩阵，
# 每列为一个时间点$t$处的条件概率分布
pois_HMM_state_probs <- function(
    x,    # 观测值
    mod){ # 模型自然参数列表
  n <- length(x)  # 观测长度
  # 向前概率对数值，矩阵
  la <- pois_HMM_lforward(x, mod)
  # 向后概率对数值，矩阵
  lb <- pois_HMM_lbackward(x, mod)
  # 归一化用：
  sc <- max(la[,n])
  # logLikelihood:
  llk <- sc + log(sum(exp(la[,n] - sc)))
  
  # 输出的条件概率矩阵，每列对应于一个时间$t$：
  stateprobs <- exp(la + lb - llk)
  
  return(stateprobs)
}
```

局部解码的程序：

```
# 泊松HMM，不要求马氏链平稳，局部解码
pois_HMM_local_decoding <- function(
    x,    # 观测数据
    mod){ # 自然参数列表
  n <- length(x)
  stateprobs <- pois_HMM_state_probs(x, mod)
  ild <- rep(NA_integer_, n)
  for(i in 1:n){
    ild[i] <- which.max(stateprobs[,i])[[1]]
  } 
  
  ild
}
```

#### 34.5.3.2 全局解码

注意求得每一个\(C\_t\)最可能的状态，
并不能组成整个序列\(C\_1, \dots, C\_T\)的最可能的状态。
这涉及到\(C\_1, \dots, C\_T\)在\(\boldsymbol X^{(T)} = \boldsymbol x^{(T)}\)条件下的联合分布的问题。
需要求\(\boldsymbol c^{(T)}\)，
最大化如下的联合条件概率：
\[
P(\boldsymbol C^{(T)} = \boldsymbol c^{(T)} |
\boldsymbol X^{(T)} = \boldsymbol x^{(T)}) .
\]

上式乘以常数\(L\_T\)就变成了\(\boldsymbol C^{(T)}\)和\(\boldsymbol X^{(T)}\)的联合分布概率，
只要最大化这个联合分布概率：
\[
P(\boldsymbol C^{(T)} = \boldsymbol c^{(T)},
\boldsymbol X^{(T)} = \boldsymbol x^{(T)})) .
\]
此概率计算公式为
\[\begin{align}
P(\boldsymbol C^{(T)} = \boldsymbol c^{(T)},
\boldsymbol X^{(T)} = \boldsymbol x^{(T)}))
= \delta\_{c\_1}
\prod\_{t=2}^T \gamma\_{c\_{t-1} c\_t}
\prod\_{t=1}^T p\_{c\_t}(x\_t) .
\tag{34.19}
\end{align}\]
这个最大化问题称为“全局解码”，
其结果可能与局部解码结果相近，
但不完全相同。

直接对[(34.19)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-dec-dec-glob-jp)最大化，
涉及到\((c\_1, \dots, c\_T)\)的\(m^T\)种组合的概率计算，
是不可行的。
但是，
注意到穷举\(m^T\)种组合涉及到大量的重复计算，
为了最大化[(34.19)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#eq:hmm-dec-dec-glob-jp)，
我们可以沿时间\(t\)递推地进行，
并记住那些有可能最优的路径，
忽略那些一定不是最优的路径，
这就是动态规划的思想。
针对这个问题已经有高效的Viterbi算法。

先用如下向前算法计算一系列的\(\{\xi\_{tj}, j=1,\dots,m,\; t=1,\dots, T \}\)。
令
\[
\xi\_{1,j}
= P(C\_1 = j, X\_1 = x\_1)
= \delta\_j p\_j(x\_1),
\ j=1,\dots,m,
\]
然后对\(t=2,3,\dots,T\)递推计算
\[
\xi\_{t,j}
= \max\_{c\_1, \dots, c\_{t-1}}
P(\boldsymbol C^{(t-1)} = \boldsymbol c^{(t-1)},
C\_t = j,
\boldsymbol X^{(t)} = \boldsymbol x^{(t)}),
\ j=1,\dots,m,
\]
可以证明
\[
\xi\_{t,j}
= p\_j(x\_t)
\max\_{k} (\xi\_{t-1,k} \gamma\_{kj}),
\ j=1,\dots,m.
\]
这样的计算量为\(O(T)\)数量级。

然后，
反向递推获得全局解码结果：
\[\begin{aligned}
j\_T =& \text{argmax}\_{j} \xi\_{T,j}, \\
j\_t =& \text{argmax}\_{j} (\xi\_{t,j} \gamma\_{j, j\_{t+1}} ),
\ t=T-1, T-2, \dots, 1 .
\end{aligned}\]

因为要最大化的都是乘积形式的，
而不是乘积的和，
所以可以最大化其对数值，
也可以对每个\(t\)把\(\xi\_{t,j}, j=1,\dots,m\)归一化为和等于1。
Viterbi算法不要求马氏链平稳。

泊松HMM全局解码Viterbi算法程序：

```
## 泊松HMM，不要求马氏链平稳
# 全局解码的Viterbi算法，估计最可能的马氏链时间序列
pois_HMM_viterbi <- function(
    x,     # 观测值向量
    mod){  # 自然参数列表
  n <- length(x)
  # 用来保存$\xi_{t,j}$，每行对应于一个$t$值，每行归一化为和等于1
  xi <- matrix(0, n, mod$m)
  foo <- mod$delta * dpois(x[1], mod$lambda)
  xi[1,] <- foo/sum(foo)
  for (i in 2:n){
    foo <- apply(xi[i-1,] * mod$Gamma, 2, max) * 
      dpois(x[i], mod$lambda)
    xi[i,] <- foo / sum(foo)
  }
  
  # 最优状态序列：
  jv <- numeric(n)
  jv[n] <- which.max(xi[n,])[[1]]
  for (i in (n-1):1){
    jv[i] <- which.max(mod$Gamma[,jv[i+1]]*xi[i,])[[1]]
  }
    
  return(jv)
}
```

### 34.5.4 状态的预测

可以证明，
\[\begin{aligned}
& L\_T P(C\_t = j |
\boldsymbol X^{(T)} = \boldsymbol x^{(T)}) \\
=& \begin{cases}
\alpha\_t(j) \beta\_t(j),
& t=1,2,\dots,T-1 (\text{平滑问题}),\\
\alpha\_T(j)
& t = T, (\text{滤波问题}), \\
\boldsymbol\alpha\_T^T \Gamma^{t-T}
\boldsymbol e\_j,
& t > T (\text{预测问题}).
\end{cases}
\end{aligned}\]
其中\(\boldsymbol e\_j\)是第\(j\)个单位向量，
即单位阵\(I\_m\)的第\(j\)列。

平滑和滤波即局部解码问题。

预报问题公式也可以写成
\[\begin{aligned}
P(C\_t = j |
\boldsymbol X^{(T)} = \boldsymbol x^{(T)})
= \boldsymbol\alpha\_T^T \Gamma^h \boldsymbol e\_j / L\_T
= \boldsymbol\phi\_T^T \Gamma^h \boldsymbol e\_j .
\end{aligned}\]
\(h \to \infty\)时此条件概率趋于平稳分布。

泊松HMM，不要求马氏链平稳，
状态预测的条件概率计算程序如下：

```
## 泊松HMM，不要求马氏链平稳
## 状态的向前$1:h$步预测条件概率
## 输出结果为预测条件概率的矩阵，
## 每一行对应于一个预测步数
pois_HMM_state_prediction <- function(
    h=1,   # 预测步数
    x,
    mod){
  n <- length(x)
  # 向前概率对数值，矩阵
  la <- pois_HMM_lforward(x, mod)
  # 用于防止溢出
  sc <- max(la[,n])
  # 对数似然函数值
  llk <- sc + log(sum(exp(la[,n] - sc)))
  
  statepreds <- matrix(NA_real_, nrow=h, ncol=mod$m)
  foo <- exp(la[,n] - llk)
  for (i in 1:h){
    foo <- foo %*% mod$Gamma
    statepreds[i,] <- foo
  }
  
  return(statepreds)
}
```

### 34.5.5 有监督和无监督的建模

这里讨论的HMM是时间序列的观点，
主要关心观测值\(\{X\_t \}\)时间序列的推断，
隐藏的马氏链仅用来描述\(\{X\_t \}\)的分布和序列相关性，
其状态取值并不重要。
这类似于“无监督”的学习。

另外一种情况是主要关心\(C\_t\)的估计，
这时一种做法是同时收集\((C\_t, X\_t)\)的值，
然后利用完全数据似然函数进行参数估计，
然后对没有\(C\_t\)仅有\(X\_t\)的数据，
利用已经估计的模型参数，
对\(C\_t\)进行局部或者全局解码。
这类似于“有监督”的学习。

## 34.6 模型选择与模型诊断

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.97第6章。

增加状态个数\(m\)能改变拟合，
但是会以\(m^2\)速度增加参数个数，
有过度拟合风险。
某些特殊的模型可能会精简状态转移矩阵或者条件分布，
使其仅依赖于少量的参数。

可以使用AIC、BIC准则比较不同的模型。
对于模型拟合的充分性，
可以计算伪残差，
进行残差诊断。

### 34.6.1 用AIC、BIC进行模型选择

实际数据建模，
可以有不同的选择，
比如隐藏状态个数\(m\)，
观测值的条件分布取哪一种条件分布，等等。

设真实模型为\(f\)，
对实际数据拟合了\(\hat g\_1\)和\(\hat g\_2\)两个模型，
比较的准则是与\(f\)的KL散度\(D\_{KL}(f || \hat g)\)在\(f\)模型下的期望值\(E\_f D\_{KL}(f || \hat g)\)的估计越小越好，
对HMM，一定条件下推出AIC准则：
\[
\text{AIC} = -2 \log L + 2 p .
\]
其中\(\log L\)是最大似然估计对应的最大值，
\(p\)是模型中需要估计的参数的个数。
AIC是统计学中频率学派的理论的结果。

另一种方法来自贝叶斯学派。
设两个候选模型来自\(G\_1\)和\(G\_2\)两个模型族，
计算后验概率\(P(f \in G\_j | \boldsymbol x^{(T)})\)，
选后验概率最大的一个模型。
对HMM，
一定条件下这推出BIC准则：
\[
\text{BIC} = -2 \log L + p \log T .
\]
只要\(T \geq 8\)就有\(\text{BIC} > \text{AIC}\)，
所以BIC倾向于比AIC选择更简单的模型。

对前面的大地震次数时间序列数据，
AIC和BIC都选择3个状态。
如果比较独立泊松混合，
也可以发现其准则值比3状态泊松HMM要差很多。

可以计算数据的ACF，
并计算估计模型的ACF，
看两者是否相近，
以此判断模型拟合是否充分。

### 34.6.2 用伪残差进行模型诊断

拟合了模型以后，
需要评估拟合是否充分，
找出拟合效果特别差的异常点。
在正态的线性回归模型建模时，
可以用残差来进行模型诊断；
在HMM等更一般的情形下，
可以定义“伪残差”，
或称分位数残差，
用来进行模型诊断。
这里介绍两种伪残差构造。

#### 34.6.2.1 伪残差的定义

设\(X\)服从连续分布，
分布函数为\(F(\cdot)\)，
则\(U = F(X)\)服从U(0,1)分布。
随机变量\(X\_t\)若观测值为\(x\_t\)，
在假定的模型下计算其分布函数值
\[
u\_t = P(X\_t \leq x\_t) = F\_{X\_t}(x\_t),
\]
则模型正确时\(u\_t\)应该服从U(0,1)分布，
取值异常的情况是\(u\_t\)靠近0或者1。
因为将不同的分布都转换到了0和1之间，
使得不同分布的观测值都可以比较。
设数据为\(x\_1, \dots, x\_T\),
模型为\(X\_t \sim F\_t\)，
\(x\_t\)是\(X\_t\)的观测值，
因为分布不同，
这些\(x\_t\)是不可比的。
计算\(u\_t = F\_t(x\_t)\)，
称\(u\_1, \dots, u\_T\)为均匀伪残差，
这些伪残差是可比的。
可以作\(u\_1, \dots, u\_T\)的直方图和相对于均匀分布的QQ图，
如果与均匀分布表现有明显差异，
就说明模型设定有误。

均匀残差用于识别异常值则不太方便，
\(0.01\)分位数和\(0.05\)分位数也仅相差\(0.04\)，
如果在正态分布中就已经相差很大了。
因为我们熟悉正态分布，
所以定义正态伪残差
\[
z\_t = \Phi^{-1}(u\_t)
= \Phi^{-1}(F\_t(x\_t)),
\]
更容易识别异常值。
模型正确时正态伪残差应该表现为标准正态分布样本。
正态伪残差的值反映了\(x\_t\)偏离其分布中位数（不是均值）的偏离程度。
可以作直方图、正态QQ图、正态性检验验证模型是否正确。

以上基于均匀分布的伪残差仅适用于连续型分布。
对离散分布，
可以修改以上的定义。
这时伪残差不再是单个值，
而是一个区间，
这是因为，
如果\(X\_t\)在\(\{1, 2, \dots, m \}\)内取值，
在\(X\_t = i\)可以看成\(X\_t \in [i-0.5, i+0.5]\)。
若\(X\_t\)服从离散分布，
则其分布函数\(F\_t(x)\)是一个右连续的阶梯函数，
记
\[
F(x\_t-) = \lim\_{\delta \downarrow 0} F(x\_t - \delta),
\]
则\(F\_t(x\_t) - F\_t(x\_t-) = P(X\_t = x\_t)\)。
定义
\[
[u\_t^{-}, u\_t^+]
= [F\_t(x\_t-), F\_t(x\_t)],
\]
则区间\([u\_t^{-}, u\_t^+]\)的长度恰好是\(X\_t\)取\(x\_t\)的概率值，
称为均匀伪残差区间。
类似可以定义正态伪残差区间
\[
[z\_t^{-}, z\_t^+]
= [\Phi^{-1}(u\_t^{-}), \Phi^{-1}(u\_t^+)] .
\]

均匀伪残差区间和正态伪残差区间都可以用来表示\(x\_t\)值是否很极端，
而均匀伪残差区间则更为直观，
因为区间越短，
就代表了在模型正确时这个值越难被观测到。
另外当模型正确时，
\(u\_t^+\)可以表示出现\(u\_t\)以及小于\(u\_t\)的值的概率，
\(1 - u\_t^+\)表示出现比\(u\_t^-\)更大的值的概率。
模型正确时，
伪随机区间可以看成标准均匀分布（或者标准正态分布）的区间删失观测值。
这样的看法仅当模型参数已知时正确，
但未知参数较少时用最大似然估计代替也近似正确。

如果要绘制QQ图，
可以定义“伪残差区间中点”
\[
z\_t^m = \Phi^{-1} \left( \frac{u\_t^- + u\_t^+}{2} \right) .
\]

在HMM中也可以使用伪残差，
检查模型设定是否正确，
以及单时间点的拟合是否特别差。

因为伪残差计算依赖于理论分布，
所以HMM中伪残差有两种：
按基于所有观测值的条件分布计算的伪残差，
称为“普通”伪残差；
按截止到\(t-1\)时刻的观测值给定下的条件分布计算的\(t\)时刻伪残差，
称为“预测”伪残差。

伪残差最重要的性质是其分布近似标准均匀分布（或者标准正态分布），
而不能假定其相互独立，
伪残差之间是不独立的。

#### 34.6.2.2 HMM的普通伪残差

为了判断拟合是否充分，
一种办法是对每一个时间点\(t\)，
判断\(x\_t\)是否与其它观测明显偏离，
办法是在\(\boldsymbol x^{(-t)}\)已知的条件下计算\(x\_t\)的伪残差。
正态伪残差（连续分布情形）为
\[
z\_t = \Phi^{-1}(P(X\_t \leq x\_t |
\boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})) .
\]
模型正确时各\(z\_t\)应服从标准正态分布。

分布离散时，
计算正态伪残差区间\([z\_t^-, z\_t^+]\)：
\[\begin{aligned}
z\_t^- =& \Phi^{-1}(P(X\_t < x\_t |
\boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})) , \\
z\_t^+ =& \Phi^{-1}(P(X\_t \leq x\_t |
\boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})) .
\end{aligned}\]

模型正确时\(P(X\_t = x | \boldsymbol X^{(-t)} = \boldsymbol x^{(-t)})\)的计算参见[34.5.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/hmm.html#hmm-dec-conddist)。
连续情形则表示条件密度。

#### 34.6.2.3 HMM的预测伪残差

另一种残差是从预测角度考虑，
基于\(\boldsymbol x^{(t-1)}\)下的条件分布计算\(x\_t\)的伪残差，
称为预测伪残差。
对连续型分布，
令
\[
z\_t = \Phi^{-1}(P(X\_t \leq x\_t
| \boldsymbol X^{(t-1)} = \boldsymbol x^{(t-1)})) .
\]
对离散型分布，
令
\[\begin{aligned}
z\_t^- =& \Phi^{-1}(P(X\_t < x\_t
| \boldsymbol X^{(t-1)} = \boldsymbol x^{(t-1)})) , \\
z\_t^+ =& \Phi^{-1}(P(X\_t \leq x\_t
| \boldsymbol X^{(t-1)} = \boldsymbol x^{(t-1)})) .
\end{aligned}\]

离散情况下，
预测分布为
\[
P(X\_t = x | \boldsymbol X^{(t-1)} = \boldsymbol x^{(t-1)})
= \frac{\boldsymbol\alpha\_{t-1}^T \Gamma P(x) \boldsymbol 1}{
\boldsymbol\alpha\_{t-1}^T \boldsymbol 1} .
\]

预测伪残差可以用来分析下一个观测是否还符合原有的模型，
或者与原有观测相比是一个异常值。

### 34.6.3 例子

#### 34.6.3.1 泊松HMM伪残差计算

考虑泊松HMM，不要求马氏链平稳，
计算普通伪残差的计算程序如下：

```
## 泊松HMM，不要求马氏链平稳
## 计算普通伪残差，表示为区间
## 输出为三列的矩阵，
## 行数等于观测长度，
## 第二、三列是普通正态伪残差区间下界、上界，
## 第一列是伪残差区间中点转换的正态逆变换值
pois_HMM_pseudo_residuals <- function(
    x,
    mod){
  n <- length(x)
  # 观测值的条件分布
  cdists   <- pois_HMM_conditional(
    xc=0:max(x), x, mod)
  # 将离散的条件分布转换为累积分布函数值
  cumdists <- cbind(
    0,
    t(apply(cdists, 1, cumsum)))
  # 用来保存每个时间点的伪残差区间下界和上界
  ulo <- uhi <- rep(NA_real_, n)
  for (i in 1:n)  {
    ulo[i]  <- cumdists[i, x[i]]
    uhi[i]  <- cumdists[i, x[i]+1]
  }
  # 伪残差区间中点
  umi <- 0.5*(ulo + uhi)
  # 转换为正态伪残差
  npsr <- qnorm(cbind(ulo, umi, uhi))
  
  return(npsr)
}
```

#### 34.6.3.2 伪残差序列相关性的例子

伪残差序列\(z\_t, t=1,\dots,T\)并不一定是独立的。
下面举一个时间序列的例子说明。
设\(X\_t\)服从平稳的零均值AR(1)：
\[
X\_t = \phi X\_{t-1} + \varepsilon\_t,
\ t=1,2,\dots, T,
\]
\(\varepsilon\_t\)独立同\(\text{N}(0, 1)\)分布。
\(|\phi| < 1\)，
\[
\text{Var}(X\_t) = \frac{1}{1 - \phi^2} .
\]

这个模型具有马氏性，
所以可以证明，
\(X\_t\)在给定\(\boldsymbol x^{(-t)}\)后的条件分布，
等于\(X\_t\)在给定\(x\_{t-1}\)和\(x\_{t+1}\)后的条件分布。

\((X\_{t-1}, X\_t, X\_{t+1})\)服从多元正态分布，
均值为0，
方差阵为
\[
\frac{1}{1 - \phi^2}
\begin{pmatrix}
1 & \phi & \phi^2 \\
\phi & 1 & \phi \\
\phi^2 & \phi & 1
\end{pmatrix} .
\]
由此可求得\(X\_t | x\_{t-1}, x\_{t+1}\)的条件分布为
\[
\text{N}(\frac{\phi}{1 + \phi^2}(X\_{t-1} + X\_{t+1}),
\ \frac{1}{1 + \phi^2}) .
\]
普通均匀伪残差为
\[
U\_t
= \Phi\left( \sqrt{1 + \phi^2}
(X\_t - \frac{\phi}{1 + \phi^2}(X\_{t-1} + X\_{t+1}))
\right),
\]
于是正态伪残差为
\[
Z\_t
= \sqrt{1 + \phi^2}
(X\_t - \frac{\phi}{1 + \phi^2}(X\_{t-1} + X\_{t+1})) .
\]
显然\(Z\_t\)服从正态分布，
计算可得\(Z\_t \sim \text{N}(0, 1)\)。
这是原始序列的线性滤波所以是高斯平稳列。

但是，
\(Z\_1, Z\_2, \dots, Z\_T\)是相关列。
由联合正态分布计算可得
\[
\text{corr}(Z\_t, Z\_{t+1})
= \frac{-\phi}{1 + \phi^2},
\]
这与\(\text{corr}(X\_t, X\_{t+1}) = \phi\)符号相反且绝对值变小。
计算可得
\[
\text{corr}(Z\_t, Z\_{t+2}) = 0,
\]
并可证明
\[
\text{Cov}(Z\_t, Z\_{t+k})
= \phi \text{Cov}(Z\_t, Z\_{t+k-1}),
\ k \geq 3,
\]
从而\(\{Z\_t \}\)的自相关系数是一步截尾的，
是一个MA(1)序列。

## 34.7 泊松HMM的贝叶斯推断

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.111第7章。

HMM建模的另一种思路是利用贝叶斯体系。

### 34.7.1 对泊松HMM应用Gibbs抽样

#### 34.7.1.1 介绍

考虑\(m\)个状态的泊松HMM。
给定参数\(\boldsymbol\lambda = (\lambda\_1, \dots, \lambda\_m)^T\)和\(\Gamma\)的先验分布，
观测数据\((y\_1, \dots, y\_T)\)，
要求参数的后验分布，
以及状态的后验分布。
使用Gibbs抽样方法估计，
设法生成服从后验分布的大批样本。

对\(\Gamma\)的第\(r\)行，
设先验分布服从Dirichlet分布，
参数为\(\boldsymbol\nu\_r\)。
对\(\boldsymbol\lambda\)，
设其元素递增，
设两项间的增量\(\lambda\_j - \lambda\_{j-1}\)先验分布为Gamma(\(a\_j, b\_j\))分布。
设所有这些先验分布相互独立。

称随机变量\((Y\_1, \dots, Y\_m)\)服从参数为\((\nu\_1, \dots, \nu\_m)\)的Dirichlet分布，
若其分布密度正比于
\[
y\_1^{\nu\_1 - 1} y\_2^{\nu\_2 - 1}
\dots y\_m^{\nu\_m - 1} .
\]

Gamma分布密度为
\[
f(x, a, b)
= \frac{b^a}{\Gamma(a)} x^{a-1} e^{-b x} , x > 0 .
\]
均值为\(a/b\)，
方差为\(a/b^2\)，
变异系数为\(1/\sqrt{a}\)。

如果能获得状态马氏链的观测值，
就可以计算\(\Gamma\)的后验密度；
在泊松HMM中状态未知，
可以产生状态的模拟值，
然后计算后验密度。

将观测值\(x\_t\)看成是\(x\_{jt}\)的求和，
其中\(x\_{jt}\)是第\(j\)种“机制”对\(x\_t\)的贡献；
当\(C\_t = i\)时，
称机制\(1, \dots, i\)是“活跃的”，
机制\(i+1, \dots, m\)是“不活跃”的。

用\(\tau\_j = \lambda\_j - \lambda\_{j-1}\)作为条件泊松分布参数的另外表示。
这可以避免各个状态的条件分布没有一定的次序的问题。
\(\tau\_j\)看成是机制\(j\)的平均贡献。

泊松HMM的Gibbs抽样算法框架如下：

* 给定观测值\(\boldsymbol x^{(T)}\)和当前参数值\(\boldsymbol\lambda\)、\(\Gamma\)，
  模拟生成马氏链的一条路径；
* 利用模拟路径，将观测值分解为机制的贡献；
* 利用模拟路径和机制贡献，
  更新\(\Gamma\)和\(\boldsymbol\tau\)，并转换得到\(\boldsymbol\lambda\)。

将上述步骤重复许多次，
经过一段“烧入”时间后，
分布稳定下来，
就可以认为每一次重复得到的参数值是一个后验分布样本点，
但样本点之间是不独立的。

详细算法略。

### 34.7.2 状态个数的选择

从贝叶斯观点看，
状态个数\(m\)的选择，
应该从其后验分布\(p(m | \boldsymbol x^{(T)})\)求众数获得。
但是\(p(m | \boldsymbol x^{(T)})\)是出名的难以计算。
\[
p(m | \boldsymbol x^{(T)})
\propto p(\boldsymbol x^{(T)}) | m) \pi(m),
\]
其中\(p(\boldsymbol x^{(T)}) | m)\)称为“积分似然”，
需要将参数\(\boldsymbol\lambda\)和\(\Gamma\)的部分积分去掉。

还有一种选择\(m\)的方法是“并行抽样”。

具体方法略。

## 34.8 R中的隐马氏模型建模扩展包

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.123第8章。

可用的R扩展包：

* depmixS4;
* HiddenMarkov;
* msm;
* R2OpenBUGS(用于贝叶斯估计);
* HMM(仅支持类别值时间序列)。

### 34.8.1 depmixS4扩展包

depmixS4支持多种相依的混合模型，
包括HMM的估计与解码。

设R向量`x`保存了泊松HMM观测值序列，
长度为`n`，
先用`depmix()`函数定义一个模型如下：

```
  mod <- depmix(
  x ~ 1, nstats=3, 
  family = poisson(),
  ntimes = n)
```

然后可以用`fit(mod)`估计模型，
用`summary()`显示估计结果，
用`forwardbackward()`进行局部解码。
支持的观测值条件分布还有二项分布、正态分布、伽马分布。

如：

```
library(depmixS4)
```

```
## Warning: package 'depmixS4' was built under R version 4.2.3
```

```
## Loading required package: nnet
```

```
## Loading required package: MASS
```

```
## 
## Attaching package: 'MASS'
```

```
## The following object is masked from 'package:dplyr':
## 
##     select
```

```
## Loading required package: Rsolnp
```

```
## Loading required package: nlme
```

```
## 
## Attaching package: 'nlme'
```

```
## The following object is masked from 'package:dplyr':
## 
##     collapse
```

```
x <- da_quakes[["quakes"]]
mod <- depmix(x ~ 1, nstates=3,
  family=poisson(), ntimes=length(x))
mfit <- fit(mod)
```

```
## converged at iteration 24 with logLik: -328.5275
```

```
summary(mfit)
```

```
## Initial state probabilities model 
## pr1 pr2 pr3 
##   0   0   1 
## 
## Transition matrix 
##         toS1  toS2  toS3
## fromS1 0.810 0.190 0.000
## fromS2 0.053 0.906 0.040
## fromS3 0.029 0.032 0.939
## 
## Response parameters 
## Resp 1 : poisson 
##     Re1.(Intercept)
## St1           3.392
## St2           2.981
## St3           2.575
```

注意其中输出的响应参数是各个\(\lambda\_j\)的自然对数值。
本扩展包将马氏链初始分布也作为未知参数估计。

每个时间点的状态的条件概率可以用如下程序获得：

```
forwardbackward(mfit)$gamma
```

结果是\(T \times m\)的条件概率矩阵。

为了获得Viterbi算法全局解码结果，程序如：

```
posterior(mfit, type="viterbi")$state
```

```
##   [1] 3 3 3 3 3 1 1 1 1 1 1 2 2 2 2 2 2 2 2 3 3 3 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2
##  [38] 2 2 2 2 2 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 2 2 2
##  [75] 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
```

### 34.8.2 HiddenMarkov扩展包

HiddenMarkov扩展包支持的观测值条件分布有泊松、贝塔、伽马、正态、对数正态和二项分布。

先用`dthmm()`规定模型，
需要人为指定参数初值。
用`BaumWelch()`估计模型，
用`sumamry()`显示结果。
估计使用EM算法，
将初始分布也作为参数进行估计。
如：

```
library(HiddenMarkov)
x <- da_quakes[["quakes"]]
mod <- dthmm(
  x, 
  Pi = matrix(c(
    0.9, 0.05, 0.05, 
    0.05, 0.9, 0.05, 
    0.05, 0.05, 0.9 ), 
    3, 3, byrow=TRUE),
  delta = rep(1/3, 3),
  "pois", 
  list(lambda = c(15,18,23)))
# 最大似然估计的EM：
mfit <- BaumWelch(mod)
```

```
## iter = 1 
## LL = -345.0987621 
## diff = Inf 
## 
## iter = 2 
## LL = -335.1567736 
## diff = 9.941988 
## 
## iter = 3 
## LL = -332.2581270 
## diff = 2.898647 
## 
## iter = 4 
## LL = -330.3949181 
## diff = 1.863209 
## 
## iter = 5 
## LL = -329.3371983 
## diff = 1.05772 
## 
## iter = 6 
## LL = -328.8611716 
## diff = 0.4760267 
## 
## iter = 7 
## LL = -328.6611607 
## diff = 0.2000109 
## 
## iter = 8 
## LL = -328.5806748 
## diff = 0.08048593 
## 
## iter = 9 
## LL = -328.5488777 
## diff = 0.03179715 
## 
## iter = 10 
## LL = -328.5362136 
## diff = 0.0126641 
## 
## iter = 11 
## LL = -328.5310856 
## diff = 0.005128018 
## 
## iter = 12 
## LL = -328.5289800 
## diff = 0.002105603 
## 
## iter = 13 
## LL = -328.5281075 
## diff = 0.0008724087 
## 
## iter = 14 
## LL = -328.5277442 
## diff = 0.0003633288 
## 
## iter = 15 
## LL = -328.5275925 
## diff = 0.0001517269 
## 
## iter = 16 
## LL = -328.5275290 
## diff = 6.344728e-05 
## 
## iter = 17 
## LL = -328.5275025 
## diff = 2.654819e-05 
## 
## iter = 18 
## LL = -328.5274914 
## diff = 1.111142e-05 
## 
## iter = 19 
## LL = -328.5274867 
## diff = 4.65095e-06
```

```
summary(mfit)
```

```
## $delta
## [1] 1.000000e+00 1.112276e-30 1.145858e-80
## 
## $Pi
##              [,1]       [,2]       [,3]
## [1,] 9.393347e-01 0.03202454 0.02864073
## [2,] 4.037278e-02 0.90645412 0.05317310
## [3,] 1.168281e-12 0.19022948 0.80977052
## 
## $nonstat
## [1] TRUE
## 
## $distn
## [1] "pois"
## 
## $pm
## $pm$lambda
## [1] 13.13379 19.71156 29.70739
## 
## 
## $discrete
## [1] TRUE
## 
## $n
## [1] 107
```

`mfit$u`中还包括了状态的条件分布。
为了进行全局解码，
用`Viterbi()`，
如：

```
Viterbi(mfit)
```

```
##   [1] 1 1 1 1 1 3 3 3 3 3 3 2 2 2 2 2 2 2 2 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2
##  [38] 2 2 2 2 2 3 3 3 3 3 3 3 3 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 3 3 3 2 2 2
##  [75] 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```

可以用`residuals(mfit)`获得伪残差的值，
是从区间修正计算为单点值的正态伪残差的结果。

### 34.8.3 msm扩展包

msm支持的观测值条件分布包括泊松、正态、伽马、二项分布。
主要支持对连续状态马氏链的估计。

用`msm()`函数拟合模型，
如：

```
library(msm)
```

```
## Warning: package 'msm' was built under R version 4.2.3
```

```
x <- da_quakes[["quakes"]]
ti <- seq(along=x)
mfit <- msm(
  x ~ ti,
  qmatrix = matrix(c(
    -0.1, 0.05, 0.05,
    0.05, -0.1, 0.05,
    0.05, 0.05, -0.1
  ), 3, 3, byrow=TRUE),
  hmodel = list(
    hmmPois(rate=15),
    hmmPois(rate=18),
    hmmPois(rate=23) ))
mfit
```

```
## 
## Call:
## msm(formula = x ~ ti, qmatrix = matrix(c(-0.1, 0.05, 0.05, 0.05,     -0.1, 0.05, 0.05, 0.05, -0.1), 3, 3, byrow = TRUE), hmodel = list(hmmPois(rate = 15),     hmmPois(rate = 18), hmmPois(rate = 23)))
## 
## Maximum likelihood estimates
## 
## Transition intensities
##                   Baseline                          
## State 1 - State 1 -0.0634154 (-2.970e-01,-1.354e-02)
## State 1 - State 2  0.0318147 ( 1.600e-03, 6.326e-01)
## State 1 - State 3  0.0316007 ( 2.679e-03, 3.728e-01)
## State 2 - State 1  0.0422330 ( 8.452e-03, 2.110e-01)
## State 2 - State 2 -0.1033492 (-3.595e-01,-2.971e-02)
## State 2 - State 3  0.0611162 ( 1.040e-02, 3.593e-01)
## State 3 - State 1  0.0000255 ( 1.644e-51, 3.956e+41)
## State 3 - State 2  0.2168013 ( 5.702e-02, 8.243e-01)
## State 3 - State 3 -0.2168268 (-8.243e-01,-5.704e-02)
## 
## Hidden Markov model, 3 states
## State 1 - poisson distribution
## Parameters: 
##      Estimate      LCL      UCL
## rate 13.13418 11.90136 14.49469
## 
## State 2 - poisson distribution
## Parameters: 
##      Estimate      LCL     UCL
## rate 19.72041 18.08191 21.5074
## 
## State 3 - poisson distribution
## Parameters: 
##      Estimate      LCL     UCL
## rate 29.71422 26.59722 33.1965
## 
## 
## -2 * log-likelihood:  657.1835
```

因为拟合的是连续时间的隐马氏链，
所以初始参数输入的是Q矩阵（转移强度矩阵），
输出的也是Q矩阵。
提取状态转移矩阵如下：

```
pmatrix.msm(mfit)
```

```
##             State 1    State 2    State 3
## State 1 0.939219983 0.03236026 0.02841975
## State 2 0.038949406 0.90823133 0.05281926
## State 3 0.004057823 0.18528279 0.81065939
```

msm采用了将离散时间马氏链嵌入到连续时间马氏链的做法，
有些离散时间马氏链是不可嵌入的。

全局解码：

```
pmatrix.msm(mfit)
```

## 34.9 HMM中一般的观测值条件分布

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.133第9章。

前面以泊松HMM为例介绍了HMM的一般理论，
这里先将观测值条件分布推广到一般的一元分布，
再推广到多元分布，
并区分多项分布与其它多元分布。

### 34.9.1 一般的一元观测值条件分布

HMM的观测值条件分布并没有限制，
可以是离散型、连续型或者混合分布，
甚至于不同状态下的分布属于不同分布族。

在计算似然函数的过程中涉及到观测值条件分布的是各个对角阵
\[
P(x\_t)
= \text{diag}(P(X\_t = x\_t | C\_t=1), \dots, P(X\_t = x\_t | C\_t=m)) .
\]
只要对\(X\_t | C\_t = j\)的条件分布进行适当的参数化，
改用无约束的参数变换，
或者使用约束优化算法即可。

下面举一些例子。

#### 34.9.1.1 无上界的计数值

泊松分布是最常用的无上界计数值分布模型。
一个替代是负二项分布：
\[
p(x)
= \frac{\Gamma(x + \eta^{-1})}{\Gamma(\eta^{-1}) \Gamma(x+1)}
\left( 1 + \eta \mu \right)^{-\eta^{-1}}
\left( \frac{\eta \mu}{ 1 + \eta \mu} \right)^x ,
\ x=0,1,2,\dots
\]
其中\(\mu>0\)为均值，\(\eta>0\)。
当\(\eta=1\)时为几何分布，
\(\eta \to 0\)时为泊松分布。

#### 34.9.1.2 二值数据

当观测值条件分布仅取0,1值时，
HMM是最简单的模型。
这时
\[
p(x) = \begin{cases}
\pi, & x = 1, \\
1-\pi, & x = 0 .
\end{cases}
\]

#### 34.9.1.3 有界计数数据

二项分布是常用的有界计数模型，
仅依赖于成功概率\(\pi\)。
\(P(X\_t = x\_t | C\_t = j)\)的条件分布为\(\text{B}(n\_t, \pi\_j)\)。
注意这个条件分布可能还依赖于时间\(t\)。

#### 34.9.1.4 连续取值数据

若观测值取值连续，
则条件分布为条件密度。
可以用正态、伽马、对数正态等分布族，
也可以对条件密度进行非参数估计。

正态混合分布的似然函数是无上界的，
只要在第\(t\)项中令\(\mu\_i = x\_i\)同时\(\sigma\_i \to 0\)，
这时
\[
\frac{1}{\sqrt{2\pi} \sigma\_i}
e^{-\frac{(x\_t - \mu\_i)^2}{2\sigma\_i^2}}
= \frac{1}{\sqrt{2\pi} \sigma\_i}
\to \infty,
\]
所以似然函数就会趋于\(\infty\)。
虽然优化算法常常会错过似然函数的这些尖峰，
但还是应该保证算法的稳定性。
可以预先设置方差的下界，
或者使用“离散化似然函数”，
即将\(1.2\)这样的数值看成是\([1.15, 1.25]\)这样的区间删失值。

#### 34.9.1.5 比例数据

取值在\([0, 1]\)中的比例数据常用贝塔分布：
\[
f(x)
= \frac{1}{B(a,b)} x^{a-1} (1-x)^{b-1},
\ x \in [0, 1] .
\]
如果需要给一组初始参数，
可以用均值\(\mu = a/(a+b)\)，
和精度\(\nu=\alpha+\beta\)。

#### 34.9.1.6 周期值数据

方向角度，
一天中的时间等属于周期取值的数据。
折叠的正态、t都可以，
比较常用的是von Mises分布。
密度为
\[
f(x)
= (2 \pi I\_0(\kappa))^{-1}
\exp(\kappa \cos(x-\mu)),
\ x \in (-\pi, \pi] .
\]
其中\(\mu\)为期望值，
\(\kappa>0\)是分布集中度，
\(\kappa\)越大分布越集中在\(\mu\)附近，
\(I\_0\)是0阶第一类修正贝塞尔函数。

### 34.9.2 多项分布和分类数据

#### 34.9.2.1 多项分布

多项分布是二项分布的推广，
在\(C\_t = j\)条件下，
\(X\_t\)是一个\(q\)元随机向量，
代表\(q\)种结果各自的个数，
且已知\(\sum\_{i=1}^q x\_{t,i} = n\_t\)，
第\(i\)种结果的概率为\(\pi\_{ji}\),
\(\sum\_{i=1}^q \pi\_{ji} = 1\)。

似然函数为
\[
L\_T
= \boldsymbol\delta^T
P(n\_1, x\_1)
\Gamma P(n\_2, x\_2)
\dots
\Gamma P(n\_T, x\_T)
\boldsymbol 1 .
\]
其中
\[
P(n\_t, x\_t)
= \text{diag}(p\_1(n\_t, x\_t), \dots, p\_m(n\_t, x\_t)),
\]
而
\[
p\_j(n\_t, x\_t)
= P(X\_t = x\_t | C\_t = j)
= \binom{n\_t}{x\_{t,1}, \dots, x\_{t,q}}
\pi\_{j1}^{x\_{t,1}} \dots \pi\_{jq}^{x\_{t,q}} .
\]
其中
\[
\binom{n\_t}{x\_{t,1}, \dots, x\_{t,q}}
= \frac{n\_t!}{x\_{t,1}! \dots x\_{t,q}!} .
\]

可以对参数进行适当参数化，然后求最大似然估计。

#### 34.9.2.2 分类数据

在多项分布中，
如果\(n\_t\)恒等于1，
则每次\(x\_t\)可以看成标量，
仅取\(q\)个类中的某一个类。
这时似然函数可以简化，
若\(x\_t\)取第\(i\)类，
则
\[
P(x\_t = i | C\_t = j)
= \pi\_{ji},
\]
于是\(P(x\_t) = \text{diag}(\pi\_{i1}, \dots, \pi\_{im})\)。

#### 34.9.2.3 Dirichlet条件分布

如果\(x\_t\)是\(q\)个比例且和等于0，
就可以用Dirichlet分布。
这是伽马分布的推广。

### 34.9.3 条件分布为一般多元分布情形

#### 34.9.3.1 纵向条件独立

设观测值\(X\_t\)为\(q\)元随机向量。
如果给定马氏链\(C\_1, \dots, C\_T\)，
则\(X\_1, \dots, X\_T\)条件独立，
则称其为纵向条件独立的，
与后面的同期条件独立相区分。

这样的模型需要给出\(X\_t\)在\(C\_t=j\)下的条件分布模型。
不要求不同状态下的条件分布来自同一分布族。

#### 34.9.3.2 同期条件独立

设观测值\(X\_t\)为\(q\)元随机向量。
如果给定马氏链\(C\_1, \dots, C\_T\)，
则\(X\_1, \dots, X\_T\)条件独立，
且每个\(X\_t\)的各个分量条件独立，
则称其为同期条件独立。
这时\(P(X\_t = x\_t | C\_t = j)\)由各个分量的条件概率乘积给出：
\[
P(X\_t = x\_t | C\_t = j)
= \prod\_{i=1}^q P(X\_{ti} = x\_{ti} | C\_t = j) .
\]

以上的两种独立性都不能推出观测值各个分量时间序列为独立序列，
也不能推出各分量时间序列之间独立。

## 34.10 协变量以及其它相依性

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.145第10章。

如时间趋势、季节项之类的影响，
可以作为非随机的协变量引入模型中。
还可以考虑隐藏状态模型为二阶或高阶马氏链的情形。
还可以放松对条件独立性的假定。

### 34.10.1 含有协变量的HMM

可以运行观测值条件分布参数或者马氏链转移概率依赖于协变量。
这样仍可以进行最大似然估计。
协变量的值看做是已知的。

#### 34.10.1.1 观测值条件分布参数依赖于协变量值的情形

设观测值仍为\(X\_t\)，
但存在协变量\(\boldsymbol y\_t\)，
\(X\_t | C\_t = j\)的条件分布，
实际上是依赖于\(\boldsymbol y\_t\)的值。
比如，
在泊松HMM中，
设\(X\_t | C\_t = j\)服从参数为\(\lambda\_{jt}\)的泊松分布，
其中
\[
\log\lambda\_{jt} = \boldsymbol\beta\_j^T \boldsymbol y\_t.
\]
若模型为二项HMM，
设\(X\_t | C\_t = j\)服从参数为\((n\_t, \pi\_{jt})\)的条件分布，其中
\[
\text{logit} \pi\_{jt}
= \boldsymbol\beta\_j^T \boldsymbol y\_t.
\]

\(\boldsymbol\beta\_j^T \boldsymbol y\_t\)可以包含非随机趋势和季节成分，
比如在二项HMM中取
\[
\text{logit} \pi\_{jt}
= \beta\_{j1} + \beta\_{j2} t
+ \beta\_{j3} \cos(\frac{2\pi}{12} t)
+ \beta\_{j4} \sin(\frac{2\pi}{12} t)
+ \beta\_{j5} z\_t + \beta\_{j6} w\_t,
\]
其中\(z\_t, w\_t\)是观测到的协变量。

似然函数仅改变了计算公式中的\(P(x\_t)\)部分，
变成\(P(x\_t, \boldsymbol y\_t)\)，
仍为对角阵，
第\(j\)对角元素是在第\(j\)状态下\(x\_t\)的条件概率（或条件密度）。
\[
L\_T
= \boldsymbol\delta^T P(x\_1, \boldsymbol y\_1)
\Gamma P(x\_2, \boldsymbol y\_2)
\dots
\Gamma P(x\_T, \boldsymbol y\_T)
\boldsymbol 1 .
\]

还可以考虑带有变点的协变量模型，
比如泊松HMM取
\[
\log\lambda\_{jt}
= \begin{cases}
\mu\_{j1}, & t \leq t^\*, \\
\mu\_{j2}, & t > t^\* .
\end{cases}
\]

以上的例子实际上是推广了逻辑斯谛回归、泊松回归这样的广义线性模型，
原来广义线性模型在给定了协变量（自变量）值以后是条件独立的，
现在需要给定协变量以及不可观测的潜在状态后才能条件独立。

#### 34.10.1.2 转移概率依赖于协变量值的情形

设转移概率矩阵\(\Gamma\)通过协变量依赖于时间\(t\)，
这样状态过程不是时齐的马氏链，
当然也不会平稳。
如果是两状态的，
协变量可以用logit变换引入到\(\gamma\_{12}\)和\(\gamma\_{21}\)中，
如果有更多状态，
协变量的引入比较复杂，
需要保证\(\Gamma\)的行和等于1以及非负条件。
为了解释协变量对观测值的作用，
有时需要这样引入。

### 34.10.2 基于二阶马氏链的HMM

#### 34.10.2.1 二阶马氏链

用AR模型类比，
高斯平稳的AR(1)是马氏过程：
\[
y\_t = \phi y\_{t-1} + \varepsilon\_t,
\]
这时给定\(y\_{t-1}\)条件下\(y\_t\)的条件分布，
等于给定\(y\_{t-1}, y\_{t-2}, \dots\)下的条件分布。

如果是二阶的AR:
\[
y\_t = \phi\_1 y\_{t-1} + \phi\_2 y\_{t-2} + \varepsilon\_t,
\]
则需要在给定\(y\_{t-1}, y\_{t-2}\)条件下，
\(y\_t\)的条件分布才等于给定\(y\_{t-1}, y\_{t-2}, \dots\)下的条件分布。

令\(\boldsymbol\alpha\_t = (y\_t, y\_{t-1})^T\)，
则\(\boldsymbol\alpha\_t\)是马氏过程。
事实上有
\[
\begin{pmatrix}
y\_t \\ y\_{t-1}
\end{pmatrix}
=
\begin{pmatrix}
\phi\_1 & \phi\_2 \\
1 & 0
\end{pmatrix}
\begin{pmatrix}
y\_{t-1} \\ y\_{t-2}
\end{pmatrix}
+
\begin{pmatrix}
\varepsilon\_t \\ 0
\end{pmatrix} .
\]
这称为AR模型的马氏扩张。

对于离散状态随机过程\(\{C\_t \}\)，
如果给定\(C\_{t-1}=j, C\_{t-2}=k\)条件下，
\(C\_t\)的条件分布等于给定\(C\_{t-1}=j, C\_{t-2}=k, C\_{t-3}=j\_{t-3}, \dots\)下的条件分布，
则称\(\{C\_t \}\)为二阶马氏链，
也可以有时齐和平稳的概念。

类似于AR模型的马氏扩张，
令\(\boldsymbol\alpha\_t = (C\_{t-1}, C\_t)^T\)，
则\(\boldsymbol\alpha\_t\)构成马氏链。

若二阶马氏链\(C\_t\)时齐，
则在给定\((C\_1, C\_2)\)初始分布后，
整个链的有限维分布由如下转移概率决定：
\[
\gamma(i,j,k) = P(C\_t = i | C\_{t-1}=j, C\_{t-2}=k) .
\]
若初始分布为平稳分布，
记为\(u(j,k)\)，
则
\[
u(j,k)
= \sum\_{i=1}^m u(i,j) \gamma(i,j,k),
\quad
\sum\_{j=1}^m \sum\_{k=1}^m \gamma(i,j,k) = 1 .
\]

如果\(m=2\)，
则\((j,k)\)组合仅有4种，
即\((1,1)\), \((1,2)\), \((2,1)\), \((2,2)\)，
仅需要指定4个转移概率即可。
而且\((C\_{t-2}, C\_{t-1})\)在\((1,1)\)或\((2,1)\)状态，
因为其中\(C\_{t-1}=1\)，
所以\((C\_{t-1}, C\_t)\)只能转移到\((1,1)\)或\((1,2)\);
若\((C\_{t-2}, C\_{t-1})\)在\((1,2)\)或\((2,2)\)状态，
则\((C\_{t-1}, C\_t)\)只能转移到\((2,1)\)或\((2,2)\)。
因此\(m=2\)时的二阶马氏链，
扩张为一阶马氏链，
其状态转移矩阵为
\[
\begin{pmatrix}
& | & (1,1) & (1,2) & (2,1) & (2,2) \\
\hline
(1,1) & | & 1-a & a & 0 & 0 \\
(1,2) & | & 0 & 0 & 1-c & c \\
(2,1) & | & 1-d & d & 0 & 0 \\
(2,2) & | & 0 & 0 & 1-b & b
\end{pmatrix} .
\]

以\(\ell\)阶的马氏链为状态的HMM需要有\(m^\ell (m-1)\)个参数，
参数个数增长过快。
Raftery提供了一种类似于样条基表示的高阶转移概率表达式，
公式如
\[
P(C\_t = j\_0 | C\_{t-1} = j\_1, \dots, C\_{t-\ell} = j\_\ell)
= \sum\_{i=1}^\ell \lambda\_i q(j\_i, j\_0) .
\]
这样，
阶数\(\ell\)增加一个，
参数个数也仅增加一个。

#### 34.10.2.2 二阶的隐马氏过程

设潜在的状态过程\(\{ C\_t \}\)服从时齐、平稳的二阶马氏链。
这种模型的似然函数可以用\(O(m^3 T)\)计算量计算。
当\(m\)较大时，
为了避免过多的未知参数参数可以假定转移概率矩阵有较简单的参数化形式。

#### 34.10.2.3 其它的相依性类型

基本的HMM模型，
观测\(X\_t\)的分布在\(C\_t\)给定后完全确定。

可以令\(X\_t\)的分布在给定\(C\_t\)和\(X\_{t-1}\)后给定，
这实际是一个变系数的AR(1)模型，
模型的系数依\(C\_t\)的状态不同而不同。
也可以定义变系数的AR(2)模型。
对变系数AR(1)模型，
可以写出假定\(x\_1\)已知的似然函数，
然后计算最大似然估计。
这样的模型一般称为马氏机制转换模型(Markov-switching model)。

还可以令\(X\_t\)依赖于\(C\_t\)和\(C\_{t-1}\)。
这比二阶马氏链方便一些，
\(C\_t\)本身仍是一阶马氏链。

## 34.11 连续状态的HMM

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.155第11章。

### 34.11.1 介绍

状态个数\(m\)有时很难客观选择，
当\(m\)很大时未知参数个数会过多。
所以有时连续状态的隐马氏模型可能更有优势。
这就与状态空间模型很接近了。

例如，
对地震数据，
可以假定大地震发生频率是随时间连续渐变的，
就可以用如下模型：
\[\begin{aligned}
X\_t \sim& \text{Poisson}(\beta e^{C\_t}), \\
C\_t =& \phi C\_{t-1} + \sigma\eta\_t .
\end{aligned}\]
其中\(|\phi|<1\)，
\(\sigma>0\), \(\beta>0\)，
\(\eta\_t\)独立同标准正态分布。
这其实就是一个非线性、非高斯状态空间模型。
\(C\_t\)服从零均值高斯AR(1)模型，
因为均值为0，
所以\(X\_t\)服从的泊松分布速率参数在\(\beta\)上下波动。
参数\(\phi\)控制速率参数的均值回归速率，
\(\sigma\)控制波动的范围。
这个模型的AIC值优于三状态泊松HMM模型。

非线性、非高斯状态空间模型(SSM)本质上与HMM是相同的，
其状态转移方程也是马氏的，
但一般是连续状态的。
其观测方程也是给定状态后服从某个条件分布。
非线性、非高斯SSM的似然函数计算涉及到多重积分，
很难计算；
通过将这样的问题离散化，
可以借助于HMM的工具近似解决SMM的估计问题。

### 34.11.2 模型

连续状态的HMM一般框架为
\[\begin{aligned}
p(c\_t | \boldsymbol c^{(t-1)})
=& p(c\_t | c\_{t-1}),
\ t=2,3,\dots\\
p(x\_t | \boldsymbol x^{(t-1)}, \boldsymbol c^{(t)})
=& p(x\_t | c\_t),
\ t=1,2,\dots
\end{aligned}\]

可以将状态变量的取值空间用充分大状态个数离散化，
然后用HMM的向前概率计算方法近似计算似然函数。

### 34.11.3 似然函数用HMM逼近计算

对一个SMM，
设状态\(C\_t\)的可取值范围为\([b\_0, b\_m]\)。
将其分为\(m\)个小区间\(B\_i = [b\_{i-1}, b\_i]\),
\(i=1,2,\dots, m\)。
小区间不必等宽，
为简单起见设等宽，
宽度为\(h=(b\_m-b\_0)/m\)。
设\(b\_i^\*\)为\(B\_i\)中的代表点，
比如取为区间中点，
用\(\int\_a^b f(x) \,dx = f(x^\*)(b-a)\)这样的近似，
可以近似计算似然函数如下：
\[\begin{aligned}
L\_T
=& \int\cdots\int
p(x\_1, \dots, x\_T, c\_1, \dots, c\_T)
\,dc\_T \dots, dc\_1 \\
=& \int\cdots\int
p(x\_1, \dots, x\_T | c\_1, \dots, c\_T)
p(c\_1, \dots, c\_T)
\,dc\_T \dots, dc\_1 \\
=& \int\cdots\int
p(c\_1) p(x\_1 | c\_1)
\prod\_{t=2}^T [p(c\_t | c\_{t-1}) p(x\_t | c\_t)]
\,dc\_T \dots, dc\_1 \\
\approx& \int\_{b\_0}^{b\_m} \cdots\int\_{b\_0}^{b\_m}
p(c\_1) p(x\_1 | c\_1)
\prod\_{t=2}^T [p(c\_t | c\_{t-1}) p(x\_t | c\_t)]
\,dc\_T \dots, dc\_1 \\
\approx& h^T \sum\_{i\_1=1}^m \cdots \sum\_{i\_T=1}^m
p(b\_{i\_1}^\*) p(x\_1 | b\_{i\_1}^\*)
\prod\_{t=2}^T [p(b\_{i\_{t}}^\* | b\_{i\_{t-1}}^\*)
p(x\_t | b\_{i\_{t}}^\*)] .
\end{aligned}\]
其中最内层的积分近似计算为
\[
\int\_{b\_0}^{b\_m}
p(c\_T | c\_{T-1})
p(x\_T | c\_T) \,dc\_T
= \sum\_{i\_T=1}^m
p(b\_{i\_{T}}^\* | c\_{T-1})
p(x\_T | b\_{i\_{T}}^\*) ,
\]
然后逐层近似，
得到\(T\)重求和的积分近似公式。
因为求和项有\(m^T\)个，
所以上述公式不能直接计算，
但可以看出这样的近似本质上是将状态的取值空间离散化了。

将状态空间的\(m\)个小区间看做是\(m\)个离散状态，
建立HMM。
考虑初始分布\(\boldsymbol\delta\)，
易见\(\delta\_i = h p\_{C\_1}(b\_i^\*)\)。
对转移概率\(\gamma\_{ij}\)，
只要计算\(\gamma\_{ij} = h P\_{C\_t|C\_{t-1}}(b\_j^\* | C\_{t-1} = b\_i^\*)\)。
观测值条件概率为\(p\_{X\_t|C\_t}(x\_t | c\_t=b\_j^\*)\)，\(j=1,\dots,m\)。
于是
\[
L\_T
= \boldsymbol\delta^T P(x\_1)
\Gamma P(x\_2)
\cdots
\Gamma P(x\_T)
\boldsymbol 1 .
\]
这可以递推计算，
要注意防止数值溢出。
计算量为\(O(m^2 T)\)阶。
近似计算得到的\(\boldsymbol\delta\)不一定严格求和等于1，
\(\Gamma\)的行和不一定严格等于1，
这都可以进行归一化。

用HMM近似计算SMM的似然函数，
即使\(T\)很大，\(m\)足够大，
经常也能达到足够精度。
\(m=50\)常常就足够用。
注意模型中未知参数个数并不依赖于\(m\)的值。
同样地，
也要注意数值溢出、参数约束、局部极值等问题。
用来近似的HMM也可以帮助对SMM进行预测、解码、诊断分析。

### 34.11.4 地震数据用SMM建模

考虑前面的发生率对数值为AR(1)的SSM。
取\([b\_0, b\_m] = [-1.5, 1.5]\)，
取\(m=200\)进行离散化。
用HMM的向前概率算法近似计算似然函数，
普通笔记本电脑一秒内可以完成最大似然估计计算。
如果取的状态空间区间不要太小也不要太大，
\(m=40\)以上基本上就不再改进精度。
AIC结果优于3状态泊松HMM模型。

为什么不直接使用\(m\)值很大的HMM？
如果没有基础的SMM模型，
无法精简参数个数，
未知参数个数会过多。

用HMM近似SMM的方法主要适用于状态空间一维的情形，
一旦状态空间为多维，
近似计算所需的\(m\)个数就指数级增长，
方法不再适用。

如果状态是离散的，
但个数非常多，
可以用这里的类似方法，
将状态合并到较少的组数。

## 34.12 半隐马氏模型

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.165第12章。

### 34.12.1 介绍

状态变量用一阶马氏链表示有时是不够准确的。
改用高阶马氏链会增大参数个数。
另一种推广是状态过程取为半马氏链。

注意对时齐马氏链，
一旦进入状态\(i\)后，
状态停留在\(i\)的步数的概率为几何分布模型：
\[
d\_i(r)
= (1-\gamma\_{ii}) \gamma\_{ii}^{r-1},
\ r=1,2,\dots
\]
概率的最大值出现在\(r=1\)，
即停留步数的最大可能取值是1，
即下一步就离开。
半马氏链就取消了这样的限制。

### 34.12.2 半马氏链和隐半马氏链

设\(Y\_t\)是状态空间为\(\{1, \dots, m\}\)的时齐马氏链，
其状态转移矩阵\(\Omega\)对角线元素等于0。
这样在状态序列中相邻两个时间点状态必然不同。
设\(d\_i\)是一个正整数集上的概率分布，
对\(i=1,2,\dots,m\)有\(m\)个这样的分布，
称为停留时间分布。
从\(Y\_t\)与\(\{d\_i\}\)构造过程\(\{C\_t \}\)如下。
每个\(Y\_t\)值代表连续的若干个状态不变的\(C\_s\)的值，
而连续不变的个数，
当\(Y\_t=i\)时服从停留时间分布\(d\_i\)。

例如，
\(m=3\)，
\(\{Y\_t \} = (1,2,1,3,2,\dots)\),
\(\{C\_t \}\)的一条轨道可能为
\[
1, 1, 1, | 2 | 1, 1, 1, 1 | 3, 3 | 2,2,2,2,2 | \dots
\]
其中\(1,1,1\)和\(1,1,1,1\)的重复个数来自停留时间分布\(d\_1\)，
\(2\)和\(2,2,2,2,2\)重复个数来自停留时间分布\(d\_2\)，
\(3,3\)的重复个数来自停留时间分布\(d\_3\)。

这样得到的\(\{C\_t \}\)一般不是马氏链，
称为半马氏链(SMC)。
若\(\{C\_t \}\)所有\(d\_i\)都是几何分布，
则仍是马氏链。

将隐马氏模型中的状态\(C\_t\)替换成半马氏链，
就称为隐半马氏模型（HSMM）。
隐半马氏模型计算比HMM要复杂得多。
引入协变量也很困难。

可以通过扩充HMM的状态空间用HMM近似任意的HSMM。
详细内容待完成。

## 34.13 纵向数据的HMM

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.187第13章。

### 34.13.1 介绍

设有\(K\)个个体，
每个个体持续观测了\(T\)个时间点，
观测为\(\{x\_{tk}, t=1,\dots,T, i=1,\dots,K\}\)。
这称为纵向数据，
经济学中称为面板(panel)数据。
要注意到同一个个体的多次观测之间有相关性。
设每个个体的时间序列使用相同模型，
但参数可以不同。

某些情况下可以假定\(K\)个序列都依赖于一个共同的潜在状态序列\(C\_t\)，
给定状态序列后各个序列之间条件独立，
则可以考虑HMM。
一个例子是考虑多只股票的收益率，
设其共同受到同一个潜在市场状态的影响。
这可以看成是一个观测值为多维的HMM。

有些情况下不能认为各个序列有共同的状态序列，
比如，
不同病人在不同时间点上的多个测量值。
如果能假定各个序列、以及相应状态独立，
则似然函数为各个序列的似然函数的乘积。
如果假定各个观测序列模型中的一部分参数是相同的，
就可以利用所有观测序列的数据共同来估计模型，
可以增加估计精度，
在数据长度不足时这种联合起来的做法能够对单个建模无法估计的模型进行建模。

可以用协变量值区分个体之间的变化。

向线性混合模型那样，
也可以假定随个体变化的某些参数是随机变量，
这样也可以减少要估计的参数个数。

作为简单示例，
设\(\{x\_{tk}: t=1,\dots,T, k=1,\dots, K\}\)是计数数据，
希望对每个个体的序列拟合泊松HMM。
为了简化，
设长度都等于\(T\),
\(\lambda\_j^{(k)}\)代表第\(k\)个体的条件泊松分布参数，
\(\gamma\_{ij}^{(k)}\)代表第\(k\)个体的状态转移概率。

### 34.13.2 假定某些参数在个体之间相同的模型

内容待补充。

### 34.13.3 有随机效应的模型

内容待补充。

## 34.14 应用案例

### 34.14.1 癫痫病人每日发作次数的数据

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.201第15章。
一个癫痫病人204天，
每天发作次数的时间序列。
其病情可以用马氏过程建模，
发作次数依赖于病情。

使用泊松HMM，
并假定马氏链平稳，
考虑\(m=1,2,3,4\)的模型。
选择了\(m=2\)模型。
计算了正态伪残差区间，
计算了基于前100点的预测正态伪残差区间。

### 34.14.2 黄石公园“老忠实”喷泉数据

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.213第17章。

将喷发时间分为短和长两类，
用伯努利HMM建模。
这个序列明显是相关的，
短绝大多数会变成长，
长则有可能持续。

因为状态简单，
所以可以考虑直接用马氏链建模，
而不是隐马氏链。
研究发现一阶马氏链不能良好拟合ACF，
需要用二阶马氏链（类似于AR(2)）。

拟合隐马氏模型。
\(m=2\)的估计结果
\[
\Gamma
= \begin{pmatrix}
0 & 1 \\
0.827 & 0.173
\end{pmatrix},
\]
其中隐状态1代表短喷发，
2代表长喷发。
在状态1下发生长喷发的概率估计为\(0.225\)，
在状态2下发生长喷发的概率估计为\(1\)。
这个模型有些像是状态空间模型中的局部水平模型，
局部水平是马氏的，
但观测值增加了观测噪声。
这个隐马氏模型在状态1下有一定小概率发生长喷发，
否则观测值序列本身就是马氏链。
隐马氏模型对转移频率和ACF拟合较好。

对实际喷发时间，
可以拟合正态HMM。
对实际喷发和间隔时间的二元时间序列，
可以拟合二元正态HMM。

### 34.14.3 动物跟踪轨迹数据

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.227第18章。

感兴趣的一类问题是将动物的移动分类，
这比较适合HMM。
也有人用状态空间模型，
这适合记录的位置有较大误差的情形。

#### 34.14.3.1 方向数据

方向数据，
比如\(5\)度和\(355\)度，不能直接进行算数运算，
求这两个度数的平均，应该在\(0\)度，而不是\(180\)度。
可以先将度数映射到单位圆上，
计算完毕后再计算度数。
R语言的`atan2(y,x)`函数可以从坐标\((x,y)\)得到\((-\pi, \pi]\)内取值的极角弧度值。

这样，
如果\(X\)是一个\((-\pi, \pi]\)内取值的随机角度，
为了求\(E(X)\)，
注意\(X\)在单位圆上的表示为\((\cos X, \sin X)\)，
应求\((E\cos X, E\sin X)\)，
然后转换回弧度值为\(\text{atan2}(E\sin X, E\cos X)\)。

也可以定义复值随机变量，
这时其期望的辐角即随机角度的期望值。

#### 34.14.3.2 von Mises分布

von Mises分布比较适合作为单位圆周上单峰分布的模型。
密度函数为
\[
f(x) = c \exp\{ \kappa \cos(x - \mu) \},
\ x \in (-\pi, \pi] .
\]
参数\(\mu\)为均值（方向角度的均值，见前面所述），
\(\kappa > 0\)代表集中程度，
\(\kappa\)越大分布越集中。

如果HMM的观测值是方向角度，
可以采用von Mises分布作为条件分布。

#### 34.14.3.3 动物轨迹数据建模

设数据是以恒等时间间隔将GPS坐标无线传送并记录。
如果要对动物运动状态分类，
HMM一般仅能给出比较粗略的近似。

可以使用的数据有：

* 位置的x, y坐标；
* 两次记录之间位移的x, y值；
* 移动距离（称为步长）；
* 两次位移之间的角度变化值（称为转向角）。

考虑由步长和转向角组成的二元数据。
给定初始位置和初始运动方向，
这种数据能恢复整个运动轨迹。

#### 34.14.3.4 多状态随机游动

从运动的角度看，
可以用有相关、有偏向的随机游动。
有相关比如转向角以\(0\)为均值。
偏向主要指运动方向的偏向，
可以是偏向于某一个方向（比如南方），
也可以是偏向于某个位置的方向。
比如，
设运动方向偏向于位置坐标\((x\_0, y\_0)\)的方向，
则第\(t+1\)步运动的方向就是向量\((x\_0-x\_t, y\_0-y\_t)\)的方向。
\((x\_0, y\_0)\)可以作为参数进行估计。

如果同时有相关（持续向某个方向运动）也有偏向（比如被吸引向某个固定位置运动），
则需要考虑这两种倾向的折衷。
可以计算两个角度的角度加权平均。
若\(\phi\_p\)代表原来的运动方向，
\(\phi\_b\)是收到某个位置吸引的运动方向，
则动物实际的运动方向可能是
\[
\phi = \text{atan2}(
\eta\sin\phi\_p + (1-\eta)\sin\phi\_b,
\eta\cos\phi\_p + (1-\eta)\cos\phi\_b),
\]
其中\(\eta\)是一个权重系数。

HMM就可以用于表示动物运动处于不同的随机游动模式的情况，
状态\(C\_t\)的值决定了动物运动是处于持续某一方向，
收到某一位置吸引，
还是两者都有的状态。
为简单起见经常假设同期独立性，
即步长和转向角两个分量是条件独立的。

对一组果蝇运动数据拟合了HMM，
选择了\(m=3\)状态，
给定状态时步长为伽马分布，
转向角为von Mises分布。
最大似然估计很容易被局部极值吸引，
难以得到全局最大值。

### 34.14.4 风向数据

见([Zucchini, MacDonald, and Langrock 2016](#ref-Zucchini2016:HMM-TS-R)) P.245第19章。
南非Koeberg的每小时风向数据，
每小时的风向是前一小时的风向的角度平均值。
共有4年数据，35064个观测，没有缺失值。

#### 34.14.4.1 三个角度类别的HMM模型

将风向角度分为16个方向，
作为分类数据建模。

第一个模型：\(m=2\)个状态，
\(q=16\)个观测值类别，
参数个数\(2 + 15\times 2 = 32\)。

第二个模型是\(m=3\)个状态。
有\(3\times 2 + 15\times 3 = 51\)个未知参数。

第三个模型有\(m=2\)个状态，
加入了以年为周期的变化，
以及以24小时为周期的变化。
周期性协变量加入到了马氏链的模型中，
设两个转移概率\(P(C\_t \neq i | C\_{t-1}=i)\)是\(t\)的函数，
\[\begin{aligned}
& \log P(C\_t \neq i | C\_{t-1}=i) \\
=& a\_i
+ b\_i \cos(\frac{2\pi}{24} t)
+ c\_i \sin(\frac{2\pi}{24} t)
+ d\_i \cos(\frac{2\pi}{8766} t)
+ e\_i \sin(\frac{2\pi}{8766} t) .
\end{aligned}\]
其中\(8766=365.25\times 24\)是一年的小时数。
此马氏链非时齐、非平稳。
可以假定初始状态为状态1，
然后进行最大似然估计，
再比较初始状态为状态2时的似然函数值。
模型三与模型一得到的条件分布参数很接近。

#### 34.14.4.2 模型比较与其它模型

将16个风向本身作为马氏链建模，
与前面三个模型比较，
发现AIC、BIC最优的是直接用马氏链建模。

用Raftery简化参数化的二阶马氏链建模，
结果优于一阶马氏链模型。

#### 34.14.4.3 直接以风向角度为观测值

将风向分为16个类别，
不能直接反映出这些类别的邻近关系。
用von Mises分布作为给定\(C\_t=j\)条件下风向角度的条件分布。
考虑状态个数\(m=1,2,3,4\)的不同模型。
取每天的最后一个小时的子列作为观测值。
如果数据是16个类别的，
也可以当作von Mises分布区间删失值。

注意在计算似然函数时，
对于离散分布，
似然函数本身是概率值；
对于连续型分布，
使用密度的乘积作为似然函数仅是惯例，
因为其结果不是概率，
实际上应该是观测值属于记录精度决定的区间的概率。

从AIC比较，
von Mises-HMM比直接建立一阶马氏链都占优。

从数据来看，
风向随时间的变化具有较强的缓慢变化特性，
所以计算风向的变换（差分）作为观测数据可能更合理。

设\(X\_t\)是风向角度的改变值，
条件分布为von Mises分布。
隐马氏链以上一小时的风速\(S\_{t-1}\)为协变量，
设\(P(C\_t = j | C\_{t-1}=i, S\_{t-1}=s)\)可以有适当的参数化表示。

还可以假定\(X\_t\)条件分布的von Mises分布的集中程度参数\(\kappa\)受到协变量\(S\_{t-1}\)影响。

### B 参考文献

Zucchini, Walter, Iain L. MacDonald, and Roland Langrock. 2016. *Hidden Markov Models for Time Series - an Introduction Using r*. 2nd ed.