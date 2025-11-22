---
layout: post
title: "NP-Hard Introduction: Rudrata Path and Rudrata (s,t)-Path"
date: 2025-11-16
categories: [Algorithms, Complexity Theory, NP-Hard, Graph Theory]
excerpt: "An introduction to NP-hardness through the Rudrata Path and Rudrata (s,t)-Path problems, covering problem definitions, NP-completeness proofs, and connections to Hamiltonian Cycle."
---

## Introduction

The Rudrata Path Problem (also known as the Hamiltonian Path Problem) is a fundamental graph problem that asks whether a graph contains a path visiting every vertex exactly once. The Rudrata (s,t)-Path Problem is a variant that requires the path to start at a specific vertex s and end at a specific vertex t. These problems are closely related to the Hamiltonian Cycle Problem and serve as excellent examples of NP-completeness in graph theory.

## What is a Rudrata Path?

A **Rudrata path** (also called a **Hamiltonian path**) in an undirected graph is a path that visits each vertex exactly once. Unlike a cycle, a path doesn't need to return to the starting vertex.

### Problem Definitions

**Rudrata Path Decision Problem:**

**Input:** 
- An undirected graph G = (V, E)

**Output:** YES if G contains a Rudrata path (a path visiting every vertex exactly once), NO otherwise

**Rudrata (s,t)-Path Decision Problem:**

**Input:** 
- An undirected graph G = (V, E)
- Two vertices s, t ∈ V

**Output:** YES if G contains a Rudrata path starting at s and ending at t, NO otherwise

### Example

Consider the following graph:

```
    1---2---3
    |   |   |
    4---5---6
```

