---
crawl_time: '2026-01-17 14:20:00'
framework: rbook
title: B 用bookdown制作图书 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/bookdown.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# B 用bookdown制作图书

## B.1 介绍

R的bookdown扩展包(<https://github.com/rstudio/bookdown>)
是继knitr和rmarkdown扩展包之后，
另一个增强markdown格式的扩展，
使得Rmd格式可以支持公式、定理、图表自动编号和引用、链接，
文献引用和链接等适用于编写书籍的功能。
在bookdown的管理下一本书的内容可以分解成多个Rmd文件，
其中可以有可执行的R代码，
R代码生成的文字结果、表格、图形可以自动插入到生成的内容中，
表格和图形可以是浮动排版的。
输出格式主要支持gitbook格式的网页图书，
这种图书在左侧显示目录，
右侧显示内容，
并可以自动链接到上一章和下一章；
通过单独安装的LaTeX编译器支持将书籍转换为一个PDF文件，
支持中文；
可以生成ePub等格式的电子书。

主要用于编写有多个章节的书籍，
也可以用来生成单一文件的研究报告。

建议使用RStudio集成环境制作这样的图书，
该软件内建了一键编译整本书的功能。
需要安装bookdown扩展包的最新版本。
bookdown扩展包现在还比较新，
还有一些BUG，
所以尽可能使用最新版的bookdown扩展包并且及时更新RStudio软件。
查看编译的网站建议使用Google Chrome浏览器，
此浏览器对gitbook的支持较好。

为了新写一本书或者从已有的书转换，
最简单的做法是从bookdown的网站下载bookdown配套的例书的zip文件
(见<https://github.com/rstudio/bookdown-demo>)，
将其解压到本地硬盘某个子目录，
然后修改其中的内容适应自己的书的需要。

因为中文需要一些特殊的设置，
以及在网络条件不好的条件下支持数学公式显示，
本书作者提供了一个粗浅的中文书bookdown模板，
下载链接为:

* [`bookdown-template-v0-6.zip`](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/bookdown-template-v0-6.zip)。

其中的CBook子目录包含了所需的中文书模板，
CArticle子目录包含了论文格式模板，
其它子目录有一些别的模板，
为了在本地支持网页中的数学公式显示还有一个MathJax目录。
参见其中的readme.txt说明文件。

## B.2 一本书的设置

一本用bookdown管理的书，
一般放置在某个子目录下，
并作为一个RStudio项目(project)用RStudio管理。

也可以自己新建一个目录，
然后编辑生成必要的文件。
注意，所有的文本文件都要使用UTF-8编码。
一本bookdown书，
一般都需要有一个`index.Rmd`文件，
这是最后生成的网站的主页的原始文件，
可以在这个文件中写一些书的说明，
并在开头的YAML元数据部分进行有关设置，
如标题、作者、日期等。
`index.Rmd`的一个例子如下：

```
--- 
title: "统计计算"
author: "李东风"
date: `r Sys.Date()`
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: book
bibliography: [myrefs.bib, ../docs/refs.bib]
biblio-style: apa
link-citations: true
description: "本科生《统计计算》教材。采用R的bookdown制作，输出格式为bookdown::gitbook."
---
```

```
# 前言 {-}

统计计算研究如何将统计学的问题用计算机正确、高效地实现。
```

其中在三个减号组成的两行之间的内容叫做YAML元数据，
是一本书的设置，
上例中有书的标题、作者名、日期（用R程序自动生成）、描述。
其中的`site`选项很重要，
一定要有这个选项，
`site: bookdown::bookdown_site`使得RStudio软件能辨认这是一个bookdown图书项目，
从而为其提供一键编译快捷方式。
元数据中`output`项指定默认的输出格式。
`documentclass`项为借助LaTeX编译PDF格式指定LaTeX的模板，
现在还不能支持ctexbook模板所以使用了book模板。
`bibliography`项指定一个或者几个.bib格式的文献数据库。

一个bookdown图书项目除了`index.Rmd`文件之外，
一般还应该有一个`_bookdown.yml`文件存放与整本书有关的YAML元数据。
例如

```
new_session: true
book_filename: 'statcompc'
language:
  label:
    thm: '定理'
    def: '定义'
    exm: '例'
    proof: '证明: '
    solution: '解: '
    fig: '图'
    tab: '表'
  ui:
    chapter_name: ''
delete_merged_file: true
```

其中`new_session: true`设置很重要，
这使得每一个Rmd文件中的R程序都在一个单独的R会话中独立地运行，
避免了不同Rmd文件之间同名变量和同名标签的互相干扰。
`book_filename`是最终生成的LaTeX PDF图书或者ePub电子书的主文件名。
`language`下可以定制一些与章节名、定理名等有关的名称。

另外一个需要的设置文件是`_output.yml`文件，
用于输出格式的设置。
这部分内容也可以包含在`index.Rmd`的元数据中`output`条目下面。
内容如

```
bookdown::gitbook:
  includes:
    in_header: mathjax-local.html
  config:
    toc:
      before: |
        <li><a href="http://www.math.pku.edu.cn/teachers/lidf/course/statcomp/_book/index.html">统计计算</a></li>
      after: |
        <li><a href="http://www.math.pku.edu.cn/teachers/lidf/" target="blank">编著：李东风</a></li>
    download: ["pdf"]
bookdown::pdf_book:
  includes:
    in_header: preamble.tex
  latex_engine: xelatex
  citation_package: biblatex
  keep_tex: yes
bookdown::epub_book: default
```

其中的`style.css`是自定义的CSS显示格式，
可以去掉这一行，使用默认的CSS格式。
这个例子文件分为三部分，
gitbook、pdf\_book和epub\_book三种输出格式分别设置了一些输出选项。
在gitbook部分，
设置了目录上方显示的书的主页的链接（`before`项）和目录下方显示的作者信息。
在`in_header`部分插入了一部分个性化的HTML代码，
这部分代码是使用本地的数学公式显示支持以免外网不通时数学公式不能显示，
插入的内容将出现在每个生成的HTML文件的head部分。

在`pdf_book`部分，设置了通过LaTeX编译整本书为PDF的一些选项。
指定了`latex_engine`为`xelatex`，
这对中文支持很重要。
`in_header`选项要求在LaTeX文件导言部分插入一个`preamble.tex`文件，
内容如：

```
\usepackage{ctex}

\usepackage{amsthm,mathrsfs}
\usepackage{booktabs}
\usepackage{longtable}
\makeatletter
\def\thm@space@setup{%
  \thm@preskip=8pt plus 2pt minus 4pt
  \thm@postskip=\thm@preskip
}
\makeatother
```

其中很重要的是使用ctex包来支持中文。
在index.Rmd的元数据中也可以指定一些LaTeX选项，
比如指定页边距等（这些设置有些是非标准的，一般不需要）：

```
classoption: twoside
fontsize: 12pt
linestretch: 1.5
geometry: "left=4cm, right=3cm, top=2.5cm, bottom=2.5cm"
fontsize: 12pt
linestretch: 1.5
toc-depth: 1
lof: True
lot: True
```

在bookdown项目中与`index.Rmd`同级的所有.Rmd文件都自动作为书的一章，
除非文件名以下划线开头。
这样做的好处是作者可以任意地增删章节，
编译整本书时章节编号会自动调整。
但是，
章节的顺序将按照文件名的字典序排列，
所以，
所有的包含一章内容的.Rmd文件，
最好命名为类似`0201-rng.Rmd`这样的名字，
文件名前面人为地加上排序用的序号，
使得章节按照自己的次序排列。
实际上，
也可以设置不自动将每个.Rmd文件都作为一章，
而是在`_output.yml`中设置一项`rmd_files`，
列出所有需要作为一章的文件，并以列出次序编译，如

```
rmd_files: ["index.Rmd", "rng.Rmd", "simulation.Rmd", "refs.Rmd"]
```

这时，应该添加一个`_site.yml`文件，内容如：

```
site: "bookdown::bookdown_site"
output: bookdown::gitbook
```

## B.3 章节结构

除了`index.Rmd`文件，
项目中每个.Rmd文件都作为一章。
每个.Rmd文件第一行，
应该是以一个井号和空格开头的一级标题，
后面再加空格然后有大括号内以井号开头的章标签，
如

```
# 随机数 {#rng}
```

这些章标签去掉井号后会作为生成的HTML文件的名字，
所以一定要有章标签，
而且章节标签在全书中都不要重复以免冲突。
文件内可以用两个井号和一个空格开始的行表示节标题，
最后也应该有大括号内以井号开头的节标签，如

```
# 随机数 {#rng}

## 均匀随机数发生器 {#rng-unif}
```

使用bookdown写书，
一般每章不要太长，
否则编译预览很慢，
读者浏览网页格式也慢。

内容相近的章节可以作为一个“部分”。
为此，
在一个部分的第一个章节文件的章标题前面增加一行，
以`# (PART)`开头，
以`{-}`结尾，
中间是部分的名称，如

```
# (PART) 随机数和随机模拟 {-}

# 随机数 {#rng}
```

书的最后可以有附录，
附录的章节将显示为`A.1`, `B.1`这样的格式。
为此，
在附录章节的第一个文件开头加如下的第一行标题行：

```
# (APPENDIX) 附录 {-}

# 一些定理的证明 {#formula}
```

## B.4 书的编译

建议使用RStudio软件编辑内容，
管理和编译整本书。

在`index.Rmd`或者`_bookdown.yml`中设置`site: bookdown::bookdown_site`后，
RStudio就能识别这个项目是一个bookdown项目，
这时RStudio会有一个Build窗格，其中有“Build book”快捷图标，
从下拉菜单中选择一个输出格式（包括gitbook、pdf\_book、epub\_book），
就可以编译整本书。
对gitbook格式，
即HTML网页格式，
编译完成后会弹出一个预览窗口，
其中的“Open in Browser”按钮可以将内容在操作系统默认的网络浏览器中打开。

另一种办法是在命令窗口用如下命令编译（以输出gitbook为例），
我个人认为这种办法更好用：

```
bookdown::render_book("index.Rmd", 
  output_format="bookdown::gitbook")
```

编译结果默认保存在`_book`子目录中，
可以在`_bookdown.yml`中设置`output_dir`项改为其它子目录。
编译整本书为pdf\_book格式时，如果成功编译，
也会弹出一个PDF预览窗口。
可以在`_book`子目录中找到这个PDF文件。

将书编译为PDF需要利用LaTeX编译器，
这需要单独安装LaTeX编译软件，
LaTeX编译器对输入要求十分严格，
一丁点儿错误都会造成整本书的编译失败，
所以对于不熟悉LaTeX的用户，
不建议使用bookdown的pdf\_book输出格式。
当前的R Markdown仅支持谢益辉的[TinyTeX](https://yihui.name/tinytex/)软件，
安装和管理参见[A.12.1](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-latex-tinytex)。

对于较短的书，
做了一定修改后都可以重新编译gitbook结果和pdf\_book结果。
在书比较长了以后，
每次编译都花费很长时间，
所以可以仅编译gitbook格式的一章，
修改满意后再编译整本书。
仅编译一章也需要所有的.Rmd文件都是已经编译过一遍的，
新增的Rmd文件会使得编译单章出错，
每次新增了Rmd文件都应该重新编译整本书，
但是内容修改后不必要重新编译整本书，
可以仅编译单章。

编译单章现在没有快捷图标，
只能在RStudio控制台（命令行）运行如下命令：

```
bookdown::preview_chapter("chap-name.Rmd", 
  output_format="bookdown::gitbook")
```

其中`chap-name.Rmd`是要编译的单章的文件名。
编译完成后在结果目录（默认是`_book`）中找到相应的HTML文件打开查看，
再次编译后仅需在浏览器中重新载入文件。
建议使用Google chrome浏览器，
用MS IE或者Edge浏览器对gitbook的Javascrpt支持不够好，
使得目录的层级管理、自动滚动、单章编译后的目录更新不正常，
而chrome则没有问题。

编译单章也不能解决所有的问题，
有些问题还是需要编译整本书，
而章节很多时整本书编译又太慢。
为此，
可以在项目中增加一个临时的部分内容子目录，
如`testing`子目录，
在子目录中存放相同的设置文件`index.Rmd`、`_bookdown.yml`、`_output.yml`，
以及图形文件、文献数据库文件，
并将要检查的若干章节复制到`testing`子目录中，
在`testing`中新建一个bookdown项目，
然后编译其中的整本书。
这在调试部分章节的HTML和PDF输出时很有效。
解决问题后只要将修改过的章节复制回原始的书的目录中。

有时仅仅想验证某个长数学公式或者表格，
用上述的编译单章或者单独一个小规模测试项目的办法也不经济。
这时，单独开一个备用的普通RStudio项目，
不能是bookdown项目，
在其中的Rmd文件中验证数学公式和表格的编排，
这样最为方便。

有时需要根据编译输出目标采用不同的设置，
比如，
输出为html时可以使用HTML插件(widgets)，
而输出到PDF则不可以。
可以用
`knitr::opts_knit$get("rmarkdown.pandoc.to")`
感知当前编译过程的目标，
对HTML类，输出为`"html"`，
对PDF，输出为`"latex"`。

## B.5 交叉引用

在写作时，每个一级到三级标题都应该有自定义的标签，
格式是在标题行末尾空格后添加`{#label}`，
其中label是自己指定的标签，
使用英文、数字、减号，
不要使用中文，
而且整本书不要有重复的标签。
为避免不同章节使用了重复标签，
可以取`label`的前一部分为所在章节的文件名。

如果要引用某一章节，
有如下的做法：

* `§\@ref(label)`，`label`是某个标题对应的标签。
  结果显示为如`§3.1.1`这样的章节号，并可点击，
  点击时跳跃到相应的章节。
* `[链接文本](#label)` 其中`label`是某个标题对应的标签。
  结果产生一个链接，显示为`链接文本`，点击时跳到`label`对应的章节。

有时需要在数学公式内部、图形的说明中引用定理编号、公式编号等，
这需要预先定义单独的文字性引用，
然后再使用该文字引用。
定义文字引用如：

```
(ref:mytextlabel) 见定理\@ref(thm:orth)
```

可以在数学公式内部、图形说明的字符串内用`(ref:mytextlabel)`的格式调用上面定义的文字引用。
目前在数学公式内部使用有BUG。

## B.6 数学公式和公式编号

通过R的knitr和rmarkdown扩展包以及pandoc软件，
.Rmd格式文件已经支持数学公式，
见[R Markdown说明](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html)。

在用`$$`符号在两端界定的公式后面，
可以用`\tag{标号}`命令增加人为的公式编号，如

```
$$
y = f(x)
\tag{*}
$$
```

结果显示为

\[
y = f(x)
\tag{\*}
\]

要注意的是，
在`$$`界定的数学公式内用了aligned环境后，
仅能在`\end{aligned}`之后加`\tag{标号}`命令，
而不能写在aligned环境内。
这样，
多行的公式将不能为每行编号。

用`\tag`命令人为编号比较简单易用，
但是在有大量公式需要编号时就很不方便，
只要增加了一个公式就需要人为地重新编号并修改相应的引用。
bookdown包支持对公式自动编号，
并可以按公式标签引用公式，
引用带有超链接。

bookdown的自动编号对LaTeX的equation环境、align环境都可以使用，
而且不需要在两端用`$$`界定。
在公式的末尾或者一行公式的`\\`换行符之前，
写`(\#eq:mylabel)`，
其中`mylabel`是自己给公式的文字标签，
文字标签可以使用英文字母、数字、减号、下划线。
如

```
\begin{align}
f(x) =& \sum_{k=0}^\infty \frac{1}{k!} x^k (\#eq:efunc-sum) \\
  = e^x (\#eq:efunc-ex)
\end{align}
```

将会对两行公式自动编号。
引用公式时，
用如`\@ref(eq:mylabel)`，其中`mylabel`是公式的自定义标签，
编译后这样的引用会变成带有链接的圆括号内的编号。

公式编号在全书中都不要有冲突（不同的公式定义了相同的编号）。
一种办法是，自定义的公式标签的开头以章节文件名开头。

## B.7 定理类编号

定理、引理、命题、例题等，
使用特殊的markdown代码格式，
以三个冒号开头，
以三个冒号结尾，
在开头的三个反单撇号后面空格后写`{.theorem}`表示定理。
在`.theorem`后面，
可以用空格分隔后写一个定理的自定义标签，
标签以`#`开头，由字母、数字、减号组成，
`#`号作为标签的开头标志但不作为标签的一部分。
可以用`name="定理名称"`指定一个显示的定理名。

设某个定理的自定义标签是`#mythlabel`，
则可以用如`\@ref(thm:mythlabel)`引用此定理的编号，
编号是在每一章内从头编号的。
编号有自动生成的链接。

例如：

```
::: {.theorem #norlim-weakconv name="弱收敛"}
$\xi_n$依分布收敛到$\xi$，
当且仅当对任意$\mathbb R$上的一元实值连续函数$f(\cdot)$都有
$$
  E f(\xi_n) \to E f(\xi), \ n \to \infty .
$$
:::
```

当定理或例子内有列表时，
一定注意列表前后要空行，
否则会导致嵌套错误。
这种错误在编译HTML时无法发现，
但是会造成结果莫名其妙地出错。

bookdown提供了证明环境，
但是不太实用。

对例题，
将`theorem`替换成`example`，
在引用时将`thm`替换成`exm`。
如：

```
::: {.example #noralim-aspr-ce name="依概率收敛不a.s.收敛反例"}
a.s.收敛推出依概率收敛，
但是反之不然。给出反例。
:::
```

定理类的段落包括如下的种类：

表B.1:  定理类段落

| 环境名 | 默认显示名 | 标签前缀 |
| --- | --- | --- |
| theorem | Theorem | thm |
| lemma | Lemma | lem |
| corollary | Corollary | cor |
| proposition | Proposition | prp |
| conjecture | Conjecture | cnj |
| definition | Definition | def |
| example | Example | exm |
| exercise | Exercise | exr |

其中的显示名可以在`_bookdown.yml`的`language`的`label`属性中修改，
见[B.2](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/bookdown.html#bookdown-settings)。

## B.8 文献引用

bookdown使用.bib格式的文献数据库，
关于.bib格式的文献数据库请参考LaTeX的有关说明。
在`index.Rmd`的YAML元数据部分或者`_bookdown.yml`中用`bibliography`可以设置使用的一个或者多个.bib格式的文献数据库文件。
设某篇文章的.bib索引键是`Qin2007:comp`，
用 `@Qin2007:comp` 可以引用此文献，
用`[@Qin2007:comp]` 可以生成带有括号的引用。

指定.bib文件时可以用相对路径，
如“`../docs/mybib.bib`”。
有多个文件时，
写在方括号`[]`中并用逗号分隔，
如`bibliography: [mybib.bib, ../docs/refs.bib]`。

为了使得产生的文献引用包括跳转超链接，
可以在`index.Rmd`的元数据部分添加`link-citations: true`设置。

为了生成某个R扩展包的.bib格式的引用，
可以用R程序制作，例如：

```
toBibtex(citation("MASS"))
## @Book{,
##   title = {Modern Applied Statistics with S},
##   author = {W. N. Venables and B. D. Ripley},
##   publisher = {Springer},
##   edition = {Fourth},
##   address = {New York},
##   year = {2002},
##   note = {ISBN 0-387-95457-0},
##   url = {https://www.stats.ox.ac.uk/pub/MASS4/},
## }
```

下面是一个样例.bib文件的内容（注意要用UTF-8编码保存）：

```
% Encoding: UTF-8

@Book{MWP06-HighStat,
  author    = {茆诗松 and 王静龙 and 濮晓龙},
  title     = {高等数理统计},
  year      = {2006},
  edition   = {第二版},
  publisher = {高等教育出版社}
}


@BOOK{Unwin-Visualize06,
  title = {Graphics of Large Datasets Visualizing a Million},
  publisher = {Springer},
  year = {2006},
  author = {Antony Unwin and Martin Theus and Heike Hofmann}
}


@Article{Wichmann1982:RNG,
  author  = {Wichmann, B. A. and Hill, I. D.},
  title   = {Algorithm as 183: An efficient and portable pseudo-random number generator},
  journal = {Applied Statistics},
  year    = {1982},
  volume  = {31},
  pages   = {188–190. Remarks: 34, 198 and 35, 89},
}

@Book{QGCZ2011:StochProc,
  author    = {钱敏平 and 龚光鲁 and 陈大岳 and 章复熹},
  title     = {应用随机过程},
  year      = {2011},
  publisher = {高等教育出版社},
}
```

## B.9 插图

bookdown图书的插图有两种，
一种是已经保存为图形文件的，
主要是png、jpg和pdf图片；
另一种是文中的R代码生成的图形。

已经有图形文件的，
可以用markdown格式原来的插图方法，
见[markdown格式介绍](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/markdown.html)。
但是，这样做不能给图形自动编号，
另外因为制作图书是有网页和PDF书两种主要输出格式的，
原有的插图方式在这两种输出格式上有细微的不一致。
所以，最好是统一使用Rmd的插图方法。

Rmd的插图方法就是写一段R代码段来插图，
如果是用程序作图，则代码中写作图的代码；
如果是已有的图形文件，
可以在一个单独的R代码段中用类似下面的命令插图：

`{r} knitr::include_graphics("figs/myfig01.png")`

其中`figs`是存放图形文件的子目录名，
`myfig01.png`是要插入的图形文件名。
这样，
如果同时还有`myfig01.pdf`的话，
则HTML输出使用png图片而PDF输出自动选用pdf文件。
另外，
插图的选项在代码段的选项中规定：
用代码段的`fig.with`和`fig.height`选项指定作图的宽和高（英寸），
用`out.width`和`out.height`选项指定在输出中实际显示的宽和高，
实际显示的宽和高如果使用如`"90%"`这样的百分数单位则可以自动适应输出的大小。

为了使得插图可以自动编号并可以被引用，
为代码段指定标签并增加一个`fig.cap="..."`选项指定图形标题。
代码段的标签变成浮动图形的标签，如`myfiglabel`，
则为了引用这个图只要用`\@ref(fig:myfiglabel)`。
注意，在整本书中这些标签都不能重复，
否则编译LaTeX支持的PDF输出会失败。

有些插图会伴随很长的说明文字，
这可以用代码段的`fig.cap=`选项指定，
但是其中的Markdown特有的格式在转换LaTeX时不一定支持，
而且在代码段选项中写太长的文字说明也是的程序难以辨认。
所以，
可以使用文字引用的方式：
在单独的一段中，
用如下格式定义一段可引用的文字内容：

```
(ref:mylabel) 这里用实际的文字内容代替，不允许换行，不能分段。
```

其中`mylabel`是自己定义的仅由英文大小写字母、数字和减号组成的引用标志符。在需要使用这段文字的位置，用`(ref:mylabel)`这种格式引用。
注意定义和引用都是用的`(ref:mylabel)`语法。

用R生成PDF格式的图形时，
需要指定中文作为`family`选项，
所以在每个源文件的开头应该加上如下的设置，
使得生成PDF图时中文能够正确显示：

```
```{r setup-pdf, include=FALSE}
pdf.options(family="GB1")
```
```

其中`include=FALSE`表示要不显示代码段的代码，
有运行结果也不插入到输出结果中，
是否允许运行视缺省的`eval=`的值而定。

## B.10 表格

### B.10.1 Markdown表格

bookdown书的表格也有两种，
一种是原来markdown格式的表格，
最好仅使用管道表，
管道表对中文内容支持最好。
为了对这样的表格自动编号，
需要在表格的前面或者后面空开一行的位置，
写

```
Table: (\#tab:mylabel) 表的说明
```

其中`mylabel`是自定义的表格标签。
在引用这个表时用如 `\@ref(tab:mylabel)`。

### B.10.2 用`kable()`函数制作表格

另一种表格是R代码生成的表格，
主要使用`knitr::kable()`函数。
在`knitr::kable()`函数中用选项`caption=`指定表格的说明文字（标题），
这时生成表格的R代码段的标签，如`myfiglab`，
就自动构成了表格的引用标签主干，
实际引用如 `\@ref(tab:myfiglab)`。

加选项digits，
可以指定小数点后的数字位数。
不同列使用不同位数时，
可以指定一个包含各列小数点后位数的向量。

为了适应较长的表，
可以在LaTeX的`preamble.tex`中引入longtable包，
并在`knitr::kable()`中加选项`longtable=TRUE`。

可以设置`kable()`显示缺失值的方式，
如；

```
options(knitr.kable.NA = "")
```

则缺失值显示成空单元。
默认显示是`NA`。

### B.10.3 R中其它制作表格的包

如果要对表格进行更丰富的定制，
可以使用[kableExtra包](https://haozhu233.github.io/kableExtra/)。

flextable和fluxtable支持较多的输出格式和常见表格功能。

还有许多扩展包，不一一列举。

## B.11 数学公式的设置

bookdown在生成PDF时使用LaTeX软件，
所以PDF输出的数学公式的支持很好，
但是LaTeX编译器也很挑剔，
稍微一点错误也造成编译失败。
比如，在行内公式内部如果紧邻`$`符号有多余的空格，
如`$ y $`，编译PDF时会出错。

bookdown生成的gitbook格式的网页书籍，
在有数学公式时，
使用MathJax库在浏览器中显示数学公式。
[MathJax](https://www.mathjax.org/)是用于网络浏览器中显示数学公式的优秀的Javascript程序库，
可免费使用。
但是，
当数学公式中含有中文（用`\text{}`或`\mbox{}`命令）时，
数学公式可能会显示不正常。
另外，数学公式默认使用远程服务器上的MathJax程序库处理，
在网络不通畅时显示很慢或者无法显示。
bookdown中使用MathJax的版本2，
但MathJax已经升级到版本3，
版本3有很大改动，
暂时还应该使用版本2以避免冲突。

为了避免远程调用MathJax程序库的麻烦，
改为本地使用。
将MathJax安装在了书生成的网站主目录的上三层，用`../../../MathJax/mathjax.js`路径访问。
假设下载整个MathJax库后解压放在了书的网页文件所在子目录的上三层的位置。

增加设置文件`_header.html`：

```
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  jax: ["input/TeX","output/SVG"],
  extensions: ["tex2jax.js","MathMenu.js","MathZoom.js"],
  TeX: {
    extensions: ["AMSmath.js","AMSsymbols.js","noErrors.js","noUndefined.js"]
  }
});
</script>
<script type="text/javascript"
   src="../../../MathJax/MathJax.js">
</script>
```

如果希望用远程服务器上的MathJax，可以使用如下的`mathjax-cdnjs.html`设置文件：

```
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  jax: ["input/TeX","output/HTML-CSS"],
  extensions: ["tex2jax.js","MathMenu.js","MathZoom.js"],
  TeX: {
    extensions: ["AMSmath.js","AMSsymbols.js","noErrors.js","noUndefined.js"]
  }
});
</script>
<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.9/MathJax.js">
</script>
```

为了调用这样的设置文件，在`_output.yml`设置文件中如下设置：

```
bookdown::gitbook:
  css: style.css
  includes:
    in_header: _header.html
```

注意，MathJax要求先设置再调入。
由于浏览器对MathJax输出方式有记忆，
所以如果不能正常显示中文公式，
需要右键单击公式，选择“Math Settings—Math Renderer”中的“SVG”或者“HTML-CSS”。

## B.12 使用经验

### B.12.1 学位论文

Ed Berry在网上分享了用bookdown生成PDF学位论文的经验：
<https://eddjberry.netlify.com/post/writing-your-thesis-with-bookdown/>
。
Chester Ismay提供了一个R扩展包thesisdown，
见
<https://github.com/ismayc/thesisdown>，
例子见<https://thesisdown.netlify.com/>。

### B.12.2 LaTeX

有数学公式时，
对行内公式边缘处多余的空格十分敏感，
所以行内公式边缘不要有空格。

如果已经有用LaTeX写的书，
要转换为bookdown的Rmd格式，
可以用RStudio的支持RegEx的替换模式，如

* `\\keyword{([^}]+?)}`替换成`**\1**`。
* `\\textbf{([^}]+?)}`替换成`**\1**`。
* `\\texttt{([^}]+?)}`替换成`\1`。

可以写一个函数对LaTeX文件进行转换生成Rmd文件，
再手工修改。函数如

```
latex2rmd <- function(fname="_tmp/tomd.tex", encoding="UTF-8"){
  lines <- readLines(fname, encoding=encoding)
  if(encoding != "UTF-8")
    lines <- iconv(lines, from=encoding, to="UTF-8")
  print(head(lines, 10))
  lines <- gsub("\\\\keyword[{](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/[^}]+?)[}]", "**\\1**", lines, perl=TRUE)
  lines <- gsub("\\\\textbf[{](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/[^}]+?)[}]", "**\\1**", lines, perl=TRUE)
  lines <- gsub("\\\\texttt[{](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/[^}]+?)[}]", "`\\1`", lines, perl=TRUE)
  lines <- gsub("\\\\label[{](eq:[^}]+?)[}]", "(\\\\#\\1)", lines, perl=TRUE)
  lines <- gsub("\\\\eqref[{](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/[^}]+?)[}]", "\\\\@ref(\\1)", lines, perl=TRUE)
  lines <- gsub("\\\\bs\\\\", "\\\\boldsymbol\\\\", lines, perl=TRUE)
  lines <- gsub("\\\\bs ", "\\\\boldsymbol ", lines, perl=TRUE)
  lines <- gsub("\\\\Rbb", "\\\\mathbb R", lines, perl=TRUE)
  lines <- gsub("\\\\Zbb", "\\\\mathbb Z", lines, perl=TRUE)
  lines <- gsub("\\\\Var[(]", "\\\\text\\{Var\\}(", lines, perl=TRUE)
  lines <- gsub("\\\\Cov[(]", "\\\\text\\{Cov\\}(", lines, perl=TRUE)
  lines <- gsub("(\\\\S)(\\d)", "§\\2", lines, perl=TRUE)
  lines <- gsub("(\\\\S) ", "§ ", lines, perl=TRUE)
  lines <- gsub("(\\\\defeq)", "\\\\stackrel{\\\\triangle}{=}", lines, perl=TRUE)
  lines <- gsub("\\\\argmin_", "\\\\mathop{\\\\text{argmin}}_", lines, perl=TRUE)
  lines <- gsub("^\\s*\\\\item", "*", lines, perl=TRUE)
  lines <- gsub("\\\\begin[{]frame[}]", "", lines, perl=TRUE)
  lines <- gsub("\\\\end[{]frame[}]", "", lines, perl=TRUE)
  lines <- gsub("\\\\begin[{]itemize[}]\\[<\\+->\\]", "", lines, perl=TRUE)
  lines <- gsub("\\\\end[{]itemize[}]", "", lines, perl=TRUE)
  lines <- gsub("\\\\begin[{]itemize[}]", "", lines, perl=TRUE)
  lines <- gsub("\\\\bm ", "\\\\boldsymbol ", lines, perl=TRUE)
  lines <- gsub("\\\\bm\\\\", "\\\\boldsymbol\\\\", lines, perl=TRUE)
  lines <- gsub("\\\\tr[(]", "\\\\text{tr}(", lines, perl=TRUE)
  lines <- gsub("\\begin{align*}", "$$\\begin{aligned}", lines, fixed=TRUE)
  lines <- gsub("\\end{align*}", "\\end{aligned}$$", lines, fixed=TRUE)
  
  print(head(lines, 20))
  ##writeLines(lines, "_tmp/fromtex-clean.Rmd")
  con1 <- file("_tmp/fromtex-clean.Rmd", "wt", encoding="UTF-8")
  writeLines(lines, con=con1)
  close(con1)
}
```

### B.12.3 算法

Rmd格式对算法编排的支持不够好。
编排算法可以用表格来换行，
仍用`\qquad`和`\qquad`缩进，
不要用空格缩进，因为在LaTeX转PDF时空格会损失。
但是算法内容中有公式含有竖线时还是无法与表格线区分开来。

编排算法也可以用数学公式，
写在`$$`界定范围内，
用aligned环境分行，
缩进使用`\quad`和`\qquad`。
只是无法对`if`这样的关键字用重体排印。

### B.12.4 中文乱码

为了能够生成中文的PDF，不要指定documentclass为ctexbook，
而是指定为book，然后在`preamble.tex`中引入`ctex`包。

为了PDF输出，不要引入太多的数学包，
因为从markdown到HTML不支持复杂的数学。

用R作图时如果图形中有汉字，
在代码块选项中加上`dev="png", dpi=300`。
否则生成PDF时会有中文编码问题。
另一办法是在每个.Rmd文件开头的setup源代码段插入

```
pdf.options(family="GB1")
```

这样可以生成支持中文字的PDF图形。

在编译出错时，会在主目录留下编译的tex源文件及相关文件。
但是，此tex源文件中使用的R生成的图片路径不对，
需要将tex源文件复制到`_bookdown_files`目录，
将直接插入的图片也复制到这个目录，
然后编译tex文件发现问题，逐个修复。
建议建立小的测试项目专门调试有问题的文件。

### B.12.5 图片格式

Bookdown在网页和PDF输出格式中都支持PNG，
所以这是应尽量采纳的格式。
也支持JPEG和PDF格式，但网页中可能无法显示PDF格式。
支持SVG格式，但仅在网页中可显示。

对于少量的图形文件，
可以安装一个图像编辑软件GIMP，
这是一个自由软件，
与PhotoShop功能类似。
比如，
要将一个SVG文件转换为PNG，
可以在GIMP中打开，
打开时可以选一个放大比例，如2倍，
然后调用文件菜单的导出功能，
将扩展名改为“.png”就可以保存成PNG。

如果有大批图片格式要转换，
可以安装ImageMagick软件。
可以用命令行命令转换，
这样可以编程生成一系列的转换命令进行批量转换。
在Windows的命令行窗口运行转换命令如：

```
magick test.svg -resize 300% test.png
```

产生批量转换命令如：

```
setwd("d:/tmp")
li = list.files(pattern="[.]svg$")
for (fi in li){
  fn = gsub("(.*)[.]svg$", "\\1.png", fi)
  cmd = paste("magick", fi, "-resize 300%", fn)
  cat(cmd, "\n")
}
```

### B.12.6 其它经验

连分数的加号是`\genfrac{}{}{0pt}{}{}{+}`。

## B.13 bookdown的一些使用问题

* 在数学公式中用`\text{\@ref(eq:label)}`引用公式，HTML成功，
  LaTeX版本会有BUG，重复了`\`。
  如果使用文内的链接，则LaTeX成功，
  HTML不成功。
* YAML部分有的中文文字（如作者名）会出错，
  代以ASCII则不出错。
  2020-06-05发现此问题已消失。
* example的自动编号有问题，不加label的example，
  其HTML结果在不同章之间会编号混淆，
  避免问题的临时办法是example都加上label。
* 用本地文件格式提供的下载文件不能用中文文件名。
* `# 参考文献 {-}`这种在一本书末尾生成参考文献列表的办法，
  可能造成“file name too long”错误。
  2020-06-05发现此问题已消失。
* BUG: 编译时出现“file name too long”错误，可能原因：

  * 章节名称必须有英文、数字、减号的标签。
  * 用行尾标记`{-}`的方法取消章节进入目录时，中文标题会出错，改用英文标题。`# (PART) 部分名 {-}`也是如此，需要用英文名。

  2020-06-05发现此问题已消失。