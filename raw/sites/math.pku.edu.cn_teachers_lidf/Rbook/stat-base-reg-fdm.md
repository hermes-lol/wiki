---
crawl_time: '2026-01-17 14:20:35'
framework: rbook
title: 34 R公式界面与设计阵 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-base-reg-fdm.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 34 R公式界面与设计阵

## 34.1 R语言公式界面

R语言继承了来自S语言的公式界面，
用以描述统计模型中因变量和自变量的关系，
并有相应的将自变量群组转换为相应的线性模型设计阵的默认规则。

R语言的线性回归(`lm`)、方差分析(`aov`)、广义线性模型(`glm`)、线性混合模型(`nlme::lme`)等回归类建模函数都使用公式(formula)界面描述因变量与自变量之间的关系。
公式都包含符号`~`，在`~`左边是因变量，
在`~`右边有一到多个自变量或者固定效应，
各个自变量（效应）之间用加号`+`连接；
两个自变量之间的交互作用效用之间以冒号`:`连接。

### 34.1.1 公式中的运算符

`+`和`~`是公式中最基本的运算符，
还有其它一些辅助的运算符用来简化公式格式。
下面给出一些仅使用基本运算符的公式的例子，
其中`y`表示因变量，
`x1`是连续型自变量，
`f1`, `f2`, `f3`是因子：

* `y ~ x1`： 一元线性回归；
* `y ~ 1 + x1`: 显式表明有截距项;
* `y ~ 0 + x1`: 表明没有截距项；
* `y ~ f1 + x1`: 仅有因子主效应的协方差分析，
  或平行线回归模型;
* `y ~ f1 + x1 - 1`: 也是平行线模型但是`f1`每个水平都直接有截距项估计；
* `y ~ f1 + x1 + f1:x1`:
  包含因子主效应以及因子与连续型自变量的交互作用，
  即截距和斜率都不同的模型;
* `y ~ -1 + f1 + f1:x1`,
  也是因子不同水平下都关于`x1`有不同截距和斜率的模型，
  而且对每个水平都直接有截距项和斜率项估计；
* `y ~ f1 + f2 + f1:f2`:
  包含主效应和交互作用效应的两因素方差分析；
* `y ~ f1 + f1:f3`:
  包含`f1`主效应和嵌套在`f1`内部的`f3`效应的方差分析；
* `y ~ x1 + f1 + f2 + x1:f1 + x1:f2 + f1:f2`:
  包括三个主效应和所有交互效应的模型。

在公式中，
将截距项称为0阶项，
主效应称为1阶项，
两个自变量的交互作用称为2阶项，
等等。

公式中一般自动包含截距项，
可以用一个`0`或者`-1`项表示没有截距项。

公式中还可以用如下的操作符对公式进行简化描述：

* `*`: 两个因子的主效应以及交互作用效应，
  如`f1 * f2`等价于`f1 + f2 + f1:f2`。
* `f3 %in% f1`: 表示因子`f3`嵌套在因子`f1`内部，
  如`f1 + f3 %in% f1`等价于`f1 + f1:f3`，
  这时，`f1`的效应照常估计，
  由于因子的哑变量编码通常会去掉第一个水平作为基线，
  所以\(K\)个水平`f1`的主效应仅显示后\(K-1\)个水平的效应，
  而`f3`的效应是针对`f1`的\(K\)个水平的每一个都显示。
* `f1 / f3`：表示`f1`主效应以及`f3`嵌套在`f1`内的交互作用，
  等价于`f1 + f1:f3`。
* `f1 / x1`: 表示`f1`主效应和`x1`嵌套在`f1`内，
  实际效果是`f1`的每组对`x1`有不同的截距项和不同的斜率项，
  不同的截距项实际是由`f1`主效应提供的，
  等效于`f1 + f1:x`。
  `-1 + f1 / x1`则不使用共同的截距项，
  使得每个`f1`水平对`x1`有单独的截距和斜率，
  其中的截距的解释更直接。
* `^`：将若干项的和进行平方表示这些项的主效应以及两两的交互作用，
  更高的方幂可以表示更多因子的交互作用，
  如`(x1 + f1 + f2)^2`表示这三项的主效应以及两两的交互作用效应，
  注意`x1:x1`, `f1:f1`并没有意义。
