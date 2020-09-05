---
title: Motion Average
updated: 2020-07-02 20:00
---

* TOC
{:toc}

## Rotation Averaging

Since the camera rotations are independent of their translations in the perspective of motion averaging, the rotation averaging problem can be solved prior to translation averaging to attain the orientations of cameras.
The rotation averaging algorithms are based on the equations below:

$$\mathbf{R}_{ij} = \mathbf{R}_j \mathbf{R}_i^{-1}, \;\;\; \forall \{i, j \} \in \mathcal{E}.$$

The objective of rotation averaging is thus defined as 

$$\min_{\mathbf{R}_i, \mathbf{R}_j} \sum_{\{i, j \} \in \mathcal{E}} \| \mathbf{R}_j^{-1} \mathbf{R}_{ij} \mathbf{R}_i  \| .$$

An iterative step for $$\mathbf{R}_i$$ and $$\mathbf{R}_j$$ can be computed by 

$$\mathbf{R}_{ij} = (\mathbf{R}_j \Delta\mathbf{R}_j) (\mathbf{R}_i \Delta \mathbf{R}_i)^{-1},$$

$$\mathbf{R}_j^{-1} \mathbf{R}_{ij} \mathbf{R}_i = \Delta\mathbf{R}_j \Delta\mathbf{R}_i^{-1}.$$

Let $$e^{\Delta\omega_{ij}} = \mathbf{R}_j^{-1} \mathbf{R}_{ij} \mathbf{R}_i$$, $$e^{\Delta \omega_j} = \Delta\mathbf{R}_j$$, $$e^{\Delta \omega_i} = \Delta\mathbf{R}_i$$, then

$$e^{\Delta\omega_{ij}} = e^{\Delta \omega_j} e^{-\Delta \omega_i}.$$

When $$e^{\Delta \omega_j}$$ and $$e^{\Delta \omega_i}$$ are close to identity, we can take the first order approximation of the solution of **Baker–Campbell–Hausdorff formula** to give the linear equation:

$$\Delta\omega_{ij} = \Delta \omega_j - \Delta \omega_i.$$

By aggregating such equations, the iteration steps of all the global rotations can be solved by 

$$\mathbf{A} \Delta \omega_{global} = \Delta \omega_{rel},$$

where $$\mathbf{A}$$ consists of a $$\mathbf{I}_3$$ and a $$-\mathbf{I}_3$$ on each row for each camera edge.

## Translation Averaging

After rotation averaging is solved, translation averaging can be addressed by the following equation:

$$\lambda_{ij}\mathbf{t}_{ij} = \mathbf{R}_j (\mathbf{c}_i - \mathbf{c}_j),$$

where $$\|\mathbf{t}_{ij} \|_2 = 1$$ and $$\lambda_{ij}$$ is the unknown baseline length between camera centers $$\mathbf{c}_i$$ and $$\mathbf{c}_j$$. The objective of translation averaging is defined as 

$$\min_{\mathbf{c}_i, \mathbf{c}_j} \sum_{\{i, j \} \in \mathcal{E}} \| \mathbf{R}_j^T \mathbf{t}_{ij} - \frac{\mathbf{c}_i - \mathbf{c}_j}{\| \mathbf{c}_i - \mathbf{c}_j\|_2}  \| .$$

## Reference

* [Efficient and Robust Large-Scale Rotation Averaging](https://www.cv-foundation.org/openaccess/content_iccv_2013/papers/Chatterjee_Efficient_and_Robust_2013_ICCV_paper.pdf)
* [Robust Global Translations with 1DSfM](https://research.cs.cornell.edu/1dsfm/docs/1DSfM_ECCV14.pdf)