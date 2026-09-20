# 🧩 Module 02: Problem Formulation & Classic AI Puzzles
**Reference Source**: `Basics_of_Problem_Solving.pdf` (Dr. Ritesh Kumar, IIIT Surat)  
**Estimated Study Time**: 45 – 50 Minutes  
**Exam Importance**: Extremely High (Guaranteed 15–20 marks: Water Jug, Cryptarithmetic, Cannibals, State-Space Components)

---

## 🧭 Executive Summary: What This Module Is About

In AI, before an algorithm can solve a problem, a human engineer must formulate that problem into a rigorous mathematical structure. This module teaches:
1. The **4-step cycle** of a problem-solving agent.
2. The **3 representation paradigms** (State-Space, AND-OR graphs, Production Systems).
3. The **5-tuple formal problem formulation**.
4. The difference between **Satisfiability** and **Optimality**.
5. Complete, step-by-step solved traces of the **5 classic AI problems**:
   - The Water Jug Problem (4L & 3L jugs $\to$ 2L)
   - Missionaries and Cannibals (3M, 3C river crossing)
   - The 8-Puzzle (Inversion parity theorem)
   - Cryptarithmetic ($SEND + MORE = MONEY$)
   - The 8-Queens / 4-Queens Problem

---

## 1. What Is Problem Solving in AI?

### Formal Definition
> *"Problem solving in AI is the systematic process of finding a sequence of actions that leads from an initial state to a desired goal state, using a formal computational representation."*

### The 4-Phase Problem-Solving Cycle
```
[ 1. Goal Formulation ] ──> [ 2. Problem Formulation ] ──> [ 3. Search ] ──> [ 4. Execution ]
  Decide what states          Define states, actions,       Simulate paths to find    Carry out the actions
   are desirable               transition model, costs       a sequence to the goal    in the physical world
```

1. **Goal Formulation**: The agent establishes its targets based on current percepts and performance measures (e.g., "Reach Bucharest from Arad"). Goals help organize behavior by limiting the objectives the agent tries to achieve.
2. **Problem Formulation**: Deciding what actions and states to consider, given the goal. It abstracts away irrelevant details (e.g., we care about road connections, not the color of the asphalt or trees along the road).
3. **Search**: An offline exploration phase. The agent simulates sequences of actions in its internal model until it discovers a path to the goal.
4. **Execution**: The agent takes the planned actions one by one in the real physical environment.

---

## 2. The Three Problem Representation Paradigms

How you represent a problem determines what algorithms can solve it:

```
+---------------------------------------------------------------------------------------+
|                              REPRESENTATION PARADIGMS                                 |
+---------------------------+-------------------------------+---------------------------+
| 1. STATE-SPACE            | 2. PROBLEM REDUCTION (AND-OR) | 3. PRODUCTION SYSTEMS     |
| • States + Operators      | • Decomposing goal into       | • Rule-based reasoning:   |
| • Explored as graph/tree  |   sub-problems                |   IF condition THEN action|
| • Best for: Pathfinding,  | • Best for: Symbolic math,    | • Best for: Expert        |
|   games, route search     |   complex planning            |   systems, puzzle rules   |
+---------------------------+-------------------------------+---------------------------+
```

### 1. State-Space Representation
- The world is modeled as a set of discrete **States** connected by **Operators (Actions)**.
- Solving the problem equals finding a connected path from the start node to a goal node in a graph.
- *Examples*: 8-Puzzle, Route finding, Chess.

### 2. Problem-Reduction Representation (AND–OR Graphs)
- Rather than searching paths, a complex goal is recursively broken down into smaller, simpler **sub-problems**:
  - **OR Branch**: Any ONE sub-problem solution suffices to solve the parent goal.
  - **AND Branch**: ALL sub-problems MUST be solved together to solve the parent goal.
- *Examples*: Symbolic integration in calculus $\int (u + v)dx = \int u dx + \int v dx$ (both must be solved = AND); Tower of Hanoi; complex hardware assembly.

