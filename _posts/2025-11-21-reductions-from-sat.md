---
layout: post
title: "Reductions from SAT: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from SAT to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

SAT (Boolean Satisfiability) is the first problem proven NP-complete by the Cook-Levin Theorem. This post provides a reference for common reduction questions when using SAT as the starting point to prove other problems are NP-complete.

## Problem Definition: SAT

**SAT Problem:**
- **Input:** A Boolean formula φ in CNF (Conjunctive Normal Form)
- **Output:** YES if φ is satisfiable, NO otherwise

**Key Properties:**
- First NP-complete problem (Cook-Levin, 1971)
- Foundation for all NP-completeness proofs
- Variables can appear with any number of literals per clause

## Common Reduction Questions

### Q1: How do you reduce SAT to 3-SAT?

**Answer:** Convert each clause with k > 3 literals into multiple 3-literal clauses.

**Reduction:**
- For clause (l₁ ∨ l₂ ∨ ... ∨ lₖ) where k > 3:
  - Introduce new variables y₁, y₂, ..., y_{k-3}
  - Replace with: (l₁ ∨ l₂ ∨ y₁) ∧ (¬y₁ ∨ l₃ ∨ y₂) ∧ ... ∧ (¬y_{k-3} ∨ l_{k-1} ∨ lₖ)
- For clause with k < 3 literals:
  - Add dummy variables or duplicate literals

**Correctness:**
- Original clause satisfiable ↔ at least one new clause satisfiable
- New variables can be set to satisfy intermediate clauses

**Time:** O(m) where m is number of clauses

### Q2: How do you reduce SAT to Integer Linear Programming?

**Answer:** Encode Boolean variables as 0-1 ILP variables and clauses as constraints.

**Reduction:**
- For each Boolean variable xᵢ, create ILP variable xᵢ ∈ {0,1}
- For clause (l₁ ∨ l₂ ∨ ... ∨ lₖ):
  - If literal is xᵢ, use xᵢ
  - If literal is ¬xᵢ, use (1 - xᵢ)
  - Constraint: sum of literals ≥ 1

**Example:**
- Clause (x₁ ∨ ¬x₂ ∨ x₃) becomes: x₁ + (1 - x₂) + x₃ ≥ 1
- Simplifies to: x₁ - x₂ + x₃ ≥ 0

**Correctness:**
- SAT satisfiable ↔ ILP feasible
- Each clause requires at least one literal TRUE

**Time:** O(n + m)

### Q3: How do you reduce SAT to Zero-One Equations?

**Answer:** Convert SAT to system of linear equations over GF(2).

**Reduction:**
- For each variable xᵢ, create variable xᵢ ∈ {0,1}
- For clause (l₁ ∨ l₂ ∨ ... ∨ lₖ):
  - Create equation: sum of literals = 1 (mod 2)
  - Use xᵢ for positive literal, (1 - xᵢ) for negative

**Correctness:**
- SAT satisfiable ↔ ZOE has solution
- Each clause must have exactly one TRUE literal (mod 2)

**Time:** O(nm)

### Q4: How do you reduce SAT to Set Cover?

**Answer:** Encode variables and clauses as sets.

**Reduction:**
- Universe U: All clauses
- For each variable xᵢ:
  - Create set Sᵢ containing clauses where xᵢ appears positively
  - Create set S'ᵢ containing clauses where ¬xᵢ appears
- Target: Cover all clauses with minimum sets

**Correctness:**
- SAT satisfiable ↔ Set Cover of size n exists
- Each clause covered by at least one set (satisfied literal)

**Time:** O(nm)

### Q5: How do you reduce SAT to Graph Coloring?

**Answer:** Create graph where vertices represent literals and edges enforce constraints.

**Reduction:**
- For each variable xᵢ, create vertices vᵢ (xᵢ) and v'ᵢ (¬xᵢ)
- Connect vᵢ and v'ᵢ (can't both be TRUE)
- For each clause, connect all its literal vertices (at least one must be TRUE)
- Use 3-coloring to encode satisfaction

**Correctness:**
- SAT satisfiable ↔ Graph is 3-colorable
- Colors represent TRUE/FALSE/UNUSED

**Time:** O(n + m)

## Reduction Patterns from SAT

### Pattern 1: Direct Variable Encoding
- **Use when:** Target problem has natural Boolean structure
- **Examples:** ILP, ZOE, Set Cover
- **Key:** Map variables directly to problem elements

### Pattern 2: Clause Gadgets
- **Use when:** Target problem has selection/covering structure
- **Examples:** Set Cover, Hitting Set
- **Key:** Encode clauses as elements to cover

### Pattern 3: Constraint Encoding
- **Use when:** Target problem has constraint satisfaction
- **Examples:** Graph Coloring, Constraint Satisfaction
- **Key:** Encode clauses as constraints

## Key Takeaways

1. **SAT is universal:** Can reduce to most NP-complete problems
2. **Variable encoding:** Map Boolean variables to problem variables
3. **Clause encoding:** Ensure at least one literal per clause is TRUE
4. **Polynomial time:** Most reductions are straightforward and efficient
5. **Foundation:** SAT reductions form basis for other reductions

## Practice Problems

1. Reduce SAT to Hamiltonian Cycle
2. Reduce SAT to Traveling Salesman Problem
3. Reduce SAT to Subset Sum
4. Reduce SAT to Partition
5. Reduce SAT to 3D Matching

---

SAT serves as the foundation for NP-completeness proofs, and understanding reductions from SAT is essential for proving new problems are NP-complete.

