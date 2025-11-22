---
layout: post
title: "Reductions from Vertex Cover: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from Vertex Cover to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

Vertex Cover is a fundamental covering problem proven NP-complete by reduction from Independent Set. This post provides answers to common reduction questions when using Vertex Cover to prove other problems are NP-complete.

## Problem Definition: Vertex Cover

**Vertex Cover Problem:**
- **Input:** Graph G = (V, E) and integer k
- **Output:** YES if G has a vertex cover of size ≤ k, NO otherwise

**Vertex Cover:** A subset of vertices such that every edge has at least one endpoint in the cover.

## Common Reduction Questions

### Q1: How do you reduce Vertex Cover to Set Cover?

**Answer:** Encode edges as elements and vertices as sets.

**Reduction:**
- Given Vertex Cover instance: graph G = (V, E), integer k
- Universe U = E (all edges)
- For each vertex v ∈ V, create set Sᵥ = {e ∈ E : v ∈ e} (edges incident to v)
- Return Set Cover: universe U, sets {Sᵥ : v ∈ V}, target k

**Correctness:**
- Vertex cover of size k ↔ Set cover of size k
- Covering edges ↔ Covering elements

**Time:** O(n + m)

### Q2: How do you reduce Vertex Cover to Hitting Set?

**Answer:** Similar to Set Cover, edges as elements.

**Reduction:**
- Given Vertex Cover instance: graph G = (V, E), integer k
- Collection C = {Sᵥ : v ∈ V} where Sᵥ = {e ∈ E : v ∈ e}
- Return Hitting Set: collection C, target k

**Correctness:**
- Vertex cover of size k ↔ Hitting set of size k
- Hitting all sets ↔ Covering all edges

**Time:** O(n + m)

### Q3: How do you reduce Vertex Cover to Dominating Set?

**Answer:** Add edges to make vertex cover also dominate.

**Reduction:**
- Given Vertex Cover instance: graph G, integer k
- Create graph G' by adding edges to ensure domination
- Return Dominating Set: graph G', integer k

**Correctness:**
- Vertex cover in G → Dominating set in G'
- Requires careful edge addition

**Time:** O(n²)

### Q4: How do you reduce Vertex Cover to Feedback Vertex Set?

**Answer:** Vertex cover covers edges, feedback set breaks cycles.

**Reduction:**
- Given Vertex Cover instance: graph G, integer k
- Return Feedback Vertex Set: graph G, integer k

**Correctness:**
- In bipartite graphs, vertex cover = feedback vertex set
- General graphs require modification

**Time:** O(n + m)

### Q5: How do you reduce Vertex Cover to Independent Set?

**Answer:** Use complement relationship.

**Reduction:**
- Given Vertex Cover instance: graph G, integer k
- Return Independent Set: graph G, k' = |V| - k

**Correctness:**
- Vertex cover of size k ↔ Independent set of size |V| - k
- S is vertex cover ↔ V \ S is independent set

**Time:** O(1)

## Reduction Patterns from Vertex Cover

### Pattern 1: Covering Problems
- **Use when:** Target problem involves covering elements
- **Examples:** Set Cover, Hitting Set
- **Key:** Encode edges as elements to cover

### Pattern 2: Set Representation
- **Use when:** Target problem uses sets
- **Examples:** Set Cover, Exact Cover
- **Key:** Vertices become sets, edges become elements

### Pattern 3: Complement Relationship
- **Use when:** Target problem is complement
- **Examples:** Independent Set
- **Key:** V \ S relationship

## Key Takeaways

1. **Covering structure:** Natural for covering problems
2. **Set encoding:** Edges → elements, vertices → sets
3. **Complement:** Relationship with independent set
4. **Generalization:** Many covering problems reduce from VC

## Practice Problems

1. Reduce Vertex Cover to Edge Cover
2. Reduce Vertex Cover to Set Cover (weighted)
3. Reduce Vertex Cover to Minimum Vertex Cover

---

Vertex Cover reductions often use covering structures and set representations.

