---
crawl_time: '2026-01-17 14:19:54'
framework: rbook
title: 1 R语言介绍 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/intro.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 1 R语言介绍

## 1.1 R的历史和特点

### 1.1.1 R的历史

R语言来自S语言，是S语言的一个变种。S语言由Rick Becker, John Chambers, Alan Wilks等人在贝尔实验室开发，
著名的C语言、Unix系统也是贝尔实验室开发的。

S语言第一个版本开发于1976-1980，基于Fortran；
于1980年移植到Unix, 并对外发布源代码。
1984年出版的“棕皮书” ([R. A. Becker and Chambers 1984](#ref-Becker1984:SLang))
总结了1984年为止的版本, 并开始发布授权的源代码。
这个版本叫做旧S，与我们现在用的S语言(R语言)有较大差别，
比如，
不支持自定义函数。

1984–1988对S进行了较大更新，
变成了我们现在使用的S语言，称为第二版。
1988年出版的“蓝皮书” ([Richard A. Becker, Chambers, and Wilks 1988](#ref-Becker1988:NewSLang)) 做了总结。

1992年出版的“白皮书” ([J. M. Chambers and Hastie 1992](#ref-Chambers1992:SMS))
描述了在S语言中实现的统计建模功能，
增强了面向对象的特性。软件称为第三版，这是我们现在用的多数版本。

1998年出版的“绿皮书” ([John M. Chambers 1998](#ref-Chambers1998))
描述了第四版S语言，主要是编程功能的深层次改进。
现行的S系统并没有都采用第四版，S-PLUS的第5版才采用了S语言第四版。

S语言商业版本为S-PLUS, 1988年发布，现在为Tibco Software拥有。
命运多舛，多次易主。现在已经基本被R语言取代。

R是一个自由源代码软件，GPL授权，
最初由新西兰Auckland大学的Ross Ihaka和Robert Gentleman于1997年发布，
R实现了与S语言基本相同的功能和统计功能。
现在由R核心团队开发，但全世界的用户都可以贡献软件包。
R的网站:

* <http://www.r-project.org/>

### 1.1.2 R的特点

#### 1.1.2.1 R语言一般特点

* 自由软件，免费、开放源代码，支持各个主要计算机系统；
* 完整的程序设计语言，基于函数和对象，可以自定义函数，调入C、C++、Fortran编译的代码；
* 具有完善的数据类型，如向量、矩阵、因子、数据集、一般对象等，支持缺失值，代码像伪代码一样简洁、可读;
* 强调交互式数据分析，支持复杂算法描述，图形功能强;
* 实现了经典的、现代的统计方法，如参数和非参数假设检验、线性回归、广义线性回归、非线性回归、可加模型、树回归、混合模型、方差分析、判别、聚类、时间序列分析等。
* 统计科研工作者广泛使用R进行计算和发表算法。R有上万扩展包(截止2023年4月在R扩展包主要分发网站CRAN上有一万九千多个)。

#### 1.1.2.2 R语言和R软件的技术特点

S语言作者，R语言专家J. M. Chambers(([J. M. Chambers 2016](#ref-Chambers2016:ExtendR)))指出R的本质特征：

* R中所有的存在都是对象；
* R中发生的动作都是函数调用。

详细地说R有如下技术特点：

* 函数编程（functional programming）。R语言虽然不是严格的functional programming语言，但可以遵照其原则编程，得到可验证的可靠程序。
* 支持对象类和类方法。基于对象的程序设计。
* 是动态类型语言，解释执行，运行速度较慢。
* 数据框是基本的观测数据类型，类似于数据库的表。
* 开源软件（Open source software）。可深入探查，开发者和用户交互。
* 可以用作C和C++、FORTRAN语言编写的算法库的接口。
* 主要数值算法采用已广泛测试和采纳的算法实现，如排序、随机数生成、线性代数（LAPACK软件包）。

## 1.2 参考资料

### 1.2.1 推荐参考书

* Hadley Wickham and Garrett Grolemund(2022).
  R for Data Science，<https://r4ds.hadley.nz/>, 2nd ed.
  讲基本的数据整理、汇总。
* Hadley Wickham(2019).
  Advanced R, 2nd ed.,
  <https://adv-r.hadley.nz/>,
  Chapman & Hall/CRC The R Series.
  高级R编程，属于对R高级编程技术的讲解。
* Hadley Wickham(2016).
  ggplot2 Elegant Graphics for Data Analysis,
  2nd ed.,
  <https://ggplot2-book.org/>,
  Springer.
  优雅易用的R作图功能。
* Susan Holmes, Wolfgang Huber(2020).
  Modern Statistics for Modern Biology,
  <https://www.huber.embl.de/msmb/index.html>.  
  R的统计功能在生物学中的应用。

### 1.2.2 其它参考书

* R网站上的初学者手册“An Introduction to R”和其它技术手册。
* John M. Chambers(2008),
  Software for Data Analysis-Programming with R,
  Springer.
* Venables, W. N. & Ripley, B. D.(2002).
  Modern Applied Statistics with S (MASS),
  Springer.
* Max Kuhn and Julia Silge(2022).
  Tidy Modeling with R.
  <https://www.tmwr.org/>
* Chester Ismay and Albert Y. Kim(2019).
  Statistical Inference via Data Science:
  A moderndive into R and the tidyverse.
  CRC Press.
  <https://moderndive.com/index.html>
  免费的入门统计学教材，用R和tidyverse。
* David Diez, Mine Cetinkaya-Rundel, and Christopher Barr(2019).
  OpenIntro Statistics, 4th Ed.
  <https://www.openintro.org/index.php>
  免费统计学教材。
* Kieran Healy.
  Data Visualization: A practical introduction.
  <https://socviz.co/index.html>
  数据可视化。
* Claus O. Wilke.
  Fundamentals of Data Visualization.
  <https://serialmentor.com/dataviz/>
  数据可视化。
* 赵鹏，谢益辉，黄湘云(2021).
  现代统计图形.
  人民邮电出版社。
  讲R的各种各样的图形。
* Winston Chang(2019).
  R Graphics Cookbook, 2nd edition.
  <https://r-graphics.org/>
  各种R图形的代码集锦。
* Jenny Bryan.
  Happy GIT with R.
  <https://happygitwithr.com/>
  GIT是软件版本管理和协作工具软件。
* Jenny Bryan.
  Stat 545.
  <https://stat545.com/>
  讲R, Github, 数据管理，作图, make.
* Trevor Hastie, Jerome Friedman, Robert Tibshirani(2009).
  The Elements of Statistical Learning.
  2nd Ed. Springer.
  统计学习引论第二版。
* Paul Roback and Julie Legler(2021).
  Beyond Multiple Linear Regression:
  Applied Generalized Linear Models and Multilevel Models in R.
  <https://bookdown.org/roback/bookdown-BeyondMLR/>
  讲广义线性模型、相关误差、多层次模型。
* R.L. Kabacoff(2012)《R语言实战》，人民邮电出版社。
* 薛毅、陈立萍（2007）《统计建模与R软件》，清华大学出版社。
* 汤银才（2008），《R语言与统计分析》，高等教育出版社。
* 李东风（2006）《统计软件教程》，人民邮电出版社。

## 1.3 R的下载与安装

### 1.3.1 R的下载

以MS Windows操作系统为例。R的主网站在<https://www.r-project.org/>。
从CRAN的镜像网站下载软件，其中一个镜像如<http://mirror.bjtu.edu.cn/cran/>。
选“Download R for Windows-base-Download R 4.2.2 for windows”
(4.2.2是版本号，应下载网站上给出的最新版本）链接进行下载。
在“Download R for Windows”链接的页面，
除了base为R的安装程序，
还有contrib为R附加的扩展软件包下载链接（一般不需要从这里下载），
以及Rtools链接，
是在R中调用C、C++和Fortran程序代码时需要用的编译工具。

RStudio（<https://www.rstudio.com/>）是功能更强的一个R图形界面，
在安装好R的官方版本后安装RStudio可以更方便地使用R。

### 1.3.2 R软件安装

下载官方的R软件后按提示安装。
安装后获得一个桌面快捷方式，如“R x64 4.2.1”。

安装官方的R软件后，
可以安装RStudio。
平时使用可以使用RStudio，
其界面更方便，
对Qmd文件支持更好。

如果使用RStudio，
每个分析项目需要单独建立一个“项目”（project），
每个项目也有一个工作文件夹。

### 1.3.3 辅助软件

R可以把一段程序写在一个以.r或.R为扩展名的文本文件中，
如“date.r”, 称为一个\_源程序\_文件，
然后在R命令行用

```
source("date.r")
```

运行源程序。
这样的文件可以用记事本生成和编辑。

如果不使用RStudio，
在MS Windows操作系统中建议使用notepad++软件，
这是MS Windows下记事本程序的增强型软件。
安装后，
在MS Windows资源管理器中右键弹出菜单会有“edit with notepadpp”选项。
notepad++可以方便地在不同的中文编码之间转换。

RStudio则是一个集成环境，
可以在RStudio内进行源程序文件编辑和运行。

## 1.4 R扩展软件包的安装与管理

### 1.4.1 扩展包使用

R有一万多个扩展软件包，提供了各种各样的功能。
在安装基本R软件时，
已经伴随安装了一些必要的扩展包，
如base, stats, graphics等，
这些包在启动R时会默认载入，
不需要用户干预。

其它扩展包在安装好以后如果需要调用，
一般需要先用`library()`函数载入运行。
例如，载入`readr`扩展包，并使用其中的`read_csv()`函数读入一个CSV文件：

```
library(readr)
d <- read_csv("class.csv")
```

有些扩展包允许不载入而直接用“扩展包名::函数名()”的格式调用其中的功能，如：

```
d <- readr::read_csv("class.csv")
```

因为不同的扩展包可能会使用相同的函数名称，
所以在两个扩展包中的函数名字有冲突时，
也应该使用“扩展包名::函数名()”这种语法。
例如，stats包和dplyr包都提供了名为`filter()`的函数，
在调用此函数时，
就应该指定为`stats::filter()`和`dplyr::filter()`。

在使用`library()`函数载入扩展包时，
会显示一些诊断信息、提示信息、警告信息。
在交互运行时这些信息是必要的，
但是在使用Quarto或者R Markdown格式制作论文、技术报告、图书时，
这些信息则会分散注意力，
显得不专业。
在R Markdown文件中，
可以设置代码段选项`results="hide"`,
`warning=FALSE`, `message=FALSE`。
如：

```
```{r, results="hide", warning=FALSE, message=FALSE}
library(tidyverse)
```
```

在Quarto文件中，
只要设置代码段选项`output: false`。
如：

```
```{r}
#| output: false
library(tidyverse)
```
```

还有一种办法是用如下的比较复杂的调用方式：

```
library(tidyverse, 
  warn.conflicts=FALSE,
  quietly=TRUE,
  verbose=FALSE) |> 
  suppressMessages() |> 
  suppressWarnings()
```

### 1.4.2 安装

以安装sos包为例。sos包用来搜索某些函数的帮助文档。
在RStudio中调用用“Tools”菜单的“Install Packages”，
输入或者在列表中选中sos就可以安装该扩展包。

如果不用RStudio，
在R图形界面选菜单“程序包-安装程序包”，
在弹出的“CRAN mirror”选择窗口中选择一个中国的镜像如“China (Beijing 2)”，
然后在弹出的“Packages”选择窗口中选择要安装的扩展软件包名称，
即可完成下载和安装。

还可以用如下程序指定镜像网站(例子中是位于清华大学的镜像网站)并安装指定的扩展包：

```
options(repos=c(CRAN="https://mirror.tuna.tsinghua.edu.cn/CRAN/"))
install.packages("sos")
```

还可以选择扩展包的安装路径，
如果权限允许，
可以选择安装在R软件的主目录内或者用户自己的私有目录位置。
由于用户的对子目录的读写权限问题，
有时不允许一般用户安装扩展包到R的主目录中。
用`.libPaths()`查看允许的扩展包安装位置，
在`install.packages()`中用`lib=`指定安装位置：

```
print(.libPaths())
## [1] "D:/R/R-4.1.0/library"
install.packages("sos", lib=.libPaths()[1])
```

### 1.4.3 Github和BioConductor的扩展包

还有一些扩展包没有在CRAN系统提供，
而是放在了Github网站。
对于这样的包，
安装方法举例如下：

```
if(!require(devtools)) install.packages('devtools')
devtools::install_github("kjhealy/socviz")
```

其中`kjhealy`是Github网站的某个作者的名称，
`socviz`是该作者名下的一个R扩展包。

还有一些包需要从Bioconductor网站安装，可利用镜像网站。
利用清华大学镜像的示例如下：

```
options(repos=c(CRAN="https://mirror.tuna.tsinghua.edu.cn/CRAN/"))
if (!requireNamespace("BiocManager", quietly = TRUE)){
  install.packages("BiocManager")
  BiocManager::install()
}
options(BioC_mirror="https://mirrors.tuna.tsinghua.edu.cn/bioconductor")
BiocManager::install(c("Biostrings"))
```

随后安装Bioconductor的其它包，
也应该先设置`repos`和`Bioc_mirror`选项。

### 1.4.4 更新扩展包

在RStudio中用“Tools–Check for Package Updates”菜单，
可以显示有新版本的扩展包，
并选择进行更新。

或者在命令行用如下命令更新本地安装的所有有新版本的CRAN扩展包：

```
options(repos=c(CRAN="http://mirror.tuna.tsinghua.edu.cn/CRAN/"))
update.packages(checkBuilt=TRUE, ask=FALSE)
```

RStudio在运行时会载入某些包，
如rlang，
这使得RStudio无法更新这些包，
需要在R的命令行程序（就是基本R软件）中更新。

### 1.4.5 迁移扩展包

在每一次R软件更新后，
需要重新安装原来的软件包，
这个过程很麻烦。
如果仅仅是小的版本更新，
比如从3.5.1变成3.5.2，
或者从3.4.2变成3.5.0，
可以在安装新版本后，
临时将新版本的library子目录更名为library0，
将老版本的library子目录剪切为新版本的library子目录，
然后将library0中所有内容复制并覆盖进入library子目录，
删除library0即可。
然后在基本R中（不要用RStudio）运行如下命令以更新有新版本的包：

```
options(repos=c(CRAN="http://mirror.tuna.tsinghua.edu.cn/CRAN/"))
update.packages(checkBuilt=TRUE, ask=FALSE)
```

如果版本改变比较大，
可以用如下方法批量地重新安装原有的软件包。
首先，在更新R软件前，在原来的R中运行：

```
packages <- .packages(TRUE)
dump("packages", file="packages-20180704.R")
```

这样可以获得要安装的软件包的列表。
在更新R软件后，
运行如下程序：

```
options(repos=c(CRAN="http://mirror.tuna.tsinghua.edu.cn/CRAN/"))
source("packages-20180704.R")
install.packages(packages)
```

安装时如果提问是否安装需要编译的源代码包，
最好选择否，
因为安装源代码包速度很慢还有可能失败。

### 1.4.6 项目私有扩展包目录

在使用了R一段较长时间以后，
会安装了许多扩展包，
这些扩展包在某个时期是有用的，
但是一旦某个任务完成了就不再有用。
但是，
用户自己无法判断哪些包已经不需要。

R的renv扩展包支持每个项目保存私有的扩展包目录，
这样，
不同的项目使用不同的扩展包集合，
不至于引发版本冲突，
也不必总是为公用的R扩展包目录增加许多仅是短暂使用的扩展包。
用renv管理项目私有扩展包目录，
也有利于将项目迁移到其它电脑中，
并且保证每次使用的扩展包版本号不变，
避免多个项目共用的扩展包升级造成兼容性问题。

那些不需要安装许多扩展包的项目仍可以不启用renv，
使用公用的R扩展包目录。

在生成新的RStudio项目时，
可以点击选中“Use renv with this project”复选框；
对已有的RStdio项目，
如果要启用renv，
可以选菜单“Tools – Project Options – Environment”，
选中“Use renv with this project”复选框。

启用了renv的项目，
在安装新的扩展包时，
将安装在项目目录中，
而不再修改R的公用的扩展包目录。
这也有助于将项目迁移到其它计算机上。

## 1.5 基本R软件的用法

### 1.5.1 基本运行

在MS Windows操作系统中的R软件有一个R GUI软件，
即图形窗口模式的R软件，如图[1.1](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/intro.html#fig:intro-ruse-rgui01)。

![RGUI截图](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/rgui01.png)

图1.1: RGUI截图

R GUI中有一个命令行窗口(R Console)，
以大于号为提示符，
在提示符后面键入命令，
命令的文字型结果马上显示在命令下方，
命令的图形结果单独显示在一个图形窗口中。

在命令行可以通过左右光标键移动光标到适当位置进行修改。
可以用上下光标在已经运行过的历史命令中切换，
调回已经运行过的命令，
修改后重新执行。

如果某个文件如`myprog.R`在当前工作目录中，
保存的都是R程序，
称这样的文件为**源程序**文件。
可以在命令行用如下命令运行其中的程序：

```
source("myprog.R")
```

但是，
在MS Windows操作系统中，
默认的中文编码是GB18030编码。
R源程序文件的中文编码可能是GB18030也可能是UTF-8。
UTF-8是在世界范围更通用的编码。
如果发现用上述命令运行时出现中文乱码，
可能是因为源程序用了UTF-8编码，
这时`source()`命令要加上编码选项如下：

```
source("myprog.R", encoding="UTF-8")
```

### 1.5.2 项目目录

用R进行数据分析，
不同的分析问题需要放在不同的文件夹中。
以MS Windows操作系统为例，
设某个分析问题的数据与程序放在了`c:\work` 文件夹中。
把R的快捷方式从桌面复制入此文件夹，
在Windows资源管理器中，
右键单击此快捷方式，在弹出菜单中选“属性”，
把“快捷方式”页面的“起始位置”的内容清除为空白，
把“目标”的内容中的`--cd-to-userdocs`删去，
点击确定按钮。
启动在work文件夹中的R快捷方式，出现命令行界面。
这时，`C:\work`称为**当前工作目录**。

在命令行运行如下命令可以显示当前工作目录位置：

```
getwd()
## "C:/work"
```

显示结果中的目录、子目录、文件之间的分隔符用了`/`符号，
在Windows操作系统中一般应该使用`\\`符号，
但是，
在R的字符串中一个`\`需要写成两个，
所以等价的写法是`"C:\\work"`。

不同的分析项目需要存放在不同的文件夹中，
每个文件夹都放置一个“起始位置”为空的R快捷方式图标，
分析哪一个项目，
就从对应的快捷图标启动，而不是从桌面上的R图标启动。
这样做的好处是，
用到源文件和数据文件时，
只要文件在该项目的文件夹中，
就不需要写完全路径而只需要用文件名即可。

## 1.6 RStudio软件

### 1.6.1 介绍

RStudio软件是R软件的应用界面与增强系统，
可以在其中编辑、运行R的程序文件，
可以跟踪运行，
还可以构造文字、R结果图表融合在一起的研究报告、论文、图书、网站等。
一个运行中的RStudio界面见图[1.2](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/intro.html#fig:intro-rstudio-gui01)。

![RStudio截图](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/rstudio01.png)

图1.2: RStudio截图

界面一般分为四个窗格，
其中编辑窗口与控制台（Console）是最重要的两个窗格。
编辑窗格用来查看和编辑程序、文本型的数据文件、程序与文字融合在一起的Rmd文件等。
控制台与基本R软件的命令行窗口基本相同，
功能有所增强。

在编辑窗口中可以用操作系统中常用的编辑方法对源文件进行编辑，
如复制、粘贴、查找、替换，
还支持基于正则表达式的查找替换（关于正则表达式见[48](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/text.html#text)）。

其它的一些重要窗格包括：

* Files: 列出当前项目的目录（文件夹）内容。
  其中以`.R`或者`.r`为扩展名的是R源程序文件，
  单击某一源程序文件就可以在编辑窗格中打开该文件。
* Plots: 如果程序中有绘图结果，
  将会显示在这个窗格。
  因为绘图需要足够的空间，
  所以当屏幕分辨率过低或者Plots窗格太小的时候，
  可以点击“Zoom”图标将图形显示在一个单独的窗口中，
  或者将图形窗口作为唯一窗格显示。
  如何放大窗格见下面的使用技巧。
* Help: R软件的文档与RStudio的文档都在这里。
* Environment: 已经有定义的变量、函数都显示在这里。
* History: 以前运行过的命令都显示在这里。
  不限于本次RStdudio运行期间，
  也包括以前使用RStudio时运行过的命令。
* Packages: 显示已安装的R扩展包及其文档。
* Viewer, Connection, Build, Git等窗格。

### 1.6.2 项目

用R和RStudio进行研究和数据分析，
每个研究问题应该单独建立一个文件夹（目录）。
该问题的所有数据、程序都放在对应的文件夹中。
在RStudio中，
用“File – New Project – Existing Directory”选中该问题的目录，
建立一个新的“项目”（project）。

再次进入RStudio后，
用菜单“File – Recent Projects”找到已有的项目打开，
然后就可以针对该项目进行分析了。
这样分项目进行研究的好处是，
不同项目的可以使用同名的文件而不会有冲突，
程序中用到某个文件时，
只需要写文件名而不需要写文件所在的目录。

一个项目还可以有项目本身的一些特殊设置，
用“Tools – Project Options”菜单打开设置。

### 1.6.3 帮助

在RStudio中有一个单独的Help窗格，
如果需要，可以用菜单“View–Panes–Zoom help”将其放大到占据整个窗口空间。
但是，这一功能目前不支持放大显示字体的功能，
不如在浏览器中方便。

RStudio的帮助窗格中包含R软件的官方文档，
以及RStudio软件的的文档。
“Search engine and keywords”项下面有分类的帮助。
有软件包列表。

在基本R软件而不是RStudio的命令行中运行命令`help.start()`或者用RGUI的帮助菜单中“html帮助”可以打开系统默认的互联网浏览器，
在其中查看帮助文档。

在命令行，用问号后面跟随函数名查询某函数的帮助。
用`example(`函数名`)`的格式可以运行此函数的样例，如:

```
?mean
example(mean)
```

有时仅知道一些方法的名字而不知道具体的扩展包和函数名称，
可以安装sos扩展包（package），
用`findFn("函数名")`查询某个函数，
结果显示在互联网浏览器软件中。

### 1.6.4 使用技巧

RStudio使用方法概要PDF下载：

<rstudio-ide.pdf>。

#### 1.6.4.1 使用历史

在控制台（命令行窗格）中，
除了可以用左右光标键移动光标位置，
用上下光标键调回以前运行过的命令，
还有一个重要的增强（以MS Windows操作系统为例）：
键入要运行的命令的前几个字母，如`book`，
按“Ctrl+向上光标键”，
就可以显示历史命令中以`book`开头的所有命令，
单击哪一个，
哪一个就自动复制到命令行。
这一技巧十分重要，
我们需要反复运行同一命令或类似命令时，
这一方法让我们很容易从许多命令历史中找到所需的命令。

#### 1.6.4.2 放大显示某一窗格

当屏幕分辨率较低时，
将整个RStudio界面分为四个窗格会使得每个窗格都没有足够的显示精度。
为此，
可以将某个窗格放大到整个窗口区域，
需要使用其它窗格时再恢复到四个窗格的状态或者直接放大其它窗格到整个窗口区域。

使用菜单“View – Panes – Zoom Source”可以将编辑窗格放到最大，
在MS Windows下也可以使用快捷键“Ctrl+Alt+1”。
其它操作系统也有类似的快捷键可用。
使用菜单“View – Panes – Show All Panes”可以显示所有四个窗格。

放大其它窗格也可以用“Ctrl + Alt + 数字”，数字与窗格的对应关系为：

* 1: 编辑窗格；
* 2: 控制台（Console）；
* 3: 帮助；
* 4: 历史；
* 5: 文件；
* 6: 图形；
* 7: 扩展包；
* 8: 已定义变量和函数；
* 9: 研究报告或网站结果显示。

#### 1.6.4.3 运行程序

可以在命令行直接输入命令运行，
文字结果会显示在命令行窗口，
图形结果显示在“Plots”窗格中。
在命令行窗口（Console）中可以用左右光标键移动光标，
用上下光标键查找历史命令，
输入命令的前几个字母后用“Ctrl+向上光标键”可以匹配地查找历史命令。

一般情况下，
还是应该将R源程序保存在一个源程序文件中运行。
RStudio中“File – New File – R Script”可以打开一个新的无名的R源程序文件窗口供输入R源程序用。
输入一些程序后，保存文件，
然后点击“Source”快捷图标就可以运行整个文件中的所有源程序，
并会自动加上关于编码的选项。

编写R程序的正常做法是一边写一边试验运行，
运行一般不是整体的运行而是写完一部分就运行一部分，
运行没有错误才继续编写下一部分。
在R源程序窗口中，
当插入光标在某行程序上的时候，
点击窗口的“Run”快捷图标或者用快捷键“Ctrl+Enter键”可以运行该行；
选中若干程序行后，
点击窗口的“Run”快捷图标或者用快捷键“Ctrl+Enter键”可以运行这些行。

#### 1.6.4.4 注释和源文件框架

R源文件中用井号（`#`）开头的行，
以及行内从井号开始的部分为注释。

对于较长的源文件，
应适当分节，
在RStudio软件中，
可以用特殊的注释进行分节，
并可点击源程序编辑窗格右上角的“Outline”快捷图标显示源程序分节结构。

这种特殊的注释格式如：

```
# 1. 辅助函数 ####
## 1.1 函数A ####
## 1.2 函数B ####
### 1.2.1 说明
# 2. 输入数据
# 3. 建模
```

特殊注释的末尾有4个以上的井号，
开头用1到多个井号表示框架分级，
开头井号越少，
级别越高。

目前这样的特殊注释对中文的识别能力不好，
所以需要用编号数字帮助正确识别结构层级。

#### 1.6.4.5 中文编码问题

对于中文内容的R源程序、R Markdown源文件（.Rmd文件）、文本型数据文件(.txt，.csv)，
其中的中文内容可能有不同的编码选择，
在中国除港澳台以外的地区主要使用GB18030(基本兼容于GB, GBK)和UTF-8，
UTF-8是国际上更普遍使用的统一文字编码，
涉及到计算机编程时应尽可能使用此编码系统。

在RStudio中新生成的R源程序、Rmd源文件一般自动用UTF-8编码。
点击RStudio的文件窗格中显示的源文件，
可以打开该源文件，
但是因为已有源文件的编码不一定与RStudio的默认编码一致，
可以会显示成乱码。
为此，
RStdio提供了“File – Reopen with Encoding”命令，
我们主要试验其中GB18030和UTF-8两种选择一般就可以解决问题。
如果选择GB18030显示就没有乱码了，
最好再用菜单“File – Save with Encoding”并选择UTF-8将其保存为UTF-8编码。

其它的文本格式的文件也可以类似地处理，
后面将会陆续提及。

## 1.7 Qmd文件

在科学研究中，
R软件可以用来分析数据，
生成数据分析报表和图形。
Quarto(简称Qmd)是一种特殊的文件格式，
在这种文件中，
既有R程序，
又有说明文字，
通过R和RStudio软件，
可以运行其中的程序，
并将说明文字、程序、程序的文字结果、图形结果统一地转换为一个研究报告，
支持Word、PDF、网页、网站、幻灯片等许多种输出格式。
在打开的Qmd源文件中，
也可以选择其中的某一段R程序单独运行。
所以，
Qmd文件也可以作为一种特殊的R源程序文件。

用RStudio的“File – New File – Quarto Document”菜单就可以生成一个新的Qmd文件并显示在编辑窗格中，
其中已经有了一些样例内容，
可以修改这些样例内容为自己的文字和程序。

Qmd文件中用```` ```{r} ````开头，
用```` ``` ````结尾的段落是R程序段，
在显示的程序段的右侧有一个向右箭头形状的小图标（类似于媒体播放图标），
点击该图标就可以运行该程序段。

打开Qmd文件后，
用编辑窗口的Render命令可以选择将文件整个地转换为HTML(网页)或者MS Word格式，
如果操作系统中安装有LaTeX软件，
还可以以LaTeX为中间格式转换为PDF文件。

为了将网页转换为PDF文件，
建议使用Chrome浏览器打开HTML文件，
然后选择菜单“打印”，
选打印机为“另存为PDF”，
然后选“更多设置”，
将其中的“缩放”改为自定义，
比例改为“90%”。

详见[20](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/knitr.html#knitr)、[21](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/markdown.html#markdown)、[22](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/quarto.html#quarto)。

## 1.8 练习

1. 下载R安装程序，安装R，建立work文件夹并在其中建立R的快捷方式。
   Windows用户还需要下载RTools软件并安装。
2. 下载RStudio软件并安装。
3. 在work文件夹中建立一个新的项目。
4. 下载安装notepad++软件(仅MS Windows用户)。
5. 在RStudio中下载安装sos扩展软件包。

### References

Becker, R. A., and J. M. Chambers. 1984. *S: An Interactive Environment for Data Analysis and Graphics*. Wadsworth Advanced Books Program, Belmont CA.

Becker, Richard A., John M. Chambers, and Allan Reeve Wilks. 1988. *The New S Language.* Chapman; Hall, New York.

Chambers, J. M. 2016. *Extending R*. Chapman; Hall/CRC, London.

Chambers, J. M., and T. Hastie. 1992. *Statistical Models in S*. Chapman; Hall, New York.

Chambers, John M. 1998. *Programming with Data - a Guide to the S Language*. Springer.