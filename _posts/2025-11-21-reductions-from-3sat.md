---
layout: post
title: "Reductions from 3-SAT: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from 3-SAT to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

3-SAT is the most commonly used starting point for NP-completeness proofs. This post provides answers to common reduction questions when using 3-SAT to prove other problems are NP-complete.

## Problem Definition: 3-SAT

**3-SAT Problem:**
- **Input:** A Boolean formula φ in 3-CNF (exactly 3 literals per clause)
- **Output:** YES if φ is satisfiable, NO otherwise

**Why 3-SAT is Popular:**
- Simpler structure than general SAT
- Exactly 3 literals per clause simplifies gadget design
- Most reductions use 3-SAT as starting point

## Common Reduction Questions

### Q1: How do you reduce 3-SAT to Independent Set?

**Answer:** Create variable gadgets (pairs) and clause gadgets (triangles).

**Reduction:**
- **Variable gadgets:** For each xᵢ, create vertices vᵢ (TRUE) and v'ᵢ (FALSE), connect with edge
- **Clause gadgets:** For each clause, create triangle of 3 vertices (one per literal)
- **Connections:** Connect clause vertex to opposite variable vertex
- **Target:** k = n + m (n variables + m clauses)

**Correctness:**
- 3-SAT satisfiable ↔ Independent set of size n+m exists
- Pick one vertex per variable pair, one per clause triangle

**Time:** O(n + m)

### Q2: How do you reduce 3-SAT to Vertex Cover?

**Answer:** Use complement relationship with Independent Set.

**Reduction:**
- Reduce 3-SAT to Independent Set → graph G, k = n+m
- Return Vertex Cover: graph G, k' = |V| - (n+m)

**Correctness:**
- Independent set of size n+m ↔ Vertex cover of size |V| - (n+m)

**Time:** O(n + m)

### Q3: How do you reduce 3-SAT to Clique?

**Answer:** Use complement graph of Independent Set reduction.

**Reduction:**
- Reduce 3-SAT to Independent Set → graph G, k = n+m
- Create complement graph G̅
- Return Clique: graph G̅, k = n+m

**Correctness:**
- Independent set in G ↔ Clique in G̅

**Time:** O(n²)

### Q4: How do you reduce 3-SAT to Subset Sum?

**Answer:** Use base representation to encode constraints.

**Reduction:**
- Create numbers with n+m digits (n variables + m clauses)
- **Variable numbers:** Two per variable, encode assignment
- **Clause numbers:** Encode clause satisfaction
- **Target:** Sum encoding all variables assigned and all clauses satisfied

**Correctness:**
- 3-SAT satisfiable ↔ Subset sums to target
- Digits encode variable assignments and clause satisfaction

**Time:** O(nm)

### Q5: How do you reduce 3-SAT to 3D Matching?

**Answer:** Create variable gadgets (chains) and clause gadgets (triples).

**Reduction:**
- **Variable gadgets:** For each xᵢ, create chain of triples
- **Clause gadgets:** For each clause, create triples connecting to variable gadgets
- **Matching:** Covers all elements ↔ consistent assignment + satisfied clauses

**Correctness:**
- 3-SAT satisfiable ↔ Perfect 3D matching exists
- Matching enforces variable consistency and clause satisfaction

**Time:** O(nm)

### Q6: How do you reduce 3-SAT to Integer Linear Programming?

**Answer:** Encode variables as 0-1 variables and clauses as constraints.

**Reduction:**
- For each xᵢ, create variable xᵢ ∈ {0,1}
- For clause (l₁ ∨ l₂ ∨ l₃):
  - If lⱼ = xₖ, use xₖ
  - If lⱼ = ¬xₖ, use (1 - xₖ)
  - Constraint: l₁ + l₂ + l₃ ≥ 1

**Correctness:**
- 3-SAT satisfiable ↔ ILP feasible
- Each clause requires at least one TRUE literal

**Time:** O(n + m)

### Q7: How do you reduce 3-SAT to Zero-One Equations?

**Answer:** Convert to system of equations over GF(2).

**Reduction:**
- For each variable xᵢ, create xᵢ ∈ {0,1}
- For clause (l₁ ∨ l₂ ∨ l₃):
  - Equation: l₁ + l₂ + l₃ = 1 (mod 2)
  - Use xᵢ for positive, (1 - xᵢ) for negative

**Correctness:**
- 3-SAT satisfiable ↔ ZOE has solution
- Exactly one literal TRUE per clause (mod 2)

**Time:** O(nm)

### Q8: How do you reduce 3-SAT to Rudrata Cycle?

**Answer:** Create variable gadgets (paths) and clause gadgets (requiring visits).

**Reduction:**
- **Variable gadgets:** For each xᵢ, create path with two routes (TRUE/FALSE)
- **Clause gadgets:** For each clause, create vertices that must be visited
- **Connections:** Link gadgets to enforce consistency

**Correctness:**
- 3-SAT satisfiable ↔ Hamiltonian cycle exists
- Cycle encodes variable assignment and clause satisfaction

**Time:** O(n²m)

### Q9: How do you reduce 3-SAT to Traveling Salesman Problem?

**Answer:** Reduce via Rudrata Cycle.

**Reduction:**
- Reduce 3-SAT to Rudrata Cycle → graph G
- Create complete graph with weights:
  - Weight 1 if edge exists in G
  - Weight 2 if edge doesn't exist
- Target: n (number of vertices)

**Correctness:**
- Rudrata cycle exists ↔ TSP tour of weight n exists

**Time:** O(n²)

### Q10: How do you reduce 3-SAT to Set Cover?

**Answer:** Encode variables and clauses as sets.

**Reduction:**
- Universe: All clauses
- For each variable xᵢ:
  - Set Sᵢ: Clauses where xᵢ appears positively
  - Set S'ᵢ: Clauses where ¬xᵢ appears
- Target: Cover all clauses with n sets

**Correctness:**
- 3-SAT satisfiable ↔ Set Cover of size n exists
- Each clause covered by satisfied literal's set

**Time:** O(nm)

## Reduction Patterns from 3-SAT

### Pattern 1: Graph Problems
- **Variable gadgets:** Pairs of vertices (TRUE/FALSE)
- **Clause gadgets:** Triangles or other structures
- **Examples:** Independent Set, Vertex Cover, Clique

### Pattern 2: Number Problems
- **Base representation:** Digits encode constraints
- **Variable encoding:** Numbers represent assignments
- **Examples:** Subset Sum, Partition

### Pattern 3: Set Problems
- **Universe:** Clauses or elements
- **Sets:** Variable assignments or choices
- **Examples:** Set Cover, Exact Cover, 3D Matching

### Pattern 4: Constraint Problems
- **Variables:** Direct mapping
- **Constraints:** Clause requirements
- **Examples:** ILP, ZOE, Graph Coloring

## Key Takeaways

1. **3-SAT is standard:** Most reductions start from 3-SAT
2. **Gadget design:** Variable gadgets + clause gadgets
3. **Consistency:** Ensure variable assignments are consistent
4. **Satisfaction:** Ensure all clauses are satisfied
5. **Polynomial time:** Most reductions are efficient

## Practice Problems

1. Reduce 3-SAT to Dominating Set
2. Reduce 3-SAT to Graph Coloring
3. Reduce 3-SAT to Longest Path
4. Reduce 3-SAT to Knapsack
5. Reduce 3-SAT to Bin Packing

---

3-SAT is the workhorse of NP-completeness proofs, and mastering reductions from 3-SAT is essential for complexity theory.

