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
* A **subgraph** $$G^S = (S, E_S)$$ of $$G=(V, E)$$ is composed of a set of vertices $$S \subseteq V$$ and a set of edges $$E_S \subseteq E$$ such that $$\{u,v\}\in E_S$$ implies $$u, v \in S$$; the graph $$G$$ is a **supergraph** of $$G^S$$.
* A connected acylic subgraph that includes all vertices is called a **spanning tree** of the graph. A spanning tree has necessarily exactly $$n-1$$ edges.
* The spanning tree with smallest total weight is called the **minimum spanning tree**.
* An **induced subgraph** $$G(S)$$ of a graph $$G=(V,E)$$ is the graph with the vertex set $$S\subseteq V$$ with an edge set being $$E(S) = \{\{v,u\}\| v\in S, u\in S, \{v,u\}\in E  \}$$.
* An induced subgraph that is a complete graph is called a **clique**.
* **Isomorphism**
* The **spectrum** of a graph is defined as the list of eigenvalues (together with their multiplicities) of its adjacency matrix $$A_G$$. 
* **Cospectral or Isospectral**. Graphs that have the same spectrum are called cospectral or isospectral. Even nonisomorphic graphs can be cospectral.
* **Laplacian matrix** of graph $$G$$: $$L = D_G - A_G$$.
* **Normalized laplacian matrix** of graph $$G$$: $$\mathcal{L} = D_G^{-\frac{1}{2}} L D_G^{-\frac{1}{2}} = I - D_G^{-\frac{1}{2}} A_G D_G^{-\frac{1}{2}}$$. The eigenvalues of $$\mathcal{L}$$ is always zero. The smallest eigenvalue is always zero, as $$\mathcal{L}$$ is singular, and the corresponding eigenvector is simply a vector with each element being the square-root of the degree of the corresponding vertex.
* **Line graph**. A line graph $$G_L = (V_L, E_L)$$ of a given graph $$G = (V, E)$$ is a graph where the vertex set $$V_L$$ is the set of edges $$E$$ of the original graph $$G$$ and two vertices $$v_{ij} \in V_L$$ (corresponding to the edge $$\{v_i, v_j \} \in E$$) and $$v_{kl} \in V_L$$ are connected by an edge $$\{v_{ij}, v_{kl} \} \in E_L$$ if and only if the vertex subsets $$\{v_i, v_j \}$$ and $$\{v_k, v_l \}$$ have a nonempty intersection.