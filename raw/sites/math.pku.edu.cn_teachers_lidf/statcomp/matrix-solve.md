---
crawl_time: '2026-01-17 14:24:57'
framework: sphinx
title: 28 线性方程组求解与LU分解 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 28 线性方程组求解与LU分解

统计计算和其它科学工程计算中很多的问题会转化为线性方程组求解问题。
本节讨论稳定、高效的线性方程组求解方法。

## 28.1 三角形线性方程组求解

我们熟知的解线性方程组的一种方法是消元法，
用线性变化把增广矩阵化为阶梯形然后用回代法求解。
我们先给出系数矩阵为三角形矩阵的线性方程组解法。

设有如下的三角形\(n\)阶线性方程组
\[\begin{aligned}
a\_{11} x\_1 + a\_{12} x\_2 + \dots + a\_{1,n-1} x\_{n-1} + a\_{1n} x\_n =& \ b\_1 \\
\phantom{a\_{21} x\_1 + }
a\_{22} x\_2 + \dots + a\_{2,n-1} x\_{n-1} + a\_{2n} x\_n =& \ b\_2 \\
\ddots \phantom{a\_{n,n-1} x\_{n-1} + a\_{nn} x\_n} \phantom{=}& \ \vdots \\
\phantom{a\_{n-1,1} x\_1 + a\_{n-1,2} x\_2 + \dots +}
a\_{n-1,n-1} x\_{n-1} + a\_{n-1,n} x\_n =& \ b\_{n-1} \\
\phantom{a\_{n1} x\_1 + a\_{n2} x\_2 + \dots + a\_{n,n} x\_n + } a\_{nn} x\_n =& \ b\_n
\end{aligned}\]
设其系数矩阵满秩（当且仅当\(a\_{11} a\_{22} \dots a\_{nn} \neq 0\)），
则求解过程可以写成：
\[\begin{aligned}
x\_{n} =& b\_n / a\_{nn} \\
x\_{n-1} =& \left( b\_{n-1} - a\_{n-1,n} x\_n \right) / a\_{n-1,n-1} \\
\vdots & \\
x\_2 =& \left( b\_2 - a\_{23} x\_3 - \dots - a\_{2n} x\_n \right) / a\_{22} \\
x\_1 =& \left( b\_1 - a\_{12} x\_2 - \dots - a\_{1,n} x\_n \right) / a\_{11}
\end{aligned}\]
这种求解过程叫做**回代法**。
需要的话可以把解出的\(\boldsymbol x\)元素保存在\(\boldsymbol b\)原来的存储空间中。

如果系数矩阵是下三角的，
也可以类似求解。

算法在求\(x\_k\)时，
用到\(n-k\)次乘法，\(n-k\)次减法，1次除法，
所以浮点运算次数为
\[
\sum\_{k=1}^n (2(n-k) + 1) = n^2 .
\]

即回代法的运算次数为\(O(n^2)\)。

## 28.2 高斯消元法

高斯消元法是众所周知的线性方程组解法，
配合主元元素选取可以得到稳定的解。

