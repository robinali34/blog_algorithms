---
layout: page
title: "Algorithms Index"
permalink: /algorithms-index/
---

# Algorithms Index

This page provides an organized index of all algorithm posts, grouped by topic and relationships.

## NP-Completeness and Complexity Theory

### Introduction to NP-Complete Problems

**Core Problems:**
- [SAT (Boolean Satisfiability)](/2025/11/16/np-hard-intro-sat/)
- [3-SAT](/2025/11/16/np-hard-intro-sat/) - Special case of SAT with 3 literals per clause
- [Clique](/2025/11/16/np-hard-intro-clique/)
- [Independent Set](/2025/11/16/np-hard-intro-independent-set/)
- [Vertex Cover](/2025/11/16/np-hard-intro-vertex-cover/)
- [Subset Sum](/2025/11/16/np-hard-intro-subset-sum/)
- [Rudrata Path](/2025/11/16/np-hard-intro-rudrata-path/)
- [Rudrata Cycle](/2025/11/16/np-hard-intro-rudrata-cycle/)
- [Traveling Salesman Problem (TSP)](/2025/11/16/np-hard-intro-traveling-salesman-problem/)
- [Zero-One Equations (ZOE)](/2025/11/16/np-hard-intro-zero-one-equations/)
- [3D Matching](/2025/11/16/np-hard-intro-3d-matching/)
- [Integer Linear Programming (ILP)](/2025/11/16/np-hard-intro-integer-linear-programming/)

### Reduction Reference Guides

- [NP-Complete Reduction Examples](/2025/11/21/np-complete-reduction-examples/) - How to prove NP-completeness using reductions
- [NP-Complete Reduction Reference](/2025/11/21/np-complete-reduction-reference/) - Common NP-complete problems for reductions

### Reductions from SAT/3-SAT

- [Reductions from SAT](/2025/11/21/reductions-from-sat/)
- [Reductions from 3-SAT](/2025/11/21/reductions-from-3sat/)
- [3-SAT to Partition](/2025/11/21/reduction-3sat-to-partition/)
- [3-SAT to Exact 4-SAT](/2025/11/22/reduction-3sat-to-exact-4sat/)
- [SAT to Stingy SAT](/2025/11/22/reduction-sat-to-stingy-sat/)
- [SAT to Max SAT](/2025/11/22/reduction-sat-to-max-sat/)
- [SAT to Circuit Design](/2025/11/22/reduction-sat-to-circuit-design/)

### Reductions from Clique

- [Reductions from Clique](/2025/11/21/reductions-from-clique/)
- [Clique to Subgraph Isomorphism](/2025/11/22/reduction-clique-to-subgraph-isomorphism/)
- [Clique to Clique-3](/2025/11/22/reduction-clique-to-clique-3/)
- [Clique to Dense Subgraph](/2025/11/22/reduction-clique-to-dense-subgraph/)
- [Clique to Kite](/2025/11/22/reduction-clique-to-kite/)
- [Clique to Maximum Common Subgraph](/2025/11/22/reduction-clique-to-maximum-common-subgraph/)

### Reductions from Independent Set

- [Reductions from Independent Set](/2025/11/21/reductions-from-independent-set/)
- [Independent Set to Clique + Independent Set](/2025/11/22/reduction-independent-set-to-clique-independent-set/)
- [Independent Set to Sparse Subgraph](/2025/11/22/reduction-independent-set-to-sparse-subgraph/)

### Reductions from Vertex Cover

- [Reductions from Vertex Cover](/2025/11/21/reductions-from-vertex-cover/)
- [Vertex Cover to Hitting Set](/2025/11/22/reduction-vertex-cover-to-hitting-set/)
- [Vertex Cover to Set Cover](/2025/11/22/reduction-vertex-cover-to-set-cover/)
- [Vertex Cover to Dominating Set](/2025/11/22/reduction-vertex-cover-to-dominating-set/)
- [Vertex Cover to Flower Search](/2025/11/22/reduction-vertex-cover-to-flower-search/)

### Reductions from Rudrata Path/Cycle

