---
crawl_time: '2026-01-17 14:20:08'
framework: rbook
title: 55 Rcpp提供的C++数据类型 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp-vectype.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 55 Rcpp提供的C++数据类型

## 55.1 RObject类

Rcpp包为C++定义了NumericVector, IntegerVector,
CharacterVector, Matrix等新数据类型，
可以直接与R的numeric, charactor, matrix对应。

Rcpp最基础的R数据类型是RObject, 这是NumericVector, IntegerVector等的基类,
通常不直接使用。
RObject包裹了原来R的C API的SEXP数据结构，
并且提供了自动的内存管理，
不再需要用户自己处理建立内存和消除内存的工作。
RObject存储数据完全利用R C API的SEXP数据结构，
不进行额外的复制。

因为RObject类是基类，
所以其成员函数也适用于NumericVector等类。
`isNULL`, `isObject`, `isS4`可以查询是否NULL, 是否对象，
是否S4对象。
`inherits`可以查询是否继承自某个特定类。
用`attributeNames`, `hasAttribute`, `attr`可以访问对象的属性。
用`hasSlot`, `slot`可以访问S4对象的插口（slot）。

RObject有如下导出类:

* IntegerVector: 整数向量；
* NumercicVector: 数值向量；
* LogicalVector: 逻辑向量；
* CharacterVector: 字符型向量；
* GenericVector: 列表；
* ExpressionVector: 表达式向量；
* RawVector: 元素为raw类型的向量。
* IntergerMatrix, NumericMatrix: 整数值或数值矩阵。

在R向量中，如果其元素都是同一类型（如整数、双精度数、
逻辑、字符型），则称为原子向量。
Rcpp提供了IntegerVector, NumericVector, LogicalVector,
CharacterVector等数据类型与R的原子向量类型对应。
在C++中可以用`[]`运算符存取向量元素，
也可以用STL的迭代器。用`.begin()`, `.end()`等界定范围，
用循环或或者accumulate等STL算法处理整个向量。

## 55.2 IntegerVector类

在R中通常不严格区分整数与浮点实数，
但是在与C++交互时，C++对整数与实数严格区分，
所以RCpp中整数向量与数值向量是区分的。

在R中，如果定义了一个仅有整数的向量，
其类型是整数(integer)的，否则是数值型(numeric)的，如：

```
x <- 1:5
class(x)
## [1] "integer"
y <- c(0, 0.5, 1)
class(y)
## [1] "numeric"
```

用`as.integer()`和`as.numeric()`
函数可以显式地确保其自变量转为需要的整数型或数值型。

RCpp可以把R的整数向量传递到C++的IntegerVector中，
也可以把C++的IntegerVector函数结果传递回R中变成一个整数向量。
也可以在C++中生成IntegerVector向量，填入整数值。

### 55.2.1 IntegerVector示例1：返回完全数

如果一个正整数等于它所有的除本身以外的因子的和，
称这个数为完全数。如

\[\begin{aligned}
6 =& 1 + 2 + 3 \\
28 =& 1 + 2 + 4 + 7 + 14 \\
\end{aligned}\]

是完全数。

任务：用C++程序输入前4个完全数偶数，返回到R中。
这4个数为6, 28, 496, 8182。

程序：

```
library(Rcpp)
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
IntegerVector wholeNumberA(){
    IntegerVector epn(4);
    epn[0] = 6;    epn[1] = 28;
    epn[2] = 496;  epn[3] = 8182;

    return epn;
}
')
print(wholeNumberA())
## [1]    6   28  496 8182
```

对`IntegerVector`类型的`x`，
访问单个下标`i`格式为`x[i]`，
下标从0开始计算，
与R向量下标从1开始的规则不同。

从例子看出，可以在C++中建立一个IntegerVector,
需指定大小。
可以逐个填入数值。
直接返回IntegerVector到R即可，
不需用`wrap()`显式地转换，
函数返回值的声明可以用`IntegerVector`声明。

### 55.2.2 IntegerVector示例2：输入整数向量

任务：用C++编写函数，从R中输入整数向量，
计算其元素乘积（与R的`prod()`函数功能类似）。
程序如下：

```
library(Rcpp)
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
IntegerVector prod1(IntegerVector x){
    int prod = 1;
    for(int i=0; i < x.size(); i++){
        prod *= x[i];
    }
    return wrap(prod);
}
')
print(prod1(1:5))
```

