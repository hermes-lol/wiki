---
crawl_time: '2026-01-17 14:20:13'
framework: rbook
title: 51 数据库访问 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/db.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 51 数据库访问

## 51.1 介绍

对于大型的数据，
或者保存在多个表中的复杂数据，
经常会保存在一个数据库中。
数据库可以存在于专用的数据库服务器硬件上，
也可以是本机中的一个系统程序，
或者R直接管理的一个文件。

比较通用的数据库是关系数据库，
这样的数据库已经有很标准的设计理念和管理方法，
从用户使用的角度来看，
都可以使用一种专用的SQL语言来访问和管理。

常用的数据库架构包括客户服务器式的，
如Oracle, SQL Server，
比较适用于同一企业（机构）；
基于云服务的，如Snowflake，
可扩展性更好；
在一台电脑上仅适用于同一用户的，
如sqlite。

R通过扩展包可以访问许多种常用的关系数据库系统，
这些扩展包大多按照DBI扩展包规定的接口规范为用户提供了方便的访问功能。
R的dbplyr包使用与tidyverse风格相同的方法访问数据库，
不需要使用SQL语言。

## 51.2 SQLite数据库访问

SQLite是一个开源的、轻量级的数据库软件，
其数据库可以保存在本机的一个文件中，
R的RSQLite扩展包直接提供了SQLite数据库功能。
如果自己的研究数据规模很大，比如有几个GB，
不利于整体读入到计算机内存当中，
而每次使用时只需要其中的一个子集，
就可以采用保存数据到SQLite数据库文件的方法。

学会了在R中使用SQLite数据库，
其它的数据库也可以类似地使用。

### 51.2.1 NHANES数据

我们以NHANES扩展包的NHANES数据框为例演示在R中访问关系数据库的方法。
在数据库中一个数据框叫做一个表（table）。
NHANES表来自美国国家健康统计中心的调查数据，
该调查项目从1960年代开始对美国非住院非军人住户进行健康与营养方面的抽样调查，
从1999年开始大约5000各个年龄的受调查者每年在自己家接受调查员面访，
完成调查中的健康检查部分，
健康检查是在流动检查中心进行的。
抽样调查有复杂的抽样设计，
并不是简单随机抽样。
R扩展包中NHANES数据框经过了重抽样使其尽可能与目标总体的各种比例一致，
但数据应仅用作教学演示目的。
NHANES中有10000个观测，
是在2009-2010和2011-2012两次得到的，
有75个测试变量。
部分变量为：

* SurveyYr：用来区分两次考察。
* ID：受试者编码。
* Gender: 性别，male 或 female。
* Age: 年龄，80岁以上录入为80。
* Race1: 种族，可取Mexican, Hispanic, White, Black, 或 Other。
* Education：20岁及以上受试者的受教育级别，可取8thGrade, 9-11thGrade, HighSchool, SomeCollege, 或 CollegeGrad。
* MaritalStatus：婚姻状态，可取Married, Widowed, Divorced, Separated, NeverMarried, 或 LivePartner（与伴侣生活）。
* Weight: 体重（千克）。
* Height: 身高（厘米），对2岁以上。

### 51.2.2 初始化新SQLite数据库

用使用RSQLite，先载入RSQLite扩展包，
然后指定一个SQLite数据库文件（不需要是已经存在的），
用`dbConnect()`打开该数据库建立连接（如果没有就新建）：

```
library(RSQLite)
f_sqlite <- "_tmp/db2020.SQLITE"
con <- dbConnect(drv=SQLite(), dbname=f_sqlite)
```

可以用`dbWriteTable()`函数将NHANES数据框写入到打开的数据库中：

```
data(NHANES, package="NHANES")
dbWriteTable(conn=con, name="nh", value=NHANES)
```

写入后，数据库中的表名为nh。

### 51.2.3 查看数据库中的表

假设连接`con`还保持打开状态，
可以用`dbListTables()`查看数据库中有哪些表：

```
dbListTables(con)
## [1] "nh"
```

还可以用`dbExistsTable("tablename")`查看指定的表是否存在。

可以用`dbListFields()`查看某个表有哪些列，
数据库中称为域（fields）：

```
dbListFields(con, "nh")
##  [1] "ID"               "SurveyYr"         "Gender"          
##  [4] "Age"              "AgeDecade"        "AgeMonths" 
##  ………………
```

### 51.2.4 读入数据库中的表

为了从数据库读取整个数据表（数据框），
建立连接后用`dbReadTable()`函数读取，如：

```
d1 <- dbReadTable(
  conn=con, name="nh")
d1 |>
  count(SurveyYr)
## # A tibble: 2 x 2
##   SurveyYr     n
##   <chr>    <int>
## 1 2009_10   5000
## 2 2011_12   5000
```

两次调查各有5000个观测。

### 51.2.5 用SQL命令访问数据

还可以用`dbGetQuery()`执行SQL查询并以数据框格式返回查询结果。
比如，
仅返回SurveyYr和ID两列，
且仅选择男性：

```
d2 <- dbGetQuery(
  conn=con, 
  statement=paste(
    "SELECT SurveyYr, ID",
    "  FROM nh",
    "  WHERE Gender='male'"))
d2 |>
  count(SurveyYr)
## # A tibble: 2 x 2
##   SurveyYr     n
##   <chr>    <int>
## 1 2009_10   2475 
## 2 2011_12   2505
```

### 51.2.6 分批读入数据库中的表