* `-`: 扣除某一项，
  如`(x1 + f1 + f2)^2 - f1:f2`会扣除本来会包括的`f1`和`f2`的交互作用效应。

### 34.1.2 公式中的复合项

在公式中还可以用自变量的变换函数、自变量的多项式、基本样条等函数，
以及用`I()`保护的一般自变量表达式。
如：

* 用`sqrt(x1)`, `log(x1)`, `exp(x1)`等表示自变量或因变量的某种函数变换；
* 用`orderd(x1, breaks)`表示将连续型自变量用指定的分点转换成有序因子；
* 用`poly(x1, 2)`表示`x1`的一次项和二次项（零次项即截距项一般与模型截距项合并）；
* 用`bs(x1, df=3)`表示`x1`的基本样条基展开。

因为自变量的计算公式也用到加减乘除这些符号，
而这些符号在公式界面中有另外的解释，
所以用计算表达式表示某个公式项时，
应将这个公式项用`I()`保护，
如`I(x1 + x2*x3)`是直接计算`x1 + x2*x3`的值作为一个效应而不是表示四个公式效应。

可以用`formula()`函数显式地生成公式，
用`update()`函数修改公式。
如：

```
form1 <- formula(y ~ (x1 + x2 + x3)^2)
form2 <- update(form1, . ~ . - x2:x3)
```

`update()`中的`.`表示公式原来的左侧或右侧内容。

### 34.1.3 term类型

在`lm()`等函数使用公式时，
会产生一个`term`类型的对象，
这个对象与输入数据框配合就可以产生模型框(model frame)和设计阵。
term对象保存了公式用到的变量、公式中各个项、有无截距项等信息，
如果公式中用了`*`，`^`这样的简写，
term对象会将其转换成用`:`表达的形式。

## 34.2 R的设计阵

在线性模型应用中，
给定了公式和输入数据框后，
将这两者合并成为一个模型框(model frame)，
然后将所有连续型自变量、因子、交互作用效应进行编码，
得到一个设计阵，
这就是线性模型\(\boldsymbol y = X \boldsymbol{\beta} + \boldsymbol{\epsilon}\)中的\(X\)矩阵。
实际上，
这些转换一般不需要建模用户自己操作，
而是由`lm()`, `glm()`, `nlme::lme()`等函数自动在后台完成。
但是，当效应比较复杂时，
为了能够更好地理解估计问题并看懂估计结果，
有必要了解这些背后的转换。

这些转换的步骤为：从公式生成terms对象，
将terms对象与输入数据框转换成模型框，
将模型框转换成设计阵。

当所有自变量都是连续型变量时，
如果有截距项，
这时设计阵就是每个自变量的观测值占一列，
并在第一列有代表截距项的全为1的一列。
这与一般的多元线性回归教材的做法一致。

### 34.2.1 生成模型框

模型框是一个和数据框类似的R对象，
是将公式所需的变量从输入数据框中提取计算而得，
并将公式以terms对象的形式作为属性保存。
除了各种建模函数自动生成以外，
可以用`model.frame()`显式地生成模型框，
此函数输入`formula`, `data`, `subset`和`na.action`自变量。
`na.action`默认为`na.omit`，
即删除有缺失的观测；
类似的`na.exclude`在建模时删除有缺失的观测但计算残差和预测值时可对有缺失的观测计算。

例如，nmleU包的armd.wide数据框包含了ARMD眼科疾病的治疗数据，
如下的程序生成了某个模型的模型框：

```
data(armd.wide, package="nlmeU")
```

公式：

```
armd.fm1 <- formula(
  visual52 ~                    # 因变量，连续型
    sqrt(line0) +               # 连续型自变量的函数变换
    factor(lesion) +            # 有四个水平的因子
    treat.f * log(visual24) +   
    # 两水平的因子与上颚连续型自变量函数变换的主效应和交互作用效应
    poly(visual0, 2))           # 连续型自变量的一次和二次项
```

模型框：

```
armd.mf1 <- model.frame(
  armd.fm1,                             # 输入公式
  data = armd.wide,                     # 输入数据框
  subset = !(subject %in% c("1", "2")), # 取子集
  na.action = na.exclude, # 缺失值处理策略
  SubjectId = subject)    # 可以额外定义变量
class(armd.mf1)
```

```
## [1] "data.frame"
```

```
## 原始数据的观测行数和变量数
dim(armd.wide)
```

