---
crawl_time: '2026-01-17 14:20:09'
framework: rbook
title: 54 Rcpp 属性 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp-attr.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 54 Rcpp 属性

## 54.1 Rcpp属性介绍

Rcpp属性(attributes)用来简化把C++函数变成R函数的过程，
这可以方便在交互使用中将C++和C代码载入到R中，
也有利于扩展包中C++和C代码的使用。
做法是在C++源程序中加入一些特殊注释，
利用其指示自动生成C++与R的接口程序。
属性是C++11标准的内容，
现在的编译器支持还不多，
所以在Rcpp支持的C++程序中写成了特殊格式的注释。

Rcpp属性有如下优点：

* 降低了同时使用R与C++的学习难度；
* 取消了很多繁复的接口代码；
* 可以在R会话中很简单地调用C++代码，
  不需要用户自己考虑编译、连接、接口问题；
* 可以先交互地调用C++，
  成熟后改编为R扩展包而不需要修改界面代码。

Rcpp属性的主要组成部分如下：

* 在C++中，提供`Rcpp::export`标注要输出到R中的C++函数。
* 在R中，提供`sourceCpp()`，
  用来自动编译连接保存在文件或R字符串中的C++代码，
  并自动生成界面程序把C++函数转换为R函数。
* 在R中，提供`cppFunction()`函数，
  用来把保存在R字符串中的C++函数自动编译连接并转换成R函数。
  提供`evalCpp()`函数，
  用来把保存在R字符串中的C++代码片段自动编译连接并执行。
* 在C++中，提供`Rcpp::depends`标注，
  为了`sourceCpp()`说明编译连接C++代码时需要的外部头文件和库的位置。
* 在构建R扩展包时，提供`compileAttributes()` R函数，
  自动给C++函数生成相应的 `extern C`声明和`.Call`接口代码。

## 54.2 在C++源程序中指定要导出的C++函数

### 54.2.1 特殊注释`//[[Rcpp::export]]`

用特殊注释`//[[Rcpp::export]]`说明某C++函数需要在编译成动态链接库时，
把这个函数导出到链接库的对外可见部分。

例如

```
//[[Rcpp::export]]
NumericVector convolveCpp(
  NumericVector a, NumericVector b){
  ......
}
```

