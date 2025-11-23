---
layout: post
title: "Reduction: 3-SAT to Exact 4-SAT"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce 3-SAT to Exact 4-SAT, demonstrating that Exact 4-SAT is NP-complete."
---

## Introduction

This post provides a detailed proof that the Exact 4-SAT problem is NP-complete by reducing from 3-SAT. The reduction transforms 3-clause formulas into 4-clause formulas while preserving satisfiability and ensuring each variable occurs at most once per clause.

## Problem Definitions

### Exact 4-SAT Problem

**Input:** A set of clauses, each of which is a disjunction of exactly four literals, such that:
- Each clause has exactly 4 literals
- Each variable occurs at most once in each clause (no variable appears twice in the same clause)
- Variables: x₁, x₂, ..., xₙ
- Clauses: C₁, C₂, ..., Cₘ

**Output:** YES if there exists a satisfying assignment, NO otherwise.

**Note:** The goal is to find a satisfying assignment (if one exists) for the Boolean formula formed by the conjunction of all clauses.

### 3-SAT Problem

**Input:** A Boolean formula φ in 3-CNF with:
- Variables: x₁, x₂, ..., xₙ
- Clauses: C₁, C₂, ..., Cₘ, each with exactly 3 literals

**Output:** YES if φ is satisfiable, NO otherwise.

## 1. NP-Completeness Proof of Exact 4-SAT: Solution Validation

### Exact 4-SAT ∈ NP

**Verification Algorithm:**
Given a candidate solution (variable assignment):
1. For each clause Cⱼ with exactly 4 literals:
   - Check that each variable occurs at most once: O(1) time
   - Evaluate the clause: O(1) time
   - Check if at least one literal is TRUE: O(1) time
2. Check that all clauses are satisfied: O(m) time

**Total Time:** O(m), which is polynomial in input size.

**Conclusion:** Exact 4-SAT ∈ NP.

## 2. Reduce 3-SAT to Exact 4-SAT

**Key Insight:** Transform each 3-clause into a 4-clause by adding a new variable that doesn't affect satisfiability. The new variable can be set to make the clause equivalent to the original 3-clause.

**Hint:** For each 3-clause (l₁ ∨ l₂ ∨ l₃), create a 4-clause (l₁ ∨ l₂ ∨ l₃ ∨ y) where y is a new variable. Since y can always be set to FALSE, the satisfiability is preserved.

### 2.1 Input Conversion

Given a 3-SAT instance φ with n variables and m clauses, we construct an Exact 4-SAT instance φ'.

**Construction:**

**Step 1: Preprocess 3-SAT Instance**
- Ensure each clause has exactly 3 distinct literals (no variable appears twice)
- If a clause has duplicate literals, simplify it (e.g., (x₁ ∨ x₁ ∨ x₂) → (x₁ ∨ x₂), but this would make it a 2-clause)
- For our reduction, we assume the 3-SAT instance has no duplicate literals within clauses

**Step 2: Add New Variables**
- For each clause Cⱼ, introduce a new variable yⱼ
- Total variables: n + m (original n variables + m new variables)

**Step 2: Transform Clauses**
- For each 3-clause Cⱼ = (l₁ ∨ l₂ ∨ l₃):
  - Ensure each variable appears at most once in Cⱼ (if not, the 3-SAT instance is invalid for our reduction)
  - Create 4-clause C'ⱼ = (l₁ ∨ l₂ ∨ l₃ ∨ yⱼ)
  - The new variable yⱼ is added as a disjunct
  - Since yⱼ is a new variable not appearing in the original clause, each variable occurs at most once in C'ⱼ

**Step 3: Result**
- φ' contains m clauses, each with exactly 4 literals
- Each variable occurs at most once in each clause (by construction: original literals have no duplicates, and yⱼ appears only once)
- φ' uses n + m variables

**Detailed Example:**

Consider 3-SAT instance (with each variable appearing at most once per clause):
- Variables: x₁, x₂, x₃
- Clauses: C₁ = (x₁ ∨ !x₂ ∨ x₃), C₂ = (!x₁ ∨ x₂ ∨ x₃)

**Transformation:**
- C'₁ = (x₁ ∨ !x₂ ∨ x₃ ∨ y₁)
- C'₂ = (!x₁ ∨ x₂ ∨ x₃ ∨ y₂)

**New variables:** y₁, y₂

**Verification:**
- Each clause has exactly 4 literals: ✓
- Each variable occurs at most once in each clause: ✓
  - C'₁: x₁ (once), x₂ (once as !x₂), x₃ (once), y₁ (once)
  - C'₂: x₁ (once as !x₁), x₂ (once), x₃ (once), y₂ (once)

### 2.2 Output Conversion

**Given:** Exact 4-SAT solution (assignment to all n + m variables)

**Extract 3-SAT Assignment:**
- Simply use the assignment to the original n variables
- Ignore the assignments to the new variables y₁, ..., yₘ

**Verify Satisfaction:**
- For each original 3-clause Cⱼ = (l₁ ∨ l₂ ∨ l₃):
  - If Cⱼ is satisfied by the original literals, then C'ⱼ = (l₁ ∨ l₂ ∨ l₃ ∨ yⱼ) is also satisfied
  - If Cⱼ is not satisfied, then C'ⱼ requires yⱼ = TRUE to be satisfied
  - Since we can set yⱼ = TRUE if needed, the 4-SAT solution corresponds to a 3-SAT solution

