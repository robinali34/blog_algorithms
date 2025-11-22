---
layout: post
title: "NP-Hard Introduction: Integer Linear Programming (ILP)"
date: 2025-11-16
categories: [Algorithms, Complexity Theory, NP-Hard, Optimization]
excerpt: "An introduction to NP-hardness through Integer Linear Programming (ILP), covering problem definition, NP-completeness proof, relationship to Linear Programming, and practical solving methods."
---

## Introduction

Integer Linear Programming (ILP) is a fundamental optimization problem that extends Linear Programming by requiring variables to take integer values. While Linear Programming can be solved in polynomial time, ILP is NP-complete, making it one of the most important problems in optimization theory. ILP has widespread applications in operations research, scheduling, resource allocation, and many other domains.

## What is Integer Linear Programming?

Integer Linear Programming asks: **Given linear constraints and a linear objective function, find integer values for variables that satisfy the constraints and optimize the objective.**

### Problem Definition

**Integer Linear Programming (ILP) Decision Problem:**

**Input:** 
- A matrix A ∈ ℤ^(m×n) (constraint coefficients)
- A vector b ∈ ℤ^m (constraint bounds)
- A vector c ∈ ℤ^n (objective coefficients)
- An integer k (target value)

**Output:** YES if there exists an integer vector x ∈ ℤ^n such that:
- Ax ≤ b (constraints satisfied)
- c^T x ≥ k (objective value at least k)

NO otherwise

**ILP Optimization Problem:**

**Input:** Same as above (without k)

**Output:** The maximum value of c^T x subject to Ax ≤ b and x ∈ ℤ^n

### Variants

**0-1 Integer Programming (Binary ILP):**
- Variables restricted to {0, 1}
- Very common in practice

**Mixed Integer Linear Programming (MILP):**
- Some variables are integers, others are continuous
- Combines ILP and LP

**Unbounded ILP:**
- Variables can be any integers (not necessarily non-negative)

### Example

Consider the ILP:

**Maximize:** 3x₁ + 2x₂

**Subject to:**
- 2x₁ + x₂ ≤ 6
- x₁ + 2x₂ ≤ 8
- x₁, x₂ ≥ 0 and integer

**Feasible integer solutions:**
- (0, 0): objective = 0
- (1, 0): objective = 3
- (2, 0): objective = 6
- (0, 1): objective = 2
- (1, 1): objective = 5
- (2, 1): objective = 8
- (3, 0): objective = 9 ✓ (optimal, satisfies constraints)

**LP relaxation** (allowing fractional values) might give (2.67, 0.67) with objective 9.33, but this is not integer.

## Why ILP is in NP

To show that ILP is NP-complete, we first need to show it's in NP.

**ILP ∈ NP:**

Given a candidate solution (an integer vector x), we can verify in polynomial time:
1. Check that x has integer values: O(n) time
2. Check that Ax ≤ b: O(mn) time (matrix-vector multiplication)
3. Check that c^T x ≥ k: O(n) time

Total verification time: O(mn), which is polynomial in the input size. Therefore, ILP is in NP.

**Note:** The size of the solution x might be exponential in the input size (if values are large), but we can verify constraints in polynomial time relative to the input size.

## NP-Completeness: Reduction from 3-SAT

The standard proof that ILP is NP-complete reduces from 3-SAT.

### Construction

For a 3-SAT formula φ = C₁ ∧ C₂ ∧ … ∧ Cₘ with variables x₁, x₂, …, xₙ:

**Key Idea:** Encode Boolean variables as 0-1 integer variables and clauses as linear constraints.

1. **Variables:**
   - For each Boolean variable xᵢ, create an integer variable yᵢ ∈ {0, 1}
   - yᵢ = 1 means xᵢ = TRUE, yᵢ = 0 means xᵢ = FALSE

2. **Clauses:**
   - For each clause Cⱼ = (l₁ ∨ l₂ ∨ l₃):
     - If literal is xᵢ, use yᵢ
     - If literal is ¬xᵢ, use (1 - yᵢ)
     - Constraint: y_{l₁} + y_{l₂} + y_{l₃} ≥ 1 (at least one literal is true)
   
   Example: For clause (x₁ ∨ ¬x₂ ∨ x₃):
   - Constraint: y₁ + (1 - y₂) + y₃ ≥ 1
   - Simplifies to: y₁ - y₂ + y₃ ≥ 0

