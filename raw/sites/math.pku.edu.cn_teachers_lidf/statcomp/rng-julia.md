---
crawl_time: '2026-01-17 14:25:39'
framework: sphinx
title: 9 Julia语言中的随机数发生器与分布类介绍(*) | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/rng-julia.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 9 Julia语言中的随机数发生器与分布类介绍(`*`)

## 9.1 与分布有关的函数

Julia语言的Base部分提供了`rand`函数和`randn`函数。
`rand(n)`返回n个标准均匀分布U(0,1)随机数，
类型是Float64的一维数组，
无自变量的`rand()`调用返回一个U(0,1)随机数。
`randn(n)`和`randn()`产生标准正态分布的随机数。

加载[`Distributions`](https://juliastats.github.io/Distributions.jl/latest/)包后可以生成其它分布的随机数，
为Base的`rand`函数增加了方法，
还提供了各种与分布的有关函数。

产生随机数的函数调用格式为
`rand(distribution, n)`或`rand(distribution)`，
其中`distribution`是一个分布，用函数调用表示，
比如标准正态分布用`Normal()`表示，
\(\text{N}(10, 2^2)\)用`Normal(10,2)`表示。
用`Random.seed!(k)`设定一个随机数种子序号，
这样可以在模拟时获得可重复的结果。
比如，生成100个Possion(2)分布随机数:

```
using Random, Distributions
Random.seed!(101)
y = rand(Poisson(2), 100)
```

结果是Int64型的一维数组。作其直方图：

```
using CairoMakie
CairoMakie.activate!()
using AlgebraOfGraphics
plt = data((; y = y)) * mapping(:y) * histogram()
draw(plt, axis=(; title="Random sample from Poisson(2)"))
```

Distributions包不仅包含了R软件中提供的密度函数、对数密度函数、分布函数、分位数函数、随机数函数等与分布有关的函数，
还可以输出每个参数分布的各种矩的理论值，
计算矩母函数和特征函数，
给定观测样本后计算参数的最大似然估计，
用共轭先验计算后验分布，对参数作最大后验密度估计等。
分布类型包括一元随机变量(Univariate)、随机向量(Multivariate)、随机矩阵(Matrixvariate)，
又可分为离散型(Discrete)和连续型(Continuous)。
分布类型的大类名称包括：

* DiscreteUnivariateDistribution
* ContinuousUnivariateDistribution
* DiscreteMultivariateDistribution
* ContinuousMultivariateDistribution
* DiscreteMatrixDistribution
* ContinuousMatrixDistribution

Distributions包中定义了各种概率分布。
例如，
`Binomial(n, p)`表示二项分布，
`Normal(μ, σ)`表示正态分布分布，
`Multinomial(n, p)`表示多项分布，
`Wishart(nu, S)`表示Wishart分布。

可以从一元分布生成截断分布（truncated），
如`Truncated(Normal(mu, sigma), l, u)`。

用`params(distribution)`返回一个分布的参数，如

```
params(Binomial(10, 0.2))
```

`rand(distribution, n)`生成`n`个指定分布的随机数，
对一元分布，结果是一维数组，
连续型分布的元素类型是Float64,
离散型分布的元素类型是Int。
对多元分布，
结果是矩阵，**每列**为一个抽样。
对随机矩阵分布，
结果是元素为矩阵的数组。

有一些函数用来返回分布的特征量，
对一元分布，有：

* `maximum()` 分布的定义域上界
* `minimum()` 分布的定义域下界
* `mean()` 期望值
* `var()` 方差
* `std()` 标准差,
* `median()` 中位数
* `modes()` 众数（可有多个）
* `mode()` 第一个众数
* `skewness()` 偏度
* `kurtosis()` 峰度，正态为0
* `entropy()` 分布的熵

用`mgf(distribution, t)`计算矩母函数在`t`处的值，
用`cf(distribution, t)`计算特征函数在`t`处的值。

用`pdf(distribution, x)`计算分布密度（概率质量）函数在`x`处的值，
如果`x`是数组，结果返回数组；
如果`x`是数组且函数写成`pdf!(distribution, x)`，则结果保存在`x`中返回。
`logpdf()`计算其密度函数的自然对数值。

用`cdf(distribution, x)`计算分布函数在`x`处的值，
`logcdf`计算其对数值，
`ccdf`计算1减分布函数值，
`logccdf`计算其对数值。

用`quantile(distribution, p)`计算分位数函数在`p`处的值。

类似于`pdf`，这些函数一般支持向量化，
输入`x`为向量时返回向量，
写成以叹号结尾的函数名时，
输入为向量则结果保存在输入向量的存储中返回。
可这样向量化的函数包括
`pdf`, `logpdf`, `cdf`, `logcdf`, `ccdf`, `logccdf`, `quantile`, `cquantile`, `invlogcdf`, `invlogccdf`。

比如，计算\(\text{N}(10,2^2)\)的0.5和0.975分位数：

```
quantile(Normal(10, 2), [0.5, 0.975])
##  2-element Array{Float64,1}:
##   10.0   
##   13.9199

10 + 2*quantile(Normal(), [0.5, 0.975])
##  2-element Array{Float64,1}:
##   10.0   
##   13.9199
```

比如，计算标准正态分布密度并作曲线图：

```
x = range(-3.0, 3.0; length=100)
y = pdf.(Normal(), x)
plt = data((; x=x, y=y)) * 
    mapping(:x, :y) * visual(Lines)
draw(plt)
```

用`rand(distrbution)`生成指定分布的一个抽样，
用`rand(distrbution, n)`生成指定分布的`n`个抽样，
用`rand!(distribution, x)`生成指定分布的多个抽样保存在数组`x`中返回。

用`fit(distribution, x)`作最大似然估计，
比如模拟生成\(\text{N}(10,2^2)\)样本，然后做最大似然估计：

```
Random.seed!(101)
x=rand(Normal(10, 2), 30)
fit(Normal, x)
## Distributions.Normal{Float64}(
##   μ=10.349862945450266, σ=2.4124946578030015)
```

## 9.2 支持的分布

这里列举出Julia的Distributions包支持的分布。

### 9.2.1 连续型一元分布

* `Arcsine(a,b)`
* `Beta(α,β)`
* `BetaPrime(α,β)`
* `Biweight(μ, σ)`
* `Chi(ν)`
* `Chisq(ν)`
* `Cosine(μ, σ)`
* `Epanechnikov(μ, σ)`
* `Erlang(α,θ)`
* `Exponential(θ)`
* `FDist(ν1, ν2)`
* `Frechet(α,θ)`
* `Gamma(α,θ)`
* `GeneralizedExtremeValue(μ, σ, ξ)`
* `GeneralizedPareto(μ, σ, ξ)`
* `Gumbel(μ, θ)`
* `InverseGamma(α, θ)`
* `InverseGaussian(μ,λ)`
* `Kolmogorov()`
* `KSDist(n)`
* `KSOneSided(n)`
* `Laplace(μ,θ)`
* `Levy(μ, σ)`
* `Logistic(μ,θ)`
* `LogNormal(μ,σ)`
* `NoncentralBeta(α, β, λ)`
* `NoncentralChisq(ν, λ)`
* `NoncentralF(ν1, ν2, λ)`
* `NoncentralT(ν, λ)`
* `Normal(μ,σ)`
* `NormalCanon(η, λ)`
* `NormalInverseGaussian(μ,α,β,δ)`
* `Pareto(α,θ)`
* `Rayleigh(σ)`
* `SymTriangularDist(μ,σ)`
* `TDist(ν)`
* `TriangularDist(a,b,c)`
* `Triweight(μ, σ)`
* `Uniform(a,b)`
* `VonMises(μ, κ)`
* `Weibull(α,θ)`

### 9.2.2 离散型一元分布

* `Bernoulli(p)`
* `BetaBinomial(n,α,β)`
* `Binomial(n,p)`
* `Categorical(p)`
* `DiscreteUniform(a,b)`
* `Geometric(p)`
* `Hypergeometric(s, f, n)`
* `NegativeBinomial(r,p)`
* `Poisson(λ)`
* `PoissonBinomial(p)`
* `Skellam(μ1, μ2)`

### 9.2.3 截断分布

设`distribution`为一个一元分布，
用

```
Truncated(distribution, l, u)
```

可以返回一个截断分布，其中`l`为下界，
`u`为上界。

特别地，
`TruncatedNormal(mu, sigma, l, u)`表示截断正态分布。

### 9.2.4 多元分布

* `Multinomial`
* `MvNormal`
* `MvNormalCanon`
* `MvLogNormal`
* `Dirichlet`

### 9.2.5 随机矩阵分布

* `Wishart(nu, S)`
* `InverseWishart(nu, P)`

### 9.2.6 混合分布

通过`MixtureModel()`函数构造。