```
## [1] 240  10
```

```
## 注意模型框的观测行数少于输入数据框，因为取了子集并进行了缺失值处理
dim(armd.mf1)
```

```
## [1] 189   7
```

```
names(armd.mf1)
```

```
## [1] "visual52"         "sqrt(line0)"      "factor(lesion)"   "treat.f"         
## [5] "log(visual24)"    "poly(visual0, 2)" "(SubjectId)"
```

将模型框作为数据框显示：

```
head(armd.mf1, 3)
```

```
##   visual52 sqrt(line0) factor(lesion) treat.f log(visual24) poly(visual0, 2).1
## 4       68    3.605551              2 Placebo      4.158883        0.052346233
## 6       42    3.464102              3  Active      3.970292        0.017581526
## 7       65    3.605551              1 Placebo      4.276666        0.039309468
##   poly(visual0, 2).2 (SubjectId)
## 4       -0.005443471           4
## 6       -0.046084343           6
## 7       -0.024394420           7
```

可见模型框提取了原始连续型变量，
计算了连续型变量的变换，
原样提取了因子而没有生成哑变量，
计算了`poly()`需要的一次项和二次项，
提取了因子的主效应但是没有计算因子和连续型变量的交互作用效应。
注意前面`names(armd.mf1)`的结果为7项，
但是显示的模型框中有8列，
这是因为`poly(visual0, 2)`生成了两列，
作为一个两列的矩阵保存在模型框中。
在`model.frame()`调用中用`SubjectId=subjectid`将公式中没有出现的变量`subjectid`改名为`(SubjectId)`复制到了输出的模型框中（注意名字带有括号），
这种做法还可以将不在设计阵中用到的变量包含进模型框中，
比如模型的权重、位移等。

因为因子需要变成哑变量，
还需要计算交互作用效应，
所以模型框是一个中间结果，
不能直接用来进行线性模型计算。

### 34.2.2 模型框中terms对象的属性

数据框中包含了公式转换出来的terms对象，
以模型框的属性的形式保存。
因为公式有对应的数据框作为参考，
所以可以知道公式中哪些是连续型变量，
哪些是因子，
所以terms对象包含了比公式本身更详细的信息。如：

```
armd.tm1 <- attr(armd.mf1, "terms")
class(armd.tm1)
```

```
## [1] "terms"   "formula"
```

terms对象的属性中保存公式和模型框的重要信息，
其属性有：

```
names(attributes(armd.tm1))
```

```
##  [1] "variables"    "factors"      "term.labels"  "order"        "intercept"   
##  [6] "response"     "class"        ".Environment" "predvars"     "dataClasses"
```

```
attr(armd.tm1, "variables")
```

```
## list(visual52, sqrt(line0), factor(lesion), treat.f, log(visual24), 
##     poly(visual0, 2))
```

variable属性是模型框的各变量的名字，
注意其中`poly(visual0, 2)`代表一个两列矩阵。

```
attr(armd.tm1, "factors")
```

```
##                  sqrt(line0) factor(lesion) treat.f log(visual24)
## visual52                   0              0       0             0
## sqrt(line0)                1              0       0             0
## factor(lesion)             0              1       0             0
## treat.f                    0              0       1             0
## log(visual24)              0              0       0             1
## poly(visual0, 2)           0              0       0             0
##                  poly(visual0, 2) treat.f:log(visual24)
## visual52                        0                     0
## sqrt(line0)                     0                     0
## factor(lesion)                  0                     0
## treat.f                         0                     1
## log(visual24)                   0                     1
## poly(visual0, 2)                1                     0
```

factors属性保存一个矩阵，
表现模型框中各项出现在公式的哪些项中，
用0表示不出现，1表示出现而且对因子使用对照方式编码，
2表示出现而且对因子使用全部哑变量编码（这适用于仅有高阶项没有相应低阶项情形）。即使没有截距项因子的主效应也用1表示，这时第一个因子用全部哑变量编码。

```
attr(armd.tm1, "term.labels")
```

```
## [1] "sqrt(line0)"           "factor(lesion)"        "treat.f"              
## [4] "log(visual24)"         "poly(visual0, 2)"      "treat.f:log(visual24)"
```

term.labels属性是将公式展开后各项的标签，
这会将`*`, `^`的简写展开，
但因子和`poly()`等在设计阵中需要变成多列的不会有多个标签。