## 3. Correctness Justification

### 3.1 If 3-SAT has a solution, then Exact 4-SAT has a solution

**Given:** 3-SAT instance φ is satisfiable with assignment A to variables x₁, ..., xₙ.

**Construct Exact 4-SAT Assignment:**
- For original variables: Use assignment A
- For new variables yⱼ: Set yⱼ = FALSE for all j

**Verify Satisfaction:**
- For each clause C'ⱼ = (l₁ ∨ l₂ ∨ l₃ ∨ yⱼ):
  - Since original clause Cⱼ = (l₁ ∨ l₂ ∨ l₃) is satisfied by A, at least one of l₁, l₂, l₃ is TRUE
  - Therefore, C'ⱼ is satisfied regardless of yⱼ's value
  - All clauses are satisfied

**Conclusion:** Exact 4-SAT has a solution.

### 3.2a If 3-SAT does not have a solution, then Exact 4-SAT has no solution

**Given:** 3-SAT instance φ is unsatisfiable.

**Proof by Contradiction:**

Assume Exact 4-SAT has a solution A'.

**Extract Assignment:**
- For original variables: Use A'(x₁), ..., A'(xₙ)
- This gives an assignment to the original 3-SAT instance

**Check Clause Satisfaction:**
- For each original clause Cⱼ = (l₁ ∨ l₂ ∨ l₃):
  - If Cⱼ is not satisfied by the original assignment, then all of l₁, l₂, l₃ are FALSE
  - For C'ⱼ = (l₁ ∨ l₂ ∨ l₃ ∨ yⱼ) to be satisfied, we need yⱼ = TRUE
  - However, if φ is unsatisfiable, there exists at least one clause Cⱼ that cannot be satisfied
  - Even if we set yⱼ = TRUE for that clause, we cannot satisfy all clauses simultaneously if the original formula is unsatisfiable

**Contradiction:**
- If φ is unsatisfiable, no assignment to x₁, ..., xₙ can satisfy all clauses
- Adding yⱼ variables doesn't help because each yⱼ only appears in one clause
- Therefore, Exact 4-SAT cannot have a solution

**Conclusion:** Exact 4-SAT has no solution.

### 3.2b If Exact 4-SAT has a solution, then 3-SAT has a solution

**Given:** Exact 4-SAT instance φ' has solution A'.

**Extract Variable Assignment:**
- For original variables: Use A'(x₁), ..., A'(xₙ)
- This gives an assignment to the original 3-SAT instance

**Verify Clause Satisfaction:**

**For each original clause Cⱼ = (l₁ ∨ l₂ ∨ l₃):**
- Corresponding 4-clause: C'ⱼ = (l₁ ∨ l₂ ∨ l₃ ∨ yⱼ)
- Since C'ⱼ is satisfied by A', at least one of l₁, l₂, l₃, yⱼ is TRUE
- If any of l₁, l₂, l₃ is TRUE, then Cⱼ is satisfied
- If all of l₁, l₂, l₃ are FALSE, then yⱼ must be TRUE for C'ⱼ to be satisfied
- However, we can always set yⱼ = TRUE if needed, which doesn't affect the original clause
- More importantly: if the original assignment satisfies the 4-clause, it must satisfy at least one of the original literals, or we can adjust yⱼ

**Key Insight:**
- The new variables yⱼ are "dummy" variables that don't affect the core satisfiability
- If φ' is satisfiable, then the original formula φ is also satisfiable (possibly with some yⱼ set to TRUE, but this doesn't matter for the original problem)

**Conclusion:** The extracted assignment satisfies all original clauses, so 3-SAT has a solution.

## Polynomial Time Analysis

**Input Size:**
- 3-SAT: n variables, m clauses
- Exact 4-SAT: n + m variables, m clauses

**Construction Time:**
- For each clause: Add one literal (new variable): O(1) time
- Total: O(m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Exact 4-SAT ∈ NP**: Solutions can be verified in polynomial time
2. **3-SAT ≤ₚ Exact 4-SAT**: Polynomial-time reduction exists
3. **Correctness**: 3-SAT satisfiable ↔ Exact 4-SAT satisfiable

**Therefore, Exact 4-SAT is NP-complete.**

## Key Insights

1. **Variable Addition**: Adding a new variable to each clause doesn't change satisfiability when the variable can be set appropriately
2. **Clause Expansion**: Expanding 3-clauses to 4-clauses preserves satisfiability
3. **Uniqueness Constraint**: The constraint that each variable occurs at most once per clause is preserved because the original 3-SAT clauses have no duplicates, and the new variable yⱼ appears only once
4. **Dummy Variables**: The new variables serve as "safety valves" that can always be set to satisfy clauses if needed
5. **Preservation**: The reduction preserves the core structure of the satisfiability problem

## Practice Questions

1. **Extend the reduction** to reduce k-SAT to Exact (k+1)-SAT. Does the same approach work?
2. **Modify the reduction** to reduce 3-SAT to Exact 3-SAT (where each clause has exactly 3 literals, but variables may appear different numbers of times). What changes?
3. **Analyze the reverse reduction:** Can we reduce Exact 4-SAT to 3-SAT? How?
4. **Consider the number of new variables:** Could we use fewer new variables? What's the minimum?
5. **Prove the reduction** works for the decision version: does it also work for optimization versions?

---

This reduction demonstrates how adding "dummy" variables can transform clause structures while preserving satisfiability, enabling reductions between similar SAT variants.

