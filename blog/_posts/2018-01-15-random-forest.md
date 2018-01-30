---
title: Random Forest
updated: 2018-01-15 10:00
category: 
tag:
---

* TOC
{:toc}


>This post is a recap of the famous random forest algorithm proposed by [Leo Breiman](https://scholar.google.com.hk/citations?user=mXSv_1UAAAAJ&hl=zh-CN). And it is also my first extended study of the new year 2018. Keep evolving!

# Introduction

A random forest is an classifier consisting of an ensemble of tree-structured classifiers $$h(\mathbf{x}, \Theta_k) (k=1, 2, ...)$$ where $$\Theta_k$$ are iid, and each tree casts a unit vote of classification for input feature vector $$\mathbf{x}$$. The final classfication is yielded by the majority vote of all tree classifiers.

# Backgroud & Preliminary

### CART, Classification and Regression Tree

Classification tree and regression tree are two variabilities of prediction tree. Both of them have two parts: one is the recursive partition accomodated by the tree structure, the other is a simple model for each leaf node.

**Tree fitting.** Tree fitting is to find a good partition of the data. The basic objective is to maximize $$I[C;Y]$$, the information the cluster $$C$$ gave us about the results or response $$Y$$. For example, if answering one question easily separate data into two independent groups (similar responses inside groups, different responses across groups), we say it is a good partition because it gives us a lot information about how the partition affects the responses or regression or classification results.

One of the simplist way of defining the information decrease is **entropy** from information theory. For discrete variables, it is defined as

$$S = \sum_{c\in \operatorname{leaves}(T)} \sum_{i\in c} (y_i - m_c)^2 = \sum_{c\in \operatorname{leaves}(T)} n_c V_c,$$

where $$m_c = \frac{1}{n_c} \sum_{i\in c}y_i$$ is the prediction of leaf $$c$$, $$V_c$$ is the within-leave variance of leaf $$c$$. Splits are made to minimize $$S$$ as much as possible greedily.

To avoid severe over-fitting or under-fitting, one successful way is to use the idea of cross-validation. The data is divided into a training set and testing set randomly. One way is to grow the tree based on the training data as largely as possible and then prune the tree using testing data if the pruning reduces the sum of errors. Another way is to alternate between growing and pruning and exchange the role of training and testing sets until the size of the tree doesn't change.

**Regression trees.** "When the data has lots of features which interact in complicated, nonlinear ways, assembling a single global model can be difficult, and hopelessly confusing when you do succeed. Regression trees partition the feature space into smaller regions, where the interactions are more manageble." "Each of the leaves represents a cell of partition, and has attached to it a simple model which applies in that cell only." [2]

### Generalization error

**Margin function** is defined as 

$$mg(\mathbf{X}, Y) = E_k(I(h_k(\mathbf{X}) = Y)) - \max_{j\neq Y} E_k(I(h_k(\mathbf{X}) = j)),$$

where $$h_k(.)$$ is tree classifiers and $$I(.)$$ is indicator function. Quoted from [1], "the margin measures the extent to which the average
number of votes at $$\mathbf{X}$$, $$Y$$ for the right class exceeds the average vote for any other class. The larger the margin, the more **confidence** in the classification."

**Generalization error** is defined as 

$$PE^* = P_{\mathbf{X}, Y}(mg(\mathbf{X}, Y) < 0).$$

**Strength and correlation:** "The two ingredients involved in the generalization error for random forests are **the strength of the individual classifiers in the forest** and **the correlation between them**."

Skipping the mathematical deduction in [1], strength is related to the accuracy of the set of tree classifiers and correlation is the correlation of margin function across the tree ensemble.

The generalization error is bounded by 

$$PE^* \leq \rho (1-s^2) / s^2.$$

To make smaller generalization error, we encourage larger strength of tree classifiers and smaller correlation (i.e., intuitively the tree classifiers should have similar accuracy.).

### Out-of-bag error
When training tree classifiers, we sample data from training sets randomly as *bootstrap dataset*. The bootstrap dataset may contain duplicated records, yet as well, may miss some records. Then out-of-bag error is obtained by testing the missed records on trained tree classifiers and measured by error rate.

# Strategies

### Random forests using random input selection

At each node, select a small group of input variables at random.

### Random forests using linear combination if inputs

At a given node, $$L$$ variables are randomly selected and added together with coefficiens that are uniform random numbers on $$[-1, 1]$$. $$F$$ linear combinations are generated, and then a search is made over these for the best split.

# Applications

### Scene coordinate regression [3, 4]

The scene coordinate regression forest (ScoRe forest) output 3D global coordinate for the input of feature of RGBD pixel $$\mathbf{p}$$. In training, each split node $$n$$ is assigned $$(\theta_n, \tau_n)$$ from a pool of $$N$$ candidates, so that the decision function writes

$$df(\mathbf{p}; \theta_n, \tau_n) = \left[ f_{\theta_n}(\mathbf{p}) \geq \tau_n \right],$$

where $$f_{\theta_n}(.)$$ is the feature response function. In [4], the response function is just differences of RGB pixel values (a little bit trivial):

$$f_{\theta_n}(\mathbf{p}) = I(\mathbf{p} + \mathbf{\delta}_1, c_1) - I(\mathbf{p} + \mathbf{\delta}_2, c_2),$$

where $$\mathbf{\delta}_1$$ and $$\mathbf{\delta}_2$$ are offsets from pixel $$\mathbf{p}$$ and $$c_1$$ and $$c_2$$ are channel indexes. Thus the feature parameters are $$\theta_n = \{\mathbf{\delta}_1, \mathbf{\delta}_2, c_1, c_2 \}$$.
Each leaf node then stores some empirical 3D distribution of the samples the have arrived at the leaf (e.g., mean-shift [5], 3D mixtures of Gaussian). 

The predictions of multiple regression trees are finally combined to yield the final regression result (e.g., by geometric median averaging in [4]).


# Reference

[1] [Random Forest](https://link.springer.com/content/pdf/10.1023%2FA%3A1010933404324.pdf)

[2] [Regression Trees](http://www.stat.cmu.edu/~cshalizi/350-2006/lecture-10.pdf)

[3] [Scene Coordinate Regression Forests for Camera Relocalization in RGB-D Images](https://www.cv-foundation.org/openaccess/content_cvpr_2013/papers/Shotton_Scene_Coordinate_Regression_2013_CVPR_paper.pdf)

[4] [Random  Forests  versus  Neural  Networks - What’s  Best  for  Camera  Localization?](https://arxiv.org/pdf/1609.05797.pdf)

[5] [Mean shift algorithm](https://spin.atomicobject.com/2015/05/26/mean-shift-clustering/)


