---
layout: post
title: "Reduction: Rudrata Cycle to Traveling Salesman Problem"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Rudrata Cycle to Traveling Salesman Problem, demonstrating that TSP is NP-complete."
---

## Introduction

This post provides a detailed proof that the Traveling Salesman Problem (TSP) is NP-complete by reducing from Rudrata Cycle. The reduction transforms the problem of finding a Hamiltonian cycle into finding a minimum-cost tour visiting all cities.

## Problem Definitions

### Traveling Salesman Problem (TSP)

**Input:** 
- A complete undirected graph G = (V, E) with n vertices (cities)
- Edge weights w: E → ℤ⁺ (distances/costs)
- Integer B ≥ 0 (budget)

**Output:** YES if there exists a tour (cycle visiting all vertices) with total cost ≤ B, NO otherwise.

### Rudrata Cycle Problem

**Input:** 
- An undirected graph G = (V, E)

**Output:** YES if G contains a Rudrata cycle (Hamiltonian cycle visiting all vertices exactly once), NO otherwise.

## 1. NP-Completeness Proof of TSP: Solution Validation

### TSP ∈ NP

**Verification Algorithm:**
Given a candidate solution (tour π = v₁, v₂, ..., vₙ, v₁):
1. Check that π visits all n vertices exactly once: O(n) time
2. Check that π forms a cycle: O(1) time (v₁ = vₙ₊₁)
3. Compute total cost: ∑ᵢ₌₁ⁿ w(vᵢ, vᵢ₊₁): O(n) time
4. Check if total cost ≤ B: O(1) time

**Total Time:** O(n), which is polynomial in input size.

**Conclusion:** TSP ∈ NP.

## 2. Reduce Rudrata Cycle to TSP

**Key Insight:** Given a Rudrata Cycle instance, we can reduce it to TSP by making the graph complete, setting edge weights to 1 for existing edges and 2 for missing edges, and setting B = n. Then G has a Rudrata cycle if and only if the TSP instance has a tour of cost n.

**Hint:** A Rudrata cycle uses exactly n edges, all of which must exist in the original graph. By setting existing edges to weight 1 and missing edges to weight 2, and B = n, we ensure that only tours using existing edges are feasible.

### 2.1 Input Conversion

Given a Rudrata Cycle instance G = (V, E) with n vertices, we construct a TSP instance (G', w, B).

**Construction:**

**Step 1: Create Complete Graph**
- G' = (V, E') where E' contains all pairs of vertices (complete graph)
- |E'| = n(n-1)/2

**Step 2: Set Edge Weights**
- For each edge (u, v) ∈ E (existing edge): w(u, v) = 1
- For each edge (u, v) ∉ E (missing edge): w(u, v) = 2

**Step 3: Set Budget**
- B = n (we want a tour using exactly n edges, all of weight 1)

**Step 4: Result**
- TSP instance: (G', w, n)

**Detailed Example:**

Consider Rudrata Cycle instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (4,1), (1,3)}

**Transformation:**
- G' = complete graph on {1, 2, 3, 4}
- Edge weights:
  - w(1,2) = 1, w(2,3) = 1, w(3,4) = 1, w(4,1) = 1, w(1,3) = 1
  - w(2,4) = 2 (missing edge)
- B = 4

