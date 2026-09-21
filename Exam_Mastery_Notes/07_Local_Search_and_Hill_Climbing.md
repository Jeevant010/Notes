# ⛰️ Module 07: Local Search and Hill Climbing
**Reference Source**: `Hill_Climbing_Algorithm.pdf` (Artificial Intelligence — Local Search)  
**Estimated Study Time**: 35 – 40 Minutes  
**Exam Importance**: Very High (Guaranteed 10–12 marks: 3 Variants, 4-Queens Attack Calculation, 3 Traps & Escapes: Simulated Annealing & Random Restarts)

---

## 🧭 Executive Summary: What This Module Is About

Up to this point, our search algorithms (BFS, DFS, UCS, A*) were designed to find a **path** from a start state to a goal state. But for many real-world problems—like placing components on a microchip, scheduling aircraft, or solving N-Queens—**we don't care how we got there; we only care about the final state itself!**

This is the domain of **Local Search & Optimization**:
- Operates on a single current state (does not keep a search tree!).
- Uses **$O(1)$ constant memory**!
- Climbs toward the peak of an objective landscape.

In this module, we master:
1. The **Foggy Mountain Climbing analogy**.
2. The **3 Variants** of Hill Climbing (Simple, Steepest-Ascent, Stochastic).
3. Complete step-by-step trace of the **4-Queens Problem**.
4. The **3 Classical Landscape Traps** (Local Maxima, Plateaus, Ridges) and how to escape them using **Random Restarts** and **Simulated Annealing**.
5. Mathematical 1D optimization trace of $f(x) = -(x-4)^2 + 10$.

---

## 1. What Is Hill Climbing?

### Formal Definition
> *"Hill Climbing is an iterative, heuristic local-search optimization technique that starts with an arbitrary initial solution and repeatedly moves to a neighboring solution with a higher (better) evaluation value, terminating when no neighboring state improves upon the current state."*

```
                     Peak (Global Maximum)
                           /\
                          /  \
                         /    \
                        /      \
        Local Peak     /        \
           /\         /          \
          /  \_______/            \
         /     Plateau             \
   Start
```

### The Foggy Mountain Analogy
Imagine you are hiking on a mountain in a **thick, dense fog**:
- You cannot see the summit or the surrounding valleys.
- You can only feel the slope of the ground directly beneath your boots.
- **The Hill Climbing Strategy**: You take a step in whichever direction feels like it is sloping uphill. If every direction around you slopes downhill, you conclude you have reached the summit and stop!

### Why Use Local Search?
1. **Astronomical Search Spaces**: In problems like N-Queens or Travelling Salesperson, the state space is factorial ($N!$) or exponential ($b^d$). Generating search trees is computationally impossible.
2. **Virtually Zero Memory Footprint**: Hill Climbing tracks **only the current state**. It uses **$O(1)$ memory**, unlike A* or BFS which store millions of nodes in memory frontiers.
3. **Anytime Algorithm**: If stopped at any arbitrary moment, it returns the best solution discovered so far.

---

## 2. The Core Algorithm & Pseudocode

```python
def hill_climbing(initial_state):
    current = initial_state
    
    while True:
        # Find the best neighbor among all immediate neighbors
        neighbor = get_best_neighbor(current)
        
        # Stopping Rule: If no neighbor is strictly better, we reached a peak!
        if value(neighbor) <= value(current):
            return current
            
        current = neighbor
```

---

## 3. The Three Common Variants of Hill Climbing

Examiners frequently ask you to compare how these three variants choose their next move:

| Variant | How Next Move Is Chosen | Computational Cost per Step | Key Trade-off |
| :--- | :--- | :--- | :--- |
| **1. Simple Hill Climbing** | Evaluates neighbors **one by one sequentially**; moves to the **very first neighbor** that is better than the current state. | **Very Low**: Stops generating neighbors as soon as one improving move is found. | Fast, but may miss much better slopes nearby; highly dependent on neighbor ordering. |
| **2. Steepest-Ascent Hill Climbing** *(Gradient Search)* | Evaluates **ALL neighbors** simultaneously; selects the single neighbor with the **highest improvement** (steepest uphill slope). | **Moderate**: Must evaluate all $k$ neighbors before making a single move. | Higher quality moves; makes faster progress toward local peaks, but costs more compute per step. |
| **3. Stochastic Hill Climbing** | Chooses a move **at random** from among the uphill (improving) neighbors, with the probability of selection weighted by the steepness of the improvement. | **Moderate**: Evaluates neighbors and samples probabilistically. | Introduces controlled randomness; helps escape certain narrow traps. |

---

## 4. Worked Example: The 4-Queens Problem (Step-by-Step)

### Problem Setup
Place 4 queens on a $4 \times 4$ board so that no two queens attack each other (no two queens share a row, column, or diagonal).

- **State Representation**: A 4-element array $[r_1, r_2, r_3, r_4]$, where $r_c$ denotes the row number of the queen in column $c$.
- **Move Rule**: Move any single queen up or down within its own column.
- **Objective Function $h(x)$**: The number of **mutually attacking pairs of queens**.  
  *(Goal: Minimize $h(x)$ down to $\mathbf{h(x) = 0}$)*.

---

### Step-by-Step Execution Trace of Steepest-Ascent Search

#### Step 0: Initial State $[2, 1, 4, 3]$
```
    Col 1   Col 2   Col 3   Col 4
Row 1:  .       Q       .       .
Row 2:  Q       .       .       .
Row 3:  .       .       .       Q
Row 4:  .       .       Q       .
```
- Let's count attacking pairs ($h(x)$):
  1. Queen at $(2, 1)$ attacks Queen at $(1, 2)$ diagonally ($\Delta \text{row} = 1, \Delta \text{col} = 1$).
  2. Queen at $(1, 2)$ attacks Queen at $(3, 4)$ diagonally ($\Delta \text{row} = 2, \Delta \text{col} = 2$).
  3. Queen at $(4, 3)$ attacks Queen at $(3, 4)$ diagonally ($\Delta \text{row} = 1, \Delta \text{col} = 1$).
  4. Queen at $(2, 1)$ attacks Queen at $(4, 3)$ diagonally ($\Delta \text{row} = 2, \Delta \text{col} = 2$).
- **Current Heuristic**: $\mathbf{h(x) = 4}$

---

#### Step 1: Move Queen in Column 1 from Row 2 to Row 1
- **New State**: $[1, 1, 4, 3]$
```
    Col 1   Col 2   Col 3   Col 4
Row 1:  Q       Q       .       .  <-- Row clash!
Row 2:  .       .       .       .
Row 3:  .       .       .       Q
Row 4:  .       .       Q       .
```
- Remaining attacking pairs: $(1,1)-(1,2)$, $(1,2)-(3,4)$, $(4,3)-(3,4)$.
- **New Heuristic**: $\mathbf{h(x) = 3}$ *(Improved from 4 to 3!)*

---

#### Step 2: Move Queen in Column 4 from Row 3 to Row 2
- **New State**: $[1, 1, 4, 2]$
```
    Col 1   Col 2   Col 3   Col 4
Row 1:  Q       Q       .       .
Row 2:  .       .       .       Q
Row 3:  .       .       .       .
Row 4:  .       .       Q       .
```
- Only one attacking pair remains: Queen at $(1,1)$ attacks Queen at $(1,2)$ in row 1.
- **New Heuristic**: $\mathbf{h(x) = 1}$ *(Improved from 3 to 1!)*

---

