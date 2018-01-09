---
title: Optimization for Least Square Problems
updated: 2017-06-12 19:00
category: [optimization]
---

### The Problem Definition
The problem is to find solutions to a system of equations that have the form:

$$f_1(\mathbf{x}) = 0,$$

$$f_2(\mathbf{x}) = 0,$$

$$f_3(\mathbf{x}) = 0,$$

$$...$$

$$f_m(\mathbf{x}) = 0,$$

where $$\mathbf{x} = [x_1, x_2, ..., x_n]^T$$ is a $$n$$-dimensional vector. For convenience, we denote $$(f_1, f_2, ..., f_m)^T$$ by a vector-valued function $$\mathbf{f}$$ and $$\mathbf{f}$$ can be nonlinear functions. Then the system of equations can be re-written as

$$\mathbf{f}(\mathbf{x}) = 0,$$

i.e., we wish to find a vector that makes the vector function equal to the zero vector. Since it is the general case that $$m$$ is larger that $$n$$, i.e., the system of equations is overdetermined, the problem would infeasible. Therefore, the alternative is to find the optimal solution which minimizes the sum of the least squares which is also the objective of the *least squares* problem:

$$ F(\mathbf{x}) = \frac{1}{2} \sum_{i=1}^m \|f_i(\mathbf{x}) \|_2^2 = \frac{1}{2} \mathbf{f}(\mathbf{x})^T \mathbf{f}(\mathbf{x}).$$ 

#### Terminalogy
The first and second derivatives of the objective function are given below:

$$\nabla F(\mathbf{x}) = F'(\mathbf{x}) = \mathbf{f}(\mathbf{x})^T \mathbf{J}(\mathbf{x}),$$

where $$\mathbf{J}(\mathbf{x})$$ is the $$m \times n$$ jacobian matrix of the transformation $$\mathbf{f} : \mathbb{R}^m \times \mathbb{R}^n$$.

$$\nabla^2 F(\mathbf{x}) = F''(\mathbf{x}) = \mathbf{J}(\mathbf{x})^T \mathbf{J}(\mathbf{x}) + \sum_{i=1}^m f_i(\mathbf{x}) \mathbf{H}_i(\mathbf{x}), $$

where $$\mathbf{H}_i(\mathbf{x}) = f_i''(\mathbf{x})$$ is the $$n \times n$$ Hessian matrix of the scalar-valued function $$f_i(\mathbf{x})$$.

### Newton's Method
Newton's method is an algorithm originally designated to locate roots of equations, e.g., $$F(\mathbf{x})=0$$. It is derived from the linear approximation of the function $$F(\mathbf{x})$$ by first-order Tayor series expansion. From Calculus, the following is the linear approximation of $$F(\mathbf{x})$$ at $$\mathbf{x}+\mathbf{h}$$:

$$F(\mathbf{x} + \mathbf{h}) \approx F(\mathbf{x}) + F'(\mathbf{x}) \mathbf{h},$$

if $$\|\mathbf{h}\|$$ is sufficiently small. By rearranging the equation, we obtain 

$$F(\mathbf{x}) \approx F(\mathbf{x}_t) + F'(\mathbf{x}_t) (\mathbf{x} - \mathbf{x}_{t}).$$

