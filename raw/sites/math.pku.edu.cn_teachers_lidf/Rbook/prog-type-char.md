---
crawl_time: '2026-01-17 14:19:47'
framework: rbook
title: 6 字符型数据及其处理 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-char.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 6 字符型数据及其处理

## 6.1 字符型向量

字符型向量是元素为字符串的向量。
字符串在程序中写成用两个双撇号包围或者用两个单撇号包围的内容。
如

```
s1 <- c('abc', '', 'a cat', NA, '李明')
```

注意空字符串并不能自动认为是缺失值，
字符型的缺失值仍用`NA`表示。

字符串内容一般从文件、网络、数据库获得，
在程序中直接用双撇号或者单撇号写出只是输入字符串的办法之一。

## 6.2 转义字符和原始字符串 {p-t-char-raw}

为了在字符串中表示一个双撇号，
可以用反斜杠在前面标明，
称为“转义”，如：

```
cat("\"\n")
```

```
## "
```

其中`"\n"`也是转义的字符，
表示换行。

当需要转义的内容较多时，
可以使用原始字符串(raw string)，
方法是用`r"(...)"`的格式，
其中`...`是实际内容。如：

```
cat(r"(C:\disk\course\math\nFinished!\n)")
```

```
## C:\disk\course\math\nFinished!\n
```

注意其中`\n`也被当作普通字符解释了，
不再当作换行符。

原始字符串如果内容中包含了圆括号，
可以将边界的圆括号改为方括号`[]`或者大括号`{}`。
如果这样也不能避免歧义，
可以在开始和结尾加上相同个数的减号，
格式为`r"--(...)--"`，
其中`...`为实际内容，
减号个数可以根据需要增加。

## 6.3 paste函数

针对字符型数据最常用的R函数是`paste()`函数。
`paste()`用来连接两个字符型向量，
元素一一对应连接，
默认用空格连接。
如`paste(c("ab", "cd"), c("ef", "gh"))`
结果相当于`c("ab ef", "cd gh")`。

`paste()`在连接两个字符型向量时采用R的一般向量间运算规则，
而且可以自动把数值型向量转换为字符型向量。
可以作一对多连接，
如`paste("x", 1:3)`结果相当于`c("x 1", "x 2", "x 3")`。

用`sep=`指定分隔符，
如`paste("x", 1:3, sep="")`结果相当于`c("x1", "x2", "x3")`。

使用`collapse=`参数可以把字符型向量的各个元素连接成一个单一的字符串,
如`paste(c("a", "b", "c"), collapse="")`结果相当于`"abc"`。

## 6.4 转换大小写

`toupper()`函数把字符型向量内容转为大写，
`tolower()`函数转为小写。
比如，`toupper('aB cd')`结果为`"AB CD"`，
`tolower(c('aB', 'cd'))`结果相当于`c("ab" "cd")`。
这两个函数可以用于不区分大小写的比较，
比如，不论x的值是`'JAN'`, `'Jan'`还是`'jan'`，
`toupper(x)=='JAN'`的结果都为TRUE。

## 6.5 字符串长度

用`nchar(x, type='bytes')`计算字符型向量`x`中每个字符串的以字节为单位的长度，这一点对中英文是有差别的，
中文通常一个汉字占两个字节，英文字母、数字、标点占一个字节。
用`nchar(x, type='chars')`计算字符型向量`x`中每个字符串的以字符个数为单位的长度，这时一个汉字算一个单位。

在画图时可以用`strwidth()`函数计算某个字符串或表达式占用的空间大小。

## 6.6 取子串

`substr(x, start, stop)`从字符串x中取出从第start个到第stop个的子串，
如

```
substr('JAN07', 1, 3)
## [1] "JAN"
```

如果x是一个字符型向量，`substr`将对每个元素取子串。如

```
substr(c('JAN07', 'MAR66'), 1, 3)
## [1] "JAN" "MAR"
```

用`substring(x, start)`可以从字符串x中取出从第start个到末尾的子串。如

```
substring(c('JAN07', 'MAR66'), 4)
## [1] "07" "66"
```

## 6.7 类型转换

用`as.numeric()`把内容是数字的字符型值转换为数值，如

```
substr('JAN07', 4, 5)
## [1] "07"
substr('JAN07', 4, 5) + 2000
## Error in substr("JAN07", 4, 5) + 2000 : 
##   non-numeric argument to binary operator
as.numeric(substr('JAN07', 4, 5)) + 2000
## [1] 2007
as.numeric(substr(c('JAN07', 'MAR66'), 4, 5))
## [1]  7 66
```

`as.numeric()`是向量化的，
可以转换一个向量的每个元素为数值型。

用`as.character()`函数把数值型转换为字符型，如

```
as.character((1:5)*5)
## [1] "5"  "10" "15" "20" "25"
```

如果自变量本来已经是字符型则结果不变。

为了用指定的格式数值型转换成字符型，
可以使用`sprintf()`函数，
其用法与C语言的`sprintf()`函数相似，
只不过是向量化的。例如

```
sprintf('file%03d.txt', c(1, 99, 100))
```

```
## [1] "file001.txt" "file099.txt" "file100.txt"
```

readr包的`parse_number()`输入一个字符串向量，
对每个字符串，
找到第一个能识别为数值的内容，
舍弃其它内容，
返回转换浮点型结果。
没有数值时返回缺失值，
并增加一个表格用来记录所有的不成功转换。
如：

```
readr::parse_number(c(
  "123", "output-123.txt", "a123.456bc04", 
  "30.2%", "abc"
))
```

```
## Warning: 1 parsing failure.
## row col expected actual
##   5  -- a number    abc
```

```
## [1]  123.000 -123.000  123.456   30.200       NA
## attr(,"problems")
## # A tibble: 1 × 4
##     row   col expected actual
##   <int> <int> <chr>    <chr> 
## 1     5    NA a number abc
```

readr中还有`parse_integer`,
`parse_double`, `parse_logical`,
`parse_character`等函数，
这些函数不允许要读取内容以外的内容存在，
比如`readr::parse_number("text-123")`能正确读取`-123`，
而`readr::parse_integer("text-123")`则会返回缺失值，
并带有一个说明缺失情况的表格作为属性。

## 6.8 字符串替换功能

用`gsub()`可以替换字符串中的子串，
这样的功能经常用在数据清理中。
比如，把数据中的中文标点改为英文标点，
去掉空格，等等。
如

```
x <- '1, 3; 5'
gsub(';', ',', x, fixed=TRUE)
## [1] "1, 3, 5"
```

字符串`x`中分隔符既有逗号又有分号，
上面的程序用`gsub()`把分号都换成逗号。

更多的文本数据（字符型数据）功能参见[48](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/text.html#text)。

## 6.9 正则表达式

正则表达式(regular expression)是一种匹配某种字符串模式的方法。
用这样的方法，可以从字符串中查找某种模式的出现位置，
替换某种模式，等等。
这样的技术可以用于文本数据的预处理，
比如用网络爬虫下载的大量网页文本数据。
R中支持perl语言格式的正则表达式，
`grep()`和`grepl()`函数从字符串中查询某个模式，
`sub()`和`gsub()`替换某模式。
比如，
下面的程序把多于一个空格替换成一个空格

```
gsub('[[:space:]]+', ' ', 
    'a   cat  in a box', perl=TRUE)
```

```
## [1] "a cat in a box"
```

正则表达式功能强大但也不容易掌握。
详见[48](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/text.html#text)。