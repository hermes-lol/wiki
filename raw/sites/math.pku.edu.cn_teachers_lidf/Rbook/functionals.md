---
crawl_time: '2026-01-17 14:20:47'
framework: rbook
title: 25 函数式编程和数据框列表列 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 25 函数式编程和数据框列表列

## 25.1 函数式编程介绍

R支持类(class)和方法(method)，
实际提供了适用于多种自变量的通用函数(generic function，或称泛型函数)，
不同自变量类型调用该类特有的方法， 但函数名可以保持不变。
这可以支持一定的面向对象编程方式。

R也支持函数式编程，
但不是专门的函数式编程语言。
R语言的设计主要用函数求值来进行运算；
R的用户主要使用函数调用来访问R的功能。

按照函数式编程的要求，
函数应该是“第一级对象”，
可以将函数对象绑定到变量名上面，
可以在列表等结构中保存多个函数，
可以在函数内定义函数，
可以用函数作为函数的自变量，
R函数满足这样的要求。

函数式编程的目的是提供可理解、可证明正确的软件。
R虽然带有函数式编程语言特点，
但并不强求使用函数式编程规范。
典型的函数式编程语言如Haskel, Lisp的运行与R的显式的、顺序的执行方式相差很大。

### 25.1.1 纯函数

函数式编程要求每个函数必须功能清晰、定义确切，
最好是所谓“纯函数”。
R并不是专门的函数式编程语言，
专门的函数式编程语言提供了定义纯函数的功能。
纯函数需要满足如下条件：

* 没有副作用。调用一个函数对后续运算没有影响，
  不管是再次调用此函数还是调用其它函数。
  这样，用全局变量在函数之间传递信息就是不允许的。
  其它副作用包括写文件、打印、绘图等，
  这样的副作用对函数式要求破坏不大。
  函数返回值包含了函数执行的所有效果。
* 不受外部影响。函数返回值只依赖于其自变量及函数的定义。
  函数定义仅由对所有可能的自变量值确定返回值来确定，
  不依赖于任何外部信息（也就不能依赖于全局变量与系统设置值）。
  在专用的函数式编程语言中，
  函数定义返回值的方式是隐含地遍历所有可能的参数值给出返回值，
  而不是用过程式的计算来修改对象的值。
* 不受赋值影响。
  函数定义不需要反复对内部对象（所谓“状态变量”）赋值或修改。

R的函数一般不能修改实参的值，
这有助于实现纯函数的要求。
但是，如果多个函数之间用全局变量传递信息，
就不能算是纯函数。
像`options()`函数这样修改全局运行环境的功能会破坏函数式要求。
尽可能让自己的函数不依赖于`options()`中的参数。

如果函数对相同的输入可以有不同的输出当然不是纯函数，
例如R中的随机数函数(`sample()`, `runif()`, `rnorm`等)。

与具体硬件、软件环境有关的一些因素也破坏纯函数要求，
如不同的硬件常数、精度等。
调用操作系统的功能对函数式要求破坏较大。
减少赋值主要需要减少循环，可以用R的向量化方法解决。

一个R函数是否满足纯函数要求不仅要看函数本身，
还要看函数内部调用的其它函数是否纯函数。

R不是专用的函数式编程语言，
但可以采用函数式编程的范式，
将大多数函数写成纯函数，
将有副作用的单独写在少数的函数中。

### 25.1.2 副作用和运行环境恢复

如果函数除了输出之外还在其它方面影响了运行环境，
这样的函数就不是纯函数。
所有画图函数(`plot`等)、输出函数(`cat`, `print`, `save`等)都是这样的函数。
这些对运行环境的改变叫做**副作用**（side effects）。
又比如，`library()`函数会引入新的函数和变量，
`setwd()`, `Sys.setenv()`, `Sys.setlocale()`会改变R运行环境，
`options()`, `par()`会改变R全局设置。
自定义R函数中如果调用了非纯函数也就变成了非纯函数。
编程中要尽量控制副作用而且要意识到副作用的影响，
尤其是全局设置与全局变量的影响。

有些函数不可避免地要修改运行环境，
比如当前工作目录(用`setwd()`)、R运行选项(用`options()`)、绘图参数(用`par()`)等，
如果可能的话，
在函数结束运行前，
应该恢复对运行环境的修改。
为此，可以在函数体的前面部分调用`on.exit()`函数，
此函数的参数是在函数退出前要执行的表达式或复合表达式。
此函数可以多次调用，
一般做法是函数体内部每次更改运行环境处调用一次`on.exit()`，
预先设定退出前恢复。
这需要使用`add=TRUE`选项。

例如，
绘图的函数中经常需要用`par()`修改绘图参数，
这会使得后续程序出错。
为此，可以在函数开头保存原始的绘图参数，
函数结束时恢复到原始的绘图参数。
如

```
f <- function(){
  opar <- par(mfrow=c(1,2))
  on.exit(par(opar), add=TRUE)
  plot((-10):10)
  plot((-10):10, ((-10):10)^2)
}
f()
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals_files/figure-html/prog-func63-1.png)

如果需要指定越晚添加的恢复动作越先执行，
在`on.exit()`中还要加上`after=FALSE`选项。

### 25.1.3 R的函数式编程功能

R语言不是专用的函数式编程语言，
但支持使用函数式编程的一些常见做法。
R函数是第一级对象，
支持内嵌函数，
并可以输入函数作为函数的自变量，
称这样的函数为**泛函**(functionals)，
如`lapply`类函数；
可以输出函数作为函数结果，
称这样的函数为**函数工厂**；
可以输入函数，
进行一定修改后输出函数，
称这样的函数为**函数算子**(function operators)。
这些功能都为函数式编程风格提供了有力的支持。

利用R的purrr扩展包，
可以用统一的风格进行函数式编程，
比基本R的lapply类函数、Map、Reduce等更容易使用。

下面讲解泛函、函数工厂和函数算子。

## 25.2 泛函

许多函数需要用函数作为参数，称这样的函数为**泛函**(functionals)。
典型的泛函是`lapply`类函数。
这样的函数具有很好的通用性，
因为需要进行的操作可以输入一个函数来规定，
用输入的函数规定要进行什么样的操作。

### 25.2.1 `purrr::map`函数

设我们要对列表或向量`x`的每个元素`x[[i]]`调用函数`f()`，
将结果保存成一个列表。
这样做的一个程序框架是：

```
y <- vector(mode = "list", length = length(x))
for(i in seq_along(x)){
  y[[i]] <- f(x[[i]])
}
names(y) <- names(x)
```

其中的输入`x`是任意的，
函数`f`是任意的。
`purrr`包的`map()`函数可以用一条命令完成上述任务：

```
y <- map(x, f)
```

这个函数与基本R的`lapply`功能基本相同，
对数据框每列、列表每项进行计算或操作时最为适用。

#### 25.2.1.1 `map`数据框处理示例

下面举例说明map函数对数据框处理的应用。

`typeof()`函数求变量的存储类型，如

```
typeof(d.class[["height"]])
```

```
## [1] "double"
```

这里`d.class`是数据框，
数据框也是列表，
每个列表元素是数据框的一列。

如下程序使用`purrr::map()`求每一列的存储类型，
map的结果总是列表，每个列表元素对应于输入的一个元素，
如:

```
library(purrr)
map(d.class, typeof)
```

```
## $name
## [1] "character"
## 
## $sex
## [1] "integer"
## 
## $age
## [1] "double"
## 
## $height
## [1] "double"
## 
## $weight
## [1] "double"
```

当结果比较简单时，
保存为列表不够方便，
函数`unlist()`可以将比较简单的列表转换为基本类型的向量，如：

```
map(d.class, typeof) |> unlist()
```

```
##        name         sex         age      height      weight 
## "character"   "integer"    "double"    "double"    "double"
```

实际上，关于一个数据框的结构，
用`str()`函数或者`dplyr::glimpse()`函数可以得到更为详细的信息：

```
str(d.class)
```

```
## spc_tbl_ [19 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ name  : chr [1:19] "Alice" "Becka" "Gail" "Karen" ...
##  $ sex   : Factor w/ 2 levels "M","F": 2 2 2 2 2 2 2 2 2 1 ...
##  $ age   : num [1:19] 13 13 14 12 12 15 11 15 14 14 ...
##  $ height: num [1:19] 56.5 65.3 64.3 56.3 59.8 66.5 51.3 62.5 62.8 69 ...
##  $ weight: num [1:19] 84 98 90 77 84.5 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   .default = col_double(),
##   ..   name = col_character(),
##   ..   sex = col_factor(levels = c("M", "F"), ordered = FALSE, include_na = FALSE),
##   ..   age = col_double(),
##   ..   height = col_double(),
##   ..   weight = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

