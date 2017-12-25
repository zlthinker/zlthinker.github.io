---
title: Conditional Random Field
updated: 2017-07-18 18:00
category: [Maching Learning]
---

### Introduction

Conditional random fields (CRFs) are a probabilistic framework for labeling and segmenting sequential data by maximizing the conditional probability $$p(Y \| x)$$ over label sequences $$Y$$ given a particular observation sequence $$x$$.
Formally, we define $$G=(V,E)$$ to be an undirected graph such that there is a node $$v \in V$$ corresponding to each of the random variables representing an elements $$Y_v$$ of $$Y$$. If each random variable $$Y_v$$ obeys the Markov property with respect to $$G$$, then $$(Y, X)$$ is a conditional random field.

### Preliminary

#### Graphical Modeling

Graphical modeling is an insightful method of describing the distribution over many variables. It helps to factorize the monolithic distribution into the product of **local functions** that each depend on a much smaller subset of variables. For example, given an undirected graphical model over a full variable set $$Y$$, the distribution can be written as 

$$p(Y= \mathbf{y}) = \frac{1}{Z} \prod_{a=1}^A \Phi_a(\mathbf{y}_a).$$

Here $$Y_a = y_a$$ are a subset of variables in $$Y$$ given by fractorization. Different fractorization leads to different modeling of distribution. $$Z$$ is the normalization factor that ensures the sum of probabilities equal to 1.

To better formalize the fractorization, a fractor graph is defined as a bipartite graph $$G=(V, F, E)$$ in which nodes $$V$$ denote the set of variables in the model, and nodes $$F$$ denote the factors. The semantics of the factor graph is that if a variable node $$Y_s$$ for $$s \in V$$ is connected to a fractor node $$\Phi_a$$ for $$a \in F$$, then $$Y_s$$ is one of the arguments of $$\Phi_a$$.

### Conditional Random Field

Let $$G$$ be a factor graph over $$X$$ and $$Y$$. Then $$(X, Y)$$ is a conditional random field if for any value $$\mathbf{x}$$ of $$X$$, the distribution $$p(\mathbf{y} \| \mathbf{x})$$ fractorizes according to $$G$$. If $$F=\{ \Phi_a \}$$ is the set of fractors in $$G$$, then the conditional distribution for a CRF is 

$$p(\mathbf{y} \| \mathbf{x}) = \frac{1}{Z(\mathbf{x})} \prod_{a=1}^A \Phi_a(\mathbf{y}_a, \mathbf{x}_a).$$

### CRF VS. MRF

[An Introduction to Conditional Random Fields](http://homepages.inf.ed.ac.uk/csutton/publications/crftut-fnt.pdf), Section 2.6.2

### Reference
[An Introduction to Conditional Random Fields](http://homepages.inf.ed.ac.uk/csutton/publications/crftut-fnt.pdf)

[Conditional Random Fields: An Introduction](http://dirichlet.net/pdf/wallach04conditional.pdf)