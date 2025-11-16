---
layout: post
title: "Greedy Algorithms: Theory and Examples"
date: 2025-01-15
categories: [Algorithms, Greedy Algorithms, Optimization]
excerpt: "An introduction to greedy algorithms, covering the greedy choice property, optimal substructure, common examples including activity selection, interval scheduling, and minimum spanning trees, and when greedy algorithms work."
---

## Introduction

Greedy algorithms are a fundamental class of algorithms that make locally optimal choices at each step with the hope of finding a globally optimal solution. They are simple, intuitive, and often very efficient. However, greedy algorithms don't always produce optimal solutions - understanding when they work and when they don't is crucial for algorithm design.

## What is a Greedy Algorithm?

A **greedy algorithm** makes the choice that looks best at the moment, without considering future consequences. At each step, it:
1. Makes a locally optimal choice
2. Never reconsiders previous choices
3. Hopes this leads to a globally optimal solution

### Key Characteristics

- **Greedy Choice Property:** A globally optimal solution can be arrived at by making a locally optimal (greedy) choice
- **Optimal Substructure:** An optimal solution contains optimal solutions to subproblems
- **No Backtracking:** Once a choice is made, it's never reconsidered

## When Do Greedy Algorithms Work?

Greedy algorithms work when:

1. **Greedy Choice Property:** The greedy choice is always part of some optimal solution
2. **Optimal Substructure:** After making the greedy choice, the remaining problem is similar to the original
3. **Problem Structure:** The problem has a structure that allows local optimization to lead to global optimization

### When They Don't Work

Greedy algorithms fail when:
- Local optima don't lead to global optima
- The problem requires considering future consequences
- The greedy choice property doesn't hold

## Classic Examples

### Example 1: Activity Selection Problem

**Problem:** Given n activities with start and finish times, select the maximum number of activities that don't overlap.

**Greedy Strategy:** Always pick the activity that finishes earliest.

**Algorithm:**
```
Algorithm: ActivitySelection(activities)
1. Sort activities by finish time
2. selected = [activities[0]]
3. last_finish = activities[0].finish
4. for i = 1 to n-1:
5.     if activities[i].start >= last_finish:
6.         selected.append(activities[i])
7.         last_finish = activities[i].finish
8. return selected
```

**Example:**
- Activities: (1,4), (3,5), (0,6), (5,7), (8,9), (5,9)
- Sorted by finish: (1,4), (3,5), (0,6), (5,7), (8,9), (5,9)
- Greedy selection: (1,4), (5,7), (8,9) = 3 activities

**Time Complexity:** O(n \log n) (sorting) + O(n) (selection) = O(n \log n)
**Space Complexity:** O(1) additional space

**Why It Works:**
- Greedy choice property: If an optimal solution doesn't include the earliest-finishing activity, we can replace the first activity in the optimal solution with the earliest-finishing one without reducing the count
- Optimal substructure: After selecting an activity, the problem reduces to selecting activities from those that start after it finishes

### Example 2: Fractional Knapsack

**Problem:** Given items with weights and values, fill a knapsack of capacity W to maximize value. Items can be taken fractionally.

**Greedy Strategy:** Always take the item with highest value-to-weight ratio.

**Algorithm:**
```
Algorithm: FractionalKnapsack(items, W)
1. Sort items by value/weight ratio (descending)
2. total_value = 0
3. remaining_capacity = W
4. for each item in sorted_items:
5.     if remaining_capacity >= item.weight:
6.         take entire item
7.         total_value += item.value
8.         remaining_capacity -= item.weight
9.     else:
10.        take fraction: remaining_capacity / item.weight
11.        total_value += item.value * (remaining_capacity / item.weight)
12.        break
13. return total_value
```

**Example:**
- Items: (weight=10, value=60), (weight=20, value=100), (weight=30, value=120)
- Capacity: 50
- Ratios: 6, 5, 4
- Greedy: Take all of item 1 (10), all of item 2 (20), 2/3 of item 3 (20)
- Value: 60 + 100 + 80 = 240

