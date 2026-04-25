---
crawl_time: '2026-01-17 14:20:59'
framework: rbook
title: 18 R程序效率 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-prof.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 18 R程序效率

## 18.1 R的运行效率

R是解释型语言，在执行单个运算时，
效率与编译代码相近；
在执行迭代循环时， 效率较低，
与编译代码的速度可能相差几十倍。
在循环中对变量进行修改尤其低效，
因为R在修改某些数据类型的子集时会复制整个数据对象。
R以向量、矩阵为基础运算单元，
在进行向量、矩阵运算时效率很高，
应尽量采用向量化编程。

另外，R语言的设计为了方便进行数据分析和统计建模，
有意地使语言特别灵活，
比如，
变量为动态类型而且内容与类型均可修改，
变量查找在当前作用域查找不到可以向上层以及扩展包中查找，
函数调用时自变量仅在使用其值时才求值（懒惰求值），
这样的设计都为运行带来了额外的负担，
使得运行变慢。

在计算总和、元素乘积或者每个向量元素的函数变换时，
应使用相应的函数，如`sum`, `prod`, `sqrt`, `log`等。

对于从其它编程语言转移到R语言的学生，
如果不细究R特有的编程模式，
编制的程序效率可能比正常R程序慢上几十倍，
而且繁琐冗长。

为了提高R程序的运行效率，
需要尽可能利用R的向量化特点，
尽可能使用已有的高效函数，
必要时可以把运行速度瓶颈部分改用C++、FORTRAN等编译语言实现，
可以用R的profiler工具查找运行瓶颈。
对于大量数据的长时间计算，
可以借助于现代的并行计算工具。

如何决定是否要修改程序以提高效率？
对已有的程序，
仅在运行速度不满意时才需要进行改进，
否则没必要花费宝贵的时间用来节省几秒钟的计算机运行时间。

要改善运行速度，
首先要找到运行的瓶颈，
这可以用专门的性能分析（profiling）功能实现。
R软件中的`Rprof()`函数可以执行性能分析的数据收集工作，
收集到的性能数据用`summaryRprof()`函数可以显示运行最慢的函数。
如果使用RStudio软件，可以用`Profile`菜单执行性能数据收集与分析，
可以在图形界面中显示程序中哪些部分运行花费时间最多。

在改进已有程序的效率时，
第一要注意的就是不要把原来的正确算法改成一个速度更快但是结果错误的算法。
这个问题可以通过建立试验套装，
用原算法与新算法同时试验看结果是否一致来避免。
多种解决方案的正确性都可以这样保证，
也可以比较多种解决方案的效率。

