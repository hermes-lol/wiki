---
crawl_time: '2026-01-17 14:25:34'
framework: sphinx
title: 30 正交三角分解 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 30 正交三角分解

考虑如下线性模型的估计问题:
\[\begin{align}
\boldsymbol y = X \boldsymbol\beta + \boldsymbol\varepsilon,
\tag{30.1}
\end{align}\]
其中\(X\)是\(n \times p\)已知矩阵(\(n>p\))，
\(\boldsymbol y\)是\(n\)维因变量观测值向量，
\(\boldsymbol\beta\)是\(p\)维未知模型系数向量，
\(\boldsymbol\varepsilon\)是\(n\)维未知的模型随机误差向量。
用最小二乘法估计\(\boldsymbol\beta\)，
即求\(\boldsymbol\beta\)使得
\[\begin{align}
g(\boldsymbol\beta) = \| \boldsymbol y - X \boldsymbol\beta \|^2
\tag{30.2}
\end{align}\]
最小(本节中的向量范数\(\| \boldsymbol x \|\)都表示欧式空间中的长度\(\sqrt{\boldsymbol x^T \boldsymbol x}\))，
归结为求解如下正规方程
\[\begin{align}
X^T X \boldsymbol\beta = X^T \boldsymbol y.
\tag{30.3}
\end{align}\]
此方程一定有解，
当\(X\)列满秩时有唯一解\(\hat{\boldsymbol\beta} = (X^T X)^{-1} X^T \boldsymbol y\)。

