---
crawl_time: '2026-01-17 14:20:07'
framework: rbook
title: 56 Rcpp糖 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp-sugar.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 56 Rcpp糖

在C++中，向量和矩阵的运算通常需要逐个元素进行，
或者调用相应的函数。
Rcpp通过C++的表达式模板(expression template)功能和懒惰求值(lazy evaluation)功能，
可以在C++中写出像R中对向量和矩阵运算那样的向量化表达式。
这称为Rcpp糖(sugar)。

R中的很多函数如`sin`等是向量化的，
Rcpp糖也提供了这样的功能。
Rcpp糖提供了一些向量化的函数如`ifelse`, `sapply`等。

比如，两个向量相加可以直接写成`x + y`
而不是用循环或迭代器(iterator)逐元素计算;
若`x`是一个NumericVector,
用`sin(x)`可以返回由`x`每个元素的正弦值组成的NumericVector。

Rcpp糖不仅简化了程序， 还提高了运行效率。

## 56.1 简单示例

比如，函数
\[\begin{aligned}
f(x,y) = \begin{cases}
x^2 & x < y, \\
-y^2 & x \geq y
\end{cases}
\end{aligned}\]

如下的程序可以在C++中定义一个向量化的版本：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f11(NumericVector x, NumericVector y){
  return ifelse(x < y, x*x, -(y*y));
}
')
f11(c(1, 3), c(4,2))
## [1]  1 -4
```

上面简单例子中，`x < y`是向量化的，
`x * x`, `y * y`, `-(y * y)`都是向量化的。
`ifelse`也是向量化的。

## 56.2 向量化的运算符

### 56.2.1 向量化的四则运算

Rcpp糖向量化了`+, -, *, /`。
设`x, y`都是NumericVector, 则
`x + y`, `x - y`, `x * y`, `x * y`
将返回对应元素进行四则运算后的NumericVector变量。

向量与标量运算，
如 `x + 2.0`, `2.0 - x`, `x * 2.0`, `2.0 / x`
将返回标量与向量每个元素进行四则运算后的NumericVector变量。

还可以进行混合四则运算，如：

```
NumericVector res = x * y + y / 2.0;
NumericVector res = x * (y - 2.0);
NumericVector res = x / (y * y);
```

参加四则运算的或者都是同基本类型的向量而且长度相等，
或者一边是向量，一边是同类型的标量。

注意：对向量整体赋一个相同的值， 不能简单地写成如`x=0;`这样的赋值，
需要用循环或STL的fill算法， 如`std::fill(x.begin(), x.end(), 0);`。

### 56.2.2 向量化的赋值运算

可以将糖表达式的值赋值给一个已分配存储空间的等长向量，
如：

```
NumericVector x = NumericVector::create(1.0, 2.0);
NumericVector y(2);
y = x / 2.0;
```

### 56.2.3 向量化的二元逻辑运算

Rcpp糖扩展了两个元素的比较到两个等长向量的比较，
以及一个向量与一个同类型标量的比较，
结果是一个同长度的LogicalVector。
比较运算符包括`<, >, <=, >=, ==, !=`。

也可以使用嵌套的表达式， 比如，设x, y是两个NumericVector，可以写:

```
LogicalVector res = (x + y) < (x * x);
```

### 56.2.4 向量化的一元运算符

对数值型向量或返回数值型向量的表达式前面加负号，
可以返回每个元素取相反数的结果。
比如，设x是NumericVector, 可以写:

```
NumericVector res = -x;
NumericVector res = -x * (x + 2.0);
```

对逻辑型向量或返回逻辑型向量的表达式前面加叹号，
可以返回每个元素取反的结果，如：

```
NumericVector y, z;
NumericVector res = ! ( y < z );
```

## 56.3 用Rcpp访问数学函数

在C和C++代码中可以调用R中的函数，
但是格式比较复杂，
而且原来不支持向量化。
Rcpp糖则使得从C++中调用R函数变得和在R调用函数格式类似。

R源程序中提供了许多数学和统计相关的函数，
这些函数可以编译成一个独立的函数库，
供其它程序链接使用，
函数内容在R的Rmath.h头文件中有详细列表。

Rcpp糖把R中的数学函数在C++中向量化了，
输入一个向量，
结果是对应元素为函数值的向量。
自变量类型可以是数值型或整数型。
这些数学函数包括abs, exp, floor, ceil, pow等。
在Rcpp中支持的C++源程序中调用这些函数,
最简单的一种方法是直接在Rcpp名字空间中使用和R中相同的函数名和调用方法，
类似sqrt这样的函数允许输入一个向量，
对向量的每个元素计算。

## 56.4 用Rcpp访问统计分布类函数

R中提供了许多分布的分布密度（或概率质量函数）、分布函数、分位数函数，
分布密度函数和概率质量函数命名类似dxxxx, 分布函数命名类似pxxxx,
分位数函数命名类似qxxxx。
在Rcpp糖支持下，
这些函数可以直接用类似R中的格式调用。
比如，下面的程序输入一个向量，计算相应的标准正态分布函数值：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f10(NumericVector x){
  NumericVector y(x.size());
  y = pnorm(x);
  return y;
}
')
f10(c(0, 1.96, 2.58))
## [1] 0.5000000 0.9750021 0.9950600
```

