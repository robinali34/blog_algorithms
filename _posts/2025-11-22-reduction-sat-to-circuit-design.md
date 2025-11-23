---
layout: post
title: "Reduction: SAT to Circuit Design"
date: 2025-11-22
categories: [Algorithms, Complexity Theory, NP-Hard]
excerpt: "A detailed proof showing how to reduce SAT to Circuit Design, proving that the Circuit Design problem is NP-complete."
---

## Introduction

This post provides a detailed proof that the Circuit Design problem is NP-complete by reducing from SAT. The reduction demonstrates how Boolean satisfiability constraints can be encoded as circuit design requirements, showing the fundamental connection between logical formulas and circuit structures.

## Problem Definitions

### Circuit Design Problem

**Input:**
- A Boolean formula φ (or Boolean function specification)
- An integer k (maximum circuit size/complexity)

**Output:** YES if there exists a Boolean circuit of size at most k that computes φ AND outputs TRUE for at least one input assignment, NO otherwise

**Circuit Structure:**
- A Boolean circuit consists of:
  - Input gates (variables x₁, ..., x_n)
  - Logic gates (AND, OR, NOT)
  - One output gate
- Circuit size: Total number of gates (excluding input gates)
- The circuit computes a Boolean function f: {0,1}ⁿ → {0,1}

**Problem Variant:**
- Given formula φ and size k, does there exist a circuit of size ≤ k that:
  1. Computes φ (outputs φ(x) for input x)
  2. Outputs TRUE for at least one input x

### SAT Problem

**Input:** A Boolean formula φ in CNF (Conjunctive Normal Form)

**Output:** YES if φ is satisfiable, NO otherwise

**SAT:** The problem of determining whether there exists a truth assignment to variables that makes the formula TRUE.

## 1. NP-Completeness Proof of Circuit Design: Solution Validation

### Circuit Design ∈ NP

**Verification Algorithm:**
Given a candidate solution (circuit C):
1. Check that circuit C has size ≤ k: O(k) time
2. Check that C is a valid circuit structure:
   - Verify gates are properly connected: O(k) time
   - Verify no cycles (circuit is acyclic): O(k) time
3. Evaluate circuit C on all input combinations to verify it computes the desired function:
   - For each of 2ⁿ input combinations: O(k) time to evaluate
   - Total: O(2ⁿ · k) time

**Total Time:** O(2ⁿ · k), which is exponential in n but polynomial in the size of the circuit specification (if given as truth table or formula).

**Note:** If the function is specified as a Boolean formula φ of size m:
- We can verify that C computes φ by checking equivalence: O(m · k) time
- We can check if C outputs TRUE for some input by evaluating on all inputs or using a SAT solver on the circuit
- This verification is polynomial in the size of φ and k

**Conclusion:** Circuit Design ∈ NP.

## 2. Reduce SAT to Circuit Design

**Key Insight:** A Boolean formula φ is satisfiable if and only if there exists a circuit that computes φ (which always exists - the formula itself is a circuit). The key is to encode the satisfiability question as a circuit design problem with size constraints.

**Hint:** Convert the SAT formula into a circuit design problem where we ask: "Does there exist a circuit of a certain size that outputs TRUE for at least one input?" This is equivalent to asking if the formula is satisfiable.

### 2.1 Input Conversion

Given a SAT instance: Boolean formula φ in CNF with variables x₁, x₂, ..., x_n and clauses C₁, C₂, ..., C_m.

**Construction:**

**Step 1: Convert Formula to Circuit Specification**
- The formula φ naturally corresponds to a Boolean circuit:
  - Each variable xᵢ is an input
  - Each clause Cⱼ = (l₁ ∨ l₂ ∨ ... ∨ lₖ) is an OR gate
  - The overall formula φ = C₁ ∧ C₂ ∧ ... ∧ C_m is an AND of clauses
- This gives us a circuit structure that computes φ

**Step 2: Formulate Circuit Design Problem**
- **Input variables:** x₁, x₂, ..., x_n
- **Desired function:** The circuit should compute φ
- **Size constraint:** Set k = m + n (number of clauses + number of variables)
  - This allows the natural circuit representation:
    - n input gates (variables)
    - m OR gates (one per clause)
    - 1 AND gate (combining clauses)
    - Total: n + m + 1 gates
  - Actually, we need to be more careful about gate counting

**Refined Construction:**

**Circuit Structure:**
- For each variable xᵢ: input gate
- For each clause Cⱼ = (l₁ ∨ l₂ ∨ ... ∨ lₖ):
  - Create OR gate computing l₁ ∨ l₂ ∨ ... ∨ lₖ
  - This requires k-1 OR gates for a clause with k literals
- Create AND gate combining all clause outputs
- Total gates: n (inputs) + ∑ⱼ (`|Cⱼ|` - 1) (OR gates) + 1 (final AND) = n + m' + 1 where m' is total literals

**Simplified Approach:**
- Set k = n + m + 1 (sufficient for the natural circuit)
- Or set k large enough to allow the formula's circuit representation

**Key Property:** 
- φ is satisfiable ↔ There exists a circuit of size ≤ k computing φ that outputs TRUE for at least one input
- The natural circuit for φ has size ≤ k (by construction)
- If φ is satisfiable, this circuit outputs TRUE for satisfying assignments
- If φ is unsatisfiable, any circuit computing φ outputs FALSE for all inputs