**Time Complexity:** O(n \log n) (sorting) + O(n) (selection) = O(n \log n)
**Space Complexity:** O(1) additional space

**Why It Works:**
- Greedy choice property: Taking the highest value-to-weight ratio maximizes value per unit capacity
- Optimal substructure: After taking some items, the remaining problem is similar with reduced capacity

**Note:** This works for fractional knapsack, but NOT for 0-1 knapsack (where items must be taken whole).

### Example 3: Minimum Spanning Tree (Kruskal's Algorithm)

**Problem:** Find the minimum-weight spanning tree of a connected, weighted graph.

**Greedy Strategy:** Always add the minimum-weight edge that doesn't create a cycle.

**Algorithm:**
```
Algorithm: KruskalMST(G)
1. Sort edges by weight
2. Initialize Union-Find data structure
3. MST = []
4. for each edge (u,v) in sorted_edges:
5.     if Find(u) != Find(v):  // Not in same component
6.         MST.append((u,v))
7.         Union(u,v)
8. return MST
```

**Example:**
```
Graph:
    A---2---B
    |\     /|
    | 3   1 |
    4|     \|5
    C---6---D
```

- Edges sorted: (B,D,1), (A,B,2), (A,C,3), (A,D,4), (B,D,5), (C,D,6)
- MST: (B,D), (A,B), (A,C) = weight 6

**Time Complexity:** O(E \log E) (sorting) + O(E · \alpha(V)) (Union-Find) = O(E \log E)
**Space Complexity:** O(V) for Union-Find

**Why It Works:**
- Greedy choice property: The minimum-weight edge across a cut is always in some MST
- Optimal substructure: After adding an edge, the remaining problem is finding MST of the reduced graph

### Example 4: Minimum Spanning Tree (Prim's Algorithm)

**Problem:** Same as Kruskal's - find MST.

**Greedy Strategy:** Start from arbitrary vertex, always add minimum-weight edge connecting tree to new vertex.

**Algorithm:**
```
Algorithm: PrimMST(G, start)
1. Initialize priority queue with (start, 0)
2. visited = set()
3. MST = []
4. while priority queue not empty:
5.     u = extract_min()
6.     if u not visited:
7.         visited.add(u)
8.         if u != start:
9.             MST.append((parent[u], u))
10.        for each neighbor v of u:
11.            if v not visited and weight(u,v) < key[v]:
12.                key[v] = weight(u,v)
13.                parent[v] = u
14.                insert/update (v, key[v]) in priority queue
15. return MST
```

**Time Complexity:** 
- With binary heap: O(E \log V)
- With Fibonacci heap: O(E + V \log V)
**Space Complexity:** O(V)

### Example 5: Huffman Coding

**Problem:** Given character frequencies, construct a prefix-free binary code minimizing expected code length.

**Greedy Strategy:** Repeatedly merge the two least frequent characters/nodes.

**Algorithm:**
```
Algorithm: HuffmanCoding(frequencies)
1. Create min-heap of nodes (character, frequency)
2. while heap.size() > 1:
3.     left = extract_min()
4.     right = extract_min()
5.     merged = new Node(left.freq + right.freq)
6.     merged.left = left
7.     merged.right = right
8.     insert(merged)
9. return root of tree
```

**Example:**
- Characters: a(45%), b(13%), c(12%), d(16%), e(9%), f(5%)
- Build tree by repeatedly merging least frequent:
  1. Merge f(5) + e(9) = 14
  2. Merge c(12) + 14 = 26
  3. Merge b(13) + d(16) = 29
  4. Merge 26 + 29 = 55
  5. Merge a(45) + 55 = 100

**Time Complexity:** O(n \log n) where n is number of characters
**Space Complexity:** O(n)

**Why It Works:**
- Greedy choice property: The two least frequent characters should have the longest codes
- Optimal substructure: After merging, the problem reduces to coding the merged node plus remaining characters

