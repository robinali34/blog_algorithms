---
layout: post
title: "Linear Programming: Reductions"
date: 2025-01-15
categories: [Algorithms, Complexity Theory, Linear Programming, CS6515, Optimization]
excerpt: "An introduction to Linear Programming, its polynomial-time solvability, and how it relates to reductions in complexity theory, including LP relaxations, duality, and reductions to/from LP."
---

## Introduction

Linear Programming (LP) occupies a unique position in complexity theory: it's one of the few optimization problems that can be solved in polynomial time, yet it's closely related to many NP-complete problems through the concept of LP relaxations. Understanding Linear Programming and its role in reductions is crucial for understanding approximation algorithms, integer programming, and the boundary between polynomial-time and NP-complete problems.

## What is Linear Programming?

Linear Programming asks: **Given linear constraints and a linear objective function, find values for variables that satisfy the constraints and optimize the objective.**

### Problem Definition

**Linear Programming (LP) Problem:**

**Input:** 
- A matrix $A \in \mathbb{R}^{m \times n}$ (constraint coefficients)
- A vector $b \in \mathbb{R}^m$ (constraint bounds)
- A vector $c \in \mathbb{R}^n$ (objective coefficients)

**Output:** 
- A vector $x \in \mathbb{R}^n$ that:
  - Satisfies $Ax \leq b$ (or $Ax = b$, or $Ax \geq b$, or mixed)
  - Satisfies $x \geq 0$ (non-negativity constraints, if present)
  - Maximizes (or minimizes) $c^T x$

**Standard Form:**
- Maximize $c^T x$
- Subject to $Ax \leq b$ and $x \geq 0$

**Canonical Form:**
- Maximize $c^T x$
- Subject to $Ax = b$ and $x \geq 0$

### Example

Consider the LP:

**Maximize:** $3x_1 + 2x_2$

**Subject to:**
- $2x_1 + x_2 \leq 6$
- $x_1 + 2x_2 \leq 8$
- $x_1, x_2 \geq 0$

**Graphical Solution:**
- Feasible region is a polygon
- Optimal solution is at a vertex (corner point)
- Optimal: $(x_1, x_2) = (4/3, 10/3)$ with objective value $32/3 \approx 10.67$

**Key Insight:** The optimal solution of an LP always occurs at a vertex of the feasible region (if the problem is bounded).

## Why LP is in P

Unlike Integer Linear Programming, **Linear Programming is solvable in polynomial time**.

### Algorithms for LP

**1. Simplex Method (Dantzig, 1947):**
- Moves from vertex to vertex along edges
- Very efficient in practice
- **Worst-case:** Exponential (Klee-Minty examples)
- **Average-case:** Polynomial

**2. Ellipsoid Method (Khachiyan, 1979):**
- First polynomial-time algorithm for LP
- $O(n^4 L)$ time where $L$ is input size
- Not practical due to large constants

**3. Interior-Point Methods (Karmarkar, 1984):**
- Polynomial-time: $O(n^{3.5} L)$ time
- Practical and widely used
- Modern implementations are very efficient

**Result:** LP ∈ P (solvable in polynomial time)

## LP Relaxation: A Key Reduction Technique

One of the most important uses of LP in complexity theory is the concept of **LP relaxation**.

### What is LP Relaxation?

Given an Integer Linear Programming (ILP) problem:
- **ILP:** Variables must be integers
- **LP Relaxation:** Allow variables to be real numbers

**Key Insight:** The optimal value of the LP relaxation provides a **bound** on the optimal value of the ILP:
- For maximization: LP optimal ≥ ILP optimal
- For minimization: LP optimal ≤ ILP optimal

### Example: Vertex Cover LP Relaxation

**ILP for Vertex Cover:**
- Variables: $x_v \in \{0,1\}$ for each vertex $v$
- Constraints: $x_u + x_v \geq 1$ for each edge $(u,v)$
- Objective: Minimize $\sum_v x_v$

**LP Relaxation:**
- Variables: $x_v \in [0,1]$ (continuous, not integer)
- Same constraints and objective

