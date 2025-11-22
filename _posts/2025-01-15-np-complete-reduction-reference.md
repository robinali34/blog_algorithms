---
layout: post
title: "NP-Complete Reduction Reference: Using Known Problems to Prove NP-Completeness"
date: 2025-01-15
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive reference guide showing how to use known NP-complete problems (SAT, 3-SAT, Clique, Independent Set, Vertex Cover, Subset Sum, Rudrata Path/Cycle, ILP, ZOE, 3D Matching, TSP) to prove new problems are NP-complete through reductions."
---

## Introduction

Once we have a collection of known NP-complete problems, we can use any of them as a starting point for proving new problems are NP-complete. This post provides a comprehensive reference showing which known NP-complete problems are best suited for reducing to different types of problems, along with reduction chains and examples.

## Known NP-Complete Problems Reference

### 1. SAT (Boolean Satisfiability)

**Problem:** Given a Boolean formula, is it satisfiable?

**Best for reducing to:**
- Problems involving logical constraints
- Decision problems with yes/no answers
- Problems that naturally encode Boolean logic

**Common reductions from SAT:**
- SAT → 3-SAT (standard transformation)
- SAT → Integer Linear Programming
- SAT → Zero-One Equations

### 2. 3-SAT (3-CNF Satisfiability)

**Problem:** Given a Boolean formula in 3-CNF, is it satisfiable?

**Best for reducing to:**
- Graph problems (Independent Set, Vertex Cover, Clique)
- Set problems (Set Cover, Exact Cover)
- Optimization problems (ILP, ZOE)
- Path/cycle problems (Rudrata Path, Rudrata Cycle)

**Why it's popular:**
- First problem proven NP-complete (Cook-Levin)
- Simple structure (variables and clauses)
- Easy to encode constraints

**Common reductions from 3-SAT:**
- 3-SAT → Independent Set
- 3-SAT → Vertex Cover
- 3-SAT → Clique
- 3-SAT → 3D Matching
- 3-SAT → Subset Sum
- 3-SAT → Integer Linear Programming
- 3-SAT → Zero-One Equations

### 3. Clique

**Problem:** Given graph G and integer k, does G have a clique of size ≥ k?

**Best for reducing to:**
- Other graph problems
- Problems involving "all pairs" constraints
- Dense subgraph problems

**Common reductions from Clique:**
- Clique → Independent Set (via complement graph)
- Clique → Vertex Cover (via complement + IS relationship)
- Clique → Subgraph Isomorphism
- Clique → Maximum Common Subgraph

**Reduction chain:** 3-SAT → Independent Set → Clique

### 4. Independent Set (IS)

**Problem:** Given graph G and integer k, does G have an independent set of size ≥ k?

**Best for reducing to:**
- Graph problems with "no edges" constraints
- Problems involving mutually exclusive choices
- Packing problems

**Common reductions from Independent Set:**
- Independent Set → Clique (via complement graph)
- Independent Set → Vertex Cover (complement relationship)
- Independent Set → Maximum Cut
- Independent Set → Graph Coloring

**Reduction chain:** 3-SAT → Independent Set

### 5. Vertex Cover (VC)

**Problem:** Given graph G and integer k, does G have a vertex cover of size ≤ k?

**Best for reducing to:**
- Covering problems
- Set Cover variants
- Dominating Set problems

**Common reductions from Vertex Cover:**
- Vertex Cover → Set Cover
- Vertex Cover → Hitting Set
- Vertex Cover → Dominating Set
- Vertex Cover → Feedback Vertex Set

**Reduction chain:** 3-SAT → Independent Set → Vertex Cover

### 6. Subset Sum (SSS)

**Problem:** Given set S of integers and target t, does there exist subset S' ⊆ S with sum exactly t?

**Best for reducing to:**
- Partition problems
- Knapsack variants
- Number problems
- Scheduling with constraints

**Common reductions from Subset Sum:**
- Subset Sum → Partition
- Subset Sum → Knapsack
- Subset Sum → Bin Packing
- Subset Sum → Scheduling problems

**Reduction chain:** 3-SAT → Subset Sum

### 7. Rudrata Path

**Problem:** Given graph G, does G have a path visiting every vertex exactly once?