如果需要对单个的数值计算，
可以使用Rmath.h中定义的带有`Rf_`的版本，
如:

```
y = ::Rf_pnorm5(x, 0.0, 1.0, 1, 0);
```

这里用`::`使用了缺省的名字空间。
注意所有自变量都不能省略。

Rcpp还提供了一个`R`名字空间，
可以用不带`Rf_`前缀的函数，
但是自变量也不能省略。如

```
y = R::pnorm(x, 0.0, 1.0, 1, 0);
```

在Rcpp的`R`名字空间中有许多的数学和统计相关的函数，
各函数的自变量参见Rcpp的Rmath.h文件。

更多分布类函数使用如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
List f14(){
  NumericVector x = 
    NumericVector::create(0, 1.96, 2.58);
  NumericVector p = 
    NumericVector::create(0.95, 0.975, 0.995);
  NumericVector y1 = dnorm(x, 0.0, 1.0);
  NumericVector y2 = pnorm(x, 0.0, 1.0);
  NumericVector y3 = qnorm(p, 0.0, 1.0);
  return List::create(Named("y1")=y1,
    Named("y2")=y2, Named("y3")=y3);
}
')
f14()
## $y1
## [1] 0.39894228 0.05844094 0.01430511
## 
## $y2
## [1] 0.5000000 0.9750021 0.9950600
## 
## $y3
## [1] 1.644854 1.959964 2.575829
##
```

C++中这些分布类函数的分布参数选择可能与R中有所不同，
比如R中`dexp()`的参数是`rate`，
而Rcpp中是`scale`。

## 56.5 在Rcpp中产生随机数

R中的rxxxx类的函数可以产生各种分布的随机数向量，
随机数向量与当前种子有关。
这些函数在Rcpp中也可以使用，
Rcpp属性会自动地维护随机数发生器的状态使其与R同步。
如

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f15(){
  NumericVector x = rnorm(10, 0.0, 1.0);
  return x;
}
')
round(f15(), 2)
## [1]  0.62 -0.06 -0.16 -1.47 -0.48  
## [6]  0.42  1.36 -0.10  0.39 -0.05
```

## 56.6 返回单一逻辑值的函数

