---
layout: post
title: "Reduction: Clique to Dense Subgraph"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Clique to Dense Subgraph, demonstrating that Dense Subgraph is NP-complete."
---

## Introduction

This post provides a detailed proof that the Dense Subgraph problem is NP-complete by reducing from Clique. Dense Subgraph asks for a set of vertices with a specified number of edges between them, generalizing the clique problem.

## Problem Definitions

### Dense Subgraph Problem

**Input:** 
- An undirected graph G = (V, E)
- Two integers a ≥ 1 and b ≥ 0

**Output:** YES if there exists a set of a vertices of G such that there are at least b edges between them, NO otherwise.

**Note:** The problem asks to find a set S ⊆ V with `|S|` = a such that the number of edges in the subgraph induced by S is at least b.

### Clique Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains a clique of size k (set of k vertices, all pairwise adjacent), NO otherwise.

## 1. NP-Completeness Proof of Dense Subgraph: Solution Validation

### Dense Subgraph ∈ NP

**Verification Algorithm:**
Given a candidate solution (set S of a vertices):
1. Check that `|S|` = a: O(1) time
2. Count the number of edges in the subgraph induced by S: O(n²) time (since a ≤ n)
3. Check if the count ≥ b: O(1) time

**Total Time:** O(n²), which is polynomial in input size.

**Conclusion:** Dense Subgraph ∈ NP.

## 2. Reduce Clique to Dense Subgraph

**Key Insight:** Given a Clique instance, we can reduce it to Dense Subgraph by setting a = k and b = k(k-1)/2. A clique of size k has exactly k(k-1)/2 edges (all pairs are adjacent). Therefore, finding a k-clique is equivalent to finding a set of k vertices with at least k(k-1)/2 edges.

**Hint:** A complete graph on k vertices has k(k-1)/2 edges. Therefore, a set of k vertices forms a clique if and only if it has exactly k(k-1)/2 edges. Setting b = k(k-1)/2 ensures we find a clique.

### 2.1 Input Conversion

