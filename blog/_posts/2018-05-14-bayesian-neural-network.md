---
title: Bayesian Neural Network
updated: 2018-05-14 14:00
---

* TOC
{:toc}

### Related Works

* [Coursera: Bayesian Neural Networks](https://www.coursera.org/learn/bayesian-methods-in-machine-learning/lecture/HI8ta/bayesian-neural-networks)
* SGLD: [Bayesian Learning via Stochastic Gradient Langevin Dynamics](https://www.ics.uci.edu/~welling/publications/papers/stoclangevin_v6.pdf): (ICML2011)
* preconditioned SGLD: [Preconditioned Stochastic Gradient Langevin Dynamics for Deep Neural Networks](http://people.ee.duke.edu/~lcarin/aaai_psgld_final.pdf): (AAAI 2016)
* [Bayesian Dark Knowledge](https://arxiv.org/pdf/1506.04416.pdf): (NIPS2015)
* [Modelling Uncertainty in Deep Learning for Camera Relocalization](https://arxiv.org/pdf/1509.05909.pdf)
* [DSAC-Differentiable RANSAC for camera localization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Brachmann_DSAC_-_Differentiable_CVPR_2017_paper.pdf)
* [Learning Weight Uncertainty with Stochastic Gradient MCMC for Shape Classification](http://openaccess.thecvf.com/content_cvpr_2016/papers/Li_Learning_Weight_Uncertainty_CVPR_2016_paper.pdf)


### Formulation

Given a training dataset $$\mathcal{D} = \{d_i\}_{i=1}^N = \{(x_i, y_i)\}_{i=1}^N$$ where $$d_i = (x_i, y_i)$$ is a pair of input datum and label, the objective of training is to find the optimal model parameter $$\mathbf{\theta}$$ which maximizes the posterior $$p(\theta \| \mathcal{D})$$, i.e., 

$$\theta^* = \arg\max_{\theta} p(\theta \| \mathcal{D}).$$

The posterior can be expressed as $$p(\theta \| \mathcal{D}) \propto p(\theta) p(\mathcal{D} \| \theta) $$. Taking the logarithm of both sides, we get $$\log p(\theta \| \mathcal{D}) = \log p(\theta) + \log p(\mathcal{D} \| \theta) + Const$$. The first term of RHS, related to the prior distribution, is actually the regularization loss of model weights used in optimization. The second term is equal to the defined loss measuring how well the learned parameter $$\theta$$ fits the training dataset $$\mathcal{D}$$.

Given a new testing example $$x$$, the prediction of its output $$y$$ is given by 

$$p(y \| x, \mathcal{D}) = \int_{\theta} p(\theta \| \mathcal{D}) p(y \| x, \theta) d\theta = E_{p(\theta \| \mathcal{D})}[p(y \| x, \theta)].$$

Intuitively, it takes the average over the distribution of learned model parameters $$\theta$$. Exact estimation of the distribution $$p(\theta \| \mathcal{D})$$ is intractable. A feasible solution is to apply Monte Carlo sampling to sample $$\{\theta_i \}_{i=1}^m$$ from the latent distribution $$p(\theta \| \mathcal{D})$$. Therefore the estimation of a new testing example is approximated as 

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


### TODO
* Variational inference





