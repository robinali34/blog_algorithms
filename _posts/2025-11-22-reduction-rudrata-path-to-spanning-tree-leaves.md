---
layout: post
title: "Reduction: Rudrata Path to Spanning Tree with Exact Leaves"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Rudrata Path to the problem of finding a spanning tree with a specified set of leaves."
---

## Introduction

This post provides a detailed proof that the Spanning Tree with Exact Leaves problem is NP-complete by reducing from Rudrata Path. The problem asks whether a graph has a spanning tree whose set of leaves is precisely a given set L.

## Problem Definitions

### Spanning Tree with Exact Leaves Problem

**Input:** 
- An undirected graph G = (V, E)
- A set of nodes L ⊆ V

**Output:** YES if G has a spanning tree such that its set of leaves is precisely the set L, NO otherwise.

**Note:** The leaves of a tree are vertices of degree 1. The problem asks for a spanning tree where exactly the vertices in L are leaves, and all other vertices are not leaves.

### Rudrata Path Problem

**Input:** 
- An undirected graph G = (V, E)
- Two vertices s, t ∈ V

**Output:** YES if G contains a Rudrata (s,t)-path (path from s to t visiting all vertices exactly once), NO otherwise.

## 1. NP-Completeness Proof of Spanning Tree with L ≤ V Leaves: Solution Validation

### Spanning Tree with Exact Leaves ∈ NP

**Verification Algorithm:**
Given a candidate solution (spanning tree T):
1. Check that T is a tree: O(n) time (n-1 edges, connected)
2. Check that T spans all vertices: O(n) time
3. Identify leaves (vertices of degree 1): O(n) time
4. Check that the set of leaves equals L: O(n) time

**Total Time:** O(n), which is polynomial in input size.

**Conclusion:** Spanning Tree with Exact Leaves ∈ NP.

## 2. Reduce Rudrata Path to Spanning Tree with Exact Leaves

**Key Insight:** Given a Rudrata Path instance, we can reduce it to Spanning Tree with Exact Leaves by setting L = {s, t}. A Rudrata path has exactly 2 leaves (the endpoints s and t), and a spanning tree that is a path has exactly these two vertices as leaves.

**Hint:** A path has exactly 2 leaves (the endpoints). A spanning tree that is a path has exactly 2 leaves: the start and end vertices. Therefore, finding a Rudrata (s,t)-path is equivalent to finding a spanning tree with leaves exactly {s, t}.

### 2.1 Input Conversion

