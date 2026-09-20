# ⚖️ Module 03: Uninformed Search & Uniform Cost Search (UCS)
**Reference Source**: `Uniform Cost Search Presentation.pdf`  
**Estimated Study Time**: 35 – 40 Minutes  
**Exam Importance**: Very High (Guaranteed 10–12 marks: Step-by-Step Priority Queue Trace, Dijkstra Equivalence, Goal Test on Pop rule)

---

## 🧭 Executive Summary: What This Module Is About

Search algorithms form the core computational engine of AI agents. When an agent has no domain knowledge or heuristics to estimate distance to the goal, it must rely on **Uninformed (Blind) Search**. 

While Breadth-First Search (BFS) works well when every action costs the same, it fails completely on real-world weighted graphs. **Uniform Cost Search (UCS)** fixes this by expanding nodes in order of **cumulative path cost $g(n)$**, using a **Min-Heap Priority Queue**. This module walks you through:
1. Why BFS fails on weighted graphs.
2. The core UCS algorithm and the **#1 exam trap** (Goal Test on Pop!).
3. A complete, step-by-step traced graph example with Priority Queue state tables.
4. Complexity analysis $O(b^{1 + \lfloor C^*/\epsilon \rfloor})$ and its equivalence to Dijkstra's Algorithm.

---

## 1. Uninformed (Blind) Search: BFS vs. DFS Foundations

An **uninformed search** algorithm has no idea whether a non-goal state is close to or far from the goal. It only knows:
- How to generate successors from the current state.
- How to check if a state satisfies `GoalTest(s)`.

```
                    BREADTH-FIRST SEARCH (BFS)                   DEPTH-FIRST SEARCH (DFS)
Queue:              FIFO (First-In, First-Out)                   LIFO (Last-In, First-Out / Stack)
Strategy:           Explores shallowest nodes first (level-by-level) Explores deepest node first (plunges down)
Completeness:       ✅ YES (if branching factor b is finite)      ❌ NO (fails on infinite-depth paths)
Optimality:         ✅ YES (ONLY if all step costs are equal!)    ❌ NO (returns first path found)
Time Complexity:    O(b^d)                                       O(b^m)
Space Complexity:   O(b^d)  <-- DISASTROUS MEMORY!               O(b * m) <-- Very lightweight!
```

---

## 2. Why Do We Need Uniform Cost Search (UCS)?

### The Fatal Flaw of Standard BFS
Breadth-First Search measures path length by **number of hops (edge count)**, completely ignoring edge weights. 

Consider finding a path from Start $S$ to Goal $G$:
```
             100 (Direct Flight)
     [ S ] ──────────────────────> [ G ]
       │                             ▲
     1 │                             │ 1
       v            1                │
     [ A ] ──────────────────────> [ B ]
```
- **BFS Behavior**: BFS sees that $G$ is 1 hop away via the top edge ($S \to G$), and 3 hops away via $S \to A \to B \to G$. BFS immediately returns the 1-hop path: **Cost = 100**!
- **UCS Behavior**: UCS tracks cumulative cost $g(n)$. It sees path $S \to A \to B \to G$ has cost $1 + 1 + 1 = 3$. UCS returns the optimal path: **Cost = 3**!

### Core Concept: Cost Contours
Instead of expanding nodes in concentric circles of *depth* (like BFS), UCS expands nodes in concentric **contours of equal cumulative path cost $g(n)$** radiating outward from the start node, like ripples on a pond.

---

## 3. How UCS Works: The Algorithmic Steps

### Key Data Structure
- **Priority Queue (Min-Heap)**: Stores frontier nodes sorted in ascending order of cumulative path cost $g(n)$.
- **Explored Set (Closed List)**: Stores all nodes that have already been expanded to prevent cycles and duplicate work.

### Step-by-Step Algorithmic Procedure
1. **Initialize**:
   - Insert the Start node $S$ into the Priority Queue with cost $g(S) = 0$.
   - Initialize an empty `Explored Set`.
2. **Loop**:
   - If the Priority Queue is empty, return **FAILURE** (no path exists).
   - **Pop** the node $N$ with the **lowest cumulative path cost $g(N)$** from the Priority Queue.
3. **Goal Test (CRITICAL EXAM TRAP!)**:
   - **Check if $N$ is the Goal state right now, upon POPPING.**
   - If $N == \text{Goal}$, return the solution path and cost $g(N)$. **TERMINATE WITH SUCCESS.**
