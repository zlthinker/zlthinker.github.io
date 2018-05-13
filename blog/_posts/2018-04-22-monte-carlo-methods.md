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

### Hamiltonian Monte Carlo (Hybrid Monte Carlo)

Hamiltonian dynamics是一种物理学上的动态过程,描述了一个有质量的物体的势能和动能的相互转化.想像一个固定在弹簧末端的小球,在小球左右来回摆动时,小球的动能和弹簧的弹性势能相互转化.其中弹性势能的计算取决于小球的位置$$x$$,动能的计算取决于小球的动量$$p=mv$$,这两个值$$(x,p)$$定义了Hamiltonian dynamics动态过程的一个坐标.随着时间的变化,每一个时间点的坐标可以通过积分的方法计算得到.例如,Leap Fog Method就是一个将连续时间离散为很小的时间段计算每个离散时间点$$(x,p)$$的方法.

在Hamiltonian Monte Carlo(HMC)采样中,Hamiltonian dynamics被用来生成下一个proposal - $$x$$.此时的energy funciton定义为势能和动能之和,即$$H(x,p)=U(x)+K(p)$$.采样点$$(x,p)$$的概率为$$P(x,p)=\frac{1}{Z}e^{-U(x)-K(p)}$$.整个proposal的过程如下:

1. 在第$$t$$步,给定一个采样$$x_t$$,从变量$$p$$的标准分布$$P(p)$$(canonical distribution,人为定义)中随机采样一个值$$p_t$$构成坐标$$(x_t, p_t)$$.
2. 跑$$L$$步的Leap Fog Method来模拟Hamiltonian dynamics动态过程,到达结束位置$$(x*, p*)$$
3. 比较$$H(x_t,p_t)$$和$$H(x*,p*)$$的相对大小,按照Metropolis–Hastings algorithm的方法accept或者reject,从而得到$$(x_{t+1}, p_{t+1})$$.

在每一步生成proposal时,因为引入了随机动量$$p_t$$,所以HMC可以有效地探索整个分布空间.
因为HMC相邻两个采样的距离比Metropolis–Hastings algorithm更大,且相关性(autocorrelation)更小,所以有更快的收敛效果.

详细讲解和示例见:[https://theclevermachine.wordpress.com/2012/11/18/mcmc-hamiltonian-monte-carlo-a-k-a-hybrid-monte-carlo/](https://theclevermachine.wordpress.com/2012/11/18/mcmc-hamiltonian-monte-carlo-a-k-a-hybrid-monte-carlo/)




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