本章后面部分描述常用的改善性能的方法。
对于涉及到大量迭代的算法，
如果用R实现性能太差不能满足要求，
可以改成C++编码，用Rcpp扩展包连接到R中。
Rcpp扩展包的使用将单独讲授，见第[52](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp.html#rcpp)章。

R的运行效率也受到内存的影响，
占用内存过多的算法有可能受到物理内存大小限制无法运行，
过多复制也会影响效率。

如果要实现一个比较单纯的不需要利用R已有功能的算法，
发现用R计算速度很慢的时候，
也可以考虑先用[Julia语言](https://julialang.org/)实现。
Julia语言设计比R更先进，运算速度快得多，
运算速度经常能与编译代码相比，
缺点是刚刚诞生几年的时间，
可用的软件包还比较少。
参见本书作者的[Julia语言入门](https://www.math.pku.edu.cn/teachers/lidf/docs/Julia/html/_book/index.html)。

## 18.2 性能比较和向量化编程示例

充分利用R的向量化编程和内建函数可以提高程序效率。
R函数`system.time()`可以测量某个表达式的运行时间，
`proc.time()`可以用作会话期间的计时时钟，
扩展包bench可以用来比较不同表达式的运行时间。

**例18.1** 假设要计算如下的统计量：
\[
w = \frac{1}{n} \sum\_{i=1}^n | x\_i - \hat m |,
\]
其中\(x\_1, x\_2, \dots, x\_n\)是某总体的样本，
\(\hat m\)是样本中位数。
比较不同的编程方法。

用传统的编程风格，
把这个统计量的计算变成一个R函数，可能会写成：

```
mad_f1 <- function(x){
  n <- length(x)
  mhat <- median(x)
  s <- 0.0
  for(i in 1:n){
    s <- s + abs(x[i] - mhat)
  }
  s <- s/n
  return(s)
}
```

用R的向量化编程，函数体只需要一个表达式：

```
mad_f2 <- function(x) mean( abs(x - median(x)) )
```

其中`x - median(x)`利用了向量与标量运算结果是向量每个元素与标量运算的规则，
`abs(x - median(x))`利用了`abs()`这样的一元函数如果以向量为输入就输出每个元素的函数值组成的向量的规则，
`mean(...)`避免了求和再除以`n`的循环也不需要定义多余的变量`n`。

显然，第二种做法的程序比第一种做法简洁的多，
如果多次重复调用，
第二种做法的计算速度比第一种要快几十倍甚至上百倍。
在R中，
用`system.time()`函数可以求某个表达式的计算时间，
返回结果的第3项是流逝时间。
下面对`x`采用10000个随机数，
并重复计算1000次，比较两个程序的效率：

```
nrep <- 1000
x <- runif(10000)
y1 <- numeric(nrep); y2 <- y1
system.time(for(i in 1:nrep) y1[i] <- mad_f1(x) )[3]
## elapsed 
##    1.19
system.time(for(i in 1:nrep) y1[i] <- mad_f2(x) )[3]
## elapsed 
##    0.22
```

速度相差数倍。

用`system.time()`或者`proc.time()`测量运行时间会受到后台正在运行的其它任务的干扰。
有一个R扩展包bench可以多次运行表达式，
从而比较两个或多个表达式的运行时间（类似的包还有microbenchmark）。
如:

```
x <- runif(10000)
bench::mark(
  mad_f1(x),
  mad_f2(x)
)
## Unit: microseconds
##   expr      min       lq      mean   median        uq      max neval cld
##  f1(x) 1321.802 1519.151 1642.8350 1555.001 1590.5510 5826.800   100   b
##  f2(x)  275.801  312.951  330.2541  322.501  326.9515  781.001   100  a
```

因为测试运行受到操作系统中同时运行的其它任务的干扰，
所以我们不应该看平均时间，
而应该看中位数，
或者最小时间（最快可能）。
就运行时间中位数（单位：毫秒）来看，`f2()`比`f1()`快大约4倍。

**例18.2** 假设要编写函数计算
\[
\begin{aligned}
f(x) = \begin{cases}
1 & x \geq 0, \\
0 & \text{其它}
\end{cases}
\end{aligned}
\]
如何向量化编程？

利用传统思维，程序写成

```
pos_f1 <- function(x){
    n <- length(x)
    y <- numeric(n)

    for(i in seq_along(x)){
        if(x[i] >= 0) y[i] <- 1
        else y[i] <- 0
    }

    y
}
```

实际上，`y <- numeric(n)`使得`y`的每个元素都初始化为0，
所以程序中`else y[i] <- 0`可以去掉。

利用向量化与逻辑下标，程序可以写成:

```
pos_f2 <- function(x){
    n <- length(x)
    y <- numeric(n)
    y[x >= 0] <- 1

    y
}
```

但是，利用R中内建函数`ifelse()`，
可以把函数体压缩到仅用一个语句：

```
pos_f3 <- function(x) ifelse(x >= 0, 1, 0)
```

**例18.3** 考虑一个班的学生存在生日相同的概率。
假设一共有365个生日(只考虑月、日)。
设一个班有n个人, 当n大于365时`{`至少两个人有生日相同`}`是必然事件(概率等于1)。

当\(n\)小于等于365时：
\[
\begin{aligned}
& P(\text{至少有两人同生日}) \\
=& 1 - P(\text{$n$个人生日彼此不同}) \\
=& 1 - \frac{365\times 364\times\cdots\times(365 - (n-1))}{365^n} \\
=& 1 - \frac{365-0}{365} \cdot \frac{365-1}{365} \cdots
\frac{365 - (n-1)}{365}
\end{aligned}
\]

对\(n=1,2,\dots, 365\)来计算对应的概率。

完全用循环（两重循环），程序写成：

```
birth_f1 <- function(){
  ny <- 365
  x <- numeric(ny)
  for(n in 1:ny){
    s <- 1
    for(j in 0:(n-1)){
      s <- s * (365-j)/365
    }
    x[n] <- 1 - s
  }
  x
}
```

注意，
不能先计算\(365\times 364\times\cdots\times(365 - (n-1))\)
再和\(365^n\)相除，
这会造成数值溢出。

用`prod()`函数可以向量化内层循环：

```
birth_f2 <- function(){
  ny <- 365
  x <- numeric(ny)
  for(n in 1:ny){
    x[n] <- 1 - prod((365:(365-n+1))/365)
  }
  x
}
```

程序利用了向量与标量的除法，
以及内建函数`prod()`。

把程序用`cumprod()`函数改写，
可以完全避免循环：

```
birth_f3 <- function(){
  ny <- 365
  x <- 1 - cumprod((ny:1)/ny)
  x
}
```

用bench包比较:

```
bench::mark(
  birth_f1(),
  birth_f2(), 
  birth_f3()
)
## Unit: microseconds
##  expr      min        lq       mean    median       uq      max neval cld
##  f1() 2467.000 2792.1515 2963.27806 2979.9515 3033.202 8058.001   100   c
##  f2()  330.100  387.1005  674.42992  413.3505  438.301 8445.601   100  b 
##  f3()    1.901    2.3515   27.51003    2.8510    3.651 2322.500   100 a
```

`birth_f2()`比`birth_f1()`快大约6倍，
`birth_f3()`比`birth_f2()`又快了大约140倍倍，
`birth_f3()`比`birth_f1()`快了1000倍！

## 18.3 减少显式循环

显式循环是R运行速度较慢的部分，
有循环的程序也比较冗长，
与R的向量化简洁风格不太匹配。
另外，
在循环内修改数据子集，例如数据框子集，
可能会先制作副本再修改，
这当然会损失很多效率。
R 3.1.0版本以后列表元素在修改时不制作副本，
但数据框还会制作副本。

前面已经指出，
利用R的向量化运算可以减少很多循环程序。

R中的有些运算可以用内建函数完成，
如`sum`, `prod`, `cumsum`,
`cumprod`, `mean`, `var`, `sd`等。
这些函数以编译程序的速度运行，
不存在效率损失。

R的`sin`, `sqrt`, `log`等函数都是向量化的，
可以直接对输入向量的每个元素进行变换。

对矩阵，用`apply`函数汇总矩阵每行或每列。
`colMeans`, `rowMeans`可以计算矩阵列平均和行平均，
`colSums`, `rowSums`可以计算矩阵列和与行和。

apply类函数有多个，
包括apply, sapply, lapply, vapply, tapply, replicate等。
这些函数不一定能提高程序运行速度，
但是使用这些函数更符合R的程序设计风格，
使程序变得简洁，
当然，
程序更简洁并不等同于程序更容易理解，
要理解这样的程序，
需要更多学习与实践。
purrr扩展包提供了apply类函数的现代版本，
风格更为统一，
更容易学习和使用。
参见[25](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals.html#functionals)。

### 18.3.1 `replicate()`函数

`replicate()`函数用来执行某段程序若干次，
类似于`for()`循环但是没有计数变量。
常用于随机模拟。
`replicate()`的缺省设置会把重复结果尽可能整齐排列成一个多维数组输出。

语法为

> `replicate`(重复次数, 要重复的表达式)

其中的表达式可以是复合语句,
也可以是执行一次模拟的函数。

下面举一个简单模拟例子。
设总体\(X\)为\(\text{N}(0, 1)\), 取样本量\(n=5\)，
重复地生成模拟样本共\(B=6\)组，
输出每组样本的样本均值和样本标准差。
模拟可以用如下的`replicate()`实现：

```
set.seed(1)
replicate(6, {
  x <- rnorm(5, 0, 1); 
  c(mean(x), sd(x)) })
```

```
##           [,1]      [,2]       [,3]      [,4]       [,5]       [,6]
## [1,] 0.1292699 0.1351357 0.03812297 0.4595670 0.08123054 -0.3485770
## [2,] 0.9610394 0.6688342 1.49887443 0.4648177 1.20109623  0.7046822
```

结果是一个矩阵，矩阵行数与每次模拟的结果（均值、标准差）个数相同，
这里第一行都是均值，第二行都是标准差；
矩阵每一列对应于一次模拟。
这样结果存储应该是来源于R的数组元素次序为列优先次序。

## 18.4 避免制作副本

类似于`x <- c(x, y)`这样的累积结果每次运行都会制作一个`x`的副本，
在`x`存储量较大或者重复修改次数很多时会减慢程序。

**例18.4** 执行10000次模拟，
每次模拟求10个U(0,1)均匀随机数的极差，
保存每次的极差，
求平均极差。

用`x <- c(x, y)`这样的写法：

```
range_f1 <- function(M = 10000){
  x <- c()
  for(i in seq(M)){
    x <- c(x, diff(range(runif(10))))
  }
  mean(x)
}
```

上面的程序不仅是低效率的做法，
也没有预先精心设计用来保存结果的数据结构。
数据建模或随机模拟的程序都应该事先分配好用来保存结果的数据结构，
在每次循环中填入相应结果。如：

```
range_f2 <- function(M = 10000){
  x <- numeric(M)
  for(i in seq_along(x)){
    x[[i]] <- diff(range(runif(10)))
  }
  mean(x)
}
```

这样的程序结构更清晰，
效率更高，
而且循环次数越多，
比`x <- c(x, ...)`这样的做法的优势越大。

效率比较：

```
bench::mark(
  range_f1(10000),
  range_f2(10000),
  check=FALSE)
```

按中位时间比较，
第一个版本运行0.351秒，第二个版本运行0.084秒，
速度相差3倍多。

**例18.5** 在循环内修改数据框的值也会制作数据框副本，
当数据框很大或者循环次数很多时会使得程序很慢。
下面给出示例。

在循环内修改数据框变量的做法：

```
df_copy_f1 <- function(m=10000, n=100){
  set.seed(101)
  x <- matrix(runif(n*m), n, m) |>
    as.data.frame()
  for(j in seq(m)){
    x[[j]] <- x[[j]] + 1
  }
  
  x
}
```

在循环内修改列表元素就不会制作副本，
从而可以避免这样的效率问题，如：

```
df_copy_f2 <- function(m=10000, n=100){
  set.seed(101)
  x <- replicate(m, 
    runif(n),
    simplify=FALSE)
  for(j in seq(m)){
    x[[j]] <- x[[j]] + 1
  }
  
  as.data.frame(x)
}
```

`replicate()`函数中用`simplify=FALSE`使结果总是返回列表。
要注意的是，
上面第二个程序中的`as.data.frame(x)`也是效率较差的。
将数据保存在列表中比保存在数据框中访问效率高，
但数据框提供的功能更丰富。

效率比较：

```
bench::mark(
  df_copy_f1(),
  df_copy_f2(),
  check = FALSE)
```

以中位时间比较，
第一个版本耗时1.84秒，第二个版本耗时0.784秒，
效率相差一倍以上。

## 18.5 用性能分析工具找到程序效率的瓶颈

R的性能分析工具可以按较高频率（如每隔几毫秒）抽查正在被调用的函数，
因为函数可以嵌套调用，
所以会记录下来正在嵌套调用的各个函数。
因为抽查的速度很快，
所以会得到大量的被调用函数的抽样数据，
这样就可以进行概括，
得知哪些函数被调用最频繁，
调用的途径是怎样的。
基本R的`utils::Rprof()`可以收集程序运行的性能分析用数据，
而`utils::summaryRprof()`则可以用文本格式提供性能分析的概括。

在RStudio软件中，
借助于profvis扩展包，
我们可以从Profile菜单对选定行进行性能分析。
但是，
对比较复杂的程序，
应该将要分析的程序保存为一个R源文件，
并用`source()`的方法将要运行的函数载入，
并用`profvis()`函数调用该函数并显示运行后性能分析结果。

**例18.6** 以[18.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-prof.html#exm:p-prof-copy-cmem)的程序为例。
我们将第一个程序包装为一个函数`bad_copy()`并存入文件`bad_copy.R`中，
然后用profvis进行性能分析。

`bad_copy.R`文件内容为：

```
bad_copy <- function(){
  M <- 1E5
  x <- c()
  for(i in seq(M)){
    x <- c(x, diff(range(runif(10))))
  }
  mean(x)
}
```

安装好profvis扩展包后，运行如下的性能分析程序：

```
library(profvis)
profvis(bad_copy())
```

结果在RStudio中将显示如下的两个窗格内容：

p-prof-profiler-bc01.png

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/p-prof-profiler-bc01.png)![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/p-prof-profiler-bc02.png)

在Flame Graph窗格中，
下方显示了函数调用所占用的时间，
当鼠标光标移动到下方的某一个函数调用的色块时可以显示该函数的调用路径和耗时等信息。
可以看出是`c()`函数和`GC()`函数占用了绝大多数时间，
而`GC()`是被`c()`调用的。
反复调用内存垃圾回收函数`GC()`一般说明程序有反复分配内存的问题，
从该图上半部分的Memery图表和Time图表也可以看出这一点，
Memery图表中左侧为释放内存量，
右侧为分配内存量；
Time图表看出程序运行集中在`x`被重复赋值的那一行。

在Data窗格中，
点击Profvis后才展开详细的汇总统计图表。
可以看出耗时最多的是`c()`函数调用，
但要注意的是，
这里的耗时也包括`c()`函数内部的`diff()`, `range()`, `runif()`函数的调用时间。

## 18.6 R的计算函数

R中提供了大量的数学函数、统计函数和特殊函数，
可以打开R的HTML帮助页面， 进入“Search Enging & Keywords”链接，
查看其中与算术、数学、优化、线性代数等有关的专题。

这里简单列出一部分常用函数，
并对数学计算中常用函数`filter`, `fft`,
`convolve`进行较详细说明。

### 18.6.1 数学函数

常用数学函数包括`abs`, `sign`, `log`, `log10`, `sqrt`, `exp`,
`sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `atan2`,
`sinh`, `cosh`, `tanh`。
还有`gamma`, `lgamma`(伽玛函数的自然对数)。

用于取整的函数有`ceiling`, `floor`, `round`, `trunc`, `signif`,
`as.integer`等。
这些函数是向量化的一元函数。

`choose(n,k)`返回从\(n\)中取\(k\)的组合数。
`factorial(x)`返回\(x!\)结果。
`combn(x,m)`返回从集合\(x\)中每次取出不计次序的\(m\)个的所有不同取法，
结果为一个矩阵，矩阵每列为一种取法的\(m\)个元素值。

### 18.6.2 概括函数

`sum`对向量求和, `prod`求乘积。

`cumsum`和`cumprod`计算累计， 得到和输入等长的向量结果。

`diff`计算前后两项的差分（后一项减去前一项）。

`mean`计算均值，`var`计算样本方差或协方差矩阵， `sd`计算样本标准差,
`median`计算中位数， `quantile`计算样本分位数。
`cor`计算相关系数。

`colSums`, `colMeans`, `rowSums`, `rowMeans`对矩阵的每列或每行计算总和或者平均值，
效率比用`apply`函数要高。

`rle`和`inverse.rle`用来计算数列中“连”长度及其逆向恢复，
“连”经常用在统计学的随机性检验中。

### 18.6.3 最值

`max`和`min`求最大和最小， `cummax`和`cummin`累进计算。

`range`返回最小值和最大值两个元素。

对于`max`, `min`, `range`，
如果有多个自变量可以把这些自变量连接起来后计算。

`pmax(x1,x2,...)`对若干个等长向量计算对应元素的最大值，
不等长时短的被重复使用。 `pmin`类似。
比如，`pmax(0, pmin(1,x))`把`x`限制到\([0,1]\)内。

### 18.6.4 排序

`sort`返回排序结果。 可以用`decreasing=TRUE`选项进行降序排序。
`sort`可以有一个`partial=`选项，
这样只保证结果中`partial=`指定的下标位置是正确的。 比如:

```
sort(c(3,1,4,2,5), partial=3)
```

```
## [1] 2 1 3 4 5
```

只保证结果的第三个元素正确。 可以用来计算样本分位数估计。

在`sort()`中用选项`na.last`指定缺失值的处理， 取`NA`则删去缺失值，
取`TRUE`则把缺失值排在最后面， 取`FALSE`则把缺失值排在最前面。

`order`返回排序用的下标序列, 它可以有多个自变量，
按这些自变量的字典序排序。
可以用`decreasing=TRUE`选项进行降序排序。
如果指定选项`method="radix"`,
则`decreasing`选项可以输入一个逻辑向量，
对每个参与排序的自变量分别指定是否降序排列。

`sort.list`函数与`order`功能相同但只支持按一个变量排序。

`rank`计算秩统计量，可以用`ties.method`指定同名次处理方法，
如`ties.method=min`取最小秩。

`order`, `sort.list`, `rank`也可以有
`na.last`选项，只能为`TRUE`或`FALSE`。

`unique()`返回去掉重复元素的结果，
`duplicated()`对每个元素用一个逻辑值表示是否与前面某个元素重复。
如

```
unique(c(1,2,2,3,1))
```

```
## [1] 1 2 3
```

```
duplicated(c(1,2,2,3,1))
```

```
## [1] FALSE FALSE  TRUE FALSE  TRUE
```

`rev`反转序列。

### 18.6.5 一元定积分`integrate`

`integrate(f, lower, upper)`对一元函数`f`计算从`lower`到`upper`的定积分。
使用自适应算法保证精度。
如：

```
integrate(sin, 0, pi)
## 2 with absolute error < 2.2e-14
```

函数的返回值不仅仅包含定积分数值，
还包含精度等信息。

允许使用`-Inf`和`Inf`作为积分边界，如：

```
integrate(dnorm, -Inf, Inf)
## 1 with absolute error < 9.4e-05
```

### 18.6.6 一元函数求根`uniroot`

`uniroot(f, interval)`对函数`f`在给定区间内求一个根，
`interval`为区间的两个端点。
要求`f`在两个区间端点的值异号。
即使有多个根也只能给出一个。
如\(x(x-1)(x+1)\)函数在\([-2,2]\)的根：

```
uniroot(\(x) x*(x-1)*(x+1), c(-2, 2))
```

```
## $root
## [1] 0
## 
## $f.root
## [1] 0
## 
## $iter
## [1] 1
## 
## $init.it
## [1] NA
## 
## $estim.prec
## [1] 2
```

对于多项式，
可以用`polyroot`函数求出所有的复根，
输入为升幂排列的多项式系数组成的向量。
如多项式\(x^3 - x\)和\(x^2 + 1\)的所有复根：

```
polyroot(c(0, -1, 0, 1))
## [1]  0+0i  1+0i -1+0i
polyroot(c(1, 0, 1))
## [1] 0+1i 0-1i
```

### 18.6.7 离散傅立叶变换`fft`

R中`fft`函数使用快速傅立叶变换算法计算离散傅立叶变换。
设`x`为长度`n`的向量， `y=fft(x)`，则

```
y[k] = sum(x * complex(
    argument = -2*pi * (0:(n-1)) * (k-1)/n))
```

即
\[
\begin{aligned}
y\_{k+1} =& \sum\_{j=0}^{n-1} x\_{j+1} \exp\left(-i 2\pi \frac{k j}{n} \right),
\ k=0,1,\dots,n-1.
\end{aligned}
\]
注意没有除以\(n\)，结果是复数向量。

另外，若`y=fft(x)`, `z=fft(y, inverse=TRUE)`,
则 `x == z/length(x)`。

快速傅立叶变换是数值计算中十分常用的工具，
R软件包fftw可以进行优化的快速傅立叶变换。

### 18.6.8 用`filter`函数作迭代

R在遇到向量自身迭代时很难用向量化编程解决，
`stats::filter`函数可以解决其中部分问题。
`stats::filter`函数可以进行卷积型或自回归型的迭代。 语法为

```
stats::filter(x, filter, 
  method = c("convolution", "recursive"),
  sides=2, circular =FALSE, init)
```

下面用例子演示此函数的用途。

#### 18.6.8.1 示例1：双侧滤波

对输入序列\(x\_t, t=1,2,\dots,n\)， 希望进行如下滤波计算:
\[
\begin{aligned}
y\_{t} = \sum\_{j=-k}^k a\_j x\_{t-j},
\ k+1 \leq t \leq n-k-1,
\end{aligned}
\]
其中\((a\_{-k}, \dots, a\_0, \dots, a\_{k})\)是长度为\(2k+1\)的向量。
注意公式中\(a\_j\)与\(x\_{t-j}\)对应。

假设\(x\)保存在向量x中，
\((a\_{-k}, \dots, a\_0, \dots, a\_{k})\)保存在向量f中，
\(y\_{k+1}, \dots, y\_{n-k}\)保存在向量y中, 无定义部分取NA, 程序可以写成

```
y <- filter(x, f, method="convolution", sides=2)
```

比如，设\(x = (1, 3, 7, 12, 17, 23)\),
\((a\_{-1}, a\_0, a\_{1}) = (0.1, 0.5, 0.4)\), 则
\[
\begin{aligned}
y\_t = 0.1 \times x\_{t+1} + 0.5 \times x\_t + 0.4 \times x\_{t-1},
\ t=2,3,\dots,5
\end{aligned}
\]
用`filter()`函数计算，程序为：

```
y <- stats::filter(
  c(1,3,7,12,17,23), 
  c(0.1, 0.5, 0.4),
  method="convolution", sides=2)
y
## Time Series:
## Start = 1 
## End = 6 
## Frequency = 1 
## [1]   NA  2.6  5.9 10.5 15.6   NA
```

`filter`的结果为一个时间序列对象，
可以用`as.vector()`函数转换为普通数值型向量。

#### 18.6.8.2 示例2: 单侧滤波

对输入序列\(x\_t, t=1,2,\dots,n\)， 希望进行如下滤波计算:
\[
\begin{aligned}
y\_{t} = \sum\_{j=0}^k a\_j x\_{t-j},
\ k+1 \leq t \leq n,
\end{aligned}
\]
其中\((a\_0, \dots, a\_{k})\)是长度为\(k+1\)的向量。
注意公式中\(a\_j\)与\(x\_{t-j}\)对应。

假设\(x\)保存在向量x中，
\((a\_0, \dots, a\_{k})\)保存在向量f中，
\(y\_{k+1}, \dots, y\_{n}\)保存在向量y中,
无定义部分取NA, 程序可以写成

```
y <- filter(x, f, method="convolution", sides=1)
```

比如，设\(x = (1, 3, 7, 12, 17, 23)\),
\((a\_{0}, a\_1, a\_{2}) = (0.1, 0.5, 0.4)\), 则
\[
\begin{aligned}
y\_t = 0.1 \times x\_{t} + 0.5 \times x\_{t-1} + 0.4 \times x\_{t-2},
\ t=3,4,\dots,6
\end{aligned}
\]
程序为

```
y <- stats::filter(
  c(1,3,7,12,17,23), 
  c(0.1, 0.5, 0.4),
  method="convolution", sides=1)
y
## Time Series:
## Start = 1 
## End = 6 
## Frequency = 1 
## [1]   NA   NA  2.6  5.9 10.5 15.6
```

#### 18.6.8.3 示例3: 自回归迭代

设输入\(e\_t, t=1,2,\dots, n\)，
要计算
\[
\begin{aligned}
y\_t = \sum\_{j=1}^k a\_j y\_{t-j} + e\_t,
\ t=1,2,\dots,n,
\end{aligned}
\]
其中\((a\_1, \dots, a\_k)\)是\(k\)个实数，
\((y\_{-k+1}, \dots, y\_0)\)已知。

设\(x\)保存在向量`x`中，\((a\_1, \dots, a\_k)\)保存在向量`a`中，
\((y\_1, \dots, y\_n)\)保存在向量y中。

如果\((y\_{-k+1}, \dots, y\_0)\)都等于零，
可以用如下程序计算\(y\_1, y\_2, \dots, y\_n\):

```
stats::filter(x, a, method="recursive")
```

如果\((y\_0, \dots, y\_{-k+1})\)保存在向量b中(注意与时间顺序相反)，
可以用如下程序计算\(y\_1, y\_2, \dots, y\_n\):

```
stats::filter(x, a, method="recursive", init=b)
```

比如，
设\(e = (0.1, -0.2, -0.1, 0.2, 0.3, -0.2)\),
\((a\_1, a\_2) = (0.9, 0.1)\),
\(y\_{-1}=y\_0=0\), 则
\[
\begin{aligned}
y\_t = 0.9 \times y\_{t-1} + 0.1 \times y\_{t-2} + e\_t,
\ t=1,2,\dots,6
\end{aligned}
\]
迭代程序和结果为

```
y <- stats::filter(
  c(0.1, -0.2, -0.1, 0.2, 0.3, -0.2),
  c(0.9, 0.1), method="recursive")
print(y, digits=3)
## Time Series:
## Start = 1 
## End = 6 
## Frequency = 1 
## [1]  0.1000 -0.1100 -0.1890  0.0189  0.2981  0.0702
```

这个例子中，
如果已知\(y\_{0}=200, y\_{-1}=100\),
迭代程序和结果为：

```
y <- stats::filter(
  c(0.1, -0.2, -0.1, 0.2, 0.3, -0.2),
  c(0.9, 0.1), init=c(200, 100),
  method="recursive")
print(y, digits=6)
## Time Series:
## Start = 1 
## End = 6 
## Frequency = 1 
## [1] 190.100 190.890 190.711 190.929 191.207 190.979
```

## 18.7 并行计算

现代桌面电脑和笔记本电脑的CPU通常有多个核心或虚拟核心（线程），
如2核心或4虚拟核心。
通常R运行并不能利用全部的CPU能力，
仅能利用其中的一个虚拟核心。
使用特制的BLAS库（非R原有）可以并发运行多个线程，
一些R扩展包也可以利用多个线程。
利用多台计算机、多个CPU、CPU中的多核心和多线程同时完成一个计算任务称为并行计算。

想要充分利用多个电脑、多个CPU和CPU内的虚拟核心，
技术上比较复杂，
涉及到计算机之间与进程之间的通讯问题，
在要交流的数据量比较大时会造成并行计算的瓶颈。

实际上，
有些问题可以很容易地进行简单地并行计算。
比如，
在一个统计研究中，
需要对100组参数组合进行模拟，
评估不同参数组合下模型的性能。
假设研究人员有两台安装了R软件的计算机，
就可以在两台计算机上进行各自50组参数组合的模拟，
最后汇总在一起就可以了。

R的parallel包提供了一种比较简单的利用CPU多核心的功能，
思路与上面的例子类似，
如果有多个任务互相之间没有互相依赖，
就可以分解到多个计算机、多个CPU、多个虚拟核心中并行计算。
最简单的情形是一台具有单个CPU、多个虚拟核心的台式电脑或者笔记本电脑。
但是，
统计计算中最常见耗时计算任务是随机模拟，
随机模拟要设法避免不同进程的随机数序列的重复可能，
以及同一进程中不同线程的随机数序列的重复可能。

parallel包提供了`parLapply()`、`parSapply()`、`parApply()`函数，
作为`lapply()`、`sapply()`、`apply()`函数的并行版本，
与非并行版本相比，
需要用一个临时集群对象作为第一自变量。

并行计算的工具在不断进步，
这部分内容的做法具有一定的参考价值，
但所用工具不一定是最新的，
欢迎读者批评指正。

### 18.7.1 例1：完全不互相依赖的并行运算

考虑如下计算问题:
\[
S\_{k,n} = \sum\_{i=1}^n \frac{1}{i^k}
\]

下面的程序取`n`为一百万，`k`为2到21，循环地用单线程计算。

```
f10 <- function(k=2, n=1000){
  s <- 0.0
  for(i in seq(n)) s <- s + 1/i^k
  s
}
f11 <- function(n=1000000){
  nk <- 20
  v <- sapply(2:(nk+1), function(k) f10(k, n))
  v
}
system.time(f11())[3]
## elapsed
##    1.19
```

因为对不同的`k`，
`f0(k)`计算互相不依赖，
也不涉及到随机数序列，
所以可以简单地并行计算而没有任何风险。
先查看本计算机的虚拟核心（线程）数：

```
library(parallel)
detectCores()
## [1] 8
```

用`makeCluster()`建立临时的有8个节点的单机集群：

```
nNodes <- 8
cpucl <- makeCluster(nNodes)
```

用`parSapply()`或者`parLapply()`关于\(k\)并行地循环：

```
f12 <- function(n=1000000){
  f10 <- function(k=2, n=1000){
    s <- 0.0
    for(i in seq(n)) s <- s + 1/i^k
    s
  }

  nk <- 20
  v <- parSapply(cpucl, 2:(nk+1), function(k) f10(k, n))
  v
}
system.time(f12())[3]
## elapsed
##    0.36
```

并行版本速度提高了2倍多。

并行执行结束后，
需要解散临时的集群，
否则可能会有内存泄漏：

```
stopCluster(cpucl)
```

注意并行版本的程序还需要一些在每个计算节点上的初始化，
比如调入扩展包，定义函数，
初始化不同的随机数序列等。
parallel包的并行执行用的是不同的进程，
所以传送给每个节点的计算函数要包括所有的依赖内容。
比如，`f12()`中内嵌了`f10()`的定义，
如果不将`f10()`定义在`f12()`内部，
就需要预先将`f10()`的定义传递给每个节点。

parallel包的`clusterExport()`函数可以用来把计算所依赖的对象预先传送到每个节点。
比如，
上面的`f12()`可以不包含`f10()`的定义，
而是用`clusterExport()`预先传递：

```
cpucl <- makeCluster(nNodes)
clusterExport(cpucl, c("f10"))
f13 <- function(n=1000000){
  nk <- 20
  v <- parSapply(cpucl, 2:(nk+1), function(k) f10(k, n))
  v
}
system.time(f13())[3]
## elapsed 
##   1.16 
stopCluster(cpucl)
```

如果需要在每个节点预先执行一些语句，
可以用`clusterEvalQ()`函数执行，如

```
clusterEvalQ(cpucl, library(dplyr))
```

### 18.7.2 例2：使用相同随机数序列的并行计算

为了估计总体中某个比例\(p\)的置信区间，
调查了一组样本，
在\(n\)个受访者中选“是”的比例为\(\hat p\)。
令\(\lambda\)为标准正态分布的双侧\(\alpha\)分位数，
参数\(p\)的近似\(1-\alpha\)置信区间为
\[
\frac{\hat p + \frac{\lambda^2}{2n}}{1 + \frac{\lambda^2}{n}}
\pm
\frac{\lambda}{\sqrt{n}}
\frac{\sqrt{\hat p (1 - \hat p) + \frac{\lambda^2}{4n}}}
{1 + \frac{\lambda^2}{n}}
\]
称为Wilson置信区间。

假设要估计不同\(1-\alpha\), \(n\), \(p\)情况下，
置信区间的覆盖率（即置信区间包含真实参数\(p\)的概率）。
可以将这些参数组合定义成一个列表，
列表中每一项是一种参数组合，
对每一组合分别进行随机模拟，估计覆盖率。
因为不同参数组合之间没有互相依赖的关系，
随机数序列完全可以使用同一个序列。

不并行计算的程序示例：

```
wilson <- function(n, x, conf){
  hatp <- x/n
  lam <- qnorm((conf+1)/2)
  lam2 <- lam^2 / n
  p1 <- (hatp + lam2/2)/(1 + lam2)
  delta <- lam / sqrt(n) * sqrt(hatp*(1-hatp) + lam2/4) / (1 + lam2)
  c(p1-delta, p1+delta)
}
f20 <- function(cpar){
  set.seed(101)
  conf <- cpar[1]
  n <- cpar[2]
  p0 <- cpar[3]
  nsim <- 100000
  cover <- 0
  for(i in seq(nsim)){
    x <- rbinom(1, n, p0)
    cf <- wilson(n, x, conf)
    if(p0 >= cf[1] && p0 <= cf[2]) cover <- cover+1
  }
  cover/nsim
}
f21 <- function(){
  lp <- expand.grid(c(0.8, 0.9), c(30, 100), c(0.5, 0.1)) |> 
    as.matrix() |> 
    t() |> 
    as.data.frame() |> 
    as.list()
  res <- sapply(lp, f20)
  res
}
system.time(f21())[3]
## elapsed 
##   6.52
```

约运行6.52秒。

改为并行版本：

```
library(parallel)
nNodes <- 8
cpucl <- makeCluster(nNodes)
clusterExport(cpucl, c("f20", "wilson"))
f22 <- function(){
  lp <- expand.grid(c(0.8, 0.9), c(30, 100), c(0.5, 0.1)) |> 
    as.matrix() |> 
    t() |> 
    as.data.frame() |> 
    as.list()
  res <- parSapply(cpucl, lp, f20)
  res
}
system.time(f22())[3]
## elapsed 
##   2.83 
stopCluster(cpucl)
```

运行约2.83秒，
速度提高一倍以上。
这里模拟了8种参数组合，
每种参数组合模拟了十万次，
每种参数组合模拟所用的随机数序列是相同的。

### 18.7.3 例3：使用独立随机数序列的并行计算

大量的耗时的统计计算是随机模拟，
有时需要并行计算的部分必须使用独立的随机数序列。
比如，需要进行一千万次重复模拟，每次使用不同的随机数序列，
可以将其分解为10组模拟，每组模拟一百万次，
这就要求这10组模拟使用的随机数序列不重复。

R中实现了`L'Ecuyer`的多步迭代复合随机数发生器，
此随机数发生器周期很长，
而且很容易将发生器的状态前进指定的步数。
parallel包的`nextRNGStream()`函数可以将该发生器前进到下一段的开始，
每一段都足够长，
可以用于一个节点。

以Wilson置信区间的模拟为例。
设\(n=30\), \(p=0.01\),
\(1-\alpha=0.95\)，
取重复模拟次数为1千万次，估计Wilson置信区间的覆盖率。
单线程版本为：

```
f31 <- function(nsim=1E7){
  set.seed(101)
  n <- 30; p0 <- 0.01; conf <- 0.95
  cover <- 0
  for(i in seq(nsim)){
    x <- rbinom(1, n, p0)
    cf <- wilson(n, x, conf)
    if(p0 >= cf[1] && p0 <= cf[2]) cover <- cover+1
  }
  cover/nsim
}
system.time(cvg1 <- f31())[3]
## elapsed 
##  78.11
```

单线程版本运行了大约78秒。

改成并行版本。
比例2多出的部分是为每个节点分别计算一个随机数种子将不同的种子传给不同节点。
parallel包的`clusterApply()`函数为临时集群的每个节点分别执行同一函数，
但对每个节点分别使用列表的不同元素作为函数的自变量。

```
library(parallel)
nNodes <- 8
cpucl <- makeCluster(nNodes)
each.seed <- function(s){
  assign(".Random.seed", s, envir = .GlobalEnv)
}
RNGkind("L'Ecuyer-CMRG")
set.seed(101)
seed0 <- .Random.seed
seeds <- as.list(1:nNodes)
for(i in 1:nNodes){ # 给每个节点制作不同的种子
  seed0 <- nextRNGStream(seed0)
  seeds[[i]] <- seed0
}
## 给每个节点传送不同种子：
junk <- clusterApply(cpucl, seeds, each.seed)

f32 <- function(isim, nsimsub=10000){
  n <- 30; p0 <- 0.01; conf <- 0.95
  cover <- 0
  for(i in seq(nsimsub)){
    x <- rbinom(1, n, p0)
    cf <- wilson(n, x, conf)
    if(p0 >= cf[1] && p0 <= cf[2]) cover <- cover+1
  }
  cover
}
clusterExport(cpucl, c("f32", "wilson"))

f33 <- function(nsim=1E7){
  nbatch <- 40
  nsimsub <- nsim / nbatch
  cvs <- parSapply(cpucl, 1:nbatch, f32, nsimsub=nsimsub)
  sum(cvs)/(nsim*nbatch)
}

system.time(cvg2 <- f33())[3]
## elapsed 
##  30.72
stopCluster(cpucl)
```

并行版本运行了大约41秒，速度提高一倍以上。
从两个版本各自一千万次重复模拟结果来看，
用随机模拟方法得到的覆盖率估计的精度大约为3位有效数字。

### 18.7.4 其它并行计算扩展包

更大规模的随机模拟问题，
可以考虑使用多CPU的计算工作站或者服务器，
或用多台计算机通过高速局域网组成并行计算集群。

还有一种选择是租用云计算服务。

[clustermq](https://mschubert.github.io/clustermq/)扩展包支持现有的各种集群调度软件，
也可以在单个电脑上使用多个进程或者多个线程并行计算。

[future](https://future.futureverse.org/)扩展包扩展了R语言的“期货”功能，
期货是一个确保可以求值、但当前不一定有值的变量，
可以异步并行地对期货求值。

[foreach](https://github.com/RevolutionAnalytics/foreach)扩展包可以利用R的parallel包或其它并行计算扩展包进行列表元素遍历。

[targets](https://books.ropensci.org/targets/)扩展包用于管理数据科学项目的多个任务之间的依赖关系，
也支持并行处理多个任务，
可以将大任务分解为多个小任务并行处理。

### 18.7.5 用foreach重算例1

foreach包可以并行遍历列表元素，
它需要一个并行计算后端，
比如，
可以用doParallel包调用R的parallel包的并行计算功能。
以前面的例1为例。

```
library(foreach)
library(doParallel)
f12fe <- function(n=1000000){
  f10 <- function(k=2, n=1000){
    s <- 0.0
    for(i in seq(n)) s <- s + 1/i^k
    s
  }
  
  nk <- 20
  data_list <- 2:(nk+1)
  foreach(k = data_list) %dopar% {
    f10(k, n)
  }
}
cl <- makeCluster(8)
registerDoParallel(cl)
system.time(f12fe())[3]
stopCluster(cl)
```