**Best for reducing to:**
- Path problems
- Routing problems
- Sequencing problems

**Common reductions from Rudrata Path:**
- Rudrata Path → Rudrata (s,t)-Path
- Rudrata Path → Longest Path
- Rudrata Path → Graph Bandwidth

**Reduction chain:** Hamiltonian Cycle → Rudrata Path

### 8. Rudrata (s,t)-Path

**Problem:** Given graph G and vertices s, t, does G have a path from s to t visiting every vertex exactly once?

**Best for reducing to:**
- Constrained path problems
- Problems with fixed start/end points
- Routing with endpoints

**Common reductions from Rudrata (s,t)-Path:**
- Rudrata (s,t)-Path → Rudrata Path
- Rudrata (s,t)-Path → Longest (s,t)-Path
- Rudrata (s,t)-Path → Graph Traversal problems

**Reduction chain:** Hamiltonian Cycle → Rudrata (s,t)-Path → Rudrata Path

### 9. Rudrata Cycle (Hamiltonian Cycle)

**Problem:** Given graph G, does G have a cycle visiting every vertex exactly once?

**Best for reducing to:**
- Cycle problems
- TSP (unweighted version)
- Tour problems

**Common reductions from Rudrata Cycle:**
- Rudrata Cycle → Traveling Salesman Problem
- Rudrata Cycle → Rudrata Path
- Rudrata Cycle → Longest Cycle
- Rudrata Cycle → Graph Hamiltonicity variants

**Reduction chain:** 3-SAT → Rudrata Cycle

### 10. Integer Linear Programming (ILP)

**Problem:** Given linear constraints and objective, does there exist integer solution satisfying constraints?

**Best for reducing to:**
- Optimization problems with integer constraints
- Scheduling problems
- Resource allocation problems

**Common reductions from ILP:**
- ILP → 0-1 ILP (Binary ILP)
- ILP → Knapsack
- ILP → Set Cover (via 0-1 ILP)
- ILP → Scheduling problems

**Reduction chain:** 3-SAT → Integer Linear Programming

### 11. Zero-One Equations (ZOE)

**Problem:** Given matrix A and vector b, does there exist 0-1 vector x such that Ax = b?

**Best for reducing to:**
- Exact cover problems
- Matching problems
- Problems with binary constraints

**Common reductions from ZOE:**
- ZOE → 3D Matching
- ZOE → Exact Cover
- ZOE → Set Partitioning
- ZOE → Integer Linear Programming

**Reduction chain:** 3-SAT → Zero-One Equations

### 12. 3D Matching

**Problem:** Given sets X, Y, Z and triples T ⊆ X × Y × Z, does there exist matching covering all elements?

**Best for reducing to:**
- Matching problems
- Covering problems
- Assignment problems

**Common reductions from 3D Matching:**
- 3D Matching → Set Cover
- 3D Matching → Exact Cover
- 3D Matching → Assignment problems

**Reduction chain:** 3-SAT → 3D Matching

### 13. Traveling Salesman Problem (TSP)

**Problem:** Given complete graph with edge weights and bound B, does there exist tour of weight ≤ B?

**Best for reducing to:**
- Routing problems
- Tour problems
- Sequencing problems with costs

**Common reductions from TSP:**
- TSP → Metric TSP (restriction)
- TSP → Vehicle Routing
- TSP → Job Sequencing

**Reduction chain:** Hamiltonian Cycle → TSP

## Reduction Chains and Relationships

### Primary Chain: SAT → 3-SAT → Everything

```
SAT
 └─→ 3-SAT
     ├─→ Independent Set
     │   ├─→ Clique (complement graph)
     │   └─→ Vertex Cover (complement relationship)
     │       └─→ Set Cover
     ├─→ 3D Matching
     │   └─→ Set Cover
     ├─→ Subset Sum
     │   ├─→ Partition
     │   └─→ Knapsack
     ├─→ Integer Linear Programming
     │   └─→ 0-1 ILP
     ├─→ Zero-One Equations
     │   └─→ 3D Matching
     └─→ Rudrata Cycle
         ├─→ Rudrata (s,t)-Path
         │   └─→ Rudrata Path
         └─→ TSP
```

