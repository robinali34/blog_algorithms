---
layout: post
title: "Reduction: SAT to Stingy SAT"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce SAT to Stingy SAT, demonstrating that Stingy SAT is NP-complete."
---

## Introduction

This post provides a detailed proof that the Stingy SAT problem is NP-complete by reducing from SAT. Stingy SAT requires finding a satisfying assignment in which at most k variables are true, adding a constraint on the number of true variables.

## Problem Definitions

### Stingy SAT Problem

**Input:** 
- A set of clauses (each a disjunction of literals) forming a Boolean formula φ
- Variables: x₁, x₂, ..., xₙ
- An integer k ≥ 0

**Output:** YES if there exists a satisfying assignment in which at most k variables are true, NO otherwise.

**Note:** The problem asks to find a satisfying assignment (if such an assignment exists) where at most k variables are set to TRUE.

**Example:**

Consider a Boolean formula φ with:
- Variables: x₁, x₂, x₃
- Clauses:
  - C₁ = (x₁ ∨ ¬x₂)  (read as: "x₁ OR NOT x₂")
  - C₂ = (¬x₁ ∨ x₃)  (read as: "NOT x₁ OR x₃")
  - C₃ = (x₂ ∨ ¬x₃)  (read as: "x₂ OR NOT x₃")

The formula φ = C₁ ∧ C₂ ∧ C₃ = (x₁ ∨ ¬x₂) ∧ (¬x₁ ∨ x₃) ∧ (x₂ ∨ ¬x₃)

**What is a clause?**
- A clause is a disjunction (OR) of literals
- A literal is either a variable (xᵢ) or its negation (¬xᵢ)
- Example: (x₁ ∨ ¬x₂) means "x₁ is TRUE OR x₂ is FALSE"

**What does it mean for a formula to be satisfiable?**
- A formula is satisfiable if there exists an assignment of TRUE/FALSE to variables that makes the entire formula TRUE
- For a clause to be TRUE, at least one of its literals must be TRUE
- For the formula to be TRUE, all clauses must be TRUE

**Stingy SAT Example:**

Given the formula above and k = 2:

**Trying assignment: x₁ = TRUE, x₂ = FALSE, x₃ = TRUE**
- C₁ = (TRUE ∨ ¬FALSE) = (TRUE ∨ TRUE) = TRUE ✓
- C₂ = (¬TRUE ∨ TRUE) = (FALSE ∨ TRUE) = TRUE ✓
- C₃ = (FALSE ∨ ¬TRUE) = (FALSE ∨ FALSE) = FALSE ✗
- Formula is FALSE (not satisfiable with this assignment)

**Trying assignment: x₁ = TRUE, x₂ = FALSE, x₃ = FALSE**
- C₁ = (TRUE ∨ ¬FALSE) = (TRUE ∨ TRUE) = TRUE ✓
- C₂ = (¬TRUE ∨ FALSE) = (FALSE ∨ FALSE) = FALSE ✗
- Formula is FALSE

**Trying assignment: x₁ = TRUE, x₂ = TRUE, x₃ = TRUE**
- C₁ = (TRUE ∨ ¬TRUE) = (TRUE ∨ FALSE) = TRUE ✓
- C₂ = (¬TRUE ∨ TRUE) = (FALSE ∨ TRUE) = TRUE ✓
- C₃ = (TRUE ∨ ¬TRUE) = (TRUE ∨ FALSE) = TRUE ✓
- Formula is TRUE ✓
- Number of TRUE variables: 3
- Since k = 2 and we have 3 TRUE variables, this assignment does NOT satisfy the "at most k variables true" constraint

**Trying assignment: x₁ = FALSE, x₂ = TRUE, x₃ = TRUE**
- C₁ = (FALSE ∨ ¬TRUE) = (FALSE ∨ FALSE) = FALSE ✗
- Formula is FALSE

