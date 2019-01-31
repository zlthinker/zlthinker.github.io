---
title: Point Cloud Registration
updated: 2019-01-14 10:00
---


* TOC
{:toc}

## DoF of transformations

|n-D Transformation|DoF|
|:----------------:|:-:|
|2D Euclidean      | 3 |
|2D Affine         | 6 |
|2D Projective     | 8 |
|3D Euclidean      | 6 |
|3D Similarity     | 7 |
|3D Affine         | 12|
|3D Projective     | 15|

## Related works

#### Feature-based

* [Fast Global Registration](http://qianyi.info/docs/papers/eccv16_registration.pdf)

#### ICP

* **经典** [Robust Registration of 2D and 3D Point Sets](https://www.robots.ox.ac.uk/~cvrg/michaelmas2004/fitzgibbon01c.pdf)
* [Colored-ICP](http://qianyi.info/docs/papers/iccv17_colored_registration.pdf)

#### Branch and bound

* [Go-ICP: A Globally Optimal Solution to 3D ICP Point-Set Registration](https://arxiv.org/pdf/1605.03344.pdf)
* [GOGMA: Globally-Optimal Gaussian Mixture Alignment](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Campbell_GOGMA_Globally-Optimal_Gaussian_CVPR_2016_paper.pdf)
* [Efficient Global Point Cloud Alignment using Bayesian Nonparametric Mixtures](http://openaccess.thecvf.com/content_cvpr_2017/papers/Straub_Efficient_Global_Point_CVPR_2017_paper.pdf) [[github]](https://github.com/jstraub/dpOptTrans)


#### Probabilistic Registration

* [**JRMPC** A Generative Model for the Joint Registration of
Multiple Point Sets](https://hal.archives-ouvertes.fr/hal-01019661v2/document)
* [**CPPSR** A Probabilistic Framework for Color-Based Point Set Registration](http://openaccess.thecvf.com/content_cvpr_2016/papers/Danelljan_A_Probabilistic_Framework_CVPR_2016_paper.pdf)
* [**FPPSR** Aligning the dissimilar: A probabilistic method for feature-based point set registration](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7899641&tag=1)
* [**DARE** Density Adaptive Point Set Registration](https://arxiv.org/pdf/1804.01495.pdf) [[github]](https://github.com/felja633/DARE)
