---
layout: post
title: "Reduction: Vertex Cover to Dominating Set"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Vertex Cover to Dominating Set, demonstrating that Dominating Set is NP-complete."
---

## Introduction

This post provides a detailed proof that the Dominating Set problem is NP-complete by reducing from Vertex Cover. Dominating Set asks for a dominating set in the graph of size at most b, if one exists.

## Problem Definitions

### Dominating Set Problem

**Input:** 
- An undirected graph G = (V, E)
- A budget b ≥ 0

**Output:** YES if there exists a dominating set in the graph of size at most b, NO otherwise.

**Dominating Set Definition:** In an undirected graph G = (V, E), we say D ⊆ V is a dominating set if every v ∈ V is either in D or adjacent to at least one member of D.

**Note:** The aim is to find a dominating set in the graph of size at most b, if one exists.

### Vertex Cover Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 0

**Output:** YES if G has a vertex cover of size at most k (set of vertices covering all edges), NO otherwise.

## 1. NP-Completeness Proof of Dominating Set: Solution Validation

### Dominating Set ∈ NP

**Verification Algorithm:**
Given a candidate solution (dominating set D):
1. Check that |D| ≤ b: O(1) time
2. For each vertex v ∈ V:
   - Check if v ∈ D: O(1) time, OR
   - Check if v is adjacent to at least one vertex in D: O(deg(v)) time
3. Check all vertices: O(n + m) time

**Total Time:** O(n + m), which is polynomial in input size.

**Conclusion:** Dominating Set ∈ NP.

## 2. Reduce Vertex Cover to Dominating Set

**Key Insight:** Given a Vertex Cover instance, we can reduce it to Dominating Set by adding a new vertex for each edge, connected only to the endpoints of that edge. Then a vertex cover in the original graph corresponds to a dominating set in the modified graph.

**Hint:** In a vertex cover, every edge has at least one endpoint covered. By adding a vertex for each edge connected only to its endpoints, we ensure that covering edges (vertex cover) is equivalent to dominating the new vertices (dominating set). The original vertices are also dominated since a vertex cover naturally dominates them.

### 2.1 Input Conversion

