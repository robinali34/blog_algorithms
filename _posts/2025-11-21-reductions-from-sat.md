---
layout: post
title: "Reductions from SAT: Detailed Proofs"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "Comprehensive detailed proofs showing how to reduce from SAT to prove other problems are NP-complete, with full correctness justifications."
---

## Introduction

SAT (Boolean Satisfiability) is the first problem proven NP-complete by the Cook-Levin Theorem. This post provides detailed proofs following the standard template for reducing from SAT to prove other problems are NP-complete.

## Problem Definition: SAT

**SAT Problem:**
- **Input:** A Boolean formula φ in CNF (Conjunctive Normal Form)
- **Output:** YES if φ is satisfiable, NO otherwise

**Key Properties:**
- First NP-complete problem (Cook-Levin, 1971)
- Foundation for all NP-completeness proofs
- Variables can appear with any number of literals per clause

---

## Q1: How do you reduce SAT to 3-SAT?

**Answer:** Convert each clause with k > 3 literals into multiple 3-literal clauses.

**Key Insight:** 
- Introduce new variables to break long clauses into 3-literal clauses
- New variables can be set to satisfy intermediate clauses
- Original clause satisfiable ↔ at least one new clause satisfiable

**Hint:** For a clause (l₁ ∨ l₂ ∨ ... ∨ lₖ) with k > 3, use chain: (l₁ ∨ l₂ ∨ y₁) ∧ (!y₁ ∨ l₃ ∨ y₂) ∧ ... The new variables yᵢ act as "switches" that can be set to propagate satisfaction.

### 1. NP-Completeness Proof of 3-SAT: Solution Validation

**3-SAT Problem:**
- **Input:** A Boolean formula φ in 3-CNF (exactly 3 literals per clause)
- **Output:** YES if φ is satisfiable, NO otherwise

**3-SAT ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (assignment A):
1. Check that A assigns values to all variables: O(n) time
2. For each clause, check if at least one literal is TRUE: O(m) time

**Total Time:** O(n + m), which is polynomial.

**Conclusion:** 3-SAT ∈ NP.

### 2. Reduce SAT to 3-SAT

#### 2.1 Input Conversion

Given a SAT instance: formula φ in CNF with clauses C₁, C₂, ..., C_m.

**Construction:**

**Case 1: Clause has k > 3 literals**
- For clause (l₁ ∨ l₂ ∨ ... ∨ lₖ) where k > 3:
  - Introduce new variables y₁, y₂, ..., y_{k-3}
  - Replace with: (l₁ ∨ l₂ ∨ y₁) ∧ (!y₁ ∨ l₃ ∨ y₂) ∧ ... ∧ (!y_{k-3} ∨ l_{k-1} ∨ lₖ)

**Case 2: Clause has k = 2 literals**
- For clause (l₁ ∨ l₂):
  - Replace with: (l₁ ∨ l₂ ∨ y) ∧ (l₁ ∨ l₂ ∨ !y) where y is new variable
  - Or simply: (l₁ ∨ l₂ ∨ l₁) (duplicate literal)

**Case 3: Clause has k = 1 literal**
- For clause (l₁):
  - Replace with: (l₁ ∨ l₁ ∨ l₁) (triple the literal)

**Result:** 3-CNF formula φ' with O(m) clauses

#### 2.2 Output Conversion

**Given:** 3-SAT solution (assignment A' for φ')

**Extract SAT Solution:**
- Restrict A' to original variables (ignore new variables yᵢ)
- Return assignment A for original formula φ

### 3. Correctness Justification

#### 3.1 If SAT has a solution, then 3-SAT has a solution

**Given:** SAT instance φ is satisfiable with assignment A.

**Construct 3-SAT Assignment:**
- For original variables: use assignment A
- For new variables yᵢ: set to satisfy intermediate clauses
- For clause (l₁ ∨ l₂ ∨ ... ∨ lₖ) replaced by (l₁ ∨ l₂ ∨ y₁) ∧ ... ∧ (!y_{k-3} ∨ l_{k-1} ∨ lₖ):
  - If any original literal is TRUE, set all yᵢ appropriately
  - If all original literals are FALSE, set y₁ = FALSE, then y₂ = FALSE, etc., until last clause needs y_{k-3} = TRUE

**Conclusion:** 3-SAT has a solution.

#### 3.2a If SAT does not have a solution, then 3-SAT has no solution

**Given:** SAT instance φ is unsatisfiable.

**Proof:**
- If 3-SAT formula φ' is satisfiable, then original formula φ is satisfiable (by construction)
- Contradiction