```
attr(armd.tm1, "order")
```

```
## [1] 1 1 1 1 1 2
```

orders属性是terms.label各项对应的阶数，
主效应是1阶，
两个变量的交互作用效用是2阶。

```
attr(armd.tm1, "intercept")
```

```
## [1] 1
```

虽然公式中没有显式地写出模型的截距项，
但默认有截距项，
上面结果1就是表示截距项。

```
attr(armd.tm1, "response")
```

```
## [1] 1
```

response属性保存因变量在模型框中的次序，
0表示公式中没有因变量。

```
attr(armd.tm1, "predvars")
```

```
## list(visual52, sqrt(line0), factor(lesion), treat.f, log(visual24), 
##     poly(visual0, 2, coefs = list(alpha = c(54.9541666666667, 
##     50.5097520799239), norm2 = c(1, 240, 52954.4958333333, 16341393.4347853
##     ))))
```

predvars列出了模型因变量和自变量，
其中`poly()`给出了所用的正交多项式参数，
在利用模型进行预测时可能需要利用这个属性中保存的参数。

```
attr(armd.tm1, "dataClasses")
```

```
##         visual52      sqrt(line0)   factor(lesion)          treat.f 
##        "numeric"        "numeric"         "factor"         "factor" 
##    log(visual24) poly(visual0, 2)      (SubjectId) 
##        "numeric"      "nmatrix.2"         "factor"
```

dataClasses属性给出了模型框中变量的数据类型。
注意其中`poly(visual0, 2)`保存了一个两列的数值型矩阵。

### 34.2.3 模型框中poly等变量

在上面的例子中，
公式中的`poly(visual0, 2)`在模型框中生成了一个两列的矩阵。
`poly()`函数会生成自变量的正交多项式系数，
而使用的正交多项式是与输入自变量值有关的。
splines包提供的`bs()`和`ns()`函数也需要从自变量值计算节点，
所以也是与输入自变量有关的。
与之相对照，`sqrt(x)`等变换则总是执行固定的函数变换。

`poly()`函数在计算所用到的正交多项式系数时会使用输入数据框的所有观测，
不会按照`model.frame()`函数中的`subset`选项和`na.action`选项进行子集处理。
计算出的参数会保存在terms对象的`predvars`属性中。
如果对原始输入数据中的visual0计算得到的带有参数的`poly()`函数，
可以发现结果是两列的矩阵，
分别是一次多项式变换和二次多项式变换，
每列的列和等于0，从而与截距项（零次项）正交，
每列的平方和等于1（标准化），
两列之间正交。

### 34.2.4 生成设计阵

为了能够利用线性模型计算方法进行模型估计，
需要将模型框中的因子变成可计算的哑变量或者对照矩阵，
交互作用效应计算出来。
R的建模函数如`lm()`会在后台自动完成这样的转换，
为了理解估计过程以及输出的模型参数估计结果，
可以用`model.matrix()`函数直接产生设计阵进行学习。
另外，R中某些包（如机器学习类）不支持公式界面，
必须自己输入可直接计算的自变量矩阵（设计阵），
这也需要调用`model.matrix()`完成。

`model.matrix()`以公式和模型框为输入，
输出数值型的设计阵：

```
armd.dm1 <- model.matrix(
  armd.fm1, armd.mf1)
dim(armd.dm1)
```

```
## [1] 189  10
```

生成的设计阵与模型框观测行数相同，
列数增加了。
各列为：

```
colnames(armd.dm1)
```

```
##  [1] "(Intercept)"                 "sqrt(line0)"                
##  [3] "factor(lesion)2"             "factor(lesion)3"            
##  [5] "factor(lesion)4"             "treat.fActive"              
##  [7] "log(visual24)"               "poly(visual0, 2)1"          
##  [9] "poly(visual0, 2)2"           "treat.fActive:log(visual24)"
```

使用列名的简写将前几行列表显示：

```
knitr::kable(head(armd.dm1),
  col.names = abbreviate(colnames(armd.dm1)),
  digits=2)
```