对于特别大的表，
可能超出了计算机的内存，
无法整体读入到R当中。
如果只需要一个子集，
可以用上面的`dbGetQuery()`执行SQL命令在数据库中提取子集并仅将需要的子集读入到R中。
如果需要读入的部分还是超过了内存，
或者全部读入会使得处理速度很慢，
可以分批读入，分批处理。

先用`dbSendQuery()`发送SQL查询命令：

```
qry <- dbSendQuery(
  conn=con, 
  statement=paste(
    "SELECT Weight ",
    "  FROM nh",
    "  WHERE Gender='male'"))
```

然后，
用`dbHasCompleted()`检查是否已经将结果取完，
如果没有取完就用`dbFetch()`取出一定行数，
分段处理：

```
s <- 0
n <- 0
while(!dbHasCompleted(qry)){
  chunk = dbFetch(qry, n=1000)
  n <- n + sum(!is.na(chunk[["Weight"]]))
  s <- s + sum(chunk[["Weight"]], na.rm=TRUE)
}
cat("Average Weight = ", s/n, "\n")
```

需要释放查询对应的资源：

```
dbClearResult(qry)
```

这段程序只是举例说明如何分段读入、分段处理，
如果只是要计算nh表中男性的Weight变量平均值，
可以读入整个nh表或者nh表中男性的Weight变量的所有值然后计算，
或者用SQL命令计算。

### 51.2.7 关闭数据库连接

在数据库使用完毕以后，
要记得关闭数据库连接：

```
dbDisconnect(con)
```

### 51.2.8 其它数据库操作

* 删除表：`dbRemoveTable(conn, name)`
* 检查某个表是否存在：`dbExistsTable(conn, name)`
* 向一个表中插入保存在数据框中的一些行：`dbAppendTable(conn, name, value)`
* 执行SQL命令并返回受到影响的行数：`dbExecute(con, statement)`

## 51.3 duckdb数据库访问

duckdb与SQLite数据库类似，
也是作用于本地电脑的数据库，
比较适用于数据科学的大数据量快速访问需求。

生成一个临时的空数据库并建立连接如：

```
library(duckdb)
drv <- duckdb()
con <- dbConnect(drv)
```

为了生成的空数据库能够保存使用，
方法如：

```
library(duckdb)
drv <- duckdb(dbdir = "duckdb")
con <- dbConnect(drv)
```

这种方法也用于打开一个已有的duckdb数据库。

为了关闭打开的数据库连接和数据库服务器，
方法如：

```
library(duckdb)
dbDisconnect(don, shutdown = TRUE)
```

## 51.4 用dbplyr包访问数据库

在数据库建立连接后，
dbplyr包的函数在添加一个一个`con`(连接)参数后，
可以用与dplyr类似的方法访问并处理数据库中的表。

可以用`tbl`函数建立一个表的访问连接，
这个访问连接在R中使用时和一个数据框相同，
但仅在需要访问其中数据时，
才从数据库中将数据下载到R中，
所以类似于表的“视图”。
如(设`con`为已建立的连接，其中nh表是NHANES数据集)：

```
dv.nh <- tbl(con, "nh")
dim(dv.nh)
```

因为没有实际将数据下载，
所以现在显示的行数还是缺失值。

提取一个数据子集（也不会下载数据）：

```
dv.nh2 <- dv.nh |>
  select(ID, Gender, Age) |>
  filter(SurveyYr == "2011_12")
```

计算男女2011年观测频数：

```
dv.nh |>
  filter(SurveyYr == "2011_12") |>
  count(Gender)
```

```
# Source:   SQL [2 x 2]
# Database: DuckDB 0.6.0 [Lenovo@Windows 10 x64:R 4.2.1/:memory:]
  Gender     n
  <fct>  <dbl>
1 male    2505
2 female  2495
```

这些查询一般有对应的SQL命令，
可以用`show_query()`函数显示对应的SQL命令，
如：

```
dv.nh |>
  filter(SurveyYr == "2011_12") |>
  count(Gender) |>
  show_query()
```

```
<SQL>
SELECT Gender, COUNT(*) AS n
FROM nh
WHERE (SurveyYr = '2011_12')
GROUP BY Gender
```

为了实际将dblyr访问结果数据载入到R中，
应该用`collect()`函数，
如：

```
d.nh2_loc <- dv.nh |>
  select(ID, Gender, Age) |>
  filter(SurveyYr == "2011_12") |>
  collect()
```

许多简单的计数、总和等计算可以在数据库中完成，
不需要下载数据。
实际使用R进行各种计算时，
就用`collect()`把数据下载下来。

## 51.5 SQL命令简介

SQL是关系数据库查询和管理的专用语言，
关系数据库都支持SQL语言，
但彼此之间可能有一些技术性的差别。

用d.class数据框演示，有19个学生的姓名、性别、年龄、身高、体重。

```
str(d.class)
```

```
## spc_tbl_ [19 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ name  : chr [1:19] "Alice" "Becka" "Gail" "Karen" ...
##  $ sex   : chr [1:19] "F" "F" "F" "F" ...
##  $ age   : num [1:19] 13 13 14 12 12 15 11 15 14 14 ...
##  $ height: num [1:19] 56.5 65.3 64.3 56.3 59.8 66.5 51.3 62.5 62.8 69 ...
##  $ weight: num [1:19] 84 98 90 77 84.5 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   name = col_character(),
##   ..   sex = col_character(),
##   ..   age = col_double(),
##   ..   height = col_double(),
##   ..   weight = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

下面建立一个duckdb数据库，
将d.class保存在数据库中:

```
library(duckdb)
drv <- duckdb()
con <- dbConnect(drv)
dbWriteTable(con, name="class", value=d.class)
```

### 51.5.1 取行子集

取出所有行的命令：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT *",
    "  FROM class"
  ))
dim(dtmp)
## [1] 19  5
```

