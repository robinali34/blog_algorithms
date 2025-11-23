---
layout: post
title: "Reduction: Clique to Kite"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Clique to Kite, demonstrating that Kite is NP-complete."
---

## Introduction

This post provides a detailed proof that the Kite problem is NP-complete by reducing from Clique. A kite is a graph structure consisting of a clique with a path (tail) attached, and the problem asks whether a graph contains such a structure of a given size.

## Problem Definitions

### Kite Problem

**Input:** 
- An undirected graph G = (V, E)
- A goal g ≥ 1

**Output:** YES if G contains a subgraph which is a kite and which contains 2g nodes, NO otherwise.

**Kite Definition:** A kite is a graph on an even number of vertices, say 2n, in which:
- n vertices form a clique
- The remaining n vertices are connected in a "tail" that consists of a path
- The tail is joined to one of the vertices of the clique

**Note:** The kite has exactly 2n = 2g vertices total, with n = g vertices in the clique and n = g vertices in the tail.

### Clique Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains a clique of size k (set of k vertices, all pairwise adjacent), NO otherwise.

## 1. NP-Completeness Proof of Kite: Solution Validation

### Kite ∈ NP

**Verification Algorithm:**
Given a candidate solution (subgraph K with 2g vertices):
1. Check that K has exactly 2g vertices: O(1) time
2. Identify the clique part (g vertices): O(g²) time
3. Check that the clique part forms a clique: O(g²) time
4. Identify the tail part (g vertices): O(g) time
5. Check that the tail forms a path: O(g) time
6. Check that the tail is connected to exactly one vertex of the clique: O(1) time

**Total Time:** O(g²), which is polynomial in input size.

**Conclusion:** Kite ∈ NP.

## 2. Reduce Clique to Kite

**Key Insight:** Given a Clique instance, we can reduce it to Kite by adding a tail of k vertices to the graph, with the tail's first vertex connected to all original vertices. Then G has a k-clique if and only if the modified graph has a kite with 2k vertices.

**Hint:** For a kite with 2g = 2k vertices, we need k vertices forming a clique and k vertices forming a tail. If G has a k-clique C, we can use C as the clique part and attach the tail to any vertex in C. We construct G' by adding a tail of k vertices, with the first tail vertex connected to all original vertices, allowing the tail to attach to any vertex.

### 2.1 Input Conversion