在R中， `any()`和`all()`对一个逻辑向量分别判断是否有任何真值，
以及所有元素为真值。
Rcpp糖在C++中也提供了这样的`any()`和`all()`函数。
如

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
List f12(){
  IntegerVector x = seq_len(1000);
  LogicalVector res1 = any( x*x < 3 );
  LogicalVector res2 = all( x*x < 3 );
  return List::create(Named("any")=res1,
    Named("all")=res2);
}
')
f12()
## $any
## [1] TRUE
## 
## $all
## [1] FALSE
```

`any()`和`all()`不是直接返回逻辑值，
而是返回一个类对象，
该类定义了`is_true`, `is_false`, `is_na`方法与向SEXP转换的运算符。
此种结果可以保存到LogicalVector中， 但不能赋值到bool类型，
因为可能有缺失值。

逻辑型糖表达式结果可以用 `is_true`, `is_false`, `is_na`来判断结果。
之所以这样，
而不是将表达式结果直接转换为布尔型，
是因为R的这种结果可能是缺失值，
而布尔型并不包括缺失值。
如

```
bool res1 = is_true( any( x < y ) );
bool res2 = is_na( all( x < y ) );
```

在求`any()`结果时，一旦遇到一个真值结果就为真值，
即使后面有缺失值也没有关系。
在求`all()`结果时，一旦遇到一个假值结果就为假值，
即使后面有缺失值也没有关系。

## 56.7 返回糖表达式的函数

### 56.7.1 `is_na`

`is_na`以任何糖表达式为输入，
输出一个逻辑类型的元素个数相同的糖表达式。
结果每个元素当输入中对应元素缺失时为TRUE, 否则为FALSE。
如

```
IntegerVector x = IntegerVector::create(0, 1, NA_INTEGER, 3);
LogicalVector res1 is_na(x);
bool res2 = all( is_na(x) );
if( is_true( any( ! is_na(x) ) ) ) ...
```

### 56.7.2 `seq_along`

`seq_along`输入一个向量，
输出一个元素为该向量的各个下标值的糖表达式。
如

```
IntegerVector x = IntegerVector::create(0, 1, NA_INTEGER, 3);
IntegerVector ind1 = seq_along(x);
IntegerVector ind2 = seq_along(x*x*x*x*x);
```

注意上述程序不会计算`x*x*x*x*x`的值而只利用其结果的元素个数。
所以两次调用的计算量是一样的。

### 56.7.3 `seq_len`

`seq_len`自变量为个数，
返回一个元素值为正数的元素个数等于自变量值的糖表达式，
第\(i\)个元素等于\(i\)。
常可与`sapply`, `lapply`配合使用。
如

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
List f13(){
  IntegerVector x =seq_len(3);
  List y = lapply( seq_len(3),  seq_len);
  return List::create(Named("x")=x,
    Named("y")=y);
}
')
f13()
## $x
## [1] 1 2 3
## 
## $y
## $y[[1]]
## [1] 1
## 
## $y[[2]]
## [1] 1 2
## 
## $y[[3]]
## [1] 1 2 3
##
```

### 56.7.4 `pmin`和`pmax`

`pmin`和`pmax`
自变量为两个等长的向量或糖表达式，
返回长度相同的结果，
结果元素等于对应元素的最小值或最大值。

自变量也可以取一个向量一个标量，
向量的每个元素与标量比较得到最小值或最大值。
如

```
IntegerVector x = seq_len(10);
IntegerVector y = pmin(x, x*x);
IntegerVector z = pmin(x*x, 2);
```

### 56.7.5 `ifelse`

`ifelse`函数有三个自变量，
第一自变量是一个逻辑型向量值的糖表达式，
第二和第三自变量或者是两个同类型并与第一自变量等长的糖表达式，
或者其中一个是同类型标量，
结果仍为向量型糖表达式，
第\(i\)元素当第一自变量第\(i\)元素为真时取第一自变量的第\(i\)元素，
当第一自变量第\(i\)元素为假时取第二自变量的第\(i\)元素，
当第一自变量第\(i\)元素为缺失值时取缺失值。
如