```
dplyr::glimpse(d.class)
```

```
## Rows: 19
## Columns: 5
## $ name   <chr> "Alice", "Becka", "Gail", "Karen", "Kathy", "Mary", "Sandy", "S…
## $ sex    <fct> F, F, F, F, F, F, F, F, F, M, M, M, M, M, M, M, M, M, M
## $ age    <dbl> 13, 13, 14, 12, 12, 15, 11, 15, 14, 14, 14, 15, 12, 13, 12, 16,…
## $ height <dbl> 56.5, 65.3, 64.3, 56.3, 59.8, 66.5, 51.3, 62.5, 62.8, 69.0, 63.…
## $ weight <dbl> 84.0, 98.0, 90.0, 77.0, 84.5, 112.0, 50.5, 112.5, 102.5, 112.5,…
```

#### 25.2.1.2 `map`返回基本类型向量

`purrr::map()`总是返回列表。
如果确知其调用的函数总是返回某种类型的标量值，
可以用`map`的变种：

* `map_lgl()`：返回逻辑向量；
* `map_int()`：返回整型向量；
* `map_dbl()`: 返回双精度浮点型向量(double类型)；
* `map_chr()`: 返回字符型向量。

比如，
求`mtcars`各列类型，
因为确知`typeof()`函数对每列返回一个标量字符串，所以可以写成：

```
map_chr(d.class, typeof)
```

```
##        name         sex         age      height      weight 
## "character"   "integer"    "double"    "double"    "double"
```

用`map_lgl()`返回每一列是否数值型列：

```
map_lgl(d.class, is.numeric)
```

```
##   name    sex    age height weight 
##  FALSE  FALSE   TRUE   TRUE   TRUE
```

#### 25.2.1.3 `...`形参

在R函数的形参中，
允许有一个特殊的`...`形参（三个小数点），
这在调用泛函类型的函数时起到重要作用。
在调用泛函时，
所有没有形参与之匹配的实参，
不论是带有名字还是不带有名字的，
都自动归入这个参数，
将会由泛函传递给作为其自变量的函数。
`...`参数的类型相当于一个列表，
列表元素可以部分有名部分无名，
用`list(...)`可以将其转换成列表再访问。

例如，函数`mean()`可以计算去掉部分最低、最高值之后的平均值，
用选项`trim=`指定一个两边分别舍弃的值的个数比例。
为了将`d.cancer`的v0, v1列计算上下各自扣除10%的平均值，
需要利用`map_dbl()`函数的`...`参数输入`trim`选项值，如：

```
map_dbl(d.cancer[,c("v0", "v1")], 
  mean, trim=0.10)
```

```
##        v0        v1 
## 103.74357  40.98786
```

用tidyverse方式：

```
d.cancer |>
  summarise(across(c(v0, v1),
    ~ mean(., trim = 0.10)))
```

```
## # A tibble: 1 × 2
##      v0    v1
##   <dbl> <dbl>
## 1  104.  41.0
```

选择数值型列，然后用`map_double`计算截尾的均值：

```
d.cancer[, map_lgl(d.cancer, is.numeric)] |>
  map_dbl(mean, trim=0.10, na.rm = TRUE)
```

```
##        id       age        v0        v1 
##  17.50000  64.31579 103.74357  40.98786
```

purrr包提供了一个`keep`函数，
可以专门用来选择数据框各列或列表元素中满足某种条件的子集，
这个条件用一个返回逻辑值的函数来给出。如：

```
d.cancer |>
  keep(is.numeric) |>
  map_dbl(mean, trim = 0.10, na.rm = TRUE)
```

```
##        id       age        v0        v1 
##  17.50000  64.31579 103.74357  40.98786
```

也可以用tidyverse解决：

```
d.cancer |>
  summarise(across(
    where(is.numeric),
    ~ mean(., trim = 0.10, na.rm = TRUE) ))
```

```
## # A tibble: 1 × 4
##      id   age    v0    v1
##   <dbl> <dbl> <dbl> <dbl>
## 1  17.5  64.3  104.  41.0
```

需要注意的是，
在`map`类泛函中`...`仅用来将额外的选项传递给要调用的函数，
不支持向量化，
如果需要对两个或多个自变量的对应元素作变换，
需用用purrr包的`map2`等函数。
如果泛函中调用的是无名函数，
则`...`参数会造成变量作用域理解困难。

#### 25.2.1.4 用`map`处理`strsplit`函数结果示例

假设有4个学生的3次小测验成绩，
每个学生的成绩记录到了一个以逗号分隔的字符串中，如：

```
s <- c('10, 8, 7', 
      '5, 2, 2', 
      '3, 7, 8', 
      '8, 8, 9')
```

对单个学生，可以用`strsplit()`函数把三个成绩拆分，如：

```
strsplit(s[1], ',', fixed=TRUE)[[1]]
```

```
## [1] "10" " 8" " 7"
```

注意这里`strsplit()`的结果是仅有一个元素的列表，
用了“`[[...]]`”格式取出列表元素。
拆分的结果可以用`as.numeric()`转换为有三个元素的数值型向量：

```
strsplit(s[1], ',', fixed=TRUE)[[1]] |> as.numeric()
```

```
## [1] 10  8  7
```

还可以求三次小测验的总分：

```
strsplit(s[1], ',', fixed=TRUE)[[1]] |> 
  as.numeric() |>
  sum()
```

```
## [1] 25
```

用`strsplit()`处理有4个字符串的字符型向量s,
结果是长度为4的列表：

```
tmpr <- strsplit(s, ',', fixed=TRUE); tmpr
```

```
## [[1]]
## [1] "10" " 8" " 7"
## 
## [[2]]
## [1] "5"  " 2" " 2"
## 
## [[3]]
## [1] "3"  " 7" " 8"
## 
## [[4]]
## [1] "8"  " 8" " 9"
```

用`map()`和`as.numeric()`可以把列表中所有字符型转为数值型，
输出为一个列表，
然后再对各个列表元素中的向量求和。
使用管道运算符表达逐步的操作：

```
s |>
  strsplit(split=",", fixed=TRUE) |>
  map(as.numeric) |>
  map_dbl(sum)
```

```
## [1] 25  9 18 25
```

#### 25.2.1.5 在`map`中使用无名函数以及简写方法

`map()`中调用的函数可以是在`map()`中直接现场定义的无名函数。

仍考虑上面的问题，有4个学生，
每个学生有三门成绩，成绩之间用逗号分隔。
将每个学生的成绩拆分为三个字符串后，
就可以对每个学生调用一个统一的无名函数，
将字符型转换为数值型以后求和：

```
s |> 
  strsplit(split=",", fixed=TRUE) |>
  map_dbl(\(x) sum(as.numeric(x)))
```

```
## [1] 25  9 18 25
```

使用无名函数格式比较复杂，
purrr包为在`map()`等泛函中使用无名函数提供了简化的写法，
将无名函数写成“`~ 表达式`”格式，
表达式就是无名函数定义，
用`.x`表示只有一个自变量时的自变量名，
用`.x`和`.y`表示只有两个自变量时的自变量名，
用`..1`、`..2`、`..3`这样的名字表示有多个自变量时的自变量名。
如：

```
s |> 
  strsplit(split=",", fixed=TRUE) |>
  map_dbl(~ sum(as.numeric(.x)))
```

```
## [1] 25  9 18 25
```