**Conclusion:** 3-SAT has no solution.

#### 3.2b If 3-SAT has a solution, then SAT has a solution

**Given:** 3-SAT instance φ' has solution A'.

**Extract Assignment:**
- Restrict A' to original variables
- For each original clause C = (l₁ ∨ ... ∨ lₖ):
  - At least one of the new clauses derived from C is satisfied
  - This means at least one original literal is TRUE
- Therefore, original formula φ is satisfied

**Conclusion:** SAT has a solution.

**Polynomial Time:** O(m) to convert clauses.

**Therefore, 3-SAT is NP-complete.**

---

## Q2: How do you reduce SAT to Integer Linear Programming?

**Answer:** Encode Boolean variables as 0-1 ILP variables and clauses as constraints.

**Key Insight:** 
- Map Boolean variables to 0-1 ILP variables
- Encode clause satisfaction as linear inequality: sum of literals ≥ 1
- Use (1 - xᵢ) for negated literals

**Hint:** Each clause becomes a constraint requiring at least one literal to be TRUE. For clause (x₁ ∨ !x₂ ∨ x₃), use constraint: x₁ + (1 - x₂) + x₃ ≥ 1.

### 1. NP-Completeness Proof of ILP: Solution Validation

**ILP Problem:**
- **Input:** Matrix A, vectors b, c, integer k
- **Output:** YES if there exists integer vector x such that Ax ≤ b and c^T x ≥ k, NO otherwise

**ILP ∈ NP:**

**Verification Algorithm:**
Given a candidate solution (integer vector x):
1. Check that x has correct dimension: O(1) time
2. Check that Ax ≤ b: O(mn) time
3. Check that c^T x ≥ k: O(n) time

**Total Time:** O(mn), which is polynomial.

**Conclusion:** ILP ∈ NP.

### 2. Reduce SAT to ILP

#### 2.1 Input Conversion

Given a SAT instance: formula φ in CNF with n variables and m clauses.

**Construction:**
- For each Boolean variable xᵢ, create ILP variable xᵢ ∈ {0,1}
- For each clause Cⱼ = (l₁ ∨ l₂ ∨ ... ∨ lₖ):
  - Create constraint: ∑_{i=1}^k l'ᵢ ≥ 1
  - Where l'ᵢ = xⱼ if lᵢ = xⱼ, and l'ᵢ = (1 - xⱼ) if lᵢ = !xⱼ
- Return ILP instance: variables x ∈ {0,1}ⁿ, constraints as above

**Example:**
- Clause (x₁ ∨ !x₂ ∨ x₃) becomes: x₁ + (1 - x₂) + x₃ ≥ 1
- Simplifies to: x₁ - x₂ + x₃ ≥ 0

#### 2.2 Output Conversion

**Given:** ILP solution (0-1 vector x)

**Extract SAT Assignment:**
- For each variable xᵢ:
  - Set xᵢ = TRUE if xᵢ = 1, else xᵢ = FALSE
- Return assignment A

### 3. Correctness Justification

#### 3.1 If SAT has a solution, then ILP has a solution

**Given:** SAT instance φ is satisfiable with assignment A.

**Construct ILP Solution:**
- For each variable xᵢ:
  - Set xᵢ = 1 if A(xᵢ) = TRUE, else xᵢ = 0
- For each clause constraint:
  - At least one literal is TRUE, so constraint sum ≥ 1
  - Constraint is satisfied

**Conclusion:** ILP has a solution.

#### 3.2a If SAT does not have a solution, then ILP has no solution

**Given:** SAT instance φ is unsatisfiable.

**Proof:**
- If ILP has solution, then extracted assignment satisfies all clauses
- Contradiction

**Conclusion:** ILP has no solution.

#### 3.2b If ILP has a solution, then SAT has a solution

**Given:** ILP instance has solution (0-1 vector x).

**Extract Assignment:**
- Set xᵢ = TRUE if xᵢ = 1, else FALSE
- For each clause:
  - Constraint ensures at least one literal is TRUE
  - Clause is satisfied

**Conclusion:** SAT has a solution.

**Polynomial Time:** O(n + m) to create constraints.

**Therefore, ILP is NP-complete.**

---

## Key Takeaways

1. **Clause Conversion:** SAT → 3-SAT uses new variables
2. **Constraint Encoding:** SAT → ILP maps clauses to constraints
3. **Template Structure:** All reductions follow rigorous format
4. **Foundation:** SAT is the basis for all NP-completeness proofs

---

SAT reductions form the foundation of NP-completeness theory.
