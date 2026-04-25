---
crawl_time: '2026-01-17 14:20:11'
framework: rbook
title: 52 Rcpp介绍 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 52 Rcpp介绍

为了提高R程序的运行效率，
可以尽量使用向量化编程，
减少循环，
尽量使用内建函数。
对于效率的瓶颈，
尤其是设计迭代算法时，
可以采用编译代码，
而Rcpp扩展包可以很容易地将C++代码连接到R程序中，
并且支持在C++中使用类似于R的数据类型。

没有学过C++语言的读者，
如果需要编写比较独立的不太依赖于R的已有功能的算法，
可以考虑学习使用Julia语言编写。
Julia语言是最近几年才发明的一种比R更现代、理念更先进的程序语言，
其运行效率一般比R高得多，
经常接近编译代码的效率。

Rcpp可以很容易地把C++代码与R程序连接在一起，
可以从R中直接调用C++代码而不需要用户关心那些繁琐的编译、链接、接口问题。
可以在R数据类型和C++数据类型之间容易地转换。

因为涉及到编译，
所以Rcpp比一般的扩展包有更多的安装要求：
除了要安装Rcpp包之外，
MS Windows用户还需要安装RTools包，
这是用于C, C++, Fortran程序编译链接的开发工具包， 是自由软件。
用户的应用程序路径(PATH)中必须有RTools包可执行程序的路径
(安装RTools可以自动设置)。
如果Rcpp不能找到编译器，
可以把编译器安装到Rcpp默认的位置。
Mac系统的用户可以从应用商店安装Xcode软件，
Linux操作系统可以在操作系统命令行用如下命令安装编译软件：

```
sudo apt-get install r-base-dev
```