**Rudrata Path:**
- Path: 1 → 4 → 5 → 2 → 3 → 6 ✓ (visits all 6 vertices exactly once)
- Path: 1 → 2 → 5 → 4 ✗ (doesn't visit all vertices)
- Path: 1 → 2 → 3 → 2 ✗ (visits vertex 2 twice)

**Rudrata (1,6)-Path:**
- Path: 1 → 4 → 5 → 2 → 3 → 6 ✓ (starts at 1, ends at 6, visits all vertices)
- Path: 1 → 2 → 5 → 4 → 3 → 6 ✓ (another valid (1,6)-path)

### Visual Example

A graph with a Rudrata path highlighted:

```
    1---2
    |\ /|
    | X |
    |/ \|
    3---4
```

- Rudrata path: 1 → 2 → 4 → 3 (visits all 4 vertices)
- Note: This graph also has a Hamiltonian cycle: 1 → 2 → 4 → 3 → 1

## Why Rudrata Path is in NP

To show that Rudrata Path is NP-complete, we first need to show it's in NP.

**Rudrata Path ∈ NP:**

Given a candidate solution (a sequence of vertices representing a path), we can verify in polynomial time:
1. Check that the path has exactly `|V|` vertices: O(`|V|`) time
2. Check that each vertex appears exactly once: O(`|V|`) time (use a set or array)
3. Check that consecutive vertices in the path are adjacent: O(`|V|`) time (check `|V|`-1 edges)

Total verification time: O(`|V|`), which is polynomial in the input size. Therefore, Rudrata Path is in NP.

Similarly, **Rudrata (s,t)-Path ∈ NP** with the additional checks:
- First vertex is s
- Last vertex is t

## NP-Completeness: Reduction from Hamiltonian Cycle

The most elegant proof that Rudrata Path is NP-complete reduces from the **Hamiltonian Cycle Problem**.

### Hamiltonian Cycle Problem

**Hamiltonian Cycle:**
- **Input:** An undirected graph G = (V, E)
- **Output:** YES if G contains a cycle visiting every vertex exactly once, NO otherwise

Hamiltonian Cycle is known to be NP-complete (can be proven by reduction from 3-SAT).

### Reduction from Hamiltonian Cycle to Rudrata Path

**Reduction:**
1. Given a Hamiltonian Cycle instance: graph G = (V, E)
2. Pick an arbitrary vertex v ∈ V
3. Create graph G' by:
   - Adding a new vertex v' (copy of v)
   - Connecting v' to all neighbors of v
   - Removing vertex v (or equivalently, work with G' having v' instead)
4. Return Rudrata Path instance: graph G'

**Alternative Simpler Reduction:**
1. Given Hamiltonian Cycle instance: graph G
2. Pick an arbitrary edge (u, v) ∈ E
3. Create graph G' by removing edge (u, v) and adding two new vertices s and t
4. Connect s only to u and t only to v
5. Return Rudrata (s,t)-Path instance: graph G' with start s and end t

**Correctness:**
- If G has a Hamiltonian cycle, we can break it at edge (u,v) to get a path from u to v visiting all original vertices
- Add s at the start and t at the end to get a Rudrata (s,t)-path
- If G' has a Rudrata (s,t)-path, it must go s → u → … → v → t
- Removing s and t and adding edge (u,v) gives a Hamiltonian cycle in G

**Polynomial Time:**
- The reduction takes O(`|V|` + `|E|`) time

Therefore, **Rudrata Path and Rudrata (s,t)-Path are NP-complete**.

## Alternative Reduction: Direct from 3-SAT

We can also prove Rudrata (s,t)-Path is NP-complete by directly reducing from 3-SAT, similar to other graph problems.

### Construction

For a 3-SAT formula φ = C₁ ∧ C₂ ∧ … ∧ Cₘ with variables x₁, x₂, …, xₙ:

**Key Idea:** Create a graph where a Rudrata (s,t)-path corresponds to a satisfying assignment.

1. **For each variable xᵢ:**
   - Create a "variable gadget": a path with vertices representing choosing xᵢ = TRUE or xᵢ = FALSE
   - Typically: vertices vᵢ,₁, vᵢ,₂, …, vᵢ,ₖ where the path can go "left" (TRUE) or "right" (FALSE)

2. **For each clause Cⱼ:**
   - Create a "clause gadget": vertices that can be visited if the clause is satisfied
   - Connect clause vertices to variable vertices corresponding to literals in the clause

3. **Connect gadgets:**
   - Chain variable gadgets together
   - Connect clause gadgets appropriately
   - Ensure a Rudrata (s,t)-path visits all vertices exactly once

4. **Set s and t:** Start and end vertices of the constructed graph

### Why This Works

**Intuition:**
- A Rudrata (s,t)-path must visit all vertices exactly once
- The path through variable gadgets encodes a truth assignment
- The path can only visit clause vertices if corresponding literals are true
- A valid path exists if and only if the formula is satisfiable

**Formal Proof:**

**Forward Direction (3-SAT satisfiable → Rudrata (s,t)-Path exists):**
- If φ is satisfiable, construct path through variable gadgets based on assignment
- Visit clause vertices for satisfied clauses
- This gives a valid Rudrata (s,t)-path

**Reverse Direction (Rudrata (s,t)-Path exists → 3-SAT satisfiable):**
- Extract truth assignment from path through variable gadgets
- Since all clause vertices are visited, each clause has at least one true literal
- This gives a satisfying assignment

## Relationship Between Rudrata Path Variants

### Rudrata Path vs Rudrata (s,t)-Path

**Rudrata (s,t)-Path ≤ₚ Rudrata Path:**
- Given Rudrata (s,t)-Path instance: graph G, vertices s and t
- Add two new vertices s' and t'
- Connect s' only to s and t' only to t
- Return Rudrata Path instance: modified graph
- Rudrata (s,t)-path exists ↔ Rudrata path exists (must start at s' and end at t')

**Rudrata Path ≤ₚ Rudrata (s,t)-Path:**
- Given Rudrata Path instance: graph G
- Pick arbitrary vertices s and t
- Try all pairs (s,t) or use a more clever reduction
- Actually, we can reduce by trying all pairs, but this is not polynomial
- Better: Add two vertices connected only to all original vertices, then require path between them

### Relationship to Hamiltonian Cycle

As we saw:
- **Hamiltonian Cycle ≤ₚ Rudrata Path**: Break cycle into path
- **Rudrata Path ≤ₚ Hamiltonian Cycle**: Add edges to connect path ends

They are polynomially equivalent.

## Practical Implications

### Why Rudrata Path is Hard

The Rudrata Path Problem is NP-complete, which means:

1. **No Known Polynomial-Time Algorithm**: Best known algorithms have exponential time complexity
2. **Brute Force**: Try all n! permutations of vertices - factorial time
3. **Dynamic Programming**: Can solve in O(2ⁿ · n²) time using bitmask DP (similar to TSP)
4. **Backtracking**: Practical for small instances, but still exponential worst-case

### Dynamic Programming Solution

**Subproblem:** dp[mask][v] = true if there exists a path visiting all vertices in mask ending at vertex v.

**Recurrence:**
- Base case: dp[2ⁱ][i] = true for all i (path of length 1)
- Recurrence: dp[mask][v] = ∨(u ∈ mask, (u,v) ∈ E) dp[mask \ {v}][u]
  - Path ending at v visiting vertices in mask exists if there's a neighbor u such that path ending at u visiting mask \ {v} exists

**Algorithm:**
```
Algorithm: RudrataPathDP(G)
1. n = `|V|`
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
12.     if dp[2^n - 1][v]:
13.         return true
14. return false
```

**Time Complexity:** O(2ⁿ · n²)
**Space Complexity:** O(2ⁿ · n)

## Runtime Analysis

### Brute Force Approach

**Algorithm:** Try all n! permutations of vertices
- **Time Complexity:** O(n! · n)
- **Space Complexity:** O(n) for storing current path
- **Analysis:** For each permutation, verify it forms a valid path (O(n) checks)

### Dynamic Programming (Held-Karp Style)

**Algorithm:** Bitmask DP as described above
- **Time Complexity:** O(2ⁿ · n²)
- **Space Complexity:** O(2ⁿ · n) (can be optimized with careful implementation)
- **Subproblem:** dp[mask][v] = true if path exists visiting all vertices in mask ending at v
- **Recurrence:** dp[mask][v] = ∨(u ∈ mask, (u,v) ∈ E) dp[mask \ {v}][u]

### Backtracking

**Algorithm:** Systematic search with pruning
- **Time Complexity:** Exponential worst-case, but better than brute force with pruning
- **Space Complexity:** O(n) for recursion stack
- **Pruning:** Stop if current path can't be extended to visit all vertices

### Special Cases

**Trees:**
- **Time Complexity:** O(n) - any path covering all vertices works
- **Space Complexity:** O(n)

**Complete Graphs:**
- **Time Complexity:** O(1) - any permutation works, trivial to find

### Verification Complexity

**Given a candidate path:**
- **Time Complexity:** O(n) - verify all n-1 edges exist and all vertices appear once
- **Space Complexity:** O(1) additional space
- This polynomial-time verifiability shows Rudrata Path is in NP

### Real-World Applications

Rudrata Path has numerous applications:

1. **Route Planning**: Finding routes that visit all locations exactly once
2. **Circuit Design**: Designing circuits that visit all components
3. **DNA Sequencing**: Finding paths through overlap graphs
4. **Network Analysis**: Analyzing connectivity and reachability
5. **Game Theory**: Solving puzzles like the "traveling" variants
6. **Scheduling**: Sequencing tasks with dependencies

### Special Cases

Some restricted versions of Rudrata Path are tractable:

- **Trees**: Always has a Rudrata path (any path covering all vertices)
- **Complete Graphs**: Always has a Rudrata path (any permutation works)
- **Path Graphs**: Trivial (the graph itself is a Rudrata path)
- **Grid Graphs**: Can be solved efficiently for certain grid structures
- **Bounded Treewidth**: Can be solved efficiently using tree decomposition

## Key Takeaways

1. **Rudrata Path is NP-Complete**: Proven by reduction from Hamiltonian Cycle or directly from 3-SAT
2. **Two Variants**: Rudrata Path (any start/end) and Rudrata (s,t)-Path (fixed start/end) are polynomially equivalent
3. **Hamiltonian Cycle Connection**: Rudrata Path and Hamiltonian Cycle are closely related and polynomially equivalent
4. **Dynamic Programming**: O(2ⁿ · n²) time solution using bitmask DP works for small graphs
5. **Practical Algorithms**: Despite NP-completeness, DP and backtracking work well for many practical instances

## Reduction Summary

**Hamiltonian Cycle ≤ₚ Rudrata (s,t)-Path:**
- Given Hamiltonian Cycle instance: graph G
- Pick edge (u,v), remove it, add vertices s and t
- Connect s to u and t to v
- Hamiltonian cycle exists ↔ Rudrata (s,t)-path exists

**Rudrata (s,t)-Path ≤ₚ Rudrata Path:**
- Add vertices s' and t' connected only to s and t respectively
- Rudrata (s,t)-path exists ↔ Rudrata path exists (must use s' and t')

**3-SAT ≤ₚ Rudrata (s,t)-Path:**
- Construct graph with variable and clause gadgets
- Satisfying assignment ↔ Rudrata (s,t)-path

All reductions are polynomial-time, establishing both problems as NP-complete.

## Further Reading

- **Garey & Johnson**: "Computers and Intractability" - Standard reference for NP-completeness proofs
- **Hamiltonian Cycle**: Understanding the relationship between path and cycle problems
- **Dynamic Programming**: CLRS covers bitmask DP techniques for TSP and related problems
- **Graph Algorithms**: Books on graph algorithms cover special cases and heuristics

## Practice Problems

1. **Find a Rudrata path**: For the graph with vertices {1,2,3,4} and edges {(1,2), (2,3), (3,4), (1,4)}, find all Rudrata paths. How many are there?

2. **Prove the reduction**: Show that Hamiltonian Cycle reduces to Rudrata (s,t)-Path. Can you also show the reverse reduction?

3. **DP implementation**: Implement the dynamic programming algorithm for Rudrata Path. Test it on small graphs and analyze its performance.

4. **Modify for (s,t)-path**: Modify the DP algorithm to find a Rudrata (s,t)-path. What changes are needed?

5. **Time complexity analysis**: For the DP algorithm, verify the O(2ⁿ · n²) time complexity. What is the space complexity?

6. **Special cases**: Prove that every tree has a Rudrata path. What about complete graphs?

7. **Reduction practice**: Show that Rudrata (s,t)-Path reduces to Rudrata Path. Is the reverse reduction also polynomial-time?

8. **Extension**: Research the Traveling Salesman Problem (TSP). How does it relate to Rudrata Path? Can you reduce one to the other?

---

Understanding the Rudrata Path Problem and its variants provides crucial insight into path and cycle problems in graph theory. The connection to Hamiltonian Cycle and the dynamic programming solution demonstrate important techniques for handling NP-complete graph problems.

