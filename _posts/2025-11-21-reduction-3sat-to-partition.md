---
layout: post
title: "Reduction: 3-SAT to Partition"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce 3-SAT to Partition, encoding variable assignments and clause satisfaction using subset sums."
---

## Introduction

This post provides a detailed proof that the Partition problem is NP-complete by reducing from 3-SAT. The reduction encodes variable assignments and clause satisfaction using numbers and subset sums, demonstrating how logical constraints can be represented arithmetically.

## Problem Definitions

### Partition Problem

**Input:** A set of integers A = {a₁, a₂, ..., a_n}

**Output:** YES if A can be partitioned into two subsets S₁ and S₂ such that:
- S₁ ∪ S₂ = A
- S₁ ∩ S₂ = ∅
- ∑_{a ∈ S₁} a = ∑_{a ∈ S₂} a

NO otherwise.

### 3-SAT Problem

**Input:** A Boolean formula φ in 3-CNF with:
- Variables: x₁, x₂, ..., x_n
- Clauses: C₁, C₂, ..., C_m, each with exactly 3 literals

**Output:** YES if φ is satisfiable, NO otherwise.

## 1. NP-Completeness Proof of Partition: Solution Validation

### Partition ∈ NP

**Verification Algorithm:**
Given a candidate solution (partition S₁, S₂):
1. Check that S₁ ∪ S₂ = A: O(n) time
2. Check that S₁ ∩ S₂ = ∅: O(n) time
3. Compute sum₁ = ∑_{a ∈ S₁} a: O(n) time
4. Compute sum₂ = ∑_{a ∈ S₂} a: O(n) time
5. Check if sum₁ = sum₂: O(1) time

**Total Time:** O(n), which is polynomial in input size.

**Conclusion:** Partition ∈ NP.

## 2. Reduce 3-SAT to Partition

### 2.1 Input Conversion

Given a 3-SAT instance with n variables and m clauses, we construct a Partition instance.

**Key Idea:** Use base representation to encode variable assignments and clause satisfaction.

**Construction:**

**Step 1: Create Variable Numbers**
- For each variable xᵢ, create two numbers:
  - **vᵢ**: Represents xᵢ = TRUE
  - **v'ᵢ**: Represents xᵢ = FALSE
- Each number has n + m digits (base representation)
- Variable digits: positions 1 through n
- Clause digits: positions n+1 through n+m

**Step 2: Encode Variable Assignments**
- For variable xᵢ:
  - vᵢ has digit i = 1, all other variable digits = 0
  - v'ᵢ has digit i = 1, all other variable digits = 0
- This ensures exactly one of vᵢ or v'ᵢ is chosen (representing TRUE or FALSE)

**Step 3: Encode Clause Satisfaction**
- For clause Cⱼ and variable xᵢ:
  - If xᵢ appears positively in Cⱼ: add 1 to clause digit (n+j) of vᵢ
  - If ¬xᵢ appears in Cⱼ: add 1 to clause digit (n+j) of v'ᵢ
- This ensures that if a clause is satisfied, the corresponding clause digit contributes to the sum

**Step 4: Create Clause Numbers**
- For each clause Cⱼ, create two "slack" numbers:
  - **sⱼ**: Has digit (n+j) = 1, all other digits = 0
  - **s'ⱼ**: Has digit (n+j) = 1, all other digits = 0
- These allow flexibility in achieving the target sum

**Step 5: Set Target**
- Compute total sum T = sum of all numbers created
- Target for Partition: T/2
- Since we have variable numbers (2n numbers) and clause slack numbers (2m numbers), total is 2(n+m) numbers

**Detailed Example:**

Consider 3-SAT instance:
- Variables: x₁, x₂
- Clauses: C₁ = (x₁ ∨ ¬x₂ ∨ x₁), C₂ = (¬x₁ ∨ x₂ ∨ x₂)

**Variable Numbers (base 10, but think in base representation):**
- v₁ (x₁ = TRUE): digit 1 = 1, digit 3 (clause 1) = 1, digit 4 (clause 2) = 0 → value encoding
- v'₁ (x₁ = FALSE): digit 1 = 1, digit 3 = 0, digit 4 = 1
- v₂ (x₂ = TRUE): digit 2 = 1, digit 3 = 0, digit 4 = 1
- v'₂ (x₂ = FALSE): digit 2 = 1, digit 3 = 1, digit 4 = 0

**Clause Slack Numbers:**
- s₁: digit 3 = 1
- s'₁: digit 3 = 1
- s₂: digit 4 = 1
- s'₂: digit 4 = 1

**Total Set:** {v₁, v'₁, v₂, v'₂, s₁, s'₁, s₂, s'₂}
**Target:** T/2 where T = sum of all numbers

### 2.2 Output Conversion

**Given:** Partition solution (S₁, S₂) where sum(S₁) = sum(S₂) = T/2

**Extract Variable Assignment:**
- For each variable xᵢ:
  - If vᵢ ∈ S₁ (or S₂), set xᵢ = TRUE
  - If v'ᵢ ∈ S₁ (or S₂), set xᵢ = FALSE
  - Exactly one of vᵢ or v'ᵢ must be in each partition (by construction)

**Verify Clause Satisfaction:**
- For each clause Cⱼ:
  - Check if at least one literal is TRUE
  - This corresponds to clause digit (n+j) summing to at least 1 in the chosen partition

## 3. Correctness Justification

### 3.1 If 3-SAT has a solution, then Partition has a solution

**Given:** 3-SAT instance φ is satisfiable with assignment A.

**Construct Partition:**

