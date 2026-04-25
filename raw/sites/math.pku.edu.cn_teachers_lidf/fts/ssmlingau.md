---
crawl_time: '2026-01-17 14:30:51'
framework: sphinx
title: 32 线性高斯状态空间模型 | 金融时间序列分析讲义
url: https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html
---

# [金融时间序列分析讲义](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/)

# 32 线性高斯状态空间模型

参考：

* ([Durbin and Koopman 2012](#ref-DurbinKoopman2012:TSASSM)) 第4、5、6、7章。

## 32.1 卡尔曼滤波算法

### 32.1.1 模型和问题

考虑如下的线性高斯状态空间模型：

\[\begin{align}
\boldsymbol y\_t
=& Z\_t \boldsymbol \alpha\_t + \boldsymbol \varepsilon\_t,
\ \boldsymbol \varepsilon\_t \sim \text{N}(0, H\_t),
\tag{32.1} \\
\boldsymbol \alpha\_{t+1}
=& T\_t \boldsymbol \alpha\_t + R\_t \boldsymbol \eta\_t,
\ \boldsymbol \eta\_t \sim \text{N}(0, Q\_t),
\tag{32.2} \\
\boldsymbol \alpha\_1 \sim& \text{N}(\boldsymbol a\_1, P\_1) .
\end{align}\]

其中\(\boldsymbol y\_t\)是\(t\)时刻的观测值，
为\(p \times 1\)向量，设有\(t=1,2,\dots,n\)时刻的观测值；
\(\boldsymbol \alpha\_t\)是\(t\)时刻系统的状态，
是不可观测的\(m\times 1\)随机向量。
[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modobs)称为观测方程，
[(32.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modtran)称为状态方程。
\(\{\boldsymbol \varepsilon\_t\}\)和\(\{\boldsymbol \eta\_t \}\)相互独立，
都是独立同分布向量白噪声列，
\(\boldsymbol \varepsilon\_t\)为\(p\times 1\)随机向量，
\(\boldsymbol \eta\_t\)为\(r \times 1\)随机向量，\(r \leq m\)。
设各矩阵\(Z\_t, T\_t, R\_t, H\_t, Q\_t\)已知，
\(Z\_t\)和\(T\_{t-1}\)允许依赖于\(\boldsymbol y\_1, \dots, \boldsymbol y\_{t-1}\)，
初始状态\(\boldsymbol\alpha\_1\)服从\(N(\boldsymbol a\_1, P\_1)\)，
设\(\boldsymbol a\_1, P\_1\)已知，
\(\boldsymbol\alpha\_1\)与\(\{\boldsymbol \varepsilon\_t\}\)和\(\{\boldsymbol \eta\_t \}\)独立。
当参数未知时，
设\(\boldsymbol\psi\)为未知参数，
矩阵\(Z\_t, T\_t, R\_t, H\_t, Q\_t\)可以依赖于未知参数\(\boldsymbol\psi\)。

各个矩阵的维数如下：

| 向量 |  | 矩阵 |  |
| --- | --- | --- | --- |
| \(\boldsymbol y\_t\) | \(p \times 1\) | \(Z\_t\) | \(p \times m\) |
| \(\boldsymbol \alpha\_t\) | \(m \times 1\) | \(T\_t\) | \(m \times m\) |
| \(\boldsymbol \varepsilon\_t\) | \(p \times 1\) | \(H\_t\) | \(p \times p\) |
| \(\boldsymbol \eta\_t\) | \(r \times 1\) | \(R\_t\) | \(m \times r\) |
|  |  | \(Q\_t\) | \(m \times r\) |
| \(\boldsymbol a\_1\) | \(p \times 1\) | \(P\_1\) | \(m \times m\) |

记
\[
\boldsymbol Y\_t
= \begin{pmatrix}
\boldsymbol y\_1 \\ \boldsymbol y\_2 \\
\vdots \\ \boldsymbol y\_t
\end{pmatrix} .
\]

设\(\boldsymbol a\_1, P\_1\)已知，
\(\boldsymbol \alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\)。

归纳地，
设\(\boldsymbol \alpha\_t | \boldsymbol Y\_{t-1} \sim \text{N}(\boldsymbol a\_t, P\_t)\)。
在获得\(\boldsymbol y\_t\)后，
递推计算\(\boldsymbol \alpha\_t | \boldsymbol Y\_{t}\)的条件分布，
和\(\boldsymbol \alpha\_{t+1} | \boldsymbol Y\_{t}\)的条件分布。
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)可知这两个条件分布正态。
记\(\boldsymbol a\_{t|t} = E(\boldsymbol \alpha\_t | \boldsymbol Y\_t)\),
\(P\_{t|t} = \text{Var}(\boldsymbol \alpha\_t | \boldsymbol Y\_t)\),
\(\boldsymbol a\_{t+1} = E(\boldsymbol \alpha\_{t+1} | \boldsymbol Y\_t)\),
\(P\_{t+1} = \text{Var}(\boldsymbol \alpha\_{t+1} | \boldsymbol Y\_t)\)。

### 32.1.2 更新和预报步

记
\[\begin{align}
\boldsymbol v\_t
=& \boldsymbol y\_t - E(\boldsymbol y\_t | \boldsymbol Y\_{t-1})
= \boldsymbol y\_t
- E(Z\_t \boldsymbol \alpha\_t + \boldsymbol\varepsilon\_t
| \boldsymbol Y\_{t-1})
= \boldsymbol y\_t - Z\_t \boldsymbol a\_t .
\tag{32.3}
\end{align}\]
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)可知\(\boldsymbol v\_t\)与\(\boldsymbol y\_1, \dots, \boldsymbol y\_{t-1}\)独立，\(E \boldsymbol v\_t = \boldsymbol 0\)。
\(\sigma(\boldsymbol Y\_{t-1}, \boldsymbol v\_t) = \sigma(\boldsymbol Y\_t)\)
(\(\sigma(\cdot)\)表示随机变量生成的\(\sigma\)代数)，
所以给定\(\boldsymbol Y\_t\)的条件分布，
等于给定\(\boldsymbol Y\_{t-1}\)和\(\boldsymbol v\_t\)的条件分布。

令\(F\_t = \text{Var}(\boldsymbol v\_t)\)，
因\(\boldsymbol v\_t\)与\(\boldsymbol Y\_{t-1}\)独立，
所以有
\[\begin{align}
F\_t =& \text{Var}(\boldsymbol v\_t | \boldsymbol Y\_{t-1})
= \text{Var}(Z\_t \boldsymbol\alpha\_t + \boldsymbol\varepsilon\_t
- Z\_t \boldsymbol a\_t | \boldsymbol Y\_{t-1}) \\
=& \text{Var}(Z\_t (\boldsymbol\alpha\_t - \boldsymbol a\_t)
+ \boldsymbol\varepsilon\_t | \boldsymbol Y\_{t-1}) \\
=& Z\_t P\_t Z\_t^T + H\_t .
\tag{32.4}
\end{align}\]

由\(\sigma(\boldsymbol Y\_{t-1}, \boldsymbol v\_t) = \sigma(\boldsymbol Y\_t)\)可得
\[\begin{aligned}
\boldsymbol a\_{t|t}
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1}, \boldsymbol v\_t) \\
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1})
+ \text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_t)
[\text{Var}(\boldsymbol v\_t)]^{-1} \boldsymbol v\_t
\end{aligned}\]
其中
\[\begin{align}
\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_t)
=& E \left\{ E\left[
\boldsymbol\alpha\_t
(Z\_t \boldsymbol\alpha\_t + \boldsymbol\varepsilon\_t
- Z\_t \boldsymbol a\_t)^T
| \boldsymbol Y\_{t-1} \right] \right\} \\
=& E \left\{ E\left[
\boldsymbol\alpha\_t
(\boldsymbol\alpha\_t - \boldsymbol a\_t)^T Z\_t^T
| \boldsymbol Y\_{t-1} \right] \right\} \\
=& P\_t Z\_t^T .
\tag{32.5}
\end{align}\]
于是得
\[\begin{align}
\boldsymbol a\_{t|t}
= \boldsymbol a\_t + P\_t Z\_t^T F\_t^{-1} \boldsymbol v\_t .
\tag{32.6}
\end{align}\]

仍利用定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)可知
\[\begin{align}
P\_{t|t}
=& \text{Var}(\boldsymbol\alpha\_t
| \boldsymbol Y\_{t-1}, \boldsymbol v\_t) \\
=& \text{Var}(\boldsymbol\alpha\_t
| \boldsymbol Y\_{t-1})
- \text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_t)
[\text{Var}(\boldsymbol v\_t)]^{-1}
\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_t)^T \\
=& P\_t - P\_t Z\_t^T F\_t^{-1} Z\_t P\_t .
\tag{32.7}
\end{align}\]

以上设\(\boldsymbol v\_t\)的方差阵\(F\_t\)可逆，
对于设计正确的模型这一般是能满足的。
称[(32.6)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-atont)和[(32.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-Ptont)为卡尔曼滤波的“更新步”。

下面递推计算\(\boldsymbol a\_{t+1}\)和\(P\_{t+1}\)。
\[\begin{aligned}
\boldsymbol a\_{t+1}
=& T\_t \boldsymbol a\_{t|t} \\
=& T\_t \boldsymbol a\_t + T\_t P\_t Z\_t^T F\_t^{-1} \boldsymbol v\_t . \\
P\_{t+1}
=& \text{Var}(\boldsymbol\alpha\_{t+1} | \boldsymbol Y\_t) \\
=& \text{Var}(T\_t \boldsymbol\alpha\_t + R\_t \boldsymbol\eta\_t
| \boldsymbol Y\_t) \\
=& T\_t P\_{t|t} T\_t^T + R\_t Q\_t R\_t^T .
\end{aligned}\]
记
\[\begin{align}
K\_t =& T\_t P\_t Z\_t^T F\_t^{-1} .
\tag{32.8}
\end{align}\]
则
\[\begin{align}
\boldsymbol a\_{t+1}
=& T\_t \boldsymbol a\_t
+ K\_t \boldsymbol v\_t .
\tag{32.9}
\end{align}\]
而
\[\begin{align}
P\_{t+1}
=& T\_t(P\_t - P\_t Z\_t^T F\_t^{-1} Z\_t P\_t)T\_t^T + R\_t Q\_t R\_t^T \\
=& T\_t P\_t T\_t^T - T\_t P\_t Z\_t^T F\_t^{-1} Z\_t P\_t T\_t^T
+ R\_t Q\_t R\_t^T \\
=& T\_t P\_t T\_t^T - T\_t P\_t Z\_t^T K\_t^T
+ R\_t Q\_t R\_t^T \\
=& T\_t P\_t (T\_t - K\_t Z\_t)^T
+ R\_t Q\_t R\_t^T .
\tag{32.10}
\end{align}\]
称[(32.9)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-atp1)和[(32.10)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-Ptp1)为卡尔曼滤波的“预测步”。

递推公式[(32.6)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-atont),[(32.7)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-Ptont),[(32.9)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-atp1)和[(32.10)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-Ptp1)就构成了卡尔曼滤波的递推算法。
在已知\(\boldsymbol Y\_{t-1}\)的基础上，
新增了观测\(\boldsymbol y\_t\)后，
可以更新关于\(\boldsymbol\alpha\_t\)和\(\boldsymbol\alpha\_{t+1}\)的条件分布。

如果直接计算模型[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modobs)和[(32.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modtran)的似然函数，
因为\(\boldsymbol Y\_n\)是\(pn\)维正态分布，
所以需要求一个\(pn \times pn\)阶矩阵的逆矩阵，
计算量是\(O(p^3 n^3)\)阶的。
如果使用卡尔曼滤波，
则只需要计算\(n\)步，
每一步仅需要对\(p\times p\)矩阵\(F\_t\)求逆，
计算量是\(O(p^3 n)\)阶的。

即使\(\boldsymbol y\_t\)是\(p \times 1\)向量，
在实际计算时，
还是将其转化为\(p\)个一维观测值更有利于提高计算效率。

### 32.1.3 卡尔曼滤波公式

设\(\boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\)，
\(\boldsymbol a\_1\), \(P\_1\)已知，
观测值为\(\boldsymbol y\_1, \dots, \boldsymbol y\_n\)。
对\(t=1, 2, \dots, n\)，计算
\[\begin{equation}
\begin{aligned}
\boldsymbol v\_t =& \boldsymbol y\_t - Z\_t \boldsymbol a\_t,
& F\_t =& Z\_t P\_t Z\_t^T + H\_t, \\
\boldsymbol a\_{t|t}
=& \boldsymbol a\_t + P\_t Z\_t^T F\_t^{-1} \boldsymbol v\_t,
& P\_{t|t}
=& P\_t - P\_t Z\_t^T F\_t^{-1} Z\_t P\_t, \\
K\_t =& T\_t P\_t Z\_t^T F\_t^{-1},
& L\_t =& T\_t - K\_t Z\_t, \\
\boldsymbol a\_{t+1}
=& T\_t \boldsymbol a\_t + K\_t \boldsymbol v\_t,
& P\_{t+1}
=& T\_t P\_t L\_t^T + R\_t Q\_t R\_t^T .
\end{aligned}
\tag{32.11}
\end{equation}\]

[(32.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-form)称为卡尔曼滤波。
如果不需要\(\boldsymbol\alpha\_t | \boldsymbol Y\_t\)的分布参数也可以略过\(\boldsymbol a\_{t|t}\)和\(P\_{t|t}\)的计算；
如果计算了\(\boldsymbol a\_{t|t}\)和\(P\_{t|t}\)，
则可以如下计算\(\boldsymbol a\_{t+1}\)和\(P\_{t+1}\)：
\[
\boldsymbol a\_{t+1}
= T\_t \boldsymbol a\_{t|t},
\quad
P\_{t+1}
= T\_t P\_{t|t} T\_t^T + R\_t Q\_t R\_t^T .
\]

### 32.1.4 带有偏移项的模型

可以在观测方程和状态方程中分别加入一个均值偏移项：
\[\begin{aligned}
\boldsymbol y\_t
=& Z\_t \boldsymbol \alpha\_t
+ \boldsymbol d\_t
+ \boldsymbol \varepsilon\_t,
\ \boldsymbol \varepsilon\_t \sim \text{N}(0, H\_t), \\
\boldsymbol \alpha\_{t+1}
=& T\_t \boldsymbol \alpha\_t
+ \boldsymbol c\_t
+ R\_t \boldsymbol \eta\_t,
\ \boldsymbol \eta\_t \sim \text{N}(0, Q\_t), \\
\boldsymbol \alpha\_1 \sim& \text{N}(\boldsymbol a\_1, P\_1) .
\end{aligned}\]

其中\(p \times 1\)向量\(\boldsymbol d\_t\)已知，
\(m \times 1\)向量\(\boldsymbol c\_t\)已知。
这时，
卡尔曼滤波公式变成：
\[\begin{aligned}
\boldsymbol v\_t
=& \boldsymbol y\_t - Z\_t \boldsymbol a\_t - \boldsymbol d\_t,
& F\_t =& Z\_t P\_t Z\_t^T + H\_t, \\
\boldsymbol a\_{t|t}
=& \boldsymbol a\_t + P\_t Z\_t^T F\_t^{-1} \boldsymbol v\_t,
& P\_{t|t}
=& P\_t - P\_t Z\_t^T F\_t^{-1} Z\_t P\_t, \\
&& K\_t =& T\_t P\_t Z\_t^T F\_t^{-1}, \\
\boldsymbol a\_{t+1}
=& T\_t \boldsymbol a\_t
+ K\_t \boldsymbol v\_t
+ \boldsymbol c\_t,
& P\_{t+1}
=& T\_t P\_t (T\_t - K\_t Z\_t)^T + R\_t Q\_t R\_t^T .
\end{aligned}\]
即仅有\(\boldsymbol v\_t\)和\(\boldsymbol a\_{t+1}\)公式改变了。

### 32.1.5 稳定状态

若模型[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modobs)、[(32.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modtran)中的均值\(Z\_t, H\_t, T\_t, R\_t, Q\_t\)都是不随时间变化的，
则\(n \to \infty\)时\(P\_t\), \(K\_t\), \(F\_t\)极限存在。
记极限为\(\bar P, \bar K, \bar F\)，
应满足方程
\[\begin{aligned}
\bar P =& T \bar P T^T
- T \bar P Z^T \bar F^{-1} Z \bar P T^T
+ R Q R^T , \\
\bar K =& T \bar P Z^T \bar F^{-1} , \\
\bar L =& T - \bar K Z, \\
\bar F =& Z \bar P Z^T + H .
\end{aligned}\]
当\(t\)充分大时，
可以恒定取\(P\_t = P\_{t|t} = \bar P\),
\(K\_t = \bar K\),
\(L\_t = \bar L\),
\(F\_t = \bar F\)，
不需要再计算\(P\_{t|t}\)、\(K\_t\)、\(L\_t\)、\(F\_t\)和\(P\_{t+1}\)，
可以节省计算量。

### 32.1.6 状态和观测值的一步预测误差

观测值的一步预报误差是\(\boldsymbol v\_t = \boldsymbol y\_t - E(\boldsymbol y\_t | \boldsymbol Y\_{t-1}) = \boldsymbol y\_t - Z\_t \boldsymbol a\_t\)。
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)可知\(\boldsymbol v\_t\)与\(\boldsymbol Y\_{t-1}\)独立，
从而各个\(\boldsymbol v\_t\)相互独立。
有
\[
p(\boldsymbol y\_1, \dots, \boldsymbol y\_n)
= p(\boldsymbol y\_1)
\prod\_{t=2}^n p(\boldsymbol y\_t | \boldsymbol Y\_{t-1})
= \prod\_{t=1}^n p(\boldsymbol v\_t)
\]

令
\[
\boldsymbol x\_t
= \boldsymbol\alpha\_t - E(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1})
= \boldsymbol\alpha\_t - \boldsymbol a\_t,
\]
称\(\boldsymbol x\_t\)为状态的一步预测误差。
下面给出\(\boldsymbol x\_t, \boldsymbol v\_t\)的模型。
\[\begin{aligned}
\boldsymbol v\_t
=& \boldsymbol y\_t - Z\_t \boldsymbol a\_t \\
=& Z\_t \boldsymbol\alpha\_t
+ \boldsymbol\varepsilon\_t
- Z\_t \boldsymbol a\_t \\
=& Z\_t \boldsymbol x\_t + \boldsymbol\varepsilon\_t .
\end{aligned}\]
而
\[\begin{aligned}
\boldsymbol x\_{t+1}
=& \boldsymbol\alpha\_{t+1} - \boldsymbol a\_{t+1} \\
=& T\_t \boldsymbol\alpha\_t + R\_t \boldsymbol\eta\_t
- T\_t \boldsymbol a\_t - K\_t \boldsymbol v\_t \\
=& T\_t \boldsymbol x\_t + R\_t \boldsymbol\eta\_t
- K\_t Z\_t \boldsymbol x\_t
- K\_t \boldsymbol\varepsilon\_t \\
=& L\_t \boldsymbol x\_t
+ (R\_t \boldsymbol\eta\_t - K\_t \boldsymbol\varepsilon\_t) .
\end{aligned}\]
统一写成
\[\begin{align}
\boldsymbol v\_t
=& Z\_t \boldsymbol x\_t + \boldsymbol\varepsilon\_t,
\tag{32.12} \\
\boldsymbol x\_{t+1}
=& L\_t \boldsymbol x\_t
+ (R\_t \boldsymbol\eta\_t - K\_t \boldsymbol\varepsilon\_t) .
\tag{32.13}
\end{align}\]
这是类似于SSM的模型，
\(\boldsymbol x\_t\)作为状态向量，
\(\boldsymbol v\_t\)作为观测值向量。
与模型[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modobs)、[(32.2)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modtran)不完全相符的是这两个方程的误差项不是相互独立的。

## 32.2 状态平滑算法

### 32.2.1 问题和记号

考虑给定所有观测\(\boldsymbol Y\_n\)条件下状态\(\boldsymbol\alpha\_t\)的条件分布，
包括联合分布。
\((\boldsymbol\alpha\_1, \dots, \boldsymbol\alpha\_t)\)在\(\boldsymbol Y\_n\)给定条件下的联合分布也是多元正态分布，
所以只需要计算\(\hat{\boldsymbol\alpha}\_t = E(\boldsymbol\alpha\_t | \boldsymbol Y\_n)\)，
\(V\_t = \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_n)\)，
\(\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol\alpha\_s | \boldsymbol Y\_n)\)。
称\(\hat{\boldsymbol\alpha}\_t\)为状态平滑，
简称平滑。

