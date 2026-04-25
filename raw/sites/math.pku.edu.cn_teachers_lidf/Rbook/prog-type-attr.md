---
crawl_time: '2026-01-17 14:19:45'
framework: rbook
title: 8 R数据类型的性质 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-attr.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 8 R数据类型的性质

## 8.1 存储模式与基本类型

R的变量可以存储多种不同的数据类型，
可以用`typeof()`函数来返回一个变量或表达式的类型。比如

```
typeof(1:3)
## [1] "integer"
typeof(c(1,2,3))
## [1] "double"
typeof(c(1, 2.1, 3))
## [1] "double"
typeof(c(TRUE, NA, FALSE))
## [1] "logical"
typeof('Abc')
## [1] "character"
typeof(factor(c('F', 'M', 'M', 'F')))
## [1] "integer"
```

注意因子的结果是`integer`而不是因子。

R还有两个函数`mode()`和`storage.mode()`起到与`typeof()`类似的作用，
这是为了提供与S语言兼容所遗留的，
应停止使用。

R中数据的最基本的类型包括logical,
integer, double, character, complex, raw,
其它数据类型都是由基本类型组合或转变得到的。
character类型就是字符串类型，
raw类型是直接使用其二进制内容的类型。
为了判断某个向量`x`保存的基本类型，
可以用`is.xxx()`类函数，
如`is.integer(x)`,
`is.double(x)`,
`is.numeric(x)`,
`is.logical(x)`,
`is.character(x)`,
`is.complex(x)`,
`is.raw(x)`。
其中`is.numeric(x)`对integer和double内容都返回真值。

在R语言中数值一般看作double,
如果需要明确表明某些数值是整数，
可以在数值后面附加字母L，如

```
is.integer(c(1, -3))
## [1] FALSE
is.integer(c(1L, -3L))
## [1] TRUE
```

整数型的缺失值是`NA`，
而double型的特殊值除了`NA`外，
还包括`Inf`, `-Inf`和`NaN`，
其中`NaN`也算是缺失值, `Inf`和`-Inf`不算是缺失值。
如:

```
c(-1, 0, 1)/0
## [1] -Inf  NaN  Inf
is.na(c(-1, 0, 1)/0)
## [1] FALSE  TRUE FALSE
```

对double类型，可以用`is.finite()`判断是否有限值，
`NA`、`Inf`, `-Inf`和`NaN`都不是有限值；
用`is.infinite()`判断是否`Inf`或`-Inf`；
`is.na()`判断是否`NA`或`NaN`；
`is.nan()`判断是否`NaN`。

严格说来，
`NA`表示逻辑型缺失值，
但是当作其它类型缺失值时一般能自动识别。
`NA_integer_`是整数型缺失值，
`NA_real_`是double型缺失值，
`NA_character_`是字符型缺失值。

在R的向量类型中，
integer类型、double类型、logical类型、character类型、还有complex类型和raw类型称为原子类型(atomic types)，
原子类型的向量中元素都是同一基本类型的。
比如，
double型向量的元素都是double或者缺失值。

除了原子类型的向量，
在R语言的定义中，
向量还包括后面要讲到的列表（list），
列表的元素不需要属于相同的基本类型，
而且列表的元素可以不是单一基本类型元素。
用`typeof()`函数可以返回向量的类型，
列表返回结果为`"list"`:

```
typeof(list("a", 1L, 1.5))
## [1] "list"
```

原子类型的各个元素除了基本类型相同，
还不包含任何嵌套结构，如：

```
c(1, c(2,3, c(4,5)))
## [1] 1 2 3 4 5
```

R有一个特殊的`NULL`类型，
这个类型只有唯一的一个`NULL`值，
表示不存在。
`NULL`长度为0，
不能有任何属性值。
用`is.null()`函数判断某个变量是否取`NULL`。

`NULL`值可以用来表示类型未知的零长度向量，
如`c()`没有自变量时返回值就是`NULL`；
也经常用作函数缺省值，
在函数内用`is.null()`判断其缺省后再用一定的计算逻辑得到真正的缺省情况下的数值。

要把`NULL`与`NA`区分开来，
`NA`是有类型的（integer、double、logical、character等),
`NA`表示存在但是未知。
数据库管理系统中的NULL值相当于R中的NA值。

## 8.2 类型转换与类型升档

可以用`as.xxx()`类的函数在不同类型之间进行强制转换。
如

```
as.numeric(c(FALSE, TRUE))
## [1] 0 1
as.character(sqrt(1:4))
## [1] "1"                "1.4142135623731"  "1.73205080756888" "2"
```

类型转换也可能是隐含的，比如，
四则运算中数值会被统一转换为double类型，
逻辑运算中运算元素会被统一转换为logical类型。
逻辑值转换成数值时，`TRUE`转换成1，
`FALSE`转换成0。

在用`c()`函数合并若干元素时，
如果元素基本类型不同，
将统一转换成最复杂的一个，复杂程度从简单到复杂依次为：
`logical<integer<double<character`。
这种做法称为类型升档，如