为了获得向量的元素个数，
用`x.size()`这种方法。

从程序看出，
用IntegerVector从R中接受一个整数值向量时，
不需要显式地转换。
把一个C++整数值返回给R时，
可以用IntegerVector返回，
因为返回值原来是一个C++的`int`类型，
所以需要用`Rcpp::wrap()`转换一下。
在`sourceCpp`中可以省略`Rcpp::`部分。

也可以返回int, 返回值类型用int声明：

```
library(Rcpp)
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
int prod2(IntegerVector x){
    int prod = 1;
    for(int i=0; i < x.size(); i++){
        prod *= x[i];
    }
    return prod;
}
')
print(prod2(1:5))
```

还可以用C++ STL的算法库进行这样的累计乘积计算，
`std::accumlate()`可以对指定范围进行遍历累计运算。
前两个参数是一个范围，用迭代器(iterators)表示开始和结束，
第三个参数是初值，第四个参数是对每个元素累计的计算。
程序如下：

```
require(Rcpp)
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
int prod3(IntegerVector x){
    int prod = std::accumulate(
        x.begin(), x.end(),
        1, std::multiplies<int>());
    return prod;
}
')
print(prod3(1:5))
```

在以上的输入IntegerVector的C++程序中，
如果从R中输入了实数型的向量，
则元素被转换成整数型。比如
`prod2(seq(1,1.9,by=0.1))`
结果将等于1。
如果输入了无法转换为整数型向量的内容，
比如`prod2(c(’a’, ’b’, ’c’))`, 程序会报错。

## 55.3 NumericVector类

NumericVector类在C++中保存双精度型一维数组，
可以与R的实数型向量(class为numeric)相互转换。
这是自己用C++程序与R交互时最常用到的数据类型。

`x.size()`返回元素个数。

### 55.3.1 示例1：计算元素\(p\)次方的和

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
double ssp(
    NumericVector vec, double p){
  double sum = 0.0;
  for(int i=0; i < vec.size(); i++){
    sum += pow(vec[i], p);
  }
  return sum;
}
')
ssp(1:4, 2)
## [1] 30
ssp((1:4)/10, 2.2)
## [1] 0.2392496
sum( ((1:4)/10)^2.2 )
## [1] 0.2392496
```

从程序看出，用Rcpp属性编译时，
C++函数的输入与返回值类型转换有如下规则：
R中数值型向量在C++中可以用NumericVector接收；
R中单个的实数在C++中可以用double来接收；
为了返回单个的实数，
可以直接返回double，
也可以返回NumericVector类型,
或者从一个double型用`wrap()`转换。
C++中如果返回NumericVector, 在R中转换为数值型向量。

### 55.3.2 示例2：`clone`函数

在自定义R函数时， 输入的自变量的值不会被改变，
相当于自变量都是局部变量。
如果在自定义函数中修改了自变量的值，
实际上只能修改自变量的一个副本的值。如

```
x <- 100
f <- function(x){
  print(x); x <- 99; print(x)
}
c(f(x), x)
## [1] 100
## [1] 99
## [1]  99 100
```

但是，在用Rcpp编写R函数时，
因为RObject传递的是指针，
并不自动复制自变量值，
所以修改自变量值会真的修改原始的自变量变量值。
如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f2(NumericVector x){
  x[0] = 99.0;
  return(x);
}
')
x <- 100
c(f2(x), x)
[1] 99 99
```

可见自变量的值被修改了。
当然，对这个问题而言， 因为输入的是一个标量，
只要函数自变量不是NumericVector类型而是用double类型，
则自变量值会被复制，达到值传递的效果，
自变量值也就不会被真的修改。
如

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f3(double x){
  x = 99.0;
  return(wrap(x));
}
')
x <- 100
c(f3(x), x)
## [1]  99 100
```

下面的程序把输入向量每个元素平方后返回，
为了不修改输入自变量的值而是返回一个修改后的副本，
使用了Rcpp的`clone`函数：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector square(NumericVector x){
  NumericVector y = clone(x);
  for(int i=0; i < x.size(); i++){
    y[i] = x[i]*x[i];
  }
  return(y);
}
')
x <- c(2, 7)
cbind(square(x), x)
##         x
## [1,]  4 2
## [2,] 49 7
```

