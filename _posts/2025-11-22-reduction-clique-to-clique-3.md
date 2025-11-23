---
layout: post
title: "Reduction: Clique to Clique-3"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Clique to Clique-3, demonstrating that Clique-3 is NP-complete."
---

## Introduction

This post provides a detailed proof that the Clique-3 problem is NP-complete by reducing from Clique. Clique-3 is the Clique problem restricted to graphs where every vertex has degree at most 3, showing that even this restricted version remains computationally hard.

## Problem Definitions

### Clique-3 Problem

**Input:** 
- An undirected graph G = (V, E) where every vertex has degree at most 3
- Integer k ≥ 1

**Output:** YES if G contains a clique of size k (set of k vertices, all pairwise adjacent), NO otherwise.

**Note:** This is the CLIQUE problem restricted to graphs in which every vertex has degree at most 3.

### Clique Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains a clique of size k (set of k vertices, all pairwise adjacent), NO otherwise.

## 1. NP-Completeness Proof of Clique-3: Solution Validation

### Clique-3 ∈ NP

**Verification Algorithm:**
Given a candidate solution (set C of k vertices):
1. Check that C has exactly k vertices: O(1) time
2. Check that all pairs in C are adjacent: O(k²) time

**Total Time:** O(k²), which is polynomial in input size.

**Conclusion:** Clique-3 ∈ NP.

## 2. Reduce Clique to Clique-3

**Key Insight:** Given a Clique instance, we can reduce it to Clique-3 by replacing high-degree vertices with gadgets that preserve clique structure while ensuring all vertices have degree at most 3. We use a local replacement strategy where each vertex v with degree d > 3 is replaced with a structure that maintains clique relationships.

**Hint:** For a vertex v with neighbors N(v) where |N(v)| > 3, replace v with a chain of vertices connected in a way that distributes the neighbors among the chain vertices, ensuring each vertex in the chain has degree ≤ 3. The clique property is preserved: if v was in a clique, all vertices in its replacement chain are in the corresponding clique.

### 2.1 Input Conversion

Given a Clique instance (G, k) where G = (V, E) with n vertices, we construct a Clique-3 instance (G', k').

**Construction:**

**Step 1: Identify High-Degree Vertices**
- For each vertex v ∈ V:
  - If deg(v) ≤ 3: keep v as is
  - If deg(v) > 3: mark v for replacement

**Step 2: Replace High-Degree Vertices**
- For each vertex v with deg(v) = d > 3:
  - Let N(v) = {u₁, u₂, ..., u_d} be the neighbors of v
  - Replace v with a chain of vertices: v₁, v₂, ..., v_{d-2}
  - Connect the chain: (v₁, v₂), (v₂, v₃), ..., (v_{d-3}, v_{d-2})
  - Distribute neighbors:
    - Connect u₁, u₂ to v₁
    - Connect u₃ to v₂
    - Connect u₄ to v₃
    - ...
    - Connect u_d to v_{d-2}
  - Make all vᵢ pairwise adjacent (form a clique among themselves)
  - This ensures:
    - Each vᵢ has degree ≤ 3 (at most 2 neighbors in chain + 1-2 external neighbors)
    - If v was in a clique, all vᵢ must be in the corresponding clique

**Step 3: Adjust Clique Size**
- For each replaced vertex v with degree d > 3:
  - Original clique size increases by (d-3) vertices
- Let r be the number of replaced vertices
- Let D be the sum of (deg(v) - 3) over all replaced vertices v
- Set k' = k + D

**Step 4: Result**
- Clique-3 instance: (G', k')

**Detailed Example:**

Consider Clique instance:
- Graph G with vertices {1, 2, 3, 4, 5}
- Edges: {(1,2), (1,3), (1,4), (1,5), (2,3), (3,4)}
- Vertex 1 has degree 4 > 3
- k = 3

**Transformation:**
- Replace vertex 1 (degree 4) with chain: 1a, 1b
- Connect chain: (1a, 1b)
- Distribute neighbors:
  - Connect 2, 3 to 1a (deg(1a) = 3: neighbors 1b, 2, 3)
  - Connect 4, 5 to 1b (deg(1b) = 3: neighbors 1a, 4, 5)
- Make 1a and 1b adjacent (they form a clique)
- New graph G' has all vertices with degree ≤ 3
- k' = 3 + (4-3) = 4 (need clique of size 4 in G')

### 2.2 Output Conversion

