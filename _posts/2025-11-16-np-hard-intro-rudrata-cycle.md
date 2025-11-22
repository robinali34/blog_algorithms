---
layout: post
title: "NP-Hard Introduction: Rudrata Cycle"
date: 2025-11-16
categories: [Algorithms, Complexity Theory, NP-Hard, Graph Theory]
excerpt: "An introduction to NP-hardness through the Rudrata Cycle Problem (also known as Hamiltonian Cycle), covering problem definition, NP-completeness proof, and connections to Rudrata Path and TSP."
---

## Introduction

The Rudrata Cycle Problem (also known as the Hamiltonian Cycle Problem) is one of the most fundamental graph problems in computer science. It asks whether a graph contains a cycle that visits each vertex exactly once. This problem is closely related to the Traveling Salesman Problem (TSP) and serves as a cornerstone for understanding NP-completeness in graph theory. The problem is named after Sir William Rowan Hamilton, who studied it in the context of the Icosian game.

## What is a Rudrata Cycle?

A **Rudrata cycle** (also called a **Hamiltonian cycle**) in an undirected graph is a cycle that visits each vertex exactly once and returns to the starting vertex. Unlike an Eulerian cycle (which visits each edge exactly once), a Hamiltonian cycle visits each vertex exactly once.

### Problem Definition

**Rudrata Cycle Decision Problem:**

**Input:** 
- An undirected graph G = (V, E)

**Output:** YES if G contains a Rudrata cycle (a cycle visiting every vertex exactly once), NO otherwise

**Rudrata Cycle Optimization Problem (TSP):**

**Input:** 
- A complete undirected graph G = (V, E) with edge weights w: E → ℝ⁺

**Output:** The minimum weight Rudrata cycle (this is the Traveling Salesman Problem)

### Example

Consider the following graph:

```
    1---2---3
    |   |   |
    4---5---6
```