**Result:** LP optimal ≤ ILP optimal (since we relaxed constraints)

**2-Approximation Algorithm:**
1. Solve LP relaxation
2. Round: Include vertex $v$ if $x_v \geq 1/2$
3. This gives a 2-approximation for Vertex Cover!

## Reductions Involving LP

### Reduction: ILP to LP (Relaxation)

**ILP ≤ₚ LP (via relaxation):**
- Given ILP instance, remove integrality constraints
- Solve LP relaxation
- If LP solution is integer, we're done
- Otherwise, use branch-and-bound or cutting planes

**Note:** This is not a polynomial-time reduction in the complexity sense, but it's a practical technique.

### Reduction: LP to Feasibility

**LP Optimization ≤ₚ LP Feasibility:**
- Given LP: maximize $c^T x$ subject to $Ax \leq b$, $x \geq 0$
- Add constraint: $c^T x \geq k$ (where $k$ is a guess for optimal value)
- Use binary search on $k$ to find optimal value
- This reduces optimization to feasibility checking

### Reduction: General LP to Standard Form

**General LP ≤ₚ Standard Form LP:**
- Convert inequalities to equations using slack variables
- Convert unconstrained variables: $x = x^+ - x^-$ where $x^+, x^- \geq 0$
- Convert maximization to minimization: negate objective
- All conversions are polynomial-time

## LP Duality and Reductions

### Duality Theorem

Every LP has a **dual** LP:

**Primal:** Maximize $c^T x$ subject to $Ax \leq b$, $x \geq 0$

**Dual:** Minimize $b^T y$ subject to $A^T y \geq c$, $y \geq 0$

**Strong Duality:** If both have feasible solutions, then:
- Primal optimal = Dual optimal

**Weak Duality:** For any feasible $x$ and $y$:
- $c^T x \leq b^T y$

### Using Duality in Reductions

Duality provides a powerful tool for:
1. **Proving optimality:** If primal and dual have same value, both are optimal
2. **Finding bounds:** Dual provides upper bounds (for maximization)
3. **Sensitivity analysis:** Understanding how changes affect solution
4. **Designing algorithms:** Many algorithms use duality

## LP in Approximation Algorithms

LP relaxation is fundamental to many approximation algorithms.

### Vertex Cover: 2-Approximation

**Algorithm:**
1. Solve LP relaxation: minimize $\sum_v x_v$ subject to $x_u + x_v \geq 1$ for all edges
2. Round: $S = \{v : x_v \geq 1/2\}$
3. Return $S$ as vertex cover

**Analysis:**
- $S$ is a vertex cover (each edge has at least one endpoint with $x_v \geq 1/2$)
- $|S| = \sum_{v \in S} 1 \leq \sum_{v \in S} 2x_v \leq 2 \cdot \text{LP optimal} \leq 2 \cdot \text{ILP optimal}$
- Therefore, 2-approximation

### Set Cover: LP-Based Approximation

**Set Cover ILP:**
- Variables: $x_S \in \{0,1\}$ for each set $S$
- Constraints: $\sum_{S: e \in S} x_S \geq 1$ for each element $e$
- Objective: Minimize $\sum_S x_S$

**LP Relaxation + Rounding:**
- Solve LP relaxation
- Use randomized rounding or greedy rounding
- Achieves $O(\log n)$ approximation (or better with specific techniques)

### Maximum Flow: LP Formulation

**Max Flow as LP:**
- Variables: flow $f_e$ on each edge $e$
- Constraints: flow conservation, capacity constraints
- Objective: maximize flow from source to sink

**Result:** Max flow can be solved via LP (though specialized algorithms are faster)

## Reductions from NP-Complete Problems to LP

While LP is polynomial-time, we can reduce NP-complete problems to LP feasibility questions.

### 3-SAT to LP Feasibility