在MS Windows系统中，
从[CRAN](https://mirrors.tuna.tsinghua.edu.cn/CRAN/)网站下载RTools工具包，
并将其安装到默认位置`C:\RTools`中，
否则RStudio中使用Rcpp可能会出错。
注意，
RStudio自己的自动安装RTools的功能可能有问题，
安装后不能正常编译含有Rcpp的Rmd文件。
按照RTools 4.0的要求，
还要在自己的“我的文档”文件夹中生成名为.Renviron的如下文本文件：

```
PATH="${RTOOLS40_HOME}\usr\bin;${PATH}"
```

Rcpp支持把C++代码写在R源程序文件内，
执行时自动编译连接调用；
也支持把C++代码保存在单独的源文件中，
执行R程序时自动编译连接调用；
对较复杂的问题， 应制作R扩展包，
利用构建R扩展包的方法实现C++代码的编译连接，
这时接口部分也可以借助Rcpp属性功能或模块功能完成。

## 52.1 Rcpp的用途

* 把已经用R代码完成的程序中运行速度瓶颈部分改写成C++代码，
  提高运行效率。
* 对于C++或C程序源代码或二进制代码提供的函数库，
  可以用Rcpp编写C++界面程序进行R与C++程序的输入、输出的传送，
  并在C++界面程序中调用外来的函数库。
* 注意，用Rcpp编写C++程序，
  不利于把程序脱离R运行或被其他的C++程序调用。
  当然，可以只把Rcpp作为界面，
  主要的算法引擎完全不用Rpp的数据类型。
* RInside扩展包支持把R嵌入到C++主程序中。

## 52.2 Rcpp入门样例

### 52.2.1 用`evalCpp()`转换单一计算表达式

如下的程序可以用来检测Rcpp以及所需要的编译器环境是否正确安装设置：

```
library(Rcpp)
evalCpp("1 + 2")
## [1] 3
```

`evalCpp()`函数输入包含一个C++表达式的字符串，
自动进行编译、连接、接口生成、在R中运行。

### 52.2.2 用`cppFunction()`转换简单的C++函数—Fibnacci例子

考虑用C++程序计算Fibonacci数的问题。
Fibonacci数满足\(f\_0=0, f\_1=1, f\_t=f\_{t-1}+ f\_{t-2}\)。

可以使用如下R代码，其中有一部分C++代码，
用`cppFunction`转换成了R可以调用的同名R函数。

```
library(Rcpp)
cppFunction(code='
  int fibonacci(const int x){
    if(x < 2) return x;
    if(x > 40) return -1; 
    // 太大的x值会占用过多计算资源
    return ( fibonacci(x-1) + fibonacci(x-2) );
  }
')
print(fibonacci(5))
## [1] 5
```

编译、链接、导入是在后台由Rcpp控制自动进行的，
不需要用户去设置编译环境，
也不需要用户执行编译、链接、导入R的工作。
在没有修改C++程序时,
同一R会话期间重新运行不必重新编译。

上面的Fibonacci函数仅接受标量数值作为输入， 不允许向量输入。
这个例子也说明RCpp变异的C++函数允许输入标量(int, double, bool)，
输出标量(int, double, bool)。

从算法角度评价，
这个计算Fibonacci序列的算法是极其低效的，
其算法规模是\(O(2^n)\)，
\(n\)是自变量值。

### 52.2.3 用`sourceCpp()`转换C++程序—正负交替迭代例子

设\(x\_t, t=1,2,\dots,n\)保存在R向量x中，
令
\[\begin{aligned}
y\_1 =& x\_1 \\
y\_t =& (-1)^t y\_{t-1} + x\_t, \ t=2,\dots,n
\end{aligned}\]
希望用C++函数对输入序列x计算输出y，并用R调用这样的函数。

下面的程序用R函数`sourceCpp()`
把保存在R字符串中的C++代码编译并转换为同名的R函数。

```
sourceCpp(code='
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
  NumericVector iters(NumericVector x){
    int n = x.size();
    NumericVector y(n);

    y[0] = x[0];
    double sign=-1;
    for(int t=1; t<n; t++){
      sign *= -1;
      y[t] = sign*y[t-1] + x[t];
    }

    return y;
  }
')
print(iters(1:5))
## [1] 1 3 0 4 1
```

这个例子说明C++程序可以直接写在R程序文件内
(保存为R多行字符串)，
用`sourceCpp()`函数编译。

Rcpp包为C++提供了一个`NumericVector`数据类型，
用来存储数值型向量。
用成员函数`size()`访问其大小，
用方括号下标访问其元素，
但按照C和C++的惯例，
下标从0开始计数。

C程序中定义的函数可以返回`NumericVector`数据类型，
将自动转换为R的数值型向量。

特殊的注释`//[[Rcpp::export]]`
用来指定哪些C++函数是要转换为R函数的。
这叫做Rcpp属性（attributes）功能。

### 52.2.4 用`sourceCpp()`转换C++源文件中的程序—正负交替迭代例子

直接把C++代码写在R源程序内部的好处是不用管理多个源文件，
缺点是当C++代码较长时， 不能利用专用C++编辑环境和调试环境,
出错时显示的错误行号不好定位， 而且把代码保存在R字符串内，
C++代码中用到字符时需要特殊语法。
所以，稍复杂的C++代码应该放在单独的C++源文件内。

假设上面的iters函数的C++代码单独存入了一个[`iters.cpp`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/iters.cpp)源文件中，内容如下：

```
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector iters(NumericVector x){
  int n = x.size();
  NumericVector y(n);
  
  y[0] = x[0];
  double sign=-1;
  for(int t=1; t<n; t++){
    sign *= -1;
    y[t] = sign*y[t-1] + x[t];
  }
  
  return y;
}
```

用如下的`sourceCpp()`函数把C++源文件中代码编译并转换为R可访问的同名函数，
测试调用:

```
sourceCpp(file='iters.cpp')
print(iters(1:5))
## [1] 1 3 0 4 1
```

### 52.2.5 用`sourceCpp()`转换C++源程序文件—卷积例子

考虑向量\(\boldsymbol x=(x\_0, x\_1, \dots, x\_{n-1})\),
\(\boldsymbol y=(y\_0, y\_1, \dots, y\_{m-1})\),
定义\(\boldsymbol x\)与\(\boldsymbol y\)
的离散卷积\(\boldsymbol z=(z\_0, z\_1, \dots, z\_{n+m-2})\)为
\[\begin{aligned}
z\_k = \sum\_{(i,j):\, i+j=k} x\_i y\_j
= \sum\_{i=\max(0, k-m+1)}^{\min(k,n)} x\_i y\_{k-i}.
\end{aligned}\]

假设如下的C++程序保存到了源文件conv.cpp中。

```
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector convolveCpp(
    const NumericVector a, 
    const NumericVector b){
  int na = a.size(), nb = b.size();
  int nab = na + nb - 1;
  NumericVector xab(nab);

  for(int i=0; i < na; i++)
    for(int j=0; j < nb; j++)
      xab[i+j] += a[i] * b[j];

  return xab;
}
```

如下的代码用`sourceCpp()`函数把上面的C++源文件conv.cpp
自动编译并转换为同名的R函数，
进行测试:

```
sourceCpp(file='conv.cpp')
print(convolveCpp(1:5, 1:3))
## [1]  1  4 10 16 22 22 15
```

### 52.2.6 随机数例子

Rcpp的属性(attributes)功能使得在C++中可以访问R的随机数生成功能，
并且可以自动维护随机数种子状态。
例如，
生成一个标准正态随机数的例子：

```
set.seed(101)
evalCpp('
R::rnorm(0,1)
')
## [1] -0.3260365
rnorm(1,0,1)
## [1] 0.5524619
```

```
set.seed(101)
rnorm(2,0,1)
## [1] -0.3260365  0.5524619
```

可以看出C++中`R::rnorm()`生成了一个正态随机数并且更新了R中的随机数种子状态。

在C++中为了生成多个正态随机数，
应该使用`Rcpp::rnorm(n, mean, sd)`。
如：

```
set.seed(101)
evalCpp('
Rcpp::rnorm(2, 0, 1)
')
## [1] -0.3260365  0.5524619
```

对标准正态随机数，也可以简写成`Rcpp::rnorm(n)`。

### 52.2.7 bootstrap例子

考虑单总体\(X\)的样本\(x\_1, \dots, x\_n\)用bootstrap方法近似\(\bar X\)和\(S\)(样本标准差)抽样分布的问题。
bootstrap方法从\(x\_1, \dots, x\_n\)中有放回抽取\(n\)个值\(x\_1^{(1)}, \dots, x\_n^{(1)}\)，
从中计算\(\bar x^{(1)}\)和\(S^{(1)}\)，
并重复\(B\)次（如B=200），
得到\(\bar x^{(j)}\), \(j=1,\dots,n\)，
作为\(\bar x\)的抽样分布的近似样本，
称为bootstrap样本；
得到\(S^{(j)}\), \(j=1,\dots,n\)，
作为\(S\)的抽样分布的近似样本。

R的boot包可以执行这样的计算，
见§[40.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/simulation.html#simexamp-bootstrap)。
如果直接用R程序编程计算，
也比较容易，如：

```
boot_mean_sd <- function(x, B=200){
  out_stat <- matrix(0, B, 2)
  n <- length(x)
  for(j in 1:B){
    x_boot <- x[sample(1:n, size=n, replace=TRUE)]
    out_stat[j,1] <- mean(x_boot)
    out_stat[j,2] <- sd(x_boot)
  }
  
  out_stat
}
```

测试运行：

```
set.seed(101)
xsamp <- rnorm(20, mean=10, sd=2)
set.seed(111)
stats <- boot_mean_sd(xsamp)
hist(stats[,1], 
  main="Bootstrap Sample of Mean",
  xlab = "Sample Mean")
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp_files/figure-html/rcpp-intro-examp-boot02-1.png)

```
hist(stats[,2], 
  main="Bootstrap Sample of Standard Deviation",
  xlab = "Sample Standard Deviation")
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp_files/figure-html/rcpp-intro-examp-boot03-1.png)

注意受到原始样本的限制，
bootstrap样本的分布，
与理论抽样分布还是有偏离的。
只有在对多组实际样本进行bootstrap抽样的意义下，
bootstrap方法近似的统计量抽样分布才是近似于理论抽样分布的。

改用C++实现，
设如下C++源程序保存在源文件`boot-mean-sd.cpp`中：

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericMatrix boot_mean_sd_cpp(
  NumericVector x,
  int B = 200){
  int n = x.size();
  NumericMatrix out_stat(B, 2);
  NumericVector x_boot(n);
  
  for(int j=0; j<B; j++){
    x_boot = x[floor(runif(n, 0, n))];
    out_stat(j,0) = mean(x_boot);
    out_stat(j,1) = sd(x_boot);
  }
  
  return out_stat;
}
```

导入为R函数：

```
library(Rcpp)
sourceCpp(file="boot-mean-sd.cpp")
```

运行测试：

```
set.seed(101)
xsamp <- rnorm(20, mean=10, sd=2)
set.seed(111)
stats_cpp <- boot_mean_sd_cpp(xsamp)
hist(stats_cpp[,1], 
  main="Bootstrap Sample of Mean(C++ version)",
  xlab = "Sample Mean")
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp_files/figure-html/rcpp-intro-examp-boot21-1.png)