为了取出满足条件的某些行，
在SQL命令中加上`WHERE`子句：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT *",
    "  FROM class",
    "  WHERE sex='F' AND age <= 12"  ))
dtmp
##    name sex age height weight
## 1 Karen   F  12   56.3   77.0
## 2 Kathy   F  12   59.8   84.5
## 3 Sandy   F  11   51.3   50.5
```

### 51.5.2 取行列子集

可以在`SELECT`子句中指定要取出的列，列名之间用逗号分隔，
如：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT name, age, height, weight",
    "  FROM class",
    "  WHERE sex='F' AND age <= 12"))
dtmp
##    name age height weight
## 1 Karen  12   56.3   77.0
## 2 Kathy  12   59.8   84.5
## 3 Sandy  11   51.3   50.5
```

在WHERE子句中可以使用如下的比较和逻辑运算：

* `= ^= > < >= <= IN`
* 或`EQ NE GT LT GE LE IN`
* `IS NULL`表示“是空值”，与缺失值类似
* `BETWEEN a AND b`, 表示属于闭区间\([a,b]\)
* `AND`逻辑与，`OR`逻辑或，`NOT`逻辑非。

### 51.5.3 取某列的所有不同取值

在列名前加DISTINCT修饰，可以取出该列的所有不重复的值：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT DISTINCT sex",
    "  FROM class"))
dtmp
##   sex
## 1   F
## 2   M
```

多个变量的不重复的组合：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT DISTINCT sex, age",
    "  FROM class",
    "  WHERE age >= 15"))
dtmp
##   sex age
## 1   F  15
## 2   M  15
## 3   M  16
```

### 51.5.4 查询结果排序

用`ORDER BY`子句排序：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT name, height",
    "  FROM class",
    "  WHERE name IN ('Alice', 'Becka', 'Gail')",
    "  ORDER BY height DESC"))
dtmp
##    name height
## 1 Becka   65.3
## 2  Gail   64.3
## 3 Alice   56.5
```

在变量名后面加后缀`DESC`表示降序。

### 51.5.5 查询时计算新变量

在`SELECT`中用“表达式 AS 变量名”的格式计算新变量。
例如，class表中的身高以英寸为单位，
体重以磅为单位，分别转换为厘米和千克单位：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT name, round(height*2.54) AS hcm,",
    "    round(weight*0.4535924) as wkg",
    "  FROM class"))
head(dtmp, 3)
##    name hcm wkg
## 1 Alice 144  38
## 2 Becka 166  44
## 3  Gail 163  41
```

### 51.5.6 分组汇总

用`GROUP BY`子句对观测分组，
用`SUM`, `AVG`等统计函数计算汇总统计量。

统计函数有：

* `COUNT(*)`表示行数；
* `SUM(x)`求和；
* `AVG(x)`求平均；
* `MAX(x)`, `MIN(x)`求最大值和最小值。

例如：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT COUNT(*) AS n, AVG(height) AS avgh",
    "  FROM class"))
dtmp
##    n     avgh
## 1 19 62.33684
```

分组计算如：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT sex, COUNT(*) AS n, AVG(height) AS avgh",
    "  FROM class",
    "  GROUP BY sex"))
dtmp
##   sex  n     avgh
## 1   F  9 60.58889
## 2   M 10 63.91000
```

在用了GROUP BY以后，
对统计结果的筛选条件要写在`HAVING`子句中而不是用`WHERE`子句，如：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT age, COUNT(*) AS n",
    "  FROM class",
    "  GROUP BY age",
    "  HAVING COUNT(*)=1"))
dtmp
##   age n
## 1  16 1
```

### 51.5.7 将查询结果保存为新表

可以将查询结果保存在数据库中，而不是返回到R中。
这时，
需要用`dbExecute()`函数，
返回值是受到影响的行数：

```
for(tab in c("class1", "class2")){
  if(dbExistsTable(con, tab)){
    dbRemoveTable(con, tab)
  }
}

dbExecute(
  conn=con,
  statement=paste(
    "CREATE TABLE class1 AS",
    "  SELECT name, sex",
    "    FROM class"
  ))
## [1] 0

dbExecute(
  conn=con,
  statement=paste(
    "CREATE TABLE class2 AS",
    "  SELECT name, age",
    "    FROM class"
  ))
## [1] 0

dbListTables(con)
## [1] "class"  "class1" "class2"
```

在执行比较复杂的查询时，
可以用这种方法生成一些中间结果，
最后要删除这些作为中间结果的表。

### 51.5.8 从多个表查询

使用数据库的好处除了可以管理大型数据，
还有许多其它好处，
比如可以保证数据被修改时不会因断电、网络故障等出错，
可以并发读取或修改，
可以备份、恢复，
等等。

关系数据库经常需要将有关的信息保存在多张表中，
而不是使用一张大表，
这与数据库的设计理念有关，
可以减少数据冗余，
增强数据的一致性。
但是，
在使用这些信息时，
就需要从多张表进行查询，
称为表的连接查询。

最常见的连接查询是所谓内连接（INNER JOIN），
按照某一个或者某几个关键列将数据行对齐进行查询。
最容易理解的一对一的查询，
比如，
设学生的姓名、性别保存在class1表中，
姓名、年龄保存在class2表中，
没有重名，
则姓名可以作为关键列。
如果要查询女生年龄大于大于15的人，
就需要使用两张表：

```
dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT a.name, sex, age",
    "  FROM class1 AS a, class2 AS b",
    "  WHERE a.name = b.name AND sex='F' AND age >= 15"))
