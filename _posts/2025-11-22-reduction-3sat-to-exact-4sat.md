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

**Input:** A Boolean formula φ in CNF with:
- Variables: x₁, x₂, ..., xₙ
- Clauses: C₁, C₂, ..., Cₘ, each with at most 3 literals

**Output:** YES if φ is satisfiable, NO otherwise.

**Note:** Each clause has at most 3 literals (can have 1, 2, or 3 literals).

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

**Key Insight:** Transform each clause (with at most 3 literals) into multiple 4-clauses by adding new variables in all possible combinations. This ensures that the original clause is satisfied if and only if all the new 4-clauses are satisfied.

**Hint:** 
- For a 3-literal clause (l₁ ∨ l₂ ∨ l₃), create two 4-clauses: (l₁ ∨ l₂ ∨ l₃ ∨ w) and (l₁ ∨ l₂ ∨ l₃ ∨ !w). Both must be satisfied, which is equivalent to (l₁ ∨ l₂ ∨ l₃) being satisfied.
- For a 2-literal clause (l₁ ∨ l₂), create four 4-clauses covering all combinations of two new variables.
- For a 1-literal clause (l₁), create eight 4-clauses covering all combinations of three new variables.

### 2.1 Input Conversion

Given a 3-SAT instance φ with n variables and m clauses, we construct an Exact 4-SAT instance φ'.

**Construction:**

**Step 1: Preprocess 3-SAT Instance**
- Ensure each clause has at most 3 distinct literals (no variable appears twice within a clause)
- If a clause has duplicate literals, simplify it (e.g., (x₁ ∨ x₁ ∨ x₂) → (x₁ ∨ x₂))
- For our reduction, we assume the 3-SAT instance has no duplicate literals within clauses

**Step 2: Transform Clauses Based on Size**

For each clause Cⱼ, transform it based on the number of literals:

**Case 1: Clause has 3 literals** Cⱼ = (l₁ ∨ l₂ ∨ l₃)
- Add 1 new variable wⱼ
- Create 2 clauses:
  - C'ⱼ₁ = (l₁ ∨ l₂ ∨ l₃ ∨ wⱼ)
  - C'ⱼ₂ = (l₁ ∨ l₂ ∨ l₃ ∨ !wⱼ)
- Both clauses must be satisfied, which is equivalent to (l₁ ∨ l₂ ∨ l₃) being satisfied

**Case 2: Clause has 2 literals** Cⱼ = (l₁ ∨ l₂)
- Add 2 new variables wⱼ₁ and wⱼ₂
- Create 4 clauses (all combinations of wⱼ₁ and wⱼ₂):
  - C'ⱼ₁ = (l₁ ∨ l₂ ∨ wⱼ₁ ∨ wⱼ₂)
  - C'ⱼ₂ = (l₁ ∨ l₂ ∨ wⱼ₁ ∨ !wⱼ₂)
  - C'ⱼ₃ = (l₁ ∨ l₂ ∨ !wⱼ₁ ∨ wⱼ₂)
  - C'ⱼ₄ = (l₁ ∨ l₂ ∨ !wⱼ₁ ∨ !wⱼ₂)
- All 4 clauses must be satisfied, which is equivalent to (l₁ ∨ l₂) being satisfied

**Case 3: Clause has 1 literal** Cⱼ = (l₁)
- Add 3 new variables wⱼ₁, wⱼ₂, and wⱼ₃
- Create 8 clauses (all combinations of wⱼ₁, wⱼ₂, wⱼ₃):
  - C'ⱼ₁ = (l₁ ∨ wⱼ₁ ∨ wⱼ₂ ∨ wⱼ₃)
  - C'ⱼ₂ = (l₁ ∨ wⱼ₁ ∨ wⱼ₂ ∨ !wⱼ₃)
  - C'ⱼ₃ = (l₁ ∨ wⱼ₁ ∨ !wⱼ₂ ∨ wⱼ₃)
  - C'ⱼ₄ = (l₁ ∨ wⱼ₁ ∨ !wⱼ₂ ∨ !wⱼ₃)
  - C'ⱼ₅ = (l₁ ∨ !wⱼ₁ ∨ wⱼ₂ ∨ wⱼ₃)
  - C'ⱼ₆ = (l₁ ∨ !wⱼ₁ ∨ wⱼ₂ ∨ !wⱼ₃)
  - C'ⱼ₇ = (l₁ ∨ !wⱼ₁ ∨ !wⱼ₂ ∨ wⱼ₃)
  - C'ⱼ₈ = (l₁ ∨ !wⱼ₁ ∨ !wⱼ₂ ∨ !wⱼ₃)