|  | (In) | s(0) | f()2 | f()3 | f()4 | tr.A | l(24 | p(0,2)1 | p(0,2)2 | t.A: |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 4 | 1 | 3.61 | 1 | 0 | 0 | 0 | 4.16 | 0.05 | -0.01 | 0.00 |
| 6 | 1 | 3.46 | 0 | 1 | 0 | 1 | 3.97 | 0.02 | -0.05 | 3.97 |
| 7 | 1 | 3.61 | 0 | 0 | 0 | 0 | 4.28 | 0.04 | -0.02 | 0.00 |
| 8 | 1 | 2.83 | 0 | 1 | 0 | 0 | 3.61 | -0.07 | -0.01 | 0.00 |
| 9 | 1 | 3.46 | 1 | 0 | 0 | 1 | 3.99 | 0.02 | -0.05 | 3.99 |
| 12 | 1 | 3.00 | 0 | 0 | 0 | 1 | 3.30 | -0.04 | -0.04 | 3.30 |

设计阵的第一列是`(Intercept)`，
模型截距项，是一列1。
因子`factor(lesion)`有4个水平，
用三个哑变量表示；
因子`factor(treat.f)`有两个水平，
用一个哑变量表示；
交互作用`treat.f:log(visual24)`是两水平的因子和连续型变量的交互，
等于一个零壹变量和一个连续型变量的乘积，
在设计阵中变量名为`treat.fActive:log(visual24)`，
即处理组的示性函数与`log(visual24)`的乘积。

设计阵有一个属性`assign`，
表示设计阵的每一列来自模型框term对象的哪一项：

```
attr(armd.dm1, "assign")
```

```
##  [1] 0 1 2 2 2 3 4 5 5 6
```

```
attr(armd.tm1, "term.labels")
```

```
## [1] "sqrt(line0)"           "factor(lesion)"        "treat.f"              
## [4] "log(visual24)"         "poly(visual0, 2)"      "treat.f:log(visual24)"
```

这表明设计阵的第1列截距项在`term.labels`没有对应的项，
设计阵的第2列`sqrt(line0)`对应于`term.labels`的同名的第1项，
设计阵的第3-5列`factor(lesion)2`、`factor(lesion)3`、`factor(lesion)4`对应于`term.labels`的第2项`factor(lesion)`，
设计阵的第6列`treat.fActive`对应于`term.labels`的第3项`treat.f`，
设计阵的第7列`log(visual24)`对应于`term.labels`的同名的第4项，
设计阵的第8-9列`poly(visual0, 2)1`、`poly(visual0, 2)2`对应于的第5项`poly(visual0, 2)`，
设计阵的第10列`treat.fActive:log(visual24)`对应于`term.labels`的第6项`treat.f:log(visual24)`。

因子和含有因子的交互作用需要进行编码，
其编码方式存放在设计阵的属性`contrasts`中：

```
attr(armd.dm1, "contrasts")
```

```
## $`factor(lesion)`
## [1] "contr.treatment"
## 
## $treat.f
## [1] "contr.treatment"
```

## 34.3 R对照矩阵

因子的主效应反映在设计阵中，
需要使得因子的每个水平有自己对截距项的不同贡献。
例如，最简单的仅有一个\(K\)水平因子\(a\)作为自变量的模型为
\[\begin{align}
y\_{ji} = \mu + \alpha\_{j} + e\_{ji},
i=1,2,\dots,n\_i; j=1,2,\dots, K,
\tag{34.1}
\end{align}\]
其中\(y\_{ji}\)是自变量因子在第\(j\)水平的第\(i\)个观测。
这时，
设计阵可以使用\(X = (x\_{ij})\_{n \times (K+1)}\)，
其中第一列都等于1，
对应于参数\(\mu\)，
第\(j+1\)列取因子\(a\)的第\(j\)水平的示性函数值，
对应于参数\(\alpha\_j\)，
\(x\_{i,j+1}=1\)当且仅当\(y\_{ij}\)属于第\(j\)组，否则取0。
这样的设计阵的问题是\(X\)的第一列都等于1而其余列的和等于第一列，
使得设计阵不满秩。
记这个设计阵为\(X = (\boldsymbol 1 \ X\_a)\)，
其列数为\(K+1\)但列秩为\(K\)。
为此，
取一个\(K \times (K-1)\)的“对照矩阵”\(C\_a\)，
取新的设计阵为\(X^\* = (\boldsymbol 1 \ X\_a C\_a)\)，
则\(X^\*\)为\(n \times K\)列满秩矩阵。

关于对照矩阵的一个必要条件是\((\boldsymbol 1 \ C\_a)\)构成满秩\(K \times K\)方阵，
这个条件也经常是充分条件。

原来的矩阵形式模型为
\[
\boldsymbol{Y}
= (\boldsymbol 1 \ \; X\_a) \begin{pmatrix} \mu \\ \boldsymbol{\alpha} \end{pmatrix} + \boldsymbol{e},
\]
其中\(\boldsymbol{\alpha} = (\alpha\_1, \dots, \alpha\_K)^T\)。
改成\(X^\*\)后，
除了\(\mu\)以外关于因子\(a\)就只能有\(K-1\)个参数\(\alpha^\*\)，
矩阵形式的模型变成
\[
\boldsymbol{Y}
= (\boldsymbol 1 \ \; X\_a C\_a) \begin{pmatrix} \mu \\ \boldsymbol{\alpha}^\* \end{pmatrix} + \boldsymbol{e},
\]
所以要求
\[
\boldsymbol{\alpha} = C\_a \boldsymbol{\alpha}^\* .
\]

实际上，模型[(34.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-base-reg-fdm.html#eq:stat-mreg-fdm-dm-ctr01)是不可辨识的，
为了模型可估，
必须对参数添加某些约束。
如果存在非零\(\boldsymbol c\_a\)向量使得\(\boldsymbol c\_a^T C\_a = \boldsymbol 0\)，
则对模型参数\(\boldsymbol{\alpha}\)添加如下约束：
\[
\boldsymbol c\_a^T \boldsymbol{\alpha}
= \boldsymbol c\_a^T C\_a \boldsymbol{\alpha}
= 0,
\]
于是模型[(34.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-base-reg-fdm.html#eq:stat-mreg-fdm-dm-ctr01)在添加上式的约束后变得可辨识，
同时因为\(C\_a\)列满秩且\(C\_a \boldsymbol{\alpha} = \boldsymbol{\alpha}^\*\)，
所以有
\[
C\_a^T C\_a \boldsymbol{\alpha}^\* = C\_a^T \boldsymbol{\alpha},
\ \boldsymbol{\alpha}^\* = (C\_a^T C\_a)^{-1} C\_a^T \boldsymbol{\alpha},
\]
即原始的参数\(\boldsymbol{\alpha}\)可以与新的参数\(\boldsymbol{\alpha}^\*\)互相唯一表示。
记\(C\_a^+ = (C\_a^T C\_a)^{-1} C\_a^T\)，则
\[
\boldsymbol{\alpha} = C\_a \boldsymbol{\alpha}^\*,
\quad
\boldsymbol{\alpha}^\* = C\_a^+ \boldsymbol{\alpha} .
\]
其中\(C\_a\)是\(K \times (K-1)\)矩阵，
\(C^+\)是\((K-1) \times K\)矩阵，
矩阵\(C\_a\)称为因子\(a\)的对照矩阵。

R中提供的对照函数有`contr.treatment()`, `contr.sum()`,
`contr.helmert()`, `contr.poly()`, `contr.SAS()`。
除了`contr.treatment()`以外，
R中的对照矩阵一般都以\(\boldsymbol 1^T C\_a = \boldsymbol 0\)作为约束。

`options()$contrasts`可以访问因子和有序因子的默认使用的对照函数，
对因子默认使用`contr.treatment()`,
对有序因子默认使用`contr.poly()`：

```
options("contrasts")
```

```
## $contrasts
##         unordered           ordered 
## "contr.treatment"      "contr.poly"
```

对照函数默认生成\(K \times (K-1)\)矩阵。
其中\(K\)是因子的水平个数。

以3个水平的因子为例，
在有公共截距项的情况下3个水平的因子需要用两个哑变量编码：

```
contr.treatment(3)
```

```
##   2 3
## 1 0 0
## 2 1 0
## 3 0 1
```

这个矩阵表明因子水平1用\((0,0)\)表示，
水平2用\((1,0)\)编码，
水平3用\((0,1)\)编码。
对\(\boldsymbol\alpha\)的约束相当于\((1,0,0) \boldsymbol\alpha = \alpha\_1 = 0\)，
显然\((1,0,0) C\_a = (0,0)\),
\(C\_a\)表示上面的对照矩阵。

在这种参数设置下，
设截距项为\(\beta\_0\)，
两个哑变量的系数为\(\beta\_1\), \(\beta\_2\)，
则水平1的均值为\(\beta\_0\)，
水平2的均值为\(\beta\_0 + \beta\_1\)，
水平3的均值为\(\beta\_0 + \beta\_2\)。
检验\(H\_0: \beta\_1 = 0\)可以检验水平2与水平1是否有显著差异，
检验\(H\_0: \beta\_2 = 0\)可以检验水平3与水平1是否有显著差异，
没有直接检验水平2与水平3有无显著差异的方法。

可以用如下程序给出用旧参数\(\boldsymbol{\alpha}\)表示新参数\(\boldsymbol{\alpha}^\* = C\_a^+ \boldsymbol{\alpha}\)的变换矩阵\(C\_a^+\)：

```
contr.treatment(3) |> MASS::ginv() |> MASS::fractions()
```

```
##      [,1] [,2] [,3]
## [1,] 0    1    0   
## [2,] 0    0    1
```

这说明\(\beta\_1 = \alpha\_2\), \(\beta\_2 = \alpha\_3\)，
约束为\(\alpha\_1 = 0\)。

`contr.treatment()`默认与第一个水平比较，
可以加`base=`选项指定与另外一个水平比较：

```
contr.treatment(3, base=2)
```

```
##   1 3
## 1 1 0
## 2 0 0
## 3 0 1
```

这是水平1和水平3分别与水平2比较。

`contr.SAS()`是`contr.treatment()`设定最后一个水平为基线的变种。

`contr.sum()`函数生成的对照：

```
contr.sum(3)
```

```
##   [,1] [,2]
## 1    1    0
## 2    0    1
## 3   -1   -1
```

这个对照矩阵对应的参数约束向量为\(\boldsymbol c\_a = \boldsymbol 1\)，
即要求\(\alpha\_1 + \alpha\_2 + \alpha\_3 = 0\)，
这也是方差分析模型常用的约束。
设截距项为\(\beta\_0\)，
两个哑变量的系数为\(\beta\_1\), \(\beta\_2\)，
哑变量系数分别代表水平1和水平2与模型总平均值的比较。
事实上，
对水平1，
因变量均值为\(\beta\_0 + \beta\_1\),
对水平2，
因变量均值为\(\beta\_0 + \beta\_2\),
对水平3，
因变量均值为\(\beta\_0 - (\beta\_1 + \beta\_2)\),
这时\(\beta\_1\), \(\beta\_2\), \(-\beta\_1 - \beta\_2\)恰好相当于单因素方差分析模型中三个水平的主效应，
\(\beta\_0\)相当于总平均值。

新参数用老参数表示的矩阵\(C^+\)为：

```
contr.sum(3) |> MASS::ginv() |> MASS::fractions()
```

```
##      [,1] [,2] [,3]
## [1,]  2/3 -1/3 -1/3
## [2,] -1/3  2/3 -1/3
```

这说明\(\beta\_1 = (2\alpha\_1 - (\alpha\_2 + \alpha\_3))/3 = \alpha\_1 - \frac{1}{3}(\alpha\_1 + \alpha\_2 + \alpha\_3)\),
\(\beta\_2 = \alpha\_2 - \frac{1}{3}(\alpha\_1 + \alpha\_2 + \alpha\_3)\)，
分别表示前两个水平与三个水平均值的平均值的比较，
仅在均衡设计时个水平的平均值\(\frac{1}{3}(\alpha\_1 + \alpha\_2 + \alpha\_3)\)才有意义。

`contr.helmert()`将水平2与水平1比较，
将水平3与前2个水平的平均值比较，
以此类推。
参数约束向量仍为\(\boldsymbol c\_a = \boldsymbol 1\)。

```
contr.helmert(3)
```

```
##   [,1] [,2]
## 1   -1   -1
## 2    1   -1
## 3    0    2
```

新参数用旧参数表示的矩阵\(C^+\)为：

```
contr.helmert(3) |> MASS::ginv() |> MASS::fractions()
```

```
##      [,1] [,2] [,3]
## [1,] -1/2  1/2    0
## [2,] -1/6 -1/6  1/3
```

这里\(\beta\_1 = \alpha\_2 - \alpha\_1\)是水平2与水平1的比较，
\(\beta\_2 = \frac{1}{3}[\alpha\_3 - \frac{1}{2}(\alpha\_1 + \alpha\_2)]\)是水平3与前两个水平均值的比较。

`contr.poly()`利用正交多项式产生对照。

```
contr.poly(3)
```

```
##                 .L         .Q
## [1,] -7.071068e-01  0.4082483
## [2,] -7.850462e-17 -0.8164966
## [3,]  7.071068e-01  0.4082483
```

在某些特殊情况下可能需要用\(k\)个哑变量表示有\(k\)个水平的因子，
比如，
模型中没有截距项时。
在对照函数中设置选项`contrasts=FALSE`可以生成\(k \times k\)的对照矩阵。
如

```
contr.treatment(3, contrasts=FALSE)
```

```
##   1 2 3
## 1 1 0 0
## 2 0 1 0
## 3 0 0 1
```

如果有两个因子\(a, b\)，
分别有\(K, J\)个水平，
没有交互作用时，
只要将每个因子利用对照矩阵将其原始设计阵\(X\_a\), \(X\_b\)转换成\(n \times (K-1)\)矩阵\(X\_a C\_a\)和\(n \times (J-1)\)矩阵\(X\_b C\_b\)。
如果有交互作用效应\(a:b\)，
则只要将\(X\_a C\_a\)的\(K-1\)列与\(X\_b C\_b\)的\(J-1\)列两两进行对应元素乘法，
得到\((K-1) \times (J-1)\)列，
这些列就作为设计阵中对应于两个因子的交互作用效应的部分。

如果一个因子\(a\)和一个连续变量\(x\)作交互作用\(a:x\)，
只要将因子\(a\)产生的\(K-1\)列\(X\_a C\_a\)的每一列与\(x\)产生的一列作对应元素乘法，
得到新的\(K-1\)列，
就可以作为\(a\)和\(x\)的交互作用在设计阵中的部分。
这相当于在因子\(a\)的不同水平下连续变量\(x\)有不同的斜率项。

两个连续变量\(x\), \(z\)的交互作用\(x:z\)就是将两列的对应元素相乘产生一列，
相当于增加了一个新变量\(v = x z\)。

可以用如下方法对某个因子附加非默认的对照函数：

```
lesion.f <- factor(armd.wide$lesion)
contrasts(lesion.f) <- contr.sum(4)
```

提取因子的对照矩阵：

```
contrasts(lesion.f)
```

```
##   [,1] [,2] [,3]
## 1    1    0    0
## 2    0    1    0
## 3    0    0    1
## 4   -1   -1   -1
```

如果模型是均衡设计的方差分析，
则使用默认的`contr.treatment()`或`contr.poly()`可以使设计阵各列正交，
这样各回归系数的估计值不相关。

## 34.4 R信息提取函数

前面演示过可以用`summary()`从回归结果中显示参数估计、检验等信息，
用`anova()`比较嵌套的模型，
用`AIC()`比较模型。
这些函数称为信息提取函数，
这里对信息提取函数进行简单列举，
设`lmr`为`lm()`的输出结果，
`summ`为`summary()`的结果。

关于参数估计：

* `summary(lmr)`：结果包括参数估计、标准误差、t检验、F检验、复相关系数平方等；
* `coef(lmr)`: 提取回归系数，返回一个向量；
* `coef(summ)`: 返回包括参数估计、标准误差、t统计量、p值的矩阵；
* `vcov(lmr)`: 返回系数估计向量的协方差阵估计；
* `confint(lmr)`：返回系数估计向量的置信区间，默认置信度为95%；
* `summ$sigma`：返回误差项标准差估计\(\hat\sigma\)。

如果使用了最大似然估计(ML)或者限制最大似然估计(REML)，
可以提取：

* `logLik(lmr)`：最大似然估计对应的似然函数值；
* `logLik(lmr, REML=TRUE)`：限制最大似然估计对应的限制似然函数值；
* `AIC(lmr)`, `BIC(lmr)`：AIC或者BIC值。

关于拟合值与残差：

* `fitted(lmr)`或`predict(lmr)`: 拟合值；
* `residuals(lmr)`：原始的残差；
* `rstandard(lmr)`：内部学生化残差；
* `rstudent(lmr)`: 外部学生化残差；
* `predict(lmr, interval="confidence")`：拟合值及均值的置信区间；
* `predict(lmr, interval="prediction")`：拟合值及观测值的置信区间。