dtmp
##     name sex age
## 1   Mary   F  15
## 2 Sharon   F  15
```

上面的程序中WHERE子句中的`a.name = b.name`就是内连接。
在使用多张表时，
在`FROM`子句的多张表之间用逗号分隔，
一般在表名后用`AS`关键字引入一个临时的别名，
对两张表中共同的变量名如`name`需要用`a.name`和`b.name`区分。
连接两个表一般使用两列来匹配，

内连接也支持一对多的连接。
比如，
下面的表将`F`映射到`女`, `M`映射到`男`，
可以按`sex`连接：

```
dclass.sexm <- data.frame(
  sex = c("F", "F"), 
  sexc = c("女", "男"))
if(dbExistsTable(con, "sexm")){
  dbRemoveTable(con, "sexm")
}
dbWriteTable(con, name="sexm", value=dclass.sexm)

dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT a.name, a.sex, sexc",
    "  FROM class1 AS a, sexm AS b",
    "  WHERE a.sex = b.sex AND name IN ('Alice', 'Alfred')"))
dtmp
##    name sex sexc
## 1 Alice   F   女
## 2 Alice   F   男
```

在内连接时如果关键列的值匹配后形成多对多的连接，
将会做两两搭配组合。

内连接仅保留关键列能匹配的行。
如果希望保留不能匹配的行，
就要使用外连接，
分为：

* 左外连接，保留左表的所有行，右表仅保留匹配的行；
* 右外连接，保留右表的所有行，左表仅保留匹配的行；
* 全外连接，保留所有匹配和不匹配的行。

左外连接的程序示例：

```
d1 <- data.frame(
  id = c("a", "b"),
  x = c(11, 12))
d2 <- data.frame(
  id = c("a", "c"),
  y = c(21, 22))
```

```
knitr::kable(d1)
```

| id | x |
| --- | --- |
| a | 11 |
| b | 12 |

```
knitr::kable(d2)
```

| id | y |
| --- | --- |
| a | 21 |
| c | 22 |

```
dbWriteTable(con, name="table1", value=d1)
dbWriteTable(con, name="table2", value=d2)

dtmp <- dbGetQuery(
  conn=con,
  statement=paste(
    "SELECT a.id, x, y",
    "  FROM table1 AS a LEFT JOIN table2 AS b",
    "  ON A.id=b.id"))
dtmp
## id  x  y
## 1  a 11 21
## 2  b 12 NA
```

右外连接用`RIGHT JOIN`关键字，
全外连接用`FULL OUTER JOIN`关键字。
其它的数据库一般是可以支持的。

## 51.6 用dm包管理表

数据库中常常用多个表来存储某一系统的数据，
这些表通过键来连接。
[dm包](https://dm.cynkra.com/)可以获取数据库中表的连接关系，
使得从多个表访问信息变得容易；
也可以将R中的多个数据框建立联系，
然后上传到数据库中。

设表A和表B连接，
按A中的IDA变量与B中的IDB变量匹配连接，
则IDA列称为表A的“主键”（primary key），
而IDB列称为表A关于参考表B的一个“外键”（foreign key）。
有时键需要用多列的组合。

一个dm对象可以包含多个表，
这些表的主键和外键一般在从数据库查询时就已经确定，
如果不然，
可以在程序中指定每个表的主键和外键。
在用主键和外键规定了各个表的连接方式后，
可以作图显示这些连接。

指定了连接关系后，
可以自动化地产生连接后的大表用来做统计分析。

dm支持dplyr方式的访问和处理。

## 51.7 访问Oracle数据库

注：这部分内容编写较早，
有可能已经过时。

Oracle是最著名的数据库服务器软件。
要访问的数据库，
可以是安装在本机上的，
也可以是安装在网络上某个服务器中的。
如果是远程访问，
需要在本机安装Oracle的客户端软件。

假设已经在本机安装了Oracle服务器软件，
并设置orcl为本机安装的Oracle数据库软件或客户端软件定义的本地或远程Oracle数据库的标识，
test和oracle是此数据库的用户名和密码，
testtab是此数据库中的一个表。

为了在R中访问Oracle数据库服务器中的数据库，
在R中需要安装ROracle包。
这是一个源代码扩展包，
需要用户自己编译安装。
在MS Windows环境下，
需要安装R软件和RTools软件包（在CRAN网站的Windows版本软件下载栏目中）。
在MS Windows命令行窗口，用如下命令编译R的ROracle扩展包：

```
set OCI_LIB32=D:\oracle\product\10.2.0\db_1\bin
set OCI_INC=D:\oracle\product\10.2.0\db_1\oci\include
set PATH=D:\oracle\product\10.2.0\db_1\bin;C:\Rtools\bin;C:\Rtools\gcc-4.6.3\bin;"%PATH%"
C:\R\R-3.2.0\bin\i386\rcmd INSTALL ROracle_1.2-1.tar.gz
```

其中的前三个set命令设置了Oracle数据库程序或客户端程序链接库、头文件和可执行程序的位置，
第三个set命令还设置了RTools编译器的路径。
这些路径需要根据实际情况修改。
这里的设置是在本机运行的Oracle 10g服务器软件的情况。
最后一个命令编译ROracle扩展包，相应的rcmd程序路径需要改成自己的安装路径。

如果服务器在远程服务器上，
设远程服务器的数据库标识名为ORCL，
本机需要安装客户端Oracle instant client软件，
此客户端软件需要与服务器同版本号，
如`instantclient-basic-win32-10.2.0.5.zip`,
这个软件不需要安装，
只需要解压到一个目录如 `C:\instantclient_10_2`中。
在本机（以MS Windows操作系统为例）中，
双击系统，选择高级–环境变量，
增加如下三个环境变量：

```
NLS_LANG = SIMPLIFIED CHINESE_CHINA.ZHS16GBK
ORACLE_HOME = C:\instantclient_10_2
TNS_ADMIN = C:\instantclient_10_2
```

并在环境变量PATH的值的末尾增加Oracle客户端软件所在的目录 `verb|C:\instantclient_10_2`,
并与前面内容用分号分开。

然后，在client所在的目录 `C:\instantclient_10_2` 中增加如下内容的tnsnames.ora`文件