- All 8 clauses must be satisfied, which is equivalent to (l₁) being satisfied

**Note:** In each case, the new variables are unique to that clause and don't appear in other clauses. This ensures each variable occurs at most once in each clause.

**Step 3: Result**
- φ' contains clauses, each with exactly 4 literals
- Each variable occurs at most once in each clause (by construction: original literals have no duplicates, and new variables are unique per clause)
- Total new variables: depends on clause sizes (1 variable per 3-literal clause, 2 per 2-literal clause, 3 per 1-literal clause)

**Detailed Example:**

Consider 3-SAT instance (with each variable appearing at most once per clause):
- Variables: x₁, x₂, x₃
- Clauses: 
  - C₁ = (x₁ ∨ !x₂ ∨ x₃)  (3 literals)
  - C₂ = (!x₁ ∨ x₂)  (2 literals)
  - C₃ = (x₃)  (1 literal)

**Transformation:**

**For C₁ (3 literals):**
- Add variable w₁
- C'₁₁ = (x₁ ∨ !x₂ ∨ x₃ ∨ w₁)
- C'₁₂ = (x₁ ∨ !x₂ ∨ x₃ ∨ !w₁)

**For C₂ (2 literals):**
- Add variables w₂₁ and w₂₂
- C'₂₁ = (!x₁ ∨ x₂ ∨ w₂₁ ∨ w₂₂)
- C'₂₂ = (!x₁ ∨ x₂ ∨ w₂₁ ∨ !w₂₂)
- C'₂₃ = (!x₁ ∨ x₂ ∨ !w₂₁ ∨ w₂₂)
- C'₂₄ = (!x₁ ∨ x₂ ∨ !w₂₁ ∨ !w₂₂)

**For C₃ (1 literal):**
- Add variables w₃₁, w₃₂, w₃₃
- C'₃₁ = (x₃ ∨ w₃₁ ∨ w₃₂ ∨ w₃₃)
- C'₃₂ = (x₃ ∨ w₃₁ ∨ w₃₂ ∨ !w₃₃)
- C'₃₃ = (x₃ ∨ w₃₁ ∨ !w₃₂ ∨ w₃₃)
- C'₃₄ = (x₃ ∨ w₃₁ ∨ !w₃₂ ∨ !w₃₃)
- C'₃₅ = (x₃ ∨ !w₃₁ ∨ w₃₂ ∨ w₃₃)
- C'₃₆ = (x₃ ∨ !w₃₁ ∨ w₃₂ ∨ !w₃₃)
- C'₃₇ = (x₃ ∨ !w₃₁ ∨ !w₃₂ ∨ w₃₃)
- C'₃₈ = (x₃ ∨ !w₃₁ ∨ !w₃₂ ∨ !w₃₃)

**New variables:** w₁, w₂₁, w₂₂, w₃₁, w₃₂, w₃₃
**Total clauses:** 2 + 4 + 8 = 14 clauses, all with exactly 4 literals

**Verification:**
- Each clause has exactly 4 literals: ✓
- Each variable occurs at most once in each clause: ✓
  - Original variables appear at most once per clause (by assumption)
  - New variables (w₁, w₂₁, w₂₂, w₃₁, w₃₂, w₃₃) are unique to their clause groups and appear at most once per clause

### 2.2 Output Conversion

**Given:** Exact 4-SAT solution (assignment to all variables including new ones)

**Extract 3-SAT Assignment:**
- Simply use the assignment to the original n variables (x₁, ..., xₙ)
- Ignore the assignments to the new variables (wⱼ, wⱼ₁, wⱼ₂, wⱼ₃, etc.)