#### Step 3: Move Queen in Column 1 from Row 1 to Row 3
- **New State**: $[3, 1, 4, 2]$
```
    Col 1   Col 2   Col 3   Col 4
Row 1:  .       Q       .       .
Row 2:  .       .       .       Q
Row 3:  Q       .       .       .
Row 4:  .       .       Q       .
```
- Verify all conflicts:
  - Column 1 at row 3: No horizontal conflict; diagonals clear.
  - Column 2 at row 1: Clear.
  - Column 3 at row 4: Clear.
  - Column 4 at row 2: Clear.
- **New Heuristic**: $\mathbf{h(x) = 0}$

$$\mathbf{GOAL\ REACHED\ IN\ 3\ MOVES!\ Solution:\ [3, 1, 4, 2]}$$

---

## 5. The Three Classic Landscape Traps (Must-Know for Exams!)

Why does basic Hill Climbing fail on complex landscapes? It gets trapped in three distinct topographical features:

```
        1. LOCAL MAXIMUM                 2. PLATEAU / SHOULDER                 3. RIDGE

              /\                                _________                         /\
             /  \                              /         \                       /  \
            /    \   <-- Trapped here!        /           \                     /    \
           /      \                          /             \                   /  /\  \
          /        \                        /               \                 /  /  \  \
         /          \                      Flat region! Zero                 Narrow diagonal crest.
     Higher peak     Lower peak            gradient in all directions.       Orthogonal steps go downhill!
     exists elsewhere!
```

---

### 1. Local Maximum
- **Definition**: A state that has a higher evaluation value than all of its immediate neighbors, but is **lower than the global maximum**.
- **Why Algorithm Fails**: Because all neighboring states lead downhill, the algorithm's stopping condition triggers: `if value(neighbor) <= value(current): return current`. The algorithm halts, believing it has reached the top.

---

### 2. Plateau / Flat Region
- **Definition**: A flat area of the state space where all neighboring states have the **exact same evaluation value** ($h(neighbor) = h(current)$).
- **Two Variations**:
  - **Flat Local Maximum**: A flat top from which no uphill path exists.
  - **Shoulder**: A flat region that eventually leads uphill if you keep walking across it.
- **Why Algorithm Fails**: The gradient is zero ($\nabla = 0$). The algorithm has no signal or direction to guide which way to move, leading to random wandering or infinite stalling.

---

### 3. Ridge
- **Definition**: A narrow, elevated spine sloping upward along a **diagonal direction**.
- **Why Algorithm Fails**: Most search operators adjust **one variable at a time** (orthogonal moves along coordinate axes). On a steep diagonal ridge, any single-step move along the $x$-axis or $y$-axis steps off the ridge and goes downhill! As a result, the algorithm thinks it is at a peak and halts, or oscillates pointlessly from side to side.

---

## 6. How to Escape the Traps: Modern Enhancements

Examiners will ask: *"How do you modify Hill Climbing to overcome local maxima, plateaus, and ridges?"* State these four techniques:

### 1. Random-Restart Hill Climbing
- **Core Philosophy**: *"If at first you don't succeed, try, try again from a different random starting point!"*
- **Mechanism**: Run standard hill climbing for $k$ iterations, each time picking a completely random initial state. Save the best peak found across all runs.
- **Optimality & Completeness**: **Complete in the limit!** If the probability of finding the global maximum in a single run is $p$, the probability of failure after $k$ random restarts is $(1 - p)^k$, which approaches $0$ as $k \to \infty$.

---

### 2. Sideways Moves (Plateau Crossing)
- **Mechanism**: When all neighbors have the same value ($value(neighbor) == value(current)$), allow the algorithm to make a "sideways move".
- **Safety Precaution**: To prevent getting stuck in an infinite loop on a flat plateau, impose a **maximum sideways moves limit** (e.g., allow at most 100 consecutive sideways moves before terminating).

---