```
  orcl = 
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.102 )
     (PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```

其中HOST的值是安装Oracle服务器的服务器的IP地址，
orcl是一个服务器实例名,
能够在服务器端的tnsnames.ora文件中查到，
等号前面的orcl是对数据库给出的客户端别名，
这里就干脆用了和服务器端的数据库标识名相同的名字orcl。

不论是在本机的数据框服务器还是在本机安装设置好客户端后，
在R中用如下的程序可以读入数据库中的表：

```
libraryROracle)
drv <- dbDriver("Oracle")

conn <- dbConnect(drv, username="test", 
                  password="oracle",  dbname="orcl")

rs <- dbSendQuery(conn, "select * from testtab")
d <- fetch(rs)
```

可以用`dbGetTable()`取出一个表并存入R数据框中。
用`dbSendQuery()`发出一个SQL命令，
用`fetch()`可以一次性取回或者分批取回，
在表行数很多时这种方法更适用。

## 51.8 MySQL数据库访问

MySQL是高效、免费的数据库服务器软件，
在很多行业尤其是互联网行业占有很大的市场。
为了在R中访问MySQL数据库，
只要安装RMySQL扩展包（有二进制版本）。
现在性能更好的一个连接MySQL的扩展包是RMariaDB。

假设服务器地址在 192.168.1.111,
可访问的数据库名为 world,
用户为 test,
密码为 mysql。
设world库中有表country。

在R中要访问MySQL数据框，首先要建立与数据库服务器的连接:

```
library(RMySQL)
con <- dbConnect(RMySQL::MySQL(), 
    dbname="world",
    username="test", password="mysql",
    host="192.168.1.111")
```

下列代码列出world库中的所有表，
然后列出其中的country表的所有变量:

```
dbListTables(con)
dbListFields(con, "country")
```

下列代码取出country表并存入R数据框d.country中:

```
d.country <- dbReadTable(con, "country")
```

下列代码把R中的示例数据框USArrests写入MySQL库world的表arrests中:

```
data(USArrests)
dbWriteTable(con, "arrests", USArrests, 
    overwrite=TRUE)
```

当然，这需要用户对该库有写权限。

可以用`dbGetQuery()`执行一个SQL查询并返回结果，如

```
dbGetQuery(con, "select count(*) from arrests")
```

当表很大时，可以用`dbSendQuery()`发送一个SQL命令，
返回一个查询结果指针对象，
用`dbFetch()`从指针对象位置读取指定行数，
用`dbHasCompleted()`判断是否已读取结束。如

```
res <- dbSendQuery(con, "SELECT * FROM country")
while(!dbHasCompleted(res)){
  chunk <- dbFetch(res, n = 5)
  print(chunk[,1:2])
}
dbClearResult(res)
```

数据库使用完毕时，
需要关闭用`dbConnect()`打开的连接：

```
dbDisconnect(con)
```

## 51.9 利用RODBC访问Access数据库

扩展包RODBC在MS Windows操作系统中可以访问Excel、Access、dBase、FoxPro等微机数据库软件的数据库，
也可以在安装了需要的ODBC客户端后访问Oracle等数据库。
用odbc包访问远程数据库速度更快，
而且遵从DBI的接口规范，
RODBC不使用DBI接口规范。

假设有Access数据库在文件`c:/Friends/birthdays.mdb`中，
内有两个表Men和Women，
每个表包含域Year, Month, Day, First Name, Last Name, Death。
域名应尽量避免用空格。

下面的程序把女性记录的表读入为R数据框:

```
library(RODBC)
con <- odbcConnectAccess("c:/Friends/birthdays.mdb")
women <- sqlFetch(con, sqtable="Women")
close(con)
```

RODBC还有许多与数据库访问有关的函数，
比如，`sqlQuery()`函数可以向打开的数据库提交任意符合标准的SQL查询，
`sqlSave()`可以将R数据框保存到数据库中。

## 51.10 arrow包

parquet格式是一种大型数据高效存储、交换的格式。
Apache的arrow包可以高效地访问和分析大型数据，
包括parquet格式数据，
在R中可以用arrow扩展包调用Apache arrow的功能，
实现快速的数据存储、交换，
并可以实现对超过内存容量的大型数据的分析能力。
arrow扩展包提供了dplyr形式的工作方式。