### Example 6: Dijkstra's Algorithm (Shortest Paths)

**Problem:** Find shortest paths from a source vertex to all other vertices in a weighted graph (non-negative weights).

**Greedy Strategy:** Always relax the vertex with minimum distance that hasn't been processed.

**Algorithm:**
```
Algorithm: Dijkstra(G, source)
1. Initialize distances: dist[source] = 0, dist[v] = ∞ for v ≠ source
2. Initialize priority queue with (source, 0)
3. visited = set()
4. while priority queue not empty:
5.     u = extract_min()
6.     if u in visited: continue
7.     visited.add(u)
8.     for each neighbor v of u:
9.         if dist[u] + weight(u,v) < dist[v]:
10.            dist[v] = dist[u] + weight(u,v)
11.            insert/update (v, dist[v]) in priority queue
12. return dist[]
```

**Example:**
```
Graph:
    A---1---B
    |\     /|
    | 4   2 |
    3|     \|1
    C---5---D
```

- Source: A
- Process A: dist[B]=1, dist[C]=3, dist[D]=4
- Process B: dist[D]=min(4,1+2)=3
- Process C: no updates
- Process D: done
- Result: A→B=1, A→C=3, A→D=3

**Time Complexity:**
- With binary heap: O((V+E) \log V)
- With Fibonacci heap: O(E + V \log V)
**Space Complexity:** O(V)

**Why It Works:**
- Greedy choice property: The unprocessed vertex with minimum distance has its shortest path determined
- Optimal substructure: Shortest path to v through u contains shortest path to u

**Note:** Only works for non-negative edge weights!

### Example 7: Interval Scheduling

**Problem:** Schedule maximum number of non-overlapping intervals.

**Greedy Strategy:** Sort by finish time, always pick the interval that finishes earliest and doesn't conflict.

