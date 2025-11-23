---
layout: post
title: "Reduction: Clique to Subgraph Isomorphism"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Clique to Subgraph Isomorphism, demonstrating that Subgraph Isomorphism is NP-complete."
---

## Introduction

This post provides a detailed proof that the Subgraph Isomorphism problem is NP-complete by reducing from Clique. Subgraph Isomorphism asks whether one graph is a subgraph of another (obtained by deleting certain vertices and edges), and if so, returns the corresponding vertex mapping.

## Problem Definitions

### Subgraph Isomorphism Problem

**Input:** 
- Two undirected graphs G = (V_G, E_G) and H = (V_H, E_H)

**Output:** YES if G is a subgraph of H (i.e., by deleting certain vertices and edges of H, we obtain a graph that is, up to renaming of vertices, identical to G), and if so, return the corresponding mapping of V(G) into V(H). NO otherwise.

**Note:** This means determining whether there exists a subgraph H' of H such that H' is isomorphic to G, and providing the isomorphism mapping f: V_G → V_H' ⊆ V_H.

### Clique Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains a clique of size k (set of k vertices, all pairwise adjacent), NO otherwise.

## 1. NP-Completeness Proof of Subgraph Isomorphism: Solution Validation

### Subgraph Isomorphism ∈ NP

**Verification Algorithm:**
Given a candidate solution (mapping f: V_G → V_H):
1. Check that f is injective (one-to-one): O(|V_G|) time
2. Check that f maps V_G into V_H: O(|V_G|) time
3. For each edge (u, v) ∈ E_G:
   - Check that (f(u), f(v)) ∈ E_H: O(1) time
4. Verify that the subgraph H' induced by f(V_G) is isomorphic to G:
   - Check that for each edge (u, v) ∈ E_G, (f(u), f(v)) ∈ E_H: O(|E_G|) time
   - Check that for each non-edge (u, v) ∉ E_G, (f(u), f(v)) ∉ E_H: O(|V_G|²) time

**Total Time:** O(|V_G|²), which is polynomial in input size.

**Conclusion:** Subgraph Isomorphism ∈ NP.

## 2. Reduce Clique to Subgraph Isomorphism

**Key Insight:** Given a Clique instance, we can reduce it to Subgraph Isomorphism by setting the pattern graph G to be a complete graph on k vertices (K_k), and the host graph H to be the original graph. Then the original graph contains a clique of size k if and only if K_k is a subgraph of H.

**Hint:** A clique of size k is exactly a complete graph on k vertices. Therefore, finding a k-clique in H is equivalent to determining whether K_k (as graph G) is a subgraph of H.

### 2.1 Input Conversion

Given a Clique instance (H, k) where H = (V_H, E_H) is the original graph and we want a clique of size k, we construct a Subgraph Isomorphism instance (G, H).

**Construction:**

**Step 1: Use Original Graph as Host**
- H remains the same (the original graph)

**Step 2: Create Pattern Graph**
- G = K_k (complete graph on k vertices)
- V_G = {1, 2, ..., k}
- E_G = {(i, j) : 1 ≤ i < j ≤ k} (all pairs of vertices are adjacent)

**Step 3: Result**
- Subgraph Isomorphism instance: (G, H) where G = K_k and H is the original graph

**Detailed Example:**

Consider Clique instance:
- Graph H with vertices {1, 2, 3, 4}
- Edges: {(1,2), (1,3), (2,3), (2,4), (3,4)}
- k = 3 (want clique of size 3)

**Transformation:**
- H remains the same (host graph)
- G = K₃ (pattern graph - complete graph on 3 vertices)
  - Vertices: {a, b, c}
  - Edges: {(a,b), (a,c), (b,c)}

**Subgraph Isomorphism instance:** (G, H) where G = K₃

**Question:** Is G (K₃) a subgraph of H? This is equivalent to asking if H contains a 3-clique.

### 2.2 Output Conversion

**Given:** Subgraph Isomorphism solution (mapping f: V_G → V_H showing that G = K_k is a subgraph of H)

**Extract Clique:**
- The image f(V_G) = {f(1), f(2), ..., f(k)} forms a set of k vertices in H
- Since G = K_k is complete, for every edge (i, j) in G, we have (f(i), f(j)) ∈ E_H
- Therefore, all pairs of vertices in f(V_G) are adjacent in H
- f(V_G) is a k-clique in H

**Verify Satisfaction:**
- f(V_G) has k vertices (since f is injective and |V_G| = k)
- All pairs of vertices in f(V_G) are adjacent in H (since G is complete and f preserves edges)
- Therefore, f(V_G) is a clique of size k in H

## 3. Correctness Justification

### 3.1 If Clique has a solution, then Subgraph Isomorphism has a solution

**Given:** Clique instance (G, k) has solution C (clique of size k).