**Verify Satisfaction:**
- For each original clause Cⱼ:
  - **Case 1 (3 literals):** If Cⱼ = (l₁ ∨ l₂ ∨ l₃) is satisfied, then both C'ⱼ₁ = (l₁ ∨ l₂ ∨ l₃ ∨ wⱼ) and C'ⱼ₂ = (l₁ ∨ l₂ ∨ l₃ ∨ !wⱼ) are satisfied. If Cⱼ is not satisfied, then both new clauses require wⱼ to be both TRUE and FALSE, which is impossible. Therefore, Cⱼ must be satisfied.
  - **Case 2 (2 literals):** If Cⱼ = (l₁ ∨ l₂) is satisfied, then all four new clauses are satisfied. If Cⱼ is not satisfied, then all four clauses require specific combinations of wⱼ₁ and wⱼ₂ that cannot all be satisfied simultaneously. Therefore, Cⱼ must be satisfied.
  - **Case 3 (1 literal):** If Cⱼ = (l₁) is satisfied, then all eight new clauses are satisfied. If Cⱼ is not satisfied, then all eight clauses require specific combinations of wⱼ₁, wⱼ₂, wⱼ₃ that cannot all be satisfied simultaneously. Therefore, Cⱼ must be satisfied.
- Since all original clauses are satisfied, the 3-SAT instance has a solution

## 3. Correctness Justification

### 3.1 If 3-SAT has a solution, then Exact 4-SAT has a solution

**Given:** 3-SAT instance φ is satisfiable with assignment A to variables x₁, ..., xₙ.

**Construct Exact 4-SAT Assignment:**
- For original variables: Use assignment A
- For new variables: Set them arbitrarily (e.g., all FALSE)

**Verify Satisfaction:**
- For each original clause Cⱼ:
  - **Case 1 (3 literals):** Since Cⱼ = (l₁ ∨ l₂ ∨ l₃) is satisfied by A, at least one of l₁, l₂, l₃ is TRUE. Therefore, both C'ⱼ₁ = (l₁ ∨ l₂ ∨ l₃ ∨ wⱼ) and C'ⱼ₂ = (l₁ ∨ l₂ ∨ l₃ ∨ !wⱼ) are satisfied regardless of wⱼ's value.
  - **Case 2 (2 literals):** Since Cⱼ = (l₁ ∨ l₂) is satisfied by A, at least one of l₁, l₂ is TRUE. Therefore, all four new clauses are satisfied regardless of wⱼ₁ and wⱼ₂'s values.
  - **Case 3 (1 literal):** Since Cⱼ = (l₁) is satisfied by A, l₁ is TRUE. Therefore, all eight new clauses are satisfied regardless of wⱼ₁, wⱼ₂, wⱼ₃'s values.
- All clauses in φ' are satisfied

**Conclusion:** Exact 4-SAT has a solution.

### 3.2a If 3-SAT does not have a solution, then Exact 4-SAT has no solution

**Given:** 3-SAT instance φ is unsatisfiable.

**Proof by Contradiction:**

Assume Exact 4-SAT has a solution A'.

**Extract Assignment:**
- For original variables: Use A'(x₁), ..., A'(xₙ)
- This gives an assignment to the original 3-SAT instance

**Check Clause Satisfaction:**
- For each original clause Cⱼ:
  - **Case 1 (3 literals):** If Cⱼ = (l₁ ∨ l₂ ∨ l₃) is not satisfied, then all of l₁, l₂, l₃ are FALSE. For both C'ⱼ₁ = (l₁ ∨ l₂ ∨ l₃ ∨ wⱼ) and C'ⱼ₂ = (l₁ ∨ l₂ ∨ l₃ ∨ !wⱼ) to be satisfied, we need wⱼ = TRUE and wⱼ = FALSE simultaneously, which is impossible.
  - **Case 2 (2 literals):** If Cⱼ = (l₁ ∨ l₂) is not satisfied, then both l₁ and l₂ are FALSE. For all four new clauses to be satisfied, we need specific combinations of wⱼ₁ and wⱼ₂ that cannot all be satisfied simultaneously.
  - **Case 3 (1 literal):** If Cⱼ = (l₁) is not satisfied, then l₁ is FALSE. For all eight new clauses to be satisfied, we need specific combinations of wⱼ₁, wⱼ₂, wⱼ₃ that cannot all be satisfied simultaneously.
- If φ is unsatisfiable, there exists at least one clause Cⱼ that cannot be satisfied, leading to a contradiction
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

