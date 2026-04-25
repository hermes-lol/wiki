---
crawl_time: '2026-01-17 14:25:01'
framework: sphinx
title: 25 R中的近似计算函数(*) | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/approx-rlang.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 25 R中的近似计算函数(`*`)

## 25.1 多项式

R用扩展包polynom提供了一元多项式类，
用多项式系数表示一个多项式，
提供了多项式运算、微分、积分等功能。

用`polynomial(coef)`生成一个多项式，
自变量`coef`是多项式的0次、1次、2次、……项系数的向量。
例如，\(P(x)= -3 + 2x + x^2\)可表示为：

```
library(polynom)
P2 = polynomial(c(-3,2,1))
```

也可以利用多项式的根定义多项式，
如\((x+1)(x-1)\):

```
poly.from.roots(c(-1, 1))
```

```
## -1 + x^2
```

用`as.character(p)`可以将多项式`p`转换成R的表达式字符串，如：

```
as.character(P2)
```

```
## [1] "-3 + 2*x + x^2"
```

用`coef(p)`可以提取多项式的升幂表示的各个系数为一个向量，如

```
coef(P2)
```

```
## [1] -3  2  1
```

用`as.function(p)`可以将多项式转换成一个R函数，
这时它才能对自变量值求函数值，
如

```
curve(as.function(P2)(x), -4, 2, ylab="P(x)"); abline(h=0, lty=3)
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/0442-approx-rlang_files/figure-html/approx-rlang-poly15-1.png)

实际上，
可以直接对多项式作图，
如

```
plot(P2, xlim=c(-5, 5))
```

![](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/0442-approx-rlang_files/figure-html/approx-rlang-poly16-1.png)

多项式可以进行通常的加、减、乘法，如

```
P1 <- polynomial(c(-2, 1))
print(P1 + P2)
```

```
## -5 + 3*x + x^2
```

```
print(P1 * P2)
```

```
## 6 - 7*x + x^3
```

多项式除法比较特殊。
除尽时：

```
P2 / polynomial(c(3,1))
```

```
## -1 + x
```

除不尽时：

```
P3 = P2 / polynomial(c(1,1)); print(P3)
```

```
## 1 + x
```

```
P4 = P2 - P3*polynomial(c(1,1)); print(P4)
```

```
## -4
```

这说明
\[
\frac{-3 + 2x + x^2}{1 + x} = 1 + x + \frac{-4}{1+x}
\]

可以用函数`GCD()`求多项式的最大公因式，
用`LCM()`求多项式的最小公倍式。

用`deriv(p)`求多项式`p`的一阶导数，如

```
deriv(P2)
```

```
## 2 + 2*x
```

用`integral(p)`求多项式`p`的不定积分，
看成是\(\int\_0^x p(t) \,dt\)。
`integral(p, limits)`则可求定积分，
其中`limits`是表示积分上界和下界的两个元素的向量。如

```
integral(P2)
```

```
## -3*x + x^2 + 0.3333333*x^3
```

```
integral(P2, c(-3, 1))
```

```
## [1] -10.66667
```

`poly.calc(x, y)`可以从给定的x值和y值的组合计算相应的Lagrange插值多项式，
阶数由点数决定。

**例25.1** 对\(h(x)=\sqrt(x)\)在\(x=1/16\), \(1/4\), \(1\)用抛物线插值。

```
ph <- poly.calc(x=c(1/16, 1/4, 1), y=sqrt(c(1/16, 1/4, 1)))
print(ph)
```

```
## 0.1555556 + 1.555556*x - 0.7111111*x^2
```

可以用MASS包的`fractions()`函数得到分数表示：

```
MASS::fractions(coef(ph))
```

```
## [1]   7/45   14/9 -32/45
```

即此插值多项式的理论值是
\[
\frac{1}{45} \left( 7 + 70x - 32 x^2 \right) .
\]

用`polylist()`可以生成若干个多项式的列表，
可以用`sum()`函数求多项式的和，
用`prod()`求多项式乘积，
用`deriv()`对其中每个微分，
用`integral()`对其中每个积分。

## 25.2 插值

R函数`approx(x, y)`对给定的x和y坐标计算线性插值，
输出格子点上的x, y坐标的列表。
可以用`n=`指定输出的格子点的个数，
可以用`xout=`指定需要计算的x坐标。

`approx.fun(x, y)`计算插值函数，
结果是类型为R函数的插值函数。

## 25.3 样条插值

R函数`spline(x, y)`对给定的x和y坐标计算样条插值，
输出格子点上的x, y坐标的列表。
可以用`n=`指定输出的格子点的个数，
可以用`xout=`指定需要计算的x坐标。
默认使用fmm样条，
这时在两端用各自最边缘处的4格点分别拟合三次多项式。
选`method="natural"`用自然样条。

`splinefun(x, y)`计算样条插值函数，
结果是类型为R函数的插值函数，
得到的插值函数可以用来计算x处的插值函数值及导数值，
在调用生成的插值函数时加选项`deriv=`可以指定导数阶数，
最多3阶。

函数`spline.smooth()`可以计算样条曲线拟合。

在用`lm()`模型，
可以用`bs()`函数计算给定的自变量\(x\)的若干阶的样条基函数值作为非线性项。

## 25.4 积分

R函数`integrate()`用自适应方法计算一元定积分。
但是，当被积函数在大部分区域为常数时，
自适应算法的收敛判断可能会误判，
得到完全错误的结果。

## 25.5 Gauss-Legendre积分

R的gss包的`gauss.quad()`函数可以对给定点数和区间端点计算高斯-勒让德积分的节点位置和相应积分权重。
用法为

```
gauss.quad(size, interval)
```

## 25.6 微分

R的`deriv()`函数计算表达式的导数，
有符号显示表示时用符号计算得到结果，
否则计算数值导数。
如
`{rapprox-rlang-deriv01, cache=TRUE} deriv(~ sin(x)^2, "x")`