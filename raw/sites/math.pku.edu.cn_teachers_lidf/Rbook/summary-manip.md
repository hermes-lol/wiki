---
crawl_time: '2026-01-17 14:20:51'
framework: rbook
title: 23 数据整理 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/summary-manip.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 23 数据整理

## 23.1 tidyverse系统

tidyverse是一系列用于数据输入输出、数据整理和数据汇总的R扩展包集合，
使用这些包遵循相近的编程风格，
比直接使用基本R编程要更直观、容易理解。
其中readr包用于读入数据，
tidyr包用于进行长、宽表转换，
dplyr包用于数据整理与汇总，
purr包进行map-reduce类操作，
等等。

假设数据以tibble格式保存。
数据集如果用于统计与绘图，
需要满足一定的格式要求，
([H. Wickham 2014](#ref-Wickham2014:tidy))称之为整洁数据(tidy data)，
基本要求为：

* 每行一个观测，代表一个样本点（个体），
  每列一个变量，代表个体的一个属性，
  每个单元格（每行、每列交叉的位置）恰好有一个数据值。
* 这些变量应该是不同的属性，
  而不是同一属性在不同年、月等时间的值分别放到单独的列。

数据集经常需要选行子集、选列子集、排序、定义新变量、横向合并、长宽转换等操作，
而且经常会用若干个连续的操作分步处理，
R的管道运算符`|>`和magrittr包的`%>%`特别适用于这种分步处理。

dplyr包和tidyr包定义了一系列“动词”，
可以用比较自然的方式进行数据整理。
较复杂的分组操作还可以利用purrr包的`map`类函数。

为了使用这些功能，可以载入`tidyverse`包，
则magrittr包，readr包，dplyr包和tidyr包都会被自动载入:

```
library(tidyverse)
```

下面的例子中用如下的一个班的学生数据作为例子，
保存在如下[`data/class.csv`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/data/class.csv)文件中：

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

读入为tibble:

```
d.class <- read_csv(
  "data/class.csv", 
  col_types=cols(
  .default = col_double(),
  name=col_character(),
  sex=col_factor(levels=c("M", "F"))
))
```

这个数据框有19个观测，
有如下5个变量：

* name
* sex
* age
* height
* weight

另一个例子数据集是R的NHANES扩展包提供的NHANES，
这是一个规模更大的示例数据框，
可以看作是美国扣除住院病人以外的人群的一个随机样本，
有10000个观测，有76个变量，
主题是个人的健康与营养方面的信息。
仅作为教学使用而不足以作为严谨的科研用数据。
原始数据的情况详见<http://www.cdc.gov/nchs/nhanes.htm>。
载入NHANES数据框：

```
library(NHANES)
data(NHANES)
```

```
print(dim(NHANES))
```

```
## [1] 10000    76
```

```
print(names(NHANES))
```

```
##  [1] "ID"               "SurveyYr"         "Gender"           "Age"             
##  [5] "AgeDecade"        "AgeMonths"        "Race1"            "Race3"           
##  [9] "Education"        "MaritalStatus"    "HHIncome"         "HHIncomeMid"     
## [13] "Poverty"          "HomeRooms"        "HomeOwn"          "Work"            
## [17] "Weight"           "Length"           "HeadCirc"         "Height"          
## [21] "BMI"              "BMICatUnder20yrs" "BMI_WHO"          "Pulse"           
## [25] "BPSysAve"         "BPDiaAve"         "BPSys1"           "BPDia1"          
## [29] "BPSys2"           "BPDia2"           "BPSys3"           "BPDia3"          
## [33] "Testosterone"     "DirectChol"       "TotChol"          "UrineVol1"       
## [37] "UrineFlow1"       "UrineVol2"        "UrineFlow2"       "Diabetes"        
## [41] "DiabetesAge"      "HealthGen"        "DaysPhysHlthBad"  "DaysMentHlthBad" 
## [45] "LittleInterest"   "Depressed"        "nPregnancies"     "nBabies"         
## [49] "Age1stBaby"       "SleepHrsNight"    "SleepTrouble"     "PhysActive"      
## [53] "PhysActiveDays"   "TVHrsDay"         "CompHrsDay"       "TVHrsDayChild"   
## [57] "CompHrsDayChild"  "Alcohol12PlusYr"  "AlcoholDay"       "AlcoholYear"     
## [61] "SmokeNow"         "Smoke100"         "Smoke100n"        "SmokeAge"        
## [65] "Marijuana"        "AgeFirstMarij"    "RegularMarij"     "AgeRegMarij"     
## [69] "HardDrugs"        "SexEver"          "SexAge"           "SexNumPartnLife" 
## [73] "SexNumPartYear"   "SameSex"          "SexOrientation"   "PregnantNow"
```

变量ID是受试者编号，
SurveyYr是调查年份，
同一受试者可能在多个调查年份中有数据。
变量中包括性别、年龄、种族、收入等人口学数据，
包括体重、身高、脉搏、血压等基本体检数据，
以及是否糖尿病、是否抑郁、是否怀孕、已生产子女数等更详细的健康数据，
运动习惯、饮酒、性生活等行为方面的数据。
这个教学用数据集最初的使用者是Cashmere高中的Michelle Dalrymple
和新西兰奥克兰大学的Chris Wild。

## 23.2 查看数据框一般信息

对一个数据框（包括tibble）`d`，
可以用`dim(d)`获得行、列数，
用`names(d)`获得各列的变量名，
用`str(d)`显示类型、大小、各列变量名以及类型、前几个值，
以及其他属性。
`dplyr::glimpse(d)`则可以显示数据框的大小、各列变量名、类型等，如：

```
glimpse(d.class)
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

## 23.3 用`filter()`选择行子集

数据框的任何行子集仍为数据框，即使只有一行而且都是数值也是如此。
行子集可以用行下标选取，
如`d.class[8:12,]`。
函数`head()`取出数据框的前面若干行，
`tail()`取出数据框的最后若干行。

dplyr包的`filter()`函数可以按条件选出符合条件的行组成的子集。
下例从`d.class`中选出年龄在13岁和13岁以下的女生：

```
d.class |>
  filter(sex=="F", age<=13) |>
  knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Alice | F | 13 | 56.5 | 84.0 |
| Becka | F | 13 | 65.3 | 98.0 |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |
| Sandy | F | 11 | 51.3 | 50.5 |

`filter()`函数第一个参数是要选择的数据框，
后续的参数是条件，
这些条件是需要同时满足的，
另外，
条件中取缺失值的观测自动放弃，
这一点与直接在数据框的行下标中用逻辑下标有所不同，
逻辑下标中有缺失值会在结果中产生缺失值。

`filter()`会自动舍弃行名，
如果需要行名只能将其转换成数据框的一列。

`filter()`的结果为行子集数据框。
用在管道操作当中的时候第一自变量省略（是管道传递下来的）。

## 23.4 按行序号选择行子集

基本R的utils包的函数`head(x, n)`可以用来选择数据框`x`前面`n`行，
`tail(x, n)`可以用来选择数据框`x`后面`n`行，如：

```
d.class |>
  head(n=5) |>
  knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Alice | F | 13 | 56.5 | 84.0 |
| Becka | F | 13 | 65.3 | 98.0 |
| Gail | F | 14 | 64.3 | 90.0 |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |

dplyr包的函数`slice(.data, ...)`可以用来选择指定序号的行子集，
正的序号表示保留，负的序号表示排除。如：

```
d.class |>
  slice(3:5) |>
  knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Gail | F | 14 | 64.3 | 90.0 |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |

还有几个`slice_xxx()`方便函数：

* `slice_head(n=5)`提取前n行；
* `slice_tail(n=5)`提取最后n行；
* `slice_min(x, n=1)`提取`x`值最小的n行；
* `slice_max(x, n=1)`提取`x`值最大的n行；
* `slice_sample(n=5)`随机无放回抽取`n`行，
  也可以用`sample_n(size = n)`函数。

这些函数也可以用`prop=0.1`取代`n=5`这样的写法，
输入一个要提取的行数的比例。

## 23.5 用`sample_n()`对观测随机抽样

dplyr包的`sample_n(tbl, size)`函数可以从数据集`tbl`中随机无放回抽取`size`行，如：

```
d.class |>
  sample_n(size = 3) |>
  knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Duke | M | 14 | 63.5 | 102.5 |
| Sandy | F | 11 | 51.3 | 50.5 |
| William | M | 15 | 66.5 | 112.0 |

`sample_n()`中加选项`replace=TRUE`可以变成有放回抽样。
可以用`weight`选项指定数据框中的一列作为抽样权重，
进行不等概抽样。

## 23.6 用`distinct()`去除重复行

有时我们希望得到一个或若干个变量组合的所有不同值。
dplyr包的`distinct()`函数可以对数据框指定若干变量，
然后筛选出所有不同值，
每组不同值仅保留一行。
指定变量名时不是写成字符串形式而是直接写变量名，
这是dplyr和tidyr包的特点。
例如，筛选出性别与年龄的所有不同组合：

```
d.class |>
  distinct(sex, age) |>
  knitr::kable()
```

| sex | age |
| --- | --- |
| F | 13 |
| F | 14 |
| F | 12 |
| F | 15 |
| F | 11 |
| M | 14 |
| M | 15 |
| M | 12 |
| M | 13 |
| M | 16 |
| M | 11 |

如果希望保留数据框中其它变量，
可以加选项`.keep_all=TRUE`。

下面的程序查看NHANES数据框中ID与SurveyYr的组合的不同值的个数：

```
NHANES |>
  distinct(ID, SurveyYr) |>
  nrow()
```

```
## [1] 6779
```

这个结果提示有些人在某一调查年中有多个观测。

## 23.7 用`drop_na()`去除指定的变量有缺失值的行

在进行统计建模时，
通常需要用到的因变量和自变量都不包含缺失值。
tidyr包的`drop_na()`函数可以对数据框指定一到多个变量，
删去指定的变量有缺失值的行。
不指定变量时有任何变量缺失的行都会被删去。

例如，将NHANES中所有存在缺失值的行删去后数出保留的行数，
原来有10000行：

```
NHANES |>
  drop_na() |>
  nrow()
```

```
## [1] 0
```

可见所有行都有缺失值。下面仅剔除AlcoholDay缺失的观测并计数：

```
NHANES |>
  drop_na(AlcoholDay) |>
  nrow()
```

```
## [1] 4914
```

基本stats包的`complete.cases`函数返回是否无缺失值的逻辑向量，
`na.omit`函数则返回无缺失值的观测的子集。

## 23.8 用`select()`选择列子集

dplyr包的`select()`选择列子集，并返回列子集结果。

可以指定变量名，如

```
d.class |>
  select(name, age) |>
  head(n=3) |>
  knitr::kable()
```

| name | age |
| --- | --- |
| Alice | 13 |
| Becka | 13 |
| Gail | 14 |

可以用冒号表示列范围，如

```
d.class |>
  select(age:weight) |>
  head(n=3) |>
  knitr::kable()
```

| age | height | weight |
| --- | --- | --- |
| 13 | 56.5 | 84 |
| 13 | 65.3 | 98 |
| 14 | 64.3 | 90 |

可以用数字序号表示列范围，如

```
d.class |>
  select(3:5) |>
  head(n=3) |>
  knitr::kable()
```

| age | height | weight |
| --- | --- | --- |
| 13 | 56.5 | 84 |
| 13 | 65.3 | 98 |
| 14 | 64.3 | 90 |

参数中前面写负号表示扣除，如

```
d.class |>
  select(-name, -age) |>
  head(n=3) |>
  knitr::kable()
```

| sex | height | weight |
| --- | --- | --- |
| F | 56.5 | 84 |
| F | 65.3 | 98 |
| F | 64.3 | 90 |

可以用`where(示性函数)`用输入的示性函数选择满足条件的列，
比如，
选择所有数据型列：

```
d.class |>
  select(where(is.numeric)) |>
  head(n=3) |>
  knitr::kable()
```

| age | height | weight |
| --- | --- | --- |
| 13 | 56.5 | 84 |
| 13 | 65.3 | 98 |
| 14 | 64.3 | 90 |

如果要选择的变量名已经保存为一个字符型向量，
可以用`all_of()`函数引入，如

```
vars <- c("name", "sex")
d.class |>
  select(all_of(vars)) |>
  head(n=3) |>
  knitr::kable()
```

| name | sex |
| --- | --- |
| Alice | F |
| Becka | F |
| Gail | F |

R的字符串函数（如`paste()`）和正则表达式函数可以用来生成变量名子集，
然后在`select`中配合`all_of()`使用。
`all_of()`要求指定的所有变量名都是数据框中存在的；
如果指定的变量有些可能是不存在的，
想将确实存在的那些变量选取进来，
应使用`any_of()`。

`select()`有若干个配套函数可以按名字的模式选择变量列，
如

* `starts_with("se"): 选择名字以`“se”`开头的变量列；
* `ends_with("ght"): 选择名字以`“ght”`结尾的变量列；
* `contains("no"): 选择名字中含有子串`“no”`的变量列；
* `matches("^[[:alpha:]]+[[:digit:]]+$")`，
  选择列名匹配某个正则表达式模式的变量列，
  这里匹配前一部分是字母，后一部分是数字的变量名，如`abc12`。
* `num_range("x", 1:3)`，选择`x1`, `x2`, `x3`。
* `everything()`: 代指所有选中的变量，
  这可以用来将指定的变量次序提前，
  其它变量排在后面。

选择变量时，
可以用“新变量=老变量”的格式同时改名，
如：

```
d.class |>
  select(id = name, gender=sex) |>
  head(n=3) |>
  knitr::kable()
```

| id | gender |
| --- | --- |
| Alice | F |
| Becka | F |
| Gail | F |

R函数subset也能对数据框选取列子集和行子集。

## 23.9 取出单个变量为向量

如果需要选择单个变量并使得结果为普通向量，
可以用dplyr包的`pull()`函数，如：

```
d.class |> 
  head(n=3) |>
  pull(name) |>
  paste(collapse=":")
```

```
## [1] "Alice:Becka:Gail"
```

`pull()`可以指定单个变量名，
也可以指定变量序号，
负的变量序号从最后一个变量数起。
缺省变量名和序号时取出最后一个变量。

如果要取出的变量名保存在一个字符型变量`varname`中，
可以用`pull(.data, !!sym(varname))`这种格式；
如果`varname`是函数的自变量，
可以用`pull(.data, {{ varname }})`这种格式。
或者，
先选择仅有一个变量的子数据框再用`pull()`，如：

```
varname <- "name"
d.class |> 
  head(n=3) |>
  select(one_of(varname)) |>
  pull() |>
  paste(collapse=":")
```

```
## [1] "Alice:Becka:Gail"
```

基于基本R，
也可以用`d.class[["name"]]`这种格式取出一列为普通变量，
如果`varname`保存了变量名，
可以用`d.class[[varname]]`这种格式。

不能用`d.class[,"name"]`这种方法，
对于tibble类型，
其结果仍是一个子数据框；
用`d.class["name"]`这种格式，
结果也是一个子数据框。

## 23.10 用`arrange()`排序

dplyr包的`arrange()`按照数据框的某一列或某几列排序，
返回排序后的结果，如

```
d.class |>
  arrange(sex, age) |>
  knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Thomas | M | 11 | 57.5 | 85.0 |
| James | M | 12 | 57.3 | 83.0 |
| John | M | 12 | 59.0 | 99.5 |
| Robert | M | 12 | 64.8 | 128.0 |
| Jeffrey | M | 13 | 62.5 | 84.0 |
| Alfred | M | 14 | 69.0 | 112.5 |
| Duke | M | 14 | 63.5 | 102.5 |
| Guido | M | 15 | 67.0 | 133.0 |
| William | M | 15 | 66.5 | 112.0 |
| Philip | M | 16 | 72.0 | 150.0 |
| Sandy | F | 11 | 51.3 | 50.5 |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |
| Alice | F | 13 | 56.5 | 84.0 |
| Becka | F | 13 | 65.3 | 98.0 |
| Gail | F | 14 | 64.3 | 90.0 |
| Tammy | F | 14 | 62.8 | 102.5 |
| Mary | F | 15 | 66.5 | 112.0 |
| Sharon | F | 15 | 62.5 | 112.5 |

用`desc()`包裹想要降序排列的变量，如

```
d.class |>
  arrange(sex, desc(age)) |>
  knitr::kable()
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Philip | M | 16 | 72.0 | 150.0 |
| Guido | M | 15 | 67.0 | 133.0 |
| William | M | 15 | 66.5 | 112.0 |
| Alfred | M | 14 | 69.0 | 112.5 |
| Duke | M | 14 | 63.5 | 102.5 |
| Jeffrey | M | 13 | 62.5 | 84.0 |
| James | M | 12 | 57.3 | 83.0 |
| John | M | 12 | 59.0 | 99.5 |
| Robert | M | 12 | 64.8 | 128.0 |
| Thomas | M | 11 | 57.5 | 85.0 |
| Mary | F | 15 | 66.5 | 112.0 |
| Sharon | F | 15 | 62.5 | 112.5 |
| Gail | F | 14 | 64.3 | 90.0 |
| Tammy | F | 14 | 62.8 | 102.5 |
| Alice | F | 13 | 56.5 | 84.0 |
| Becka | F | 13 | 65.3 | 98.0 |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |
| Sandy | F | 11 | 51.3 | 50.5 |

排序时不论升序还是降序，
所有的缺失值都自动排到末尾。

R函数`order()`可以用来给出数据框的排序次序，
然后以其输出为数据框行下标，
可以将数据框排序。

## 23.11 用`rename()`修改变量名

在dplyr包的`rename()`中用“新名字=旧名字”格式修改变量名，
如

```
d2.class <- d.class |>
  dplyr::rename(h=height, w=weight)
```

注意这样改名字不是对原始数据框修改而是返回改了名字后的新数据框。
也可以利用赋值运算符`->`写成：

```
d.class |>
  dplyr::rename(h=height, w=weight) ->
  d2.class
```

`rename()`这个函数可能出现在其它包中，
保险起见写成`dplyr::rename()`。

如果数据框中有大批变量名需要按某种规范统一修改，
可以考虑使用[`janitor::clean_names()`](https://rdrr.io/cran/janitor/man/clean_names.html)。

## 23.12 用`relocate()`调整变量次序

`relocate(df, var1, var2)`将指定的变量调整到最前面。
如：

```
d.class |>
  relocate(height, weight) |>
  head(n=3) |>
  knitr::kable()
```

| height | weight | name | sex | age |
| --- | --- | --- | --- | --- |
| 56.5 | 84 | Alice | F | 13 |
| 65.3 | 98 | Becka | F | 13 |
| 64.3 | 90 | Gail | F | 14 |

可以用`.before`指定一个变量名或变量序号，
使得指定的变量调整到此变量前面，
用`.after`指定一个变量，
使得指定的变量调整到此变量后面。
如：

```
d.class |>
  relocate(height, weight, .after=name) |>
  head(n=3) |>
  knitr::kable()
```

| name | height | weight | sex | age |
| --- | --- | --- | --- | --- |
| Alice | 56.5 | 84 | F | 13 |
| Becka | 65.3 | 98 | F | 13 |
| Gail | 64.3 | 90 | F | 14 |

## 23.13 用`mutate()`计算新变量

dplyr包的`mutate()`可以为数据框计算新变量，
返回含有新变量以及原变量的新数据框。
如

```
d.class |>
  mutate(
    rwh=weight/height, 
    sexc=if_else(sex=="F", "女", "男")) |>
  head(n=3) |>
  knitr::kable()
```

| name | sex | age | height | weight | rwh | sexc |
| --- | --- | --- | --- | --- | --- | --- |
| Alice | F | 13 | 56.5 | 84 | 1.486726 | 女 |
| Becka | F | 13 | 65.3 | 98 | 1.500766 | 女 |
| Gail | F | 14 | 64.3 | 90 | 1.399689 | 女 |

上面程序中的`if_else()`是dplyr包对基本R函数`ifelse()`的一个改进版本，
能够好处理缺失值和结果类属的问题。

用`mutate()`计算新变量时如果计算比较复杂，
也可以用多个语句组成复合语句，如：

```
d.class |>
  mutate(
    sexc = {
      x <- rep("男", length(sex))
      x[!is.na(sex) & sex == "F"] <- "女"
      x
    }  ) |>
  head(n=3) |>
  knitr::kable()
```

| name | sex | age | height | weight | sexc |
| --- | --- | --- | --- | --- | --- |
| Alice | F | 13 | 56.5 | 84 | 女 |
| Becka | F | 13 | 65.3 | 98 | 女 |
| Gail | F | 14 | 64.3 | 90 | 女 |

注意这样生成新变量不是在原来的数据框中添加，
原来的数据框没有被修改，
而是返回添加了新变量的新数据框。
R软件的巧妙设计保证了这样虽然是生成了新数据框，
但是与原来数据框重复的列并不会重复保存。

计算公式中可以包含对数据框中变量的统计函数结果，如

```
d.class |>
  mutate(
    cheight = height - mean(height)) |>
  knitr::kable()
```

新变量可以与老变量名相同，
这样就在输出中修改了老变量。

可以加`.before=1`选项，
使得新定义的变量添加到原有变量的最前面。
可以用`.after=变量名`使得新添加的变量放在指定的变量后面。
可以用选项`.keep="used"`仅保留新变量以及计算新变量时用到的变量。

dplyr定义了一些新的函数：

* `row_number()`返回每个观测的行号。
* `min_rank(x)`，计算秩统计量，冲突时按最小值，
  类似函数有`dense_rank(x)`, `percent_rank(x)`,
  `cume_rank(x)`等。
  基本R的`rank(x)`函数配合`ties.method`选项实现类似功能。
* `dplyr::lag(x)`将`x`看成一个时间为`1:n`的时间序列，
  然后在第\(i\)个时间点返回第\(i-1\)个时间点的值，
  如`dplyr::lag(c(2,3,5,7))`返回`NA, 2, 3, 5`。
  `dplyr::lead(x)`则在时间点\(i\)返回时间点\(i+1\)的值，
  如`dplyr::lead(c(2,3,5,7))`返回`3,5,7,NA`。
  注意`stats::lag()`函数有很大的差别，
  它主要针对时间序列数据，
  对一般向量反而不好用。

## 23.14 用`tranmute()`生成新变量的数据框

函数`transmute()`用法与`mutate()`类似，
但是仅保留新定义的变量，
不保留原来的所有变量。
如：

```
d.class |>
  transmute(
    height_cm = round(height*2.54),
    weight_kg = round(weight*0.4535924),
    bmi =  weight_kg / (height_cm / 100)^2) |>
  head(n=3) |>
  knitr::kable()
```

| height\_cm | weight\_kg | bmi |
| --- | --- | --- |
| 144 | 38 | 18.32562 |
| 166 | 44 | 15.96748 |
| 163 | 41 | 15.43152 |

可见结果中仅保留了新定义的变量。

定义新变量也可以直接为数据框的新变量赋值：

```
d.class[["rwh"]] <- d.class[["weight"]] / d.class[["height"]]
```

这样的做法与`mutate()`的区别是这样不会生成新数据框，
新变量是在原数据框中增加的。

给数据框中某个变量赋值为`NULL`可以修改数据框，
从数据框中删去该变量。

## 23.15 缺失值填补

从统计角度出发考虑如何填补数据中的缺失值是比较复杂的。
这里仅给出从编程考虑的做法。

有些非整洁的数据在某变量值持续多行保持不变时，
往往仅输入第一个值，
随后重复的值就输入为空白，
在读入为R数据框时自动变成缺失值。
对于这样的缺失值，
可以用`tidyr::fill()`函数，
可以指定需要填补的变量。
如：

| A | B | n |
| --- | --- | --- |
| 1 | 1 | 12 |
| NA | 2 | 10 |
| NA | 3 | NA |
| 2 | 1 | 15 |
| NA | 2 | NA |
| NA | 3 | 14 |

填补变量A的缺失值：

```
d.miss |>
  fill(A)
```

```
## # A tibble: 6 × 3
##       A     B     n
##   <dbl> <dbl> <dbl>
## 1     1     1    12
## 2     1     2    10
## 3     1     3    NA
## 4     2     1    15
## 5     2     2    NA
## 6     2     3    14
```

有时某个变量的缺失值实际上有含义，比如取值为0。
这时，可以用`dplyr::coalesce(x, default)`使`x`的缺失值用`default`取代。如：

```
d.miss |>
  mutate(
    x = coalesce(n, 0)
  )
```

```
## # A tibble: 6 × 4
##       A     B     n     x
##   <dbl> <dbl> <dbl> <dbl>
## 1     1     1    12    12
## 2    NA     2    10    10
## 3    NA     3    NA     0
## 4     2     1    15    15
## 5    NA     2    NA     0
## 6    NA     3    14    14
```

有时，有可能缺失值被输入成了一个特殊数值，
比如，
`x`的值仅取正数，
输入了`-1`就表示缺失值。
在读入CSV文件时，
如果所有缺失值都用`-1`表示，
可以在`read_csv()`中加选项`na = -1`。
如果不同列有不同的缺失值编码，
可以用`na_if(x, na_value)`将`x`中等于`na_value`的值改为缺失值，
可以用在`mutate()`中，
如`mutate(x = na_if(x, -1))`。

在涉及到交叉分组的数据时，
有可能某些交叉分组没有出现在数据中，
这可能对某些分析程序造成影响。
可以用`complete(A, B)`这样的方法将数据框中`A`, `B`的交叉分类补全。
如果这样不能包含所有可取值，
也可以生成一个包含所有组合的小数据框，
用`dplyr::full_join()`作外连接。
另外，
`dplyr::anti_join()`可以用变量取值全集的数据框来比对，
发现没有出现的变量取值。

在使用`dplyr::count()`对因子进行频数统计时，
数据中不出现的水平或水平组合默认在结果中也不出现，
但可以加选项`.drop = FALSE`使得这些水平或水平组合以频数0出现。
用`dplyr::group_by()`时，
也可以加`.drop = FALSE`使得没有观测的分组（但因子水平存在）也出现在结果中。
但更好的办法是在分组汇总完以后，
用`complete(A, B)`这样的办法补全汇总结果中的因子水平或水平组合。

## 23.16 用管道连接多次操作

管道运算符特别适用于对同一数据集进行多次操作。
例如，对`d.class`数据，先选出所有女生，
再去掉性别和age变量：

```
d.class |>
  filter(sex=="F") |>
  select(-sex, -age) |>
  knitr::kable()
```

| name | height | weight |
| --- | --- | --- |
| Alice | 56.5 | 84.0 |
| Becka | 65.3 | 98.0 |
| Gail | 64.3 | 90.0 |
| Karen | 56.3 | 77.0 |
| Kathy | 59.8 | 84.5 |
| Mary | 66.5 | 112.0 |
| Sandy | 51.3 | 50.5 |
| Sharon | 62.5 | 112.5 |
| Tammy | 62.8 | 102.5 |

管道操作的结果可以保存为新的tibble，如:

```
class_F <- d.class |>
  filter(sex=="F") |>
  select(-sex, -age)
```

也可以将赋值用`->`写在最后，如：

```
d.class |>
  filter(sex=="F") |>
  select(-sex, -age) -> class_F
```

如果管道传递的变量在下一层调用中不是第一自变量，
可以用`_`代表，
如：

```
d.class |>
  lm(weight ~ height, data = _) |>
  coef()
```

```
## (Intercept)      height 
##  -143.02692     3.89903
```

## 23.17 `expand_grid()`函数

在进行有多个因素的试验设计时，
往往需要生成多个因素完全搭配并重复的表格。
tidyr包的函数`expand_grid()`可以生成这样的重复模式。
基本R的`expand.grid()`功能类似。
基本R的gl函数提供了更简单的功能。

比如，下面的例子：

```
tidyr::expand_grid(
  group=1:3,
  subgroup=1:2,
  obs=1:2) |>
  knitr::kable()
```

| group | subgroup | obs |
| --- | --- | --- |
| 1 | 1 | 1 |
| 1 | 1 | 2 |
| 1 | 2 | 1 |
| 1 | 2 | 2 |
| 2 | 1 | 1 |
| 2 | 1 | 2 |
| 2 | 2 | 1 |
| 2 | 2 | 2 |
| 3 | 1 | 1 |
| 3 | 1 | 2 |
| 3 | 2 | 1 |
| 3 | 2 | 2 |

结果的数据框d有三个变量: group是大组，共分3个大组，每组4个观测；
subgroup是子组，在每个大组内分为2个子组，每个子组2个观测。
共有\(3 \times 2 \times 2 = 12\)个观测（行）。

## 23.18 宽表转换为长表

“整洁数据”的要求是每行为一个观测，
每列为一个变量（属性），
如果有同一个体在不同时间的测量值应该放在不同的观测中，
如果同一个体在同一时间的不同属性应该放在同一观测中。
满足这些要求的数据框才容易作为作图、汇总、统计建模函数的输入使用。

实际数据经常不满足上述的要求，
所以需要将数据框进行长宽格式的转换。
以典型的纵向数据为例，
每个受试者有多次随访的记录值，
如果每个受试者的所有随访记录值存放在一个观测中，
就称为宽表，
这不符合整洁数据要求；
如果每个受试者的随访记录值同一时间的多个属性用了多个观测保存，
称为长表，
也不符合整洁数据要求。

有些数据将受试者的每个属性放在一行，
每一列代表一个受试者，
这也不符合整洁数据要求。

tidyr的`pivot_longer()`可以将宽表转换成整洁数据，
`pivot_wider()`可以将长表转换成整洁数据。
基本R的`reshape()`函数也可以进行长宽格式的转换。
reshape2包也提供了较丰富的长宽表转换功能。
建议优先使用tidyr包的功能。

### 23.18.1 `pivot_longer`函数

tidyr的`pivot_longer()`函数可以将横向的多次观测堆叠在一列中。
例如，
下面的数据：

```
knitr::kable(dwide1)
```

| subject | 1 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- |
| 1 | 1 | NA | NA | NA |
| 2 | NA | 7 | NA | 4 |
| 3 | 5 | 10 | NA | NA |
| 4 | NA | NA | 9 | NA |

subject是受试者编号，
每个受试者有4次随访，
NA表示缺失。
数据分析和绘图用的函数一般不能直接使用这样的数据，
一般需要将4次测量合并在一列中作为分析变量，
将随访序号单独放在另外一列中。
用`pivot_longer()`函数实现：

```
dwide1 |>
  pivot_longer(`1`:`4`, 
     names_to = "time", 
     values_to = "response") |>
  knitr::kable()
```

| subject | time | response |
| --- | --- | --- |
| 1 | 1 | 1 |
| 1 | 2 | NA |
| 1 | 3 | NA |
| 1 | 4 | NA |
| 2 | 1 | NA |
| 2 | 2 | 7 |
| 2 | 3 | NA |
| 2 | 4 | 4 |
| 3 | 1 | 5 |
| 3 | 2 | 10 |
| 3 | 3 | NA |
| 3 | 4 | NA |
| 4 | 1 | NA |
| 4 | 2 | NA |
| 4 | 3 | 9 |
| 4 | 4 | NA |

选项`names_to`指定一个新变量名，
将原来的列标题转换为该变量的值；
选项`values_to`指定一个新变量名，
将原来的各个列对应的测量值保存在该变量名的列中。

注意原来的变量名不是合法R变量名，
所以在`pivot_longer()`中用反单撇号保护，
并用了冒号来表示变量范围，
也可以仿照`select`函数中指定变量名的方法将程序中的`` `1`:`4` ``替换为:

* `c("1", "2", "3", "4")`
* `` c(`1`, `2`, `3`, `4`) ``
* `-subject`
* `cols = all_of(vars)`, 其中`vars`被赋值为`c("1", "2", "3", "4")`。

如果转换结果中不希望保留那些NA，
可以加`values_drop_na=TRUE`:

```
dwide1 |>
  pivot_longer(`1`:`4`, 
     names_to = "time", 
     values_to = "response",
     values_drop_na = TRUE) |>
  knitr::kable()
```

| subject | time | response |
| --- | --- | --- |
| 1 | 1 | 1 |
| 2 | 2 | 7 |
| 2 | 4 | 4 |
| 3 | 1 | 5 |
| 3 | 2 | 10 |
| 4 | 3 | 9 |

### 23.18.2 从列名中提取数值

有时要合并的列名中带有数值，
需要将这些数值部分提取出来，
这时可以用`names_prefix`指定要去掉的非数值前缀，
用`names_transform`指定将列名转换为值时结果的类型，
`names_transform`是一个列表，
实现列表元素名到转换函数的映射。
例如，上述的dwide1数据框变成这样：

| subject | FU1 | FU2 | FU3 | FU4 |
| --- | --- | --- | --- | --- |
| 1 | 1 | NA | NA | NA |
| 2 | NA | 7 | NA | 4 |
| 3 | 5 | 10 | NA | NA |
| 4 | NA | NA | 9 | NA |

可以用如下程序将随访编号变成整数值存入一列：

```
dwide2 |>
  pivot_longer(cols = starts_with("FU"), 
     names_to = "time", 
     values_to = "response",
     names_prefix = "FU",
     names_transform = list(time = as.integer),
     values_drop_na = TRUE) |>
  knitr::kable()
```

| subject | time | response |
| --- | --- | --- |
| 1 | 1 | 1 |
| 2 | 2 | 7 |
| 2 | 4 | 4 |
| 3 | 1 | 5 |
| 3 | 2 | 10 |
| 4 | 3 | 9 |

其中的`cols = starts_with("FU")`也可以写成`cols = paste0("FU", 1:4)`。

考虑nlmeU扩展包的armd.wide数据框。
这个数据框中有240个受试者的信息，
每行为一个受试者，
包括5个时间点：visual0, visual4, visual12, visual24, visual52。
数据如：

```
library(nlmeU)
```

```
## 
## Attaching package: 'nlmeU'
```

```
## The following object is masked from 'package:stats':
## 
##     sigma
```

```
data(armd.wide)
knitr::kable(head(armd.wide, 3))
```

| subject | lesion | line0 | visual0 | visual4 | visual12 | visual24 | visual52 | treat.f | miss.pat |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 3 | 12 | 59 | 55 | 45 | NA | NA | Active | –XX |
| 2 | 1 | 13 | 65 | 70 | 65 | 65 | 55 | Active | —- |
| 3 | 4 | 8 | 40 | 40 | 37 | 17 | NA | Placebo | —X |

将其转换为长表格式并提取时间信息的程序如下：

```
armd.wide |>
  pivot_longer(
    cols = starts_with("visual"),
    names_to = "time",
    values_to = "visual",
    names_prefix = "visual",
    names_transform = list(time = as.integer),
    values_drop_na = TRUE) |>
  head(10) |> 
  knitr::kable()
```

| subject | lesion | line0 | treat.f | miss.pat | time | visual |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 3 | 12 | Active | –XX | 0 | 59 |
| 1 | 3 | 12 | Active | –XX | 4 | 55 |
| 1 | 3 | 12 | Active | –XX | 12 | 45 |
| 2 | 1 | 13 | Active | —- | 0 | 65 |
| 2 | 1 | 13 | Active | —- | 4 | 70 |
| 2 | 1 | 13 | Active | —- | 12 | 65 |
| 2 | 1 | 13 | Active | —- | 24 | 65 |
| 2 | 1 | 13 | Active | —- | 52 | 55 |
| 3 | 4 | 8 | Placebo | —X | 0 | 40 |
| 3 | 4 | 8 | Placebo | —X | 4 | 40 |

在转换的长表中，
希望增加基线测量值到每个观测中，
并希望将时间0, 4, 12, 24, 52增加一个时间序号0, 1, 2, 3, 4。
程序修改为：

```
armd.wide |>
  mutate(base = visual0) |>
  pivot_longer(
    cols = starts_with("visual"),
    names_to = "time",
    values_to = "visual",
    names_prefix = "visual",
    names_transform = list(time = as.integer),
    values_drop_na = TRUE) |>
  mutate(timep = as.integer(factor(time, levels=c(0, 4, 12, 24, 52))) - 1) |>
  head(10) |> 
  knitr::kable()
```

| subject | lesion | line0 | treat.f | miss.pat | base | time | visual | timep |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 3 | 12 | Active | –XX | 59 | 0 | 59 | 0 |
| 1 | 3 | 12 | Active | –XX | 59 | 4 | 55 | 1 |
| 1 | 3 | 12 | Active | –XX | 59 | 12 | 45 | 2 |
| 2 | 1 | 13 | Active | —- | 65 | 0 | 65 | 0 |
| 2 | 1 | 13 | Active | —- | 65 | 4 | 70 | 1 |
| 2 | 1 | 13 | Active | —- | 65 | 12 | 65 | 2 |
| 2 | 1 | 13 | Active | —- | 65 | 24 | 65 | 3 |
| 2 | 1 | 13 | Active | —- | 65 | 52 | 55 | 4 |
| 3 | 4 | 8 | Placebo | —X | 40 | 0 | 40 | 0 |
| 3 | 4 | 8 | Placebo | —X | 40 | 4 | 40 | 1 |

### 23.18.3 从列名中提取多个分类变量值

上面的dwide2数据集的FU1到FU4变量中包含了随访次数这一个变量的值。
有些数据集在列名中用编码形式保存了不止一个变量的信息，
假设那些列保存的数值仍属于同一属性。
例如，下面的数据：

| unit | F\_1 | F\_2 | M\_1 | M\_2 |
| --- | --- | --- | --- | --- |
| 1 | 55 | 52 | 64 | 60 |
| 2 | 98 | 93 | 120 | 116 |
| 3 | 40 | 38 | 44 | 40 |

假设这是对某个问题的赞成或反对意见的某个抽样调查的频数表，
unit是不同的抽样子集，
其它四列都是频数，
`F_1`代表女性中赞成人数，
`F_2`代表女性中反对人数，
`M_1`代表男性中赞成人数，
`M_2`代表男性中反对人数。

为了利用这样的数据，
需要将不同性别和两种意见的人数都合并到一列中，
增加性别和意见列。
这时，
`names_to`指定多个变量名，
即在变量名中包含的多个变量的新变量名，
用`names_sep = "_"`指定在各个分类值直接的分隔符号是下划线。
程序如：

```
dwide3 |>
  pivot_longer(
    cols = F_1:M_2,
    names_to = c("gender", "response"),
    values_to = "freq",
    names_sep = "_",
    names_ptypes = list(
      gender = factor(
        levels = c("F", "M")),
      response = factor(
        levels = c("1", "2"))
    )
  ) |>
  knitr::kable()
```

| unit | gender | response | freq |
| --- | --- | --- | --- |
| 1 | F | 1 | 55 |
| 1 | F | 2 | 52 |
| 1 | M | 1 | 64 |
| 1 | M | 2 | 60 |
| 2 | F | 1 | 98 |
| 2 | F | 2 | 93 |
| 2 | M | 1 | 120 |
| 2 | M | 2 | 116 |
| 3 | F | 1 | 40 |
| 3 | F | 2 | 38 |
| 3 | M | 1 | 44 |
| 3 | M | 2 | 40 |

如果多个分类变量值编码组合列名时没有用分隔符分隔，
就需要使用正则表达式的方式将有变量值的部分用正则表达式的捕获子集标记出来，
关于正则表达式详见[49](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/regex.html#regex)。
比如，若变量名为`F1`, `M1`, `F2`, `M2`，
则应将上面程序中的`names_sep = "_"`修改为`names_pattern = "(F|M)(1|2)"`。
其中的`"(F|M)(1|2)"`就是正则表达式，
`F|M`表示F或者M，
`1|2`表示1或者2，
圆括号表示分组，
前后两组分别表示两个分类变量`gender`和`response`。

### 23.18.4 一行中有多个属性的多次观测的情形

设有多个属性的多次测量用编号的列名保存在了同一观测中，
例如，
基本R软件中的anscombe数据集的一部分行：

```
dwide4 <- anscombe |>
  slice(1:3) |>
  mutate(id = 1:3) |>
  select(id, x1, x2, y1, y2)
knitr::kable(dwide4)
```

| id | x1 | x2 | y1 | y2 |
| --- | --- | --- | --- | --- |
| 1 | 10 | 10 | 8.04 | 9.14 |
| 2 | 8 | 8 | 6.95 | 8.14 |
| 3 | 13 | 13 | 7.58 | 8.74 |

这可以看成是每个受试者的x, y两个变量的2次随访的值保存在了一个观测中。
用`names_pattern`指定切分变量名和随访号的模式，
在对应的`names_to`中用特殊的`".value"`名字表示切分出来的那一部分实际是变量名，
这时不需要`values_to`选项。
程序如下：

```
dwide4 |>
  pivot_longer(
    -id,
    names_pattern = "(x|y)([[:digit:]])",
    names_to = c(".value", "time")
  ) |>
  knitr::kable()
```

| id | time | x | y |
| --- | --- | --- | --- |
| 1 | 1 | 10 | 8.04 |
| 1 | 2 | 10 | 9.14 |
| 2 | 1 | 8 | 6.95 |
| 2 | 2 | 8 | 8.14 |
| 3 | 1 | 13 | 7.58 |
| 3 | 2 | 13 | 8.74 |

这里“`(x|y)([[:digit:]])`”是正则表达式，
“`x|y`”意思是匹配x或y，
“`[[:digit:]]`”意思是匹配阿拉伯数字，
圆括号用来分组。

## 23.19 长表转换为宽表

### 23.19.1 将多个混在一起的变量拆开

tidyr包的`pivot_wider`函数可以将长表变成宽表。
这适用于将多个变量保存到了一列的情况。
例如，下面的长表将变量x和y放在了同一列中：

| id | variable | value |
| --- | --- | --- |
| 1 | x | 11 |
| 1 | y | 23 |
| 2 | x | 10 |
| 2 | y | 20 |
| 3 | x | 15 |
| 3 | y | 28 |

这样的数据也不利于进行统计分析，
我们用`pivot_wider`函数将两个变量放到各自的列中，
用`names_from`选项指定区分不同变量的列，
用`values_from`指定保存实际变量值的列：

```
dlong1 |>
  pivot_wider(
    names_from = "variable",
    values_from = "value"  ) |>
  knitr::kable()
```

| id | x | y |
| --- | --- | --- |
| 1 | 11 | 23 |
| 2 | 10 | 20 |
| 3 | 15 | 28 |

例子数据框dlong1除了分组用的变量id和要展开的列variable、value以外没有额外的变量，
如果有额外的变量，
就无法自动识别哪一列用来区分不同个体，
这时应该用`id_cols`指定用来区分个体的一列或多列，如：

```
dlong1 |>
  pivot_wider(
    id_cols = c("id"),
    names_from = "variable",
    values_from = "value"  ) |>
  knitr::kable()
```

| id | x | y |
| --- | --- | --- |
| 1 | 11 | 23 |
| 2 | 10 | 20 |
| 3 | 15 | 28 |

在这样拆分列时，
有可能某些变量值不存在，例如：

| id | variable | value |
| --- | --- | --- |
| 1 | x | 11 |
| 1 | y | 23 |
| 2 | x | 10 |
| 3 | y | 28 |

这里2号id缺少y，3号id缺少x。
直接转换为宽表：

```
dlong2 |>
  pivot_wider(
    id_cols = c("id"),
    names_from = variable,
    values_from = value  ) |>
  knitr::kable()
```

| id | x | y |
| --- | --- | --- |
| 1 | 11 | 23 |
| 2 | 10 | NA |
| 3 | NA | 28 |

产生了缺失值。
如果知道缺失值实际等于0，
可以用选项`values_fill=`选项指定，如：

```
dlong2 |>
  pivot_wider(
    id_cols = c("id"),
    names_from = variable,
    values_from = value,
    values_fill = list(
      value = 0)  ) |>
  knitr::kable()
```

| id | x | y |
| --- | --- | --- |
| 1 | 11 | 23 |
| 2 | 10 | 0 |
| 3 | 0 | 28 |

### 23.19.2 将多个类别合并到一个观测

在某些情况下也可能需要将数据转换为非整洁形态。
比如，为了给读者用列联表形式显示数据，
为了某个特定的多元分析程序使用，
等等。

设3个受试者的2次测量值放在变量x中，
用`time`区分2次测量值：

| id | time | x |
| --- | --- | --- |
| 1 | 1 | 11 |
| 1 | 2 | 10 |
| 2 | 1 | 15 |
| 2 | 2 | 13 |
| 3 | 1 | 18 |
| 3 | 2 | 16 |

下面的程序将x的两次测量变成变量x1和x2：

```
dlong3 |>
  pivot_wider(
    id_cols = c("id"),
    names_from = time,
    values_from = x,
    names_prefix = "x") |>
  knitr::kable()
```

| id | x1 | x2 |
| --- | --- | --- |
| 1 | 11 | 10 |
| 2 | 15 | 13 |
| 3 | 18 | 16 |

### 23.19.3 将交叉类别合并到一个观测

这个例子也是将数据转换为非整洁形态。
考虑如下的频数表数据：

| year | sex | type | count |
| --- | --- | --- | --- |
| 2018 | F | Benign | 4 |
| 2018 | F | Malignant | 9 |
| 2018 | M | Benign | 18 |
| 2018 | M | Malignant | 3 |
| 2019 | F | Benign | 6 |
| 2019 | F | Malignant | 10 |
| 2019 | M | Benign | 20 |
| 2019 | M | Malignant | 5 |

下面的程序将每年的数据合并到一行中：

```
dlong4 |>
  pivot_wider(
    id_cols = c("year"),
    names_from = c("sex", "type"),
    values_from = "count"
  ) |>
  knitr::kable()
```

| year | F\_Benign | F\_Malignant | M\_Benign | M\_Malignant |
| --- | --- | --- | --- | --- |
| 2018 | 4 | 9 | 18 | 3 |
| 2019 | 6 | 10 | 20 | 5 |

### 23.19.4 多个变量的多种值

设有如下的x变量和y变量的分组汇总统计数据：

| group | variable | avg | sd |
| --- | --- | --- | --- |
| 1 | x | 1.2 | 0.5 |
| 1 | y | -5.1 | 0.4 |
| 2 | x | 1.4 | 0.5 |
| 2 | y | -4.9 | 0.8 |
| 3 | x | 1.3 | 0.7 |
| 3 | y | -4.3 | 0.9 |

下面将x和y的两种统计量都放到同一行中：

```
dlong5 |>
  pivot_wider(
    id_cols = c("group"),
    names_from = "variable",
    values_from = c("avg", "sd")
  ) |> 
  knitr::kable()
```

| group | avg\_x | avg\_y | sd\_x | sd\_y |
| --- | --- | --- | --- | --- |
| 1 | 1.2 | -5.1 | 0.5 | 0.4 |
| 2 | 1.4 | -4.9 | 0.5 | 0.8 |
| 3 | 1.3 | -4.3 | 0.7 | 0.9 |

### 23.19.5 长宽转换混合使用

有时数据需要使用两个方向的转换才能达到可用的程度，
比如下面的数据：

```
knitr::kable(dlong6)
```

| id | variable | 2018 | 2019 |
| --- | --- | --- | --- |
| 1 | x | 1.2 | 1.3 |
| 1 | y | -5.1 | -5.4 |
| 2 | x | 1.4 | 1.6 |
| 2 | y | -4.9 | -4.2 |
| 3 | x | 1.3 | 1.5 |
| 3 | y | -4.3 | -4.1 |

这个数据的问题是x, y应该放在两列中却合并成一个了，
2018和2019应该放在一列中却分成了两列。
先合并2018和2019这两列，
然后再拆分x和y：

```
dlong6 |>
  pivot_longer(
    `2018`:`2019`,
    names_to = "year",
    values_to = "value" 
    ) |>
  pivot_wider(
    id_cols = c("id", "year"),
    names_from = "variable",
    values_from = "value"
    ) |>
  knitr::kable()
```

| id | year | x | y |
| --- | --- | --- | --- |
| 1 | 2018 | 1.2 | -5.1 |
| 1 | 2019 | 1.3 | -5.4 |
| 2 | 2018 | 1.4 | -4.9 |
| 2 | 2019 | 1.6 | -4.2 |
| 3 | 2018 | 1.3 | -4.3 |
| 3 | 2019 | 1.5 | -4.1 |

## 23.20 拆分数据列

### 23.20.1 separate函数

有时应该放在不同列的数据用分隔符分隔后放在同一列中了。
比如，下面数据集中“succ/total”列存放了用“/”分隔开的成功数与试验数：

```
d.sep <- read_csv(
"testid, succ/total
1, 1/10
2, 3/5
3, 2/8
")
```

```
## Rows: 3 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): succ/total
## dbl (1): testid
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
knitr::kable(d.sep)
```

| testid | succ/total |
| --- | --- |
| 1 | 1/10 |
| 2 | 3/5 |
| 3 | 2/8 |

用`tidyr::separate()`可以将这样的列拆分为各自的变量列，如

```
d.sep |>
  separate(
    `succ/total`, 
    into=c("succ", "total"), 
    sep="/", 
    convert=TRUE) |> 
  knitr::kable()
```

| testid | succ | total |
| --- | --- | --- |
| 1 | 1 | 10 |
| 2 | 3 | 5 |
| 3 | 2 | 8 |

其中`into`指定拆分后新变量名，
`sep`指定分隔符，
`convert=TRUE`要求自动将分割后的值转换为适当的类型。
`sep`还可以指定取子串的字符位置，
按位置拆分各个子串。

选项`extra`指出拆分时有多余内容的处理方法，
选项`fill`指出有不足内容的处理方法。

拆分的也可以是变量名和因子，
比如，
变量包括血压的高压和低压，
分男女计算了平均值，
结果表格可能为如下格式：

```
knitr::kable(dbpa)
```

| var | avg |
| --- | --- |
| male:systolicbp | 118 |
| male:diastolicbp | 85 |
| female:systolicbp | 115 |
| female:diastolicbp | 83 |

用`separate()`函数将变量名和性别值分开：

```
dbpa2 <- dbpa |>
  separate(var, into = c("sex", "var"), sep=":")
knitr::kable(dbpa2)
```

| sex | var | avg |
| --- | --- | --- |
| male | systolicbp | 118 |
| male | diastolicbp | 85 |
| female | systolicbp | 115 |
| female | diastolicbp | 83 |

实际上，这个数据集可能还需要将高压和低压变成两列，
用`pivot_wider()`函数：

```
dbpa3 <- dbpa2 |>
  pivot_wider(
    names_from = "var", values_from = "avg")
knitr::kable(dbpa3)
```

| sex | systolicbp | diastolicbp |
| --- | --- | --- |
| male | 118 | 85 |
| female | 115 | 83 |

也可以拆分出更多列，如：

```
knitr::kable(dth)
```

| id | sm |
| --- | --- |
| 1 | 1,2,3 |
| 2 | 4,5,6 |
| 3 | 7,8,9 |

```
dth |>
  separate(
    sm, 
    into=c("x1", "x2", "x3"),
    sep=",",
    convert=TRUE)
```

```
## # A tibble: 3 × 4
##      id    x1    x2    x3
##   <int> <int> <int> <int>
## 1     1     1     2     3
## 2     2     4     5     6
## 3     3     7     8     9
```

### 23.20.2 extract函数

函数`extract()`可以按照某种正则表达式表示的模式从指定列拆分出对应于正则表达式中捕获组的一列或多列内容。
例如，下面的数据中factors水平AA, AB, BA, BB实际是两个因子的组合，
将其拆分出来：

```
dexp <- tibble(
  design = c("AA", "AB", "BA", "BB"),
  response = c(120, 110, 105, 95))
knitr::kable(dexp)
```

| design | response |
| --- | --- |
| AA | 120 |
| AB | 110 |
| BA | 105 |
| BB | 95 |

```
dexp |>
  extract(
    design,
    into = c("fac1", "fac2"),
    regex = "(.)(.)"
  ) |>
  knitr::kable()
```

| fac1 | fac2 | response |
| --- | --- | --- |
| A | A | 120 |
| A | B | 110 |
| B | A | 105 |
| B | B | 95 |

### 23.20.3 separate\_wider\_delim函数

`tidyr::separate_wider_delim(x, delim, names)`将用分隔符分隔的内容拆分为多列，
新的列名用`names`给出。
相当于`separate()`的一个简化版本。

`separate_wider_position(x, width)`用指定宽度从字符串中提取多列，
`width`用一个有名向量格式指定，
输入的元素名作为提取的列名。

### 23.20.4 separate\_longer\_delim函数

`tidyr::separate_longer_delim()`将某列中用分隔符分开的值拆分出来并堆叠在同一列中，如：

```
d.tmp <- tibble(
  xmixed=c("1,2", "3,4,5"))
d.tmp |>
  mutate(
    x = separate_longer_delim(xmixed, sep=",")  )
```

类似地，`separate_longer_position(x, width)`可以按指定宽度拆分字符串中的内容并堆叠到一列中。

## 23.21 合并数据列

`tidyr::unite()`函数可以将同一行的两列或多列的内容合并成一列。
这是`separate()`的反向操作，
如：

```
d.sep |>
  separate(`succ/total`, into=c("succ", "total"), 
           sep="/", convert=TRUE) |>
  unite(ratio, succ, total, sep=":") |>
  knitr::kable()
```

| testid | ratio |
| --- | --- |
| 1 | 1:10 |
| 2 | 3:5 |
| 3 | 2:8 |

`unite()`的第一个参数是要修改的数据框，
这里用管道`|>`传递进来，
第二个参数是合并后的变量名（`ratio`变量），
其它参数是要合并的变量名，`sep`指定分隔符。
实际上用`mutate()`、`paste()`或者`sprintf()`也能完成合并。

## 23.22 数据框纵向合并

矩阵或数据框要纵向合并，使用`rbind`函数即可。
dplyr包的`bind_rows()`函数也可以对两个或多个数据框纵向合并。
要求变量集合是相同的，变量次序可以不同。

比如，有如下两个分开男生、女生的数据框：

```
d3.class <- d.class |>
  select(name, sex, age) |>
  filter(sex=="M")
d4.class <- d.class |>
  select(name, sex, age) |>
  filter(sex=="F")
```

合并行如下:

```
d3.class |>
  bind_rows(d4.class) |>
  knitr::kable()
```

| name | sex | age |
| --- | --- | --- |
| Alfred | M | 14 |
| Duke | M | 14 |
| Guido | M | 15 |
| James | M | 12 |
| Jeffrey | M | 13 |
| John | M | 12 |
| Philip | M | 16 |
| Robert | M | 12 |
| Thomas | M | 11 |
| William | M | 15 |
| Alice | F | 13 |
| Becka | F | 13 |
| Gail | F | 14 |
| Karen | F | 12 |
| Kathy | F | 12 |
| Mary | F | 15 |
| Sandy | F | 11 |
| Sharon | F | 15 |
| Tammy | F | 14 |

将下面的数据框的变量列次序打乱，
合并不受影响：

```
d3.class |>
  select(age, name, sex) |>
  bind_rows(d4.class) |>
  knitr::kable()
```

| age | name | sex |
| --- | --- | --- |
| 14 | Alfred | M |
| 14 | Duke | M |
| 15 | Guido | M |
| 12 | James | M |
| 13 | Jeffrey | M |
| 12 | John | M |
| 16 | Philip | M |
| 12 | Robert | M |
| 11 | Thomas | M |
| 15 | William | M |
| 13 | Alice | F |
| 13 | Becka | F |
| 14 | Gail | F |
| 12 | Karen | F |
| 12 | Kathy | F |
| 15 | Mary | F |
| 11 | Sandy | F |
| 15 | Sharon | F |
| 14 | Tammy | F |

## 23.23 横向合并

为了将两个行数相同的数据框按行号对齐合并，
可以用基本R的`cbind()`函数或者dplyr包的`bind_cols()`函数。

### 23.23.1 连接介绍

实际数据往往没有存放在单一的表中，
需要从多个表查找数据。
多个表之间的连接，
一般靠关键列（key）对准来连接。
连接可以是一对一的，
一对多的。
多对多连接应用较少，
因为多对多连接是所有两两组合。

在规范的数据库中，每个表都应该有主键，
这可以是一列，也可以是多列的组合。
为了确定某列是主键，
可以用`count()`和`filter()`，如

```
d.class |>
  count(name) |>
  filter(n>1) |>
  nrow()
```

```
## [1] 0
```

没有发现重复出现的`name`，
说明`d.class`中`name`可以作为主键。

### 23.23.2 一对一内连接

为了演示一对一的横向连接，
我们将d.class取11和12岁的子集，
然后拆分为两个数据集d1.class和d2.class,
两个数据集都有主键`name`，
d1.class包含变量`name`, `sex`,
d2.class包含变量`name`, `age`, `height`, `weight`,
并删去某些观测：

```
d1.class <- d.class |>
  filter(age <= 12) |>
  select(name, sex) |>
  filter(!(name %in%  "Sandy"))
d2.class <- d.class |>
  filter(age <= 12) |>
  select(name, age, height, weight)
```

用dplyr包的`inner_join()`函数将两个数据框按键值横向合并，
仅保留能匹配的观测。因为d1.class中丢失了Sandy的观测，
所以合并后的数据框中也没有Sandy的观测：

```
d1.class |>
  inner_join(d2.class) |>
  knitr::kable()
```

```
## Joining, by = "name"
```

| name | sex | age | height | weight |
| --- | --- | --- | --- | --- |
| Karen | F | 12 | 56.3 | 77.0 |
| Kathy | F | 12 | 59.8 | 84.5 |
| James | M | 12 | 57.3 | 83.0 |
| John | M | 12 | 59.0 | 99.5 |
| Robert | M | 12 | 64.8 | 128.0 |
| Thomas | M | 11 | 57.5 | 85.0 |

横向连接自动找到了共同的变量`name`作为连接的键值，
可以在`inner_join()`中用`join_by(键)`指定键值变量名，
如果有不同的键名，
可以用`join_by(左键 == 右键)`的格式指定，
这里用了`==`是因为这类似于SQL中左右连接时的连接条件，
这里是相等情况下的连接，
而R中用两个等于号表示是否相等的判断。

如果你确信左右表的连接都是一对一的，
如果出现重复匹配意味着数据错误，
可以在`inner_join()`中加选项`multiple = "error"`，
其默认值是`"warn"`，
即会显示警告信息。
如果在`inner_join()`或`left_join()`中使用`multiple = "all"`，
则意味着允许左表的一个观测可以和右表的多个观测匹配。

如果左右表的一对一连接不应该有遗漏，
如果有任何不匹配意味着数据有错，
可以在`inner_join()`中加选项`unmatched = "error"`，
其默认值是`"drop"`。

### 23.23.3 多对一左连接

两个表的横向连接，
经常是多对一连接，
这用于从右表向左表中添加一些额外的变量。

例如，
`d.stu`中有学生学号、班级号、姓名、性别，
`d.cl`中有班级号、班主任名、年级，
可以通过班级号将两个表连接起来：

```
d.stu <- tibble(
  sid=c(1,2,3,4,5,6),
  cid=c(1,2,1,2,1,2),
  sname=c("John", "Mary", "James", "Kitty", "Jasmine", "Kim"),
  sex=c("M", "F", "M", "F", "F", "M"))
knitr::kable(d.stu)
```

| sid | cid | sname | sex |
| --- | --- | --- | --- |
| 1 | 1 | John | M |
| 2 | 2 | Mary | F |
| 3 | 1 | James | M |
| 4 | 2 | Kitty | F |
| 5 | 1 | Jasmine | F |
| 6 | 2 | Kim | M |

```
d.cl <- tibble(
  cid=c(1,2),
  tname=c("Philip", "Joane"),
  grade=c("2017", "2016")
)
knitr::kable(d.cl)
```

| cid | tname | grade |
| --- | --- | --- |
| 1 | Philip | 2017 |
| 2 | Joane | 2016 |

```
d.stu |>
  left_join(d.cl, join_by(cid)) |>
  knitr::kable()
```

| sid | cid | sname | sex | tname | grade |
| --- | --- | --- | --- | --- | --- |
| 1 | 1 | John | M | Philip | 2017 |
| 2 | 2 | Mary | F | Joane | 2016 |
| 3 | 1 | James | M | Philip | 2017 |
| 4 | 2 | Kitty | F | Joane | 2016 |
| 5 | 1 | Jasmine | F | Philip | 2017 |
| 6 | 2 | Kim | M | Joane | 2016 |

`left_join()`按照`join_by()`指定的关键列匹配观测，
左数据集所有观测不论匹配与否全部保留，
右数据集仅使用与左数据集能匹配的观测。
所以，
结果数据集总是与左数据集行数相同，
其观测也域左数据集的观测一一对应，
仅用来添加部分右数据集中出现的变量。
不指定关键列时，
使用左、右数据集的共同列作为关键列。
如果左右数据集关键列变量名不同，
可以用`by=c("左名"="右名")`的格式。

### 23.23.4 右连接和外连接

与`left_join()`类似，
`right_join()`保留右数据集的所有观测，
对左数据集仅保留中能与右数据集匹配的观测。
`full_join()`保留左、右数据集所有观测，
包括能匹配的观测，也包括不能匹配的观测。
`inner_join()`仅保留能匹配的观测。

### 23.23.5 两两组合

`cross_join()`则不需要一个用来匹配的关键列，
而是进行两两搭配组合。
例如：

```
tibble(A = c(1,1,2)) |>
  cross_join(tibble(B=c(11, 12))) |>
  knitr::kable()
```

| A | B |
| --- | --- |
| 1 | 11 |
| 1 | 12 |
| 1 | 11 |
| 1 | 12 |
| 2 | 11 |
| 2 | 12 |

### 23.23.6 利用第二个数据集筛选

`left_join()`将右表中与左表匹配的观测的额外的列添加到左表中。
如果希望按照右表筛选左表的观测，
在左表中仅保留键值与右表键值匹配的观测，
可以用`semi_join()`，

函数`anti_join()`则与`semi_join()`相反，
是要求从左表中删去与右表匹配的观测，
仅保留与右表不匹配的观测。

这两种情况中右表仅使用关键列，
右表中其它变量不起作用。

### 23.23.7 非相等条件的连接

前面的例子都是左、右表键值相等的连接。
也可以将`join_by(左键 == 右键)`，
改成其它条件，
如`join_by(左键 <= 右键)`。

也可以要求左、右表不按照某个关键列连接，
而是生成所有两两组合，
这样，
比如左表有5行，右表有3行，
则结果有所有的\(5 \times 3\)行两两组合。
其写法是`full_join()`中加选项`join_by()`，
注意`join_by()`中没有关键列。

dplyr包还支持其它的一些更复杂的连接，
比如，
满足条件的最近观测的连接（用`join_by(closest(连接条件))`），
还提供了进行区间判断的方便连接函数。

复杂的连接可以考虑安装一个支持SQL语言的扩展包，
如sqlite扩展包，
直接使用SQL语言解决问题。

## 23.24 数据集的集合操作

R的`intersect()`，`union()`, `setdiff()`本来是以向量作为集合进行集合操作。
dplyr包也提供了这些函数，
但是将两个tibble的各行作为元素进行集合操作。

## 23.25 标准化

设x是各列都为数值的列表(包括数据框和tibble)或数值型矩阵，
用`scale(x)`可以把每一列都标准化，
即每一列都减去该列的平均值，然后除以该列的样本标准差。
用`scale(x, center=TRUE, scale=FALSE)`仅中心化而不标准化。
如

```
d.class |> 
  select(height, weight) |>
  scale()
```

```
##            height      weight
##  [1,] -1.13843504 -0.70371312
##  [2,]  0.57794313 -0.08897522
##  [3,]  0.38290015 -0.44025402
##  [4,] -1.17744363 -1.01108207
##  [5,] -0.49479323 -0.68175819
##  [6,]  0.81199469  0.52576268
##  [7,] -2.15265850 -2.17469309
##  [8,]  0.03182280  0.54771760
##  [9,]  0.09033569  0.10861910
## [10,]  1.29960213  0.54771760
## [11,]  0.22686577  0.10861910
## [12,]  0.90951618  1.44786952
## [13,] -0.98240066 -0.74762297
## [14,]  0.03182280 -0.70371312
## [15,] -0.65082761 -0.02311045
## [16,]  1.88473105  2.19433697
## [17,]  0.48042164  1.22832027
## [18,] -0.94339207 -0.65980327
## [19,]  0.81199469  0.52576268
## attr(,"scaled:center")
##    height    weight 
##  62.33684 100.02632 
## attr(,"scaled:scale")
##    height    weight 
##  5.127075 22.773933
```

为了把`x`的每列变到\([0,1]\)内，可以用如下的方法：

```
d.class %>%
  select(height, weight) %>%
  scale(center=apply(., 2, min),
      scale=apply(., 2, max) - apply(., 2, min))
```

其中的`.`在`%>%`管道操作中表示被传递处理的变量（一般是数据框）。
也可以写一个自定义的进行零一标准化的函数：

```
scale01 <- function(x){
  mind <- apply(x, 2, min)
  maxd <- apply(x, 2, max)
  scale(x, center=mind, scale=maxd-mind)
}
d.class |> 
  select(height, weight) |>
  scale01()
```

```
##          height    weight
##  [1,] 0.2512077 0.3366834
##  [2,] 0.6763285 0.4773869
##  [3,] 0.6280193 0.3969849
##  [4,] 0.2415459 0.2663317
##  [5,] 0.4106280 0.3417085
##  [6,] 0.7342995 0.6180905
##  [7,] 0.0000000 0.0000000
##  [8,] 0.5410628 0.6231156
##  [9,] 0.5555556 0.5226131
## [10,] 0.8550725 0.6231156
## [11,] 0.5893720 0.5226131
## [12,] 0.7584541 0.8291457
## [13,] 0.2898551 0.3266332
## [14,] 0.5410628 0.3366834
## [15,] 0.3719807 0.4924623
## [16,] 1.0000000 1.0000000
## [17,] 0.6521739 0.7788945
## [18,] 0.2995169 0.3467337
## [19,] 0.7342995 0.6180905
## attr(,"scaled:center")
## height weight 
##   51.3   50.5 
## attr(,"scaled:scale")
## height weight 
##   20.7   99.5
```

函数`sweep()`可以执行对每列更一般的变换。

## 23.26 读入多个文件

设同类的数据放置在了多个文件中，
这时可以利用多种技术，
将多个文件读入并纵向合并，
需要时增加文件名代表的变量信息。

作为举例，
我们将`d.class`分为男生和女生两个数据框，
分别保存到`data`子目录中：

```
d.class |>
  filter(sex == "F") |>
  select(-sex) |>
  write_csv(file = "data/class-F.csv")
d.class |>
  filter(sex == "M") |>
  select(-sex) |>
  write_csv(file = "data/class-M.csv")
```

如果要读入这两个文件并合并为一个大数据框，
当然可以使用重复的写法：

```
dc1 <- read_csv(
  "data/class-F.csv",
  show_col_types = FALSE)
dc1[["sex"]] <- "F"
dc2 <- read_csv(
  "data/class-M.csv", 
  show_col_types = FALSE)
dc2[["sex"]] <- "M"
dc <- rbind(dc1, dc2)
dc[c(1:2, 11:12), ]
```

```
## # A tibble: 4 × 5
##   name    age height weight sex  
##   <chr> <dbl>  <dbl>  <dbl> <chr>
## 1 Alice    13   56.5    84  F    
## 2 Becka    13   65.3    98  F    
## 3 Duke     14   63.5   102. M    
## 4 Guido    15   67     133  M
```

如果有许多个文件要合并，
这样的硬编码方式就不够简洁，
也容易出错。
为此，
可以联合使用许多技巧。

`list.files()`函数可以指定某种文件名模式，
获取所有要读入的文件名，如：

```
flis <- list.files(
  "data", pattern = "class-[[:alnum:]]+[.]csv")
flis
```

```
## [1] "class-F.csv" "class-M.csv"
```

其中的`pattern`选项输入了一个模式（正则表达式），
见第[49](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/regex.html#regex)章。

可以用`purrr::map()`（见§[25.2.1](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/functionals.html#fnctnl-fnctnl-map)）将所有文件读入为一个数据框的列表，
用`purrr::list_rbind()`函数将作为列表元素的数据框纵向合并：

```
dca <- file.path("data", flis) |>
  map(\(file) read_csv(file, show_col_types = FALSE)) |>
  list_rbind() 
dca |>
  slice(c(1:2, 11:12))
```

```
## # A tibble: 4 × 4
##   name    age height weight
##   <chr> <dbl>  <dbl>  <dbl>
## 1 Alice    13   56.5    84 
## 2 Becka    13   65.3    98 
## 3 Duke     14   63.5   102.
## 4 Guido    15   67     133
```

这个程序的缺点是丢失了性别变量，
这个变量的值保存在文件名内。
为此，
可以用`purrr::set_names()`给各个文件名指定元素名，
这个函数的第二自变量可以直接输入元素名向量，
也可以指定一个函数，
从元素值产生元素名。
用`map()`读入了数据框列表后，
列表元素名会沿用文件名向量的元素名，
然后在`list_rbind()`中可以用`names_to`选项，
将数据框列表的元素名转换为合并后大数据框的一列：

```
dca2 <- file.path("data", flis) |>
  set_names(basename) |>
  map(\(file) read_csv(file, show_col_types = FALSE)) |>
  list_rbind(names_to = "filename") 
dca2 |>
  slice(c(1:2, 11:12))
```

```
## # A tibble: 4 × 5
##   filename    name    age height weight
##   <chr>       <chr> <dbl>  <dbl>  <dbl>
## 1 class-F.csv Alice    13   56.5    84 
## 2 class-F.csv Becka    13   65.3    98 
## 3 class-M.csv Duke     14   63.5   102.
## 4 class-M.csv Guido    15   67     133
```

可以用适当的字符串处理函数提取文件名中的变量值，如：

```
dca3 <- file.path("data", flis) |>
  set_names(basename) |>
  map(\(file) read_csv(file, show_col_types = FALSE)) |>
  list_rbind(names_to = "filename") |>
  mutate(sex = str_sub(filename, 7,7)) |>
  select(-filename)
dca3 |>
  slice(c(1:2, 11:12))
```

```
## # A tibble: 4 × 5
##   name    age height weight sex  
##   <chr> <dbl>  <dbl>  <dbl> <chr>
## 1 Alice    13   56.5    84  F    
## 2 Becka    13   65.3    98  F    
## 3 Duke     14   63.5   102. M    
## 4 Guido    15   67     133  M
```

这里用了`str_sub`取子串的方法提取变量值，
这适用于文件名中的变量值占据固定的位置的情形。
更复杂的文件名，
可以用正则表达式，
或者`tidyr::separate_wider_delim()`指定分隔符分割成多列。

如：

```
dca4 <- file.path("data", flis) |>
  set_names(basename) |>
  map(\(file) read_csv(file, show_col_types = FALSE)) |>
  list_rbind(names_to = "filename") |>
  separate_wider_delim(
    filename, delim="-", names=c(NA, "sexext")) |>
  separate_wider_delim(
    sexext, delim=".", names=c("sex", NA)) 
dca4 |>
  slice(c(1:2, 11:12))
```

上面的程序先用“`-`”分割成两部分，
再用“`.`”将第二部分分割成两部分。

也可以直接使用正则表达式，
如：

```
dca5 <- file.path("data", flis) |>
  set_names(basename) |>
  map(\(file) read_csv(file, show_col_types = FALSE)) |>
  list_rbind(names_to = "filename") |>
  mutate(
    sex = str_replace(
      filename, "class-([[:alnum:]]+)[.]csv", "\\1")  ) |>
  select(-filename)
dca5 |>
  slice(c(1:2, 11:12))
```

data.table包的rbindlist可以将保存为列表元素的多个数据框纵向合并，
运行效率高。

## 23.27 使用data.table包

data.table包不仅能高效地读写文本格式的数据，
也能高效地进行数据整理。
它可以利用CPU的多个核心并行处理，
并可以巧妙地使用内存，
避免冗余的复制。

data.table在读入数据时不会自动将字符型数据转换为因子，
也不会生成数据框行名。
在显示data.table时，
过大的表会自动简化显示。

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

用如下命令快速查看基本用法示例：

```
examples(data.table)
```

### 23.27.1 查询

data.table的`[`运算符支持增强的查询功能，
可以指定行子集条件、列子集条件和分组条件，
一般格式为`DT[i, j, by]`。

以nycflights13中的航班数据为例。

```
library(nycflights13)
```

```
## Warning: package 'nycflights13' was built under R version 4.2.2
```

```
dt_flights <- as.data.table(flights)
```

### 23.27.2 行子集与排序

仅提供行子集条件，
注意可以直接使用数据框变量名，
也不需要写成`DT[i,]`的格式：

```
dtsub <- dt_flights[origin == "JFK" & month == 6]
head(dtsub)
```

```
##    year month day dep_time sched_dep_time dep_delay arr_time sched_arr_time
## 1: 2013     6   1        2           2359         3      341            350
## 2: 2013     6   1      538            545        -7      925            922
## 3: 2013     6   1      539            540        -1      832            840
## 4: 2013     6   1      553            600        -7      700            711
## 5: 2013     6   1      554            600        -6      851            908
## 6: 2013     6   1      557            600        -3      934            942
##    arr_delay carrier flight tailnum origin dest air_time distance hour minute
## 1:        -9      B6    739  N618JB    JFK  PSE      200     1617   23     59
## 2:         3      B6    725  N806JB    JFK  BQN      203     1576    5     45
## 3:        -8      AA    701  N5EAAA    JFK  MIA      140     1089    5     40
## 4:       -11      EV   5716  N835AS    JFK  IAD       42      228    6      0
## 5:       -17      UA   1159  N33132    JFK  LAX      330     2475    6      0
## 6:        -8      B6    715  N766JB    JFK  SJU      198     1598    6      0
##              time_hour
## 1: 2013-06-01 23:00:00
## 2: 2013-06-01 05:00:00
## 3: 2013-06-01 05:00:00
## 4: 2013-06-01 06:00:00
## 5: 2013-06-01 06:00:00
## 6: 2013-06-01 06:00:00
```

按行号取行子集：

```
dt_flights[1:2]
```

```
##    year month day dep_time sched_dep_time dep_delay arr_time sched_arr_time
## 1: 2013     1   1      517            515         2      830            819
## 2: 2013     1   1      533            529         4      850            830
##    arr_delay carrier flight tailnum origin dest air_time distance hour minute
## 1:        11      UA   1545  N14228    EWR  IAH      227     1400    5     15
## 2:        20      UA   1714  N24211    LGA  IAH      227     1416    5     29
##              time_hour
## 1: 2013-01-01 05:00:00
## 2: 2013-01-01 05:00:00
```

返回按某些列排序的结果，如：

```
dt_flights[order(origin, -dest)]
```

在`[]`中的`order()`函数是data.table提供的改进版本，
可以用减号表示降序，
排序速度更快。

### 23.27.3 列子集

用`DT[,j]`可以将指定的列提取成一个R向量，
这与data.frame的做法相同，
但与tibble做法不同。
如

```
dt_flights[, arr_delay] |> head(5)
```

```
## [1]  11  20  33 -18 -25
```

注意变量名不需要用撇号保护。

选择多列，
或者选择一列但需要结果为data.table，
将这些列名写在`.()`内，如：

```
dt_flights[, .(arr_delay, dep_delay)] |> head(5)
```

```
##    arr_delay dep_delay
## 1:        11         2
## 2:        20         4
## 3:        33         2
## 4:       -18        -1
## 5:       -25        -6
```

取列子集时可以改变量名，
如`dt_flights[, .(dalaya = arr_delay, dalayd = dep_delay)]`。

因为在`j`的位置直接使用变量名，
而不是用撇号保护起来的变量名，
所以如果将变量名保存在了某个R变量中（如`vars`），
希望访问这样的列子集，
就需要用`..vars`的格式，
如：

```
vars <- c("arr_delay", "dep_delay")
dt_flights[, ..vars] |> head(5)
```

```
##    arr_delay dep_delay
## 1:        11         2
## 2:        20         4
## 3:        33         2
## 4:       -18        -1
## 5:       -25        -6
```

### 23.27.4 计算汇总

可以在`DT[i,j,by]`的`j`位置计算新变量或者汇总统计量。
比如，
计算总延误小于0的个数：

```
dt_flights[, 
  sum( arr_delay + dep_delay < 0, na.rm=TRUE)]
```

```
## [1] 188401
```

取行子集并计算统计量：

```
dt_flights[origin == "JFK" & month == 6, 
  .(m_arr = mean(arr_delay, na.rm=TRUE), 
    m_dep = mean(dep_delay, na.rm=TRUE))]
```

```
##       m_arr    m_dep
## 1: 17.59693 20.49973
```

计算子集行数：

```
dt_flights[origin == "JFK" & month == 6, .N]
```

```
## [1] 9472
```

其中`.N`是data.table提供的特殊语法。
这比先取子集再用`nrow()`计算的好处是不需要先生成子集，
而是直接计数。

### 23.27.5 用by做分组汇总

用`by =`指定一个或多个分组变量，
可以进行分组汇总。

例如，
数据框中每个出发机场的计数：

```
dt_flights[, .(N = .N), by = .(origin)]
```

```
##    origin      N
## 1:    EWR 120835
## 2:    LGA 104662
## 3:    JFK 111279
```

注意`j`和`by =`的参数都用了`.()`保护。
当`j`和`by =`的参数仅有一个变量时，
也可以不用`.()`的格式，如：

```
dt_flights[, .N, by = origin]
```

```
##    origin      N
## 1:    EWR 120835
## 2:    LGA 104662
## 3:    JFK 111279
```

`by =`后面也可以使用保存了变量名的字符型变量，
如：

```
vars <- c("origin", "dest")
dt_flights[, .N, by = vars]
```

```
##      origin dest     N
##   1:    EWR  IAH  3973
##   2:    LGA  IAH  2951
##   3:    JFK  MIA  3314
##   4:    JFK  BQN   599
##   5:    LGA  ATL 10263
##  ---                  
## 220:    LGA  TVC    77
## 221:    LGA  MYR     3
## 222:    EWR  TVC    24
## 223:    EWR  ANC     8
## 224:    EWR  LGA     1
```

但是，这种语法设计有问题，
在上例中无法分辨`vars`是保存了变量名的变量，
还是数据框中的变量名，
直接使用变量名时用`by = .(dest)`这样的格式较好。

取子集并分组汇总计数的例子：

```
dt_flights[carrier == "AA", 
  .(N = .N), 
  by = .(origin)]
```

```
##    origin     N
## 1:    JFK 13783
## 2:    LGA 15459
## 3:    EWR  3487
```

计算该子集按出发机场分组后的平均延误：

```
dt_flights[carrier == "AA", 
  .(marr = mean(arr_delay, na.rm=TRUE),
    mdep = mean(dep_delay, na.rm=TRUE)), 
  by = .(origin)]
```

```
##    origin       marr      mdep
## 1:    JFK  2.0812500 10.302155
## 2:    LGA -1.3317539  6.705769
## 3:    EWR  0.9776985 10.035419
```

### 23.27.6 分组汇总并将结果排序

用`by =`分组汇总时，
输出的各组次序是在原始数据中各组最先出现的次序。
为了使得汇总结果按组别取值排序，
将`by =`改为`keyby =`。
如：

```
dt_flights[, .(N = .N), keyby = .(origin)]
```

```
##    origin      N
## 1:    EWR 120835
## 2:    JFK 111279
## 3:    LGA 104662
```

也可以先用`by`分组汇总，
再用`order()`排序，
这样还可以降序排列，
如：

```
dt_flights[carrier == "AA", 
  .(marr = mean(arr_delay, na.rm=TRUE),
    mdep = mean(dep_delay, na.rm=TRUE)), 
  by = .(origin, dest)][order(origin, -dest)] |>
  head(6)
```

```
##    origin dest        marr      mdep
## 1:    EWR  MIA  0.06332703  9.204524
## 2:    EWR  LAX  0.91643454  5.861496
## 3:    EWR  DFW  1.48612539 11.250254
## 4:    JFK  TPA  5.20000000 10.918831
## 5:    JFK  STT -4.49501661  4.036545
## 6:    JFK  SJU -0.77603687  8.600917
```

### 23.27.7 数据子集`.SD`

data.table的`[]`运算内用`.SD`表示对一个数据子集(Subset of Data)的处理。
用这个特殊语法，
可以在分组后统一对数据子集进行分析，
如：

```
dt_flights[
  carrier == "AA",
  lapply(.SD, \(x) mean(x, na.rm = TRUE)),
  by = .(origin, dest, month),
  .SDcols = c("arr_delay", "dep_delay")] |>
  head(6)
```

```
##    origin dest month  arr_delay dep_delay
## 1:    JFK  MIA     1  0.4789474 12.236842
## 2:    LGA  ORD     1  1.6497462  5.972152
## 3:    LGA  DFW     1  2.1135802  5.148780
## 4:    EWR  MIA     1  7.6559140 15.494624
## 5:    LGA  MIA     1 -4.8750000  2.823171
## 6:    JFK  SJU     1  1.5040650 11.766129
```

在上例中，
用`SDcols =`指定对每个数据子集要分析的列，
用`lapply(.SD, f)`指定对每个数据子集的每一要分析的列应用`f`进行汇总。
这里的`f`用了R的无名函数形式。

### 23.27.8 以引用方式增添、修改、删除列

data.table使用特殊的`:=`运算符，
可以直接以引用方式增添、修改、删除data.table列，
这样做的好处是对大型数据节约了存储量，
提高了效率。
参见该包的帮助文档中名为“Reference semantics”的vignette。

### 23.27.9 使用关键列

data.table不再使用行名，
但可以指定一个或多个关键列，
起到与行名类似的作用，
但功能更强，
访问效率更高。
关键列不需要唯一区分各行。
用`setkey()`或者`setkeyv()`给data.table指定关键列，
这会使得数据框按照这些列的升序排列。
如：

```
setkey(dt_flights, origin)
```

或者

```
setkeyv(dt_flights, "origin")
```

这时，
可以直接以关键列的值作为行下标，
如`dt_flights["JFK"]`可以取出满足`origin == "JFK"`条件的行，
又如`dt_flights[c("JFK", "LGA")]`取出origin等于JFK或LGA的行。
这样取行子集时，
因为用了二分法定位，
所以在大型数据上比`dt_flights[origin == "JFK"]`这样的线性访问算法的行子集计算效率高得多。
但是，
data.table对每个表仅允许设置一组关键列，
而不允许设置多组。

可以指定多列作为关键列，
如：

```
setkey(dt_flights, origin, dest)
```

或

```
setkeyv(dt_flights, c("origin", "dest"))
```

取出origin为JFK，dest为MIA的子集：

```
dt_flights[.("JFK", "MIA")]
```

为了明确表示用了关键列帮助定位子集，
可以加上`on =`选项，如：

```
dt_flights[.("JFK", "MIA"),
  on = c("origin", "dest")]
```

这个语法主要用在辅助索引列功能，
但是在使用关键列时，
也可以使得程序意图更为明确。

取出origin为JFK或LGA,
dest为MIA的子集：

```
dt_flights[.(c("JFK", "LGA"), "MIA")]
```

取出dest为MIA，origin为任意值的子集：

```
dt_flights[.(unique(origin), "MIA")]
```

这里用了`unique(origin)`来获取origin的所有可取值。

在行下标中使用关键列后，
仍可以用列下标取列子集或汇总计算，
用by分组。
如：

```
dt_flights[.("LGA", "TPA"), 
  max(arr_delay, na.rm=TRUE)]
## [1] 821
```

又如，
计算出发机场为JFK的航班每个月的延误最大值：

```
dt_flights[
  "JFK", 
  .(mmax_dep_delay = max(dep_delay, na.rm = TRUE)), 
  keyby = month]
```

可以用`mult = "first"`要求仅返回每个组的第一行，
用`mult = "last"`仅返回每个组的最后一行。

可以用`nomatch = NULL`指定关键列的值对应的行不存在时，
就在结果中不包含对应的结果行。

### 23.27.10 使用辅助索引列

关键列仅允许有一组，
而且指定关键列会按关键列对数据框排序，
这都是使用关键列的障碍。

data.table提供了辅助索引列(secondary indeces)，
能够起到关键列的作用，
但是不需要对数据框重排，
还可以有多组。
增加辅助索引列，
仅增加一个按指定的列重排所用的行号序列。
还可以在取行子集时临时生成辅助索引列。

类似于`setkey()`和`setkeyv()`，
可以用`setindex()`和`setindexv()`设置关键列。
如：

```
setindex(dt_flights, origin)
setindex(dt_flights, origin, dest)
```

注意这设置了两组辅助索引列。

因为允许有多组辅助索引列，
所以在用辅助索引加速取行子集操作时，
需要用`on =`选项指定所用的辅助索引列，
如：

```
dt_flights[
  .("LGA", "TPA"), 
  on = c("origin", "dest")]
```

与使用关键列类似，
仍可以取列子集或者进行汇总，
用`by =`或`sortby =`进行分组计算。

如果`on =`指定的辅助索引列没有预先建立索引，
这会临时创建需要的辅助索引，
并在完成查询任务后自动删除。

data.table设置了自动索引功能，
一旦根据某个变量值进行查询，
就自动设置该变量的辅助索引，
并且不会自动删除。

### 23.27.11 横向合并

一对一横向合并，如：

```
dt1_class <- setDT(d1.class)
dt2_class <- setDT(d2.class)
dt3_class <- dt1_class[dt2_class, on = .(name)]
dt3_class
```

```
##      name  sex age height weight
## 1:  Karen    F  12   56.3   77.0
## 2:  Kathy    F  12   59.8   84.5
## 3:  Sandy <NA>  11   51.3   50.5
## 4:  James    M  12   57.3   83.0
## 5:   John    M  12   59.0   99.5
## 6: Robert    M  12   64.8  128.0
## 7: Thomas    M  11   57.5   85.0
```

一对一横向合并用了行子集的语法，
将要合并的右表写在左表的行子集位置。
这作的是右外连接，
包括与右表匹配的每一个观测，
左表中不存在的观测用缺失值代替，
仅在左表中存在的观测被忽略。

为了作内连接，
只要加选项`nomatch = NULL`。

如果左表与右表用来对齐的列名不相同，
可以用如`a[b, on = c(avar = "bvar")]`的形式。
结果中这一列的列名用`avar`，
取值则用`bvar`的值。

可以用`a[!b, on=.(id)]`的格式表示选取所有不能与b匹配的观测，
即反向连接(anti-join)。

这个语法还有许多功能，
用如下命令查看`[`运算功能：

```
?"[.data.table"
```

### 23.27.12 长宽表转换

用`melt()`函数将宽表转换为长表。
data.table可以对很大的表（比如，占用几个GB内存的表）进行高速转换。

如：

```
dt_dwide1 <- setDT(dwide1)
dt2 <- melt(dt_dwide1, 
  id.vars = c("subject"),
  measure.vars = c("1", "2", "3", "4"),
  variable.name = "time", 
  value.name = "y")
dt2[!is.na(y)]
```

```
##    subject time  y
## 1:       1    1  1
## 2:       3    1  5
## 3:       2    2  7
## 4:       3    2 10
## 5:       4    3  9
## 6:       2    4  4
```

可以用`dcast()`将长表转换为宽表，如：

```
dcast(dt2, 
  subject ~ time,
  value.var = "y")
```

```
##    subject  1  2  3  4
## 1:       1  1 NA NA NA
## 2:       2 NA  7 NA  4
## 3:       3  5 10 NA NA
## 4:       4 NA NA  9 NA
```

当有多个测量变量时，
data.table也提供了相应的语法，
详见该包的帮助文档中题目为“Efficient reshaping using data.tables”的vignette。

关于data.table的使用，
还可参见：

* [Quick R Tutorial](https://franknarf1.github.io/r-tutorial/_book/index.html)

## 23.28 数据库访问与SQL

R支持与多种数据库连接，
比如基于内存的sqlite, duckdb,
基于本地文件的arrow数据库，
基于客户/服务器的MySQL，Oracle等。
见[51](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/db.html#db)。

SQL是数据库的一种专用语言，
可以很容易地实现数据查询、多个数据集连接等操作，见[51.5](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/db.html#db-sql)。

### References

Wickham, H. 2014. “Tidy Data.” *J Stat Software* 59. <http://www.jstatsoft.org/v59/i10/>.