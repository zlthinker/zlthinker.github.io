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

$$\|F(w) - F(w')\|_2 \leq L\|w - w' \|_2, \textrm{for all} w \in \mathbb{R}^d, w' \in \mathbb{R}^d. $$

# SG for Strongly Convex Objectives

# SG for General Objectives

# Complexity Analysis