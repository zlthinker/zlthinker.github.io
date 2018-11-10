---
title: Gaussian Process Regression
updated: 2018-11-08 14:00
---


* TOC
{:toc}

## Gaussian Process

A Gaussian process is a collection of **infinite** random variables $$\{f(\mathbf{x}): \mathbf{x} \in \mathcal{X} \}$$, any finite number of which have a joint Gaussian distribution. It is completely specified by its mean function $$m(\mathbf{x}) = E(f(\mathbf{x}))$$ and covariance function $$k(\mathbf{x}, \mathbf{x}') = E( (f(\mathbf{x})-m(\mathbf{x}))(f(\mathbf{x}')-m(\mathbf{x}')) )$$.

Consider a simple example of a zero-mean Gaussian process,

$$f(.) \sim \mathcal{GP}(0, k(.,.)),$$

where $$k(x, x') = exp(-\frac{1}{2\tau ^2}\|x-x'\|^2)$$ is the squared exponential kernel function. $$f(x)$$ and $$f(x')$$ will tend to have high covariance (i.e., nearby in output space) if $$x$$ and $$x'$$ are nearby in the input space, and low covariance in the reverse case. It means that functions drawn from this Gaussian process will tend to be "locally smooth", as shown in figure below. As the bandwidth parameter $$\tau$$ increases (Panel a to b to c), points located far away will have higher correlations, and hence the function tends to be smoother overall.

![]({{site.baseurl}}/images/gaussian_process.png)

## Gaussian Process Regression

Given a set of training data $$D = \{(X,Y)\} = \{(x_i, y_i) \}$$, $$i \in [1,n]$$, we would like to fit a model to it ($$y=f(x)$$), so that a new output $$y_{n+1}$$ can be predicted for the new input set $$X^*$$. Sometimes, it is hard to claim $$f(x)$$ to some specific models. A Gaussian process is clearly preferable, because it can represent $$f(x)$$ obliquely but rigorously by letting the data "speak" for themselves. To make prediction for the new input $$x_{n+1}$$ is equivalent to drawing function $$f$$ from the posterior distribution $$p(f\|D)$$.

For ease of elaboration, we still assume a prior Gaussian process with zero mean and squared exponential kernel for regression as above. By definition, previous outputs and to-be-determined outputs folow a joint multivariate normal distribution:

$$\begin{matrix} Y \\ Y^* \end{matrix}$$


![]({{site.baseurl}}/images/gaussian_process_regression.png)


## Reference

[Gaussian processes](http://cs229.stanford.edu/section/cs229-gaussian_processes.pdf)

[Gaussian Processes for Dummies](http://katbailey.github.io/post/gaussian-processes-for-dummies/)

[Gaussian Processes for Machine Learning (MIT)](http://www.gaussianprocess.org/gpml/chapters/RW2.pdf)
