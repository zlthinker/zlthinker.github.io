---
title: Fisher Vector
updated: 2017-06-02 09:00
category: [CV]
tags: [FV]
---

### Introduction

Fisher Vector is a principled patch aggregation mechanism based on the **Fisher Kernel** (FK). The FK combines the benefits of *generative* and *discriminative* approaches to pattern classification by deriving a kernel from a generative model of the data. One of the most popular generative model is Gaussian Mixture Model (GMM), which can be understood as a "probability visual vocabulary". In the nutshell, the rationale consists in characterizing a sample by its deviation from the generative model. The deviation is measured by computing the gradient of the sample log-likelihood with respect to the model parameters. The gradient leads to a vectorial representation which we call Fisher Vector (FV).

**Fisher Vector VS. BoV** The FV representation has a lot of advantages over BoV. First, the FV has higher accuracy than BoV. Actually, the BoV can be regarded as a particular case of the FV where the gradient computation is restricted to the mixture weight parameters of the GMM. However, the FV incorporates additional gradients with respect to the means and covariances of GMM and hence brings large improvement in terms of accuracy. Second, the FV is more efficient than BoV because it can be computed from much smaller vocabularies. Third, the learning process of the FV is efficient (linear in the number of training samples) using techniques such as Stochastic Gradient Descent (SGD).

### Mathematical Formulation

We start with introduction to Fisher Kernel, then explain how to transfer it to Fisher Vector.

Let $$X = \{x_t, t=1,...,T \}$$ be a sample of $$T$$ observations $$x_t \in \mathcal{X}$$. Let $$u_{\lambda} = P(X \| \lambda)$$ be a probability density function which models the generative process of elements in $$\mathcal{X}$$. $$\lambda = [\lambda_1, ..., \lambda_M]^T \in \mathbb{R}^M$$ denote the vector of $$M$$ parameters of $$u_{\lambda}$$. In statistics, the *score function* of $$X$$ is given by the gradient of the log-likelihood of the data on the model:

$$G_{\lambda}^X = \nabla_{\lambda} log u_{\lambda}(X).$$

**The gradients describes how the parameters of the generative model $$u_{\lambda}$$ should be modified to better fit the data $$X$$.** We note that $$G_{\lambda}^X \in \mathbb{R}^M$$, and thus that the dimensionality of the score function only depends on the number of the paramters: $$M$$.

A parametric family of distribution $$\mathcal{u} = \{u_{\lambda}, \lambda \in \Lambda \}$$ can be regarded as a Riemanninan manifold $$M_{\Lambda}$$ (黎曼流体, we refer the reader [here](https://www.quora.com/What-is-the-mathematical-intuition-of-what-a-Riemannian-Manifold-is) for intuitive interpretation) with a local metric given by the positive semi-definite Fisher Information Matrix (FIM) $$F_{\lambda} \in \mathbb{R}^{M \times M}$$:

$$F_{\lambda} = E_{x \text{~} u_{\lambda}}\left[ G_{\lambda}^{X} G_{\lambda}^{X'} \right].$$

Based on the local metric, a similarity measurement between two samples $$X$$ and $$Y$$ using the Fisher Kernel is defined as:

$$K_{FK}(X, Y) = G_{\lambda}^{X^T} F_{\lambda}^{-1} G_{\lambda}^{Y}.$$

Since $$F_{\lambda}^{-1}$$ is positive semi-definite, so is its inverse. Using the Cholesky decomposiiton $$F_{\lambda}^{-1} = L_{\lambda}' L_{\lambda}$$, the above equation can be re-written explicitly as a dot-product:

$$K_{FK}(X, Y) = \mathcal{G}_{\lambda}^{X^T} \mathcal{G}_{\lambda}^{Y},$$

where

$$\mathcal{G}_{\lambda}^{X} = L_{\lambda}G_{\lambda}^X = L_{\lambda} \nabla_{\lambda} log u_{\lambda}(X).$$

We call this normalized gradient vector the *Fisher Vector* (FV) of $$X$$ (Left multiplication of $$L_{\lambda}$$ acts as the normalization operation). In this way, a non-linear kernel machine using $$K_{KF}$$ as a kernel is equivalent to a linear kernel machine using $$\mathcal{G}_{\lambda}^X$$ as feature vector. In other words, learning a feature vector $$\mathcal{G}_{\lambda}^X$$ is equivalent to learning a non-linear classifier $$K_{KF}$$, but makes things much easier.

After getting through the tedious deductions, the problem now boils down to how to obtain a Fisher Vector for a set of feature descriotors $$X$$, e.g. SIFT. Considering that the distribution of descriptors can be very complicated, we choose $$u_{\lambda}$$ to be a Gaussian Mixture Model (GMM) as **one can approximate with arbitrary precision any continuous distribution with a GMM**. In the computer vision literature, a GMM which models the generation process of local descriptors in any scene has been referred to as a **universal (probabilistic) visual vocabulary**. (Recall the BoW that uses a smaller set of k-means centers to approximate the distribution of all desciptors in feature space.) The parameters of the K-component GMM is $$\lambda = \{ \omega_k, \mathbf{\mu_k}, \mathbf{\Sigma_k}, k=1,...,K\}$$, where $$\omega_k$$, $$\mathbf{\mu_k}$$ and $$\mathbf{\Sigma_k}$$ are respectively the mixture weight, mean vector and covariance matrix of Gaussian $$k$$ (the covariance matrix is diagonal under the assumption that different dimensions are independent). We write:

$$u_{\lambda}(x) = \sum_{k=1}^K \omega_k u_k(x),$$

where $$u_k$$ denotes the multivariate Gaussian $$k$$:

$$u_k(x) = \frac{1}{(2 \pi)^{\frac{D}{2}} \|\Sigma_k\|^{\frac{1}{2}}} exp \Big( -\frac{1}{2} (x-\mu_k)^T \Sigma_k^{-1} (x - \mu_k) \Big),$$

and we require:

$$\forall_k: \omega_k \geq 0, \sum_{k=1}^K \omega_k = 1.$$

To enforce above constraints implicitly, we re-parameterize the weight parameters using $$\alpha_k$$:

$$\omega_k = \frac{exp(\alpha_k)}{\sum_{j=1}^K exp(\alpha_j)}.$$

After the re-parameterization, the parameters of the GMM model $$u_k$$ becomes $$\lambda=\{\alpha_k, \mu_k, \Sigma_k, k=1,...,K\}$$. Note that there are $$E=(2D+1)K$$ parameters in total. Then the expressions of the gradients w.r.t the parameters can be easily computed by $$\nabla_{\lambda} log u_{\lambda}(X)$$ (To simplify the computation of $$u_{\lambda}(X)$$, we hold an inappropriate assumption that all the elements are independent. This independence assumption is compensated to correct the negative effect it brings by normalization as shown in Section 2.3 in [1]). 

Having the expression for the gradients, the remaining question is how to compute $$L_{\lambda} \in \mathbb{R}^{E\times E}$$, which is the square root of the inverse of the FIM. We first define the posterior probability or responsibility which describes the soft assignment of $$x_t$$ to Gaussian $$k$$:

$$\gamma_t(k) = \frac{\omega_k u_k(x_t)}{ \sum_{j=1}^K \omega_j u_j(x_t)}.$$ 

The claim in [1] says that if the assignment is almost hard, the FIM is diagonal. Then the element on the diagonal line can be calculated by equations shown in [1].



### Reference

[1] [Image Classification with the Fisher Vector: Theory
and Practice](https://hal.inria.fr/hal-00779493/file/RR-8209.pdf)





