**Trying assignment: x₁ = TRUE, x₂ = FALSE, x₃ = TRUE** (already tried, FALSE)

**Trying assignment: x₁ = FALSE, x₂ = TRUE, x₃ = FALSE**
- C₁ = (FALSE ∨ ¬TRUE) = (FALSE ∨ FALSE) = FALSE ✗
- Formula is FALSE

**Trying assignment: x₁ = FALSE, x₂ = FALSE, x₃ = TRUE**
- C₁ = (FALSE ∨ ¬FALSE) = (FALSE ∨ TRUE) = TRUE ✓
- C₂ = (¬FALSE ∨ TRUE) = (TRUE ∨ TRUE) = TRUE ✓
- C₃ = (FALSE ∨ ¬TRUE) = (FALSE ∨ FALSE) = FALSE ✗
- Formula is FALSE

**Trying assignment: x₁ = FALSE, x₂ = FALSE, x₃ = FALSE**
- C₁ = (FALSE ∨ ¬FALSE) = (FALSE ∨ TRUE) = TRUE ✓
- C₂ = (¬FALSE ∨ FALSE) = (TRUE ∨ FALSE) = TRUE ✓
- C₃ = (FALSE ∨ ¬FALSE) = (FALSE ∨ TRUE) = TRUE ✓
- Formula is TRUE ✓
- Number of TRUE variables: 0
- Since k = 2 and we have 0 TRUE variables (0 ≤ 2), this assignment satisfies the "at most k variables true" constraint ✓

**Answer:** YES, the Stingy SAT instance (φ, k=2) has a solution: x₁ = FALSE, x₂ = FALSE, x₃ = FALSE (0 variables are TRUE, which is ≤ 2).

### SAT Problem

**Input:** A Boolean formula φ in CNF with:
- Variables: x₁, x₂, ..., xₙ
- Clauses: C₁, C₂, ..., Cₘ

**Output:** YES if φ is satisfiable, NO otherwise.

## 1. NP-Completeness Proof of Stingy SAT: Solution Validation

### Stingy SAT ∈ NP

**Verification Algorithm:**
Given a candidate solution (variable assignment):
1. Check that at most k variables are true: O(n) time
2. For each clause Cⱼ:
   - Evaluate the clause: O(1) time
   - Check if at least one literal is TRUE: O(1) time
3. Check that all clauses are satisfied: O(m) time

**Total Time:** O(n + m), which is polynomial in input size.

**Conclusion:** Stingy SAT ∈ NP.

## 2. Reduce SAT to Stingy SAT

**Key Insight:** Given a SAT instance, we can reduce it to Stingy SAT by setting k = n (the number of variables). This allows any assignment, making Stingy SAT equivalent to SAT.

**Hint:** The constraint "at most k variables are true" is always satisfied when k = n, since any assignment sets at most n variables to TRUE. Therefore, Stingy SAT with k = n is equivalent to SAT.

### 2.1 Input Conversion