**Reduction:**
- For 3-SAT instance, create LP:
  - Variables: $x_i \in [0,1]$ for each Boolean variable
  - For clause $(l_1 \lor l_2 \lor l_3)$: constraint ensuring at least one literal is "true"
  - But LP doesn't naturally encode Boolean logic...

**Better:** Reduce to ILP, then use LP relaxation
- 3-SAT → ILP (as we saw earlier)
- ILP feasibility can be checked via LP (but LP solution might not be integer)

**Key Point:** LP relaxation gives bounds, but doesn't solve the original problem.

## LP Rounding Techniques

Various rounding techniques convert LP solutions to integer solutions:

### Deterministic Rounding

**Example - Vertex Cover:**
- Round $x_v \geq 1/2$ to 1, else 0
- Guarantees feasibility and approximation ratio

### Randomized Rounding

**Example - Set Cover:**
- Include set $S$ with probability proportional to $x_S^*$ (LP solution)
- Expected cost equals LP cost
- May need derandomization

### Dependent Rounding

**Example - Matching:**
- Round edges while maintaining constraints
- More sophisticated than independent rounding

## Practical Implications

### Why LP Matters

Linear Programming is crucial because:

1. **Polynomial-Time Solvability:** Can solve large instances efficiently
2. **LP Relaxation:** Provides bounds for NP-complete problems
3. **Approximation Algorithms:** Foundation for many approximation schemes
4. **Duality:** Powerful theoretical and practical tool
5. **Widespread Applications:** Used in many real-world optimization problems

### Modern LP Solvers

**Commercial Solvers:**
- **CPLEX:** Industry standard, very fast
- **Gurobi:** Excellent performance, good academic licenses
- **XPRESS:** Commercial solver

**Open-Source Solvers:**
- **GLPK:** GNU Linear Programming Kit
- **CLP:** COIN-OR Linear Programming
- **HiGHS:** Modern, high-performance solver

**Interfaces:**
- **PuLP** (Python)
- **CVXPY** (Python)
- **JuMP** (Julia)
- **OR-Tools** (Google)

### Real-World Applications

LP has countless applications:

1. **Resource Allocation:** Allocating limited resources optimally
2. **Production Planning:** Optimizing production schedules
3. **Transportation:** Network flow, transportation problems
4. **Finance:** Portfolio optimization, risk management
5. **Scheduling:** Workforce scheduling, project scheduling
6. **Network Design:** Designing efficient networks
7. **Game Theory:** Finding Nash equilibria in some games

## Runtime Analysis

### Simplex Method

**Algorithm:** Move from vertex to vertex along edges
- **Time Complexity:** Exponential worst-case (Klee-Minty examples), but polynomial average-case
- **Space Complexity:** $O(mn)$ for storing tableau
- **Practical Performance:** Very efficient in practice, often faster than polynomial methods
- **Iterations:** Typically $O(m)$ to $O(m+n)$ iterations

### Ellipsoid Method

**Algorithm:** Shrink ellipsoid containing feasible region
- **Time Complexity:** $O(n^4L)$ where $L$ is input size (bit complexity)
- **Space Complexity:** $O(n^2)$
- **Significance:** First proven polynomial-time algorithm for LP
- **Practical Performance:** Not used in practice due to large constants

### Interior-Point Methods

**Algorithm:** Move through interior of feasible region
- **Time Complexity:** $O(n^{3.5}L)$ using path-following methods
- **Space Complexity:** $O(n^2)$ for storing matrices
- **Practical Performance:** Very efficient, widely used in modern solvers
- **Iterations:** Typically $O(\sqrt{n} \log(1/\epsilon))$ iterations for $\epsilon$-accuracy

### Primal-Dual Methods

**Algorithm:** Solve primal and dual simultaneously
- **Time Complexity:** $O(n^{3.5}L)$ similar to interior-point
- **Space Complexity:** $O(n^2)$
- **Advantage:** Can exploit structure better in some cases

### Special Cases

**Network Flow Problems:**
- **Time Complexity:** $O(n^2m)$ using specialized algorithms (faster than general LP)
- **Space Complexity:** $O(n + m)$

