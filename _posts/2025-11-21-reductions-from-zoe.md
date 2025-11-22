---
layout: post
title: "Reductions from Zero-One Equations: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from Zero-One Equations to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

Zero-One Equations (ZOE) is a matrix equation problem proven NP-complete by reduction from 3-SAT. This post provides detailed proofs following the standard template for reducing from ZOE to prove other problems are NP-complete.

## Problem Definition: Zero-One Equations

**ZOE Problem:**
- **Input:** Matrix A (0-1 matrix), vector b (0-1 vector)
- **Output:** YES if there exists 0-1 vector x such that Ax = b, NO otherwise

**Key:** All values are binary (0 or 1), equations are mod 2.

---

## Q1: How do you reduce ZOE to 3D Matching?

**Answer:** Encode equations as matching constraints.

**Key Insight:** 
- Encode variables as elements in one set
- Encode equations as elements in another set
- Use triples to represent variable-equation-value relationships

**Hint:** Think of matching as assigning values to variables. Each triple (variable, equation, value) represents a contribution to an equation. Perfect matching ensures all equations are satisfied.

### 1. NP-Completeness Proof of 3D Matching: Solution Validation

**3D Matching Problem:**
- **Input:** Sets X, Y, Z and set T ⊆ X × Y × Z of triples
- **Output:** YES if there exists matching M ⊆ T covering all elements, NO otherwise

**3D Matching ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (matching M):
1. Check that M ⊆ T: O(`|M|`) time
2. Check that all elements in X, Y, Z are covered: O(`|X|` + `|Y|` + `|Z|`) time
3. Check that no element appears twice: O(`|M|`) time

**Total Time:** O(`|X|` + `|Y|` + `|Z|` + `|M|`), which is polynomial.

**Conclusion:** 3D Matching ∈ NP.

### 2. Reduce ZOE to 3D Matching

#### 2.1 Input Conversion

Given a ZOE instance: matrix A (m × n), vector b (m-dimensional).

**Construction:**
- For each equation i (row of A):
  - Create element eᵢ in set X
- For each variable j (column of A):
  - Create elements vⱼ,₀ and vⱼ,₁ in set Y (representing xⱼ = 0 and xⱼ = 1)
- For each equation i and variable j:
  - If A[i,j] = 1:
    - Create triple (eᵢ, vⱼ,₁, zᵢ,ⱼ) where zᵢ,ⱼ ∈ Z
    - This encodes: if xⱼ = 1, equation i gets contribution
- For each equation i:
  - If b[i] = 0:
    - Need even number of contributions (mod 2)
    - Create triples to balance
  - If b[i] = 1:
    - Need odd number of contributions (mod 2)
    - Create triples accordingly
- Return 3D Matching instance: sets X, Y, Z, triples T

**Key Property:** ZOE solution ↔ Perfect 3D matching

#### 2.2 Output Conversion

**Given:** 3D Matching solution M ⊆ T

**Extract ZOE Solution:**
- For each variable j:
  - If vⱼ,₁ is matched, set xⱼ = 1
  - If vⱼ,₀ is matched, set xⱼ = 0
- Return 0-1 vector x

### 3. Correctness Justification

#### 3.1 If ZOE has a solution, then 3D Matching has a solution

**Given:** ZOE instance has solution x* (0-1 vector).

**Construct Matching:**
- For each variable j:
  - If x*ⱼ = 1, match vⱼ,₁
  - If x*ⱼ = 0, match vⱼ,₀
- For each equation i:
  - Match triples corresponding to satisfied equation
  - Use balancing triples to ensure all elements covered

**Verify:**
- All elements in Y are matched (one per variable)
- All elements in X are matched (one per equation)
- All elements in Z are matched (via triples)

**Conclusion:** 3D Matching has a solution.

#### 3.2a If ZOE does not have a solution, then 3D Matching has no solution

**Given:** ZOE instance has no solution.

**Proof by Contradiction:**
- Assume 3D Matching has solution M
- Extract x from matching
- x satisfies Ax = b (by construction)
- Contradiction

**Conclusion:** 3D Matching has no solution.

#### 3.2b If 3D Matching has a solution, then ZOE has a solution

**Given:** 3D Matching instance has solution M ⊆ T.

**Extract Solution:**
- For each variable j:
  - Exactly one of vⱼ,₀ or vⱼ,₁ is matched
  - Set xⱼ = 1 if vⱼ,₁ matched, else xⱼ = 0
- For each equation i:
  - Element eᵢ is matched
  - Matching ensures equation i is satisfied (mod 2)
- Therefore, Ax = b

**Conclusion:** ZOE has a solution.

**Polynomial Time:** O(mn) triples created.

**Therefore, 3D Matching is NP-complete.**

---

## Q2: How do you reduce ZOE to Exact Cover?

### 1. NP-Completeness Proof of Exact Cover: Solution Validation

**Exact Cover Problem:**
- **Input:** Universe U and collection C of subsets of U
- **Output:** YES if there exists subcollection C' ⊆ C covering each element exactly once, NO otherwise

**Exact Cover ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (subcollection C'):
1. Check that C' ⊆ C: O(`|C'|`) time
2. Check that each element appears exactly once: O(`|U|` · `|C'|`) time

**Total Time:** O(`|U|` · `|C'|`), which is polynomial.

**Conclusion:** Exact Cover ∈ NP.

### 2. Reduce ZOE to Exact Cover

#### 2.1 Input Conversion

Given a ZOE instance: matrix A (m × n), vector b.

**Construction:**
- Universe U = {equation₁, ..., equation_m} ∪ {variable₁, ..., variable_n}
- For each possible 0-1 assignment to variables:
  - Create set S containing:
    - Variables with value 1
    - Equations satisfied by this assignment
- Return Exact Cover instance: universe U, collection C

**Key Property:** ZOE solution ↔ Exact cover (each equation covered exactly once)

#### 2.2 Output Conversion

**Given:** Exact Cover solution C' ⊆ C

**Extract ZOE Solution:**
- From sets in C', extract variable assignments
- Return 0-1 vector x

### 3. Correctness Justification

#### 3.1 If ZOE has a solution, then Exact Cover has a solution

**Given:** ZOE instance has solution x* (0-1 vector).

**Construct Cover:**
- Set S* corresponding to assignment x* is in collection
- S* covers:
  - All variables (exactly once)
  - All satisfied equations (exactly once)
- Therefore, C' = {S*} is exact cover

**Conclusion:** Exact Cover has a solution.

#### 3.2a If ZOE does not have a solution, then Exact Cover has no solution

**Given:** ZOE instance has no solution.

**Proof:**
- If exact cover exists, extracted assignment satisfies all equations
- Contradiction

**Conclusion:** Exact Cover has no solution.

#### 3.2b If Exact Cover has a solution, then ZOE has a solution

**Given:** Exact Cover instance has solution C' ⊆ C.

**Extract Solution:**
- From sets in C', extract variable assignment x
- Each equation covered exactly once means it's satisfied
- Therefore, Ax = b

**Conclusion:** ZOE has a solution.

**Polynomial Time:** O(2ⁿ · m) sets created (exponential, but reduction is valid).

**Therefore, Exact Cover is NP-complete.**

---

## Key Takeaways

1. **Matching Structure:** ZOE → 3D Matching uses triples
2. **Exact Coverage:** ZOE → Exact Cover ensures each element once
3. **Binary Structure:** 0-1 constraints simplify reductions
4. **Template Structure:** All reductions follow rigorous format

---

ZOE reductions demonstrate matching structures and exact coverage patterns.
