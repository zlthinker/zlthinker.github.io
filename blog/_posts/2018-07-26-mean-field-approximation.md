---
title: Mean Field Approximation
updated: 2018-07-26 14:00
---


* TOC
{:toc}

> 基于概率图模型的近似推断方法大概分为三种：(1) Mean field approximation; (2) Belief propagation; (3) Monte Carlo Sampling. ----林达华

## Overview

In mean field theory, a complex probabilistic model is approximated by a set of individual models defined on each vertex. It overlooks the interaction within cliques and averages the interactive effect among the vertices. Mathematically, it approximates the joint distribution $$p^*(\mathbf{z})$$ by the product of factorized distributions: $$q(\mathbf{z}) \approx \prod_{i=1}^d q_i(z_i)$$ by minimizing the KL divergence. Throughout the writing, we are going to elaborate mean field approximation based on Ising model.

## Formulation

The objective is mean field inference is to minimize the KL divergence:

$$\min_{q_1, ..., q_n} KL(q \| p^*) = KL(\prod_{i=1}^d q_i(z_i) \| p^*).$$

Steps are taken iteratively by optimizing $$q_k$$ while fixing other factorized distributions until convergence, i.e.,

$$\min_{q_k} KL(q \| p^*) = KL(\prod_{i=1}^d q_i(z_i) \| p^*).$$

$$KL(\prod_{i=1}^d q_i(z_i) \| p^*) = \int \prod_{i=1}^d q_i \log \frac{\prod_{i=1}^d q_i}{p^*} d \mathbf{z} \\  = \sum_{i=1}^d \int \log q_i \prod_{j=1}^d q_j d \mathbf{z} - \int \prod_{j=1}^d q_j \log p^* d \mathbf{z}  \\ = \int \log q_k \prod_{j=1}^d q_j d \mathbf{z} + \sum_{i \neq k} \int \log q_i \prod_{j=1}^d q_j d \mathbf{z} - \int \prod_{j=1}^d q_j \log p^* d \mathbf{z} \\$$

$$\left( \int \log q_k \prod_{j=1}^d q_j d \mathbf{z} = \int q_k \log q_k \int \prod_{j \neq k}^d q_j d z_{\neq k} d z_k \\ = \int q_k \log q_k d z_k \right)$$

$$\left( \sum_{i \neq k} \int \log q_i \prod_{j=1}^d q_j d \mathbf{z} = const\right)$$

$$= \int q_k \log q_k d z_k - \int q_k \int \prod_{j\neq k} q_j \log p^* d z_{\neq k} d z_k + const \\ = \int q_k (\log q_k - \int \prod_{j\neq k} q_j \log p^* d z_{\neq k}) d z_k + const $$

$$\left( h(z_k) = \int \prod_{j\neq k} q_j \log p^* d z_{\neq k} = E_{q_{-k}}(\log p^*) \right)$$

$$\left(t(z_k) = e^{h(z_k)} = E_{q_{-k}}(p^*)  \right)$$

$$= \int q_k \log \frac{q_k}{t} d z_k + const$$

So in each optimization step, we minimize $$KL(q_k \| t)$$. 

$$\log q_k = \log t(z_k) = h(z_k) = E_{q_{-k}}(\log p^*) + const,$$

where the constant is added to ensure a legal distribution.

## Mean Field Inference on Ising Model

Ising model is defined by 

$$p(y) \propto exp(\frac{1}{2} J \sum_i \sum_{j \in Nbr(i)} y_i y_j + \sum_i b_i y_i ),$$

where $$y_i \in \{1, -1 \}$$.

Then we approximate the distribution by mean field $$p(y) \approx q(y) = \prod_i q_i(y_i)$$.

$$log(q_k(y_k)) = E_{q_{-k}} \log p(y) + const \\ = E_{q_{-k}} (J \sum_{j \in Nbr(k)} y_k y_j + b_k y_k) + const \\ = J \sum_{j \in Nbr(k)} y_k E_{q_{-k}}(y_j) + b_k y_k + const \\ = J y_k \sum_{j \in Nbr(k)} E(y_j) + b_k y_k + const \\ = y_k (J \sum_{j \in Nbr(k)} E(y_j) + b_k) + const \\ =  y_k M + const $$

$$\left( q_k(y_k=1) + q_k(y_k=-1) = C exp(M) + C exp(-M) = 1 \right)$$

$$\left( C = \frac{1}{exp(M) + exp(-M)}  = \right)$$

$$q_k(y_k=1) = \frac{exp(M)}{exp(M) + exp(-M)} = \frac{1}{1+exp(-2M)} = sigmoid(2M)$$

$$q_k(y_k=-1) = \frac{exp(-M)}{exp(M) + exp(-M)} = \frac{1}{1+exp(2M)} = sigmoid(-2M)$$

$$E(y_k) = q_k(y_k=1) - q_k(y_k=-1) =\frac{exp(M) - exp(-M)}{exp(M) + exp(-M)} = tanh(M)$$


