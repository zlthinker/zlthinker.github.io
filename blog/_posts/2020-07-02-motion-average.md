---
title: Motion Average
updated: 2020-07-02 20:00
---

* TOC
{:toc}

## Rotation average

The rotation averaging algorithms are based on the equations below:

$$\mathbf{R}_{ij} = \mathbf{R}_j \mathbf{R}_i^{-1}, \;\;\; \forall \{i, j \} \in \mathcal{E}.$$

The objective of rotation averaging is thus defined as 

$$\min_{\mathbf{R}_i, \mathbf{R}_j} \sum_{\{i, j \} \in \mathcal{E}} \| \mathbf{R}_j^{-1} \mathbf{R}_{ij} \mathbf{R}_i  \| .$$

An iterative step for $$\mathbf{R}_i$$ and $$\mathbf{R}_j$$ can be computed by 

$$\mathbf{R}_{ij} = (\mathbf{R}_j \Delta\mathbf{R}_j) (\mathbf{R}_i \Delta \mathbf{R}_i)^{-1},$$

$$\mathbf{R}_j^{-1} \mathbf{R}_{ij} \mathbf{R}_i = \Delta\mathbf{R}_j \Delta\mathbf{R}_i^{-1}.$$

Let $$e^{\omega_{ij}} = \mathbf{R}_j^{-1} \mathbf{R}_{ij} \mathbf{R}_i$$, $$e^{\Delta \omega_j} = \Delta\mathbf{R}_j$$, $$e^{\Delta \omega_i} = \Delta\mathbf{R}_i$$, then

$$e^{\Delta\omega_{ij}} = e^{\Delta \omega_j} e^{-\Delta \omega_i}.$$

When $$e^{\Delta \omega_j}$$ and $$e^{\Delta \omega_i}$$ are close to identity, we can take the first order approximation of the solution of **Baker–Campbell–Hausdorff formula** to give the linear equation:

$$\Delta\omega_{ij} = \Delta \omega_j - \Delta \omega_i.$$

By aggregating such equations, the iteration steps of all the global rotations can be solved by 

$$\mathbf{A} \Delta \omega_{global} = \Delta \omega_{rel},$$

where $$\mathbf{A}$$ consists of a $$\mathbf{I}_3$$ and a $$-\mathbf{I}_3$$ on each row for each camera edge.