```
IntegerVector x, y;
IntegerVector res1 = ifelse( x < y, x, (x+y)*y );
IntegerVector res2 = ifelse( x > y, x, 2 );
```

### 56.7.6 `sapply`和`lapply`

`sapply`函数第一自变量是一个向量或列表， 第二自变量是一个函数。
返回值类型在编译时从函数的结果类型导出。
第二自变量可以是任意的C++函数，
比如， 可以是如下的重载的模板化函数：

```
template <typename T>
T square( const T& x ){
  return x * x;
}
sapply( seq_len(4), square<int> );
```

下面的例子中`sapply`第二自变量使用了functor，
这是一种能够产生函数的函数：

```
template <typename T>
struct square : std::unaray_function<T,T> {
  T operator() (const T& x) {
    return x * x;
  }
}
sapply( seq_len(4), square<int>() );
```

`lapply`函数与`sapply`函数基本相同，
只不过`lapply`函数总是返回列表,
列表在Rcpp中为List或GenericVector, 在R API中类型为VECSXP。

### 56.7.7 `sign`

`sign`函数输入一个数值型或整型表达式，
返回各元素值在\(\{1, 0, -1, \text{NA} \}\)中取值的糖表达式，
可以保存到IntegerVector中。
结果各元素的取值表示输入中对应元素为正、零、负和缺失。
如

```
IntegerVector x;
IntegerVector res1 = sign( x );
IntegerVector res2 = sign( x*x );
```

### 56.7.8 `diff`

`diff`函数自变量为数值型或整数型向量或有这样结果的糖表达式，
输出后一元素减去前一元素的结果， 结果长度比输入长度少一。
如

```
IntegerVector res = diff( seq_len(5) );
```

## 56.8 R与Rcpp不同语法示例

RCpp糖通过一些现代的C++技术， 支持部分的向量化运算，
比如，NumericVector与标量相加， 两个等长NumericVector相加，
对NumericVector计算如绝对值这样的数学函数值等。
但是，C++毕竟和R有很大差别， 要区分Rcpp能做的和不能做的。

例如，NumericVector为了把向量所有元素都改成同一值，
不能直接用等于赋值。
可以用`std::fill(y.begin(), y.end(), 99.99)`这样的做法。

## 56.9 用RcppArmadillo执行矩阵运算

虽然Rcpp糖提供了许多与R类似的向量化功能，
但是Rcpp仍未支持像R那样用简单表达式表示矩阵运算。
RcppArmadillo包提供了与C++库Armadillo的接口，
Armadillo库利用C++的模板功能实现了矩阵运算表达式功能，
这样，
借助于Rcpp和RcppArmadillo，
可以将一些涉及到矩阵的算法完全用C++完成，
而Rcpp可以轻易地将R输入转换为C++类型，
将C++代码的输出转换为R类型。

本节例子来自Rcpp文档。

### 56.9.1 生成多元正态分布随机数

设随机向量\(\boldsymbol X\)服从多元正态分布\(\text{N}(\boldsymbol\mu, \Sigma)\)，
为了生成\(\boldsymbol X\)的随机样本，
可以从标准多元正态分布\(\boldsymbol Z \sim \text{N}(\boldsymbol 0, I)\)的样本进行线性变换得到\(\boldsymbol X\)的样本。
有两种方法，
一种是对\(\Sigma\)作Cholesky分解\(\Sigma = A A^T\)，
其中\(A\)是下三角阵，
然后令\(\boldsymbol X = \boldsymbol\mu + A \boldsymbol Z\)。
另一种方法是利用\(\Sigma\)的特征值分解。
Cholesky分解方法效率较高，
而特征值分解方法较稳健但效率较低。

在C++源程序中，
要引入`RcppArmadillo.h`头文件，
并且用`Rcpp::depends`说明链接时对`RcppArmadillo`库的依赖。
设C++源程序保存在`rmvnorm.cpp`文件中，
内容如下：