```
hist(stats_cpp[,2], 
  main="Bootstrap Sample of Standard Deviation(C++ version)",
  xlab = "Sample Standard Deviation")
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp_files/figure-html/rcpp-intro-examp-boot22-1.png)

在C++中抽样用了`runif()`函数，
而R版本用的是`sample()`，
所以两个版本的结果不完全相同。

注意C和C++的数组下标是从0开始计数的，
所以上面`out_stat`第\(j+1\)行第1列是`out_stat(j,0)`，
另外要注意矩阵用的是圆括号而不是方括号，
因为C和C++中的矩阵下标格式是`A[i][j]`。

### 52.2.8 在Rmd文件中使用C++源程序文件

在Rmd文件中，
可以使用特殊的`Rcpp`代码块，
其中包含C++源代码，
可以直接起到对其调用`sourceCpp()`的作用。

在Rmd文件中输入如下的代码块：

```
```{Rcpp}
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
NumericVector convolveCpp(
    NumericVector a, NumericVector b){
  int na = a.size(), nb = b.size();
  int nab = na + nb - 1;
  NumericVector xab(nab);

  for(int i=0; i < na; i++)
    for(int j=0; j < nb; j++)
      xab[i+j] += a[i] * b[j];

  return xab;
}
```
```

可以使得C++函数`convolveCpp()`被编译并转换成R可以调用的同名函数，
调用如：

```
convolveCpp(1:5, 1:3)
## [1]  1  4 10 16 22 22 15
```

Rcpp代码块也可以加`cache=TRUE`选项，
使得C++程序在没有修改时不必每次重新编译。

在MS Windows系统下如果出错，
可以将RTools安装在默认路径，
即`C:\RTools\bin`与`C:\RTools\mingw_64\bin`中，
并可能需要将这两个路径人为地添加到系统的PATH变量中，
办法是“Windows程序–Windows系统–控制面板–系统和安全–系统–高级系统设置–高级–环境变量–系统变量”，
其中的PATH变量是用分号分开的可执行程序搜索路径，
将其中添加RTools包的`C:\RTools\bin`与`C:\RTools\mingw_64\bin`。