需要注意的是，
如果`map()`等泛函中的无名函数需要访问其它变量的话，
需要理解其变量作用域或访问环境。
另外，
无名函数中的其它变量在每次被`map()`应用到输入列表的元素时都会重新计算求值。
建议这样的情况改用有名函数，
这样其中访问其它变量时作用域规则比较容易掌控，
也不会重复求值。

#### 25.2.1.6 在`map`中提取列表元素成员的简写

较为复杂的数据，
有时表现为列表的列表，
每个列表元素都是列表或者向量。
JSON、YAML等格式转换为R对象就经常具有这种嵌套结构。

例如，
有如下的嵌套格式数据，
这样的数据不利于用数据框格式保存：

```
od <- list(
  list(
    101, name="李明", age=15, 
    hobbies=c("绘画", "音乐")),
  list(
    102, name="张聪", age=17,
    hobbies=c("足球"),
    birth="2002-10-01")
)
```

为了取出每个列表元素的第一项，本来应该写成：

```
map_dbl(od, \(x) x[[1]])
```

```
## [1] 101 102
```

或：

```
map_dbl(od, ~ .x[[1]])
```

```
## [1] 101 102
```

purrr包提供了进一步的简化写法，
在需要一个函数或者一个“`~ 表达式`”的地方，
可以用整数下标值表示对每个列表元素提取其中的指定成分，如：

```
map_dbl(od, 1)
```

```
## [1] 101 102
```

类似地，可以在需要函数的地方写一个成员名，
提取每个列表元素中该成员，如：

```
map_chr(od, "name")
```

```
## [1] "李明" "张聪"
```

在应该用函数的地方还可以提供一个列表，
列表元素为成员序号或者成员名，
进行逐层挖掘，如：

```
map_chr(od, list("hobbies", 1))
```

```
## [1] "绘画" "足球"
```

表示取出每个列表元素的`hobbies`成员的第一个元素（每人的第一个业余爱好）。

取出不存在的成员会出错，
但可以用一个`.default`选项指定查找不到成员时的选项，
如：

```
map_chr(od, "birth", .default=NA)
```

```
## [1] NA           "2002-10-01"
```

#### 25.2.1.7 数据框分组处理示例

对`d.class`数据框，
希望分成男女生两个组，
每组内建立用身高预测体重的一元线性回归模型，
提取各模型的斜率项。
基本R的`split`函数可以按数据框的某列将数据框分成若干个子数据框，
结果为子数据框的列表。
借助于purrr包的`map`类函数和管道运算符，
可以将分组建模过程写成：

```
d.class |>
  split(d.class[["sex"]]) |>
  map(~ lm(weight ~ height, data=.x)) |>
  # 这里的“.”是map需要输入的无名函数的自变量
  map(coef) |>
  map_dbl(2)
```

```
##        M        F 
## 3.912549 3.424405
```

