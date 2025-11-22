---
layout: post
title: "Reductions from Clique: Common Questions and Answers"
date: 2025-11-21
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A comprehensive guide to reducing from Clique to prove other problems are NP-complete, with common reduction questions and detailed answers."
---

## Introduction

Clique is a fundamental graph problem that's NP-complete. This post provides answers to common reduction questions when using Clique to prove other problems are NP-complete.

## Problem Definition: Clique

**Clique Problem:**
- **Input:** Graph G = (V, E) and integer k
- **Output:** YES if G has a clique of size ≥ k, NO otherwise

**Clique:** A subset of vertices where every pair is connected by an edge.

## Common Reduction Questions

### Q1: How do you reduce Clique to Independent Set?

**Answer:** Use complement graph.

**Reduction:**
- Given Clique instance: graph G, integer k
- Create complement graph G̅ (edge exists in G̅ ↔ edge doesn't exist in G)
- Return Independent Set: graph G̅, integer k

**Correctness:**
- Clique of size k in G ↔ Independent set of size k in G̅
- No edges in G̅ ↔ all edges in G

**Time:** O(n²) to create complement

### Q2: How do you reduce Clique to Vertex Cover?

**Answer:** Use complement graph + complement relationship.

**Reduction:**
- Reduce Clique to Independent Set → graph G̅, k
- Return Vertex Cover: graph G̅, k' = |V| - k

**Correctness:**
- Clique of size k in G ↔ Independent set of size k in G̅ ↔ Vertex cover of size |V| - k in G̅

**Time:** O(n²)

### Q3: How do you reduce Clique to Subgraph Isomorphism?

**Answer:** Check if complete graph Kₖ is subgraph of G.

**Reduction:**
- Given Clique instance: graph G, integer k
- Create complete graph Kₖ (clique of size k)
- Return Subgraph Isomorphism: graph G, pattern Kₖ

**Correctness:**
- Clique of size k in G ↔ Kₖ is subgraph of G

**Time:** O(1) (trivial reduction)

### Q4: How do you reduce Clique to Maximum Common Subgraph?

**Answer:** Find common subgraph with another complete graph.

**Reduction:**
- Given Clique instance: graph G, integer k
- Create complete graph Kₖ
- Return Maximum Common Subgraph: graphs G and Kₖ, target size k

**Correctness:**
- Clique of size k in G ↔ Common subgraph of size k exists

**Time:** O(1)

### Q5: How do you reduce Clique to Dense Subgraph?

**Answer:** Clique is special case of dense subgraph.

**Reduction:**
- Given Clique instance: graph G, integer k
- Return Dense Subgraph: graph G, size k, density threshold = 1.0 (complete)

**Correctness:**
- Clique of size k ↔ Dense subgraph with density 1.0

**Time:** O(1)

## Reduction Patterns from Clique

### Pattern 1: Complement Graph
- **Use when:** Target problem is complement of clique-like structure
- **Examples:** Independent Set, Vertex Cover
- **Key:** Complement graph preserves structure

### Pattern 2: Subgraph Problems
- **Use when:** Target problem involves finding subgraphs
- **Examples:** Subgraph Isomorphism, Maximum Common Subgraph
- **Key:** Clique is complete subgraph

### Pattern 3: Restriction
- **Use when:** Target problem is generalization
- **Examples:** Dense Subgraph, k-Clique
- **Key:** Clique is special case

## Key Takeaways

1. **Complement graph:** Powerful tool for graph reductions
2. **Complete subgraphs:** Clique represents perfect connectivity
3. **Subgraph problems:** Many reduce from Clique
4. **Restriction:** Clique is special case of many problems

## Practice Problems

1. Reduce Clique to k-Clique (fixed k)
2. Reduce Clique to Maximum Clique
3. Reduce Clique to Clique Cover
4. Reduce Clique to Partition into Cliques

---

Clique reductions often use complement graphs and subgraph relationships.

