---
title: Stochastic Gradient Methods
updated: 2019-04-04 20:30
---

* TOC
{:toc}

> These are the notes on the [great material](https://arxiv.org/pdf/1606.04838.pdf), including the key insights and my undertanding about stochastic gradient methods. 

# Overview

Let $$w\in\mathbb{R}^d$$ denote the vector of model parameters to be determined, and $$\xi$$ denote a sample of data. Then in a optimization problem, the objective $$F(w): \mathbb{R}^d \rightarrow \mathbb{R}$$ is defined by the **expected risk** with respect to the distribution of $$\xi$$, which writes

$$F(w) = R(w) = \mathbb{E} [f(w; \xi)],$$

where $$f(\cdot; \cdot)$$ is the loss function. In the context of stochastic gradient methods, the objective is defined as the **empirical risk** which equals to the mean loss of samples drawn from the distribution of $$\xi$$, i.e.,

$$F(w) = R_n(w) = \frac{1}{n} \sum_{i=1}^n f(w; \xi_i) := \frac{1}{n} \sum_{i=1}^n f_i(w),$$

where $$f_i(w)$$ denotes the loss term of the i-th sample. If the samples are drawn exactly from its distribution, e.g., through Monte Carlo sampling, the empirical risk shall converge to the expected risk as $$n$$ grows.

For the simple SG methods, the stochastic direction at iterate $$k$$ is evaluated as $$g(w_k, \xi_k) = \nabla f(w_k; \xi_k)$$. As opposed, the mini-bacth SG methods compute the stochastic direction as $$g(w_k, \xi_k) = \frac{1}{n_k} \sum_{i=1}^{n_k}\nabla f(w_k; \xi_k)$$, where $$n_k$$ is the batch size.


# Two fundamental Lemmas

The SG methods are built upon two assumptions stated below. The first is about the smoothness of the objective function $$F(w)$$. The second is about the first and second moments of the stochastic vector $$g(w_k, \xi_k)$$. _(In the context of probability, the first moment is the mean, the second central moment is the variance.)_

### Assumption 1

The objective function $$F(w): \mathbb{R}^d \rightarrow \mathbb{R}$$ is continuously differentiable and its gradient, namely $$\nabla F(w): \mathbb{R}^d \rightarrow \mathbb{R}^d$$, is Lipschitz continuous with Lipschitz constant $$L>0$$, i.e.,

$$\|F(w) - F(w')\|_2 \leq L\|w - w' \|_2, \textrm{ for all } w \in \mathbb{R}^d, w' \in \mathbb{R}^d. $$

**Interpretation** The Lipschitz continuity assumption over the gradient of $$F(w)$$ requires that the gradient should not change arbitrarily quickly with respect to the parameter vector $$w$$. It also means that the Hessian function $$\nabla^2F(w): \mathbb{R}^d \rightarrow \mathbb{R}^{d\times d}$$ is bounded above by the Lipschitz constant $$L$$. Then based on the second-order taylor expansion of $$F(w)$$, we arrive at the inequality that

$$F(w) \leq F(w') + \nabla F(w') (w-w') + \frac{1}{2} L \|w-w'\|_2^2, \textrm{ for all } w \in \mathbb{R}^d, w' \in \mathbb{R}^d. $$

Now, let's consider the k-th iterate where we draw a sample $$\xi_k$$ to compute the stochastic gradient vector $$g(w_k, \xi_k)$$ and update the step as $$w_{k+1} = w_k - \alpha_k g(w_k,xi_k)$$. Under Assumption 1, the iterate satisfies the following inequality:

$$\begin{split} F(w_{k+1}) - F(w_k) &\leq \nabla F(w_k)^T (w_{k+1} - w_k) + \frac{1}{2} L\|w_{k+1} - w_k \|_2^2 \\ &\leq \alpha_k \nabla F(w_k)^T g(w_k, \xi_k) + \frac{1}{2} \alpha_k^2 L \|g(w_k, \xi_k) \|_2^2. \end{split}$$

**Lemma 1** Taking expectations in these equalities with respect to the distribution of $$\xi_k$$, we have

$$\mathbb{E}_{\xi_k} [F(w_{k+1})] - F(w_k) \leq -\alpha_k \nabla F(w_k)^T \mathbb{E}_{\xi_k} [g(w_k, \xi_k)] +  \frac{1}{2} \alpha_k^2 L \mathbb{E}_{\xi_k} [ \|g(w_k, \xi_k) \|_2^2 ].$$

**Interpretation** The inequality above implies that the expected decrease in the objective function yielded by the k-th step is bounded above by the quantity on the right hand side. The convergence is guaranteed as long as the quantity is negative by choosing the step size $$\alpha_k$$ and the stochastic direction $$g(w_k, \xi_k)$$ carefully. Intuitively, the first term $$\nabla F(w_k)^T \mathbb{E}_{\xi_k} [g(w_k, \xi_k)]$$ should be as large as possible (The stochastic gradient should correlate with the true gradient better with less noise), and the second term $$\mathbb{E}_{\xi_k} [ \|g(w_k, \xi_k) \|_2^2 ]$$ should be as small as possible. And this is how Assumption 2 below is derived.

### Assumption 2

The objective function and SG satisfy the following:

* $$F(w_k)$$ is bounded below by a scalar $$F_{inf}$$ for all $$w_k$$.
* There exist scalars $$\mu_G \geq \mu > 0$$ such that, for all $$k \in \mathbb{N}$$,

$$\begin{split} \nabla F(w_k)^T \mathbb{E}_{\xi_k} [g(w_k, \xi_k)] & \geq \mu \| \nabla F(w_k) \|_2^2  \\ \|\mathbb{E}_{\xi_k} [g(w_k, \xi_k)] \|_2 &\leq \mu_G  \| \nabla F(w_k) \|_2 .\end{split}$$

* There exist scalars $$M \geq 0$$ and $$M_V \geq 0$$ such that, for all $$k \in \mathbb{N}$$, 

$$\begin{split} \mathbb{V}_{\xi_k} [g(w_k, \xi_k)] &:= \mathbb{E}_{\xi_k} [ \|g(w_k, \xi_k) \|_2^2 ] - \| \mathbb{E}_{\xi_k} [g(w_k, \xi_k)] \|_2^2 \\ &\leq M + M_V \| \nabla F(w_k) \|_2^2. \end{split}$$

**Interpretation** The second condition above is to satisfy **Lemma 1** which ensures sufficient convergence of SG. The third condition restricts the variance of $$g(w_k, \xi_k)$$ ($$g(w_k, \xi_k)$$ should not change a lot for different $$\xi_k$$). For example, if $$F$$ is a quatratic function, then $$\| \nabla F(w_k) \|_2^2$$ is quatratic w.r.t $$w_k$$, so that the variance is allowed to grow quadratically in any direction.

From **Assumption 2**, we can induce that 

$$\begin{split} \mathbb{E}_{\xi_k} [ \|g(w_k, \xi_k) \|_2^2 ] &= \mathbb{V}_{\xi_k} [g(w_k, \xi_k)] + \| \mathbb{E}_{\xi_k} [g(w_k, \xi_k)] \|_2^2 \\  &\leq M + M_G \| \nabla F(w_k) \|_2^2 ,\end{split}$$

where $$M_G = M_V + \mu_G^2 \geq \mu^2 > 0.$$

**Lemma 2** Combining **Assumption 2**, the inequality of **Lemma 1** can be expanded as

$$\begin{split} \mathbb{E}_{\xi_k} [F(w_{k+1})] - F(w_k) &\leq - \mu \alpha_k \| \nabla F(w_k) \|_2^2 +  \frac{1}{2} \alpha_k^2 L \mathbb{E}_{\xi_k} [ \|g(w_k, \xi_k) \|_2^2 ] \\ & \leq - (\mu - \frac{1}{2} \alpha_k L M_G) \alpha_k \| \nabla F(w_k) \|_2^2 + \frac{1}{2} \alpha_k^2 L M .\end{split}$$

Note that the first term on the right hand side is strictly negative for small step size $$\alpha_k$$, while the second term could be large enough to allow the objective to increase. Balancing the two terms is critical in the design of SG methods.

# SG for Strongly Convex Objectives

As a starting point, we analyze SG of strong convex objective with parameter $$c (c<L)$$, i.e.,

$$F(w') \geq F(w) + \nabla F(w)^T (w - w') + \frac{1}{2} c \|w' - w \|_2^2.$$

It amounts to $$\nabla^2F(w) \succeq c$$ (the minimum eigenvalue of $$\nabla^2F(w)$$ is no less than $$c$$).

**Lemma 3 （Strongly convex objective, fixed stepsize）** Suppose that SG is run with a fixed stepsize $$0 < \bar{\alpha} \leq \frac{\mu}{LM_G}$$, then the expected optimality gap satisfies

$$\begin{split} \mathbb{E}[F(w_k) - F_*] &\leq \frac{\bar{\alpha} LM}{2c\mu}  + (1-\bar{\alpha} c \mu)^{k-1} \left( F(w_1) - F_* -\frac{\bar{\alpha} LM}{2c\mu}  \right) \\ & \rightarrow \frac{\bar{\alpha} LM}{2c\mu} (k \rightarrow \infty) \end{split}$$

**Interpretation** If there were no noise in gradient computation ($$M=0$$), the expected optimality gap will converge to zero with linear convergence rate ($$\mathbb{E}[F(w_k) - F_*] \leq (1-\bar{\alpha} c \mu)^{k-1} \left( F(w_1) - F_* \right) $$). If the gradient computation is noisy, the expected objective value will first converge linearly, but after some point, the noise prevent further convergence and a smaller step size is desired. A smaller step size allows one to arrive closer to the optimal value, but slows down the convergence. Due to the strongly convex assumption, the positive $$c$$ lead to contraction of the expected optimality gap.

**Lemma 4 （Strongly convex objective, dimishing stepsize）** Suppose that SG is run with a stepsize sequence such that $$\alpha_k = \frac{\beta}{\gamma + k}$$ for some $$\beta > \frac{1}{c\mu}$$ and $$\mu > 0$$ (ensure $$\alpha_1 \leq \frac{\mu}{LM_G}$$), then the expected optimality gap satisfies

$$\mathbb{E}[F(w_k) - F_*] \leq \frac{v}{\gamma + k},$$

where $$v = \max \{\frac{\beta^2 LM}{2(\beta c\mu - 1)}, (\gamma + 1)(F(w_1) - F_*)  \}.$$

**Remark** For practical purposes, the initial step size should be chosen as large as allowed, i.e., the upper bound $$\frac{\mu}{LM_G}$$. More rigorously, the step size should satisfy

$$\sum_{k=1}^\infty \alpha_k= \infty \textrm{ and } \sum_{k=1}^\infty \alpha_k^2 < \infty.$$ 

**Lemma 5 （Strongly convex objective, fixed step size and batch size）**

If we use a mini batch size of $$n_{mb}$$, the constants $$M$$ and $$M_V$$ will become $$M/n_{mb}$$ and $$M_V/n_{mb}$$. Then the expected optimality gap will be turned into

$$\mathbb{E}[F(w_k) - F_*] \leq \frac{\bar{\alpha} LM}{2c\mu n_{mb}}  + (1-\bar{\alpha} c \mu)^{k-1} \left( F(w_1) - F_* -\frac{\bar{\alpha} LM}{2c\mu n_{mb}}  \right) .$$

We notice that the gap to the optimal value is also reduced by a factor of $$n_{mb}$$. Suppose we run the simple SG with batch size of $$1$$, we should set the step size to $$\bar{\alpha}/n_{mb}$$ to achieve the same optimality gap when convergence, then the expected optimality gap at iterate $$k$$ becomes

$$\mathbb{E}[F(w_k) - F_*] \leq \frac{\bar{\alpha} LM}{2c\mu n_{mb}}  + (1- \frac{\bar{\alpha} c \mu}{n_{mb}})^{k-1} \left( F(w_1) - F_* -\frac{\bar{\alpha} LM}{2c\mu n_{mb}}  \right) .$$

We can see that the gap contracts more slowly than mini-batch SG, which means one need to run $$n_{mb}$$ times more iterations to obtain an equivalent optimality gap. However, the simple SG only consumes $$1/n_{mb}$$ cost of mini-batch SG at each iterate, it amounts to the same total computation as for the mini-bacth SG.

# SG for General Objectives

**Lemma 6 （Nonconvex objective, fixed stepsize）** Suppose that SG is run with a fixed stepsize $$0 < \bar{\alpha} \leq \frac{\mu}{LM_G}$$, then the sum-of-square and average-squared gradients of the objective satisfy

$$\begin{split} \mathbb{E}\left[ \sum_{k=1}^K \| \nabla F(w_k) \|_2^2 \right] & \leq \frac{K \bar{\alpha} LM}{\mu} + \frac{2(F(w_1) -F_{inf})}{ \mu \bar{\alpha}}, \textrm{ therefore,} \\ \mathbb{E}\left[\frac{1}{K} \sum_{k=1}^K \| \nabla F(w_k) \|_2^2 \right] & \leq \frac{\bar{\alpha} LM}{\mu} + \frac{2(F(w_1) -F_{inf})}{ K \mu \bar{\alpha}} \rightarrow \frac{\bar{\alpha} LM}{\mu} (K \rightarrow \infity)   \end{split}$$

**Interpretation** If there were no noise with the gradient ($$M=0$$), the sum-of-square gradients will remain finite, implying that $$\| \nablaF(w_k)\| \rightarrow 0 (k \rightarrow \infty)$$. If there were noise with the gradient ($$M>0$$), the average norm of gradients decreases when $$K$$ increases but cannot converges to zero. It illustrate that noise in the gradients inhibits further progress.

**Lemma 6 （Nonconvex objective, diminishing stepsize）** Suppose that SG is run with a stepsize sequence satisfying $$\sum_{k=1}^\infty \alpha_k= \infty$$ and $$\sum_{k=1}^\infty \alpha_k^2 < \infty$$, then

$$\begin{split} lim_{K \rightarrow \infty} \mathbb{E} \left[ \sum_{k=1}^K \alpha_k \| \nabla(w_k)  \|_2^2 \right] & < \infty \textrm{ therefore,} \\ lim_{K \rightarrow \infty} \mathbb{E} \left[ frac{1}{A_k} \sum_{k=1}^K \alpha_k \| \nabla(w_k)  \|_2^2 \right] &= 0 \end{split}$$

**Interpretation** The second equality above indicates that the weighted average of the gradient normal converges to zero even if the gradients are noisy.


# Complexity Analysis