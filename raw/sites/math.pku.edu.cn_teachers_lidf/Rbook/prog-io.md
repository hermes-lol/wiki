---
crawl_time: '2026-01-17 14:19:36'
framework: rbook
title: 15 R输入输出 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-io.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 15 R输入输出

## 15.1 输入输出的简单方法

### 15.1.1 简单的输出

用`print()`函数显示某个变量或表达式的值，
如

```
x <- 1.234
print(x)
## [1] 1.234
y <- c(1,3,5)
print(y[2:3])
## [1] 3 5
```

在命令行使用R时，
直接以变量名或表达式作为命令可以起到用`print()`函数显示的相同效果。

用`cat()`函数把字符串、变量、表达式连接起来显示，
其中变量和表达式的类型一般是标量或向量，不能是矩阵、列表等复杂数据。
如

```
cat("x =", x, "\n")
## x = 1.234
cat("y =", y, "\n")
## y = 1 3 5
```

注意`cat()`显示中需要换行需要在自变量中包含字符串`"\n"`，
即换行符。

`cat()`默认显示在命令行窗口，
为了写入指定文件中，
在`cat()`调用中用`file=`选项，
这时如果已有文件会把原有内容覆盖，
为了在已有文件时不覆盖原有内容而是在末尾添加，
在`cat()`中使用`append=TRUE`选项。
如:

```
cat("=== 结果文件 ===\n", file="res.txt")
cat("x =", x, "\n", file="res.txt", append=TRUE)
```

函数`sink()`可以用来把命令行窗口显示的运行结果转向保存到指定的文本文件中，
如果希望保存到文件的同时也在命令行窗口显示，
使用`split=TRUE`选项。如

```
sink("allres.txt", split=TRUE)
```

为了取消这样的输出文件记录，
使用不带自变量的`sink()`调用，如

```
sink()
```

在R命令行环境中定义的变量、函数会保存在工作空间中，
并在退出R会话时可以保存到硬盘文件中。
用`save()`命令要求把指定的若干个变量（直接用名字，不需要表示成字符串）
保存到用`file=`指定的文件中，
随后可以用`load()`命令恢复到工作空间中。
虽然允许保存多个变量到同一文件中，
但尽可能仅保存一个变量，
而且使用变量名作为文件名。
用`save()`保存的R特殊格式的文件是通用的，
不依赖于硬件和操作系统。
如

```
save(scores, file="scores.RData")
load("scores.RData")
```

保存多个变量，如`x`, `zeta`，命令如：

```
save(x, zeta, file="myvars20200315.RData")
```

或

```
save(list = c("x", "zeta"), file="myvars20200315.RData")
```

对于一个数据框，
可以用`write.csv()`或`readr::write_csv()`将其保存为逗号分隔的文本文件，
这样的文件可以很容易地被其它软件识别访问，
如Microsoft Excel软件可以很容易地把这样的文件读成电子表格。
用如

```
da <- tibble("name"=c("李明", "刘颖", "张浩"),
    "age"=c(15, 17, 16))
write_csv(da, path="mydata.csv")
```

结果生成的mydata.csv文件内容如下：

```
name,age
李明,15
刘颖,17
张浩,16
```

但是，在Microsoft的中文版Windows操作系统中，
默认编码是GB编码，
用`write_csv()`生成的CSV文件总是使用UTF-8编码，
系统中的MS Office 软件不能自动识别这样编码的CSV文件，
可以改用`write_excel_csv()`函数。

### 15.1.2 简单的输入

用`scan()`函数可以输入文本文件中的数值向量，
文件名用`file=`选项给出。
文件中数值之间以空格和换行分开。如

```
cat(1:12, "\n", file="d:/work/x.txt")
x <- scan("d:/work/x.txt")
```

