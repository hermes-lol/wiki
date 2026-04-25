---
crawl_time: '2026-01-17 14:25:29'
framework: sphinx
title: 33 R中的矩阵计算(*) | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/matrix-rlang.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 33 R中的矩阵计算(`*`)

## 33.1 Base包的矩阵功能

可以用`matrix()`函数生成一个矩阵，如

```
matrix(1:12, nrow=4, ncol=3)
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

`A + B`是矩阵加法，
`A - B`是矩阵减法，
`A %*% B`是矩阵乘法。
`x * A`若`x`是标量，`A`是矩阵，作矩阵数乘。
如果`x`是向量，`A`是矩阵，
则`x %*% A`表示行向量`x`左乘矩阵`A`，
`A %*% x`表示列向量`x`右乘矩阵`A`。

对矩阵\(A, B\)，
`crossprod(A)`计算\(A^T A\),
`crossprod(A, B)`计算\(A^T B\)。
注意，如果`x`是向量，
为了计算`x`的元素平方和，
不应该使用`crossprod`而应写成`sum(x^2)`。
为了计算两个向量`x, y`的内积，
应该写成`sum(x * y)`。

`solve(A)`求方阵`A`的逆矩阵；
`solve(A, x)`解线性方程组\(A x = b\)求\(x\)。
`solve(A, B)`，若`B`也是矩阵，求\(A^{-1}B\)。

R函数`backsolve (R, b)`求解上三角方程组
\(R x = b\)，
`forwardsolve(L, b)`求解下三角方程组\(L x = b\)。

R函数`chol(x)`对正定矩阵作Cholesky分解\(R^T R\)，
结果\(R\)是上三角矩阵。
默认不使用主元，加`pivot=TRUE`选项可以使用主元法，
可以对半正定矩阵分解。

如果对正定矩阵\(A\)做了Cholesky分解\(A = R^T R\)，
可以从\(R\)计算\(A = R^{-1} R^{-T}\)；
如果对矩阵\(X\)得到了QR分解\(X = QR\)，
可以从\(R\)计算\((X^T X)^{-1} = (R^T R)^{-1} = R^{-1} R^{-T}\)。
R函数`chol2inv(x)`计算这两种逆矩阵，
其中`x`是满秩上三角阵。

`qr(A)`计算QR分解\(A = QR\)，
并返回压缩格式的计算结果与辅助信息的列表，
设结果为`z`,
可以用`qr.Q(z)`返回矩阵\(Q\),
`qr.R(z)`返回矩阵\(R\)，
`qr.solve(z, b)`利用得到的QR分解求解线性方程组\(A x = b\)。

在统计计算中QR分解经常用来计算回归分析的最小二乘估计，
或用在Newton-Raphson优化时计算逆矩阵乘以梯度。
设`z`为`qr(X)`的结果，
则`qr.coef(z, y)`利用QR分解求最小二乘估计\((X^T X)^{-1} X^T y\)，
`qr.qy(z, y)`计算\(Q y\),
`qr.qty(z, y)`计算\(Q^T y\)，
`qr.resid(z, y)`计算残差，
`qr.fitted(z, y)`计算拟合值。

`eigen(A)`求方阵\(A\)的特征值和特征向量，
结果返回一个列表，
其中元素`values`为各特征值，
`vectors`是一个方阵，
矩阵的各列为对应的特征向量。
可以加选项`symmetric=TRUE`表示输入的`A`是对称阵（输入复数矩阵时表示是Hermite阵）。
加选项`only.values=TRUE`表示仅要求计算并输出特征值，
不需要输出特征向量。

`svd(A, nu, nv)`求奇异值分解\(A = Q \Lambda V^T\)，
其中\(A\)是\(n \times p\)矩阵，
`nu`表示要计算的左奇异向量个数，
`nv`表示要计算的右奇异向量个数，
默认`nu`, `nv`都是\(A\)的行、列数最小值。
因为计算奇异向量比较慢，
所以不需要所有奇异向量时可以取较小的`nu`和`nv`。
结果是包含`d`, `u`, `v`三个元素的列表，
`d`是长度为`\min(n,p)`的保存奇异值的向量，
`u`是行数为\(n\)、列数为`nu`的左奇异值列向量组成的矩阵，
当`nu=0`时不输出此项；
`v`是行数为\(n\)、列数为`nv`的右奇异值列向量组成的矩阵，
当`nv=0`时不输出此项。

## 33.2 Matrix包的矩阵功能

Matrix扩展包补充了许多base部分没有提供的稠密矩阵功能，
并提供了base部分没有的稀疏矩阵的支持。
Matrix包使用特有的Matrix类型，
此类型又有对角阵、三角阵、分块对角阵、稀疏矩阵等变种，
Matrix包的矩阵类型在做了分解后会将分解结果保存在原始矩阵的变量中，有利用分解结果的重复利用。

`Matrix(x, nr, nc)`生成一个稠密矩阵，
与`matrix()`函数类似。
`cBind()`和`rBind()`与`cbind()`和`rbind()`类似。
`t(A)`返回转置。

`as(A, "sparseMatrix")`返回`A`转换为稀疏矩阵的结果。
系数矩阵的运算结果尽可能保持为稀疏矩阵。

详见Matrix包的文档。

## 33.3 其它包的矩阵功能

`MASS::ginv(X)`求`X`的加号逆(Moore-Penrose广义逆)。