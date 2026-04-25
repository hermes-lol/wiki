---
crawl_time: '2026-01-17 14:20:22'
framework: rbook
title: 44 支持向量机 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 44 支持向量机

## 44.1 介绍

支持向量机是1990年代由计算机科学家发明的一种有监督学习方法，
使用范围较广，预测精度较高。

## 44.2 最大分隔边界判别法

### 44.2.1 分隔超平面

对因变量为两分类的情形，
设自变量\(\boldsymbol x\_i, i=1,2,\dots,n\)在\(\mathbb R^p\)空间中，
如果存在曲面将\(\mathbb R^p\)分隔成两部分，
使得两类的样本可以分开，
就有了一种简单的判别准则。
超平面是最简单的分隔曲面。

在平面空间\(\mathbb R^2 = \{(x\_1, x\_2): x\_1, x\_2 \in \mathbb R \}\)中，
超平面就是直线，其方程为
\[
\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 = 0
\]
其中\((\beta\_1, \beta\_2)^T \neq \boldsymbol 0\)。
在平面空间\(\mathbb R^2\)中，直线将空间分为两个部分，
分别满足\(\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 > 0\)和\(\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 < 0\)。

可以用其方程的法向\(\boldsymbol\beta=(\beta\_1, \beta\_2)^T\)的指向区分两个部分，
设\(\tilde{\boldsymbol x} = (\tilde x\_1, \tilde x\_2)^T\)在直线上，
而点\(\boldsymbol x = (x\_1, x\_2)^T\)满足\(\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 > 0\)，
则利用
\[\begin{aligned}
\beta\_0 + \beta\_1 \tilde x\_1 + \beta\_2 \tilde x\_2 =& 0 \\
\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 >& 0
\end{aligned}\]
两式相减得
\[
(\beta\_1, \beta\_2) \;
\left(\begin{matrix}
x\_1 - \tilde x\_1 \\ x\_2 - \tilde x\_2
\end{matrix}\right)
> 0
\]
用\(\langle \cdot, \cdot \rangle\)表示\(\mathbb R^2\)中的内积，
则\(\boldsymbol x = (x\_1, x\_2)^T\)在满足\(\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 > 0\)的半空间的充分必要条件是
\[
\langle \boldsymbol\beta, \boldsymbol x - \tilde{\boldsymbol x} \rangle > 0
\]
即从平面上的点\(\tilde{\boldsymbol x}\)出发到\(\boldsymbol x\)的向量\(\boldsymbol x - \tilde{\boldsymbol x}\)与分隔线的法线方向\(\boldsymbol\beta=(\beta\_1, \beta\_2)^T\)成锐角；
\(\tilde{\boldsymbol x} = (\tilde x\_1, \tilde x\_2)^T\)在满足\(\beta\_0 + \beta\_1 x\_1 + \beta\_2 x\_2 < 0\)的半空间的充分必要条件是
\[
\langle \boldsymbol\beta, \boldsymbol x - \tilde{\boldsymbol x} \rangle < 0
\]
即即从平面上的点\(\tilde{\boldsymbol x}\)出发到\(\boldsymbol x\)的向量\(\boldsymbol x - \tilde{\boldsymbol x}\)与分隔线的法线方向\(\boldsymbol\beta=(\beta\_1, \beta\_2)^T\)成钝角。

例如，考虑分隔线
\[
1 + 2 x\_1 + 3 x\_2 = 0
\]
法线方向为\(\boldsymbol\beta = (2,3)^T\),
上面的一个点\(\tilde{\boldsymbol x} = (1, -1)\)。
参见图[44.1](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-mmc-sepdemo)。

![直线将平面分为两部分](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm_files/figure-html/statl-svm-mmc-sepdemo-1.png)

图44.1: 直线将平面分为两部分

对于自变量\(\boldsymbol x \in \mathbb R^p\)的情形，
可以定义分隔超平面：
\[
\beta\_0 + \beta\_1 x\_1 + \dots + \beta\_p x\_p = 0
\]
将\(\mathbb R^p\)分隔为满足
\(\beta\_0 + \beta\_1 x\_1 + \dots + \beta\_p x\_p > 0\)的半空间与满足
\(\beta\_0 + \beta\_1 x\_1 + \dots + \beta\_p x\_p < 0\)的半空间，
两个空间的点可以用分隔超平面上的一个点\(\tilde{\boldsymbol x}\)到\(\boldsymbol x\)的矢量
\(\boldsymbol x - \tilde{\boldsymbol x}\)与超平面的法向量
\(\boldsymbol\beta = (\beta\_1, \dots, \beta\_p)^T\)的夹角是锐角还是钝角来区分，
即内积\(\langle \boldsymbol\beta, \boldsymbol x - \tilde{\boldsymbol x} \rangle\)的正负号来区分。

设两类判别问题的自变量数据保存为矩阵\(\boldsymbol X = (x\_{ij})\_{n \times p}\)，
其中的每一行为\(\boldsymbol R^n\)的一个点；
对应于每个点有一个\(y\_i \in \{ -1, 1 \}\)作为类别标签，
\(\boldsymbol y = (y\_1, \dots, y\_n)^T\)。
设法找到一个超平面将两类的点分开在两边。

例如，
在图[44.2](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-mmc-seplines01)中，
要区分蓝色和粉红色的点。
在左图中有三条直线都可以分开两种点。

![分隔线选择](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-demo-seplines01.png)

图44.2: 分隔线选择