```
library(tidyverse)
library(arrow)
```

```
library(NHANES)
data(NHANES, package="NHANES")
```

将NHANES数据集保存为parquet格式：

```
write_parquet(NHANES, "NHANES.parquet")
```

将parquet格式文件读入为一个Dataset类型，
这样的类型不会将数据全部载入内存，
而是在需要时从外部存储访问：

```
pq.nh <- read_parquet("NHANES.parquet")
```

随后可以用dplyr访问，如：

```
pq.nh |> 
  glimpse()
```

```
Rows: 10,000
Columns: 76
$ ID               <int> 51624, 51624, 51624, 51625,…
$ SurveyYr         <fct> 2009_10, 2009_10, 2009_10, …
$ Gender           <fct> male, male, male, male, fem…
$ Age              <int> 34, 34, 34, 4, 49, 9, 8, 45…
$ AgeDecade        <fct>  30-39,  30-39,  30-39,  0-…
.........
```

```
pq.nh |> 
  group_by(Gender) |>
  summarize(
    max_age = max(Age),
    median_age = median(Age)  )
```

```
# A tibble: 2 × 3
  Gender max_age median_age
  <fct>    <int>      <dbl>
1 female      80         37
2 male        80         36
```

当数据很大时，
仅记录要进行的分析和变换，
需要增加`collect()`步才真正执行并将结果转换为R的tibble。

将Dataset格式转换为CSV格式如：

```
write_dataset(
  pq.nh, "./", 
  basename_template="NHANES{i}.csv", 
  format="csv")
```

结果将在当前目录生成单个`NHANES0.csv`文件。
因为parquet格式主要用于大型数据，
所以通常用一个或多个分组变量将所有数据分组，
然后每组保存一个文件，
保存在分组变量形成的目录层次中。
在使用这样的大型数据时，
经常只需要使用其中的子集，
而分多个文件存储，
就可以仅访问涉及到的文件，
而且访问多个规模较小的文件也比较容易。
parquet格式是按列存储的，
所以比较适用于统计分析。

读入单个CSV文件如：

```
pq.nh2 <- open_dataset(
  "NHANES0.csv",
  partition = NULL,
  format = "csv",
  convert_options = CsvConvertOptions$create(
    col_types = schema(
      Race3 = string(),
      BMICatUnder20yrs = string(),
      Testosterone = float64(),
      TVHrsDay = string(),
      CompHrsDay = string())))
pq.nh2 |>
  glimpse()
```

```
FileSystemDataset with 1 csv file
10,000 rows x 76 columns
$ ID                <int64> 51624, 51624, 51624, 51625…
$ SurveyYr         <string> "2009_10", "2009_10", "200…
$ Gender           <string> "male", "male", "male", "m…
$ Age               <int64> 34, 34, 34, 4, 49, 9, 8, 4…
$ AgeDecade        <string> " 30-39", " 30-39", " 30-3…
$ AgeMonths         <int64> 409, 409, 409, 49, 596, 11…
$ Race1            <string> "White", "White", "White",…
$ Race3            <string> "", "", "", "", "", "", ""…
........
```

用`open_dataset`读入一个CSV文件，
并不会将所有观测读入内存，
而是保留在外部存储（硬盘）上，
先读入几千行用来判断每列的类型。
仅当需要时才从外部存储访问数据，
这使得对超过内存大小的数据也可以进行分析。
这种自动判断列的类型对因子类型可能会出错，
所以可以用上面的方法根据需要指定某些列的类型。
没有发现读入更多行以更好地判断列类型的选项。

## 51.11 层次数据

数据框是统计分析最常见、最实用的数据格式，
实际当中还有许多数据是层次的（树状的、嵌套的），
比如Quarto的YAML设置。

用来表示层次数据的格式也有很多，
比如，JSON，XML，YAML等。

在R中可以用列表表示层次数据，
因为列表的元素也可以是列表，
这样可以逐层嵌套。
在R中用jsonlite包访问JSON格式的数据，
可以用repurrrsive包辅助处理层次格式的数据。
可以用purrr包的`map`类函数对保存为嵌套列表的层次数据进行处理。
可以用yaml包读写YAML数据。

需要时，
应能够将层次数据转换为整洁格式的数据框。

```
library(tidyverse)
library(jsonlite)
```

```
## 
## Attaching package: 'jsonlite'
```

```
## The following object is masked from 'package:purrr':
## 
##     flatten
```

```
library(repurrrsive)
```

```
## Warning: package 'repurrrsive' was built under R version 4.2.2
```

### 51.11.1 嵌套列表

某个Quarto文件的YAML元数据如下：

```
title: "期末报告"
author: "张三"
output:
  html:
    toc: true
    number-sections: true
  pdf:
    number-sections: true
```

这就是嵌套数据，
可以用嵌套列表表示为：

```
lis_qmd <- list(
  "title" = "期末报告",
  "author" = "张三",
  "output" = list(
    "html" = list(
      "toc" = TRUE,
      "number-sections" = TRUE
    ),
    "pdf" = list(
      "number-sections" = TRUE
    )
  )
)
```

列表显示比较冗长，
可以用`utils::str()`获得较简单的显示：

```
str(lis_qmd)
```

```
## List of 3
##  $ title : chr "期末报告"
##  $ author: chr "张三"
##  $ output:List of 2
##   ..$ html:List of 2
##   .. ..$ toc            : logi TRUE
##   .. ..$ number-sections: logi TRUE
##   ..$ pdf :List of 1
##   .. ..$ number-sections: logi TRUE
```