`x.sort()`会对`x`从小到大排序，
同时返回排序结果，
`x`被修改了。如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector test_sort(NumericVector x){
  return x.sort();
}
')
x <- c(3, 5, 1)
test_sort(x)
## [1] 1 3 5
x
## [1] 1 3 5
```

用`clone`制作副本，避免修改`x`:

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector test_sort(NumericVector x){
  NumericVector y = clone(x);
  return y.sort();
}
')
x <- c(3, 5, 1)
test_sort(x)
## [1] 1 3 5
x
## [1] 3 5 1
```

### 55.3.3 示例3：向量子集

C++中Rcpp支持的向量和列表，
仍可以用正整数向量作为下标。
例如，
下面的C++函数求指定的元素子集的和：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
double sumsub(
  NumericVector x, IntegerVector i){
  NumericVector y = x[i-1];
  double s = 0.0;
  int n = y.size();
  for(int j=0; j<n; j++)
    s += y[j];
  
  return s;
}
')
x <- 1:10
sumsub(x, c(1, 3, 5))
## [1] 9
```

注意在程序中将输入的下标向量减了1，
以适应从R规则到C++规则的变化。
Rcpp在输入和输出时会自动在整型和双精度型之间转换，
比如上面例子中输入的x是整型的，
要求输入NumericVector是双精度型的，
Rcpp会自动转换。

Rcpp也支持逻辑下标，
比如，下面在C++定义的函数可以取出向量的正值的子集：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector get_positive(NumericVector x){
  NumericVector y = x[x > 0];

  return y;
}
')
x <- c(1, -1, 2, -2)
get_positive(x)
## [1] 1 2
```

Rcpp也支持按元素名访问向量元素或向量子集。

## 55.4 NumericMatrix类

`NumericMatrix x(n,m)`产生一个未初始化的\(n\times m\)矩阵，
元素为`double`类型。
`x.nrow()`返回行数，
`x.ncol()`返回列数，
`x.size()`返回元素个数。

下标从0开始计算。
为了访问\(x\_{ij}\)，
用`x(i,j)`的格式：
注意，**不是**`x[i,j]`也不是`x[i][j]`。

为了访问`x`的第`i`行，用`x.row(i)`或`x(i,_)`；
为了访问`x`的第`j`列，用`x.column(j)`或`x(_,j)`。

`transpose(x)`返回`x`的转置。

`NumericMatrix::zeros(n)`返回`n`阶元素为0的方阵。
`NumericMatrix::eye(n)`返回`n`阶单位阵。

### 55.4.1 示例1：计算矩阵各列模的最大值

输入一个矩阵，计算各列的向量模，并返回模的最大值。

```
library(Rcpp)
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
double testfun(NumericMatrix x){
  int nr = x.nrow();
  int nc = x.ncol();
  double y, y1;

  y = 0.0;
  for(int j=0; j<nc; j++){
    y1 = 0.0;
    for(int i=0; i<nr; i++){
      y1 += pow(x(i,j), 2);
    }
    if(y1 > y) y = y1;
  }
  y = sqrt(y);

  return y;
}
')
x <- matrix(c(1, 2, 4, 9), 2, 2)
print(x)
print(testfun(x))
##      [,1] [,2]
## [1,]    1    4
## [2,]    2    9
## [1] 9.848858
```

其中对每列计算模的部分，也可以改写成：

```
  y = 0.0;
  NumericVector ycol(nr);
  for(int j=0; j<nc; j++){
    ycol = x.column(j);
    y1 = sum(ycol * ycol);
    if(y1 > y) y = y1;
  }
  y = sqrt(y);
```

这里用了`x.column(j)`函数提取矩阵的一列，
用了`ycol * ycol`作向量元素之间的四则运算，
用了R的`sum`函数求和。

### 55.4.2 示例2：把输入矩阵制作副本计算元素平方根

