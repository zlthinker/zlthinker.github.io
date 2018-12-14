---
title: Image Localization
updated: 2018-03-16 20:00
category:
tag:
---

* TOC
{:toc}

# Related works

* Feature-Based
  * [From Coarse to Fine: Robust Hierarchical Localization at Large Scale](https://arxiv.org/pdf/1812.03506.pdf)

* Tutorial
	* [Learning-based Visual Localization](https://alexgkendall.com/media/presentations/lsvpr_2017_cvpr_tutorial_alex_kendall.pdf)

* Scene coordinate regression
	* [Scene Coordinate Regression Forests for Camera Relocalization in RGB-D Images](http://openaccess.thecvf.com/content_cvpr_2013/papers/Shotton_Scene_Coordinate_Regression_2013_CVPR_paper.pdf)
	* [Backtracking regression forests for accurate camera relocalization](https://arxiv.org/pdf/1710.07965)
	* [Random forests versus Neural Networks—What's best for camera localization?](http://cvlab-dresden.de/HTML/people/alexander_krull/publications/ICRA_2017.pdf)
	* **DSAC** [DSAC-Differentiable RANSAC for camera localization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Brachmann_DSAC_-_Differentiable_CVPR_2017_paper.pdf)
	* **DSAC++** [Learning Less is More – 6D Camera Localization via 3D Surface Regression](https://arxiv.org/pdf/1711.10228.pdf)
	* **DSAC++ angle loss** [Scene Coordinate Regression with Angle-Based Reprojection Loss for Camera Relocalization](https://arxiv.org/pdf/1808.04999.pdf)
	* **Dense regression:** [Full-Frame Scene Coordinate Regression for Image-Based Localization](https://arxiv.org/pdf/1802.03237.pdf): The same idea as mine
	* [Scene Coordinate and Correspondence Learning for Image-Based Localization](https://arxiv.org/pdf/1805.08443.pdf)

* Pose regression
	* [Posenet: A convolutional network for real-time 6-dof camera relocalization](https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Kendall_PoseNet_A_Convolutional_ICCV_2015_paper.pdf?utm_source=mandiner&utm_medium=link&utm_campaign=mandiner_digit_201512)
	* [Modelling Uncertainty in Deep Learning for Camera Relocalization](https://arxiv.org/pdf/1509.05909.pdf)
	* [Image-based localization using LSTMs for structured feature correlation](https://arxiv.org/pdf/1611.07890.pdf)
	* [Geometric loss functions for camera pose regression with deep learning](http://openaccess.thecvf.com/content_cvpr_2017/papers/Kendall_Geometric_Loss_Functions_CVPR_2017_paper.pdf)
	* [RelocNet: Continuous Metric Learning Relocalisation using Neural Nets](http://openaccess.thecvf.com/content_ECCV_2018/papers/Vassileios_Balntas_RelocNet_Continous_Metric_ECCV_2018_paper.pdf): Retrieval + relative pose regression

* Temporal/RNN
	* [VidLoc: A deep spatio-temporal model for 6-DoF video-clip relocalization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Clark_VidLoc_A_Deep_CVPR_2017_paper.pdf)
	* [Deep Feature Flow for Video Recognition](http://openaccess.thecvf.com/content_cvpr_2017/papers/Zhu_Deep_Feature_Flow_CVPR_2017_paper.pdf)
	* [Recurrent Fully Convolutional Networks for Video Segmentation](https://arxiv.org/pdf/1606.00487.pdf)
	* [LSTM](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
	* [End-to-end Flow Correlation Tracking with Spatial-temporal Attention](https://arxiv.org/pdf/1711.01124.pdf) Aggregate temporal feature maps through FlowNet & similarity-weighting.
	[Learning Blind Video Temporal Consistency](http://openaccess.thecvf.com/content_ECCV_2018/papers/Wei-Sheng_Lai_Real-Time_Blind_Video_ECCV_2018_paper.pdf) Fuse last prediction and current prediction by directly regressing the residual
	[Spatio-Temporal Transformer Network
for Video Restoration](http://openaccess.thecvf.com/content_ECCV_2018/papers/Tae_Hyun_Kim_Spatio-temporal_Transformer_Network_ECCV_2018_paper.pdf) Spatio-temporal transformer samples an image from multiple frames ($$T\times H \times W \rightarrow 1\times H \times W$$)

* Traditional
	* [Efficient & effective prioritized matching for large-scale image-based localization](http://ieeexplore.ieee.org/abstract/document/7572201/)

* Feature selection
	* [Avoiding confusing features in place recognition](http://www.di.ens.fr/~josef/publications/knopp10.pdf): (ECCV 2010)
	* feature re-weighting: [Learned Contextual Feature Reweighting for Image Geo-Localization](http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8099829)

* Recent works
	* [MapNet: Geometry-Aware Learning of Maps for Camera Localization](https://arxiv.org/pdf/1712.03342.pdf): Leverage the full map (CVPR2018, spotlight)
	* [X-View: Graph-Based Semantic Multi-View Localization](https://arxiv.org/pdf/1709.09905.pdf): Use segmentation
	* [Semantic Visual Localization](https://arxiv.org/pdf/1712.05773.pdf): Use segmentation (CVPR2018, ETH)
	* [Benchmarking 6DOF Outdoor Visual Localization in Changing Conditions](https://arxiv.org/pdf/1707.09092.pdf): Outdoor benchmark (CVPR2018, spotlight, ETH)
	* [InLoc: Indoor Visual Localization with Dense Matching and View Synthesis](): (CVPR2018, spotlight, ETH)
	* Pose regression + visual odometry [Deep Auxiliary Learning for Visual Localization and Odometry](https://arxiv.org/pdf/1803.03642.pdf): (ICLA2018)


* encoder-decoder network
	* Scene coordinate regression: [Full-Frame Scene Coordinate Regression for Image-Based Localization](https://arxiv.org/pdf/1802.03237.pdf)
	* Single-view depth estimation: [Unsupervised Learning of Depth and Ego-Motion from Video](https://arxiv.org/pdf/1704.07813.pdf) [Github](https://github.com/tinghuiz/SfMLearner)
	* Single-view depth estimation: [UnDeepVO: Monocular Visual Odometry through Unsupervised Deep Learning](https://arxiv.org/pdf/1709.06841.pdf)
	* Depth completion: [Deep Depth Completion of a Single RGB-D Image](http://deepcompletion.cs.princeton.edu/paper.pdf)
	* **DispNet**: [A Large Dataset to Train Convolutional Networks for Disparity, Optical Flow, and Scene Flow Estimation](https://arxiv.org/pdf/1512.02134.pdf)

* geometry + deep learning
	* [Eigendecomposition-free Training of Deep Networks with Zero Eigenvalue-based Losses](https://gfx.uvic.ca/pubs/2018/zheng2018eigen/paper.pdf)
	* [Learning to Find Good Correspondences](https://gfx.uvic.ca/pubs/2018/yi2018learning/paper.pdf)

* Bayesian Neural Network & MCMC Sampling
	* [Coursera: Bayesian Neural Networks](https://www.coursera.org/learn/bayesian-methods-in-machine-learning/lecture/HI8ta/bayesian-neural-networks)
	* SGLD: [Bayesian Learning via Stochastic Gradient Langevin Dynamics](https://www.ics.uci.edu/~welling/publications/papers/stoclangevin_v6.pdf): (ICML2011)
	* preconditioned SGLD: [Preconditioned Stochastic Gradient Langevin Dynamics for Deep Neural Networks](http://people.ee.duke.edu/~lcarin/aaai_psgld_final.pdf): (AAAI 2016)
	* [Bayesian Dark Knowledge](https://arxiv.org/pdf/1506.04416.pdf): (NIPS2015)
	* [Modelling Uncertainty in Deep Learning for Camera Relocalization](https://arxiv.org/pdf/1509.05909.pdf)
	* [DSAC-Differentiable RANSAC for camera localization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Brachmann_DSAC_-_Differentiable_CVPR_2017_paper.pdf)
	* [Learning Weight Uncertainty with Stochastic Gradient MCMC for Shape Classification](http://openaccess.thecvf.com/content_cvpr_2016/papers/Li_Learning_Weight_Uncertainty_CVPR_2016_paper.pdf)

* Optical Flow
	* [FlowNet](https://arxiv.org/pdf/1504.06852.pdf)
	* [FlowNet2.0](https://arxiv.org/pdf/1612.01925.pdf) [[github]](https://github.com/lmb-freiburg/flownet2)
	* [GeoNet: unsupervide depth, optical flow & pose](http://openaccess.thecvf.com/content_cvpr_2018/papers/Yin_GeoNet_Unsupervised_Learning_CVPR_2018_paper.pdf) [[github]](https://github.com/yzcjtr/GeoNet)
	* [joint optical flow + segmentation](https://arxiv.org/pdf/1607.07716.pdf)
	* [Unsupervised depth & ego-motion](https://people.eecs.berkeley.edu/~tinghuiz/projects/SfMLearner/cvpr17_sfm_final.pdf) [[github]](https://github.com/tinghuiz/SfMLearner) (CVPR 2017 Oral)
	* [SegFlow: optical flow + object segmentation](https://arxiv.org/pdf/1709.06750.pdf) [[github]](https://github.com/JingchunCheng/SegFlow)
	* [stereo + segmentation + optical flow](http://www.cvlibs.net/publications/Behl2017ICCV.pdf)
	* [Unsupervised, depth + stereo](https://arxiv.org/pdf/1609.03677.pdf) (CVPR 2017)

* CRF
	* [Deep Convolutional Neural Fields for Depth Estimation from a Single Imag](https://arxiv.org/pdf/1411.6387.pdf)
	* [Multi-Scale Continuous CRFs as Sequential Deep Networks for Monocular Depth Estimation](http://openaccess.thecvf.com/content_cvpr_2017/papers/Xu_Multi-Scale_Continuous_CRFs_CVPR_2017_paper.pdf)
	* [Efficient Inference in Fully Connected CRFs with Gaussian Edge Potentials](https://arxiv.org/pdf/1210.5644.pdf)





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

### [EPnP](http://icwww.epfl.ch/~lepetit/papers/lepetit_ijcv08.pdf)

EPnP algorithm is one of the most efficient PnP algorithm which has O(n) complexity. It is composed of three steps:

Step 1. In world coordinate system, find four control points: $$c_j^w (j = 1,...,4)$$. One of them is the centroid of the 3D points and the other three are vectors aligned with principle directions of the 3D point set. Then all the 3D points are represented by the **[barycentric coordinates](https://en.wikipedia.org/wiki/Barycentric_coordinate_system)** as a linear combination of the four control points: $$p_i^w = \sum_{j=1}^4 \alpha_{ij} c_j^w (i=1,...,n)$$.

Step 2. In camera coordinate system, the equations between 3D points and control points still preserve:  $$p_i^c = \sum_{j=1}^4 \alpha_{ij} c_j^c$$. Given the 2D projections of the 3D points $$\{p_i\}i=1,...,n$$, we have $$\omega_i \begin{bmatrix} u_i \\v_i \\ 1 \end{bmatrix} = K p_i^c = K \sum_{j=1}^4 \alpha_{ij} c_j^c$$. Here, $$\omega_i$$ and $$\{c_j^c\}j=1,...,4$$ are unknowns. By filling in the camera intrinsic matrix, the equation can be expressed as follow:

$$\omega_i \begin{bmatrix} u_i \\ v_i \\ 1 \end{bmatrix} = \begin{bmatrix} f_u & 0 & u_c \\ 0 & f_v & v_c \\ 0 & 0 & 1 \end{bmatrix}  \begin{bmatrix} \sum_{j=1}^4 \alpha_{ij} x_j^c \\ \sum_{j=1}^4 \alpha_{ij} y_j^c \\ \sum_{j=1}^4 \alpha_{ij} z_j^c \end{bmatrix}.$$

By eliminating $$\omega_i = \sum_{j=1}^4 \alpha_{ij}z_j^c$$, we get two linear equations:

$$\begin{cases} \sum_{j=1}^4 \big( \alpha_{ij}f_u x_j^c + \alpha_{ij} (u_c - u_i) z_j^c \big) = 0 \\ \sum_{j=1}^4 \big( \alpha_{ij}f_v y_j^c + \alpha_{ij} (v_c - v_i) z_j^c \big) = 0 \end{cases}.$$

Therefore, each 3D-2D correspondence yields two linear equations with respect to a 12-vector $$x=[x_1^c, y_1^c, z_1^c, ..., x_4^c, y_4^c, z_4^c]^T$$. If consider all the correspondences, we generate a linear system of the form

$$Mx = 0.$$

If we have normalized the coordinates of image pixels to $$[u_i, v_i]^T$$, the equation system becomes

$$\begin{bmatrix} \alpha_{i1} & \alpha_{i2} & \alpha_{i3} & \alpha_{i4} & 0 & 0 & 0 & 0 & -u_i \alpha_{i1} & -u_i \alpha_{i2} & -u_i \alpha_{i3} & -u_i \alpha_{i4} \\ 0 & 0 & 0 & 0 & \alpha_{i1} & \alpha_{i2} & \alpha_{i3} & \alpha_{i4} & -v_i \alpha_{i1} & -v_i \alpha_{i2} & -v_i \alpha_{i3} & -v_i \alpha_{i4} \end{bmatrix} \begin{bmatrix} x_1^c \\ x_2^c \\ x_3^c \\ x_4^c \\ y_1^c \\ y_2^c \\ y_3^c \\ y_4^c \\ z_1^c \\ z_2^c \\ z_3^c \\ z_4^c \end{bmatrix} \\ =   \left(\begin{bmatrix} 1 & 0 & -u_i \\ 0 & 1 & -v_i \end{bmatrix}  \otimes \mathbf{\alpha}_i^T \right)  \mathbf{x} =  \mathbf{0},$$

where $$\otimes$$ denotes Kronecker product.

Step 3. The solution of the linear equation system lies in the **null space** or **kernel** of $$M^TM$$ and can be expressed as $$x = \sum_{i=1}^N \beta_i v_i$$, where $$v_i$$ are the right-singular vectors of $$M$$ corresponding to the zero singular values. Now the unknowns become $$\{\beta_i\}i=1,...,N$$. Based on the analysis in [2], the dimension $$N$$ of null-space of $$M^TM$$ varies from 1 to 4. The algorithm then considers all the four cases and choose the best set of $$\{\beta_i\}i=1,...,N$$ with the miminum reprojection error.

### Other PnPs and insights

* RPnP: [A Robust O(n) Solution to the Perspective-n-Point Problem](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6143946)
* OPnP: [Revisiting the PnP Problem: A Fast, General and Optimal Solution](http://www2.maths.lth.se/vision/publdb/reports/pdf/zheng-kuang-etal-iiccvi-13.pdf)
* REPPnP: [Very Fast Solution to the PnP Problem with Algebraic Outlier Rejection](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6909465)
* ASPnP: [ASPnP: An Accurate and Scalable Solution to the Perspective-n-Point Problem](https://www.jstage.jst.go.jp/article/transinf/E96.D/7/E96.D_1525/_pdf)
* The accuracy of solutions to the PnP problem is closely related to the configuration of the 3D point set [RPnP]. Ordinary 3D case, planar case, quasisingular case and "uncentered data" [EPnP].
* Under the assumption of independently and identically distribution (i.i.d.) Gaussian noise, minimizing the reprojection error is statistically optimal in the sense of maximum likelihood estimation (MLE).
* Pose ambiguity
	1. [Robust Pose Estimation from a Planar Target](https://pdfs.semanticscholar.org/85ad/143e7fe0eafe536bb769de3cc5324500dd58.pdf)

# Scene COordinate REgression Network (SCORE-Net)

### Network architecture

* [Globally and Locally Consistent Image Completion](http://hi.cs.waseda.ac.jp/~iizuka/projects/completion/en/)

* Dilated convolution (空洞卷积) Fully convolutional betwork, dilated conv

* **[Heteroscedasticity](http://statisticsbyjim.com/regression/heteroscedasticity-regression/)** 异方差，变量的方差各不相同。Remidial actions for severe heteroscedasticity are necessary.

* [Weighted least square](https://stats.stackexchange.com/questions/97832/how-do-you-find-weights-for-weighted-least-squares-regression?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

* ["Data normalization (or called pre-conditioning in the numerical literature) is an essential step in the DLT algorithm."](http://cvrs.whu.edu.cn/downloads/ebooks/Multiple%20View%20Geometry%20in%20Computer%20Vision%20(Second%20Edition).pdf) from Page 107

* rank-constrained optimization

* degenerate cases and the coplanar case




# Reference
[1] [Linear N-Point Camera Pose Determination](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=784291)

[2] [EPnP: An Accurate O(n)Solution to the PnP Problem](http://icwww.epfl.ch/~lepetit/papers/lepetit_ijcv08.pdf)
