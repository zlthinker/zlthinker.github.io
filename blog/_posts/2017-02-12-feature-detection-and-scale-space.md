---
title: Feature Detection And Scale Space
updated: 2017-02-12 13:50
category: CV
tags: [feature, scale space]
---

## Feature Detection at a given scale in scale-space

Consider scale-space representation of image $$I(x, y)$$:

$$L(x, y, \sigma) = G(x, y, \sigma) * I(x, y),$$

where $$\sigma \in  \mathbb{R}_+$$ is the scale parameter, and $$G(x, y, \sigma)$$ is the two-dimensional variable-scale Gaussian kernel written as

$${G(x, y, \sigma) = \frac{1}{2\pi \sigma^2} e^{ -(x^2 + y^2) / 2 \sigma ^ 2}}.$$

The 2-jet at a given image point and a given scale contains the partial derivatives

$$(L_x, L_y, L_{xx}, L_{xy}, L_{yy}).$$

From the five components in the 2-jet, four differential invariants can be constructed, which are invariant to local rotations; the gradient magnitude $$\|\nabla L\|$$, the Laplacian $$\nabla ^2 L$$, the determinant of the Hessian $$det\mathcal{H}L$$ and the rescaled level curve curvature $$\tilde{\mathbb{k}}(L)$$:

* $$\|\nabla L\|^2 = L_x^2 + L_y^2$$,
* $$\nabla ^2 L = L_{xx} + L_{yy}$$,
* $$det\mathcal{H}L = L_{xx}L_{yy}-L_{xy}^2$$,
* $$\tilde{\mathcal{k}}(L)=L_x^2L_{yy},+L_y^2L_{xx}-2L_xL_yL_{xy}$$.

These operators can detect different types of local structures:

* A single-scale blob detector can be expressed from the minima and the maxima of the Laplacian response $$\nabla^2L$$.
* An affine covariant blob detector or a saddle detector can be expressed from the maxima and the minima of the determinant of the Hessian $$det \mathcal{H}L$$.
* A straightforward and affine covariant corner detector can be expressed from the maxima and minima of the rescaled level curve curvature $$\tilde{\mathbb{k}}(L)$$.

Some conclusions drawn by SURF:
* Hessian-based detectors are more stable and repeatable than Harris-based counterparts.
* Using the determinant of the Hessian matrix rather than its trace (the Laplacian) seems advantageous, as it fires less elongated, ill-localised structures.








## Reference
[Lindeberg, Tony. "Scale-space."](https://www.researchgate.net/profile/Tony_Lindeberg/publication/227992111_Scale_space/links/0fcfd504211a11bd1b000000.pdf)

[The impressive graphs of Gaussian derivative functions](http://campar.in.tum.de/Chair/HaukeHeibelGaussianDerivatives)
