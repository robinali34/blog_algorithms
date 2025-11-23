---
layout: post
title: "Reduction: Rudrata Cycle to Reliable Network"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Rudrata Cycle to Reliable Network, demonstrating that Reliable Network is NP-complete."
---

## Introduction

This post provides a detailed proof that the Reliable Network problem is NP-complete by reducing from Rudrata Cycle. Reliable Network asks for a graph that satisfies connectivity requirements within a budget, generalizing the Hamiltonian cycle problem.

## Problem Definitions

### Reliable Network Problem

**Input:** 
- Two n × n matrices:
  - Distance matrix d_ij (cost of edge between vertices i and j)
  - Connectivity requirement matrix r_ij (required number of vertex-disjoint paths between i and j)
- A budget b ≥ 0

**Output:** YES if there exists a graph G = ({1, 2, ..., n}, E) such that:
1. The total cost of all edges is b or less: ∑_{(i,j) ∈ E} d_ij ≤ b
2. Between any two distinct vertices i and j, there are r_ij vertex-disjoint paths

NO otherwise.

**Note:** Vertex-disjoint paths are paths that share no vertices except possibly their endpoints.

### Rudrata Cycle Problem

**Input:** 
- An undirected graph G = (V, E)

**Output:** YES if G contains a Rudrata cycle (Hamiltonian cycle visiting all vertices exactly once), NO otherwise.

## 1. NP-Completeness Proof of Reliable Network: Solution Validation

### Reliable Network ∈ NP

**Verification Algorithm:**
Given a candidate solution (graph G = ({1, 2, ..., n}, E)):
1. Check that total cost ≤ b: O(|E|) time
2. For each pair (i, j) with i ≠ j:
   - Find r_ij vertex-disjoint paths: O(n²) time using max-flow algorithms
   - Check that at least r_ij paths exist: O(n²) time
3. Check all pairs: O(n⁴) time

**Total Time:** O(n⁴), which is polynomial in input size.

**Conclusion:** Reliable Network ∈ NP.

## 2. Reduce Rudrata Cycle to Reliable Network

**Key Insight:** Given a Rudrata Cycle instance, we can reduce it to Reliable Network by setting all distances to 1 or 2, all connectivity requirements to 2, and budget to n. A graph with n vertices, n edges, cost ≤ n, and 2 vertex-disjoint paths between all pairs must be a cycle, which is exactly a Rudrata cycle.

**Hint:** If all d_ij's are 1 or 2, b = n, and all r_ij's are 2, then we need a graph with exactly n edges (each of cost 1) that is 2-connected. A cycle on n vertices has exactly n edges and provides 2 vertex-disjoint paths between any two vertices (going clockwise and counterclockwise).

### 2.1 Input Conversion

Given a Rudrata Cycle instance G = (V, E) with n vertices, we construct a Reliable Network instance (d, r, b).

**Construction:**