Given a Clique instance (G, k) where G = (V, E) with n vertices, we construct a Kite instance (G', g).

**Construction:**

**Step 1: Add Tail**
- Add k new vertices: t₁, t₂, ..., tₖ
- Connect them to form a path: (t₁, t₂), (t₂, t₃), ..., (tₖ₋₁, tₖ)
- Connect t₁ to all vertices in V (the tail's first vertex connects to all original vertices)
- This allows the tail to attach to any vertex in the original graph

**Step 2: Set Goal**
- g = k (we want a kite with 2k = 2g vertices)

**Step 3: Result**
- Kite instance: (G', k) where G' has the original graph plus the tail

**Detailed Example:**

Consider Clique instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (1,3), (2,3), (2,4), (3,4)}
- k = 3 (want clique of size 3)

**Transformation:**
- Add tail vertices: t₁, t₂, t₃
- Add tail edges: (t₁, t₂), (t₂, t₃)
- Connect t₁ to all original vertices: (t₁,1), (t₁,2), (t₁,3), (t₁,4)
- g = 3

**Kite instance:** (G', 3)

### 2.2 Output Conversion

**Given:** Kite solution (subgraph K that is a kite with 2g = 2k vertices)

**Extract Clique:**
- The kite K has 2k vertices: k in the clique part and k in the tail part
- The tail vertices t₁, ..., tₖ form a path, so they cannot be part of the clique part
- Therefore, the clique part must consist entirely of k vertices from the original graph G
- These k vertices form a clique in G (since edges are preserved from G to G')

**Verify Satisfaction:**
- We extract k vertices from G that form a clique
- Therefore, G has a clique of size k

## 3. Correctness Justification

### 3.1 If Clique has a solution, then Kite has a solution

**Given:** Clique instance (G, k) has solution C (clique of size k in G).

**Construct Kite Solution:**

**Kite Construction:**
- Clique part: Use all k vertices from C (the original clique)
- Tail part: Use the tail vertices t₁, t₂, ..., tₖ
- Tail attachment: The tail attaches to any vertex v ∈ C via t₁ (since t₁ is connected to all vertices in V, including v)
- Total: 2k vertices ✓

**Verify Satisfaction:**
- Clique part has k vertices forming a clique: ✓ (C is a clique)
- Tail part has k vertices forming a path: ✓ (t₁-t₂-...-tₖ is a path)
- Tail attaches to exactly one vertex of the clique: ✓ (via t₁ connected to v)
- Total vertices: 2k = 2g ✓

**Conclusion:** Kite has a solution.

### 3.2a If Clique does not have a solution, then Kite has no solution

**Given:** Clique instance (G, k) has no solution (no clique of size k).

**Proof:**

**Assume:** Kite instance (G', k) has a solution (kite K with 2k vertices).

**Extract Clique:**
- K has 2k vertices: k in clique part, k in tail part
- The tail vertices t₁, ..., tₖ form a path (not a clique)
- Therefore, the clique part must consist entirely of k vertices from the original graph G
- These k vertices form a clique in G (since edges are preserved from G to G')

**Contradiction:**
- If a kite exists, the clique part is a k-clique in G
- This contradicts the assumption that G has no k-clique

**Conclusion:** Kite has no solution.

### 3.2b If Kite has a solution, then Clique has a solution

**Given:** Kite instance (G', k) has solution (kite K with 2k vertices).

**Extract Clique:**
- K has 2k vertices: k in clique part, k in tail part
- The clique part consists of k vertices
- Since tail vertices form a path (not a clique), the clique part must consist of vertices from the original graph G
- These k vertices form a clique in G (since edges are preserved from G to G')

**Verify Satisfaction:**

**Clique Requirements:**
- We have k vertices from G: ✓
- These k vertices form a clique: ✓ (they form the clique part of the kite)
- Therefore, G has a clique of size k

**Key Insight:**
- A kite requires exactly k vertices forming a clique
- The tail vertices form a path, not a clique
- Therefore, the clique part must come from the original graph G
- Any kite solution gives us a k-clique in G

**Conclusion:** The k vertices from the clique part form a clique of size k in G, so Clique has a solution.

## Polynomial Time Analysis

**Input Size:**
- Clique: Graph G with n vertices and m edges, integer k
- Kite: Graph G' with n + k + 1 vertices (original n + tail k + possibly u) and m + O(n + k) edges, goal g = k

**Construction Time:**
- Add tail vertices: O(k) time
- Connect tail as path: O(k) time
- Connect t₁ to all vertices in V: O(n) time
- Set g = k: O(1) time
- Total: O(n + k) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Kite ∈ NP**: Solutions can be verified in polynomial time
2. **Clique ≤ₚ Kite**: Polynomial-time reduction exists
3. **Correctness**: Clique has solution ↔ Kite has solution

**Therefore, Kite is NP-complete.**

## Key Insights

1. **Kite Structure**: A kite combines a clique (k vertices) with a tail path (k vertices), totaling 2k vertices
2. **Clique Extraction**: The clique part of any kite must come from the original graph, since tail vertices form a path
3. **Tail Attachment**: The tail can attach to any vertex via t₁, which is connected to all original vertices
4. **Preservation**: The reduction preserves clique structure while adding the necessary tail structure

## Practice Questions

1. **Modify the reduction** to use a different tail attachment strategy. What if we only connect t₁ to a subset of vertices?
2. **Analyze alternative constructions:** Could we use a shorter tail? What's the minimum tail length needed?
3. **Consider induced subgraphs:** What if we require the kite to be an induced subgraph? Does the reduction still work?
4. **Prove the reverse reduction:** Can we reduce Kite to Clique? How?
5. **Investigate special cases:** For what graph classes is Kite polynomial-time solvable?

---

This reduction demonstrates how structural constraints (kite = clique + tail) can encode clique-finding problems, enabling reductions between graph structure problems.