Given a SAT instance φ with n variables and m clauses, we construct a Stingy SAT instance (φ', k).

**Construction:**

**Step 1: Copy Formula**
- φ' = φ (same formula, no changes)

**Step 2: Set Parameter**
- k = n (number of variables)

**Step 3: Result**
- Stingy SAT instance: (φ', n)

**Detailed Example:**

Consider SAT instance:
- Variables: x₁, x₂, x₃
- Clauses: C₁ = (x₁ ∨ ¬x₂), C₂ = (¬x₁ ∨ x₃), C₃ = (x₂ ∨ ¬x₃)

**Transformation:**
- φ' = (x₁ ∨ ¬x₂) ∧ (¬x₁ ∨ x₃) ∧ (x₂ ∨ ¬x₃)
- k = 3

**Stingy SAT instance:** (φ', 3)

### 2.2 Output Conversion

**Given:** Stingy SAT solution (assignment that satisfies φ' with at most k variables true)

**Extract SAT Assignment:**
- Use the same assignment
- Since k = n, any assignment satisfies the "at most k variables are true" constraint
- If the assignment satisfies φ', it also satisfies φ (since φ' = φ)

**Verify Satisfaction:**
- The assignment satisfies all clauses in φ' = φ
- Therefore, it satisfies the original SAT instance

## 3. Correctness Justification

### 3.1 If SAT has a solution, then Stingy SAT has a solution

**Given:** SAT instance φ is satisfiable with assignment A.

**Construct Stingy SAT Assignment:**
- Use the same assignment A
- Since k = n, and any assignment sets at most n variables to TRUE, the constraint "at most k variables are true" is satisfied
- Since A satisfies φ, it also satisfies φ' = φ

**Verify Satisfaction:**
- All clauses in φ' are satisfied by A
- At most n variables are true (always true for any assignment)
- Therefore, A satisfies the Stingy SAT instance (φ', n)

**Conclusion:** Stingy SAT has a solution.

### 3.2a If SAT does not have a solution, then Stingy SAT has no solution

**Given:** SAT instance φ is unsatisfiable.

**Proof:**

**Assume:** Stingy SAT instance (φ', n) has a solution A.

**Extract Assignment:**
- Use assignment A for the original SAT instance

**Check Satisfaction:**
- Since A satisfies φ' = φ, it would satisfy the original SAT instance
- This contradicts the assumption that φ is unsatisfiable

**Conclusion:** Stingy SAT has no solution.

### 3.2b If Stingy SAT has a solution, then SAT has a solution

**Given:** Stingy SAT instance (φ', n) has solution A.

**Extract Variable Assignment:**
- Use assignment A for the original SAT instance

**Verify Clause Satisfaction:**

**For each clause Cⱼ in φ:**
- Since φ' = φ, clause Cⱼ appears in φ'
- Since A satisfies φ', clause Cⱼ is satisfied by A
- Therefore, A satisfies all clauses in φ

**Key Insight:**
- Since k = n, the constraint "at most n variables are true" is always satisfied
- Therefore, Stingy SAT with k = n is equivalent to SAT
- Any satisfying assignment for Stingy SAT is also a satisfying assignment for SAT

**Conclusion:** The assignment A satisfies all clauses in φ, so SAT has a solution.

## Polynomial Time Analysis

**Input Size:**
- SAT: n variables, m clauses
- Stingy SAT: n variables, m clauses, integer k = n

**Construction Time:**
- Copy formula: O(m) time (assuming clauses are represented efficiently)
- Set k = n: O(1) time
- Total: O(m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Stingy SAT ∈ NP**: Solutions can be verified in polynomial time
2. **SAT ≤ₚ Stingy SAT**: Polynomial-time reduction exists
3. **Correctness**: SAT satisfiable ↔ Stingy SAT satisfiable

**Therefore, Stingy SAT is NP-complete.**

## Key Insights

1. **Parameter Setting**: Setting k = n makes the constraint trivial, reducing Stingy SAT to SAT
2. **Constraint Relaxation**: When k is large enough, Stingy SAT becomes equivalent to SAT
3. **Preservation**: The reduction preserves the core satisfiability structure
4. **Trivial Constraint**: Any assignment sets at most n variables to TRUE, so k = n imposes no real restriction on the number of true variables

## Practice Questions

1. **Modify the reduction** to reduce 3-SAT to Stingy SAT. Does the same approach work?
2. **Analyze the case** when k < n. Can we still reduce SAT to Stingy SAT in this case?
3. **Consider optimization versions:** How would you reduce Max-SAT to Stingy SAT?
4. **Prove the reverse reduction:** Can we reduce Stingy SAT to SAT? How?
5. **Investigate the parameterized complexity:** For what values of k is Stingy SAT polynomial-time solvable?

---

This reduction demonstrates how adding a constraint that is always satisfied (when k = n) can transform a problem while preserving its complexity, enabling reductions between SAT variants.