**TSP instance:** (G', w, 4)

### 2.2 Output Conversion

**Given:** TSP solution (tour π with cost ≤ B = n)

**Extract Rudrata Cycle:**
- The tour π visits all n vertices exactly once
- Since cost ≤ n and each edge has weight ≥ 1, the tour uses exactly n edges
- Since B = n and missing edges have weight 2, all edges in the tour must have weight 1
- Therefore, all edges in the tour exist in the original graph G
- π forms a Rudrata cycle in G

**Verify Satisfaction:**
- π visits all n vertices exactly once: ✓
- π forms a cycle: ✓
- All edges in π exist in G: ✓ (since they have weight 1)
- Therefore, π is a Rudrata cycle in G

## 3. Correctness Justification

### 3.1 If Rudrata Cycle has a solution, then TSP has a solution

**Given:** Rudrata Cycle instance G has solution C (Hamiltonian cycle).

**Construct TSP Solution:**
- Use cycle C as the tour
- C visits all n vertices exactly once
- C uses exactly n edges, all of which exist in G
- Therefore, all edges in C have weight 1
- Total cost = n ≤ B = n

**Verify Satisfaction:**
- Tour visits all vertices: ✓ (by definition of Rudrata cycle)
- Tour forms a cycle: ✓ (by definition)
- All edges have weight 1: ✓ (since they exist in G)
- Total cost = n ≤ B: ✓

**Conclusion:** TSP has a solution.

### 3.2a If Rudrata Cycle does not have a solution, then TSP has no solution

**Given:** Rudrata Cycle instance G has no solution (no Hamiltonian cycle).

**Proof:**

**Assume:** TSP instance (G', w, n) has a solution (tour π with cost ≤ n).

**Extract Cycle:**
- π visits all n vertices exactly once
- π forms a cycle
- Since cost ≤ n and each edge has weight ≥ 1, π uses exactly n edges
- Since missing edges have weight 2, all edges in π must have weight 1
- Therefore, all edges in π exist in G
- π is a Rudrata cycle in G

**Contradiction:**
- π is a Rudrata cycle in G
- This contradicts the assumption that G has no Rudrata cycle

**Conclusion:** TSP has no solution.

### 3.2b If TSP has a solution, then Rudrata Cycle has a solution

**Given:** TSP instance (G', w, n) has solution (tour π with cost ≤ n).

**Extract Cycle:**
- π visits all n vertices exactly once
- π forms a cycle
- Since cost ≤ n and each edge has weight ≥ 1, π uses exactly n edges
- Since B = n and missing edges have weight 2, all edges in π must have weight 1
- Therefore, all edges in π exist in the original graph G

**Verify Satisfaction:**

**Rudrata Cycle Requirements:**
- π visits all n vertices exactly once: ✓ (by definition of tour)
- π forms a cycle: ✓ (by definition)
- All edges in π exist in G: ✓ (since they have weight 1)
- Therefore, π is a Rudrata cycle in G

**Key Insight:**
- A tour of cost n must use exactly n edges, each of weight 1
- Edges of weight 1 correspond to edges in the original graph
- Therefore, the tour is a Rudrata cycle in the original graph

**Conclusion:** The tour π forms a Rudrata cycle in G, so Rudrata Cycle has a solution.

## Polynomial Time Analysis

**Input Size:**
- Rudrata Cycle: Graph G with n vertices and m edges
- TSP: Complete graph G' with n vertices and n(n-1)/2 edges, weights w, integer B = n

**Construction Time:**
- Create complete graph: O(n²) time
- Set edge weights: O(n²) time (check each pair)
- Set B = n: O(1) time
- Total: O(n²) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **TSP ∈ NP**: Solutions can be verified in polynomial time
2. **Rudrata Cycle ≤ₚ TSP**: Polynomial-time reduction exists
3. **Correctness**: Rudrata Cycle has solution ↔ TSP has solution

**Therefore, TSP is NP-complete.**

## Key Insights

1. **Complete Graph**: Making the graph complete allows any tour while controlling feasibility through weights
2. **Weight Encoding**: Weight 1 for existing edges, weight 2 for missing edges ensures only existing edges are used
3. **Budget Constraint**: Setting B = n forces the tour to use exactly n edges, all of weight 1
4. **Cycle Preservation**: The reduction preserves the cycle structure through the weight assignment

## Practice Questions

1. **Modify the reduction** to reduce Rudrata Path to TSP. How would you handle the start and end vertices?
2. **Extend the reduction** to handle weighted graphs. How would you set the weights?
3. **Consider metric TSP:** What if we require the triangle inequality? Does the reduction still work?
4. **Prove the reverse reduction:** Can we reduce TSP to Rudrata Cycle? How?
5. **Investigate approximation:** How does this reduction relate to approximation algorithms for TSP?

---

This reduction demonstrates how edge weights can encode graph structure, enabling reductions between cycle-finding and optimization problems.

