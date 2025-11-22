---
layout: post
title: "Reductions from Traveling Salesman Problem: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from Traveling Salesman Problem (TSP) to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

Traveling Salesman Problem (TSP) is a fundamental routing problem proven NP-complete by reduction from Hamiltonian Cycle. This post provides answers to common reduction questions when using TSP to prove other problems are NP-complete.

## Problem Definition: Traveling Salesman Problem

**TSP Problem:**
- **Input:** Complete graph G with edge weights w, bound B
- **Output:** YES if there exists tour visiting all vertices with total weight ≤ B, NO otherwise

**TSP Tour:** A cycle visiting each vertex exactly once.

## Common Reduction Questions

### Q1: How do you reduce TSP to Metric TSP?

**Answer:** TSP is general case, Metric TSP is restriction.

**Reduction:**
- Given TSP instance: graph G, weights w, bound B
- Check if weights satisfy triangle inequality
- If yes, return Metric TSP instance (same)
- If no, TSP is harder than Metric TSP

**Note:** This is a restriction, not a reduction. Metric TSP is still NP-complete.

**Correctness:**
- TSP generalizes Metric TSP
- Metric TSP is restriction of TSP

**Time:** O(n²) to check triangle inequality

### Q2: How do you reduce TSP to Vehicle Routing Problem?

**Answer:** TSP is special case with one vehicle.

**Reduction:**
- Given TSP instance: graph G, weights w, bound B
- Create Vehicle Routing instance:
  - Vehicles: 1
  - Depots: One depot (starting vertex)
  - Customers: All other vertices
  - Distances: Same as TSP weights
  - Capacity: Unlimited (or large enough)

**Correctness:**
- TSP tour ↔ Vehicle route covering all customers

**Time:** O(1)

### Q3: How do you reduce TSP to Job Sequencing?

**Answer:** Encode tour as job sequence.

**Reduction:**
- Given TSP instance: graph G, weights w, bound B
- Create Job Sequencing instance:
  - Jobs: Vertices (except start)
  - Setup times: Edge weights
  - Sequence: Tour order
  - Target: Total setup time ≤ B

**Correctness:**
- TSP tour ↔ Job sequence with setup times ≤ B

**Time:** O(n²)

### Q4: How do you reduce TSP to Longest Tour?

**Answer:** Negate weights and maximize.

**Reduction:**
- Given TSP instance: graph G, weights w, bound B
- Create Longest Tour instance:
  - Graph G with weights w' = M - w (M large constant)
  - Target: Tour length ≥ M·n - B

**Correctness:**
- TSP tour weight ≤ B ↔ Longest tour weight ≥ M·n - B

**Time:** O(n²)

### Q5: How do you reduce TSP to Hamiltonian Cycle?

**Answer:** TSP reduces to Hamiltonian Cycle via weight threshold.

**Reduction:**
- Given TSP instance: graph G, weights w, bound B
- Create graph G' with edges:
  - Keep edge if weight ≤ B
  - Remove edge if weight > B
- Return Hamiltonian Cycle: graph G'

**Correctness:**
- TSP tour weight ≤ B ↔ Hamiltonian cycle in G'
- Cycle uses only edges with weight ≤ B

**Time:** O(n²)

## Reduction Patterns from TSP

### Pattern 1: Restriction
- **Use when:** Target problem is special case
- **Examples:** Metric TSP
- **Key:** TSP generalizes many problems

### Pattern 2: Routing Problems
- **Use when:** Target problem involves routing
- **Examples:** Vehicle Routing, Delivery Problem
- **Key:** Tour structure

### Pattern 3: Sequencing
- **Use when:** Target problem sequences items
- **Examples:** Job Sequencing
- **Key:** Tour defines sequence

## Key Takeaways

1. **Tour structure:** Natural for routing problems
2. **Restriction:** Many problems are special cases
3. **Weight manipulation:** Negate or threshold weights
4. **Generalization:** TSP generalizes many routing problems

## Practice Problems

1. Reduce TSP to Multiple TSP (multiple salesmen)
2. Reduce TSP to Prize-Collecting TSP
3. Reduce TSP to TSP with Time Windows

---

TSP reductions often involve routing structures and weight manipulations.