4. **Expand Node $N$**:
   - Add $N$ to the `Explored Set`.
   - For each child node $C$ of $N$ with step cost $c(N, C)$:
     - Compute new cumulative cost: $g(C) = g(N) + c(N, C)$.
     - **Case 1**: If $C$ is not in the Explored Set and not in the Priority Queue:
       - Insert $C$ into Priority Queue with cost $g(C)$ and record $N$ as its parent.
     - **Case 2**: If $C$ is already in the Priority Queue with a higher cost:
       - **Decrease-Key**: Update $C$'s cost to the cheaper $g(C)$ and point its parent to $N$.
     - **Case 3**: If $C$ is already in the Explored Set:
       - Ignore it (since edge costs are non-negative, we already found the optimal path to $C$).

---

## ⚠️ THE #1 EXAM TRAP: Why Test for Goal on POP, NOT on GENERATION?

Examiners frequently ask: *"Why does UCS perform the goal test only when popping a node from the priority queue, unlike BFS which tests upon generating a child?"*

> **The Golden Answer**:
> If UCS checked for the goal upon *generation*, it would terminate the very first time the goal node is added to the priority queue. However, that first path discovered might be a high-cost path! 
> 
> By waiting until the goal node is **popped**, UCS guarantees that **all nodes with cumulative cost less than $g(\text{Goal})$ have already been completely expanded**. Because the Priority Queue always pops the minimum cost node, the first time the goal node emerges from the top of the min-heap, its path is mathematically guaranteed to be the cheapest possible path!

---

## 4. Complete Worked Numerical Trace (The $V_1$ to $V_6$ Problem)

### Problem Statement
Given the following weighted directed graph, find the optimal path from Start node $V_1$ to Goal node $V_6$ using **Uniform Cost Search**.

```
                   (2)
            [ V1 ] ────> [ V2 ] ────(4)────> [ V4 ]
              │            │                   │
             (5)          (2)                 (3)
              │            v                   v
              v         [ V3 ] ────(1)────> [ V5 ] ────(2)────> [ V6 ]
            [ V3 ] <───────────────────────────┘
```

#### Graph Edge Weights:
- $V_1 \to V_2$ : cost 2
- $V_1 \to V_3$ : cost 5
- $V_2 \to V_3$ : cost 2
- $V_2 \to V_4$ : cost 4
- $V_2 \to V_5$ : cost 7
- $V_3 \to V_5$ : cost 1
- $V_4 \to V_6$ : cost 3
- $V_5 \to V_6$ : cost 2

---

### Step-by-Step Execution Table (Draw this exact table in your exam!)

| Step | Action / Node Popped | Cumulative Cost $g(N)$ | Priority Queue Contents `[(Node, Cost, Parent)]` | Explored Set | Remarks & Decisions |
| :---: | :--- | :---: | :--- | :--- | :--- |
| **0** | **Initialize** | — | `[(V1, 0, None)]` | `{}` | Insert start node $V_1$ with cost 0. |
| **1** | **Pop $V_1$** | $0$ | `[]` | `{V1}` | Not goal. Expand $V_1$:<br>• Child $V_2$: $g = 0 + 2 = 2$<br>• Child $V_3$: $g = 0 + 5 = 5$ |
| | *After Step 1* | | `[(V2, 2, V1), (V3, 5, V1)]` | `{V1}` | Min node is $V_2$ with cost 2. |
| **2** | **Pop $V_2$** | $2$ | `[(V3, 5, V1)]` | `{V1, V2}` | Not goal. Expand $V_2$:<br>• Child $V_3$: $g = 2 + 2 = 4$. *(Cheaper than existing cost 5! Update $V_3$ to cost 4, parent $V_2$)*.<br>• Child $V_4$: $g = 2 + 4 = 6$<br>• Child $V_5$: $g = 2 + 7 = 9$ |
| | *After Step 2* | | `[(V3, 4, V2), (V4, 6, V2), (V5, 9, V2)]` | `{V1, V2}` | Min node is $V_3$ with cost 4. |
| **3** | **Pop $V_3$** | $4$ | `[(V4, 6, V2), (V5, 9, V2)]` | `{V1, V2, V3}` | Not goal. Expand $V_3$:<br>• Child $V_5$: $g = 4 + 1 = 5$. *(Cheaper than existing cost 9 in queue! Update $V_5$ to cost 5, parent $V_3$)*. |
| | *After Step 3* | | `[(V5, 5, V3), (V4, 6, V2)]` | `{V1, V2, V3}` | Min node is $V_5$ with cost 5. |
| **4** | **Pop $V_5$** | $5$ | `[(V4, 6, V2)]` | `{V1, V2, V3, V5}` | Not goal. Expand $V_5$:<br>• Child $V_6$: $g = 5 + 2 = 7$<br>Insert $(V_6, 7, V_5)$ into queue. |
| | *After Step 4* | | `[(V4, 6, V2), (V6, 7, V5)]` | `{V1, V2, V3, V5}` | Min node is $V_4$ with cost 6. |
| **5** | **Pop $V_4$** | $6$ | `[(V6, 7, V5)]` | `{V1, V2, V3, V4, V5}`| Not goal. Expand $V_4$:<br>• Child $V_6$: $g = 6 + 3 = 9$. *(Cost 9 is worse than existing cost 7 for $V_6$! Discard)*. |
| | *After Step 5* | | `[(V6, 7, V5)]` | `{V1, V2, V3, V4, V5}`| Min node is $V_6$ with cost 7. |
| **6** | **Pop $V_6$** | **$7$** | `[]` | `{V1, V2, V3, V4, V5, V6}`| **$V_6$ IS THE GOAL NODE! STOP SEARCH!** |

