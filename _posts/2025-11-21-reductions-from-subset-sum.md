---
layout: post
title: "Reductions from Subset Sum: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from Subset Sum to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

Subset Sum is a fundamental number problem proven NP-complete by reduction from 3-SAT. This post provides detailed proofs following the standard template for reducing from Subset Sum to prove other problems are NP-complete.

## Problem Definition: Subset Sum

**Subset Sum Problem:**
- **Input:** Set S of integers and target integer t
- **Output:** YES if there exists subset S' ⊆ S with sum exactly t, NO otherwise

---

## Q1: How do you reduce Subset Sum to Partition?

**Answer:** Add balancing elements to create equal partition.

**Key Insight:** 
- If subset sums to t, add elements that force equal partition
- Add elements 2T - t and T + t where T is total sum
- These balance the partition

**Hint:** The key is to add two elements that, together with the subset and its complement, create equal sums. If one partition has the subset (sum t) and 2T-t, the other has the complement (sum T-t) and T+t, both sum to 2T.

### 1. NP-Completeness Proof of Partition: Solution Validation

**Partition Problem:**
- **Input:** Set A of integers
- **Output:** YES if A can be partitioned into two subsets with equal sum, NO otherwise

**Partition ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (partition S₁, S₂):
1. Check that S₁ ∪ S₂ = A: O(|A|) time
2. Check that S₁ ∩ S₂ = ∅: O(|A|) time
3. Compute sum₁ = ∑_{a ∈ S₁} a: O(|A|) time
4. Compute sum₂ = ∑_{a ∈ S₂} a: O(|A|) time
5. Check if sum₁ = sum₂: O(1) time

**Total Time:** O(|A|), which is polynomial.

**Conclusion:** Partition ∈ NP.

### 2. Reduce Subset Sum to Partition

#### 2.1 Input Conversion

Given a Subset Sum instance: set S = {a₁, a₂, ..., a_n}, target t.

**Construction:**
- Compute T = ∑_{i=1}^n aᵢ (total sum)
- Create set A = S ∪ {2T - t, T + t}
- Return Partition instance: set A

**Key Property:** Subset summing to t ↔ Partition exists

#### 2.2 Output Conversion

**Given:** Partition solution (S₁, S₂) with sum(S₁) = sum(S₂)

**Extract Subset Sum:**
- Total sum of A = T + (2T - t) + (T + t) = 4T
- Each partition sums to 2T
- If {2T - t, T + t} are in different partitions:
  - One partition has subset from S summing to t
  - Return that subset

### 3. Correctness Justification

#### 3.1 If Subset Sum has a solution, then Partition has a solution

**Given:** Subset Sum instance has solution S' ⊆ S with sum(S') = t.

**Construct Partition:**
- Let S'' = S \ S'
- sum(S'') = T - t
- Partition:
  - S₁ = S' ∪ {2T - t}
  - S₂ = S'' ∪ {T + t}
- sum(S₁) = t + (2T - t) = 2T
- sum(S₂) = (T - t) + (T + t) = 2T
- Therefore, partition exists

**Conclusion:** Partition has a solution.

#### 3.2a If Subset Sum does not have a solution, then Partition has no solution

**Given:** Subset Sum instance has no solution summing to t.

**Proof:**
- Total sum of A = 4T
- Each partition must sum to 2T
- If partition exists, then:
  - One partition contains {2T - t} and subset from S summing to t
  - Other partition contains {T + t} and remaining subset
- But no subset sums to t, contradiction

**Conclusion:** Partition has no solution.

#### 3.2b If Partition has a solution, then Subset Sum has a solution

**Given:** Partition instance has solution (S₁, S₂) with sum(S₁) = sum(S₂) = 2T.

**Extract Subset:**
- Since sum(A) = 4T, each partition sums to 2T
- Elements {2T - t, T + t} must be in different partitions (they sum to 3T - t + t = 3T > 2T)
- Without loss of generality, assume 2T - t ∈ S₁, T + t ∈ S₂
- Then S₁ \ {2T - t} ⊆ S and sums to 2T - (2T - t) = t
- Therefore, subset summing to t exists

**Conclusion:** Subset Sum has a solution.

**Polynomial Time:** O(n) to compute sum and add elements.

**Therefore, Partition is NP-complete.**

---

## Q2: How do you reduce Subset Sum to Knapsack?

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

### 2. Reduce Subset Sum to Knapsack

#### 2.1 Input Conversion

Given a Subset Sum instance: set S = {a₁, a₂, ..., a_n}, target t.

**Construction:**
- Create n items:
  - Item i: weight wᵢ = aᵢ, value vᵢ = aᵢ
- Capacity W = t
- Target value V = t
- Return Knapsack instance: items, capacity W, target V

**Key Property:** Subset summing to t ↔ Knapsack solution with weight = value = t

#### 2.2 Output Conversion

**Given:** Knapsack solution (subset of items) with total weight ≤ t and total value ≥ t

**Extract Subset:**
- Since weights = values, total weight = total value
- If total weight = t, return corresponding subset
- If total weight < t, cannot achieve value t (since values = weights)

### 3. Correctness Justification

#### 3.1 If Subset Sum has a solution, then Knapsack has a solution

**Given:** Subset Sum instance has solution S' ⊆ S with sum(S') = t.

**Construct Knapsack Solution:**
- Select items corresponding to S'
- Total weight = sum(S') = t ≤ W
- Total value = sum(S') = t ≥ V
- Therefore, Knapsack solution exists

**Conclusion:** Knapsack has a solution.

#### 3.2a If Subset Sum does not have a solution, then Knapsack has no solution

**Given:** Subset Sum instance has no solution summing to t.

**Proof:**
- To achieve value V = t, need total value = t
- Since values = weights, need total weight = t
- But no subset sums to t, contradiction

**Conclusion:** Knapsack has no solution.

#### 3.2b If Knapsack has a solution, then Subset Sum has a solution

**Given:** Knapsack instance has solution with total weight ≤ t and total value ≥ t.

**Extract Subset:**
- Since weights = values, total weight = total value
- To have total value ≥ t and total weight ≤ t, must have total weight = total value = t
- Corresponding subset sums to t

**Conclusion:** Subset Sum has a solution.

**Polynomial Time:** O(n) to create items.

**Therefore, Knapsack is NP-complete.**

---

## Key Takeaways

1. **Balancing Elements:** Partition reductions add balancing numbers
2. **Restriction:** Knapsack generalizes Subset Sum
3. **Weight-Value Equality:** Simplifies reductions
4. **Template Structure:** All reductions follow rigorous format

---

Subset Sum reductions demonstrate balancing techniques and restriction relationships.