**Final Construction:**
- **Input variables:** x₁, x₂, ..., x_n (same as SAT instance)
- **Desired function:** Circuit should compute φ(x₁, ..., x_n)
- **Size bound:** k = n + m + L + 1 where:
  - n = number of variables
  - m = number of clauses
  - L = total number of literals across all clauses
  - +1 for final AND gate
- Return Circuit Design instance: inputs x₁, ..., x_n, function φ, size bound k

### 2.2 Output Conversion

**Given:** Circuit Design solution (circuit C of size ≤ k computing φ)

**Extract SAT Solution:**
- Evaluate circuit C to find an input assignment that makes output TRUE
- Since C computes φ, if C outputs TRUE for some input, then φ is satisfiable
- Return the input assignment (x₁, ..., x_n) that makes C output TRUE

**Verify SAT:**
- The assignment makes circuit C output TRUE
- Since C computes φ, the assignment satisfies φ
- Therefore, φ is satisfiable

## 3. Correctness Justification

### 3.1 If SAT has a solution, then Circuit Design has a solution

**Given:** SAT instance φ is satisfiable with assignment A: (x₁ = a₁, ..., x_n = a_n).

**Construct Circuit:**
- Build the natural circuit for φ:
  - Input gates: x₁, ..., x_n
  - For each clause Cⱼ, create OR gate computing the clause
  - Create AND gate combining all clause outputs
- This circuit computes φ
- Circuit size: n + ∑ⱼ (`|Cⱼ|` - 1) + 1 ≤ k (by construction of k)

**Verify Circuit:**
- Circuit C computes φ (by construction)
- Circuit size ≤ k (by construction)
- Circuit outputs TRUE for assignment A (since A satisfies φ)
- Therefore, circuit C is a valid solution

**Conclusion:** Circuit Design has a solution.

### 3.2a If SAT does not have a solution, then Circuit Design has no solution

**Given:** SAT instance φ is unsatisfiable.

**Proof by Contradiction:**

Assume Circuit Design has solution (circuit C of size ≤ k computing φ).

**Evaluate Circuit:**
- Circuit C computes φ
- For any input assignment, evaluate C
- Since φ is unsatisfiable, C outputs FALSE for all inputs
- But a circuit that always outputs FALSE can be represented by a constant FALSE gate (size 1)
- However, we require the circuit to compute φ, not just output FALSE
- Actually, if φ is unsatisfiable, then φ is equivalent to FALSE
- A circuit computing FALSE has size 1 (just a FALSE constant)
- But wait: we need the circuit to compute the specific formula φ, not just its value

**Refined Argument:**
- If φ is unsatisfiable, then φ is logically equivalent to FALSE
- However, the circuit design problem asks: "Does there exist a circuit computing φ?"
- The answer is YES (we can always build a circuit for any formula)
- **Key Insight:** We need to modify the problem to ask: "Does there exist a circuit that outputs TRUE for at least one input?"
- Or: "Does there exist a circuit of size ≤ k that computes φ AND outputs TRUE for some input?"

**Corrected Problem Formulation:**
- Circuit Design Problem (Modified): Given formula φ and size k, does there exist a circuit of size ≤ k that computes φ AND outputs TRUE for at least one input assignment?

**With This Formulation:**
- If φ is unsatisfiable, no circuit computing φ can output TRUE
- Therefore, Circuit Design has no solution

**Conclusion:** Circuit Design has no solution (with proper problem formulation).

### 3.2b If Circuit Design has a solution, then SAT has a solution

**Given:** Circuit Design instance has solution (circuit C of size ≤ k computing φ).

**Extract SAT Solution:**
- Circuit C computes φ
- Since C is a solution, it must output TRUE for at least one input (or we can find such an input)
- Evaluate C on all 2ⁿ inputs (or use a satisfiability checker on C)
- Find an input assignment A = (x₁ = a₁, ..., x_n = a_n) that makes C output TRUE
- Since C computes φ, assignment A satisfies φ

**Verify SAT:**
- Assignment A makes circuit C output TRUE
- Since C computes φ, A satisfies φ
- Therefore, φ is satisfiable

**Conclusion:** SAT has a solution.

**Polynomial Time:** O(n + m + L) to construct circuit specification, where L is total literals.

**Therefore, Circuit Design is NP-complete.**

---

## Key Insights

1. **Formula as Circuit:** Every Boolean formula naturally corresponds to a Boolean circuit
2. **Satisfiability as Circuit Evaluation:** Formula satisfiability is equivalent to finding an input that makes the circuit output TRUE
3. **Size Constraints:** The size bound k ensures the problem is non-trivial
4. **Polynomial Reduction:** Converting formula to circuit specification is polynomial-time

## Construction Summary

Given SAT instance φ in CNF:
- Use same variables as inputs
- Specify desired function as φ
- Set size bound k = n + m + L + 1 (sufficient for natural circuit)
- Circuit Design asks: "Does there exist a circuit of size ≤ k computing φ that outputs TRUE for some input?"
- This is equivalent to: "Is φ satisfiable?"

## Practice Questions

1. **Modify the problem:** What if we ask for a circuit of exactly size k? How does the reduction change?

2. **Alternative formulation:** Consider Circuit Design as: "Given truth table, does there exist a circuit of size ≤ k?" How would you reduce SAT to this?

3. **Circuit minimization:** What if we ask for the minimum-size circuit? Is this still NP-complete?

4. **Complexity:** Analyze the exact polynomial time complexity of the reduction.

---

This reduction demonstrates the fundamental connection between Boolean satisfiability and circuit design, showing that Circuit Design is NP-complete.