### 3. Production System Representation
- A foundational architecture in symbolic AI and expert systems. It consists of three core components:
  1. **Working Memory (Global Database)**: Contains current facts, state variables, and assertions about the world.
  2. **Production Rules (Rule Base)**: A collection of condition-action rules:
     $$\text{IF } \langle\text{condition}\rangle \text{ THEN } \langle\text{action / modification to working memory}\rangle$$
  3. **Inference Engine & Conflict Resolution**: Matches rules against working memory. If multiple rules match simultaneously (a **conflict set**), it uses a strategy to pick one:
     - **Specificity**: Pick the rule with the most specific conditions.
     - **Recency**: Pick the rule using the most recently added facts.
     - **Refraction**: Prevent the exact same rule from firing repeatedly on unchanged facts.
     - **Rule Priority**: Fixed manual priorities assigned to rules.

---

## 3. Formal 5-Tuple Problem Formulation

To feed a problem into any search algorithm, you must formally define these **5 components**:

1. **Initial State ($s_0$)**: The starting configuration of the agent.
2. **Actions ($Actions(s)$)**: The legal moves available to the agent in a given state $s$.
3. **Transition Model ($Result(s, a)$)**: Returns the resulting state $s'$ reached by executing action $a$ in state $s$.
4. **Goal Test ($GoalTest(s)$)**: A boolean function that checks whether state $s$ satisfies the goal conditions (can be an explicit state or an abstract property like "no two queens attack each other").
5. **Path Cost ($c(s, a, s')$)**: A numeric cost function that assigns a step cost to doing action $a$ in state $s$ to reach $s'$. Total path cost is the sum of step costs.

> **State Space Definition**: The set of all states reachable from the initial state $s_0$ by any sequence of actions, forming a directed graph.

---

## 4. Search Tree vs. Search Graph

When searching a state space, should you use a **Tree** or a **Graph**?

```
          SEARCH TREE                                     SEARCH GRAPH
      (Suffers from Cycles!)                           (Cycle-Safe & Fast)

              [ A ]                                           [ A ]
             /     \                                         /     \
          [ B ]   [ C ]                                   [ B ]   [ C ]
          /   \                                            \     /
       [ A ]  [ D ]   <-- Repeated state A!                 [ D ]
       /   \              Infinite loop risk!
    [ B ]  ...                                     Maintains an EXPLORED SET
                                                   (Closed List). If node is already
                                                   explored, discard it immediately!
```

- **Search Tree**: Generates paths branching outward. **Fatal Flaw**: If the state space has undirected edges or cycles (e.g., moving between Room A and Room B), a search tree will generate duplicate nodes endlessly, leading to an **infinite loop** and memory crash.
- **Search Graph**: Maintains an **Explored Set (Closed List)**. Whenever a state is expanded, it is recorded. If an action leads to an already explored state, it is pruned immediately. **Always use Graph Search for cyclic problems!**

---

## 5. Satisfiability vs. Optimality

AI problems split fundamentally into two classes based on what constitutes an acceptable solution:

| Aspect | Satisfiability Problem | Optimality / Optimization Problem |
| :--- | :--- | :--- |
| **Goal Objective** | Find **any valid solution** that satisfies all constraints. | Find the **best possible solution** that minimizes (or maximizes) cost. |
| **Path Cost Relevance** | Path cost is completely irrelevant. Only final state matters. | Path cost is paramount. Total path cost must be minimized. |
| **Classic Examples** | 8-Queens, Sudoku, Cryptarithmetic, Boolean SAT. | Shortest Route (GPS), Travelling Salesperson Problem (TSP), Job-Shop Scheduling. |
| **Typical Algorithms** | DFS, Backtracking, Hill Climbing, Constraint Satisfaction. | Uniform Cost Search (UCS), A* Search, Dijkstra, Branch & Bound. |
| **Stopping Condition** | Stops the instant the **first valid state** passes the Goal Test. | Must explore until it guarantees **no cheaper path exists**. |
| **Computational Effort** | Usually faster; stops early once a solution is found. | Slower; requires exploring competing branches to verify optimality. |

---

## 6. Pattern Classification Problems

In many real-world domains, the task is not pathfinding, but **categorization**: assigning an input pattern to one of several predefined classes.

```
+-----------+       +----------------------+       +------------------+       +------------------+
| Raw Input | ───>  | Preprocessing &      | ───>  | Classifier /     | ───>  | Predicted Class  |
| (Image/   |       | Feature Extraction   |       | Decision Rule    |       | Label            |
|  Audio)   |       | (Vector of numbers)  |       | (Searched Model) |       | ("Spam", "Cat")  |
+-----------+       +----------------------+       +------------------+       +------------------+
```

### Connection to Search
How is pattern classification related to AI search?
- Creating a classifier can be formulated as **searching through a hypothesis space**: finding the optimal mathematical decision boundary (weights/parameters) that minimizes classification error on training data.
- Applications: Optical Character Recognition (OCR), Spam detection, Medical diagnosis from X-rays, Biometric fingerprint verification.

---

## 7. Deep-Dive Solved Classic AI Problems

Examiners love asking students to formulate and trace these problems. Memorize these exact formulations!

---

### 💧 Problem 1: The Water Jug Problem

#### Problem Statement
You are given two jugs: a **4-gallon jug** (Jug A) and a **3-gallon jug** (Jug B). Neither jug has measuring markings. You have an unlimited tap of water and a sink.  
**Goal**: Measure **exactly 2 gallons** in the 4-gallon jug.

#### Formal Formulation
- **State**: A pair of integers $(x, y)$, where:
  - $x \in \{0, 1, 2, 3, 4\}$ = amount of water currently in the 4-gallon jug.
  - $y \in \{0, 1, 2, 3\}$ = amount of water currently in the 3-gallon jug.
- **Initial State**: $(0, 0)$ (both jugs empty).
- **Goal Test**: $x = 2$ (the 4-gallon jug has exactly 2 gallons; $y$ can be anything).
- **Path Cost**: 1 per step (minimize number of pourings).

#### The 8 Production Rules
```
Rule 1: IF x < 4 THEN (4, y)                   -- Fill 4-gal jug completely
Rule 2: IF y < 3 THEN (x, 3)                   -- Fill 3-gal jug completely
Rule 3: IF x > 0 THEN (0, y)                   -- Empty 4-gal jug onto ground
Rule 4: IF y > 0 THEN (x, 0)                   -- Empty 3-gal jug onto ground
Rule 5: IF x + y >= 4 AND y > 0 THEN (4, y - (4 - x))  -- Pour 3-gal into 4-gal until 4-gal is full
Rule 6: IF x + y >= 3 AND x > 0 THEN (x - (3 - y), 3)  -- Pour 4-gal into 3-gal until 3-gal is full
Rule 7: IF x + y <= 4 AND y > 0 THEN (x + y, 0)        -- Pour all water from 3-gal into 4-gal
Rule 8: IF x + y <= 3 AND x > 0 THEN (0, x + y)        -- Pour all water from 4-gal into 3-gal
```

#### Step-by-Step Solved Solution Trace (Memorize this sequence!)
| Step | State $(x, y)$ | Action Applied | Plain English Explanation |
| :---: | :---: | :--- | :--- |
| **0** | **$(0, 0)$** | Initial State | Both jugs start completely empty. |
| **1** | **$(0, 3)$** | Apply Rule 2 | Fill the 3-gallon jug to full capacity. |
| **2** | **$(3, 0)$** | Apply Rule 7 | Pour all 3 gallons from Jug B into Jug A. |
| **3** | **$(3, 3)$** | Apply Rule 2 | Fill the 3-gallon jug to full capacity again. |
| **4** | **$(4, 2)$** | Apply Rule 5 | Pour from Jug B into Jug A until Jug A is full (takes 1 gal, leaving 2 gal in Jug B!). |
| **5** | **$(0, 2)$** | Apply Rule 3 | Empty the 4-gallon jug completely. |
| **6** | **$(2, 0)$** | Apply Rule 7 | Pour the 2 gallons from Jug B into Jug A. **GOAL REACHED ($x=2$)!** |

*Total Steps: 6 moves. (Satisfies Goal Test).*

---

### 🛶 Problem 2: The Missionaries and Cannibals Problem

#### Problem Statement
Three missionaries ($M$) and three cannibals ($C$) are on the left bank of a river. They have a boat that can hold at most **2 people**.  
**Constraint**: If cannibals ever outnumber missionaries on either bank of the river, the cannibals will eat the missionaries.  
**Goal**: Transport all 6 people safely to the right bank.

#### Formal Formulation
- **State Vector**: $(M_L, C_L, B)$, where:
  - $M_L \in \{0, 1, 2, 3\}$: Number of missionaries on the Left bank.
  - $C_L \in \{0, 1, 2, 3\}$: Number of cannibals on the Left bank.
  - $B \in \{1, 0\}$: Position of the boat ($1 = \text{Left bank}, 0 = \text{Right bank}$).
  *(People on right bank are implicitly $M_R = 3 - M_L$, $C_R = 3 - C_L$)*.
- **Initial State**: $(3, 3, 1)$
- **Goal State**: $(0, 0, 0)$
- **Safety Condition**: For both banks, $M$ cannot be less than $C$ unless $M = 0$:
  $$(M_L = 0 \text{ OR } M_L \ge C_L) \quad \text{AND} \quad ((3 - M_L) = 0 \text{ OR } (3 - M_L) \ge (3 - C_L))$$
- **Legal Boat Moves**: Between 1 and 2 people in the boat:
  $$\Delta(M, C) \in \{(1,0), (2,0), (0,1), (0,2), (1,1)\}$$

#### Complete 11-Step Safe Solution Trace
| Step | Left Bank $(M_L, C_L)$ | Boat | Right Bank $(M_R, C_R)$ | Boat Move | Reason & Validity |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | **$(3, 3)$** | **L (1)** | **$(0, 0)$** | Initial State | 3M, 3C on left bank. |
| **1** | $(3, 1)$ | R (0) | $(0, 2)$ | Send 2 Cannibals $\to$ | Left: 3M, 1C (Safe: $3 \ge 1$). Right: 2C (Safe). |
| **2** | $(3, 2)$ | L (1) | $(0, 1)$ | 1 Cannibal returns $\leftarrow$ | Left: 3M, 2C (Safe: $3 \ge 2$). |
| **3** | $(3, 0)$ | R (0) | $(0, 3)$ | Send 2 Cannibals $\to$ | Left: 3M, 0C (Safe). Right: 3C (Safe). |
| **4** | $(3, 1)$ | L (1) | $(0, 2)$ | 1 Cannibal returns $\leftarrow$ | Left: 3M, 1C (Safe). |
| **5** | $(1, 1)$ | R (0) | $(2, 2)$ | Send 2 Missionaries $\to$ | Left: 1M, 1C ($1 \ge 1$, Safe!). Right: 2M, 2C ($2 \ge 2$, Safe!). |
| **6** | $(2, 2)$ | L (1) | $(1, 1)$ | 1M + 1C return $\leftarrow$ | Crucial trick! Left: 2M, 2C (Safe). Right: 1M, 1C (Safe). |
| **7** | $(0, 2)$ | R (0) | $(3, 1)$ | Send 2 Missionaries $\to$ | Left: 0M, 2C (Safe: 0M). Right: 3M, 1C (Safe: $3 \ge 1$). |
| **8** | $(0, 3)$ | L (1) | $(3, 0)$ | 1 Cannibal returns $\leftarrow$ | Left: 3C. Right: 3M (Safe). |
| **9** | $(0, 1)$ | R (0) | $(3, 2)$ | Send 2 Cannibals $\to$ | Left: 1C. Right: 3M, 2C ($3 \ge 2$, Safe). |
| **10**| $(0, 2)$ | L (1) | $(3, 1)$ | 1 Cannibal returns $\leftarrow$ | Left: 2C. Right: 3M, 1C (Safe). |
| **11**| **$(0, 0)$** | **R (0)** | **$(3, 3)$** | Send 2 Cannibals $\to$ | **ALL SAFELY ON RIGHT BANK! GOAL!** |

---

### 🔢 Problem 3: The 8-Puzzle Problem

#### Problem Statement
A $3 \times 3$ board contains 8 numbered sliding tiles and one blank space. Tiles adjacent to the blank space can slide into it.  
**Goal**: Reach a specified goal tile arrangement.

```
    INITIAL STATE               GOAL STATE
    +---+---+---+             +---+---+---+
    | 1 | 2 | 3 |             | 1 | 2 | 3 |
    +---+---+---+             +---+---+---+
    | 4 |   | 6 |     ───>    | 4 | 5 | 6 |
    +---+---+---+             +---+---+---+
    | 7 | 5 | 8 |             | 7 | 8 |   |
    +---+---+---+             +---+---+---+
```

#### Formal Formulation
- **State**: A $3 \times 3$ grid of numbers $\{1, \dots, 8, \text{blank}\}$.
- **Actions**: Move blank tile $\{\text{Up}, \text{Down}, \text{Left}, \text{Right}\}$ (subject to board boundary constraints).
- **Transition Model**: Swaps the blank tile with the adjacent tile in the specified direction.
- **Goal Test**: Matches the designated target grid.
- **Path Cost**: 1 per move.

#### 💡 The Solvability & Inversion Parity Theorem (Exam Favorite!)
Not all random initial states can reach a given goal state! Exactly **50% of all initial 8-puzzle configurations are unsolvable**.
- **Definition of an Inversion**: Write the tiles in row-major order (ignoring the blank). An inversion is a pair of tiles $(a, b)$ where $a > b$ but $a$ appears *before* $b$.
- **The Parity Rule for Odd Grid Width ($3 \times 3$)**:
  - Sliding a tile horizontally does not change the tile sequence $\implies$ Inversion count is unchanged.
  - Sliding a tile vertically shifts it past exactly 2 other tiles $\implies$ Inversion count changes by $+2, 0$, or $-2$ (the parity of inversions never changes!).
  - **Theorem**: A configuration can reach the goal if and only if:
    $$\text{Inversion\_Count}(\text{Initial State}) \equiv \text{Inversion\_Count}(\text{Goal State}) \pmod 2$$
    *(Both must have either an even number of inversions or an odd number of inversions).*

---

### 🔠 Problem 4: Cryptarithmetic ($SEND + MORE = MONEY$)

#### Problem Statement
Assign a distinct decimal digit $\{0, 1, 2, \dots, 9\}$ to each letter in the equation:
$$\begin{array}{r@{\quad}c@{\quad}c@{\quad}c@{\quad}c@{\quad}c}
  & S & E & N & D \\
+ & M & O & R & E \\
\hline
M & O & N & E & Y \\
\end{array}$$
**Constraints**:
1. Each letter represents a unique digit ($\text{AllDifferent}(S, E, N, D, M, O, R, Y)$).
2. No leading zeros: $S \ne 0$ and $M \ne 0$.
3. The column arithmetic with carries must hold exactly.

#### Step-by-Step Mathematical Deduction (Write this down step-by-step in exam!)

```
  Carries:   c4   c3   c2   c1
              S    E    N    D
         +    M    O    R    E
         ──────────────────────
         M    O    N    E    Y
```

1. **Deduce $M$**:
   - The sum of two 4-digit numbers can at most produce a 5-digit number with leading digit 1 (maximum sum is $9999 + 9999 = 19998$).
   - Therefore, the carry into the 5th column $c_4 = 1$.
   - Since $M$ is the leading digit of $MONEY$, **$M = 1$**.

2. **Deduce $O$ and $S$**:
   - Look at Column 4 (Thousands column): $S + M + c_3 = O + 10 \times c_4 = O + 10$.
   - Since $M = 1$: $S + 1 + c_3 = O + 10 \implies S + c_3 = O + 9$.
   - The carry $c_3$ can only be $0$ or $1$.
   - Since letters are unique, $O$ cannot equal $1$ (since $M = 1$).
   - If $S + c_3 = O + 9$, and digits are $\le 9$:
     - For the sum to be $\ge 9$, $S$ must be $8$ or $9$.
     - If $S = 9$: $9 + c_3 = O + 9 \implies O = c_3$.
       - If $c_3 = 1$, then $O = 1$ (Contradiction! $M=1$, so $O$ cannot be 1).
       - Therefore, $c_3 = 0$, which gives **$O = 0$** and **$S = 9$**!

3. **Deduce $E$ and $N$**:
   - Look at Column 3 (Hundreds column): $E + O + c_2 = N + 10 \times c_3$.
   - We know $O = 0$ and $c_3 = 0$:
     $$E + 0 + c_2 = N \implies N = E + c_2$$
   - Since $N \ne E$ (all letters are distinct), $c_2$ cannot be 0.
   - Therefore, **$c_2 = 1$**, which means:
     $$N = E + 1$$

4. **Deduce $R$**:
   - Look at Column 2 (Tens column): $N + R + c_1 = E + 10 \times c_2 = E + 10$.
   - Substitute $N = E + 1$:
     $$(E + 1) + R + c_1 = E + 10$$
     $$E + 1 + R + c_1 = E + 10 \implies R + c_1 = 9$$
   - Can $c_1 = 0$? If $c_1 = 0$, then $R = 9$. But $S = 9$ already! Contradiction.
   - Therefore, **$c_1 = 1$**, which immediately gives **$R = 8$**!

5. **Deduce $D, E, Y$**:
   - Look at Column 1 (Units column): $D + E = Y + 10 \times c_1 = Y + 10$.
   - Since $c_1 = 1$, $D + E \ge 12$ (to produce a carry and a distinct $Y$).
   - Let's check remaining available digits:
     - Digits used so far: $\{M=1, O=0, S=9, R=8\}$.
     - Remaining digits: $\{2, 3, 4, 5, 6, 7\}$.
   - We know $N = E + 1$.
   - If $E = 5 \implies N = 6$ (both available!).
   - Then from $D + E = Y + 10$:
     $$D + 5 = Y + 10 \implies D = Y + 5$$
   - From remaining digits $\{2, 3, 4, 7\}$:
     - If $Y = 2 \implies D = 2 + 5 = 7$ (Both available!).
   - Let's verify all assignments:
     - **$S = 9, E = 5, N = 6, D = 7$**
     - **$M = 1, O = 0, R = 8, E = 5, Y = 2$**

#### Arithmetic Verification:
$$\begin{array}{r@{\quad}c@{\quad}c@{\quad}c@{\quad}c}
  & 9 & 5 & 6 & 7 \\
+ & 1 & 0 & 8 & 5 \\
\hline
1 & 0 & 6 & 5 & 2 \\
\end{array} \quad \checkmark \textbf{ PERFECT MATCH!}$$

---

### 👑 Problem 5: The 8-Queens / 4-Queens Problem

#### Problem Statement
Place 8 queens on an $8 \times 8$ chessboard (or 4 queens on a $4 \times 4$ board) such that no two queens attack each other. (No two queens share the same row, column, or diagonal).

#### Formal Formulation
- **Satisfiability Classification**: Pure Constraint Satisfaction Problem (CSP). Any conflict-free state is accepted; path cost is irrelevant.
- **State Representation**: Vector $[r_1, r_2, r_3, r_4]$, where $r_i$ denotes the row position of the queen in column $i$.
- **4-Queens Solved Arrangement**:
  $$\text{Board} = [2, 4, 1, 3] \quad \text{or} \quad [3, 1, 4, 2]$$

```
    Visual Grid for [3, 1, 4, 2]:
         Col 1   Col 2   Col 3   Col 4
    Row 1:  .       Q       .       .
    Row 2:  .       .       .       Q
    Row 3:  Q       .       .       .
    Row 4:  .       .       Q       .
    (Notice: No two queens share a row, column, or diagonal!)
```

---

## 🎯 Exam Practice Checklist: Questions to Master
- [ ] List the 5 components of formal problem formulation with their notations.
- [ ] Compare Satisfiability vs Optimality in a 4-point table.
- [ ] Write the 8 production rules for the Water Jug Problem and trace the 6-step solution.
- [ ] Solve the Missionaries and Cannibals problem from $(3,3,1)$ to $(0,0,0)$.
- [ ] Show the mathematical proof of $SEND + MORE = MONEY$ deriving $M=1, S=9, O=0$.

---
*Next Topic: [Module 03: Uninformed Search and Uniform Cost Search](./03_Uninformed_Search_and_Uniform_Cost_Search.md)*
