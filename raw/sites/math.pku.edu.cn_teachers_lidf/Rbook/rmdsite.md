---
crawl_time: '2026-01-17 14:19:59'
framework: rbook
title: C 用R Markdown制作简易网站 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmdsite.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# C 用R Markdown制作简易网站

## C.1 介绍

为了从多个.Rmd源文件制作网站，
可以使用blogdown扩展包或者bookdown扩展包。
bookdown扩展包可以生成gitbook格式的网站，
带有左侧的章节目录和前后页面的导航链接，
很适用于一本书的网站。
但是，
bookdown使用时需要重新编译整个网站才能产生正确的链接。

也可以仅利用rmarkdown包的`render.site()`功能制作简易的网站。

## C.2 简易网站制作

一种简易的制作网站的办法是仅仅利用rmarkdown包的`render.site()`功能。
为了使得rmarkdown和RStudio将一个子目录（项目）看成一个网站项目，
只要此项目中含有`index.Rmd`和`_site.yml`文件。
所有网页.Rmd源文件都必须在项目的顶级目录中。
只要将编译所得的网站目录上传到任意的网站服务器就可以发布到网上。

### C.2.1 网站结构

`index.Rmd`是主页内容，
可以在此处人工加入其它页面链接（参见markdown格式说明中链接写法），
也可以制作单独的目录页面。
内容如

```
---
title: "概率论"
---
```

```
* [概率分布](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/dist.html)
* [期望](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/expect.html)
```

`_site.yml`是一个YAML文件，
其中包含站点的设定和输出设定。
内容如

```
name: "概率论"
output_dir: "_probbook"
output:
  html_document: 
    toc: true
    mathjax: "../../../MathJax/MathJax.js"
    self_contained: false
    includes:
      in_header: "_header.html"
navbar:
  title: "概率论"
  left:
    - text: "Home"
      href: index.html
    - text: "About"
      href: about.html
```

这里`name`设置站点名称，
`output_dir`给出生成的html及其他辅助文件的存放目录，缺省时用`_site`子目录。
navbar域设置了网站的菜单，这里包括Home(主页), Contents(目录), About(关于)三个页面的导航菜单。
output域设定输出的选项，
其中`html_document`就是用`rmarkdown::render_site()`生成的网站的输出文件格式，
其中`toc: true`说明每个页面都有自身内容的目录,
关于`mathjax`, `self_contained`, `includes`都与数学公式设置有关，
这里设置数学公式使用与本网站在同一目录系统或同一网站地址存放的MathJax数学公式显示库，
该库放在生成的HTML文件所在目录向上两层的MathJax子目录中。
`in_header`指定一个要插入到生成的每个HTML的head部分的内容文件，
这里内容文件`_header.html`的内容是

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

是对MathJax的一些设置，主要使用LaTeX作为输入格式。

### C.2.2 编译

具有`index.Rmd`和`_site.yml`文件的项目，
在RStudio软件中会显示一个Build窗格，
点击其中的`Build Website`可以自动调用`rmarkdown::render_site()`生成整个站点，
存放在`output_dir`指定的输出目录中，默认目录名为`_site`。
为了将生成的站点发布到网站，
只要将输出目录的所有内容全部复制到服务器的某个子目录中就可以了。

`rmarkdown::render_site()`生成整个站点时会自动逐个编译.Rmd文件，
并将项目目录中其它的文件和子目录复制到输出目录中，
以句点或者下划线开头的文件和子目录不复制，
.R, .r, .Rmd文件不复制。

因为是自动依次编译所有的.Rmd文件，
所以中间某个源文件编译错误就会使得整个编译失败，
修正错误后还是需要再次编译所有文件，
这一点不太方便。
但是，`rmarkdown::render_site()`提供了这样一种操作模式：
将有错的文件先移动到其它目录中，
编译整个站点，这时生成的站点仅包含确认无误的各个页面；
然后，
再将可能有错的文件移动回到项目目录，
逐个地打开每个未成功编译的文件，
用`Knit`按钮单独编译，
编译成功后结果文件也自动进入输出目录。

RStudio的Knit按钮或者`rmarkdown::render_site()`命令可以人工控制一个一个地编译.Rmd文件，
编译成功一个就存放到输出目录中一个，
这是用`rmarkdown::render_site()`制作网站比用bookdown扩展包制作网站的一个优点。
因为网站的目录是自己编写的链接，
所以这样逐个编译不会破坏网站的链接。
编译单个文件的命令如

