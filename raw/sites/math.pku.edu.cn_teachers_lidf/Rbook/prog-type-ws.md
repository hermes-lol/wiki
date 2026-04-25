---
crawl_time: '2026-01-17 14:19:37'
framework: rbook
title: 14 工作空间和变量赋值 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-type-ws.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 14 工作空间和变量赋值

## 14.1 工作空间

R把在命令行定义的变量都保存到工作空间中，
在退出R时可以选择是否保存工作空间。
这也是R与其他如C、Java这样的语言的区别之一。

用`ls()`命令可以查看工作空间中的内容。

随着多次在命令行使用R，
工作空间的变量越来越多，
使得重名的可能性越来越大，
而且工作空间中变量太多也让我们不容易查看其内容。
在命令行定义的变量称为“全局变量”，
在编程实践中，
全局变量是需要慎用的。

可以用`rm()`函数删除工作空间中的变量，格式如

```
rm(d, h, name, rec, sex, x)
```

要避免工作空间杂乱，
最好的办法还是所有的运算都写到自定义函数中。
自定义函数中定义的变量都是临时的，
不会保存到工作空间中。
这样，仅需要时才把变量值在命令行定义，
这样的变量一般是读入的数据或自定义的函数
（自定义函数也保存在工作空间中）。

可以定义如下的`sandbox()`函数：

```
sandbox <- function(){
  cat('沙盘：接连的空行回车可以退出。\n')
  browser()
}
```

运行`sandbox()`函数，将出现如下的browser命令行：

```
沙盘：接连的空行回车可以退出。
Called from: sandbox()
Browse[1]>
```

提示符变成了“Browser[n]”，其中`n`代表层次序号。
在这样的browser命令行中随意定义变量，
定义的变量不会保存到工作空间中。
用“Q”命令可以退出这个沙盘环境，
接连回车也可以退出。

## 14.2 非法变量名

R的变量名要求由字母、数字、下划线、小数点组成，
开头不能是数字、下划线、小数点，
中间不能使用空格、减号、井号等特殊符号，
变量名不能与`if`、`NA`等保留字相同。

有时为了与其它软件系统兼容，
需要使用不符合规则的变量名，
这只要将变量名两边用反向单撇号“`` ` ``”保护，
如：

```
`max score` <- 100
99 + `max score`
```

```
## [1] 199
```

如果变量名（元素名、列名等）是以字符串形式使用，
就不需要用“`` ` ``”保护。如：

```
x <- c("score a"=85, "score b"=66)
x
```

```
## score a score b 
##      85      66
```

## 14.3 变量赋值与绑定

本小节内容技术上比较复杂，
初学者可以略过。

在R中赋值本质上是把一个存储的对象与一个变量名“绑定”(bind)在一起，
比如：

```
x <- c(1,2,3)
```

并不是像C++、JAVA等语言那样，
`x`代表某个存储位置，
“`x <- c(1,2,3)`”代表将1到3这些值存储到`x`所指向的存储位置。
实际上，`<-`右边的`c(1,2,3)`是一个表达式，
其结果为一个R对象(object)，
而`x`只是一个变量名，
并没有固定的类型、固定的存储位置，
赋值的结果是将`x`绑定到值为`(1,2,3)`的R对象上。
R对象有值，但不必有对应的变量名；
变量名必须经过绑定才有对应的值和存储位置。

这样，同一个R对象也可以被两个或多个变量名绑定。
对于基本的数据类型如数值型向量，
两个指向相同对象的变量当一个变量被修改时自动制作副本。
`tracemem(x)`可以显示变量名`x`绑定的地址并在其被制作副本时显示地址变化。
如：

```
x <- c(1,2,3)
cat(tracemem(x), "\n")
```

```
## <0000025765221598>
```

```
y <- x    # 这时y和x绑定到同一R对象
cat(tracemem(y), "\n")
```

```
## <0000025765221598>
```

```
y[3] <- 0 # 这时y制作了副本
```

```
## tracemem[0x0000025765221598 -> 0x000002576536c4c8]: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/eval eval eval_with_user_handlers withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir in_input_dir eng_r block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous> do.call eval eval eval eval eval.parent local
```

```
x
```

```
## [1] 1 2 3
```

```
y
```

```
## [1] 1 2 0
```

```
untracemem(x); untracemem(y)
rm(x, y)
```

可见`y <- x`并没有制作副本，
但是修改`y[3]`值时就对`y`制作了副本。

修改某个变量名所指向的对象可能会制作副本，如：

```
x <- c(1,2,3)
cat(tracemem(x), "\n")
```

```
## <00000257659A3998>
```

```
x[3] <- 0
```

```
## tracemem[0x00000257659a3998 -> 0x0000025765a2d198]: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/eval eval eval_with_user_handlers withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir in_input_dir eng_r block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous> do.call eval eval eval eval eval.parent local
```

```
x
```

```
## [1] 1 2 0
```

```
untracemem(x)
```

在调用函数时，
如果函数内部不修改自变量的元素值，
输入的自变量并不制作副本，
而是直接被函数使用实参绑定的对象。
如：

```
x <- c(1,2,3)
cat(tracemem(x), "\n")
```

```
## <0000025765F54FD8>
```

```
f <- function(v){ return(v) }
z <- f(x)
cat(tracemem(z), "\n")
```

