---
title: Matrix Computation
updated: 2016-10-18 23:37
tags: [matrix, math]
category: Linear Algebra
---

> 矩阵计算的三大问题：线性方程组求解问题、最小二乘问题、代数特征值问题。

## 特殊矩阵

### - 置换矩阵

每一行和每一列都只有一个1，其余都是0。对矩阵的横行（置换矩阵左乘）或纵列（置换矩阵右乘）进行置换操作。

### - 可对角化矩阵

n阶方阵$$A$$可通过相似变换对角化的充要条件是它具有$$n$$个线性无关的特征向量。此时，存在可逆矩阵$$P$$使得

$$P^{-1}AP=\Sigma,$$

而$$P$$是由$$n$$个特征向量作为纵列构成的矩阵。

### - 正规矩阵

$$A\in \mathbb{C}^{n \times n}$$是正规矩阵，当且仅当（$$*$$表示共轭转置）

$$A^*A=AA^*$$

### - 酉矩阵（正规矩阵的特例）

$$U^* U = UU^* = I$$

当$$U$$为实矩阵时，酉矩阵变成正交矩阵。

### - 幂零矩阵

$$n\times n$$的方块矩阵$$M$$是幂零矩阵，当且仅当存在整数$$q$$使得

$$M^q=0$$.

### - 正定矩阵

> One intuitive definition is as follows. Multiply any vector with a positive definite matrix. The angle between the original vector and the resultant vector will always be less than $$\pi / 2$$. The positive definite matrix tries to keep the vector within a certain half space containing the vector. This is analogous to what a positive number does to a real variable. Multiply it and it only stretches or contracts the number but never reflects it about the origin.


#### 常见的正定矩阵
* 矩阵$$A$$等于一个叉积$$X^TX$$，其中$$X$$是列满秩的。
* $$A$$和$$A^T$$均为对角占优的且每个$$a_{ii}$$都大于零。

#### 性质
* 正定矩阵的对角元素都为正，且最大的元素位于对角线上，相对而言，正定矩阵拥有一条比较重的“对角线”。
* 正定矩阵是可逆的，且其逆矩阵也是正定的。如果$$M\geq N>0$$，则$$N^{-1}\geq M^{-1}>0$$。
* 如果$$M$$和$$N$$是正定阵，那么$$M+N$$、$$MNM$$、$$NMN$$都是正定的，$$NM$$和$$MN$$不一定正定（除非满足$$MN＝NM$$）。
* 如果$$M>0$$为实系数矩阵，那么存在$$\sigma >0$$使得$$M>\sigma I$$


## 分解

### - LU 分解

$$A=LU,$$

其中，$$L$$和$$U$$分别是**单位下三角矩阵**（主对角线系数为1）和**上三角矩阵**。LU分解是高斯消去法的代数描述。

> 数值分析的一大基本原则是：求解任一问题都应利用它的结构特性。LU分解针对矩阵的特性如对称性、定型性、稀疏性也有专用的特殊分解方法。

#### $$LDM^T$$和$$LDL^T$$分解

$$A=LDM^T,$$

其中，$$D$$是**对角矩阵**，$$L$$和$$M$$是**单位下三角矩阵**。如果$$A$$是对称且非奇异的，则有$$L＝M$$。

#### 正定矩阵的$$LDM^T$$分解

若$$A=LDM^T$$是正定矩阵$$A$$的$$LDM^T$$分解，则对角矩阵$$D$$的对角元均大于零。特别地，当$$A\in \mathbb{R}^{n\times n}$$正定且对称时，有**Cholesky分解**：

存在唯一的一个对角元全部大于零的下三角阵$$G\in \mathbb{R}^{n \times n}$$, 满足$$A=GG^T$$。



### - SVD 分解

$$A = U\Sigma V^T,$$

其中，$$A \in \mathbb{R}^{m \times n}$$, $$U \in \mathbb{R}^{m \times m}$$和$$V \in \mathbb{R}^{n \times n}$$是正交矩阵。$$\Sigma = diag(\sigma_1, ..., \sigma_p) \in \mathbb{R}^{m \times n}, p=min\{m, n\}$$，其中$$\sigma_1 \geq \sigma_2 \geq ... \geq \sigma_p > 0$$。

特别地，当$$A$$是**正规矩阵**时，即$$A^*A=AA^*$$，可以被正交对角化为

$$A=U\Sigma U^*,$$

此时，$$U=V$$。实对称矩阵是一种特殊的正规矩阵。

#### 性质
* Frobenius norm: $$\|A\|_F^2 = \sigma_1^2 + ... + \sigma_p^2$$
* 二范数距离$$\|A\|_2 = \sigma_1 （最大的奇异值）$$
* SVD展开：$$A=\sum\limits_{i=1}^r \sigma_i u_i v_i^T, r = rank(A)$$
* $$V$$可以对角化矩阵$$A^TA$$, $$v_i$$是$$A^TA$$的特征向量
* $$U$$可以对角化矩阵$$AA^T$$, $$u_i$$是$$AA^T$$的特征向量

