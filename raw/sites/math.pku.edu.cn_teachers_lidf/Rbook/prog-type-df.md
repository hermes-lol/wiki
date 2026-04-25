---
crawl_time: '2026-01-17 14:19:38'
framework: rbook
title: 13 数据框 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-df.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 13 数据框

## 13.1 数据框

### 13.1.1 数据框定义

统计分析中最常见的原始数据形式是类似于数据库表或Excel数据表的形式。
这样形式的数据在R中叫做数据框(data.frame)。
数据框类似于一个矩阵，有\(n\)个横行、\(p\)个纵列，
但各列允许有不同类型：数值型向量、因子、字符型向量、日期时间向量。
同一列的数据类型相同。
在R中数据框是一个特殊的列表，
其每个列表元素都是一个长度相同的向量。
事实上，数据框还允许一个元素是一个矩阵，
但这样会使得某些读入数据框的函数发生错误。

函数`data.frame()`可以生成数据框，如

```
d <- data.frame(
    name=c("李明", "张聪", "王建"), 
    age=c(30, 35, 28), 
    height=c(180, 162, 175),
    stringsAsFactors=FALSE)
print(d)
```

```
##   name age height
## 1 李明  30    180
## 2 张聪  35    162
## 3 王建  28    175
```

`data.frame()`函数会将字符型列转换成因子，
加选项`stringsAsFactors=FALSE`可以避免这样的转换。

如果数据框的某一列为常数，
可以在`data.frame()`调用中仅给该列赋一个值，
生成的结果会自动重复这个值使得该列与其他列等长。

`nrow(d)`求`d`的行数，
`ncol(d)`或`length(d)`求`d`的列数。
数据框每列叫做一个变量，
每列都有名字，称为列名或变量名，
可以用`names()`函数和`colnames()`函数访问。
如

```
names(d)
## [1] "name"   "age"    "height"
colnames(d)
## [1] "name"   "age"    "height"
```

给`names(d)`或`colnames(d)`赋值可以修改列名。

用`as.data.frame(x)`可以把`x`转换成数据框。
如果`x`是一个向量，
转换结果是以`x`为唯一一列的数据框。
如果`x`是一个列表并且列表元素都是长度相同的向量，
转换结果中每个列表变成数据框的一列。
如果`x`是一个矩阵，转换结果把矩阵的每列变成数据框的一列。