```
## <0000025765F54FD8>
```

```
untracemem(x); untracemem(z)
```

从上面的例子可以看出，
函数`f`以`x`为实参，
但不修改`x`的元素，
不会生成`x`的副本，
返回的值是`x`指向的对象本身，
再次赋值给`z`，
也不制作副本，
`z`和`x`绑定到同一对象。

如果函数内部修改自变量的元素值，
则输入的自变量也会制作副本。
如：

```
x <- c(1,2,3)
cat(tracemem(x), "\n")
```

```
## <0000025766551E58>
```

```
f2 <- function(v){ v[1] <- -999; return(v) }
z <- f2(x)
```

```
## tracemem[0x0000025766551e58 -> 0x0000025766606dd8]: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/f2 eval eval eval_with_user_handlers withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir in_input_dir eng_r block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous> do.call eval eval eval eval eval.parent local
```

```
cat(tracemem(z), "\n")
```

```
## <0000025766606DD8>
```

```
untracemem(x); untracemem(z)
```

从程序输出看，
函数`f2()`以`x`为实参，
并修改`x`的内部元素，
就制作了`x`的副本，
返回的结果赋给变量`z`，
绑定的是修改后的副本。

如果在函数中对自变量重新赋值，
这实际是重新绑定，
也不会制作输入的实参的副本。

如果修改`y`的元素值时还修改了其存储类型，
比如整型改为浮点型，
则会先制作`y`的副本，
然后制作类型改变后的副本，
然后再修改其中的元素值。

在当前的R语言中，
一个对象的引用（如绑定的变量名）个数，
只区分0个、1个或多个这三种情况。
在没有引用时，
R的垃圾收集器会定期自动清除这些对象。
`rm(x)`只是删除绑定，
并不会马上清除`x`绑定的对象。
如果已经有多个引用，
即使是只有2个，
减少一个引用也还是“多个”状态，
不会变成1个。

垃圾收集器是在R程序要求分配新的对象空间时自动运行的，
R函数`gc()`可以要求马上运行垃圾收集器，
并返回当前程序用到的存储量；
lobstr包的`mem_used()`函数则报告当前会话内存字节数。

在上面的示例中，
用了基本类型的向量讲解是否制作副本。
考虑其它类型的复制。

如果`x`是一个有5个元素的列表，
则`y <- x`使得`y`和`x`指向同一个列表对象。
但是，
列表对象的每个元素实际上也相当于一个绑定，
每个元素指向一个元素值对象。
所以如果修改`y`：`y[[3]] <- 0`，
这时列表`y`首先被制作了副本，
但是每个元素指向的元素值对象不变，
仍与`x`的各个元素指向的对象相同；
然后，
`y[[3]]`指向的元素值进行了重新绑定，
不再指向`x[[3]]`，
而是指向新的保存了值`0`的对象，
但`y`的其它元素指向的对象仍与`x`公用。
列表的这种复制方法称为浅拷贝，
列表对象及各个元素绑定被复制，
但各个元素指向（保存）的对象不变。
这种做法节省空间也节省运行时间。
在R的3.1.0之前则用的深拷贝方法，
即复制列表时连各个元素保存的值也制作副本。

如果`x`是一个数据框，
这类似于一个列表，
每个变量相当于一个列表元素，
数据框的每一列实际绑定到一个对象上。
如果`y <- x`，
则修改`y`的某一列会对`y`进行浅拷贝，
然后仅该列被制作了副本并被修改，
其它未修改的列仍与`x`共用值对象。

但是如果修改数据框`y`的一行，
因为这涉及到所有列，
所以整个数据框的所有列都会制作副本。

对于字符型向量，
实际上R程序的所有字符型常量都会建立一个全局字符串池，
这样有许多重复值时可以节省空间。

用lobstr包的`obj_size()`函数可以求变量的存储大小，
如`obj_size(x)`，
也可以求若干个变量的总大小，
如`obj_size(x,y)`。
因为各种绑定到同一对象的可能性，
所以变量的存储大小可能会比想象要少，
比如，
共用若干列的两个数据框，
字符型向量，
等等。
基本R软件的`object.size()`则不去检查是否有共享对象，
所以对列表等变量的存储大小估计可能会偏高。

从R 3.5.0版开始，`1:n`这种对象仅保存其开始值和结束值。

在自定义函数时，
自变量通常是按引用使用的，
函数内部仅使用自变量的值而不对其进行修改时不会制作副本，
但是如果函数内部修改了自变量的值，
就会制作副本，
所以如果自变量的存储量很大而且被修改后返回，
调用这个函数时就会造成运行速度缓慢。
在函数内应慎重修改自变量的值。

在循环中修改数据框的列，
也会造成反复的复制。

## 14.4 环境

前面讲的工作空间就是一个环境，
称为“全局环境”。
环境可以认为是包含了变量绑定和函数定义的一个对象，
对环境的修改都不会制作副本。
一般我们只需要用到全局环境，
在自定义函数的运行过程中会有一个局部的运行环境，
但是在函数内定义的嵌套函数可以带有其定义处的环境，
这样可以实现有历史记忆的函数，
类似于其它面向对象程序设计语言如C++等，
对象可以同时有状态（变量成员）和适用操作（方法成员）。

环境的应用将在“函数进阶”部分讲到内嵌函数、函数工厂时详细讲解。