这些步骤用基本R的lapply或者for循环也能够完成，
但会更难读懂，
或者需要生成许多中间临时变量，
不如上面例子这样步骤清晰而且不需要产生中间结果。
上述问题使用tidyr的nest和unnest可以将分组结果、回归结果保存为tibble的列表列，
管理起来更为直观，
见[25.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals.html#fnctnl-nest)。

dplyr包和plyr包与这个例子的思想类似，
只不过更有针对性。

### 25.2.2 purrr包中`map`函数的变种

purrr包的`map`函数输入一个数据自变量和一个函数，
输出为列表；
`map_dbl()`等将输出转化为基础类型的向量。

purrr包还提供了与`map`目的类似，
但输入输出类型有变化的一些函数，
包括：

* `modify()`，输入一个数据自变量和一个函数，
  输出与输入数据同类型的结果；
* `map2()`可以输入两个数据自变量和一个函数，
  将两个自变量相同下标的元素用函数进行变换，
  输出列表；
* `imap()`根据一个下标遍历；
* `walk()`输入一个数据自变量和一个函数，
  不返回任何结果，仅利用输入的函数的副作用；
* `pmap()`输入若干个数据自变量和一个函数，
  对数据自变量相同下标的元素用函数进行变换。

将这些`map`变种按输入类型分为：

* 一个数据自变量，代表为`map()`；
* 两个自变量，代表为`map2()`；
* 一个自变量和一个下标变量，代表为`imap()`；
* 多个自变量，代表为`pmap()`。

将这些`map`变种按输出结果类型分为：

* 列表，代表为`map()`;
* 基础类型的向量，如`map_dbl()`, `map_chr()`等；
* 与输入数据类型相同的输出，代表为`modify()`；
* 不输出结果，代表为`walk()`。

输入类型和输出类型两两搭配，
purrr包提供了27种`map`类函数。

#### 25.2.2.1 输入输出类型相同的`modify`函数

purrr的`modify`函数与`map`函数作用类似，
并不会原地修改输入数据，
而是制作修改后的副本，
输出的结果类型与输入数据的结果类型相同，
所以可以用来修改数据框各列生成一个副本数据框。

比如，
对`d.class`中的三个数值型列，
都减去列中位数，其它列保持不变：

```
d1 <- modify(d.class, ~ if(is.numeric(.x)) .x - median(.x) else .x )
```

purrr包还提供了一个`modify_if()`函数，
可以对满足条件的列进行修改，如：

```
d2 <- modify_if(d.class, is.numeric, ~ .x - median(.x))
```

#### 25.2.2.2 对两个自变量的相同下标元素调用函数

`map()`函数仅支持一个输入数据的列表或向量。
`map2()`函数支持两个输入数据的列表或向量，
`map2(x, y, f, ...)`对每个下标`i`调用`f(x[[i]], y[[i]], ...)`，
结果返回一个列表。
如果知道函数`f()`会返回类型确定的标量值，
可以用`map2_dbl()`等变种。

例如，
`d1`是某市2001年四个季度的若干项经济指标，
`d2`是2002年的对应指标，
计算每项指标年度总和的同比增幅：

```
d1 <- tibble(
  x1 = c(106, 108, 103, 110),
  x2 = c(101, 112, 107, 105) )
d2 <- tibble(
  x1 = c(104, 111, 112, 109),
  x2 = c(102, 114, 105, 107) )
d3 <- map2_dbl(d1, d2, ~ (sum(.y) - sum(.x)) / sum(.x))
knitr::kable(list(d1=d1, d2=d2, d3=as_tibble(rbind(d3))))
```

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| | x1 | x2 | | --- | --- | | 106 | 101 | | 108 | 112 | | 103 | 107 | | 110 | 105 | | | x1 | x2 | | --- | --- | | 104 | 102 | | 111 | 114 | | 112 | 105 | | 109 | 107 | | | x1 | x2 | | --- | --- | | 0.0210773 | 0.0070588 | |

上述`map2_dbl()`调用等价于：

```
d3 <- numeric(2)
names(d3) <- names(d1)
for(j in seq_along(d1)){
  d3[[j]] <- (sum(d2[[j]]) - sum(d1[[j]]))/sum(d1[[j]])
}
d3
```

```
##          x1          x2 
## 0.021077283 0.007058824
```

如果计算结果与两个输入数据类型相同，
可以用`modify2()`。
比如，
上面的例子数据计算每个指标的同比增幅：

```
modify2(d1, d2, ~ (.y - .x) / .x)
```

```
## # A tibble: 4 × 2
##         x1       x2
##      <dbl>    <dbl>
## 1 -0.0189   0.00990
## 2  0.0278   0.0179 
## 3  0.0874  -0.0187 
## 4 -0.00909  0.0190
```

注意和`modify()`一样，
上述程序并不会修改`d1`和`d2`的值，
而是返回与其类型和大小相同的一个数据框。

`map2()`允许输入的`x`和`y`两个列表其中一个长度为1，
这时长度为1的列表的元素被重复利用。如：

```
d1b <- d1[,1,drop=FALSE]
map2_dbl(d1b, d2, ~ (sum(.y) - sum(.x)) / sum(.x))
```

```
##         x1         x1 
## 0.02107728 0.00234192
```

基本R的`Map()`函数起到与`map2()`和`pmap()`类似的作用。

#### 25.2.2.3 不产生输出的`walk`类函数

有时仅需要遍历一个数据结构调用函数进行一些显示、绘图，
这称为函数的副作用，
不需要返回结果。
purrr的`walk`函数针对这种情形。

例如，
显示数据框中每个变量的类别：

```
walk(d.class, ~ cat(typeof(.x), "\n"))
```

```
## character 
## integer 
## double 
## double 
## double
```

上面这个例子缺点是没有显示对应的变量名。

`walk2()`函数可以接受两个数据自变量，
类似于`map2()`。
例如，
需要对一组数据分别保存到文件中，
就可以将数据列表与保存文件名的字符型向量作为`walk2()`的两个数据自变量。
下面的程序将`d.class`分成男女生两个子集，
保存到两个csv文件中：

```
dl <- split(d.class, d.class[["sex"]])
walk2(dl, paste0("class-", names(dl), ".csv"), 
      ~ write.csv(.x, file=.y))
```

改用管道运算符：

```
d.class |>
  split(d.class[["sex"]]) |>
  walk2(paste0("class-", names(.), ".csv"), 
        ~ write.csv(.x, file=.y))
```

事实上，
`walk`、`walk2`并不是没有输出，
它们返回不显示的第一个自变量，
所以也适合用在管道运算的中间使得管道不至于中断。

基本R没有提供类似`walk`的功能。

#### 25.2.2.4 可同时访问下标或元素名与元素值的`imap`类函数

在前面用`walk`函数显示数据框各列类型的例子中，
没有能够同时显示变量名。
如果`x`有元素名，
`imap(x, f)`相当于`map2(x, names(x), f)`；
如果`x`没有元素名，
`imap(x, f)`相当于`map2(x, seq_along(x), f)`。
`iwalk()`与`imap()`类似但不返回信息。
`f`是对数据每一项要调用的函数，
输入的函数的第二个自变量或者无名函数的`.y`自变量会获得输入数据的元素名或者元素下标。
`imap_chr()`等是固定返回类型的变种。

例如，
显示数据框各列的变量名：

```
iwalk(d.class, ~ cat(.y, ": ", typeof(.x), "\n"))
```

```
## name :  character 
## sex :  integer 
## age :  double 
## height :  double 
## weight :  double
```

返回字符型向量的写法：

```
imap_chr(d.class, ~ paste0(.y, " ==> ", typeof(.x))) |>
  unname()
```

```
## [1] "name ==> character" "sex ==> integer"    "age ==> double"    
## [4] "height ==> double"  "weight ==> double"
```

输入数据没有元素名的演示：

```
dl <- list(1:5, 101:103)
iwalk(dl, ~ cat("NO. ", .y, ": ", .x[[1]], "\n"))
```

```
## NO.  1 :  1 
## NO.  2 :  101
```

显示了每个列表元素的第一项。

基本R没有提供类似`imap`的功能。

#### 25.2.2.5 多个数据自变量的`pmap`类函数

R的函数调用时支持向量化，
可以很好地处理各个自变量是向量的情形，
但是当自变量是列表、数据框等复杂类型时不能自动进行向量化处理。
purrr包的`pmap`类函数支持对多个列表、数据框、向量等进行向量化处理。
`pmap`不是将多个列表等作为多个自变量，
而是将它们打包为一个列表。
所以，
`map2(x, y, f)`用`pmap()`表示为`pmap(list(x, y), f)`。

在确知输出类型时可以用`pmap_chr()`, `pmap_dbl()`等变种，
在不需要输出结果时可以用`pwalk()`。

比如，
将三个列表中的对应项用`c()`函数连接：

```
x <- list(101, name="李明")
y <- list(102, name="张聪")
z <- list(103, name="王国")
pmap(list(x, y, z), c)
```

```
## [[1]]
## [1] 101 102 103
## 
## $name
## [1] "李明" "张聪" "王国"
```

因为数据框是列表，
所以`pmap()`也可以输入一个数据框和要并行执行的函数，
对数据框的每一行执行该函数。
例如：

```
d <- tibble::tibble(
  x = 101:103, 
  y=c("李明", "张聪", "王国"))
knitr::kable(d)
```

| x | y |
| --- | --- |
| 101 | 李明 |
| 102 | 张聪 |
| 103 | 王国 |

```
pmap_chr(d, \(...) paste(..., sep=":"))
```

```
## [1] "101:李明" "102:张聪" "103:王国"
```

`pmap()`和其它的`map()`类函数有一个区别是，
因为将输入数据打包在一个列表中，
而列表元素是有变量名的，
这样就可以将列表变量名取为要调用的函数的自变量名，
使得对输入列表中各元素的每个成员调用函数时，
可以带有对应的形参名调用。

例如，`mean()`函数可以计算去掉最小、最大一部分后的平均值，
`mean(x, trim)`用`trim`选项控制两端分别去掉的比例，
但是`trim`选项必须是标量。
用`map_dbl()`解决方法如下：

```
set.seed(101)
x <- rcauchy(1000)
trims <- c(0.05, 0.1, 0.2, 0.3, 0.4)
map_dbl(trims, ~ mean(x=x, trims=.x))
```

```
## [1] 0.7271278 0.7271278 0.7271278 0.7271278 0.7271278
```

可以用`pmap()`的列表元素名自动对应到调用函数形参名的方法：

```
pmap_dbl(list(trims = trims), mean, x=x)
```

```
## [1] 0.7271278 0.7271278 0.7271278 0.7271278 0.7271278
```

或：

```
pmap_dbl(list(trims = trims), ~ mean(x))
```

```
## [1] 0.7271278 0.7271278 0.7271278 0.7271278 0.7271278
```

基本R的`Map()`函数提供了类似的功能，
但是不允许多个自变量中有长度为1的；
基本R的`mapply()`函数与`Map()`类似，
但是会像`sapply()`函数那样试图自动找到最简化的输出数据结构，
这在通用程序中会使得结果不可控。

### 25.2.3 purrr包中`reduce`类函数

#### 25.2.3.1 `reduce`函数

许多二元运算符如加法、乘法，
可以很自然地推广到多个运算元之间的运算，
变成连加、连乘积等等。
某些常用的操作已经变成了R函数，
比如`sum()`、`prod()`，
但是其它一些运算，
包括用函数表示的运算，
也需要推广到对多个进行，
比如`intersect(x, y)`求两个集合的交集，
希望能推广到求多个集合的交集。

purrr包的`reduce`函数把输入列表（或向量）的元素逐次地用给定的函数进行合并计算。
比如，设`f(x,y)`是一个二元函数，
设`z`是有4个元素的列表，
则`reduce(z, f)`表示

```
f(f(f(z[[1]], z[[2]]), z[[3]]), z[[4]])
```

例如，

```
reduce(1:4, `+`)
```

```
## [1] 10
```

实际执行的是\((((1 + 2) + 3) + 4)\)。
当然，求\(1:4\)的和只需要`sum(1:4)`，
但是`reduce`可以对元素为复杂类型的列表进行逐项合并计算。

考虑多个集合的交集的问题。
下面的例子产生了4个集合，
然后反复调用`intersect()`求出了交集：

```
x <- list(
  c(2, 3, 1, 3, 1), 
  c(1, 5, 3, 3, 2),
  c(5, 4, 2, 5, 3),
  c(1, 4, 3, 2, 5))
intersect(intersect(intersect(x[[1]], x[[2]]), x[[3]]), x[[4]])
```

```
## [1] 2 3
```

也可以用管道运算写成:

```
x[[1]] |> intersect(x[[2]]) |> intersect(x[[3]]) |> intersect(x[[4]])
```

```
## [1] 2 3
```

还可以写成循环:

```
y <- x[[1]]
for(i in 2:4) y <- intersect(y, x[[i]])
y
```

```
## [1] 2 3
```

都比较繁琐。

利用purrr包的`reduce`函数，只要写成

```
reduce(x, intersect)
```

```
## [1] 2 3
```

泛函的好处是需要进行的变换或计算是作为参数输入的，
只要输入其它函数就可以改变要做的计算，
比如，
变成求各个集合的并集：

```
reduce(x, union)
```

```
## [1] 2 3 1 5 4
```

`reduce()`支持`...`参数，
所以可以给要调用的函数额外的自变量或选项。

`reduce`函数对多个输入默认从左向右计算，
可以用`.dir = "backward"`选项改为从右向左合并。

可以用选项`.init`给出合并初值，
在通用程序中使用`reduce()`时应该提供此选项，
这样如果输入了零长度数据，
可以有一个默认的返回值；
输入非零长度数据时，
此初值作为第一个元素之前的值参与计算，
所以一定要取为与要进行的运算一致的值，
比如连加的初始值自然为0，
连乘积的初始值自然为1，
多个集合交集的初始值为全集（所有参与运算的各个集合应为此初值的子集），
等等。

基本R的`Reduce()`函数提供了类似`purrr::reduce()`的功能，
不支持`...`参数。

#### 25.2.3.2 `reduce2`函数

`reduce2(x, y, f)`中的`x`是要进行连续运算的数据列表或向量，
而`y`是给这些运算提供不同的参数。
如果没有`.init`初始值，
`f`仅需调用`length(x)-1`次，
所以`y`仅需要有`length(x)-1`个元素；
如果有`.init`初始值，
`f`需要调用`length(x)`次，
`y`也需要与`x`等长。

#### 25.2.3.3 `accumulate`函数

对于加法，
R的`sum()`函数可以计算连加，
而`cumsum()`函数可以计算逐步的连加。如：

```
sum(1:4)
```

```
## [1] 10
```

```
cumsum(1:4)
```

```
## [1]  1  3  6 10
```

`purrr::reduce()`将连加推广到了其它的二元运算，
`purrr::accumulate()`则类似`cumsum()`的推广。

例如，对前面例子中的4个集合，
计算逐步的并集，
结果的第一项保持原来的第一项不变：

```
accumulate(x, union)
```

```
## [[1]]
## [1] 2 3 1 3 1
## 
## [[2]]
## [1] 2 3 1 5
## 
## [[3]]
## [1] 2 3 1 5 4
## 
## [[4]]
## [1] 2 3 1 5 4
```

将上述结果简化显示：

```
accumulate(x, union) |>
  map(~ sort(unique(.x)))
```

```
## [[1]]
## [1] 1 2 3
## 
## [[2]]
## [1] 1 2 3 5
## 
## [[3]]
## [1] 1 2 3 4 5
## 
## [[4]]
## [1] 1 2 3 4 5
```

#### 25.2.3.4 Map-reduce算法

Map-reduce是大数据技术中的重要算法，
在Hadoop分布式数据库中主要使用此算法思想。
将数据分散存储在不同计算节点中，
将需要的操作先映射到每台计算节点，
进行信息提取压缩，
最后用reduce的思想将不同节点的信息整合在一起。

### 25.2.4 purrr包中使用示性函数的泛函

返回逻辑向量的函数称为示性函数，
R中有许多`is.xxx`函数都是示性函数(predicate functions)。
示性函数本身不是泛函，
但是它们可以作为泛函的输入。

purrr包提供了如下的以示性函数函数为输入的泛函：

* `some(.x, .p)`，对数据列表或向量`.x`的每一个元素用`.p`判断，
  只要至少有一个为真，结果就为真；
  `every(.x, .p)`与`some`类似，但需要所有元素的结果都为真结果才为真。
  这些函数与`any(map_lgl(.x, .p))`和`all(map_lgl(.x, .p))`类似，
  但是只要在遍历过程中能提前确定返回值就提前结束计算，
  比如`some`只要遇到一个真值就不再继续判断，
  `every`只要遇到一个假值就不再继续判断。
* `detect(.x, .p)`返回数据`.x`的元素中第一个用`.p`判断为真的元素值，
  而`detect_index(.x, .p)`返回第一个为真的下标值。
* `keep(.x, .p)`选取数据`.x`的元素中用`.p`判断为真的元素的子集；
  `discard(.x, .p)`返回不满足条件的元素子集。

例如，判断数据框中有无因子类型的列：

```
some(d.class, is.factor)
```

```
## [1] TRUE
```

判断数据框是否完全由数值型列组成：

```
every(d.class, is.numeric)
```

```
## [1] FALSE
```

返回向量中的第一个超过100的元素的值：

```
detect(c(1, 5, 77, 105, 99, 123), ~ .x >= 100)
```

```
## [1] 105
```

返回向量中的第一个超过100的元素的下标：

```
detect_index(c(1, 5, 77, 105, 99, 123),
  ~ .x >= 100)
```

```
## [1] 4
```

对于上一个例子，`which(x >= 100)`可以返回所有满足条件的元素的下标。

下面的例子筛选出数据框的数值型列，
并用`map_dbl`求每列的平方和：

```
d.class |>
  keep(is.numeric) |>
  map_dbl(~ sum(.x ^ 2))
```

```
##       age    height    weight 
##   3409.00  74304.92 199435.75
```

用tidyverse的方法：

```
d.class |>
  summarise(across(
    where(is.numeric),
    \(x) sum(x^2)  ))
```

```
## # A tibble: 1 × 3
##     age height  weight
##   <dbl>  <dbl>   <dbl>
## 1  3409 74305. 199436.
```

从数据框（或列表）中选一部分满足某种条件的子集进行变换是常用的做法，
所以`map`提供了`map_if()`和`modify_if()`变种，
允许输入一个示性函数，
对满足条件的子集才应用输入的变换函数进行处理，
输入数据中其它元素原样返回。
`map_if`返回列表，
`modify_if`返回与输入数据相同类型的输出。
例如，
将数据框中数值型列除以100， 其它列保持不变：

```
modify_if(d.class, is.numeric, `/`, 100) |> head()
```

```
## # A tibble: 6 × 5
##   name  sex     age height weight
##   <chr> <fct> <dbl>  <dbl>  <dbl>
## 1 Alice F      0.13  0.565  0.84 
## 2 Becka F      0.13  0.653  0.98 
## 3 Gail  F      0.14  0.643  0.9  
## 4 Karen F      0.12  0.563  0.77 
## 5 Kathy F      0.12  0.598  0.845
## 6 Mary  F      0.15  0.665  1.12
```

基本R的`Find`函数与`detect`作用类似，
`Position`与`detect_index`作用类似，
`Filter`函数与`keep`作用类似。

### 25.2.5 基本R的函数式编程支持

使用purrr包的泛函的好处是用法风格一致，
有许多方便功能。
对于少量的使用泛函的需求，
在不想使用purrr包的情况下，
可以使用基本R中的类似功能。

基本R的`apply`函数可以对矩阵的每行或每列进行计算，
或对多维向量的某个维度进行计算。
参见[12.6](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-matrix.html#p-t-array-apply)。

基本R中的`integrate`、`uniroot`、`optim`、`omptimize`等函数也需要输入函数，
但主要用于数学计算，
与一般的函数式编程关系不大。

基本R`lapply`函数用输入的函数对数据的每个元素进行变换，格式为

```
lapply(X, FUN, ...)
```

其中`X`是一个列表或向量，
`FUN`是一个函数（可以是有名或无名函数），
结果也总是一个列表，
结果列表的第\(i\)个元素是将`X`的第\(i\)个元素输入到`FUN`中的返回结果。
`...`参数会输入到`FUN`中。
这与`purrr::map()`功能类似。

`sapply`与`lapply`函数类似，
但是`sapply`试图简化输出结果为向量或矩阵，
在不可行时才和`lapply`返回列表结果。
如果`X`长度为零，结果是长度为零的列表；
如果`FUN(X[i])`都是长度为1的结果，
`sapply()`结果是一个向量；
如果`FUN(X[i])`都是长度相同且长度大于1的向量，
`sapply()`结果是一个矩阵，
矩阵的第\(i\)列保存`FUN(X[i])`的结果。
因为`sapply()`的结果类型的不确定性，
在自定义函数中应慎用。

`vapply()`函数与`sapply()`函数类似，
但是它需要第三个参数即`函数`返回值类型的例子，格式为

```
vapply(X, FUN, FUN.VALUE, ...)
```

其中`FUN.VALUE`是每个`FUN(X[i])`的返回值的例子，
要求所有`FUN(X[i])`结果类型和长度相同。

例如，求`d.class`每一列类型的问题，用lapply，写成：

```
lapply(d.class, typeof)
```

```
## $name
## [1] "character"
## 
## $sex
## [1] "integer"
## 
## $age
## [1] "double"
## 
## $height
## [1] "double"
## 
## $weight
## [1] "double"
```

`lapply`的结果总是列表。
`sapply`会尽可能将结果简化为向量或矩阵，如：

```
sapply(d.class, typeof)
```

```
##        name         sex         age      height      weight 
## "character"   "integer"    "double"    "double"    "double"
```

或使用`vapply()`:

```
vapply(d.class, typeof, "")
```

```
##        name         sex         age      height      weight 
## "character"   "integer"    "double"    "double"    "double"
```

`vapply`可以处理遍历时每次调用的函数返回值不是标量的情形，
结果为矩阵，
purrr包的`map_dbl`等只能处理调用的函数返回值是标量的情形。

R提供了 `Map`, `Reduce`, `Filter`, `Find`,
`Negate`, `Position`等支持函数式编程的泛函。

`Map()`与`purrr::map`、`purrr::pmap`功能类似，
以一个函数作为参数，
可以对其它参数的每一对应元素进行变换，
结果为列表。

例如，
对数据框`d`，
如下的程序可以计算每列的平方和：

```
d <- data.frame(
  x = c(1, 7, 2),
  y = c(3, 5, 9))
Map(function(x) sum(x^2), d)
```

```
## $x
## [1] 54
## 
## $y
## [1] 115
```

实际上，这个例子也可以用`lapply()`改写成

```
lapply(d, function(x) sum(x^2))
```

```
## $x
## [1] 54
## 
## $y
## [1] 115
```

`Map()`比`lapply()`增强的地方在于它允许对多个列表的对应元素逐一处理。
例如，为了求出`d`中每一行的最大值，可以用

```
Map(max, d$x, d$y)
```

```
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 7
## 
## [[3]]
## [1] 9
```

可以用`unlist()`函数将列表结果转换为向量，如

```
unlist(Map(max, d$x, d$y))
```

```
## [1] 3 7 9
```

`mapply()`函数与`Map()`类似，
但是可以自动简化结果类型，
可以看成是`sapply()`推广到了可以对多个输入的对应元素逐项处理。
`mapply()`可以用参数`MoreArgs`指定逐项处理时一些共同的参数。
如

```
mapply(max, d$x, d$y)
```

```
## [1] 3 7 9
```

当`d`数据框有多列时为了求每行的最大值，
可以用`Reduce`函数将两两求最大值的运算推广到多个之间的运算。

`Reduce`函数功能与`purrr::reduce`类似，
把输入列表（或向量）的元素逐次地用给定的函数进行合并计算。

例如，求四个集合的交集：

```
set.seed(5)
x <- replicate(4, sample(
  1:5, size=5, replace=TRUE), simplify=FALSE); x
```

```
## [[1]]
## [1] 2 3 1 3 1
## 
## [[2]]
## [1] 1 5 3 3 2
## 
## [[3]]
## [1] 5 4 2 5 3
## 
## [[4]]
## [1] 1 4 3 2 5
```

```
Reduce(intersect, x)
```

```
## [1] 2 3
```

`Reduce`函数对多个输入默认从左向右计算，
可以用`right`参数选择是否从右向左合并。
参数`init`给出合并初值，
参数`accumulate`要求保留每一步合并的结果（累计）。
这个函数可以把很多仅适用于两个运算元的运算推广到多个参数的情形。

`Filter(f, x)`与`purrr::keep`作用类似，
用一个示性函数`f`作为筛选规则，
从列表或向量`x`中筛选出用`f`作用后为真值的元素子集。
例如

```
f <- function(x) x > 0 & x < 1
Filter(f, c(-0.5, 0.5, 0.8, 1))
```

```
## [1] 0.5 0.8
```

当然，这样的简单例子完全可以改写成：

```
x <- c(-0.5, 0.5, 0.8, 1)
x[x>0 & x < 1]
```

```
## [1] 0.5 0.8
```

但是，对于比较复杂类型的判断，
比如当`x`是列表且其元素本身也是复合类型的时候，
就需要把判断写成一个函数，
然后可以用`Filter`比较简单地表达按照判断规则取子集的操作。

`Find()`功能与`purrr::detect`类似，
返回满足条件的第一个元素，
也可以用参数`right=TRUE`要求返回满足条件的最后一个。

`Position()`功能与`purrr::detect_index`类似，
返回第一个满足条件的元素所在的下标位置。

### 25.2.6 自定义泛函

用户也可以自定义泛函。
比如，希望对一个数据框中所有的数值型变量计算某些统计量，
要计算的统计量由用户决定而不是由此自定义函数决定，
输入的函数的结果总是数值，
编写自定义的泛函为：

```
library(purrr)
summary.df.numeric <- function(df, FUN, ...){
  df |>
    keep(is.numeric) |>
    map_dbl(FUN, ...)
}
```

这里参数`FUN`是用来计算统计量的函数。
例如对d.cancer中每个数值型变量计算最小值：

```
summary.df.numeric(d.cancer, min, na.rm=TRUE)
```

```
##    id   age    v0    v1 
##  1.00 49.00 12.58  2.30
```

为了说明上面定义的泛函是如何对数据框进行处理的，
我们对其进行如下的改写：

```
summary.df.numeric2 <- function(df, FUN, ...){
  res <- c()
  nd <- c()
  for(j in seq_along(df)){
    if(is.numeric(df[[j]])){
      resj <- FUN(df[[j]], ...) 
      res <- cbind(res, resj)
      nd <- c(nd, names(df)[j])
    }
  }
  if(ncol(res)>0) {
    colnames(res) <- nd
    if(nrow(res) == 1) {
      res <- c(res)
      names(res) <- nd
    }
  }
  res
}
summary.df.numeric2(d.cancer, min, na.rm=TRUE)
```

```
##    id   age    v0    v1 
##  1.00 49.00 12.58  2.30
```

也可以用基本R的`Filter()`, `sapply()`实现相同功能：

```
summary.df.numeric <- function(df, FUN, ...){
  Filter(is.numeric, df) |>
    sapply(FUN, ...)
}
```

## 25.3 函数工厂

函数的返回值可以是函数，
为此只要在函数内部定义嵌套函数并以嵌套函数为返回值。
返回函数的函数称为**函数工厂**，
函数工厂的输出结果称为一个闭包(closure)。
因为函数由形参表、函数体和定义环境三个部分组成，
函数工厂输出的闭包的定义环境是函数工厂的内部环境，
即函数工厂运行时产生的运行环境，
所以闭包包含了生产它的函数工厂的运行环境，
可以将闭包的一些状态信息保存在该环境中，
实现带有状态的函数。

基本R函数`approxfun`和`splinefun`就是以函数为输出的函数工厂。

### 25.3.1 闭包例子

利用函数工厂和闭包可以解决前面提出的记录函数已运行次数的问题。如

```
f.gen <- function(){
  runTimes <- 0

  function(){
    runTimes <<- runTimes + 1
    print(runTimes)
  }
}
f <- f.gen()
f()
```

```
## [1] 1
```

```
f()
```

```
## [1] 2
```

函数`f.gen`中定义了内嵌函数并以内嵌函数为输出，
`f.gen`是一个函数工厂,
其返回值是一个闭包，
闭包也是一个R函数，
这个返回值“绑定”(bind)到变量名`f`上，
所以`f`是一个函数。

调用函数`f`时用到变量`runTimes`，
用了`<<-`这种格式给这个变量赋值，
这样赋值的含义是在定义时的环境中逐层向上(向外，向父环境方向)查找变量是否存在，
在哪一层找到变量就给那里的变量赋值。
这样查找的结果是变量`runTimes`在`f.gen`的运行环境中。
调用`f`的时候`f.gen`已经结束运行了，
一般说来`f.gen`的运行环境应该已经不存在了；
但是，
函数的定义环境是随函数本身一同保存的，
因为函数工厂`f.gen`输出了函数`f`，
`f`的定义环境是`f.gen`的运行环境，
所以起到了把`f.gen`的运行环境保存在`f`中的效果，
而`f.gen`运行环境中的变量值`runTimes`也就保存在了函数`f`中，
可以持续被`f`访问，
不像`f`的局部变量那样每次运行结束就会被清除掉。

注意，
如果函数工厂生产出了两个闭包，
这两个闭包的定义环境是不同的，
因为生产时的运行环境是不同的。
例如，
生产两个计数器，
这两个计数器是分别计数的：

```
c1 <- f.gen()
c2 <- f.gen()
c1()
```

```
## [1] 1
```

```
c1()
```

```
## [1] 2
```

```
c2()
```

```
## [1] 1
```

下面是一个类似的函数工厂例子,
产生的闭包可以显示从上次调用到下次调用之间经过的时间：

```
make_stop_watch <- function(){
  saved.time <- proc.time()[3]
  
  function(){
    t1 <- proc.time()[3]
    td <- t1 - saved.time
    saved.time <<- t1
    cat("流逝时间（秒）：", td, "\n")
    invisible(td)
  }
}
ticker <- make_stop_watch()
ticker()
## 流逝时间（秒）： 0 
for(i in 1:1000) sort(runif(10000))
ticker()
## 流逝时间（秒）： 1.53
```

其中`proc.time()`返回当前的R会话已运行的时间，
结果在MS Windows系统中有三个值，分别是用户时间、系统时间、流逝时间，
其中流逝时间比较客观。

### 25.3.2 动态查找和懒惰求值引起的问题

上面的两个函数工厂都没有使用任何选项。
如果函数工厂有选项，
其中的选项值会被保存到生产出的闭包函数中，
但是因为懒惰求值规则的影响，
有可能调用闭包函数时才对选项求值，
如果保存选项的变量在生产和调用之间改变了值，
就会发生错误。

比如，
下面的函数工厂可以生产出进行幂变换的函数：

```
make.pf <- function(power){
  function(x) x^power
}
p <- 2
square <- make.pf(p)
p <- 3
square(2)
```

```
## [1] 8
```

在生产出`square`函数时，
选项`p`的值是2，
所以函数`square`应该是做平方变换的函数，
虽然在调用之前`p`的值被改成了3，
但是按理说不应该修改已经生产出来的`square`定义。
程序结果说明调用`square`时用的是`p=3`的值，
这是怎么回事？

R函数有懒惰求值规则，
在生产出`square`的那一步，
因为并不需要实际计算`x^power`，
所以实参`p`的值并没有被计算，
而是将`square`的定义环境中的`power`指向了全局空间的变量`p`，
调用`square(4)`的时候才实际需要`power`的值，
这时`power`才求值，
其值为`p`的**当前**值。

避免这样的问题的办法是在函数工厂内用`force()`函数命令输入的参数当场求值而不是懒惰求值。
如：

```
make.pf <- function(power){
  force(power)
  function(x) x^power
}
p <- 2
square <- make.pf(p)
p <- 3
square(2)
```

```
## [1] 4
```

这个版本的程序结果正确。

### 25.3.3 函数工厂的内存负担

因为函数工厂生产出的闭包函数保存了函数工厂的运行环境，
如果这个运行环境很大，
就会造成较大的不必要的内存占用。
所以，
函数工厂内应尽量不要有占用大量内存的变量。
可以在函数工厂内用`rm()`删除不再使用的变量。

### 25.3.4 函数算子

函数算子输入函数，输出函数，
通常用来对输入函数的行为进行改进或做细微的修改。
基本R的`Vectorize`函数输入一个函数，
将其改造成支持向量化的版本。

下面的`dot_every`函数输入一个函数，
将其改造为被循环调用时可以每调用一定次数就显示一个小数点，
这样可以用来显示循环的进度，
也适用于在`lapply()`或`purrr::map()`等函数调用时显示进度。
`purrr::map()`有一个`.progress`选项可以显示进度。

```
dot_every <- function(f, n) {
  force(f)
  force(n)
  
  i <- 0
  function(...) {
    i <<- i + 1
    if (i %% n == 0) cat(".")
    f(...)
  }
}
sim <- function(i){
  x <- runif(1E6)
  invisible(sort(x))
}
walk(1:100, dot_every(sim, 10))
```

使用`.progress`选项：

```
walk(1:100, sim, .progress = TRUE)
```

## 25.4 tibble中的列表列

### 25.4.1 `nest`和`unnest`

dplyr包的`group_by`与`summarise`、`summarise_at`等函数配合，
可以对数据框分组计算各种概括统计量。

但是，如果分组以后希望进行更复杂的统计分析，
比如分组回归建模，
`summarise`就不够用了。
这时，
可以用基本R的`split`函数将数据框按某个分类变量拆分为子数据框的列表，
然后用purrr包的map类函数分类建模，
最终将各个模型的结果合并为一个数据框。
参见[25.2.1.7](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals.html#fnctnl-fnctnl-map-split)。

上面的办法虽然可行，
但是管理不够方便。
tidyr包（属于tidyverse系列，载入tidyverse时会自动载入）提供了`nest`和`unnest`函数，
可以将子数据框保存在tibble中，
可以将保存在tibble中的子数据框合并为一个大数据框。
实际上，
tibble允许存在数据类型是列表(list)的列，
子数据框就是以列表数据类型保存在tibble的一列中的。

### 25.4.2 `group_by`与`nest`配合

对数据框用`group_by`分组后调用`nest`函数就可以生成每个组的子数据框。

例如，
将d.cancer数据框按`type`分类拆分为2个子数据框，
存入tibble的data列中：

```
d.cancer |>
  group_by(type) |>
  nest()
```

```
## # A tibble: 2 × 2
## # Groups:   type [2]
##   type  data             
##   <chr> <list>           
## 1 腺癌  <tibble [12 × 5]>
## 2 鳞癌  <tibble [22 × 5]>
```

现在`data`列是列表类型的，
有2个元素，
每个元素是一个子数据框。
`group_by()`中也可以用两个或多个分类变量构成交叉分组。

可以用purrr包的`map()`等函数对每个子数据框进行处理，
结果可以用`mutate`保存为新的列表类型的列，
如果结果是数值型标量也可以保存为普通的数据框列。

例如，下面先定义对子数据框回归建模的函数，
然后用purrr包的`map`函数将回归建模的函数作用到`data`列的每个元素，
用`mutate`保存到列表类型的`lmr`列中：

```
fmod <- function(subdf)  lm(v1 ~ v0, data = subdf)
mod.cancer <- d.cancer |>
  group_by(type) |>
  nest() |>
  mutate(lmr = map(data, fmod))
mod.cancer
```

```
## # A tibble: 2 × 3
## # Groups:   type [2]
##   type  data              lmr   
##   <chr> <list>            <list>
## 1 腺癌  <tibble [12 × 5]> <lm>  
## 2 鳞癌  <tibble [22 × 5]> <lm>
```

上面程序的`mutate`中的`map(data, fmod)`，
`data`是用`nest()`生成的结果数据框中`data`列，
此列为一个两个元素的列表，
每个元素是一个子数据框，
用`map(data, fmod)`对每个子数据框分别建立回归模型，
得到的两个回归模型，
作为两个列表元素组合成一个列表，
存放在结果数据框的`lmr`列中，
`lmr`列的类型是列表。

下面写一个函数从一个回归模型及相应子数据框中，
提取R方，
将提取的结果保存为普通数值型列r.squared：

```
frsqr <- function(mod){
  summary(mod)$r.squared
}
mod.cancer |>
  mutate(
    r.squared = map_dbl(lmr, frsqr)) |>
  select(-data, -lmr)
```

```
## # A tibble: 2 × 2
## # Groups:   type [2]
##   type  r.squared
##   <chr>     <dbl>
## 1 腺癌      0.710
## 2 鳞癌      0.520
```

`map()`和`map_dbl()`中输入函数时可以用purrr包的无名函数写法，如：

```
d.cancer |>
  group_by(type) |>
  nest() |>
  mutate(
    lmr = map(data, ~ lm(v1 ~ v0, data = .x)),
    r.squared = map_dbl(lmr, ~ summary(.x)$r.squared)) |>
  select(-data, -lmr)
```

```
## # A tibble: 2 × 2
## # Groups:   type [2]
##   type  r.squared
##   <chr>     <dbl>
## 1 腺癌      0.710
## 2 鳞癌      0.520
```

也可以从每个模型提取多个值，
这时，
为了使得多个值在展开时能保存在同一行中，
需要将每个子数据框的提取结果保存为一个一行的子数据框：

```
fextract <- function(mod){
  x1 <- coef(mod)
  tibble(
    intercept = x1[1],
    v0 = x1[2],
    r.squared = summary(mod)$r.squared)
}
mod.cancer |>
  mutate(
    outlist = map(lmr, fextract))
```

```
## # A tibble: 2 × 4
## # Groups:   type [2]
##   type  data              lmr    outlist         
##   <chr> <list>            <list> <list>          
## 1 腺癌  <tibble [12 × 5]> <lm>   <tibble [1 × 3]>
## 2 鳞癌  <tibble [22 × 5]> <lm>   <tibble [1 × 3]>
```

结果的`outlist`列是列表类型的，
每个元素是一个\(1 \times 3\)的tibble。
下面，就可以用`unnest()`将每个组提取的回归结果转换为普通的数据框列：

```
mod.cancer |>
  mutate(
    outlist = map(lmr, fextract)) |>
  select(-data, -lmr) |>
  unnest(outlist)
```

```
## # A tibble: 2 × 4
## # Groups:   type [2]
##   type  intercept    v0 r.squared
##   <chr>     <dbl> <dbl>     <dbl>
## 1 腺癌      0.225 0.370     0.710
## 2 鳞癌      5.51  0.374     0.520
```

提取的结果也可以是一个不止一行的子数据框，例如，
提取回归结果中的系数估计、标准误差、t统计量和检验p值组成的矩阵：

```
fcoefmat <- function(mod){
  as_tibble(
    summary(mod)$coefficients,
    rownames="term")
}
mod.cancer |>
  mutate(
    outlist = map(lmr, fcoefmat)) |>
  select(-data, - lmr) |>
  unnest(outlist)
```

```
## # A tibble: 4 × 6
## # Groups:   type [2]
##   type  term        Estimate `Std. Error` `t value` `Pr(>|t|)`
##   <chr> <chr>          <dbl>        <dbl>     <dbl>      <dbl>
## 1 腺癌  (Intercept)    0.225      10.2       0.0221   0.983   
## 2 腺癌  v0             0.370       0.0749    4.94     0.000585
## 3 鳞癌  (Intercept)    5.51       10.8       0.510    0.616   
## 4 鳞癌  v0             0.374       0.0803    4.66     0.000151
```

为了更好地提取统计模型的信息为规整的数据框格式，
broom扩展包提供了`tidy`函数，
可以将统计模型的输出转换为数据框，
这些功能与tidyr的`nest`, `unnest`配合，
可以很好地提取统计模型的信息，如：

```
mod.cancer |>
  mutate(
    outlist = map(lmr, broom::tidy)) |>
  select(-data, -lmr) |>
  unnest(outlist)
```

```
## # A tibble: 4 × 6
## # Groups:   type [2]
##   type  term        estimate std.error statistic  p.value
##   <chr> <chr>          <dbl>     <dbl>     <dbl>    <dbl>
## 1 腺癌  (Intercept)    0.225   10.2       0.0221 0.983   
## 2 腺癌  v0             0.370    0.0749    4.94   0.000585
## 3 鳞癌  (Intercept)    5.51    10.8       0.510  0.616   
## 4 鳞癌  v0             0.374    0.0803    4.66   0.000151
```

`unnest`提取出的信息也可以是一个向量，
在展开时会展开到同一列中。
例如，
对每个组提取回归的拟合值：

```
mod.cancer |>
  mutate(
    v1hat = map(lmr, ~ fitted(.))) |>
  select(-lmr) |>
  unnest(c(data, v1hat))
```

```
## # A tibble: 34 × 7
## # Groups:   type [2]
##    type     id   age sex      v0     v1  v1hat
##    <chr> <dbl> <dbl> <chr> <dbl>  <dbl>  <dbl>
##  1 腺癌      1    70 F      26.5   2.91  10.0 
##  2 腺癌      2    70 F     135.   35.1   50.4 
##  3 腺癌      3    69 F     210.   74.4   77.9 
##  4 腺癌      4    68 M      61    35.0   22.8 
##  5 腺癌      6    75 F     330.  112.   123.  
##  6 腺癌     11    55 M     125.   12.3   46.6 
##  7 腺癌     13    55 F      12.9   2.3    5.01
##  8 腺癌     14    75 M      40.2  24.0   15.1 
##  9 腺癌     15    61 F      12.6   7.39   4.88
## 10 腺癌     19    NA F      32.9   9.45  12.4 
## # … with 24 more rows
```

程序中的`unnest`将`data`列和`v1hat`列都释放为普通的数据框列了，
`data`列中释放出了多列原始数据，
`fitted`列中释放出了v1回归拟合值。

### 25.4.3 `summarise`统计量用列表表示

实际上，
`summarise`等函数如果将结果用`list()`声明，
汇总结果就可以保存为列表类型的列，
结果可以包含多个值，
`unnest`可以将结果恢复成正常的数据框，
如：

```
vnames <- expand_grid(
  var = c("v0", "v1"),
  stat = c("min", "max")) |>
  pmap_chr(paste, sep="_")

d.cancer |>
  group_by(type) |>
  summarise(
    stat = list(vnames),
    value = list(c(range(v0), range(v1)))  ) |>
  unnest(c(stat, value))
```

```
## # A tibble: 8 × 3
##   type  stat    value
##   <chr> <chr>   <dbl>
## 1 腺癌  v0_min  12.6 
## 2 腺癌  v0_max 330.  
## 3 腺癌  v1_min   2.3 
## 4 腺癌  v1_max 122.  
## 5 鳞癌  v0_min  13.2 
## 6 鳞癌  v0_max 238.  
## 7 鳞癌  v1_min   3.34
## 8 鳞癌  v1_max 128.
```

这个例子可以用长宽表转换方法变成每个统计量占一列：

```
d.cancer |>
  group_by(type) |>
  summarise(
    stat = list(vnames),
    value = list(c(range(v0), range(v1)))  ) |>
  unnest(c(stat, value)) |>
  separate(stat, into = c("variable", "stat"), sep="_") |>
  pivot_wider(
    names_from = "stat",
    values_from = "value"
  )
```

```
## # A tibble: 4 × 4
##   type  variable   min   max
##   <chr> <chr>    <dbl> <dbl>
## 1 腺癌  v0       12.6   330.
## 2 腺癌  v1        2.3   122.
## 3 鳞癌  v0       13.2   238.
## 4 鳞癌  v1        3.34  128.
```

### 25.4.4 `unnest`的语法格式

`unnest()`第一自变量为管道输入的数据框，
第二自变量`cols`可以用如下格式：

* 单个变量名，不需要写成字符串形式；
* 多个变量名，写成`c(x, y, z)`这样的格式，不需要写成字符型向量；
* 保存在字符型向量中的变量名，用`all_of(vnames)`格式，
  其中`vnames`是保存了要释放的列名的字符型向量的变量名;
  也可以写成`all_of(c("x", "y", "z"))`这样的格式。

### 25.4.5 直接生成列表类型的列

也可以直接生成列表类型的列，
符合条件时可以用`unnest()`合并为大数据框。
如：

```
d1 <- tibble(
  id = 1:2,
  df = vector("list", length=2))
d1[["df"]][1] <- list(
  tibble(x=1, y=2)
)
d1[["df"]][2] <- list(
  tibble(x=11:12, y=21:22)
)
d1 |>
  unnest(cols = c(df))
```

```
## # A tibble: 3 × 3
##      id     x     y
##   <int> <dbl> <dbl>
## 1     1     1     2
## 2     2    11    21
## 3     2    12    22
```

但是，不能直接写成`d1[["df"]][[1]] <- tibble(x=1, y=2)`。
这样写需要提前将列表`d1$df`的元素类型设定为tibble类型。