The first-order Tayor series expansion essentially approximate the origin function **locally** with the tangent line at the local point. If the local point is near the zero point we want, it is reasonable to approximate the desired zero point ($$F(\mathbf{x})=0$$) with the zero point of tangent line ($$F(\mathbf{x}_t) + F'(\mathbf{x}_t) (\mathbf{x} - \mathbf{x}_{t}) = 0$$). In this way, we get the improved estimate of $$\mathbf{x}$$ as $$\mathbf{x}_{t+1}$$ from last estimate $$\mathbf{x}_t$$ that satisfies

$$F(\mathbf{x}_t) + F'(\mathbf{x}_t) (\mathbf{x}_{t+1} - \mathbf{x}_{t}) = 0.$$

The update step is explicitly written as 

$$\mathbf{x}_{t+1} = \mathbf{x}_{t} - \frac{F(\mathbf{x}_t)}{F'(\mathbf{x}_t)} \equiv \mathbf{x}_t + \mathbf{h} $$

![Minion](../images/newton's_method.png)


Newton's method can also be used as a **descent method** for iterative non-linear optimization: from a starting point $$\mathbf{x}_0$$ it produces a series of vectors $$..., \mathbf{x}_t, \mathbf{x}_{t+1},...$$ which (hopefully) converges to $$\mathbf{x}^*$$, a local stationary minimizer for the given function $$F(\mathbf{x})$$. Since $$\mathbf{x}^*$$ is a stationary point, it satisfies $$F'(\mathbf{x}^*) = 0$$. Similarly, we have

$$F'(\mathbf{x}+\mathbf{h}) \approx F'(\mathbf{x}) + F''(\mathbf{x}) \mathbf{h},$$

Here the Newton's method is used to locate the root of $$F'(\mathbf{x}^*) = 0$$. The update step is 

$$\mathbf{x}_{t+1} = \mathbf{x}_t + \mathbf{h}_N,$$

where $$\mathbf{h}_N$$ is the solution to 

$$F'(\mathbf{x}) + F''(\mathbf{x}) \mathbf{h} = 0.$$

If $$F''(\mathbf{x})$$ is positive definite, $$\mathbf{h}_N^T F''(\mathbf{x}) \mathbf{h}_N = - \mathbf{h}_N^T F'(\mathbf{x}) > 0$$, which means $$\mathbf{h}_N$$ is a descent direction when minimizing $$F(\mathbf{x})$$. Therefore, Newton's method is only useful in the final stage of the iteration where $$\mathbf{x}$$ is close to $$\mathbf{x}^*$$ so that $$F''(\mathbf{x})$$ is positive definite. The good news about Newton's method is that if $$F''(\mathbf{x})$$ is positive definite, we get **quadratic convergence** while gradient descent (the steepest descent method) can only produce **linear convergence**.


### Gauss-Newton Method
The Gauss-Newton method is based on the linear approximation to $$\mathbf{f}$$ by the first-order **Tayor series expansion** in the neighbourhood of $$\mathbf{x}$$: For small $$\|\mathbf{h}\|$$, we have

$$\mathbf{f}(\mathbf{x} + \mathbf{h}) = \ell(\mathbf{h}) \approx \mathbf{f}(\mathbf{x}) + \mathbf{J}(\mathbf{x}) \mathbf{h},$$

where $$\mathbf{J}(\mathbf{x})$$ is the Jacobian matrix of $$\mathbf{f}(\mathbf{x})$$. Then

$$F(\mathbf{x} + \mathbf{h}) = L(\mathbf{h}) = \frac{1}{2} \ell(\mathbf{h})^T \ell(\mathbf{h})$$

$$\approx \frac{1}{2} \mathbf{f}^T\mathbf{f} + \mathbf{h}^T\mathbf{J}^T\mathbf{f} + \frac{1}{2} \mathbf{h}^T \mathbf{J}^T\mathbf{J}\mathbf{h}$$

$$=F(\mathbf{x}) + \mathbf{h}^T\mathbf{J}^T\mathbf{f} + \frac{1}{2} \mathbf{h}^T \mathbf{J}^T\mathbf{J}\mathbf{h}, $$

where $$\mathbf{f} = \mathbf{f}(\mathbf{x})$$ and $$\mathbf{J} = \mathbf{J}(\mathbf{x})$$. 


There is a little interesting fact to address here. If we directly apply the second-order taylor expansion to $$F(\mathbf{x})$$, we get

$$
F(\mathbf{x} + \mathbf{h}) = L(\mathbf{h}) =
F(\mathbf{x}) + \mathbf{h}^T\mathbf{J}^T\mathbf{f} + \frac{1}{2} \mathbf{h}^T \mathbf{H}\mathbf{h},
$$

where $$\mathbf{H}$$ is the Hessian matrix at current $$\mathbf{x}$$. Since the Hessian matrix is generally hard to compute, the square of jacobian $$\mathbf{J}^T\mathbf{J}$$ is often used to approximate the Hessian matrix $$\mathbf{H}$$. As shown by the second equation in **Terminalogy**, the approximation just eliminates the second term $$\sum_{i=1}^m f_i(\mathbf{x}) \mathbf{H}_i(\mathbf{x})$$.


Getting back to the point, the Gauss-Newton step is to minimize $$L(\mathbf{h})$$, i.e., the objective at next point, 

$$\mathbf{h}_{GN} = \arg\min_{\mathbf{h}} L(\mathbf{h}).$$

It is easily seen that the gradient and the Hessian if $$L$$ with respect to $$\mathbf{h}$$ are

$$L'(\mathbf{h}) = \mathbf{f}^T\mathbf{J} + \mathbf{h}^T \mathbf{J}^T \mathbf{J},$$

$$L''(\mathbf{h}) = \mathbf{J}^T \mathbf{J}.$$

We can see that $$L''(\mathbf{h})$$ is independent of $$\mathbf{h}$$. If $$\mathbf{J}$$ is *full rank*, i.e., if the columns are linearly independent, then $$L''(\mathbf{h})$$ is positive definite, which means $$L(\mathbf{h})$$ is convex. Inspired by Newton's method, the minimization problem can be solved by finding $$\mathbf{h}$$ that makes $$L'(\mathbf{h}) = \mathbf{0}$$, i.e.

$$(\mathbf{J}^T \mathbf{J}) \mathbf{h}_{GN} = - \mathbf{J}^T\mathbf{f}.$$

This is a descent direction for $$F$$ since

$$\mathbf{h}_{GN}^T F'(\mathbf{x}) = \mathbf{h}_{GN}^T (\mathbf{J}^T \mathbf{f}) = - \mathbf{h}_{GN}^T (\mathbf{J}^T \mathbf{J})\mathbf{h}_{GN} < 0.$$

The update step of the Gauss-Newton method is typically:

$$\text{Solve}\,\,\,\, (\mathbf{J}^T \mathbf{J}) \mathbf{h}_{GN} = - \mathbf{J}^T\mathbf{f},$$

$$\mathbf{x}_{t+1} = \mathbf{x}_t + \alpha \mathbf{h}_{GN},$$

where $$\alpha$$ is found by linear search. The classical Gauss-Newton method uses $$\alpha = 1$$ in all steps. In general, the Gauss-Newton method ensure linear convergence $$(\|\mathbf{x}_{t+1} - \mathbf{x}^* \| \leq a\|\mathbf{x}_t - \mathbf{x}^* \|, 0 < a < 1)$$ and even quadratic convergence $$(\|\mathbf{x}_{t+1} - \mathbf{x}^* \| = O(\|\mathbf{x}_t - \mathbf{x}^* \|^2))$$ when $$\mathbf{x}$$ is close to $$\mathbf{x}^*$$.

### The Levenberg–Marquardt Method

The LM method suggests to use a *damped Gauss-Newton mothod*. The step $$\mathbf{h}_{LM}$$ is defined by the following modification:

$$(\mathbf{J}^T \mathbf{J} + \mu \mathbf{I}) \mathbf{h}_{LM} = - \mathbf{J}^T\mathbf{f} \text{ and } \mu \geq 0.$$

The damping parameter $$\mu$$ has several effects:

* For all $$\mu > 0$$ the coefficient matrix $$\mathbf{J}^T \mathbf{J} + \mu \mathbf{I}$$ is positive definite, and this ensures that $$\mathbf{h}_{LM}$$ is a descent direction.
* For large values of $$\mu$$ we get $$\mathbf{h}_{LM} = \frac{1}{\mu} \mathbf{J}^T\mathbf{f} = \frac{1}{\mu} F'(\mathbf{x}),$$
i.e. a short step in the steepest descent direction. This is desired if the current iterate is far from the solution. But, the larger damping factor $$\mu$$ is, the slower convergence gets.
* For small values of $$\mu$$, $$\mathbf{h}_{LM} = \mathbf{h}_{GN}$$, which is a good step in the final stage of the iteration. When $$\mathbf{x}$$ is close to $$\mathbf{x}^*$$, the LM can have almost quatratic final convergence as the Gauss-Newton method.

Thus, the damping parameter influences both the direction and the size of the update step. This leads us to make a method without a specific line search (i.e., to explicitlt determine the step size).

### The Golden Bundle Adjustment

Bundle Adjustment (BA) is a classic optimization problem developed in photogrammetry in the 50's. Each error function corresponds to a reprojection error concerning a camera and a 3D point.

Suppose a BA problem considers a set of $$m$$ cameras $$\mathcal{C} = \{c_1, c_2, \ldots, c_m \}$$, a set of $$n$$ points $$\mathcal{P}= \{p_1, p_2, \ldots, p_n \}$$ and a set of projections $$\mathcal{E} \in \mathcal{C} \times \mathcal{P}$$, the objective is to estimate the $$(m+n)$$-dimensional vector $$\mathbf{x} = [c_1, c_2, \ldots, c_m, p_1, p_2, \ldots, p_n]^T$$. The total $$k$$ reprojection functions form a $$k$$ dimensional vector denoted by $$\mathbf{e} = [e_1, e_2, \ldots, e_k]^T$$. The Jacobian matrix is computed by taking the first-order derivative of the error vector $$\mathbf{e}$$ with respect to the input vector $$\mathbf{x}$$:

$$\mathbf{J}_{k \times (m+n)} = 
\Big[ 
\frac{\partial \mathbf{e} }{\partial c_1},
\ldots,
\frac{\partial \mathbf{e} }{\partial c_m},
\frac{\partial \mathbf{e} }{\partial p_1},
\ldots,
\frac{\partial \mathbf{e} }{\partial p_n}
 \Big]
\equiv \Big[ 
 \mathbf{J}_{c_1},
 \ldots,
 \mathbf{J}_{c_m},
 \mathbf{J}_{p_1},
 \ldots,
 \mathbf{J}_{p_n}
\Big]
\equiv \Big[ 
 \mathbf{J}_{c},
 \mathbf{J}_{p}
 \Big].
 $$
 
 Based on the LM algorithm elaborated above, the update step is to solve the equation:
 
 $$
 (\mathbf{J}^T \mathbf{J} + \mu \mathbf{I} ) \Delta \mathbf{x} = - \mathbf{J}^T\mathbf{e}.
 $$
 
 The symmetric matrix $$\mathbf{J}^T \mathbf{J}$$ is quite sparse because each reprojection error function only involves one camera and one track. Here it can be re-written as 
 
$$
\mathbf{J}^T \mathbf{J} =
\left[ {\begin{array}{cc}
   \mathbf{J}_c^T \mathbf{J}_c & \mathbf{J}_c^T \mathbf{J}_p \\
   \mathbf{J}_p^T \mathbf{J}_c & \mathbf{J}_p^T \mathbf{J}_p \\
  \end{array} } \right].
$$

For simplicity, the damping term $$\mu \mathbf{I}$$ is omitted here, so that the update equation is turned into 

$$
\left[ {\begin{array}{cc}
   \mathbf{J}_c^T \mathbf{J}_c & \mathbf{J}_c^T \mathbf{J}_p \\
   \mathbf{J}_p^T \mathbf{J}_c & \mathbf{J}_p^T \mathbf{J}_p \\
  \end{array} } \right]
\left[ {\begin{array}{cc}
   \Delta \mathbf{c} \\
   \Delta \mathbf{p} \\
  \end{array} } \right]
=
\left[ {\begin{array}{cc}
   -\mathbf{J}_c^T \mathbf{e} \\
   -\mathbf{J}_p^T \mathbf{e} \\
  \end{array} } \right].
$$

Now, the problem is to solve the linear equation system of this form:

$$
\left[ {\begin{array}{cc}
   B & E \\
   E^T & C \\
  \end{array} } \right]
\left[ {\begin{array}{cc}
   \Delta y \\
   \Delta z \\
  \end{array} } \right]
=
\left[ {\begin{array}{cc}
   u \\
   w \\
  \end{array} } \right].
$$

Its solution is obtained as the solution of 

$$
\left[ B-EC^{-1}E^T \right] \Delta y = u - E C^{-1} w,
$$

$$\Delta z = C^{-1}(w - E^T \Delta y)$$

by using the **Schur Complement**. And the first equation can then be solved by applying **Cholesky factorization** (
$$\mathbf{A} \mathbf{x} = \mathbf{b} $$
$$ \Rightarrow $$
$$\mathbf{L} \mathbf{L}^T \mathbf{x} = \mathbf{b}$$
$$ \Rightarrow $$
Solve $$\mathbf{y}$$ in $$\mathbf{L} \mathbf{y} = \mathbf{b}$$ 
$$ \Rightarrow $$ 
Solve $$\mathbf{x}$$ in $$\mathbf{L}^T \mathbf{x} = \mathbf{y}$$ ).
Depending on the sparsity of the matrix, Cholesky factorization can be implemented in dense, sparse and iterative ways.







### Reference
[Nonlinear Systems - Newton’s Method](http://www.math.ohiou.edu/courses/math3600/lecture13.pdf)

[METHODS FOR
NON-LINEAR LEAST
SQUARES PROBLEMS](http://www2.imm.dtu.dk/pubdb/views/edoc_download.php/3215/pdf/imm3215.pdf)