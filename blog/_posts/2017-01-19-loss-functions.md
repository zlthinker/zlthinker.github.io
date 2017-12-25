---
title: Loss Functions
updated: 2017-01-19 13:50
tags: [CNN, loss]
category: General Research
---

## Loss

### Cross Entropy

Cross entrpoy measures the difference between two probabilistic distributions $$p$$ and $$q$$. It can be used to define the loss function in machine learning and optmization, where the true probility $$p_i$$ is the true label and the given distribution $$q_i$$ is the predicted value of the currenr model.

For logistic regression where $$p\in\{y, 1-y\}$$ and $$q\in\{\hat{y}, 1-\hat{y}\}$$, the cross entropy measuring the similarity between $$p$$ and $$q$$ is

$$H(p, q)=-\sum\limits_{i}p_ilogq_i = -ylog \hat{y} - (1-y)log(1-\hat{y}).$$

```
SigmoidCrossEntropyLoss
```

The multinomial logistic loss for multinomial logistic regression (also known as softmax regression) is

$$H(p, q)=-\sum\limits_{i=1}^K 1\{i=k\}logq_i,$$

where $$K$$ is the class number, $$k$$ is the true class.

```
SoftMaxLoss
```

### Contrative Loss

The objective of constrative loss is to enhance the constrast between two unsimilar feature vectors by some margin but minimize distance between two similar feature vectors. It is widely used in siamese network. The loss function is written as

$$L = \frac{1}{2N} \sum\limits_{n=1}^N y d^2 + (1-y)max(margin - d, 0)^2,$$

where $$y\in\{0, 1\}$$ is the binary label and $$d = \|a_n-b_n\|$$ is the Euclidean distance between two feature vectors.

```
ConstrastiveLoss
```



## BGD, SGD & Mini-batch GD

### Batch Gradient Descent (BGD)
每次迭代的梯度下降方向由所有样本决定，精度高，速度慢

### Mini-batch Gradient Descent
将一次迭代的样本分成若干个mini-batch，每次迭代的梯度下降方向由一个mini-batch计算得到，精度低，速度快

### Stochastic Gradient Descent (SGD)
Mini-batch GD的特例，每个mini-batch的大小是1

## Solver

### Weight Decay
A loss function of an optimization process can be written as

$$L(W) = \frac{1}{N} \sum\limits_i^N f_W(X^{(i)}) + \lambda r(W).$$

The value $$\lambda$$ is called `weight_decay`, which is used to prevent overfitting in which case the weights get very large. It determines how dominant the regularization term will be in gradient computation. Options for selecting regularization type is provided, such as `regularization_type: "L1"` or `regularization_type: "L2"` (default).
