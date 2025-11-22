---
layout: post
title: "Reductions from 3D Matching: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from 3D Matching to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

3D Matching is a fundamental matching problem proven NP-complete by reduction from 3-SAT. This post provides answers to common reduction questions when using 3D Matching to prove other problems are NP-complete.

## Problem Definition: 3D Matching

**3D Matching Problem:**
- **Input:** Sets X, Y, Z and set T ⊆ X × Y × Z of triples
- **Output:** YES if there exists matching M ⊆ T covering all elements, NO otherwise

**Perfect 3D Matching:** A set of triples covering each element exactly once.

## Common Reduction Questions

### Q1: How do you reduce 3D Matching to Set Cover?

**Answer:** Encode triples as sets covering elements.

**Reduction:**
- Given 3D Matching instance: sets X, Y, Z, triples T
- Universe U = X ∪ Y ∪ Z (all elements)
- For each triple t = (x, y, z) ∈ T, create set Sₜ = {x, y, z}
- Return Set Cover: universe U, sets {Sₜ : t ∈ T}, target = |X| = |Y| = |Z|

**Correctness:**
- Perfect 3D matching ↔ Set cover of size |X|
- Matching covers all elements ↔ Sets cover universe

**Time:** O(|T|)

### Q2: How do you reduce 3D Matching to Exact Cover?

**Answer:** 3D Matching is special case of Exact Cover.

**Reduction:**
- Given 3D Matching instance: sets X, Y, Z, triples T
- Universe U = X ∪ Y ∪ Z
- Sets: Each triple t becomes set Sₜ = {x, y, z}
- Return Exact Cover: universe U, sets {Sₜ : t ∈ T}

**Correctness:**
- Perfect 3D matching ↔ Exact cover
- Each element covered exactly once

**Time:** O(1)

### Q3: How do you reduce 3D Matching to Set Partitioning?

**Answer:** Partition triples to cover all elements.

**Reduction:**
- Given 3D Matching instance: sets X, Y, Z, triples T
- Universe U = X ∪ Y ∪ Z
- Sets: Each triple t becomes set Sₜ
- Return Set Partitioning: universe U, sets {Sₜ : t ∈ T}

**Correctness:**
- Perfect 3D matching ↔ Set partition
- Partition covers all elements exactly once

**Time:** O(1)

### Q4: How do you reduce 3D Matching to Assignment Problem?

**Answer:** Encode as three-dimensional assignment.

**Reduction:**
- Given 3D Matching instance: sets X, Y, Z, triples T
- Create Assignment Problem:
  - Three sets: X, Y, Z
  - Assignments: Triples t = (x, y, z)
  - Cost: 0 if t ∈ T, ∞ otherwise
  - Target: Assign all elements

**Correctness:**
- Perfect 3D matching ↔ Assignment covering all elements

**Time:** O(|T|)

### Q5: How do you reduce 3D Matching to Bipartite Matching?

**Answer:** Cannot directly (3D is harder than 2D).

**Note:** 3D Matching is NP-complete, but Bipartite Matching is polynomial-time. However, can reduce to weighted matching.

**Reduction (to Weighted Matching):**
- Given 3D Matching instance
- Create bipartite graph: X on left, Y×Z on right
- Edges: (x, (y,z)) if (x,y,z) ∈ T
- Weight: 1 for each edge
- Return: Maximum weight matching covering all of X

**Correctness:**
- Perfect 3D matching ↔ Perfect bipartite matching

**Time:** O(|T|)

## Reduction Patterns from 3D Matching

### Pattern 1: Covering Problems
- **Use when:** Target problem involves covering
- **Examples:** Set Cover, Exact Cover
- **Key:** Triples cover elements

### Pattern 2: Partitioning
- **Use when:** Target problem partitions elements
- **Examples:** Set Partitioning
- **Key:** Each element exactly once

### Pattern 3: Assignment
- **Use when:** Target problem assigns elements
- **Examples:** Assignment Problem
- **Key:** Three-way assignment

## Key Takeaways

1. **Covering structure:** Natural for covering problems
2. **Exact coverage:** Each element exactly once
3. **Triple structure:** Three sets make it harder than 2D
4. **Partitioning:** Perfect matching = partition

## Practice Problems

1. Reduce 3D Matching to Hitting Set
2. Reduce 3D Matching to Set Packing
3. Reduce 3D Matching to Resource Allocation

---

3D Matching reductions often use covering and partitioning structures.