```
rmarkdown::render_site("sec1.Rmd", output_format = "html_document", encoding="UTF-8")
```

其中`sec1.Rmd`是某个网页内容源文件。

可以用`rmarkdown::clean_site()`删除生成的文件，
包括程序结果缓存。

如果用bookdown包，
只有编译所有文件才能生成正确的网站目录，
bookdown也能编译单个文件，
但是仅能作为调试，
调试成功后还是需要编译所有文件才能使得网站目录正确。

bookdown站点的优势在于自动生成网站目录而不需要人工管理目录，
而且bookdown支持图书的公式、定理、图标自动编号和链接，
文献列表和文献引用，
所以bookdown是比`rmarkdown::render_site()`编译调试速度比较慢但是功能更强大的一种制作网站的工具，
而且还可以生成PDF版图书。

所以，一本书如果不需要很多的公式自动编号、图表编号、文献，
在编写阶段可以使用`rmarkdown::render_site()`制作，
定稿后再改成bookdown图书格式，
实际上每个源文件内容部分不需要改变，
只需要修改一下头部就可以了。
站点的设置需要将`_site.yml`中的`site`域的值改为`bookdown::bookdown_site`，
并增加`_bookdown.yml`和`_output.yml`两个设置文件。
详见[B](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/bookdown.html#bookdown)。

### C.2.3 内容文件

内容文件只要用普通的Rmd文件即可，如某一网页的源文件如

```
---
title: "期望"
---
```

```
随机变量**数学期望**可以看成是对随机变量取值的加权平均，
某个值的取值概率越大，加权越大。

对离散随机变量，
期望是加权和；
对连续型随机变量，
期望是用密度函数加权对`x`积分。
```

### C.2.4 网站设置

在`_site.yml`文件中进行网站设置，
样例见[C.2.1](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmdsite.html#rmdsite-rmarkdown-struct)。
顶级设置有：

* `name`: 网站名称；
* `output_dir`: 保存编译结果的子目录名，默认为`_site`，
  如果设置为`.`，则表示编译结果放在项目的顶级目录中而不是子目录中；
* `includes`: 指定输出的网站中要包含的文件，
  输出时默认不包含.R，.r, .Rmd，.RData, .rds文件和文件名以小数点或者下划线开头的文件，
  所以例外要写在这里，
  如`includes: ["data1.R", "data2.R"]`;
* `excludes`: 指定项目中不希望输出的结果网站的文件，
  可以有通配符，如
  `excludes: ["backup.txt", "*.xlsx"]`;
* `output`: 其中的`html_document`属性的设置是所有Rmd网页源文件的共同设置，
  参见[A.11.3](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-output-attr)，
  每一个Rmd文件本身也可以有自己的属性设置；
* `navbar`: 用来设置导航菜单，属性`left`指定主菜单栏，
  根据所用的网站主题，可能显示在左侧或者顶部。
  属性`right`指定一个右侧导航栏。

`includes`和`excludes`设置主要是为了改变默认的文件发布规则，
所有的.Rmd文件只要不是文件名以下划线开头就都会被编译并且复制到结果子目录中。

## C.3 用blogdown制作网站

R扩展包blogdown可以与Hugo软件配合制作简单的静态网站。
网站的所有文件都存在于一个目录中，
只要上传到任意的网站服务器就可以发布，
没有任何限制。

网站内容用R Markdown格式编写，
在R或者RStudio中编译为网站。
这样的做法使得网站内容容易再现，
而且也可以将内容转换为PDF、Word等格式。
blogdown包的R Markdown格式支持bookdown包的扩充，
如公式、图表编号与引用。

blogdown包的名称虽然包含了blog，
但是其建站并不限于博客类型，
任何类型的静态网站都可以构建，
比较适合的是个人网站。

参考：

* ([Yihui Xie 2017](#ref-Xie2017:blogdown))
* ([Xie, Allaire, and Grolemund 2019](#ref-Xie2019:rmarkdown))

### C.3.1 生成新网站的框架

在RStudio中，
菜单“File – New Project – blogdown site”可以生成一个网站框架，
注意一定要新建目录而不是用已有目录，
而且第一次新建网站时还会自动下载Hugo静态网站生成程序。

生成的网站框架包括如下文件与子目录：

* index.Rmd：主页面的源文件。
* config.toml: 网站设置文件。
* ./content/: 存放作为网站内容的md或者Rmd文件。可以在下面再建立子目录。
  ./content/post/用来保存博客类型的网页，但是不做要求。
* ./resources:
* ./static: 保存图片、CSS等文件，在发布网站时会被复制到./public子目录中。
* ./themes: 存放多个网站主题。
* ./public: 发布网站时所用到的所有文件。
  生成的内容被复制到这个目录中。
  只要将这个目录的内容全部复制到任何一个支持静态内容的网站服务器，
  就可以发布网站。

这样建站，
建成的网站应该在RStudio的Viewer窗格自动显示，
如果没有自动显示网站或者再次打开网站项目，
点击图标栏的`Addins -- Serve site`可以启动网站服务器发布该网站。
或者用用命令

```
blogdown::serve_site()
```

启动网站服务器。
每次在RStudio或者R中打开一个网站项目，
都只需要启动一次服务器。
blogdown的服务器会在后台运行，
侦测到文件修改时会自动重建，
这称为在线重建(Live Reload)。
在线重建功能基于R扩展包servr。
所以用户只要发布新的消息或者修改已有消息，
不用关心后台的转换问题。

blogdown提供了一些RStudio的addin程序，
可以为网站功能提供方便，
比如`New Post`可以生成新消息，
`Update Metadata`可以修改消息的YAML元数据，
`Insert Image`可以在消息中插入图片。

使用在线重建功能时，
应关闭一些其他功能。
在RStudio的“Tools – Project Options – Build Tools”对话框中，
取消选定“Preview site after building”和“Re-knit current preview when supporting files change”。

如果不使用在线重建功能，
即不使用Serve site，
可以在RStudio的Build窗格中用“Build Book”功能手工控制网站的重建。

### C.3.2 网页内容文件及其设置

网页内容文件可以用Rmd文件也可以用md文件，
但是Rmd文件使用Pandoc和bookdown包支持的文件格式，
功能更强，
比如支持LaTeX数学公式，
公式、图表自动标号与交叉索引，等等，
所以最好是用Rmd文件。

如果有某个Rmd源文件暂时不想编译输出，
可以将其扩展名临时改为一个不被编译的扩展名如.txt。

blogdown编译结果的网页是HTML格式，
其Rmd元数据输出类型为`blogdown::html_page`，
此类型基于`bookdown::html_document2`，
又基于`rmarkdown::html_document`。
这样，可以在Rmd源文件的元数据部分对该输出类型设置相应的属性，
如`toc: true`等。
例子：

```
---
title: "一个试验网页"
author: "李东风"
date: "2019-07-08"
output:
  blogdown::html_page:
    toc: true
    fig_width: 6
    dev: svg
---
```

为了给所有输出设置统一的属性，
可以在项目目录中添加`_output.yml`文件，
在其中设置输出文件的各种属性，如：

```
blogdown::html_page:
  toc: true
  fig_width: 6
  dev: svg
```

### C.3.3 初学者的工作流程

Hugo建站程序比较复杂，
如果要挑选自己喜爱的网站主题(theme)，
移植网站也比较麻烦。
对于了解网站技术不多的建站者，
建议用如下的流程建立新站：

* 在<https://themes.gohugo.io/>仔细地挑选一个合适的网站主题(theme)；
* 在RStudio中通过生成一个在已有目录的新项目；
* 在新项目的控制台中提交命令`blogdown::new_site(theme = 'user/repo')`，
  其中`user/repo`是前面所选的主题的用户名与资源(repository)名；
* 试验新网站看是否满意。
  如果不满意可以重复以上步骤，
  如果满意，
  就可以修改`config.toml`中的设置。
  如果某个选项不明白，
  可以在该主题的文档中查找，
  通常是资源的README文件。
  只需要修改必须需改的选项。

为修改已有网站内容，
用如下的步骤：

* 点击RStudio的addin “Serve Site”，
  可以在修改过程中动态地构建网站。
  每次打开RStudio的网站项目仅需执行一次这个命令。
* 用“New Post” addin添加新的网页。
* 在修改某个网页时，用“Update Metadata” addin修改网页元数据，
  主要是网页标签等。

为了发布网站，
只要将项目内的public目录上传到任一支持静态内容的网站服务器。
如果没有合适的服务提供商，
可以考虑<https://www.netlify.com/>，
可以用Github账户登录并有一定的免费建站服务。
如果熟悉Github，
还可以将自己的网站源文件作为一个Github 资源(repository)，
将Github资源发布到Netlify，
就不需将public的内容上传到网站，
而只要有源文件就可以了，
Netlify支持Hugo。

### C.3.4 网站设置文件

在项目根目录中有一个`config.toml`文件，
是Hugo软件的建站设置文件，
可以设置网站标题、描述、菜单等。
一个例子如下：

```
baseurl = "/"
languageCode = "en-us"
title = "试验Blogdown网站"
theme = "hugo-lithium"
googleAnalytics = ""
disqusShortname = ""
ignoreFiles = ["\\.Rmd$", "\\.Rmarkdown$", "_files$", "_cache$"]

[permalinks]
    post = "/:year/:month/:day/:slug/"

[[menu.main]]
    name = "About"
    url = "/about/"
[[menu.main]]
    name = "Li Dongfeng Homepage"
    url = "http://www.math.pku.edu.cn/teachers/lidf/"

[params]
    description = "用Hugo和blogdown制作的网站"

    # options for highlight.js (version, additional languages, and theme)
    highlightjsVersion = "9.12.0"
    highlightjsCDN = "//cdnjs.cloudflare.com/ajax/libs"
    highlightjsLang = ["r", "yaml"]
    highlightjsTheme = "github"

    MathJaxCDN = "//cdnjs.cloudflare.com/ajax/libs"
    MathJaxVersion = "2.7.5"

    # path to the favicon, under "static"
    favicon = "favicon.ico"

    [params.logo]
    url = "logo.png"
    width = 50
    height = 50
    alt = "Logo"
```

在设置文件中，
字符串要写成用双撇号界定的形式，
布尔值要写成没有双撇号的true或者false。
用方括号包围的项（如`[permalinks]`）会定义一个列表类型的变量，
下面缩进后的内容是列表的元素。
用双方括号包围的项是列表数组，
即每一项都是列表，
但是合并在一起由组成一个元素为列表的数组。
如上面`[[menu.main]]`有若干项，
每一项是一个主菜单项，
其设置是一个列表。
主菜单位置、配色等在其它地方设置，
主设置文件中仅设置其文字和对应链接。

不同的网站主题需要不同的`config.toml`文件。

下面给出一些选项的解释。

* `baseURL`：这是你的网页发布到网站上时，
  该网站给你的主页面的地址。
  一般是需要设置为自己发布以后的divide。
* `languageCode`: 中文是`zh-cn`，英文是`en-us`，
  可能会影响一些排版规则，
  但是编码都是UTF-8。
* `permalinks`: Hugo和blogdown在发布HTML与图片等内容时需要不变的链接，
  默认是content中的每个Rmd文件变成`public`或`public/post`下的同名子目录中的`index.html`文件。可以用`permalinks`指定命名规则。
  `slug`是网页元数据可以设置的一项类似于文章标识符的东西，
  不设置则使用文章标题。
* `publishDir`: 生成的网站存放的文件，默认为`public`目录。
* `theme`：在`themes/`目录中保存所选主题的目录的名字。
* `ignoreFiles`: 项目中那些文件不被复制到生成结果网站中。
* `hasCJKLanguage`: 如果有大量中文、日文、韩文内容，
  设置为`true`可以在网页统计和单词书统计时更准确。
* `[params]`: 主题特有的设置放在这里。

### C.3.5 静态文件

项目中在`./static/`下的文件称为静态文件，
可以保存自己的图片、Javascript库等，
发布时自动复制到`./public/`中。
比如，
文件`.static/figs/mypic01.png`，
在Rmd文件中可以用`![](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/mypic01.png)`引用。

每个主题中一般也有一个static目录，
自己的static目录中的同名文件在发布时可以覆盖主题中的同名文件，
这样可以修改主题的一些资源或者设置。
从Rmd编译生成的文件也可能会被static中的同名文件覆盖。

还可以将网页编译为PDF等格式存放在static目录中。
`rmarkdown::build_dir()`命令可以将目录中所有Rmd文件按照其元数据的输出格式编译输出。

### References

Xie, Yihui, J. J. Allaire, and Garrett Grolemund. 2019. *R Markdown: The Definitive Guide*. CRC Press. <https://bookdown.org/yihui/rmarkdown/>.

Yihui Xie, Alison Presmanes Hill, Amber Thomas. 2017. *Blogdown: Creating Websites with r Markdown*.