### Graph Problem Chain

```
3-SAT
 └─→ Independent Set
     ├─→ Clique (G̅)
     └─→ Vertex Cover (V \ S)
         └─→ Dominating Set
```

### Set Problem Chain

```
3-SAT
 └─→ 3D Matching
     └─→ Set Cover
         └─→ Hitting Set
```

### Optimization Problem Chain

```
3-SAT
 └─→ Integer Linear Programming
     └─→ 0-1 ILP
         └─→ Knapsack
```

## Choosing the Right Starting Point

### Use 3-SAT when:
- Target problem has logical constraints
- Need to encode "at least one" or "exactly one" constraints
- Problem involves choices or assignments
- **Examples:** Graph problems, set problems, optimization problems

### Use Independent Set when:
- Target problem involves selecting non-conflicting elements
- Problem has "mutually exclusive" constraints
- Need to encode "no edges" or "no conflicts"
- **Examples:** Clique, Maximum Cut, Graph Coloring

### Use Vertex Cover when:
- Target problem involves covering elements
- Problem has "every element must be covered" constraints
- Need to encode covering relationships
- **Examples:** Set Cover, Hitting Set, Dominating Set

### Use Subset Sum when:
- Target problem involves numbers or weights
- Problem has sum/total constraints
- Need to encode "exactly" or "at most" constraints with numbers
- **Examples:** Partition, Knapsack, Bin Packing

### Use Rudrata Cycle/Path when:
- Target problem involves paths or tours
- Problem requires visiting all elements
- Need to encode ordering or sequencing
- **Examples:** TSP variants, Longest Path, Graph Traversal

### Use Integer Linear Programming when:
- Target problem has linear constraints
- Problem involves optimization with integer variables
- Need to encode multiple constraints simultaneously
- **Examples:** Scheduling, Resource Allocation, Network Design

### Use 3D Matching when:
- Target problem involves matching or assignment
- Problem has triple constraints
- Need to encode "exactly one" constraints with three sets
- **Examples:** Set Cover, Exact Cover, Assignment problems

## Detailed Reduction Examples

### Example 1: Using 3-SAT → Independent Set → Clique

**Prove Clique is NP-complete:**

1. **Clique ∈ NP:** Given set of k vertices, verify all pairs are connected: O(k²) time

2. **Independent Set ≤ₚ Clique:**
   - Given Independent Set instance: graph G, integer k
   - Create complement graph G̅
   - Return Clique instance: graph G̅, integer k
   - G has independent set of size k ↔ G̅ has clique of size k

3. **Polynomial time:** Creating complement graph takes O(|V|²) time

**Therefore, Clique is NP-complete.**

### Example 2: Using 3-SAT → Vertex Cover → Set Cover

**Prove Set Cover is NP-complete:**

1. **Set Cover ∈ NP:** Given collection of sets, verify they cover universe: O(nm) time

2. **Vertex Cover ≤ₚ Set Cover:**
   - Given Vertex Cover instance: graph G = (V, E), integer k
   - Create Set Cover instance:
     - Universe U = E (all edges)
     - For each vertex v ∈ V, create set Sᵥ = {e ∈ E : v ∈ e} (edges incident to v)
     - k' = k
   - G has vertex cover of size k ↔ Sets cover universe with k sets

3. **Polynomial time:** O(|V| + |E|) time

**Therefore, Set Cover is NP-complete.**

### Example 3: Using 3-SAT → Subset Sum → Partition

**Prove Partition is NP-complete:**

1. **Partition ∈ NP:** Given partition, verify sums are equal: O(n) time

2. **Subset Sum ≤ₚ Partition:**
   - Given Subset Sum instance: set S, target t
   - Let T = sum of all elements in S
   - Create Partition instance: set S' = S ∪ {2T - t, T + t}
   - S has subset summing to t ↔ S' can be partitioned into equal sums
   - If subset sums to t, partition: {subset, 2T-t} and {complement, T+t}
   - Both sum to 2T

3. **Polynomial time:** O(n) time

**Therefore, Partition is NP-complete.**

### Example 4: Using 3-SAT → Rudrata Cycle → TSP

**Prove TSP is NP-complete:**

