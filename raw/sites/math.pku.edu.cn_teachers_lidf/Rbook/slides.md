---
crawl_time: '2026-01-17 14:19:57'
framework: rbook
title: D 制作幻灯片 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/slides.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# D 制作幻灯片

## D.1 介绍

R Markdown文件(.Rmd)文件支持多种输出，
如网页(`html_document`)、MS Word(`word_document`)、PDF(`pdf_document`, 需要LaTeX编译器支持)等，
还支持生成网页格式的幻灯片(`slidy_presentation`, `ioslides_presentation`)，
以及LaTeX beamer格式的PDF幻灯片(`beamer_presentation`)，
和Microsoft Office的PowerPoint幻灯片(`powerpoint_presentation`)格式。

## D.2 Slidy幻灯片

Rmd文件选用输出格式`slidy_presentation`可以生成网页格式的幻灯片，
并具有缩放字体大小、显示幻灯片目录等功能。
只要在.Rmd文件开头的YAML元数据部分指定`output: slidy_presentation`。
因为幻灯片的单位是帧(frame)，
与论文的结构有很大区别，
所以幻灯片Rmd文件很难同时作为论文的源文件。

### D.2.1 文件格式

幻灯片分为多个帧，每帧用二级标题作为标志并以其为标题。
二级标题就是行首以两个井号和空格开始的行，
或者在标题下面画由减号组成的线的行。
用一级标题作为单独的分节帧，
将单独显示在一帧中。
一级标题是行首以一个井号和空格开始的行，
或者在标题下面画由等于号组成的线的行。

帧也可以没有标题，比如仅有照片的帧，
这时，
用三个或三个以上的减号连在一起标识新帧的开始。

一个简单的slidy\_presentation幻灯片源文件example-slidy.Rmd，
内容如：

```
---
title: "R幻灯片演示样例"
author: "李东风"
date: "2017-11-16"
output: slidy_presentation
---
```

```
# 用R Markdown的slidy输出作幻灯片

## 幻灯片结构

- 用二级标题标志一个页面开始
- 用一级标题制作单独的分节页面
- 用三个或三个以上减号标志没有标题的页面开始
- 每个页面一般用markdown列表显示若干个项目

## 幻灯片编译

- 用RStudio编辑
- 用RStudio的Knit按钮，选`slidy_presentation`作为输出格式

-----

[一个演示画面](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/demoscreen.png)
```

### D.2.2 幻灯片编译

幻灯片用RStudio的knit按钮编译，
选择输出格式为`slidy_presentation`，
结果在浏览器中播放。

除了使用knit按钮，还可以用类似如下命令：

```
rmarkdown::render("mydemo.Rmd", output_format = "slidy_presentation", encoding="UTF-8")
```

其中`mydemo.Rmd`是源文件。

为了制作幻灯片，最好单独设置一个RStudio项目，
并且此项目仅生成幻灯片，
而不生成普通网页、Word、PDF等输出，
否则可能造成结果混乱。
希望rmarkdown包的后续版本能取消这个限制。

### D.2.3 播放控制

播放时，用如下方式控制：

* 鼠标左键单击、光标右移键、向下翻页键、空格键都可以翻到下一页；
* 光标左移键、向上翻页键回退一页；
* 单击下方的Contents或单击C键显示幻灯片目录列表，可单击转移到任意页面；
* Home键回到幻灯片开头；
* 用A键切换是否将所有页面合并显示成一个长的网页；
* 用S键缩写字体，用B键放大字体；
* 通过选择保存为PDF的打印机进行打印，
  可以将幻灯片转换为PDF，
  但是打印生成的PDF仍是每页仅有原来的一帧，
  所以转换成一个单页的HTML再打印可能更合适。

### D.2.4 生成单页HTML

对slidy幻灯片的Rmd源文件不加修改，
也可以通过命令直接转换为普通的单页HTML文件，
但是因为在slidy幻灯片源文件中二级标题用来分帧，
而普通Rmd文件中二级标题用来分小节，
所以生成的单页HTML文件会有许多小节。
这样的文件更适合打印以及转换为PDF文件。

命令如：

```
rmarkdown::render("mydemo.Rmd", output_file="handout.html", output_format="html_document", encoding="UTF-8")
```

### D.2.5 数学公式处理与输出设置文件

讲课用的幻灯片经常会有数学公式，
比较关键的问题是数学公式如何处理。
网页中的数学公式一般使用一个公开的自由Javascript库MathJax显示，
但是这个库很大，
如果使用远程的库，
在网络不畅通时显示公式就不正常。
更好的办法是使用局部的MathJax库或将MathJax库安装在临近的网站服务器上。

为了使用局部的MathJax库，
简单的办法是在YAML的`ioslides_presentation`项目下面指定`mathjax: local`，
如：

```
---
title: "R幻灯片演示样例"
output: 
  slidy_presentation:
    mathjax: local
---
```

上述办法容易使用，
缺点是多个不同的演示项目无法共用一个局部的MathJax库，
生成的结果包含了许多小的支持文件。

为此，
可以将MathJax库装在演示项目所在目录的上层（比如上两层的`MathJax`目录内），
将设置MathJax的代码放在`_header.html`文件中，
`_header.html`中的内容如：

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
```

在演示项目所在目录中增加一个`_output.yml`文件，
这使得该项目所有输出的共用输出设置，内容如：

```
slidy_presentation: 
    toc: false
    mathjax: "../../MathJax/MathJax.js"
    self_contained: false
    includes:
      in_header: "_header.html"
html_document: 
    toc: true
    number_sections: false
    mathjax: "../../MathJax/MathJax.js"
    self_contained: false
    includes:
      in_header: "_header.html"
