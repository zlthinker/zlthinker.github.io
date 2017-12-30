---
title: SLAM Related
updated: 2017-12-28 14:00
category: 
tag:
---

* TOC
{:toc}

# Pose Graph

Constraints of graph-based SLAM can be concluded as two types:

1. Sequential constraints introduced by odometry.
2. Non-sequential constraints from place recognition (or say loop closure).

# Loop closure

![Factor graph]({{site.baseurl}}/images/loop_closure.png)
**<center>Fig1. Pose graph in five loops</center>**

Loop closure equips mobile robots with the capability of recognizing revisted places and re-estimating the map of environments during long-term move.

**Perceptual aliasing.** One challenge posed to loop closure is perceptual aliasing, which means false positive revisits are identified leading to fake loops. Associating two different places incorrectly can severely corrupt map estimate, as shown by the red lines not parallel to z-axis in Fig 1.

# Reference

[Robust Loop Closing Over time for Pose Graph SLAM](http://webdiis.unizar.es/~ylatif/papers/IJRR.pdf)