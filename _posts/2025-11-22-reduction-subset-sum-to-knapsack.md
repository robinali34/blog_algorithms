---
layout: post
title: "Reduction: Subset Sum to Knapsack"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce Subset Sum to Knapsack, demonstrating that Knapsack is NP-complete."
---

## Introduction

This post provides a detailed proof that the Knapsack problem is NP-complete by reducing from Subset Sum. The reduction transforms the problem of finding a subset with a target sum into finding items that fit in a knapsack with a given capacity and value.

## Problem Definitions

### Knapsack Problem

**Input:** 
- A set of items I = {1, 2, ..., n}
- For each item i: weight wᵢ ≥ 0 and value vᵢ ≥ 0
- Capacity W ≥ 0
- Target value V ≥ 0

**Output:** YES if there exists a subset S ⊆ I such that:
- ∑_{i ∈ S} wᵢ ≤ W (total weight ≤ capacity)
- ∑_{i ∈ S} vᵢ ≥ V (total value ≥ target)

NO otherwise.

### Subset Sum Problem

**Input:** 
- A set of integers A = {a₁, a₂, ..., aₙ}
- Target integer t ≥ 0

**Output:** YES if there exists a subset S ⊆ {1, 2, ..., n} such that ∑_{i ∈ S} aᵢ = t, NO otherwise.

## 1. NP-Completeness Proof of Knapsack: Solution Validation

### Knapsack ∈ NP

**Verification Algorithm:**
Given a candidate solution (subset S):
1. Check that ∑_{i ∈ S} wᵢ ≤ W: O(n) time
2. Check that ∑_{i ∈ S} vᵢ ≥ V: O(n) time

**Total Time:** O(n), which is polynomial in input size.

**Conclusion:** Knapsack ∈ NP.

## 2. Reduce Subset Sum to Knapsack

**Key Insight:** Given a Subset Sum instance, we can reduce it to Knapsack by setting each item's weight and value equal to the corresponding number, and setting capacity and target value equal to the target sum. Then finding a subset summing to t is equivalent to finding items with total weight ≤ t and total value ≥ t.

**Hint:** When weights equal values, the knapsack constraint becomes: find items with total weight ≤ t and total value ≥ t. Since weights = values, this is equivalent to finding items with total weight = t, which is exactly Subset Sum.

### 2.1 Input Conversion

Given a Subset Sum instance (A, t) where A = {a₁, a₂, ..., aₙ} and target t, we construct a Knapsack instance (I, w, v, W, V).

**Construction:**

**Step 1: Create Items**
- I = {1, 2, ..., n} (one item per number)

**Step 2: Set Weights and Values**
- For each item i: wᵢ = aᵢ (weight equals the number)
- For each item i: vᵢ = aᵢ (value equals the number)

**Step 3: Set Capacity and Target**
- W = t (capacity equals target sum)
- V = t (target value equals target sum)

**Step 4: Result**
- Knapsack instance: (I, w, v, t, t)

**Detailed Example:**

Consider Subset Sum instance:
- Set A = {3, 1, 4, 2, 2}
- Target t = 6

**Transformation:**
- Items: I = {1, 2, 3, 4, 5}
- Weights: w₁ = 3, w₂ = 1, w₃ = 4, w₄ = 2, w₅ = 2
- Values: v₁ = 3, v₂ = 1, v₃ = 4, v₄ = 2, v₅ = 2
- Capacity: W = 6
- Target value: V = 6

**Knapsack instance:** (I, w, v, 6, 6)

### 2.2 Output Conversion

**Given:** Knapsack solution (subset S with total weight ≤ W = t and total value ≥ V = t)

**Extract Subset Sum Solution:**
- Use S as the subset
- Since wᵢ = vᵢ = aᵢ for all i:
  - Total weight = ∑_{i ∈ S} aᵢ ≤ t
  - Total value = ∑_{i ∈ S} aᵢ ≥ t
