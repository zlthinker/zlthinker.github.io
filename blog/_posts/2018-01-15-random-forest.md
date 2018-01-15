---
title: Random Forest
updated: 2018-01-15 10:00
category: 
tag:
---

* TOC
{:toc}


>This post is a recap of the famous random forest algorithm proposed by [Leo Breiman](https://scholar.google.com.hk/citations?user=mXSv_1UAAAAJ&hl=zh-CN). And it is also my first extended study of the new year 2018. Keep evolving!

# Introduction

A random forest is an classifier consisting of an ensemble of tree-structured classifiers $$h(\mathbf{x}, \Theta_k) (k=1, 2, ...)$$ where $$\Theta_k$$ are iid, and each tree casts a unit vote of classification for input feature vector $$\mathbf{x}$$. The final classfication is yielded by the majority vote of all tree classifiers.

# Backgroud & Preliminary

### Generalization error

#### Margin function

The margin function is defined as $$mg(\mathbf{X}, Y) = E_k(I(h_k(\mathbf{X}) = Y)) - \max_{j\neq Y} E_k(I(h_k(\mathbf{X}) = j))$$,

where $$h_k(.)$$ is tree classifiers and $$I(.)$$ is indicator function. Quoted from [1], "the margin measures the extent to which the average
number of votes at $$\mathbf{X}$$, $$Y$$ for the right class exceeds the average vote for any other class. The larger the margin, the more **confidence** in the classification."

#### Generalization error
The generalization error is defined as 

$$PE^* = P_{\mathbf{X}, Y}(mg(\mathbf{X}, Y) < 0)$$.

#### Strength and correlation

"The two ingredients involved in the generalization error for random forests are **the strength of the individual classifiers in the forest** and **the correlation between them**."

Skipping the mathematical deduction in [1], strength is related to the accuracy of the set of tree classifiers and correlation is the correlation of margin function across the tree ensemble.

The generalization error is bounded by $$PE^* \leq \rho (1-s^2) / s^2$$. To make smaller generalization error, we encourage larger strength of tree classifiers and smaller correlation (i.e., intuitively the tree classifiers should have similar accuracy.).

### Out-of-bag error
When training tree classifiers, we sample data from training sets randomly as *bootstrap dataset*. The bootstrap dataset may contain duplicated records, yet as well, may miss some records. Then out-of-bag error is obtained by testing the missed records on trained tree classifiers and measured by error rate.

# Strategies

### Random forests using random input selection

At each node, select a small group of input variables at random.

### Random forests using linear combination if inputs

At a given node, $$L$$ variables are randomly selected and added together with coefficiens that are uniform random numbers on $$[-1, 1]$$. $$F$$ linear combinations are generated, and then a search is made over these for the best split.

# Reference

[1] [Random Forest](https://link.springer.com/content/pdf/10.1023%2FA%3A1010933404324.pdf)