**例28.1** 考虑线性方程组
\[\begin{align}
\left\{
\begin{array}{\*9r}
3 x\_1 & + & x\_2 & + & 2 x\_3 & + & x\_4 &=& 5 \\
6 x\_1 & + & 4 x\_2 & + & 7 x\_3 & + & 11 x\_4 &=& 5 \\
15 x\_1 & + & 11 x\_2 & + & 18 x\_3 & + & 34 x\_4 &=& 6 \\
18 x\_1 & + & 16 x\_2 & + & 25 x\_3 & + & 56 x\_4 &=& -4
\end{array} \right.
\tag{28.1}
\end{align}\]

只要把方程组变成上三角形就可以用§[28.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#mat-lineq-tri)描述的方法求解。
记
\[\begin{aligned}
A = \left(\begin{array}{rrrr}
3 & 1 & 2 & 1 \\
6 & 4 & 7 & 11 \\
15 & 11 & 18 & 34 \\
18 & 16 & 25 & 56
\end{array}\right)
\quad
\boldsymbol b = \left(\begin{array}{c}
5 \\ 5 \\ 6 \\ -4
\end{array}\right)
\quad
\overline{A} = (A \, | \, \boldsymbol b)
\quad
\boldsymbol x = \left(\begin{array}{c}
x\_1 \\ x\_2 \\ x\_3 \\ x\_4
\end{array}\right)
\end{aligned}\]
令\(\boldsymbol m^{(1)}=(0, 2, 5, 6)^T\)，
把第一个方程乘以\(-m^{(1)}\_2=-2\)加到第二个方程上，
可以消去第二个方程中\(x\_1\)的系数。
类似地，
把第一个方程乘以\(-m^{(1)}\_3=-5\)加到第三个方程上，
把第一个方程乘以\(-m^{(1)}\_4=-6\)加到第四个方程上，
可以消去第三、第四方程中\(x\_1\)的系数。
方程组变成
\[\begin{align}
\left\{
\begin{array}{\*9r}
3 x\_1 & + & x\_2 & + & 2 x\_3 & + & x\_4 &=& 5 \\
& & 2 x\_2 & + & 3 x\_3 & + & 9 x\_4 &=& -5 \\
& & 6 x\_2 & + & 8 x\_3 & + & 29 x\_4 &=& -19 \\
& & 10 x\_2 & + & 13 x\_3 & + & 50 x\_4 &=& -34
\end{array} \right.
\tag{28.2}
\end{align}\]
记\(\boldsymbol m^{(2)} = (0, 0, 3, 5)^T\)，
把第二个方程乘以\(-m^{(2)}\_3 = -3\)加到第三个方程上，
可以消去第三个方程中\(x\_2\)的系数；
把第二个方程乘以\(-m^{(2)}\_4 = -5\)加到第四个方程上，
可以消去第四个方程中\(x\_2\)的系数。
方程组变成
\[\begin{align}
\left\{
\begin{array}{\*9r}
3 x\_1 & + & x\_2 & + & 2 x\_3 & + & x\_4 &=& 5 \\
& & 2 x\_2 & + & 3 x\_3 & + & 9 x\_4 &=& -5 \\
& & & & -x\_3 & + & 2 x\_4 &=& -4 \\
& & & & -2 x\_3 & + & 5 x\_4 &=& -9
\end{array} \right.
\tag{28.3}
\end{align}\]
记\(\boldsymbol m^{(3)} = (0, 0, 0, 2)^T\),
把第三个方程乘以\(-m^{(3)}\_4 = -2\)加到第四个方程上，
可以消去第四个方程中\(x\_3\)的系数。
方程组变成
\[\begin{align}
\left\{
\begin{array}{\*9r}
3 x\_1 & + & x\_2 & + & 2 x\_3 & + & x\_4 &=& 5 \\
& & 2 x\_2 & + & 3 x\_3 & + & 9 x\_4 &=& -5 \\
& & & - & x\_3 & + & 2 x\_4 &=& -4 \\
& & & & & & x\_4 &=& -1
\end{array} \right.
\tag{28.4}
\end{align}\]
用回代法可以解出\(\boldsymbol x = (1, -1, 2, -1)^T\)。

## 28.3 LU分解

考虑例[28.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#exm:mat-lineq-mlg1)把系数矩阵变成上三角形的过程。
第\(j\)步用初等变换把系数矩阵第\(j\)列的主对角线下方的元素都变成零。
设\(A^{(0)}=A\)，\(A\)为\(n\times n\)系数矩阵，
第\(j\)步把矩阵\(A^{(j-1)}\)变成矩阵\(A^{(j)}\)，
实际是左乘了一个初等变换矩阵\(M^{(j)}\):
\[\begin{aligned}
A^{(j)} = M^{(j)} A^{(j-1)}, \ j=1,2,\dots,n-1
\end{aligned}\]
其中\(M^{(j)}\)是一个与\(I\_n\)仅在第\(j\)列不同的方阵，
其第\(j\)列为
\[\begin{aligned}
m^{(j)}\_{ij} = \begin{cases}
0, & \text{当} i < j \\
1, & \text{当} i = j \\
- a^{(j-1)}\_{ij} / a^{(j-1)}\_{jj}, & \text{当} i > j
\end{cases}
\end{aligned}\]
定义\(n\)维向量\(\boldsymbol m^{(j)}\)为
\[\begin{align}
m^{(j)}\_i = \begin{cases}
0, & \text{当} i \leq j \\
a^{(j-1)}\_{ij} / a^{(j-1)}\_{jj}, & \text{当} i > j
\end{cases}
\tag{28.5}
\end{align}\]
则初等变换矩阵\(M^{(j)}\)可以表示为
\[\begin{align}
M^{(j)} = I\_n - \boldsymbol m^{(j)} \boldsymbol e\_j^T
\tag{28.6}
\end{align}\]
经过\(n-1\)步消去变换后，
得到的上三角形系数矩阵为
\[\begin{aligned}
A^{(n-1)} = M^{(n-1)} \dots M^{(2)} M^{(1)} A
\end{aligned}\]

如果每一步中的分母\(a^{(j-1)}\_{jj}\)都不等于零(注意：浮点运算中不等于零的判断是不可靠的），
则上述步骤可以把方程组的系数矩阵变成上三角形,
对线性方程组右边的\(\boldsymbol b\)也作相同的变换就得到三角形线性方程组，
可以用回代法求解。

但是，\(a^{(j-1)}\_{jj}=0\)的情况是存在的，
例如
\[\begin{aligned}
\left(\begin{array}{cc}
0 & 1 \\
1 & 0
\end{array}\right)
\left(\begin{array}{c}
x\_1 \\
x\_2
\end{array}\right)
=
\left(\begin{array}{c}
b\_1 \\
b\_2
\end{array}\right)
\end{aligned}\]
就无法用例[28.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#exm:mat-lineq-mlg1)的方法化为上三角形。

这种情况怎么办？
当第\(j\)步的\(a^{(j-1)}\_{jj}=0\)时，
我们可以看第\(j+1, j+2, \dots, n\)个方程中\(x\_j\)的系数是否非零，
如果第\(s\_j\)个方程的\(x\_j\)系数不等于零，
可以把第\(j\)个方程与第\(s\_j\)个方程互换，
然后继续消元。
把第\(j\)个方程与第\(s\_j\)个方程互换的操作相当于对系数矩阵左乘简单置换阵\(P(j, s\_j)\)。

那么，
是不是第\(j+1, j+2, \dots, n\)个方程中只要\(x\_j\)的系数非零就可以与第\(j\)个方程互换？
要注意的是，
第\(j\)个方程与第\(s\_j\)个方程互换后原来的\(a^{(j-1)}\_{s\_j j}\)就变成了第\(j\)列的第\(j\)行元素，
要作为消去的分母(称为**主元**)；
而数值计算中“非零”的判断是很不可靠的，
应该等于零的值在数值计算中很可能因为舍入误差变成非零。
所以，
在轮到对第\(j\)列消元时，不论\(a\_{jj}^{(j-1)}\)是不是非零值，
都要从系数矩阵第\(j\)列的第\(j, j+1, \dots, n\)行元素中找到绝对值最大的一个，
假设此元素在第\(j\)列的第\(s\_j\)行，
就把第\(s\_j\)行与第\(j\)行互换，
然后进行消去。
这种解方程的方法叫做**列主元的高斯消元法**。

得到三角形矩阵及相应方程组的过程为:
\[\begin{aligned}
A^{(n-1)} =& M^{(n-1)} P(n-1,s\_{n-1}) \dots M^{(2)} P(2, s\_2) M^{(1)} P(1, s\_1) A \\
A^{(n-1)} \boldsymbol x =& M^{(n-1)} P(n-1,s\_{n-1}) \dots M^{(2)} P(2, s\_2) M^{(1)} P(1, s\_1)
\boldsymbol b
\end{aligned}\]
注意，其中的\(M^{(j)}\)是基于行置换后的\(P(j, s\_j) A^{(j-1)}\)计算的。
实际求解时，
通行的算法是把每一步的\(A^{(j)}\)保存在\(A\)的存储空间，
但已经消去变成零的元素不保存，
代之以将\(\boldsymbol m^{(j)}\)的第\(j+1, j+2, \dots, n\)号元素存放在\(A^{(j)}\)第\(j\)列的最后\(n-j\)个元素中
(这\(n-j\)本来被消去变成了零),
解\(\boldsymbol x\)的元素可以存放在\(\boldsymbol b\)的存储空间中。
要注意的是，在第\(j+1, j+2, \dots\)步时的行置换会打乱这样保存的\(\boldsymbol m^{(j)}\)的元素次序。

可以证明，
按照以上所述的步骤，
把原始矩阵\(A\)做了如下LU分解:
\[\begin{align}
P A = L U
\tag{28.7}
\end{align}\]
其中\(P\)是一个置换矩阵:
\[\begin{aligned}
P = P(n-1, s\_{n-1}) \dots P(2, s\_2) P(1, s\_1) ,
\end{aligned}\]
只要保存向量\((s\_1, s\_2, \dots, s\_{n-1})\)就可以恢复矩阵\(P\)。
\(U\)是一个上三角矩阵，
其上三角元素保存在原来输入的\(A\)存储空间的上三角部分，
为列主元法消元最后得到的\(A^{(n-1)}\)矩阵。
\(L\)是一个单位下三角矩阵（主对角线元素都等于1的下三角矩阵），
它严格下三角部分（不包括主对角线在内的下三角部分）的元素保存在原来输入的\(A\)的存储空间的严格下三角部分，
第\(j\)列的最后的\(n-j\)个元素保存了被第\(j+1, j+2, \dots, n-1\)次行置换打乱次序的\(\boldsymbol m^{(j)}\),
记为\(\boldsymbol m\_\*^{(j)}\):
\[\begin{align}
\boldsymbol m\_\*^{(j)} = P(n-1, s\_{n-1}) \dots P(j+1, s\_{j+1}) \boldsymbol m^{(j)},
\tag{28.8}
\end{align}\]
所以\(L\)可以表示为
\[\begin{aligned}
L = I\_n + \boldsymbol m^{(1)}\_\* \boldsymbol e\_1^T + \dots + \boldsymbol m^{(n-1)}\_\* \boldsymbol e\_{n-1}^T.
\end{aligned}\]
公式[(28.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:lu-decomp)称为矩阵\(A\)的三角分解或LU分解。

在得到LU分解[(28.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:lu-decomp)后，
对任何一个常数向量\(\boldsymbol b\)，
要求解\(A \boldsymbol x = \boldsymbol b\)，
可以化为\(P A \boldsymbol x = P \boldsymbol b\)，
注意左乘矩阵\(P\)只是进行了\(n-1\)次行置换。
由\(PA = LU\)得
\(L \, U \, \boldsymbol x = (P \boldsymbol b)\),
令\(\boldsymbol y = U \boldsymbol x\)，
先用回代法解下三角形方程组\(L \boldsymbol y = P \boldsymbol b\)，
再用回代法解上三角形方程组\(U \boldsymbol x = \boldsymbol y\)就可以得到方程组的解。

求解线性方程组\(A \boldsymbol x = \boldsymbol b\)时，
为什么不先求逆矩阵\(A^{-1}\)再求\(\boldsymbol x = A^{-1} \boldsymbol b\)？
仔细分析上述算法的运算次数可以发现，
得到LU分解[(28.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:lu-decomp)只需\(\frac23 n^3 + O(n^2)\)次浮点运算，
额外地再用两遍回代法对一个\(\boldsymbol b\)求解\(\boldsymbol x\)仅需\(2 n^2\)次浮点运算，
但是在得到[(28.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:lu-decomp)后如果要求\(A^{-1}\)则需要额外的
\(\frac23 n^3\)次浮点运算，
即求\(A^{-1}\)需要\(\frac43 n^3 + O(n^2)\)次浮点运算。
直接用消元法求\(A^{-1}\)也需要\(\frac43 n^3 + O(n^2)\)次浮点运算。
得到\(A^{-1}\)后计算\(\boldsymbol x = A^{-1} \boldsymbol b\)需要\(2 n^2\)次浮点运算，
与两遍回代法求\(\boldsymbol x\)计算量相同,
但多出了求逆的\(\frac23 n^3\)次浮点运算。
所以如果仅需要解线性方程组的话不需要计算逆矩阵。

得到LU分解[(28.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:lu-decomp)后可以给出\(A\)的行列式：
\[\begin{aligned}
\det(P) \det(A) = \det(PA) = \det(LU) = \det(U)
\end{aligned}\]
\(\det(U)\)等于\(U\)的主对角线元素乘积，
\(\det(P)\)等于\(1\)或\(-1\)，
当\(P\)代表偶数次行置换时\(\det(P)=1\)，
否则\(\det(P)=-1\)。
注意\(P\)是\(n-1\)个行置换的乘积，
当\(s\_j = j\)时\(P(j, s\_j)=I\_n\)，
\(\det(P(j, s\_j))=1\)；
当\(s\_j \neq j\)时\(\det(P(j, s\_j))=-1\)。
所以，如果向量\((s\_1, s\_2, \dots, s\_{n-1})\)与向量
\((1,2,\dots,n-1)\)不相等的元素个数是偶数，
则\(\det(P)=1\)，否则\(\det(P)=-1\)。

在R语言中，用`solve(A, b)`求解\(A \boldsymbol x = \boldsymbol b\),
用`solve(A)`求\(A\)的逆矩阵，
用Matrix包的`lu`函数求LU分解。

在Julia语言中用`A \ b`求解\(A x = b\)。

## 28.4 Cholesky分解

统计计算中比一般矩阵更常用到的是非负定矩阵和正定矩阵。
比如，随机向量\(\boldsymbol X\)的协方差阵\(\Sigma = \text{Var}(\boldsymbol X)\)是非负定的，
如果满秩，就是正定的。
回归分析中正规方程[(27.2)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-intro.html#eq:linreg-beta-eq)中的叉积阵\(X^T X\)是非负定的，
如果\(X\)列满秩则\(X^T X\)是正定的。
如果方程\(A \boldsymbol x = \boldsymbol b\)的系数矩阵\(A\)是正定矩阵，
虽然还可以用高斯消元法或LU分解来求解，
但这样不能利用\(A\)的特殊结构。
正定矩阵\(A\)可以进行所谓Cholesky分解:
\[\begin{align}
A = L L^T
\tag{28.9}
\end{align}\]
其中\(L\)是一个主对角线元素都取正值的下三角阵。

先承认[(28.9)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol)的存在性,
设法求\(L\)。
设\(L\)的元素为\(l\_{ij}\), \(i \geq j\), 则
\[\begin{aligned}
A = \left(\begin{array}{cccc}
l\_{11} \\
l\_{21} & l\_{22} \\
\vdots & \vdots & \ddots \\
l\_{n1} & l\_{n2} & \cdots & l\_{nn}
\end{array}\right)
\left(\begin{array}{cccc}
l\_{11} & l\_{21} & \cdots & l\_{n1} \\
& l\_{22} & \cdots & l\_{n2} \\
& & \ddots & \vdots \\
& & & l\_{nn}
\end{array}\right)
\end{aligned}\]
显然\(a\_{11} = l\_{11}^2\)。
用\(A^{[k]}\)表示\(A\)的前\(k\)行和前\(k\)列组成的子矩阵，
\(L^{[k]}\)表示\(L\)的前\(k\)行和前\(k\)列组成的子矩阵，
易见\(A^{[k]} = L^{[k]} (L^{[k]})^T\)。
归纳地，
设\(L^{[k-1]}\)已经求得，
要求\(L\)的第\(k\)行,
记\((\boldsymbol l^{[k]})^T = (l\_{k1}, l\_{k2}, \dots, l\_{k,k-1})\),
\(\boldsymbol a^{[k]} = (a\_{1,k}, a\_{2,k}, \dots, a\_{k-1,k})^T\),
由\(A^{[k]} = L^{[k]} (L^{[k]})^T\)有
\[\begin{aligned}
A^{[k]} =& \left(\begin{array}{cc}
A^{[k-1]} & \boldsymbol a^{[k]} \\
(\boldsymbol a^{[k]})^T & a\_{kk}
\end{array}\right) \nonumber\\
=& L^{[k]} (L^{[k]})^T
=
\left(\begin{array}{cc}
L^{[k-1]} & \boldsymbol 0 \\
(\boldsymbol l^{[k]})^T & l\_{kk}
\end{array}\right)
\left(\begin{array}{cc}
(L^{[k-1]})^T & \boldsymbol l^{[k]} \\
\boldsymbol 0 & l\_{kk}
\end{array}\right)
\end{aligned}\]
得方程
\[\begin{align}
L^{[k-1]} \boldsymbol l^{[k]} = \boldsymbol a^{[k]} \tag{28.10}\\
l\_{kk}^2 = a\_{kk} - (\boldsymbol l^{[k]})^T \boldsymbol l^{[k]}
\tag{28.11}
\end{align}\]
只要用回代法解下三角方程组[(28.10)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it1)得到\(\boldsymbol l^{[k]}\),
然后开平方根得到
\(l\_{kk} = \left( a\_{kk} - (\boldsymbol l^{[k]})^T \boldsymbol l^{[k]} \right)^{1/2}\)。

**例28.2** 矩阵
\[\begin{aligned}
A = \left(\begin{array}{\*4c}
4 & 2 & 0 & 2 \\
2 & 10 & 12 & 1 \\
0 & 12 & 17 & 2 \\
2 & 1 & 2 & 9
\end{array}\right)
\end{aligned}\]
是正定阵，来求它的Cholesky分解。

* \(k=1\): \(l\_{11} = \sqrt{a\_{11}} = \sqrt{4} = 2\).
* \(k=2\): 方程\(L^{[1]} \boldsymbol l^{[2]} = \boldsymbol a^{[2]}\)即
  \(l\_{11} l\_{21} = a\_{21}\)(注意\(a\_{12}=a\_{21}\))，
  所以\(2 l\_{21} = 2\), \(l\_{21} = 1\)；
  于是用[(28.11)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it2)得
  \(l\_{22} = \sqrt{a\_{22} - l\_{21}^2} = \sqrt{10 - 1^2} = 3\);
* \(k=3\): 方程\(L^{[2]} \boldsymbol l^{[3]} = \boldsymbol a^{[3]}\) 即

\[\begin{aligned}
\left(\begin{array}{cc}
2 & 0 \\
1 & 3
\end{array}\right)
\left(\begin{array}{c}
l\_{31} \\
l\_{32}
\end{array}\right)
=
\left(\begin{array}{c}
0 \\
12
\end{array}\right)
\end{aligned}\]

解得\(l\_{31}=0\), \(l\_{32}=4\),
再求出
\(l\_{33} = \sqrt{a\_{33} - (l\_{31}^2 + l\_{32}^2)} = \sqrt{17 - (0^2 + 4^2)} = 1\)。

* \(k=4\): 方程\(L^{[3]} \boldsymbol l^{[4]} = \boldsymbol a^{[4]}\)即

\[\begin{aligned}
\left(\begin{array}{ccc}
2 & 0 & 0 \\
1 & 3 & 0 \\
0 & 4 & 1
\end{array}\right)
\left(\begin{array}{c}
l\_{41} \\ l\_{42} \\ l\_{43}
\end{array}\right)
=
\left(\begin{array}{c}
2 \\ 1 \\ 2
\end{array}\right)
\end{aligned}\]
解得\(l\_{41}=1\), \(l\_{42} = 0\), \(l\_{43}=2\),
再开平方根得到
\(l\_{44} = \sqrt{a\_{44} - (l\_{41}^2 + l\_{42}^2 + l\_{43}^2)} = 2\)。

于是，
\[
\begin{aligned}
&L = \left(\begin{array}{\*4c}
2 \\
1 & 3 \\
0 & 4 & 1 \\
1 & 0 & 2 & 2
\end{array}\right),
\quad A = L L^T .
\end{aligned}
\]

※※※※※

当\(A\)为正定阵时，各\(A^{[k]}\)也是正定阵，
取向量
\[\begin{align}
\boldsymbol\alpha = \left(\begin{array}{c}
- (A^{[k-1]})^{-1} \boldsymbol a^{[k]} \\
1 \\
\boldsymbol 0
\end{array}\right)
\tag{28.12}
\end{align}\]
则[(28.11)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it2)的右边是二次型\(\boldsymbol\alpha^T A \boldsymbol\alpha\)，
应该为正值，
所以解方程[(28.10)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it1)–[(28.11)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it2)逐次得到的\(a\_{kk}>0\)，
这样\(L^{[k]}\)是满秩的，
于是下一步的方程[(28.10)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it1)有唯一解，
先计算\(l\_{11}=\sqrt{a\_{11}}\)然后对\(k=2,3,\dots,n\)
重复解[(28.10)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it1)–[(28.11)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it2)
一定每步都可以进行且解是唯一的。
于是，
正定阵\(A\)的Cholesky分解是存在唯一的。

Cholesky分解需要\(\frac13 n^3 + O(n^2)\)次浮点运算和\(n\)次开平方根，
比LU分解的\(\frac23 n^3\)次浮点运算次数少，
开平方根所需的时间只是\(n\)的倍数。

编写Cholesky算法的程序时，
如果输入了一个正定阵\(A\),
因为正定阵是对称的，
可以只输入\(A\)的下三角部分，
返回的Cholesky分解\(L^T L\)的下三角矩阵\(L\)可以保存在输入的矩阵\(A\)的下三角部分的存储空间中。
对于对称矩阵和下三角矩阵也可以只保存其下三角部分的元素，
这样按行排列的话，
第\((i,j)\)元素存放在第\(\frac12 i (i-1) + j\)号位置。

统计中经常需要计算正定矩阵逆矩阵的二次型\(\boldsymbol \alpha^{T} A^{-1} \boldsymbol \alpha\),
就可以用Cholesky分解转化为
\[\begin{aligned}
\boldsymbol \alpha^{T} A^{-1} \boldsymbol \alpha
= \boldsymbol \alpha^{T} (L L^T)^{-1} \boldsymbol \alpha
= (L^{-1} \boldsymbol \alpha)^T L^{-1} \boldsymbol \alpha,
\end{aligned}\]
只要用回代法解\(L \boldsymbol x = \boldsymbol\alpha\),
则有\(\boldsymbol \alpha^{T} A^{-1} \boldsymbol \alpha = \boldsymbol x^T \boldsymbol x\)。
这样计算只需要Cholesky分解的\(\frac13 n^3 + O(n^2)\)次运算，
如果直接计算逆矩阵，
则需要\(\frac43 n^3 + O(n^2)\)次运算。

在R软件中，用`chol()`函数求正定阵的Cholesky分解。

设\(A\)的Cholesky分解\(A = L L^T\)中\(L\)的主对角线元素组成的对角阵为
\(D = \text{diag}(l\_{11}, l\_{22}, \dots, l\_{nn})\),
令\(\tilde L = L D^{-1}\), \(\tilde D = D^2\),
则\(\tilde L\)为单位下三角阵，
\(\tilde D\)是主对角线元素为正值的对角阵，\(A\)有如下分解:
\[\begin{align}
A = L L^T = \tilde L D D \tilde L^T = \tilde L \tilde D \tilde L^T,
\tag{28.13}
\end{align}\]
当\(A\)为对称正定阵时此分解存在唯一，
称为矩阵\(A\)的LDL分解。

## 28.5 线性方程组求解的稳定性(`*`)

设\(A\)为\(n\)阶方阵，则线性方程组\(A \boldsymbol x = \boldsymbol b\)存在唯一解的充分必要条件是\(A\)满秩，
即\(A\)的列向量的任何一个非零线性组合都不等于零向量。
但是，如果存在\(A\)的列向量的一个非零线性组合与零向量十分接近会怎样？
这时，方程求解的误差很大而且不稳定（计算误差和舍入误差的影响很大）。

**例28.3** 方程组
\[\begin{aligned}
\left(\begin{array}{cc}
3 & 1 \\
3.0001 & 1
\end{array}\right)
\left(\begin{array}{c}
x\_1 \\ x\_2
\end{array}\right)
=
\left(\begin{array}{c}
4 \\ 4.0001
\end{array}\right)
\end{aligned}\]
有精确解\((1, 1)^T\)。
考虑扰动对其的影响。

对\(A, \boldsymbol b\)作微小变化：
\[\begin{aligned}
\left(\begin{array}{cc}
3 & 1 \\
2.9999 & 1
\end{array}\right)
\left(\begin{array}{c}
x\_1 \\ x\_2
\end{array}\right)
=
\left(\begin{array}{c}
4 \\ 4.0002
\end{array}\right)
\end{aligned}\]
则精确解变为\((-2, 10)\)。
这里, \(\det(A) = -0.0001\), \(A\)已经很接近于不满秩。

为了考虑线性方程组求解的适定性，
首先回顾一些关于向量和矩阵运算的内容。
设\(\boldsymbol x \in \mathbb R^n\),
定义
\[\begin{aligned}
\| \boldsymbol x \|\_p = \left( \sum\_{i=1}^n |x\_i|^p \right)^{1/p},
\end{aligned}\]
这叫做向量\(\boldsymbol x\)的\(p\)范数。
特别地，\(p=1, 2, +\infty\)时有
\[\begin{aligned}
\| \boldsymbol x \|\_1 =& \sum\_{i=1}^n |x\_i|, \\
\| \boldsymbol x \|\_2 =& \sqrt{ \sum\_{i=1}^n x\_i^2 } = \sqrt{\boldsymbol x^T \boldsymbol x}, \\
\| \boldsymbol x \|\_\infty =& \max\_{1 \leq i \leq n} |x\_i|.
\end{aligned}\]
向量范数作为向量空间中广义的长度，
满足:

* \(\| \boldsymbol x \| \geq 0\), 等号成立当且仅当\(\boldsymbol x = \boldsymbol 0\);
* 对任意\(\lambda \in \mathbb R\)有\(\| \lambda \boldsymbol x \| = |\lambda| \cdot \| \boldsymbol x \|\);
* 有三角不等式\(\| \boldsymbol x + \boldsymbol y \| \leq \| \boldsymbol x \| + \| \boldsymbol y \|\)。

给定矩阵\(A\), \(f(\boldsymbol x) = A \boldsymbol x\)是一个多元线性函数，
或称为一个线性算子。
为了考察\(\boldsymbol x\)的变化引起的\(A \boldsymbol x\)的变化大小，
引入矩阵范数\(\| A \|\)。定义
\[\begin{aligned}
\| A \|\_p = \sup\_{\| \boldsymbol x \|\_p = 1} \| A \boldsymbol x \|,
\end{aligned}\]
称为\(A\)的\(p\)范数。
可以证明，
\[\begin{align}
\| A \|\_1 =& \max\_{1 \leq j \leq n} \sum\_{i=1}^n |a\_{ij}|, \tag{28.14}\\
\| A \|\_2 =& \sqrt{ A^T A\text{的最大特征值}}, \tag{28.15}\\
\| A \|\_\infty =& \max\_{1 \leq i \leq n} \sum\_{j=1}^n |a\_{ij}|. \tag{28.16}
\end{align}\]
定义矩阵\(p\)范数后有
\[\begin{aligned}
\| A \boldsymbol x \|\_p
\leq \| A \|\_p \cdot \| \boldsymbol x \|\_p,
\ \forall \boldsymbol x \in \mathbb R^n,
\end{aligned}\]
进一步
\[\begin{aligned}
\| A B \boldsymbol x \|\_p
\leq \| A \|\_p \cdot \| B \boldsymbol x \|\_p
\leq \|A \|\_p \cdot \| B \|\_p \cdot \| \boldsymbol x \|\_p .
\end{aligned}\]
所以\(\| A B \|\_p \leq \| A \|\_p \cdot \| B \|\_p\)。

一般的矩阵范数定义如下。

**定义28.1 (矩阵范数)** 若对任意\(n\)阶实方阵\(A\)，
都有一个实数\(\| A \|\)与之对应，且满足

1. 非负性: \(\| A \| \geq 0\), 且等号成立当且仅当\(A\)为零矩阵;
2. 齐次性: 对任意\(\lambda \in \mathbb R\), 有\(\| \lambda A \| = |\lambda| \, \| A \|\);
3. 三角不等式: 对任意\(n\)阶实方阵\(A\)和\(B\)，都有\(\| A + B \| \leq \| A \| + \| B \|\);
4. 相容性: 对任意\(n\)阶实方阵\(A, B\), 都有\(\| AB \| \leq \| A \| \, \| B \|\),

则称\(\| A \|\)为\(A\)的矩阵范数。

定义
\[\begin{aligned}
\| A \|\_F = \left( \sum\_{i=1}^n \sum\_{j=1}^n a\_{ij}^2 \right)^{1/2},
\end{aligned}\]
则\(\| A \|\_F\)是\(A\)的一个矩阵范数，叫做Frobenius范数。
\(\| A \|\_F\)实际是将矩阵\(A\)按列拉直成一个列向量计算其欧式模,
所以算法中为了判断两个矩阵\(A, B\)是否近似相当，
只要看\(\| A - B \|\_F\)是否小于给定的误差界限\(\epsilon\)。

定义了矩阵范数就可以用来评估解线性方程组\(A \boldsymbol x = \boldsymbol b\)的适定性，
即输入的\(\boldsymbol b\)的误差导致的解\(\boldsymbol x\)的误差大小。
理论上，当\(A\)满秩时\(\boldsymbol x = A^{-1} \boldsymbol b\)，
设\(\boldsymbol b\)有一个输入误差\(\Delta \boldsymbol b\)，
令
\[\begin{aligned}
A \boldsymbol x = \boldsymbol b, \quad A(\boldsymbol x + \Delta \boldsymbol x) = \boldsymbol b + \Delta \boldsymbol b,
\end{aligned}\]
则\(\boldsymbol b\)输入的误差\(\Delta \boldsymbol b\)造成的解的误差\(\Delta \boldsymbol x\)满足(见 关治 and 陆金甫 ([1998](#ref-Guan-Lu98)) §5.4.2)
\[\begin{align}
\frac{\| \Delta \boldsymbol x \|\_p}{\| \boldsymbol x \|\_p}
\leq \kappa\_p(A) \cdot \frac{\| \Delta \boldsymbol b \|\_p}{\| \boldsymbol b \|\_p},
\tag{28.17}
\end{align}\]
其中\(\kappa\_p(A) = \| A \|\_p \cdot \| A^{-1} \|\_p\)叫做\(A\)的**条件数**。
条件数用来衡量以\(A\)为系数矩阵的线性方程组求解的稳定性，
\(A\)的条件数越大，求逆和解线性方程组越不稳定,
称系数矩阵条件数很大的线性方程组是**病态**的。
对\(p=2\)，\(\kappa\_2(A)\)是\(A^T A\)的最大特征值与最小特征值的比值的算数平方根，
当\(A\)是正定阵时\(\kappa\_2(A)\)是\(A\)
的最大特征值与最小特征值的比值的平方根。

对于病态的线性方程组，
某些变换可以解决一些明显的问题，
比如，每一个方程都乘以一个调整因子使得各行元素大小相近，
\(A\)的每列都乘以一个调整因子使得各列元素大小相近（最后对应的未知数要作反向的调整），
但是没有完全通用的方法解决病态问题。

求解线性方程组的误差还涉及到系数矩阵\(A\)的误差，
更详细的讨论见([关治 and 陆金甫 1998](#ref-Guan-Lu98)) §5.4.2。

## 28.6 附录：一些Julia程序(`*`)

回代法求解并计算浮点运算次数的程序：

```
function solve_tri(A, b)
    local n, x, flops 
    flops = 0
    n = size(A, 2)
    x = zeros(n)
    x[n] = b[n] / A[n,n]
    flops += 1
    for k=n-1:-1:1
        x[k] = (b[k] - sum(A[k,j]*x[j] for j=k+1:n)) / A[k,k]
        flops += n-k + n-k + 1
        # n-k个乘法，n-k-1个加法，1个减法，1个除法
    end

    return x, flops
end
```

测试用的程序：

```
using LinearAlgebra
using Random 

function rand_utri(n)
    x = rand(n,n) .+ 1.0
    [i <= j ? x[i,j] : 0 for i=1:n, j=1:n]
end

function test()
    Random.seed!(101)
    n = 100
    A = rand_utri(n)
    b = rand(n)
    x, flops = solve_tri(A, b)
    bhat = A * x 
    err = norm(b - bhat)

    @show det(A);

    bhatj = A * (A \ b)
    errj = norm(b - bhatj)
    err, errj 
end
test()
## det(A) = 2.8928051016413476e16
## (3.594509928397779e-14, 1.8418266205926928e-14)
```

注意，如果在模拟生成矩阵\(A\)时用了\((0,1)\)内取值的随机数，
则\(A\)的行列式接近于0，
解的误差极大。

## 习题

#### 习题1

对满秩\(n\)阶上三角矩阵\(A\)，编写算法求解线性方程组
\(A \boldsymbol x = \boldsymbol b\),
要求把\(\boldsymbol x\)的解保存在\(\boldsymbol b\)原来的存储空间中。
计算算法需要的乘除法和加减法次数。

#### 习题2

对\(n\)阶单位下三角矩阵\(A\)，
编写算法求解线性方程组
\(A \boldsymbol x = \boldsymbol b\),
要求把\(\boldsymbol x\)的解保存在\(\boldsymbol b\)原来的存储空间中。
计算算法需要的乘除法和加减法次数。

#### 习题3

设\(A\)为上三角矩阵，\(\boldsymbol e\_k=(0,\dots, 0, 1, 0, \dots, 0)^T\)为单位向量，
写算法求解\(A \boldsymbol x = \boldsymbol e\_k\)。
解\(\boldsymbol x\)中那些元素是一定等于零的？
如何简化求解过程？
编写算法求\(A^{-1}\)并把\(A^{-1}\)保存在\(A\)原来的存储空间中。

#### 习题4

证明用列主元法进行矩阵LU分解[(28.7)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:lu-decomp)的算法正确。

1. 对\(M^{(i)} = I\_n - \boldsymbol m^{(i)} \boldsymbol e\_i^T\)矩阵，
   证明当\(k, j>i\)时有
   \[\begin{align\*}
   P(k,j) M^{(i)} = [I\_n - P(k,j) \boldsymbol m^{(i)} \boldsymbol e\_i^T] P(k,j).
   \end{align\*}\]
2. 令\(M\_\*^{(k)} = I\_n - \boldsymbol m\_\*^{(k)} \boldsymbol e\_k^T\)
   (\(\boldsymbol m\_\*^{(k)}\)定义见[(28.8)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-lineq-mstark))，证明
   \[\begin{align\*}
   & M^{(n-1)} P(n-1, s\_{n-1}) \dots M^{(2)} P(2, s\_2) M^{(1)} P(1, s\_1) \\
   =& M\_\*^{(n-1)} \dots M\_\*^{(1)} P(n-1, s\_{n-1}) \dots P(1, s\_1).
   \end{align\*}\]
3. 证明
   \[\begin{align\*}
   (M\_\*^{(k+1)} M\_\*^{(k)})^{-1} = I\_n + \boldsymbol m\_\*^{(k)} \boldsymbol e\_k^T + \boldsymbol m\_\*^{(k+1)} \boldsymbol e\_{k+1}^T.
   \end{align\*}\]
4. 证明\(PA = LU\)成立。

#### 习题4

编写用列主元法作高斯消元同时得到矩阵LU分解的算法程序。

#### 习题5

编写Cholesky分解算法的程序。
对输入的正定阵\(A\)（只有下三角部分需要输入值）,
求Cholesky分解\(A = L L^T\)并把矩阵\(L\)存放在\(A\)的下三角部分中作为函数返回值。
(如果使用R语言的编写一个输入\(A\)输出\(L\)的函数，
由于R语言的设计特点，这样修改的是矩阵\(A\)的一个副本)。

#### 习题6

编写Cholesky分解算法的程序。
设正定阵\(A\)只保存了下三角部分，
元素按行次序保存在一个一维数组中输入，
Cholesky分解\(A = L L^T\)得到的下三角矩阵\(L\)保存到\(A\)所用的存储空间中返回。

#### 习题7

对正定阵\(A\)，证明[(28.12)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-prove-qf)定义的向量与\(A\)组成的二次型
\(\boldsymbol\alpha^T A \boldsymbol\alpha\)等于[(28.11)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-chol-it2)的右边。

#### 习题8

设\(A\)为正定阵，给出计算\(\boldsymbol x^T A^{-1} \boldsymbol y\)
的有效算法。

#### 习题9

考虑线性回归模型\(\boldsymbol y = X \boldsymbol\beta + \boldsymbol\varepsilon\)，
其中\(X\)为\(n \times p\)矩阵(\(n>p\)),
设\(X\)列满秩，
则回归系数\(\boldsymbol\beta\)的最小二乘估计为\(\hat{\boldsymbol\beta} = (X^T X)^{-1} X^T \boldsymbol y\),
残差平方和SSE为\(\boldsymbol y^T \boldsymbol y - \boldsymbol y^T X (X^T X)^{-1} X^T \boldsymbol y\)。
写出利用Cholesky分解方法计算\(\hat{\boldsymbol\beta}\), SSE和\((X^T X)^{-1}\)的算法。

#### 习题10

证明矩阵范数的三个公式[(28.14)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-norm-m1)–[(28.16)](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-solve.html#eq:mat-norm-mi)。

### 参考文献

关治, and 陆金甫. 1998. *数值分析基础*. 高等教育出版社.