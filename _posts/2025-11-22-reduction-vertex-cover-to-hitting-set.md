---
layout: post
title: "Reduction: Vertex Cover to Hitting Set"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Vertex Cover to Hitting Set, demonstrating that Hitting Set is NP-complete."
---

## Introduction

This post provides a detailed proof that the Hitting Set problem is NP-complete by reducing from Vertex Cover. Hitting Set asks for a set of size at most b that intersects every set in a family, generalizing Vertex Cover.

## Problem Definitions

### Hitting Set Problem

**Input:** 
- A family of sets {S₁, S₂, ..., Sₙ} over a universe U
- A budget b ≥ 0

**Output:** YES if there exists a set H ⊆ U of size ≤ b such that H ∩ Sᵢ ≠ ∅ for all i (i.e., H intersects every Sᵢ), NO otherwise.

**Note:** In other words, we want H ∩ Sᵢ ≠ ∅ for all i, meaning H must contain at least one element from each set Sᵢ.

### Vertex Cover Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 0

**Output:** YES if G has a vertex cover of size at most k (set of vertices covering all edges), NO otherwise.

## 1. NP-Completeness Proof of Hitting Set: Solution Validation

### Hitting Set ∈ NP

**Verification Algorithm:**
Given a candidate solution (hitting set H):
- Let n = number of sets in the family
- Let m = number of unique elements in the universe U

1. Check that `|H|` ≤ b: O(m) time (need to verify H is a valid set and count its elements)
2. Create a boolean helper array A of size n (one for each subset), initialized to false: O(n) time
3. For each element h in H:
   - For each set Sᵢ (i = 1 to n):
     - If h ∈ Sᵢ, set A[i] = true: O(nm²) time total (for all h in H and all sets Sᵢ)
4. Check that all entries in A are true: O(n) time

**Total Time:** O(m + n + nm² + n) = O(nm²), which is polynomial in input size.

**Conclusion:** Hitting Set ∈ NP.

## 2. Reduce Vertex Cover to Hitting Set

**Key Insight:** Given a Vertex Cover instance, we can reduce it to Hitting Set by representing each edge as a set containing its two endpoints. Then a vertex cover corresponds to a hitting set that intersects every edge-set.

**Hint:** An edge is covered by a vertex if the vertex is an endpoint. Therefore, covering all edges is equivalent to hitting all edge-sets (where each edge-set contains the two endpoints of an edge).

### 2.1 Input Conversion

Given a Vertex Cover instance (G, k) where G = (V, E) with n vertices and m edges, we construct a Hitting Set instance ({S₁, S₂, ..., Sₘ}, b).

**Construction:**

**Step 1: Set Universe**
- U = V (vertices become universe elements)

**Step 2: Create Sets from Edges**
- For each edge e = (u, v) ∈ E:
  - Create set Sₑ = {u, v} (set containing the two endpoints)
- Family of sets: {S₁, S₂, ..., Sₘ} where m = `|E|` (one set per edge)

**Step 3: Set Budget**
- b = k (same size constraint)

**Step 4: Result**
- Hitting Set instance: ({S₁, S₂, ..., Sₘ}, b) where b = k

**Detailed Example:**

Consider Vertex Cover instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (4,1)}
- k = 2

**Transformation:**
- U = {1, 2, 3, 4}
- Family of sets:
  - S₁ = {1, 2} (edge (1,2))
  - S₂ = {2, 3} (edge (2,3))
  - S₃ = {3, 4} (edge (3,4))
  - S₄ = {4, 1} (edge (4,1))
- b = 2

**Hitting Set instance:** ({S₁, S₂, S₃, S₄}, 2)

### 2.2 Output Conversion

**Given:** Hitting Set solution (hitting set H of size ≤ b)

**Extract Vertex Cover:**
- Use H as the vertex cover
- H ⊆ U = V (vertices)
- For each edge e = (u, v), set Sₑ = {u, v} must be hit by H
- Therefore, H ∩ {u, v} ≠ ∅, meaning at least one of u or v is in H
- This means edge e is covered by H

**Verify Satisfaction:**
- H has size ≤ b = k: ✓
- H ⊆ V: ✓
- For each edge e = (u, v), H ∩ {u, v} ≠ ∅: ✓ (by hitting set property)
- Therefore, H covers all edges

## 3. Correctness Justification

### 3.1 If Vertex Cover has a solution, then Hitting Set has a solution

