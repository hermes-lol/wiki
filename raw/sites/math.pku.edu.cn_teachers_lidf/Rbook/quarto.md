---
crawl_time: '2026-01-17 14:20:52'
framework: rbook
title: 22 Quarto格式文件 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/quarto.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 22 Quarto格式文件

## 22.1 介绍

[Quarto](https://quarto.org/)是POSIT(原RStudio)团队开发的一个开源软件，
可以将包含R、Python、Julia、Observable JS源程序的markdown文件产生运行结果后转换为各种输出格式，
这些源文件可以是普通的包含程序代码的markdown文件(扩展名为`.qmd`)，
也可以是Jupyter笔记本文件(扩展名为`.ipynb`)。
支持HTML、MS Word、LaTeX PDF、ePub、网站、幻灯片等多种输出格式。

软件基于Pandoc的扩展的markdown文件转换功能。
在转换前，
会运行其中的源程序，
将得到的文字、表格、图形结果插入到文档中。
可以在R的knitr包支持下运行这些程序，
也可以在Jupyter软件的支持下运行。

有另外一种较早的“R Markdown”格式，
与quarto格式相似，
但是R markdown主要针对R语言用户，
而quarto软件则可以支持多种编程语言，
可以适用于更广泛的程序语言用户。

现在qmd格式还有一些不完善的地方，
比如多行公式编号还没有实现，
仅能对一个公式使用统一编号。

## 22.2 安装

Quarto软件有一个单独的可执行程序，
为了实现所有功能应安装此可执行程序；
如果仅在RStudio中使用其功能，
而不需要使用Jupyter格式，
可以不必安装此程序。

为了在R和RStudio中使用Quarto格式，
应同时安装rmarkdown扩展包，
建议安装quarto扩展包。

RStudio支持使用“可见即所得”方式编辑qmd格式的文件，
所以是Quarto软件很好的伴随软件。
即使使用Python或Julia作为主要编程语言，
也可以用RStudio来进行qmd文档的编辑、运行和转换。
RStudio软件内置了Quarto软件，
也可以使用已独立安装的Quarto软件。

为了能够将Quarto源文件通过LaTeX转换为PDF，
需要安装TinyTeX软件，
这是一个LaTeX编译系统软件，
安装方法参见§[22.20](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/quarto.html#qmd-tinytex)。

## 22.3 简单样例文件

Quarto可以用来生成多个源文件组成的图书、网站，
但最简单的用法是用单个源文件生成单个的网页、Word、PDF文件结果。
在RStudio中，
可以用“File – New File – Quarto Document”菜单生成一个包含模板内容的空文件。
在文件开头，
一般有用三个减号组成的行在首尾界定的一部分，
称为YAML属性设置。
后面是正文内容。
一个简单的文件如：

```
---
title: "演示基本功能"
format: html
---
```

```
## 格式介绍

Quarto格式基于markdown格式与pandoc软件，
可以转换“.qmd”和“.ipynb”(Jupyter笔记本)格式的文件，
参见<https://quarto.org>.

## R代码

可以用如下方法插入R代码段：

```{r}
1 + 1
```

可以用“`#|`”添加特殊注释，
作为代码段属性，如：

```{r}
#| echo: false
2 * 2
```
```

## 22.4 RStudio的可视化编辑

RStudio支持visual（可视化）编辑模式和source（源代码）编辑模式。
可视化编辑对初学者比较友好，
所以习惯了用Word的用户可以从可视化编辑方法开始使用Quarto的功能。
在可视化编辑方式中，
可以直接将图形粘贴入编辑界面，
这时RStudio会将该图形的副本复制保存进入项目目录中，
也可以修改图形的大小、标题等属性。

可视化编辑的一个缺点是会自动调整换行，
这使得切换回到源代码编辑模式时感觉自己输入的内容一片混乱，
提供的各种设置都不能解决这个问题。
习惯使用源代码编辑模式的用户不要切换到可视化编辑模式！
如果偶尔不小心切换过去了，
可以切换回到源代码模式然后用Windows的`Ctrl+Z`快捷键恢复编辑内容。

因为可视化编辑模式也有很大的优势，
对于习惯源代码方式的人，
可以打开一个小的临时文件使用可视化编辑方法编辑，
然后将生成的源代码剪切进入主要的源文件。

## 22.5 生成输出文件

### 22.5.1 使用RStudio

Quarto格式可以生成HTML、Word、PDF等输出格式。
输出PDF格式需要安装TinyTeX软件，见§[22.20](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/quarto.html#qmd-tinytex)。

只要在RStudio的文件编辑窗口点击“Render”快捷图标就可以转换为指定的输出文件。
如果使用Preview快捷图标，
则支持在修改文件后自动更新结果。

还可以在R中安装quarto扩展包，
并用如下R命令转换：

```
library(quarto)
quarto_render("index.qmd")
```

在`quarto_render()`中可以加选项`output_format = "pdf"`或`output_format = "docx"`转换为PDF或Word格式。

### 22.5.2 使用Quarto可执行程序

也可以不借助于RStudio，
而是单独安装Quarto的可执行程序，
在操作系统命令行使用如下命令转换某个文件：

```
quarto render index.qmd
```

这默认生成HTML结果，
在上述命令末尾空格并添加`--to pdf`转换为PDF格式（需要预先安装TinyTeX软件）:

```
quarto render index.qmd --to pdf
```

添加`--to docx`转换为MS Word格式:

```
quarto render index.qmd --to docx
```

Quarto可以使用R的knitr扩展包或者Jupyter软件执行qmd文件中的代码。
可以用YAML选项`engine: knitr`选用knitr,
`engine: jupyter`选用Jupyter。

### 22.5.3 Jupyter笔记本源文件的转换

Jupyter笔记本软件是Python系统生态的重要集成编辑、执行、报告环境，
也支持Julia等语言。
Jupyter笔记本软件本身也有保存为HTML、PDF、Word等的功能，
为什么还要使用Quarto？
这是因为科技论文常用的编号图表、公式、章节编号、章节引用、参考文献引用等功能在Jupyter笔记本中没有很好地被支持。

在操作系统中运行如下命令，
可以建立Jupyter笔记本对HTML格式输出结果的预览：

```
quarto preview mydoc.ipynb
```

这个预览会自动更新，
也就是说，
每次笔记本保存时，
预览会自动刷新。
如果要生成最后结果，
不想自动刷新，
可以用`render`命令，如：

```
quarto render mydoc.ipynb
```

为了生成其它格式的输出，
可以加`--to`选项，如：

```
quarto render mydoc.ipynb --to docx
```

生成Word格式，
`--to pdf`生成PDF格式。

Jupyter笔记本文件如果需要一些关于Quarto的设置，
仍可以在文件头部添加YAML选项，
办法是在文件头部添加一个单元格，
将其类型转换为`raw`(原样使用的单元格)，
并在其中添加需要的YAML内容，如：

```
---
title: 测试Python和Quarto
author: XXX
date: 2022-11-27
---
```

Jupyter笔记本不自动运行其中的程序(Python、Julia、R等)，
转换前应确保笔记本中的程序结果是最新的，
可以在转换前重新运行一遍笔记本，
还有一种办法是在`quarto`命令后面加`--execute`选项，
如：

```
quarto render mydoc.ipynb --execute
```

对于.qmd格式的文件，如`mydoc.qmd`，
在操作系统命令行界面中用如下命令转换为Jupyter笔记本：

```
quarto convert mydoc.qmd
```

有些程序计算运行时间很长，
可以安装Jupyter笔记本的缓存功能模块`jupyter-cache`，
使用缓存，
参见：

* <https://quarto.org/docs/computations/python.html>

## 22.6 插入R程序代码

Quarto这样的软件的一个重要目的，
是实现“文学化编程”（literate programming），
并支持可重复科研。
文学化编程的意思是，
将论文和论文用到的计算机程序写在同一个源代码中，
经过一个运行、编译过程，
将计算机程序运行产生结果，
将论文内容、源程序、程序产生的文字、表格、图形结果有机地结合成一个最终论文，
可以用多种形式保存，如HTML、Word、PDF等。
用这种办法可以将研究过程很好地记录下来并可以很容易地重现研究结果，
所以可以支持可重复科研。

在Quarto源文件中插入R、Python、Julia、Observable JS代码，
就是实现这种功能的最重要的一步。

如果仅是插入R程序代码，而不需执行，
只要写成```` ```r ...``` ````的格式，
以```` ```r ````行开头，
以```` ``` ````行结尾。
这会自动对代码进行彩色加亮显示。

为了执行，
以```` ```{r} ````行开头，
以```` ``` ````行结尾。
在Windows版的RStudio中，
可以用“Ctrl+Alt+I”快捷键插入代码段。

在代码内容中可以用“`#|`”开头的特殊注释作为代码段选项，
“`#| label: #mylabel`”用来给代码段一个可引用标签，
可引用标签需要在整本书（整篇文章）中都唯一。
如：

```
```{r}
#| label: myaddex
1+2
```
```

在“`#|`”选项中使用YAML格式，如“`#| warning: false`”。
为了在“`#|`”选项中使用R代码，使用“`!expr 代码`”的格式，
如“`#| warning: !expr preview`”，
其中“`preview`”是在前面的R代码段中已定义的逻辑变量。

在RStudio软件中使用时，
这些R代码块都可以点击代码块右上角的“Run current chunk”图标（向右的三角形图标）执行代码块，
并将代码块输出结果直接显示在文档内部，
代码块下方。
这个行为可以通过RStudio软件的“Tools – Global Options”进行设置，
比如改为仅在Console窗格和Plots窗格显示结果，
或者在浮动窗口显示结果。

还有一种插入R源程序的办法是在段内使用R代码，
称为行内代码，
行内代码的结果插入到一个段落中间，
代码以`` `r ``开头，以`` ` ``结尾，
如`` `r sin(pi/2)` ``在结果中会显示为1。
为了原样显示一个反向单撇号，
可以在两边用双反向单撇号界定并用空格隔开内部的内容。

## 22.7 代码段选项

### 22.7.1 标签

在代码段内用“`#| label: mylabel`”添加一个标签，
在RStudio中编辑时可以很容易地按标签导航到某个代码段，
代码段如果生成图形，可以按标签（图形标签以`fig-`开头）在别处交叉引用。
在使用缓存功能时，
可以按代码段标签表示代码段之间的依赖关系。

标签可以包含字母、数字、减号和下划线，
生成图形的代码段标签必须以`fig-`开头。
标签必须在文件中唯一，
如果是多个源文件的图书、网站项目，
标签在不同文件之间也必须唯一。

### 22.7.2 代码段执行选项

代码段可以设置各种执行选项。
可以设置在整个项目的`_quarto.qmd`文件或单个qmd文件的顶层`execute`下面，
如：

```
execute:
  echo: false
  warning: false
  message: false
```

也可以设置在单个代码段中，
如`#| echo: false`，
这时不需要`execute`这个父节点。

可用选项：

* `eval`: true或false，表示是否执行;
* `echo`: true或false，表示是否显示程序代码;
* `output`: true, false或asis，表示显示为经整理的格式、不显示、不进行整理地显示;
* `fig-show`: 选`false`表示不显示生成的图形。
* `message`, `warning`: true或false，表示是否显示提示或警告信息;
* `error`: 选`true`表示在出错时仍继续运行，缺省是`false`。
* `include`: true或false, 表示代码与结果是否都包含，或都不包含，
  但不影响程序代码执行。

在用`library()`载入扩展包时，
经常有大量的版本信息、函数名冲突信息，
这时只要设置`output: false`就可以抑制这些信息显示到排版结果中。

## 22.8 插图

### 22.8.1 插图方法

可以用R程序生成图形，
Quarto会自动将该图形插入到结果文档中。

为了在.qmd文件中插入已有图形文件，
若使用本地文件，
仍可以用markdown语法，
即“`![代替显示文字](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/图片地址)`”，
如“`![百度](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/baidu01.png)`”

在使用RStudio编辑文件时，
可以打开可视化编辑模式(Visual模式)，
选快捷图标Figure/Image，
选择要插入的图片，
并选择一些选项。
也可以将本项目目录或子目录内的图形文件直接拖曳到要插入的位置。
还可以复制图片，
然后在可视化编辑模式中的文档中粘贴图片，
这会在项目文件夹中生成图片的副本。

可以使用R程序插图，如：

```
```{r}
#| label: fig-mytestimg
#| fig-cap: "百度图标"
#| echo: false
knitr::include_graphics("figs/baidu01.png")
```
```

### 22.8.2 插图有关代码段选项

可以用选项修改插入的图形属性。
这些选项适用于用代码段程序生成或者插入的图形。

代码段中设置“`#| fig-cap: 图形说明`”和“`#| label: fig-mylabel`”后自动生成图形编号，
生成的图形文件名会利用标签，
并可用“`[@fig-mylabel]`”格式引用图形，
显示如“图2.1”。
如果仅需要生成编号数字，
可以写成“`[-@fig-mylabel]`”。
设置`fig-cap`和`lable`后使得图形变成“浮动”图形，
在PDF输出中可以自动调整图形在页面中的位置。
“图形说明”会出现在图形下方。

图形大小有默认值。
可以在源文件或者整个项目中每个输出格式的设置下面，
用`fig-width`和`fig-height`设置图形的英寸为单位的宽度和高度，
也可以作为代码段选项设置。
图形可以分为以下两种：

* 矢量图，这种图形可以任意缩放而不会产生锯齿效果，
  代表是PDF，缺点是可能受汉字编码影响出现乱码，
  当图形中数据量过大时显示变慢；
* 点阵图，过度放大会产生模糊或者锯齿，
  代表是PNG和JPEG。

Quarto中可以设置的图形大小，
实际上有两个含义：

* 图形的显示占用页面大小；
* 图形内容的大小。

如果是点阵图，
点阵越大，图形内容越大，或者分辨率越高。

代码段关于图形大小、分辨率的选项包括：

* `fig-width`, `fig-height`: 以英寸为单位的图形宽度、高度。
  这两个值越大，图形的尺寸越大，
  分辨率越高；
* `fig-asp`：高宽比，
  如`fig-asp: 0.75`。
  这样可以仅规定`fig-width`，
  用`fig-asp`推算高度。
* `out-width`: 显示出的图片占页面行宽的比例，
  如`out-width: 80%`。
  同时可以设置`fig-align: center`使得图片在行宽范围内居中放置。
  如果有并排放置的图片，
  可以用`out-width: 45%`，
  并设置`fig-align: default`。
* `fig-format`: 可设置为`png`。
  当输出为PDF格式时程序生成的图形文件默认使用`pdf`格式，
  可以用这个选项设置为PNG格式。

`out-width`设置了显示宽度，
而`fig-width`实际会影响到图片分辨率。
在用程序生成的图形中包含较多文字内容时，
可以设置较大的`fig-width`使得实际分辨率较高，
这样文字内容不至于混在一起。

## 22.9 插入表格

仍可以用markdown的方法插入表格，
参见§[21.3.10](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/markdown.html#markdown-format-table)。
在表格的标题行末尾空格后添加如“`{#tbl-mytable}`”这样的标签，
就可以产生可交叉引用的编号表格，
用“`[@tbl-mytable]`”这样的格式引用，
显示如“表2.1”。
如果仅需要产生引用数字，
可用格式“`[-@tbl-mytable]`”。

如果使用RStudio编辑，
可以打开可视化编辑模式(Visual模式)，
选择插入表格，
并用可见即所得方式输入表格。

可以将表格输入为R数据框，
然后用`knitr::kable()`函数插入表格。
在插入表格的代码段中，
定义代码段选项`label`为一个标签，
以`tbl-`开头，
如`tbl-mytable`，
定义代码段`tbl-cap`为表格要显示的说明文字，
就可以生成可交叉引用的表格，
引用格式如`[@tbl-mytable]`，
并且在PDF输出中是浮动表格。

插入的代码段例子如：

```
```{r, echo=FALSE}
#| label: tbl-mytable
#| tbl-cap: "汽车数据"
knitr::kable(mccars[1:10, 1:5])
```
```

`knitr::kable()`函数本身有一些选项，
如`digits`规定小数点后的位数。

支持并排放置的表格，
详见Quarto的文档。

[gt](https://gt.rstudio.com/)是一个制表用的R扩展包，
支持从数据框转换生成比较复杂的表格，
支持HTML、LaTeX、RTF输出。

## 22.10 数学公式

### 22.10.1 在Markdown中输入数学公式

原始的Markdown格式并不支持数学公式。
Pandoc扩展的markdown格式提供了对数学公式的支持，
可以在Markdown文件中插入LaTeX格式的数学公式。
虽然不能提供所有的LaTeX公式能力，
但是常用的数学公式还是能做得很好，
转换到HTML、docx都可以得到正常显示的公式。

用RStudio软件编译Markdown文件，
可以在其中插入LaTeX格式的数学公式，
数学公式可以在编辑器内部显示预览，
编译成HTML或者docx格式后都可以正常显示数学公式,
HTML结果可以直接利用RStudio内部的浏览器预览，
在另外安装的LaTeX编译器的支持下也可以将.qmd格式编译LaTeX格式然后再转换为PDF格式，
这种基于LaTeX的方法对数学公式的支持会更完善。

`.qmd`和`.ipynb`文件在Quarto软件支持下都可以使用LaTeX数学公式。

### 22.10.2 数学公式类别

数学公式公式分为行内公式和独立公式。
行内公式和段落的文字混排，
写在两个美元符号`$`中间，或者`\(`和`\)`之间。
例如`$f(x)=\frac{1}{2} \int_0^1 \sin^2 (t x) dt$`变成
\(f(x)=\frac{1}{2} \int\_0^1 \sin^2 (t x) dt\)。
开头的`$`后面不能紧跟着空格，
结尾的`$`不能紧跟在空格后面。

独立公式写在成对的美元符号中间，或者`\[`和`\]`之间。
例如：

```
$$
  f(x) = \frac{1}{2} \sum_{j=1}^\infty \int_0^1 \sin^2(j t x) dt .
$$
```

显示为

\[
f(x) = \frac{1}{2} \sum\_{j=1}^\infty \int\_0^1 \sin^2(j t x) dt .
\]

### 22.10.3 基本功能

在数学公式中，
用下划线表示下标，
比如`$x_1$`结果为\(x\_1\)。
用`^`表示上标，
如`$x^2$`结果为\(x^2\)。
上下标都有，如`$x_1^2$`结果为\(x\_1^2\)。

公式中用大括号表示一个整体，
比如`$x^(1)$`不能得到\(x^{(1)}\)，
需要用`$x^{(1)}$`。

公式中用反斜杠开始一个命令，
命令仅包含字母而不能包含数字，
数字只能作为参数。
如`$\frac{1}{2}$`和`$\frac12$`都可以生成\(\frac{1}{2}\)。
类似的命令如`$\sqrt{2}$`为\(\sqrt{2}\)。

希腊字母都有对应的命令，
如`$\alpha$`为\(\alpha\)，
`$\Sigma$`为\(\Sigma\)。

常用数学函数有自己的命令，
如`$\sin x$`变成\(\sin x\)，
`$\exp (x)$`为\(\exp (x)\)。

注意，在公式中使用单词、缩写时，
要写在`\text{}`内，
使其显示为正体，
而非表示数学符号的斜体。
比如，变量ht表示以厘米为单位的身高，
转换为以米为单位的变量\(y\)，正确的做法为：

```
$$
  y = \text{ht} / 100 .
$$
```

显示为：
\[
y = \text{ht} / 100 .
\]

如果不使用`\text{}`保护，
则会显示为：
\[
y = ht / 100 .
\]
这在数学公式解释上，一般会解释为
\[
y = h \times t / 100 .
\]

圆括号与方括号可以直接使用，
绝对值符号用`|`，
如`$|x|$`变成\(|x|\)。
但是，大括号必须写成`\{`和`\}`的形式，
如`$\exp\{-\frac12 x^2\}$`变成\(\exp\{-\frac12 x^2\}\)。

为了使得括号能够与括号内部的内容等高，
可以用`\left`和`\right`命令进行修饰，
如

```
$$
\phi(x) = \frac{1}{\sqrt{2\pi}} \exp\left\{ \frac{1}{2} x^2 \right\}
$$
```

变成
\[
\phi(x) = \frac{1}{\sqrt{2\pi}} \exp\left\{ \frac{1}{2} x^2 \right\}
\]
注意`\left`与`\right`必须成对使用，否则出错。

求和如`$\sum_{i=1}^n x_i$`变成\(\sum\_{i=1}^n x\_i\)。
乘积如`$\prod_{i=1}^n x_i$`变成\(\prod\_{i=1}^n x\_i\)。
积分如`$\int_0^1 f(x) dx$`变成\(\int\_0^1 f(x) dx\)。

### 22.10.4 修饰符

`$f'(x)$`显示为\(f'(x)\)，
表示导数；
`$f''(x)$`显示为\(f''(x)\)，
表示二阶导数。
顺便说一句，
偏导数写法如`$\frac{\partial f(x,t)}{\partial x}$`，
显示为\(\frac{\partial f(x,t)}{\partial x}\)。

\(\bar{x}\)的写法是`$\bar{x}$`。
\(\overline{\text{span}}\)的写法是`$\overline{\text{span}}$`。
\(\hat{x}\)的写法是`$\hat{x}$`。
\(\tilde{x}\)的写法是`$\tilde{x}$`。
\(\vec{x}\)的写法是`$\vec{x}$`。

### 22.10.5 对齐与矩阵

为了产生对齐的公式，
在独立公式中使用aligned环境。
公式中的环境以`\begin{环境名}`开始，
以`\end{环境名}`结束，
用`\\`表示换行，用`&`表示一个上下对齐位置。
如

```
$$\begin{aligned}
  f(x) =& \sum_{k=0}^\infty \frac{1}{k!} x^k \\
  =& e^x
\end{aligned}$$
```

转化成
\[
\begin{aligned}
f(x) =& \sum\_{k=0}^\infty \frac{1}{k!} x^k \\
=& e^x
\end{aligned}
\]

可以用pmatrix环境制作写在圆括号中的矩阵，
用bmatrix环境制作写在方括号中的矩阵，
用vmatrix制作写在绝对值号中的矩阵，如

```
$$
\begin{pmatrix}
x_{11} & x_{12} \\
x_{21} & x_{22}
\end{pmatrix}
$$
```

变成
\[
\begin{pmatrix}
x\_{11} & x\_{12} \\
x\_{21} & x\_{22}
\end{pmatrix}
\]
列向量、行向量也可以用这种办法制作。

矩阵\(A\)的转置可以写成`$A^T$`，显示为\(A^T\)；
也可以写成`$A^\top$`，显示为\(A^\top\)。

### 22.10.6 特殊字体

有些教材用粗体表示向量或者矩阵，
办法是用`\boldsymbol{...}`说明。如

```
$$
  \boldsymbol{v} = (v_1, v_2)^T .
$$
```

变成
\[
\boldsymbol{v} = (v\_1, v\_2)^T .
\]

美术体英文字母用`\mathcal{...}`，
如\(\mathcal{A, B, C}\)写法为`$\mathcal{A}, \mathcal{B}, \mathcal{C}$`。
数学中常用来表示集合类。

手写花体字母用`\mathscr{...}`，
如\(\mathscr{B, C, F, G}\)写法为`$\mathscr{B}, \mathscr{C}, \mathscr{F}, \mathscr{G}$`。
在测度论、概率论中经常用来表示σ代数。

空心字体用`\mathbb{...}`，
如\(\mathbb{R, C}\)写法为`$\mathbb{R}, \mathbb{C}$`
数学中经常用来表示整数集、实数域、复数域等。

### 22.10.7 数学公式引用

使用LaTeX制作科技论文时，
一个重要的功能是给数学公式贴上标签，
并使用标签引用公式，
编译时自动生成公式编号并引用编号。
Quarto支持这样的功能。

在独立公式的`$$`后面，
用`{#eq-mylabel}`的方式加公式标签，
引用时用“`(@eq-mylabel)`”格式引用。

需要的情况下可以在YAML中加选项：

```
crossref:
  eq-prefix: ""
```

使得公式引用仅包含编号，
如`(2.1)`，
否则可能会变成`(方程式2.1)`。
也将引用写成如“`(-@eq-mylabel)`”。

目前不支持多行公式中的多个编号。

### 22.10.8 数学公式的显示设置

当Quarto的转换目标为HTML时，
数学公式默认使用MathJax库显示。
这是一个Javascript函数库，
从远程访问使用，
有很强的数学公式显示能力，
但断网时会无法显示。

另一种HTML格式的数学公式显示方式是用KATEX显示，
显示效果也很优秀。
为了使用KATEX，
可以在YAML的`format--html`设置部分，
增加如下设置：

```
format:
  html:
    html-math-method: katex
```

## 22.11 设置

### 22.11.1 使用单个源文件

只有一个源文件时，
所有的设置都放在文件头部的YAML块中，
如：

```
---
title: "作业框架"
author: "李东风"
date: "2023-07-27"
lang: zh
format:
  html:
    toc: true
    toc-location: body
    toc-depth: 3
    number-sections: true
    html-math-method: katex
  docx:
    toc: true
    toc-depth: 3
    number-sections: true
  pdf:
    documentclass: article
    toc: true
    toc-depth: 3
    include-in-header: 
      text: |
        \usepackage{ctex}
        \usepackage{amsthm,mathrsfs}
---
```

在设置中，`number-sections: true`表示自动生成章节的编号并显示编号。
`toc: true`表示要自动生成目录，
`toc-depth: 3`表示目录内容包含到小节级别。

在`format`下的`pdf`部分是先转换为.tex文件，
再用单独安装的TinyTeX软件转换为PDF的运行设置。
设置中调用了ctex包，
这时中文内容所需要的，
如果没有这个设置，
产生的PDF可能无法显示中文内容。

### 22.11.2 生成图书

如果用多个源文件同时构成一本书或网站，
应该在项目目录中添加一个“`_quarto.yml`”文件，
将关于书（网站）的设置都放在这个文件中。

为了用Quarto格式写书，
应该在RStudio中用新建项目方式自动生成新文件夹，
并选择Quarto book格式。
这会自动生成文件夹和主要文件框架，
其中`index.qmd`是主控文件，
`_quarto.yml`是主要的设置文件。
`_quarto.yml`中会包括如下设置：

```
project:
  type: book
```

这说明整个项目的目标是生成一本书，
书的输出结果一般包括网站形式和PDF形式。

仍用`title`, `author`, `date`给出书名、作者、日期，
如：

```
title: "统计计算"
author: "李东风"
date: "2022-11-28"
```

其中日期如果想动态填入编译时的日期，
可以用：

```
title: "统计计算"
author: "李东风"
date: !r lubridate::today()
```

### 22.11.3 图书章节结构

章节结构最简单的情况是仅有若干章，
这时在`_quarto.qml`中，
写在`book:`下面如下的内容：

```
book:
  chapters:
    - index.qmd
    - intro.qmd
    - summary.qmd
    - references.qmd
```

如果有部分，
有附录，结构如：

```
book:
  chapters:
    - index.qmd
    - preface.qmd
    - part: "基础知识"
      chapters:
        - elem.qmd
        - linear.qmd
    - part: "进阶知识"
      chapters:
        - nonlin.qmd
        - solve.qmd
    - appendices
      - summary.qmd
      - references.qmd
```

注意“部分”下面的章需要用`chapters`作为父节点，
而`appendices`下面则直接列出各个附录文件。

### 22.11.4 多种输出格式

在单个源文件或者图书的`_quarto.yml`文件中，
可以设置`format`项，
在其下层列出要生成的输出格式类型，
以及专用于该输出格式的选项。
比如在`_quarto.yml`中：

```
format:
  html:
    toc-float: true
    html-math-method: katex
  pdf:
    documentclass: scrbook
  docx: default
```

其中`default`表示使用默认设置，
但是不能仅列出输出格式而没有任何设置。

### 22.11.5 目录

在`_quarto.qml`中用`toc: true`指定对所有输出格式都自动生成目录。
用`toc-depth: 3`指定目录中显示内容的层级，3是显示到小节层次。
对于HTML输出，
用`toc-location: right`指定目录单独放在正文右侧，
用`toc-location: body`指定目录单独放在正文内部开头处。

在`_quarto.qml`中用`number-sections: true`使得所有输出格式都有章节编号数字。
不需要编号的章节，在章节标题内容后面空格后写`{.unnumbered}`。

### 22.11.6 中文支持

中文内容，需要指定在`_quarto.qml`的顶级设置中指定`lang: zh`。
默认的中文翻译有些不尽合理，
可以从<https://github.com/quarto-dev/quarto-cli/tree/main/src/resources/language/>下载`_language-zh.yml`，
修改成需要的样子，
改名为`_language.yml`，
放在自己的项目目录中。
这时不需要使用`lang: zh`选项。

为了使得LaTeX输出的PDF支持中文，
必须在YAML的`format--pdf--include-in-header`中指定`\usepakage{ctex}`，
如：

```
format:
  pdf:
    documentclass: scrbook
    include-in-header: 
      text: |
        \usepackage{ctex}
        \usepackage{amsthm,mathrsfs}
```

注意其中的`text:`项，这用来指定具体的插入内容，而不是插入文件。
也可以将这些LaTeX头文件中需要的内容写在一个`preamble.tex`文件中，
这时就可以写成`include-in-header: preamble.tex`。

另外一种办法是直接指定使用的中文字体，
这种方法因为仅指定一种字体，
效果不太美观。
在安装了LaTeX软件如TinyTeX后，
在Windows的命令行窗口(cmd)，
运行如下命令：

```
fc-list :lang=zh > tmp.txt
```

则产生一个`tmp.txt`文件，
其中包含了可用字体的列表，
比如`NSimSun`代表“新宋体”。
在YAML中设置`format -- pdf -- mainfont: "NSimSun"`可以使得通过LaTeX生成的PDF支持中文内容。

### 22.11.7 参考文献数据库设置

在在`_quarto.yml`中可以设置一个或多个文献数据库，
会自动生成参考文献列表，如：

```
bibliography: ["myref1.bib", "myref2.bib"]
```

这些`.bib`文件目前需要放在项目目录中，
不支持用绝对路径或相对路径。

可以用[Jabref软件](https://www.jabref.org/)管理文献数据库。
这是一个Java程序，
可以很容易地编辑文献数据库。

如果使用RStudio对Quarto源文件的可视化编辑模式，
还支持利用DOI等方法加入参考文献引用，
这时RStudio自动添加一个名为“`bibliography.bib`”，
将这些引用保存在里面。

关于`.bib`文件的格式，
可以参考一些讲LaTeX用法的书。
一个典型的.bib文件如：

```
@Book{MWP06-HighStat,
  author    = {茆诗松 and 王静龙 and 濮晓龙},
  title     = {高等数理统计},
  year      = {2006},
  edition   = {第二版},
  publisher = {高等教育出版社}
}
@ARTICLE{Ansley90,
  author = {Ansley, C.F. and Kohn, R.},
  title = {Filtering and smoothing in state space 
  models with partially diffuse
    initial conditions},
  journal = {J. Time Series Analysis},
  year = {1990},
  volume = {11},
  pages = {275--93}
}
```

引用方法如“`[@Ansley90]`”。
多个文献引用如“`[参见 @Ansly90; @MWP06-HighStat pp 101-103]`”。
注意“`@文献标签`”前后不允许直接使用汉字和中文标点。

### 22.11.8 其它选项

#### 22.11.8.1 编辑模式选择

为了在RStudio中设置总是使用源代码编辑模式，
可以在`_quarto.yml`中加入：

```
editor: source
```

#### 22.11.8.2 HTML自包含

生成HTML结果时，
会有图片、CSS等文件伴随HTML文件，
如果需要放在网站，
可以将这些文件同时放在网站上。
如果是单个源文件，
希望生成的HTML文件不要依赖于外部文件，
可以在源文件的YAML内容中添加如下内容：

```
format:
  html:
    embed-resources: true
```

## 22.12 定理类

定理：使用`:::{#thm-label} ... :::`的方案，
其中`...`可以使用各种内容段落。定理自动在章内编号，
引用如`参见[@thm-label]`，
显示如“参见定理2.1”。
如果引用时仅希望生成引用数字，
可用如“`参见定理[-@thm-label]`”。

可以加一个`name="..."`定理名称。
因为不同章在不同文件中，
但是标签是统一的，
所以不同的章也需要使用不同的标签。

定理的标题可以用二级标题在内容中表示。
例如：

```
:::{#thm-lebesgue01 name="勒贝格定理"}
一元函数黎曼可积，
当且仅当其不连续点的集合为零测集。
:::

引用如：见[@thm-lebesgue01]。
```

表22.1:  定理类段落

| 环境名 | 默认显示名 | 标签前缀 |
| --- | --- | --- |
| theorem | Theorem | #thm- |
| lemma | Lemma | #lem- |
| corollary | Corollary | #cor- |
| proposition | Proposition | #prp- |
| conjecture | Conjecture | #cnj- |
| definition | Definition | #def- |
| example | Example | #exm- |
| exercise | Exercise | #exr- |

另外还提供了proof, remark, solution环境，
这些环境不提供编号和交叉索引功能，
没有必要使用。
使用的格式如`::: {.proof} ... :::`。

## 22.13 引用

### 22.13.1 引用章节

为了引用章节，
在章节标题空格后写`{#sec-label}`，
其中`label`是全书都唯一的标签。
引用如“`参见[@sec-label]`”。
如果仅希望生成引用数字，
可用如“`参见第[-@sec-label]章`”。

### 22.13.2 可引用插图和表格

可引用插图示例：

```
#| label: fig-test-fig01
#| fig-cap: "测试图形编号"

plot(1:10)
```

```
参见[@fig-test-fig01]。
```

注意代码开始注释不要忘记`#`号后面的竖线符号`|`。
`label`的内容必须以`fig-`开头。
后面的选项是`fig-cap`而不是`fig-caption`。

### 22.13.3 图、表、交叉引用定制

在引用图、表、公式、章节时，
默认的引用格式不一定合适，
可以用上述修改`_language.yml`的方法，
也可以在`_quarto.yml`中进行设置，如：

```
crossref:
  eq-prefix: ""
  sec-prefix: "§"
```

这里`eq-prefix`设置了在引用公式时，
`(@eq-mylabel)`的编译结果如(1.1)，
而不是(公式1.1)。
`sec-prefix`使得在引用某一节时，
使用适当的格式如“§1.1”。

## 22.14 代码段缓存

R代码段支持缓存，
使得已经花比较长时间运行得到的结果不必每次都重复运行程序。
为了所有文件中的R代码段都缓存，
可以在`_quarto.qmd`中顶层设置“`execute -- cache`”为“`true`”，
如：

```
execute:
  cache: true
```

为了使得某一段可缓存，
可以在代码段开头设置“`#| cache: true`”。

设置了缓存的代码段，
其运行结果会被保存下来，
仅该代码段的程序被修改时才重新运行。
但是，
如果B代码段依赖于前面的A代码段的结果，
而A代码段被修改了，
则B也应该重新运行，
这并不能自动识别。
这可以靠`dependson`选项和`cache-extra`选项建立依赖关系。

例如，
代码段A读入原始数据：

```
```{r}
#| label: code-A
#| cache: true
da0 <- read.csv("myinput.csv")
```
```

代码段B处理读入的数据产生建模用数据，
使用缓存保存生成的数据，
用`dependson`选项令其依赖于代码段A：

```
```{r}
#| label: code-B
#| cache: true
#| dependson: code-A
da <- da0[,c("weight", "height")]
```
```

这样，
当代码段A的程序被修改时，
代码段B也会重新运行。
但是，
如果代码段A输入的原始数据“`myinput.csv`”的内容被修改了，
这里还是无法识别这种修改，
代码段B不会重新运行，
导致数据`da`使用过时的数据。
解决方法是，
在代码段A中增加`cache-extra`选项，
指定当输入文件的信息改变时就对依赖于代码A的代码段发出缓存更新提示。
如：

```
```{r}
#| label: code-A
#| cache: true
#| cache-extra: file-info("myinput.csv")
da0 <- read.csv("myinput.csv")
```
```

人为地管理这些依赖关系是比较繁琐易错的，
所以非必要时不要使用这种缓存的办法。
另一种管理不同的代码模块之间的运行结果依赖关系的方法是使用target包，
参见[20.5](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/knitr.html#knitr-targets)。

当怀疑缓存陈旧时，
可以在R中运行如下命令清除缓存：

```
knitr::clean_cache()
```

对单个文件，可以用quarto命令如下刷新缓存：

```
quarto render file1.qmd --cache-refresh
```

对于整个项目，
刷新缓存命令为

```
quarto render --cache-refresh
```

## 22.15 编译管理

在RStudio中可以用`Preview Book`或`Render Book`将整本书编译为输出格式。
当项目较大或者其中的程序运行时间较长时，
可以应用一些技巧减少花费的时间。

### 22.15.1 增量转换

用`quarto`命令转换时，
可以仅指定一个文件或一个子目录进行转换。
这时，
项目中的其它文件使用原来已经转换过的结果。

在RStudio中，
可以用`Preview Book`打开一个动态更新的预览窗口，
这样，
在用`Render`重新转换单个文件时可以自动显示新的结果。
如果选择了“Knit on Save”也可以在保存文件时自动显示新的结果。

### 22.15.2 冻结版本

可以使用YAML选项`execute -- freeze: true`要求在编译整个项目时，
不运行文件中的源程序。
将`true`改为`auto`，
则意味着仅在源文件改变时才运行其中的源程序。
而编译单个文件或单个子目录时还是会运行文件中的源程序。

使用冻结功能要谨慎，
这个功能与使用缓存类似，
可能会使得输出结果使用旧版本的数据或程序。
这些冻结结果保存在`_freeze`子目录中，
可以删除这个子目录，
强迫重新计算结果。

## 22.16 制作演示文稿

Quarto格式可以转换为演示文稿，
支持如下形式的演示文稿：

* revealjs: 这是网页格式的演示文稿，
  功能比较强；
* pptx: 这是MS Power Point格式的演示文稿；
* beamer: 这是LaTeX的Beamer演示文稿。

演示文稿分为多个帧(frame)，
每帧用二级标题作为标志并以其为标题。
用一级标题作为单独的分节帧，
将单独显示在一帧中。

帧也可以没有标题，比如仅有照片的帧，
这时，
用三个或三个以上的减号连在一起标识新帧的开始。

演示文稿中的文字内容常用列表格式，
也支持R程序代码段。

一个简单的revealjs格式演示文稿源文件example-rj.qmd，
内容如：

```
---
title: "Quarto revealjs演示文稿样例"
author: "李东风"
date: "2022-12-31"
format: revealjs
---
```

```
# 用Quarto的revealjs格式输出制作演示

## 幻灯片结构

- 用二级标题标志一个页面开始
- 用一级标题制作单独的分节页面
- 用三个或三个以上减号标志没有标题的页面开始
- 每个页面一般用markdown列表显示若干个项目

## 幻灯片编辑和编译

- 用RStudio编辑内容
- 用RStudio的Render按钮生成结果

-----

![一个演示画面](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/bd_logo.png)


## 数学公式

$$\begin{aligned}
  \sum_{k=1}^n k^2 =& \frac{1}{6} n (n+1) (2n+1) , \\
  \sum_{k=1}^n k^3 =& \left(\frac{1}{2} n (n+1) \right)^2 .
\end{aligned}$$


## 代码段和结果

```{r}
x <- 1:10
y <- x^2
plot(x, y)
```
```

在播放时，
可以用向下或向右方向键或空格键翻到下一帧，
用向上或向左方向键退回到上一帧，
可以点击左下角的图表展开目录或者菜单，
可以用F键切换成全屏显示，
在全屏显示时用Esc键退回到正常显示。

如果需要一帧中的列表逐项显示（每按一次向下光标键显示一项），
可以写成如下格式：

```
## 幻灯片结构

::: {.incremental}

- 用二级标题标志一个页面开始
- 用一级标题制作单独的分节页面
- 用三个或三个以上减号标志没有标题的页面开始
- 每个页面一般用markdown列表显示若干个项目

:::
```

如果希望每一帧的列表都逐项显示，
可以在源文件开头的YAML设置中，
增加：

```
format:
  revealjs:
    incremental: true
```

为了增加PowerPoint格式的输出，
可以将YAML的format部分改为：

```
format:
  revealjs: default
  pptx: default
```

## 22.17 运行Julia程序

Qmd文件中除了R代码段以外，
还可以插入Rcpp、Python、Julia、SQL等许多编程语言的代码段，
常用编程语言还可以与R代码段进行信息交换。
在Jupyter软件支持下直接支持将.ipynb格式的文件转换为各种输出格式。

为了在.qmd文件中运行Julia程序并使用其结果，
需要在操作系统PATH环境变量中正确设置所用Julia可执行的路径。

## 22.18 用作科学计算笔记本

Quarto的.qmd格式与Jupyter笔记本格式，
都很适用于当作一个科学研究时的计算分析笔记本使用。
这个笔记本可以记录自己的想法，
研究过程，
成功与失败，
其中的计算，
包括计算程序和结果。

当作计算分析笔记本时，
有如下建议：

* 应该包括一个有意义的标题，
  并使用有意义的文件名保存。
* 应该用日期属性表明写作日期或者最后修改日期。
* 内容的第一段应该对研究进行简要说明。
* 研究中失败的研究内容也不要删除，
  而是说明原因，避免自己和读者再次犯类似错误。
* 一般不要将数据包含在笔记本源文件中，
  仅在数据量很小时，
  可以写在代码段中。
* 在一个研究项目中，
  如果数据有错误，
  一般不应该直接修改数据文件，
  而是在笔记本源文件中用程序修改并注明。
* 使用一个笔记本源文件连续进行研究时，
  最好每天结束时清除缓存，
  用render功能生成一个版本。
* 持续较长时间的研究，
  可能会受到R扩展包版本变化的影响。
  较正规的解决思路是使用renv扩展包，
  将需要的扩展包变成项目私有扩展包。
* 因为Quarto很好用，
  你的多个研究项目都可以使用，
  随着时间累积出许多笔记本源文件。
  最好将这些研究用“项目”表达，
  并使用合适的文件夹名，
  进行合理的层次组织。

## 22.19 问题

* 多行数学公式只能使用一个编号。
* 参考文献只能放在项目文件夹而不能使用相对或绝对路径。
* 在定理环境中，使用列表会使得定理本身进入列表。
* RStudio中设置为visual编辑方式后，
  会自动调整qmd文件的空白，
  使得切换回sourse编辑方式后内容编排很不习惯。
  这主要是有数学公式时不方便，
  如果仅文字内容倒是没有关系。

## 22.20 附录：TinyTex的安装使用

### 22.20.1 基于Quarto可执行程序

如果不使用Rmarkdown、bookdown，
仅使用Quarto格式，
而且安装了单独的Quarto可执行程序，
可以在操作系统命令行运行如下安装TinyTeX的命令：

```
quarto install tool tinytex
```

这种安装方式不更新操作系统的PATH环境变量，
仅供Quarto本身使用，
从而不会影响到操作系统中已经安装的其它LaTeX编译环境。
Quarto在编译PDF时遇到缺少的格式文件可以自动下载安装。

### 22.20.2 基于RStudio

如果要在rmarkdown、bookdown中使用PDF输出功能，
可以在在R中安装tinytex扩展包并安装TinyTeX编译软件：

```
install.packages('tinytex')
tinytex::tlmgr_repo('http://mirrors.tuna.tsinghua.edu.cn/CTAN/')
tinytex::install_tinytex()
```

其中上面第一行命令安装R的tinytex扩展包，
第二行将下载LaTeX编译程序的服务器设置为清华大学tuna镜像站，
第三行安装LaTeX编译程序。

如果安装成功，
TinyTeX软件包在MS Windows系统中一般会安装在
`C:\Users\用户名\AppData\Roaming\TinyTeX`目录中，
其中“用户名”应替换成系统当前用户名。
如果需要删除TinyTeX软件包，
只要直接删除那个子目录就可以。

为了判断TinyTeX是否安装成功，
在RStudio中运行

```
tinytex::is_tinytex()
```

结果应为`TRUE`,
出错或者结果为`FALSE`都说明安装不成功。

当用户使用RMarkdown和tinytex包转换latex并编译为PDF时，
如果缺少某些latex宏包，
tinytex会自动安装缺少的宏包。