---
layout: post
title: "Reduction: Independent Set to Sparse Subgraph"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Independent Set to Sparse Subgraph, demonstrating that Sparse Subgraph is NP-complete."
---

## Introduction

This post provides a detailed proof that the Sparse Subgraph problem is NP-complete by reducing from Independent Set. Sparse Subgraph asks for a set of vertices with at most a specified number of edges between them, generalizing the independent set problem.

## Problem Definitions

### Sparse Subgraph Problem

**Input:** 
- An undirected graph G = (V, E)
- Two integers a ≥ 1 and b ≥ 0

**Output:** YES if there exists a set of a vertices of G such that there are at most b edges between them, NO otherwise.

**Note:** The problem asks to find a set S ⊆ V with |S| = a such that the number of edges in the subgraph induced by S is at most b.

### Independent Set Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains an independent set of size k (set of k vertices with no edges between them), NO otherwise.

## 1. NP-Completeness Proof of Sparse Subgraph: Solution Validation

### Sparse Subgraph ∈ NP

**Verification Algorithm:**
Given a candidate solution (set S of a vertices):
1. Check that |S| = a: O(1) time
2. Count the number of edges in the subgraph induced by S: O(a²) time
3. Check if the count ≤ b: O(1) time

**Total Time:** O(a²), which is polynomial in input size.

**Conclusion:** Sparse Subgraph ∈ NP.

## 2. Reduce Independent Set to Sparse Subgraph

**Key Insight:** Given an Independent Set instance, we can reduce it to Sparse Subgraph by setting a = k and b = 0. An independent set of size k has exactly 0 edges (no pairs are adjacent). Therefore, finding a k-independent set is equivalent to finding a set of k vertices with at most 0 edges.

**Hint:** An independent set has no edges between its vertices. Therefore, a set of k vertices is an independent set if and only if it has 0 edges. Setting b = 0 ensures we find an independent set.

### 2.1 Input Conversion

Given an Independent Set instance (G, k) where G = (V, E) with n vertices, we construct a Sparse Subgraph instance (G', a, b).

**Construction:**

**Step 1: Copy Graph**
- G' = G (same graph, no changes)

**Step 2: Set Parameters**
- a = k (we want a set of k vertices)
- b = 0 (an independent set has no edges)

**Step 3: Result**
- Sparse Subgraph instance: (G', k, 0)

**Detailed Example:**

Consider Independent Set instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (4,1)}
- k = 2 (want independent set of size 2)

**Transformation:**
- G' = G (same graph)
- a = 2
- b = 0

**Sparse Subgraph instance:** (G', 2, 0)

**Note:** A set of 2 vertices with at most 0 edges must have exactly 0 edges, which means the two vertices are not adjacent (an independent set of size 2).

### 2.2 Output Conversion

**Given:** Sparse Subgraph solution (set S of a = k vertices with at most b = 0 edges)

**Extract Independent Set:**
- Use S as the independent set
- S has k vertices
- S has at most 0 edges
- Since the number of edges is non-negative, S has exactly 0 edges
- Therefore, no pairs of vertices in S are adjacent
- S is an independent set of size k

**Verify Satisfaction:**
- S has k vertices: ✓
- No pairs of vertices in S are adjacent: ✓ (since there are 0 edges)
- Therefore, S is an independent set of size k

## 3. Correctness Justification

### 3.1 If Independent Set has a solution, then Sparse Subgraph has a solution

**Given:** Independent Set instance (G, k) has solution I (independent set of size k).

**Construct Sparse Subgraph Solution:**
- Use I as the set S
- S = I has k = a vertices
- Since I is an independent set, no pairs of vertices in I are adjacent
- Therefore, I has exactly 0 edges
- Number of edges = 0 ≤ b = 0 ✓

**Verify Satisfaction:**
- S has a = k vertices: ✓ (since I has k vertices)
- S has at most b = 0 edges: ✓ (since I is an independent set)
- Therefore, S is a solution to Sparse Subgraph

**Conclusion:** Sparse Subgraph has a solution.

### 3.2a If Independent Set does not have a solution, then Sparse Subgraph has no solution

**Given:** Independent Set instance (G, k) has no solution (no independent set of size k).

**Proof:**

**Assume:** Sparse Subgraph instance (G', k, 0) has a solution (set S of k vertices with at most 0 edges).

**Extract Independent Set:**
- Use S as the candidate independent set
- S has k vertices
- S has at most 0 edges
- Since the number of edges is non-negative, S has exactly 0 edges
- Therefore, no pairs of vertices in S are adjacent
- S is an independent set of size k in G' = G

**Contradiction:**
- S is an independent set of size k in G
- This contradicts the assumption that G has no independent set of size k

**Conclusion:** Sparse Subgraph has no solution.

### 3.2b If Sparse Subgraph has a solution, then Independent Set has a solution

**Given:** Sparse Subgraph instance (G', k, 0) has solution (set S of k vertices with at most 0 edges).

**Extract Independent Set:**
- Use S as the independent set
- S has k vertices
- S has at most 0 edges
- Since the number of edges is non-negative, S has exactly 0 edges
- Therefore, no pairs of vertices in S are adjacent

**Verify Satisfaction:**

**Independent Set Requirements:**
- S has exactly k vertices: ✓ (by assumption)
- No pairs of vertices in S are adjacent: ✓ (since there are 0 edges)
- Therefore, S is an independent set of size k in G' = G

**Key Insight:**
- A set of k vertices can have at least 0 edges (when it forms an independent set)
- Requiring at most 0 edges forces the set to have exactly 0 edges
- This means no pairs are adjacent, which is exactly the definition of an independent set
- Therefore, any solution to Sparse Subgraph gives us an independent set

**Conclusion:** The set S forms an independent set of size k in G, so Independent Set has a solution.

## Polynomial Time Analysis

**Input Size:**
- Independent Set: Graph G with n vertices and m edges, integer k
- Sparse Subgraph: Graph G' with n vertices and m edges, integers a = k and b = 0

**Construction Time:**
- Copy graph G: O(n + m) time
- Set a = k: O(1) time
- Set b = 0: O(1) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Sparse Subgraph ∈ NP**: Solutions can be verified in polynomial time
2. **Independent Set ≤ₚ Sparse Subgraph**: Polynomial-time reduction exists
3. **Correctness**: Independent Set has solution ↔ Sparse Subgraph has solution

**Therefore, Sparse Subgraph is NP-complete.**

## Key Insights

1. **Edge Count Equivalence**: An independent set of size k has exactly 0 edges (the minimum possible)
2. **Minimum Constraint**: Requiring at most 0 edges forces exactly 0 edges (since that's the minimum)
3. **Independent Set Characterization**: A set of k vertices is an independent set if and only if it has 0 edges
4. **Preservation**: The reduction preserves the independent set structure through edge count requirements

## Practice Questions

1. **Modify the reduction** to reduce k-Independent Set to Sparse Subgraph for fixed k. Does the complexity change?
2. **Extend the reduction** to handle weighted graphs. How would you set the parameters?
3. **Consider the optimization version:** How would you reduce the optimization version of Independent Set to Sparse Subgraph?
4. **Prove the reverse reduction:** Can we reduce Sparse Subgraph to Independent Set? How?
5. **Investigate special cases:** For what values of a and b is Sparse Subgraph polynomial-time solvable?

---

This reduction demonstrates how edge sparsity constraints can encode independent set requirements, enabling reductions between graph structure problems.