Given a Rudrata Path instance (G, s, t) where G = (V, E) and we want a path from s to t visiting all vertices, we construct a Spanning Tree with Exact Leaves instance (G', L).

**Construction:**

**Step 1: Copy Graph**
- G' = G (same graph, no changes)

**Step 2: Set Leaf Set**
- L = {s, t} (we want a spanning tree with exactly s and t as leaves)

**Step 3: Result**
- Spanning Tree with Exact Leaves instance: (G', {s, t})

**Detailed Example:**

Consider Rudrata Path instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (1,4)}
- s = 1, t = 4

**Transformation:**
- G' = G (same graph)
- L = {1, 4} (s = 1, t = 4)

**Spanning Tree with Exact Leaves instance:** (G', {1, 4})

### 2.2 Output Conversion

**Given:** Spanning Tree with Exact Leaves solution (spanning tree T with leaves exactly L = {s, t})

**Extract Rudrata Path:**
- Since T has exactly 2 leaves (s and t) and T is a tree on n vertices, T must be a path
- A tree with exactly 2 leaves is a path
- The path has endpoints s and t
- The path visits all vertices exactly once
- Therefore, T is a Rudrata (s,t)-path

**Verify Satisfaction:**
- T is a spanning tree: ✓
- T has leaves exactly {s, t}: ✓ (by assumption)
- T is a path: ✓ (since it has exactly 2 leaves)
- The path visits all vertices: ✓ (since T is spanning)
- The path goes from s to t: ✓ (since s and t are the endpoints)
- Therefore, T is a Rudrata (s,t)-path: ✓

## 3. Correctness Justification

### 3.1 If Rudrata Path has a solution, then Spanning Tree with Exact Leaves has a solution

**Given:** Rudrata Path instance (G, s, t) has solution P (path from s to t visiting all vertices).

**Construct Spanning Tree Solution:**
- Use path P as the spanning tree T
- P visits all n vertices exactly once
- P has exactly 2 leaves: s and t
- P is a tree (connected, n-1 edges)
- Therefore, T = P is a spanning tree with leaves exactly {s, t} = L

**Verify Satisfaction:**
- T spans all vertices: ✓ (since P visits all vertices)
- T is a tree: ✓ (path is a tree)
- T has leaves exactly {s, t}: ✓ (s and t are the endpoints, all other vertices have degree 2)
- Therefore, the set of leaves equals L = {s, t}: ✓

**Conclusion:** Spanning Tree with Exact Leaves has a solution.

### 3.2a If Rudrata Path does not have a solution, then Spanning Tree with Exact Leaves has no solution

**Given:** Rudrata Path instance (G, s, t) has no solution (no path from s to t visiting all vertices).

**Proof:**

**Assume:** Spanning Tree with Exact Leaves instance (G', {s, t}) has a solution (spanning tree T with leaves exactly {s, t}).

**Extract Path:**
- Since T has exactly 2 leaves (s and t) and T is a tree, T must be a path
- A tree with exactly 2 leaves is a path
- T has endpoints s and t
- T visits all n vertices exactly once
- T is a path from s to t in G' = G
- Therefore, T is a Rudrata (s,t)-path

**Contradiction:**
- T is a Rudrata (s,t)-path in G
- This contradicts the assumption that G has no Rudrata (s,t)-path

**Conclusion:** Spanning Tree with Exact Leaves has no solution.

### 3.2b If Spanning Tree with Exact Leaves has a solution, then Rudrata Path has a solution

**Given:** Spanning Tree with Exact Leaves instance (G', {s, t}) has solution (spanning tree T with leaves exactly {s, t}).

**Extract Path:**
- Since T has exactly 2 leaves (s and t) and T is a tree, T must be a path
- A tree with exactly 2 leaves is a path
- T has endpoints s and t
- T visits all n vertices exactly once
- T is a path from s to t in G' = G

**Verify Satisfaction:**

**Rudrata Path Requirements:**
- Path visits all n vertices exactly once: ✓ (since T is a path spanning all vertices)
- Path goes from s to t: ✓ (since s and t are the endpoints)
- All edges in path exist in G: ✓ (since T is a subgraph of G)
- Therefore, T is a Rudrata (s,t)-path

**Key Insight:**
- A spanning tree with exactly 2 leaves is a path
- If the leaves are exactly {s, t}, then the path goes from s to t
- A path visiting all vertices from s to t is exactly a Rudrata (s,t)-path
- Therefore, any solution to Spanning Tree with Exact Leaves gives us a Rudrata (s,t)-path

**Conclusion:** The spanning tree T forms a Rudrata (s,t)-path in G, so Rudrata Path has a solution.

## Polynomial Time Analysis

**Input Size:**
- Rudrata Path: Graph G with n vertices and m edges, vertices s and t
- Spanning Tree with Exact Leaves: Graph G' with n vertices and m edges, set L = {s, t}

**Construction Time:**
- Copy graph G: O(n + m) time
- Set L = {s, t}: O(1) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Spanning Tree with Exact Leaves ∈ NP**: Solutions can be verified in polynomial time
2. **Rudrata Path ≤ₚ Spanning Tree with Exact Leaves**: Polynomial-time reduction exists
3. **Correctness**: Rudrata Path has solution ↔ Spanning Tree with Exact Leaves has solution

**Therefore, Spanning Tree with Exact Leaves is NP-complete.**

## Key Insights

1. **Path Structure**: A tree with exactly 2 leaves is a path
2. **Spanning Property**: A spanning tree that is a path visits all vertices exactly once
3. **Exact Leaf Constraint**: Requiring exactly {s, t} as leaves forces the path to go from s to t
4. **Preservation**: The reduction preserves the path structure through the exact leaf set constraint

## Practice Questions

1. **Modify the reduction** to reduce Rudrata Cycle to Spanning Tree with Exact Leaves. What set L would you use?
2. **Extend the reduction** to handle directed graphs. How would the reduction change?
3. **Consider weighted versions:** How would you reduce weighted Rudrata Path to weighted Spanning Tree?
4. **Prove the reverse reduction:** Can we reduce Spanning Tree with Exact Leaves to Rudrata Path? How?
5. **Investigate special cases:** For what sets L is the problem polynomial-time solvable?

---

This reduction demonstrates how structural constraints (number of leaves) can capture path properties, enabling reductions between path-finding and tree problems.

