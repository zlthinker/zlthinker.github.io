---
title: Image Localization
updated: 2018-03-16 20:00
category: 
tag:
---

* TOC
{:toc}

# Related works

* Tutorial
	* [Learning-based Visual Localization](https://alexgkendall.com/media/presentations/lsvpr_2017_cvpr_tutorial_alex_kendall.pdf)

* Scene coordinate regression
	* [Scene Coordinate Regression Forests for Camera Relocalization in RGB-D Images](http://openaccess.thecvf.com/content_cvpr_2013/papers/Shotton_Scene_Coordinate_Regression_2013_CVPR_paper.pdf)
	* [Backtracking regression forests for accurate camera relocalization](https://arxiv.org/pdf/1710.07965)
	* [Random forests versus Neural Networks—What's best for camera localization?](http://cvlab-dresden.de/HTML/people/alexander_krull/publications/ICRA_2017.pdf)
	* [DSAC-Differentiable RANSAC for camera localization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Brachmann_DSAC_-_Differentiable_CVPR_2017_paper.pdf)

* Pose regression
	* [Posenet: A convolutional network for real-time 6-dof camera relocalization](https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Kendall_PoseNet_A_Convolutional_ICCV_2015_paper.pdf?utm_source=mandiner&utm_medium=link&utm_campaign=mandiner_digit_201512)
	* [Image-based localization using LSTMs for structured feature correlation](https://arxiv.org/pdf/1611.07890.pdf)
	* [Geometric loss functions for camera pose regression with deep learning](http://openaccess.thecvf.com/content_cvpr_2017/papers/Kendall_Geometric_Loss_Functions_CVPR_2017_paper.pdf)

* Temporal
	* [VidLoc: A deep spatio-temporal model for 6-DoF video-clip relocalization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Clark_VidLoc_A_Deep_CVPR_2017_paper.pdf)
	* [Deep Feature Flow for Video Recognition](http://openaccess.thecvf.com/content_cvpr_2017/papers/Zhu_Deep_Feature_Flow_CVPR_2017_paper.pdf)

* Traditional
	* [Efficient & effective prioritized matching for large-scale image-based localization](http://ieeexplore.ieee.org/abstract/document/7572201/)

* Feature selection
	* [Avoiding confusing features in place recognition](http://www.di.ens.fr/~josef/publications/knopp10.pdf): (ECCV 2010)
	
* State-of-the-art
	* [MapNet: Geometry-Aware Learning of Maps for Camera Localization](https://arxiv.org/pdf/1712.03342.pdf): Leverage the full map (CVPR2018, spotlight)
	* [Learning Less is More – 6D Camera Localization via 3D Surface Regression](https://arxiv.org/pdf/1711.10228.pdf): Align to surface
	* [X-View: Graph-Based Semantic Multi-View Localization](https://arxiv.org/pdf/1709.09905.pdf): Use segmentation
	* [Semantic Visual Localization](https://arxiv.org/pdf/1712.05773.pdf): Use segmentation (CVPR2018, ETH)
	* [Benchmarking 6DOF Outdoor Visual Localization in Changing Conditions](https://arxiv.org/pdf/1707.09092.pdf): Outdoor benchmark (CVPR2018, spotlight, ETH)
	* [InLoc: Indoor Visual Localization with Dense Matching and View Synthesis](): (CVPR2018, spotlight, ETH)
	* [Full-Frame Scene Coordinate Regression for Image-Based Localization](https://arxiv.org/pdf/1802.03237.pdf): The same idea as mine


# PnP algorithm

> "Perspective-n-Point (PnP) is the problem of estimating the pose of a calibrated camera given a set of n 3D points in the world and their corresponding 2D projections in the image." from wikipedia

### P3P

P3P is the PnP problem in its minimal form. Let P be the camera center, and $$A$$, $$B$$, $$C$$ be the 3D points in world frame. Let $$\|PA\|=X$$, $$\|PB\|=Y$$, $$\|PC\|=Z$$, $$\alpha=\angle BPC$$, $$\beta=\angle APC$$, $$\gamma=\angle APB$$, $$p=2cos\alpha$$, $$q=2cos\beta$$, $$r=2cos\gamma$$, $$\|AB\| = c'$$, $$\|BC\| = a'$$, $$\|AC\| = b'$$, as illustrated in Figure 1.

![P3P]({{site.baseurl}}/images/p3p.png)
**<center>Figure1. The P3P problem </center>**

Then the P3P equation system is derived based on the law of cosines (余弦定理):

$$\begin{cases} Y^2 + Z^2 - YZp - a'^2 = 0 \\ Z^2 + X^2 - XZq - b'^2 = 0  \\ X^2 + Y^2 - YXr - c'^2 = 0, \end{cases}$$

where $$X$$, $$Y$$, $$Z$$ are variables to be determined.

To simply the equation system, we let $$X= xZ$$, $$Y = yZ$$, $$c'= \sqrt{v}Z$$, $$a'=\sqrt{av}Z$$, $$b'=\sqrt{bv}Z$$, then the equation system is turned into

$$\begin{cases} y^2 + 1 - py - av = 0 \\ x^2 + 1 - qx -bv = 0 \\ x^2 + y^2 - xyr - v = 0, \end{cases}$$

where $$x$$, $$y$$, $$v$$ are variables to be determined.

Eliminating $$v = x^2 + y^2 - xyr$$, we have

$$\begin{cases} ax^2 + (a-1)y^2 - ar xy + py =1 \\ (b-1)x^2 + b y^2 - brxy + qx = 1.  \end{cases}$$

Essentially, the two quatratic equations define two conics, which may have infinite or at most four intersections. Therefore, *the P3P problem has either an infinite number of solutions or at most four physical solutions*.

### P4P, P5P, PnP

Using more than 3 points provides redundant data for the solution of pose estimation. The linear algorithm is generally used [1]. The $$n$$ points can form $$\frac{n(n-1)}{2}$$ triangles and thus derive $$\frac{n(n-1)}{2}$$ second degree polynomial equations based on 余弦定理. By eliminating the set of variables into a single one, we still have $$\frac{n(n-1)}{2} - (n-1) = \frac{(n-1)(n-2)}{2}$$ fourth degree polynomial equations. When n = 5, we have the homogeneous equation system:

![P5P]({{site.baseurl}}/images/p5p.png)

The overdermined equation system can be easily solved by SVD decomposition, where $$t_5$$ equals to the right singular vector corresponding to the smallest singular value. The solution can be generalized to cases when $$n>5$$.

When $$n=4$$, things are a little bit more complicated since the equation system above is underdetermined. We refer the readers to [1] for details.

# Scene COordinate REgression Network (SCORE-Net)

### Network architecture

1. [Globally and Locally Consistent Image Completion](http://hi.cs.waseda.ac.jp/~iizuka/projects/completion/en/)

* Dilated convolution (空洞卷积)

Fully convolutional betwork, dilated conv




# Reference
[1] [Linear N-Point Camera Pose Determination](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=784291)