3. **Objective:** Maximize ∑ᵢ₌₁ⁿ yᵢ (or any objective, since we're just checking feasibility)

4. **Bounds:** 0 ≤ yᵢ ≤ 1 for all i (enforces binary variables)

### Why This Works

**Forward Direction (3-SAT satisfiable → ILP feasible):**
- If φ is satisfiable, set yᵢ = 1 if xᵢ = TRUE, else yᵢ = 0
- Each clause constraint is satisfied (at least one literal is 1)
- This gives a feasible ILP solution

**Reverse Direction (ILP feasible → 3-SAT satisfiable):**
- If ILP has feasible solution with yᵢ ∈ {0,1}, set xᵢ = TRUE if yᵢ = 1, else xᵢ = FALSE
- Since each clause constraint requires at least one yᵢ = 1 (or (1-yᵢ) = 1), each clause has at least one true literal
- This gives a satisfying assignment

**Polynomial Time:**
- Construction takes O(mn) time (one constraint per clause)

Therefore, **ILP is NP-complete**.

## Relationship to Linear Programming

### Linear Programming (LP)

**LP Decision Problem:**
- Same as ILP but variables can be **real numbers** (not necessarily integers)
- **Solvable in polynomial time** using interior-point methods or simplex method (though simplex has exponential worst-case, it's efficient in practice)

### Key Difference

**LP:** Variables x ∈ ℝ^n (continuous)
**ILP:** Variables x ∈ ℤ^n (integer)

This seemingly small restriction makes the problem NP-complete!

### LP Relaxation

A common technique for solving ILP:
1. Solve the **LP relaxation** (allow fractional values)
2. If LP solution is integer, we're done
3. Otherwise, use branch-and-bound or cutting planes to find integer solution

**Example:**
- ILP: maximize 3x₁ + 2x₂ subject to 2x₁ + x₂ ≤ 6, x₁, x₂ ≥ 0 integer
- LP relaxation: maximize 3x₁ + 2x₂ subject to 2x₁ + x₂ ≤ 6, x₁, x₂ ≥ 0 (real)
- LP solution: (3, 0) with objective 9 (happens to be integer!)
- If LP gave (2.5, 1), we'd need to branch or add cuts

## Practical Implications

### Why ILP is Hard

Integer Linear Programming is NP-complete, which means:

1. **No Known Polynomial-Time Algorithm**: Best known algorithms have exponential worst-case time
2. **Branch-and-Bound**: Systematic search through solution space
3. **Cutting Planes**: Add constraints to eliminate fractional solutions
4. **Branch-and-Cut**: Combines branch-and-bound with cutting planes
5. **Heuristics**: Various heuristics work well in practice

### Solving Methods

**1. Branch-and-Bound:**
- Solve LP relaxation
- If solution is fractional, branch on a fractional variable
- Create two subproblems: xᵢ ≤ ⌊xᵢ*⌋ and xᵢ ≥ ⌈xᵢ*⌉
- Recursively solve subproblems
- Prune branches that can't improve best known solution

**2. Cutting Planes:**
- Solve LP relaxation
- If solution is fractional, find a "cut" (constraint) that:
  - Is satisfied by all integer solutions
  - Is violated by current fractional solution
- Add cut and re-solve
- Repeat until integer solution found

**3. Branch-and-Cut:**
- Combines both techniques
- Most effective in practice

**4. Special Cases:**
- **Unimodular matrices**: If constraint matrix is totally unimodular, LP solution is automatically integer
- **Network flow problems**: Often have integer solutions
- **Assignment problems**: Can be solved as LP (Hungarian algorithm)

### Real-World Applications

ILP has numerous applications:

1. **Scheduling**: Job scheduling, course scheduling, employee scheduling
2. **Resource Allocation**: Allocating resources optimally
3. **Network Design**: Designing networks with capacity constraints
4. **Production Planning**: Optimizing production schedules
5. **Facility Location**: Choosing where to place facilities
6. **Cutting Stock**: Optimizing material cutting
7. **Set Covering/Packing**: Various covering and packing problems
8. **Vehicle Routing**: Optimizing delivery routes
9. **Portfolio Optimization**: With integer constraints
10. **Game Theory**: Finding Nash equilibria in some games

### Modern Solvers

Despite NP-completeness, modern ILP solvers are very effective:

- **CPLEX**: Commercial solver (very powerful)
- **Gurobi**: Commercial solver (excellent performance)
- **GLPK**: Open-source solver
- **CBC**: Open-source solver from COIN-OR
- **SCIP**: Academic solver

These solvers use sophisticated techniques:
- Advanced preprocessing
- Strong cutting planes
- Efficient branch-and-bound
- Parallel processing
- Heuristics

Many practical ILP instances can be solved efficiently, even though worst-case is exponential.

## Runtime Analysis

### Brute Force Approach

**Algorithm:** Try all possible integer assignments (exponential space)
- **Time Complexity:** Exponential in number of variables
- **Space Complexity:** Exponential
- **Not Practical:** Only feasible for very small instances

### Branch-and-Bound

**Algorithm:** Systematic search with LP relaxation bounds
- **Time Complexity:** Exponential worst-case, but much better in practice
- **Space Complexity:** O(n) for recursion stack
- **Key:** Use LP relaxation to get bounds, prune branches that can't improve best solution
- **Practical Performance:** Very effective for many real-world instances

### Cutting Planes

**Algorithm:** Solve LP relaxation, add cuts, repeat
- **Time Complexity:** Exponential worst-case (may need exponential number of cuts)
- **Space Complexity:** O(mn) for storing constraints
- **Cuts:** Gomory cuts, Chvátal-Gomory cuts, etc.
- **Practical Performance:** Often converges quickly in practice

### Branch-and-Cut

**Algorithm:** Combine branch-and-bound with cutting planes
- **Time Complexity:** Exponential worst-case, but state-of-the-art approach
- **Space Complexity:** O(mn)
- **Practical Performance:** Most effective method, used by modern solvers

### LP Relaxation

**Algorithm:** Solve continuous relaxation (ignore integrality)
- **Time Complexity:** O(n^{3.5}L) using interior-point methods where L is input size
- **Space Complexity:** O(mn)
- **Use:** Provides bounds for branch-and-bound, sometimes gives integer solution

### Special Cases

**Totally Unimodular Matrices:**
- **Time Complexity:** O(n^{3.5}L) - solve as LP, solution automatically integer
- **Space Complexity:** O(mn)

**Network Flow Problems:**
- Often have integer solutions when solved as LP

### Modern Solvers (CPLEX, Gurobi)

**Techniques Used:**
- Preprocessing: O(mn) to simplify problem
- Branch-and-cut: Exponential worst-case, but very efficient in practice
- Heuristics: Fast approximate solutions
- **Practical Performance:** Can solve instances with thousands of variables and constraints

### Verification Complexity

**Given a candidate integer solution:**
- **Time Complexity:** O(mn) - verify all constraints satisfied
- **Space Complexity:** O(1) additional space
- This polynomial-time verifiability shows ILP is in NP

## Key Takeaways

1. **ILP is NP-Complete**: Proven by reduction from 3-SAT
2. **LP vs ILP**: Linear Programming is polynomial-time, but requiring integer variables makes it NP-complete
3. **LP Relaxation**: Solving the continuous relaxation is a key technique
4. **Solving Methods**: Branch-and-bound, cutting planes, and branch-and-cut are standard approaches
5. **Practical Solvers**: Modern solvers are very effective despite theoretical hardness

## Reduction Summary

**3-SAT ≤ₚ ILP:**
- Encode Boolean variables as 0-1 integer variables
- Encode clauses as linear constraints requiring at least one true literal
- 3-SAT satisfiable ↔ ILP feasible

**ILP ≤ₚ 0-1 ILP:**
- Can reduce general ILP to binary ILP using binary expansion
- This shows 0-1 ILP is also NP-complete

All reductions are polynomial-time, establishing ILP as NP-complete.

## Further Reading

- **Garey & Johnson**: "Computers and Intractability" - Standard reference for NP-completeness proofs
- **Linear Programming**: Understanding the polynomial-time LP algorithms
- **Integer Programming**: Books by Nemhauser & Wolsey, or Schrijver
- **Modern Solvers**: Documentation for CPLEX, Gurobi, or other solvers
- **Cutting Planes**: Research on Gomory cuts, Chvátal-Gomory cuts, and other cutting plane methods

## Practice Problems

1. **Formulate as ILP**: Convert the following problem to ILP:
   - You have items with weights and values
   - You want to select items to maximize value
   - Total weight must be ≤ capacity
   - Each item can be selected at most once

2. **Reduce 3-SAT to ILP**: For the 3-SAT instance (x₁ ∨ ¬x₂ ∨ x₃) ∧ (¬x₁ ∨ x₂ ∨ x₃), construct the corresponding ILP instance.

3. **LP Relaxation**: Solve the LP relaxation of a small ILP instance. Is the solution integer? If not, how would you proceed?

4. **Branch-and-Bound**: Trace through a branch-and-bound algorithm on a small ILP instance. Show the search tree.

5. **Unimodularity**: Research what it means for a matrix to be totally unimodular. Why does this make ILP easier?

6. **Formulation practice**: Formulate the following as ILP:
   - Set cover problem
   - Maximum independent set (in a graph)
   - Traveling salesman problem

7. **Solver comparison**: Try solving the same ILP instance with different solvers (if available). Compare their performance.

8. **Cutting planes**: Research Gomory cuts. How are they derived? Why do they work?

---

Understanding Integer Linear Programming provides crucial insight into optimization problems and the dramatic impact that requiring integrality can have on problem complexity. The relationship to Linear Programming and the practical solving methods make ILP a cornerstone of operations research and optimization.