找到的分隔线必须满足
\[\begin{aligned}
& \beta\_0 + \beta\_1 x\_{i1} + \beta\_2 x\_{i2} > 0 \Longleftrightarrow y\_i > 0 \\
& \beta\_0 + \beta\_1 x\_{i1} + \beta\_2 x\_{i2} < 0 \Longleftrightarrow y\_i < 0
\end{aligned}\]
可以统一地写成
\[\begin{aligned}
y\_i (\beta\_0 + \beta\_1 x\_{i1} + \beta\_2 x\_{i2}) > 0, \ i=1,2,\dots,n .
\end{aligned}\]

这种要求可以推广到\(\boldsymbol x\_i \in \mathbb R^p\)的情形。
即找到超平面使得
\[
y\_i (\beta\_0 + \beta\_1 x\_{i1} + \dots + \beta\_p x\_{ip}) > 0,
\ i=1,2,\dots,n .
\]

当分隔超平面存在时，
就可以用\(\beta\_0 + \beta\_1 x\_{i1} + \dots + \beta\_p x\_{ip}\)的正负号来选择\(y=1\)还是\(-1\)。
对一个待判别的观测\(\boldsymbol x^\*\)，
计算
\[
f(\boldsymbol x^\*)
= \beta\_0 + \beta\_1 x\_{i1}^\* + \dots + \beta\_p x\_{ip}^\*
\]
就可以判别\(y\)的类别，
\(f(\boldsymbol x^\*) > 0\)时判为\(1\)，
\(f(\boldsymbol x^\*) < 0\)时判为\(-1\)。
\(f(\boldsymbol x^\*)\)的绝对值大小也有意义，
绝对值越大时，
\(\boldsymbol x^\*\)距离分隔边界越远，
判决越可靠。
如果\(f(\boldsymbol x^\*)\)的绝对值接近于零，
则点\(\boldsymbol x^\*\)靠近分隔边界，
对其判决不够可信。

### 44.2.2 最大分隔边界超平面

如图[44.2](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-mmc-seplines01)的左图所示，
只要分隔超平面能分隔开训练集的样本点，
就不止有一个分隔超平面。
需要找到最优的分隔超平面。
想法是让两类的点都尽可能远离分隔面。
对某个候选的分隔超平面，
计算各个观测样本点到超平面的距离并求出最近距离，
称为margin（分隔边界）。
从所有的分隔超平面中求margin最大的一个
（见图[44.3](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-mmc-sepline-margin)），
用这个最大边界的超平面作为判决准则，
这种方法称为**最大分隔边界判别法**(maximal margin classifier)。

![分隔边界](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-demo-sepline-margin.png)

图44.3: 分隔边界

在图[44.3](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-mmc-sepline-margin)中，
有三个观测点到最优超平面的距离相等，都等于最大分隔边界（margin），
这样的点称为**支持向量**(support vector)。
这些点改变位置会使得分隔线改变位置，
而其它点轻微改变位置则不会使得分隔线改变位置，
除非其它点到分隔线的距离小于分隔边界了。
这是最大分隔边界判别法以及后面讲到的支持向量机方法的重要性质。

### 44.2.3 最大分隔边界超平面的构造

设法求最大分隔边界超平面。
为了标准化，
令法向量\(\boldsymbol\beta\)长度等于1。
求\(\beta\_0, \beta\_1, \dots, \beta\_p\)为如下优化问题的解：
\[\begin{aligned}
& \max\_{\beta\_0, \beta\_1, \dots, \beta\_p} \text{分隔边界} \text{ s.t.} \\
& y\_i (\beta\_0 + \beta\_1 x\_{i1} + \dots + \beta\_p x\_{ip}) \geq \text{分隔边界}, \ i=1,2,\dots,n . \\
& \beta\_1^2 + \dots + \beta\_p^2 = 1
\end{aligned}\]
其中的第二个约束条件对可行的分隔超平面是不需要的约束，
而\(\boldsymbol\beta\)长度为1的约束也不是必须的，
找到长度不为1的解后除以其长度即可。
不过，
当\(\| \boldsymbol\beta \| = 1\)成立时，
\(y\_i (\beta\_0 + \beta\_1 x\_{i1} + \beta\_2 x\_{i2})\)正是\(\boldsymbol x\_i\)到分隔超平面的垂直距离。
有高效的算法可以快速求解上述优化问题。

### 44.2.4 不能分隔的情形

如果两类的点混杂在一起，
有可能任何的超平面都不能将两个类完全分开。
见图[44.4](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-mmc-nonsep)。
这时，
只能退而求其次，
试图找到超平面使得两类尽可能被分开。

![找不到分隔超平面的情形](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-demo-nonsep01.png)

图44.4: 找不到分隔超平面的情形

## 44.3 支持向量判别法

### 44.3.1 问题

要求分隔超平面将两类完全分开有时不能做到，
即使能做到，
因为最大分隔边界超平面的选取过于依赖于边界处的支持向量，
几个点对结果的影响过大，
结果不够稳健，
也容易造成过度拟合。
见图[44.5](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svc-instab)，
右图中增加一个点后，分隔线偏离了很多，
分隔边界变得很窄而且对原有点的判别效果变差了。
各个点距离边界越远，判别效果越好。

![分隔超平面的不稳健性](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-demo-instability.png)

图44.5: 分隔超平面的不稳健性

解决这个问题的办法是，
不严格要求能够区分所有点，
而是将大多数点区分好，
结果更具有稳健性，
有少数点可以落在边界区域，
甚至于落入分隔超平面的错误一面。
这种方法称为支持向量判别法，
或者软分隔边界法。
见图[44.6](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svc-softma01)，
左图中的1号和8号点进入了分隔区域，
右图中的1号和8号点进入了分隔区域，
11和12号点进入了错误的一面。

![软分隔边界演示](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-demo-softmargin01.png)

图44.6: 软分隔边界演示

### 44.3.2 支持向量判别法

