---
title: Monte Carlo Methods
updated: 2018-04-22 20:00
---

* TOC
{:toc}

# Overview


# Draw random samples uniformly over the cumulative distribution function (CDF)

# Markov Chain Monte-Carlo (MCMC)

Markov chain Monte-Carlo uses a homogeneous, ergodic and reversible (see [1] for the meaning of the adjectives) Markov chain to generate consistent samples draw from any given distribution.

### Glossary

* Stationary. A markov chain is stationary if $$P(x_{t+1} \| x_t)$$ does not depend on $$t$$.

* Ergodic. A markov chain is ergodic if $$x_t$$ converges to a distribution not matter what the starting value is.

* Irreducible. A markov chain is irreducible if the samples of the chain can move all over the state space, i.e., can eventually reach any region of the state space, no matter its initial value.

* Aperiodic. Sampling不会周期性地回到某个点.

### Metropolis–Hastings algorithm

1. Given $$X_t=x_t$$, generate $$Y_t \sim q(y \| x_t)$$, where the conditional density $$q(y \| x)$$ is called **proposal**, which is used to generate next sample.

2. Take $$X^{t+1} = Y^t$$ with probability $$\rho(x^t, Y^t)$$  or $$X^{t+1} = x^t$$ with probability $$1 - \rho(x^t, Y^t)$$, where $$\rho(x, y) = \min\{ \frac{\pi(y)}{\pi(x)} \frac{q(x\|y)}{q(y\|x)}, 1 \}$$. Here we do not need to obtain the exact value of $$\pi(y)$$ and $$\pi(x)$$, but more easily, their ratio.

(The proposal $$q(y \| x)$$ must be carefully chosen to ensure that the samples of MCMC converge to the target distribution $$\pi(x)$$.)


### Gibbs sampling

Gibbs sampling is a multivariate version of Metropolis–Hastings algorithm. Here we consider the multivariate distribution of $$\mathbf{X} = (x_1, ..., x_n)$$.

In the transition step （step 2 above）, we generate next proposal $$\mathbf{X}^{t+1}$$ from $$\mathbf{X}^t$$ based on the conditional density $$q(x_j^{t+1} \| x_1^{t+1}, ..., x_{j-1}^{t+1}, x_{j+1}^{t}, ..., x_n^{t})$$, since the variables are correlated. It means the $$j$$th component $$x_j^{t+1}$$ is conditioned on all the other components at hand.

### Metropolis within Gibbs (Hybrid Gibbs)

Metropolis within Gibbs uses the Metropolis Hasting proposal for each component of $$\mathbf{X}^{t+1}$$.

[http://www.chadfulton.com/fulton_statsmodels_2017/sections/5-posterior_simulation.html](http://www.chadfulton.com/fulton_statsmodels_2017/sections/5-posterior_simulation.html)


### Adaptive rejection sampling

The main idea is to asymptotically optimize the transition function to minimize the rejection rate.

### Cluster sampling

It split an image into individual clusters, which is useful for image segmentation.

[Interpretations of Cluster Sampling by Swendsen-Wang](https://pdfs.semanticscholar.org/4256/35f354bab06f3811eb0093d67ba37445fe12.pdf)

### Random Walk
[Google Page Rank via random walk](http://www.cs.bu.edu/~snyder/cs109/CS109.Lect15.pdf)

## Convergence

The structural properties required for a constructed chain to converge appropriately are well understood: essentially irreducibility and aperiodicity.

















# Reference
[1] [Lecture notes](https://www.unige.ch/sciences/astro/files/2713/8971/4086/3_Paltani_MonteCarlo.pdf)

[2] [Introduction to Markov Chain Monte Carlo](http://www.mcmchandbook.net/HandbookChapter1.pdf)

[3] Very useful for the newers: [A simple introduction to Markov Chain Monte–Carlo sampling](https://link.springer.com/content/pdf/10.3758%2Fs13423-016-1015-8.pdf)

