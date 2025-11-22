---
layout: post
title: "Reductions from Clique: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from Clique to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

Clique is a fundamental graph problem proven NP-complete. This post provides detailed proofs following the standard template for reducing from Clique to prove other problems are NP-complete.

## Problem Definition: Clique

**Clique Problem:**
- **Input:** Graph G = (V, E) and integer k
- **Output:** YES if G has a clique of size ≥ k, NO otherwise

**Clique:** A subset of vertices where every pair is connected by an edge.

---

## Q1: How do you reduce Clique to Independent Set?

**Answer:** Use complement graph.

**Key Insight:** 
- S is a clique in G ↔ S is an independent set in G̅ (complement graph)
- Complement graph reverses edge relationships

**Hint:** The complement graph G̅ has edges exactly where G doesn't. This transforms "all pairs connected" into "no pairs connected".

### 1. NP-Completeness Proof of Independent Set: Solution Validation

**Independent Set Problem:**
- **Input:** Graph G = (V, E) and integer k
- **Output:** YES if G has an independent set of size ≥ k, NO otherwise

**Independent Set ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (set S of vertices):
1. Check that S ⊆ V: O(`|S|`) time
2. Check that `|S|` ≥ k: O(1) time
3. For each pair (u, v) in S, check if (u, v) ∈ E: O(`|S|`²) time
4. If no edges found, return YES; else return NO

**Total Time:** O(`|S|`²), which is polynomial.

**Conclusion:** Independent Set ∈ NP.

### 2. Reduce Clique to Independent Set

#### 2.1 Input Conversion

Given a Clique instance: graph G = (V, E), integer k.

**Construction:**
- Create complement graph G̅ = (V, E̅) where:
  - V(G̅) = V(G)
  - E̅ = {(u, v) : u, v ∈ V, u ≠ v, (u, v) ∉ E}
- Return Independent Set instance: graph G̅, integer k

**Key Property:** S is clique in G ↔ S is independent set in G̅

#### 2.2 Output Conversion

**Given:** Independent Set solution S of size k in G̅

**Extract Clique:**
- S is independent set in G̅ ↔ S is clique in G
- Return S as clique

### 3. Correctness Justification

#### 3.1 If Clique has a solution, then Independent Set has a solution

**Given:** Clique instance has solution S of size k in G.

**Construct Independent Set:**
- S is clique in G
- By definition of complement graph: S is independent set in G̅
- Therefore, S is independent set of size k in G̅

**Conclusion:** Independent Set has a solution.

#### 3.2a If Clique does not have a solution, then Independent Set has no solution

**Given:** Clique instance has no solution of size k in G.

**Proof by Contradiction:**
- Assume Independent Set has solution S of size k in G̅
- Then S is clique of size k in G
- Contradiction

**Conclusion:** Independent Set has no solution.

#### 3.2b If Independent Set has a solution, then Clique has a solution

**Given:** Independent Set instance has solution S of size k in G̅.

**Extract Clique:**
- S is independent set in G̅ ↔ S is clique in G
- Therefore, S is clique of size k in G

**Conclusion:** Clique has a solution.

**Polynomial Time:** O(n²) to create complement graph.

**Therefore, Independent Set is NP-complete.**

---

## Q2: How do you reduce Clique to Subgraph Isomorphism?

### 1. NP-Completeness Proof of Subgraph Isomorphism: Solution Validation

**Subgraph Isomorphism Problem:**
- **Input:** Graphs G and H
- **Output:** YES if H is isomorphic to a subgraph of G, NO otherwise

**Subgraph Isomorphism ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (mapping f: V(H) → V(G)):
1. Check that f is injective: O(`|V(H)|`²) time
2. For each edge (u, v) ∈ E(H), check if (f(u), f(v)) ∈ E(G): O(`|E(H)|`) time

**Total Time:** O(`|V(H)|`² + `|E(H)|`), which is polynomial.

**Conclusion:** Subgraph Isomorphism ∈ NP.

### 2. Reduce Clique to Subgraph Isomorphism

#### 2.1 Input Conversion

Given a Clique instance: graph G = (V, E), integer k.

**Construction:**
- Create complete graph Kₖ (clique of size k)
- Return Subgraph Isomorphism instance: graph G, pattern H = Kₖ

**Key Property:** Clique of size k in G ↔ Kₖ is subgraph of G

#### 2.2 Output Conversion

**Given:** Subgraph Isomorphism solution (mapping f: V(Kₖ) → V(G))

**Extract Clique:**
- S = {f(v) : v ∈ V(Kₖ)}
- `|S|` = k
- S is clique in G (Kₖ is complete graph)

### 3. Correctness Justification

#### 3.1 If Clique has a solution, then Subgraph Isomorphism has a solution

**Given:** Clique instance has solution S of size k in G.

**Construct Mapping:**
- S is clique of size k
- Create bijection f: V(Kₖ) → S
- For each edge (u, v) in Kₖ, (f(u), f(v)) is edge in G (since S is clique)
- Therefore, Kₖ is isomorphic to subgraph induced by S

**Conclusion:** Subgraph Isomorphism has a solution.

#### 3.2a If Clique does not have a solution, then Subgraph Isomorphism has no solution

**Given:** Clique instance has no solution of size k in G.

**Proof:**
- If Kₖ is subgraph of G, then vertices of Kₖ form clique of size k
- Contradiction

**Conclusion:** Subgraph Isomorphism has no solution.

#### 3.2b If Subgraph Isomorphism has a solution, then Clique has a solution

**Given:** Subgraph Isomorphism instance has solution (mapping f: V(Kₖ) → V(G)).

**Extract Clique:**
- S = {f(v) : v ∈ V(Kₖ)}
- `|S|` = k
- Since Kₖ is complete, all pairs in S are connected
- S is clique of size k

**Conclusion:** Clique has a solution.

**Polynomial Time:** O(1) (trivial reduction).

**Therefore, Subgraph Isomorphism is NP-complete.**

---

## Key Takeaways

1. **Complement Graph:** Powerful tool for reductions
2. **Subgraph Structure:** Clique is complete subgraph
3. **Restriction:** Many problems generalize Clique
4. **Template Structure:** All reductions follow rigorous format

---

Clique reductions demonstrate the power of complement relationships and subgraph structures.