仍设\(\boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\),
\(\boldsymbol a\_1, P\_1\)已知。

若考虑\(E(\boldsymbol\alpha\_t | \boldsymbol y\_s, \dots, \boldsymbol y\_n)\)，
其中\(s, n\)给定，
\(t \in [s, n]\)，则称这样的问题为固定区间平滑。

若考虑\(E(\boldsymbol\alpha\_t | \boldsymbol y\_1, \dots, \boldsymbol y\_n)\)，
但\(t\)固定，
\(n=t+1, t+2, \dots\)可变，
则称为“定点”平滑。

若考虑\(E(\boldsymbol\alpha\_{n-j} | \boldsymbol y\_1, \dots, \boldsymbol y\_n)\)，
其中\(j\)固定，
\(n=j+1, j+2, \dots\)可变，
则称为“定滞后”平滑。

本节考虑的是给定所有样本\(\boldsymbol y\_1, \dots, \boldsymbol y\_n\)条件下\(\boldsymbol\alpha\_t\)的估计，
是定区间平滑问题。

### 32.2.2 状态平滑

记
\[
\boldsymbol v\_{t:n}
= \begin{pmatrix}
\boldsymbol v\_t \\ \vdots \\ \boldsymbol v\_n
\end{pmatrix} .
\]
\(\boldsymbol Y\_n\)与\((\boldsymbol Y\_{t-1}, \boldsymbol v\_{t:n})\)可以互相决定。
所以条件分布\(p(\boldsymbol\alpha\_t | \boldsymbol Y\_n)\)，
等于\(p(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1}, \boldsymbol v\_{t:n})\)，
其中\(\boldsymbol Y\_{t-1}\)与\(\boldsymbol v\_{t:n}\)相互独立。
利用定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)来计算状态平滑：
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_t
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_n)
= E(\boldsymbol\alpha\_t
| \boldsymbol Y\_{t-1}, \boldsymbol v\_{t:n}) \\
=& \boldsymbol a\_t
+ \text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_{t:n}
| \boldsymbol Y\_{t-1})
[\text{Var}(\boldsymbol v\_{t:n}
| \boldsymbol Y\_{t-1})]^{-1}
\boldsymbol v\_{t:n} \\
=& \boldsymbol a\_t
+ \sum\_{j=t}^n
\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_j
| \boldsymbol Y\_{t-1})
F\_j^{-1} \boldsymbol v\_j .
\end{aligned}\]
这里以\(\boldsymbol Y\_{t-1}\)条件下的条件分布为基础计算条件期望。
注意\(\boldsymbol v\_j\)与\(\boldsymbol Y\_{t-1}\)相互独立，
所以\(\text{Var}(\boldsymbol v\_t | \boldsymbol Y\_{t-1}) = \text{Var}(\boldsymbol v\_t) = F\_t\)。

需要计算\(\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_j | \boldsymbol Y\_{t-1})\)，
\(j=t,t+1,\dots,n\)。
\[\begin{aligned}
\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_j | \boldsymbol Y\_{t-1})
=& E(\boldsymbol\alpha\_t \boldsymbol v\_j^T | \boldsymbol Y\_{t-1}) \\
=& E[\boldsymbol\alpha\_t
(Z\_j \boldsymbol x\_j + \boldsymbol\varepsilon\_j)^T
| \boldsymbol Y\_{t-1}] \\
=& E(\boldsymbol\alpha\_t \boldsymbol x\_j^T
| \boldsymbol Y\_{t-1}) Z\_j^T .
\end{aligned}\]
利用[(32.13)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-prederr-xt)可得
\[\begin{aligned}
E(\boldsymbol\alpha\_t \boldsymbol x\_t^T
| \boldsymbol Y\_{t-1})
=& E[\boldsymbol\alpha\_t (\boldsymbol\alpha\_t - \boldsymbol a\_t)^T
| \boldsymbol Y\_{t-1}]
= \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1})
= P\_t, \\
E(\boldsymbol\alpha\_t \boldsymbol x\_{t+1}^T
| \boldsymbol Y\_{t-1})
=& E[\boldsymbol\alpha\_t
(L\_t \boldsymbol x\_t + R\_t \boldsymbol\eta\_t
- K\_t \boldsymbol\varepsilon\_t)^T
| \boldsymbol Y\_{t-1}] \\
=& E(\boldsymbol\alpha\_t \boldsymbol x\_t^T
| \boldsymbol Y\_{t-1}) L\_t^T
= P\_t L\_t^T, \\
E(\boldsymbol\alpha\_t \boldsymbol x\_{t+2}^T
| \boldsymbol Y\_{t-1})
=& E[\boldsymbol\alpha\_t
(L\_{t+1} \boldsymbol x\_{t+1} + R\_{t+1} \boldsymbol\eta\_{t+1}
- K\_{t+1} \boldsymbol\varepsilon\_{t+1})^T
| \boldsymbol Y\_{t-1}] \\
=& E(\boldsymbol\alpha\_t \boldsymbol x\_{t+1}^T
| \boldsymbol Y\_{t-1}) L\_{t+1}^T
= P\_t L\_t^T L\_{t+1}^T, \\
& \cdots\cdots \\
E(\boldsymbol\alpha\_t \boldsymbol x\_{n}^T
| \boldsymbol Y\_{t-1})
=& P\_t L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T .
\end{aligned}\]

当\(t=n\)时，
\(L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T\)应看作\(I\_m\)。
当\(t=n-1\)时，
\(L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T\)应看作\(L\_{n-1}^T\)。
有了\(\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_j | \boldsymbol Y\_{t-1})\)公式后，
就可得
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_n
=& \boldsymbol a\_n + P\_n Z\_n^T F\_n^{-1} \boldsymbol v\_n, \\
\hat{\boldsymbol\alpha}\_{n-1}
=& \boldsymbol a\_{n-1}
+ P\_{n-1} Z\_{n-1}^T F\_{n-1}^{-1} \boldsymbol v\_{n-1}
+ P\_{n-1} L\_n^T Z\_{n-1}^T F\_{n-1}^{-1} \boldsymbol v\_{n}, \\
\hat{\boldsymbol\alpha}\_t
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_n) \\
=& \boldsymbol a\_t
+ P\_t Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ P\_t L\_t^T Z\_{t+1}^T F\_{t+1}^{-1} \boldsymbol v\_{t+1}
+ P\_t L\_t^T L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} \boldsymbol v\_{t+2} \\
&\ + \cdots
+ P\_t L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T F\_n^{-1} \boldsymbol v\_n .
\ t=n-2,n-3, \dots, 2, 1 .
\end{aligned}\]

记
\[\begin{aligned}
\boldsymbol r\_n =& \boldsymbol 0, \\
\boldsymbol r\_{n-1} =& Z\_n^T F\_n^{-1} \boldsymbol v\_n,
\end{aligned}\]
并令
\[\begin{align}
\boldsymbol r\_{t-1}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ L\_t^T Z\_{t+1}^T F\_{t+1}^{-1} \boldsymbol v\_{t+1}
+ L\_t^T L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} \boldsymbol v\_{t+2}
\\
&\ + \cdots
+ L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T F\_n^{-1} \boldsymbol v\_n .
\ t=n-2,n-3, \dots, 2, 1.
\tag{32.14}
\end{align}\]
注意
\[\begin{aligned}
\boldsymbol r\_{t}
=& Z\_{t+1}^T F\_{t+1}^{-1} \boldsymbol v\_{t+1}
+ L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} \boldsymbol v\_{t+2} \\
&\ + \cdots
+ L\_{t+1}^T L\_{t+2}^T \cdots L\_{n-1}^T Z\_n^T F\_n^{-1} \boldsymbol v\_n .
\end{aligned}\]
比较可得
\[\begin{aligned}
\boldsymbol r\_{t-1}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ L^T \boldsymbol r\_t,
\ t=n, n-1, \dots, 1 .
\end{aligned}\]

于是，状态平滑的算法为：
\[\begin{equation}
\begin{aligned}
\boldsymbol r\_n =& \boldsymbol 0, \\
\boldsymbol r\_{t-1}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ L^T \boldsymbol r\_t,
\ t=n, n-1, \dots, 1 . \\
\hat{\boldsymbol\alpha}\_t
=& \boldsymbol a\_t + P\_t \boldsymbol r\_{t-1} .
\end{aligned}
\tag{32.15}
\end{equation}\]

\(\boldsymbol r\_{t-1}\)是\(\boldsymbol v\_t, \boldsymbol v\_{t+1}, \dots, \boldsymbol v\_n\)的线性组合。
\(\boldsymbol a\_t\)是\(\boldsymbol Y\_{t-1}\)的线性组合。
于是\(\boldsymbol a\_t + P\_t \boldsymbol r\_{t-1}\)是\(1:(t-1)\)的信息与\(t:n\)的信息的组合。

### 32.2.3 状态平滑方差阵

下面给出\(\text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_n)\)的算法。
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)可得
\[\begin{aligned}
V\_t
=& \text{Var}(\boldsymbol \alpha\_t
| \boldsymbol Y\_{t-1}, \boldsymbol v\_{t:n}) \\
=& \text{Var}(\boldsymbol \alpha\_t
| \boldsymbol Y\_{t-1})
- \text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{t:n}
| \boldsymbol Y\_{t-1})
[\text{Var}(\boldsymbol v\_{t:n}
| \boldsymbol Y\_{t-1})]^{-1}
[\text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{t:n}
| \boldsymbol Y\_{t-1})]^T \\
=& P\_t - \sum\_{j=t}^n
\text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{j}
| \boldsymbol Y\_{t-1})
[\text{Var}(\boldsymbol v\_{j}
| \boldsymbol Y\_{t-1})]^{-1}
[\text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{j}
| \boldsymbol Y\_{t-1})]^T \\
=& P\_t - \sum\_{j=t}^n
\text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{j}
| \boldsymbol Y\_{t-1})
F\_j^{-1}
[\text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{j}
| \boldsymbol Y\_{t-1})]^T .
\end{aligned}\]
由上一小节可知
\[\begin{aligned}
\text{Cov}(\boldsymbol \alpha\_t, \boldsymbol v\_{j}
| \boldsymbol Y\_{t-1})
= P\_t L\_t^T L\_{t+1}^T \cdots L\_{j-1}^T Z\_j^T,
\end{aligned}\]
可得
\[\begin{aligned}
V\_t
=& P\_t - P\_t Z\_t^T F\_t^{-1} Z\_t P\_t
- P\_t L\_t^T Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1} L\_t P\_t \\
& - P\_t L\_t^T L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} Z\_{t+2} L\_{t+1} L\_t P\_t
- \cdots \\
& - P\_t L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T F\_n^{-1}
Z\_n L\_{n-1} \cdots L\_{t+1} L\_t P\_t \\
=& P\_t - P\_t N\_{t-1} P\_t,
\end{aligned}\]
其中
\[\begin{equation}
\begin{aligned}
N\_{t-1}
=& Z\_t^T F\_t^{-1} Z\_t
+ L\_t^T Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1} L\_t \\
& + L\_t^T L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} Z\_{t+2} L\_{t+1} L\_t
+ \cdots \\
& + L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T F\_n^{-1}
Z\_n L\_{n-1} \cdots L\_{t+1} L\_t .
\end{aligned}
\tag{32.16}
\end{equation}\]

当\(t=n\)时\(L\_t^T L\_{t+1}^T \cdots L\_{n-1}^T\)应看作\(I\_m\)，
当\(t=n-1\)时应看作\(L\_{n-1}^T\)。
对比
\[\begin{aligned}
N\_t
=& Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1}
+ L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} Z\_{t+2} L\_{t+1} \\
& + \cdots
+ L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T F\_{n}^{-1}
Z\_n L\_{n-1} \cdots L\_{t+1},
\end{aligned}\]
可见
\[\begin{aligned}
N\_{t-1}
=& Z\_t F\_t^{-1} Z\_t
+ L\_t^T N\_t L\_t,
\ t=n,n-1,\dots, 2, 1.
\end{aligned}\]
并应取\(N\_n = 0\)。

于是，
\(\text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_n)\)的递推算法为
\[\begin{equation}
\begin{aligned}
N\_n =& 0\_{m \times m}, \\
N\_{t-1}
=& Z\_t^T F\_t^{-1} Z\_t
+ L\_t^T N\_t L\_t,
\ t=n, n-1, \dots, 2, 1; \\
V\_t =& P\_t - P\_t N\_{t-1} P\_t .
\end{aligned}
\tag{32.17}
\end{equation}\]

由[(32.14)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-sm-state-rtm1)和[(32.16)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-sm-statevar-Ntm1)，
以及\(\boldsymbol v\_t, \boldsymbol v\_{t+1}, \dots, \boldsymbol v\_n\)相互独立，
可知
\[\begin{aligned}
\text{Var}(\boldsymbol r\_{t-1})
= N\_{t-1},
\ t=n,n-1,\dots,1 .
\end{aligned}\]

### 32.2.4 状态平滑算法汇总

将状态平滑和方差阵计算的算法罗列如下：
\[\begin{equation}
\begin{aligned}
\boldsymbol r\_n =& \boldsymbol 0\_{m \times 1},
& N\_n =& 0\_{m \times m} , \\
\boldsymbol r\_{t-1}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t + L\_t^T \boldsymbol r\_t,
& N\_{t-1}
=& Z\_t^T F\_t^{-1} Z\_t + L\_t^T N\_t L\_t, \\
&&& t=n,n-1,\dots,1 . \\
\hat{\boldsymbol\alpha}\_t
=& \boldsymbol a\_t + P\_t \boldsymbol r\_{t-1},
& V\_t =& P\_t - P\_t N\_{t-1} P\_t .
\end{aligned}
\tag{32.18}
\end{equation}\]
这称为“状态平滑递推公式”。
其中
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_t
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_n),
& V\_t
=& \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_n) .
\end{aligned}\]
\(\boldsymbol r\_{t-1}\)与\(N\_{t-1}\)满足
\[\begin{aligned}
\text{Var}(\boldsymbol r\_{t-1})
= N\_{t-1} .
\end{aligned}\]

结合滤波递推公式[(32.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-form)和状态平滑递推公式[(32.18)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-sm-state-alg)，
可以统称为“卡尔曼滤波平滑算法”。
[(32.11)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-form)是向前递推，
可以获得\(t=1,2,\dots,n\)时刻的\(\boldsymbol a\_t\), \(P\_t\),
\(\boldsymbol v\_t\)，\(K\_t\), \(L\_t\)，
并可以计算似然函数值;
[(32.18)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-sm-state-alg)是向后递推，
可以获得\(t=n,n-1,\dots,1\)时刻的平滑分布参数\(\hat{\boldsymbol\alpha}\_t\), \(V\_t\)，
以及\(\boldsymbol r\_{t-1}\), \(N\_{t-1}\)。

### 32.2.5 平滑结果的在线更新

基于\(\boldsymbol Y\_n\)得到平滑结果后，
又获得了新观测\(\boldsymbol y\_{n+1}\)，
要更新平滑结果。
仍由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)可知
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_{t|n}
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_n), \\
\hat{\boldsymbol\alpha}\_{t|n+1}
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_{n+1})
= E(\boldsymbol\alpha\_t | \boldsymbol Y\_n, \boldsymbol v\_{n+1}) \\
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_n)
- \text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_{n+1}
| \boldsymbol Y\_n)
[\text{Var}(\boldsymbol v\_{n+1} | \boldsymbol Y\_n)]^{-1}
\boldsymbol v\_{n+1} \\
=& \hat{\boldsymbol\alpha}\_{t|n}
+ P\_t L\_t^T \cdots L\_n^T Z\_{t+1}^T F\_{n+1}^{-1} \boldsymbol v\_{n+1},
\ t=1,2,\dots,n; \\
\hat{\boldsymbol\alpha}\_{n+1|n+1}
=& \boldsymbol a\_{n+1}
+ P\_{n+1} Z\_{n+1}^T F\_{n+1}^{-1} \boldsymbol v\_{n+1} . \\
V\_{t|n} =& \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_{n}), \\
V\_{t|n+1} =& \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_{n+1})
= \text{Var}(\boldsymbol\alpha\_t
| \boldsymbol Y\_{n}, \boldsymbol v\_{n+1}) \\
=& \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_{n})
- \text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_{n+1}
| \boldsymbol Y\_{n})
[\text{Var}(\boldsymbol v\_{n+1}
| \boldsymbol Y\_{n})]^{-1}
[\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol v\_{n+1}
| \boldsymbol Y\_{n})]^T \\
=& V\_{t|n}
- P\_t L\_t^T \cdots L\_n^T Z\_{n+1}^T F\_{n+1}^{-1}
Z\_{n+1} L\_n \cdots L\_t P\_t,
\ t=1,2,\dots,n ; \\
V\_{n+1|n+1}
=& P\_{n+1} - P\_{n+1} Z\_{n+1}^T F\_{n+1}^{-1} Z\_{n+1} P\_{n+1} .
\end{aligned}\]