程序中用全路径给出了输入文件位置，
注意路径中用了正斜杠`/`作为分隔符，
如果在MS Windows环境下使用`\`作为分隔符，
在R的字符串常量中`\`必须写成`\\`。

如果`scan()`中忽略输入文件参数，
此函数将从命令行读入数据。
可以在一行用空格分开多个数值，
可以用多行输入直到空行结束输入。

这样的方法也可以用来读入矩阵。
设文件`mat.txt`包含如下矩阵内容:

```
3  4  2
5 12 10
7  8  6
1  9 11
```

可以先把文件内容读入到一个R向量中，
再利用`matrix()`函数转换成矩阵，
注意要使用`byrow=TRUE`选项，
而且只要指定`ncol`选项，
可以忽略`nrow`选项。如

```
M <- matrix(scan("mat.txt", quiet=TRUE), ncol=3, byrow=TRUE)
M
```

`scan()`中的`quite=TRUE`选项使得读入时不自动显示读入的数值项数。

上面读入数值矩阵的方法在数据量较大的情形也可以使用，
与之不同的是，
`read.table()`或`readr::read_table()`函数也可以读入这样的数据，
但是会保存成数据框而不是矩阵，
而且`read.table()`函数在读入大规模的矩阵时效率很低。

## 15.2 读取CSV文件

### 15.2.1 CSV格式

对于保存在文本文件中的电子表格数据，
R可以用`read.csv()`, `read.table()`,
`read.delim()`, `read.fwf()`等函数读入,
但是建议在readr包的支持下用`read_csv()`,
`read_table2()`, `read_delim()`, `read_fwf()`等函数读入，
这些将读入的数据框保存为tibble类型，
tibble是数据框的一个变种，
改善了数据框的一些不适当的设计。
readr的读入速度比基本R软件的`read.csv()`等函数的速度快得多，
速度可以相差十倍，
也不自动将字符型列转换成因子，
不自动修改变量名为合法变量名，
不设置行名。

对于中小规模的数据，
CSV格式作为文件交换格式比较合适，
兼容性强，
各种数据管理软件与统计软件都可以很容易地读入和生成这样格式的文件，
但是特别大型的数据读入效率很低。

CSV格式的文件用逗号分隔开同一行的数据项，
一般第一行是各列的列名（变量名）。
对于数值型数据，
只要表示成数值常量形式即可。
对于字符型数据，
可以用双撇号包围起来，
也可以不用撇号包围。
但是，
如果数据项本身包含逗号，
就需要用双撇号包围。
例如，下面是一个名为`testcsv.csv`的文件内容，
其中演示了内容中有逗号、有双撇号的情况。

```
id,words
1,"PhD"
2,Master's degree 
3,"Bond,James"
4,"A ""special"" gift"
```

为读入上面的内容，只要用如下程序:

```
d <- read_csv("testcsv.csv")
```

读入的数据框显示如下:

```
# A tibble: 4 × 2
     id            words
  <int>            <chr>