在RStudio中，
可以在“Environment”窗格点击查看嵌套列表内容，
或者用“`View(变量名)`”的方法查看。

### 51.11.2 tibble中的列表

列表可以保存在tibble的列中，
参见§[25.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals.html#fnctnl-nest)。

多个类似的内容可以作为tibble表的多行保存，
每行都可以是嵌套数据。
例如：

```
tib_qmd <- tibble(
  id = c(1,2,3),
  title = c("作业", "期中报告", "期末报告"),
  output = list(
    list(
      "html" = list("toc" = TRUE)
    ),
    list(
      "html" = list("toc" = TRUE),
      "pdf" = list("number-sections" = TRUE)
    ),
    list(
      "html" = list("toc" = TRUE),
      "pdf" = list(
        "toc" = TRUE,
        "number-sections" = TRUE)
    )
  )
)
knitr::kable(tib_qmd)
```

| id | title | output |
| --- | --- | --- |
| 1 | 作业 | TRUE |
| 2 | 期中报告 | TRUE, TRUE |
| 3 | 期末报告 | TRUE, TRUE, TRUE |

可以用`tidyr::unnest_wider()`函数将列表列展开为多列，如：

```
tib_qmd |>
  unnest_wider(output) |>
  unnest_wider(col = c(html, pdf), names_sep="_")
```

```
## # A tibble: 3 × 5
##      id title    html_toc `pdf_number-sections` pdf_toc
##   <dbl> <chr>    <lgl>    <lgl>                 <lgl>  
## 1     1 作业     TRUE     NA                    NA     
## 2     2 期中报告 TRUE     TRUE                  NA     
## 3     3 期末报告 TRUE     TRUE                  TRUE
```

可以用`unnest_longer()`将列表列展开并堆叠起来。

可以用`hoist()`函数从列表列中仅提取需要的元素并保存为新列。

### 51.11.3 展开示例

#### 51.11.3.1 github数据

repurrrsive包的`gh_repos`是嵌套列表，
有6个列表元素，
每个列表元素代表一个开发者，
而每个开发者的数据又是列表，
其列表元素至多有30个，
每个元素代表一个github项目，
这些项目也是列表但有元素名，
格式统一。

如下的程序先将数据包装到tible列表列中，
然后用`unnest_longer()`将所有开发者的所有项目堆叠在一起，
形成了170行，每行为一个项目，
然后用`unnest_wider()`将每个项目内容从有名列表转换为数据框的多列。如：

```
tibble(json = gh_repos) |> # 将6位开发者数据转换为列表列的6行
  unnest_longer(json) |>   # 将每位开发者的项目放到不同行
  unnest_wider(json) |>    # 提取项目数据到不同列
  select(id, full_name, owner, description)
```

```
## # A tibble: 176 × 4
##          id full_name               owner             description               
##       <int> <chr>                   <list>            <chr>                     
##  1 61160198 gaborcsardi/after       <named list [17]> Run Code in the Background
##  2 40500181 gaborcsardi/argufy      <named list [17]> Declarative function argu…
##  3 36442442 gaborcsardi/ask         <named list [17]> Friendly CLI interaction …
##  4 34924886 gaborcsardi/baseimports <named list [17]> Do we get warnings for un…
##  5 61620661 gaborcsardi/citest      <named list [17]> Test R package and repo f…
##  6 33907457 gaborcsardi/clisymbols  <named list [17]> Unicode symbols for CLI a…
##  7 37236467 gaborcsardi/cmaker      <named list [17]> port of cmake to r        
##  8 67959624 gaborcsardi/cmark       <named list [17]> CommonMark parsing and re…
##  9 63152619 gaborcsardi/conditions  <named list [17]> <NA>                      
## 10 24343686 gaborcsardi/crayon      <named list [17]> R package for colored ter…
## # … with 166 more rows
```

结果中的`owner`还是嵌套列表，
可以再多执行一轮`|> unnest_wider(owner, names_sep="_")`。

#### 51.11.3.2 角色数据

repurrrsive包的`got_chars`变量是一个列表，
有30个元素，
每个元素代表了冰火之歌中的一个视角角色信息，
是有名列表，
本身就是比较像数据框结构的。

```
tibble(json = got_chars) |>  # 变成有30行的列表列
  unnest_wider(json) |>      # 每行展开成多列 
  select(id, name, gender, titles)
```

```
## # A tibble: 30 × 4
##       id name               gender titles   
##    <int> <chr>              <chr>  <list>   
##  1  1022 Theon Greyjoy      Male   <chr [2]>
##  2  1052 Tyrion Lannister   Male   <chr [2]>
##  3  1074 Victarion Greyjoy  Male   <chr [2]>
##  4  1109 Will               Male   <chr [1]>
##  5  1166 Areo Hotah         Male   <chr [1]>
##  6  1267 Chett              Male   <chr [1]>
##  7  1295 Cressen            Male   <chr [1]>
##  8   130 Arianne Martell    Female <chr [1]>
##  9  1303 Daenerys Targaryen Female <chr [5]>
## 10  1319 Davos Seaworth     Male   <chr [4]>
## # … with 20 more rows
```

还有许多列是列表列。
将其中的titles堆叠展开：

```
tibble(json = got_chars) |>  
  unnest_wider(json) |>      
  select(id, name, gender, titles) |>
  unnest_longer(titles)
```

```
## # A tibble: 59 × 4
##       id name              gender titles                                        
##    <int> <chr>             <chr>  <chr>                                         
##  1  1022 Theon Greyjoy     Male   "Prince of Winterfell"                        
##  2  1022 Theon Greyjoy     Male   "Lord of the Iron Islands (by law of the gree…
##  3  1052 Tyrion Lannister  Male   "Acting Hand of the King (former)"            
##  4  1052 Tyrion Lannister  Male   "Master of Coin (former)"                     
##  5  1074 Victarion Greyjoy Male   "Lord Captain of the Iron Fleet"              
##  6  1074 Victarion Greyjoy Male   "Master of the Iron Victory"                  
##  7  1109 Will              Male   ""                                            
##  8  1166 Areo Hotah        Male   "Captain of the Guard at Sunspear"            
##  9  1267 Chett             Male   ""                                            
## 10  1295 Cressen           Male   "Maester"                                     
## # … with 49 more rows
```

### 51.11.4 JSON格式

JSON(Javascript object notation)是许多网络接口的默认数据数据格式。
其基本数据类型与R相近但不完全相同。

JSON基本数据类型有：

* `null`，类似R的缺失值；
* `string`，字符串，必须使用双撇号而不能使用单撇号；
* `number`，包括R的整型与双精度型；
* `boolean`，可取`true`，`false`值。

JSON中的标量就是标量，
不像R那样将标量看成是长度为1的向量。
为了在JSON中保存向量，
可以用`array`数据格式，
将内容写在方括号内，如`["red", "blue", "yellow"]`，
对应于R的列表。
还可以用`object`数据格式，
这类似R的列表，
写在大括号内，
元素名与元素值用冒号分隔，
如`{"x": 1, "y": 2}`，
或`{"Name": "John", "age": 32}`。

jsonlite包的`read_json()`函数将一个JSON格式的文本文件读入到R中，
变成嵌套列表格式。
而函数`parse_json()`则直接输入一个JSON格式字符串，
将其转换为R格式，
如：

```
str(parse_json('1'))
```

```
##  int 1
```

```
str(parse_json('[1,2,3]'))
```

```
## List of 3
##  $ : int 1
##  $ : int 2
##  $ : int 3
```

```
str(parse_json('{"x": 1, "y": 2}'))
```

```
## List of 2
##  $ x: int 1
##  $ y: int 2
```

```
str(parse_json('
  {"Name": "John", 
  "age": 32, 
  "scores": [90, 85, 88]}'))
```

```
## List of 3
##  $ Name  : chr "John"
##  $ age   : int 32
##  $ scores:List of 3
##   ..$ : int 90
##   ..$ : int 85
##   ..$ : int 88
```

jsonlite的`fromJSON`在从JSON字符串转换时可以进行一些简化，
但最好还是显式地用`unnest_wider()`、`unnest_longer()`等转换。
如：

```
str(fromJSON('1'))
```

```
##  int 1
```

```
str(fromJSON('[1,2,3]'))
```

```
##  int [1:3] 1 2 3
```

```
str(fromJSON('{"x": 1, "y": 2}'))
```

```
## List of 2
##  $ x: int 1
##  $ y: int 2
```

```
str(fromJSON('
  {"Name": "John", 
  "age": 32, 
  "scores": [90, 85, 88]}'))
```

```
## List of 3
##  $ Name  : chr "John"
##  $ age   : int 32
##  $ scores: int [1:3] 90 85 88
```

将前面的`lis_qmd`用JSON字符串表示，
可写成：

```
json_qmd <- '{
  "title": "期末报告",
  "author": "张三",
  "output": {
    "html": {
      "toc": true,
      "number-sections": true
      },
    "pdf": {
      "number-sections": true
      }
    }
  }
'
```

转换：

```
json_qmd |>
  parse_json() |>
  str()
```

```
## List of 3
##  $ title : chr "期末报告"
##  $ author: chr "张三"
##  $ output:List of 2
##   ..$ html:List of 2
##   .. ..$ toc            : logi TRUE
##   .. ..$ number-sections: logi TRUE
##   ..$ pdf :List of 1
##   .. ..$ number-sections: logi TRUE
```

对应于前面`tib_qmd`的数据，用JSON格式可以写成：

```
json_qmd2 <- '[
  {"id": 1, "title": "作业", 
  "html": {"toc": true},
  "pdf": {"number-sections": true}},
  {"id": 2, "title": "期中报告", 
  "html": {"toc": true},
  "pdf": {"number-sections": true}},
  {"id": 2, "title": "期中报告", 
  "html": {"toc": true},
  "pdf": {"toc": true, "number-sections": true}}
]
'
```

转换为R的嵌套列表，然后再转换为tibble：

```
json_qmd2 |>
  parse_json() |>
  tibble(json = _) |>
  unnest_wider(json) |>
  unnest_wider(c(html, pdf), names_sep="_")
```

```
## # A tibble: 3 × 5
##      id title    html_toc `pdf_number-sections` pdf_toc
##   <int> <chr>    <lgl>    <lgl>                 <lgl>  
## 1     1 作业     TRUE     TRUE                  NA     
## 2     2 期中报告 TRUE     TRUE                  NA     
## 3     2 期中报告 TRUE     TRUE                  TRUE
```

JSON格式没有单独的日期和日期时间格式，
会保存成字符串，
可以随后用`readr::parse_date()`和`readr::parse_datetime()`转换。
有些数值会保存成字符串，
可以随后用`readr::parse_double()`转换。