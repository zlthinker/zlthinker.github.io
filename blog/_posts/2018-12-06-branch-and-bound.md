---
title: Branch and Bound
updated: 2018-12-16 11:00
---


* TOC
{:toc}

## Introduction

> Branch and bound is the most commonly used tool for solving NP-hard optimization problems.

$$f: \mathcal{R}^m \arrow \mathcal{R} $$ is the objective function defined over a m-dimensional domain $$\mathcal{Q}_{init}$$. We aim to find the global optimal value $$f^* = inf_{x \in \mathcal{Q}_{init}} f(x) = \Phi_{min}(\mathcal{Q}_{init})$$.

For any subspace $$\mathcal{Q} \in \mathcal{Q}_{init}$$, we use lower and upper bound functions $$\Phi_{lb}$$ and $$\Phi_{ub}$$ that satisfy

$$\Phi_{lb}(\mathcal{Q}) < \Phi_{min}(\mathcal{Q}) < \Phi_{ub}(\mathcal{Q}).$$

Then, the Branch and Bound algorithm takes steps as follows:

1. compute lower and upper bounds on $$f^*$$
  * set $$L_1 = \Phi_{lb}(\mathcal{Q}_{init})$$ and $$U_1 = \Phi_{ub}(\mathcal{Q}_{init})$$
  * terminate if $$U_1 - L1 \leq \sigma$$


## Reference

[Branch and Bound Methods](https://web.stanford.edu/class/ee364b/lectures/bb_slides.pdf)