考虑正规方程[(30.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-normal-eq)的数值计算问题。
我们需要解出\(\boldsymbol\beta\)(记作\(\hat{\boldsymbol\beta}\)),
并计算残差平方和
\[\begin{align}
\text{SSE} =& \| \boldsymbol y - X \hat{\boldsymbol\beta} \|^2.
\end{align}\]
在\(X\)列满秩时，
正规方程[(30.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-normal-eq)的系数矩阵\(X^T X\)是正定阵，
可以用Cholesky分解方法高效地计算出\(\hat{\boldsymbol\beta}\)和SSE，
并且所需的存储空间也只有\(O(p^2)\)阶。
但是，\(X^T X\)相当于做了平方，
当\(X^T X\)条件数\(\kappa\_2(X^T X)\)很大时，
直接求解正规方程[(30.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-normal-eq)会引入很大误差。
有结果表明（参见 Stewart ([1973](#ref-Stewart-Matrix73)) 定理5.2.4），
\(\hat{\boldsymbol\beta}\)的相对误差有如下控制式:
\[\begin{align}
\frac{ \| \Delta \hat{\boldsymbol\beta} \|}{ \| \hat{\boldsymbol\beta} \|}
\leq& \sqrt{\kappa\_2(X^T X)}
\frac{ \| \Delta \hat{\boldsymbol y} \|}{\| \hat{\boldsymbol y}\| },
\end{align}\]
其中\(\hat{\boldsymbol y} = X \hat{\boldsymbol\beta}\),
\(\Delta \hat{\boldsymbol y}\)是观测值\(\boldsymbol y\)中的误差引起的\(\hat{\boldsymbol y}\)的误差，
\(\Delta \hat{\boldsymbol\beta}\)是观测值\(\boldsymbol y\)中的误差引起的\(\hat{\boldsymbol\beta}\)的误差。
所以，
应该寻找计算更稳定的算法来求解最小二乘问题[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-ls)。
另外，在样本量\(n\)很大时，
计算\(X^T X\)时的累积误差可能导致其实际不正定，
使得Cholesky分解算法失败。
新的算法最好能够在\(X\)非列满秩时也可以求出适当的最小二乘解。
直接利用\(X\)计算而不是对\(X^T X\)计算可以提高稳定性。

注意，在统计数据分析中，
数据中的随机误差和舍入误差往往超过计算中的误差，
但不稳定的算法会放大这些数据中的误差。
一般只要控制计算中造成的误差比随机误差小一个数量级就可以满足要求。

## 30.1 Gram-Schmidt正交化方法

**定义30.1 (QR分解)** 对\(n \times p\)矩阵\(X\)，
若有\(n \times p\)矩阵\(Q\)满足\(Q^T Q = I\_p\)，
以及\(p\)阶上三角矩阵\(R\)使得
\[\begin{align}
X = QR,
\tag{30.4}
\end{align}\]
则称[(30.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-qr)为矩阵\(X\)的QR分解，
或正交—三角分解。

假设最小二乘问题[(30.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-ls)中的\(X\)有QR分解\(X = QR\)且\(R\)满秩。
设\(Q^\* = (Q\,|\,Q\_2)\)是\(n\)阶正交阵，则
\[\begin{align}
\| \boldsymbol y - X \boldsymbol\beta \|^2
=& \| (Q^\*)^T (\boldsymbol y - Q R \boldsymbol\beta) \|^2
= \| Q^T \boldsymbol y - R \boldsymbol\beta \|^2 + \| Q\_2^T \boldsymbol y \|^2,
\tag{30.5}
\end{align}\]
\(\| \boldsymbol y - X \boldsymbol\beta \|^2\)与\(\| Q^T \boldsymbol y - R \boldsymbol\beta \|^2\)有相同的最小值点，
用回代法求解
\[\begin{align}
R \boldsymbol\beta = Q^T \boldsymbol y
\tag{30.6}
\end{align}\]
就可以得到最小二乘估计\(\hat{\boldsymbol\beta}\),
而[(30.5)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-ss-decomp)式右边的\(\| Q\_2^T \boldsymbol y \|^2\)就是残差平方和。
由于\(X^T X = R^T R\)，
\(R^T\)是下三角形矩阵，
所以从QR分解也可以得到Cholesky分解(可能符号有差别)。

[(30.4)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-qr)实际是把\(X\)的各列进行了正交化。
把\(X\)的第一列标准化为\(Q\)的第一列，
把\(X\)的第一、二列正交化和标准化得到\(Q\)的第二列，
以此类推，这正是线性代数中的Gram-Schmidt正交化过程,
算法用伪代码表示如下:

| QR 分解的Gram-Schmidt算法(RGS) |
| --- |
| 令\(r\_{11} \leftarrow \| X\_{\cdot 1} \|\), \(Q\_{\cdot 1} \leftarrow r\_{11}^{-1} X\_{\cdot 1}\). |
| **for**(\(j\) **in** \(2:p\)) { # 求解\(Q\_{\cdot j}\)和\(R\)的第\(j\)列 |
| \(\qquad\) \(Q\_{\cdot j} \leftarrow X\_{\cdot j}\) |
| \(\qquad\) **for**(\(k\) **in** \(1:(j-1)\)){ |
| \(\qquad\qquad\) \(r\_{kj} \leftarrow X\_{\cdot j}^T Q\_{\cdot k}\) |
| \(\qquad\qquad\) \(Q\_{\cdot j} \leftarrow Q\_{\cdot j} - r\_{kj} Q\_{\cdot k}\) |
| \(\qquad\) } |
| \(\qquad\) \(r\_{jj} \leftarrow \| \boldsymbol Q\_{\cdot j} \|\),  \(Q\_{\cdot j} \leftarrow r\_{jj}^{-1} Q\_{\cdot j}\) |
| } # end for \(j\) |
| 输出\(Q\)和\(R\) |

以上的Gram-Schmidt算法(记作RGS)虽然得到了\(X^T X\)的QR分解，
但是该算法得到的\(Q\)矩阵由于计算误差影响可能会正交性不够好，
使得用这样的到\(Q\)矩阵以及[(30.6)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-bs)求解\(\hat{\boldsymbol\beta}\)的条件数与直接求解正规方程[(30.3)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-normal-eq)的条件数相同。
对Gram-Schmidt算法略作修改就可以得到更高精度的分解结果。

修正的Gram-Schmidt算法(记作MGS)仅仅调整了计算的次序，
并把\(Q\)的分解结果保存在了\(X\)的存储空间中。
注意
\[
r\_{ij} = Q\_{\cdot i}^T X\_{\cdot j}
= Q\_{\cdot i}^T \left( X\_{\cdot j} - \sum\_{k=1}^{i-1} b\_{ik} Q\_{\cdot k}\right),
\ j=i+1, \dots, p
\]
其中\(b\_{ik}\)为任意实数。
在MGS的第\(i\)步，
\(Q\)的前\(i-1\)列已经求得并保存在\(X\)的前\(i-1\)列中，
并且\(R\)的前\(i-1\)行作为\(Q\)的前\(i-1\)列与\(X\)的所有列的内积已经求得，
另外，
\(X\)的第\(i\)到第\(p\)列中已经扣除了\(Q\)的前\(i-1\)列的投影。
所以，第\(i\)步只需要求当前保存在\(X\)的第\(i\)列中的向量长度作为\(r\_{ii}\)，
然后将当前保存在\(X\)第\(i\)列中的向量标准化作为\(Q\)的第\(i\)列，
并计算\(Q\_{\cdot i}\)(保存在\(X\_{\cdot i}\)中)与当前\(X\)的第\(i+1, \dots, p\)列的内积，
作为\(r\_{i, i+1}, r\_{i, i+2}, \dots, r\_{i,p}\)的值，
然后从\(X\)的第\(i+1, \dots, p\)列中扣除\(Q\_{\cdot i}\)(保存在\(X\_{\cdot i}\)中)的投影。
所以，算法的第\(i\)步将求得\(Q\)的第\(i\)列(保存在\(X\_{\cdot i}\)中)和\(R\)的第\(i\)行，
并把\(X\)的第\(i+1, \dots, p\)列减去其在\(Q\_{\cdot i}\)(保存在\(X\_{\cdot i}\)中)上的投影。
这样，循环到某后续列\(j'>j\)时，
\(X\)的第\(j'\)列中该减去的投影就都已经减去了，
只需要标准化第\(j'\)列。
算法用伪代码表示如下:

| QR分解的修正GS算法(MGS) |
| --- |
| **for**(\(i\) **in** \(1:p\)) {  # 求解\(Q\_{\cdot i}\)和\(R\)的第\(i\)行 |
| \(\qquad\) \(r\_{ii} \leftarrow \| X\_{\cdot i} \|\), \(X\_{\cdot i} \leftarrow r\_{ii}^{-1} X\_{\cdot i}\) |
| \(\qquad\) **for**(\(j\) **in** \((i+1):p\)){ |
| \(\qquad\qquad\) \(r\_{ij} \leftarrow X\_{\cdot i}^T X\_{\cdot j}\) |
| \(\qquad\qquad\) \(X\_{\cdot j} \leftarrow X\_{\cdot j} - r\_{ij} X\_{\cdot i}\) |
| \(\qquad\) } |
| } # end for \(i\) |
| 输出\(Q=X\)和\(R\) |

上面的伪代码用了R语言的语法，
但是没有特别针对R语言特点进行定制，
所以伪代码不能作为真正的R语言程序。
在R语言中，`for j in (i+1):p`在运行到`i=p`时会出错，
因为这时相当于`for j in (p+1):p`，
在其他语言中这会执行0次循环，
但R语言会执行`j=p+1`和`j=p`两步。

在以上的RGS和MGS算法过程中，
\(r\_{jj}\)是\(X\)的第\(j\)列对\(X\)的第\(1,2,\dots,j-1\)列作线性回归(无截距项)得到的残差平方和的平方根,
所以某一步中如果\(r\_{jj}=0\)说明\(X\)的第\(j\)列与前面的列共线。
在MGS算法过程中，
第\(j\)步以后，
\(X\)的第\(j+1, j+2, \dots, p\)列中保存的是原始的\(X\)的那些列关于
\(Q\)的前\(j\)列回归(无截距项)后的残差,
这时类似于解线性方程组时的主元方法,
可以优先选后续列中向量范数最大的列对应的自变量进入模型。

在\(X\)列满秩时，
RGS和MGS算法得到的上三角阵\(R\)的主对角线元素都为正值，
所以\(X^T X = R^T R\)是\(X^T X\)的Cholesky分解。

如果\(X\)不满秩，
\(\text{rank}(X)=r < \min(n,m)\)，
则存在\(V\_{n \times r}\),
\(U\_{m\times r}\), \(V^T V=I\_r\), \(U^T U=I\_r\)，
使得
\(X = V D U^T\)，
其中\(D\)是\(n\times m\)对角阵，
前\(r\)个主对角线元素为正，其余元素均为零，
这称为矩阵的奇异值分解，
见[特征值和奇异值](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-eig.html#matrix-eig)。

## 30.2 Householder变换(`*`)

Householder变换方法逐次对\(X\)进行正交变换把\(X\)变成上三角形，
这样就得到了\(X\)的QR分解。

设\(\boldsymbol x = (x\_1, x\_2, \dots, x\_m)^T\)是一个\(m\)维向量，
找一个正交变换把\(\boldsymbol x\)最后\(m-1\)个元素都变成零。
取\(U = I\_m - d \boldsymbol u \boldsymbol u^T\),
其中\(d = 2 / \boldsymbol u^T \boldsymbol u\)，
易见对任意的\(m\)维非零向量\(\boldsymbol u\)，\(U\)都是对称的正交方阵(\(U^T U = U U^T = I\_m\)),
且对任意非零常数\(b\)，\(b \boldsymbol u\)和\(\boldsymbol u\)对应相同的\(U\)矩阵。
给定向量\(\boldsymbol x\)后，来求\(\boldsymbol u\)使得\(U \boldsymbol x\)仅有第一个元素非零，
即\(U \boldsymbol x = s \boldsymbol e\_1\)(\(\boldsymbol e\_1\)是仅有第一个元素为1，其它元素都等于0的\(m\)维向量)。

由于正交变换是使得向量长度不变的，
有\(\| \boldsymbol x \|^2 = \| U \boldsymbol x \|^2 = s^2 \| \boldsymbol e\_1 \|^2 = s^2\),
即\(s^2 = \| \boldsymbol x \|^2 = \boldsymbol x^T \boldsymbol x\)。
由\(U \boldsymbol x = s \boldsymbol e\_1\)得
\((d \boldsymbol u^T \boldsymbol x) \boldsymbol u = \boldsymbol x - s \boldsymbol e\_1\),
即\(\boldsymbol u\)是\(\boldsymbol x - s \boldsymbol e\_1\)的数乘结果，
因为\(\boldsymbol u\)和\(\boldsymbol u\)的非零数乘结果对应相同的矩阵\(U\),
所以不妨取\(\boldsymbol u = \boldsymbol x - s \boldsymbol e\_1\)，
其中\(s = \| \boldsymbol x \|\)。
容易验证，
这样选取的\(u\)使得\(U \boldsymbol x = \| \boldsymbol x \| \boldsymbol e\_1\)。
对向量\(\boldsymbol x\)的这种正交变换叫做**Householder变换**。
因为矩阵\(\boldsymbol u \boldsymbol u^T\)秩为1，
这种变换也称为秩1的更新。

可以看出，对任何的\(m\)维向量\(\boldsymbol x\)都可以用Householder变换把最后\(m-1\)个元素都变成零。
对一个\(n \times p\)矩阵\(X\)(\(n > p\)),
可以左乘\(X\_{\cdot 1}\)的Householder变换阵，
把\(X\_{\cdot 1}\)的最后\(n-1\)个元素变成零；
然后对变换后的\(X\),
左乘一个变换矩阵，
使得\(X\)的第1行不变，
而使得\(X\_{\cdot 2}\)的最后\(n-2\)个元素变成零，
即仅对\(X\)的第\(2\)到\(n\)行作\(n-1\)维的Householder变换，
而且第一列中的最后\(n-1\)个零不变。
以此类推，
在第\(j\)次变换时仅对\(X\)的第\(j\)到\(n\)行作\(n-j+1\)维的Householder变换。
于是，变换\(p\)次后，
\(X\)变成了一个上三角矩阵\(R\)
(\(n \times p\)的上三角矩阵是指当\(i>j\)时总有\(r\_{ij}=0\))。

令\(X^{(0)} = X\),
\[\begin{align\*}
U\_j =& \left(\begin{array}{cc}
I\_{j-1} & \boldsymbol 0 \\
\boldsymbol 0 & U^{(j)}
\end{array}\right),
\quad
X^{(j)} = U\_j X^{(j-1)},
\end{align\*}\]
其中\(U\_j\)是\(X^{(j-1)}\_{\cdot j}\)的最后\(n-j+1\)个元素的Householder变换矩阵。
用\(\boldsymbol x^{(j)}\)表示\(X^{(j-1)}\_{\cdot j}\)的最后\(n-j+1\)个元素，
令\(s\_j = \| \boldsymbol x^{(j)} \|\),
\(\boldsymbol u^{(j)} = \boldsymbol x^{(j)} - s\_j \boldsymbol e\_1^{(n-j+1)}\),
\(U^{(j)} = I\_{n-j+1} - \frac{2}{\| \boldsymbol u^{(j)} \|^2} \boldsymbol u^{(j)} (\boldsymbol u^{(j)})^T\)，
其中\(\boldsymbol e\_1^{(n-j+1)}\)表示仅有第一个元素等于1，所有其它元素等于0的\(n+1-j\)维向量。
在\(p\)步变换后得到
\[\begin{align\*}
R^\* = U\_p U\_{p-1} \dots U\_1 X
\end{align\*}\]
是一个\(n \times p\)的上三角阵，
设\(R^\*\)的前\(p\)行组成的矩阵为\(R\),
则\(R\)是\(p\)阶上三角方阵，
而\(R^\*\)的后\(n-p\)行均为零。
令\(U^\* = U\_p U\_{p-1} \cdot U\_1\),
则\(U^\*\)是一个\(n\)阶对称正交阵,
设\(U^\*\)的前\(p\)列组成的\(n \times p\)矩阵为\(U\)，
则
\[\begin{align\*}
X =& U^\* R^\*
= (U \ \ \*)
\left(\begin{array}{c}
R \\ \boldsymbol 0
\end{array}\right)
= U R,
\end{align\*}\]
于是用\(p\)次Householder变换得到了\(X\)的QR分解。
用Householder变换作QR分解来求解最小二乘问题是计算精度较高的方法。

编程实现时，
在\(X\)的存储空间中保存每次得到的\(X^{(j)}\)，
因为第\(j\)列的最后\(n-j\)个元素为零，
第\(j\)个元素为\(s\_j\),
所以可以单独保存\(s\_1, s\_2, \dots, s\_p\),
并把\(\boldsymbol u^{(j)}\)保存在\(X\)的第\(j\)列的最后\(n-j+1\)个元素的存储空间中。

当\(X\)列满秩时，\(R\)的主对角线元素都为正值，
\(X^T X = R^T R\)是\(X^T X\)的Cholesky分解。

如果\(X\)存在共线问题，比如，
\(X\_{\cdot j}\)可以用\(X\)的前\(j-1\)列线性表示，
则Householder变换过程进行了\(j-1\)次以后，
因为\(X\)的前\(j-1\)列的最后\(n-j+1\)行的元素都变成了零，
所以\(X\)的第\(j\)列的最后\(n-j+1\)个元素也都变成了零，
这时\(s\_j = \| \boldsymbol x^{(j)} \| = 0\)。
这样在回归计算中可以判断共线是否发生，
也可以用类似选主元的方法，
在第\(j\)步看后面的\(n-j+1\)列的后\(n-j+1\)行组成的子矩阵中哪一列的向量范数最大，
就把那一列对应的自变量与第\(j\)个自变量交换位置。

## 30.3 Givens变换(`*`)

Givens变换也是一种正交变换，
是二维向量的旋转变换的推广，
可以把向量的指定元素变成零。

对二维非零向量\(\boldsymbol x = (x\_1, x\_2)^T\),
把\(\boldsymbol x\)看成直角坐标平面上的向量，
作旋转变换把新的\(x\)轴旋转到\(\boldsymbol x\)的方向上，
设旋转角度为\(\theta\),
则
\[\begin{aligned}
\cos\theta =& \frac{x\_1}{\sqrt{x\_1^2 + x\_2^2}},
\quad
\sin\theta = \frac{x\_2}{\sqrt{x\_1^2 + x\_2^2}},
\end{aligned}\]
旋转变换矩阵和变换结果为
\[\begin{aligned}
R =& \left(\begin{array}{cc}
\cos\theta & \sin\theta \\
-\sin\theta & \cos\theta
\end{array}\right)
= (x\_1^2 + x\_2^2)^{-\frac12}
\left(\begin{array}{cc}
x\_1 & x\_2 \\
-x\_2 & x\_1
\end{array}\right),
\\
R \boldsymbol x =&
\left(\begin{array}{c}
\sqrt{x\_1^2 + x\_2^2} \\
0
\end{array}\right).
\end{aligned}\]

对\(n\)维向量\(\boldsymbol x\),
为了把\(x\_k\)变成零，
可以对\((x\_i, x\_k)\)(\(i<k\))作旋转使得\(x\_k\)变成零，
保持其它元素不变。
这相当于左乘了一个正交变换矩阵\(U\_{ik}\),
\(U\_{ik}\)与单位阵\(I\_n\)仅在4个元素上有差别:
其\((i,i), (i,k), (k,i), (k,k)\)元素组成的子矩阵恰好为
\((x\_i, x\_k)\)对应的旋转阵
\[\begin{align}
R\_{ik} =&
(x\_i^2 + x\_k^2)^{-\frac12}
\left(\begin{array}{cc}
x\_i & x\_k \\
-x\_k & x\_i
\end{array}\right),
\end{align}\]
则\(U\_{ik} \boldsymbol x\)除去第\(i, k\)号元素以外不变，
第\(i\)号元素变成\(\sqrt{x\_i^2 + x\_k^2}\),
第\(k\)号元素变成\(0\)。

仿照用Householder变换进行QR分解的方法，
利用Givens变换也可以对\(n\times p\)矩阵\(X\)(\(n>p\))作QR分解。
首先，用\(n-1\)次Givens变换可以把\(X\)的第1列的最后\(n-1\)个元素变成零，
每次用该列主对角线元素与其下面的元素构成Givens变换。
然后，用\(n-2\)次Givens变换可以把\(X\)的第2列的最后\(n-2\)个元素变成零，
每次用该列主对角线元素与其下面的一个元素构成Givens变换。
设已经把\(X\)的前\(j-1\)列变成了上三角形(前\(j-1\)列主对角线下方的元素都变成了零),
在第\(j\)步，
用\(n-j\)次Givens变换把主对角线下方的元素都变成零，
每次用该列主对角先元素与其下面的一个元素构成Givens变换，
注意此变换仅影响到第\(j\)行和第\(k\)行(\(k>j\)),
所以不会影响到所有列的前\(j-1\)行元素，
也就不会影响到前\(j-1\)列的上三角部分，
前\(j-1\)列已经变成零的那些元素也保持为零，
第\(j\)列中已经变成零的元素也保持为零。
注意，第\(j\)步关于第\(j\)和第\(k\)元素作Givens变换时，
仅需对当前\(X\)的第\(j, k\)两行和第\(j, j+1, \dots, p\)列组成的两行的子矩阵作二维的Givens变换即可,
并且对第\(j\)列的两个元素的变换结果分别为\(\sqrt{x\_j^2 + x\_k^2}\)和\(0\)，
不用重复计算。
如此进行\(p\)步后就把\(X\)变成了上三角形，
共需要进行
\((n-1) + (n-2) + \dots + (n-p) = np - \frac12 p(p+1)\)次Givens变换。

用Givens变换作QR分解，还可以按照每次消除主对角下方的一行的方式:
首先消除主对角线下方第2行(只有\((2,1)\)号元素),
然后消除主对角线下方第3行(有\((3,1,), (3,2)\)两个元素),
如此重复直到消除了主对角线下方第\(n\)行元素(该行除主对角元素以外的所有元素)。
容易看出，按这种次序变换，
在消去第\(i+1\)行下三角部分的元素时，不会影响前面的第\(1,2,\dots,i\)行中下三角部分已经变成零的元素，
但是会改变第\(1,2,\dots, i\)行的上三角部分(包含对角线)的元素。
在回归计算中采用这种变换次序可以适应不断获得新的观测需要更新回归结果的情况（参见 Monahan ([2001](#ref-Monahan01)) §5.8）。

借助于QR分解可以很容易地计算帽子矩阵\(X(X^T X)^{-1} X^T\)的主对角线元素\(h\_i\)的值，
并由此计算多种回归诊断统计量，比如外学生化残差。
回归的一般线性约束的假设检验也可以借助于正交分解的方法计算。
参见([Monahan 2001](#ref-Monahan01)) §5.9和§5.10。
**消去变换(sweep)**是解线性方程组时完全消元法（把系数矩阵通过初等变换化为单位阵）的变种，
在原来系数矩阵的存储空间中保存得到的逆矩阵，
为回归变量选择的计算提供了很多便利，
参见 ([高惠璇 1995](#ref-Gao95)) §5.6 和 ([Monahan 2001](#ref-Monahan01)) §5.12。

在R软件中，
用`qr()`函数可以计算矩阵的QR分解。

Julia程序示例见 [Julia的Givens变换示例](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-julia.html#matrix-julia-qr-givens) 。

## 习题

#### 习题1

对\(n \times p\)矩阵\(X\)（设\(n>p\)）
编写用Gram-Schmidt正交化方法和修正的Gram-Schmidt方法作QR分解的程序。

#### 习题2

对\(n \times p\)矩阵\(X\)（设\(n>p\)）
编写用Householder变换方法作QR分解的程序。

#### 习题3

对\(n \times p\)矩阵\(X\)（设\(n>p\)）
编写用Givens变换方法作QR分解的程序。

#### 习题4

对线性模型
[(30.1)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-qr.html#eq:mat-qr-lm),
设\(X\)列满秩，
编写用QR分解求\(\hat{\boldsymbol\beta}\)、SSE、
回归残差\(\boldsymbol e = \boldsymbol y - X \boldsymbol \beta\)和\((X^T X)^{-1}\)的R程序。

### 参考文献

Monahan, John F. 2001. *Numerical Methods of Statistics*. Cambridge University Press.

Stewart, G. W. 1973. *Introduction to Matrix Computations*. Academic Press.

高惠璇. 1995. *统计计算*. 北京大学出版社.