- [Reductions from Rudrata Path](/2025/11/21/reductions-from-rudrata-path/)
- [Reductions from Rudrata (s,t)-Path](/2025/11/21/reductions-from-rudrata-st-path/)
- [Reductions from Rudrata Cycle](/2025/11/21/reductions-from-rudrata-cycle/)
- [Rudrata Path to Longest Path](/2025/11/22/reduction-rudrata-path-to-longest-path/)
- [Rudrata Path to Spanning Tree with k or Fewer Leaves](/2025/11/22/reduction-rudrata-path-to-spanning-tree-k-leaves/)
- [Rudrata Path to Spanning Tree with Exact Leaves](/2025/11/22/reduction-rudrata-path-to-spanning-tree-leaves/)
- [Rudrata (s,t)-Path to White and Gold Path](/2025/11/22/reduction-rudrata-st-path-to-white-gold-path/)
- [Rudrata Cycle to TSP](/2025/11/22/reduction-rudrata-cycle-to-tsp/)
- [Rudrata Cycle to Reliable Network](/2025/11/22/reduction-rudrata-cycle-to-reliable-network/)

### Reductions from Subset Sum

- [Reductions from Subset Sum](/2025/11/21/reductions-from-subset-sum/)
- [Subset Sum to Knapsack](/2025/11/22/reduction-subset-sum-to-knapsack/)

### Reductions from Other Problems

- [Reductions from Zero-One Equations (ZOE)](/2025/11/21/reductions-from-zoe/)
- [Reductions from 3D Matching](/2025/11/21/reductions-from-3d-matching/)
- [Reductions from ILP](/2025/11/21/reductions-from-ilp/)
- [Reductions from TSP](/2025/11/21/reductions-from-tsp/)

## Linear Programming (LP)

### LP Fundamentals

- [Linear Programming Fundamentals](/2025/11/21/linear-programming-fundamentals/) - LP basics, algorithms, duality, and applications
- [Integer Linear Programming (ILP) Introduction](/2025/11/16/np-hard-intro-integer-linear-programming/) - ILP as an NP-complete problem

### LP Reductions

- [Linear Programming Reductions](/2025/11/16/linear-programming-reductions/) - Reductions involving LP and ILP

## Greedy Algorithms

- [Greedy Algorithms Examples](/2025/11/16/greedy-algorithms-examples/) - Examples and applications of greedy algorithms

## Problem Relationships

### Graph Problems
- **Clique** ↔ **Independent Set** (complement graphs)
- **Vertex Cover** ↔ **Independent Set** (complement relationship)
- **Clique** → **Subgraph Isomorphism** → **Maximum Common Subgraph**
- **Vertex Cover** → **Hitting Set** → **Set Cover**
- **Rudrata Path** → **Longest Path**, **Spanning Tree variants**

### SAT Variants
- **SAT** → **3-SAT** → **Exact 4-SAT**
- **SAT** → **Stingy SAT**, **Max SAT**, **Circuit Design**

### Number Problems
- **Subset Sum** → **Partition** → **Knapsack**

### Network Problems
- **Rudrata Cycle** → **TSP** → **Reliable Network**

## Study Paths

### Starting with SAT
1. [SAT Introduction](/2025/11/16/np-hard-intro-sat/)
2. [3-SAT](/2025/11/16/np-hard-intro-sat/)
3. [Reductions from SAT](/2025/11/21/reductions-from-sat/)
4. [Reductions from 3-SAT](/2025/11/21/reductions-from-3sat/)

### Starting with Graph Problems
1. [Clique](/2025/11/16/np-hard-intro-clique/)
2. [Independent Set](/2025/11/16/np-hard-intro-independent-set/)
3. [Vertex Cover](/2025/11/16/np-hard-intro-vertex-cover/)
4. [Reductions from Clique](/2025/11/21/reductions-from-clique/)
5. [Reductions from Vertex Cover](/2025/11/21/reductions-from-vertex-cover/)

### Starting with Linear Programming
1. [Linear Programming Fundamentals](/2025/11/21/linear-programming-fundamentals/)
2. [Integer Linear Programming](/2025/11/16/np-hard-intro-integer-linear-programming/)
3. [Linear Programming Reductions](/2025/11/16/linear-programming-reductions/)

### Learning Reduction Techniques
1. [NP-Complete Reduction Examples](/2025/11/21/np-complete-reduction-examples/)
2. [NP-Complete Reduction Reference](/2025/11/21/np-complete-reduction-reference/)
3. Individual reduction posts for specific techniques

---

*Last updated: November 2025*