1     1              PhD
2     2  Master's degree
3     3       Bond,James
4     4 A "special" gift
```

### 15.2.2 从字符串读入

`read_csv()`还可以从字符串读入一个数据框，如

```
d.small <- read_csv("name,x,y
John, 33, 95
Kim, 21, 64
Sandy, 49, 100
")
```

```
## Rows: 3 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): name
## dbl (2): x, y
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
d.small
```

```
## # A tibble: 3 × 3
##   name      x     y
##   <chr> <dbl> <dbl>
## 1 John     33    95
## 2 Kim      21    64
## 3 Sandy    49   100
```

### 15.2.3 `read_csv`选项

`read_csv()`的`skip=`选项跳过开头的若干行。
当数据不包含列名时，
只要指定`col_names=FALSE`，
变量将自动命名为`X1, X2, ...`，
也可以用`col_names=`指定各列的名字，如

```
d.small <- read_csv("John, 33, 95
Kim, 21, 64
Sandy, 49, 100
", col_names=c("name", "x", "y") )
```

```
## Rows: 3 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): name
## dbl (2): x, y
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
d.small
```

```
## # A tibble: 3 × 3
##   name      x     y
##   <chr> <dbl> <dbl>
## 1 John     33    95
## 2 Kim      21    64
## 3 Sandy    49   100
```

### 15.2.4 编码设置

CSV文件是文本文件，是有编码问题的，
尤其是中文内容的文件。
readr包的默认编码是UTF-8编码。
例如，文件[`data/bp.csv`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/data/bp.csv)以GBK编码（有时称为GB18030编码，
这是中文Windows所用的中文编码）保存了如下内容：

```
序号,收缩压
1,145
5,110
6, 未测
9,150
10, 拒绝
15,115
```

如果直接用`read_csv()`：

```
d <- read_csv("data/bp.csv")
```

可能在读入时出错，或者访问时出错。
为了读入用GBK编码的中文CSV文件，
需要利用`locale`参数和`locale()`函数：

```
d <- read_csv("data/bp.csv", 
  locale=locale(encoding="GBK"))
```

```
## Rows: 6 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): 收缩压
## dbl (1): 序号
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
d
```

```
## # A tibble: 6 × 2
##    序号 收缩压
##   <dbl> <chr> 
## 1     1 145   
## 2     5 110   
## 3     6 未测  
## 4     9 150   
## 5    10 拒绝  
## 6    15 115
```

### 15.2.5 缺失值设置

`read_csv()`将空缺的值读入为缺失值，
将”NA”也读入为缺失值。
可以用`na=`选项改变这样的设置。
也可以将带有缺失值的列先按字符型原样读入，
然后再进行转换。

比如，上面的bp.csv文件中，
先将血压列按字符型读入，
再增加一列转换为数值型的列，
非数值转换为`NA`:

```
d <- read_csv("data/bp.csv", 
  locale=locale(encoding="GBK"))
```

```
## Rows: 6 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): 收缩压
## dbl (1): 序号
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
d[["收缩压数值"]] <- as.numeric(d[["收缩压"]])
```

```
## Warning: NAs introduced by coercion
```

```
d
```

```
## # A tibble: 6 × 3
##    序号 收缩压 收缩压数值
##   <dbl> <chr>       <dbl>
## 1     1 145           145
## 2     5 110           110
## 3     6 未测           NA
## 4     9 150           150
## 5    10 拒绝           NA
## 6    15 115           115
```

### 15.2.6 各列类型设置

对每列的类型，
readr用前1000行猜测合理的类型，
并在读取后显示猜测的每列类型。

但是有可能类型改变发生在1000行之后。
`col_types`选项可以指定每一列的类型，
如`"col_double()"`, `"col_integer()"`,
`"col_character()"`, `"col_factor()"`, `"col_date()"`, `"col_datetime"`等。
`cols()`函数可以用来规定各列类型，
并且有一个`.default`参数指定缺省类型。
对因子，需要在`col_factor()`中用`lelvels=`指定因子水平。

可以复制readr猜测的类型作为`col_types`的输入，
这样当数据变化时不会因为偶尔猜测错误而使得程序出错。如

```
d <- read_csv("data/bp.csv", locale=locale(encoding="GBK"),
    col_types=cols(
      `序号` = col_integer(),
      `收缩压` = col_character()
      ))
d
```

```
## # A tibble: 6 × 2
##    序号 收缩压
##   <int> <chr> 
## 1     1 145   
## 2     5 110   
## 3     6 未测  
## 4     9 150   
## 5    10 拒绝  
## 6    15 115
```

当猜测的文件类型有问题的时候，
可以先将所有列都读成字符型，
然后用`type_convert()`函数转换，
如：

```
d <- read_csv("filename.csv",
              col_types=cols(.default = col_character()))
d <- type_convert(d)
```

读入有错时，对特大文件可以先少读入一些行，
用`nmax=`可以指定最多读入多少行。
调试成功后再读入整个文件。

### 15.2.7 因子类型设置

设文件[`data/class.csv`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/data/class.csv)内容如下：

```
name,sex,age,height,weight
Alice,F,13,56.5,84
Becka,F,13,65.3,98
Gail,F,14,64.3,90
Karen,F,12,56.3,77
Kathy,F,12,59.8,84.5
Mary,F,15,66.5,112
Sandy,F,11,51.3,50.5
Sharon,F,15,62.5,112.5
Tammy,F,14,62.8,102.5
Alfred,M,14,69,112.5
Duke,M,14,63.5,102.5
Guido,M,15,67,133
James,M,12,57.3,83
Jeffrey,M,13,62.5,84
John,M,12,59,99.5
Philip,M,16,72,150
Robert,M,12,64.8,128
Thomas,M,11,57.5,85
William,M,15,66.5,112
```

最简单地用`read_csv()`读入上述CSV文件，程序如:

```
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

从显示看出，
读入后显示了每列的类型。
对性别变量，没有自动转换成因子，
而是保存为字符型。
为了按自己的要求转换各列类型，
用了`read_csv()`的`coltypes=`选项和`cols()`函数如下：

```
d.class <- read_csv(
  "data/class.csv", 
  col_types=cols(
  .default = col_double(),
  name=col_character(),
  sex=col_factor(levels=c("M", "F")) ))
str(d.class)
```

```
## spec_tbl_df [19 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
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

其中`str()`函数可以显示数据框的行数(obs.)和变量数(variables)，
以及每个变量（列）的类属等信息。

### 15.2.8 读入日期

设文件[`data/dates.csv`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/data/dates.csv)中包含如下内容，并设其文件编码为GBK:

```
序号,出生日期,发病日期
1,1941/3/8,2007/1/1
2,1972/1/24,2007/1/1
3,1932/6/1,2007/1/1
4,1947/5/17,2007/1/1
5,1943/3/10,2007/1/1
6,1940/1/8,2007/1/1
7,1947/8/5,2007/1/1
8,2005/4/14,2007/1/1
9,1961/6/23,2007/1/2
10,1949/1/10,2007/1/2
```

可以先把日期当作字符串读入:

```
d.dates <- read_csv('data/dates.csv', 
  locale=locale(encoding="GBK"))
```

```
## Rows: 10 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): 出生日期, 发病日期
## dbl (1): 序号
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

然后用`lubridate::ymd()`函数转换为R日期类型：

```
d.dates[["出生日期ct"]] <- lubridate::ymd(
  d.dates[["出生日期"]], tz='Etc/GMT-8')
d.dates[["发病日期ct"]] <- lubridate::ymd(
  d.dates[["发病日期"]], tz='Etc/GMT-8')
```

也可以用R本身的`as.POSIXct`函数转换：

```
d.dates[["出生日期ct"]] <- as.POSIXct(
  d.dates[["出生日期"]], format='%Y/%m/%d', tz='Etc/GMT-8')
d.dates[["发病日期ct"]] <- as.POSIXct(
  d.dates[["发病日期"]], format='%Y/%m/%d', tz='Etc/GMT-8')
```

这时保存的是POSIXct类型。
经过转换后的数据为:

```
knitr::kable(d.dates)
```

| 序号 | 出生日期 | 发病日期 | 出生日期ct | 发病日期ct |
| --- | --- | --- | --- | --- |
| 1 | 1941/3/8 | 2007/1/1 | 1941-03-08 | 2007-01-01 |
| 2 | 1972/1/24 | 2007/1/1 | 1972-01-24 | 2007-01-01 |
| 3 | 1932/6/1 | 2007/1/1 | 1932-06-01 | 2007-01-01 |
| 4 | 1947/5/17 | 2007/1/1 | 1947-05-17 | 2007-01-01 |
| 5 | 1943/3/10 | 2007/1/1 | 1943-03-10 | 2007-01-01 |
| 6 | 1940/1/8 | 2007/1/1 | 1940-01-08 | 2007-01-01 |
| 7 | 1947/8/5 | 2007/1/1 | 1947-08-05 | 2007-01-01 |
| 8 | 2005/4/14 | 2007/1/1 | 2005-04-14 | 2007-01-01 |
| 9 | 1961/6/23 | 2007/1/2 | 1961-06-23 | 2007-01-02 |
| 10 | 1949/1/10 | 2007/1/2 | 1949-01-10 | 2007-01-02 |

也可以用R本身的`as.Date`函数转换：

```
d.dates[["出生日期ct"]] <- as.Date(
  d.dates[["出生日期"]], format='%Y/%m/%d')
d.dates[["发病日期ct"]] <- as.Date(
  d.dates[["发病日期"]], format='%Y/%m/%d')
```

这时保存的是Date类型。

上面将日期先读入为字符型再转换是比较保险的做法。
还可以直接在`read_csv()`函数中指定某列为`col_date()`：

```
d.dates <- read_csv(
  'data/dates.csv', 
  locale=locale(encoding="GBK"),
  col_types=cols(
    `序号`=col_integer(),
    `出生日期`=col_date(format="%Y/%m/%d"),
    `发病日期`=col_date(format="%Y/%m/%d")
  ))
print(d.dates)
```

```
## # A tibble: 10 × 3
##     序号 出生日期   发病日期  
##    <int> <date>     <date>    
##  1     1 1941-03-08 2007-01-01
##  2     2 1972-01-24 2007-01-01
##  3     3 1932-06-01 2007-01-01
##  4     4 1947-05-17 2007-01-01
##  5     5 1943-03-10 2007-01-01
##  6     6 1940-01-08 2007-01-01
##  7     7 1947-08-05 2007-01-01
##  8     8 2005-04-14 2007-01-01
##  9     9 1961-06-23 2007-01-02
## 10    10 1949-01-10 2007-01-02
```

### 15.2.9 其它函数

除了`read_csv()`函数以外，
R扩展包readr还提供了其它的从文本数据读入数据框的函数，
如`read_table()`, `read_tsv()`, `read_fwf()`等。
这些函数读入的结果保存为tibble。
`read_table()`读入用空格作为间隔的文本文件，
同一行的两个数据项之间可以用一个或多个空格分隔，
不需要空格个数相同，
也不需要上下对齐。
`read_tsv()`读入用制表符分隔的文件。
`read_fwf()`读入上下对齐的文本文件。

另外，
`read_lines()`函数将文本文件各行读入为一个字符型向量。
`read_file()`将文件内容读入成一整个字符串，
`read_file_raw()`可以不管文件编码将文件读入为一个二进制字符串。

对特别大的文本格式数据，
data.table扩展包的`fread()`读入速度更快。

readr包的`write_excel_csv()`函数将tibble保存为csv文件，
总是使用UTF-8编码，结果可以被MS Office读取。

文本格式的文件都不适用于大型数据的读取与保存。
大型数据可以通过数据库接口访问，
可以用R的`save()`和`load()`函数按照R的格式访问，
还有一些特殊的针对大数据集的R扩展包。

## 15.3 保存与恢复R变量

基本R的`save()`命令可以将一个或者多个当前的R变量保存到文件中，
保存结果是经过压缩的，
在不同的R运行环境中兼容，
但是有可能依赖于R的软件版本。
如`save(x, y, file="mydata.RData")`。
随后可以用`load()`命令恢复这些变量到当前环境中，
如`load("mydata.RData")`可以恢复保存的变量`x`, `y`。

推荐使用`saveRDS()`,
它仅保存单个变量，
如`save(x, file="x.RData")`。
用`loadRDS()`载入并返回变量，
如`xold <- loadRDS("x.RData")`。
但是，`saveRDS()`保存的文件不适合用于不同计算机之间的数据交换。

R的arrow扩展包提供了对parquet数据文件格式的支持，
这种文件适用于在不同计算机、不同编程语言之间传递变量值，
包括数据框类型，
对于大规模数据的读写访问效率也比RDS格式文件高。

## 15.4 Excel表访问

### 15.4.1 借助于文本格式

为了把Microsoft Excel格式的数据读入到R中，
最容易的办法是在Excel软件中把数据表转存为CSV格式，
然后用`read.csv()`读取。

为了把R的数据框保存为Excel格式，
只要用`write.csv()`或`readr::write_excel_csv()`把数据框保存成CSV格式，
然后在Excel中打开即可。
例如，下面的程序演示了`write.csv()`的使用:

```
d1 <- tibble("学号"=c("101", "103", "104"),
             "数学"=c(85, 60, 73), 
             "语文"=c(90, 78, 80))
write.csv(d1, file="tmp1.csv", row.names=FALSE)
```

保存在文件中的结果显示如下：

```
学号,数学,语文
101,85,90
103,60,78
104,73,80
```

### 15.4.2 使用剪贴板

为了把Excel软件中数据表的选中区域读入到R中，
可以借助于剪贴板。
在Excel中复制选中的区域，然后在R中用如

```
myDF <- read.delim("clipboard")
```

就可以把选中部分转换成一个R的数据框。
如果复制的区域不含列名，
应加上`header=FALSE`选项。

这种方法也可以从R中复制数据到在Excel中打开的电子表格中，
例如

```
write.table(iris, file="clipboard", sep = "\t", col.names = NA)
```

首先把指定的数据框（这里是iris）写入到了剪贴板，
然后在用Excel软件打开的工作簿中只要粘贴就可以。
上述程序中`write.table()`函数把指定的数据框写入到指定的文件中,
其中的`col.names=NA`选项是一个特殊的约定，
这时保存的文件中第一行是列名，
如果有行名的话，行名所在的列对应的列名是空白的（但是存在此项）。

如果从R中复制数据框到打开的Excel文件中时不带行名，
但是带有列名，可以写这样一个通用函数

```
write.clipboard <- function(df){ 
  write.table(df, file="clipboard", sep="\t", 
              row.names=FALSE)
}
```

### 15.4.3 利用readxl扩展包

readxl扩展包的`readxl()`函数利用独立的C和C++库函数读入.xls和.xlsx格式的Excel文件。一般格式为

```
read_excel(path, sheet = 1, col_names = TRUE, 
    col_types = NULL, na = "",  skip = 0)
```

结果返回读入的表格为一个数据框。
各个自变量为：

* `path`: 要读入的Excel文件名，
  可以是全路径，
  路径格式要符合所用操作系统要求。
* `sheet`: 要读入哪一个工作簿(sheet)，
  可以是整数序号，
  也可以是工作簿名称的字符串。
  缺省为第一个工作簿。
* `col_names`: 是否用第一行内容作为列名，缺省为是。
* `col_types`: 可以在读入时人为指定各列的数据类型，缺省时从各列内容自动判断，有可能会不够准确。人为指定时，指定一个对应于各列的字符型向量，元素可取值为:

  * `blank`: 自动判断该列；
  * `numeric`: 数值型；
  * `date`: 日期；
  * `text`: 字符型。
* `na`: 指定作为缺失值的编码，
  默认完全空的单元格表示缺失值，
  如果还想包括`NA`, `N/A`，
  可表示为`na = c(““,”NA”, “N/A”, “na”, “n/a”)。

当Excel文件中包含多个工作簿时，
可以用`excel_sheets()`函数返回所有的工作簿名，
随后可以在`read_excel()`中用`sheet=`指定要读入的工作簿。
多个工作簿如果属于同一大数据集的不同观测，
可以用`bind_rows()`函数作上下合并。

writexl扩展包可以用来将数据框保存为Excel格式，
如`writexl::write_xlsx(df, path)`。

除了readxl和writexl扩展包，
XLConnect, xlsx, tidyxl也可以进行与Excel文件或者Excel软件的交互。
为了输出多个工作簿以及进行一些不同格式设置，
可以使用openxlsx扩展包。

## 15.5 用data.table包读入数据

tibble格式的数据框和readr包已经对R的原来的数据框(data.frame)类型做了很多改进。
data.table包则在高效读写、访问、存储上对数据框做了改进，
可以自动使用CPU的多个核心完成工作，
支持很大的表，
有丰富而强度的连接支持，
表长宽转换功能也高效、易用，
完全基于R软件，
不需要其它软件的支持。

`fread()`是类似`read.table()`和`readr::read_table()`的函数，
用来读入CSV等文本文件。
可以直接将日期读入为POSIXct格式。
可以自动判断分隔符是逗号还是空格、制表符，
这与读入文件的扩展名无关。
各列的类型从等间隔选取的100个100行片段推断，
而且有例外时会自动重新读取纠正。

为了读入中文GB编码的csv文件，
应先将其转换为UTF-8编码，
然后再读入。
见[15.7.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-io.html#p-io-enc-gb2utf)。

`fread()`的详细用法见帮助文档，
用`?fread`命令即可查询。

`fwrite()`函数类似于`write.csv()`函数，
但速度快得多，
可以充分利用CPU的多个核心进行处理。

读入一个班的19名学生信息的例子：

```
library(data.table)
```

```
## 
## Attaching package: 'data.table'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last
```

```
## The following object is masked from 'package:purrr':
## 
##     transpose
```

```
d <- fread(
  "data/class.csv", 
  header = TRUE)
head(d, 5) |> knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Alice | F | 13 | 56.5 | 84.0 |
| Becka | F | 13 | 65.3 | 98.0 |
| Gail | F | 14 | 64.3 | 90.0 |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |

读入一个远程文件的例子：

```
(flights <- fread(paste0(
  "https://raw.githubusercontent.com/",
  "Rdatatable/data.table/master/vignettes/",
  "flights14.csv") ) ) |>
  system.time()
## 用户 系统 流逝 
## 0.14 0.08 1.24 
dim(flights)
## [1] 253316     11
```

直接在程序中生成data.table的例子：

```
dt = data.table(
  ID = c("b","b","b","a","a","c"),
  a = 1:6,
  b = 7:12,
  c = 13:18
)
knitr::kable(dt)
```

| ID | a | b | c |
| --- | --- | --- | --- |
| b | 1 | 7 | 13 |
| b | 2 | 8 | 14 |
| b | 3 | 9 | 15 |
| a | 4 | 10 | 16 |
| a | 5 | 11 | 17 |
| c | 6 | 12 | 18 |

为了将data.frame类型的数据框df转换为data.table，
应使用`dt <- setDT(df)`，
`setDT()`的好处是不进行数据复制，
而是用引用的方式访问数据。
也可以使用`dt <- as.data.table(df)`，
这个版本会复制数据。

data.table的数据查询、汇总、转换，
参见[23.27](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/summary-manip.html#summ-trans-dtab)。

## 15.6 文件访问

### 15.6.1 连接

输入输出可以针对命令行，针对文件，R支持扩展的文件类型，
称为“连接(connection)”。

函数`file()`生成到一个普通文件的连接，
函数`url()`生成一个到指定的URL的连接，
函数`gzfile`, `bzfile`, `xzfile`,
`unz`支持对压缩过的文件的访问（不是压缩包，只对一个文件压缩）。
这些函数大概的用法如下：

```
file("path", open="", blocking=T,
     encoding = getOption("encoding"), 
     raw = FALSE)

url(description, open = "", blocking = TRUE,
    encoding = getOption("encoding"))

textConnection(description, open="r", 
    local = FALSE,
    encoding = c("", "bytes", "UTF-8"))

gzfile(description, open = "", 
       encoding = getOption("encoding"),
       compression = 6)

bzfile(description, open = "", 
       encoding = getOption("encoding"),
       compression = 9)

xzfile(description, open = "", 
       encoding = getOption("encoding"),
       compression = 6)

unz(description, filename, open = "",
    encoding = getOption("encoding"))
```

生成连接的函数不自动打开连接。
给定一个未打开的连接， 读取函数从中读取时会自动打开连接，
函数结束时自动关闭连接。
用`open()`函数打开连接，返回一个句柄；
生成连接时可以用`open`参数要求打开连接。
要多次从一个连接读取时就应该先打开连接，
读取完毕用`close`函数关闭。

函数`textConnection()`打开一个字符串用于读写。

在生成连接与打开连接的函数中用`open`参数指定打开方式，
取值为：

* `r`—文本型只读;
* `w`—文本型只写;
* `a`—文本型末尾添加;
* `rb`—二进制只读;
* `wb`—二进制只写;
* `ab`—二进制末尾添加;
* `r+`或`r+b`—允许读和写;
* `w+`或`w+b`—允许读和写，但刚打开时清空文件;
* `a+`或`a+b`—末尾添加并允许读。

### 15.6.2 文本文件访问

函数`readLines()`、`readr::read_lines()`、`scan()`可以从一个文本型连接读取。

给定一个打开的连接或文件名，
用`readLines`函数可以把文件各行读入为字符型向量的各个元素，
不包含文件中用来分开各行的换行标志。
可以指定要读的行数。 如

```
ll <- readLines("data/class.csv")
print(head(ll, 3))
```

```
## [1] "name,sex,age,height,weight" "Alice,F,13,56.5,84"        
## [3] "Becka,F,13,65.3,98"
```

用`readr`包的`read_lines()`如：

```
ll <- readr::read_lines("data/class.csv")
print(head(ll, 3))
```

```
## [1] "name,sex,age,height,weight" "Alice,F,13,56.5,84"        
## [3] "Becka,F,13,65.3,98"
```

用`writeLines`函数可以把一个字符型向量各元素作为不同行写入一个文本型连接。如

```
vnames <- strsplit(ll, ",")[[1]]
writeLines(vnames, "class-names.txt")
```

其中的第二参数应该是一个打开的文本型写入连接，
但是可以直接给出一个要写入的文件名。
用readr包的`write_lines()`:

```
readr::write_lines(vnames, "class-names.txt")
```

用scan函数读入用空格和空行分隔的字符串向量：

```
vnames <- scan(
  "class-names.txt", what=character(),
  quiet=TRUE)
vnames
## [1] "name"   "sex"    "age"    "height" "weight"
```

### 15.6.3 文本文件分批读写

`readLines()`、`readr::read_lines()`、
`writeLines()`、`readr::write_lines()`支持分批读写。
这需要预先打开要读取和写入的文件，
所有内容都处理一遍以后关闭读取和写入的文件。

使用`file()`函数打开文件用于读写，
使用`close()`函数关闭打开的文件。
打开文件时可以用`encoding=`指定编码。

下面的程序每次将GB18030编码的文件`cancer.csv`读入至多10行，
转为UTF-8写入`tmp.csv`中，
使用`readLines()`和`writeLines()`:

```
fin <- file("data/cancer.csv", "rt", encoding="GBK")
fout <- file("tmp.csv", "wt", encoding="UTF-8")
repeat{
  lines <- readLines(fin, n=10)
  cat("Read", length(lines), "lines.", "\n")
  if(length(lines)==0) break
  writeLines(lines, fout)
}
close(fout)
close(fin)
```

下面是使用`readr::read_lines()`的版本，
需要打开的文件为二进制格式，
并且需要在打开文件时和读取时都说明编码为GBK：

```
fin <- file("data/cancer.csv", "rb", encoding="GBK")
fout <- file("tmp.csv", "wt", encoding="UTF-8")
repeat{
  lines <- read_lines(fin, n_max=10, locale=locale(encoding="GBK"))
  cat("Read", length(lines), "lines.", "\n")
  if(length(lines)==0) break
  writeLines(lines, fout)
}
close(fout)
close(fin)
```

### 15.6.4 二进制文件访问

函数`save`用来保存R变量到文件，
函数`load`用来从文件中读取保存的R变量。

```
save(x, y, file="saved20210811.RData")
```

或

```
save(list=c("x", "y"), file="saved20210811.RData")
```

读入如

```
load("saved20210811.RData")
```

对较大的数据，常常每个保存文件仅保存一个变量，
并以变量名为文件名，如：

```
save(data01, file="data01.RData")
```

函数`readBin`和`writeBin`对R变量进行二进制文件存取。

如果要访问其它软件系统的二进制文件，
请参考R手册中的“R Data Import/Export Manual”。

### 15.6.5 字符型连接

函数`textConnection`打开一个字符串用于读取或写入，
是很好用的一个R功能。
可以把一个小文件存放在一个长字符串中，
然后用`textConnection`读取，如

```
fstr <-
"name,score
王芳,78
孙莉,85
张聪,80
"
d <- read.csv(textConnection(fstr), header=TRUE)
print(d)
##   name score
## 1 王芳    78
## 2 孙莉    85
## 3 张聪    80
```

读取用的`textConnection`的参数是一个字符型变量。
readr包的函数不支持从`textConnection`读取。

在整理输出结果时，经常可以向一个字符型变量连接写入，
最后再输出整个字符串值。
例如：

```
tc <- textConnection("sres", open="w")
cat("Trial of text connection.\n", file=tc)
cat(1:10, "\n", file=tc, append=TRUE)
close(tc)
print(sres)
```

注意写入用的`textConnection`
的第一个参数是保存了将要写入的字符型变量名的字符串，
而不是变量名本身，
第二个参数表明是写入操作，
使用完毕需要用`close`关闭。

## 15.7 中文编码问题

读写文本格式的数据，
或者用`readLines()`、`readr::read_lines()`读写文本文件，
可能会遇到中文编码不匹配的问题。
这里总结一些常用解决方法，
所用的操作系统为中文Windows10,
在RStudio中运行，R版本为4.2。
常见的中文编码有GBK(或GB18030, GB)，
UTF-8，
UTF-8有BOM标志等。

可以用`iconvlist()`查看R支持的编码名称。

假设有如下的含有中文的文件：

```
序号,收缩压
1,145
5,110
6, 未测
9,150
10, 拒绝
15,115
```

这个文件是在中文版MS Office的Excel软件中输入后，
用Office的“文件——另存为——.csv格式”生成的，
结果的编码是GBK编码，
或GB18030编码。
文件下载：
<data/bp.csv>

我们用工具软件将其转换成UTF-8无BOM格式，下载链接：
<data/bp-utf8nobom.csv>

转为UTF-8有BOM格式，下载链接：
<data/bp-utf8bom.csv>

### 15.7.1 用基本R的读取函数读取

对于GBK编码的文本文件，
较早期的R的基本函数`read.csv()`、`read.table()`、`readLines()`函数都可以正常读取，
但是，现在的版本就需要使用`file`函数打开并使用`encoding="GBK"`说明才能正常读取。
比如，GBK编码的`bp.csv`的读取：

```
read.csv(file("data/bp.csv", encoding="GBK"))
```

```
##   序号 收缩压
## 1    1    145
## 2    5    110
## 3    6   未测
## 4    9    150
## 5   10   拒绝
## 6   15    115
```

```
readLines(file("data/bp.csv", encoding="GBK"))
```

```
## [1] "序号,收缩压" "1,145"       "5,110"       "6, 未测"     "9,150"      
## [6] "10, 拒绝"    "15,115"
```

以UTF-8编码的文件，不论有没有BOM开头，
在当前的R版本中都可以正确读入而不需说明编码：

```
read.csv("data/bp-utf8nobom.csv")
```

```
##   序号 收缩压
## 1    1    145
## 2    5    110
## 3    6   未测
## 4    9    150
## 5   10   拒绝
## 6   15    115
```

```
readLines("data/bp-utf8bom.csv")
```

```
## [1] "序号,收缩压" "1,145"       "5,110"       "6, 未测"     "9,150"      
## [6] "10, 拒绝"    "15,115"
```

### 15.7.2 用readr包读取

readr包的`read_csv()`、`read_table2()`、`read_lines()`函数默认从UTF-8编码的文件中读取，
无BOM或者有BOM都可以。
如：

```
read_csv("data/bp-utf8nobom.csv")
```

```
## Rows: 6 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): 收缩压
## dbl (1): 序号
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## # A tibble: 6 × 2
##    序号 收缩压
##   <dbl> <chr> 
## 1     1 145   
## 2     5 110   
## 3     6 未测  
## 4     9 150   
## 5    10 拒绝  
## 6    15 115
```

```
read_csv("data/bp-utf8bom.csv")
```

```
## Rows: 6 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): 收缩压
## dbl (1): 序号
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## # A tibble: 6 × 2
##    序号 收缩压
##   <dbl> <chr> 
## 1     1 145   
## 2     5 110   
## 3     6 未测  
## 4     9 150   
## 5    10 拒绝  
## 6    15 115
```

```
read_lines("data/bp-utf8nobom.csv")
```

```
## [1] "序号,收缩压" "1,145"       "5,110"       "6, 未测"     "9,150"      
## [6] "10, 拒绝"    "15,115"
```

```
read_lines("data/bp-utf8bom.csv")
```

```
## [1] "序号,收缩压" "1,145"       "5,110"       "6, 未测"     "9,150"      
## [6] "10, 拒绝"    "15,115"
```

但是，对GBK编码的文件，不能直接读取：

```
read_csv("data/bp.csv")
## Error in nchar(x, "width") : 
## invalid multibyte string, element 1
```

```
read_lines("data/bp.csv")
```

```
## [1] "\xd0\xf2\xba\xc5,\xca\xd5\xcb\xf5ѹ" "1,145"                             
## [3] "5,110"                              "6, δ\xb2\xe2"                      
## [5] "9,150"                              "10, \xbeܾ\xf8"                      
## [7] "15,115"
```

```
## [1] "\xd0\xf2�\xc5,\xca\xd5\xcb\xf5ѹ" "1,145"
## [3] "5,110"                           "6, δ�\xe2"   
## [5] "9,150"                           "10, �ܾ\xf8"     
## [7] "15,115"
```

为了读取GBK(或GB18030)编码的文件，
在`read_csv()`和`read_lines()`函数中加入
`locale=locale(encoding="GBK")`选项，
GBK也可以写成GB18030：

```
read_csv("data/bp.csv", locale=locale(encoding="GBK"))
```

```
## Rows: 6 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): 收缩压
## dbl (1): 序号
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## # A tibble: 6 × 2
##    序号 收缩压
##   <dbl> <chr> 
## 1     1 145   
## 2     5 110   
## 3     6 未测  
## 4     9 150   
## 5    10 拒绝  
## 6    15 115
```

```
read_lines("data/bp.csv", locale=locale(encoding="GBK"))
```

```
## [1] "序号,收缩压" "1,145"       "5,110"       "6, 未测"     "9,150"      
## [6] "10, 拒绝"    "15,115"
```

### 15.7.3 输出文件的编码

`write.csv()`、`writeLines()`生成的含有中文的文件的编码在当前R版本(4.2以后)已经使用UTF-8编码，
在较早的R版本中默认为操作系统的默认中文编码。

readr的`write_csv()`、`write_lines()`函数生成的含有中文的文件的编码默认UTF-8无BOM。
如

```
write_csv(tibble("姓名"=c("张三", "李四")), "tmp.csv")
```

结果生成的文件编码为UTF-8无BOM，
这样的文件可以被R的`readr::read_csv()`正确读取，
但是不能被MS Excel软件正确读取。

`write_lines()`输出的文件也是编码为UTF-8无BOM。

`write_excel_csv()`可以生成带有UTF-8有BOM的CSV文件，
这样的文件可以被MS Office正确识别：

```
write_excel_csv(tibble("姓名"=c("张三", "李四")), "tmp2.csv")
```

### 15.7.4 编码转换

因为现在主要支持的编码是UTF-8，
所以GB编码的文件在某些软件中不能使用或者使用比较麻烦。
可以读入GB编码的文本文件，
并改写为UTF-8编码，
如：

```
inhdl <- file("data/bp.csv", encoding="GB18030")
lines <- readLines(inhdl)
close(inhdl)
writeLines(lines, con = "tmp-UTF.csv")
```

当文件较大时，
上述的将文件整体读入内存再转存的办法可能速度很慢。
这时，
可以分批读入、写出，如：

```
gb2utf <- function(infile, outfile, block_size=1000){
  fin <- file(infile, "rt", encoding="GB18030")
  fout <- file(outfile, "wt", encoding="UTF-8")
  repeat{
    lines <- readLines(fin, n = block_size)
    if(length(lines)==0) break
    writeLines(lines, fout)
  }
  close(fout)
  close(fin)
}
```

## 15.8 目录和文件管理

目录和文件管理函数:

* `getwd()`—返回当前工作目录。
* `setwd(path)`—设置当前工作目录。
* `list.files()`或`dir()`—查看目录中内容。
  `list.files(pattern=’.*[.]r$’)`可以列出所有以“.r”结尾的文件。
* `file.path()`—把目录和文件名组合得到文件路径。
* `file.info(filenames)`—显示文件的详细信息。
* `file.exists()`—查看文件是否存在。
* `file.access()`—考察文件的访问权限。
* `create.dir()`—新建目录。
* `file.create()`—生成文件。
* `file.remove()`或`unlink()`—删除文件。`unlink()`可以删除目录。
* `file.rename()`—为文件改名。
* `file.append()`—把两个文件相连。
* `file.copy()`—复制文件。
* `basename()`和`dirname()`— 从一个全路径文件名获取文件名和目录。