- Therefore, ∑_{i ∈ S} aᵢ = t (since it's both ≤ t and ≥ t)

**Verify Satisfaction:**
- ∑_{i ∈ S} aᵢ = t: ✓ (by the knapsack constraints)

## 3. Correctness Justification

### 3.1 If Subset Sum has a solution, then Knapsack has a solution

**Given:** Subset Sum instance (A, t) has solution S (subset summing to t).

**Construct Knapsack Solution:**
- Use subset S
- Total weight = ∑_{i ∈ S} wᵢ = ∑_{i ∈ S} aᵢ = t ≤ W = t ✓
- Total value = ∑_{i ∈ S} vᵢ = ∑_{i ∈ S} aᵢ = t ≥ V = t ✓

**Verify Satisfaction:**
- Total weight ≤ W: ✓ (equals t = W)
- Total value ≥ V: ✓ (equals t = V)
- Therefore, S is a valid knapsack solution

**Conclusion:** Knapsack has a solution.

### 3.2a If Subset Sum does not have a solution, then Knapsack has no solution

**Given:** Subset Sum instance (A, t) has no solution (no subset summing to t).

**Proof:**

**Assume:** Knapsack instance (I, w, v, t, t) has a solution (subset S).

**Extract Subset:**
- Use subset S
- Since wᵢ = vᵢ = aᵢ:
  - Total weight = ∑_{i ∈ S} aᵢ ≤ t
  - Total value = ∑_{i ∈ S} aᵢ ≥ t
- Therefore, ∑_{i ∈ S} aᵢ = t

**Contradiction:**
- S is a subset summing to exactly t
- This contradicts the assumption that no such subset exists

**Conclusion:** Knapsack has no solution.

### 3.2b If Knapsack has a solution, then Subset Sum has a solution

**Given:** Knapsack instance (I, w, v, t, t) has solution (subset S).

**Extract Subset:**
- Use subset S
- Since wᵢ = vᵢ = aᵢ for all i:
  - Total weight = ∑_{i ∈ S} wᵢ = ∑_{i ∈ S} aᵢ ≤ t
  - Total value = ∑_{i ∈ S} vᵢ = ∑_{i ∈ S} aᵢ ≥ t
- Therefore, ∑_{i ∈ S} aᵢ = t (since it's both ≤ t and ≥ t)

**Verify Satisfaction:**

**Subset Sum Requirements:**
- S is a subset of {1, 2, ..., n}: ✓ (by construction)
- ∑_{i ∈ S} aᵢ = t: ✓ (by knapsack constraints)

**Key Insight:**
- When weights equal values, the knapsack constraints become:
  - Total weight ≤ t
  - Total value ≥ t
- Since total weight = total value, we must have total weight = total value = t
- This is exactly the Subset Sum requirement

**Conclusion:** The subset S sums to exactly t, so Subset Sum has a solution.

## Polynomial Time Analysis

**Input Size:**
- Subset Sum: n integers a₁, ..., aₙ, target t
- Knapsack: n items with weights w₁, ..., wₙ and values v₁, ..., vₙ, capacity W = t, target V = t

**Construction Time:**
- Create n items: O(n) time
- Set weights and values: O(n) time (wᵢ = vᵢ = aᵢ)
- Set W = t, V = t: O(1) time
- Total: O(n) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Knapsack ∈ NP**: Solutions can be verified in polynomial time
2. **Subset Sum ≤ₚ Knapsack**: Polynomial-time reduction exists
3. **Correctness**: Subset Sum has solution ↔ Knapsack has solution

**Therefore, Knapsack is NP-complete.**

## Key Insights

1. **Weight-Value Equality**: Setting wᵢ = vᵢ = aᵢ makes the knapsack problem equivalent to Subset Sum
2. **Constraint Equivalence**: When weights equal values, "weight ≤ t and value ≥ t" implies "weight = value = t"
3. **Preservation**: The reduction preserves the subset sum structure through weight-value encoding
4. **Simplicity**: The reduction is straightforward because Knapsack generalizes Subset Sum

## Practice Questions

1. **Modify the reduction** to reduce Partition to Knapsack. How would you set the parameters?
2. **Extend the reduction** to handle 0-1 Knapsack (each item can be taken at most once). Does the reduction still work?
3. **Consider fractional knapsack:** What if items can be taken fractionally? Is the problem still NP-complete?
4. **Prove the reverse reduction:** Can we reduce Knapsack to Subset Sum? How?
5. **Investigate special cases:** For what weight/value ratios is Knapsack polynomial-time solvable?

---

This reduction demonstrates how optimization constraints (weight and value) can encode exact sum requirements, enabling reductions between subset selection problems.

