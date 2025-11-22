---
layout: post
title: "Reductions from 3D Matching: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from 3D Matching to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

3D Matching is a fundamental matching problem proven NP-complete by reduction from 3-SAT. This post provides detailed proofs following the standard template for reducing from 3D Matching to prove other problems are NP-complete.

## Problem Definition: 3D Matching

**3D Matching Problem:**
- **Input:** Sets X, Y, Z and set T ⊆ X × Y × Z of triples
- **Output:** YES if there exists matching M ⊆ T covering all elements, NO otherwise

**Perfect 3D Matching:** A set of triples covering each element exactly once.

---

## Q1: How do you reduce 3D Matching to Set Cover?

**Answer:** Encode triples as sets covering elements.

**Key Insight:** 
- Each triple covers three elements (one from each set X, Y, Z)
- Perfect matching covers all elements exactly once
- Set cover allows covering elements multiple times, but perfect matching ensures exactly once

**Hint:** Each triple becomes a set containing its three elements. A perfect 3D matching corresponds to a set cover of size |X| = |Y| = |Z|, where each element is covered exactly once.

### 1. NP-Completeness Proof of Set Cover: Solution Validation

**Set Cover Problem:**
- **Input:** Universe U, collection C of subsets of U, integer k
- **Output:** YES if there exists subcollection C' ⊆ C with |C'| ≤ k covering U, NO otherwise

**Set Cover ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (subcollection C'):
1. Check that C' ⊆ C: O(|C'|) time
2. Check that |C'| ≤ k: O(1) time
3. Check that ∪_{S ∈ C'} S = U: O(|U| · |C'|) time

**Total Time:** O(|U| · |C'|), which is polynomial.

**Conclusion:** Set Cover ∈ NP.

### 2. Reduce 3D Matching to Set Cover

#### 2.1 Input Conversion

Given a 3D Matching instance: sets X, Y, Z, triples T ⊆ X × Y × Z.

**Construction:**
- Universe U = X ∪ Y ∪ Z (all elements)
- For each triple t = (x, y, z) ∈ T, create set Sₜ = {x, y, z}
- Collection C = {Sₜ : t ∈ T}
- Target k = |X| = |Y| = |Z| (assuming equal sizes)
- Return Set Cover instance: universe U, collection C, integer k

**Key Property:** Perfect 3D matching ↔ Set cover of size k

#### 2.2 Output Conversion

**Given:** Set Cover solution C' ⊆ C with |C'| ≤ k covering U

**Extract Matching:**
- M = {t ∈ T : Sₜ ∈ C'}
- |M| = |C'| ≤ k
- Since |X| = |Y| = |Z| = k and each set covers 3 elements, need exactly k sets
- M is perfect 3D matching

### 3. Correctness Justification

#### 3.1 If 3D Matching has a solution, then Set Cover has a solution

**Given:** 3D Matching instance has solution M ⊆ T (perfect matching).

**Construct Set Cover:**
- C' = {Sₜ : t ∈ M}
- |C'| = |M| = k
- Since M covers all elements:
  - Each x ∈ X appears in exactly one triple t ∈ M
  - Each y ∈ Y appears in exactly one triple t ∈ M
  - Each z ∈ Z appears in exactly one triple t ∈ M
- Therefore, C' covers all elements in U

**Conclusion:** Set Cover has a solution.

#### 3.2a If 3D Matching does not have a solution, then Set Cover has no solution

**Given:** 3D Matching instance has no perfect matching.

**Proof:**
- If Set Cover has solution C' of size k:
  - Then M = {t : Sₜ ∈ C'} is matching of size k
  - Since |X| = |Y| = |Z| = k, M covers all elements
  - M is perfect matching
  - Contradiction

**Conclusion:** Set Cover has no solution.

#### 3.2b If Set Cover has a solution, then 3D Matching has a solution

**Given:** Set Cover instance has solution C' ⊆ C with |C'| ≤ k covering U.

**Extract Matching:**
- M = {t ∈ T : Sₜ ∈ C'}
- |M| = |C'| ≤ k
- Since C' covers U and |U| = 3k:
  - Need at least k sets (each covers 3 elements)
  - Have at most k sets
  - Therefore, exactly k sets, each covering 3 distinct elements
- M is perfect 3D matching

**Conclusion:** 3D Matching has a solution.

**Polynomial Time:** O(|T|) to create sets.

**Therefore, Set Cover is NP-complete.**

---

## Q2: How do you reduce 3D Matching to Exact Cover?

### 1. NP-Completeness Proof of Exact Cover: Solution Validation

**Exact Cover Problem:**
- **Input:** Universe U and collection C of subsets of U
- **Output:** YES if there exists subcollection C' ⊆ C covering each element exactly once, NO otherwise

**Exact Cover ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (subcollection C'):
1. Check that C' ⊆ C: O(|C'|) time
2. Check that each element appears exactly once: O(|U| · |C'|) time

**Total Time:** O(|U| · |C'|), which is polynomial.

**Conclusion:** Exact Cover ∈ NP.

### 2. Reduce 3D Matching to Exact Cover

#### 2.1 Input Conversion

Given a 3D Matching instance: sets X, Y, Z, triples T ⊆ X × Y × Z.

**Construction:**
- Universe U = X ∪ Y ∪ Z
- For each triple t = (x, y, z) ∈ T, create set Sₜ = {x, y, z}
- Collection C = {Sₜ : t ∈ T}
- Return Exact Cover instance: universe U, collection C

**Key Property:** Perfect 3D matching ↔ Exact cover

#### 2.2 Output Conversion

**Given:** Exact Cover solution C' ⊆ C

**Extract Matching:**
- M = {t ∈ T : Sₜ ∈ C'}
- M is perfect 3D matching (each element covered exactly once)

### 3. Correctness Justification

#### 3.1 If 3D Matching has a solution, then Exact Cover has a solution

**Given:** 3D Matching instance has solution M ⊆ T (perfect matching).

**Construct Exact Cover:**
- C' = {Sₜ : t ∈ M}
- Since M is perfect matching:
  - Each element in X appears in exactly one triple t ∈ M
  - Each element in Y appears in exactly one triple t ∈ M
  - Each element in Z appears in exactly one triple t ∈ M
- Therefore, C' covers each element exactly once

**Conclusion:** Exact Cover has a solution.

#### 3.2a If 3D Matching does not have a solution, then Exact Cover has no solution

**Given:** 3D Matching instance has no perfect matching.

**Proof by Contradiction:**
- Assume Exact Cover has solution C'
- Then M = {t : Sₜ ∈ C'} is perfect matching
- Contradiction

**Conclusion:** Exact Cover has no solution.

#### 3.2b If Exact Cover has a solution, then 3D Matching has a solution

**Given:** Exact Cover instance has solution C' ⊆ C.

**Extract Matching:**
- M = {t ∈ T : Sₜ ∈ C'}
- Since C' covers each element exactly once:
  - M covers all elements
  - Each element appears in exactly one triple
  - M is perfect 3D matching

**Conclusion:** 3D Matching has a solution.

**Polynomial Time:** O(1) (trivial reduction).

**Therefore, Exact Cover is NP-complete.**

---

## Key Takeaways

1. **Set Encoding:** 3D Matching → Set Cover encodes triples as sets
2. **Exact Coverage:** Perfect matching ensures exact coverage
3. **Triple Structure:** Three sets make matching harder than 2D
4. **Template Structure:** All reductions follow rigorous format

---

3D Matching reductions demonstrate covering structures and exact coverage patterns.