**Step 1: Set Distance Matrix**
- For each pair (i, j) with i ≠ j:
  - If (i, j) ∈ E: d_ij = 1 (edge exists in original graph)
  - If (i, j) ∉ E: d_ij = 2 (edge doesn't exist, higher cost)
- d_ii = 0 (no self-loops)

**Step 2: Set Connectivity Requirements**
- For all pairs (i, j) with i ≠ j: r_ij = 2
- r_ii = 0 (no requirement for same vertex)

**Step 3: Set Budget**
- b = n (we can afford exactly n edges of cost 1)

**Step 4: Result**
- Reliable Network instance: (d, r, n) where:
  - d_ij ∈ {1, 2} for all i ≠ j
  - r_ij = 2 for all i ≠ j
  - b = n

**Detailed Example:**

Consider Rudrata Cycle instance:
- Graph G with vertices {1, 2, 3, 4}
- Edges: {(1,2), (2,3), (3,4), (4,1)}

**Transformation:**
- Distance matrix d:
  - d₁₂ = d₂₃ = d₃₄ = d₄₁ = 1 (edges exist)
  - All other d_ij = 2 (edges don't exist)
- Connectivity matrix r:
  - r_ij = 2 for all i ≠ j
- Budget: b = 4

**Reliable Network instance:** (d, r, 4)

**Note:** A cycle on 4 vertices has 4 edges (cost 4) and provides 2 vertex-disjoint paths between any pair.

### 2.2 Output Conversion

**Given:** Reliable Network solution (graph G' = ({1, 2, ..., n}, E') with cost ≤ n and r_ij = 2 vertex-disjoint paths between all pairs)

**Extract Rudrata Cycle:**
- Since b = n and all edges cost at least 1, G' has exactly n edges, all of cost 1
- Since r_ij = 2 for all pairs, G' is 2-connected
- A 2-connected graph with n vertices and n edges must be a cycle
- Therefore, G' is a cycle visiting all n vertices exactly once (a Rudrata cycle)

**Verify Satisfaction:**
- G' visits all n vertices: ✓ (by construction)
- G' forms a cycle: ✓ (2-connected with n vertices and n edges)
- All edges in G' exist in original graph G: ✓ (since d_ij = 1 only for edges in G)
- Therefore, G' is a Rudrata cycle in G

## 3. Correctness Justification

### 3.1 If Rudrata Cycle has a solution, then Reliable Network has a solution

**Given:** Rudrata Cycle instance G has solution C (Hamiltonian cycle).

**Construct Reliable Network Solution:**
- Use cycle C as the graph G'
- G' has exactly n edges (one per vertex in the cycle)
- All edges in C have cost 1 (since they exist in G, so d_ij = 1)
- Total cost = n ≤ b = n ✓

**Verify Connectivity:**
- For any two vertices i and j in the cycle:
  - Path 1: Follow the cycle in one direction from i to j
  - Path 2: Follow the cycle in the other direction from i to j
  - These two paths are vertex-disjoint (except at endpoints)
  - Therefore, there are 2 vertex-disjoint paths between i and j ✓
- All connectivity requirements r_ij = 2 are satisfied

**Conclusion:** Reliable Network has a solution.

### 3.2a If Rudrata Cycle does not have a solution, then Reliable Network has no solution

**Given:** Rudrata Cycle instance G has no solution (no Hamiltonian cycle).

**Proof:**

**Assume:** Reliable Network instance (d, r, n) has a solution (graph G' with cost ≤ n and 2 vertex-disjoint paths between all pairs).

**Extract Cycle:**
- Since b = n and all edges cost at least 1, G' has at most n edges
- Since r_ij = 2 for all pairs, G' is 2-connected
- A 2-connected graph with n vertices needs at least n edges (by connectivity requirements)
- Therefore, G' has exactly n edges, all of cost 1
- A 2-connected graph with n vertices and exactly n edges must be a cycle
- Therefore, G' is a cycle on n vertices
- Since all edges have cost 1, all edges in G' exist in the original graph G
- Therefore, G' is a Rudrata cycle in G

**Contradiction:**
- G' is a Rudrata cycle in G
- This contradicts the assumption that G has no Rudrata cycle

**Conclusion:** Reliable Network has no solution.

### 3.2b If Reliable Network has a solution, then Rudrata Cycle has a solution

**Given:** Reliable Network instance (d, r, n) has solution (graph G' with cost ≤ n and 2 vertex-disjoint paths between all pairs).

**Extract Cycle:**
- Since b = n and all edges cost at least 1, G' has at most n edges
- Since r_ij = 2 for all pairs, G' is 2-connected
- A 2-connected graph with n vertices needs at least n edges
- Therefore, G' has exactly n edges, all of cost 1
- A 2-connected graph with n vertices and exactly n edges must be a cycle
- Since all edges have cost 1, they correspond to edges in the original graph G (where d_ij = 1)
- Therefore, G' is a cycle in G visiting all n vertices

**Verify Satisfaction:**

**Rudrata Cycle Requirements:**
- G' visits all n vertices: ✓ (by construction, G' = ({1, 2, ..., n}, E'))
- G' forms a cycle: ✓ (2-connected with n vertices and n edges)
- All edges in G' exist in G: ✓ (since d_ij = 1 only for edges in G)
- Therefore, G' is a Rudrata cycle in G

**Key Insight:**
- With budget b = n and all edges costing at least 1, we can afford exactly n edges
- Requiring r_ij = 2 vertex-disjoint paths between all pairs forces the graph to be 2-connected
- A 2-connected graph with n vertices and n edges must be a cycle
- A cycle provides exactly 2 vertex-disjoint paths between any two vertices (clockwise and counterclockwise)
- Therefore, any solution to Reliable Network gives us a Rudrata cycle

**Conclusion:** The graph G' forms a Rudrata cycle in G, so Rudrata Cycle has a solution.

## Polynomial Time Analysis

**Input Size:**
- Rudrata Cycle: Graph G with n vertices and m edges
- Reliable Network: Two n × n matrices (n² entries each), integer b = n

**Construction Time:**
- Create distance matrix: O(n²) time (check each pair)
- Create connectivity matrix: O(n²) time (set all r_ij = 2)
- Set b = n: O(1) time
- Total: O(n²) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Reliable Network ∈ NP**: Solutions can be verified in polynomial time
2. **Rudrata Cycle ≤ₚ Reliable Network**: Polynomial-time reduction exists
3. **Correctness**: Rudrata Cycle has solution ↔ Reliable Network has solution

**Therefore, Reliable Network is NP-complete.**

## Key Insights

1. **Budget Constraint**: Setting b = n with all edges costing at least 1 forces exactly n edges
2. **Connectivity Requirement**: Requiring r_ij = 2 vertex-disjoint paths forces 2-connectivity
3. **Cycle Characterization**: A 2-connected graph with n vertices and n edges must be a cycle
4. **Path Structure**: A cycle provides exactly 2 vertex-disjoint paths between any two vertices
5. **Cost Encoding**: Using d_ij = 1 for existing edges and d_ij = 2 for non-existing edges ensures only original edges are used

## Practice Questions

1. **Modify the reduction** to handle different connectivity requirements. What if r_ij varies?
2. **Extend the reduction** to handle weighted graphs. How would you set the distance matrix?
3. **Consider the optimization version:** How would you reduce the optimization version of Rudrata Cycle to Reliable Network?
4. **Prove the reverse reduction:** Can we reduce Reliable Network to Rudrata Cycle? How?
5. **Investigate special cases:** For what values of r_ij and b is Reliable Network polynomial-time solvable?

---

This reduction demonstrates how connectivity and budget constraints can encode Hamiltonian cycle requirements, enabling reductions between network design and cycle-finding problems.

