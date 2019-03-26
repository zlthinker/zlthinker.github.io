---
title: Progressive Large Scale-Invariant Image Matching in Scale Space
updated: 2018-01-01 23:37
---

* TOC
{:toc}

# Introduction

The power of modern image matching approaches is still fundamentally limited by the abrupt scale changes in images. In this paper, we propose a scale-invariant image matching approach to tackling the very large scale variation of views. Drawing inspiration from the scale space theory, we start with encoding the image’s scale space into a compact multi-scale representation. Then, rather than trying to find the exact feature matches all in one step, we propose a progressive two-stage approach. First, we determine the related scale levels in scale space, enclosing the inlier feature correspondences, based on an optimal and exhaustive matching in a limited scale space. Second, we produce both the image similarity measurement and feature correspondences simultaneously after restricting matching between the related scale levels in a robust way. The matching performance has been intensively evaluated on vision tasks including image retrieval, feature matching and Structure-from-Motion (SfM). The successful integration of the challenging fusion of high aerial and low ground-level views with significant scale differences manifests the superiority of the proposed approach.

# Datasets

### Ground-Aerial SfM

| Area | #Images | #Aerial images | #Ground images |
|:----:|:-------:|:--------------:|:--------------:|
| Block A | 2309 | 822 | 1487 |
| Block B | 3292 | 1945| 1347 |
| Block C | 4866 | 1963| 2903 |

![]({{site.baseurl}}/images/HKUST.pdf)