下面的例子输入一个R矩阵，
输出其元素的平方根，
用了clone函数来避免对输入的直接修改。
没有按行列循环而是将矩阵看作一个按列拉直的向量进行遍历，
矩阵实际上也是按向量存储的。

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericMatrix matSqrt(NumericMatrix x){
  NumericMatrix y = clone(x);
  std::transform(y.begin(), y.end(),
    y.begin(), ::sqrt);
  return(y);
}
')
x <- rbind(c(1,2), c(3,4))
cbind(matSqrt(x), x)
##          [,1]     [,2] [,3] [,4]
## [1,] 1.000000 1.414214    1    2
## [2,] 1.732051 2.000000    3    4
```

在上面的C++程序中， NumericMatrix看成了一维数组，
用STL的iterater遍历， 用STL的transform对每个元素计算变换。

### 55.4.3 示例3：访问列子集

可以用`x(_,j)`这样的格式将矩阵`x`的第`j`列取出为普通向量，
并可以赋值修改。
也可以用`x.column(j)`，
用法相同。
`x(i,_)`取出第`i`行为一个普通向量。
比如，
下面的C++函数将输入的矩阵的每一行计算累加和：

```
library(Rcpp)
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericMatrix testfun(NumericMatrix x){
  NumericMatrix y = clone(x);
  for(int j=1; j<x.ncol(); j++){
    y(_,j) = y(_,j-1) + x(_,j);
  }

  return y;
}
')
x <- matrix(c(1, 2, 4, 9), 2, 2)
print(x)
##      [,1] [,2]
## [1,]    1    4
## [2,]    2    9
print(testfun(x))
##      [,1] [,2]
## [1,]    1    5
## [2,]    2   11
```

## 55.5 Rcpp的其它向量类

### 55.5.1 Rcpp的LogicalVector类

LogicalVector类可以存储C++值true, false,
还可以保存缺失值NA\_REAL, R\_NaN, R\_PosInf,
但是这些不同的缺失值转换到R中都变成NA。

如:

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
LogicalVector f4(){
  LogicalVector x(5);
  x[0] = false; x[1] = true;
  x[2] = NA_REAL;
  x[3] = R_NaN; x[4] = R_PosInf;
  return(x);
}
')
f()
## [1] FALSE  TRUE    NA    NA    NA
identical(f(), c(FALSE, TRUE, rep(NA,3)))
## [1] TRUE
```

### 55.5.2 Rcpp的CharacterVector类型

CharacterVector类型可以与R的字符型向量相互交换信息，
在C++中其元素为字符串。
字符型缺失值在C++中为R\_NAString。
R的字符型向量也可以转换为std::vector。

Rcpp定义了`String`用来表示单个字符串。

如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
CharacterVector f5(){
  CharacterVector x(3);
  x[0] = "This is a string";
  x[1] = "Test";
  x[2] = R_NaString; 
  return(x);
}
')
f5()
## [1] "This is a string" "Test"             NA
```

## 55.6 Rcpp提供的其它数据类型

### 55.6.1 Named类型

R中的向量、矩阵、数据框可以有元素名、列名、行名。
这些名字可以借助Named类添加。

例如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f6(){
  NumericVector x = NumericVector::create(
    Named("math") = 82,
    Named("chinese") = 95,
    Named("English") = 60);
  return(x);
}
')
f6()
##   math chinese English 
##     82      95      60
```