```
c(FALSE, 1L, 2.5, "3.6")
## [1] "FALSE" "1"     "2.5"   "3.6"
```

不同类型参与要求类型相同的运算时，
也会统一转换为最复杂的类型，
也称为类型升档，
如：

```
TRUE + 10
## [1] 11
paste("abc", 1)
## [1] "abc 1"
```

## 8.3 属性

除了`NULL`以外，
R的变量都可以看成是对象，
都可以有属性。
在R语言中，
属性是把变量看成对象后，
除了其存储内容（如元素）之外的其它附加信息，
如维数、类属等。
R对象一般都有`length`和`mode`两个属性。

常用属性有`names`, `dim`，`class`等。

### 8.3.1 `attributes`函数

对象`x`的所有属性可以用`attributes()`读取，
如

```
x <- table(c(1,2,1,3,2,1)); print(x)
## 
## 1 2 3 
## 3 2 1
attributes(x)
## $dim
## [1] 3
## 
## $dimnames
## $dimnames[[1]]
## [1] "1" "2" "3"
## 
## 
## $class
## [1] "table"
```

`table()`函数用了输出其自变量中每个不同值的出现次数，称为频数。
从上例可以看出，
`table()`函数的结果有三个属性，前两个是dim和dimnames,
这是数组(array)具有的属性；
另一个是class属性，值为`"table"`。
因为`x`是数组，可以访问如

```
x[1]
## 1 
## 3
x["3"]
## 3 
## 1
```

也可以用`attributes()`函数修改属性，
如

```
attributes(x) <- NULL
x
## [1] 3 2 1
```

如上修改后`x`不再是数组，也不是table。

### 8.3.2 `attr`函数

可以用`attr(x, "属性名")`的格式读取或定义`x`的属性。
如：

```
x <- c(1,3,5)
attr(x, "theta") <- c(0, 1)
print(x)
## [1] 1 3 5
## attr(,"theta")
## [1] 0 1
```

可以让向量`x`额外地保存一个`theta`属性，
这样的属性常常成为“元数据”(meta data)，
比如，
用来保存数据的说明、模拟数据的真实模型参数，等等。

### 8.3.3 `names`属性

有元素名的向量、列表、数据框等都有`names`属性，
许多R函数的输出本质上也是列表，
所以也有`names`属性。
用`names(x)`的格式读取或设定。
如：

```
x <- 1:5
y <- x^2
lmr <- lm(y ~ x)
print(names(lmr))
##  [1] "coefficients"  "residuals"     "effects"       "rank"         
##  [5] "fitted.values" "assign"        "qr"            "df.residual"  
##  [9] "xlevels"       "call"          "terms"         "model"
```

对于没有元素名的向量`x`，`names(x)`的返回值是`NULL`。

### 8.3.4 `dim`属性

`dim`属性的存在表明对象是矩阵或一维、多维数组。
如：

```
x <- matrix(1:12, nrow=3, ncol=4)
attr(x, "dim") # 等同于dim(x)
## [1] 3 4
```

修改`dim`属性就将向量转换成矩阵（数组），
或修改了矩阵的性质，
元素按列次序重排填入新的矩阵。如：

```
x <- 1:4
dim(x) <- c(2,2)
x
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
```

R允许`dim`仅有一个元素，
这对应于一维向量，
与普通的没有`dim`属性的向量有区别。
另外要注意，
取矩阵子集时如果结果仅有一列或一行，
除非用了`drop=FALSE`选项，
结果不再有`dim`属性，
退化成了普通向量。

## 8.4 类属

R具有一定的面向对象语言特征，
其数据类型有一个`class`属性，
函数`class()`可以返回变量类型的类属，
比如

```
typeof(factor(c('F', 'M', 'M', 'F')))
## [1] "integer"
mode(factor(c('F', 'M', 'M', 'F')))
## [1] "numeric"
storage.mode(factor(c('F', 'M', 'M', 'F')))
## [1] "integer"
class(factor(c('F', 'M', 'M', 'F')))
## [1] "factor"
class(as.numeric(factor(c('F', 'M', 'M', 'F'))))
## [1] "numeric"
```

class属性是特殊的。
如果一个对象具有class属性，
某些所谓“通用函数(generic functions)”会针对这样的对象进行专门的操作，
比如，
`print()`函数在显示向量和回归结果时采用完全不同的格式。
这在其它程序设计语言中称为“重载”(overloading)。

class属性用来支持R的S3风格的类，
常用的有factor, 日期，日期时间，
数据框，tibble。
R还有S4、R6等风格的类。

## 8.5 `str()`函数

用`print()`函数可以显示对象内容。
如果内容很多，
显示行数可能也很多。
用`str()`函数可以显示对象的类型和主要结构及典型内容。例如

```
s <- 101:200
attr(s,'author') <- '李小明'
attr(s,'date') <- '2016-09-12'
str(s)
##  int [1:100] 101 102 103 104 105 106 107 108 109 110 ...
##  - attr(*, "author")= chr "李小明"
##  - attr(*, "date")= chr "2016-09-12"
```