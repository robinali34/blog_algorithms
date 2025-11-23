---
layout: post
title: "Reduction: Clique to Maximum Common Subgraph"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Clique to Maximum Common Subgraph, demonstrating that Maximum Common Subgraph is NP-complete."
---

## Introduction

This post provides a detailed proof that the Maximum Common Subgraph problem is NP-complete by reducing from Clique. Maximum Common Subgraph asks for sets of nodes to delete from two graphs such that the remaining graphs are identical and each has at least b nodes.

## Problem Definitions

### Maximum Common Subgraph Problem

**Input:** 
- Two graphs G₁ = (V₁, E₁) and G₂ = (V₂, E₂)
- A budget b ≥ 0

**Output:** YES if there exist two sets of nodes V₁' ⊆ V₁ and V₂' ⊆ V₂ whose deletion leaves at least b nodes in each graph, and makes the two graphs identical (isomorphic), NO otherwise.

**Note:** After deleting V₁' from G₁ and V₂' from G₂, the remaining graphs must be isomorphic and each must have at least b vertices.

### Clique Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains a clique of size k (set of k vertices, all pairwise adjacent), NO otherwise.

## 1. NP-Completeness Proof of Maximum Common Subgraph: Solution Validation

### Maximum Common Subgraph ∈ NP

**Verification Algorithm:**
Given a candidate solution (sets V₁' ⊆ V₁ and V₂' ⊆ V₂):
1. Check that |V₁ \ V₁'| ≥ b: O(1) time
2. Check that |V₂ \ V₂'| ≥ b: O(1) time
3. Construct remaining graphs G₁' and G₂' by deleting V₁' and V₂': O(n₁ + n₂ + m₁ + m₂) time
4. Check that G₁' and G₂' are isomorphic: O(n²) time using graph isomorphism algorithms

**Total Time:** O(n²), which is polynomial in input size.

**Conclusion:** Maximum Common Subgraph ∈ NP.

## 2. Reduce Clique to Maximum Common Subgraph

**Key Insight:** Given a Clique instance, we can reduce it to Maximum Common Subgraph by setting G₁ = G (the original graph) and G₂ = K_k (the complete graph on k vertices). Then G has a k-clique if and only if we can delete nodes from G₁ to make it isomorphic to K_k, which means the remaining subgraph is a k-clique.

**Hint:** If we set G₂ = K_k and b = k, then finding sets V₁' and V₂' such that the remaining graphs are identical means finding a subgraph of G₁ isomorphic to K_k. Since K_k is a complete graph on k vertices, this is exactly a k-clique.

### 2.1 Input Conversion

Given a Clique instance (G, k) where G = (V, E) with n vertices, we construct a Maximum Common Subgraph instance (G₁, G₂, b).

**Construction:**

**Step 1: Set First Graph**
- G₁ = G (the original graph)

**Step 2: Set Second Graph**
- G₂ = K_k (complete graph on k vertices)
- V₂ = {1, 2, ..., k}
- E₂ = {(i, j) : 1 ≤ i < j ≤ k} (all pairs are adjacent)

**Step 3: Set Budget**
- b = k (we want at least k nodes remaining in each graph)

**Step 4: Result**
- Maximum Common Subgraph instance: (G₁, G₂, k) where G₁ = G and G₂ = K_k

**Detailed Example:**

Consider Clique instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (1,3), (2,3), (2,4), (3,4)}
- k = 3 (want clique of size 3)

**Transformation:**
- G₁ = G (same graph)
- G₂ = K₃ (complete graph on 3 vertices)
  - Vertices: {a, b, c}
  - Edges: {(a,b), (a,c), (b,c)}
- b = 3

**Maximum Common Subgraph instance:** (G₁, K₃, 3)

### 2.2 Output Conversion

**Given:** Maximum Common Subgraph solution (sets V₁' ⊆ V₁ and V₂' ⊆ V₂ such that after deletion, graphs are identical and each has at least b = k nodes)

**Extract Clique:**
- Since G₂ = K_k has exactly k vertices, we must delete V₂' = ∅ (delete nothing)
- The remaining graph G₁' (after deleting V₁') must be isomorphic to K_k
- Since K_k is a complete graph on k vertices, G₁' is a k-clique
- The vertices of G₁' form a clique of size k in G

**Verify Satisfaction:**
- G₁' has k vertices: ✓ (since isomorphic to K_k)
- All pairs of vertices in G₁' are adjacent: ✓ (since G₁' is isomorphic to K_k, which is complete)
- Therefore, G₁' is a clique of size k in G

## 3. Correctness Justification

### 3.1 If Clique has a solution, then Maximum Common Subgraph has a solution

**Given:** Clique instance (G, k) has solution C (clique of size k).