### 32.2.6 定点平滑与定滞后平滑

若对给定\(t\)，
令\(n=t+1, t+2, \dots\)要计算\(\boldsymbol\alpha\_t | \boldsymbol Y\_{n}\)的条件分布，
可以先针对\(1:t\)时间点进行滤波平滑，
然后利用上一小节结果获得\(n=t+1\)时刻的平滑结果，
反复利用上一小节结果即可递推得到\(n=t+2,t+3,\dots\)的平滑结果。

如果对固定的滞后值\(j\)，
要对\(n=j+1,j+2,\dots\)，
计算\(\hat{\boldsymbol\alpha}\_{n-j|n}\)，
需要对固定的\(n\)反向递推计算
\[\begin{aligned}
\boldsymbol r\_n^{(n)}
= \boldsymbol 0, \\
\boldsymbol r\_{t-1}^{(n)}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ L\_t^T \boldsymbol r\_{t}^{(n)},
\ t=n, n-1, \dots, n-j.
\end{aligned}\]
然后得
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_{n-j|n}
=& \boldsymbol a\_{n-j}
+ P\_{n-j} \boldsymbol r\_{n-j-1}^{(n)} .
\end{aligned}\]
对每个\(n\)，反向递推计算
\[\begin{aligned}
N\_n^{(n)}
=& 0, \\
N\_{t-1}^{(n)}
=& Z\_t^T F\_t^{-1} Z\_t
+ L\_t N\_{t}^{(n)} L\_t,
\ t=n,n-1,\dots,n-j,
\end{aligned}\]
然后得
\[\begin{aligned}
V\_{n-j|n}
=& \text{Var}(\boldsymbol\alpha\_{n-j} | \boldsymbol Y\_n)
= P\_{n-j} - P\_{n-j} N\_{n-j-1}^{(n)} P\_{n-j} .
\end{aligned}\]

## 32.3 扰动项的平滑算法

考虑给定\(\boldsymbol Y\_n\)后，
观测扰动项（误差）\(\boldsymbol\varepsilon\_t\)和系统扰动项（误差）\(\boldsymbol\eta\_t\)的估计问题。
这可以用于参数估计和模型诊断。

### 32.3.1 条件均值

记\(\hat{\boldsymbol\varepsilon}\_t = E(\boldsymbol\varepsilon | \boldsymbol Y\_n)\)。
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)有
\[\begin{aligned}
\hat{\boldsymbol\varepsilon}\_t
=& E(\boldsymbol\varepsilon\_t | \boldsymbol Y\_t,
\boldsymbol v\_t, \dots, \boldsymbol v\_n) \\
=& E(\boldsymbol\varepsilon\_t | \boldsymbol Y\_t)
+ \sum\_{j=t}^n \text{Cov}(\boldsymbol\varepsilon\_t,
\boldsymbol v\_j)
F\_j^{-1} \boldsymbol v\_j \\
=& \sum\_{j=t}^n
E(\boldsymbol\varepsilon\_t \boldsymbol v\_j^T)
F\_j^{-1} \boldsymbol v\_j .
\end{aligned}\]
由[(32.12)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-prederr-vt)，
\[\begin{aligned}
E(\boldsymbol\varepsilon\_t \boldsymbol v\_j^T)
=& E(\boldsymbol\varepsilon\_t \boldsymbol x\_j^T) Z\_j^T
+ E(\boldsymbol\varepsilon\_t \boldsymbol\varepsilon\_j^T) \\
=& \begin{cases}
H\_t, & j = t, \\
E(\boldsymbol\varepsilon\_t \boldsymbol x\_j^T) Z\_j^T,
& j=t+1, \dots, n .
\end{cases}
\end{aligned}\]
由[(32.13)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-prederr-xt)，
\[\begin{aligned}
E(\boldsymbol\varepsilon\_t \boldsymbol x\_{t+1}^T)
=& E \{ \boldsymbol\varepsilon\_t
[L\_t \boldsymbol x\_t
+ (R\_t \boldsymbol\eta\_t - K\_t \boldsymbol\varepsilon\_t)]^T \} \\
=& E(\boldsymbol\varepsilon\_t \boldsymbol x\_t^T) L\_t^T
+ E(\boldsymbol\varepsilon\_t \boldsymbol\eta\_t^T) R\_t^T
- E(\boldsymbol\varepsilon\_t \boldsymbol\varepsilon\_t^T) K\_t^T \\
=& -H\_t K\_t^T; \\
E(\boldsymbol\varepsilon\_t \boldsymbol x\_{t+2}^T)
=& E \{ \boldsymbol\varepsilon\_t
[L\_{t+1} \boldsymbol x\_{t+1}
+ (R\_{t+1} \boldsymbol\eta\_{t+1}
- K\_{t+1} \boldsymbol\varepsilon\_{t+1})]^T \} \\
=& E (\boldsymbol\varepsilon\_t \boldsymbol x\_{t+1}^T) L\_{t+1}^T
= -H\_t K\_t^T L\_{t+1}^T; \\
& \cdots\cdots \\
E(\boldsymbol\varepsilon\_t \boldsymbol x\_{n}^T)
=& -H\_t K\_t^T L\_{t+1}^T \cdots L\_{n-1}^T ,
\ t=1,2,\dots,n-1 .
\end{aligned}\]
其中\(L\_{t+1}^T \cdots L\_{n-1}^T\)当\(t=n-1\)时应视为\(I\_m\)，
当\(t=n-2\)时应视为\(L\_{n-1}^T\)。

于是，
\[\begin{equation}
\begin{aligned}
\hat{\boldsymbol\varepsilon}\_t
=& H\_t \left(
F\_t^{-1} \boldsymbol v\_t
- K\_t^T Z\_{t+1}^T F\_{t+1}^{-1} \boldsymbol v\_{t+1}
- K\_t^T L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} \boldsymbol v\_{t+2}
\right. \\
& \left. - \cdots
- K\_t^T L\_{t+1}^T \cdots L\_{n-1}^T
Z\_{n}^T F\_{n}^{-1} \boldsymbol v\_{n}
\right) \\
=& H\_t (F\_t^{-1} \boldsymbol v\_t
- K\_t^T \boldsymbol r\_t) \\
=& H\_t \boldsymbol u\_t,
\ t=n,n-1,\dots,1 .
\end{aligned}
\tag{32.19}
\end{equation}\]
其中
\[\begin{equation}
\begin{aligned}
\boldsymbol u\_t
=& F\_t^{-1} \boldsymbol v\_t - K\_t^T \boldsymbol r\_t,
\end{aligned}
\tag{32.20}
\end{equation}\]
称为“平滑误差”。

记\(\hat{\boldsymbol\eta}\_t = E(\boldsymbol\eta\_t | \boldsymbol Y\_n)\)。
由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)有
\[\begin{aligned}
\hat{\boldsymbol\eta}\_t
=& E(\boldsymbol\eta\_t | \boldsymbol Y\_{t-1},
\boldsymbol v\_t, \dots, \boldsymbol v\_n) \\
=& E(\boldsymbol\eta\_t | \boldsymbol Y\_{t-1})
+ \sum\_{j=t}^n \text{Cov}(\boldsymbol\eta\_t,
\boldsymbol v\_j) F\_j^{-1} \boldsymbol v\_j \\
=& \sum\_{j=t}^n \text{Cov}(\boldsymbol\eta\_t,
\boldsymbol v\_j) F\_j^{-1} \boldsymbol v\_j .
\end{aligned}\]
注意\(\boldsymbol v\_t\)仅依赖于截止到\(\boldsymbol\alpha\_t\)和\(\boldsymbol Y\_{t-1}\)，
\(\boldsymbol\eta\_t\)与这两项独立，
所以上式\(j=t\)的项等于0。

对\(j=t+1,t+2,\dots,n\)，
利用[(32.12)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-kf-prederr-vt)，
以及\(\boldsymbol x\_t\)仅依赖于\(\boldsymbol\alpha\_t\)和\(\boldsymbol Y\_{t-1}\)，
可知\(\boldsymbol x\_t\)与\(\boldsymbol\eta\_t\)独立，
\[\begin{aligned}
\text{Cov}(\boldsymbol\eta\_t,
\boldsymbol v\_j)
=& E(\boldsymbol\eta\_t \boldsymbol v\_j^T)
= E[\boldsymbol\eta\_t
(Z\_j \boldsymbol x\_j + \boldsymbol\varepsilon\_j)^T] \\
=& E(\boldsymbol\eta\_t \boldsymbol x\_j^T) Z\_j^T .
\end{aligned}\]
其中\(E(\boldsymbol\eta\_t \boldsymbol x\_t^T) = 0\)，
\[\begin{aligned}
E(\boldsymbol\eta\_t \boldsymbol x\_{t+1}^T)
=& E \{ \boldsymbol\eta\_t
[L\_t \boldsymbol x\_t
+ (R\_t \boldsymbol\eta\_t -K\_t \boldsymbol\varepsilon\_t)]^T\} \\
=& Q\_t R\_t^T; \\
E(\boldsymbol\eta\_t \boldsymbol x\_{t+2}^T)
=& E \{ \boldsymbol\eta\_t
[L\_{t+1} \boldsymbol x\_{t+1}
+ (R\_{t+1} \boldsymbol\eta\_{t+1}
- K\_{t+1} \boldsymbol\varepsilon\_{t+1})]^T\} \\
=& E (\boldsymbol\eta\_t \boldsymbol x\_{t+1}^T ) L\_{t+1}^T
Q\_t R\_t^T L\_{t+1}^T; \\
E(\boldsymbol\eta\_t \boldsymbol x\_{t+2}^T)
=& Q\_t R\_t^T L\_{t+1}^T L\_{t+2}^T; \\
& \cdots\cdots \\
E(\boldsymbol\eta\_t \boldsymbol x\_{n}^T)
=& Q\_t R\_t^T L\_{t+1}^T \cdots L\_{n-1}^T .
\end{aligned}\]
于是
\[\begin{equation}
\begin{aligned}
\hat{\boldsymbol\eta}\_t
=& Q\_t R\_t^T \left(
Z\_{t+1}^T F\_{t+1}^{-1} \boldsymbol v\_{t+1}
+ L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} \boldsymbol v\_{t+2}
\right. \\
& \left.
+ \cdots
+ L\_{t+1}^T \cdots L\_{n-1}^T
Z\_{n}^T F\_{n}^{-1} \boldsymbol v\_{n}
\right) \\
=& Q\_t R\_t^T \boldsymbol r\_t,
\ t=n,n-1,\dots,1 .
\end{aligned}
\tag{32.21}
\end{equation}\]

这也给出了\(\boldsymbol r\_t\)的一个解释。
\(\boldsymbol r\_t\)包含了\(\hat{\boldsymbol\eta}\_t\)的信息。

### 32.3.2 条件方差阵

由定理[30.1](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/structts.html#thm:sts-llm-fsp-normc)有
\[\begin{aligned}
\text{Var}(\boldsymbol\varepsilon\_t
| \boldsymbol Y\_n)
=& \text{Var}(\boldsymbol\varepsilon\_t
| \boldsymbol Y\_{t-1},
\boldsymbol v\_t, \dots, \boldsymbol v\_n) \\
=& \text{Var}(\boldsymbol\varepsilon\_t
| \boldsymbol Y\_{t-1})
- \sum\_{j=t}^n \text{Cov}(\boldsymbol\varepsilon\_t,
\boldsymbol v\_j) F\_j^{-1}
[\text{Cov}(\boldsymbol\varepsilon\_t,
\boldsymbol v\_j)]^T \\
=& H\_t - \sum\_{j=t}^n
E(\boldsymbol\varepsilon\_t \boldsymbol v\_j^T)
F\_j^{-1}
[E(\boldsymbol\varepsilon\_t \boldsymbol v\_j^T)]^T .
\end{aligned}\]
由前一小节可知其中
\[\begin{aligned}
E(\boldsymbol\varepsilon\_t \boldsymbol v\_t^T)
=& H\_t, \\
E(\boldsymbol\varepsilon\_t \boldsymbol v\_j^T)
=& -H\_t K\_t^T L\_{t+1}^T \cdots L\_{j-1}^T Z\_j^T,
\ j=t+1, \dots, n .
\end{aligned}\]
从而
\[\begin{aligned}
\text{Var}(\boldsymbol\varepsilon\_t
| \boldsymbol Y\_n)
=& H\_t
- H\_t F\_t^{-1} H\_t
- H\_t K\_t^T Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1} K\_t H\_t \\
& - H\_t K\_t^T L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1}
Z\_{t+2} L\_{t+1} K\_t H\_t \\
& - H\_t K\_t^T L\_{t+1}^T L\_{t+2}^T Z\_{t+3}^T F\_{t+3}^{-1}
Z\_{t+3} L\_{t+2} L\_{t+1} K\_t H\_t \\
& - \cdots \\
& - H\_t K\_t^T L\_{t+1}^T \cdots L\_{n-1}^T Z\_{n}^T F\_{n}^{-1}
Z\_{n} L\_{n-1} \cdots L\_{t+1} K\_t H\_t \\
=& H\_t
- H\_t \left[
F\_t^{-1}
+ K\_t^T Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1} K\_t
\right. \\
& + K\_t L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1}
Z\_{t+2} L\_{t+1} K\_t \\
& + K\_t L\_{t+1}^T L\_{t+2}^T Z\_{t+3}^T F\_{t+3}^{-1}
Z\_{t+3} L\_{t+2} L\_{t+1} K\_t \\
& + \cdots \\
& \left. + K\_t L\_{t+1}^T \cdots L\_{n-1}^T Z\_{n}^T F\_{n}^{-1}
Z\_{n} L\_{n-1} \cdots L\_{t+1} K\_t
\right] \\
=& H\_t
- H\_t \left[
F\_t^{-1}
+ K\_t^T \left(
Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1}
\right.
\right. \\
& + L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1}
Z\_{t+2} L\_{t+1} \\
& + L\_{t+1}^T L\_{t+2}^T Z\_{t+3}^T F\_{t+3}^{-1}
Z\_{t+3} L\_{t+2} L\_{t+1} \\
& + \cdots \\
& \left. \left.
+ L\_{t+1}^T \cdots L\_{n-1}^T Z\_{n}^T F\_{n}^{-1}
Z\_{n} L\_{n-1} \cdots L\_{t+1}
\right) K\_t \right] H\_t \\
=& H\_t - H\_t (F\_t^{-1} - K\_t^T N\_t K\_t) H\_t \\
=& H\_t - H\_t D\_t H\_t,
\ t=n,n-1,\dots,1 .
\end{aligned}\]
其中
\[\begin{aligned}
D\_t
=& F\_t^{-1} - K\_t^T N\_t K\_t .
\end{aligned}\]

类似地，
\[\begin{aligned}
\text{Var}(\boldsymbol\eta\_t | \boldsymbol Y\_n)
=& \text{Var}(\boldsymbol\eta\_t | \boldsymbol Y\_{t-1},
\boldsymbol v\_t, \dots, \boldsymbol v\_n) \\
=& \text{Var}(\boldsymbol\eta\_t | \boldsymbol Y\_{t-1})
- \sum\_{j=t}^n
\text{Cov}(\boldsymbol\eta\_t, \boldsymbol v\_j)
F\_j^{-1}
[\text{Cov}(\boldsymbol\eta\_t, \boldsymbol v\_j)]^T \\
=& Q\_t
- \sum\_{j=t}^n
E(\boldsymbol\eta\_t \boldsymbol v\_j^T)
F\_j^{-1}
[E(\boldsymbol\eta\_t \boldsymbol v\_j^T)]^T,
\end{aligned}\]
其中
\[\begin{aligned}
E(\boldsymbol\eta\_t \boldsymbol v\_t^T)
=& 0, \\
E(\boldsymbol\eta\_t \boldsymbol v\_{t+1}^T)
=& Q\_t R\_t^T Z\_{t+1}^T, \\
& \cdots\cdots \\
E(\boldsymbol\eta\_t \boldsymbol v\_{n}^T)
=& Q\_t R\_t^T L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T,
\end{aligned}\]
于是
\[\begin{aligned}
\text{Var}(\boldsymbol\eta\_t | \boldsymbol Y\_n)
=& Q\_t
- Q\_t R\_t^T \left(
Z\_{t+1}^T F\_{t+1}^{-1} Z\_{t+1}
\right. \\
& + L\_{t+1}^T Z\_{t+2}^T F\_{t+2}^{-1} Z\_{t+2} L\_{t+1} \\
& + \cdots \\
& \left.
+ L\_{t+1}^T \cdots L\_{n-1}^T Z\_n^T F\_n^{-1}
Z\_n L\_{n-1} \cdots L\_{t+1}
\right) R\_t Q\_t \\
=& Q\_t
- Q\_t R\_t^T N\_t R\_t Q\_t,
\ t=n,n-1,\dots,1 .
\end{aligned}\]

### 32.3.3 扰动项平滑的递推计算公式汇总

为了获得\(\boldsymbol Y\_n\)下\(\boldsymbol\varepsilon\_t\)和\(\boldsymbol\eta\_t\)的条件期望和条件方差，
应反向递推如下：
\[\begin{equation}
\begin{aligned}
\boldsymbol r\_n
=& \boldsymbol 0,
& N\_n =& 0 , \\
\hat{\boldsymbol\varepsilon}\_t
=& H\_t(F\_t^{-1} \boldsymbol v\_t - K\_t^T \boldsymbol r\_t),
& \text{Var}(\boldsymbol\varepsilon\_t | \boldsymbol Y\_n)
=& H\_t - H\_t(F\_t^{-1} + K\_t^T N\_t K\_t) H\_t, \\
\hat{\boldsymbol\eta}\_t
=& Q\_t R\_t^T \boldsymbol r\_t,
& \text{Var}(\boldsymbol\eta\_t | \boldsymbol Y\_n)
=& Q\_t - Q\_t R\_t^T N\_t R\_t Q\_t, \\
\boldsymbol r\_{t-1}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ L\_t^T \boldsymbol r\_t,
& N\_{t-1}
=& Z\_t^T F\_t^{-1} Z\_t + L\_t^T N\_t L\_t, \\
& t=n,n-1, \dots, 1 .
\end{aligned}
\tag{32.22}
\end{equation}\]

