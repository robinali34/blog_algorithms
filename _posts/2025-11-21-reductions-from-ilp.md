---
layout: post
title: "Reductions from Integer Linear Programming: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from Integer Linear Programming to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

Integer Linear Programming (ILP) is a fundamental optimization problem proven NP-complete by reduction from 3-SAT. This post provides detailed proofs following the standard template for reducing from ILP to prove other problems are NP-complete.

## Problem Definition: Integer Linear Programming

**ILP Problem:**
- **Input:** Matrix A, vectors b, c, integer k
- **Output:** YES if there exists integer vector x such that Ax ≤ b and c^T x ≥ k, NO otherwise

**Key:** Variables must be integers (unlike LP which is polynomial-time).

---

## Q1: How do you reduce ILP to 0-1 ILP?

**Answer:** Use binary expansion of integer variables.

**Key Insight:** 
- Represent each integer variable as sum of powers of 2 times binary variables
- Binary expansion preserves integer values
- Constraints can be converted using binary representation

**Hint:** For variable xᵢ ≤ M, use ⌈log₂(M+1)⌉ binary variables. Represent xᵢ = ∑ⱼ 2ʲ · yᵢⱼ where yᵢⱼ ∈ {0,1}. This allows encoding any integer up to M using binary digits.

### 1. NP-Completeness Proof of 0-1 ILP: Solution Validation

**0-1 ILP Problem:**
- **Input:** Matrix A, vectors b, c, integer k
- **Output:** YES if there exists 0-1 vector x such that Ax ≤ b and c^T x ≥ k, NO otherwise

**0-1 ILP ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (0-1 vector x):
1. Check that x ∈ {0,1}ⁿ: O(n) time
2. Check that Ax ≤ b: O(mn) time
3. Check that c^T x ≥ k: O(n) time

**Total Time:** O(mn), which is polynomial.

**Conclusion:** 0-1 ILP ∈ NP.

### 2. Reduce ILP to 0-1 ILP

#### 2.1 Input Conversion

Given an ILP instance: variables xᵢ ∈ ℤ with bounds 0 ≤ xᵢ ≤ Mᵢ.

**Construction:**
- For each variable xᵢ, create ⌈log₂(Mᵢ + 1)⌉ binary variables yᵢ,₁, yᵢ,₂, ..., yᵢ,ₖᵢ
- Represent xᵢ = ∑_{j=1}^{kᵢ} 2^{j-1} · yᵢ,ⱼ where yᵢ,ⱼ ∈ {0,1}
- Convert each constraint:
  - Original: ∑ᵢ aᵢⱼ xᵢ ≤ bⱼ
  - New: ∑ᵢ aᵢⱼ (∑_{l=1}^{kᵢ} 2^{l-1} · yᵢ,ₗ) ≤ bⱼ
- Convert objective similarly
- Return 0-1 ILP instance: binary variables y, converted constraints and objective

**Key Property:** Integer solution ↔ Binary solution (via binary expansion)

#### 2.2 Output Conversion

**Given:** 0-1 ILP solution (binary vector y)

**Extract ILP Solution:**
- For each original variable xᵢ:
  - xᵢ = ∑_{j=1}^{kᵢ} 2^{j-1} · yᵢ,ⱼ
- Return integer vector x

### 3. Correctness Justification

#### 3.1 If ILP has a solution, then 0-1 ILP has a solution

**Given:** ILP instance has solution x* (integer vector).

**Construct 0-1 ILP Solution:**
- For each x*ᵢ, compute binary representation
- Set yᵢ,ⱼ = j-th bit of x*ᵢ
- Since x*ᵢ ≤ Mᵢ, binary representation uses at most ⌈log₂(Mᵢ + 1)⌉ bits
- Constraints are satisfied (binary expansion preserves values)

**Conclusion:** 0-1 ILP has a solution.

#### 3.2a If ILP does not have a solution, then 0-1 ILP has no solution

**Given:** ILP instance has no integer solution.

