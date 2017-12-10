---
title: Paper Notes
updated: 2017-04-06 09:00
category: [CV]
tags: [Notes]
---

* TOC
{:toc}

>This is a blog for notes of insights after reading papers.

### 3D Model Matching with Viewpoint-Invariant Patches (VIP), Changchang Wu
As less of local texture variation is sacrificed to achieve invariance, more is left for discrimination.

The SIFT features achieve 2D similarity transformation invariance (translation, rotation, scale).

### Representation Learning: A Review and New Perspectives

**Depth, abstraction and invariance** Deep architectures can lead to abstract representations because more abstract concepts can often be constructed in terms of less abstract ones. In some
cases, such as in the convolutional neural network (LeCun et al., 1998b), we build this abstraction in explicitly via a pooling mechanism. **More abstract concepts are generally invariant to most local changes of the input.** That makes the representations that capture these concepts generally
highly non-linear functions of the raw input. This is obviously true of categorical concepts, where more abstract representations detect categories that cover more varied phenomena (e.g. larger manifolds with more wrinkles) and thus they potentially have greater predictive power. **Abstraction can also appear in high-level continuous-valued attributes that are only sensitive to some very specific types of changes in the input.** Learning these sorts of invariant features has been a long-standing goal in pattern recognition.

**Pooling and invariance** A hallmark of the
convolutional architecture is that values computed by the same feature detector applied at several neighboring input locations are then summarized through a **pooling operation**, typically taking their max or their sum. This confers the resulting pooled feature layer some degree of invariance to input translations. The output of a pooling unit will be the same irrespective of where a specific feature
is located inside its pooling region. (Sec 11.2)

### Going deeper with convolutions (GooLenet)

If the added capacity is used inefficiently (e.g. if most weights end up to be zero), then a lot of cpmputation is wasted.

### Rethinking the Inception Architecture for Computer Vision (GoogLenet 2.0)

The success of the Inception style building blocks is enabled by the generous use of dimensional reduction and parallel structures of the Inception modules which allows for **mitigating the impact of structural changes on nearby components**.

General Principles of Designing Networks

1. Avoid representational bottlenecks, especially early in the network. In general the representation size (roughly speaking, the dimension) should gently decrease without drastical compression.
2. Increasing the activation per tile in a convolutional network allows for more disentangled features.
3. Dimension reduction can be employed in low-level layers without expecting serious adverse effects, because of the strong correlation in the neighborhood.
4. Balance the width and depth of the network, i.e., the number of the filters per stage and the depth of the network.

Principles of Factorizing Convolutions

1. Replace large spatial filters with consecutive smaller filters. It does not result in loss of expressiveness because of **the enhanced space of variations that the network can learn especially we batch-normalize the output activations**.
2. Replace symmetric convolutions with two asymmetric convolutions (e.g. replcace 5x5 with 1x5 and 5x1). In practice, it does not work well on early layers but gives very good results on medium grid-sizes (On mxm feature maps, where m ranges between 12 and 20).

### Discriminative Learning of Deep Convolutional Feature Point Descriptors

**How it post-processes activations after convolution? L2 pooling + Substractive Norm + Tanh**
We use $$L_2$$ pooling for the poolingsublayers, which has been shown to outperform the more standard max pooling. **Normalization is also important for deep networks and paramount for descriptors.** We use subtractive normalization, i.e. subtract the weighted average over 5x5 neighbourhood with a Gaussian kernel. Regarding the non-linear layer, we use hyperbolic tangent units (Tanh), as we found it to perform better than ReLU.

**How is hard samples crucial?** Once the network has reached a reasonable level of performance, most non-corresponding points will already have a distance above the margin, contributing nothing to the loss - and the gradient. This can result in a very small and noisy estimate of the gradient, effectively stalling the learning process.

**How is PRC computed to measure the match performance?** Compute precisions and recalls with respect to different settings of parameters like distance thresholds.

### Mask R-CNN

1. **Instance segmentation = semantic segmentation + object detection** Instance segmentation requires the correct detection of all objects in an image while also precisely segmenting each instances.

2. **ROIPool VS ROIAlign** ROIPool has the drawback of performing coarse spatial quantization for feature extraction. To fix the misalignment, the quantization-free layer, ROIAlign is proposed, that faithfully preserves exact spatial locations.

3. **Loss function** Apply a per-pixel sigmoid and define $$L$$ as the average binary cross-entropy loss.

### Batch Normalization

![Batch normalization]({{site.baseurl}}/images/batch_normalization.png)

Batch normalization is applied to activations that belong to the same channel in a mini-batch. For example, the input activations has dimension $$(N, C, H, W)$$, the normalization is applied to $$N\times H \times W$$ activations. Totally $$C$$ pairs of parameters $$(\gamma, \beta)$$ are learned. 

### t-SNE Embedding

t-Distributed Stochastic Neighbor Embedding (t-SNE) is an excellent technique for dimensionality reduction that is particularly well suited for the visualization of high-dimensional datasets.