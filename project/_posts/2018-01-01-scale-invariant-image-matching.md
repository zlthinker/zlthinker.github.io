---
title: Progressive Large Scale-Invariant Image Matching in Scale Space
updated: 2018-01-01 23:37
---

* TOC
{:toc}

# Abstract

The power of modern image matching approaches is still fundamentally limited by the abrupt scale changes in images. In this paper, we propose a scale-invariant image matching approach to tackling the very large scale variation of views. Drawing inspiration from the scale space theory, we start with encoding the image’s scale space into a compact multi-scale representation. Then, rather than trying to find the exact feature matches all in one step, we propose a progressive two-stage approach. First, we determine the related scale levels in scale space, enclosing the inlier feature correspondences, based on an optimal and exhaustive matching in a limited scale space. Second, we produce both the image similarity measurement and feature correspondences simultaneously after restricting matching between the related scale levels in a robust way. The matching performance has been intensively evaluated on vision tasks including image retrieval, feature matching and Structure-from-Motion (SfM). The successful integration of the challenging fusion of high aerial and low ground-level views with significant scale differences manifests the superiority of the proposed approach. (Access the full paper [here](./files/iccv2017.pdf))

# Datasets

### The Ground-Aerial Dataset

This dataset encompasses a collection of ground and aerial images. It is composed of three individual blocks (see table below) each of which is captured from both aerial views (of resolution $$4000\times3000$$) and street views (of resolution $$4032\times3024$$). 3D reconstruction from such image sets is challenging due to the abrupt scale difference, as shown in the figure below. Download link: [Block A (8.3G)](https://drive.google.com/open?id=1HhZfye7_fPGiXNnbrpqzhskfcl3d9Pgm), [Block B (13.3G)](https://drive.google.com/open?id=1mkYktK_q-C2ykpb1slVAclBIlrq7Fsci), [Block C (19.5G)](https://drive.google.com/open?id=1E-IUbNfX_l8gP_zFt0-Zpclk6Jk72IUP).

| Area | #Images | #Aerial images | #Ground images |
|:----:|:-------:|:--------------:|:--------------:|
| Block A | 2309 | 822 | 1487 |
| Block B | 3292 | 1945| 1347 |
| Block C | 4866 | 1963| 2903 |

![]({{site.baseurl}}/images/ground_aerial.png)


### The Book-Cover Dataset

The Book-Cover dataset is composed of two series of photographs acquired from two viewpoint angles $$0$$° and $$30$$° with increasing zoom. The $$28$$ images have the same size of $$3024\times4032$$ pixels. The closet-captured photograph of a book cover is used as reference image and is matched against other $$27$$ ones with image scale ratio ranging from $$2$$ to $$64$$ times.
See sample matching results in the figure below. Download link: [The Book-Cover Dataset (87.7M)](https://drive.google.com/file/d/1IO533xhsEAME-phqYX0fnOPI3TWkWSA9/view?usp=sharing).

![]({{site.baseurl}}/images/scale_match.png)

### Terms of Use

The datasets above are provided for research purposes only and without any warranty. Any commercial use is prohibited. 

# Reference

Here is the Bibtex snippet for citing the Dataset in your work.

```
@InProceedings{Zhou_2017_ICCV,
author = {Zhou, Lei and Zhu, Siyu and Shen, Tianwei and Wang, Jinglu and Fang, Tian and Quan, Long},
title = {Progressive Large Scale-Invariant Image Matching in Scale Space},
booktitle = {ICCV},
year = {2017}
}
```