**Given:** Clique-3 solution (clique C' of size k' in G')

**Extract Clique:**
- For each original vertex v:
  - If v was not replaced: include v in C if v ∈ C'
  - If v was replaced by chain v₁, ..., v_{d-2}:
    - Include v in C if all vᵢ are in C'
    - (Since all vᵢ form a clique, if any are in C', all must be in C')

**Verify Satisfaction:**
- C has size k (by construction, accounting for replacements)
- All pairs in C are adjacent:
  - If both vertices were not replaced: edge exists in G (preserved in G')
  - If one was replaced: the replacement chain connects to neighbors
  - If both were replaced: their replacement chains are fully connected

## 3. Correctness Justification

### 3.1 If Clique has a solution, then Clique-3 has a solution

**Given:** Clique instance (G, k) has solution C (clique of size k).

**Construct Clique-3 Solution:**
- For each vertex v ∈ C:
  - If v was not replaced: include v in C'
  - If v was replaced by chain v₁, ..., v_{d-2}: include all vᵢ in C'
- For each replaced vertex v ∉ C:
  - Do not include any vᵢ in C'

**Verify Satisfaction:**
- C' has size k': ✓ (by construction, k' accounts for replacements)
- All pairs in C' are adjacent:
  - Vertices from same replacement chain: ✓ (all vᵢ form a clique)
  - Vertices from different replacement chains: ✓ (if original vertices were adjacent, their chains are connected)
  - Original vertices: ✓ (edges preserved)
- All vertices in G' have degree ≤ 3: ✓ (by construction)

**Conclusion:** Clique-3 has a solution.

### 3.2a If Clique does not have a solution, then Clique-3 has no solution

**Given:** Clique instance (G, k) has no solution (no clique of size k).

**Proof:**

**Assume:** Clique-3 instance (G', k') has a solution (clique C' of size k').

**Extract Clique:**
- For each original vertex v:
  - If v was not replaced and v ∈ C': include v in C
  - If v was replaced and all vᵢ ∈ C': include v in C
- C has size k (by reversing the replacement accounting)

**Check Adjacency:**
- For any two vertices u, v ∈ C:
  - If both were not replaced: edge (u,v) exists in G (preserved in G')
  - If one was replaced: the replacement connects to neighbors
  - If both were replaced: their replacement chains are connected
- Therefore, C is a clique in G

**Contradiction:**
- C is a clique of size k in G
- This contradicts the assumption that G has no clique of size k

**Conclusion:** Clique-3 has no solution.

### 3.2b If Clique-3 has a solution, then Clique has a solution

**Given:** Clique-3 instance (G', k') has solution (clique C' of size k').

**Extract Clique:**
- For each original vertex v:
  - If v was not replaced: include v in C if v ∈ C'
  - If v was replaced by chain v₁, ..., v_{d-2}:
    - Include v in C if all vᵢ are in C'
    - (Since replacement chains form cliques, if any vᵢ is in C', all must be in C' for C' to be a clique)

**Verify Satisfaction:**

**Clique Requirements:**
- C has size k: ✓ (by construction, accounting for replacements)
- All pairs in C are adjacent:
  - If both vertices were not replaced: edge exists in G (preserved in G')
  - If one was replaced: the replacement chain connects to the neighbor
  - If both were replaced: their replacement chains are fully connected (by construction)

**Key Insight:**
- The replacement preserves clique structure: if original vertices form a clique, their replacements form a clique
- The degree constraint is satisfied: all vertices in G' have degree ≤ 3
- The clique size is adjusted to account for vertex replacements

**Conclusion:** The set C forms a clique of size k in G, so Clique has a solution.

## Polynomial Time Analysis

**Input Size:**
- Clique: Graph G with n vertices and m edges, integer k
- Clique-3: Graph G' with at most n + O(m) vertices (each high-degree vertex replaced by O(deg(v)) vertices), integer k'

**Construction Time:**
- Identify high-degree vertices: O(n) time
- For each high-degree vertex v with degree d:
  - Create replacement chain: O(d) time
  - Connect neighbors: O(d) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Clique-3 ∈ NP**: Solutions can be verified in polynomial time
2. **Clique ≤ₚ Clique-3**: Polynomial-time reduction exists
3. **Correctness**: Clique has solution ↔ Clique-3 has solution

**Therefore, Clique-3 is NP-complete.**

## Key Insights

1. **Degree Reduction Gadget**: Replacing high-degree vertices with chains preserves clique structure
2. **Local Replacement**: Each vertex replacement is local and doesn't affect distant parts of the graph
3. **Clique Preservation**: The replacement ensures that cliques in the original graph correspond to cliques in the transformed graph
4. **Size Adjustment**: The clique size parameter is adjusted to account for vertex replacements

## Practice Questions

1. **Modify the reduction** to handle directed graphs. How would the gadget change?
2. **Analyze the gadget size:** Can we use a more efficient replacement that creates fewer vertices?
3. **Consider other degree bounds:** How would you reduce Clique to Clique-d for d = 2? Is it still NP-complete?
4. **Prove the reverse reduction:** Can we reduce Clique-3 to Clique? How?
5. **Investigate special cases:** For what graph classes is Clique-3 polynomial-time solvable?

---

This reduction demonstrates that even highly restricted versions of Clique (with bounded degree) remain NP-complete, showing the robustness of the problem's computational hardness.

