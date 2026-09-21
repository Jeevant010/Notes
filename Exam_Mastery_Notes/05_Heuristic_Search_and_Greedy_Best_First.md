# 🎯 Module 05: Heuristic Search & Greedy Best-First Search
**Reference Source**: `best_first_search.pdf` & `search_algorithms.pdf`  
**Estimated Study Time**: 25 – 30 Minutes  
**Exam Importance**: High (Guaranteed 8–10 marks: Informed vs Uninformed comparison, Greedy BFS Trace, Suboptimality proofs)

---

## 🧭 Executive Summary: What This Module Is About

Uninformed search methods (like BFS, DFS, and UCS) explore blindly because they treat all unexplored branches with equal ignorance. **Informed (Heuristic) Search** introduces domain-specific knowledge to guide the search smartly toward the goal.

This module introduces:
1. What a **heuristic function $h(n)$** is and how it acts as an intelligent compass.
2. The difference between **Informed** and **Uninformed** search.
3. **Greedy Best-First Search**: How it works, its Open/Closed list priority queue mechanics, and a complete traced example.
4. Why Greedy BFS is **NOT optimal** and **NOT complete**—paving the way for the A* algorithm!

---

## 1. What Is a Heuristic?

### Formal Definition
> *"A **Heuristic Function $h(n)$** is an evaluation function that estimates the cheapest path cost from the state at node $n$ to the nearest goal state."*

$$\mathbf{h(n) \approx \text{Estimated cost from node } n \text{ to Goal}}$$
$$\mathbf{h(\text{Goal}) = 0}$$

### Intuition: The Compass Analogy
Imagine you are lost in a dense, dark forest trying to reach a mountain peak:
- **Uninformed (Blind) Search**: You walk in every possible direction randomly or in expanding spirals, bumping into trees, completely unaware of where the mountain is.
- **Heuristic Search**: You look up at the mountain peak (or use an altitude compass). Even though you don't know the exact footpath through the trees, you always choose trails that lead *uphill toward the visible peak*.

---

## 2. Informed vs. Uninformed Search: Key Differences

*(A guaranteed 4–6 mark comparison question in university exams)*

| Basis of Comparison | Uninformed Search (Blind Search) | Informed Search (Heuristic Search) |
| :--- | :--- | :--- |
| **Domain Knowledge** | Zero problem knowledge beyond start state and goal test. | Uses domain-specific heuristic function $h(n)$ estimating distance to goal. |
| **Search Direction** | Explores blindly in all directions (or plunges deep). | Focuses exploration directly toward the goal state. |
| **Efficiency / Speed** | Slower; explores large portions of state space. | Much faster; prunes unpromising branches early. |
| **Memory / Time** | Exponentially high in wide/deep trees. | Significantly lower when guided by an accurate heuristic. |
| **Examples** | Breadth-First (BFS), Depth-First (DFS), Uniform Cost (UCS), IDDFS. | Greedy Best-First Search, A* Search, AO* Search, Hill Climbing. |

---

## 3. Best-First Search & Greedy Best-First Search

### General Best-First Search Family
**Best-First Search** is a general search paradigm that explores a graph by expanding the most promising node according to an **evaluation function $f(n)$**, using a **Priority Queue**.

### Greedy Best-First Search
In **Greedy Best-First Search**, the evaluation function is defined purely by the heuristic estimate:
$$\mathbf{f(n) = h(n)}$$

**Core Philosophy**:  
*"Expand the node that appears to be closest to the goal right now, regardless of how much effort it took to get here."*

### Key Data Structures
1. **Open List (Frontier)**: A Priority Queue containing discovered but unexpanded nodes, ordered in **ascending order of $h(n)$** (lowest estimated distance to goal at the top).
2. **Closed List (Explored Set)**: A set storing all nodes that have already been expanded to prevent infinite loops on graphs.

---

## 4. The 4-Step Algorithmic Cycle

```
[ Step 1: Initialize ] ──> Insert Start node S into Open List with priority h(S). Closed List = empty.
           │
           ▼
[ Step 2: Pick Best  ] ──> Pop node 'Current' with lowest h(Current) from Open List.
           │
           ▼
[ Step 3: Goal Check ] ──> IF Current is Goal -> Terminate & Return Path!
           │               ELSE -> Add Current to Closed List.
           ▼
[ Step 4: Expand     ] ──> For each unvisited neighbor:
                           Compute h(neighbor), add to Open List, record Current as parent.
                           Repeat from Step 2!
```

---

## 5. Step-by-Step Worked Example (From Course Presentation)

### The Problem Graph
Find a path from Start node $S$ to Goal node $G$ using Greedy Best-First Search.

```
                  [ A ] (h = 8)
                 /     \
               ( )     ( )
               /         \
   [ S ] (h = 10)         [ C ] (h = 4) ────> [ G ] (h = 0, GOAL)
               \         /
               ( )     ( )
                 \     /
                  [ B ] (h = 6)
```

