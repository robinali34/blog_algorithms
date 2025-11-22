---
layout: post
title: "Reductions from Subset Sum: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from Subset Sum to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

Subset Sum is a fundamental number problem proven NP-complete by reduction from 3-SAT. This post provides answers to common reduction questions when using Subset Sum to prove other problems are NP-complete.

## Problem Definition: Subset Sum

**Subset Sum Problem:**
- **Input:** Set S of integers and target integer t
- **Output:** YES if there exists subset S' ⊆ S with sum exactly t, NO otherwise

## Common Reduction Questions

### Q1: How do you reduce Subset Sum to Partition?

**Answer:** Add balancing elements to create equal partition.

**Reduction:**
- Given Subset Sum instance: set S, target t
- Compute T = sum of all elements in S
- Create set S' = S ∪ {2T - t, T + t}
- Return Partition: set S'

**Correctness:**
- Subset summing to t exists ↔ Partition exists
- If subset sums to t, partition: {subset, 2T-t} and {complement, T+t}
- Both partitions sum to 2T

**Time:** O(n)

### Q2: How do you reduce Subset Sum to Knapsack?

**Answer:** Subset Sum is special case of Knapsack.

**Reduction:**
- Given Subset Sum instance: set S, target t
- Create Knapsack instance:
  - Items: Each element aᵢ ∈ S becomes item with weight wᵢ = aᵢ, value vᵢ = aᵢ
  - Capacity: t
  - Target value: t

**Correctness:**
- Subset summing to t ↔ Knapsack solution with value t and weight t

**Time:** O(n)

### Q3: How do you reduce Subset Sum to Bin Packing?

**Answer:** Encode subset sum as bin packing with two bins.

**Reduction:**
- Given Subset Sum instance: set S, target t
- Compute T = sum(S)
- Create Bin Packing instance:
  - Items: Elements of S
  - Bin capacity: max(t, T - t)
  - Number of bins: 2

**Correctness:**
- Subset summing to t ↔ Items fit in 2 bins
- One bin has sum t, other has sum T - t

**Time:** O(n)

### Q4: How do you reduce Subset Sum to Scheduling?

**Answer:** Encode subset sum as job scheduling with deadlines.

**Reduction:**
- Given Subset Sum instance: set S, target t
- Create Scheduling instance:
  - Jobs: Each element aᵢ becomes job with processing time aᵢ
  - Deadline: t
  - Target: Schedule jobs to meet deadline

**Correctness:**
- Subset summing to t ↔ Jobs can be scheduled to finish by t

**Time:** O(n)

### Q5: How do you reduce Subset Sum to Number Partitioning?

**Answer:** Subset Sum is related to Partition (see Q1).

**Reduction:**
- Use Partition reduction (Q1)
- Partition is special case of Number Partitioning

**Correctness:**
- Subset Sum → Partition → Number Partitioning

**Time:** O(n)

## Reduction Patterns from Subset Sum

### Pattern 1: Restriction
- **Use when:** Target problem is generalization
- **Examples:** Knapsack, Bin Packing
- **Key:** Subset Sum is special case

### Pattern 2: Balancing
- **Use when:** Target problem requires equal partitions
- **Examples:** Partition
- **Key:** Add balancing elements

### Pattern 3: Scheduling
- **Use when:** Target problem involves time/resources
- **Examples:** Scheduling, Resource Allocation
- **Key:** Numbers represent time/resource requirements

## Key Takeaways

1. **Restriction:** Many problems generalize Subset Sum
2. **Balancing:** Partition reductions use balancing
3. **Scheduling:** Numbers encode time/resource constraints
4. **Pseudo-polynomial:** Has DP solution, but still NP-complete

## Practice Problems

1. Reduce Subset Sum to 0-1 Knapsack
2. Reduce Subset Sum to Multiple Knapsack
3. Reduce Subset Sum to Job Scheduling with Precedence

---

Subset Sum reductions often use restriction and balancing techniques.

