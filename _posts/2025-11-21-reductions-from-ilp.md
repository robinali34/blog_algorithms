---
layout: post
title: "Reductions from Integer Linear Programming: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from Integer Linear Programming to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

Integer Linear Programming (ILP) is a fundamental optimization problem proven NP-complete by reduction from 3-SAT. This post provides answers to common reduction questions when using ILP to prove other problems are NP-complete.

## Problem Definition: Integer Linear Programming

**ILP Problem:**
- **Input:** Matrix A, vectors b, c, integer k
- **Output:** YES if there exists integer vector x such that Ax ≤ b and c^T x ≥ k, NO otherwise

**Key:** Variables must be integers (unlike LP which is polynomial-time).

## Common Reduction Questions

### Q1: How do you reduce ILP to 0-1 ILP?

**Answer:** Use binary expansion of integer variables.

**Reduction:**
- Given ILP instance with variables xᵢ ∈ ℤ, bounds xᵢ ≤ M
- For each xᵢ, create ⌈log₂(M+1)⌉ binary variables
- Represent xᵢ = ∑ⱼ 2ʲ · yᵢⱼ where yᵢⱼ ∈ {0,1}
- Convert constraints using binary representation

**Correctness:**
- ILP feasible ↔ 0-1 ILP feasible
- Binary expansion preserves integer values

**Time:** O(n log M)

### Q2: How do you reduce ILP to Knapsack?

**Answer:** Encode ILP constraints as knapsack constraints.

**Reduction:**
- Given ILP instance: maximize c^T x subject to Ax ≤ b, x ≥ 0, x integer
- Create Knapsack instance:
  - Items: Each variable xᵢ becomes item
  - Weights: Constraint coefficients
  - Values: Objective coefficients
  - Capacity: Constraint bounds

**Correctness:**
- ILP feasible ↔ Knapsack has solution
- Constraints become capacity limits

**Time:** O(nm)

### Q3: How do you reduce ILP to Set Cover?

**Answer:** Encode ILP as covering problem.

**Reduction:**
- Given ILP instance with 0-1 variables
- Universe: All constraints
- Sets: Variable assignments satisfying constraints
- Target: Cover all constraints

**Correctness:**
- ILP feasible ↔ Set Cover exists
- Variables cover constraints

**Time:** O(2ⁿ · m) (exponential, but reduction is valid)

### Q4: How do you reduce ILP to Scheduling?

**Answer:** Encode ILP constraints as scheduling constraints.

**Reduction:**
- Given ILP instance
- Jobs: Variables become jobs
- Resources: Constraints become resource limits
- Time: Objective becomes makespan

**Correctness:**
- ILP feasible ↔ Scheduling feasible
- Constraints become resource/time limits

**Time:** O(nm)

### Q5: How do you reduce ILP to Network Flow?

**Answer:** Special ILP structures map to flow problems.

**Reduction:**
- Given ILP with special structure (e.g., transportation problem)
- Create flow network:
  - Nodes: Variables/constraints
  - Edges: Variable-constraint relationships
  - Capacities: Constraint bounds

**Correctness:**
- ILP feasible ↔ Flow exists
- Flow conservation ↔ Constraint satisfaction

**Time:** O(nm)

## Reduction Patterns from ILP

### Pattern 1: Binary Expansion
- **Use when:** Target problem requires binary variables
- **Examples:** 0-1 ILP
- **Key:** Expand integers in binary

### Pattern 2: Constraint Encoding
- **Use when:** Target problem has similar constraints
- **Examples:** Knapsack, Scheduling
- **Key:** Map constraints directly

### Pattern 3: Special Structures
- **Use when:** ILP has special form
- **Examples:** Network Flow, Transportation
- **Key:** Exploit problem structure

## Key Takeaways

1. **Binary expansion:** Convert to 0-1 ILP
2. **Constraint mapping:** Direct encoding to similar problems
3. **Special structures:** Exploit problem-specific structure
4. **Generalization:** ILP generalizes many problems

## Practice Problems

1. Reduce ILP to Multiple Knapsack
2. Reduce ILP to Resource Allocation
3. Reduce ILP to Assignment Problem

---

ILP reductions often use binary expansion and constraint encoding techniques.

