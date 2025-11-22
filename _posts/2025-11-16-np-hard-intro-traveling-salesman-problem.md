---
layout: post
title: "NP-Hard Introduction: Traveling Salesman Problem (TSP)"
date: 2025-11-16
categories: [Algorithms, Complexity Theory, NP-Hard, Graph Theory, Optimization]
excerpt: "An introduction to NP-hardness through the Traveling Salesman Problem (TSP), covering problem definition, NP-completeness proof, dynamic programming solution, approximation algorithms, and practical applications."
---

## Introduction

The Traveling Salesman Problem (TSP) is one of the most famous and extensively studied problems in computer science and operations research. It asks: given a set of cities and distances between them, what is the shortest route that visits each city exactly once and returns to the starting city? Despite its simple formulation, TSP is NP-complete and serves as a benchmark for optimization algorithms. TSP has countless applications in logistics, manufacturing, DNA sequencing, and many other fields.

## What is the Traveling Salesman Problem?

The Traveling Salesman Problem asks: **Given a complete graph with edge weights (distances), find the minimum-weight cycle that visits every vertex exactly once.**

### Problem Definition

**TSP Decision Problem:**

**Input:** 
- A complete undirected graph G = (V, E) with `|V|` = n
- Edge weights w: E → ℝ⁺ (distances/costs)
- A bound B

**Output:** YES if there exists a Hamiltonian cycle (tour) with total weight ≤ B, NO otherwise

**TSP Optimization Problem:**

**Input:** Same as above (without B)

**Output:** The minimum weight of a Hamiltonian cycle, or the cycle itself

### Variants

**Metric TSP:**
- Distances satisfy triangle inequality: w(u,v) ≤ w(u,w) + w(w,v)
- More structured, better approximation algorithms exist

**Euclidean TSP:**
- Cities are points in Euclidean plane
- Distance is Euclidean distance
- Still NP-complete, but has PTAS (Polynomial-Time Approximation Scheme)

**Asymmetric TSP (ATSP):**
- Directed graph (different distances in each direction)
- Also NP-complete

**TSP with Triangle Inequality:**
- Same as metric TSP
- Allows for approximation algorithms

### Example

Consider 4 cities with distances:

```
    A
   /|\
  / | \
 /  |  \
B---C---D
```

Distances:
- AB = 10, AC = 15, AD = 20
- BC = 10, BD = 25, CD = 10

**Possible tours:**
- A → B → C → D → A: 10 + 10 + 10 + 20 = 50
- A → B → D → C → A: 10 + 25 + 10 + 15 = 60
- A → C → B → D → A: 15 + 10 + 25 + 20 = 70
- A → C → D → B → A: 15 + 10 + 25 + 10 = 60
- A → D → C → B → A: 20 + 10 + 10 + 10 = 50 (same as first, reversed)

**Optimal tour:** A → B → C → D → A (or reverse) with weight 50.

## Why TSP is in NP

To show that TSP is NP-complete, we first need to show it's in NP.

**TSP ∈ NP:**

Given a candidate solution (a sequence of vertices representing a tour), we can verify in polynomial time:
1. Check that the tour has exactly n vertices: O(n) time
2. Check that each vertex appears exactly once: O(n) time
3. Check that it forms a cycle (returns to start): O(1) time
4. Sum the edge weights: O(n) time
5. Check if total weight ≤ B: O(1) time

Total verification time: O(n), which is polynomial in the input size. Therefore, TSP is in NP.

## NP-Completeness: Reduction from Hamiltonian Cycle

The most direct proof that TSP is NP-complete reduces from the Hamiltonian Cycle Problem.

### Reduction from Hamiltonian Cycle to TSP

**Reduction:**
1. Given a Hamiltonian Cycle instance: graph G = (V, E)
2. Create complete graph G' with same vertex set V
3. Set edge weights:
   - If edge (u,v) ∈ E, set w(u,v) = 1
   - If edge (u,v) ∉ E, set w(u,v) = 2 (or any value > n)
4. Set bound B = n
5. Return TSP instance: graph G' with weights and bound B

**Correctness:**
- If G has a Hamiltonian cycle, then G' has a tour of weight n (all edges have weight 1)
- If G' has a tour of weight ≤ n, it must use only edges of weight 1
- These edges exist in G, so G has a Hamiltonian cycle

**Polynomial Time:**
- Creating G' takes O(n^2) time
- Setting weights takes O(n^2) time

Therefore, **TSP is NP-complete**.

### Relationship to Rudrata Cycle

As we saw in the Rudrata Cycle post:
- **Rudrata Cycle** is essentially unweighted TSP
- **TSP** is weighted Rudrata Cycle
- They are polynomially equivalent