数据框是一个随着R语言前身S语言继承下来的概念，
现在已经有一些不足之处，
tibble包提供了tibble类，
这是数据框的一个改进版本。
见[13.2](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-df.html#p-t-tibble)。

### 13.1.2 数据框显示

在R markdown文件中，
可以将数据框保存的表格显示为富文本格式，
方法是使用`knitr::kable()`函数。
详见[A.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-kable)。

程序示例如：

```
knitr::kable(d)
```

| name | age | height |
| --- | --- | --- |
| 李明 | 30 | 180 |
| 张聪 | 35 | 162 |
| 王建 | 28 | 175 |

### 13.1.3 数据框内容访问

数据框可以用矩阵格式访问，如

```
d[2,3]
## [1] 162
```

访问单个元素。

```
d[[2]]
## [1] 30 35 28
```

访问第二列，结果为向量。

```
d[,2]
## [1] 30 35 28
```

也访问第二列，但是这种作法与tibble不兼容，
所以应避免使用。

按列名访问列可用如

```
d[["age"]]
## [1] 30 35 28
d[,"age"]
## [1] 30 35 28
d$age
## [1] 30 35 28
```

其中`d[,"age"]`的用法与tibble不兼容，应避免使用。

因为数据框的一行不一定是相同数据类型，
所以数据框的一行作为子集，
结果还是数据框，而不是向量。如

```
x <- d[2,]; x
##   name age height
## 2 张聪  35    162
is.data.frame(x)
## [1] TRUE
```

可以同时取行子集和列子集，如

```
d[1:2, "age"]
## [1] 30 35
d[1:2, c("age", "height")]
##   age height
## 1  30    180
## 2  35    162
d[d[,"age"]>=30,]
##   name age height
## 1 李明  30    180
## 2 张聪  35    162
```

与矩阵类似地是，
用如`d[,"age"]`, `d[,2]`这样的方法取出的数据框的单个列是向量而不再是数据框。
但是，如果取出两列或者两列以上，
结果则是数据框。
如果取列子集时不能预先知道取出的列个数，
则子集结果有可能是向量也有可能是数据框，
容易造成后续程序错误。
对一般的数据框，
可以在取子集的方括号内加上`drop=FALSE`选项，
确保取列子集的结果总是数据框。
数据框的改进类型tibble在用`d[,ind]`这种语法取出列子集时保持为tibble格式，
为了取出tibble中的一列作为向量，
应该使用`d[[ind]]`这样的语法。

对数据框变量名按照字符串与集合进行操作可以实现复杂的列子集筛选，
但是建议使用[23](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/summary-manip.html#summary-manip)中所述的借助于tidyverse库的做法。

### 13.1.4 数据框的行名

数据框每一行可以有行名，
这在原始的S语言和传统的R语言中是重要的技术，
但是在改进类型tibble中则取消了行名，
需要用行名实现的功能一般改用`left_join()`函数实现。
建议新的R程序不要再利用数据框的行名功能。

比如，每一行定义行名为身份证号，则可以唯一识别各行。
下面的例子以姓名作为行名:

```
rownames(d) <- d$name
d$name <- NULL
d
##      age height
## 李明  30    180
## 张聪  35    162
## 王建  28    175
```

用数据框的行名可以建立一个值到多个值的对应表。
比如，有如下的数据框：

```
dm <- data.frame(
  "年级"=1:6,
  "出游"=c(0, 2, 2, 2, 2, 1),
  "疫苗"=c(TRUE, FALSE, FALSE, FALSE, TRUE, FALSE)
)
```

其中“出游”是每个年级安排的出游次数，
“疫苗”是该年级有全体无计划免疫注射。
把年级变成行名，可以建立年级到出游次数与疫苗注射的对应表：

```
rownames(dm) <- dm[["年级"]]
dm[["年级"]] <- NULL
```

这样，假设某个社区的小学中抽取的4个班的年级为
`c(2,1,1,3)`，
其对应的出游和疫苗注射信息可查询如下：

```
ind <- c(2,1,1,3)
dm[as.character(ind),]
##     出游  疫苗
## 2      2 FALSE
## 1      0  TRUE
## 1.1    0  TRUE
## 3      2 FALSE
```

结果中包含了不必要也不太合适的行名，可以去掉，以上程序改成：

```
ind <- c(2,1,1,3)
xx <- dm[as.character(ind),]
rownames(xx) <- NULL
xx
```

```
##   出游  疫苗
## 1    2 FALSE
## 2    0  TRUE
## 3    0  TRUE
## 4    2 FALSE
```

如果要从多个值建立映射，
比如，从省名与县名映射到经度、纬度，
可以预先用`paste()`函数把省名与县名合并在一起，
中间以适当字符（如`-``)分隔，
以这样的合并字符串为行名。

实际上，这个例子可以不用行名而是用`match()`函数实现。
`match(x, table)`对`x`的每个元素返回其在`table`中出现的位置序号。
找不到的元素返回`NA`。
如：

```
match(c(12, 15), 11:14)
## [1]  2 NA
```

对于上面的学校年级信息查询的例子，
可以首先查找每个班对应的年级在数据框中的行序号，
然后再返回这些行组成的数据框：

```
dm <- data.frame(
  "年级"=1:6,
  "出游"=c(0, 2, 2, 2, 2, 1),
  "疫苗"=c(T, F, F, F, T, F)
)
ind <- match(c(2,1,1,3), dm[["年级"]]); ind
## [1] 2 1 1 3
dm[ind,]
##     年级 出游  疫苗
## 2      2    2 FALSE
## 1      1    0  TRUE
## 1.1    1    0  TRUE
## 3      3    2 FALSE
```

对于代替数据框的tibble类型，
如果要实现行名的功能，
可以将行名作为单独的一列，
然后用dplyr包的`inner_join()`、`left_join()`、`full_join()`等函数横向合并数据集。
参见[23.23](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/summary-manip.html#dplyr-join)。

### 13.1.5 数据框与矩阵的区别

数据框不能作为矩阵参加矩阵运算。
需要时，可以用`as.matrix()`函数转换数据框或数据框的子集为矩阵。
如

```
d2 <- as.matrix(d[,c("age", "height")])
d3 <- crossprod(d2); d3
##          age height
## age     2909  15970
## height 15970  89269
```

这里`crossprod(A)`表示\(A^T A\)。

## 13.2 `tibble`类型

### 13.2.1 生成方法

tibble类型是一种改进的数据框。
readr包的`read_csv()`函数是`read.csv()`函数的一个改进版本，
它将CSV文件读入为tibble类型，如文件[`class.csv`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/class.csv)的读入:

```
library(tibble)
library(readr)
d.class <- read_csv("data/class.csv")
```

```
## Rows: 19 Columns: 5
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): name, sex
## dbl (3): age, height, weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
d.class
```

```
## # A tibble: 19 × 5
##    name    sex     age height weight
##    <chr>   <chr> <dbl>  <dbl>  <dbl>
##  1 Alice   F        13   56.5   84  
##  2 Becka   F        13   65.3   98  
##  3 Gail    F        14   64.3   90  
##  4 Karen   F        12   56.3   77  
##  5 Kathy   F        12   59.8   84.5
##  6 Mary    F        15   66.5  112  
##  7 Sandy   F        11   51.3   50.5
##  8 Sharon  F        15   62.5  112. 
##  9 Tammy   F        14   62.8  102. 
## 10 Alfred  M        14   69    112. 
## 11 Duke    M        14   63.5  102. 
## 12 Guido   M        15   67    133  
## 13 James   M        12   57.3   83  
## 14 Jeffrey M        13   62.5   84  
## 15 John    M        12   59     99.5
## 16 Philip  M        16   72    150  
## 17 Robert  M        12   64.8  128  
## 18 Thomas  M        11   57.5   85  
## 19 William M        15   66.5  112
```

tibble类型的类属依次为`spec_tbl_df`, `tbl_df`, `tbl`, `data.frame`：

```
class(d.class)
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```

用`as_tibble()`可以将一个数据框转换为tibble,
dplyr包提供了`filter()`、`select()`、`arrange()`、`mutate()`
等函数用来对tibble选取行子集、列子集，排序、修改或定义新变量，等等。
见[23](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/summary-manip.html#summary-manip)。

可以用`tibble()`函数生成小的tibble，如

```
d.bp <- tibble(
  `序号`=c(1,5,6,9,10,15),
  `收缩压`=c(145, 110, "未测", 150, "拒绝", 115)) 
knitr::kable(d.bp)
```

| 序号 | 收缩压 |
| --- | --- |
| 1 | 145 |
| 5 | 110 |
| 6 | 未测 |
| 9 | 150 |
| 10 | 拒绝 |
| 15 | 115 |

在调用`tibble()`函数时，
定义在后面的列可以调用前面的列的值。

用`tribble`可以按类似于CSV格式输入一个tibble, 如

```
tribble(
~`序号`,~`收缩压`,
1,145,
5,110,
6,NA,
9,150,
10,NA,
15,115
) |> knitr::kable()
```

| 序号 | 收缩压 |
| --- | --- |
| 1 | 145 |
| 5 | 110 |
| 6 | NA |
| 9 | 150 |
| 10 | NA |
| 15 | 115 |

注意`tribble()`中数据每行末尾也需要有逗号，
最后一行末尾没有逗号。
这比较适用于在程序中输入小的数据集。
一列中混杂数值型和字符型会出错。

实际上，`readr::read_csv()`也支持从一个多行字符串直接读入数据，
如：

```
readr::read_csv("序号,收缩压
1,145
5,110
6,NA
9,150
10,NA
15,115
") |> knitr::kable()
```

```
## Rows: 6 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (2): 序号, 收缩压
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

| 序号 | 收缩压 |
| --- | --- |
| 1 | 145 |
| 5 | 110 |
| 6 | NA |
| 9 | 150 |
| 10 | NA |
| 15 | 115 |

tibble与数据框的一大区别是在显示时不自动显示所有内容，
这样可以避免显示很大的数据框将命令行的所有显示都充满。
没有显示的列的列名会罗列在显示下方，
显示时每列的类型同时显示，也会显示tibble的行列数。
可以在`print()`用`n=`和`width=`选项指定要显示的行数和列数。
如果指定`print(df, n=Inf, width=Inf)`，
则等同于对数据框的默认显示方式，
能够显示整个tibble，
但列数过多时会分页显示。

也可以用`view(df)`在一个单独的窗口显示整个tibble。

tibble在生成或输入时不自动将字符型列转换为因子。

### 13.2.2 列子集问题

另外，用`d[,ind]`这样的单重的方括号取列子集时，
即使仅取一列，
从tibble取出的一列结果仍是tibble而不是向量，
为了提取一列为向量应使用双方括号格式或`$`格式。
因为这个原因有些原来的程序输入tibble会出错，
这时可以用`as.data.frame()`转换成数据框。
如：

```
d.bp[,"收缩压"]
```

```
## # A tibble: 6 × 1
##   收缩压
##   <chr> 
## 1 145   
## 2 110   
## 3 未测  
## 4 150   
## 5 拒绝  
## 6 115
```

如下的语法取出一列向量：

```
d.bp[["收缩压"]]
## [1] "145"  "110"  "未测" "150"  "拒绝" "115"
```

tibble在定义时不需要列名为合法变量名，
但是这样的变量名在作为变量名使用时需要用反单撇号包裹。

### 13.2.3 行名问题

tibble默认不使用行名(rownames)。
有行名的数据框用`as_tibble()`转换为tibble时，
可以用`rownames="变量名"`选项将行名转换成tibble的一列，
该列的变量名由选项值确定。

原来用行名完成的功能，
可以改用dplyr包的`left_join()`等函数，
这些函数进行数据框的横向连接。
详见[23](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/summary-manip.html#summary-manip)。

实际上，旧式数据框支持行名，有如下的缺点：

* 行名本身往往也是有效的数据，如身份证号，
  将有效数据以数据框中的列和行名两种不同形式保存，
  增加了复杂度；
* 为了使用某些变量辨识不同的行（观测），
  行名也具有局限性：
  行名必须是相互不同的，
  必须是字符型，
  而用来区分各个观测的变量有可能有多个，
  也可能不是字符型。
* 行名要求互不相同是有局限性的，
  如果用来辨识各行的变量有重复值，
  就可以构成对各行的一种自然的分组。

### 13.2.4 bibble和数据框的转换

因为有一些老的R函数仅支持data.frame，
所以有时需要将tibble转换为数据框。
设`dt`为tibble,
如果不需要添加行名，
则`as.data.frame(dt)`就可以转换为data.frame。

如果需要添加行名，
设`dt`中第一列要转换为行名，
转化的模板为

```
da <- as.data.frame(dt[,-1])
rownames(da) <- dt[[1]]
```

如果`dt`中要转化为行名的列为`id`列，
转化的模板为

```
da <- as.data.frame(dt[,-match("id", names(dt))]),
rownames(da) <- dt[["id"]]
```

反过来，
如果da是一个数据框，
要转化为tibble，
如果不需要保留行名，
则`as_tibble(da)`就可以。

如果需要将行名转换为结果的一列，
模板为

```
dt <- as_tibble(da, rownames="id")
```

则原来的行名变成了`dt`的`id`列。
必要的话，
tibble也支持行名，
这时，
只要用

```
dt <- as_tibble(da, rownames=NA)
```

就表示转换成tibble后仍保留原来的行名。

### 13.2.5 列表类型的列

tibble类型允许其中的列是列表类型，
这样，
该列的每个元素就可以是复杂类型，
比如建模结果（列表），
元素之间可以保存不等长的值。
如：

```
tibble(x = 1:3,
  y = list(1, 1:2, 1:3))
```

```
## # A tibble: 3 × 2
##       x y        
##   <int> <list>   
## 1     1 <dbl [1]>
## 2     2 <int [2]>
## 3     3 <int [3]>
```

## 13.3 练习

假设[`class.csv`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/class.csv)已经读入为R数据框d.class,
其中的sex列已经自动转换为因子。

1. 显示d.class中年龄至少为15的行子集；
2. 显示女生且年龄至少为15的学生姓名和年龄；
3. 取出数据框中的age变量赋给变量x。