**Given:** Vertex Cover instance (G, k) has solution C (vertex cover of size ≤ k).

**Construct Hitting Set Solution:**
- Use C as the hitting set H
- H = C ⊆ V = U
- `|H|` = `|C|` ≤ k = b

**Verify Hitting Property:**
- For each set Sᵢ (corresponding to edge e = (u, v) ∈ E):
  - Set Sᵢ = {u, v} must be hit
  - Since C is a vertex cover, at least one of u or v is in C
  - Therefore, H ∩ Sᵢ = H ∩ {u, v} ≠ ∅
- All sets Sᵢ are hit by H (H ∩ Sᵢ ≠ ∅ for all i)

**Conclusion:** Hitting Set has a solution.

### 3.2a If Vertex Cover does not have a solution, then Hitting Set has no solution

**Given:** Vertex Cover instance (G, k) has no solution (no vertex cover of size ≤ k).

**Proof:**

**Assume:** Hitting Set instance ({S₁, S₂, ..., Sₘ}, b) has a solution (hitting set H of size ≤ b).

**Extract Vertex Cover:**
- Use H as the vertex cover
- H ⊆ U = V
- `|H|` ≤ b = k

**Check Coverage:**
- For each set Sᵢ (corresponding to edge e = (u, v) ∈ E):
  - Set Sᵢ = {u, v} must be hit by H
  - Therefore, H ∩ Sᵢ = H ∩ {u, v} ≠ ∅
  - This means at least one of u or v is in H
  - Therefore, edge e is covered by H
- All edges are covered by H

**Contradiction:**
- H is a vertex cover of size ≤ k
- This contradicts the assumption that G has no vertex cover of size ≤ k

**Conclusion:** Hitting Set has no solution.

### 3.2b If Hitting Set has a solution, then Vertex Cover has a solution

**Given:** Hitting Set instance ({S₁, S₂, ..., Sₘ}, b) has solution (hitting set H of size ≤ b).

**Extract Vertex Cover:**
- Use H as the vertex cover
- H ⊆ U = V
- `|H|` ≤ b = k

**Verify Coverage:**

**For each set Sᵢ (corresponding to edge e = (u, v) ∈ E):**
- Set Sᵢ = {u, v}
- Since H hits Sᵢ, we have H ∩ Sᵢ = H ∩ {u, v} ≠ ∅
- Therefore, at least one of u or v is in H
- This means edge e is covered by H

**Key Insight:**
- Each edge corresponds to a set containing its endpoints
- Hitting all sets Sᵢ (i.e., H ∩ Sᵢ ≠ ∅ for all i) means covering all edges
- Therefore, a hitting set is a vertex cover

**Conclusion:** The set H covers all edges in G, so Vertex Cover has a solution.

## Polynomial Time Analysis

**Input Size:**
- Vertex Cover: Graph G with n vertices and m edges, integer k
- Hitting Set: Universe U with n elements, family of m sets (each of size 2), budget b = k

**Construction Time:**
- Create universe U: O(n) time
- For each edge: Create set of size 2: O(m) time
- Set b = k: O(1) time
- Total: O(n + m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Hitting Set ∈ NP**: Solutions can be verified in polynomial time
2. **Vertex Cover ≤ₚ Hitting Set**: Polynomial-time reduction exists
3. **Correctness**: Vertex Cover has solution ↔ Hitting Set has solution

**Therefore, Hitting Set is NP-complete.**

## Key Insights

1. **Edge-to-Set Mapping**: Each edge becomes a set containing its endpoints
2. **Coverage Equivalence**: Covering an edge is equivalent to hitting the corresponding set
3. **Generalization**: Hitting Set generalizes Vertex Cover to arbitrary set systems
4. **Preservation**: The reduction preserves the coverage structure through set intersection

## Practice Questions

1. **Modify the reduction** to reduce Set Cover to Hitting Set. How would you do it?
2. **Extend the reduction** to handle weighted Vertex Cover. How would you encode weights?
3. **Consider d-Hitting Set:** What if all sets have size at most d? Is the problem still NP-complete?
4. **Prove the reverse reduction:** Can we reduce Hitting Set to Vertex Cover? How?
5. **Investigate special cases:** For what set sizes is Hitting Set polynomial-time solvable?

---

This reduction demonstrates how set systems can encode graph structures, enabling reductions between graph covering and set intersection problems.

