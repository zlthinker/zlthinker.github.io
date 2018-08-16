---
title: Semi-global Matching for Stereo Processing
updated: 2018-08-12 14:00
---


* TOC
{:toc}

## Overview

Typical steps of stereo methods:
* Matching cost computation: compute pixelwise matching cost between pixels of subjective image and pixels of reference image
* Cost aggreation: often sum over costs over a fixed sized window with certain weighting strategy (like computing correlation)
* Disparity computation/optimization: select the disparity by winner takes all or global graph-based strategy; even consider occlusions, visibility and left/right or symmetric consistency
* Disparity refinement: refinement steps like removing peaks, checking consistency and subpixel interpolation


