## Dynamic Programming Solution

Despite being NP-complete, TSP has a **pseudo-polynomial** dynamic programming solution (exponential in n, but polynomial in the representation of weights if weights are small).

### DP Algorithm (Held-Karp Algorithm)

**Subproblem:** dp[mask][v] = minimum cost to visit all vertices in mask ending at vertex v, starting from vertex 0.

**Recurrence:**
- Base case: dp[2^0][0] = 0 (at start, cost is 0)
- Recurrence: dp[mask][v] = min_{u ∈ mask, u \neq v} {dp[mask \ {v}][u] + w(u,v)}
- Final answer: min_{v} {dp[2^n-1][v] + w(v,0)} (return to start)

**Algorithm:**
```
Algorithm: TSP_DP(G, w)
1. n = `|V|`
2. Let dp[0..2^n-1][0..n-1] be a 2D array
3. dp[2^0][0] = 0
4. for mask = 1 to 2^n - 1:
5.     for v = 0 to n-1:
6.         if v in mask:
7.             dp[mask][v] = infinity
8.             for each u in mask, u != v:
9.                 if (u,v) is an edge:
10.                    dp[mask][v] = min(dp[mask][v], 
11.                                     dp[mask - 2^v][u] + w(u,v))
12. result = infinity
13. for v = 0 to n-1:
14.     result = min(result, dp[2^n-1][v] + w(v,0))
15. return result
```

**Time Complexity:** O(2^n · n^2)
**Space Complexity:** O(2^n · n)

This is the **Held-Karp algorithm**, one of the most efficient exact algorithms for TSP.

## Approximation Algorithms

Since TSP is NP-complete, approximation algorithms are important for practical applications.

### 2-Approximation for Metric TSP

**Nearest Neighbor Heuristic:**
1. Start at an arbitrary vertex
2. Repeatedly go to nearest unvisited vertex
3. Return to start

**Approximation Ratio:** No guaranteed ratio (can be arbitrarily bad)

**Double Tree + Matching (Christofides Algorithm):**
1. Find minimum spanning tree (MST)
2. Find minimum-weight perfect matching on odd-degree vertices
3. Combine to form Eulerian tour
4. Shortcut to get Hamiltonian cycle

**Approximation Ratio:** 1.5 (for metric TSP)

### 2-Approximation for Metric TSP (Simpler)

**MST-Based Algorithm:**
1. Find MST T
2. Double all edges to get Eulerian graph
3. Find Eulerian tour
4. Shortcut to get Hamiltonian cycle (skip already-visited vertices)

**Approximation Ratio:** 2 (for metric TSP)

**Why it works:**
- MST weight ≤ optimal TSP tour (remove one edge from optimal tour to get spanning tree)
- Doubled MST has weight 2 · MST ≤ 2 · OPT
- Shortcutting doesn't increase cost (triangle inequality)

## Practical Implications

### Why TSP is Hard

The Traveling Salesman Problem is NP-complete, which means:

1. **No Known Polynomial-Time Algorithm**: Best known exact algorithms are exponential
2. **Brute Force**: Try all (n-1)!/2 tours - factorial time
3. **Dynamic Programming**: O(2^n · n^2) time (Held-Karp)
4. **Branch-and-Bound**: Practical for medium instances
5. **Heuristics**: Very effective in practice

### Real-World Applications

TSP has countless applications:

1. **Logistics**: Vehicle routing, delivery optimization
2. **Manufacturing**: Drilling holes in circuit boards, cutting materials
3. **DNA Sequencing**: Finding shortest path through overlap graph
4. **Network Design**: Designing network topologies
5. **Scheduling**: Scheduling with travel time
6. **Astronomy**: Planning telescope observations
7. **X-Ray Crystallography**: Determining molecular structure

### Modern Solving Methods

**Exact Algorithms:**
- **Held-Karp DP**: O(2^n · n^2) - best for small-medium instances
- **Branch-and-Bound**: Combine with LP relaxation
- **Cutting Planes**: Add constraints to eliminate suboptimal tours
- **Concorde**: State-of-the-art exact TSP solver

**Heuristic Algorithms:**
- **Nearest Neighbor**: Simple but not guaranteed
- **2-Opt/3-Opt**: Local search improvements
- **Lin-Kernighan**: Advanced local search
- **Genetic Algorithms**: Evolutionary approaches
- **Simulated Annealing**: Probabilistic optimization

**Approximation Algorithms:**
- **Christofides**: 1.5-approximation for metric TSP
- **MST-based**: 2-approximation for metric TSP
- **PTAS**: For Euclidean TSP

## Runtime Analysis

### Brute Force Approach

