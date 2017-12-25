---
title: Convex Optimization
updated: 2017-02-07 15:30
tags: [convex, math]
category: Convex Optimization
---

### Dual cones

Dual cone of cone $$K$$:

$$K^* = \{y | y^T x \geq 0 \ for\ all\ x \in K \}$$


### Common convex function

* Affine function:  $$\;f(X)=tr(A^T X)+b \in R$$
* All norm functions

### Convex Optimization Problems

#### LP
#### QP
#### QCQP
#### SOCP
#### GP (Geometric Programming)
#### SDP (Semidefinite)


### Notes

#### [Material on Gradient Descent Method](http://sebastianruder.com/optimizing-gradient-descent/index.html#stochasticgradientdescent)

1. There is a number of challenges to mini-batch gradient descent.
	1. The same learning rate applies to all parameter updates. If our data is sparse and our features have very different frequencies, we might not want to update all of them to the same extent, but perform a larger update for rarely occurring features.
	2. SGD are prone to get trapped in saddle points. The saddle points are usually surrounded by a plateau of the same error, which makes it notoriously hard for SGD to escape, as the gradient is close to zero in all dimensions.
