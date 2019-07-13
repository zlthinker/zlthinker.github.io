---
title: Graph Theory
updated: 2019-07-13 17:30
---

* TOC
{:toc}

# Terminology and definitions

A graph $$G$$ is a pair of sets $$G = (V, E)$$. $$V$$ is the set of vertices and $$E$$ is the set of edges.

* Order of graph. $$n = \|V\|$$.
* Size of graph. $$m = \|E\|$$.
* Density of graph. $$\delta(G) = \frac{m}{C_n^2}$$.
* Neighborhood of $$v$$. $$\Gamma(v)$$.
* Adjacency matrix $$A_G \in R^{n\times n}$$. $$A_G(u, v) = 1$$, if $$\{u, v\} \in E$$.
* Degree of $$v$$. $$deg(v)$$.
* Degree matrix $$D_G \in R^{n\times n}$$. $$D_G(v, v) = deg(v)$$.
* A graph is regular if all of the vertices have the same degree. The graph is k-regular, if all the degrees are $$k$$.
* Edge-connectivity of graph. The minimum number of edges that would need to be removed from $$G$$ in order to make it disconnected is the edge-connectivity of the graph.
* A graph that contains no cycles is **acyclic** and is also called a **forest**. A connected forest is called a **tree**.
* A **subgraph** $$G^S = (S, E_S)$$ of $$G=(V, E)$$ is composed of a set of vertices $$S \subseteq V$$ and a set of edges $$E_S \subseteq E$$ such that $$\{u,v\}\inE_S$$ implies $$u, v \in S$$; the graph $$G$$ is a **supergraph** of $$G^S$$.
* A connected acylic subgraph that includes all vertices is called a **spanning tree** of the graph. A spanning tree has necessarily exactly $$n-1$$ edges.
* The spanning tree with smallest total weight is called the **minimum spanning tree**.
* An **induced subgraph** $$G(S)$$ of a graph $$G=(V,E)$$ is the graph with the vertex set $$S\subseteq V$$ with an edge set being $$E(S) = \{\{v,u\}\| v\inS, u\in S, \{v,u\}\inE  \}$$.