**Algorithm:** Same as Activity Selection (they're equivalent problems).

**Time Complexity:** O(n \log n)
**Space Complexity:** O(1)

### Example 8: Set Cover (Greedy Approximation)

**Problem:** Given a universe U and collection of sets S, find minimum number of sets covering U.

**Greedy Strategy:** Repeatedly pick the set covering the most uncovered elements.

**Algorithm:**
```
Algorithm: GreedySetCover(U, S)
1. covered = set()
2. selected = []
3. while covered != U:
4.     best_set = None
5.     best_new = 0
6.     for set s in S:
7.         new = |s - covered|
8.         if new > best_new:
9.             best_new = new
10.            best_set = s
11.     selected.append(best_set)
12.     covered = covered ∪ best_set
13. return selected
```

**Time Complexity:** O(|U| · |S|)
**Space Complexity:** O(|U|)

**Approximation Ratio:** H_n where H_n = sum_{i=1}^n 1/i approx ln n (harmonic number)

**Why It's an Approximation:**
- Greedy doesn't always give optimal solution
- But provides good approximation guarantee

## Greedy vs Dynamic Programming

### Key Differences

| Aspect | Greedy | Dynamic Programming |
|--------|--------|---------------------|
| Choices | Makes choice and never reconsiders | Considers all choices |
| Subproblems | Usually one subproblem | Multiple overlapping subproblems |
| Optimality | May not be optimal | Always optimal |
| Efficiency | Usually faster | May be slower |

### When to Use Greedy

- Problem has greedy choice property
- Optimal substructure holds
- Need fast algorithm (greedy is usually efficient)
- Approximation is acceptable (if exact solution not needed)

### When to Use DP

- Need optimal solution
- Greedy choice property doesn't hold
- Overlapping subproblems
- Problem requires considering all possibilities

## Common Greedy Patterns

### 1. Sorting + Greedy Selection

Many greedy algorithms:
1. Sort input by some criterion
2. Process in sorted order, making greedy choices

Examples: Activity Selection, Fractional Knapsack, Interval Scheduling

### 2. Priority Queue Based

Use priority queue to always process "best" option:
- Dijkstra's: process closest unvisited vertex
- Prim's: process minimum edge to tree
- Huffman: merge least frequent nodes

### 3. Union-Find Based

Use Union-Find to track connected components:
- Kruskal's MST: avoid cycles by checking connectivity

## Proving Greedy Correctness

To prove a greedy algorithm is correct:

1. **Show Greedy Choice Property:**
   - Prove that a greedy choice is always part of some optimal solution
   - Usually done by showing we can modify any optimal solution to include the greedy choice

2. **Show Optimal Substructure:**
   - Prove that after making the greedy choice, the remaining problem is similar
   - Show that optimal solution contains optimal solutions to subproblems

### Example Proof: Activity Selection

**Greedy Choice Property:**
- Let a_1 be the activity that finishes earliest
- Let S be an optimal solution
- If S doesn't include a_1, let a_k be the first activity in S
- Since a_1 finishes before a_k starts, we can replace a_k with a_1 in S
- This gives another optimal solution containing a_1 ✓

**Optimal Substructure:**
- After selecting a_1, remaining problem: select activities starting after a_1 finishes
- If S' is optimal for remaining problem, then {a_1} cup S' is optimal for original ✓

## Runtime Analysis Summary

| Problem | Greedy Algorithm | Time Complexity | Space Complexity |
|---------|------------------|-----------------|------------------|
| Activity Selection | Sort by finish time | O(n \log n) | O(1) |
| Fractional Knapsack | Sort by value/weight | O(n \log n) | O(1) |
| MST (Kruskal) | Sort edges, Union-Find | O(E \log E) | O(V) |
| MST (Prim) | Priority queue | O(E \log V) | O(V) |
| Huffman Coding | Min-heap | O(n \log n) | O(n) |
| Dijkstra's | Priority queue | O((V+E) \log V) | O(V) |
| Set Cover | Greedy selection | O(|U| · |S|) | O(|U|) |

## Key Takeaways

1. **Greedy Choice Property:** The greedy choice must be part of some optimal solution
2. **Optimal Substructure:** Optimal solutions contain optimal solutions to subproblems
3. **Efficiency:** Greedy algorithms are usually efficient (often O(n \log n) or better)
4. **Not Always Optimal:** Greedy doesn't always give optimal solutions (e.g., 0-1 Knapsack)
5. **Common Patterns:** Sorting + selection, priority queues, Union-Find
6. **Proof Technique:** Show greedy choice property and optimal substructure

## Further Reading

- **CLRS:** "Introduction to Algorithms" - Comprehensive coverage of greedy algorithms
- **Kleinberg & Tardos:** "Algorithm Design" - Greedy algorithms with proofs
- **Greedy vs DP:** Understanding when to use each approach
- **Approximation Algorithms:** Greedy algorithms for NP-hard problems

## Practice Problems

1. **Activity Selection:** Implement the greedy algorithm and prove its correctness.

2. **Fractional vs 0-1 Knapsack:** Why does greedy work for fractional but not 0-1? Give a counterexample.

3. **MST Algorithms:** Compare Kruskal's and Prim's algorithms. When is each better?

4. **Dijkstra's Limitation:** Why doesn't Dijkstra's work with negative weights? Give an example.

5. **Huffman Coding:** Construct Huffman tree for frequencies: a(40), b(30), c(20), d(10). What are the codes?

6. **Set Cover:** Design a greedy algorithm for weighted set cover (sets have costs). What approximation ratio does it achieve?

7. **Interval Coloring:** Given intervals, color them with minimum colors so overlapping intervals have different colors. Design a greedy algorithm.

8. **Job Scheduling:** Given jobs with deadlines and profits, schedule to maximize profit. Design a greedy algorithm.

---

Understanding greedy algorithms is essential for algorithm design. They provide elegant, efficient solutions to many optimization problems when the greedy choice property and optimal substructure hold.