“`Named(`元素名`)`”可以简写成“\_[元素名](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-index.html#p-t-subset-names)”。
如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector f6b(){
  NumericVector x = NumericVector::create(
    _["math"] = 82,
    _["chinese"] = 95,
    _["English"] = 60);
  return(x);
}
')
```

### 55.6.2 List类型

Rcpp提供的List类型对应于R的list(列表)类型，
在C++中也可以写成GenericVector类型。 其元素可以不是同一类型，
在C++中可以用方括号和字符串下标的格式访问其元素。

例如，下面的函数输入一个列表， 列表元素vec是数值型向量，
列表元素multiplier是数值型标量， 返回一个列表，
列表元素sum为vec元素和，
列表元素dsum为vec元素和乘以multiplier的结果：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
List f7(List x){
  NumericVector vec = as<NumericVector>(x["vec"]);
  double multiplier = as<double>(x["multiplier"]);
  double y = 0.0, y2;
  for(int i=0; i<vec.length(); i++){
    y += vec[i];
  }
  y2 = y*multiplier;
  return(List::create(Named("sum")=y,
    Named("dsum")=y2));
}
')
f7(list(vec=1:5, multiplier=10))
## $sum
## [1] 15
## 
## $dsum
## [1] 150
```

上面的程序用了`Rcpp::List::create()`当场生成List类型，
因为用Rcpp属性功能编译所以可以略写`Rcpp::`。
也可以在程序中预先生成指定大小的列表， 然后再给每个元素赋值，
元素值可以是任意能够转化为SEXP的类型， 如：

```
.........
List gv(2);
gv[0] = "abc";
gv[1] = 123;
```

可以用List的reserve函数为列表指定元素个数。

### 55.6.3 Rcpp的DataFrame类

Rcpp的DataFrame类用来与R的data.frame交换信息。

示例如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
DataFrame f8(){
  IntegerVector vec = 
    IntegerVector::create(7,8,9);
  std::vector<std::string> s(3);
  s[0] = "abc"; s[1] = "ABC"; s[2] = "123";
  return(DataFrame::create(
    Named("x") = vec,
    Named("s") = s));
}
')
f8()
##   x   s
## 1 7 abc
## 2 8 ABC
## 3 9 123
```

### 55.6.4 Rcpp的Function类

Rcpp的Function类用来接收一个R函数，
并且可以在C++中调用这样的函数。

示例如:

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
SEXP dsort(Function sortFun, SEXP x){
  return sortFun(x, Named("decreasing", true));
}
')
dsort(sort, c('bb', 'ab', 'ca'))
## [1] "ca" "bb" "ab"
## dsort(sort, c(2,1,3))
## [1] 3 2 1
```

程序用Function对象sortFun接收从R中传递过来的排序函数，
实际调用时传递过来的是R的sort函数。

在C++中调用R函数时，
有名的自变量用“Named(自变量字符串, 自变量值)”的格式给出。

程序中的待排序的向量与排序后的向量都用了SEXP来说明，
即直接传送原始R API指针，
这样可以不管原来类型是什么，
在C++中完全不进行类型转换或复制。

从运行例子看出数值和字符串都正确地按照降序排序后输出了。

R函数可以不作为C++的函数自变量传递进来，
而是直接调用R的函数。
R的许多函数都已经在Rcpp名字空间中输出了，
所以可以直接调用R的各个向量化的函数，
包括随机数函数。
例如，
下面的C++函数返回一个各列为随机数的数值型矩阵：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericMatrix rngCpp(const int N) {
  NumericMatrix X(N, 4);
  X(_, 0) = runif(N);
  X(_, 1) = rnorm(N);
  X(_, 2) = rt(N, 5);
  X(_, 3) = rbeta(N, 1, 1);
  return X;
}')
```

使用了Rcpp属性时，
生成的界面程序会自动生成一个RNGScope的实例，
用来保存当前的随机数发生器状态，
在解构时将自动更新随机数发生器状态。
这是R调用编译代码时对编译代码的要求，
如果不使用Rcpp和属性，
就需要用户自己添加命令来维护随机数种子。

测试上面的随机数调用：

```
set.seed(101)
xm <- rngCpp(3)
xm
##            [,1]       [,2]      [,3]       [,4]
## [1,] 0.37219838  0.4061679  0.107946 0.78664800
## [2,] 0.04382482 -0.5242428  1.539425 0.07668112
## [3,] 0.70968402 -0.4303593 -2.259442 0.92878745
set.seed(101)
xm2 <- cbind(
  runif(3),
  rnorm(3),
  rt(3, 5),
  rbeta(3,1,1)
)
all.equal(xm, xm2)
## [1] TRUE
```

还可以使用名字空间R中的标量随机数发生器，如：

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericVector rngCppScalar() {
  NumericVector x(4);
  x[0] = R::runif(0,1);
  x[1] = R::rnorm(0,1);
  x[2] = R::rt(5);
  x[3] = R::rbeta(1,1);
  return(x);
}')
```

### 55.6.5 Rcpp的Environment类

R的环境是分层的，可以逐层查找变量名对应的内容。
Rcpp的Environment类用来与R环境对应。
可以利用Environment来定位R的扩展包中的函数或数据，
例如下面的程序片段在C++中定位了stats扩展包中的
rnorm函数并进行了调用:

```
Environment stats("package:stats");
Function rnorm = stats["rnorm"];
return rnorm(3, Named("sd", 100.0));
```

当然，也可以用Function对象直接从各环境中搜索rnorm函数名，
但是这样指定环境更可靠。

下面的例子访问了R全局环境，
取出了全局变量`x`的值存入C++的双精度型STL向量中，
并把一个C++的STL map型数据转换成有名字符型向量存到了全局变量y中。

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
void f9(){
  Environment global = Environment::global_env();
  std::vector<double> vx = global["x"];
  std::map<std::string, std::string> ma;
  ma["foo"] = "abc";
  ma["bar"] = "123";
  global["y"] = ma;
  return;
}
')
x <- c(1,5)
f9()
y
##   bar   foo 
## "123" "abc"
```