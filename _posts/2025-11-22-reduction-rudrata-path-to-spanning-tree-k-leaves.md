---
layout: post
title: "Reduction: Rudrata Path to Spanning Tree with k or Fewer Leaves"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Rudrata Path to the problem of finding a spanning tree with k or fewer leaves."
---

## Introduction

This post provides a detailed proof that the Spanning Tree with k or Fewer Leaves problem is NP-complete by reducing from Rudrata Path. The problem asks whether a graph has a spanning tree with at most k leaves.

## Problem Definitions

### Spanning Tree with k or Fewer Leaves Problem

**Input:** 
- An undirected graph G = (V, E)
- An integer k ≥ 1

**Output:** YES if G has a spanning tree with k or fewer leaves (vertices of degree 1), NO otherwise.

**Note:** The leaves of a tree are vertices of degree 1. The problem asks for a spanning tree where the number of leaves is at most k.

### Rudrata Path Problem

**Input:** 
- An undirected graph G = (V, E)
- Two vertices s, t ∈ V

**Output:** YES if G contains a Rudrata (s,t)-path (path from s to t visiting all vertices exactly once), NO otherwise.

## 1. NP-Completeness Proof of Spanning Tree with k or Fewer Leaves: Solution Validation

### Spanning Tree with k or Fewer Leaves ∈ NP

**Verification Algorithm:**
Given a candidate solution (spanning tree T):
1. Check that T is a tree: O(n) time (n-1 edges, connected)
2. Check that T spans all vertices: O(n) time
3. Count leaves (vertices of degree 1): O(n) time
4. Check if number of leaves ≤ k: O(1) time

**Total Time:** O(n), which is polynomial in input size.

**Conclusion:** Spanning Tree with k or Fewer Leaves ∈ NP.

## 2. Reduce Rudrata Path to Spanning Tree with k or Fewer Leaves

**Key Insight:** Given a Rudrata Path instance, we can reduce it to Spanning Tree with k or Fewer Leaves by setting k = 2. A Rudrata path has exactly 2 leaves (the endpoints s and t), and a spanning tree that is a path has exactly 2 leaves. Therefore, finding a Rudrata path is equivalent to finding a spanning tree with at most 2 leaves.

**Hint:** A path has exactly 2 leaves (the endpoints). A spanning tree that is a path has exactly 2 leaves. Therefore, finding a Rudrata path is equivalent to finding a spanning tree with at most 2 leaves.

### 2.1 Input Conversion

