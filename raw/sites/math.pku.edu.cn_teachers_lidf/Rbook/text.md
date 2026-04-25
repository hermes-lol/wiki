---
crawl_time: '2026-01-17 14:20:17'
framework: rbook
title: 48 R语言的文本处理 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/text.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 48 R语言的文本处理

## 48.1 介绍

在信息爆炸性增长的今天， 大量的信息是文本型的，
如互联网上的大多数资源。
R具有基本的文本数据处理能力，
而且因为R的向量语言特点和强大的统计计算和图形功能，
用R处理文本数据是可行的。

## 48.2 字符型常量与字符型向量

字符串常量写在两个双撇号或者两个单撇号中间，
建议仅使用双撇号，
因为这是大多数常见程序语言的做法。
如果内容中有单撇号或者双撇号，
可以在前面加反斜杠`\`。
为了在字符串中写一个反斜杠，
需要写成两个，
比如路径`C:\work`写成R字符串，
要写成`"C:\\work"`。
注意，
这些规定都是针对程序中的字符串常量，
数据中的文本类型数据是不需要遵照这些规定的。

在用`print()`显示字符串变量时，
也会按照上述的办法显示，
比如字符串内的双撇号会被自动加上前导反斜杠，
但保存的实际内容中并没有反斜杠。

字符串中可以有一些特殊字符，
如`"\n"`表示换行符，
`"\t"`表示制表符，
`"\r"`表示回车符，等等。

R的字符型向量每个元素是一个字符串，
如：

```
s <- c("123", "abc", "张三李四", "@#$%^&")
s
```

```
## [1] "123"      "abc"      "张三李四" "@#$%^&"
```

R中处理文本型数据的函数有文件访问函数以及`readLines`，`nchar`,
`paste`，`sprintf`， `format`，`formatC`， `substring`等函数。

R支持正则表达式， 函数`grep`, `grepl`, `sub`, `gsub`, `regexpr`,
`gregexpr`, `strsplit`与正则表达式有关。

字符型函数一般都是向量化的，
对输入的一个字符型向量的每个元素操作。

R扩展包stringr和stringi提供了更方便、功能更强的字符串功能，
包括正则表达式功能。
其中stringr是常用功能，
stringi是更基本、更灵活的功能，
一般使用stringr就足够了。
stringr包的函数名大多都以`str_`开头。

下面先介绍常用的较简单的字符串函数，
包括stringr包的函数与基本R函数。

```
library(stringr)
```

## 48.3 字符串连接、重复

`stringr::str_c()`用来把多个输入自变量按照元素对应组合为一个字符型向量，
用`sep`指定分隔符，默认为不分隔。
类似于R中向量间运算的一般规则，
各自变量长度不同时短的自动循环使用。
非字符串类型自动转换为字符型。
如

```
str_c(c("x", "y"), c("a", "b"), sep="*")
```

```
## [1] "x*a" "y*b"
```

```
str_c("data", 1:3, ".txt")
```

```
## [1] "data1.txt" "data2.txt" "data3.txt"
```

字符型缺失值参与连接时，
结果变成缺失值；
可以用`str_replace_na()`函数将待连接的字符型向量中的缺失值转换成字符串`"NA"`再连接。

用`collapse`选项要求将连接后的字符型向量的所有元素连接在一起，
`collapse`的值为将多个元素合并时的分隔符。
如

```
str_c(c("a", "bc", "def"), collapse="---")
```

```
## [1] "a---bc---def"
```

在使用了`collapse`时如果有多个要连接的部分，
`str_c()`函数先将各部分连接成为一个字符型向量，
然后再把结果的各个向量元素连接起来。
如

```
str_c("data", 1:3, ".txt", sep="", collapse=";")
```

```
## [1] "data1.txt;data2.txt;data3.txt"
```

`stringr::str_flatten()`类似于`stringr::str_c()`仅有`collapse`参数作用一样，
仅将一个字符型向量的各个元素按照`collapse`参数指定的分隔符连接成一个长字符串，
`collapse`默认值是空字符串，如：

```
str_flatten(c("a", "bc", "def"), collapse="---")
```

```
## [1] "a---bc---def"
```

```
str_flatten(c("a", "bc", "def"))
```

```
## [1] "abcdef"
```

基本R的`paste()`函数与`stringr::str_c()`函数有类似的用法，
但是参数`sep`的默认值是空格。
基本R的`paste0()`函数相当于`stringr::str_c()`函数固定`sep`参数为空字符串。
如：

```
paste(c("x", "y"), c("a", "b"), sep="*")
```

```
## [1] "x*a" "y*b"
```

```
paste("data", 1:3, ".txt", sep="")
```

```
## [1] "data1.txt" "data2.txt" "data3.txt"
```

```
paste0("data", 1:3, ".txt")
```

```
## [1] "data1.txt" "data2.txt" "data3.txt"
```

```
paste(c("a", "bc", "def"), collapse="---")
```

```
## [1] "a---bc---def"
```

```
paste("data", 1:3, ".txt", sep="", collapse=";")
```

```
## [1] "data1.txt;data2.txt;data3.txt"
```

`stringr::str_dup(string, times)`类似于`rep()`函数，
可以将字符型向量的元素按照`times`指定的次数在同一字符串内重复，如：

```
str_dup(c("abc", "长江"), 3)
```

```
## [1] "abcabcabc"    "长江长江长江"
```

也可以针对每个元素指定不同重复次数，如

```
str_dup(c("abc", "长江"), c(3, 2))
```

```
## [1] "abcabcabc" "长江长江"
```

## 48.4 格式化输出

### 48.4.1 `format()`函数

`format()`函数可以将一个数值型向量的各个元素按照统一格式转换为字符型，
如：

```
as.character(1.000)
```

```
## [1] "1"
```

```
as.character(1.2)
```

```
## [1] "1.2"
```

```
as.character(1.23)
```

```
## [1] "1.23"
```

```
format(c(1.000, 1.2, 1.23))
```

```
## [1] "1.00" "1.20" "1.23"
```

选项`digits`与`nsmall`共同控制输出的精度，
`nsmall`控制非科学记数法显示时小数点后的至少要有的位数，
`digits`控制至少要有的有效位数。
这使得输出的宽度是不可控的，
如：

```
format(c(pi, pi*10000), digits=8, nsmall=4)
```

```
## [1] "    3.1415927" "31415.9265359"
```

`width`参数指定至少要有的输出宽度，
不足时默认在左侧用空格填充，如：

```
format(1.000, width=6, nsmall=2)
```

```
## [1] "  1.00"
```

`format()`还有许多选项，
详见函数的帮助。

### 48.4.2 `sprintf()`函数

`format()`函数无法精确控制输出长度和格式。
`sprintf`是C语言中`sprintf`的向量化版本，
可以把一个元素或一个向量的各个元素按照C语言输出格式转换为字符型向量。
第一个自变量是C语言格式的输出格式字符串，
其中`%d`表示输出整数，`%f`表示输出实数，
`%02d`表示输出宽度为2、不够左填0的整数，
`%6.2f`表示输出宽度为6、宽度不足时左填空格、含两位小数的实数，
等等。

比如，标量转换

```
sprintf("%6.2f", pi)
```

```
## [1] "  3.14"
```

又如，向量转换：

```
sprintf("tour%03d.jpg", c(1, 5, 10, 15, 100))
```

```
## [1] "tour001.jpg" "tour005.jpg" "tour010.jpg" "tour015.jpg" "tour100.jpg"
```

还可以支持多个向量同时转换，如：

```
sprintf("%1dx%1d=%2d", 1:5, 5:1, (1:5)*(5:1))
```

```
## [1] "1x5= 5" "2x4= 8" "3x3= 9" "4x2= 8" "5x1= 5"
```

### 48.4.3 字符串插值函数

许多脚本型程序设计语言都有在字符串的内容中插入变量值的功能，
R本身不具有这样的功能，
`sprintf()`函数有类似作用但只是一个不方便使用的副作用。

`stringr::str_glue()`和`stringr::str_glue_data()`提供了字符串插值的功能。
只要在字符串内用大括号写变量名，
则函数可以将字符串内容中的变量名替换成变量值，如：

```
name <- "李明"
tele <- "13512345678"
str_glue("姓名: {name}\n电话号码: {tele}\n")
```

```
## 姓名: 李明
## 电话号码: 13512345678
```

上面的例子直接用了换行符`"\n"`来分开不同内容。
也可以输入多个字符串作为自变量，
内容自动连接在一起，可以用参数`.sep`指定分隔符：

```
name <- "李明"
tele <- "13512345678"
str_glue("姓名: {name}, ", "电话号码: {tele}")
```

```
## 姓名: 李明, 电话号码: 13512345678
```

```
str_glue("姓名: {name}", "电话号码: {tele}", .sep="; ")
```

```
## 姓名: 李明; 电话号码: 13512345678
```

也可以直接在`str_glue()`中指定变量值，如：

```
str_glue("姓名: {name}", "电话号码: {tele}", .sep="; ",
         name = "张三", tele = "13588888888")