---

### Final Result & Path Reconstruction
Backtracking through parent pointers from $V_6$:
- Parent of $V_6$ is $V_5$
- Parent of $V_5$ is $V_3$
- Parent of $V_3$ is $V_2$
- Parent of $V_2$ is $V_1$

$$\mathbf{Optimal\ Path:\ V_1 \longrightarrow V_2 \longrightarrow V_3 \longrightarrow V_5 \longrightarrow V_6}$$
$$\mathbf{Total\ Optimal\ Cost:\ g(V_6) = 2 + 2 + 1 + 2 = 7}$$

---

## 5. Complexity Analysis of UCS

Let:
- $C^*$ = Cost of the optimal solution path.
- $\epsilon > 0$ = Minimum positive step cost on any edge in the graph.
- $b$ = Branching factor (maximum children per node).

The algorithm expands nodes up to an effective depth:
$$d_{\text{effective}} = \left\lfloor \frac{C^*}{\epsilon} \right\rfloor + 1$$

### Asymptotic Complexities:
- **Time Complexity**:
  $$O\left(b^{1 + \lfloor C^* / \epsilon \rfloor}\right)$$
  In the worst case, UCS explores all paths whose cost is less than $C^*$. If $\epsilon$ is very small, this can be significantly larger than $b^d$.
- **Space Complexity**:
  $$O\left(b^{1 + \lfloor C^* / \epsilon \rfloor}\right)$$
  UCS must keep all generated frontier nodes in the Priority Queue and all expanded nodes in the Explored Set.

### Conditions for Completeness and Optimality
1. **Completeness**: UCS is complete (guaranteed to find a solution if one exists) if the branching factor $b$ is finite and every step cost is bounded away from zero by a small positive constant:
   $$c(n, a, n') \ge \epsilon > 0$$
2. **Optimality**: UCS is guaranteed to return the minimal cumulative cost path under the same condition ($c \ge \epsilon > 0$).
3. **What if Edge Weights are Negative or Zero?**
   - If edges have zero cost ($c = 0$), UCS can get trapped in an infinite loop of zero-cost actions without making progress toward the goal.
   - If edges have negative weights, UCS fails because adding an edge can *decrease* path cost, violating the fundamental assumption of the min-heap.

---

## 6. Equivalence to Dijkstra's Algorithm

Students often wonder: *"Isn't UCS just Dijkstra's Algorithm?"*
- **Yes!** UCS is functionally identical to Dijkstra's Shortest Path Algorithm.
- The only subtle difference in AI terminology:
  - **Dijkstra's Algorithm** was traditionally formulated to find shortest paths from a single source to *all* other vertices in a finite graph.
  - **Uniform Cost Search** is targeted for AI state spaces: it searches from a Start state toward a specific **Goal state**, stopping the instant the goal node is popped from the priority queue (early termination).

---

## 7. Real-World Applications of UCS
1. **GPS & Turn-by-Turn Navigation**: Finding the fastest or cheapest route when road segments have different travel times or toll costs.
2. **Network Packet Routing**: Internet routing protocols like **OSPF (Open Shortest Path First)** use Dijkstra/UCS principles to route data packets across links with varying latency and bandwidth weights.
3. **Robotic Motion Planning**: Navigating terrain with different friction or elevation resistance costs.

---

## 🎯 Exam Practice Checklist: Questions to Master
- [ ] Why does standard BFS fail to find optimal paths on weighted graphs?
- [ ] State the exact condition under which Goal Test is executed in UCS.
- [ ] Reproduce the 6-step Priority Queue trace table for the $V_1 \to V_6$ graph.
- [ ] State the time and space complexity of UCS and define $C^*$ and $\epsilon$.
- [ ] What happens to UCS if edge weights can be zero or negative?

---
*Next Topic: [Module 04: Iterative Deepening DFS (IDDFS)](./04_Iterative_Deepening_DFS_IDDFS.md)*
