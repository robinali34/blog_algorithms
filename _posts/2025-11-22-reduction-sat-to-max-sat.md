---
layout: post
title: "Reduction: SAT to Max SAT"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce SAT to Max SAT, demonstrating that Max SAT is NP-complete."
---

## Introduction

This post provides a detailed proof that the Max SAT problem is NP-complete by reducing from SAT. Max SAT asks for a truth assignment that satisfies at least g clauses, generalizing the satisfiability problem.

## Problem Definitions

### Max SAT Problem

**Input:** 
- A CNF formula φ with clauses C₁, C₂, ..., Cₘ
- An integer g ≥ 0

**Output:** YES if there exists a truth assignment that satisfies at least g clauses, NO otherwise.

**Note:** The goal is to find a truth assignment that satisfies at least g clauses (not necessarily all clauses).

### SAT Problem

**Input:** A Boolean formula φ in CNF with:
- Variables: x₁, x₂, ..., xₙ
- Clauses: C₁, C₂, ..., Cₘ

**Output:** YES if φ is satisfiable (all clauses can be satisfied), NO otherwise.

## 1. NP-Completeness Proof of Max SAT: Solution Validation

### Max SAT ∈ NP

**Verification Algorithm:**
Given a candidate solution (truth assignment):
1. For each clause Cⱼ:
   - Evaluate the clause: O(1) time
   - Check if at least one literal is TRUE: O(1) time
2. Count the number of satisfied clauses: O(m) time
3. Check if the count ≥ g: O(1) time

**Total Time:** O(m), which is polynomial in input size.

**Conclusion:** Max SAT ∈ NP.

## 2. Reduce SAT to Max SAT

**Key Insight:** Given a SAT instance, we can reduce it to Max SAT by setting g = m (the number of clauses). Then φ is satisfiable if and only if there's an assignment satisfying at least m clauses (i.e., all clauses).

**Hint:** If we require at least m clauses to be satisfied and there are m clauses total, then all clauses must be satisfied. Therefore, Max SAT with g = m is equivalent to SAT.

### 2.1 Input Conversion

Given a SAT instance φ with n variables and m clauses, we construct a Max SAT instance (φ', g).

**Construction:**

**Step 1: Copy Formula**
- φ' = φ (same formula, no changes)

**Step 2: Set Goal**
- g = m (number of clauses)

**Step 3: Result**
- Max SAT instance: (φ', m)

**Detailed Example:**

Consider SAT instance:
- Variables: x₁, x₂, x₃
- Clauses: C₁ = (x₁ ∨ ¬x₂), C₂ = (¬x₁ ∨ x₃), C₃ = (x₂ ∨ ¬x₃)

**Transformation:**
- φ' = (x₁ ∨ ¬x₂) ∧ (¬x₁ ∨ x₃) ∧ (x₂ ∨ ¬x₃)
- g = 3

**Max SAT instance:** (φ', 3)

**Note:** Satisfying at least 3 clauses means satisfying all 3 clauses, which is equivalent to SAT.

### 2.2 Output Conversion

**Given:** Max SAT solution (truth assignment that satisfies at least g = m clauses)

**Extract SAT Assignment:**
- Use the same truth assignment
- Since g = m and there are m clauses total, the assignment satisfies all m clauses
- Therefore, the assignment satisfies the original SAT instance φ

**Verify Satisfaction:**
- The assignment satisfies all m clauses in φ' = φ
- Therefore, it satisfies the original SAT instance

## 3. Correctness Justification

### 3.1 If SAT has a solution, then Max SAT has a solution

**Given:** SAT instance φ is satisfiable with assignment A.

**Construct Max SAT Assignment:**
- Use the same assignment A
- Since A satisfies all m clauses in φ, it satisfies at least m clauses
- Therefore, A satisfies at least g = m clauses in φ' = φ

**Verify Satisfaction:**
- All m clauses in φ' are satisfied by A
- Number of satisfied clauses = m ≥ g = m ✓
- Therefore, A satisfies the Max SAT instance (φ', m)

**Conclusion:** Max SAT has a solution.

### 3.2a If SAT does not have a solution, then Max SAT has no solution

**Given:** SAT instance φ is unsatisfiable (no assignment satisfies all clauses).

**Proof:**

**Assume:** Max SAT instance (φ', m) has a solution (assignment A that satisfies at least m clauses).

**Extract Assignment:**
- Use assignment A for the original SAT instance

**Check Satisfaction:**
- Since A satisfies at least m clauses and there are m clauses total, A satisfies all m clauses
- Therefore, A satisfies the original SAT instance φ

**Contradiction:**
- A satisfies all clauses in φ
- This contradicts the assumption that φ is unsatisfiable

**Conclusion:** Max SAT has no solution.

### 3.2b If Max SAT has a solution, then SAT has a solution

**Given:** Max SAT instance (φ', m) has solution (assignment A that satisfies at least m clauses).

**Extract Variable Assignment:**
- Use assignment A for the original SAT instance

**Verify Clause Satisfaction:**

**For each clause Cⱼ in φ:**
- Since φ' = φ, clause Cⱼ appears in φ'
- Since A satisfies at least m clauses and there are m clauses total, A satisfies all m clauses
- Therefore, clause Cⱼ is satisfied by A

**Key Insight:**
- Since g = m and there are m clauses total, satisfying at least m clauses means satisfying all m clauses
- Therefore, Max SAT with g = m is equivalent to SAT
- Any solution to Max SAT gives us a solution to SAT

**Conclusion:** The assignment A satisfies all clauses in φ, so SAT has a solution.

## Polynomial Time Analysis

**Input Size:**
- SAT: n variables, m clauses
- Max SAT: n variables, m clauses, integer g = m

**Construction Time:**
- Copy formula: O(m) time (assuming clauses are represented efficiently)
- Set g = m: O(1) time
- Total: O(m) time

**Conclusion:** Reduction is polynomial-time.

## Summary

We have shown:
1. **Max SAT ∈ NP**: Solutions can be verified in polynomial time
2. **SAT ≤ₚ Max SAT**: Polynomial-time reduction exists
3. **Correctness**: SAT satisfiable ↔ Max SAT satisfiable

**Therefore, Max SAT is NP-complete.**

## Key Insights

1. **Goal Setting**: Setting g = m makes the constraint equivalent to satisfying all clauses
2. **Constraint Equivalence**: When g equals the total number of clauses, "at least g" means "all"
3. **Preservation**: The reduction preserves the core satisfiability structure
4. **Generalization**: Max SAT generalizes SAT by allowing partial satisfaction

## Practice Questions

1. **Modify the reduction** to reduce 3-SAT to Max SAT. Does the same approach work?
2. **Analyze the case** when g < m. Can we still reduce SAT to Max SAT in this case?
3. **Consider weighted Max SAT:** How would you reduce weighted SAT to weighted Max SAT?
4. **Prove the reverse reduction:** Can we reduce Max SAT to SAT? How?
5. **Investigate approximation:** How does this reduction relate to approximation algorithms for Max SAT?

---

This reduction demonstrates how partial satisfaction constraints (at least g clauses) can encode complete satisfaction requirements (all clauses), enabling reductions between satisfiability problems.