Given a Vertex Cover instance (G, k) where G = (V, E) with n vertices and m edges, we construct a Dominating Set instance (G', b).

**Construction:**

**Step 1: Copy Original Graph**
- Keep all vertices V and edges E from G

**Step 2: Add Edge Vertices**
- For each edge e = (u, v) ∈ E:
  - Add a new vertex wₑ
  - Connect wₑ only to u and v: add edges (wₑ, u) and (wₑ, v)
  - wₑ is not connected to any other vertices

**Step 3: Set Budget**
- b = k (same size constraint)

**Step 4: Result**
- Dominating Set instance: (G', b) where G' has n + m vertices (original n + m new edge vertices)

**Detailed Example:**

Consider Vertex Cover instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (4,1)}
- k = 2

**Transformation:**
- Keep original vertices: {1, 2, 3, 4}
- Keep original edges: {(1,2), (2,3), (3,4), (4,1)}
- Add edge vertices:
  - w₁₂ (for edge (1,2)): connected to 1 and 2
  - w₂₃ (for edge (2,3)): connected to 2 and 3
  - w₃₄ (for edge (3,4)): connected to 3 and 4
  - w₄₁ (for edge (4,1)): connected to 4 and 1
- b = 2

**Dominating Set instance:** (G', 2)

### 2.2 Output Conversion

**Given:** Dominating Set solution (dominating set D of size ≤ b = k)

**Extract Vertex Cover:**
- Use D as the vertex cover
- D ⊆ V' (vertices in G')
- Since edge vertices wₑ are only connected to the endpoints of edge e, to dominate wₑ, D must contain at least one endpoint of e
- Therefore, for each edge e = (u, v), at least one of u or v is in D
- This means D covers all edges in the original graph G

**Verify Satisfaction:**
- D has size ≤ k: ✓
- D ⊆ V (original vertices): Need to check - D might contain edge vertices
- If D contains edge vertices, we can replace them with adjacent original vertices
- For each edge e = (u, v), at least one endpoint is dominated by D
- Therefore, D (or a modification) covers all edges

## 3. Correctness Justification

### 3.1 If Vertex Cover has a solution, then Dominating Set has a solution

**Given:** Vertex Cover instance (G, k) has solution C (vertex cover of size ≤ k).

**Construct Dominating Set Solution:**
- Use C as the dominating set D
- D = C ⊆ V ⊆ V'
- |D| = |C| ≤ k = b

**Verify Dominating Property:**

**For edge vertices wₑ (for each edge e = (u, v)):**
- Since C is a vertex cover, at least one of u or v is in C
- Therefore, wₑ is adjacent to at least one vertex in C
- So wₑ is dominated by C ✓

**For original vertices v ∈ V:**
- If v ∈ C = D, then v is dominated (it's in D) ✓
- If v ∉ C and deg(v) > 0:
  - Since C is a vertex cover, all edges incident to v are covered
  - Therefore, at least one neighbor of v is in C
  - So v is adjacent to at least one vertex in C
  - Therefore, v is dominated by C ✓
- If v ∉ C and deg(v) = 0 (isolated vertex):
  - Isolated vertices don't need to be in a vertex cover (they have no edges to cover)
  - However, isolated vertices must be in a dominating set (they have no neighbors to dominate them)
  - For the reduction to work, we assume G has no isolated vertices, OR
  - We can preprocess: isolated vertices don't affect vertex cover, so we can remove them before the reduction

**Note:** The standard reduction assumes G has no isolated vertices. If G has isolated vertices, we can either:
1. Remove them before reduction (they don't affect vertex cover)
2. Or ensure they're included in any solution (which doesn't affect the vertex cover property)

**Conclusion:** C is a dominating set in G', so Dominating Set has a solution.

### 3.2a If Vertex Cover does not have a solution, then Dominating Set has no solution

**Given:** Vertex Cover instance (G, k) has no solution (no vertex cover of size ≤ k).

**Proof:**

**Assume:** Dominating Set instance (G', b) where b = k has a solution (dominating set D of size ≤ b).

**Extract Vertex Cover:**
- If D contains only original vertices: Use D as vertex cover
- If D contains edge vertices: For each edge vertex wₑ in D, replace it with one of its neighbors (u or v)
- This gives us a set C ⊆ V of size ≤ |D| ≤ k

**Check Coverage:**
- For each edge e = (u, v):
  - Edge vertex wₑ must be dominated by D
  - Since wₑ is only connected to u and v, at least one of u or v must be in D (or we replaced wₑ with u or v)
  - Therefore, at least one endpoint of e is in C
  - Edge e is covered by C

**Contradiction:**
- C is a vertex cover of size ≤ k
- This contradicts the assumption that G has no vertex cover of size ≤ k

**Conclusion:** Dominating Set has no solution.

### 3.2b If Dominating Set has a solution, then Vertex Cover has a solution

**Given:** Dominating Set instance (G', b) where b = k has solution (dominating set D of size ≤ b).

**Extract Vertex Cover:**

**Step 1: Handle Edge Vertices**
- For each edge vertex wₑ in D:
  - Remove wₑ from D
  - Add one of its neighbors (u or v, arbitrarily choose u) to D
- This gives us a set C ⊆ V (only original vertices)

**Step 2: Verify Size**
- |C| ≤ |D| ≤ b = k (we replaced each edge vertex with one neighbor, possibly duplicates)

**Step 3: Verify Coverage**

**For each edge e = (u, v) ∈ E:**
- Edge vertex wₑ must be dominated by D
- Since wₑ is only connected to u and v, at least one of u or v must be in D or adjacent to D
- If wₑ ∈ D: We replaced it with u (or v), so u ∈ C
- If wₑ ∉ D: Then at least one of u or v is in D (to dominate wₑ), so at least one is in C
- Therefore, at least one endpoint of e is in C
- Edge e is covered by C

**Key Insight:**
- Each edge vertex wₑ is only connected to the endpoints of edge e
- To dominate wₑ, we need at least one endpoint of e in the dominating set
- This ensures that the dominating set (or its modification) covers all edges
- Therefore, a dominating set gives us a vertex cover

**Conclusion:** The set C covers all edges in G, so Vertex Cover has a solution.

## Polynomial Time Analysis

**Input Size:**
- Vertex Cover: Graph G with n vertices and m edges, integer k
- Dominating Set: Graph G' with n + m vertices (original n + m edge vertices) and m + 2m = 3m edges (original m + 2m for edge vertices), budget b = k

**Construction Time:**
- Copy original graph: O(n + m) time
- For each edge: Add vertex and 2 edges: O(m) time
- Set b = k: O(1) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Dominating Set ∈ NP**: Solutions can be verified in polynomial time
2. **Vertex Cover ≤ₚ Dominating Set**: Polynomial-time reduction exists
3. **Correctness**: Vertex Cover has solution ↔ Dominating Set has solution

**Therefore, Dominating Set is NP-complete.**

## Key Insights

1. **Edge Vertex Gadget**: Adding a vertex for each edge, connected only to its endpoints, creates a gadget that links edge coverage to vertex domination
2. **Coverage Equivalence**: Covering an edge (vertex cover) is equivalent to dominating the corresponding edge vertex (dominating set)
3. **Vertex Replacement**: Edge vertices in a dominating set can be replaced with their neighbors to obtain a vertex cover
4. **Preservation**: The reduction preserves the coverage structure through the edge vertex construction

## Practice Questions

1. **Modify the reduction** to handle graphs with isolated vertices. How would you ensure isolated vertices are handled correctly?
2. **Extend the reduction** to handle weighted Vertex Cover. How would you encode weights?
3. **Consider connected dominating set:** What if we require the dominating set to be connected? Is the problem still NP-complete?
4. **Prove the reverse reduction:** Can we reduce Dominating Set to Vertex Cover? How?
5. **Investigate special cases:** For what graph classes is Dominating Set polynomial-time solvable?

---

This reduction demonstrates how vertex domination can encode edge coverage, enabling reductions between graph covering problems.

