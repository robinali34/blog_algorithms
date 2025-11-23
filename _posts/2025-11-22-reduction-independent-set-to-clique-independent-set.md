---
layout: post
title: "Reduction: Independent Set to Clique + Independent Set"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Independent Set to the problem of finding both a clique and an independent set of given sizes."
---

## Introduction

This post provides a detailed proof that the Clique + Independent Set problem is NP-complete by reducing from Independent Set. The problem asks whether a graph contains both a clique of size k and an independent set of size k simultaneously.

## Problem Definitions

### Clique + Independent Set Problem

**Input:** 
- An undirected graph G = (V, E)
- An integer k ≥ 1

**Output:** YES if G contains both:
- A clique of size k (set of k vertices, all pairwise adjacent)
- An independent set of size k (set of k vertices, no two adjacent)

NO otherwise.

**Note:** The problem requires returning both a clique of size k and an independent set of size k, provided both exist. The clique and independent set may share vertices.

### Independent Set Problem

**Input:** 
- An undirected graph G = (V, E)
- Integer k ≥ 1

**Output:** YES if G contains an independent set of size k, NO otherwise.

## 1. NP-Completeness Proof of Clique + Independent Set: Solution Validation

### Clique + Independent Set ∈ NP

**Verification Algorithm:**
Given a candidate solution (clique C of size k, independent set I of size k):
1. Check that C has exactly k vertices: O(1) time
2. Check that all pairs in C are adjacent: O(n²) time (since k ≤ n)
3. Check that I has exactly k vertices: O(1) time
4. Check that no two vertices in I are adjacent: O(n²) time (since k ≤ n)

**Total Time:** O(n²), which is polynomial in input size.

**Conclusion:** Clique + Independent Set ∈ NP.

## 2. Reduce Independent Set to Clique + Independent Set

**Key Insight:** Given an Independent Set instance, we can reduce it to Clique + Independent Set by adding k isolated vertices to the graph. This ensures a k-independent set always exists (using the isolated vertices), reducing the problem to finding a k-clique. However, a simpler approach is to reduce from Clique by ensuring a k-independent set always exists through graph modification.

**Hint:** We can reduce from Clique by adding k isolated vertices, ensuring a k-independent set always exists. Then finding both a k-clique and k-independent set reduces to finding a k-clique. Alternatively, we can reduce from Independent Set by ensuring a k-clique always exists (e.g., by adding a k-clique disconnected from the rest).

### 2.1 Input Conversion

Given an Independent Set instance (G, k) where G = (V, E) and we want an independent set of size k, we construct a Clique + Independent Set instance (G', k').

**Construction:**

**Step 1: Add Disjoint Clique**
- Add k new vertices: v₁, v₂, ..., vₖ
- Connect them to form a clique: add all edges (vᵢ, vⱼ) for i < j
- This clique is disjoint from the original graph (no edges between new and old vertices)

**Step 2: Set Parameter**
- k' = k (we want both a clique of size k and an independent set of size k)

**Step 3: Result**
- Clique + Independent Set instance: (G', k)

**Detailed Example:**

Consider Independent Set instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4)}
- k = 2 (want independent set of size 2)

**Transformation:**
- Add vertices: {5, 6}
- Add clique edges: {(5,6)}
- G' has vertices {1, 2, 3, 4, 5, 6}
- G' has edges: {(1,2), (2,3), (3,4), (5,6)}
- k' = 2