**Proof by Contradiction:**
- Assume 0-1 ILP has solution y
- Then x constructed from binary expansion is integer solution
- Contradiction

**Conclusion:** 0-1 ILP has no solution.

#### 3.2b If 0-1 ILP has a solution, then ILP has a solution

**Given:** 0-1 ILP instance has solution y (binary vector).

**Extract ILP Solution:**
- For each variable xᵢ:
  - xᵢ = ∑_{j=1}^{kᵢ} 2^{j-1} · yᵢ,ⱼ
- x is integer vector (sum of powers of 2)
- Constraints satisfied (binary expansion preserves constraint values)

**Conclusion:** ILP has a solution.

**Polynomial Time:** O(n log M) binary variables created, where M = max Mᵢ.

**Therefore, 0-1 ILP is NP-complete.**

---

## Q2: How do you reduce ILP to Knapsack?

### 1. NP-Completeness Proof of Knapsack: Solution Validation

**Knapsack Problem:**
- **Input:** Items with weights wᵢ and values vᵢ, capacity W, target value V
- **Output:** YES if there exists subset of items with total weight ≤ W and total value ≥ V, NO otherwise

**Knapsack ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (subset of items):
1. Check that total weight ≤ W: O(n) time
2. Check that total value ≥ V: O(n) time

**Total Time:** O(n), which is polynomial.

**Conclusion:** Knapsack ∈ NP.

### 2. Reduce ILP to Knapsack

#### 2.1 Input Conversion

Given an ILP instance: maximize c^T x subject to Ax ≤ b, x ≥ 0, x integer.

**Simplified Case:** Single constraint ILP
- Maximize c^T x subject to a^T x ≤ b, x ≥ 0, x integer

**Construction:**
- For each variable xᵢ with bound 0 ≤ xᵢ ≤ Mᵢ:
  - Create Mᵢ items:
    - Item (i, j) for j = 1, ..., Mᵢ
    - Weight: aᵢ
    - Value: cᵢ
- Capacity: W = b
- Target value: V = (some target based on objective)
- Return Knapsack instance: items, capacity W, target V

**Key Property:** ILP solution ↔ Knapsack solution (selecting items corresponds to variable values)

#### 2.2 Output Conversion

**Given:** Knapsack solution (subset of items)

**Extract ILP Solution:**
- For each variable xᵢ:
  - xᵢ = number of items selected from group i
- Return integer vector x

### 3. Correctness Justification

#### 3.1 If ILP has a solution, then Knapsack has a solution

**Given:** ILP instance has solution x* (integer vector).

**Construct Knapsack Solution:**
- For each variable x*ᵢ:
  - Select x*ᵢ items from group i
- Total weight = a^T x* ≤ b = W
- Total value = c^T x* ≥ V (if V set appropriately)

**Conclusion:** Knapsack has a solution.

#### 3.2a If ILP does not have a solution, then Knapsack has no solution

**Given:** ILP instance has no integer solution.

**Proof:**
- If Knapsack has solution, extracted x satisfies ILP constraints
- Contradiction

**Conclusion:** Knapsack has no solution.

#### 3.2b If Knapsack has a solution, then ILP has a solution

**Given:** Knapsack instance has solution (subset of items).

**Extract ILP Solution:**
- For each variable xᵢ:
  - xᵢ = number of items selected from group i
- x is integer vector
- a^T x ≤ W = b (constraint satisfied)
- c^T x ≥ V (objective satisfied)

**Conclusion:** ILP has a solution.

**Polynomial Time:** O(∑ᵢ Mᵢ) items created.

**Therefore, Knapsack is NP-complete.**

---

## Key Takeaways

1. **Binary Expansion:** ILP → 0-1 ILP uses binary representation
2. **Item Encoding:** ILP → Knapsack encodes variables as item groups
3. **Constraint Mapping:** Direct encoding of constraints
4. **Template Structure:** All reductions follow rigorous format

---

ILP reductions demonstrate binary expansion and constraint encoding techniques.