```
#include <RcppArmadillo.h>
// [[Rcpp::depends(RcppArmadillo)]]

// Sample N x P observations from a Standard
// Multivariate Normal given N observations, a
// vector of P means, and a P x P cov matrix

// [[Rcpp::export]]
arma::mat rmvnorm(
    int n,
    const arma::vec& mu,
    const arma::mat& Sigma) {
  unsigned int p = Sigma.n_cols;
  
  // First draw N x P values from a N(0,1)
  Rcpp::NumericVector draw = Rcpp::rnorm(n*p);
  
  // Instantiate an Armadillo matrix with the
  // drawn values using advanced constructor
  // to reuse allocated memory
  arma::mat Z = arma::mat(
    draw.begin(), n, p, false, true);
  // Simpler, less performant alternative
  // arma::mat Z = Rcpp::as<arma::mat>(draw);
  
  // Generate a sample from the Transformed
  // Multivariate Normal
  arma::mat Y = arma::repmat(mu, 1, n).t() +
    Z * arma::chol(Sigma);
  return Y;
}
```

注意C++源程序中仅引入了`RcppArmadillo.h`头文件，
该头文件会引入`Rcpp.h`头文件，
用户不能自己引用`Rcpp.h`头文件。

在R中测试：

```
library(Rcpp)
sourceCpp(file = "rmvnorm.cpp")
set.seed(101)
Y <- rmvnorm(50, c(100, 200),
  rbind(c(2,1), c(1,4)))
colMeans(Y)
## [1]  99.82468 200.00511
var(Y)
##          [,1]     [,2]
## [1,] 1.737795 1.061398
## [2,] 1.061398 3.701758
```

### 56.9.2 快速计算线性回归

R中的`lm()`函数利用了数值稳健方法进行计算，
效率较低，
多次重复调用也会影响速度。

这里用RcppArmadillo进行较简单的计算，
仅返回回归系数、误差项标准差和回归系数标准误差估计。
RcppArmadillo已经提供了`fastLm()`函数，
这里给出源程序：

```
#include <RcppArmadillo.h>
// [[Rcpp::depends(RcppArmadillo)]]

// Compute coefficients and their standard error
// during multiple linear regression given a
// design matrix X containing N observations with
// P regressors and a vector y containing of
// N responses

// [[Rcpp::export]]
Rcpp::List fastLm(
    const arma::mat& X,
    const arma::colvec& y) {
  
  // Dimension information
  int n = X.n_rows, p = X.n_cols;
  
  // Fit model y ~ X
  arma::colvec coef = arma::solve(X, y);
  
  // Compute the residuals
  arma::colvec res = y - X*coef;
  
  // Estimated variance of the random error
  double s2 = std::inner_product(
    res.begin(), res.end(),
    res.begin(), 0.0) / (n - p);
  
  // Standard error matrix of coefficients
  arma::colvec std_err = arma::sqrt(
    s2 * arma::diagvec(arma::pinv(X.t()*X)));
  
  // Create named list with the above quantities
  return Rcpp::List::create(
    Rcpp::Named("coefficients") = coef,
    Rcpp::Named("stderr") = std_err,
    Rcpp::Named("df.residual") = n - p );
}
```

在R中测试（不自行编译，
直接调用RcppArmadillo中已有函数）：

```
library(RcppArmadillo)
test_fastlm <- function(n=30){
  x <- sample(1:10, size=2*n, replace=TRUE) |>
    matrix(nrow=n, ncol=2)
  x <- cbind(1, x)
  y <- x %*% c(100, 2, 3) + rnorm(n, 0, 0.5)
  fastLm(x, y)
}
set.seed(101)
res <- test_fastlm()
res$coefficients
## [1] 100.268414   1.984362   2.972958
res$stderr
##            [,1]
## [1,] 0.25189799
## [2,] 0.02958671
## [3,] 0.02621272
res$df.residual
## [1] 27
```