**Construct Subgraph Isomorphism Solution:**
- Let C = {v₁, v₂, ..., vₖ} be the clique vertices in H
- Define mapping f: V_G → V_H where V_G = {1, 2, ..., k} (vertices of K_k)
- Set f(i) = vᵢ for i = 1, ..., k
- Since C is a clique, for every edge (i, j) in G = K_k, we have (f(i), f(j)) = (vᵢ, vⱼ) ∈ E_H
- Therefore, f shows that G = K_k is a subgraph of H

**Verify Satisfaction:**
- f is injective: ✓ (maps distinct vertices to distinct vertices)
- f maps V_G into V_H: ✓ (C ⊆ V_H)
- For each edge (i, j) ∈ E_G: (f(i), f(j)) ∈ E_H: ✓ (since C is a clique)
- Therefore, G = K_k is a subgraph of H with mapping f

**Conclusion:** Subgraph Isomorphism has a solution.

### 3.2a If Clique does not have a solution, then Subgraph Isomorphism has no solution

**Given:** Clique instance (G, k) has no solution (no clique of size k).

**Proof:**

**Assume:** Subgraph Isomorphism instance (G, H) where G = K_k has a solution (mapping f showing G is a subgraph of H).

**Extract Clique:**
- f(V_G) = {f(1), ..., f(k)} has k vertices in H (since f is injective)
- Since G = K_k is complete, for every pair (i, j) with i < j, we have (f(i), f(j)) ∈ E_H
- Therefore, f(V_G) forms a clique of size k in H

**Contradiction:**
- f(V_G) is a clique of size k in H
- This contradicts the assumption that H has no clique of size k

**Conclusion:** Subgraph Isomorphism has no solution.

### 3.2b If Subgraph Isomorphism has a solution, then Clique has a solution

**Given:** Subgraph Isomorphism instance (G, H) where G = K_k has solution (mapping f: V_G → V_H).

**Extract Clique:**
- Let C = f(V_G) = {f(1), f(2), ..., f(k)} be the image of the mapping
- C has k vertices (since f is injective and |V_G| = k)
- Since G = K_k is complete, for every edge (i, j) in G, we have (f(i), f(j)) ∈ E_H
- Therefore, all pairs of vertices in C are adjacent in H

**Verify Satisfaction:**

**Clique Requirements:**
- C has exactly k vertices: ✓ (by injectivity of f)
- All pairs of vertices in C are adjacent in H: ✓ (since G is complete and f preserves edges)
- Therefore, C is a clique of size k in H

**Key Insight:**
- A complete graph on k vertices (K_k) is exactly a clique of size k
- Determining whether K_k is a subgraph of H is equivalent to finding a k-clique in H
- The mapping f provides the vertex correspondence showing which k vertices form the clique
- Therefore, any solution to Subgraph Isomorphism gives us a clique of size k and its vertex mapping

**Conclusion:** The set C forms a clique of size k in G, so Clique has a solution.

## Polynomial Time Analysis

**Input Size:**
- Clique: Graph H with n vertices and m edges, integer k
- Subgraph Isomorphism: Graph H with n vertices and m edges, graph G = K_k with k vertices and k(k-1)/2 edges

**Construction Time:**
- Use graph H as is: O(1) time
- Create K_k: O(k²) time (k vertices, k(k-1)/2 edges)
- Total: O(k²) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Subgraph Isomorphism ∈ NP**: Solutions can be verified in polynomial time
2. **Clique ≤ₚ Subgraph Isomorphism**: Polynomial-time reduction exists
3. **Correctness**: Clique has solution ↔ Subgraph Isomorphism has solution

**Therefore, Subgraph Isomorphism is NP-complete.**

## Key Insights

1. **Complete Graph Pattern**: Using K_k as the pattern graph captures the clique structure
2. **Isomorphism Equivalence**: A clique is exactly a complete graph, so finding a subgraph isomorphic to K_k is equivalent to finding a clique
3. **Preservation**: The reduction preserves the clique structure through isomorphism
4. **Pattern Construction**: The pattern graph H = K_k encodes the clique requirement

## Practice Questions

1. **Modify the reduction** to reduce Independent Set to Subgraph Isomorphism. What pattern graph would you use?
2. **Extend the reduction** to reduce k-Clique to Subgraph Isomorphism for fixed k. Does the complexity change?
3. **Consider induced subgraphs:** What if we require G' to be an induced subgraph? Does the reduction still work?
4. **Prove the reverse reduction:** Can we reduce Subgraph Isomorphism to Clique? How?
5. **Investigate special cases:** For what classes of pattern graphs H is Subgraph Isomorphism polynomial-time solvable?

---

This reduction demonstrates how pattern matching (subgraph isomorphism) can capture structural properties (cliques), enabling reductions between graph problems.

