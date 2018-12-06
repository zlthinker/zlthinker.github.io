---
title: Branch and Bound
updated: 2018-12-16 11:00
---


* TOC
{:toc}

## Related works

[Go-ICP: A Globally Optimal Solution to 3D ICP Point-Set Registration](https://arxiv.org/pdf/1605.03344.pdf)

## Introduction

> Branch and bound is the most commonly used tool for solving NP-hard optimization problems.

$$f: \mathcal{R}^m \rightarrow \mathcal{R} $$ is the objective function defined over an m-dimensional domain $$\mathcal{Q}_{init}$$. We aim to find the global optimal value $$f^* = inf_{x \in \mathcal{Q}_{init}} f(x) = \Phi_{min}(\mathcal{Q}_{init})$$.

For any subspace $$\mathcal{Q} \in \mathcal{Q}_{init}$$, we use lower and upper bound functions $$\Phi_{lb}$$ and $$\Phi_{ub}$$ that satisfy

$$\Phi_{lb}(\mathcal{Q}) < \Phi_{min}(\mathcal{Q}) < \Phi_{ub}(\mathcal{Q}).$$

Then, the Branch and Bound algorithm takes steps as follows:

1. compute lower and upper bounds on $$f^*$$
  * set $$L = \Phi_{lb}(\mathcal{Q}_{init})$$ and $$U = \Phi_{ub}(\mathcal{Q}_{init})$$
  * terminate if $$U - L \leq \sigma$$, (tight enough)

2. partition $$\mathcal{Q}_{init}$$ into two subspaces $$\mathcal{Q}_{init} = \mathcal{Q}_1 \cup \mathcal{Q}_2$$

3. compute $$\Phi_{lb}(\mathcal{Q}_i)$$ and $$\Phi_{ub}(\mathcal{Q}_i)$$ , $$i=1,2$$

4. update lower and upper bounds on $$f^*$$
  * $$L = \min\{\Phi_{lb}(\mathcal{Q}_1), \Phi_{lb}(\mathcal{Q}_2) \}$$
  * $$U = \min\{\Phi_{ub}(\mathcal{Q}_1), \Phi_{ub}(\mathcal{Q}_2) \}$$
  * terminate if $$U - L \leq \sigma$$

5. partition $$\mathcal{Q}_1$$ and $$\mathcal{Q}_2$$, and repeat 3 and 4

## Reference

[Branch and Bound Methods](https://web.stanford.edu/class/ee364b/lectures/bb_slides.pdf)