#### 直觉理解
[https://en.wikipedia.org/wiki/Singular_value_decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition)

[https://www.quora.com/What-is-an-intuitive-explanation-of-singular-value-decomposition-SVD](https://www.quora.com/What-is-an-intuitive-explanation-of-singular-value-decomposition-SVD)

考虑矩阵$$A\in \mathbb{R}^{M\times N}(M\geq N)$$, $$A$$的奇异值是$$A^TA$$的平方根。因为$$A^TA$$描述了数据矩阵$$A$$在$$N$$维空间下的covariance（假设已归一化），所以奇异值$$\sigma_i$$描述了在$$v_i$$方向上的covariance强度。在解释实际的例子时，可以说奇异值代表了数据中的某种特征(pattern)的强度，奇异值向量则描述了这种特征。

### - QR 分解

一个$$m\times n$$矩阵A的QR分解为

$$A = QR,$$

其中$$Q \in \mathbb{R}^{m \times m}$$ 是正交阵 (i.e. $$Q^TQ = I$$), $$R\in \mathbb{R}^{m \times n}$$是上三角阵。如果$$Q$$是非奇异的(i.e. 满秩的)，那么QR分解是唯一的。QR分解的一大应用是用来解线性方程组，与高斯消除法(Gaussian Elimination)和LU分解相比，虽然计算成本更高，但是具有**数值稳定(numerical stability)**的优势。例如要求解线性方程组$$y=Ax+b$$，首先对$$A$$进行QR分解：$$A = QR$$。然后方程组可以转化为$$Q^Ty=Rx+Q^Tb$$。因为矩阵$$R$$是上三角矩阵，所以可以非常容易地通过back substitution求出$$x$$。

QR分解的方法有Householder QR分解和Givens QR分解。
#### Householder QR 分解

Householder反射变换常用于消去一个向量中除去第一个分量外的其他分量。例如:

$$Hx = y,$$

这里$$y=[1\ 0\ ...\ 0]^T$$

#### Givens QR 分解

Given旋转变换用于有选择的消除一些元素。

### - Schur 分解

如果$$A \in \mathbb{C}^{n \times n}$$, 那么存在一个酉矩阵$$Q\in \mathbb{C}^{n\times n}$$, 使得

$$Q^HAQ=T=D+N,$$

这里$$D=diag(\lambda_1, ..., \lambda_n)$$, $$N\in \mathbb{C}^{n \times n}$$是严格上三角阵。并且，可以选取$$Q$$使得特征值$$\lambda_i$$沿对角线按任意给定的次序出现。

特别地，$$A$$是正规阵，当且仅当存在一个酉矩阵$$Q\in \mathbb{C}^{n\times n}$$, 使得

$$Q^HAQ=T=D＝diag(\lambda_1, ..., \lambda_n),$$

即，$$N=0$$。 $$A$$是正规阵 $$\Longleftrightarrow A$$酉相似于一个对角阵。

对于一般的$$n \times n$$矩阵$$A$$，$$\|N\|_F$$与$$Q$$的选择无关：

$$\|N\|_F^2 = \|A\|_F^2 - \sum\limits_{i=1}^n |\lambda_i|^2 \equiv \triangle^2(A),$$

这个量称为$$A$$的正规偏离度。正规矩阵的正规偏离度为0。

## 距离的衡量

### - 向量、矩阵的距离
用向量范数、矩阵范数来度量

### - 向量子空间的距离
设$$S_1$$和$$S_2$$是$$\mathbb{R}^n$$中满足$$dim(S_1)=dim(S_2)$$的两个子空间，定义这两个子空间的距离为

$$dist(S_1, S_2)= \|P_1-P_2\|,$$

其中$$P_i$$是向$$S_i$$上的正交投影。如果$$V=[v_1, ..., v_k]$$的列是子空间$$S$$的一组正交基，则$$P=VV^T$$是向$$S$$上的唯一正交投影。


## Matrix Calculus
[http://www.atmos.washington.edu/~dennis/MatrixCalculus.pdf](http://www.atmos.washington.edu/~dennis/MatrixCalculus.pdf)

## Matrix Differentiation

[A Note on Matrix Differentiation](https://mpra.ub.uni-muenchen.de/3917/1/MPRA_paper_3917.pdf)

### Derivatives of matrix determinant, trace and inverse

Given a vector $$\theta \in R^k$$,

$$\begin{split}  \frac{\partial \mathbf{X}}{\partial \theta} &= \[ \frac{\partial \mathbf{X}}{\partial \theta_1}, \frac{\partial \mathbf{X}}{\partial \theta_2}, ..., \frac{\partial \mathbf{X}}{\partial \theta_k} \]  \\ \frac{\partial \mathbf{XY}}{\partial \theta} &= \frac{\partial \mathbf{X}}{\partial \theta} (\mathbf{I}_k \otimes \mathbf{Y}) + \mathbf{X} \frac{\partial \mathbf{Y}}{\partial \theta} \\
\frac{\partial det(\mathbf{X})}{\partial \theta} &= det(\mathbf{X}) \cdot tr_k (\mathbf{X}^{-1} \frac{\partial \mathbf{X}}{\partial \theta}) \\  \frac{\partial tr(\mathbf{X})}{\partial \theta} &= tr(\frac{\partial \mathbf{X}}{\partial \theta}) \\ \frac{\partial \mathbf{X} ^ {-1}}{\partial \theta} &=  -\mathbf{X}^{-1} \frac{\partial \mathbf{X}}{\partial \theta} (\mathbf{I}_k \otimes \mathbf{X}^{-1}) \end{split} $$  
