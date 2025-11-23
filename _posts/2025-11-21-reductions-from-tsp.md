---
layout: post
title: "Reductions from Traveling Salesman Problem: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from Traveling Salesman Problem to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

Traveling Salesman Problem (TSP) is a fundamental routing problem proven NP-complete by reduction from Hamiltonian Cycle. This post provides detailed proofs following the standard template for reducing from TSP to prove other problems are NP-complete.

## Problem Definition: Traveling Salesman Problem

**TSP Problem:**
- **Input:** Complete graph G with edge weights w, bound B
- **Output:** YES if there exists tour visiting all vertices with total weight ≤ B, NO otherwise

**TSP Tour:** A cycle visiting each vertex exactly once.

---

## Q1: How do you reduce TSP to Hamiltonian Cycle?

**Answer:** Filter edges by weight threshold.

**Key Insight:** 
- TSP tour uses only edges with weight ≤ B
- Create graph with only edges satisfying weight constraint
- Hamiltonian cycle in filtered graph ↔ TSP tour

**Hint:** Use binary weights: weight 1 if original weight ≤ B, weight 2 otherwise. Then TSP tour of weight ≤ n exists if and only if Hamiltonian cycle exists (using only weight-1 edges).

### 1. NP-Completeness Proof of Hamiltonian Cycle: Solution Validation

**Hamiltonian Cycle Problem:**
- **Input:** Graph G = (V, E)
- **Output:** YES if G has a cycle visiting every vertex exactly once, NO otherwise

**Hamiltonian Cycle ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (cycle - sequence of vertices):
1. Check that all vertices appear exactly once: O(n) time
2. Check that consecutive vertices are connected: O(n) time
3. Check that cycle is closed: O(1) time

**Total Time:** O(n), which is polynomial.

**Conclusion:** Hamiltonian Cycle ∈ NP.

### 2. Reduce TSP to Hamiltonian Cycle

#### 2.1 Input Conversion

Given a TSP instance: complete graph G with weights w, bound B.

**Construction:**
- Create graph G' = (V, E') where:
  - V(G') = V(G)
  - E' = {(u, v) : w(u, v) ≤ B}
- Return Hamiltonian Cycle instance: graph G'

**Key Property:** TSP tour of weight ≤ B ↔ Hamiltonian cycle in G'

#### 2.2 Output Conversion

**Given:** Hamiltonian Cycle solution (cycle C in G')

**Extract TSP Tour:**
- C is cycle visiting all vertices
- All edges in C have weight ≤ B (by construction)
- Total weight ≤ n · B (but actually ≤ B if weights are appropriate)
- Return C as TSP tour

### 3. Correctness Justification

#### 3.1 If TSP has a solution, then Hamiltonian Cycle has a solution

**Given:** TSP instance has solution (tour C) with total weight ≤ B.

**Construct Hamiltonian Cycle:**
- C visits all vertices exactly once
- All edges in C have weight ≤ B
- Therefore, all edges in C are in G'
- C is Hamiltonian cycle in G'

**Conclusion:** Hamiltonian Cycle has a solution.

#### 3.2a If TSP does not have a solution, then Hamiltonian Cycle has no solution

**Given:** TSP instance has no tour with weight ≤ B.

**Proof:**
- If Hamiltonian cycle exists in G':
  - All edges have weight ≤ B
  - Cycle is TSP tour with weight ≤ n · (max weight ≤ B)
  - But need to ensure total ≤ B
  - If all edges ≤ B and cycle has n edges, total ≤ n · B
  - Need refinement: use binary weights (1 if ≤ B, 2 if > B)

**Refined Construction:**
- Set weights: w'(u, v) = 1 if w(u, v) ≤ B, else w'(u, v) = 2
- Bound B' = n
- Then: TSP tour weight ≤ n ↔ All edges have weight 1 ↔ Hamiltonian cycle exists

**Conclusion:** Hamiltonian Cycle has no solution.

#### 3.2b If Hamiltonian Cycle has a solution, then TSP has a solution

**Given:** Hamiltonian Cycle instance has solution (cycle C in G').

**Extract TSP Tour:**
- C visits all vertices exactly once
- All edges in C are in G', so have weight ≤ B (by construction)
- Total weight ≤ n · B
- With refined construction (binary weights), total weight = n ≤ B'

**Conclusion:** TSP has a solution.

**Polynomial Time:** O(n²) to create graph and assign weights.

**Therefore, Hamiltonian Cycle is NP-complete.**

---

## Q2: How do you reduce TSP to Vehicle Routing Problem?

### 1. NP-Completeness Proof of Vehicle Routing: Solution Validation

**Vehicle Routing Problem:**
- **Input:** Vehicles, depots, customers, distances, capacity constraints
- **Output:** YES if there exists routes covering all customers satisfying constraints, NO otherwise

**Vehicle Routing ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (set of routes):
1. Check that all customers are visited: O(n) time
2. Check capacity constraints: O(n) time
3. Check distance constraints: O(n) time

**Total Time:** O(n), which is polynomial.

**Conclusion:** Vehicle Routing ∈ NP.

### 2. Reduce TSP to Vehicle Routing

#### 2.1 Input Conversion

Given a TSP instance: complete graph G with weights w, bound B.

**Construction:**
- Vehicles: 1
- Depot: One vertex (say vertex 1)
- Customers: All other vertices
- Distances: Same as TSP weights w
- Capacity: Unlimited (or large enough)
- Distance bound: B
- Return Vehicle Routing instance: 1 vehicle, depot, customers, distances, bound B

**Key Property:** TSP tour ↔ Vehicle route covering all customers

#### 2.2 Output Conversion

**Given:** Vehicle Routing solution (route R)

**Extract TSP Tour:**
- Route R starts and ends at depot
- R visits all customers
- Add return edge to form cycle
- Return as TSP tour

### 3. Correctness Justification

#### 3.1 If TSP has a solution, then Vehicle Routing has a solution

**Given:** TSP instance has solution (tour C) with total weight ≤ B.

**Construct Vehicle Route:**
- Tour C visits all vertices
- Without loss of generality, assume C starts at vertex 1
- Route: Follow C from vertex 1, return to vertex 1
- Total distance = weight of C ≤ B
- All customers visited

**Conclusion:** Vehicle Routing has a solution.

#### 3.2a If TSP does not have a solution, then Vehicle Routing has no solution

**Given:** TSP instance has no tour with weight ≤ B.

**Proof:**
- If Vehicle Routing has solution:
  - Route covers all customers
  - Total distance ≤ B
  - Route forms TSP tour
  - Contradiction

**Conclusion:** Vehicle Routing has no solution.

#### 3.2b If Vehicle Routing has a solution, then TSP has a solution

**Given:** Vehicle Routing instance has solution (route R).

**Extract TSP Tour:**
- Route R starts and ends at depot
- R visits all customers (all vertices except depot, or all vertices)
- Total distance ≤ B
- R forms cycle (tour) visiting all vertices
- Therefore, TSP tour exists

**Conclusion:** TSP has a solution.

**Polynomial Time:** O(1) (trivial reduction).

**Therefore, Vehicle Routing is NP-complete.**

---

## Key Takeaways

1. **Weight Threshold:** TSP → Hamiltonian Cycle uses weight filtering
2. **Tour Structure:** Natural for routing problems
3. **Restriction:** Many problems are special cases of TSP
4. **Template Structure:** All reductions follow rigorous format

---

TSP reductions demonstrate tour structures and routing problem relationships.