支持向量判别法仍然求出一个分隔超平面，
对待判的观测点，
按其在超平面的那一面判决其类属。
超平面的求法使用软分隔边界，
表述为如下的优化问题：
\[\begin{aligned}
& \max\_{\beta\_0, \beta\_1, \dots, \beta\_p, \epsilon\_1, \dots, \epsilon\_n}
\text{分隔边界} \text{ s.t.} \\
& y\_i (\beta\_0 + \beta\_1 x\_{i1} + \dots + \beta\_p x\_{ip})
\geq \text{分隔边界} \times (1 - \epsilon\_i), \ i=1,2,\dots,n . \\
& \sum\_{i=1}^p \beta\_i^2 = 1,
\quad\epsilon\_i \geq 0, i=1,\dots,n;\ \sum\_{i=1}^n \epsilon\_i \leq C
\end{aligned}\]
其中\(C\)是一个非负的调节参数，
\(C\)越大，
容许进入边界和错判的点的比例越大。
\(\epsilon\_1, \dots, \epsilon\_n\)是**松弛变量**，
使得对点在分割边界以外的要求略微放宽。

求得上述优化问题的解以后，
对新的观测\(\boldsymbol x^\* = (x\_1^\*, \dots, x\_p^\*)^T\),
只要计算函数
\(f(\boldsymbol x^\*) = \beta\_0 + \beta\_1 x\_1^\* + \dots x\_p^\*\)，
以\(f(\boldsymbol x^\*)\)的正负号决定将因变量判入\(1\)或者\(-1\)。

优化问题中的\(\epsilon\_i\)由第\(i\)个点与分隔区域的关系决定。
如果\(\epsilon\_i = 0\)，
则软分隔规则对第\(i\)个点不起作用，
第\(i\)个点仍然被分隔区域分开，
例如图[44.6](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svc-softma01)右图的3, 4, 5, 6，10和2, 7, 9号点。
当\(0 < \epsilon\_i < 1\)时，
第\(i\)个点可以进入分隔边界规定的分隔区域范围内，
如第1，8号点。
当\(\epsilon\_i > 1\)时，
第\(i\)个点可以在超平面的错误一侧，
如第11, 12号点。

调节参数\(C\)设置了将严格要求所有点在正确一侧且不能进入分隔区域的要求放松到多大程度。
当\(C=0\)时对应于最大分隔边界判别法，
不能有任何点在错误一侧，
也不能进入分隔边界区域。
对\(C>0\)，
至多有\(\text{floor}(C)\)个点可以位于分隔超平面的错误一侧。
增大\(C\)的值，
解出的边界宽度也会增大。
见图[44.7](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svc-softma02)，
各图形对应的调节参数由大到小，
过大的\(C\)使得错判点较多，
结果方差较小，偏差较大；
过小的\(C\)使得结果不稳健，
预测方差较大，偏差较小。
所以，
这里的调节参数与其它有监督学习方法中的调节参数类似，
应该在偏差与方差之间折衷，
可以用交叉验证方法求最优调节参数值。
\(C\)越小，模型复杂度越高。

![软分隔边界不同调节参数的影响](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-demo-softmargin02.png)

图44.7: 软分隔边界不同调节参数的影响

上面的优化问题的解有一个重要性质：
最终的解仅仅依赖边界点（即落在分隔区域的边界上的点）以及进入边界区域或者进入错误一面的点，
这些点称为**支持向量**；
那些被分隔区域完全正确地区分在两边的点不起作用。

从这个性质来看条件参数\(C\)的影响，
当\(C\)大的时候，
分隔边界很宽，
许多点进入了边界区域或者错误一面，
有许多个支持向量，
最终解由许多个点决定，
这时解的方差较低，
但对应于较简单的模型，所以可能会有较大偏差。
当\(C\)小的时候，
只有少数几个支持向量，
模型稳定性差，
有过度拟合危险，
方差可能较大。

支持向量判别法只依赖于少数支持向量这个性质与距离判别法很不同，
结果仅由靠近边界的点决定，
那些离边界很远的点则不起作用；
距离判别法则是由所有点决定。
逻辑回归判别法与支持向量判别法有类似的性质，
也是主要依赖那些边界附近的点。

## 44.4 支持向量机方法

支持向量判别法仅仅支持超平面作为分隔，
对于分隔区域是曲面的情形则不能处理。
支持向量机方法可以解决这个问题。

### 44.4.1 非线性边界方法

分隔超平面是线性边界，
对于图[44.8](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svm-nonlin01)这样的分隔边界非线性的问题无法解决，
其它的线性判别法也不能解决这样的非线性边界判别问题。

![线性边界无法解决的例子](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-curved01.png)

图44.8: 线性边界无法解决的例子

解决非线性问题的常用方法是增加非线性项如二次项、三次项。
比如，将自变量由\(x\_1, \dots, x\_p\)增加到
\[
x\_1, \dots, x\_p;
x\_1^2, \dots, x\_p^2
\]
则支持向量判别法的优化问题变成了
\[\begin{aligned}
& \max\_{\beta\_0, \beta\_{11}, \dots, \beta\_{p1}, \beta\_{12}, \dots, \beta\_{p2}, \epsilon\_1, \dots, \epsilon\_n}
\text{分隔边界} \text{ s.t.} \\
& y\_i (\beta\_0 + \sum\_{j=1}^p \beta\_{j1} x\_{ij} + \sum\_{j=1}^p \beta\_{j2} x\_{ij}^2)
\geq \text{分隔边界} \times (1 - \epsilon\_i), \ i=1,2,\dots,n . \\
& \sum\_{k=1}^2 \sum\_{j=1}^p \beta\_{jk}^2 = 1 \\
& \epsilon\_i \geq 0, \sum\_{i=1}^n \epsilon\_i \leq C .
\end{aligned}\]
这样得到的边界在\((x\_1, \dots, x\_p, x\_1^2, \dots, x\_p^2)\)所在的\(\mathbb R^{2p}\)空间内是线性的超平面，
但是在原来\((x\_1, \dots, x\_p)\)所在的\(\mathbb R^p\)空间内则是由
\(q(\boldsymbol x) = 0\)决定的曲面，
\(q(\boldsymbol x)\)是二次多项式函数。

