---
crawl_time: '2026-01-17 14:20:05'
framework: rbook
title: 57 用Rcpp帮助制作R扩展包 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp-package.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 57 用Rcpp帮助制作R扩展包

R扩展包是把解决某种问题的可复用代码、文档整合在一起的最好的方法。
写成R扩展包后，可以自己用，也可以利用CRAN分发。
扩展包用户一般不用自己编译。

使用扩展包来组织程序，
多个源程序、头文件之间的依赖关系可以自动得到处理。

扩展包提供了测试、文档和一致性检查的统一框架。

扩展包中代码可以仅有R程序，也可以包括C程序、C++程序、Fortran程序。
如果仅有R代码，就不需要借助于Rcpp，可以使用
`package.skeleton()`函数生成一个扩展包框架。
如果有C++代码，就可以用Rcpp作为接口，
并用Rcpp提供的`Rcpp.package.skeleton()`函数制作扩展包框架。

Rcpp属性的exports注释仍可在制作扩展包时指定如何输出
C++中定义的函数使其在R中可调用。

## 57.1 不用扩展包共享C++代码的方法

Rcpp属性的`sourceCpp()`通常只适用于写在R程序内部的简短C++代码，
或者写在一个单独C++文件中，不依赖于其它C++程序的单独代码。
如果有多个C++源程序、头文件，彼此有依赖关系， 最好使用扩展包。

在多个单独的C++文件共享某些简单的代码，
彼此不互相依赖时，可以用C++的预处理include命令共享这些代码。

比如，有多个C++源程序都用到如下的代码：

```
#ifndef __UTILITIES__
#define __UTILITIES__
inline double timesTwo(double x) {
  return x * 2;
}
#endif // __UTILITIES__
```

假设这段代码保存到当前子目录的“utilities.hpp”文件中。
则在每个需要用到这段代码的C++源程序中，插入如:

```
#include "utilities.hpp"
//[[Rcpp::export]]
double transformValue(double x){
  return timesTwo(x) * 10;
}
```

## 57.2 生成扩展包

### 57.2.1 利用已有基于Rcpp属性的源程序制作扩展包

假设在当前目录中有了若干个C++文件，
其中需要转换到R中的C++函数已经用`Rcpp::export`声明过。
其中一个是conv1.cpp。

从当前目录启动R，运行

```
Rcpp.package.skeleton("testpack",
  example_code=FALSE,
  attributes=TRUE,
  cpp_files=c("conv1.cpp"))
```

运行显示：

```
Creating directories ...
Creating DESCRIPTION ...
Creating NAMESPACE ...
Creating Read-and-delete-me ...
Saving functions and data ...
Making help files ...
Done.
Further steps are described in './testpack/Read-and-delete-me'.

Adding Rcpp settings
 >> added Imports: Rcpp
 >> added LinkingTo: Rcpp
 >> added useDynLib directive to NAMESPACE
 >> added importFrom(Rcpp, evalCpp) directive to NAMESPACE
 >> copied conv1.cpp to src directory
```

运行完后，在当前目录生成了一个testpack子目录，
这是要制作的扩展包的名字。
在testpack子目录中，有文件DESCRIPTION, NAMESPACE,
Read-and-delete-me, 有子目录src, R, man。

子目录src中为C++和C源程序、头文件。
子目录R中为Rcpp从C++程序转换过来的R接口程序，
用户自己的R程序也可以放在这里。
子目录man是特殊格式的文档， 其格式类似LaTeX。

### 57.2.2 DESCRIPTION文件

在DESCRIPTION文件中，
有扩展包名称、版本、日期、作者姓名、维护者姓名和联系方式、
简单描述、授权， 还有Imports和LinkingTo两项。
除此之外，还有许多可选的域，如Depends。

Imports给出本软件包要调用的其它扩展包,
但是这些扩展包并不随本扩展包一起调入，
仅是会调入其名字空间。这里的值为

```
Imports: Rcpp (>= 0.12.3)
```

LinkingTo指定在编译本软件包的C、C++、Fortran等源程序时，
会用到哪些其它扩展包的头文件。这里的值为

```
LinkingTo: Rcpp
```

指定的这些扩展包一般是编译时才有用的，
所以一般不会出现在Depends和Imports域中。

LinkingTo只解决了头文件的问题，
要链接除了Rcpp之外的二进制库文件，
还需要手工编辑src/Makevars和src/Makevars.win文件。

和Imports有些相像的的DESCRIPTION域是Depends，
指定调入本扩展包时必须预先调入的软件包。
这里“调入”是指用`libraray()`或`require()`调入扩展包。
多个扩展包名用逗号分开，可以在扩展包名字后面加圆括号，
在圆括号内写上\(>=\)某个版本号, 如“`MASS(>=3.1-20)`”。

Depends也可以指定依赖于某个R版本之后，如“`R(>=2.14.0)`”。

DESCRIPTION文件中的Suggests与和Depends域类似，
但不是本扩展包必须的，
比如仅用在某个例子中或测试中、仅用来编译vignettes。

### 57.2.3 NAMESPACE文件

示例NAMESPACE文件如下：

```
useDynLib(testpack)
exportPattern("^[[:alpha:]]+")
importFrom(Rcpp, evalCpp)
```

其中第一行指定调用本软件包时， 需要调入的本扩展包的动态链接库。
第二行指定扩展包需要对外部可见的R函数是所有函数名字以字母开头的R函数。
用户可以自己指定其它的模式或者指定固定的若干个函数。
第三行说明了需要从Rcpp包导入evalCpp函数。

## 57.3 重新编译

修改了扩展包中的C++源程序后， 需要重新编译。
只要在R中把工作目录设为软件包的子目录内， 运行

```
compileAttributes()
```

这会自动生成两个文件，一个是`src/RcppExports.cpp`，
是C++程序的接口函数。
另一个是`R/RcpExports.R`，
用.Call来调用C++接口函数，转换成R函数。
这两个文件不要自己修改。

## 57.4 建立C++用的接口界面

利用了Rcpp属性可以指定要输出到R中的函数。

在C++源程序中加入特殊注释

```
//[[Rcpp::interfaces(r, cpp)]]
```

则软件包在编译时也会生成该源程序文件中函数的外部可访问的接口，
这些接口的界面会在安装后的包的include子目录中出现，
在开发时出现在inst/include子目录中。

设要生成的扩展包名为testpack,
则界面文件包括`include`子目录中的
`testpack_RcppExports.h`文件和`testpack.h`文件，
`testpack.h`文件仅用来包含入`testpack_RcppExports.h`文件。

如果需要添加自己的一些界面程序，
可以修改`testpack.h`文件，
这时需要去掉文件开始的自动生成标记，
并且保留对`testpack_RcppExports.h`文件的包含。

导出的C++界面都在与制作的扩展包同名的名字空间中，
比如，如果制作的软件包名为testpack,
其中导出的一个C++函数为`convolveCpp`,
则在别的包的C++源程序中调用时，
应该包含`testpack.h`文件，
并用`testpack::convolveCpp()`格式调用。

如果自己本扩展包需要在编译时包含这些头文件，
需要自己编辑src子目录中的Makevars文件和Makevars.win文件，
添加行:

```
PKG_CPPFLAGS += -I../inst/include/
```