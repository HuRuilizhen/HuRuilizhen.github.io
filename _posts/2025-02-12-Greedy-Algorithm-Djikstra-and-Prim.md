---
layout: post
title: "Greedy Algorithm, Djikstra and Prim"
description: "This post provides an overview of the algorithms covered in the SC2001 course in NTU, including Djikstra's algorithm and Prim's algorithm."
date: 2025-02-12
feature_image: images/graph-theory.jpg
tags: ['algorithm-design', 'discrete-mathematics', 'graph-theory']
---

Greedy algorithms make the locally optimal choice on each iteration with the hope of finding a global optimum solution. They are typically used to solve optimization problems, and are usually more efficient than other algorithms. This post provides a comprehensive review of the algorithms covered in Lecture 6 of the `SC2001` course in NTU, including Djikstra's algorithm and Prim's algorithm.

<!--more-->

## Table of Contents

- [Strategies of Greedy Algorithms](#strategies-of-greedy-algorithms)
- [Solving shortest paths problem using Dijkstra's algorithm](#solving-shortest-paths-problem-using-dijkstras-algorithm)
- [Solving minimum spanning tree problem using Prim's algorithm](#solving-minimum-spanning-tree-problem-using-prims-algorithm)
  - [Minimal Spanning Tree Problem Formulation](#minimal-spanning-tree-problem-formulation)

---

# Strategies of Greedy Algorithms

In optimization problems, the algorithm needs to make a series of choices whose overall effect is to **minimize the total cost**, or **maximize the total benefit**, of some system.

There is a class of algorithms, called the **greedy algorithms**, in which we can find a solution by using only **knowledge available at the time** when the next choice (or guess) must be made. Each individual choice is **the best within the knowledge available** at the time and **not very expensive to compute**.

Once a chioce is made, it **can't be undone in the future** even if it is found to be a bad choice. Greedy algorithms cannot guarantee to produce the
optimal solution for a problem except in a few special cases.

---

# Solving shortest paths problem using Dijkstra's algorithm

---

# Solving minimum spanning tree problem using Prim's algorithm

## Minimal Spanning Tree Problem Formulation

A **subgraph** $G'=(V',E')$ of a graph $G=(V,E)$ is a graph such that
- $V'\subseteq V$
- $E'\subseteq \{ e \mid e \in E \text{ and } e \in V' \times V' \}$$

The **weight** of a subgraph $G'=(V',E')$ is the sum of the weights of its edges: $w(G') = \sum_{e\in E'}w(e)$.

A **spanning tree** of a graph $G = (V, E)$ is a connected acyclic subgraph that spans all the vertices of $G$.