**Clique + Independent Set instance:** (G', 2)

**Note:** The clique {5, 6} always exists. Finding both a 2-clique and 2-independent set reduces to finding a 2-independent set in the original graph.

### 2.2 Output Conversion

**Given:** Clique + Independent Set solution (clique C of size k, independent set I of size k)

**Extract Independent Set:**
- The clique C must be the added clique {v₁, ..., vₖ} (since it's disjoint and always exists)
- Use the independent set I
- I must come from the original graph G (since the added clique vertices are all connected to each other)
- I is an independent set of size k in G' that doesn't use the added clique vertices
- Therefore, I is an independent set of size k in the original graph G

**Verify Satisfaction:**
- I has k vertices
- No two vertices in I are adjacent (by definition of independent set)
- I uses only vertices from the original graph G
- Therefore, I is an independent set of size k in the original graph G

## 3. Correctness Justification

### 3.1 If Independent Set has a solution, then Clique + Independent Set has a solution

**Given:** Independent Set instance (G, k) has solution I (independent set of size k).

**Construct Clique + Independent Set Solution:**
- Clique C: Use the added clique {v₁, v₂, ..., vₖ} (always exists, size k)
- Independent set I: Use the given independent set of size k from G

**Verify Satisfaction:**
- C has size k: ✓ (the added clique)
- C is a clique: ✓ (all pairs in {v₁, ..., vₖ} are adjacent)
- I has size k: ✓ (by assumption)
- I is an independent set: ✓ (by assumption)
- I uses vertices from G, which are disjoint from the clique vertices
- Therefore, (C, I) is a valid solution

**Conclusion:** Clique + Independent Set has a solution.

### 3.2a If Independent Set does not have a solution, then Clique + Independent Set has no solution

**Given:** Independent Set instance (G, k) has no solution (no independent set of size k).

**Proof:**

**Assume:** Clique + Independent Set instance (G', k) has a solution (C, I).

**Extract Independent Set:**
- The clique C must be the added clique {v₁, ..., vₖ} (since it's the only k-clique guaranteed to exist)
- Use independent set I from the solution
- I has size k (by assumption)
- I must use vertices from the original graph G (since the added clique vertices form a clique and can't be in an independent set together)
- I is an independent set in G' that uses only vertices from G
- Therefore, I is an independent set of size k in G

**Contradiction:**
- I is an independent set of size k in G
- This contradicts the assumption that G has no independent set of size k

**Conclusion:** Clique + Independent Set has no solution.

### 3.2b If Clique + Independent Set has a solution, then Independent Set has a solution

**Given:** Clique + Independent Set instance (G', k) has solution (C, I).

**Extract Independent Set:**
- The clique C is the added clique {v₁, ..., vₖ}
- Use independent set I
- I has size k (by assumption)
- I must use vertices from the original graph G (since the added clique vertices are all connected)
- I is an independent set in G' that uses only vertices from G
- Therefore, I is an independent set of size k in G

**Verify Satisfaction:**

**Independent Set Requirements:**
- I has exactly k vertices: ✓ (by assumption)
- No two vertices in I are adjacent: ✓ (by definition of independent set)
- I uses only vertices from G: ✓ (since added clique vertices can't be in I together)
- Therefore, I is an independent set of size k in G

**Key Insight:**
- By adding a disjoint k-clique, we ensure a k-clique always exists
- The problem reduces to finding a k-independent set in the original graph
- The added clique doesn't interfere with finding an independent set since it's disjoint
- Therefore, any solution to Clique + Independent Set gives us an independent set of size k

**Conclusion:** The independent set I satisfies all requirements, so Independent Set has a solution.

## Polynomial Time Analysis

**Input Size:**
- Independent Set: Graph G with n vertices and m edges, integer k
- Clique + Independent Set: Graph G' with n + k vertices and m + k(k-1)/2 edges, integer k

**Construction Time:**
- Add k vertices: O(k) time
- Add clique edges: O(k²) time
- Set k' = k: O(1) time
- Total: O(n + m + k²) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Clique + Independent Set ∈ NP**: Solutions can be verified in polynomial time
2. **Independent Set ≤ₚ Clique + Independent Set**: Polynomial-time reduction exists
3. **Correctness**: Independent Set has solution ↔ Clique + Independent Set has solution

**Therefore, Clique + Independent Set is NP-complete.**

## Key Insights

1. **Disjoint Clique Addition**: Adding a disjoint k-clique ensures a k-clique always exists
2. **Problem Reduction**: With a guaranteed k-clique, the problem reduces to finding a k-independent set
3. **Preservation**: The reduction preserves the core independent set structure
4. **Disjoint Construction**: Making the added clique disjoint ensures it doesn't interfere with finding an independent set

## Practice Questions

1. **Modify the reduction** to reduce Clique to Clique + Independent Set. How would you do it?
2. **Analyze the case** when k' > 1 and l' > 1. Is the problem still NP-complete?
3. **Consider disjoint versions:** What if the clique and independent set must be disjoint?
4. **Prove the reverse reduction:** Can we reduce Clique + Independent Set to Independent Set? How?
5. **Investigate parameterized complexity:** For what values of k' and l' is the problem polynomial-time solvable?

---

This reduction demonstrates how making one part of a combined problem trivial (by setting k' = 1) can reduce a complex problem to a simpler one, enabling reductions between graph problems.