引入\(\boldsymbol u\_t\)与\(D\_t\)，
算法可以写成
\[\begin{equation}
\begin{aligned}
\boldsymbol r\_n
=& \boldsymbol 0,
& N\_n =& 0 , \\
\boldsymbol u\_t
=& F\_t^{-1} \boldsymbol v\_t - K\_t^T \boldsymbol r\_t,
& D\_t
=& F\_t^{-1} + K\_t^T N\_t K\_t, \\
\hat{\boldsymbol\varepsilon}\_t
=& H\_t \boldsymbol u\_t,
& \text{Var}(\boldsymbol\varepsilon\_t | \boldsymbol Y\_n)
=& H\_t - H\_t D\_t H\_t, \\
\hat{\boldsymbol\eta}\_t
=& Q\_t R\_t^T \boldsymbol r\_t,
& \text{Var}(\boldsymbol\eta\_t | \boldsymbol Y\_n)
=& Q\_t - Q\_t R\_t^T N\_t R\_t Q\_t, \\
\boldsymbol r\_{t-1}
=& Z\_t^T \boldsymbol u\_t
+ T\_t^T \boldsymbol r\_t,
& N\_{t-1}
=& Z\_t^T D\_t Z\_t
+ T\_t^T N\_t T\_t \\
&&&
- Z\_t^T K\_t^T N\_t T\_t
- T\_t^T N\_t K\_t Z\_t, \\
& t=n,n-1, \dots, 1 .
\end{aligned}
\tag{32.23}
\end{equation}\]

算法[(32.23)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-err-alg-utDt)直接利用了原始的\(T\_t\)和\(Z\_t\)，
这两个矩阵常常是稀疏矩阵，
所以后面的算法通常效率更高。

[(32.23)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-err-alg-utDt)中\(\boldsymbol r\_{t-1}\)递推公式推导如下：
\(\boldsymbol u\_t = F\_t^{-1} \boldsymbol v\_t - K\_t^T \boldsymbol r\_t\)，
所以\(F\_t^{-1} \boldsymbol v\_t = \boldsymbol u\_t + K\_t^T \boldsymbol r\_t\)，
代入[(32.22)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-err-alg-orig)中\(\boldsymbol r\_{t-1}\)递推公式可得
\[\begin{aligned}
\boldsymbol r\_{t-1}
=& Z\_t^T (\boldsymbol u\_t + K\_t^T \boldsymbol r\_t)
+ L\_t^T \boldsymbol r\_t \\
=& Z\_t^T \boldsymbol u\_t + (K\_t Z\_t + L\_t)^T \boldsymbol r\_t \\
=& Z\_t^T \boldsymbol u\_t + T\_t^T \boldsymbol r\_t,
\end{aligned}\]
这里用了\(L\_t = T\_t - K\_t Z\_t\)。

[(32.23)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-err-alg-utDt)中\(N\_{t-1}\)的递推公式推导如下：
在[(32.22)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-err-alg-orig)关于\(N\_{t-1}\)的递推公式中，
代入\(F\_t^{-1} = D\_t - K\_t^T N\_t K\_t\)，
\(L\_t = T\_t - K\_t Z\_t\)，
化简后即得该递推公式。

## 32.4 关于平滑

由
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_t
=& \boldsymbol a\_t + P\_t \boldsymbol r\_{t-1} ,
\end{aligned}\]
可见
\[\begin{aligned}
\boldsymbol r\_{t-1}
=& P\_t^{-1}(\hat{\boldsymbol\alpha}\_t - \boldsymbol a\_t).
\end{aligned}\]

还有许多其它的平滑算法，
这里给出的算法是比较通用且高效的。

## 32.5 平滑分布的协方差

考虑\(\text{Cov}(\boldsymbol\alpha\_t, \boldsymbol\alpha\_s | \boldsymbol Y\_n)\)的计算，
配合前面已有的条件均值、条件方差阵公式，
可以确定\((\boldsymbol\alpha\_1, \dots, \boldsymbol\alpha\_n)\)在\(\boldsymbol Y\_n\)条件下的联合分布。
\(t=s\)时已有递推计算公式，
见[32.2.3](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#ssmlin-sm-statevar)。

内容待完成。

## 32.6 从平滑分布抽样

通过对\(\boldsymbol Y\_n\)条件下从\(\boldsymbol\alpha\_t\),
\(\boldsymbol\varepsilon\_t\), \(\boldsymbol\eta\_t\)条件分布抽样，
可以生成多条类似的样本轨道，
包括状态的轨道，
借以研究模型的表现。
更重要的是，
研究这样的抽样算法，
可以用于解决非高斯、非线性的状态空间模型推断问题。
这种抽样方法称为“抽样平滑”。

这一节考虑在\(\boldsymbol Y\_n\)条件下从\(\boldsymbol\alpha\_t\),
\(\boldsymbol\varepsilon\_t\), \(\boldsymbol\eta\_t\)条件分布抽样，
算法可称为“向前滤波、向后抽样”算法。

### 32.6.1 抽样平滑的均值校正方法

考虑\(\boldsymbol Y\_n\)条件下从\(\boldsymbol\varepsilon\_t\), \(\boldsymbol\eta\_t\)的条件分布抽样。
记
\[\begin{aligned}
\boldsymbol w
= \begin{pmatrix}
\boldsymbol\varepsilon\_1 \\
\boldsymbol\eta\_1 \\
\vdots \\
\boldsymbol\varepsilon\_n \\
\boldsymbol\eta\_n
\end{pmatrix} .
\end{aligned}\]
令
\[\begin{aligned}
\hat{\boldsymbol w}
=& E(\boldsymbol w | \boldsymbol Y\_n),
& W =& \text{Var}(\boldsymbol w | \boldsymbol Y\_n) .
\end{aligned}\]
由线性高斯状态空间模型的高斯过程性质可知\(\boldsymbol w\)在\(\boldsymbol Y\_n\)条件下的分布是\(\text{N}(\hat{\boldsymbol w}, W)\)。
[32.3.3](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#ssmlin-err-alg)中已经给出了\(\hat{\boldsymbol w}\)的算法。
而计算\(W\)是应尽可能避免的，
其中的协方差阵会涉及到复杂与繁重的计算，
直接从这样的高维正态分布抽样涉及到很大的矩阵求逆，
算法不稳定。
这里要介绍的均值校正方法可以避免这样的高维抽样困难。

\(\boldsymbol w\)的无条件分布为
\[
p(\boldsymbol w)
\sim
\text{N}(\boldsymbol 0, \Phi),
\quad
\Phi = \text{diag}(H\_1, Q\_1, \dots, H\_n, Q\_n) .
\]

设\(\boldsymbol w^+\)为从无条件分布\(p(\boldsymbol w)\)中抽取的随机向量。
这只要分别抽取\(\boldsymbol\varepsilon\_t^+\),
\(\boldsymbol\eta\_t^+\), \(t=1,\dots,n\)。

设\(\boldsymbol\alpha\_1 \sim p(\boldsymbol\alpha\_1)\)已知，
从此无条件分布抽取随机向量\(\boldsymbol\alpha\_1^+\)，
然后令\(\boldsymbol y\_1^+ = Z\_1 \boldsymbol\alpha\_1^+ + \boldsymbol\varepsilon\_1^+\)，
\(\boldsymbol\alpha\_2^+ = T\_1 \boldsymbol\alpha\_1^+ + R\_1 \boldsymbol\eta\_1^+\)，
如此递推可以得到\((\boldsymbol\alpha\_1^+, \dots, \boldsymbol\alpha\_n^+)\)和\((\boldsymbol y\_1^+, \dots, \boldsymbol y\_n^+)\)。
这样，
我们得到了一组新的观测轨道\((\boldsymbol y\_1^+, \dots, \boldsymbol y\_n^+)\)，
这组观测值符合原始的[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modobs)-[(32.1)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssm-kf-modobs)模型分布。

记
\[\begin{aligned}
\boldsymbol y^+
= \begin{pmatrix}
\boldsymbol y\_1^+ \\
\vdots \\
\boldsymbol y\_n^+
\end{pmatrix} .
\end{aligned}\]
则\(\boldsymbol y^+\)与\(\boldsymbol Y\_n\)同分布，
且\((\boldsymbol w^+, \boldsymbol y^+)\)与\((\boldsymbol w, \boldsymbol Y\_n)\)同分布。

将\(\boldsymbol y^+\)看成是一组观测数据，
重新进行向前滤波、向后平滑，
得到\(\boldsymbol w\)基于\(\boldsymbol y^+\)的平滑结果
\[
\hat{\boldsymbol w}^+
= E(\boldsymbol w | \boldsymbol y^+) .
\]
\(\hat{\boldsymbol w}^+\)与\(\hat{\boldsymbol w}\)同分布。

由正态分布性质，
条件方差不依赖于作为条件的随机变量，
所以
\[\begin{aligned}
W =& \text{Var}(\boldsymbol w | \boldsymbol Y\_n)
= \text{Var}(\boldsymbol w^+ | \boldsymbol y^+) \\
=& E \{ [\boldsymbol w^+ - \hat{\boldsymbol w}^+]
[\boldsymbol w^+ - \hat{\boldsymbol w}^+]^T \},
\end{aligned}\]
即
\[
\boldsymbol w^+ - \hat{\boldsymbol w}^+
\sim \text{N}(0, W) .
\]
于是
\[
\tilde{\boldsymbol w}
= \boldsymbol w^+ - \hat{\boldsymbol w}^+
+ \hat{\boldsymbol w}
\sim \text{N}(\hat{\boldsymbol w}, W) .
\]
\(\tilde{\boldsymbol w}\)是\(\boldsymbol w\)在\(\boldsymbol Y\_n\)条件下的条件分布的抽样。
这避免了计算很大的方差阵\(W\)以及直接从高维的正态分布抽样的问题。

注意这种方法假定初始分布\(\boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\)已知；
如果初始分布有部分未知参数，
或者是发散先验，
算法需要进行修改。

### 32.6.2 状态向量的抽样

记\(\boldsymbol\alpha = (\boldsymbol\alpha\_1^T, \dots, \boldsymbol\alpha\_n^T)^T\)。
记\(\hat{\boldsymbol\alpha} = E(\boldsymbol\alpha | \boldsymbol Y\_n)\)，
这只要使用向前滤波、向后平滑即可计算。

考虑从条件分布\(p(\boldsymbol\alpha | \boldsymbol Y\_n)\)抽样的问题，
设这样的样本为\(\tilde{\boldsymbol\alpha}\)。

类似上一小节，
可以从无条件分布产生\(\boldsymbol w^+\),
\(\boldsymbol\alpha^+\)以及\(\boldsymbol y^+\)，
然后从\(\boldsymbol y^+\)平滑得到\(\hat{\boldsymbol\alpha}^+ = E(\boldsymbol\alpha | \boldsymbol y^+)\)。
然后只要令
\[
\tilde{\boldsymbol\alpha}
= \boldsymbol\alpha^+ - \hat{\boldsymbol\alpha}^+
+ \hat{\boldsymbol\alpha},
\]
则\(\tilde{\boldsymbol\alpha}\)就是条件分布\(p(\boldsymbol\alpha | \boldsymbol Y\_n)\)的抽样。

## 32.7 缺失值处理

线性高斯状态空间模型对于有缺失值的情形很容易处理。
设\(\boldsymbol y\_t\)在\(t=\tau, \dots, \tau^\*\)区间缺失，
其中\(1 < \tau \leq \tau\* < n\)。
一种办法是将时间轴重新定义，
令\(\tau^\*+1\)为\(\tau-1\)后面的点。
这只要适当调整系统方程中误差项方差阵和转移矩阵即可。

另一种方法则更方便，
不需要调整时间轴。
对\(t=\tau, \dots, \tau^\*\)，
有
\[\begin{aligned}
\boldsymbol a\_{t|t}
=& E(\boldsymbol\alpha\_t | \boldsymbol Y\_t)
= E(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1})
= \boldsymbol a\_t, \\
P\_{t|t}
=& \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_t)
= \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1})
= P\_t, \\
\boldsymbol a\_{t+1}
=& E(\boldsymbol\alpha\_{t+1} | \boldsymbol Y\_t)
= E(T\_t \boldsymbol\alpha\_t + R\_t \boldsymbol\eta\_t
| \boldsymbol Y\_{t-1})
= T\_t \boldsymbol a\_t, \\
P\_{t+1}
=& \text{Var}(\boldsymbol\alpha\_{t+1} | \boldsymbol Y\_t)
= \text{Var}(T\_t \boldsymbol\alpha\_t + R\_t \boldsymbol\eta\_t
| \boldsymbol Y\_{t-1}) \\
=& T\_t P\_t T\_t^T + R\_t Q\_t R\_t^T .
\end{aligned}\]
对比原来的向前滤波公式，
这相当于取\(Z\_t=0\), \(K\_t=0\), \(L\_t=T^T\)对\(t=\tau, \dots, \tau^\*\)。
反向平滑的公式变成
\[\begin{aligned}
\boldsymbol r\_{t-1}
=& T\_t^T \boldsymbol r\_t,
& N\_{t-1}
=& T\_t^T N\_t T\_t,
\ t=\tau^\*, \dots, \tau .
\end{aligned}\]

算法仍是取消高斯分布限制下的最小方差线性无偏估计，
以及贝叶斯估计。
对缺失值的这样的简单处理能力是状态空间模型的优势之一。

如果\(\boldsymbol y\_t\)仅有部分分量缺失怎么办？
设\(\boldsymbol y\_t\)是没有缺失的，
\(\boldsymbol y\_t^\*\)有部分分量缺失，
则\(\boldsymbol y\_t^\* = W\_t \boldsymbol y\_t\)，
其中\(W\_t\)是\(I\_p\)的部分行组成。
这时观测方程变成
\[\begin{aligned}
\boldsymbol y\_t^\*
= Z\_t^\* \boldsymbol\alpha\_t
+ \boldsymbol\varepsilon\_t^\*,
\ \boldsymbol\varepsilon\_t^\*
\sim \text{N}(0, H\_t^\*),
\end{aligned}\]
其中\(Z\_t^\* = W\_t Z\_t\)，
\(\boldsymbol\varepsilon\_t^\* = W\_t \boldsymbol\varepsilon\_t\)，
\(H\_t^\* = W\_t H\_t W\_t^T\)。
只要将算法推广到允许\(\boldsymbol y\_t\)的维数可变，
就可以按正常的卡尔曼滤波算法进行计算。
还可以按\(\hat{\boldsymbol y}\_t = Z\_t \hat{\boldsymbol\alpha}\_t\)进行缺失值估计。

从滤波、平滑公式可以看出，
实际上\(\boldsymbol y\_t\)的维数可以推广到依赖于\(t\)，
这不会改变滤波、平滑公式。

可以将\(\boldsymbol y\_t\)转化成每一个分量单独作为一步，
这样就可以统一地处理有部分分量缺失的问题。

## 32.8 向前预测

只要将未来值作为缺失值向前滤波，
就可以产生最小均方误差的预测。
条件期望是最小均方误差的，
所以若\(\boldsymbol y\_1, \dots, \boldsymbol y\_n\)已知，
要预测\(\boldsymbol y\_{n+j}\), \(j=1,\dots,J\)，
应计算
\[\begin{aligned}
E(\boldsymbol y\_{n+j} | \boldsymbol Y\_n)
=& E(Z\_{n+j} \boldsymbol\alpha\_{n+j} + \boldsymbol\varepsilon\_{n+j}
| \boldsymbol Y\_n) \\
=& Z\_{n+j} E(\boldsymbol\alpha\_{n+j} | \boldsymbol Y\_n) .
\end{aligned}\]
易见
\[\begin{aligned}
E(\boldsymbol\alpha\_{n+1} | \boldsymbol Y\_n)
=& \boldsymbol a\_{n+1}
= T\_n \boldsymbol a\_n, \\
\text{Var}(\boldsymbol\alpha\_{n+1} | \boldsymbol Y\_n)
=& P\_{n+1}, \\
E(\boldsymbol\alpha\_{n+j+1} | \boldsymbol Y\_n)
=& E(T\_{n+j} \boldsymbol\alpha\_{n+j}
+ R\_{n+j} \boldsymbol\eta\_{n+j} | \boldsymbol Y\_n) \\
=& T\_{n+j} E(\boldsymbol\alpha\_{n+j} | \boldsymbol Y\_n),
\ j=1,2,\dots,J-1 . \\
\text{Var}(\boldsymbol\alpha\_{n+j+1} | \boldsymbol Y\_n)
=& T\_{n+j} \text{Var}(\boldsymbol\alpha\_{n+j} | \boldsymbol Y\_n) T\_{n+j}^T
+ R\_{n+j} Q\_{n+j} R\_{n+j}^T .
\end{aligned}\]
如此递推可得\(E(\boldsymbol y\_{n+j} | \boldsymbol Y\_n)\)的值，
以及其均方误差为
\[\begin{aligned}
\text{Var}(\boldsymbol y\_{n+j} | \boldsymbol Y\_n)
=& Z\_{n+j}
\text{Var}(\boldsymbol\alpha\_{n+j} | \boldsymbol Y\_n)
Z\_{n+j}^T
+ H\_{n+j} .
\end{aligned}\]

这相当于将\(\boldsymbol y\_{n+j}\)看成缺失值然后向前进行卡尔曼滤波。
对\(j=1,2,\dots,J\)，
令\(Z\_{n+j}=0\), \(K\_{n+j}=0\), \(L\_{n+j} = T\_{n+j}\)。

## 32.9 初始分布问题

### 32.9.1 问题介绍