### 3. Simulated Annealing (The Metallurgical Escape)
- **Inspiration**: From metallurgy, where metal is heated to high temperatures (atoms move wildly) and then cooled slowly (**annealed**) so that its crystals settle into a state of minimal energy (maximum strength).
- **Key Idea**: Instead of only picking uphill moves, **occasionally allow DOWNHILL (worse) moves** to escape local maxima!
- **The Metropolis Probability Formula**:
  If a neighbor is better ($\Delta E > 0$), always accept it.  
  If a neighbor is worse ($\Delta E \le 0$), accept it with probability:
  $$\mathbf{P = e^{\frac{\Delta E}{T}}}$$
  Where:
  - $\Delta E = value(neighbor) - value(current) < 0$ (the amount of penalty).
  - $T > 0$ is the **Temperature parameter**, which decreases over time according to a cooling schedule.

```
  When Temperature T is HIGH (Early on):
  • P is close to 1. The algorithm explores wildly, easily jumping OUT of local maxima!
  
  When Temperature T cools down to ZERO (Later on):
  • P approaches 0. Downhill moves are rejected; algorithm settles smoothly into the global peak!
```

---

### 4. Tabu Search
- Maintains a short-term memory (a **Tabu List**) of recently visited states.
- Prevents the algorithm from revisiting recently explored states, breaking infinite loops on plateaus and ridges.

---

## 7. Mathematical 1D Function Trace

Let's trace Hill Climbing maximizing the mathematical objective function:
$$\mathbf{f(x) = -(x - 4)^2 + 10}$$
- Starting state: $x_0 = 0$.
- Step size: $\Delta x = \pm 1$. Legal neighbors of $x$ are $\{x - 1, x + 1\}$.

### Step-by-Step Execution:
1. **Start at $x = 0$**:
   - $f(0) = -(0 - 4)^2 + 10 = -16 + 10 = \mathbf{-6}$
   - Evaluate neighbors:
     - Left: $x = -1 \implies f(-1) = -(-1-4)^2 + 10 = -25 + 10 = -15$
     - Right: $x = 1 \implies f(1) = -(1-4)^2 + 10 = -9 + 10 = \mathbf{+1}$
   - Move to $x = 1$ ($+1 > -6$).

2. **At $x = 1$ ($f=1$)**:
   - Evaluate Right: $x = 2 \implies f(2) = -(2-4)^2 + 10 = -4 + 10 = \mathbf{+6}$
   - Move to $x = 2$.

3. **At $x = 2$ ($f=6$)**:
   - Evaluate Right: $x = 3 \implies f(3) = -(3-4)^2 + 10 = -1 + 10 = \mathbf{+9}$
   - Move to $x = 3$.

4. **At $x = 3$ ($f=9$)**:
   - Evaluate Right: $x = 4 \implies f(4) = -(4-4)^2 + 10 = 0 + 10 = \mathbf{+10}$
   - Move to $x = 4$.

5. **At $x = 4$ ($f=10$)**:
   - Evaluate Left: $x = 3 \implies f(3) = 9$
   - Evaluate Right: $x = 5 \implies f(5) = -(5-4)^2 + 10 = -1 + 10 = 9$
   - Both neighbors have value $9 \le 10$.
   - **Stopping Condition Triggered!** Halt and return $x = 4$.
   - **Peak Found**: Global Maximum at $\mathbf{x = 4}$ with value $\mathbf{f(x) = 10}$!

---

## 🎯 Exam Practice Checklist: Questions to Master
- [ ] Define Hill Climbing and state its memory complexity.
- [ ] Contrast Simple, Steepest-Ascent, and Stochastic Hill Climbing in a 3-column table.
- [ ] Trace the 4-Queens problem from $[2, 1, 4, 3]$ to $[3, 1, 4, 2]$ with conflict counts.
- [ ] Draw and explain the 3 landscape traps: Local Maximum, Plateau, and Ridge.
- [ ] Write the acceptance probability formula for Simulated Annealing and explain the role of Temperature $T$.

---
*Next Topic: [Module 08: High-Yield Exam Questions and Solved Papers](./08_High_Yield_Exam_Questions_and_Answers.md)*
