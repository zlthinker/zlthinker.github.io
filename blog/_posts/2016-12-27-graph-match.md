---
title: Get to Know Graph Matching
updated: 2016-12-27 15:00
tags: [image matching, graph]
category: CV
---

> Graph matching has wide application in computer vision community, e.g. image matching, mesh matching. It embeds the topology structure into matching process, which is helpful to improve accuracy.

## Preliminaries & Definitions

### Planar Graph
In graph theory, a planar graph is a graph that can be drawn in such a way that no edges cross each other.

### Semantic graph
A semantic graph is a graph-structured data representation in which vertices represent concepts (e.g., movie, actor) and edges represent relationships between concepts (e.g. appeared-in). Both vertices and edges in a semantic graph are typed and attributed.

### Graph isomorphism (同构)
A isomorphism of graphs $$G$$ and $$H$$ is a structure-preserving bijection between the vertex sets of $$G$$ and $$H$$

$$f: V(G) \rightarrow V(H)$$

such that any two vertices $$u$$ and $$v$$ of $$G$$ are adjacent in $$G$$ if and only if $$f(u)$$ and $$f(v)$$ are adjacent in $$H$$.

### Labeled graph
A labeled graph is represented as a tuple $$G=(\mathcal{V}, \mathcal{E}, \mu, \nu)$$, where

* $$\mathcal{V}$$ is the set of vertices,
* $$\mathcal{E} \subseteq \mathcal{V} \times \mathcal{V} $$ is the set of edges,
* $$\mu : \mathcal{V} \rightarrow \mathcal{L}_{\mathcal{V}}$$ is the vertex labeling function with $$\mathcal{L}_{\mathcal{V}}$$ the vertex-label set (e.g. the descriptors of feature nodes), and
* $$\nu : \mathcal{E} \rightarrow \mathcal{L}_{\mathcal{E}}$$ is the edge labeling function with $$\mathcal{L}_{\mathcal{E}}$$ the edge-label set (e.g. the angles between edges and feature orientations).

## Formulation
The goal of graph-based pairwise image matching is to establish correspondences between two sets of visual features.

## Reference
[1] [Factorized Graph Matching](http://f-zhou.com/gm/2012_CVPR_FGM.pdf)

## Music Today

<iframe width="560" height="315" src="https://www.youtube.com/embed/T1blvohThhw" frameborder="0" allowfullscreen></iframe>