Given a Clique instance (G, k) where G = (V, E) with n vertices, we construct a Dense Subgraph instance (G', a, b).

**Construction:**

**Step 1: Copy Graph**
- G' = G (same graph, no changes)

**Step 2: Set Parameters**
- a = k (we want a set of k vertices)
- b = k(k-1)/2 (a clique of size k has exactly this many edges)

**Step 3: Result**
- Dense Subgraph instance: (G', k, k(k-1)/2)

**Detailed Example:**

Consider Clique instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (1,3), (2,3), (2,4), (3,4)}
- k = 3 (want clique of size 3)

**Transformation:**
- G' = G (same graph)
- a = 3
- b = 3(3-1)/2 = 3

**Dense Subgraph instance:** (G', 3, 3)

**Note:** A set of 3 vertices with at least 3 edges must have exactly 3 edges, which means all pairs are adjacent (a 3-clique).

### 2.2 Output Conversion

**Given:** Dense Subgraph solution (set S of a = k vertices with at least b = k(k-1)/2 edges)

**Extract Clique:**
- Use S as the clique
- S has k vertices
- S has at least k(k-1)/2 edges
- Since a set of k vertices can have at most k(k-1)/2 edges (complete graph), S has exactly k(k-1)/2 edges
- Therefore, all pairs of vertices in S are adjacent
- S is a clique of size k

**Verify Satisfaction:**
- S has k vertices: ✓
- All pairs of vertices in S are adjacent: ✓ (since there are k(k-1)/2 edges)
- Therefore, S is a clique of size k

## 3. Correctness Justification

### 3.1 If Clique has a solution, then Dense Subgraph has a solution

**Given:** Clique instance (G, k) has solution C (clique of size k).

**Construct Dense Subgraph Solution:**
- Use C as the set S
- S = C has k = a vertices
- Since C is a clique, all pairs of vertices in C are adjacent
- Therefore, C has exactly k(k-1)/2 edges
- Number of edges = k(k-1)/2 ≥ b = k(k-1)/2 ✓

**Verify Satisfaction:**
- S has a = k vertices: ✓ (since C has k vertices)
- S has at least b = k(k-1)/2 edges: ✓ (since C is a clique)
- Therefore, S is a solution to Dense Subgraph

**Conclusion:** Dense Subgraph has a solution.

### 3.2a If Clique does not have a solution, then Dense Subgraph has no solution

**Given:** Clique instance (G, k) has no solution (no clique of size k).

**Proof:**

**Assume:** Dense Subgraph instance (G', k, k(k-1)/2) has a solution (set S of k vertices with at least k(k-1)/2 edges).

**Extract Clique:**
- Use S as the candidate clique
- S has k vertices
- S has at least k(k-1)/2 edges
- Since a set of k vertices can have at most k(k-1)/2 edges, S has exactly k(k-1)/2 edges
- Therefore, all pairs of vertices in S are adjacent
- S is a clique of size k in G' = G

**Contradiction:**
- S is a clique of size k in G
- This contradicts the assumption that G has no clique of size k

**Conclusion:** Dense Subgraph has no solution.

### 3.2b If Dense Subgraph has a solution, then Clique has a solution

**Given:** Dense Subgraph instance (G', k, k(k-1)/2) has solution (set S of k vertices with at least k(k-1)/2 edges).

**Extract Clique:**
- Use S as the clique
- S has k vertices
- S has at least k(k-1)/2 edges
- Since a set of k vertices can have at most k(k-1)/2 edges (complete graph), S has exactly k(k-1)/2 edges
- Therefore, all pairs of vertices in S are adjacent

**Verify Satisfaction:**

**Clique Requirements:**
- S has exactly k vertices: ✓ (by assumption)
- All pairs of vertices in S are adjacent: ✓ (since there are k(k-1)/2 edges, which is the maximum)
- Therefore, S is a clique of size k in G' = G

**Key Insight:**
- A set of k vertices can have at most k(k-1)/2 edges (when it forms a complete graph)
- Requiring at least k(k-1)/2 edges forces the set to have exactly k(k-1)/2 edges
- This means all pairs must be adjacent, which is exactly the definition of a clique
- Therefore, any solution to Dense Subgraph gives us a clique

**Conclusion:** The set S forms a clique of size k in G, so Clique has a solution.

## Polynomial Time Analysis

**Input Size:**
- Clique: Graph G with n vertices and m edges, integer k
- Dense Subgraph: Graph G' with n vertices and m edges, integers a = k and b = k(k-1)/2

**Construction Time:**
- Copy graph G: O(n + m) time
- Set a = k: O(1) time
- Set b = k(k-1)/2: O(1) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Dense Subgraph ∈ NP**: Solutions can be verified in polynomial time
2. **Clique ≤ₚ Dense Subgraph**: Polynomial-time reduction exists
3. **Correctness**: Clique has solution ↔ Dense Subgraph has solution

**Therefore, Dense Subgraph is NP-complete.**

## Key Insights

1. **Edge Count Equivalence**: A clique of size k has exactly k(k-1)/2 edges (the maximum possible)
2. **Maximum Constraint**: Requiring at least k(k-1)/2 edges forces exactly k(k-1)/2 edges (since that's the maximum)
3. **Clique Characterization**: A set of k vertices is a clique if and only if it has k(k-1)/2 edges
4. **Preservation**: The reduction preserves the clique structure through edge count requirements

## Practice Questions

1. **Modify the reduction** to reduce k-Clique to Dense Subgraph for fixed k. Does the complexity change?
2. **Extend the reduction** to handle weighted graphs. How would you set the parameters?
3. **Consider the optimization version:** How would you reduce the optimization version of Clique to Dense Subgraph?
4. **Prove the reverse reduction:** Can we reduce Dense Subgraph to Clique? How?
5. **Investigate special cases:** For what values of a and b is Dense Subgraph polynomial-time solvable?

---

This reduction demonstrates how edge density constraints can encode clique requirements, enabling reductions between graph structure problems.

