---
title: Gaussian Process Regression
updated: 2018-11-08 14:00
---


* TOC
{:toc}

## Related Works

[Gaussian processes](http://cs229.stanford.edu/section/cs229-gaussian_processes.pdf)
[Gaussian Processes for Dummies](http://katbailey.github.io/post/gaussian-processes-for-dummies/)
[Gaussian Processes for Machine Learning (MIT)](http://www.gaussianprocess.org/gpml/chapters/RW2.pdf)

## Introduction

A Gaussian process is a collection of **infinite** random variables $$\{f(\mathbf{x}): \mathbf{x} \in \mathcal{X} \}$$, any finite number of which have a joint Gaussian distribution. It is completely specified by its mean function $$m(\mathbf{x}) = E(f(\mathbf{x}))$$ and covariance function $$k(\mathbf{x}, \mathbf{x}') = E( (f(\mathbf{x})-m(\mathbf{x}))(f(\mathbf{x}')-m(\mathbf{x}')) )$$.

Consider a simple example of a zero-mean Gaussian process,

$$f(.) \sim \mathcal{GP}(0, k(.,.),$$

where $$k(x, x') = exp(-\frac{1}{2\tau^2}||x-x'||^2)$$ is the squared exponential kernel function. $$f(x)$$ and $$f(x')$$ will tend to have high covariance (i.e., nearby in output space) if $$x$$ and $$x'$$ are nearby in the input space, and low covariance in the reverse case.