1. **TSP ∈ NP:** Given tour, verify it visits all vertices and sum weights: O(n) time

2. **Rudrata Cycle ≤ₚ TSP:**
   - Given Rudrata Cycle instance: graph G = (V, E)
   - Create complete graph G' with same vertices
   - Set edge weights: w(u,v) = 1 if (u,v) ∈ E, else w(u,v) = 2
   - Set bound B = |V|
   - G has Hamiltonian cycle ↔ G' has TSP tour of weight |V|

3. **Polynomial time:** O(|V|²) time

**Therefore, TSP is NP-complete.**

### Example 5: Using 3-SAT → ILP → 0-1 ILP

**Prove 0-1 ILP is NP-complete:**

1. **0-1 ILP ∈ NP:** Given 0-1 solution, verify constraints: O(mn) time

2. **ILP ≤ₚ 0-1 ILP:**
   - Given ILP instance with variables xᵢ ∈ ℤ
   - Use binary expansion: represent each xᵢ using binary variables
   - If xᵢ ≤ M, use ⌈log₂(M+1)⌉ binary variables
   - Convert constraints using binary representation
   - ILP feasible ↔ 0-1 ILP feasible

3. **Polynomial time:** O(n log M) binary variables created

**Therefore, 0-1 ILP is NP-complete.**

## Reduction Strategy Guide

### Strategy 1: Direct from 3-SAT

**When to use:** Most problems, especially if they have logical structure

**Steps:**
1. Design variable gadgets (encode variable assignments)
2. Design clause gadgets (encode clause satisfaction)
3. Connect gadgets to enforce constraints
4. Set parameters appropriately

**Examples:** Independent Set, Vertex Cover, 3D Matching, Subset Sum

### Strategy 2: Via Complement/Relationship

**When to use:** Problems related by complement or duality

**Steps:**
1. Reduce to related problem
2. Use complement graph or complement relationship
3. Adjust parameters

**Examples:**
- Independent Set → Clique (complement graph)
- Independent Set → Vertex Cover (complement set)

### Strategy 3: Via Restriction

**When to use:** Target problem is special case of known NP-complete problem

**Steps:**
1. Show target problem is restriction of known problem
2. Show restriction is still NP-complete

**Examples:**
- TSP → Metric TSP (restriction to metric instances)
- SAT → 3-SAT (restriction to 3-CNF)

### Strategy 4: Via Chain Reduction

**When to use:** Can reduce through intermediate problems

**Steps:**
1. Reduce known problem → intermediate problem
2. Reduce intermediate problem → target problem
3. Compose reductions

**Examples:**
- 3-SAT → Independent Set → Clique
- 3-SAT → Vertex Cover → Set Cover

## Problem-Specific Reduction Guides

### Graph Problems

**Best starting points:** 3-SAT, Independent Set, Vertex Cover, Clique

**Common patterns:**
- Variable gadgets: pairs of vertices
- Clause gadgets: triangles or other structures
- Consistency edges: connect gadgets

**Examples:**
- Independent Set, Vertex Cover, Clique (from 3-SAT)
- Dominating Set (from Vertex Cover)
- Graph Coloring (from 3-SAT or Independent Set)

### Set Problems

**Best starting points:** 3-SAT, 3D Matching, Vertex Cover

**Common patterns:**
- Universe: elements to cover
- Sets: choices or assignments
- Coverage: every element in at least one set

**Examples:**
- Set Cover (from Vertex Cover)
- Exact Cover (from 3D Matching or ZOE)
- Hitting Set (from Vertex Cover)

### Number/Sum Problems

**Best starting points:** 3-SAT, Subset Sum

**Common patterns:**
- Base representation encoding
- Digit positions for constraints
- Target sums for requirements

**Examples:**
- Subset Sum (from 3-SAT)
- Partition (from Subset Sum)
- Knapsack (from Subset Sum)

### Path/Cycle Problems

**Best starting points:** 3-SAT, Rudrata Cycle, Rudrata Path

**Common patterns:**
- Variable gadgets: paths or cycles
- Clause gadgets: structures requiring visits
- Connections: enforce ordering

**Examples:**
- Rudrata Cycle (from 3-SAT)
- Rudrata Path (from Rudrata Cycle)
- TSP (from Rudrata Cycle)