#### Node Heuristic Values $h(n)$:
- $h(S) = 10$
- $h(A) = 8$
- $h(B) = 6$
- $h(C) = 4$
- $h(G) = 0$ (Goal)

---

### Step-by-Step Execution Trace Table

| Step | Current Node Expanded | Heuristic $h(n)$ | Open List Contents `[(Node, h-value)]` | Closed List | Decision / Explanation |
| :---: | :---: | :---: | :--- | :--- | :--- |
| **0** | **Start** | — | `[(S, 10)]` | `{}` | Insert start node $S$ with $h(S)=10$. |
| **1** | **$S$** | $10$ | `[]` | `{S}` | Pop $S$. Neighbors are $A(h=8)$ and $B(h=6)$.<br>Insert both into Open List. |
| | *Queue state* | | `[(B, 6), (A, 8)]` | `{S}` | $B$ is at top of queue ($h=6 < 8$). |
| **2** | **$B$** | $6$ | `[(A, 8)]` | `{S, B}` | Pop $B$. Neighbor is $C(h=4)$.<br>Insert $C$ into Open List. |
| | *Queue state* | | `[(C, 4), (A, 8)]` | `{S, B}` | $C$ is at top of queue ($h=4 < 8$). |
| **3** | **$C$** | $4$ | `[(A, 8)]` | `{S, B, C}` | Pop $C$. Neighbor is $G(h=0)$.<br>Insert $G$ into Open List. |
| | *Queue state* | | `[(G, 0), (A, 8)]` | `{S, B, C}` | $G$ is at top of queue ($h=0$). |
| **4** | **$G$** | **$0$** | `[(A, 8)]` | `{S, B, C, G}` | **Pop $G$. Goal reached! STOP!** |

### Result:
- **Path Returned**: $\mathbf{S \longrightarrow B \longrightarrow C \longrightarrow G}$
- Notice that node $A$ was never even expanded, saving computation time!

---

## 6. The Fatal Flaws of Greedy Best-First Search

While Greedy BFS is fast, examiners love asking you to explain why it is **NOT optimal** and **NOT complete**.

### 1. Suboptimality (It Can Pick Terrible Paths!)
Greedy Best-First Search is **myopic (shortsighted)**: it looks only at $h(n)$ (estimated distance remaining) and completely ignores $g(n)$ (the actual cost incurred so far).

```
                 100 (Winding, muddy mountain track)
       [ S ] ──────────────────────────────────────────> [ A ] (h = 1)
         │                                                 │
         │ 1                                               │ 1
         v                                                 v
       [ B ] (h = 5) ─────────────(1)──────────────────> [ G ] (h = 0, Goal)
```
- **What Greedy BFS Does**:
  - $S$ sees two neighbors: $A$ ($h=1$) and $B$ ($h=5$).
  - Because $1 < 5$, Greedy BFS greedily jumps to $A$!
  - Path found: $S \to A \to G$. **Total Actual Cost = $100 + 1 = 101$!**
- **Optimal Path**:
  - Path $S \to B \to G$ had an actual cost of $1 + 1 = 2$!
  - Greedy BFS missed the optimal path because it was seduced by the artificially low heuristic value of $A$.

### 2. Incompleteness
On infinite state spaces or if implemented without an explored/closed list on cyclic graphs, Greedy BFS can get stuck in an **infinite loop**:
- It can repeatedly oscillate between two states that have low $h$-values, even if neither state leads to the goal!

### 3. Complexity Analysis
- **Time Complexity**: $O(b^m)$ in the worst case (where $m$ is the maximum depth of the search space). However, with a good heuristic, practical time is often close to $O(b \cdot d)$.
- **Space Complexity**: $O(b^m)$ because it keeps all frontier nodes in memory in the Open List.

---

## 7. The Bridge to A* Search

Greedy Best-First Search failed because it **only tracked $h(n)$** (future cost) and ignored $g(n)$ (past cost).  
Uniform Cost Search (UCS) failed to be fast because it **only tracked $g(n)$** (past cost) and ignored $h(n)$ (future guidance).

> **The Grand Idea of A\* Search**:  
> What if we combine both?
> $$\mathbf{f(n) = g(n) + h(n)}$$
> - $g(n)$ guarantees **optimality** (keeps search honest about real costs).
> - $h(n)$ provides **speed and direction** (guides search toward the goal).

---

## 🎯 Exam Practice Checklist: Questions to Master
- [ ] Define a heuristic function $h(n)$ and state what $h(\text{Goal})$ must equal.
- [ ] Give 4 differences between Uninformed and Informed search in a neat table.
- [ ] What is the evaluation function $f(n)$ for Greedy Best-First Search?
- [ ] Draw a counter-example showing why Greedy Best-First Search is not optimal.
- [ ] Trace Greedy BFS on the graph $S \to (A, B) \to C \to G$ using an Open/Closed list table.

---
*Next Topic: [Module 06: A\* Search and AO\* Search Algorithms](./06_A_Star_and_AO_Star_Algorithms.md)*