```

```
## 姓名: 张三; 电话号码: 13588888888
```

`stringr::str_glue_data()`则以一个包含变量定义的对象`.x`为第一自变量，
类型可以是环境、列表、数据框等。如：

```
str_glue_data(list(name = "王五", tele = "13500000000"),
              "姓名: {name}", "电话号码: {tele}", .sep="; ")
```

```
## 姓名: 王五; 电话号码: 13500000000
```

需要插入原样的大括号时，
可以双写，如：`"{{...}}"`。

## 48.5 字符串长度

`stringr::str_length(string)`求字符型向量`string`每个元素的长度。
一个汉字长度为1。

```
str_length(c("a", "bc", "def", "北京"))
```

```
## [1] 1 2 3 2
```

函数`nchar(text)`计算字符串长度，默认按照字符个数计算而不是按字节数计算，
如

```
nchar(c("a", "bc", "def", "北京"))
```

```
## [1] 1 2 3 2
```

注意函数对输入的字符型向量每个元素计算长度。

`nchar()`加选项`type="bytes"`可用按字符串占用的字节数计算，
这时一个汉字占用多个字节（具体占用多少与编码有关）。
如

```
nchar(c("a", "bc", "def", "北京"), type="bytes")
```

```
## [1] 1 2 3 6
```

## 48.6 取子串

`stringr::str_sub(string, start, end)`字符串字串，
用开始字符位置`start`和结束字符位置`end`设定字串位置。
用负数表示倒数位置。
默认开始位置为1，
默认结束位置为最后一个字符。

如：

```
str_sub("term2017", 5, 8)
```

```
## [1] "2017"
```

```
str_sub(c("term2017", "term2018"), 5, 8)
```

```
## [1] "2017" "2018"
```

```
str_sub("term2017", 5)
```

```
## [1] "2017"
```

```
str_sub("term2017", -4, -1)
```

```
## [1] "2017"
```

```
str_sub("term2017", end=4)
```

```
## [1] "term"
```

取子串时，一般按照字符个数计算位置，如

```
str_sub("北京市海淀区颐和园路5号", 4, 6)
```

```
## [1] "海淀区"
```

当起始位置超过总长度或结束位置超过第一个字符时返回空字符串；
当起始位置超过结束位置是返回空字符串。
如：

```
str_sub("term2017", 9)
```

```
## [1] ""
```

```
str_sub("term2017", 1, -9)
```

```
## [1] ""
```

```
str_sub("term2017", 8, 5)
```

```
## [1] ""
```

可以对`str_sub()`结果赋值，表示修改子串内容，如：

```
s <- "term2017"
str_sub(s, 5, 8) <- "18"
s
```

```
## [1] "term18"
```

字符串替换一般还是应该使用专用的替换函数如`stringr::str_replace_all()`，
`gsub()`。

基本R的`substring(text, first, last)`函数与`stringr::str_sub()`功能相同，
但`first`和`last`参数不允许用负数,
`last`的默认值是一个很大的数，所以省略`last`时会取到字符串末尾。
`substring()`对三个参数`text`, `first`, `last`都是向量化的，
长度不一致时按照一般的不等长向量间运算规则处理。如：

```
substring(c("term2017", "term2018"), first=c(1, 5), last=c(4, 8))
```

```
## [1] "term" "2018"
```

```
substring("term2017", first=c(1, 5), last=c(4, 8))
```

```
## [1] "term" "2017"
```

`substring()`也允许修改某个字符串的指定子串的内容，如

```
s <- "123456789"
substring(s, 3, 5) <- "abc"
s
```

```
## [1] "12abc6789"
```

R的`substr(x, start, stop)`作用类似，
但是仅支持`x`为字符型向量，
`start`和`stop`是标量。

## 48.7 字符串变换

### 48.7.1 大小写

`stringr::str_to_upper(string)`将字符型向量`string`中的英文字母都转换为大写。
类似函数有`stringr::str_to_lower(string)`转换为小写，
`stringr::str_to_title(string)`转换为标题需要的大小写，
`stringr::str_to_scentence(string)`转换为句子需要的大小写。
这都是针对英文的，
选项`locale`用来选语言，`locale="en"`为默认值。

基本R的`toupper()`将字符型向量的每个元素中的小写字母转换为大写,
`tolower()`转小写。

### 48.7.2 字符变换表

基本R的`chartr(old, new, x)`函数指定一个字符对应关系，
旧字符在`old`中，新字符在`new`中，`x`是一个要进行替换的字符型向量。
比如，下面的例子把所有`!`替换成`.`，把所有`;`替换成`,`：

```
chartr("!;", ".,", c("Hi; boy!", "How do you do!"))
```

```
## [1] "Hi, boy."       "How do you do."
```

```
chartr("。，；县", ".,;区", "昌平县，大兴县；固安县。")
```

```
## [1] "昌平区,大兴区;固安区."
```

第二个例子中被替换的标点是中文标点，替换成了相应的英文标点。

### 48.7.3 空白处理

`stringr::str_trim(string, side)`返回删去字符型向量`string`每个元素的首尾空格的结果，
可以用`side`指定删除首尾空格（`"both"`）、开头空格（`"left"`）、末尾空格（`"right"`）。
如：

```
str_trim(c("  李明", "李明  ", "  李明  ", "李  明"))
```

```
## [1] "李明"   "李明"   "李明"   "李  明"
```

```
str_trim(c("  李明", "李明  ", "  李明  ", "李  明"), side="left")
```

```
## [1] "李明"   "李明  " "李明  " "李  明"
```

```
str_trim(c("  李明", "李明  ", "  李明  ", "李  明"), side="right")
```

```
## [1] "  李明" "李明"   "  李明" "李  明"
```

`stringr::str_squish(string)`对字符型向量`string`每个元素，
删去首尾空格，将重复空格变成单个，返回变换后的结果。如：

```
str_squish(c("  李明", "李明  ", "  李明  ", "李  明"))
```

```
## [1] "李明"  "李明"  "李明"  "李 明"
```

基本R函数`trimws(x, which)`与`str_trim()`作用类似，
选项`which="left"`可以仅删去开头的空格，
选项`which="right"`可以仅删去结尾的空格。

```
trimws(c("  李明", "李明  ", "  李明  ", "李  明"))
```

```
## [1] "李明"   "李明"   "李明"   "李  明"
```

```
trimws(c("  李明", "李明  ", "  李明  ", "李  明"), which="left")
```

```
## [1] "李明"   "李明  " "李明  " "李  明"
```

```
trimws(c("  李明", "李明  ", "  李明  ", "李  明"), which="right")
```

```
## [1] "  李明" "李明"   "  李明" "李  明"
```

为了去掉输入字符串中所有空格，可以用`gsub()`替换功能，如：

```
gsub(" ", "", c("  李明", "李明  ", "  李明  ", "李  明"), fixed=TRUE)
```

```
## [1] "李明" "李明" "李明" "李明"
```

`stringr::str_pad(string, width)`可以将字符型向量`string`的每个元素加长到`width`个字符，
不足时左补空格，已经达到或超过`width`的则不变，如：

```
str_pad(c("12", "1234"), 3)
```

```
## [1] " 12"  "1234"
```

可以用选项`side`选择在哪里填补空格，
默认为`"left"`，
还可选`"right"`，`"both"`。

### 48.7.4 截短或分行

`stringr::str_trunc(x, width)`可以截短字符串。

`stringr::str_wrap(x, width)`可以将作为字符型向量的长字符串拆分成近似等长的行，
行之间用换行符分隔。

### 48.7.5 排序

基本R函数`sort()`可以用来对字符型向量的各个元素按照字典序排序，
但是字符的先后顺序是按照操作系统的当前编码值次序，
见关于`locales`的帮助。

`str_sort(x)`对字符型向量`x`排序。
可以用`locale`选项指定所依据的locale，
不同的locale下次序不同。
默认为`"en"`即英语，
中国大陆的GB编码(包括GBK和GB18030)对应的locale是`"zh"`。

`str_order(x)`返回将`x`的各个元素从小到大排序的下标序列。

## 48.8 简单匹配与查找

### 48.8.1 开头和结尾匹配

基本R的`startsWith(x, prefix)`可以判断字符型向量`x`的每个元素是否以`prefix`开头，
结果为一个与`x`长度相同的逻辑型向量。如

```
startsWith(c("xyz123", "tu004"), "tu")
```

```
## [1] FALSE  TRUE
```

`endsWith(x, suffix)`可以判断字符型向量`x`的每个元素是否以`suffix`结尾，
如

```
endsWith(c("xyz123", "tu004"), "123")
```

```
## [1]  TRUE FALSE
```

stringr包的`str_starts(string, pattern)`判断`string`的每个元素是否以模式`pattern`开头，
加选项`negate=TRUE`表示输出反面结果。
`pattern`是正则表达式，
如果需要用非正则表达式，可以用`fixed()`或者`coll()`保护，如：

```
str_starts(c("xyz123", "tu004"), fixed("tu"))
```

```
## [1] FALSE  TRUE
```

```
str_starts(c("xyz123", "tu004"), coll("tu"))
```

```
## [1] FALSE  TRUE
```

stringr包的`str_ends(string, pattern)`判断是否以给定模式结尾。

### 48.8.2 中间匹配

函数`grep()`, `grepl()`等可以用于查找子字符串，
位置不限于开头和结尾，
详见§[49.1](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/regex.html#regex-lang)。

在`grepl()`函数中加`fixed=TRUE`选项表示查找一般文本内容（非正则表达式）。
比如，查找字符串中是否含有our:

```
grepl("our", c("flavor", "tournament"), fixed=TRUE)
```

```
## [1] FALSE  TRUE
```

## 48.9 字符串替换

stringr包的`str_replace_all(string, fixed(pattern), replacement)`在字符型向量`string`的每个元素中查找子串`pattern`，
并将所有匹配按照`replacement`进行替换。

```
str_replace_all(c("New theme", "Old times", "In the present theme"),
  fixed("the"), "**")
