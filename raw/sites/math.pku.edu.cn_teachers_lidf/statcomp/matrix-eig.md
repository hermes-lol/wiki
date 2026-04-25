---
crawl_time: '2026-01-17 14:25:31'
framework: sphinx
title: 31 特征值和奇异值 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 31 特征值和奇异值

## 31.1 特征值和奇异值定义

在多元统计和时间序列分析中会用到特征值和奇异值,
比如，主成分分析、典型相关分析、对应分析、多元自回归模型等。

先简单回顾线性代数中特征值的定义和性质。
设\(A\)为\(n\)阶方阵，
若有非零向量\(\boldsymbol\alpha\)和复数\(\lambda\)使得
\[\begin{align}
A \boldsymbol\alpha = \lambda \boldsymbol\alpha,
\end{align}\]
则称\(\lambda\)是矩阵\(A\)的一个**特征值**，
\(\boldsymbol\alpha\)是特征值\(\lambda\)对应的**特征向量**。
特征向量具有某种不变性：矩阵\(A\)左乘特征向量，
不改变特征向量的方向（没有正反）。

当\(A\)为\(n\)阶实对称阵时，\(A\)恰有\(n\)个实特征值，
记作\(\lambda\_1, \lambda\_2, \dots, \lambda\_n\),
相应的特征向量也都是实向量,
且存在\(n\)阶正交阵\(U\)使得
\[\begin{align}
A = U \Lambda U^T
= \sum\_{j=1}^n \lambda\_j \boldsymbol u\_{\cdot j} \boldsymbol u\_{\cdot j}^T,
\tag{31.1}
\end{align}\]
其中\(\Lambda=\text{diag}(\lambda\_1, \lambda\_2, \dots, \lambda\_n)\)是对角线元素为\(A\)的特征值的对角阵,
\(U\)的第\(j\)列\(\boldsymbol u\_{\cdot j}\)是\(\lambda\_j\)对应的特征向量。

当\(A\)为非负定阵时，所有特征值非负；
当\(A\)为正定阵时，所有特征值都是正数。
\(A\)为非负定阵时，
令\(\Lambda^{1/2}=\text{diag}(\lambda\_1^{1/2}, \lambda\_2^{1/2}, \dots, \lambda\_n^{1/2})\),
令\(A^{1/2} = U \Lambda^{1/2} U^T\)，
则
\[
(A^{1/2})^2 = U \Lambda^{1/2} U^T U \Lambda^{1/2} U^T
= U \Lambda^{1/2} \Lambda^{1/2} U^T = U \Lambda U^T = A .
\]

如果\(A\)是对称阵且\(\lambda\_1, \dots, \lambda\_n\)中仅有\(\lambda\_1, \dots, \lambda\_r\)绝对值较大，
其余特征值接近于零，
则矩阵\(A\)有如下的近似：
\[\begin{align}
A \approx \sum\_{j=1}^r
\lambda\_j \boldsymbol u\_{\cdot j} \boldsymbol u\_{\cdot j}^T,
\tag{31.2}
\end{align}\]

