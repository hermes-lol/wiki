---
crawl_time: '2026-01-17 14:20:10'
framework: rbook
title: 53 R与C++的类型转换 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rcpp-typeconv.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 53 R与C++的类型转换

R程序与由Rcpp支持的C++程序之间需要传递数据，
就需要将R的数据类型经过转换后传递给C++函数，
将C++函数的结果经过转换后传递给R。

## 53.1 用`wrap()`把C++变量返回到R中

在R API中用.Call()函数调用C程序库函数时，
R对象的数据类型一般是SEXP。
Rcpp提供了模板化的`wrap()`
函数把C++的函数返回值转换成R的SEXP数据类型。
此函数的声明为

```
template <typename T> SEXP wrap(const T& object);
```

`wrap()`能转换的类型包括：

* 把int, double,
  bool等基本类型转换为R的原子向量类型
  （所有元素数据类型相同的向量）;
* 把`std::string`转换为R的字符型向量;
* 把STL容器如`std::vector<T>`或`std::map<T>`
  转换成基本类型为T的向量，条件是T能够转换；
* 把STL的映射`std::map<std::string, T>`
  转换为基本类型为T的有名向量，条件是T能够转换；
* 可以转换定义了`operator SEXP()`的C++类的对象；
* 可以转换专门化过wrap模板的C++对象。

是否可用wrap()转换是在编译时确定的。

## 53.2 用`as()`函数把R变量转换为C++类型

Rcpp提供了模板化的`as()`用来把SEXP类型转换成适当的C++类型。
`as()`函数的声明为:

```
template <typename T> T as(SEXP x);
```

`as()`可以把R对象转换为基础的类型如
int, double, bool, std::string等，
可以转换到元素为基础类型的STL向量如std::vector等。
如果C++类定义了以SEXP为输入的构造函数也可以利用`as()`来转换。
`as()`可以针对用户自定义类作专门化。

## 53.3 `as()`和`wrap()`的隐含调用

当C++中赋值运算的右侧表达式是一个R对象或R对象的部分内容时，
可以隐含地调用`as()`将其转换成左侧的C++类型。

当C++中赋值运算的左侧表达式是一个R对象或其部分内容时，
可以隐含地调用`wrap()`将右侧的C++类型转换成R类型。

在用Rcpp属性(Rcpp属性见下一节)声明的C++函数中，
可以直接以IntegerVector, NumericVector, CharacterVector, Function
等作为自变量类型或返回值，
可以与R中相应的类型直接对应。

能自动转换到R中的缺省值类型还包括：

* 用双撇号界定的字符串常量；
* 十进数值如10, 4.5;
* 预定义的常数如true, false,
  R\_NilValue, NA\_STRING, NA\_INTEGER,
  NA\_REAL, NA\_LOGICAL；