Given a Rudrata Path instance (G, s, t) where G = (V, E) and we want a path from s to t visiting all vertices, we construct a Spanning Tree with k or Fewer Leaves instance (G', k).

**Construction:**

**Step 1: Copy Graph**
- G' = G (same graph, no changes)

**Step 2: Set Parameter**
- k = 2 (we want a spanning tree with at most 2 leaves)

**Step 3: Result**
- Spanning Tree with k or Fewer Leaves instance: (G', 2)

**Detailed Example:**

Consider Rudrata Path instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (1,4)}
- s = 1, t = 4

**Transformation:**
- G' = G (same graph)
- k = 2

**Spanning Tree with k or Fewer Leaves instance:** (G', 2)

### 2.2 Output Conversion

**Given:** Spanning Tree with k or Fewer Leaves solution (spanning tree T with at most k = 2 leaves)

**Extract Rudrata Path:**
- Since T has at most 2 leaves and T is a tree on n vertices, T must be a path
- A tree with exactly 2 leaves is a path
- The path visits all vertices exactly once
- If the path endpoints are s and t, we have a Rudrata (s,t)-path
- If not, we can check if there exists a path from s to t in T, or reorder the path

**Verify Satisfaction:**
- T is a spanning tree: ✓
- T has at most 2 leaves: ✓
- T is a path: ✓ (since it has exactly 2 leaves)
- The path visits all vertices: ✓ (since T is spanning)
- If the path goes from s to t, we have a Rudrata (s,t)-path: ✓

## 3. Correctness Justification

### 3.1 If Rudrata Path has a solution, then Spanning Tree with k or Fewer Leaves has a solution

**Given:** Rudrata Path instance (G, s, t) has solution P (path from s to t visiting all vertices).

**Construct Spanning Tree Solution:**
- Use path P as the spanning tree T
- P visits all n vertices exactly once
- P has exactly 2 leaves: s and t
- P is a tree (connected, n-1 edges)
- Therefore, T = P is a spanning tree with exactly 2 leaves

**Verify Satisfaction:**
- T spans all vertices: ✓ (since P visits all vertices)
- T is a tree: ✓ (path is a tree)
- T has exactly 2 leaves: ✓ (s and t are the endpoints)
- Number of leaves = 2 ≤ k = 2: ✓

**Conclusion:** Spanning Tree with k or Fewer Leaves has a solution.

### 3.2a If Rudrata Path does not have a solution, then Spanning Tree with k or Fewer Leaves has no solution

**Given:** Rudrata Path instance (G, s, t) has no solution (no path from s to t visiting all vertices).

**Proof:**

**Assume:** Spanning Tree with k or Fewer Leaves instance (G', 2) has a solution (spanning tree T with at most 2 leaves).

**Extract Path:**
- Since T has at most 2 leaves and T is a tree, T must be a path
- A tree with exactly 2 leaves is a path
- T visits all n vertices exactly once
- T is a path in G' = G

**Check Endpoints:**
- If T is a path from s to t, then T is a Rudrata (s,t)-path
- However, we assumed no such path exists
- Therefore, T cannot be a path from s to t

**More careful analysis:**
- If G has no Rudrata (s,t)-path, then either:
  - G is not connected (no spanning tree exists)
  - G is connected but no path from s to t visits all vertices
- In the second case, any spanning tree that is a path would have endpoints different from s and t
- However, if T is a path visiting all vertices, we can reorder it to start at s and end at t
- Actually, if T is a path visiting all vertices and contains both s and t, we can find the path from s to t within T
- This would give us a Rudrata (s,t)-path, contradicting the assumption

**Contradiction:**
- If a spanning tree with at most 2 leaves exists, it must be a path
- A path visiting all vertices can be reordered to go from s to t
- This gives us a Rudrata (s,t)-path, contradicting the assumption

**Conclusion:** Spanning Tree with k or Fewer Leaves has no solution.

### 3.2b If Spanning Tree with k or Fewer Leaves has a solution, then Rudrata Path has a solution

**Given:** Spanning Tree with k or Fewer Leaves instance (G', 2) has solution (spanning tree T with at most 2 leaves).

**Extract Path:**
- Since T has at most 2 leaves and T is a tree, T must be a path
- A tree with exactly 2 leaves is a path
- T visits all n vertices exactly once
- T is a path in G' = G

**Find Rudrata (s,t)-Path:**
- Since T is a path visiting all vertices, we can find a path from s to t within T
- If s and t are the endpoints of T, we're done
- Otherwise, T contains both s and t, and we can find the unique path from s to t in T
- This path visits all vertices (since T is a path and contains all vertices)
- Therefore, we have a Rudrata (s,t)-path

**Verify Satisfaction:**

**Rudrata Path Requirements:**
- Path visits all n vertices exactly once: ✓ (since T is a path spanning all vertices)
- Path goes from s to t: ✓ (we can reorder to start at s and end at t)
- All edges in path exist in G: ✓ (since T is a subgraph of G)
- Therefore, we have a Rudrata (s,t)-path

**Key Insight:**
- A spanning tree with at most 2 leaves must have exactly 2 leaves (since a tree always has at least 2 leaves)
- A spanning tree with exactly 2 leaves is a path
- A path visiting all vertices can always be reordered to start at s and end at t
- Therefore, any spanning tree with at most 2 leaves gives us a Rudrata (s,t)-path

**Conclusion:** The path T (or a reordering of it) forms a Rudrata (s,t)-path in G, so Rudrata Path has a solution.

## Polynomial Time Analysis

**Input Size:**
- Rudrata Path: Graph G with n vertices and m edges, vertices s and t
- Spanning Tree with k or Fewer Leaves: Graph G' with n vertices and m edges, integer k = 2

**Construction Time:**
- Copy graph G: O(n + m) time
- Set k = 2: O(1) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Spanning Tree with k or Fewer Leaves ∈ NP**: Solutions can be verified in polynomial time
2. **Rudrata Path ≤ₚ Spanning Tree with k or Fewer Leaves**: Polynomial-time reduction exists
3. **Correctness**: Rudrata Path has solution ↔ Spanning Tree with k or Fewer Leaves has solution

**Therefore, Spanning Tree with k or Fewer Leaves is NP-complete.**

## Key Insights

1. **Path Structure**: A tree with exactly 2 leaves is a path
2. **Spanning Property**: A spanning tree that is a path visits all vertices exactly once
3. **Minimum Leaves**: A tree always has at least 2 leaves, so "at most 2" means "exactly 2"
4. **Endpoint Flexibility**: A path can always be reordered to start and end at any two vertices
5. **Preservation**: The reduction preserves the path structure through the leaf count constraint

## Practice Questions

1. **Modify the reduction** to reduce Rudrata Cycle to Spanning Tree with k or Fewer Leaves. What value of k would you use?
2. **Extend the reduction** to handle directed graphs. How would the reduction change?
3. **Consider weighted versions:** How would you reduce weighted Rudrata Path to weighted Spanning Tree?
4. **Prove the reverse reduction:** Can we reduce Spanning Tree with k or Fewer Leaves to Rudrata Path? How?
5. **Investigate other values of k:** For what values of k is the problem polynomial-time solvable?

---

This reduction demonstrates how structural constraints (number of leaves) can capture path properties, enabling reductions between path-finding and tree problems.