```

```
## [1] "New **me"           "Old times"          "In ** present **me"
```

用`gsub(pattern, replacement, x, fixed=TRUE)`
把字符型向量`x`中每个元素中出现的子串
`pattern`都替换为`replacement`(注意与`str_replace_all`的自变量次序区别)。
如

```
gsub("the", "**",
     c("New theme", "Old times", "In the present theme"),
     fixed=TRUE)
```

```
## [1] "New **me"           "Old times"          "In ** present **me"
```

设有些应用程序的输入要求使用逗号“`,`”分隔，
但是用户可能输入了中文逗号“，”，
就可以用`gsub()`来替换：

```
x <- c("15.34,14.11", "13.25，16.92")
x <- gsub("，", ",", x, fixed=TRUE); x
```

```
## [1] "15.34,14.11" "13.25,16.92"
```

例子中`x`的第二个元素中的逗号是中文逗号。

函数`sub()`与`gsub()`类似，但是仅替换第一次出现的`pattern`。

## 48.10 字符串拆分

`stringr::str_split(string, pattern)`对字符型向量`string`的每一个元素按分隔符`pattern`进行拆分，
每个元素拆分为一个字符型向量，结果是一个列表，列表元素为字符型向量。
其中`pattern`是正则表达式，
为了按照固定模式拆分，用`fixed()`进行保护。如

```
x <- c("11,12", "21,22,23", "31,32,33,34")
res1 <- str_split(x, fixed(","))
res1
```

```
## [[1]]
## [1] "11" "12"
## 
## [[2]]
## [1] "21" "22" "23"
## 
## [[3]]
## [1] "31" "32" "33" "34"
```

`str_split()`可以用选项`n`指定仅拆分出成几项，最后一项合并不拆分，如：

```
x <- c("11,12", "21,22,23", "31,32,33,34")
res2 <- str_split(x, fixed(","), n=2)
res2
```

```
## [[1]]
## [1] "11" "12"
## 
## [[2]]
## [1] "21"    "22,23"
## 
## [[3]]
## [1] "31"       "32,33,34"
```

拆分的结果可以用`lapply()`, `sapply()`，`vapply()`等函数处理。
例如，
将每个元素的拆分结果转换成数值型：

```
lapply(res1, as.numeric)
```

```
## [[1]]
## [1] 11 12
## 
## [[2]]
## [1] 21 22 23
## 
## [[3]]
## [1] 31 32 33 34
```

可以用`unlist()`函数将列表中的各个向量连接成一个长向量，如：

```
unlist(res1)
```

```
## [1] "11" "12" "21" "22" "23" "31" "32" "33" "34"
```

注意，即使输入只有一个字符串，`str_split()`的结果也是列表，
所以输入只有一个字符串时我们应该取出结果列表的第一个元素，如

```
strsplit("31,32,33,34", split=",", fixed=TRUE)[[1]]
```

```
## [1] "31" "32" "33" "34"
```

如果确知每个字符串拆分出来的字符串个数都相同，
可以用`stringr::str_split_fixed()`，
用参数`n`指定拆出来的项数，
这时结果为一个字符型矩阵，
原来的每个元素变成结果中的一行：

```
x <- c("11,12", "21,22", "31,32")
res3 <- str_split_fixed(x, fixed(","), n=2)
res3
```

```
##      [,1] [,2]
## [1,] "11" "12"
## [2,] "21" "22"
## [3,] "31" "32"
```

基本R的`strsplit(x,split,fixed=TRUE)`
可以把字符型向量`x`的每一个元素按分隔符`split`拆分为一个字符型向量，
`strsplit`的结果为一个列表，
每个列表元素对应于`x`的每个元素。

如

```
x <- c("11,12", "21,22,23", "31,32,33,34")
res4 <- strsplit(x, split=",", fixed=TRUE)
res4
```

```
## [[1]]
## [1] "11" "12"
## 
## [[2]]
## [1] "21" "22" "23"
## 
## [[3]]
## [1] "31" "32" "33" "34"
```

## 48.11 文本文件读写

文本文件是内容为普通文字、用换行分隔成多行的文件，
与二进制文件有区别，
二进制文件中换行符没有特殊含义，
而且二进制文件的内容往往也不是文字内容。
二进制文件的代表有图片、声音，
以及各种专用软件的的私有格式文件，
如Word文件、Excel文件。

对于文本文件，可以用`readLines()`函数将其各行的内容读入为一个字符型数组，
字符型数组的每一个元素对应于文件中的一行，
读入的字符型数组元素不包含分隔行用的换行符。

最简单的用法是读入一个本地的文本文件，
一次性读入所有内容，用如

```
lines <- readLines("filename.ext")
```

其中`filename.ext`是文件名，
也可以用全路径名或相对路径名。

当文本文件很大的时候，
整体读入有时存不下，
即使能存下处理速度也很慢，
可以一次读入部分行，逐批读入并且逐批处理，这样程序效率更高。
这样的程序要复杂一些，例如

```
infcon <- file("filename.ext", open="rt")
batch <- 1000
repeat{
  lines <- readLines(infcon, n=batch)
  if(length(lines)==0) break
  ## 处理读入的这些行
}
close(infcon)
```

以上程序先打开一个文件，`inffcon`是打开的文件的读写入口（称为一个“连接对象”）。
每次读入指定的行并处理读入的行，直到读入了0行为止，
最后关闭`infcon`连接。

对文本文件的典型处理是读入后作一些修改，
另外保存。
函数`writeLines(lines, con="outfilename.txt")`可以将字符型向量`lines`的各个元素变成输出文件的各行保存起来，
自动添加分隔行的换行符。
如果是分批读入分批处理的，
则写入也需要分批写入，
以上的分批处理程序变成：

```
infcon <- file("filename.ext", open="rt")
outfcon <- file("outfilename.txt", open="wt")
batch <- 1000
while(TRUE){
  lines <- readLines(infcon, n=batch)
  if(length(lines)==0) break
  ## 处理读入的这些行, 变换成outlines
  writeLines(outlines, con=outfcon)
}
close(outfcon)
close(infcon)
```

`readLines()`也可以直接读取网站的网页文件，
如

```
lines <- readLines(url("https://www.r-project.org/"))
length(lines)
## [1] 116
head(lines)
## [1] "<!DOCTYPE html>"                                                             
## [2] "<html lang=\"en\">"                                                          
## [3] "  <head>"                                                                    
## [4] "    <meta charset=\"utf-8\">"                                                
## [5] "    <meta http-equiv=\"X-UA-Compatible\" content=\"IE=edge\">"               
## [6] "    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">"
```

readr包的`read_lines()`和`write_lines()`函数起到与基本R中
`readLines()`和`writeLines()`类似的作用，
`read_file()`和`read_file_raw()`可以将整个文件读入为一个字符串。

关于读写文件时的编码问题，
详见[15.7](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-io.html#p-io-enc)。