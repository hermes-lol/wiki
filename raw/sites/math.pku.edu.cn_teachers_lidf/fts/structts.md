---
crawl_time: '2026-01-17 14:30:54'
framework: sphinx
title: 30 局部水平模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 30 局部水平模型

状态空间模型是时间序列分析领域中一类强大、灵活、多样的模型，
配合卡尔曼滤波技术，可以涵盖ARIMA模型、许多非平稳的、带有外生变量的模型，
比前面所述的线性时间序列模型更为灵活。
R扩展包statespacer实现了许多基于线性高斯状态空间模型的模型，
并且可以自定义模型。

参考：

* ([Durbin and Koopman 2012](#ref-DurbinKoopman2012:TSASSM))
* ([Beijers 2020](#ref-statespacer))
* ([Tsay 2010](#ref-Tsay10:FTS3ed))

作为入门，
先介绍一个局部水平模型。
这个模型很简单，
所以可以用来演示状态空间模型的表示和估计。

## 30.1 模型

设\(\{ y\_t, t=1,2,\dots,T\}\)为时间序列，
满足如下模型
\[\begin{align}
y\_t =& \mu\_t + e\_t, \ \{e\_t\} \sim \text{iid N}(0, \sigma\_e^2), \ t=1,2,\dots,n,
\tag{30.1}\\
\mu\_{t+1} =& \mu\_t + \eta\_t, \ \{\eta\_t\} \sim \text{iid N}(0, \sigma\_{\eta}^2),
\tag{30.2}
\end{align}\]
其中\(\{e\_t\}\)与\(\{\eta\_t\}\)相互独立，
初始值\(\mu\_1\)为给定值或者是服从正态分布的随机变量，
且与\(\{e\_t, \eta\_t, t>0\}\)相互独立。
称\(\{\mu\_t\}\)为\(\{y\_t\}\)的水平，
模型中\(\{y\_t\}\)可观测而\(\{\mu\_t\}\)不可观测。

方程[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-obs)-[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-sta)是线性高斯状态空间模型的一个特例。
\(\{\mu\_t\}\)代表系统在\(t\)时刻所处的状态，
不可观测，方程[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-sta)称为**状态方程**，
描述了系统的内在演变规律；
\(\{y\_t\}\)代表系统在\(t\)时刻的观测值或输出值，
[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-obs)称为**观测方程**，
\(\{e\_t\}\)是观测误差，
仅影响到\(t\)时刻，
是一个瞬态的误差或噪声。

这个模型称为局部水平模型，
也是“结构时间序列模型”的一个特例。

对于实际数据，
需要估计其中的参数\(\sigma\_e^2\)和\(\sigma\_\eta^2\)。
设\(\alpha\_1 \sim \text{N}(a\_1, P\_1)\)，
易见\(\boldsymbol y = (y\_1, \dots, y\_T)^T\)服从多元正态分布\(\text{N}(\boldsymbol\alpha, \Omega)\)，
其中\(\boldsymbol\alpha\)和\(\Omega\)是\(\sigma\_e^2\)和\(\sigma\_\eta^2\)的函数。
可以写出其似然函数
\[
(2\pi)^{-T/2} |\Omega|^{-1/2}
\exp(-\frac{1}{2} (\boldsymbol y - \boldsymbol\alpha)^T
\Omega^{-1} (\boldsymbol y - \boldsymbol\alpha)) .
\]
当\(T\)较大时计算似然函数涉及到\(T \times T\)矩阵\(\Omega\)的求逆，
计算量较大而且容易发生数值不稳定。
利用这一章讲的Kalman滤波算法可以将普通方阵的求逆问题，
转换成对角阵的求逆问题，
简化了似然函数计算而且数值稳定性得到保障。

如果参数\(\sigma\_e^2\)和\(\sigma\_\eta^2\)已知，
在给定\(\boldsymbol y\)后可以进行预测，
或者对局部水平\(\mu\_t\)进行推断。

**例30.1** 考虑Alcoa股票日现实波动率数据，
时间期间为2003-01-02到2004-05-07，
共340个观测。
日现实波动率是用交易日内每隔10分钟的对数收益率的平方和计算的。

```
da <- read_table("aa-3rv.txt", 
  col_names = FALSE,
  show_col_types = FALSE)
ts.alcoa <- ts(log(da[[2]]))
```

```
plot(ts.alcoa, main="Alcoa股票日现实波动率对数值数据")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7010-struct_files/figure-html/structts-llm-exal-tsp-1.png)

可以用局部水平模型获得\(\{\mu\_t\}\)，
作为去噪声的波动率估计。

## 30.2 局部水平模型与ARIMA模型的关系

注意到
\[
y\_t - y\_{t-1}
= \eta\_{t-1} + e\_t - e\_{t-1},
\]
记\(\xi\_t = \eta\_{t-1} + e\_t - e\_{t-1}\)，
易见\(\xi\_t\)的自相关函数在1以后截尾，
所以\(\xi\_t\)服从一个MA(1)模型。
由多元正态分布的性质可知存在\(\{a\_t \} \sim \text{N}(0, \sigma\_a^2)\)和\(|\theta|\leq 1\)使得
\[
y\_t - y\_{t-1}
= a\_t + \theta a\_{t-1},
\]
从而\(y\_t\)服从一个高斯的ARIMA(0,1,1)模型。

由
\[\begin{aligned}
\text{Var}(\xi\_t) =& 2 \sigma\_e^2 + \sigma\_{\eta}^2 = \sigma\_a^2 (1 + \theta^2), \\
\text{Cov}(\xi\_t, \xi\_{t-1}) =& -\sigma\_e^2 = \theta \sigma\_a^2,
\end{aligned}\]
可以求解出\(\theta\)且使得\(-1 < \theta < 0\):
\[\begin{aligned}
\theta = \frac{b + \sqrt{b^2 + 4}}{2},
\text{ 其中 }
b = -2 - \frac{\sigma\_{\eta}^2}{\sigma\_e^2} < -2 .
\end{aligned}\]

反过来，如果\(y\_t\)服从一个ARIMA(0,1,1)模型且\(\theta\)为负值，
则可以将其写成[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-obs)-[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-sta)的形式。
如果\(\theta\)为正值，
可以写成\(\sigma\_e=0\)的形式。

**例30.2** 对Alcoa股票日现实波动率对数值数据用ARIMA建模。

```
mod <- arima(ts.alcoa,
  order=c(0,1,1))
mod
```

```
## 
## Call:
## arima(x = ts.alcoa, order = c(0, 1, 1))
## 
## Coefficients:
##           ma1
##       -0.8582
## s.e.   0.0397
## 
## sigma^2 estimated as 0.2688:  log likelihood = -258.98,  aic = 521.95
```

得到的ARIMA(0,1,1)模型为：
\[\begin{aligned}
(1-B) y\_t = (1 - 0.8582 B) a\_t,
\ a\_t \sim \text{WN}(0, 0.2688) .
\end{aligned}\]

通过\(\sigma\_e^2\), \(\sigma\_{\eta}^2\)与\(\theta\), \(\sigma\_a^2\)的关系，
可以解出
\[
\sigma\_e^2 = -\theta \sigma\_a^2 = 0.2307,
\quad
\sigma\_{\eta}^2 = \sigma\_a^2(1 + \theta^2) - 2\sigma\_e^2
=0.0054
\]

可见噪声严重超过了信号的扰动。

下面用R的statespacer扩展包估计局部水平模型。

```
library(statespacer)
ssr1 <- statespacer(
  y = cbind(as.vector(ts.alcoa)),
  local_level_ind = TRUE,
  initial = rep(0.5*log(var(ts.alcoa)), 2),
  verbose = TRUE)
```

```
## Starting the optimisation procedure at: 2024-05-21 15:26:28
```

```
## initial  value 1.022439 
## iter  10 value 0.765458
## iter  20 value 0.764395
## final  value 0.764395 
## converged
```

```
## Finished the optimisation procedure at: 2024-05-21 15:26:28
```

```
## Time difference of 0.0222549438476562 secs
```

上面的程序中输入数据需要是矩阵形式，
每列为一个时间序列分量，
一元时间序列也需要输入为仅有一列的矩阵。
选项`local_level_ind = TRUE`表示有局部水平成分。
`initial`给出超参数初值选取，
为了保证\(\sigma\_e^2\)和\(\sigma\_{\eta}^2\)估计为正数，
模型中使用了其对数值作为参数，
所以初值取了对数。

查看估计的\(\sigma\_e^2\)和\(\sigma\_{\eta}^2\):

```
c(sigmasqr_e = ssr1$system_matrices$H$H,
  sigmasqr_level = ssr1$system_matrices$Q$level)
```

```
##     sigmasqr_e sigmasqr_level 
##    0.230623632    0.005404681
```

与从ARIMA模型换算的结果基本相同。
statespacer利用了([Durbin and Koopman 2012](#ref-DurbinKoopman2012:TSASSM))的模型和记号，
\(H\)表示观测方程误差项方差阵，
\(Q\)表示系统方程误差项方差阵。
设`statespacer()`的输出为`ssr`，
这是一个列表，
其中`ssr$system_matrices$H$H`是观测方程的方差阵估计，
这里即\(\sigma\_e^2\)，
但保存成\(1 \times 1\)矩阵；
`ssr$system_matrices$Q`是与系统方程方差阵有关的输出，
而局部水平的状态方程对应的误差项的方差阵则为`ssr$system_matrices$Q$level`，
这里就是\(\sigma\_{\eta}^2\)，
保存成\(1 \times 1\)矩阵。

## 30.3 滤波、平滑和预报

对状态空间模型如[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-obs)-[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-sta)，
输入数据\(\{y\_t, t=1,2,\dots, n \}\)，
如果认为模型中参数（这里是\(\sigma\_e^2\)和\(\sigma\_\eta^2\)）已知，
经常讨论如下的统计推断问题：

* **滤波**：计算给定\(\{y\_1, \dots, y\_t \}\)时\(\mu\_t\)的条件分布；
* **平滑**：计算给定\(\{y\_1, \dots, y\_n \}\)时\(\{\mu\_1, \mu\_2, \dots, \mu\_n \}\)的条件分布；
* **预报**：计算给定\(\{y\_1, \dots, y\_t \}\)时\(\mu\_{t+h}\)或\(y\_{t+h}\)(\(h>0\))的条件分布。

设\(\mu\_1 \sim \text{N}(a\_1, P\_1)\)，
且与扰动序列独立。
由正态分布的性质，
状态空间模型[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-obs)-[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:structts-llm-mod-sta)是高斯过程，
其条件分布仍为高斯分布，
只要给出条件期望和条件方差（联合分布还需要条件协方差）。
条件期望为最小均方误差估计，
也是最小方差线性无偏估计。

当参数已知，
记\(y\_{1:t} = (y\_1, \dots, y\_t)\),
易见：

* 滤波需要计算\(E(\mu\_t | y\_{1:t})\)和\(\text{Var}(\mu\_t | y\_{1:t})\)；
* 平滑需要计算\(E(\mu\_t | y\_{1:n})\)和\(\text{Var}(\mu\_t | y\_{1:n})\)，
  对于联合分布还需要计算\(\text{Cov}(\mu\_t, \mu\_s | y\_{1:n})\)；
* 预报需要计算\(E(\mu\_{t+h} | y\_{1:t})\)和\(\text{Var}(\mu\_{t+h} | y\_{1:t})\)，
  或\(E(y\_{t+h} | y\_{1:t})\)和\(\text{Var}(y\_{t+h} | y\_{1:t})\)。

\(\mu\_t\)在\(y\_{1:s}\)下的条件分布完全由条件期望和条件方差决定。
记\(\mu\_{t|s} = E(\mu\_t | y\_{1:s})\),
记\(\Sigma\_{t|s} = \text{Var}(\mu\_t | y\_{1:s})\)。
记\(y\_{t|s} = E(y\_t | y\_{1:s})\)。
特别地，
记\(a\_t = E(\mu\_t | y\_{1:t-1})\),
\(P\_t = \text{Var}(\mu\_t | y\_{1:t-1})\)。
由高斯分布性质，
条件方差都是非随机的。

记
\[
v\_t = y\_t - E(y\_t | y\_{1:t-1}),
\]
这是对\(y\_t\)做最优一步预报时的误差，
显然\(E v\_t = 0\)，
令
\[
F\_t = E v\_t^2 = \text{Var}(v\_t),
\]
由多元正态分布性质，
\(v\_t\)与\(y\_{1:t-1}\)独立，
所以也有
\[\begin{aligned}
F\_t =& \text{Var}(v\_t)
= E(v\_t^2)
= E(v\_t^2 | y\_{1:t-1}) \\
=& E[ (y\_t - E(y\_t | y\_{1:t-1}))^2 | y\_{1:t-1}] \\
=& \text{Var}(y\_t | y\_{1:t-1}) .
\end{aligned}\]

于是由观测方程可得
\[\begin{align}
y\_{t|t-1} =& E(y\_t | y\_{1:t-1})
= E(\mu\_t + e\_t | y\_{1:t-1}) = \mu\_{t|t-1} = a\_t, \\
v\_t =& y\_t - y\_{t|t-1}
= y\_t - a\_t,
\tag{30.3} \\
F\_t =& \text{Var}(y\_t - y\_{t|t-1} | y\_{1:t-1})
= \text{Var}(\mu\_t + e\_t - \mu\_{t|t-1} | y\_{1:t-1}) \\
=& \text{Var}(\mu\_t - \mu\_{t|t-1} | y\_{1:t-1})
+ \text{Var}(e\_t | y\_{1:t-1}) \\
=& \Sigma\_{t|t-1} + \sigma\_e^2
= P\_{t} + \sigma\_e^2 .
\tag{30.4}
\end{align}\]

由多元正态分布性质可知预报误差\(v\_t\)与\(y\_{1:s}\)(\(s<t\))独立，
所以
\[
\text{Cov}(v\_t, y\_s) = 0, \ t > s .
\]
从而\(\{v\_t, t=1,2, \dots, n \}\)相互独立。
由多元正态性质，
\[
\sigma(y\_1, \dots, y\_{t-1}, y\_t )
= \sigma(y\_{1:t-1}, v\_t ) .
\]
其中\(\sigma(\cdot)\)表示由其中的随机变量所生成的最小\(\sigma\)代数。

对随机向量\(\boldsymbol X, \boldsymbol Y\)，
记\(\boldsymbol\mu\_{\boldsymbol X} = E \boldsymbol X\),
\(\Sigma\_{\boldsymbol X \boldsymbol X} = \text{Var}(\boldsymbol X)\)，
\(\Sigma\_{\boldsymbol X \boldsymbol Y} = \text{Cov}(\boldsymbol X, \boldsymbol Y)\)。
从多元正态分布性质有如下定理([Tsay 2010](#ref-Tsay10:FTS3ed)) P.562：

**定理30.1** 设随机向量\(\boldsymbol X\), \(\boldsymbol Y\),
\(\boldsymbol Z\)的联合分布为多元正态分布，
\(\Sigma\_{XX}\)和\(\Sigma\_{ZZ}\)非退化，
\(\Sigma\_{\boldsymbol X \boldsymbol Z} = \boldsymbol 0\)。
则有如下性质：
\[\begin{aligned}
(1)\ & E(\boldsymbol Y | \boldsymbol X)
= \boldsymbol\mu\_Y
+ \Sigma\_{\boldsymbol Y \boldsymbol X}
\Sigma\_{\boldsymbol X \boldsymbol X}^{-1}
(\boldsymbol X - \boldsymbol\mu\_{\boldsymbol X}) ; \\
(2)\ & \text{Var}(\boldsymbol Y | \boldsymbol X)
= \Sigma\_{\boldsymbol Y \boldsymbol Y}
- \Sigma\_{\boldsymbol Y \boldsymbol X}
\Sigma\_{\boldsymbol X \boldsymbol X}^{-1}
\Sigma\_{\boldsymbol X \boldsymbol Y} ; \\
(3)\ & E(\boldsymbol Y | \boldsymbol X, \boldsymbol Z)
= E(\boldsymbol Y | \boldsymbol X)
+ E(\boldsymbol Y - \boldsymbol\mu\_{\boldsymbol Y} | \boldsymbol Z) \\
& \qquad\qquad\quad
= E(\boldsymbol Y | \boldsymbol X)
+ \Sigma\_{\boldsymbol Y \boldsymbol Z}
\Sigma\_{\boldsymbol Z \boldsymbol Z}^{-1}
(\boldsymbol Z - \boldsymbol\mu\_{\boldsymbol Z}); \\
(4)\ & \text{Var}(\boldsymbol Y | \boldsymbol X, \boldsymbol Z)
= \text{Var}(\boldsymbol Y | \boldsymbol X)
- \Sigma\_{\boldsymbol Y \boldsymbol Z}
\Sigma\_{\boldsymbol Z \boldsymbol Z}^{-1}
\Sigma\_{\boldsymbol Z \boldsymbol Y} \\
& \qquad\qquad\quad\ \ = \Sigma\_{\boldsymbol Y \boldsymbol Y}
- \Sigma\_{\boldsymbol Y \boldsymbol X}
\Sigma\_{\boldsymbol X \boldsymbol X}^{-1}
\Sigma\_{\boldsymbol X \boldsymbol Y}
- \Sigma\_{\boldsymbol Y \boldsymbol Z}
\Sigma\_{\boldsymbol Z \boldsymbol Z}^{-1}
\Sigma\_{\boldsymbol Z \boldsymbol Y} .
\end{aligned}\]
另外，
\(\boldsymbol Y - E(\boldsymbol Y | \boldsymbol X)\)与\(\boldsymbol X\)独立，所以
\[
\text{Var}(\boldsymbol Y | \boldsymbol X)
= E \left[ (\boldsymbol Y - E(\boldsymbol Y | \boldsymbol X))^2
| \boldsymbol X \right]
= E \left[ (\boldsymbol Y - E(\boldsymbol Y | \boldsymbol X))^2 \right],
\]
等于\(\boldsymbol X\)对\(\boldsymbol Y\)的最优估计的均方误差。

**推论30.1** 设\((X, Y)\)服从二元正态分布，
\(X \sim \text{N}(\mu\_X, \sigma\_X^2)\),
\(Y \sim \text{N}(\mu\_Y, \sigma\_Y^2)\),
\(\text{corr}(X, Y) = \rho\)。
则\(Y | X=x\)服从正态分布
\[
N\left( \mu\_Y + \rho\frac{\sigma\_Y}{\sigma\_X} (x - \mu\_X),
\quad (1-\rho^2) \sigma\_Y^2 \right),
\]
令
\[
Z = Y - E(Y | X)
= Y - \mu\_Y - \rho\frac{\sigma\_Y}{\sigma\_X} (X - \mu\_X),
\]
则\(Z\)与\(X\)相互独立，
且
\[
Z \sim \text{N}(0, \ (1-\rho^2) \sigma\_Y^2 ) .
\]

## 30.4 卡尔曼滤波

卡尔曼滤波是一种递推算法，
对\(t=1,2,\dots\)，
基于\(\mu\_t | y\_{1:t-1}\)的条件分布\(N(a\_t, P\_t)\)和新得到的观测值\(y\_t\)，
求\(\mu\_t | y\_{1:t}\)条件分布，
这等于\(\mu\_t | (y\_{1:t-1}, v\_t)\)条件分布，
只要求条件高斯分布的期望和方差。

由前一节，
\(\mu\_t | y\_{1:t-1} \sim \text{N}(a\_t, P\_t)\),
\(v\_t \sim \text{N}(0, F\_t)\)与\(y\_{1:t-1}\)独立。
注意\(\{e\_t \}\)与\(\{\eta\_t\}\)独立所以\(\{e\_t\}\)与\(\{\mu\_t \}\)独立，
利用定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)，
条件期望为
\[\begin{aligned}
& \mu\_{t|t} = E(\mu\_t | y\_{1:t}) \\
=& E(\mu\_t | y\_{1:t-1}, v\_t) \\
=& E(\mu\_t | y\_{1:t-1})
+ E(\mu\_t - E\mu\_t | v\_t) \\
=& a\_t + \frac{\text{Cov}(\mu\_t - E\mu\_t, v\_t)}{\text{Var}(v\_t)} v\_t \\
=& a\_t + \frac{\text{Cov}(\mu\_t, v\_t)}{F\_t} v\_t .
\end{aligned}\]
其中
\[\begin{aligned}
& \text{Cov}(\mu\_t, v\_t) \\
=& E(\mu\_t v\_t) \quad(\text{注意} Ev\_t = 0) \\
=& E[\mu\_t (y\_t - a\_t)] \\
=& E[\mu\_t (\mu\_t + e\_t - a\_t)] \\
=& E[\mu\_t (\mu\_t - a\_t)] + E[\mu\_t e\_t] \\
=& E[\mu\_t (\mu\_t - a\_t)] + 0 \\
=& E \left\{ E \left[ (\mu\_t - a\_t)^2 | y\_{1:t-1} \right] \right\} \\
=& E \{ P\_t \} = P\_t .
\end{aligned}\]
于是
\[
E(\mu\_t | y\_{1:t-1}, v\_t)
= a\_t + \frac{P\_t}{F\_t} v\_t,
\]
记
\[\begin{align}
K\_t = \frac{P\_t}{F\_t}
= \frac{P\_t}{P\_t + \sigma\_e^2} ,
\tag{30.5}
\end{align}\]
称\(K\_t\)为**卡尔曼增益**。
可以将条件期望写成
\[
E(\mu\_t | y\_{1:t-1}, v\_t)
= a\_t + K\_t v\_t ,
\]
这个公式将\(y\_1, \dots, y\_{t-1}, y\_t\)对\(\mu\_t\)的最优预报（滤波）公式分解为两部分，
第一部分是\(y\_1, \dots, y\_{t-1}\)对\(\mu\_t\)的最优预报，
第二部分是\(y\_t\)新增的信息\(v\_t\)对\(\mu\_t\)的最优预报，
\(v\_t\)的系数为卡尔曼增益\(K\_t\)，
最优预报是线性的。

再来求条件方差\(\text{Var}(\mu\_t | y\_{1:t-1}, v\_t)\)。
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc),
\[\begin{aligned}
& \Sigma\_{t|t} = \text{Var}(\mu\_t | y\_{1:t}) \\
=& \text{Var}(\mu\_t | y\_{1:t-1}, v\_t) \\
=& \text{Var}(\mu\_t | y\_{1:t-1})
- \frac{[\text{Cov}(\mu\_t, v\_t)]^2}{\text{Var}(v\_t)} \\
=& P\_t - \frac{P\_t^2}{F\_t}
= P\_t (1 - \frac{P\_t}{F\_t}) \\
=& P\_t (1 - K\_t ) .
\end{aligned}\]
记
\[\begin{align}
L\_t = 1 - K\_t = \frac{\sigma\_e^2}{P\_t + \sigma\_e^2},
\tag{30.6}
\end{align}\]
则有
\[
\Sigma\_{t|t}
= P\_t L\_t .
\]

因此，滤波公式为：
\[\begin{align}
\mu\_{t|t} =& a\_t + K\_t v\_t,
\tag{30.7} \\
\Sigma\_{t|t} =& P\_t (1 - K\_t),
\tag{30.8} \\
K\_t =& \frac{P\_t}{F\_t} .
\end{align}\]

有了\(\mu\_t | y\_{1:t}\)分布后，
可以给出一步预测的条件分布\(\mu\_{t+1} | y\_{1:t}\)，
这也只需要计算条件期望和条件方差：
\[\begin{align}
a\_{t+1} =& \mu\_{t+1|t} = E(\mu\_{t+1} | y\_{1:t})
= E(\mu\_{t} + \eta\_t | y\_{1:t}) \\
=& \mu\_{t|t} + 0 = \mu\_{t|t} ,
\tag{30.9} \\
P\_{t+1} =& \Sigma\_{t+1|t} = \text{Var}(\mu\_{t+1} | y\_{1:t})
= \text{Var}(\mu\_{t} | y\_{1:t}) + \text{Var}(\eta\_t | y\_{1:t}) \\
=& \Sigma\_{t|t} + \sigma\_{\eta}^2 .
\tag{30.10}
\end{align}\]

在上式推导中要注意\(\eta\_t\)是\(\mu\_{t+1}\)所对应的状态方程的误差项，
\(\eta\_t\)与\(y\_{1:t}\)和\(\mu\_1, \dots, \mu\_t\)独立。

[(30.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-form-filt-mu)–[(30.10)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-form-pred-Var)构成了卡尔曼滤波的一轮操作。
在下一轮中，
输入了新的观测\(y\_{t+1}\)后，
计算\(\mu\_{t+1} | y\_{1:t+1}\)的均值\(\mu\_{t+1 | t+1}\)和方差\(\Sigma\_{t+1 | t+1}\)，
再计算\(\mu\_{t+2} | Y\_{t+1}\)的均值\(a\_{t+2}\)和方差\(P\_{t+2}\)，……。

各个关键变量的含义汇总：

* \(y\_{1:t} = \{y\_1, y\_2, \dots, y\_t\}\)。
* \(a\_t = E(\mu\_t | y\_{1:t-1})\),
  \(P\_t = \text{Var}(\mu\_t | y\_{1:t-1})\),
  \(\mu\_t\)一步预报分布为
  \(\mu\_t | y\_{1:t-1} \sim \text{N}(a\_t, P\_t)\)。
* \(\mu\_{t|t} = E(\mu\_t | y\_{1:t})\),
  \(\Sigma\_{t|t} = \text{Var}(\mu\_t | y\_{1:t})\)，
  \(\mu\_t\)的滤波分布为
  \(\mu\_t | y\_{1:t} \sim \text{N}(\mu\_{t|t}, \Sigma\_{t|t})\)。
* \(v\_t = y\_t - E(y\_t | y\_{1:t-1})\)是\(y\_t\)的一步预报误差，
  \(F\_t = E(v\_t^2) = \text{Var}(y\_t | y\_{1:t-1})\)，
  \(v\_t \sim \text{N}(0, F\_t)\)与\(y\_{1:t-1}\)独立。
* \(K\_t = \frac{P\_t}{F\_t}\)称为Kalman增益，
  是用\(y\_{1:t-1}, v\_t\)对\(\mu\_t\)作最优线性预测时\(v\_t\)的系数。
* \(y\_t\)的一步预报分布为\(y\_t | y\_{1:t-1} \sim \text{N}(a\_t, F\_t)\)。

设初始状态\(\mu\_1\)服从\(\text{N}(\mu\_{1|0}, \Sigma\_{1|0}) = \text{N}(a\_1, P\_1)\),
将[(30.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-form-filt-mu)-[(30.8)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-form-filt-Var)
代入到[(30.9)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-form-pred-mu)-[(30.10)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-form-pred-Var)中，
得卡尔曼滤波过程如下：
\[\begin{equation}
\left\{
\begin{aligned}
v\_t =& y\_t - a\_t, \\
F\_t =& P\_t + \sigma\_e^2, \\
K\_t =& P\_t / F\_t, \\
a\_{t+1} =& \mu\_{t+1|t} = a\_t + K\_t v\_t, \\
P\_{t+1} =& \Sigma\_{t+1|t}
= P\_t (1 - K\_t) + \sigma\_{\eta}^2,
\ t=1,2,\dots, n.
\end{aligned}
\right.
\tag{30.11}
\end{equation}\]

算法中初始分布参数\(a\_1 = \mu\_{1|0}\)和\(P\_1 = \Sigma\_{1|0}\)的选取有很大影响，
后面将专门说明。

[(30.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-kfproc)的卡尔曼滤波算法在给定参数
\(\sigma\_e^2\)和\(\sigma\_{\eta}^2\)和初始分布参数\(a\_1, P\_1\)后可以迭代计算\((y\_1, y\_2, \dots, y\_n)\)的联合密度，
因此可以用来进行最大似然估计，
见[30.11](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#structts-llm-mle)。

如果不假定模型中的\(\mu\_t, y\_t\)都服从多元正态分布，
将滤波问题看成是将\(y\_{1:t}\)看成已知量，
用\(y\_{1:t}\)对\(\mu\_t\)做最优线性无偏估计(Minimum Variance Linear Unbiased Estimate, MVLUE，也称为BLUE或BLUP)的问题，
结果将得到完全相同的公式。
所以卡尔曼滤波可以看成是递推的最优线性无偏估计算法。

如果将模型中的\(\mu\_t\)看成是随机参数，
将\(y\_t\)看成是观测值，
从先验分布\(\mu\_t | y\_{1:t-1}\)到后验分布\(\mu\_t | (y\_{1:t-1}, y\_t)\)的计算公式也和卡尔曼滤波公式完全相同。
参见([Durbin and Koopman 2012](#ref-DurbinKoopman2012:TSASSM))第2.2节。

**例30.3** 对前面Alcoa现实波动率对数值数据，
利用估计的模型参数进行卡尔曼滤波计算。

将原始序列与滤波结果（\(\mu\_{t|t}\)，图中绿色线），
一步预测结果（\(y\_{t|t-1} = a\_t\)，图中红色线）同时显示：

```
plot(ts.alcoa, ylim=c(-2, 4))
lines(as.vector(time(ts.alcoa)), 
  ssr1$filtered$level, col="green")
lines(as.vector(time(ts.alcoa)), 
  ssr1$predicted$yfit, col="red")
legend("topleft", lty=1, col=c("black", "green", "red"),
  legend=c("Obs", "Filtered", "Predicted 1 step"))
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7010-struct_files/figure-html/structts-llm-exal-ssr03filt-1.png)

滤波值和一步预测值的序列都比较光滑。
程序中，
拟合结果的`$filtered`成分保存滤波结果，
`$filtered$level`为\(\{\mu\_t \}\)的滤波结果\(\{\mu\_{t|t}\}\)；
拟合结果的`$predicted`成分保存一步预报结果，
`$predicted$yfit`保存对\(y\_t\)的一步预报\(\{y\_{t|t-1} \}\)。
`$filtered`中还有`$filtered$a`表示状态的滤波，
这里即\(\{ \mu\_{t|t} \}\)，
`$filtered$P`表示滤波方差，即\(\{ \Sigma\_{t|t} \}\)，
是一个\(1 \times 1 \times n\)数组，
\(n\)为时间序列观测长度。
`$predicted`中还有`$predicted$v`，即\(\{ v\_t \}\)，
`$predicted$Fmat`即\(\{ F\_t \}\)，
`$predicted$a`即\(\{ a\_t \}\)，
`$predicted$P`即\(\{ P\_t \}\)。

一步预测误差（\(v\_t\)）的图形：

```
plot(as.vector(time(ts.alcoa)), 
  ssr1$predicted$v, 
  type="l", ylim=c(-2, 4),
  xlab="", ylab="error")
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7010-struct_files/figure-html/structts-llm-exal-ssr03se-1.png)

一步预测误差较大，
这可能是因为用高频数据计算波动率，
本身就有较大的观测噪声。

滤波的水平\(\mu\_t\)及其95%置信区间的图形：

```
plot(ts.alcoa, ylim=c(-2, 4))
lines(as.vector(time(ts.alcoa)), 
  ssr1$filtered$level, col="red")
lines(as.vector(time(ts.alcoa)), 
  ssr1$filtered$level + 1.96*sqrt(ssr1$filtered$P[1,1,]), 
  col="green", lty=3)
lines(as.vector(time(ts.alcoa)), 
  ssr1$filtered$level - 1.96*sqrt(ssr1$filtered$P[1,1,]), 
  col="green", lty=3)
legend("topleft", lty=c(1,1,3), col=c("black", "red", "green"),
  legend=c("Obs", "Filtered", "95% CI of level"))
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7010-struct_files/figure-html/structts-llm-exal-ssr03filt-ci-1.png)

## 30.5 一步预报误差

一步预报误差序列\(\{v\_t \}\)在滤波算法和参数估计中起到重要作用。
从初值\(\mu\_1\)的分布\(N(a\_1, P\_1)\)出发，
可以递推计算\(\{v\_t, t=1,2,\dots,n \}\)，
\(v\_t\)是\(y\_1, y\_2, \dots, y\_t\)的线性组合，
如：
\[\begin{aligned}
v\_1 =& y\_1 - a\_1, \\
v\_2 =& y\_2 - a\_2
= y\_2 - a\_1 - K\_1(y\_1 - a\_1), \\
v\_3 =& y\_3 - a\_3
= y\_3 - a\_1 - K\_2(y\_2 - a\_1)
- K\_1 (1 - K\_2)(y\_1 - a\_1), \\
& \cdots\cdots
\end{aligned}\]

将从\(y\)到\(v\)的这些线性变换写成矩阵形式，
令\(Y\_n = (y\_1, \dots, y\_n)^T\),
\(\boldsymbol v = (v\_1, \dots, v\_n)^T\)，
\(\boldsymbol 1\_T\)表示元素都等于1的\(n\)维列向量，
则
\[\begin{align}
\boldsymbol v
= \boldsymbol K (Y\_n - a\_1 \boldsymbol 1\_n),
\tag{30.12}
\end{align}\]
其中
\[
K = \begin{pmatrix}
1 & 0 & 0 & \cdots & 0 \\
k\_{21} & 1 & 0 & \cdots & 0 \\
k\_{31} & k\_{32} & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
k\_{n1} & k\_{n2} & k\_{n3} & \cdots & 1
\end{pmatrix} ,
\]
而\(k\_{i,i-1} = -K\_{i-1}\), \(i=2,3,\dots,n\);
对\(i=3,4,\dots,T\)和\(j=1,2,\dots,i-2\)，有
\(k\_{ij} = -(1 - K\_{i-1})(1 - K\_{i-2}) \dots (1 - K\_{j+1}) K\_j\)。

从卡尔曼滤波的公式可以看出，
卡尔曼增益\(K\_t, t=1,2,\dots,n\)并不依赖于观测值\(\{y\_t\}\)，
只依赖于参数\(\sigma\_{\eta}^2\), \(\sigma\_e^2\)以及初始分布方差\(P\_1\)，
是非随机的。

\(\{v\_t \}\)是相互独立的随机变量序列。
事实上，
\((y\_1, y\_2 \dots, y\_n)\)的联合密度函数为
\[
p\_y(y\_1, y\_2, \dots, y\_n)
= p\_y(y\_1) \prod\_{t=2}^n p\_y(y\_t | Y\_{t-1}),
\]
从[(30.12)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-kfproc-vvec)看出，
从\((y\_1, y\_2 \dots, y\_n)\)到\((v\_1, v\_2 \dots, v\_n)\)的变换的雅科比行列式绝对值等于1。
由多元的连续型随机向量变换的密度公式可知
\[\begin{aligned}
& p\_v(v\_1, v\_2, \dots, v\_n)
= p\_y(y\_1, y\_2, \dots, y\_n)
= p\_y(y\_1) \prod\_{t=2}^n p\_y(y\_t | Y\_{t-1}) \\
=& p\_v(v\_1) \prod\_{t=2}^n p\_v(v\_t)
= \prod\_{t=1}^n p\_v(v\_t),
\end{aligned}\]
其中\(p\_y(y\_1)\)表示\(y\_1\)的分布密度，
\(p\_v(v\_1)\)表示\(v\_1\)的分布密度，
因为\(y\_1 = \mu\_1 + e\_1 \sim \text{N}(a\_1, P\_1 + \sigma\_e^2)\)，
而\(v\_1 = y\_1 - y\_{1|0} = y\_1 - \mu\_{1|0} \sim \text{N}(0, P\_1 + \sigma\_e^2)\)，
所以
\[
p\_y(y\_1) = \Phi'(\frac{y\_1 - a\_1}{\sqrt{P\_1 + \sigma\_e^2}})
= \Phi'(\frac{v\_1}{\sqrt{P\_1 + \sigma\_e^2}})
= p\_v(v\_1),
\]
对\(t \geq 2\)，
\(y\_t | Y\_{t-1} \sim \text{N}(a\_t, F\_t)\)，
\(v\_t = y\_t - y\_{t|t-1} \sim \text{N}(0, F\_t)\)，
\(F\_t = P\_t + \sigma\_e^2\)，
所以
\[
p\_y(y\_t | Y\_{t-1})
= \Phi'(\frac{y\_t - a\_t}{\sqrt{F\_t}})
= \Phi'(\frac{v\_t}{\sqrt{F\_t}})
= p\_v(v\_t) ,
\]
其中\(\Phi(\cdot)\)表示标准正态分布函数。

从上述的\((v\_1, v\_2, \dots, v\_n)\)的联合密度可知各个分量相互独立，
均值为0，
\(v\_t\)的方差为\(F\_t = P\_t + \sigma\_e^2\)。

从另一个角度看，
\(\boldsymbol v\)是对\(Y\_n\)的正交化，
类似于Gram-Schimdt正交化方法，
\((v\_1, \dots, v\_t)\)与\((y\_1, \dots, y\_t)\)可以互相线性表示。
记\(\text{Var}(Y\_n) = \Omega\)，
由[(30.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-kfproc)可知
\[\begin{aligned}
\text{Var}(\boldsymbol v)
=& \text{diag}(F\_1, F\_2, \dots, F\_n)
= K \Omega K^T, \\
\Omega =& K^{-1}\,\text{diag}(F\_1, F\_2, \dots, F\_n)\, (K^{-1})^T,
\end{aligned}\]
其中\(K^{-1}\)为下三角矩阵且对角线元素都等于1，
这构成了对\(\boldsymbol y\)的方差阵\(\Omega\)的Cholesky分解。

## 30.6 状态的一步预报误差

令
\[\begin{align}
\zeta\_t = \mu\_t - \mu\_{t|t-1}
= \mu\_t - E(\mu\_t | y\_{1:t-1}),
\tag{30.13}
\end{align}\]
则
\[
\text{Var}(\zeta\_t | y\_{1:t-1})
= E \zeta\_t^2 = \text{Var}(\zeta\_t)
= P\_t,
\]
由卡尔曼滤波公式得
\[
v\_t = y\_t - \mu\_{t|t-1}
= \mu\_t + e\_t - \mu\_{t|t-1}
= \zeta\_t + e\_t,
\]
下一个时间点
\[\begin{aligned}
\zeta\_{t+1}
=& \mu\_{t+1} - \mu\_{t+1|t} \\
=& \mu\_t + \eta\_t - (\mu\_{t|t-1} + K\_t v\_t) \\
=& \mu\_t - \mu\_{t|t-1} + \eta\_t - K\_t (\zeta\_t + e\_t) \\
=& \zeta\_t + \eta\_t - K\_t(\zeta\_t + e\_t) \\
=& (1 - K\_t) \zeta\_t + \eta\_t - K\_t e\_t \\
=& L\_t \zeta\_t + \eta\_t - K\_t e\_t,
\end{aligned}\]
于是有
\[\begin{align}
v\_t =& \zeta\_t + e\_t, \tag{30.14}\\
\zeta\_{t+1} =& L\_t \zeta\_t + \eta\_t - K\_t e\_t,
\ t=1,2,\dots, T .
\tag{30.15}
\end{align}\]
这构成了以\(\{v\_t \}\)为观测，
\(\{ \zeta\_t \}\)为状态的系数时变的线性高斯状态空间模型。

## 30.7 状态平滑

模型的滤波，
是利用已有观测\(\{ y\_1, \dots, y\_t \}\)估计\(\mu\_t\)
得\(\mu\_t | y\_{1:t}\)。
在获得所有的观测\(\{ y\_1, \dots, y\_n \}\)后，
可以利用所有的观测来估计\(\mu\_t\)，
得到\(\mu\_t | Y\_n\)的分布，
这个问题称为平滑问题。

注意：

* 所有的联合分布都是正态的，
  所以记\(\mu\_t| y\_{1:n} \sim \text{N}(\mu\_{t|n}, \Sigma\_{t|n}) = \text{N}(\hat\mu\_t, V\_t)\)，
  称\(\hat\mu\_t\)为平滑状态(smoothed state)，
  称\(V\_t\)为平滑状态方差。
* \(\{ v\_1, v\_2, \dots, v\_t \}\)相互独立，
  可以与\(\{ y\_1, y\_2, \dots, y\_t \}\)互相线性表示。
  \(E v\_t = 0\), \(\text{Var}(v\_t) = F\_t\)。

显然
\[
\sigma(y\_1, \dots, y\_{t-1}, y\_t, \dots, y\_n)
= \sigma(y\_1, \dots, y\_{t-1}, v\_t, \dots, v\_n) .
\]
条件分布\(\mu\_t | y\_{1:n}\)，
等于条件分布\(\mu\_t | y\_{1:t-1}, v\_t, \dots, v\_n\)。
条件分布为高斯分布，
只要求条件期望和条件方差。

注意\(v\_t, \dots, v\_n\)与\(y\_{1:t-1}\)独立，
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)，
条件期望为
\[\begin{aligned}
& \hat\mu\_t
= E(\mu\_t | y\_{1:n})
= E(\mu\_t | y\_{1:t-1}, v\_t, \dots, v\_n) \\
=& E(\mu\_t | y\_{1:t-1})
+ E(\mu\_t - E\mu\_t | v\_t, \dots, v\_n) \\
=& a\_t
+ \text{Cov}(\mu\_t, (v\_t, \dots, v\_n))
\text{Var}^{-1}((v\_t, \dots, v\_n)) (v\_t, \dots, v\_n)^T \\
=& a\_t + (\text{Cov}(\mu\_t, v\_t), \text{Cov}(\mu\_t, v\_{t-1}), \dots, \text{Cov}(\mu\_t, v\_n))) \\
& \qquad\text{diag}(F\_t^{-1}, F\_{t+1}^{-1}, \dots, F\_n^{-1})
(v\_t, \dots, v\_n)^T \\
=& a\_t + \sum\_{j=t}^n \frac{\text{Cov}(\mu\_t, v\_j)}{F\_j} v\_j .
\end{aligned}\]
只要求\(\text{Cov}(\mu\_t, v\_j)\),
\(j=t,t+1,\dots,n\)。

对\(j \geq t\)，注意到\(\zeta\_j, e\_j, \eta\_j, v\_j\)都与\(y\_{1:t-1}\)独立，
\(e\_j\)与\(\zeta\_j\)独立，
有
\[\begin{aligned}
& \text{Cov}(\mu\_t, v\_j)
= E[(\mu\_t v\_j)] \\
=& E \{ E(\mu\_t v\_j | y\_{1:t-1}) \} \\
=& E\{ E[(\mu\_t - \mu\_{t|t-1}) v\_j | y\_{1:t-1}] \} \\
=& E\{ E[\zeta\_t v\_j | y\_{1:t-1}] \}
= E[\zeta\_t v\_j],
\end{aligned}\]
对\(j=t, t+1, \dots, n\)计算\(E[\zeta\_t v\_j]\)：
\[\begin{aligned}
E(\zeta\_t v\_t)
=& E[\zeta\_t (\zeta\_t + e\_t)]
= E(\zeta\_t^2) + 0
= P\_t, \\
E(\zeta\_t v\_{t+1})
=& E[\zeta\_t (\zeta\_{t+1} + e\_{t+1})]
= E[\zeta\_t \zeta\_{t+1}] + 0 \\
=& E[\zeta\_t (L\_t \zeta\_t + \eta\_t - K\_t e\_t)]
= E[L\_t \zeta\_t^2] + 0 + 0 \\
=& P\_t L\_t, \\
E(\zeta\_t v\_{t+2})
=& E[\zeta\_t (\zeta\_{t+2} + e\_{t+2})]
= \dots = P\_t L\_t L\_{t+1}, \\
\vdots \\
E(\zeta\_t v\_{n}) =& P\_t \prod\_{j=t}^{n-1} L\_j .
\end{aligned}\]

从而，\(\hat\mu\_t = \mu\_{t|n}\)公式为
\[\begin{aligned}
\hat\mu\_t =& \mu\_{t|n}
= a\_t + P\_t \frac{v\_t}{F\_t}
+ P\_t L\_t \frac{v\_{t+1}}{F\_{t+1}}
+ P\_t L\_t L\_{t+1} \frac{v\_{t+2}}{F\_{t+2}} \\
& + \dots + P\_t L\_t L\_{t+1} \dots L\_{n-1} \frac{v\_{n}}{F\_{n}} \\
\equiv& a\_t + P\_t r\_{t-1},
\end{aligned}\]
其中
\[\begin{align}
r\_{t-1}
=& \frac{v\_t}{F\_t}
+ L\_t \frac{v\_{t+1}}{F\_{t+1}}
+ L\_t L\_{t+1} \frac{v\_{t+2}}{F\_{t+2}}
+ \dots + L\_t L\_{t+1} \dots L\_{n-1} \frac{v\_{n}}{F\_{n}},
\tag{30.16}
\end{align}\]
是新息\((v\_t, v\_{t+1}, \dots, v\_n)\)的线性函数，
满足如下递推公式：
\[\begin{aligned}
r\_{t-1}
=& \frac{v\_t}{F\_t}
+ L\_t \left(
\frac{v\_{t+1}}{F\_{t+1}}
+ L\_{t+1} \frac{v\_{t+2}}{F\_{t+2}}
+ \dots + L\_{t+1} \dots L\_{n-1} \frac{v\_{n}}{F\_{n}}
\right) \\
=& \frac{v\_t}{F\_t} + L\_t r\_t,
\end{aligned}\]
令\(r\_n = 0\)，
有反向递推算法
\[\begin{aligned}
r\_{t-1}
= \frac{v\_t}{F\_t} + L\_t r\_t,
\ t=n, n-1, \dots, 2, 1 .
\end{aligned}\]

所以，
为了求得\(\hat\mu\_t = \mu\_{t|n}\)，
需要先进行卡尔曼滤波求出\(a\_t\), \(P\_t\), \(v\_t\), \(F\_t\), \(K\_t\), \(L\_t\),
然后令\(r\_n = 0\)，用反向递推计算：
\[\begin{align}
r\_{t-1}
=& \frac{v\_t}{F\_t} + L\_t r\_t, \\
\hat\mu\_t =& \mu\_{t|n} = a\_t + P\_t r\_{t-1},
\ t=n, n-1, \dots, 2, 1 .
\tag{30.17}
\end{align}\]

因为\(r\_{t}\)是\(v\_{t+1}, \dots, v\_n\)的线性组合，
由\(v\_1, \dots, v\_n\)的独立性可知\(r\_t, r\_{t+1}, \dots, r\_n\)与\(v\_1, v\_2, \dots, v\_t\)相互独立，\(r\_t\)与\(y\_{1:t}\)相互独立。

### 30.7.1 状态平滑方差计算

记\(v\_{t:n} = (v\_t, v\_{t+1}, \dots, v\_n)^T\)，
则\(y\_{1:t-1}\)和\(v\_{t:n}\)独立，
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)，
\[\begin{aligned}
V\_t =& \Sigma\_{t|n}
= \text{Var}(\mu\_t | y\_{1:n})
= \text{Var}(\mu\_t | y\_{1:t-1}, v\_t, \dots, v\_n) \\
=& \text{Var}(\mu\_t | y\_{1:t-1}, v\_{t:n}) \\
=& \text{Var}(\mu\_t | y\_{1:t-1})
- [\text{Cov}(\mu\_t, v\_{t:n} )]^T
[\text{Var}(v\_{t:n} )]^{-1}
[\text{Cov}(\mu\_t, v\_{t:n} )] \\
=& P\_t
- \sum\_{j=t}^{n} \frac{[\text{Cov}(\mu\_t, v\_j)]^2}{F\_j},
\end{aligned}\]
其中\(\text{Cov}(\mu\_t, v\_j) = E(\zeta\_t v\_j)\)已经在前面给出公式。
所以
\[\begin{aligned}
V\_t =& \Sigma\_{t|n} \\
=& P\_t
- P\_t^2 \frac{1}{F\_t}
- P\_t^2 L\_t^2 \frac{1}{F\_{t+1}}
- \dots - P\_t^2 (\prod\_{j=t}^{n-1} L\_j^2) \frac{1}{F\_{n}} \\
=& P\_t - P\_t^2 N\_{t-1},
\end{aligned}\]
其中
\[\begin{align}
N\_{t-1}
=& \frac{1}{F\_t}
+ L\_t^2 \frac{1}{F\_{t+1}}
+ L\_t^2 L\_{t+1}^2 \frac{1}{F\_{t+2}} + \dots
+ (\prod\_{j=t}^{n-1} L\_j^2) \frac{1}{F\_{n}}
\tag{30.18} \\
=& \frac{1}{F\_t} + L\_t^2 N\_t .
\end{align}\]

取\(N\_n=0\)，
\(N\_{t-1}\)是\(t-1\)之后的一步预报误差方差倒数的加权和，
并且由\(r\_{t-1}\)中各个\(v\_t\)的独立性恰好有
\[\begin{aligned}
\text{Var}(r\_{t-1})
=& \frac{1}{F\_t}
+ L\_t^2 \frac{1}{F\_{t+1}}
+ L\_t^2 L\_{t+1}^2 \frac{1}{F\_{t+2}} + \dots
+ (\prod\_{j=t}^{n-1} L\_j^2) \frac{1}{F\_{n}} \\
=& N\_{t-1} .
\end{aligned}\]

取\(N\_n=0\)，
状态平滑方差\(V\_t = \Sigma\_{t|n}\)可以反向递推计算如下：
\[\begin{align}
N\_{t-1} =& \frac{1}{F\_t} + L\_t^2 N\_t,
\tag{30.19}\\
V\_t =& \Sigma\_{t|n}
= P\_t - P\_t^2 N\_{t-1},
\ t = n, n-1, \dots, 2, 1 .
\tag{30.20}
\end{align}\]

**例30.4** Alcoa股票日现实波动率对数值数据的平滑。

Alcoa股票日现实波动率对数值数据以及平滑结果图形：

```
plot(ts.alcoa, ylim=c(-2, 4))
lines(as.vector(time(ts.alcoa)), 
  ssr1$smoothed$level, col="red")
lines(as.vector(time(ts.alcoa)), 
  ssr1$smoothed$level + 1.96*sqrt(ssr1$smoothed$V[1,1,]), 
  col="green", lty=3)
lines(as.vector(time(ts.alcoa)), 
  ssr1$smoothed$level - 1.96*sqrt(ssr1$smoothed$V[1,1,]), 
  col="green", lty=3)
legend("topleft", lty=c(1,1,3), col=c("black", "red", "green"),
  legend=c("Obs", "Smoothed", "95% CI of level"))
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7010-struct_files/figure-html/structts-llm-exal-ssr03sm-ci-1.png)

模型结果中`smoothed`成分保存平滑结果，
`smoothed$level`保存局部水平\(\mu\_t\)的平滑结果，
`smoothed$V`保存平滑方差估计，
这里是\(1 \times 1 \times n\)数组。

实际上，
R的基本的stats包提供了`StructTS()`函数，
可以直接拟合包括局部水平模型的结构时间序列模型，
如：

```
sts.al <- StructTS(ts.alcoa, type="level")
sts.al
```

```
## 
## Call:
## StructTS(x = ts.alcoa, type = "level")
## 
## Variances:
##    level   epsilon  
## 0.005403  0.230652
```

```
plot(ts.alcoa, ylim=c(-2, 4),
  main="Alcoa smoothed with StructTS")
lines(tsSmooth(sts.al), col="green")
legend("topleft", lty=c(1,1), 
  col=c("black", "green"),
  legend=c("Obs", "Smoothed"))
```

![](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/7010-struct_files/figure-html/structts-llm-smo-alcoa-ex01sts-1.png)

对`StructTS()`的结果，
用`tsSmooth()`可以提取平滑结果，
用`fitted()`可以提取滤波结果。
可以用`predict()`或者`forecast::forecast()`进行预报。

## 30.8 扰动的平滑

有了所有观测值\(\{y\_1, y\_2, \dots, y\_n \}\)以后，
不仅可以利用这些信息估计状态\(\mu\_t\)，
得到平滑状态和平滑方差，
还可以估计\(e\_t\)和\(\eta\_t\)的条件分布，
这个问题称为扰动的平滑。

估计\(e\_t\)和\(\eta\_t\)在\(Y\_n\)下的条件期望和条件方差，
可以用来进行模型诊断，
查找状态的突变点（对局部水平模型相当于水平的跳跃点或变点），
查找观测误差的异常值。

记
\[\begin{aligned}
\hat e\_t = E(e\_t | y\_{1:n}),
\quad
\hat\eta\_t = E(\eta\_t | y\_{1:n}),
\ t=1,2,\dots, n .
\end{aligned}\]

因为\(e\_t = y\_t - \mu\_t\)，
在\(y\_{1:n}\)条件下\(y\_t\)已知，
所以
\[
e\_t | y\_{1:n} \sim \text{N}
(y\_t - \mu\_{t|n},
\Sigma\_{t|n})
= \text{N}(y\_t - \hat\mu\_t, V\_t).
\]

对\(\eta\_t\)，
易见
\[
\hat\eta\_t = E(\mu\_{t+1} | y\_{1:n}) - E(\mu\_{t} | y\_{1:n})
= \mu\_{t+1|n} - \mu\_{t|n}
= \hat\mu\_{t+1} - \hat\mu\_t,
\]
但\(\eta\_t | y\_{1:n}\)的条件方差公式需要用到平滑的协方差，
下面给出另一种计算条件方差的算法。

### 30.8.1 观测方程扰动的递推平滑公式

从计算效率出发，
为了获得\(e\_t\)和\(\eta\_t\)在\(y\_{1:n}\)下的条件分布，
直接从\(r\_t\)和\(N\_t\)计算更简单。
关于\(e\_t | y\_{1:n}\)的公式为
\[\begin{aligned}
E(e\_t | y\_{1:n})
=& \sigma\_e^2 \left(F\_t^{-1} v\_t - K\_t r\_t \right), \\
\text{Var}(e\_t | y\_{1:n})
=& \sigma\_e^2 - \sigma\_e^4\left(
\frac{1}{F\_t} + K\_t^2 N\_t \right) ,\\
&\quad t=n, n-1, \dots, 2, 1 .
\end{aligned}\]

**证明**：
由于\(F\_t = P\_t + \sigma\_e^2\),
\(K\_t = \frac{P\_t}{F\_t}\),
\(L\_t = 1 - K\_t = \frac{\sigma\_e^2}{F\_t}\),
有
\[\begin{aligned}
E(e\_t | y\_{1:n})
=& E(y\_t - \mu\_t | y\_{1:n})
= y\_t - \mu\_{t|n} \\
=& y\_t - a\_t - P\_t r\_{t-1} \\
=& v\_t - P\_t \left[ \frac{v\_t}{F\_t} + L\_t r\_t \right] \\
=& \left(1 - \frac{P\_t}{F\_t} \right) v\_t
- P\_t L\_t r\_t \\
=& \frac{\sigma\_e^2}{F\_t} v\_t - P\_t \frac{\sigma\_e^2}{F\_t} r\_t \\
=& \sigma\_e^2 \left( \frac{v\_t}{F\_t} - K\_t r\_t \right) .
\end{aligned}\]

记
\[
u\_t = \frac{v\_t}{F\_t} - K\_t r\_t
= \sigma\_e^{-2} E(e\_t | y\_{1:n}),
\]
称\(u\_t\)为**平滑误差**(smoothing error)。

为了求\(\text{Var}(e\_t | y\_{1:n})\)，
注意在联合正态分布下条件分布的方差是非随机的，
由公式\(\text{Var(Y)} = E[\text{Var}(Y|X)] + \text{Var}[E(Y|X)]\)，
以及\(v\_t\)与\(r\_t\)独立，
\(\text{Var}(r\_t) = N\_t\)，可得
\[\begin{aligned}
\text{Var}(e\_t | y\_{1:n})
=& E[\text{Var}(e\_t | y\_{1:n})]
= \text{Var}(e\_t) - \text{Var}[E(e\_t | y\_{1:n})] \\
=& \sigma\_e^2 - \sigma\_e^4\left(
\frac{1}{F\_t} + K\_t^2 N\_t \right) .
\end{aligned}\]

### 30.8.2 状态方程扰动的递推平滑公式

对\(\eta\_t | y\_{1:n}\)有
\[\begin{aligned}
\hat\eta\_t
=& E(\eta\_t | y\_{1:n})
= \sigma\_{\eta}^2 r\_t, \\
\text{Var}(\eta\_t | y\_{1:n})
=& \sigma\_{\eta}^2 - \sigma\_{\eta}^4 N\_t,
\ t=n, n-1, \dots, 2, 1 .
\end{aligned}\]

\(r\_t\)与\(\hat\eta\_t\)的关系也说明了\(r\_t\)和\(N\_t\)的一个解释，
\(r\_t\)是平滑的状态扰动项\(\hat\eta\_t\)的常数倍，
注意\(\text{Var}(r\_t)=N\_t\)，
于是\(\text{Var}(\hat\eta\_t) = \sigma\_{\eta}^4 N\_t\)，
所以\(r\_t\)的无条件方差\(N\_t\)是平滑的状态扰动无条件方差\(\text{Var}(\hat\eta\_t)\)的常数倍。

**证明**：
\[\begin{aligned}
E(\eta\_t | y\_{1:n})
=& E(\mu\_{t+1} - \mu\_t | y\_{1:n})
= \mu\_{t+1|n} - \mu\_{t|n} \\
=& a\_{t+1} + P\_{t+1} r\_t
- a\_t - P\_t r\_{t-1} \\
=& (a\_{t+1} - a\_t)
+ \left( P\_t \frac{\sigma\_e^2}{F\_t}
+ \sigma\_{\eta}^2 \right) r\_t
- P\_t
\left( \frac{v\_t}{F\_t} + \frac{\sigma\_e^2}{F\_t} r\_t \right) \\
=& K\_t v\_t + (K\_t \sigma\_e^2 + \sigma\_{\eta}^2) r\_t
- K\_t v\_t - \sigma\_e^2 K\_t r\_t \\
=& \sigma\_{\eta}^2 r\_t .
\end{aligned}\]
而
\[\begin{aligned}
\text{Var}(\eta\_t | y\_{1:n})
=& E[ \text{Var}(\eta\_t | y\_{1:n}) ] \\
=& \text{Var}(\eta\_t) - \text{Var}[E(\eta\_t | y\_{1:n})] \\
=& \sigma\_{\eta}^2 - \sigma\_{\eta}^4 N\_t .
\end{aligned}\]

## 30.9 缺失值处理和预测

一般的时间序列模型都很难处理出现在时间区间内部的缺失值。
状态空间模型的一大优势就是可以比较容易的允许观测有缺失值。
在局部水平模型中，
设\(\{y\_t\}\_{t=\ell+1}^{\ell+h}\)缺失。
状态空间模型可以用多种方法解决缺失值问题，
这里使用不改变时间步数和模型形式的方法。

对\(t \in \{ \ell+1, \dots, \ell+h \}\)，
有
\[
\mu\_t = \mu\_{t-1} + \eta\_{t-1} = \dots
= \mu\_{\ell+1} + \sum\_{j=\ell+1}^{t-1} \eta\_j,
\]
这里约定求和下标下限大于下标上限时和为0。
于是
\[\begin{aligned}
E(\mu\_t | Y\_{t-1})
=& E(\mu\_t | Y\_{\ell})
= a\_{\ell+1}, \\
\text{Var}(\mu\_t | Y\_{t-1})
=& \text{Var}(\mu\_t | Y\_{\ell})
= P\_{\ell+1} + (t-\ell-1) \sigma\_{\eta}^2 ,
\end{aligned}\]
于是有递推式
\[\begin{align}
a\_t =& \mu\_{t|t-1} = \mu\_{t-1|t-2} = a\_{t-1},
\tag{30.21} \\
P\_t =& \Sigma\_{t|t-1} = P\_{t-1} + \sigma\_{\eta}^2,
\ t=\ell+2, \dots, \ell+h .
\tag{30.22}
\end{align}\]

所以，局部水平模型有缺失值时仍可使用[(30.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#eq:sts-llm-kfproc)进行滤波计算，
但是对缺失的\(y\_t\)，
应该相应地取\(v\_t=0\), \(K\_t=0\)。
这也符合常识，
因为观测缺失时就没有新息\(v\_t\)的信息，
也没有卡尔曼增益\(K\_t\)。

实际上，
在已知\(y\_1, y\_2, \dots, y\_n\)后对\(y\_{n+1}, \dots, y\_{n+h}\)进行预测时，
最优线性无偏估计得到的预测公式，
与设\(y\_{n+1}, \dots, y\_{n+h}\)为缺失值，
令\(v\_{n+1} = \dots = v\_{n+h} = K\_{n+1} = \dots = K\_{n+h} = 0\)后进行滤波计算的计算公式完全相同。
所以预测问题可以看成有缺失值时的滤波问题的特例。
这时
\[\begin{aligned}
E(y\_{n+j} | y\_{1:n})
=& E(\mu\_{n+j} | y\_{1:n})
= a\_{n+1}, \\
\text{Var}(y\_{n+j} | y\_{1:n})
=& \text{Var}(\mu\_{n+j} | y\_{1:n}) + \sigma\_e^2
= P\_n + (j-1) \sigma\_{\eta}^2 + \sigma\_e^2 .
\end{aligned}\]

## 30.10 初值分布参数选取

卡尔曼滤波计算需要假定已知\(\mu\_1 \sim \text{N}(a\_1, P\_1)\)。
实际中\(a\_1\)和\(P\_1\)是未知的。
利用滤波公式得
\[\begin{aligned}
v\_1 =& y\_1 - a\_1, \quad F\_1 = P\_1 + \sigma\_e^2, \\
a\_2 =& a\_1 + \frac{P\_1}{F\_1} v\_1
= a\_1 + \frac{P\_1}{F\_1} (y\_1 - a\_1) \\
\to& y\_1 \quad(P\_1 \to \infty), \\
P\_2 =& P\_1 \left(1 -
\frac{P\_1}{P\_1 + \sigma\_e^2} \right)
+ \sigma\_{\eta}^2 \\
=& \frac{P\_1}{P\_1 + \sigma\_e^2} \sigma\_e^2 + \sigma\_{\eta}^2 \\
\to& \sigma\_e^2 + \sigma\_{\eta}^2 \quad(P\_1 \to \infty),
\end{aligned}\]
因此\(P\_1 \to \infty\)时相当于认为\(y\_1\)是非随机的确定值，
而\(\mu\_1 \sim \text{N}(y\_1, \sigma\_e^2)\)。
这种初始化方法称为扩散(diffuse)初始化或者扩散先验。
扩散先验相当于对初始状态分布没有任何知识。

对于取扩散先验时的平滑问题，
\(t=n, n-1, \dots, 2\)的反向递推仍完全原样进行。
对\(t=1\)，
利用\(L\_1 = 1 - K\_1 = F\_1^{-1} \sigma\_e^2\),
\(F\_1 = P\_1 + \sigma\_e^2\)，得
\[\begin{aligned}
\hat\mu\_1 =& \mu\_{1|n}
= a\_1 + P\_1 r\_0 \\
=& a\_1 + P\_1 \left[
\frac{1}{P\_1 + \sigma\_e^2} v\_1
+ \left(1 - \frac{P\_1}{P\_1 + \sigma\_e^2}
\right) r\_1 \right] \\
=& a\_1 + \frac{P\_1}{P\_1 + \sigma\_e^2}(v\_1 + \sigma\_e^2 r\_1) \\
\to& a\_1 + v\_1 + \sigma\_e^2 r\_1
= y\_1 + \sigma\_e^2 r\_1
\quad(P\_1 \to \infty), \\
V\_1 =& \Sigma\_{1|n}
= P\_1 - P\_1^2 \left[
\frac{1}{P\_1 + \sigma\_e^2} +
\left(1 - \frac{P\_1}{P\_1 + \sigma\_e^2} \right)^2 N\_1
\right] \\
=& P\_1 \left(1 - \frac{P\_1}{P\_1 + \sigma\_e^2} \right)
- \left(1 - \frac{P\_1}{P\_1 + \sigma\_e^2} \right)^2
P\_1^2 N\_1 \\
=& \left(\frac{P\_1}{P\_1 + \sigma\_e^2} \right) \sigma\_e^2
- \left(\frac{P\_1}{P\_1 + \sigma\_e^2} \right)^2 \sigma\_e^4 N\_1 \\
\to& \sigma\_e^2 - \sigma\_e^4 N\_1
\quad(P\_1 \to \infty) .
\end{aligned}\]

另一种想法是将\(\mu\_1\)也作为一个未知参数，
并且仅根据\(y\_1\)一个样本点估计\(\mu\_1\)。
因为\(y\_1 | \mu\_1 \sim \text{N}(\mu\_1, \sigma\_e^2)\)，
所以\(\mu\_1\)的估计为\(y\_1\)，
估计方差为\(\sigma\_e^2\)，
即从\(\mu\_1 | y\_1 \sim \text{N}(y\_1, \sigma\_e^2)\)向前滤波。
这与先验\(P\_1 \to \infty\)公式相同。

## 30.11 模型参数估计

### 30.11.1 初始分布已知情形

滤波和平滑算法都是假定模型参数\(\sigma\_e^2\)和\(\sigma\_{\eta}^2\)已知的。
为了估计参数可以使用最大似然估计法，
计算似然函数时可以利用滤波算法进行计算。

\((y\_1, y\_2, \dots, y\_n)\)的似然函数为
\[\begin{aligned}
& p\_y(y\_1, y\_2, \dots, y\_n | \sigma\_e^2, \sigma\_{\eta}^2) \\
=& p\_y(y\_1 | \sigma\_e^2, \sigma\_{\eta}^2)
\prod\_{t=2}^n p\_y(y\_t | Y\_{t-1}, \sigma\_e^2, \sigma\_{\eta}^2) \\
=& p\_v(v\_1 | \sigma\_e^2, \sigma\_{\eta}^2)
\prod\_{t=2}^T p\_v(v\_t | \sigma\_e^2, \sigma\_{\eta}^2),
\end{aligned}\]
其中\(y\_1 \sim \text{N}(a\_1, P\_1)\)，
\(v\_t \sim \text{N}(0, F\_t)\)，
设\(a\_1, P\_1\)已知，
可得对数似然函数为
\[\begin{aligned}
\ln L(\sigma\_e^2, \sigma\_{\eta}^2)
= -\frac{n}{2} \ln(2\pi)
- \frac{1}{2} \sum\_{t=1}^n \left(
\ln F\_t + \frac{v\_t^2}{F\_t} \right) .
\end{aligned}\]
给定初始值\(a\_1, P\_1\)以及一组参数值\(\sigma\_e^2, \sigma\_{\eta}^2\)
就可以用卡尔曼滤波算法计算出\(v\_t, F\_t\)从而得到对数似然函数值。
用数值优化算法可以求得似然函数关于\(\sigma\_e^2\)和\(\sigma\_\eta^2\)的最大值点。

具体计算可以用R的statespacer扩展包，
KFAS, bssm扩展包等，
还有许多其它软件也可以进行状态空间模型建模。

### 30.11.2 发散先验情形

若\(P\_1 \to \infty\)，
则\(F\_1 \to \infty\)，
但\(F\_2\)有限。
所以对数似然函数中\(t=1\)的项会与\(P\_1\)有关为\(\log F\_1 + v\_1^2/F\_1\)，
这一项会趋于无穷。
所以可以将对数似然函数替换成
\[\begin{aligned}
\log L\_d
=& \lim\_{P\_1 \to \infty} (\log L + \frac{1}{2} \log P\_1) \\
=& -\frac{n}{2} \ln(2\pi)
- \frac{1}{2} \sum\_{t=2}^n \left(
\ln F\_t + \frac{v\_t^2}{F\_t} \right) .
\end{aligned}\]

这相当于从对数似然函数中去掉了\(t=1\)的项。
给定\(P\_1\)，
\(\log L\_d\)与\(\log L\)最大值点相同，
所以当\(P\_1\)取为很大的值时，
获得的是发散先验条件下最大似然估计近似值。

## 30.12 滤波的极限状态

如果\(t \to \infty\)时\(P\_t\)存在极限\(\bar P\)，
则\(F\_t \to \bar P + \sigma\_e^2\)，
\(K\_t \to \frac{\bar P}{\bar P + \sigma\_e^2}\)，
\(t\)充分大时滤波算法中的\(K\_t\), \(P\_t\), \(L\_t\)都可以取为常数值，
且
\[
a\_{t+1} = a\_t + \bar K v\_t .
\]

为了检查是否有极限状态，
令\(P\_{t+1} = P\_t = \bar P\)，
由\(P\_{t+1}\)的递推式有
\[
\bar P = \bar P \left(1 - \frac{\bar P}{\bar P + \sigma\_e^2} \right)
+ \sigma\_\eta^2 .
\]
记\(q = \sigma\_\eta^2 / \sigma\_e^2\)，
\(x = \bar P / \sigma\_e^2\)，
则\(x\)的方程为
\[
x^2 - qx + q = 0,
\]
解为
\[
x = (q + \sqrt{q^2 + 4q})/2 .
\]
所以\(P\_t\)的极限存在。

## 30.13 模型诊断

### 30.13.1 预测误差分析

\(v\_t\)是样本新息，
理论分布为\(\text{N}(0, F\_t)\)，
令
\[
z\_t = \frac{v\_t}{\sqrt{F\_t}},
\]
称\(\{z\_t \}\)为标准化残差，
应该是独立同标准正态分布的。
计算标准化残差后，
可以进行正态性检验，
检验方差是否恒等，
检验有无序列自相关。

### 30.13.2 检测异常值和结构改变

可以基于\(y\_1, \dots, y\_n\)计算\(e\_t\)的条件分布，
并计算标准化值（除以标准差）\(u\_t^\*\)。
如果某个\(u\_t^\*\)绝对值很大，
说明这个时间点模型拟合很差，
是一个异常点。

可以基于\(y\_1, \dots, y\_n\)计算\(\eta\_t\)的条件分布，
并计算标准化值（除以标准差）\(r\_t^\*\)。
如果某个\(r\_t^\*\)绝对值很大，
说明局部水平从\(t\)时刻到\(t+1\)时刻有一个很大的变化，
可能构成了水平的一个变点。

## 30.14 附录：局部水平模型建模的R程序

这里直接利用本章给出的算法编制局部水平模型建模的R语言程序。

### 30.14.1 用卡尔曼滤波计算似然函数

```
# 设状态初值分布参数$a_1$, $P_1$已知，
# 观测误差方差$\sigma_e^2$和状态方程误差方差$\sigma_\eta^2$已知，
# 进行滤波，并计算对数似然函数值。
kfll_winit <- function(
    y,  # 观测值序列
    a1, # 状态初始分布均值
    P1, # 状态初始分布方差
    var_err_obs, # 观测误差方差$\sigma_e^2$
    var_err_sys  # 状态方程误差方差$\sigma_\eta^2$
    ) { 
  n <- length(y)
  
  # 对y进行归一化以免y取值过大、过小使得算法中的阈值不合适
  y0 <- y
  my <- mean(y, na.rm=TRUE)
  sigy <- sd(y, na.rm=TRUE)
  y <- (y - my)/sigy
  
  # 预先分配一步预报分布的存储空间
  v <- numeric(n)
  F <- numeric(n)
  K <- numeric(n)
  a <- numeric(n+1); a[1] <- a1
  P <- numeric(n+1); P[1] <- P1
  
  # 向前进行Kalman滤波
  for(t in seq(n)){
    v[t] = y[t] - a[t]
    F[t] = P[t] + var_err_obs
    K[t] = P[t]/F[t]
    a[t+1] = a[t] + K[t]*v[t]
    P[t+1] = P[t]*(1-K[t]) + var_err_sys
  }
  
  # 计算对数似然函数
  logL <- ( -0.5*n*log(2*pi) 
           - 0.5*sum(log(F)) - 0.5*sum(v^2 / F) )
  
  list(logLik=logL, mean_y=my, sigma_y=sigy)
}
```

### 30.14.2 参数最大似然估计（发散先验初始化）

```
# 计算最大似然估计的程序
# 设初始化用发散先验，$a_1=0$
# 注意滤波程序内对输入观测值进行了归一化，
# 所以两个方差参数在恢复时需要乘以y的方差
mle_ll <- function(y){
  a1 <- 0
  P1 <- 1E6
  # 负对数似然函数值
  # 参数为$\log(\sigma_e^2)$, $\log(\sigma_eta^2)$
  negLL <- function(wpars){
    pars <- exp(wpars)
    var_err_obs <- pars[1]
    var_err_sys <- pars[2]
    -kfll_winit(y, a1, P1, var_err_obs, var_err_sys)$logLik
  }
  
  # 需要两个方差的初值
  wpars <- log(rep(var(y), 2))
  res <- nlm(negLL, wpars)
  pars <- exp(res$estimate)
  var_err_obs <- pars[1] * var(y, na.rm=TRUE)
  var_err_sys <- pars[2] * var(y, na.rm=TRUE)
  logL <- -res$minimum
  
  list(var_err_obs=var_err_obs,
       var_err_sys=var_err_sys,
       logLik=logL)
}
```

用波动率对数值数据测试：

```
res1 <- mle_ll(as.vector(ts.alcoa))
print(res1)
```

```
## $var_err_obs
## [1] 0.2306524
## 
## $var_err_sys
## [1] 0.005403459
## 
## $logLik
## [1] -461.4858
```

结果与statespacer结果相同。

### 30.14.3 平滑程序（发散先验初始化）

```
# 设状态初值分布参数$a_1$, $P_1$已知，
# 观测误差方差$\sigma_e^2$和状态方程误差方差$\sigma_\eta^2$已知，
# 进行滤波和平滑，输出平滑分布参数。
smll_winit <- function(
    y,  # 观测值序列
    a1, # 状态初始分布均值
    P1, # 状态初始分布方差
    var_err_obs, # 观测误差方差$\sigma_e^2$
    var_err_sys  # 状态方程误差方差$\sigma_\eta^2$
) { 
  n <- length(y)
  
  # 预先分配向前滤波存储空间
  v <- numeric(n)
  F <- numeric(n)
  K <- numeric(n)
  a <- numeric(n+1); a[1] <- a1
  P <- numeric(n+1); P[1] <- P1
  
  # 向前进行Kalman滤波
  for(t in seq(n)){
    v[t] = y[t] - a[t]
    F[t] = P[t] + var_err_obs
    K[t] = P[t]/F[t]
    a[t+1] = a[t] + K[t]*v[t]
    P[t+1] = P[t]*(1-K[t]) + var_err_sys
  }

  # 计算对数似然函数
  logL <- ( -0.5*n*log(2*pi) 
            - 0.5*sum(log(F)) - 0.5*sum(v^2 / F) )
  
  # 预先分配平滑存储空间
  r <- numeric(n+1); r[n+1] <- 0
  N <- numeric(n+1); N[n+1] <- 0
  L <- 1-K
  for(t in n:1){
    r[t] <- v[t] / F[t] + L[t]*r[t+1]
    N[t] <- 1/F[t] + L[t]^2*N[t+1]
  }
  mu_sm <- a[1:n] + P[1:n]*r[1:n]
  var_sm <- P[1:n] -P[1:n]^2 * N[1:n]
  
  list(logLik=logL, mean_sm_mu=mu_sm, var_sm_mu=var_sm)
}
```

用Alcoa数据测试：

```
res2 <- smll_winit(
  as.vector(ts.alcoa), a1=as.vector(ts.alcoa)[1], P1=1E6, 
  var_err_obs=res1$var_err_obs, 
  var_err_sys=res1$var_err_sys)
```

结果与statespacer结果相同。

### B 参考文献

Beijers, Dylan. 2020. *statespacer: State Space Modelling in r*. <https://dylanb95.github.io/statespacer/>.

Durbin, James, and Siem Jan Koopman. 2012. *Time Series Analysis by State Space Methods*. 2nd ed. Oxford University Press.

Tsay, Ruey S. 2010. *Analysis of Financial Time Series*. 3rd Ed. John Wiley & Sons, Inc.