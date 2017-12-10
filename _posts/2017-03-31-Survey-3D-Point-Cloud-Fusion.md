---
title: Survey - 3D Point Cloud Fusion
updated: 2017-03-31 10:50
category: [CV]
tags: [survey]
---

## 3D Point Cloud Fusion

The essential key to 3D point cloud fusion is to find 3D point correspondences, based on which similarity transformation can be estimated to roughly align point clouds and then adopt algorithms like ICP to register them accurately. To robustly determine the 3D point correspondences between noisy point clouds, expressive descpritors are very important. There are two main types of 3D feature descriptors to date:

1. Multiview texture based descriptor [1].
2. Local geometry based descriptor. 3D geometry can be described in terms of different native 3D formats, including voxel grid and polygon mesh. This method is more suitable for dense and watertight point clouds.

	2.1 Voxel grid. Use binary voxel representation, e.g., 3D ShapeNets [5].
	
	2.2 Polygon mesh. Use norm (1st order), surface curvature (2nd order), etc, e.g., [2].
	
	2.3 Combination of both above.
	
#### Multi-View Fusion
1. View pooling [1]: The variant local structures are **overlayed**.

 
## Reference

1. Multi-view Convolutional Neural Networks for 3D Shape Recognition
2. Aligning Point Cloud Views using Persistent Feature Histograms
3. Comparison of 3D interest point detectors and descriptors for point cloud fusion
4. Multi-View 3D Object Detection Network for Autonomous Driving
5. 3D ShapeNets: A Deep Representation for Volumetric Shapes
6. [Location Recognition Using Prioritized Feature
Matching](http://download.springer.com/static/pdf/110/chp%253A10.1007%252F978-3-642-15552-9_57.pdf?originUrl=http%3A%2F%2Flink.springer.com%2Fchapter%2F10.1007%2F978-3-642-15552-9_57&token2=exp=1494555015~acl=%2Fstatic%2Fpdf%2F110%2Fchp%25253A10.1007%25252F978-3-642-15552-9_57.pdf%3ForiginUrl%3Dhttp%253A%252F%252Flink.springer.com%252Fchapter%252F10.1007%252F978-3-642-15552-9_57*~hmac=c3d4f68f4216f25a311d2612251ba7f57791f13fce8db7f3da1aadc79bba8921)