**Construct Maximum Common Subgraph Solution:**
- Let G₁' be the subgraph of G₁ = G induced by vertices in C
- Since C is a clique, G₁' is isomorphic to K_k
- Set V₁' = V₁ \ C (delete all vertices not in the clique)
- Set V₂' = ∅ (delete nothing from K_k, since it already has exactly k vertices)
- After deletion:
  - G₁' has k vertices and is isomorphic to K_k
  - G₂' = G₂ = K_k (unchanged)
  - Both graphs are isomorphic (both are K_k)

**Verify Satisfaction:**
- `|V₁ \ V₁'|` = `|C|` = k ≥ b = k: ✓
- `|V₂ \ V₂'|` = `|V₂|` = k ≥ b = k: ✓
- G₁' is isomorphic to G₂': ✓ (both are K_k)
- Therefore, the solution is valid

**Conclusion:** Maximum Common Subgraph has a solution.

### 3.2a If Clique does not have a solution, then Maximum Common Subgraph has no solution

**Given:** Clique instance (G, k) has no solution (no clique of size k).

**Proof:**

**Assume:** Maximum Common Subgraph instance (G₁, G₂, k) where G₁ = G and G₂ = K_k has a solution (sets V₁', V₂').

**Extract Clique:**
- Since G₂ = K_k has exactly k vertices, to have at least k vertices remaining, we must have V₂' = ∅
- Therefore, G₂' = G₂ = K_k (unchanged)
- G₁' (after deleting V₁') must be isomorphic to G₂' = K_k
- Since G₁' is isomorphic to K_k, it is a complete graph on k vertices
- Therefore, G₁' is a k-clique in G₁ = G

**Contradiction:**
- G₁' is a k-clique in G
- This contradicts the assumption that G has no k-clique

**Conclusion:** Maximum Common Subgraph has no solution.

### 3.2b If Maximum Common Subgraph has a solution, then Clique has a solution

**Given:** Maximum Common Subgraph instance (G₁, G₂, k) where G₁ = G and G₂ = K_k has solution (sets V₁', V₂').

**Extract Clique:**
- Since G₂ = K_k has exactly k vertices, to have at least k vertices remaining, we must have V₂' = ∅
- Therefore, G₂' = G₂ = K_k
- G₁' (after deleting V₁') must be isomorphic to G₂' = K_k
- Since G₁' is isomorphic to K_k, it is a complete graph on k vertices
- The vertices of G₁' form a clique of size k in G₁ = G

**Verify Satisfaction:**

**Clique Requirements:**
- G₁' has exactly k vertices: ✓ (since isomorphic to K_k)
- All pairs of vertices in G₁' are adjacent: ✓ (since G₁' is isomorphic to K_k, which is complete)
- Therefore, G₁' is a clique of size k in G

**Key Insight:**
- Setting G₂ = K_k forces the common subgraph to be K_k
- Finding a subgraph of G₁ isomorphic to K_k is exactly finding a k-clique
- The budget b = k ensures we keep exactly k vertices (since K_k has k vertices)
- Therefore, any solution to Maximum Common Subgraph gives us a k-clique

**Conclusion:** The subgraph G₁' forms a clique of size k in G, so Clique has a solution.

## Polynomial Time Analysis

**Input Size:**
- Clique: Graph G with n vertices and m edges, integer k
- Maximum Common Subgraph: Graph G₁ with n vertices and m edges, graph G₂ = K_k with k vertices and k(k-1)/2 edges, budget b = k

**Construction Time:**
- Set G₁ = G: O(1) time
- Create K_k: O(k²) time (k vertices, k(k-1)/2 edges)
- Set b = k: O(1) time
- Total: O(k²) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Maximum Common Subgraph ∈ NP**: Solutions can be verified in polynomial time
2. **Clique ≤ₚ Maximum Common Subgraph**: Polynomial-time reduction exists
3. **Correctness**: Clique has solution ↔ Maximum Common Subgraph has solution

**Therefore, Maximum Common Subgraph is NP-complete.**

## Key Insights

1. **Pattern Graph**: Using K_k as G₂ forces the common subgraph to be a k-clique
2. **Isomorphism Constraint**: Requiring the graphs to be identical after deletion forces G₁' to be isomorphic to K_k
3. **Budget Constraint**: Setting b = k ensures we keep exactly k vertices
4. **Clique Characterization**: A subgraph isomorphic to K_k is exactly a k-clique
5. **Preservation**: The reduction preserves the clique structure through isomorphism requirements

## Practice Questions

1. **Modify the reduction** to reduce Independent Set to Maximum Common Subgraph. What would you use as G₂?
2. **Extend the reduction** to handle weighted graphs. How would you modify the problem?
3. **Consider the optimization version:** How would you reduce the optimization version of Clique to Maximum Common Subgraph?
4. **Prove the reverse reduction:** Can we reduce Maximum Common Subgraph to Clique? How?
5. **Investigate special cases:** For what graph classes is Maximum Common Subgraph polynomial-time solvable?

---

This reduction demonstrates how graph isomorphism constraints can encode clique-finding requirements, enabling reductions between subgraph matching and clique problems.