**Transportation Problems:**
- Specialized algorithms can be more efficient than general LP

### LP Relaxation Runtime

**For ILP Relaxation:**
- **Time Complexity:** $O(n^{3.5}L)$ - solve as regular LP
- **Space Complexity:** $O(mn)$
- **Use:** Provides bounds for branch-and-bound algorithms

### Modern Solvers

**Commercial Solvers (CPLEX, Gurobi):**
- **Time Complexity:** Polynomial (interior-point or simplex)
- **Space Complexity:** $O(mn)$
- **Practical Performance:** Can solve instances with millions of variables and constraints
- **Techniques:** Preprocessing, advanced pivoting, parallel processing

### Verification Complexity

**Given a candidate solution:**
- **Time Complexity:** $O(mn)$ - verify all constraints satisfied
- **Space Complexity:** $O(1)$ additional space
- This polynomial-time verifiability is straightforward for LP

## Key Takeaways

1. **LP is in P:** Solvable in polynomial time using interior-point methods
2. **LP Relaxation:** Fundamental technique for approximating NP-complete problems
3. **Duality:** Powerful theoretical tool with practical applications
4. **Approximation Algorithms:** Many use LP relaxation + rounding
5. **Reductions:** LP provides bounds and approximations, not exact solutions for integer problems

## Reduction Summary

**Key Reductions Involving LP:**

1. **ILP → LP (Relaxation):**
   - Remove integrality constraints
   - Provides upper/lower bounds
   - Used in branch-and-bound

2. **LP Optimization → LP Feasibility:**
   - Use binary search on objective value
   - Reduces optimization to feasibility

3. **General LP → Standard Form:**
   - Convert to standard form using slack variables
   - All conversions are polynomial-time

4. **NP-Complete → LP Relaxation:**
   - Many NP-complete problems have natural LP relaxations
   - LP solution provides approximation bounds

## Further Reading

- **Linear Programming:** Chvátal's "Linear Programming" or Vanderbei's "Linear Programming"
- **Interior-Point Methods:** Nesterov & Nemirovskii's work on interior-point methods
- **Approximation Algorithms:** Vazirani's "Approximation Algorithms" covers LP-based approximations
- **Duality:** Understanding the dual simplex method and sensitivity analysis
- **Modern Solvers:** Documentation for CPLEX, Gurobi, or other solvers

## Practice Problems

1. **Formulate as LP**: Convert the following to LP:
   - You have resources and want to maximize profit
   - Each product uses certain amounts of resources
   - You have limited resources
   - Products have different profits

2. **LP Relaxation**: For a Vertex Cover instance, write the LP relaxation. What is the relationship between LP optimal and ILP optimal?

3. **Duality**: Write the dual of the following LP:
   - Maximize $3x_1 + 2x_2$
   - Subject to $2x_1 + x_2 \leq 6$, $x_1 + 2x_2 \leq 8$, $x_1, x_2 \geq 0$

4. **Rounding**: Prove that the LP-based 2-approximation for Vertex Cover is correct. What happens if we round at threshold $1/3$ instead of $1/2$?

5. **Reduction**: Show how to reduce LP optimization to LP feasibility using binary search. What is the time complexity?

6. **Standard Form**: Convert the following to standard form:
   - Minimize $x_1 - 2x_2$
   - Subject to $x_1 + x_2 = 5$, $x_1 \geq 0$, $x_2$ unrestricted

7. **Applications**: Research one real-world application of LP. How is it formulated? What solver is used?

8. **Approximation**: Research the LP-based approximation for Set Cover. What approximation ratio does it achieve? How does rounding work?

9. **Duality Applications**: How is LP duality used in the design of algorithms? Research one example.

10. **Interior-Point Methods**: Research how interior-point methods work. How do they differ from the simplex method?

---

Understanding Linear Programming and its role in reductions provides crucial insight into the boundary between polynomial-time and NP-complete problems. LP relaxation is one of the most powerful techniques for designing approximation algorithms and understanding the structure of optimization problems.