还可以增加高次项和交叉项，
或者考虑其它的非线性变换。
增加非线性项的方法有无数多种，
一一测试是不现实的，
支持向量机方法则给出了一种增加非线性项的一般方法，
对应的边界可以快速求解。

### 44.4.2 支持向量机

支持向量机利用了Hilbert空间的方法将线性问题扩展为非线性问题。
线性的支持向量判别法可以通过\(\mathbb R^p\)的内积转化为判别函数如下的等价表示：
\[\begin{aligned}
f(\boldsymbol x)
= \beta\_0 + \sum\_{i=1}^n \alpha\_i \langle \boldsymbol x,
\boldsymbol x\_i \rangle .
\end{aligned}\]
其中\(\beta\_0, \alpha\_1, \dots, \alpha\_n\)是待定参数。
记
\[
\boldsymbol w
= \sum\_{i=1}^n \alpha\_i \boldsymbol x\_i,
\]
则
\[
f(\boldsymbol x)
= \beta\_0 + \langle \boldsymbol x,
\sum\_{i=1}^n \alpha\_i \boldsymbol x\_i \rangle
= \beta\_0 + \langle \boldsymbol x, \boldsymbol w \rangle .
\]
为了估计参数，
不需要用到各\(\boldsymbol x\_i\)的具体值，
而只需要其两两的内积值，
而且在判别函数中只有支持向量对应的\(\alpha\_i\)才非零，
记\(\mathcal S\)为支持向量点集，
则线性判别函数为
\[\begin{aligned}
f(\boldsymbol x)
= \beta\_0 + \sum\_{i \in \mathcal S} \alpha\_i \langle \boldsymbol x,
\boldsymbol x\_i \rangle .
\end{aligned}\]

