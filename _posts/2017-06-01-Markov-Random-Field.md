---
title: Markov Random Field
updated: 2017-06-01 09:00
category: [CV]
tags: [MRF]
---

### Notations

We are given a set of elements $$\mathcal{S}$$ and some observations $$\mathcal{F}=\{\mathbf{f}_s \| s\in \mathcal{S} \}$$ of each elements, and we intend to infer the states or class labels $$\Lambda = \{1, ..., L \} $$ for all of them: $$\omega=\{\omega_s, s \in \mathcal{S} \}$$ . The characteristic of the elements is that their states are homogeneous in local neighbourhood, i.e., neighbouring elements presumably share the same states or labels. The neighbouring relationship is defined depending on specific applications and can be intuitively characterized by graph structure. One concrete application is image segmentation: each pixel stands for a single elements and the observations are their intensities and textures. And we want to infer for each pixel which segment it belongs to.

### Objective

The objective is to find the optimal labeling $$\hat{\omega}$$ which maximizes the posterior probability $$P(\omega \| \mathcal{F})$$, that is a *maximum a posteriori* (MAP) estimate:

$$\hat{\omega} = arg \max_{\omega \in \Omega} P(\mathcal{F} \| \omega) P(\omega),$$

where $$\mathcal{\Omega}$$ denotes the set of all possible labelings.

### $$P(\omega)$$

The *label process* $$\omega$$ is modeled as a MRF with respect to a first order neighborhood system. According to the *Hammersley-Clifford theorem* (see Appendix A), $$P(\omega)$$ follows a Gibbs distribution. After several steps of derivations (we refer the reader to [1] for details), we have

$$P(\omega) \propto exp \bigg( - \sum_{ \{s, r\} \in \mathcal{C} } \delta(\omega_s, \omega_r)  \bigg).$$

$$\delta(\omega_s, \omega_r) = 1$$, if $$\omega_s \neq \omega_r$$, and $$\delta(\omega_s, \omega_r) = -1$$ otherwise. The equation explicitly includes the local homogemeous constraint into the MAP estimation.

### $$P(\mathcal{F} \| \omega)$$

$$P(\mathcal{F} \| \omega)$$ is generally modeled by generative models like Multivariate Gaussian Model (MGM) if the observations $$\mathbf{f} \in \mathcal{F}$$ for a given class $$\lambda \in \Lambda$$ are continuous valued and indepent in each dimension of $$\mathbf{f}$$. Suppose that elements are considered to be independent to each other, then follow the routine of MGM, we have

$$P(\mathcal{F} \| \omega) = \prod_{s \in \mathcal{S}} P(\mathbf{f}_s \| \omega_s) \\$$

$$= \prod_{s \in \mathcal{S}} \frac{1}{\sqrt{(2\pi)^n \|\Sigma_{\omega_s} \|}} exp \bigg( -\frac{1}{2} (\mathbf{f}_s - \mathbf{\mu}_{\omega_s}) \Sigma_{\omega_s}^{-1} (\mathbf{f}_s - \mathbf{\mu}_{\omega_s})^T \bigg),$$ 

where $$\mathbf{\mu}_{\omega_s}$$ and $$\Sigma_{\omega_s}$$ are the prior knowledge about the mean and covariance of the gaussian distribution of label $$\omega_s$$.

### Energy function

By combining the two equations above, the final formulation of the MAP is reached:

$$P(\mathcal{F} \| \omega) P(\omega) \propto exp\bigg(-\bigg(\sum_{s \in \mathcal{S}} V_s(\omega_s, \mathbf{f}_s) + \beta \sum_{\{s,r\} \in \mathcal{C}} \delta(\omega_s, \omega_r)  \bigg)\bigg),$$

where 

$$V_s(\omega_s, \mathbf{f}_s) = ln(\sqrt{(2\pi)^n \|\Sigma_{\omega_s} \|}) + \frac{1}{2} (\mathbf{f} - \mathbf{\mu}_{\omega_s}) \Sigma_{\omega_s}^{-1} (\mathbf{f}_s - \mathbf{\mu}_{\omega_s})^T.$$

The weighting parameter $$\beta > 0$$ controlls the importance of the prior. As $$\beta$$ increases, the labels in neighbourhood become more homogemeous.

The interpretation is that the whole posterior can be expressed as a first order MRF by including the contribution of the likelihood term via the singletons (i.e. the elements $$s \in \mathcal{S}$$) as well as the doubletons that express relationship between neighboring labels. 

### Parameter Estimation

The problem modeled by MRF has the following parameters:

