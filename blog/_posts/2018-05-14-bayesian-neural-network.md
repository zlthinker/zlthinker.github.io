---
title: Bayesian Neural Network
updated: 2018-05-14 14:00
---

* TOC
{:toc}

### Related Works

* [1] [Coursera: Bayesian Neural Networks](https://www.coursera.org/learn/bayesian-methods-in-machine-learning/lecture/HI8ta/bayesian-neural-networks)
* [2] SGLD: [Bayesian Learning via Stochastic Gradient Langevin Dynamics](https://www.ics.uci.edu/~welling/publications/papers/stoclangevin_v6.pdf): (ICML2011)
* [3] preconditioned SGLD: [Preconditioned Stochastic Gradient Langevin Dynamics for Deep Neural Networks](http://people.ee.duke.edu/~lcarin/aaai_psgld_final.pdf): (AAAI 2016)
* [4] [Bayesian Dark Knowledge](https://arxiv.org/pdf/1506.04416.pdf): (NIPS2015)
* [5] [Modelling Uncertainty in Deep Learning for Camera Relocalization](https://arxiv.org/pdf/1509.05909.pdf)
* [6] [DSAC-Differentiable RANSAC for camera localization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Brachmann_DSAC_-_Differentiable_CVPR_2017_paper.pdf)
* [7] [Learning Weight Uncertainty with Stochastic Gradient MCMC for Shape Classification](http://openaccess.thecvf.com/content_cvpr_2016/papers/Li_Learning_Weight_Uncertainty_CVPR_2016_paper.pdf)

* Dropout as Bayesian Neural Network
* [8] [Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning](https://arxiv.org/pdf/1506.02142.pdf): ICML, 2016
* [9] [Bayesian convolutional neuralnetworks with Bernoulli approximate variational inference](https://arxiv.org/pdf/1506.02158.pdf): ICLR workshop, 2016
* [10] [Variational Inference: A Review for Statisticians](https://arxiv.org/pdf/1601.00670.pdf)

### Formulation

Given a training dataset $$\mathcal{D} = \{d_i\}_{i=1}^N = \{(x_i, y_i)\}_{i=1}^N$$ where $$d_i = (x_i, y_i)$$ is a pair of input datum and label, the objective of training is to find the optimal model parameter $$\mathbf{\theta}$$ which maximizes the posterior $$p(\theta \| \mathcal{D})$$, i.e., 

$$\theta^* = \arg\max_{\theta} p(\theta \| \mathcal{D}).$$

The posterior can be expressed as $$p(\theta \| \mathcal{D}) \propto p(\theta) p(\mathcal{D} \| \theta) $$ (as known, prior $$\times$$ likelihood). Taking the logarithm of both sides, we get $$\log p(\theta \| \mathcal{D}) = \log p(\theta) + \log p(\mathcal{D} \| \theta) + Const$$. The first term of RHS, related to the prior distribution, is actually the regularization loss of model weights used in optimization. The second term is equal to the defined loss measuring how well the learned parameter $$\theta$$ fits the training dataset $$\mathcal{D}$$.

Given a new testing example $$x$$, the prediction of its output $$y$$ is given by 

$$p(y \| x, \mathcal{D}) = \int_{\theta} p(\theta \| \mathcal{D}) p(y \| x, \theta) d\theta = E_{p(\theta \| \mathcal{D})}[p(y \| x, \theta)].$$

Intuitively, it takes the average over the distribution of learned model parameters $$\theta$$. 
Traditionally, the prediction of $$y$$ is given by $$f(\theta^*, x)$$, where $$y^*$$ is deterministically determined by $$\theta^*$$. If we are aware of the distribution of $$p(\theta \| \mathcal{D})$$, the prediction can be averaged as

$$E(y\|x, D) = \int_{\theta} p(\theta \| \mathcal{D}) f(\theta, x) d\theta = E_{p(\theta \| \mathcal{D})}[f(\theta, x)].$$

Exact estimation of the distribution $$p(\theta \| \mathcal{D})$$ is intractable. A feasible solution is to apply Monte Carlo sampling to sample $$\{\theta_i \}_{i=1}^m$$ from the latent distribution $$p(\theta \| \mathcal{D})$$. Therefore the estimation of a new testing example is approximated as 

$$p(y \| x, \mathcal{D}) = \frac{1}{m} \sum_{i=1}^m p(y \| x, \theta_i).$$

In this way, MAP can be regarded as an over-confident approximation of the equation above which assumes

$$p(\theta \| \mathcal{D} ) = \begin{cases} 1,  \theta = \theta^* \\ 0 ,  \theta \neq \theta^* \end{cases}.$$

 


### Optimization

The update equation of Gradient Descent (GD) at iteration $$t$$ is expressed as 

$$\triangle \theta_t = \frac{\epsilon_t}{2} \left( \nabla \log p(\theta_t) + \sum_{i=1}^N \nabla \log p(d_i \| \theta_t) \right),$$

where $$\epsilon_t$$ is the learning rate or step size at iteration $$t$$ and we assume the training data $$\{d_i \}$$ follow iid.

In Stochastic Gradient Descent (SGD), the statistics of the whole dataset is approximated by a mini-batch. The update equation is written into 

$$\triangle \theta_t = \frac{\epsilon_t}{2} \left( \nabla \log p(\theta_t) + \frac{N}{n} \sum_{i=1}^n \nabla \log p(d_{ti} \| \theta_t) \right)$$

by considering a randomly-selected mini-batch $$\{d_{ti} \}_{i=1}^n$$.

### Dropout as Variational Inference

The main rule of **variational infernece** is to approximate the posterior distribution $$p(\theta \| \mathcal{D})$$ by a defined variational distribution $$q(\theta)$$ which is easy to evaluate. The error of approximation is measured by the KL diverence:

$$ KL(q(\theta) , p(\theta \| \mathcal{D}))$$

As stated in [10], minimize the KL divergence above is equivalent to minimize the upper bound:

$$ \begin{split} -E_{q(\theta)}[\log p(\mathcal{D} \| \theta)] + KL(q(\theta) , p(\theta) ) &= - \int q(\theta)\log p(\mathcal{D} \| \theta) d\theta + KL(q(\theta) , p(\theta) ) \\ &= -\sum_{n=1}^N \int q(\theta)\log p(d_i \| \theta) d\theta  + KL(q(\theta) , p(\theta) ) \end{split}.$$

In [8][9], the dropout is applied after each convolutional layer to simulate the variational distribution $$q(\theta)$$. 
If we assume $$q(\theta)$$ follows certain Gaussian Process (GP), "Dropout in NNs can be interpreted as an approximation to a well know Bayesian model - the Gaussian Process (GP)", as said in [9].
The parameters $$\theta$$ is composed of a set of parameters of the convolutional layers, i.e., $$\theta = \{ W_i, b_i \}_{i=1}^L$$. $$W_i$$ is a $$K_i \times K_{i-1}$$ matrix involved in the mapping from the output of the layer $$i-1$$ to the output of layer $$i$$, i.e., $$\sigma(W_i \cdot o_{i-1} + b_i)$$. Once dropout is applied, the components of the output of the layer $$i-1$$ are ramdoms set to zero with probability $$p_i$$. Therefore, the distribution of $$W_i$$ follows

$$ W_i = M_i \cdot diag([z_{i,j}]_{j=1}^{K_{i-1}}), $$

$$ z_{i,j} \sim Bernoulli(p_i).$$

Herein $$M_i$$ and $$b_i$$are variational parameters to be inferred. In this way, the variational distribution $$q(\theta)$$ is defined through dropout. Then the integeral $$\int q(\theta)\log p(d_i \| \theta) d\theta$$ is approximated by Monte Carlo with a single sample $$\theta_n \sim q(\theta)$$ to get an unbiased estimate $$\log p(d_i \| \theta_n)$$.

In [8], the term $$KL(q(\theta) , p(\theta) )$$ is approximated by $$\lambda \sum_{i=1}^L (\|W_i \|_2^2 + \|b_i \|_2^2)$$. Therefore, the error of KL divergence is written into 

$$ KL(q(\theta) , p(\theta \| \mathcal{D})) = -\sum_{n=1}^N \log p(d_i \| \theta) + \lambda \sum_{i=1}^L (\|W_i \|_2^2 + \|b_i \|_2^2), $$

which actually has the equivallent objective of learning Dropout Neural Networks. In other words, inferring the posterior distribution of $$p(\theta \| \mathcal{D})$$ via a Bayesian Neural Network is equivalent to learning variational parameters $$M_i$$ and $$b_i$$ via a Dropout Neural Network.