支持向量机方法将\(\mathbb R^p\)中的内积\(\langle \boldsymbol x, \boldsymbol x' \rangle\)推广为如下的核函数值：
\[\begin{aligned}
K(\boldsymbol x, \boldsymbol x') .
\end{aligned}\]
存在从\(\mathbb R^p\)到某个希尔伯特空间\(H\)的映射\(\phi(\boldsymbol x)\)，
使得
\[
K(\boldsymbol x, \boldsymbol x')
= \langle \phi(\boldsymbol x),
\phi(\boldsymbol x') \rangle\_H,
\ \forall \boldsymbol x, \boldsymbol x' \in \mathbb R^p .
\]
核函数\(K(\boldsymbol x, \boldsymbol x')\)是度量两个观测点\(\boldsymbol x, \boldsymbol x'\)的相似程度的函数。
比如，
取
\[\begin{aligned}
K(\boldsymbol x, \boldsymbol x')
= \sum\_{j=1}^p x\_j x\_j' ,
\end{aligned}\]
就又回到了线性的支持向量判别法。

利用核代替内积后，
判别法的判别函数变成
\[\begin{aligned}
f(\boldsymbol x)
=& \beta\_0 + \sum\_{i \in \mathcal S}
\alpha\_i K(\boldsymbol x, \boldsymbol x\_i) \\
=& \beta\_0 + \sum\_{i \in \mathcal S} \alpha\_i
\langle \phi(\boldsymbol x), \phi(\boldsymbol x\_i) \rangle \\
=& \beta\_0 + \langle \phi(\boldsymbol x),
\sum\_{i \in \mathcal S} \alpha\_i \phi(\boldsymbol x\_i) \rangle \\
=& \beta\_0 + \langle \phi(\boldsymbol x), \boldsymbol w^H \rangle .
\end{aligned}\]
这在原始观测\(\boldsymbol x\_i\)的取值空间中是非线性函数，
但在特征空间\(H\)中是线性函数。

核有多种取法。
例如，
取
\[\begin{aligned}
K(\boldsymbol x, \boldsymbol x')
= \left\{
1 + \sum\_{j=1}^p x\_j x\_j'
\right\}^d
\end{aligned}\]
其中\(d>1\)为正整数，
称为**多项式核**，
则结果是多项式边界的判别法，
本质上是对线性的支持向量方法添加了高次项和交叉项。
前一小节就是\(d=2\)时的多项式核。
可以看出，
增加了二次项以后，
将\(\mathbb R^p\)空间的点\(\boldsymbol x = (x\_1, \dots, x\_p)\)映射到了\(\mathbb R^{2p}\)空间\(\phi(\boldsymbol x) = (x\_1, \dots, x\_p, x\_1^2, \dots, x\_p^2)\)，
考虑在高维的\(\mathbb R^{2p}\)空间的分隔超平面，
但在\(\boldsymbol x\)所在的\(\mathbb R^p\)这是分隔曲面。
判决函数可以表示为\(\mathbb R^{2p}\)中\(\phi(\boldsymbol x) = (x\_1, \dots, x\_p, x\_1^2, \dots, x\_p^2)\)的线性组合，
这又可以表示为内积形式：
\[
f(\boldsymbol x)
= \beta\_0 + \sum\_{i \in \mathcal S} \alpha\_i
\langle \phi(\boldsymbol x), \phi(\boldsymbol x\_i) \rangle
= \beta\_0 + \sum\_{i \in \mathcal S} \alpha\_i
K(\boldsymbol x, \boldsymbol x\_i) .
\]

理论研究表明，给定核函数\(K(\cdot, \cdot)\)后，
必存在一个Hilbert空间\(H\)以及\(\phi: \mathbb R^p \to H\)，使得
\[
K(\boldsymbol x, \boldsymbol x')
= \langle \phi(\boldsymbol x), \phi(\boldsymbol x') \rangle\_H ,
\ \forall \boldsymbol x, \boldsymbol x \in \mathbb R^p .
\]
于是，
给了一个核函数，
就可以将原来的\(\mathbb R^p\)空间的判别问题，
通过映射\(\phi\)映射为了\(H\)空间的线性判别问题，
这样得到的判别函数在\(H\)空间是线性的，
但在原始的\(\mathbb R^p\)中是非线性的。

训练这样的SVM时只要计算观测两两的\(\binom{n}{2}\)个核函数值。
为什么不直接增加非线性项？
原因是计算这些核函数值计算量是确定的，
而增加许多非线性项，
则可能有很大的计算量，
映射\(\phi(\boldsymbol x)\)的值域\(H\)可能是高维甚至于无穷维空间，
计算核函数值而不是计算映射\(\phi(\boldsymbol x)\)效率更高。

支持向量机的理论基于再生核希尔伯特空间(RKHS)，
可参见[44.7](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#statl-svm-pdkernels)，
以及([Trevor Hastie 2009](#ref-Hastie-learng09))节5.8和节12.3.3。

图[44.9](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svm-poly01)的左图是用多项式核解决非线性边界判别问题的演示。
两条粗实线是分隔曲面，
虚线是边界区域的边缘。

![多项式核与径向核判别的例子](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/figs/svm-polydemo01.png)

图44.9: 多项式核与径向核判别的例子

另一种常用的核是**径向核**(radial kernel)，
定义为
\[\begin{aligned}
K(\boldsymbol x, \boldsymbol x')
= \exp\left\{ - \gamma
\sum\_{j=1}^p (x\_j - x\_j')^2
\right\}
\end{aligned}\]
\(\gamma\)为正常数。
当\(\boldsymbol x\)和\(\boldsymbol x'\)分别落在以原点为中心的两个超球面上时，
其核函数值不变。
[44.9](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-learn-svm.html#fig:statl-svm-svm-poly01)的右图是用径向核支持向量机进行判别的演示。

使用径向核时，
判别函数为
\[\begin{aligned}
f(\boldsymbol x)
= \beta\_0 + \sum\_{i \in \mathcal S}
\alpha\_i \exp\left\{ - \gamma
\sum\_{j=1}^p (x\_{j} - x\_{ij})^2
\right\}
\end{aligned}\]
对一个待判别的观测\(\boldsymbol x^\*\)，
如果\(\boldsymbol x^\*\)距离训练观测点\(\boldsymbol x\_i\)较远，
则\(K(\boldsymbol x^\*, \boldsymbol x\_i)\)的值很小，
\(\boldsymbol x\_i\)对\(\boldsymbol x^\*\)的判别基本不起作用。
这样的性质使得径向核方法具有很强的局部性，
只有离\(\boldsymbol x^\*\)很近的点才对其判别起作用。

## 44.5 支持向量机用于Heart数据

考虑心脏病数据Heart的判别。
共297个观测，
随机选取其中207个作为训练集，
90个作为测试集。

```
library(rsample)

Heart <- read_csv(
  "data/Heart.csv",
  show_col_types = FALSE) |>
  dplyr::select(-1) |>
  mutate(
    AHD = factor(AHD, levels=c("Yes", "No"))
  )
```

```
## New names:
## • `` -> `...1`
```

```
Heart <- na.omit(Heart)

set.seed(101)
heart_split <- initial_split(
  Heart, prop = 0.50)
heart_train <- training(heart_split)
heart_test <- testing(heart_split)
test.y <- heart_test$AHD
```

定义一个错判率函数：

```
classifier.error <- function(truth, pred){
  tab1 <- table(truth, pred)
  err <- 1 - sum(diag(tab1))/sum(c(tab1))
  err
}
```

### 44.5.1 线性的SVM

支持向量判别法就是SVM取多项式核，
阶数\(d=1\)的情形。
需要一个调节参数`C`，
`C`越大，
分隔边界越窄，
过度拟合危险越大。

先随便取调节参数`C=1`试验支持向量判别法：

```
library(kernlab)
```

```
## 
## Attaching package: 'kernlab'
```

```
## The following object is masked from 'package:purrr':
## 
##     cross
```

```
## The following object is masked from 'package:ggplot2':
## 
##     alpha
```

```
res.svc <- ksvm(
  AHD ~ ., 
  data = heart_train, 
  kernel="vanilladot", 
  C = 1, 
  scaled = TRUE)
```

```
##  Setting default kernel parameters
```

```
fit.svc <- predict(res.svc)
summary(res.svc)
```

```
## Length  Class   Mode 
##      1   ksvm     S4
```

计算拟合结果并计算错判率：

```
tab1 <- table(
  truth = heart_train$AHD, 
  fitted = fit.svc); tab1
```

```
##      fitted
## truth Yes No
##   Yes  69 10
##   No   12 57
```

```
cat("SVC错判率：", 
  round(classifier.error(heart_train$AHD, fit.svc), 2), "\n")
```

```
## SVC错判率： 0.15
```

超参数调优方法参见[47.3](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/stat-tidy-modtune.html#stat-tidy-tune)。

### 44.5.2 多项式核SVM

```
res.svm1 <- ksvm(
  AHD ~ ., 
  data = heart_train, 
  kernel="polydot", 
  kpar = list(degree = 2),
  C = 0.1, 
  scale=TRUE)
fit.svm1 <- predict(res.svm1)
summary(res.svm1)
```

```
## Length  Class   Mode 
##      1   ksvm     S4
```

```
tab1 <- table(truth = heart_train$AHD, fitted = fit.svm1); tab1
```

```
##      fitted
## truth Yes No
##   Yes  77  2
##   No    2 67
```

```
cat("2阶多项式核SVM错判率：", 
  round(classifier.error(heart_train$AHD, fit.svm1), 2), "\n")
```

```
## 2阶多项式核SVM错判率： 0.03
```

注意这是拟合情况。
在测试集上的预测错误率：

```
pred.svm1t <- predict(res.svm1, heart_test)
tab1 <- table(truth = heart_test$AHD, pred = pred.svm1t); tab1
```

```
##      pred
## truth Yes No
##   Yes  46 12
##   No   26 65
```

```
cat("测试集上2阶多项式核SVM错判率：", 
  round(classifier.error(heart_test$AHD, pred.svm1t), 2), "\n")
```

```
## 测试集上2阶多项式核SVM错判率： 0.26
```

### 44.5.3 径向核SVM

径向核需要的参数为\(\sigma\)值。
取参数`sigma = 0.1`。

```
res.svm3 <- ksvm(
  AHD ~ ., 
  data = heart_train, 
  kernel="rbfdot", 
  kpar = list(sigma = 0.1),
  C = 0.1, 
  scaled = TRUE)
fit.svm3 <- predict(res.svm3)
summary(res.svm3)
```

```
## Length  Class   Mode 
##      1   ksvm     S4
```

```
tab1 <- table(truth = heart_train$AHD, 
  fitted = fit.svm3); tab1
```

```
##      fitted
## truth Yes No
##   Yes  74  5
##   No   32 37
```

```
cat("径向核（sigma=0.1, cost=0.1）SVM拟合错判率：", 
  round(classifier.error(heart_train$AHD, fit.svm3), 2), "\n")
```

```
## 径向核（sigma=0.1, cost=0.1）SVM拟合错判率： 0.25
```

## 44.6 多分类的支持向量机

当因变量不止两个类的时候，
从分隔超平面推广过来的支持向量机分类方法很难直接推广到多个类。
所以最常用的方法是按两类处理，
或者是两两判别，
或者是一个类相对于其余类判别。

### 44.6.1 两两判别

设因变量有\(K\)个类，为每两个类都分别用SVM构造判别函数，
得到\(m= {{K}\choose{2}}=K(K-1)/2\)个判别函数。
对于待判样品\(\boldsymbol x^\*\)，
用这些判别函数每一个都判一遍，
得到\(m\)个判别结果，
然后这\(m\)个结果投票，
那个类别的票最高就判入哪一类。

### 44.6.2 一对多判别

设因变量有K个类，
对其中的每一个类，
都以此类以及此类之外的其它类作为一个两类判别问题，
建立\(K\)个判别函数。
对于待判样品\(\boldsymbol x^\*\)，
计算\(K\)个判别函数值，
以判别函数值最大的类作为最后的判别结果。

### 44.6.3 多分类支持向量机鸢尾花数据计算实例

考虑著名的鸢尾花数据集的判别问题。
所有数据用作训练集合。

取径向基，用指定的调整参数：

```
res.msvm1 <- ksvm(
  Species ~ ., 
  data = iris, 
  kernel="rbfdot",
  kpar = list(sigma = 0.01),
  C = 1, 
  scaled=TRUE)
summary(res.msvm1)
```

```
## Length  Class   Mode 
##      1   ksvm     S4
```

```
fit.msvm1 <- predict(res.msvm1)
tab1 <- table(truth=iris[,"Species"], fitted=fit.msvm1); tab1
```

```
##             fitted
## truth        setosa versicolor virginica
##   setosa         50          0         0
##   versicolor      0         46         4
##   virginica       0         11        39
```

```
cat("径向核（gamma=0.01, cost=1）多类SVM错判率：", 
    round(1 - (sum(diag(tab1)))/ sum(c(tab1)), 2), "\n")
```

```
## 径向核（gamma=0.01, cost=1）多类SVM错判率： 0.1
```

## 44.7 附录：特征空间、正定核、再生核希尔伯特空间

参考：

* ([Trevor Hastie 2009](#ref-Hastie-learng09))节5.8和节12.3.3。
* Jean Gallier and Jocelyn Quaintance(2019).
  Algebra, Topology, Differential Calculus,
  and Optimization Theory
  For Computer Science and Machine Learning, Chapter 54.
  <http://www.cis.upenn.edu/~jean/math-basics.pdf>
* Mohri, Mehryar / Rostamizadeh, Afshin / Talwalkar, Ameet(2018).
  Foundations of Machine Learning, Chapter 6.
  2nd Ed., MIT Press.
  <https://cs.nyu.edu/~mohri/mlbook/>

### 44.7.1 正定核

#### 44.7.1.1 介绍

设\(\mathcal X\)为非空的集合，
要考虑\(\mathcal X\)中点的许多非线性变换作为新的判别用自变量，
希望将\(\mathcal X\)的非线性函数转换为经过非线性变换后的线性函数。
设存在映射\(\phi(x): \mathcal X \to \mathcal H\)，
其中\(\mathcal H\)为有限维内积空间或无穷维希尔伯特空间，
具有内积\(\langle \cdot, \cdot \rangle\_{\mathcal H}\)。
称\(\mathcal H\)为\(\mathcal X\)的**特征空间**(feature space)，
称\(\phi\)为**特征映射**(feature map)。
\(\mathcal H\)可以是复数域或实数域上的希尔伯特空间。

定义
\[
K(x, y)
= \langle \phi(x), \phi(y) \rangle\_{\mathcal H},
\ \forall x, y \in \mathcal X,
\]
称\(K(\cdot, \cdot)\)为一个对称正定核函数(positive definite symmetric kernel, PDS核)，
简称正定核。

**例44.1** 设\(\mathcal X = \mathbb R^2\)，
\(\phi(x) = (x\_1^2, x\_2^2, \sqrt{2} x\_1 x\_2) \in \mathbb R^3\)。
令
\[
K(x, y) = \langle \phi(x), \phi(y) \rangle .
\]
有
\[\begin{aligned}
K(x, y)
=& x\_1^2 y\_1^2 + x\_2^2 y\_2^2 + \sqrt{2} x\_1 x\_2 \sqrt{2} y\_1 y\_2 \\
=& (x\_1 y\_1 + x\_2 y\_2)^2 \\
=& \langle x, y \rangle^2 .
\end{aligned}\]
\(K(x,y)\)是一个正定核，
涉及到从原始空间\(\mathbb R^2\)到特征空间\(\mathbb R^3\)的映射\(\phi(\cdot)\)。

对任意\(n\geq 1\)和任意\(x\_1, x\_2, \dots, x\_n \in \mathcal X\)，
有
\[
\sum\_{i=1}^n \sum\_{j=1}^n x\_i K(x\_i, x\_j) \bar x\_j
\geq 0 .
\]

这蕴含\(K(x, x) \geq 0\), \(K(x, y) = \overline{K(y, x)}\)，
不要求\(x \neq 0\)时\(K(x, x) > 0\)，
所以严格来说应该称为“对称半正定核”，
但是“正定核”的叫法已经广泛采用。

令
\[
K\_S = (K(x\_j, x\_i))\_{i=1,\dots, n; j=1, \dots,n},
\]
则\(K\_S\)是厄米特阵：
\[
\boldsymbol u^\* K\_S \boldsymbol u \geq 0,
\ \forall \boldsymbol u \in \mathbb C^n .
\]

这给出了正定核的等价定义：
设\(K(x, y)\)是\(\mathcal X \times \mathcal X \to \mathbb C\)的映射，
对任意\(n\)和任意\(x\_1, x\_2, \dots, x\_n \in \mathcal X\)，
都有
\[
\sum\_{i=1}^n \sum\_{j=1}^n x\_i K(x\_i, x\_j) \bar x\_j
\geq 0 ,
\]
则称\(K\)为一个正定核函数。
这时\(K(x,x) \geq 0\)，
\(K\_S\)矩阵为厄米特阵。

若\(K\)为\(\mathcal X \times \mathcal X \to \mathbb R\)的映射，
则\(K\)是正定核，
当且仅当\(K(x, y) = K(y, x)\), \(\forall x, y \in \mathcal X\)，
且对任意\(n\)和任意\(x\_1, x\_2, \dots, x\_n \in \mathcal X\)，
有
\[
\sum\_{i=1}^n \sum\_{j=1}^n x\_i K(x\_i, x\_j) x\_j
\geq 0 ,
\]
即\(K\_S\)矩阵对称半正定。
称这样的正定核为**实对称正定核**，
仍简称正定核。

因为正定核是\(\mathcal H\)上的内积，
所以满足Cauchy-Schwarz不等式：
\[
|K(x, y)|^2
\leq K(x,x) K(y, y),
\ \forall x, y \in \mathcal X .
\]

#### 44.7.1.2 运算封闭性

正定核的运算封闭性（见([Mohri, Rostamizadeh, and Talwalkar 2018](#ref-Mohri2018:Machine-learn))定理6.10）：

* 两个正定核的和仍为正定核。
* 两个正定核的乘积仍为正定核。
* 正定核的正常数倍仍为正定核。
* 正定核的张量积仍为正定核。即若\(K\_1(x, y)\), \(K\_2(x', y')\)为正定核，
  则\(K(u, v) = K\_1(x,y) K\_2(x', y')\)为正定核，
  其中\(u = (x, x')\), \(v = (v, v')\)。
* 正定核的点极限仍为正定核。
  即设\(K\_n(\cdot, \cdot)\)为正定核且\(\lim\_{n\to\infty} K\_n(x, y) = K(x,y)\),
  \(\forall x, y\)，则\(K(x,y)\)为正定核。
* 设映射\(\psi: \mathcal X \to \mathbb R^n\)，
  \(K\_0\)是\(\mathbb R^n \times \mathbb R^n \to \mathbb C\)的正定核，
  则\(K(x,y) = K\_0(\psi(x), \psi(y))\)是正定核。
* 设幂级数\(h(x) = \sum\_{j=0}^\infty a\_j x^j\)在\((-\rho, \rho)\)收敛(\(\rho>0\))，
  \(X(x,y)\)为正定核，取值在\((-\rho, \rho)\)内，
  则\(h(K(x,y))\)为正定核。

正定核的标准化（见([Mohri, Rostamizadeh, and Talwalkar 2018](#ref-Mohri2018:Machine-learn))引理6.9）：
设\(K(x,y)\)为正定核，
定义
\[
\tilde K(x,y) = \begin{cases}
0, & \text{若} K(x,x)=0 \text{ 或 } K(y,y)=0; \\
\frac{K(x,y)}{\sqrt{K(x,x) K(y, y)}}, & \text{否则}.
\end{cases}
\]
称\(\tilde K\)为\(K\)的标准化，
\(\tilde K\)也是正定核。

#### 44.7.1.3 一些正定核

对\(\mathcal X \subset R^n\)，
设\(A\)是对称半正定阵，
则\(K(x, y) = x^T A y\)是正定核。
事实上，
\[
\sum\_{i,j} \alpha\_i \alpha\_j \boldsymbol x\_i^T A \boldsymbol x\_j
= \sum\_{i,j} (\alpha\_i \boldsymbol x\_i)^T A (\alpha\_j \boldsymbol x\_j)
\geq 0 .
\]

对\(\mathcal X \subset R^n\)，
\(K(\boldsymbol x, \boldsymbol y) = \boldsymbol x^T \boldsymbol y\)（欧式空间内积）是正定核。
这是上一个正定核当\(A=I\)时的特例。

对映射\(f: \mathcal X \to \mathbb C\)，
\(K(\boldsymbol x, \boldsymbol y) = f(\boldsymbol x) \overline{f(\boldsymbol y)}\)是正定核。
事实上，
\[
\sum\_{i,j} \alpha\_i \alpha\_j f(\boldsymbol x\_i) \overline{f(\boldsymbol x\_j)}
= (\sum\_i \alpha\_i f(\boldsymbol x\_i))
\overline{(\sum\_j \alpha\_j f(\boldsymbol x\_j))}
\geq 0.
\]

对\(\ell > 0\)，
\(\mathcal X \subset R^n\)，
令
\[
K(\boldsymbol x, \boldsymbol y)
= \exp(- \| \boldsymbol x - \boldsymbol y \|^2 / (2 \ell^2)),
\]
这是一个正定核，称为高斯核或者径向基函数。

证明：
取\(K\_1(\boldsymbol x, \boldsymbol y) = \exp(- \boldsymbol x^T \boldsymbol y / \ell^2)\)，
易见\(K\)是\(K\_1\)的标准化。
只要证明\(K\_1\)是正定核。
而\(K\_2(\boldsymbol x, \boldsymbol y) = \boldsymbol x^T \boldsymbol y / (2 \ell^2)\)是正定核，
幂级数\(exp(x) = \sum\_{j=0}^\infty \frac{x^j}{j!}\)收敛且系数为正，
所以\(exp(K\_2(\boldsymbol x, \boldsymbol y))\)是正定核。

### 44.7.2 再生核希尔伯特空间

设\(K\)是\(\mathcal X \times \mathcal X \to \mathbb C\)的正定核（包括实正定核）。
是否存在希尔伯特空间\(\mathcal H\)和映射\(\phi: \mathcal X \to \mathcal H\)使得
\[
K(x, y) = \langle \phi(x), \phi(y) \rangle ?
\]

这个空间是存在的，
称为\(K\)决定的再生核希尔伯特空间(RKHS, reproducing kernel Hilbert space)。
一般地，
可以构造\(\mathcal X\)到\(\mathbb C\)的函数组成的空间作为\(\mathcal H\)。
某些具体的\(\mathcal H\)可以通过同构映射转换成其它类型的H空间。

设\(K\)是\(\mathcal X \times \mathcal X \to \mathbb C\)的正定核，
\(\forall x \in \mathcal X\)，
定义\(\mathcal X \to \mathbb C\)的函数\(k\_x\)为
\[
k\_x(y) = K(x, y),
\ \forall y \in \mathcal X .
\]
记\(\mathbb C^{\mathcal X}\)为\(\mathcal X \to \mathbb C\)的函数组成的线性空间，
则\(\{k\_x: x \in \mathcal X \}\)是\(\mathbb C^{\mathcal X}\)的子集。
令\(\mathcal H\_0\)为\(\{k\_x: x \in \mathcal X \}\)的所有有限线性组合在\(\mathbb C^{\mathcal X}\)中构成的子线性空间，
可以定义\(\mathcal X \to \mathcal H\_0\)的映射\(\phi\)使得
\[
\phi(x) = k\_x \in \mathcal H\_0,
\ \forall x \in \mathcal X .
\]
在\(\mathcal H\_0\)上可以定义内积，使得
\[
\langle \phi(x), \phi(y) \rangle\_{\mathcal H\_0}
= K(x, y),
\ \forall x, y \in \mathcal X .
\]
设\(\mathcal H\)为\(\mathcal H\_0\)的闭包，
则\(\mathcal H\)为希尔伯特空间，
可以定义映射\(\eta: \mathcal H \to \mathbb C^{\mathcal X}\)，使得
\[
\eta(f)(x)
= \langle f, k\_x \rangle,
\ \forall x \in \mathcal X,
\ \forall f \in \mathcal H ,
\]
则\(\eta\)是\(\mathcal H\)到\(\mathbb C^{\mathcal X}\)的单射和线性映射，
于是\(\eta\)构成从\(H\)到\(\mathbb C^{\mathcal X}\)的一个子希尔伯特空间的同构映射，
可以将\(\mathcal H\)看作\(\mathbb C^{\mathcal X}\)的一个子希尔伯特空间。
对于\(\mathcal H\)上的内积仍有
\[
\langle \phi(x), \phi(y) \rangle\_{\mathcal H}
= K(x, y),
\ \forall x, y \in \mathcal X .
\]
对任意\(f \in \mathcal H\_0\)，
有
\[
\langle f, k\_x \rangle
= f(x),
\ \forall x \in \mathcal X,
\]
上式称为再生性，
即通过特征空间\(\mathcal H\)的内积，
也即核函数\(K\)，
可以恢复（再生）\(\mathcal H\)中的元素。

当\(K\)是实对称正定核时，
\(\mathcal H\_0\)为实数域上的线性空间，
\(\mathcal H\)为实数域上的希尔伯特空间。

### References

Mohri, Mehryar, Afshin Rostamizadeh, and Ameet Talwalkar. 2018. *Foundations of Machine Learning*. 2nd Edition. MIT Press.

Trevor Hastie, Jerome Friedman, Robert Tibshirani. 2009. *The Elements of Statistical Learning*. 2nd Ed. Springer.