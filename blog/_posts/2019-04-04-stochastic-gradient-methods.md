---
title: Stochastic Gradient Methods
updated: 2019-04-04 20:30
---

* TOC
{:toc}

> These are the notes on the [great material](https://arxiv.org/pdf/1606.04838.pdf), including the key insights and my undertanding about stochastic gradient methods. 

# Overview

Let $$w\in\mathbb{R}^d$$ denote the vector of model parameters to be determined, and $$\xi$$ denote a sample of data. Then in a optimization problem, the loss is defined by the **expected risk** with respect to the distribution of $$\xi$$, which writes

$$R(w) = \mathbb{E} [f(w; \xi)],$$

where $$f(\cdot; \cdot)$$ is the loss function. In the context of stochastic gradient methods, the expected risk is approximated by the **empirical risk** which equals to the mean loss of samples drawn from the distribution of $$\xi$$, i.e.,

$$R_n(w) = \frac{1}{n} \sum_{i=1}^n f_i(w),$$

where $$f_i(w)$$ denotes the loss term of the i-th sample. If the samples are drawn exactly from its distribution, e.g., through Monte Carlo sampling, the empirical risk shall converge to the expected risk as $$n$$ grows.


# Lemmas

# SG for Strongly Convex Objectives

# SG for General Objectives

# Complexity Analysis