在统计问题如典型相关分析中还会遇到广义的特征值问题:
设\(A, B\)为\(n\)阶方阵，
若有复数\(\lambda\)和非零向量\(\boldsymbol\alpha\)使得
\[\begin{align}
A \boldsymbol\alpha = \lambda B \boldsymbol\alpha,
\tag{31.3}
\end{align}\]
则称\(\lambda\)和\(\boldsymbol\alpha\)分别为矩阵\(A\)相对于矩阵\(B\)的**广义特征值**和**广义特征向量**。
实际问题中，
\(B\)通常是正定阵，\(A\)是实对称阵，
这时[(31.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-gen)等价于\(B^{-1} A \boldsymbol\alpha = \lambda \boldsymbol\alpha\)，
可以化为普通特征值问题，并可利用Cholesky分解进行计算。
设\(B\)有Cholesky分解\(B = L L^T\),
则由\(A \boldsymbol\alpha = \lambda L L^T \boldsymbol\alpha\)得
\(L^{-1} A (L^T)^{-1} (L^T \boldsymbol\alpha) = \lambda (L^T \boldsymbol\alpha)\),
求解普通特征值问题\(\left(L^{-1} A (L^T)^{-1} \right) \boldsymbol\beta = \lambda \boldsymbol\beta\)得\(\lambda\)和\(\boldsymbol\beta\)
再求解\(L^T \boldsymbol\alpha = \boldsymbol\beta\)即可得广义特征值和广义特征向量。

对\(n\)阶非奇异矩阵\(A\), 必存在\(n\)阶正交阵\(U\)和\(V\)，
使得
\[\begin{align}
A = V \text{diag}(d\_1, d\_2, \dots, d\_n) U^T,
\tag{31.4}
\end{align}\]
其中\(d\_1, d\_2, \dots, d\_n\)是正定阵\(A^T A\)的\(n\)个特征值的算数平方根,
称[(31.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-sing)为矩阵\(A\)的**奇异值分解**,
\(d\_1, d\_2, \dots, d\_n\)称为\(A\)的**奇异值**。

若\(A\)是一般的\(n \times m\)非零矩阵，
\(A\)的秩为\(\mbox{rank}(A) = r \leq \min(n,m)\),
\(A^T A\)的非零特征值为\(\lambda\_1 \geq \lambda\_2 \geq \cdots \geq \lambda\_r > 0\),
令\(d\_i = \sqrt{\lambda\_i}, \; i=1,2,\dots,r\),
则称\(d\_i\)为\(A\)的**奇异值**,
且一定有\(m\)阶正交阵\(U\)和\(n\)阶正交阵\(V\)使得
\[\begin{align}
A = V D U^T
= V\_1 D\_1 U\_1^T,
\tag{31.5}
\end{align}\]
其中\(U = (U\_1 | U\_2)\), \(V = (V\_1 | V\_2)\),
\(U\_1\)和\(V\_1\)分别为正交阵\(U\)和\(V\)的前\(r\)列，
\(D\_1 = \text{diag}(d\_1, d\_2, \dots, d\_r)\),
\[\begin{align}
D = \left(\begin{array}{cc}
D\_1 & \boldsymbol 0 \\
\boldsymbol 0 & \boldsymbol 0
\end{array}\right)\_{n\times m},
\tag{31.6}
\end{align}\]
称[(31.5)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-sing2)为\(A\)的**奇异值分解**(singular value decomposition, SVD)。
可以看出
\[\begin{align}
A = \sum\_{i=1}^r d\_i \boldsymbol v\_i \boldsymbol u\_j^T ,
\tag{31.7}
\end{align}\]
\(D\_1 = (\boldsymbol v\_1, \dots, \boldsymbol v\_r)\)，
\(U\_1 = (\boldsymbol u\_1, \dots, \boldsymbol u\_r)\)。
当\(r\)比较小或者奇异值中仅有少数几个较大而其它奇异值都接近于零时，
可以近似\(A\)为
\[\begin{align}
A \approx \sum\_{i=1}^{r'} d\_i \boldsymbol v\_i \boldsymbol u\_j^T .
\tag{31.8}
\end{align}\]
其中\(r'\)远小于\(\max(n,m)\)。

若\(A\)为对称方阵，
则\(A\)的奇异值等于各个非零特征值的绝对值。
设\(A\)为\(n\)阶对称方阵，
非零特征值为\(\lambda\_1, \dots, \lambda\_r\)(\(r \leq n\))，
则\(A\)有特征值分解\(A = U \Lambda U^T\)，
\(\Lambda = \text{diag}(\lambda\_1, \dots, \lambda\_r, 0, \dots, 0)\)，
于是\(A^T A = U \Lambda^2 U^T\)，
\(\Lambda^2 = \text{diag}(\lambda\_1^2, \dots, \lambda\_r^2, 0, \dots, 0)\)，
从而\(A\)的奇异值为\(|\lambda\_1|, \dots, |\lambda\_r|\)。

详见 高惠璇 ([1995](#ref-Gao95)) §5.4 和 Monahan ([2001](#ref-Monahan01)) §6.6。

## 31.2 对称阵特征值分解的Jacobi算法(`*`)

矩阵\(A\)的特征值\(\lambda\)是\(A\)的特征多项式\(A - \lambda I\)的根，
但直接求多项式的根并不容易，
特征值和特征向量的计算一般都通过迭代算法实现。

§[30.3](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#mat-qr-givens)引入的Givens变换是一个旋转变换，
可以仅改变向量中指定的两个元素并使得第二个指定元素变成零。
类似这样仅改变向量中第\(i, j\)两个元素的旋转变换矩阵可以写成
\[\begin{align}
G\_{ij}(\theta) = \left( {\begin{array}{\*{20}c}
1 & {} & {} & {} & {} & {} & {} & {} & {} & {} & {} \\
{} & \ddots & {} & {} & {} & {} & {} & {} & {} & {} & {} \\
{} & {} & 1 & {} & {} & {} & {} & {} & {} & {} & {} \\
{} & {} & {} & \cos\theta & {} & {} & {} & \sin\theta & {} & {} & {} \\
{} & {} & {} & {} & 1 & {} & {} & {} & {} & {} & {} \\
{} & {} & {} & {} & {} & \ddots & {} & {} & {} & {} & {} \\
{} & {} & {} & {} & {} & {} & 1 & {} & {} & {} & {} \\
{} & {} & {} & { - \sin\theta} & {} & {} & {} & \cos\theta & {} & {} & {} \\
{} & {} & {} & {} & {} & {} & {} & {} & 1 & {} & {} \\
{} & {} & {} & {} & {} & {} & {} & {} & {} & \ddots & {} \\
{} & {} & {} & {} & {} & {} & {} & {} & {} & {} & 1 \\
\end{array} } \right).
\end{align}\]
若\(A\)为对称阵，适当选取角度\(\theta\)对\(A\)作如下变换
\[\begin{align}
A^\* = G\_{ij}(\theta) A \left(G\_{ij}(\theta) \right)^T
\tag{31.9}
\end{align}\]
可以使得\(a\_{ij}^\*=a\_{ji}^\*=0\)，
这样的变换叫做**Jacobi**变换。
对\(A\)反复地作Jocobi变换可以使得非对角线元素趋于零。

考虑Jacobi变换[(31.9)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-jacobi)中角度\(\theta\)的确定。
显然，\(A^\*\)和\(A\)的不同仅体现在第\(i, j\)行和第\(i, j\)列，
其它元素保持不变;
\(G\_{ij}(\theta) A\)与\(A\)仅在第\(i, j\)行有差别，
\(A \left(G\_{ij}(\theta) \right)^T\)与\(A\)仅在第\(i, j\)列有差别，
\(A^\*\)与\(G\_{ij}(\theta) A\)仅在第\(i, j\)列有差别。
简单推导可得
\[\begin{align}
a\_{ij}^\* =& \frac12 (a\_{jj} - a\_{ii}) \sin 2\theta + a\_{ij} \cos 2\theta,
\end{align}\]
只要取\(\theta \in (-\frac{\pi}{4}, \frac{\pi}{4})\)使得
\[\begin{align}
\tau = \cot 2\theta = \frac{a\_{ii} - a\_{jj}}{2a\_{ij}}
\tag{31.10}
\end{align}\]
即可使\(a\_{ij}^\* = a\_{ji}^\* = 0\)。

注意到\(G\_{ij}(\theta)\)仅依赖于\(\cos\theta\)和\(\sin\theta\),
设\(x = \tan\theta\)，
由三角函数公式得
\[\begin{align}
x^2 + 2 \tau x - 1 = 0.
\end{align}\]
\(x\)有两个根，为保证\(|\theta| \leq \frac{\pi}{4}\)取其中绝对值较小的一个，
为
\[\begin{align}
x = \tan\theta = \mbox{sgn}(\tau)(-|\tau| + \sqrt{\tau^2 + 1})
= \frac{\mbox{sgn}(\tau)}{|\tau| + \sqrt{\tau^2 + 1}},
\end{align}\]
(其中\(\mbox{sgn}(\cdot)\)表示符号函数，对非负数取1，对负数取-1)从\(x=\tan\theta\)再计算出
\[\begin{align}
\cos\theta = (1 + x^2)^{-\frac12},
\quad
\sin\theta = x \cos\theta.
\tag{31.11}
\end{align}\]
这样求\(\cos\theta\)和\(\sin\theta\)避免了三角函数计算并且\(x\)的计算方法考虑到了避免两个相近数相减造成精度损失的问题。

设\(A\)为实对称阵，
\(J\_{ij}\)是\(A\)关于下标\((i, j)\)的的Jacobi变换阵，
令\(A^\* = J\_{ij} A J\_{ij}^T\)，
则\(A^\*\)有如下性质:
\[\begin{align\*}
\mbox{i) } & A^\* \text{仍为对称阵;} \\
\mbox{ii) } & a\_{ij}^\* = a\_{ji}^\* = 0,
\text{且对} k, t \neq i, j \text{有} a\_{kt}^\* = a\_{kt} \,; \\
\mbox{iii) } & \text{对} t \neq i, j \text{有} (a\_{it}^\*)^2 + (a\_{jt}^\*)^2 = a\_{it}^2 +a\_{jt}^2 \,; \\
\mbox{iv) } & \sum\_i \sum\_j (a\_{ij}^\*)^2 = \sum\_i \sum\_j a\_{ij}^2 \, ; \\
\mbox{v) } & \mbox{off}(A^\*) = \mbox{off}(A) - 2 a\_{ij}^2, \text{ 其中$\mbox{off}(A)$表示$A$中非对角线元素的平方和.}
\end{align\*}\]

Jacobi算法从\(A^{(0)} = A\)和\(U^{(0)} = I\_n\)出发反复作Jacobi变换，
设已有\(A^{(k-1)}\),
则在第\(k\)步求\(A^{(k-1)}\)的非对角元素中绝对值最大者，
设为其\((i\_k, j\_k)\)元素，
用[(31.10)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-2theta)和[(31.11)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-cstheta)针对\(A^{(k-1)}\)和\((i\_k, j\_k)\)求出Jacobi变换矩阵\(J^{(k)}\),
则令\(U^{(k)} = U^{(k-1)} (J^{(k)})^T\),
\(A^{(k)} = J^{(k)} A^{(k-1)} (J^{(k)})^T\),
如此重复直到\(A^{(k)}\)的非对角元素的绝对值最大值小于预定的精度\(\epsilon\)。
这时有\(A = U \Lambda U^T\),
\(U = U^{(k)}\)是正交阵，\(\Lambda\)近似为对角阵。

可以证明上述Jacobi算法的\(A^{(k)}\)收敛到一个对角阵且对角线元素为\(A\)的特征值。
此算法收敛较快，
但每次寻找非对角元素中绝对值最大的一个比较耗时。
改进的Jacobi算法从\(A\)和\(U=I\)出发，
在第\(k\)步时基于上一步的\(A\)计算一个界限
\(\epsilon\_k = \sqrt{\mbox{off}(A^{(k)})} / [n(n-1)]\),
然后对\(A\)的每个严格上三角元素都做一次Jacobi变换，
用变换后的矩阵代替原来的\(A\)并更新矩阵\(U\)，
但是若该严格上三角元素绝对值小于\(\epsilon\_k\)就跳过该元素。
所有严格上三角元素都处理过一遍才进入第\(k+1\)步并计算新的\(\epsilon\_{k+1}\),
重复运算直到\(\epsilon\_{k+1}\)小于预先指定的误差限\(\epsilon\)为止。

在R软件中，
用`eigen()`函数计算特征值和特征向量。

## 31.3 用QR分解方法求对称矩阵特征值分解(`*`)

计算实对称矩阵特征值分解的一种较好的方法是利用Householder变换和Givens变换。
首先，若\(A\)为\(n\)阶实对称矩阵，
可以用\(n-2\)个Householder变换把它变成对称三对角矩阵:
设\(H\_1\)为一个分块对角矩阵，主对角线的第一块为1阶单位阵，
第二块是把\(A\)的第一列最后\(n-1\)个元素中后\(n-2\)个元素变成零的Householder变换阵，
则\(H\_1 A\)第一列为\((a\_{11}, a\_{21}^{(1)}, 0, \dots, 0)^T\),
其中\(a\_{21}^{(1)} = \sqrt{a\_{21}^2 + \dots + a\_{n1}^2}\)，
且\(H\_1 A\)的第一行与\(A\)的第一行完全相同，
于是，
\(H\_1 A H\_1\)的第一行为\((a\_{11}, a\_{21}^{(1)}, 0, \dots, 0)^T\),
注意到\(H\_1\)的对称性与正交性，
\(H\_1 A H\_1\)仍为对称阵，
但是第一列和第一行的最后\(n-2\)个元素已经变成了零。
在第二步，
可以构造一个分块对角矩阵\(H\_2\)，
对角线第一块为\(I\_2\)，第二块是把矩阵\(H\_1 A H\_1\)的第二列中最后\(n-3\)个元素变成零的Householder变换矩阵，
把\(H\_1 A H\_1\)变成\(H\_2 H\_1 A H\_1 H\_2\),
易见第一列和第一行不变，第二列和第二行的最后\(n-3\)个元素变成了零。
如此进行下去得到\(A^{(0)} = H\_{n-2} \cdots H\_1 A H\_1 \cdots H\_{n-2}\)，
使得\(A^{(0)}\)为三对角对称矩阵。

得到三对角对称矩阵\(A^{(0)}\)以后，
进行QR迭代。
设经过\(k-1\)次迭代后得到矩阵\(A^{(k-1)}\)，
在第\(k\)步，
先选一个平移量\(t\_k\)，
对矩阵\(A^{(k-1)} - t\_k I\)用Givens变换方法作QR分解得到
\(A^{(k-1)} - t\_k I = Q\_k R\_k\),
把得到的上三角阵\(R\_k\)右乘\(Q\_k\)再反向平移，
得到
\[\begin{align}
A^{(k)} = R\_k Q\_k + t\_k I = Q\_k^T(A^{(k-1)} - t\_k I)Q\_k + t\_k I = Q\_k^T A^{(k-1)} Q\_k,
\end{align}\]
这样的\(A^{(k)}\)仍是三对角对称矩阵，
如此迭代直到\(A^{(k)}\)变成对角形。
收敛时，
\(A^{(k)}\)的对角线元素为各个特征值，
\(H\_1 \cdots H\_{n-2} Q\_1 \cdots Q\_k\)的各列为相应特征向量。

具体的算法比较复杂，详见([Monahan 2001](#ref-Monahan01)) §6.5,
([Gentle 2007](#ref-Gentle-Matrix07)) §7.4。

## 31.4 奇异值分解的计算(`*`)

设\(A\)为任意非零\(n\times m\)实值矩阵，
先说明\(A\)的奇异值分解的存在性。
以下用\(\| \cdot \|\)表示向量的长度，即\(\| \cdot \|\_2\)。
首先，非负定阵\(A^T A\)和\(A A^T\)有共同的正特征值
\(\lambda\_1 \geq \lambda\_2 \geq \dots \geq \lambda\_r > 0\)且个数\(r\)为矩阵\(A\)的秩。
设\(\boldsymbol u^{(i)}\)是\(A^T A\)的特征值\(\lambda\_i\)对应的单位特征向量（长度为1），
则\(A^T A \boldsymbol u^{(i)} = \lambda\_i \boldsymbol u^{(i)}\),
由此式可得\(\| A \boldsymbol u^{(i)} \|^2 = \lambda\_i\)。
在\(A^T A \boldsymbol u^{(i)} = \lambda\_i \boldsymbol u^{(i)}\)两边左乘矩阵\(A\)得到
\((A A^T) (A \boldsymbol u^{(i)}) = \lambda\_i (A \boldsymbol u^{(i)})\),
即\(A \boldsymbol u^{(i)}\)是矩阵\(A A^T\)的对应于特征值\(\lambda\_i\)的特征向量，
长度为\(d\_i = \sqrt{\lambda\_i}\)。
设\(A A^T\)的所有特征向量组成的正交阵为\(V\),
\(\boldsymbol v^{(j)}\)是\(V\)的第\(j\)列，
适当构造的\(V\)可使得
\[\begin{align}
(\boldsymbol v^{(j)})^T A \boldsymbol u^{(i)} = d\_i \delta\_{i-j}, \ i=1,2,\dots,m, \ j=1,2,\dots,n,
\tag{31.12}
\end{align}\]
其中\(\delta\_{i-j}\)是Kronecker记号，
当\(i=j\)时表示1，
当\(i \neq j\)时表示0,
当\(i > r\)时令\(d\_i=0\)。
把[(31.12)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-sing-p1)式写成矩阵形式即\(V^T A U = D\)，
\(A = V D U^T\)(\(D\)的定义见[(31.6)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#eq:mat-eigen-sing-d))，
说明任何非零矩阵\(A\)均有奇异值分解。

以上的证明给出了求奇异值分解的一种方法：
先选\(A^T A\)和\(A A^T\)中阶数较低一个,
不妨设是\(A^T A\)，
求其特征值分解得到\(A\)的所有奇异值和矩阵\(U\)，
然后利用上面的关系得到\(A A^T\)的对应于非零特征值的特征向量，
如果需要再补充适当列向量组成正交方阵\(V\)即可。
这种方法比较简单，
但是计算\(A^TA\)会造成累积误差。

当\(A\)为\(n \times m\)的列满秩矩阵时，
可以用类似§[31.3](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#mat-eigen-qr)的QR分解方法来求\(A\)的奇异值分解。
方法描述如下。
仿照[正交三角分解](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#mat-eigen-qr)把一个对称矩阵变成三对角矩阵的做法，
我们可以先在\(A\)的左边乘以\(n-2\)个对称正交阵把\(A\)变成上三角形，
然后在此上三角形矩阵右边乘以\(n-1\)个对称正交阵将其变成上双对角阵，
即除对角线和上副对角线外的元素都是零的矩阵。
总之，存在\(n\)阶正交阵\(P\)和\(m\)阶正交阵\(Q\)使得
\[\begin{align}
PAQ = \left(\begin{array}{c}
B\_{m\times m} \\ \boldsymbol 0\_{(n-m)\times m}
\end{array}\right),
\end{align}\]
其中\(B\)的元素满足当\(i>j\)或\(j>i+1\)时\(b\_{ij}=0\),
且\(B\)满秩。

得到上双对角阵\(B\)后，
先求\(B^T B\)的特征值分解。
\(B^T B\)是一个三对角对称阵，
可以用§[31.3](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#mat-eigen-qr)的QR方法求特征值分解。
设\(B^T B = U\_1 D\_m^2 U\_1^T\),
其中\(D\_m = \text{diag}(\sqrt{\lambda\_1}, \dots, \sqrt{\lambda\_m})\),
\(\lambda\_1 \geq \dots \geq \lambda\_m > 0\)为\(B^T B\)的所有特征值，
\(U\_1\)各列为\(B^T B\)的特征向量。
令\(V\_1 = B U\_1 D\_m^{-1}\)，
则\(V\_1^T V\_1 = I\_m\),
于是\(B = V\_1 D\_m U\_1^T\)是\(B\)的奇异值分解。

这时，
\[\begin{align}
PAQ =& \left(\begin{array}{c}
B \\ \boldsymbol 0
\end{array}\right)
= \left(\begin{array}{c}
V\_1 D\_m U\_1^T \\ \boldsymbol 0
\end{array}\right)
= \left(\begin{array}{cc}
V\_1 & \boldsymbol 0 \\
\boldsymbol 0 & I\_{n-m}
\end{array}\right)\_{n \times n}
\left(\begin{array}{c}
D\_m \\ \boldsymbol 0
\end{array}\right)\_{n \times m}
U\_1^T \nonumber\\
\stackrel{\triangle}{=}& V\_2
\left(\begin{array}{c}
D\_m \\ \boldsymbol 0
\end{array}\right)\_{n \times m}
U\_1^T,
\end{align}\]
则\(V\_2\)为\(n\)阶正交阵，
可得\(A\)有奇异值分解
\[\begin{align}
A = (P^T V\_2) D (Q U\_1)^T
\stackrel{\triangle}{=} V D U^T,
\end{align}\]
其中\(V = P^T V\_2\)为\(n\)阶正交阵，
\(U = Q U\_1\)为\(m\)阶正交阵，
\(D\)为\(n \times m\)对角阵，
对角线元素为\(B^T B\)的特征值的算数平方根。

## 习题

#### 习题1

编写关于实对称矩阵\(A\)用Jacobi方法求特征值分解的程序。

#### 习题2

编写关于实对称矩阵\(A\)用改进的Jacobi方法求特征值分解的程序。

#### 习题3

设\(A\)为\(n\)阶实对称方阵，
编写程序用正交相似变换\(B = QAQ\)
把\(A\)变换为三对角对称矩阵\(B\)，
其中\(Q\)为\(n\)阶对称正交阵,
输出\(B\)和\(Q\)的值。

#### 习题4

设\(n\)方阵\(A\)为上双对角矩阵:
当\(j \neq i, i+1\)时总有\(a\_{ij}=0\)。
证明\(B = A^T A\)为三对角矩阵。

#### 习题5

设\(A\)为\(n\)阶非负定对称矩阵，
用拉格朗日乘子法求\(\boldsymbol x^T \boldsymbol x = 1\)条件下
\(\boldsymbol x^T A \boldsymbol x\)的最大值点。

#### 习题6

设\(A\)为\(n\)阶非负定对称矩阵，
用\(A\)的特征值分解求\(\boldsymbol x^T \boldsymbol x = 1\)条件下
\(\boldsymbol x^T A \boldsymbol x\)的最大值点。

### 参考文献

———. 2007. *Matrix Algebra: Theory, Computations, and Applications in Statistics*. Springer.

Monahan, John F. 2001. *Numerical Methods of Statistics*. Cambridge University Press.

高惠璇. 1995. *统计计算*. 北京大学出版社.