在实际的线性高斯状态空间模型中，
\(\boldsymbol\alpha\_1\)的初始分布\(\text{N}(\boldsymbol a\_1, P\_1)\)通常是未知的，
或者部分参数未知。
所以\(\boldsymbol\alpha\_1\)的一般模型为
\[\begin{equation}
\boldsymbol\alpha\_1
= \boldsymbol a + A \boldsymbol\delta
+ R\_0 \boldsymbol\eta\_0 ,
\tag{32.24}
\end{equation}\]
其中\(\boldsymbol a\)为\(m \times 1\)已知值，
一般是\(\boldsymbol 0\)。
\(\boldsymbol\eta\_0\)是\(m-q\)维随机向量，
\(\boldsymbol\eta\_0 \sim \text{N}(\boldsymbol 0, Q\_0)\)，
\(Q\_0\)已知，
代表了初始分布中方差已知的成分，
\(R\_0\)是\(m \times (m-q)\)矩阵，
其列向量来自于\(I\_m\)矩阵中的列向量。
\(\boldsymbol\delta\)是\(q \times 1\)未知向量，
可能是非随机的，
可以参与到最大似然估计中；
也可能看成是方差无穷大的正态随机向量。
\(A\)是\(m \times q\)矩阵，
其列向量来自\(I\_m\)中的列向量。
要求\(A^T R\_0 = 0\)，
且\(A\)的列向量与\(R\_0\)的列向量的集合包含\(I\_m\)的\(g\)个列向量(\(g \leq m\))。

先考虑\(\boldsymbol\delta\)是方差无穷大的正态随机向量的处理方法。
设\(\boldsymbol\delta \sim \text{N}(\boldsymbol 0, \kappa I\_q)\)，
其中\(\kappa \to \infty\)，
称为发散的分布。
这时\(\boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a, P\_1)\)，
\[\begin{equation}
P\_1
= \kappa P\_\infty + P\_\*,
\tag{32.25}
\end{equation}\]
其中\(P\_\infty = A A^T\)是对角元素仅取1和0的对角阵，
共有\(q\)个1和\(m-q\)个0，
\(P\_\* = R\_0 Q\_0 R\_0^T\)。
[(32.25)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-intro-decomp)这样的初始分布设定称为“发散初始化”。
当\(P\_\infty\)的\((i,i)\)元素非零时不妨设\(\boldsymbol a\)的第\(i\)元素为零。

发散先验时进行卡尔曼滤波的一个简化做法是取足够大的\(\kappa\)值，
然后从已知的\(\boldsymbol a\)和\(P\_1\)出发进行递推，
但这样会造成较大的舍入误差。
更好的做法是推导当\(\kappa \to \infty\)时按照[(32.25)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-intro-decomp)分布的精确递推算法。

推导的思想是将矩阵乘法表示为\(\kappa^{-1}\)的幂级数形式，
令\(\kappa\to\infty\)得到主要部分。
算法中涉及\(F\_t^{-1}\)，
对于\(p=1\)情形，
与\(P\_\infty\)对应的\(F\_t\)部分或者是0，
或者是一个取正值的标量；
对于\(p>1\)的情形，
\(F\_t\)中相应于\(P\_\infty\)的部分有可能不满秩，
我们先假设\(F\_t\)中相应于\(P\_\infty\)的部分或者满秩，
或者等于0。
将算法转化为\(p=1\)可以避免\(F\_t\)不满秩问题。

### 32.9.2 精确初始化方法

这里用\(O(\kappa^{-j})\)表示函数\(f(\kappa)\)满足\(\lim\_{\kappa\to\infty} \kappa^j f(\kappa)\)存在有限。

当\(P\_1\)有[(32.25)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-intro-decomp)那样的分解时，
可以证明
\[\begin{equation}
P\_t
= \kappa P\_{\infty,t} + P\_{\*,t}
+ O(\kappa^{-1}),
\ t=2,\dots,n,
\tag{32.26}
\end{equation}\]
其中\(P\_{\infty,t}\)和\(P\_{\*,t}\)不依赖于\(\kappa\)。
一般情况下存在\(d\)，\(d\)远小于\(n\)，
当\(t>d\)时\(P\_{\infty,t}=0\),
对\(t=d+1,\dots,n\)可以取\(P\_t = P\_{\*,t}\)，
使用正常的向前滤波算法即可，
这样就消除了发散先验的影响。
如果初始分布参数\(\boldsymbol a\_1\)和\(P\_1\)完全已知，
则\(P\_{\infty,t}=0\), \(t=1,\dots, n\)，
即\(d=0\)，
不需要使用发散先验。

#### 32.9.2.1 初始化阶段

对\(t=1,\dots,d\)，
\(P\_{\infty,t} \neq 0\)，
\(P\_t\)的方差值中存在\(\infty\)，
但我们可以正常地进行卡尔曼滤波递推，
关键是在求\(F\_t^{-1}\)时，
可以将\(\kappa^{-1}\)项令\(\kappa\to\infty\)从而忽略掉。

取\(P\_{\infty,1} = P\_\infty = A A^T\)，
这是一个主对角元素仅取1和0的对角阵。
取\(P\_{\*,1} = P\_\* = R\_0 Q\_0 R\_0^T\)。

由[(32.26)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-exact-ptdec)以及\(F\_t = Z\_t P\_t Z\_t^T + H\_t\)可知\(F\_t\)也有分解
\[\begin{equation}
F\_t = \kappa F\_{\infty,t} + F\_{\*,t} + O(\kappa^{-1}) .
\tag{32.27}
\end{equation}\]
对比[(32.26)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-exact-ptdec)可得
\[\begin{aligned}
F\_{\infty,t}
=& Z\_t P\_{\infty,t} Z\_t^T,
& F\_{\*,t}
=& Z\_t P\_{\*,t} Z\_t^T + H\_t .
\end{aligned}\]
令\(M\_t = P\_t Z\_t^T\)，
则
\[\begin{aligned}
M\_t =& \kappa M\_{\infty,t} + M\_{\*,t} + O(\kappa^{-1}), \\
M\_{\infty,t} =& P\_{\infty,t} Z\_t^T,
& M\_{\*,t}
=& P\_{\*,t} Z\_t^T .
\end{aligned}\]

注意\(M\_{\infty,t} = 0 \Longleftrightarrow F\_{\infty,t} = 0\)，
\(P\_{\infty,t} = 0 \Longrightarrow M\_{\infty,t} = 0\)。
一旦\(P\_{\infty,t} = 0\)，
则令\(d=t-1\)，对\(t=d+1, \dots, n\)，
就不需要再考虑发散先验部分，
令\(P\_t = P\_{\*,t}\)按照正常的卡尔曼滤波向前递推即可。

在\(t=1,\dots,d\)步，
需要计算\(F\_t^{-1}\)，
要考虑三种情况：

* \(F\_t = 0\);
* \(F\_t\)正定；
* \(F\_t \neq 0\)但不满秩。

我们仅处理前两种情况。
当观测为一元时间序列(\(p=1\))时，
只有前两种情况；
当观测为多元时，
一般也较少遇到第三种情况，
一旦遇到，
可以采取将多元观测转化为一元观测的办法。

考虑\(F\_t^{-1}\)求解。
对\(F\_t^{-1}\)的[(32.27)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-exact-Ftdec)关于\(\kappa^{-1}\)做泰勒展开，
得
\[\begin{aligned}
F\_t^{-1}
=& [ \kappa F\_{\infty,t} + F\_{\*,t} + O(\kappa^{-1})]^{-1} \\
=& F\_t^{(0)} + \kappa^{-1} F\_t^{(1)}
+ \kappa^{-2} F\_t^{(2)} + O(\kappa^{-3}).
\end{aligned}\]
用待定系数法求解，
当\(F\_{\infty,t}\)正定时，可解得
\[\begin{aligned}
F\_{\infty,t}
=& Z\_t P\_{\infty,t} Z\_t^T,
& F\_{\*,t}
=& Z\_t P\_{\*,t} Z\_t^T + H\_t , \\
M\_{\infty,t} =& P\_{\infty,t} Z\_t^T,
& M\_{\*,t}
=& P\_{\*,t} Z\_t^T ,\\
F\_t^{(0)}
=& 0,
\qquad F\_t^{(1)} = F\_{\infty,t}^{-1},
& F\_t^{(2)} =& -F\_{\infty,t}^{-1} F\_{\*,t} F\_{\infty,t}^{-1} . \\
K\_t =& K\_t^{(0)} + \kappa^{-1} K\_t^{(1)} + O(\kappa^{-2}), \\
K\_t^{(0)} =& T\_t M\_{\infty,t} F\_t^{(1)},
& K\_t^{(1)} =& T\_t M\_{\*,t} F\_t^{(1)} + T\_t M\_{\infty,t} F\_t^{(2)}, \\
L\_t =& L\_t^{(0)} + \kappa^{-1} L\_t^{(1)} + O(\kappa^{-2}), \\
L\_t^{(0)} =& T\_t - K\_t^{(0)} Z\_t,
& L\_t^{(1)} =& -K\_t^{(1)} Z\_t .
\end{aligned}\]

如果\(F\_{\infty,t}=0\)，
则\(M\_{\infty,t}=0\)，
\[\begin{aligned}
F\_{\infty,t}
=& 0,
& F\_{\*,t}
=& Z\_t P\_{\*,t} Z\_t^T + H\_t , \\
M\_{\infty,t} =& 0,
& M\_{\*,t}
=& P\_{\*,t} Z\_t^T ,\\
F\_t =& F\_{\*,t} + O(\kappa^{-1}),
& M\_t =& M\_{\*,t} + O(\kappa^{-1}), \\
F\_t^{-1} =& F\_{\*,t}^{-1} + O(\kappa^{-1}), \\
F\_t^{(0)} =& F\_{\*,t}^{-1},
& F\_t^{(1)} =& F\_t^{(2)} = 0, \\
K\_t =& T\_t M\_{\*,t} F\_{\*,t}^{-1} + O(\kappa^{-1}), \\
K\_t^{(0)} =& T\_t M\_{\*,t} F\_{\*,t}^{-1},
& K\_t^{(1)} =& 0, \\
L\_t =& T\_t - K\_t Z\_t, \\
L\_t^{(0)} =& T\_t - K\_t^{(0)} Z\_t ,
& L\_t^{(1)} =& 0 .
\end{aligned}\]

关于\(\boldsymbol a\_t\)的递推，
易见
\[
\boldsymbol a\_t
= \boldsymbol a\_t^{(0)}
+ \kappa^{-1} \boldsymbol a\_t^{(1)}
+ O(\kappa^{-2}) ,
\]
递推时令\(\kappa\to\infty\)，
所以不必计算\(\boldsymbol a\_t^{(1)}\)。
取初值\(\boldsymbol a\_1^{(0)} = \boldsymbol a\),
\(\boldsymbol a\_1^{(1)} = \boldsymbol 0\)。
类似有
\[
\boldsymbol v\_t
= \boldsymbol v\_t^{(0)}
+ \kappa^{-1} \boldsymbol v\_t^{(1)}
+ O(\kappa^{-2}) ,
\]
递推时不必计算\(\boldsymbol v\_t^{(1)}\)，
取\(\boldsymbol v\_t^{(0)} = \boldsymbol y\_t - Z\_t \boldsymbol a\_t^{(0)}\)，
\(\boldsymbol v\_t^{(1)} = -Z\_t \boldsymbol a\_t^{(1)}\)。
对\(t=1,\dots,d\)只要计算
\[\begin{aligned}
\boldsymbol v\_t^{(0)}
=& \boldsymbol y\_t - Z\_t \boldsymbol a\_t^{(0)}, \\
\boldsymbol a\_{t+1}^{(0)}
=& T\_t \boldsymbol a\_t^{(0)}
+ K\_t^{(0)} \boldsymbol v\_t^{(0)} .
\end{aligned}\]

关于\(P\_{\infty,t}\)和\(P\_{\*,t}\),
可得
\[\begin{aligned}
P\_{\infty,t+1}
=& T\_t P\_{\infty,t} [L\_t^{(0)}]^T, \\
P\_{\*,t+1}
=& T\_t P\_{\infty,t} [L\_t^{(1)}]^T
+ T\_t P\_{\*,t} [L\_t^{(0)}]^T
+ R\_t Q\_t R\_t^T .
\end{aligned}\]
当\(F\_t=0\)时上面的\(L\_t^{(1)}=0\)。

#### 32.9.2.2 初始化以后的衔接