**Rudrata Cycle:**
- Cycle: 1 to 2 to 5 to 4 to 1 ✗ (doesn't visit all vertices)
- Cycle: 1 to 2 to 3 to 6 to 5 to 4 to 1 ✓ (visits all 6 vertices exactly once)
- Cycle: 1 to 4 to 5 to 2 to 1 ✗ (doesn't visit vertices 3 and 6)

### Visual Example

A graph with a Rudrata cycle highlighted:

```
    1---2
    |\ /|
    | X |
    |/ \|
    3---4
```

- Rudrata cycle: 1 to 2 to 4 to 3 to 1 (visits all 4 vertices and returns to start)
- This is a complete graph K_4, so it has many Hamiltonian cycles

## Why Rudrata Cycle is in NP

To show that Rudrata Cycle is NP-complete, we first need to show it's in NP.

**Rudrata Cycle ∈ NP:**

Given a candidate solution (a sequence of vertices representing a cycle), we can verify in polynomial time:
1. Check that the cycle has exactly |V| vertices: O(|V|) time
2. Check that each vertex appears exactly once: O(|V|) time (use a set or array)
3. Check that consecutive vertices in the cycle are adjacent: O(|V|) time (check |V| edges, including the wrap-around edge from last to first)
4. Check that the cycle returns to start: O(1) time

Total verification time: O(|V|), which is polynomial in the input size. Therefore, Rudrata Cycle is in NP.

## NP-Completeness: Reduction from 3-SAT

The standard proof that Rudrata Cycle is NP-complete reduces directly from 3-SAT.

### Construction

For a 3-SAT formula φ = C_1 ∧ C_2 ∧ … ∧ C_m with variables x₁, x₂, …, x_n:

**Key Idea:** Create a graph where a Rudrata cycle corresponds to a satisfying assignment.

1. **For each variable x_i:**
   - Create a "variable gadget": a row of vertices representing the variable
   - Typically: vertices arranged horizontally, where going "left" means x_i = TRUE and going "right" means x_i = FALSE
   - Example: For variable x_i, create vertices v_{i,1}, v_{i,2}, …, v_{i,k} where the cycle can traverse left-to-right (TRUE) or right-to-left (FALSE)

2. **For each clause C_j:**
   - Create a "clause gadget": vertices that can be visited if the clause is satisfied
   - Connect clause vertices to variable vertices corresponding to literals in the clause
   - The cycle must visit clause vertices, which is only possible if at least one literal is true

3. **Connect gadgets:**
   - Chain variable gadgets together in sequence
   - Connect clause gadgets to appropriate variable positions
   - Ensure a Rudrata cycle visits all vertices exactly once

### Simplified Construction Example

A common construction uses:

**Variable Gadget:**
- For each variable x_i, create a horizontal chain of vertices
- The cycle can traverse this chain in two ways (encoding TRUE/FALSE)

**Clause Gadget:**
- For each clause C_j = (l_1 ∨ l_2 ∨ l_3), create vertices connected to the variable gadgets
- If the cycle takes the path corresponding to a true literal, it can visit the clause vertex

**Why This Works:**

**Forward Direction (3-SAT satisfiable → Rudrata Cycle exists):**
- If φ is satisfiable, construct cycle through variable gadgets based on assignment
- Visit clause vertices for satisfied clauses
- This gives a valid Rudrata cycle

**Reverse Direction (Rudrata Cycle exists → 3-SAT satisfiable):**
- Extract truth assignment from cycle's path through variable gadgets
- Since all clause vertices are visited, each clause has at least one true literal
- This gives a satisfying assignment

## Relationship to Other Problems

The Rudrata Cycle Problem is closely related to several important problems:

### Rudrata Path

As we saw in the previous post:
- **Rudrata Cycle ≤ₚ Rudrata Path**: Break cycle into path
- **Rudrata Path ≤ₚ Rudrata Cycle**: Connect path ends to form cycle
- They are polynomially equivalent

### Traveling Salesman Problem (TSP)

**TSP Decision Problem:**
- Given complete graph with edge weights and bound B
- Does there exist a Rudrata cycle with total weight ≤ B?

**Relationship:**
- TSP is a weighted version of Rudrata Cycle
- Rudrata Cycle reduces to TSP: set all edge weights to 1, ask if cycle of weight |V| exists
- TSP reduces to Rudrata Cycle: use unweighted version
- They are essentially the same problem

### Eulerian Cycle

**Eulerian Cycle:**
- Visits each **edge** exactly once (vs. Hamiltonian cycle visits each **vertex** exactly once)

**Key Difference:**
- Eulerian cycle: polynomial-time solvable (check degrees, use Fleury's or Hierholzer's algorithm)
- Hamiltonian cycle: NP-complete
- This demonstrates how a small change in problem statement can dramatically affect complexity

## Practical Implications

### Why Rudrata Cycle is Hard

The Rudrata Cycle Problem is NP-complete, which means:

1. **No Known Polynomial-Time Algorithm**: Best known algorithms have exponential time complexity
2. **Brute Force**: Try all (n-1)!/2 possible cycles (for undirected graphs) - factorial time
3. **Dynamic Programming**: Can solve in O(2^n · n^2) time using bitmask DP (similar to TSP)
4. **Backtracking**: Practical for small instances, but still exponential worst-case

### Dynamic Programming Solution

**Subproblem:** dp[mask][v] = true if there exists a path visiting all vertices in mask ending at vertex v, and this path can be extended to a cycle.

**Recurrence:**
- Base case: dp[2^i][i] = true for all i (path of length 1)
- Recurrence: dp[mask][v] = bigvee_{u in mask, (u,v) in E} dp[mask setminus {v}][u]
- Final check: For each v, check if dp[2^n-1][v] = true and (v, start) is an edge

**Algorithm:**
```
Algorithm: RudrataCycleDP(G)
1. n = |V|
2. Let dp[0..2^n-1][0..n-1] be a boolean array
3. for i = 0 to n-1:
4.     dp[2^i][i] = true
5. for mask = 1 to 2^n - 1:
6.     for v = 0 to n-1:
7.         if v in mask:
8.             for each neighbor u of v:
9.                 if u in mask:
10.                    dp[mask][v] = dp[mask][v] OR dp[mask - 2^v][u]
11. for v = 0 to n-1:
12.     if dp[2^n - 1][v] AND (v, start) is an edge:
13.         return true
14. return false
```

**Time Complexity:** O(2^n · n^2)
**Space Complexity:** O(2^n · n)

## Runtime Analysis

### Brute Force Approach

**Algorithm:** Try all (n-1)!/2 possible cycles (for undirected graphs)
- **Time Complexity:** O((n-1)! · n) = O(n!)
- **Space Complexity:** O(n) for storing current cycle
- **Analysis:** For each permutation, verify it forms a valid cycle (O(n) checks including wrap-around)

### Dynamic Programming (Held-Karp Algorithm)

**Algorithm:** Bitmask DP as described above
- **Time Complexity:** O(2^n · n^2)
- **Space Complexity:** O(2^n · n)
- **Subproblem:** dp[mask][v] = true if path exists visiting all vertices in mask ending at v
- **Final Check:** Verify edge exists from last vertex to start vertex

### Backtracking

**Algorithm:** Systematic search with pruning
- **Time Complexity:** Exponential worst-case, improved with pruning
- **Space Complexity:** O(n) for recursion stack
- **Pruning:** Stop if current path can't form a cycle visiting all vertices

### Special Cases

**Complete Graphs:**
- **Time Complexity:** O(1) - trivial, any cycle works

**Trees:**
- **Time Complexity:** O(n) - check if tree has exactly n-1 edges (then no cycle possible unless n=2)

### Verification Complexity

**Given a candidate cycle:**
- **Time Complexity:** O(n) - verify all n edges exist (including wrap-around)
- **Space Complexity:** O(1) additional space
- This polynomial-time verifiability shows Rudrata Cycle is in NP

### Real-World Applications

Rudrata Cycle has numerous applications:

1. **Route Planning**: Finding routes that visit all locations and return to start (TSP)
2. **Circuit Design**: Designing circuits that visit all components
3. **DNA Sequencing**: Finding cycles in overlap graphs
4. **Network Analysis**: Analyzing connectivity and designing network topologies
5. **Game Theory**: Solving puzzles and games
6. **Scheduling**: Sequencing tasks with return constraints
7. **Logistics**: Vehicle routing, delivery optimization

### Special Cases

Some restricted versions of Rudrata Cycle are tractable:

- **Complete Graphs**: Always has a Rudrata cycle (any permutation works)
- **Dirac's Theorem**: If deg(v) ≥ n/2 for all vertices, then Hamiltonian cycle exists (but finding it is still hard)
- **Ore's Theorem**: If deg(u) + deg(v) ≥ n for all non-adjacent u,v, then Hamiltonian cycle exists
- **Grid Graphs**: Can be solved efficiently for certain grid structures
- **Bounded Treewidth**: Can be solved efficiently using tree decomposition

## Key Takeaways

1. **Rudrata Cycle is NP-Complete**: Proven by reduction from 3-SAT
2. **TSP Connection**: Traveling Salesman Problem is essentially weighted Rudrata Cycle
3. **Path vs Cycle**: Rudrata Cycle and Rudrata Path are polynomially equivalent
4. **Dynamic Programming**: O(2^n · n^2) time solution using bitmask DP works for small graphs
5. **Practical Algorithms**: Despite NP-completeness, DP and heuristics work well for many practical instances

## Reduction Summary

**3-SAT ≤ₚ Rudrata Cycle:**
- Construct graph with variable and clause gadgets
- Satisfying assignment ↔ Rudrata cycle
- The construction ensures cycle visits all vertices exactly once

**Rudrata Cycle ≤ₚ TSP:**
- Given Rudrata Cycle instance: graph G
- Create complete graph G' with edge weights: 1 if edge exists in G, ∈fty otherwise
- Rudrata cycle exists ↔ TSP has solution of weight |V|

**TSP ≤ₚ Rudrata Cycle:**
- Given TSP instance, ask if unweighted version has Hamiltonian cycle
- They are essentially equivalent

All reductions are polynomial-time, establishing Rudrata Cycle as NP-complete.

## Further Reading

- **Garey & Johnson**: "Computers and Intractability" - Standard reference for NP-completeness proofs
- **Hamilton's Icosian Game**: Historical context of the problem
- **TSP**: Understanding the relationship to Traveling Salesman Problem
- **Dynamic Programming**: CLRS covers bitmask DP techniques for TSP
- **Graph Theory**: Dirac's and Ore's theorems provide sufficient conditions

## Practice Problems

1. **Find all Rudrata cycles**: For the complete graph K_4 with vertices {1,2,3,4}, list all distinct Rudrata cycles. How many are there? (Consider cycles equivalent if they're rotations or reversals)

2. **Prove the reduction**: Show that Rudrata Cycle reduces to TSP. What edge weights should you use?

3. **DP implementation**: Implement the dynamic programming algorithm for Rudrata Cycle. Test it on small graphs and analyze its performance.

4. **Path to Cycle**: Show that Rudrata Path reduces to Rudrata Cycle. How do you modify the graph?

5. **Time complexity analysis**: For the DP algorithm, verify the O(2^n · n^2) time complexity. Can you optimize the space complexity?

6. **Special cases**: Research Dirac's theorem. If a graph satisfies the conditions, does that mean we can find a Hamiltonian cycle in polynomial time?

7. **Eulerian vs Hamiltonian**: Explain the key difference between Eulerian cycles and Hamiltonian cycles. Why is one polynomial-time and the other NP-complete?

8. **Extension**: Research approximation algorithms for TSP. What approximation ratios can be achieved? What about metric TSP?

---

Understanding the Rudrata Cycle Problem provides crucial insight into cycle problems in graph theory and their connection to optimization problems like TSP. The relationship to 3-SAT demonstrates how logical constraints can be encoded as graph structures.