### Optimization Problems

**Best starting points:** 3-SAT, Integer Linear Programming

**Common patterns:**
- Variables: decision variables
- Constraints: linear or integer constraints
- Objective: feasibility or optimization

**Examples:**
- Integer Linear Programming (from 3-SAT)
- 0-1 ILP (from ILP)
- Knapsack (from Subset Sum or ILP)

## Quick Reference: Which Problem to Use

| Target Problem Type | Best Starting Point | Why |
|-------------------|-------------------|-----|
| Graph selection | Independent Set | Natural encoding of choices |
| Graph covering | Vertex Cover | Direct covering structure |
| Dense subgraphs | Clique | Complement of Independent Set |
| Set covering | Vertex Cover → Set Cover | Natural reduction |
| Number problems | Subset Sum | Base representation works well |
| Paths/Tours | Rudrata Cycle | Natural path structure |
| Matching | 3D Matching | Triple matching structure |
| Linear constraints | Integer Linear Programming | Direct constraint encoding |
| Binary constraints | Zero-One Equations | 0-1 structure |
| Logical constraints | 3-SAT | Boolean logic encoding |

## Practice: Construct Your Own Reductions

### Exercise 1: Using Independent Set
**Prove Maximum Cut is NP-complete:**
- Reduce from Independent Set
- Design: How do independent sets relate to cuts?

### Exercise 2: Using Vertex Cover
**Prove Dominating Set is NP-complete:**
- Reduce from Vertex Cover
- Design: How do vertex covers relate to dominating sets?

### Exercise 3: Using Subset Sum
**Prove Knapsack is NP-complete:**
- Reduce from Subset Sum
- Design: How do subset sums relate to knapsack?

### Exercise 4: Using 3D Matching
**Prove Exact Cover is NP-complete:**
- Reduce from 3D Matching
- Design: How do 3D matchings relate to exact covers?

### Exercise 5: Using TSP
**Prove Vehicle Routing is NP-complete:**
- Reduce from TSP
- Design: How does TSP relate to vehicle routing?

## Key Takeaways

1. **3-SAT is universal**: Can reduce to almost any problem type
2. **Use problem relationships**: Complement graphs, set complements, etc.
3. **Chain reductions**: Build on known reductions
4. **Choose appropriate starting point**: Match problem structure
5. **Practice patterns**: Common gadget designs work across problems
6. **Verify both directions**: Forward and reverse correctness
7. **Check polynomial time**: Ensure reduction is efficient

## Reduction Summary Table

| Known Problem | Reduces To | Method |
|--------------|-----------|--------|
| 3-SAT | Independent Set | Variable pairs + clause triangles |
| 3-SAT | Vertex Cover | Via Independent Set complement |
| 3-SAT | Clique | Via Independent Set + complement graph |
| 3-SAT | Subset Sum | Base representation encoding |
| 3-SAT | 3D Matching | Variable and clause triples |
| 3-SAT | ILP | 0-1 variables + linear constraints |
| 3-SAT | ZOE | Matrix equations |
| 3-SAT | Rudrata Cycle | Variable and clause gadgets |
| Independent Set | Clique | Complement graph |
| Independent Set | Vertex Cover | Complement set |
| Vertex Cover | Set Cover | Edges as universe, vertices as sets |
| Rudrata Cycle | TSP | Complete graph with weights 1/2 |
| Rudrata Cycle | Rudrata Path | Break cycle |
| Subset Sum | Partition | Add balancing elements |
| ILP | 0-1 ILP | Binary expansion |

## Further Reading

- **Garey & Johnson**: "Computers and Intractability" - Complete catalog of NP-complete problems
- **Karp's 21 Problems**: Original reductions establishing NP-completeness
- **Reduction Techniques**: Component design, local replacement, restriction
- **Approximation Preserving Reductions**: L-reductions, AP-reductions

---

This reference guide provides a roadmap for proving NP-completeness. By understanding which known NP-complete problems work best for different problem types, you can efficiently construct reductions and prove new problems are NP-complete. The key is matching the structure of your target problem to an appropriate starting point and designing gadgets that correctly encode the constraints.

