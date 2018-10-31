---
title: How does KinectFusion works
updated: 2018-10-20 14:00
---


* TOC
{:toc}

## Overall System Workflow

![bilateral_filter]({{site.baseurl}}/images/kinectfusion.png)
**<center>Figure1. KinectFusion workflow</center>**

KinectFusion是一个完全基于深度图的SLAM系统，输入是深度图流，输出是每帧深度图的pose和基于TSDF技术的mapping(as surface reconstruction)。工作流程主要分为四个模块：

## Measurement

这一步对输入的raw depth measurements进行预处理，计算出每个pixel在相机坐标系下的三维坐标和normal, 即$$(\mathbf{V}_k^c, \mathbf{N}_k^c)$$。注意，在计算normal map之前会对raw depth map进行bilateral filtering，以得到更高质量的normal结果。

## Pose Estimation

因为输入是连续的深度图流，所以估算下一帧的相机pose其实就是做tracking。方法是将当前帧的点云通过ICP算法align到上一帧的点云（或已重建出的全局surface）上。注意，此处用的error metric是point-plane distance，即两点连线在法线方向的投影的长度。在找correspondence时需要保证两点的距离和法线的夹角都要小于阈值。ICP初始化时，用上一帧的pose来初始化当前帧。在ICP优化的迭代过程中，考虑到每步更新的旋转角很小，因此对两帧之间的relative transformation进行如下参数化：

$$\mathbf{T}_{inc} = [\mathbf{R} \| \mathbf{t}] = \begin{bmatrix} 1 & \alpha & -\gamma & t_x\\ -\alpha & 1 & \beta & t_y \\ \gamma & -\beta & 1 & t_z  \end{bmatrix},$$

即优化6维变量$$\mathbf{x}=(\beta, \gamma, \alpha, t_x, t_y, t_z)^T$$。

在第$$n$$步迭代时，全局坐标系下点云坐标的更新表达为：

$${\mathbf{V}_k^g}^n = \mathbf{R} {\mathbf{V}_k^g}^{n-1} + \mathbf{t} = \mathbf{G} \mathbf{x} + {\mathbf{V}_k^g}^{n-1},$$

此处，$$\mathbf{G}=\left[ [{\mathbf{V}_k^g}^{n-1}]_x \| \mathbf{I}_{3x3} \right]$$。这样一来，objective fuction就变成关于$$\mathbf{x}$$的线性函数。