具体程序参见[52.2.5](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp.html#rcpp-intro-examp-ex4)(用`sourceCpp()`转换C++源程序文件—卷积例子)。
假设此C++源程序保存到了当前工作目录的conv.cpp源文件中，
为了在R中调用此C++程序，只要用如：

```
sourceCpp(file='conv.cpp')
convolveCpp(1:3, 1:5)
```

注意`sourceCpp()`把C++源程序自动进行了编译链接，
并把用`//[[Rcpp::export]]`说明的C++函数`convolveCpp`转换成了同名的R函数。
在同一R会话内，
如果源程序和其依赖资源没有变化（根据文件更新时间判断），
就不重新编译C++源代码。

### 54.2.2 修改导出的函数名

在用特殊注释说明要导出的C++函数时，
可以用特殊的name=参数指定函数导出到R中的R函数名。
如果不指定，R函数名和C++函数名是相同的。

例如

```
//[[Rcpp::export(name="conv")]]
NumericVector convolveCpp(
  NumericVector a, NumericVector b){
  ......
}
```

则C++函数convolveCpp导入到R中后， 改名为“conv”。

### 54.2.3 可导出的函数

对于要导出的C++函数有如下要求：

* 必须在全局名字空间中定义，
  而不能在某个C++名字空间声明内定义。
* 自变量必须能够用`Rcpp::as()`转换成C++类型；
* 返回值必须是空值或者能够用`Rcpp::wrap()`转换成R类型。
* 在自变量和返回值类型说明中， 必须使用完整的类型，
  比如`std::string`不能简写成`string`。
  Rcpp提供的类型如`NumericVector`可以不必用`Rcpp::`修饰。

## 54.3 在R中编译链接C++代码

### 54.3.1 `sourceCpp()`函数中直接包含C++源程序字符串

`sourceCpp()`函数可以用code=指定一个R字符串，
字符串的内容是C++源程序，
其中还是用特殊注释`//[[Rcpp::export]]`标识要导出的C++函数。 如

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector convolveCpp(
    NumericVector a, NumericVector b){
  .........
}
')
convolveCpp(1:3, 1:5)
```

### 54.3.2 `cppFunction()`函数中直接包含C++函数源程序字符串

对于比较简单的单个C++函数，
可以用`cppFunction()`函数的code=指定一个R字符串，
字符串的内容是一个C++函数定义， 转换为一个R函数。
例如

```
cppFunction(code='
  int fibonacci(const int x){
    if(x < 2) return x;
    else
      return ( fibonacci(x-1) + fibonacci(x-2) );
  }
')
print(fibonacci(5))
```

### 54.3.3 `evalCpp()`函数中直接包含C++源程序表达式字符串

为了在R中计算一个简单的C++表达式，
可以用`evalCpp('C++表达式内容')`，如

```
evalCpp('std::numeric_limits<double>::max()')
```

函数将返回该C++表达式的值。

### 54.3.4 用`depends`指定要链接的库

在`cppFunction()`和`evalCpp()`中，
可以用`depends`参数指定要链接的其它库，如

```
sourceCpp(depends='RcppArmadillo', code='......')
```

在编译代码时与RcppArmadillo的动态连接库连接。

也可以把这样的链接依赖关系写在特殊的C++注释中，如

```
//[[Rcpp::depends(RcppArmadillo)]]

#include <RcppArmadillo.h>
using namespace Rcpp;

// [[Rcpp::export]]
List fastLm(NumericVector yr, NumericMatrix Xr) { ... }
```

可以依赖多个库，如`Rcpp::depends(Matrix, RcppArmadillo)`。

这样的注释仅对`sourceCpp()`和`cppFunction()`有效，
如果是在编译R扩展包，
仍需要把依赖的包列在DESCRIPTION文件的Imports中，
把要链接的包列在LinkingTo中。

## 54.4 Rcpp属性的其它功能

### 54.4.1 自变量有缺省值的函数

借助于Rcpp属性,
自变量有缺省值的C++函数可以自动转换成自变量有缺省值的R函数。
定义时要符合C++语法，
比如带缺省值的自变量都要在不带缺省值的自变量的后面，
缺省值不能有变量。

例如

```
DataFrame readData(
  CharacterVector file,
  CharacterVector colNames = CharacterVector::create(),
  std::string comment = "#",
  bool header = true){ ... }
```

转换到R中，相当于

```
function(
  file, 
  colNames=character(), 
  comment="#", 
  header=TRUE)
```

### 54.4.2 异常传递

Rcpp支持的C++源程序不能直接调用R的`stop`，
可以使用C++的异常，如：

```
if (unexpectedCondition)
  throw Rcpp::exception(
  "Unexpected condition occurred");
```

可简写成：

```
if (unexpectedCondition)
  Rcpp::stop(
  "Unexpected condition occurred");
```

C++抛出的异常先被Rcpp捕获，
然后传递给R，
变成调用`stop()`。

也可以将警告信息发送给R：

```
if (unexpectedCondition)
  Rcpp::warning(
  "Unexpected condition occurred");
```

### 54.4.3 允许用户中断

在C++代码中进行长时间的计算时，
应该允许用户可以中断计算。
Rcpp的办法是在C++计算过程中每隔若干步循环就插入一个
`Rcpp::checkUserInterrupt();`语句。
如：

```
for (int i=0; i<1000000; i++) {
  // check for interrupt every 1000 iterations
  if (i % 1000 == 0)
    Rcpp::checkUserInterrupt();
  // ...do some expensive work...
}
```

可以每一两秒检查一次用户中断。
R用户在用Esc键或者RStudio的停止按钮引发用户中断时，
Rcpp捕捉到中断信号并终止运行，
退回到R命令行。

### 54.4.4 把R代码写在C++源文件中

正常情况下，应该把R代码和C++代码写在分别的源程序中，
当C++代码比较短时，
也可以把C++代码写在R源程序中作为一个字符串。

Rcpp允许把C++代码和R代码都写在一个C++源文件中，
R代码作为特殊的注释，以`/*** R`行开头，以正常的`*/`结束。
在R中用`sourceCpp()`调用这个C++源文件，
就可以编译C++后执行其中特殊注释内的R代码。
这样的特殊注释可以有多个。

例如，下述内容保存在文件`fibo.cpp`中:

```
//[[Rcpp::export]]
int fibonacci(const int x){
    if(x < 2) return x;
    else
      return ( fibonacci(x-1) + fibonacci(x-2) );
}

/*** R
  # 调用C++中的fibonacci()函数
  print(fibonacci(10))
*/
```

只要在R中运行

```
sourceCpp(file='fibo.cpp')
```

就可以编译连接此C++文件，
把其中用`//[[Rcpp::export]]`标识的函数转换为R函数，
并在R中执行源文件内特殊注释中的R代码。

### 54.4.5 用`invisible`要求函数结果不自动显示

R中用`invisible()`函数说明函数的返回值，
则在命令行调用该函数时不自动显示函数的返回值。
对Rcpp编译的函数，
只有不返回值（返回值用`void`声明）时才是不显示的，
如果要增加不显示返回值的要求，
可以在`Rcpp::export`特殊注释中加`invisible = true`选项，如：

```
// [[Rcpp::export(invisible = true)]]
NumericVector convolveCpp(
  NumericVector a,
  NumericVector b){ ... }
```

### 54.4.6 在C++中调用R的随机数发生器

在C或C++中调用R的随机数发生器，
需要能够同步地更新随机数发生器状态。
如果利用Rcpp属性编译C++源程序，
则Rcpp属性会自动添加一个RNGScope实例进行随机数发生器状态的同步。