```

这里关闭了`self_contained`选项，
设置了MathJax库在本地目录，具体是演示项目所在目录上面两层的MathJax目录中。

公式中如果有中文，
开始时可能显示不正常，
这时右键点击公式弹出菜单选择“Math Settings–Math Renderer”，
取为“HTML-CSS”或“SVG”应可解决问题。

上面的输出设置文件中也设置了输出`html_document`，
这是单页的HTML格式，
取消了自动章节编号，
因为源文件中每帧都是一个小节。

### D.2.6 其它选项

用`slidy_presentation`的`font_adjustment: -1`可以缩小字体一号，
类似可以设为`+1`，`-2`等。
在播放时也可以用S和B键缩小或放大显示。

可以在播放时在浏览器状态栏显示倒数计时器，
用`slidy_presentation`的`duration: 5`表示每帧显示5分钟。
这是总的预计时间，
适用于演讲有时间限制的情况。

可以用`footer`属性指定每帧都显示的状态栏脚注，如：

```
---
output:
  slidy_presentation:
    font_adjustment: -1
    duration: 5
    footer: "北京大学"
---
```

`slidy_presentation`输出属性`incremental: true`可以使得列表显示需要每点击一次才显示下一项。

### D.2.7 slidy幻灯片激光笔失效问题的修改

slidy幻灯片翻页是用空格、左右光标、上下翻页键，
而一般激光笔翻页是模拟上下光标键。
为此，
在安装的R软件目录的
`library/rmarkdown/rmd/slidy/Slidy2/scripts`子目录中，
找到slidy.js文件，
用编辑器打开，
用编辑器的搜索功能搜索`key == 37`，
将其替换成`key == 37 || key == 38`，
这里37是向左光标的编码，
替换后就是向左或者向上光标。
用编辑器的搜索功能搜索`key == 39`,
将其替换成`key == 39 || key == 40`，
这里39是向右光标的编码，
替换后就是向右或者向上光标。
修改完毕后保存，
然后将slidy.js用文件压缩程序（如7zip）压缩为slidy.js.gz。
这样就可以在用rmarkdown制作的`slidy_presentation`结果中支持激光笔翻页了。

这样修改后，
如果某帧超高，
需要滚动显示，
就只能通过鼠标滚轮滚动了。

## D.3 MS PowerPoint幻灯片

`powerpoint_presentation`输出格式可以生成MS PowerPoint文件。
设置如：

```
---
output:
  powerpoint_presentation:
    slide_level: 2
---
```

可以用`powerpoint_presentation`的`reference_doc`属性指向一个.pptx的文件，
作为模板，
模板中的样式将被输出结果采用。

可以用Rstudio的Knit快捷图标实现转换（选其中的knit to PowerPoint），
或者用如下命令：

```
rmarkdown::render("slides.Rmd", output_format="powerpoint_presentation", encoding="UTF-8")
```

如果从bookdown内容转化成演示幻灯片，
需要大量修改原始文件内容，
设置成幻灯片常用的逐条播放格式。
如果不希望进行这样的修改，
可以直接在MS PowerPoint软件中直接输入或者复制粘贴已经编译好的网页格式输出的内容。
这样复制粘贴有一个缺点，
就是基于MathJax显示在网页中的公式，
无法通过复制粘贴转换到PowerPoint中。
一种办法是用rmarkdown将需要转换的带有公式的Rmd文件编译为Word格式或者PowerPoint格式，
再复制进入PowerPoint软件的新页面中。
编译为Word格式的命令如：

```
rmarkdown::render("slides.Rmd", output_format="word_document", encoding="UTF-8")
```

## D.4 Bearmer幻灯片格式

上述Slidy格式的幻灯片，
也可以通过LaTeX编译器转换成LaTeX beamer格式的幻灯片，
设置如：

```
---
output: 
  beamer_presentation:
    includes:
      in_header: preamble.tex
    latex_engine: xelatex
    slide_level: 2
    theme: CambridgeUS
    colortheme: dolphin
    citation_package: biblatex
    keep_tex: yes
---
```

其中`slide_level`用来规定几级标题开始新的一帧。
`theme`指定一种主题，
`colortheme`指定一种配色方案，
`in_header`在LaTeX导言部分插入`preamble.tex`，
内容如：

```
\usepackage{ctex}
\usepackage{amsthm,mathrsfs}
```

用一个井号分节，
用两个井号开始一个新的帧，
在两个井号和空格之后输入帧的标题。

编译命令如：

```
rmarkdown::render("mydemo.Rmd", 
  output_format = "beamer_presentation", encoding="UTF-8")
```

结果是一个PDF文件。
目前不支持bookdown，
所以bookdown的哪些自动编号、交叉引用功能还无法使用。

## D.5 R Presentation格式

R Studio软件单独提供了对一种R Presentation格式的源文件的支持，
以.Rpres扩展名结尾，
是一种特殊的R Markdown文件，
与slidy的源文件也类似。

Rpres文件编译为HTML格式的幻灯片，
使用reveal.js控制显示。
reveal.js中也有对激光笔支持不好的问题，
这是因为reveal.js中用向右光标键翻页，
对向下光标另有定义，
激光笔一般是模拟向下和向上光标键来翻页的。
为了支持激光笔，
找到RStudio的安装目录，
在`resources/presentation/revealjs/js`中找到`reveal.js`文件，
在文件编辑器中打开，
通过搜索找到`case 38:`, 将其剪切到`case 33:`后面，
变成`case 33: case 38:`。
找到`case 40:`,
将其剪切到`case 34:`后面，
变成`case 34: case 40:`。
同一目录还有一个`reveal.min.js`文件，
也进行上述修改。