* The weight $$\beta$$ of the priori term,
* the number of classes or labels or states $$L$$ (which generally has to be mannually specified),
* the mean vector $$\mathbf{\mu}_{\lambda}$$ and covariance matrix $$\Sigma_{\lambda}$$ of each class $$\lambda \in \Lambda$$.

As claimed in [1], $$\beta$$ is independent of input data in the context of image segmentation. But the independence may vary across different applicatins and thus need to be carefully. The task of estimating the mean and covariance of a mixture density, which is called **Gaussian mixture identification** is a well-known statiscal problem. The most widely used approach is the method of *maximum likelihood*. If a labeled training set is available, then an unbiased estimate of mean and covariance can be obtained in a supervised way. However, in practical cases where elements are unlabeled, **EM algorithm** is adopted to compute the maximum likelihood estimates. Herein, the method is specialized to the **mixture density context**. Basically, we aim to fit a Gaussian mixture of $$L$$ components to the histogram data $$\mathbf{d}_s (s \in \mathcal{S})$$(or distribution) of the observations $$\mathcal{F}$$. $$\mathbf{d}_i$$ denotes the histogram points. The EM algorithm for Gaussian mixture identification can be split into two iterative steps below:

* [Estimation] Try to assign labels to each elements by estimating the posteriors based on the histogram data $$\mathbf{d}_i$$:

$$P(\lambda \| \mathbf{d}_i) = \frac{P(\mathbf{d}_i \| \lambda) P(\lambda)}{ \sum_{\lambda \in \Lambda}P(\mathbf{d}_i \| \lambda)P(\lambda)},$$

where $$P(\lambda)$$ denotes the component weight.

* [Maximization] Using the current labeling of the data, the estimation of the parameters is simple:

$$P(\lambda) = \frac{K_{\lambda}}{\|S\|},$$

$$\mathbf{\mu}_{\lambda} = \frac{1}{K_{\lambda}} \sum_{s \in \mathcal{S}} P(\lambda \| \mathbf{d}_s) \mathbf{d}_s,$$

$$\Sigma_{\lambda} = \frac{1}{K_{\lambda}} \sum_{s \in \mathcal{S}} P(\lambda \| \mathbf{d}_s) (\mathbf{d}_s - \mathbf{\mu}_{\lambda})^T (\mathbf{d}_s - \mathbf{\mu}_{\lambda}),$$

where $$K_{\lambda} = \sum_{s \in \mathcal{S}} P(\lambda \| \mathbf{d}_s)$$. Basically, the posteriors $$P(\lambda \| \mathbf{d}_s)$$ are used as a weight of the data vectors. They express the contribution of a particular data point $$\mathbf{d}_s$$ to the class $$\lambda \in \Lambda$$.


### Appendix A
The *Hammersley-Clifford theorem* says that the distribution over an MRF can be expressed as the product of potential functions $$\phi_i$$ defined on maximal cliques of the graph:

$$P(\mathbf{x}) = \frac{1}{Z} \prod_{c_i \in C} \phi_i(c_i),$$

where $$\phi_i(c_i)$$ is the ith clique potential, a function only of the values of the clique members in $$c_i$$. Each potential function $$\phi_i$$ must be positive but they need not sum to 1. A normalization constant $$Z$$ is required to create a valid probability distribution $$Z = \sum_{\mathbf{x}} \prod_{c_i \in C} \phi_i(c_i)$$.

**Intuitive interpretation of Potential Function** 
Practically it is intractable to compute the joint probability $$P(\mathbf{x})$$ directly. For example, if $$\mathbf{x}$$ is a binary vector of dimension $$n$$, up to $$2^n$$ cases have to be considered. To address this, Markov property of graph is utilized to factorize the joint probality into product of smaller local functions, e.g., the potential functions here.
The potential function ensures that it is possible to factorize the joint probability such that conditionally independent random variables do not appear in the same potential function. In other words, the nodes whose corresponding variables are in the same potential functions must be fully connected. The easiest way to fullfill this requirement is to require each potential function to operate on a set of random variables whose corresponding vertices form a maximal clique within the graph. It is worth noting that an isolated potential function does not have a direct probabilistic interpretation, but instead **represents constraints on the configuration of the random variables**. This in turn affects the probability of global configurations - a global configuration with a high probability is likely to have satisfied more of these constraints than a global configuration with a low probability.


### Reference

[1] [A Markov random field image segmentation model for color textured images](http://ac.els-cdn.com/S0262885606001223/1-s2.0-S0262885606001223-main.pdf?_tid=a6a16140-45c4-11e7-be65-00000aab0f26&acdnat=1496209837_f1d2e7c9835918aed921e3507a2b38e9)

[2] [Gibbs Fields & Markov Random Fields](http://www.cs.cmu.edu/~16831-f14/notes/F11/16831_lecture07_bneuman.pdf)