**Step 1: Choose Variable Numbers**
- For each variable xᵢ:
  - If A(xᵢ) = TRUE, put vᵢ in S₁
  - If A(xᵢ) = FALSE, put v'ᵢ in S₁
- Put the other variable number (v'ᵢ or vᵢ) in S₂

**Step 2: Balance Clause Digits**
- For each clause Cⱼ:
  - Since φ is satisfiable, at least one literal in Cⱼ is TRUE
  - This means clause digit (n+j) has contribution ≥ 1 from variable numbers in S₁
  - Use slack numbers sⱼ and s'ⱼ to balance:
    - If clause digit in S₁ needs more, add sⱼ to S₂
    - If clause digit in S₂ needs more, add s'ⱼ to S₁
    - Distribute slack numbers to achieve balance

**Step 3: Verify Balance**
- Variable digits: Each position i has exactly one 1 in S₁ and one 1 in S₂ (balanced)
- Clause digits: Can be balanced using slack numbers
- Therefore, sum(S₁) = sum(S₂) = T/2

**Conclusion:** Partition has a solution.

### 3.2a If 3-SAT does not have a solution, then Partition has no solution

**Given:** 3-SAT instance φ is unsatisfiable.

**Proof by Contradiction:**

Assume Partition has a solution (S₁, S₂).

**Extract Assignment:**
- For each variable xᵢ:
  - If vᵢ ∈ S₁, set xᵢ = TRUE
  - If v'ᵢ ∈ S₁, set xᵢ = FALSE
  - (Exactly one must be in S₁ by construction)

**Check Clause Satisfaction:**
- For each clause Cⱼ:
  - Clause digit (n+j) must sum to the same value in S₁ and S₂
  - Variable numbers contribute based on assignment
  - Slack numbers can adjust, but cannot create satisfaction

**Contradiction:**
- If φ is unsatisfiable, there exists at least one clause Cⱼ where all literals are FALSE
- This means clause digit (n+j) gets no contribution from variable numbers representing TRUE literals
- To balance, we'd need slack numbers, but this doesn't correspond to a satisfying assignment
- However, the construction ensures that if clause digit balances, at least one literal must be TRUE
- This contradicts unsatisfiability

**Conclusion:** Partition has no solution.

### 3.2b If Partition has a solution, then 3-SAT has a solution

**Given:** Partition instance has solution (S₁, S₂) with sum(S₁) = sum(S₂) = T/2.

**Extract Variable Assignment:**
- For each variable xᵢ:
  - Exactly one of {vᵢ, v'ᵢ} is in S₁ (by construction of numbers)
  - If vᵢ ∈ S₁, set xᵢ = TRUE
  - If v'ᵢ ∈ S₁, set xᵢ = FALSE

**Verify Clause Satisfaction:**

**For each clause Cⱼ:**
- Clause digit (n+j) must sum to the same value in S₁ and S₂
- Variable numbers contribute:
  - vᵢ contributes 1 to clause digit if xᵢ appears positively in Cⱼ and vᵢ ∈ S₁
  - v'ᵢ contributes 1 to clause digit if ¬xᵢ appears in Cⱼ and v'ᵢ ∈ S₁
- Slack numbers sⱼ and s'ⱼ can contribute, but:
  - If clause digit in S₁ has contribution ≥ 1 from variable numbers, then at least one literal is TRUE
  - If clause digit in S₁ has contribution 0 from variable numbers, slack must balance, but this means all literals are FALSE, which contradicts the balance requirement

**Key Insight:**
- For clause digit (n+j) to balance:
  - If variable numbers contribute k to S₁, they contribute (total - k) to S₂
  - Slack numbers can adjust, but cannot create satisfaction
  - Therefore, if balance is achieved, at least one literal in each clause must be TRUE

**Conclusion:** The extracted assignment satisfies all clauses, so 3-SAT has a solution.

## Polynomial Time Analysis

**Input Size:**
- 3-SAT: n variables, m clauses
- Partition: 2(n+m) numbers, each with O(n+m) digits

**Construction Time:**
- Create variable numbers: O(n(n+m)) = O(n² + nm)
- Create clause slack numbers: O(m(n+m)) = O(m² + nm)
- Total: O(n² + m² + nm) = O((n+m)²)

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Partition ∈ NP**: Solutions can be verified in polynomial time
2. **3-SAT ≤ₚ Partition**: Polynomial-time reduction exists
3. **Correctness**: 3-SAT satisfiable ↔ Partition has solution

**Therefore, Partition is NP-complete.**

## Key Insights

1. **Base Representation:** Using digits to encode constraints allows arithmetic encoding of logical relationships
2. **Variable Encoding:** Two numbers per variable ensure exactly one assignment
3. **Clause Encoding:** Clause digits track which literals are satisfied
4. **Slack Numbers:** Provide flexibility to achieve target sum while maintaining correctness
5. **Balance Requirement:** The partition requirement (equal sums) enforces that all clauses are satisfied

## Practice Questions

1. **Modify the reduction** to handle 2-SAT. What changes?
2. **Extend the reduction** to handle weighted clauses. How would you encode clause weights?
3. **Analyze the base** used in the reduction. What base is sufficient? Can we use base 2?
4. **Prove the reverse reduction:** Partition ≤ₚ Subset Sum. How does this relate?
5. **Consider the numbers created.** What is the maximum value? Is the reduction strongly polynomial?

---

This reduction demonstrates the power of arithmetic encoding in complexity theory, showing how logical constraints can be represented as numerical relationships, enabling reductions between seemingly different problem domains.