**Algorithm:** Try all (n-1)!/2 tours (for symmetric TSP)
- **Time Complexity:** O(n! · n)
- **Space Complexity:** O(n) for storing current tour
- **Analysis:** For each permutation, compute tour cost (O(n) time)

### Dynamic Programming (Held-Karp Algorithm)

**Algorithm:** Bitmask DP
- **Time Complexity:** O(2^n · n^2)
- **Space Complexity:** O(2^n · n)
- **Subproblem:** dp[mask][v] = minimum cost to visit all vertices in mask ending at v
- **Recurrence:** dp[mask][v] = min_{u ∈ mask, u \neq v} {dp[mask \ {v}][u] + w(u,v)}
- **Final Answer:** min_v {dp[2^n-1][v] + w(v,0)}

### Branch-and-Bound

**Algorithm:** Systematic search with LP relaxation bounds
- **Time Complexity:** Exponential worst-case, but practical for medium instances
- **Space Complexity:** O(n) for recursion stack
- **Bounds:** Use LP relaxation or MST to get lower bounds

### Approximation Algorithms

**MST-Based 2-Approximation:**
- **Time Complexity:** O(n^2 log n) for MST + Eulerian tour + shortcutting
- **Space Complexity:** O(n)
- **Approximation Ratio:** 2

**Christofides 1.5-Approximation:**
- **Time Complexity:** O(n^3) (MST + matching + Eulerian tour)
- **Space Complexity:** O(n)
- **Approximation Ratio:** 1.5

### Heuristic Algorithms

**Nearest Neighbor:**
- **Time Complexity:** O(n^2)
- **Space Complexity:** O(n)
- **Quality:** No guarantee, but often within 25% of optimal

**2-Opt Local Search:**
- **Time Complexity:** O(n^2) per iteration, multiple iterations
- **Space Complexity:** O(n)
- **Improvement:** Can improve tours significantly

### Verification Complexity

**Given a candidate tour:**
- **Time Complexity:** O(n) - compute sum of edge weights
- **Space Complexity:** O(1) additional space
- This polynomial-time verifiability shows TSP is in NP

## Key Takeaways

1. **TSP is NP-Complete**: Proven by reduction from Hamiltonian Cycle
2. **Dynamic Programming**: Held-Karp algorithm solves in O(2^n · n^2) time
3. **Approximation**: 1.5-approximation (Christofides) and 2-approximation (MST-based) for metric TSP
4. **Practical Solvers**: Modern solvers can handle instances with thousands of cities
5. **Widespread Applications**: TSP appears in many real-world optimization problems

## Reduction Summary

**Hamiltonian Cycle ≤ₚ TSP:**
- Given Hamiltonian Cycle instance: graph G
- Create complete graph with weights: 1 if edge exists, 2 (or large) if not
- Hamiltonian cycle exists ↔ TSP tour of weight n exists

The reduction is polynomial-time, establishing TSP as NP-complete.

## Further Reading

- **Garey & Johnson**: "Computers and Intractability" - Standard reference
- **Held-Karp Algorithm**: Original paper on the DP approach
- **Christofides Algorithm**: The 1.5-approximation algorithm
- **Concorde TSP Solver**: State-of-the-art exact solver
- **Approximation Algorithms**: Vazirani's book covers TSP approximations

## Practice Problems

1. **Solve by hand**: For 4 cities in a square with side length 10, find the optimal TSP tour. What if cities form a rectangle?

2. **Prove the reduction**: Show that Hamiltonian Cycle reduces to TSP. Why do we set non-existent edges to weight 2 (or large)?

3. **DP implementation**: Implement the Held-Karp algorithm for TSP. Test it on small instances (up to n=15 or so).

4. **Approximation**: Implement the MST-based 2-approximation algorithm. Test it on metric TSP instances and compare to optimal.

5. **Time complexity**: Verify the O(2^n · n^2) time complexity of Held-Karp. Can you optimize the space complexity?

6. **Christofides**: Research and implement the Christofides algorithm. Why does it achieve 1.5-approximation?

7. **Local search**: Implement 2-opt local search for TSP. How does it compare to the approximation algorithms?

8. **Applications**: Research one real-world application of TSP. How is it formulated? What solving methods are used?

9. **Euclidean TSP**: Research why Euclidean TSP has a PTAS. What is the Arora-Mitchell algorithm?

10. **Asymmetric TSP**: How does ATSP differ from symmetric TSP? What approximation algorithms exist?

---

Understanding the Traveling Salesman Problem provides crucial insight into combinatorial optimization and the power of dynamic programming and approximation algorithms. TSP serves as a benchmark problem that has driven significant advances in algorithms and optimization techniques.

