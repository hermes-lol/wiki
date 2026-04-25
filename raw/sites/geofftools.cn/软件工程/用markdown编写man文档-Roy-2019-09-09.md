---
title: 用markdown编写man文档
source_url: https://geofftools.cn/blog/write-man-page-in-markdown/
author: Roy
date: 2019-09-09
category: 软件工程
tags: [markdown, man-page, documentation, roff, linux]
---

# 用markdown编写man文档

晚上有些碎片时间可以用，想着接一接 CMPP 的 man 翻译。一直很好奇 man 文档都是怎么编写出来的，背后是什么格式的文本，于是就接触到了 roff 格式。这篇文章介绍了三种文件格式，md、roff、富文本，就 scpp 作为例子展示了从 md 如何生成 man 文档并安装。

## 准备内容

本文中介绍三种文件格式的区别的时候，转换 roff 语言与富文本使用到了 `groff` 命令，可能需要安装。

在 md 转化为富文本的时候除了 `groff` 命令，还使用到了 `pandoc` 命令，可以进行一个安装。

## 三种文档

以 scpp 的文档展示一下三种格式的区别。

### 富文本

根据不同的终端，富文本的显示是通过加入不同的特殊字符达到效果。

由于是放在代码框里面���特殊字符不会被处理成加粗、倾斜等格式，这些在终端中会显示。

下文生成的 `scpp.1` 就���包含特殊字符的文档。

终端不同，特殊字符的约定不同。也就是说 Windows 终端上生成的富文本放到 Linux 的终端上显示就可能不正常。

由于书写麻烦也不通用，而源文档需要书写简单而且通用，所以有了下面两种格式。

### roff 格式

roff 是 GNU 默认的标记语言，用纯文本就可以写出有格式的文档，大量我们 man 命令看到的内容都是用这个格式写成的。

其中例如 `.TH`、`\fB\fP` 等内容让文档可以产生加粗、标题等各种格式。

将这个文件存储为 `scpp.roff`，之后输入这个命令就可以看到显示效果：

```
groff -man -Tutf8 scpp.roff
```

是不是就看到了目标文档的效果，如果你想看到特殊字符可以把他输出到文件：

```
groff -man -Tutf8 scpp.roff | col -b > scpp.txt
```

roff 格式的文件有大量的标记，如果你感兴趣，可以在[这里](https://www.gnu.org/software/groff/manual/)扩展阅读。

### md 格式

Markdown 是目前非常常用的标记语言，也是纯文本编写有格式的文档，大量现在的博客都是用这个语言写成的。

相较而言，Markdown 的标记更少但也更简单，入门方便，也提供图片、表格等的插入。

本文档使用 md 格式就是是这个样子：

```markdown
% Manual page scpp(1)
% Author: Roy
%

# NAME

scpp \- simple C preprocessor

# SYNOPSIS

scpp [_options_] [_file_]

# DESCRIPTION

**scpp** is a simple C preprocessor written in Python.

# OPTIONS

\-D _macro_
    Define a macro.

\-I _dir_
    Add include directory.
```

将这个文件存储为 `scpp.md`，之后输入这个命令就可以看到显示效果：

```
pandoc -s -t man scpp.md -o scpp.1
```

其中 `--standalone` 标签是为了给文档生成的独立的文件头。开头的那些元数据提供了生成 roff 所需的一些必要内容。

为了让其与所有其他的 man 文档格式相同，我们先转为 roff 格式，再通过 groff 转化为富文本。

## 安装 scpp 的 man 文档

到了这一步，已经了解了三种不同的文件格式，也可以用最熟悉的 Markdown 格式写 man 文档。下一步就是将写好的内容进行安装，让帮助文件生效了。

不同的系统有不同的 man 手册目录，Linux 是 `/usr/man`，OSX 是 `/usr/share/man`。将文件生成在该目录就可以了。

之后使��� `man scpp` 就能看到你自己写的 man 文档。