考虑\(t=d+1, \dots, n\)的向前递推计算。
可以证明，
在模型设置合理的情况下，
存在\(d \geq 0\)，
使得\(P\_{\infty,t} \neq 0\),
\(t \leq d\);
\(P\_{\infty,t} = 0\),
\(t > d\)。
证明见([Durbin and Koopman 2012](#ref-DurbinKoopman2012:TSASSM)) P.129节5.2.2。

考虑[(32.24)](https://www.math.pku.edu.cn/teachers/lidf/course/fts/ftsnotes/html/_ftsnotes/ssmlingau.html#eq:ssmlin-init-intro-alpha1)中\(\boldsymbol\delta\)，
这是\(q\)维的发散先验部分，
当\(t > d\)时\(P(\boldsymbol\delta | \boldsymbol Y\_t)\)为有限值，
从而\(P\_t\)有限。
对\(t=d+1, \dots, n\)，
只要取\(\boldsymbol a\_{d+1} = \boldsymbol a\_{d+1}^{(0)}\),
\(P\_{d+1} = P\_{\*,d+1}\)，
就可以按已知初始分布时的卡尔曼向前递推公式进行递推计算了。

#### 32.9.2.3 初始化阶段方差阵简化公式

对\(t=1,\dots,d\)，
可以将\(P\_{\*,t}\)和\(P\_{\infty,t}\)的递推写成一个统一的公式。
记
\[\begin{aligned}
P\_t^\dagger
= \begin{bmatrix}
P\_{\*,t} & P\_{\infty,t}
\end{bmatrix},
\quad
L\_t^\dagger
= \begin{bmatrix}
L\_t^{(0)} & L\_t^{(1)} \\
0 & L\_t^{(0)}
\end{bmatrix},
\end{aligned}\]
取初值\(P\_1^\dagger = \begin{bmatrix} P\_{\*} & P\_{\infty} \end{bmatrix}\)，
对\(t=1,\dots,d\)有
\[\begin{aligned}
P\_{t+1}^\dagger
= T\_t P\_t^\dagger [L\_t^\dagger]^T
+ \begin{bmatrix} R\_t Q\_t R\_t^T & 0 \end{bmatrix} .
\end{aligned}\]
这与非发散先验时的公式形式相同。

若某个\(F\_{\infty,t}=0\)，
则公式中
\[\begin{aligned}
K\_t^{(0)}
=& T\_t M\_{\*,t} F\_{\*,t}^{-1},
& L\_t^{(0)}
=& T\_t - K\_t^{(0)} Z\_t,
\quad L\_t^{(1)} = 0 .
\end{aligned}\]

### 32.9.3 精确初始化平滑算法

对\(t=n,n-1,\dots,d+1\)，
仍可以按原来的反向平滑算法进行平滑分布计算。

#### 32.9.3.1 平滑的均值

对\(t=d,\dots,1\)，
考虑
\[\begin{aligned}
\boldsymbol r\_{t-1}
=& Z\_t^T F\_t^{-1} \boldsymbol v\_t
+ L\_t^T \boldsymbol r\_t .
\end{aligned}\]
其中的\(F\_t^{-1}\)和\(\boldsymbol v\_t\), \(L\_t\)都可以按\(\kappa^{-1}\)泰勒展开近似，
所以也有
\[\begin{aligned}
\boldsymbol r\_{t-1}
=& \boldsymbol r\_{t-1}^{(0)}
+ \kappa^{-1} \boldsymbol r\_{t-1}^{(1)}
+ O(\kappa^{-2}) ,
\quad
t=d,\dots,1 .
\end{aligned}\]
用待定系数法可得，
当\(F\_{\infty,t}\)正定时，
\[\begin{aligned}
\boldsymbol r\_{t-1}^{(0)}
=& [L\_t^{(0)}]^T \boldsymbol r\_{t}^{(0)}, \\
\boldsymbol r\_{t-1}^{(1)}
=& Z\_t^T F\_t^{(1)} \boldsymbol v\_{t}^{(0)}
+ [L\_t^{(0)}]^T \boldsymbol r\_{t}^{(1)}
+ [L\_t^{(1)}]^T \boldsymbol r\_{t}^{(0)},
\quad t=d, \dots, 1 . \\
\boldsymbol r\_{d}^{(0)}
=& \boldsymbol r\_{d},
\quad
\boldsymbol r\_{d}^{(1)}
= \boldsymbol 0 .
\end{aligned}\]
可以证明\(\text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_n)\)有限，
从而由待定系数法可得
\[\begin{aligned}
\hat{\boldsymbol\alpha}\_t
=E(\boldsymbol\alpha\_t | \boldsymbol Y\_n)
= \boldsymbol a\_t^{(0)}
+ P\_{\*,t} \boldsymbol r\_{t-1}^{(0)}
+ P\_{\infty,t} \boldsymbol r\_{t-1}^{(1)},
\quad t=d,\dots,1 .
\end{aligned}\]
可以合并写成
\[\begin{aligned}
\boldsymbol r\_{t-1}^\dagger
=& \begin{pmatrix}
\boldsymbol r\_{t-1}^{(0)} \\
\boldsymbol r\_{t-1}^{(1)}
\end{pmatrix}
= \begin{pmatrix}
0 \\
Z\_t^T F\_t^{(1)} \boldsymbol v\_{t}^{(0)}
\end{pmatrix}
+ [L\_t^\dagger]^T \boldsymbol r\_t^\dagger, \\
\hat{\boldsymbol\alpha}\_t
=& \boldsymbol a\_t^{(0)}
+ P\_t^\dagger \boldsymbol r\_{t-1}^\dagger,
\quad t=d,\dots, 1.
\end{aligned}\]
其中初值\(\boldsymbol r\_{d}^\dagger = \begin{pmatrix} \boldsymbol r\_{d} \\ \boldsymbol 0 \end{pmatrix}\)，
这个递推形式与非发散先验时的公式形式相同。

这里略过\(P\_{\infty,t} \neq 0\)但\(F\_{\infty,t}=0\)的讨论，
这种情况比较少见。

#### 32.9.3.2 平滑的方差阵

原来
\[\begin{aligned}
V\_t =& \text{Var}(\boldsymbol\alpha\_t | \boldsymbol Y\_n)
= P\_t - P\_t N\_{t-1} P\_t, \\
N\_n =& 0, \\
N\_{t-1} =& Z\_t^T F\_t^{-1} Z\_t
+ L\_t^T N\_t L\_t .
\end{aligned}\]
若\(F\_{\infty,t}\)正定，则有
\[\begin{aligned}
N\_t
=& N\_t^{(0)}
+ \kappa^{-1} N\_t^{(1)}
+ \kappa^{-2} N\_t^{(2)}
+ O(\kappa^{-3}),
\end{aligned}\]
用待定系数法可得递推公式为\(t=d,\dots,1\)时
\[\begin{aligned}
N\_d^{(0)} =& N\_d,
\quad N\_d^{(1)} = N\_d^{(2)} = 0, \\
N\_{t-1}^{(0)}
=& [L\_t^{(0)}]^T N\_t^{(0)} L\_t^{(0)}, \\
N\_{t-1}^{(1)}
=& Z\_t F\_t^{(1)} Z\_t
+ [L\_t^{(0)}]^T N\_t^{(1)} L\_t^{(0)} \\
& + [L\_t^{(1)}]^T N\_t^{(0)} L\_t^{(0)}
+ [L\_t^{(0)}]^T N\_t^{(0)} L\_t^{(1)}, \\
N\_{t-1}^{(2)}
=& Z\_t^T F\_t^{(2)} Z\_t
+ [L\_t^{(0)}]^T N\_t^{(2)} L\_t^{(0)} \\
& + [L\_t^{(0)}]^T N\_t^{(1)} L\_t^{(1)}
+ [L\_t^{(1)}]^T N\_t^{(1)} L\_t^{(0)} \\
& + [L\_t^{(0)}]^T N\_t^{(0)} L\_t^{(2)}
+ [L\_t^{(2)}]^T N\_t^{(0)} L\_t^{(0)} \\
& + [L\_t^{(1)}]^T N\_t^{(0)} L\_t^{(1)} .
\end{aligned}\]
可以证明\(V\_t\)都有限，
所以由待定系数法可得
\[\begin{aligned}
V\_t
=& P\_{\*,t} - P\_{\*,t} N\_{t-1}^{(0)} P\_{\*,t} \\
& - P\_{\*,t} N\_{t-1}^{(1)} P\_{\infty,t}
- P\_{\infty,t} N\_{t-1}^{(1)} P\_{\*,t} \\
& - P\_{\infty,t} N\_{t-1}^{(2)} P\_{\infty,t} .
\end{aligned}\]

#### 32.9.3.3 扰动项平滑

内容略过。

#### 32.9.3.4 平滑抽样

内容略过。

### 32.9.4 精确初始化示例

#### 32.9.4.1 结构时间序列模型

考虑局部趋势模型：
\[\begin{aligned}
y\_t =& \mu\_t + e\_t, \\
\mu\_{t+1} =& \mu\_t + \nu\_t + \xi\_t, \\
\nu\_{t+1} =& \nu\_t + \zeta\_t,
\end{aligned}\]
写成状态空间模型，为
\[\begin{aligned}
y\_t =& (1\ 0)
\begin{pmatrix}
\mu\_t \\ \nu\_t
\end{pmatrix}
+ e\_t, \\
\begin{pmatrix}
\mu\_{t+1} \\ \nu\_{t+1}
\end{pmatrix}
=&
\begin{pmatrix}
1 & 1 \\
0 & 1
\end{pmatrix}
\begin{pmatrix}
\mu\_t \\ \nu\_t
\end{pmatrix}
+ \begin{pmatrix}
\xi\_t \\ \zeta\_t
\end{pmatrix} .
\end{aligned}\]
各矩阵
\[\begin{aligned}
Z\_t =& (1, 0),
\quad
& T\_t =& \begin{pmatrix}
1 & 1 \\
0 & 1
\end{pmatrix} , \\
H\_t =& \sigma\_{\varepsilon}^2,
\quad R\_t = I\_2,
& Q\_t =& \sigma\_{\varepsilon}^2
\begin{pmatrix}
q\_\xi & 0 \\
0 & q\_\zeta
\end{pmatrix} .
\end{aligned}\]

\(t=1\)时\(\mu\_1\)和\(\nu\_1\)都是未知的，
作为\(\boldsymbol\delta\)，从而初值
\[\begin{aligned}
\boldsymbol a\_1 = \boldsymbol a\_1^{(0)} = \boldsymbol 0,
\quad
P\_{\*,1} = 0,
\quad P\_{\infty,1} = I\_2 .
\end{aligned}\]

计算第一次更新：
\[\begin{aligned}
F\_{\infty,1}
=& Z\_1 P\_{\infty,1} Z\_1^T = 1,
& F\_{\*,1}
=& Z\_1 P\_{\*,1} Z\_1^T + H\_1
= \sigma\_{\varepsilon}^2 , \\
M\_{\infty,1}
=& P\_{\infty,1} Z\_1^T
= \begin{pmatrix}
1 \\ 0
\end{pmatrix},
& M\_{\*,1}
=& P\_{\*,1} Z\_1^T
= \begin{pmatrix}
0 \\ 0
\end{pmatrix}, \\
F\_1^{(0)} =& 0,
\quad
F\_1^{(1)} = F\_{\infty,1}^{-1} = 1,
& F\_1^{(2)}
=& -F\_{\infty,1}^{-1} F\_{\*,1} F\_{\infty,1}^{-1}
= -\sigma\_{\varepsilon}^2, \\
K\_1^{(0)} =& T\_1 M\_{\infty,1} F\_1^{(1)}
= \begin{pmatrix}
1 \\ 0
\end{pmatrix},
& K\_1^{(1)}
=& T\_1 M\_{\*,1} F\_1^{(1)}
+ T\_1 M\_{\infty,1} F\_1^{(2)} \\
&& =& -\sigma\_{\varepsilon}^2
\begin{pmatrix}
1 \\ 0
\end{pmatrix}, \\
L\_1^{(0)} =& T\_1 - K\_1^{(0)} Z\_1
= \begin{pmatrix}
0 & 1 \\
0 & 1
\end{pmatrix},
& L\_1^{(1)}
=& -K\_1^{(1)} Z\_1
= \sigma\_{\varepsilon}^2
\begin{pmatrix}
1 & 0 \\
0 & 0
\end{pmatrix}, \\
\boldsymbol v\_1^{(0)}
=& y\_1 - Z\_1 \boldsymbol a\_1^{(0)} = y\_1,
\end{aligned}\]
进而
\[\begin{aligned}
\boldsymbol a\_2
=& \boldsymbol a\_2^{(0)}
= T\_1 \boldsymbol a\_1^{(0)} + K\_1^{(0)} \boldsymbol v\_1^{(0)}
= \begin{pmatrix}
y\_1 \\ 0
\end{pmatrix}, \\
P\_{\*,2}
=& T\_1 P\_{\infty,1} [L\_1^{(1)}]^T
+ T\_1 P\_{\*,1} [L\_1^{(0)}]^T
+ R\_t Q\_t R\_t \\
=& \sigma\_{\varepsilon}^2
\begin{pmatrix}
1+q\_{\xi} & 0 \\
0 & q\_{\zeta}
\end{pmatrix}, \\
P\_{\infty,2}
=& T\_1 P\_{\infty,1} [L\_1^{(0)}]^T
= \begin{pmatrix}
1 & 1 \\
1 & 1
\end{pmatrix} .
\end{aligned}\]

继续下一轮更新可得
\[\begin{aligned}
K\_2^{(0)}
=& \begin{pmatrix}
2 \\
1
\end{pmatrix},
& K\_2^{(1)}
=& -\sigma\_{\varepsilon}^2
\begin{pmatrix}
3+q\_\xi \\
2+q\_\xi
\end{pmatrix}, \\
L\_2^{(0)}
=& \begin{pmatrix}
-1 & 1 \\
-1 & 1
\end{pmatrix},
& L\_2^{(1)}
=& \sigma\_{\varepsilon}^2
\begin{pmatrix}
3+q\_\xi & 0 \\
2+q\_\xi & 0
\end{pmatrix},
\end{aligned}\]
进而
\[\begin{aligned}
\boldsymbol a\_3
=& \begin{pmatrix}
2 y\_2 - y\_1 \\
y\_2 - y\_1
\end{pmatrix}, \\
P\_{\*,3}
=& \sigma\_{\varepsilon}^2
\begin{pmatrix}
5 + 2 q\_{\xi} + q\_{\zeta} & 3 + q\_{\xi} + q\_{\zeta} \\
3 + q\_{\xi} + q\_{\zeta} & 2 + q\_{\xi} + 2 q\_{\zeta}
\end{pmatrix}, \\
P\_{\infty,3}
=& \begin{pmatrix}
0 & 0 \\
0 & 0
\end{pmatrix},
\end{aligned}\]
对\(t=3,\dots,n\)可以使用正常的卡尔曼向前滤波更新算法。

## 32.10 观测序列一元化

将\(p\)维的时间序列的状态空间模型设法改写成一元观测值的状态空间模型，
可以避免考虑精确发散先验中\(F\_t\)不等于零也不满秩的情况，
同时可以提高计算效率，
也自然地容许不同时刻的观测值分量个数可变。

### 32.10.1 各分量条件独立的情形

设\(H\_t\)是对角阵，
从而给定\(\boldsymbol\alpha\_t\)时\(\boldsymbol y\_t\)的各分量独立，
这样，
可以将各个分量看成是多个观测，
而不是一个观测的多个分量。
设\(\boldsymbol y\_t = (y\_{t,1}, \dots, y\_{t,p\_t})^T\)，
是\(p\_t \times 1\)向量，
将所有观测值按如下次序排列成一元的观测值序列：
\[
y\_{1,1}, \dots, y\_{1,p\_1}, y\_{2,1}, \dots, y\_{n,p\_n} .
\]
仍设\(\boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\)。
记\(\boldsymbol\varepsilon\_t = (\varepsilon\_{t,1}, \dots, \varepsilon\_{t,p\_t})^T\)，
\[\begin{aligned}
Z\_t = \begin{pmatrix}
Z\_{t,1} \\
\vdots \\
Z\_{t,p\_t}
\end{pmatrix},
\qquad H\_t = \text{diag}(\sigma\_{t,1}^2, \dots, \sigma\_{t,p\_t}^2),
\end{aligned}\]
其中\(Z\_{t,i}\)是\(1 \times m\)行向量。
则原来向量观测值的模型可以转换成如下的一元观测值的模型：
\[\begin{aligned}
y\_{t,i}
=& Z\_{t,i} \boldsymbol\alpha\_{t,i}
+ \varepsilon\_{t,i},
\quad \varepsilon\_{t,i} \sim \text{N}(0, \sigma\_{t,i}^2),
\quad i=1,\dots,p\_t, \\
\boldsymbol\alpha\_{t,i+1}
=& \boldsymbol\alpha\_{t,i},
\quad i=1,\dots, p\_t-1, \\
\boldsymbol\alpha\_{t+1,1}
=& T\_t \boldsymbol\alpha\_{t,p\_t}
+ R\_t \boldsymbol\eta\_t,
\quad t=1,\dots,n .
\end{aligned}\]
则\(\boldsymbol\alpha\_{1,1} = \boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\)，
\(\boldsymbol\alpha\_t = \boldsymbol\alpha\_{t,1}\)，
令
\[\begin{aligned}
\boldsymbol a\_{t,1}
=& E(\boldsymbol\alpha\_{t,1} | \boldsymbol Y\_{t-1})
= E(\boldsymbol\alpha\_{t} | \boldsymbol Y\_{t-1})
= \boldsymbol a\_t, \\
P\_{t,1}
=& \text{Var}(\boldsymbol\alpha\_{t,1} | \boldsymbol Y\_{t-1})
= \text{Var}(\boldsymbol\alpha\_{t} | \boldsymbol Y\_{t-1})
= P\_t, \\
\boldsymbol a\_{t,i}
=& E(\boldsymbol\alpha\_{t,1}
| \boldsymbol Y\_{t-1}, y\_{t,1}, \dots, y\_{t,i-1}), \\
P\_{t,i}
=& \text{Var}(\boldsymbol\alpha\_{t,1}
| \boldsymbol Y\_{t-1}, y\_{t,1}, \dots, y\_{t,i-1}),
\quad i=2,\dots,p\_t,
\ t=1,\dots,n .
\end{aligned}\]

向前滤波的计算公式分为在同一时刻不同分量的更新，
以及不同时刻的更新。

同一时刻的更新公式：
\[\begin{aligned}
v\_{t,i}
=& y\_{t,i} - Z\_{t,i} \boldsymbol a\_{t,i}, \\
F\_{t,i}
=& Z\_{t,i} P\_{t,i} Z\_{t,i}^T + \sigma\_{t,i}^2, \\
K\_{t,i}
=& P\_{t,i} Z\_{t,i}^T F\_{t,i}^{-1}, \\
\boldsymbol a\_{t,i+1}
=& \boldsymbol a\_{t,i} + K\_{t,i} v\_{t,i}, \\
P\_{t,i+1}
=& P\_{t,i} - K\_{t,i} F\_{t,i} K\_{t,i}^T,
\quad i=1,\dots,p\_t,
\ t=1,\dots, n .
\end{aligned}\]

对每一个\(t\)，
在\(i=1,\dots,p\_t\)的循环之前，
\(\boldsymbol a\_{t,1}\)保存了\(\boldsymbol a\_t\)的值，
\(P\_{t,1}\)保存了\(P\_t\)的值；
在\(i=1,\dots,p\_t\)的循环之后，
\(\boldsymbol a\_{t,p\_t+1}\)保存了\(\boldsymbol a\_{t|t}\)的值，
\(P\_{t,p\_t+1}\)保存了\(\Sigma\_{t|t}\)的值，
即滤波分布\(p(\boldsymbol\alpha\_t | \boldsymbol Y\_t)\)。
\(i=1,\dots,p\_t\)的循环起到了利用\(\boldsymbol y\_t\)更新预报分布\(p(\boldsymbol\alpha\_t | \boldsymbol Y\_{t-1})\)的作用。

从\(t\)到\(t+1\)的更新公式为
\[\begin{aligned}
\boldsymbol a\_{t+1,1}
=& T\_t \boldsymbol a\_{t, p\_t+1}, \\
P\_{t+1,1}
=& T\_t P\_{t, p\_t+1} T\_t^T
+ R\_t Q\_t R\_t^T,
\ t=1,\dots,n .
\end{aligned}\]
这一更新将滤波分布\(p(\boldsymbol\alpha\_t | \boldsymbol Y\_t)\)更新为预报分布\(p(\boldsymbol\alpha\_{t+1} | \boldsymbol Y\_t)\)，
这相当于\(\boldsymbol y\_t\)缺失情况下的一步更新，
而\(\boldsymbol y\_t\)的信息已经在前面关于\(i=1,\dots,p\_t\)的循环利用过了。

注意\(F\_{t,i}\)是标量，
在同时刻的更新公式中用到了\(F\_{t,i}^{-1}\)，
如果\(F\_{t,i}=0\)，
注意\(F\_{t,i}\)是用\(\boldsymbol Y\_{t-1}, y\_{t,1}, \dots, y\_{t,i-1}\)预测\(y\_{t,i}\)的均方误差，
\(F\_{t,i}=0\)说明\(y\_{t,i}\)是\(\boldsymbol Y\_{t-1}, y\_{t,1}, \dots, y\_{t,i-1}\)的线性组合，
没有提供额外的信息，
所以\(F\_{t,i}=0\)时递推公式只要变成
\[\begin{aligned}
\boldsymbol a\_{t,i+1}
=& E(\boldsymbol\alpha\_{t,i+1}
| \boldsymbol Y\_{t-1}, y\_{t,1}, \dots, y\_{t,i}) \\
=& E(\boldsymbol\alpha\_{t,i+1}
| \boldsymbol Y\_{t-1}, y\_{t,1}, \dots, y\_{t,i-1}) \\
=& \boldsymbol a\_{t,i}, \\
P\_{t,i+1}
=& P\_{t,i} .
\end{aligned}\]

对于反向递推平滑，
取\(\boldsymbol r\_{n,p\_n} = \boldsymbol 0\),
\(N\_{n,p\_n} = 0\)，
同时刻反向递推
\[\begin{aligned}
L\_{t,i}
=& I\_m - K\_{t,i} Z\_{t,i}, \\
\boldsymbol r\_{t,i-1}
=& Z\_{t,i}^T F\_{t,i}^{-1} v\_{t,i}
+ L\_{t,i}^T \boldsymbol r\_{t,i}, \\
N\_{t,i-1}
=& Z\_{t,i}^T F\_{t,i}^{-1} Z\_{t,i}
+ L\_{t,i}^T N\_{t,i} L\_{t,i},
\quad
i=p\_t, \dots, 1,
\end{aligned}\]
从\(t\)到\(t-1\)递归为
\[\begin{aligned}
\boldsymbol r\_{t-1,p\_{t-1}}
=& T\_{t-1}^T \boldsymbol r\_{t,0}, \\
N\_{t-1,p\_{t-1}}
=& T\_{t-1}^T N\_{t,0} T\_{t-1},
\end{aligned}\]
这时
\[\begin{aligned}
\boldsymbol r\_{t,0} =& \boldsymbol r\_{t-1},
&
N\_{t,0} =& N\_{t-1}, \\
\boldsymbol a\_t =& \boldsymbol a\_{t,1},
&
P\_t =& P\_{t,1}, \\
\hat{\boldsymbol\alpha}\_t
=& \boldsymbol a\_t + P\_t \boldsymbol r\_{t-1},
&
V\_t =& P\_t - P\_t N\_{t-1} P\_t, \\
& t=n, \dots, 1 .
\end{aligned}\]

### 32.10.2 各分量非条件独立的情形

若\(H\_t\)不是对角阵，
上面的方法无法直接使用。
一种办法是将\(\boldsymbol\varepsilon\_t\)整合进入状态向量\(\boldsymbol\alpha\_t\)中，
这样观测方程误差变成了0，
符合方差阵对角阵要求。

另一种办法是对每个时刻\(t\)将\(H\_t\)作LDL分解，
然后将\(\boldsymbol y\_t\)和\(Z\_t\)作变换，
使得新的观测变成方差阵对角形式。

## 32.11 参数的最大似然估计

状态空间模型中的参数，
主要是\(H\_t\)和\(Q\_t\)中的参数，
比如局部水平模型中的\(\sigma\_e^2\), \(\sigma\_\eta^2\)。
但是其它矩阵中也可以含有参数。
有时称这些参数为“超参数”。

因为观测值的联合分布是多元正态分布，
向前的卡尔曼滤波实际上是将很大的方差阵求逆问题，
转换成了对角阵求逆问题，
从而提高了运算效率，
确保了数值稳定性。

### 32.11.1 初始分布已知时的似然函数计算

设初始分布\(\boldsymbol\alpha\_1 \sim \text{N}(\boldsymbol a\_1, P\_1)\),
\(\boldsymbol a\_1\), \(P\_1\)已知。
似然函数为
\[
L(\boldsymbol Y\_n)
= p(\boldsymbol y\_1)
\prod\_{t=2}^n p(\boldsymbol y\_t | \boldsymbol Y\_{t-1}) .
\]
对数似然函数为
\[
\log L(\boldsymbol Y\_n)
= \sum\_{t=1}^n \log p(\boldsymbol y\_t | \boldsymbol Y\_{t-1}) .
\]
其中\(p(\boldsymbol y\_1 | \boldsymbol Y\_0)\)表示\(p(\boldsymbol y\_1)\)。
注意这些条件分布都是多元正态分布，
\(E(\boldsymbol y\_t | \boldsymbol Y\_{t-1}) = Z\_t \boldsymbol a\_t\)，
\(\text{Var}(\boldsymbol y\_t | \boldsymbol Y\_{t-1}) = F\_t\)，
所以
\[\begin{aligned}
\log L(\boldsymbol Y\_n)
=& \sum\_{t=1}^n \log p(\boldsymbol v\_t) \\
=& -\frac{1}{2} np \log(2\pi)
- \frac{1}{2} \sum\_{t=1}^n
\left\{ \log|F\_t|
+ \boldsymbol v\_t^T F\_t^{-1} \boldsymbol v\_t \right\} .
\end{aligned}\]
若\(p>1\)，
这个公式需要所有\(F\_t\)满秩。
如果\(p=1\)或者转化成一元观测值，
则\(F\_t=0\)时只要忽略相应项。

### 32.11.2 发散初始化的似然函数计算

设对\(t=1,\dots,d\)，\(P\_{\infty,t} \neq 0\)，
\(t=d+1,\dots,n\)时\(P\_{\infty,t} = 0\)。
如果沿用原来的似然函数，
会出现\(-\frac{1}{2} q \log\kappa\)这样的趋于负无穷的项，
所以似然函数应该改用发散似然函数
\[
\log L\_d(\boldsymbol Y\_n)
= -\frac{1}{2} n p \log(2\pi)
- \frac{1}{2} \sum\_{t=1}^d w\_t
- \frac{1}{2} \sum\_{t=d+1}^n
\left\{ \log|F\_t|
+ \boldsymbol v\_t^T F\_t^{-1} \boldsymbol v\_t \right\} .
\]
其中
\[
w\_t
= \begin{cases}
\log|F\_{\infty,t}|, & F\_{\infty,t} > 0, \\
\log|F\_{\*,t}|
+ [\boldsymbol v\_t^{(0)}]^T F\_{\*,t}^{-1} \boldsymbol v\_t^{(0)},
& F\_{\infty,t} = 0 .
\end{cases}
\]

### 32.11.3 一元化观测值的似然函数计算

一元化观测值以后的卡尔曼向前滤波算法产生\(v\_{t,i}\)和\(F\_{t,i}\)，
这时似然函数为

\[\begin{aligned}
L(\boldsymbol Y\_n)
=& p(y\_{1,1}) p(y\_{1,2} | y\_{1,1})
\dots p(y\_{1,p\_1} | y\_{1,1}, \dots, y\_{1,p\_1-1}) \\
& p(p\_{2,1} | y\_{1,1}, \dots, y\_{1,p\_1})
\dots p(y\_{n,n\_p} | \boldsymbol Y\_{n-1}, y\_{n,1}, \dots, y\_{n,n\_p-1}) \\
=& p(v\_{1,1}) p(v\_{1,2}) \dots p(v\_{1,p\_1})
p(v\_{2,1}) \dots p(v\_{n,n\_p}),
\end{aligned}\]
于是
\[\begin{aligned}
\log L(\boldsymbol Y\_n)
=& -\frac{1}{2} \sum\_{t=1}^n \sum\_{i=1}^{p\_t}
\iota\_{t,i}
\left[ \log(2\pi) + \log F\_{t,i}
+ v\_{t,i}^2 / F\_{t,i} \right] .
\end{aligned}\]
其中
\[\begin{aligned}
\iota\_{t,i}
=& \begin{cases}
1, & F\_{t,i} > 0, \\
0, & F\_{t,i} = 0 .
\end{cases}
\end{aligned}\]

如果使用发散先验，
对\(t=1,\dots,d\)，
计算\(v\_{t,i}^{(0)}\), \(F\_{\infty,t,i}\), \(F\_{\*,t,i}\)；
对\(t=d+1, \dots, n\)计算\(v\_{t,i}\), \(F\_{t,i}\)，
发散对数似然函数为
\[\begin{aligned}
\log L(\boldsymbol Y\_n)
=& -\frac{1}{2} \log(2\pi)
\sum\_{t=1}^n \sum\_{i=1}^{p\_t} \iota\_{t,i}
-\frac{1}{2} \sum\_{t=1}^d \sum\_{i=1}^{p\_t} w\_{t,i}
\\
&
- \frac{1}{2} \sum\_{t=d+1}^n \sum\_{i=1}^{p\_t} \iota\_{t,i}
\left[ \log F\_{t,i}
+ v\_{t,i}^2 / F\_{t,i} \right] .
\end{aligned}\]
其中
\[\begin{aligned}
\iota\_{t,i}
=& \begin{cases}
1, & 1 \leq t \leq d \text{且} F\_{\*,t,i} > 0,
\text{或} t>d \text{且} F\_{t,i}>0 \\
0, & \text{其它} .
\end{cases}
\\
w\_{t,i}
=& \begin{cases}
\log|F\_{\infty,t,i}|, & F\_{\infty,t,i} > 0, \\
\log|F\_{\*,t,i}|
+ [\boldsymbol v\_{t,i}^{(0)}]^2 / F\_{\*,t,i},
& F\_{\infty,t} = 0 .
\end{cases}
\end{aligned}\]

## 32.12 滤波平滑的R程序

### 32.12.1 初始分布已知情形

设初始分布\(N(\boldsymbol a\_1, P\_1)\)已知，
模型矩阵\(Z\_t, H\_t, T\_t, R\_t, Q\_t\)都不依赖于\(t\)。

#### 32.12.1.1 似然函数计算

```
# 1.1 似然函数计算 ####
## 仅计算对数似然函数
kf_const_logL <- function(
    y,      # 输入观测值向量（长度n），或者$n \times p$矩阵
    Zmat,   # 观测$Z$矩阵，$p \times m$
    Tmat,   # 转移$T$矩阵，$m \times m$
    Hmat,   # 观测误差方差阵，$p \times p$，$p=1$时为标量
    Rmat = NULL,   # 状态方程误差变换矩阵，$m \times r$
    Qmat,   # 状态方程误差方差阵，$r \times r$
    a1,     # $t=1$时状态初始分布均值
    P1      # $t=1$时状态初始分布方差阵
    ){
  if(!is.matrix(y)){
    y <- cbind(y)
  }
  # 观测长度：
  n <- nrow(y)
  # 观测维数：
  p <- ncol(y)
  # 状态维数：
  m <- ncol(Zmat)
  # 状态方程误差维数：
  dim_err_sta <- nrow(Qmat)
  if(is.null(Rmat)) Rmat <- diag(m)
  
  Zt <- Zmat
  Tt <- Tmat
  Ht <- Hmat
  Rt <- Rmat
  Qt <- Qmat
  
  # 单个$v_t$向量
  vt <- numeric(p)
  # 单个$F_t$矩阵，$p \times p$
  Ft <- matrix(0, p, p)
  # 保存$F_t^{-1}$:
  Ftinv <- matrix(0, p, p)
  
  # 单个$a_t$向量
  at <- a1
  # 单个$P_t$矩阵
  Pt <- P1
  
  # 单个$K_t$矩阵，$m \times p$
  Kt <- matrix(0, m, p)
  # 单个$L_t$矩阵，$m \times m$
  Lt <- matrix(0, m, m)
  # t(Z_t)
  Zt_transp <- t(Zt)
  # $R_t Q_t R_t^T$
  RQRt <- Rt %*% Qt %*% t(Rt)
  
  # 进行卡尔曼滤波并累计计算对数似然函数值
  logL <- -0.5*n*p*log(2*pi)
  for(t in 1:n){
    vt[] <- y[t,] - Zt %*% at
    Ft[] <- Zt %*% Pt %*% Zt_transp + Ht
    if(p==1){ # $p=1$时不需要计算逆矩阵
      Ftinv[] <- 1/c(Ft)
      detFt <- c(Ft)
    } else {
      Ftinv[] <- solve(Ft)
      detFt <- det(Ft)
    }
    
    # 这里计算对数似然函数值
    if(p==1){
      logL <- logL - 0.5*(log(detFt) + c(vt)^2*Ftinv)
    } else{
      logL <- logL - 0.5*(
        log(detFt) + c(vt %*% Ftinv %*% vt)  )
    }
    
    # 向前一步预测
    Kt[] <- Tt %*% Pt %*% Zt_transp %*% Ftinv
    Lt[] <- Tt - Kt %*% Zt
    at <- Tt %*% at + Kt %*% vt
    Pt <- Tt %*% Pt %*% t(Lt) + RQRt
  }
  
  return(logL)
}
```

#### 32.12.1.2 滤波平滑计算

```
## 1.2 向前滤波、向后平滑 ####
## 设初始分布$N(\boldsymbol a_1, P_1)$已知，
## 模型矩阵$Z_t, H_t, T_t, R_t, Q_t$都不依赖于$t$。
## 向前滤波得到$\boldsymbol a_t, P_t, \boldsymbol v_t, F_t, K_t, L_t$;
## 向后平滑得到$\boldsymbol r_{t-1}, N_t, \boldsymbol u_t, D_t$,
## 以及$E(\boldsymbol\alpha_t | \boldsymbol y_1, \dots, \boldsymbol y_n)$,
## $\text{Var}(\boldsymbol\alpha_t | \boldsymbol y_1, \dots, \boldsymbol y_n)$,
## 还有观测方程扰动项、系统方程扰动项的平滑分布。
kf_const_fwdbwd <- function(
    y,      # 输入观测值向量（长度n），或者$n \times p$矩阵
    Zmat,   # 观测$Z$矩阵，$p \times m$
    Tmat,   # 转移$T$矩阵，$m \times m$
    Hmat,   # 观测误差方差阵，$p \times p$，$p=1$时为标量
    Rmat = NULL,   # 状态方程误差变换矩阵，$m \times r$
    Qmat,   # 状态方程误差方差阵，$r \times r$
    a1,     # $t=1$时状态初始分布均值
    P1      # $t=1$时状态初始分布方差阵
){
  if(!is.matrix(y)){
    y <- cbind(y)
  }
  # 观测长度：
  n <- nrow(y)
  # 观测维数：
  p <- ncol(y)
  # 状态维数：
  m <- ncol(Zmat)
  # 状态方程误差维数：
  dim_err_sta <- nrow(Qmat)
  if(is.null(Rmat)) Rmat <- diag(m)
  
  Zt <- Zmat
  Tt <- Tmat
  Ht <- Hmat
  Rt <- Rmat
  Qt <- Qmat
  
  # 单个$v_t$向量
  vt <- numeric(p)
  # v[1:n], 保存为$p \times n$矩阵：
  v_arr <- matrix(0, p, n)
  
  # 单个$F_t$矩阵, $p \times p$
  Ft <- matrix(0, p, p)
  # 单个$F_t$逆矩阵, $p \times p$
  Ftinv <- matrix(0, p, p)
  # F[1:n], 保存为$p \times p \times n$的三维数组
  F_arr <- array(0, c(p, p, n))
  # F[1:n]逆矩阵, 保存为$p \times p \times n$的三维数组
  Finv_arr <- array(0, c(p, p, n))
  
  # 单个$a_t$向量
  at <- a1
  # a[1:n]，保存为$m \times n$矩阵：
  a_arr <- matrix(0, m, n)

  # 单个$P_t$矩阵
  Pt <- P1
  # P[1:n], 保存为$m \times m \times n$的三维数组
  P_arr <- array(0, c(m, m, n))

  # 状态滤波的均值向量1:n，保存为$m \times n$矩阵：
  a_filt_arr <- matrix(0, m, n)
  # 状态滤波的协方差阵1:n，保存为$m \times m \times n$三维数组
  P_filt_arr <- array(0, c(m, m, n))
  
  # 单个$K_t$矩阵，$m \times p$
  Kt <- matrix(0, m, p)
  # K[1:n]，保存为$m \times p \times n$的三维数组
  K_arr <- array(0, c(m, p, n))
  
  # 单个$L_t$矩阵，$m \times m$
  Lt <- matrix(0, m, m)
  # L[1:n], 保存为$m \times m \times n$的三维数组
  L_arr <- array(0, c(m, m, n))
  
  # t(Z_t)
  Zt_transp <- t(Zt)
  # $R_t Q_t R_t^T$
  RQRt <- Rt %*% Qt %*% t(Rt)
  
  # 进行向前的一步预报、滤波，并计算似然函数值
  logL <- -0.5*n*p*log(2*pi)
  for(t in 1:n){
    a_arr[,t] <- at
    P_arr[,,t] <- Pt
    
    vt[] <- y[t,] - Zt %*% at
    Ft[] <- Zt %*% Pt %*% Zt_transp + Ht
    v_arr[,t] <- vt
    F_arr[,,t] <- Ft
    if(p == 1){
      Ftinv[] <- 1 / c(Ft)
      detFt <- c(Ft)
    } else {
      Ftinv[] <- solve(Ft)
      detFt <- det(Ft)
    }
    Finv_arr[,,t] <- Ftinv
    
    # 这里计算对数似然函数值
    logL <- logL - 0.5*(
      log(detFt) + c(vt %*% Ftinv %*% vt)  )
    
    # 滤波
    a_filt_arr[,t] <- at + 
      Pt %*% Zt_transp %*% Ftinv %*% vt
    P_filt_arr[,,t] <- Pt - 
      Pt %*% Zt_transp %*% Ftinv %*% Zt %*% Pt
    
    # 向前一步预测
    Kt[] <- Tt %*% Pt %*% Zt_transp %*% Ftinv
    K_arr[,,t] <- Kt
    Lt[] <- Tt - Kt %*% Zt
    L_arr[,,t] <- Lt
    at <- Tt %*% at + Kt %*% vt
    Pt <- Tt %*% Pt %*% t(Lt) + RQRt
  }
  
  # 向后平滑所需存储结构：
  # 单个$r_t$，$m \times 1$
  rt <- numeric(m) # 保存$r_{t-1}$
  rtp1 <- rt # 保存$r_t$
  # r[0:(n-1)], 保存为$m \times n$矩阵
  # 第三下标表示从0开始的时刻
  r_arr <- matrix(0, m, n)
  # 单个$N_t$矩阵, $m \times m$
  Nt <- matrix(0, m, m)  # 保存$N_{t-1}$
  Ntp1 <- Nt # 保存$N_t$
  # N[0:(n-1)]
  # 第三下标表示从0开始的时刻
  N_arr <- array(0, c(m, m, n))
  
  # 状态平滑均值1:n
  a_sm_arr <- matrix(0, m, n)
  # 状态平滑方差阵1:n
  P_sm_arr <- array(0, c(m, m, n))
  
  # 观测方程扰动项平滑均值
  err_obs_mu_arr <- matrix(0, p, n)
  # 观测方程扰动项平滑方差阵
  err_obs_P_arr <- array(0, c(p, p, n))
  # 系统方程扰动项平滑均值
  err_sys_mu_arr <- matrix(0, dim_err_sta, n)
  # 系统方程扰动项平滑方差阵
  err_sys_P_arr <- array(
    0, c(dim_err_sta, dim_err_sta, n))
  
  # 向后平滑迭代
  rt[] <- 0
  Nt[] <- 0
  QtRtt <- Qt %*% t(Rt)
  for(t in n:1){
    rtp1[] <- rt 
    # $r_{t-1}$:
    rt[] <- Zt_transp %*% Finv_arr[,,t] %*% v_arr[,t] +
      t(L_arr[,,t]) %*% rt
    r_arr[,t] <- rt # 保存$r_{t-1}$
    
    Ntp1[] <- Nt 
    # $N_{t-1}$:
    Nt[] <- Zt_transp %*% Finv_arr[,,t] %*% Zt +
      t(L_arr[,,t]) %*% Nt %*% L_arr[,,t]
    N_arr[,,t] <- Nt # 保存$N_{t-1}$
    
    a_sm_arr[,t] <- a_arr[,t] + P_arr[,,t] %*% rt
    P_sm_arr[,,t] <- P_arr[,,t] - 
      P_arr[,,t] %*% Nt %*% P_arr[,,t] 
    
    err_obs_mu_arr[,t] <-
      Ht %*% (
        Finv_arr[,,t] %*% v_arr[,t] -
          t(K_arr[,,t]) %*% rtp1)
    err_obs_P_arr[,,t] <-
      Ht - Ht %*% (
        Finv_arr[,,t] + 
          t(K_arr[,,t]) %*% Ntp1 %*% K_arr[,,t]) %*% Ht
    
    err_sys_mu_arr[,t] <-
      QtRtt %*% rtp1
    err_sys_P_arr[,,t] <-
      Qt - QtRtt %*% Ntp1 %*% t(QtRtt)
  }
  
  res <- list(
    logLik = logL,
    a = a_arr, 
    P = P_arr,
    a_filt = a_filt_arr,
    P_filt = P_filt_arr,
    a_sm = a_sm_arr,
    P_sm = P_sm_arr,
    err_obs_mu = err_obs_mu_arr,
    err_obs_P = err_obs_P_arr,
    err_sys_mu = err_sys_mu_arr,
    err_sys_P = err_sys_P_arr
    )
  return(res)
}
```

### 32.12.2 各矩阵时变、初始分布已知、观测一元化情形

计算似然函数的程序。
需要将各个矩阵和观测值转换到一个列表中。

```
# 将输入转换成需要的统一格式，每个矩阵都是关于$t$的列表。
# 一元化要求$H_t$必须都是对角阵
# 输入y: 若$p=1$，输入观测值向量（长度n），
#   若$p>1$且固定，输入$n \times p$矩阵或数据框
#   若$p_t$随时间变化，输入为长度$n$的列表
kf_uni_input <- function(
    input = list(), # 用来保存已有转化结果
    y,      # 观测值
    Zmat,   # 观测$Z$矩阵，$p \times m$，可以是矩阵的列表
    Tmat,   # 转移$T$矩阵，$m \times m$，可以是矩阵的列表
    Hmat,   # 观测误差方差阵，$p \times p$，$p=1$时为标量，可以是矩阵的列表
    Rmat = NULL,   # 状态方程误差变换矩阵，$m \times r$，可以是矩阵的列表
    Qmat,   # 状态方程误差方差阵，$r \times r$，可以是矩阵的列表
    a1,     # $t=1$时状态初始分布均值
    P1      # $t=1$时状态初始分布方差阵
){
  
  # 将$Z_t$等矩阵转换为每个时间点一个矩阵的列表：
  make_mat_list <- function(Mat){
    # 以Mat保存$Z_t$为例
    if(is.matrix(Mat)){
      # Z_t 不随时间变化的情形
      Lis <- replicate(n, Mat, simplify=FALSE)
    } else if(is.array(Mat) && length(dim(Mat))==3) {
      # 保存为三维数组，Mat[,,t]为$Z_t$
      Lis <- apply(Mat, 3, rbind, simplify=FALSE)
    } else if(is.list(Mat)){
      # 已经是要求的格式
      Lis <- Mat
    } else {
      stop("Mat必须为矩阵、三维数组或者列表")
    }
    
    return(Lis)
  }
  
  if(!missing(y)){
    if(is.data.frame(y)) {
      # 适用于多个分量且维数固定情形
      y <- as.matrix(y)
    }
    
    if(is.matrix(y)) {
      # 适用于分量个数固定情形
      input$y <- apply(y, 1, c, simplify=FALSE)
    } else if(is.list(y)) {
      # 已经是列表形式
      input$y <- y
    } else {
      # 输入为长度n向量
      input$y <- as.list(y)
    }
    # 现在input$y是列表，y[[t]]保存了$t$时刻的各个分量为R向量形式
    input$n <- length(y)
    
    # 每个时刻的观测分量个数：
    input$pt <- vapply(y, length, 1L)
  }
  
  n <- input$n
  
  # $Z_t$矩阵：每个时刻保存为一个列表
  if(!missing(Zmat)){
    input$Z <- make_mat_list(Zmat)
    # 现在input$Z是列表，Z[[t]]为$Z_t$矩阵
  }
  
  # $T_t$矩阵：每个时刻保存为一个列表
  if(!missing(Tmat)){
    input$T <- make_mat_list(Tmat)
    # 现在input$T是列表，T[[t]]为$Z_t$矩阵
    input$m <- nrow(input$T[[1]])
  }
  
  # $H_t$矩阵：每个时刻保存为一个列表
  if(!missing(Hmat)){
    Ht_lis <- make_mat_list(Hmat)
    # 检查每个$H_t$必须都是对角阵
    if(!all(vapply(Ht_lis, Matrix::isDiagonal, TRUE))){
      stop("所有时刻的Hmat必须都是对角阵")
    }
    # H[[t]]保存$H_t$的对角元素
    input$H <- lapply(Ht_lis, diag)
  }
  
  # $R_t$，系统方程误差为$R_t \eta_t$
  if(is.null(input$R) || !missing(Rmat)){
    if(missing(Rmat)){
      Rmat <- diag(input$m)
    }
    input$R <- make_mat_list(Rmat)
    # 现在input$R是列表，R[[t]]为$R_t$
  } 
  
  # $Q_t$，系统扰动$\eta_t$的方差阵
  if(!missing(Qmat)){
    input$Q <- make_mat_list(Qmat)
    # 系统扰动$\eta$的维数，Durbin & Koopman(2012)中$r$
    input$dim_err_sta <- nrow(input$Q[[1]])
  }
  
  if(!missing(a1)){
    input$a1 <- a1
  }
  
  if(!missing(P1)){
    input$P1 <- P1
  }
  
  miss <- setdiff(
    c("y", "Z", "T", "H", "R", "Q", "a1", "P1"),
    names(input))
  if(length(miss) > 0){
    stop(paste("kf_uni_input: missing", miss))
  }
    
  return(input)
}

# 已知初始分布，所有输入已经列表化，
# 用一元化方法向前递推计算对数似然函数值
# x: 其中包含y, Z, T, H, R, Q, a1, P1等元素，
#    每个元素是关于时间$t$的列表。
kf_uni_logL <- function(x){
  n <- x$n
  m <- x$m
  pt <- x$pt
  
  # 判断$F_{t,i}=0$的标准
  Fti_tol <- 1E-8
  
  ati <- x$a1
  Pti <- x$P1
  Zti <- x$Z[[1]][1,]
  Kti <- Zti[]
  logL <- 0
  num_innov <- 0 # 累计$F_{t,i}>0$个数
  for(t in 1:n){
    # 当前ati的值是$\boldsymbol a_t$,
    # Pti的值是$P_t$
    for(i in 1:pt[t]){
      # 同一时间点的更新
      Zti[] <- x$Z[[t]][i,]
      vti <- x$y[[t]][i] - sum(Zti * ati)
      Fti <- c(Zti %*% Pti %*% Zti) + x$H[[t]][i]
      if(abs(Fti) > Fti_tol){
        num_innov <- num_innov + 1
        logL <- logL - 0.5 * (
          log(Fti) + vti^2 / Fti )
        
        Kti[] <- (Pti %*% Zti) / Fti
        ati[] <- ati + Kti * vti
        Pti[] <- Pti - outer(Kti, Kti)*Fti
      } # else $y_{t,i}$不包含新的信息，不用更新
    }
    # 当前ati和和Pti的值代表滤波分布
    # $p(\boldsymbol\alpha_t | \boldsymbol Y_t)$的参数
    
    # 从t到t+1的更新
    ati[] <- x$T[[t]] %*% ati
    Pti[] <- x$T[[t]] %*% Pti %*% t(x$T[[t]]) + 
      x$R[[t]] %*% x$Q[[t]] %*% t(x$R[[t]])
    # 当前ati和和Pti的值代表预报分布
    # $p(\boldsymbol\alpha_{t+1} | \boldsymbol Y_t)$的参数
  }
  logL <- logL - 0.5*log(2*pi)*num_innov
  
  return(logL)
}
```

### 32.12.3 各矩阵时变、初始分布发散、观测一元化情形

计算似然函数的程序。
需要将各个矩阵和观测值转换到一个统一的列表中保存。

```
# 将输入转换成需要的统一格式
# 要求$H_t$必须都是对角阵
# 输入:
#  y: 若$p=1$，输入观测值向量（长度n），
#     若$p>1$且固定，输入$n \times p$矩阵或数据框
#     若$p_t$随时间变化，输入为长度$n$的列表
#  P1inf: 输入时表示发散先验部分$P_{\infty}$，不输入表示没有发散先验
#  P1: 没有P1inf时就是$\boldsymbol\alpha_1$方差阵，否则是$P_{\star}$
kf_uni_input <- function(
    input = list(), # 用来保存已有转化结果
    y,      # 观测值
    Zmat,   # 观测$Z$矩阵，$p \times m$
    Tmat,   # 转移$T$矩阵，$m \times m$
    Hmat,   # 观测误差方差阵，$p \times p$，$p=1$时为标量
    Rmat = NULL,   # 状态方程误差变换矩阵，$m \times r$
    Qmat,   # 状态方程误差方差阵，$r \times r$
    a1,     # $t=1$时状态初始分布均值
    P1,     # $t=1$时状态初始分布方差阵，非发散部分
    P1inf   # 初始分布方差阵发散部分
){
  
  # 将$Z_t$等矩阵转换为列表的函数
  make_mat_list <- function(Mat){
    # 以Mat保存$Z_t$为例
    if(is.matrix(Mat)){
      # Z_t 不随时间变化的情形
      Lis <- replicate(n, Mat, simplify=FALSE)
    } else if(is.array(Mat) && length(dim(Mat))==3) {
      # 保存为三维数组，Mat[,,t]为$Z_t$
      Lis <- apply(Mat, 3, rbind, simplify=FALSE)
    } else if(is.list(Mat)){
      # 已经是要求的格式
      Lis <- Mat
    } else {
      stop("Mat必须为矩阵、三维数组或者列表")
    }
    
    return(Lis)
  }
  
  if(!missing(y)){
    if(is.data.frame(y)) {
      # 适用于多个分量且维数固定情形
      input$y <- as.matrix(y)
    }
    
    if(is.matrix(y)) {
      # 适用于分量个数固定情形
      input$y <- apply(y, 1, c, simplify=FALSE)
    } else if(is.list(y)) {
      # 已经是列表形式
      input$y <- y
    } else {
      # 输入为长度n向量
      input$y <- as.list(y)
    }
    # 现在input$y是列表，y[[t]]保存了$t$时刻的各个分量为R向量形式
    input$n <- length(y)
    
    # 每个时刻的观测分量个数：
    input$pt <- vapply(y, length, 1L)
  }
  
  n <- input$n
  
  # $Z_t$矩阵：每个时刻保存为一个列表
  if(!missing(Zmat)){
    input$Z <- make_mat_list(Zmat)
    # 现在input$Z是列表，Z[[t]]为$Z_t$矩阵
  }
  
  # $T_t$矩阵：每个时刻保存为一个列表
  if(!missing(Tmat)){
    input$T <- make_mat_list(Tmat)
    # 现在input$T是列表，T[[t]]为$Z_t$矩阵
    input$m <- nrow(input$T[[1]])
  }
  
  # $H_t$矩阵：每个时刻保存为一个列表
  if(!missing(Hmat)){
    Ht_lis <- make_mat_list(Hmat)
    # 检查每个$H_t$必须都是对角阵
    if(!all(vapply(Ht_lis, Matrix::isDiagonal, TRUE))){
      stop("所有时刻的Hmat必须都是对角阵")
    }
    # H[[t]]保存$H_t$的对角元素
    input$H <- lapply(Ht_lis, diag)
  }
  
  # $R_t$，系统方程误差为$R_t \eta_t$
  if(is.null(input$R) || !missing(Rmat)){
    if(missing(Rmat)){
      Rmat <- diag(input$m)
    }
    input$R <- make_mat_list(Rmat)
    # 现在input$R是列表，R[[t]]为$R_t$
  } 
  
  # $Q_t$，系统扰动$\eta_t$的方差阵
  if(!missing(Qmat)){
    input$Q <- make_mat_list(Qmat)
    # 系统扰动$\eta$的维数，Durbin & Koopman(2012)中$r$
    input$dim_err_sta <- nrow(input$Q[[1]])
  }
  
  if(!missing(a1)){
    input$a1 <- a1
  }
  
  if(!missing(P1)){
    input$P1 <- P1
  }
  
  if(!missing(P1inf)){
    input$P1inf <- P1inf
  }
  
  miss <- setdiff(
    c("y", "Z", "T", "H", "R", "Q", "a1", "P1"),
    names(input))
  if(length(miss) > 0){
    stop(paste("kf_uni_input: missing", miss))
  }
  
  return(input)
}

# 已知允许发散先验精确初始化，所有输入已经列表化，
# 用一元化方法向前递推计算发散先验对数似然函数值
# 注意$H_t$矩阵是对角阵
kf_uni_dif_logL <- function(x){
  # 判断$F_{t,i}=0$和$P_{\infty,t,i}=0$的标准
  Fti_tol <- 1E-8
  
  n <- x$n
  m <- x$m
  pt <- x$pt
  
  is_diffuse <- is.null(x$P1inf)
  if(is_diffuse){
    x$P1inf <- matrix(0, m, m)
  }
  
  logL <- 0
  num_innov <- 0 # 累计$F_{t,i}>0$个数
  
  # ati: 发散时保存$a^{(0)}_{t,i}$，以后保存$a_{t,i}$
  ati <- x$a1 
  # Pti: 发散时保存$P_{*,t,i}$，以后保存$P_{t,i}$
  Pti <- x$P1
  # Ptii: 发散时保存$P_{\infty,t,i}$，以后保持为0矩阵
  Ptii <- x$P1inf
  # Ftii: 保存$F_{\infty,t,i}$标量
  # Zti: $Z_t$第$i$行
  Zti <- x$Z[[1]][1,]
  # K0ti: $K_{t,i}^{(0)}$
  K0ti <- Zti[]
  # K1ti: $K_{t,i}^{(1)}$
  K1ti <- Zti[]
  # Mti: $M_{*,t,i}$
  Mti <- Zti[]
  # Mtii: $M_{\infty,t,i}$
  Mtii <- Zti[]
  # L0ti: $L^{(0)}_{t,i}$
  L0ti <- diag(m)
  # L1ti: $L^{(1)}_{t,i}$
  L1ti <- diag(m)
  Tt <- x$T[[1]]
  
  d <- 0 # 记住$P_{\infty,t}$不等于零的$t$的个数
  for(t in 1:n){ # 发散阶段
    if(max(abs(diag(Ptii))) < Fti_tol){
      break
    } else {
      d <- d + 1
    }
    
    # 当前$ati$保存了$\boldsymbol a_t$,
    # $Pti$保存了$P_{*,t}$, $Ptii$保存了$P_{\infty,t}$
    # 即$\boldsymbol\alpha_t$在$\boldsymbol Y_{t-1}$下的条件分布

    for(i in 1:pt[t]){
      # 下面是按照有无穷方差部分向前递推，
      # 同一时间点$t$的多个观测值
      Zti[] <- x$Z[[t]][i,]
      Fti <- c(Zti %*% Pti %*% t(Zti)) + x$H[[t]][i]
      Ftii <- c(Zti %*% Ptii %*% t(Zti))
      if(Fti > Fti_tol) {
        num_innov <- num_innov + 1
      }
      
      if(Ftii > Fti_tol){ # $F_{\infty,t,i} > 0$?
        Mti[] <- Pti %*% Zti
        Mtii[] <- Ptii %*% Zti
        F0ti <- 0
        F1ti <- 1/Ftii
        F2ti <- -Fti / Ftii^2
        K0ti[] <- Mtii * F1ti
        K1ti[] <- Mti * F1ti + Mtii * F2ti
        L0ti[] <- diag(m) - outer(K0ti, Zti)
        L1ti[] <- -outer(K1ti, Zti)
        v0ti <- x$y[[t]][i] - sum(Zti * ati)
        
        ati[] <- ati + K0ti*v0ti
        Pti[] <- Ptii %*% t(L1ti) + Pti %*% t(L0ti)
        Ptii[] <- Ptii %*% t(L0ti)
        
        # 似然
        if(Fti > Fti_tol){ # $F_{*,t,i} > 0$时才有贡献
          logL <- logL - 0.5*log(Ftii)
        }
        
      } else { # $F_{\infty,t,i} = 0$
        Mti[] <- Pti %*% Zti
        K0ti[] <- Mti / Fti
        L0ti[] <- diag(m) - outer(K0ti, Zti)
        v0ti <- x$y[[t]][i] - sum(Zti * ati)
        
        ati[] <- ati + K0ti * v0ti
        Pti[] <- Pti %*% t(L0ti)
        Ptii[] <- Ptii %*% t(L0ti)
        
        # 似然
        if(Fti > Fti_tol){ # $F_{*,t,i} > 0$?
          logL <- logL - 0.5*(log(Fti) + v0ti^2/Fti)
        }
      }
      
    } # for i
    
    # 当前$ati$和$Pti$, $Ptii$保存了$\boldsymbol\alpha_t$
    # 在$\boldsymbol Y_{t}$下的条件分布,
    # 即滤波分布

    # 从$t$到$t+1$步，时间$(t,p_t+1)$到$(t+1,1)$
    # 相当于观测缺失，没有新息，
    # $K_{t,p_t+1}=0$, $L_{t,p_t+1}=T_t$
    # 只有状态转移与扰动
    # 均值只需要状态转移，没有新息可以修正
    ati[] <- Tt %*% ati 
    Pti[] <- Tt %*% Pti %*% t(Tt) + 
      x$R[[t]] %*% x$Q[[t]] %*% t(x$R[[t]])
    Ptii[] <- Tt %*% Ptii %*% t(Tt)
    
    # 当前$ati$保存了$\boldsymbol a_{t+1}$,
    # $Pti$保存了$P_{*,t+1}$, $Ptii$保存了$P_{\infty,t}$，
    # 即$\boldsymbol\alpha_{t+1}$在$\boldsymbol Y_t$下的条件分布
  } # for t, 到t=d为止
  
  Kti <- numeric(m)
  for(t in (d+1):n){
    
    # 当前$ati$保存了$\boldsymbol a_t$,
    # $Pti$保存了$P_{*,t}$, $Ptii$保存了$P_{\infty,t}$
    # 即$\boldsymbol\alpha_t$在$\boldsymbol Y_{t-1}$下的条件分布
    
    for(i in 1:pt[t]){
      # 同一时间点的更新
      Zti[] <- x$Z[[t]][i,]
      vti <- x$y[[t]][i] - sum(Zti * ati)
      Fti <- c(Zti %*% Pti %*% Zti) + x$H[[t]][i]
      if(abs(Fti) > Fti_tol){
        num_innov <- num_innov + 1
        logL <- logL - 0.5 * (
          log(Fti) + vti^2 / Fti )
        
        Kti[] <- (Pti %*% Zti) / Fti
        ati[] <- ati + Kti * vti
        Pti[] <- Pti - outer(Kti, Kti)*Fti
      } # else $y_{t,i}$不包含新的信息，不用更新
    }
    
    # 当前$ati$和$Pti$保存了$\boldsymbol\alpha_t$
    # 在$\boldsymbol Y_{t}$下的条件分布,
    # 即滤波分布
    
    # 从t到t+1的更新, 看成从$(t, p_t+1)$到$(t+1,1)$的时间变化
    # 没有新息，$K_{t,p_t+1}=0$, $v_{t,p_t+1}=0$, $L_{t,p_t+1}=T_t$
    Tt[] <- x$T[[t]]
    ati[] <- Tt %*% ati
    Pti[] <- Tt %*% Pti %*% t(Tt) + 
      x$R[[t]] %*% x$Q[[t]] %*% t(x$R[[t]])
    
    # 当前$ati$保存了$\boldsymbol a_{t+1}$,
    # $Pti$保存了$P_{*,t+1}$, $Ptii$为零矩阵，
    # 即$\boldsymbol\alpha_{t+1}$在$\boldsymbol Y_t$下的条件分布
  }
  logL <- logL - 0.5*log(2*pi)*num_innov
  
  return(logL)
}
```

### B 参考文献

Durbin, James, and Siem Jan Koopman. 2012. *Time Series Analysis by State Space Methods*. 2nd ed. Oxford University Press.