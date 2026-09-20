# 🧠 Module 01: Foundations of AI and Intelligent Agents
**Reference Source**: `Introduction_to_AI.pdf` (Dr. Ritesh Kumar, IIIT Surat)  
**Estimated Study Time**: 40 – 45 Minutes  
**Exam Importance**: Very High (Guaranteed 10–15 marks: PEAS descriptions, Agent Architecture diagrams, Environment Classifications)

---

## 🧭 Executive Summary: What This Module Is About

In this module, we transition from philosophical questions ("Can a machine think?") to concrete engineering definitions. Modern AI defines an **Agent** as anything that perceives its environment through **sensors** and acts upon it using **actuators**. We study how to evaluate an agent objectively via **Rationality**, how to specify an agent's task environment using **PEAS**, how to classify environments along **6 dimensions**, and how to build the **5 fundamental agent architectures**.

---

## 1. The Four Definitions of Artificial Intelligence

Stuart Russell and Peter Norvig group all historic definitions of AI along two primary axes:
1. **Thought Processes & Reasoning** vs. **Behavior & Action**
2. **Human-like** vs. **Rational (Ideal "Right Thinking / Right Acting")**

```
                     HUMAN-LIKE                              RATIONAL (IDEAL)
        +------------------------------------+------------------------------------+
        | 1. THINKING HUMANLY                | 2. THINKING RATIONALLY             |
THOUGHT | "Machines with minds, in the full  | "The study of computations that    |
        | and literal sense."                | make it possible to perceive,      |
        | • Focus: Cognitive Modeling        | reason, and act."                  |
        | • Matches human thought steps      | • Focus: Laws of Thought (Logic)   |
        | • Example: SOAR, ACT-R models      | • Example: Logic theorem provers   |
        +------------------------------------+------------------------------------+
        | 3. ACTING HUMANLY                  | 4. ACTING RATIONALLY               |
ACTION  | "Creating machines that do things  | "Intelligent behavior in artifacts;|
        | that require intelligence if done  | acting to maximize expected goal." |
        | by people."                        | • Focus: Rational Agent Approach   |
        | • Focus: The Turing Test (1950)    | • Modern AI standard (This course) |
        | • Example: Natural language bot    | • Example: Autonomous agents, A*   |
        +------------------------------------+------------------------------------+
```

### Deep Dive into the 4 Quadrants
1. **Thinking Humanly (Cognitive Science)**:
   - To say a machine thinks like a human, we must first determine *how humans think*.
   - Done through: (a) Introspection (catching our own thoughts), (b) Psychological experiments (observing people in action), or (c) Brain imaging (fMRI/EEG).
2. **Thinking Rationally ("Laws of Thought" / Logic Approach)**:
   - Aristotle's syllogisms: *"Socrates is a man; all men are mortal; therefore Socrates is mortal."*
   - Problem: Hard to state informal real-world knowledge in formal logic notation, and computational complexity can explode exponentially.
3. **Acting Humanly (The Turing Test Approach - 1950)**:
   - Alan Turing proposed the **Imitation Game**: A human interrogator chats via text with two hidden entities—a human and a computer. If the interrogator cannot reliably tell which is the computer after 5 minutes, the computer passes!
   - Required capabilities: NLP, Knowledge Representation, Automated Reasoning, Machine Learning.
   - *Total Turing Test* adds Computer Vision and Robotics (physical perception and interaction).
4. **Acting Rationally (The Rational Agent Approach - The Modern AI Standard)**:
   - A **rational agent** is one that acts so as to achieve the **best expected outcome** (or best expected utility when there is uncertainty).
   - Why AI chose this: It is more general than the laws of thought (logic isn't enough when decisions must be made under uncertainty) and far more scientifically objective than trying to copy quirky human behaviors.

#### 🚗 Real-World Application: Autonomous Car in All 4 Quadrants
- **Thinking Humanly**: Predicting when an impatient human pedestrian is about to jaywalk by modeling human impatience.
- **Thinking Rationally**: Using formal logical rules: *"If traffic light is RED and junction is occupied, stopping distance $d < 10m$, then apply brake."*
- **Acting Humanly**: Giving a polite wave or honking gently like a human taxi driver.
- **Acting Rationally**: Calculating expected trajectories across all probabilistic sensor readings and choosing the braking profile that maximizes passenger safety while minimizing journey time.

---

## 2. Interdisciplinary Roots of AI

AI is not just computer programming—it is the synthesis of centuries of human thought:
- **Philosophy (400 BC – present)**: Can formal rules yield rational thought? How does knowledge arise from experience? (Dualism vs. Materialism).
- **Mathematics (c. 800 – present)**: Formal logic, computation limits (Turing completeness, NP-completeness), and probability theory (Bayes' rule for handling uncertainty).
- **Economics (1776 – present)**: Decision theory, Utility theory (how to make decisions when payoffs conflict), and Game theory (rational decision-making in multi-agent environments).
- **Neuroscience (1861 – present)**: How biological brains process information using networks of neurons. Provides the inspiration for artificial neural networks.
- **Psychology (1879 – present)**: Cognitive psychology, behaviorism, and how humans perceive, remember, and act.
- **Computer Engineering (1940 – present)**: Hardware (CPUs, GPUs, TPUs) capable of executing billions of floating-point operations per second.
- **Control Theory & Cybernetics (1948 – present)**: Feedback loops that adjust actions to minimize error over time (e.g., self-regulating thermostats, Kalman filters).
- **Linguistics (1957 – present)**: How language connects to thought, leading to syntax trees, semantics, and Natural Language Processing.

---

## 3. History of AI & The Two "AI Winters"

Progress in AI has historically moved in cycles of **grand promises $\to$ excessive hype $\to$ under-delivery $\to$ funding collapse ("AI Winter") $\to$ quiet technical breakthroughs $\to$ renewed boom**.

```
  1950: Turing Test proposed
  1956: Dartmouth Workshop (Term "Artificial Intelligence" coined by John McCarthy)
  1960s–70s: Early optimism, symbolic logic, General Problem Solver (GPS)
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ❄️ 1974–1980: FIRST AI WINTER
     • Cause: Lighthill Report (UK) and DARPA cuts (US).
     • Why: Early algorithms hit combinatorial explosion ($O(2^n)$); computers
       lacked memory and processing speed to handle real-world scale.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1980–1987: The Expert Systems Boom (R1/XCON at DEC saved $40M/year; LISP machines)
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ❄️ 1987–1993: SECOND AI WINTER
     • Cause: Specialized LISP workstations collapsed against cheap commodity x86 PCs;
       expert systems proved brittle, expensive to maintain, and unable to learn.
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1990s–2000s: Statistical AI, Probabilistic Reasoning (Bayesian networks), Machine Learning
  2012–2020: Deep Learning Revolution (AlexNet, ImageNet, AlphaGo, GPU compute)
  2020s–Present: Generative AI, Large Language Models (GPT-4, Claude, Gemini, diffusion models)
```

> **Exam Key Takeaway**: Why study AI Winters? It warns engineers that when algorithmic marketing outpaces computational reality and theoretical limits, funding dries up. Modern AI succeeded because it combined **Big Data + Massively Parallel GPU Compute + Better Architectures (Transformers/CNNs)**.

---

## 4. What Is an Intelligent Agent?

### Core Definitions
- **Agent**: Anything that can perceive its environment through **sensors** and act upon that environment through **actuators**.
- **Percept**: The agent's perceptual inputs at any given instant.
- **Percept Sequence**: The complete history of everything the agent has ever perceived from the start of its execution up to the current moment: $P^*$.
- **Agent Function**: An abstract mathematical mapping from every possible percept sequence to an action:
  $$f: P^* \to A$$
- **Agent Program**: The concrete, practical software code running on physical hardware (**Architecture**) that implements the agent function:
  $$\text{Agent} = \text{Architecture} + \text{Program}$$

```
                +------------------------------------+
                |            ENVIRONMENT             |
                +------------------------------------+
                       |                      ^
         Percepts via  |                      | Actions via
           Sensors     v                      |  Actuators
                +------------------------------------+
                |               AGENT                |
                |   +----------------------------+   |
                |   |       Agent Program        |   |
                |   |  (Processes Percepts       |   |
                |   |   and Decides Actions)     |   |
                |   +----------------------------+   |
                +------------------------------------+
```

---

## 5. The PEAS Framework

Before designing any AI system, you must formulate its **task environment** using the **PEAS** framework:
- **P** $\to$ **Performance Measure**: The objective criterion for how successful an agent is (What does "doing well" mean?).
- **E** $\to$ **Environment**: The external world/context the agent operates in.
- **A** $\to$ **Actuators**: The mechanisms through which the agent outputs actions to affect the world.
- **S** $\to$ **Sensors**: The devices through which the agent reads inputs from the world.

### 5 Complete Real-World PEAS Tables (High Probability Exam Topic)

#### 1. Automated Taxi Driver
| Component | Specification |
| :--- | :--- |
| **Performance Measure** | Safe trip, reach destination, minimize travel time, minimize fuel consumption, obey traffic laws, maximize passenger comfort, maximize profit. |
| **Environment** | City streets, highways, traffic, pedestrians, weather conditions, customers, road construction. |
| **Actuators** | Steering wheel, accelerator pedal, brake, turn signals, horn, display/voice speaker. |
| **Sensors** | Video cameras, LiDAR, ultrasonic sensors, radar, GPS, speedometer, accelerometer, engine sensors. |

#### 2. Medical Diagnosis System
| Component | Specification |
| :--- | :--- |
| **Performance Measure** | Healthy patient, minimize treatment cost, minimize diagnostic delay, avoid misdiagnosis/lawsuits. |
| **Environment** | Patient, hospital/clinic environment, medical staff, lab testing facilities. |
| **Actuators** | On-screen recommendations, prescription generation, test orders, alert notifications. |
| **Sensors** | Keyboard/touchscreen inputs (symptoms, patient history), direct feed from lab test machines, vital sign monitors. |

#### 3. Part-Picking Robot (Factory Conveyor Belt)
| Component | Specification |
| :--- | :--- |
| **Performance Measure** | Percentage of parts sorted into correct bins, throughput (parts/minute), zero damage to parts. |
| **Environment** | Conveyor belt carrying varying parts, collection bins, factory floor lighting. |
| **Actuators** | Jointed robotic arm, pneumatic suction gripper / robotic hand, conveyor speed control. |
| **Sensors** | Overhead optical camera, infrared depth sensor, joint angle sensors, tactile pressure sensors. |

#### 4. Interactive English Tutor
| Component | Specification |
| :--- | :--- |
| **Performance Measure** | Student's mastery score on standard English tests, student engagement, learning speed. |
| **Environment** | Diverse pool of students (different native languages), school curriculum guidelines. |
| **Actuators** | Interactive display of grammar exercises, voice pronunciation synthesis, customized feedback/hints. |
| **Sensors** | Keyboard input (essays/answers), microphone (spoken pronunciation audio), mouse clicks. |

#### 5. Spam Email Filter
| Component | Specification |
| :--- | :--- |
| **Performance Measure** | Minimize false positives (legitimate email flagged as spam), minimize false negatives (spam in inbox). |
| **Environment** | Incoming email stream, user inbox, user spam reports, evolving spam tactics. |
| **Actuators** | Move email to Spam folder, mark as Important, deliver to Inbox, strip dangerous attachments. |
| **Sensors** | Email text parser, header metadata extractor (IP, SPF/DKIM records), attachment scanner. |

---

## 6. The Six Dimensions of Task Environments

Task environments vary widely. AI classifies environments along 6 fundamental dimensions:

| Dimension | Option A | Option B | Plain English Definition |
| :--- | :--- | :--- | :--- |
| **1. Observability** | **Fully Observable** | **Partially Observable** | Can sensors perceive the **entire complete state** of the world at every point in time? (e.g., Chess = Fully; Poker / Driving = Partially). |
| **2. Determinism** | **Deterministic** | **Stochastic** | Is the next state **completely determined** by the current state and the agent's action? (e.g., Chess = Deterministic; Taxi driving with tire slips / weather = Stochastic). *(Note: If deterministic except for other agents' moves, it's called **Strategic**)*. |
| **3. Episodicity** | **Episodic** | **Sequential** | Is the agent's experience divided into atomic, independent episodes where current action doesn't affect future episodes? (e.g., Image classification / Defect detection = Episodic; Chess / Driving = Sequential). |
| **4. Dynamics** | **Static** | **Dynamic** | Does the environment change **while the agent is thinking/deliberating**? (e.g., Crossword = Static; Taxi driving = Dynamic; Chess with a ticking clock = **Semi-dynamic**). |
| **5. Discreteness** | **Discrete** | **Continuous** | Are states, time, percepts, and actions distinct and countable, or do they flow smoothly? (e.g., Chess squares/turns = Discrete; Speed/steering angle/time in driving = Continuous). |
| **6. Agent Count** | **Single-Agent** | **Multi-Agent** | Is the agent acting alone, or are there other entities whose behavior is best described as maximizing their own performance? (e.g., Crossword / Solitaire = Single-Agent; Chess = Multi-Agent Competitive; Driving = Multi-Agent Cooperative/Competitive). |

### 📋 Master Environment Classification Matrix
*(Frequently asked in exams: "Classify the following 4 environments...")*

| Environment | Observability | Determinism | Episodic? | Dynamics | Discreteness | Number of Agents |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Crossword Puzzle** | Fully Observable | Deterministic | Sequential | Static | Discrete | Single-Agent |
| **Chess (with clock)** | Fully Observable | Strategic | Sequential | Semi-Dynamic | Discrete | Multi-Agent (Competitive) |
| **Poker** | Partially Observable | Stochastic | Sequential | Static | Discrete | Multi-Agent (Competitive) |
| **Self-Driving Taxi** | Partially Observable | Stochastic | Sequential | Dynamic | Continuous | Multi-Agent (Mixed) |
| **Medical Diagnosis** | Partially Observable | Stochastic | Sequential | Dynamic | Continuous | Single-Agent (w/ nature) |
| **Part-Picking Robot**| Partially Observable | Stochastic | Episodic | Dynamic | Continuous | Single-Agent |

> **Exam Tip**: The **hardest** environment for an AI agent is: **Partially Observable, Stochastic, Sequential, Dynamic, Continuous, Multi-Agent** (e.g., Driving a car in rush-hour traffic).

---

## 7. The Concept of Rationality

### Formal Definition
> *"For each possible percept sequence, a rational agent should select an action that is expected to **maximize its performance measure**, given the evidence provided by the percept sequence and whatever built-in knowledge the agent has."*

Rationality depends on four things:
1. The **Performance Measure** defining success.
2. The **Percept Sequence** accumulated so far.
3. The agent's **Prior Knowledge** about the world.
4. The **Actions** the agent is capable of performing.

### ⚠️ Crucial Distinctions: Rationality $\ne$ Omniscience $\ne$ Perfection
- **Not Omniscience**: An omniscient agent knows the *actual outcome* of its actions and knows what is happening everywhere in real-time. This is impossible in reality. Rationality maximizes **expected** performance, not actual performance.
  - *Example*: A pedestrian looks left and right, sees no cars, and steps onto the pedestrian crossing. A falling meteor suddenly hits them. Was the pedestrian rational? **Yes!** They made the optimal choice given available percepts.
- **Not Perfection**: Perfection means 100% success every time. A rational poker player can make mathematically flawless bets and still lose due to an unlucky card draw on the river. The decision was rational; the outcome was unlucky.
- **Not Clairvoyance**: Rational agents cannot see into the future. They can only use probability and models.

---

## 8. The Five Intelligent Agent Architectures

Russell & Norvig categorize intelligent agents into 5 designs, ordered by increasing complexity, memory, and capability:

```
Simple Reflex  ──>  Model-Based Reflex  ──>  Goal-Based  ──>  Utility-Based  ──>  Learning Agent
(Current percept)   (Internal memory)       (Plans for goals)  (Weighs trade-offs)  (Improves over time)
```

---

### 1. Simple Reflex Agent
- **Core Concept**: Selects actions based **only on the current percept**, completely ignoring percept history.
- **Mechanism**: Operates via hard-coded **Condition-Action Rules** (*"If condition THEN action"*).
  - *Example*: Thermostat: `IF temperature < 20°C THEN turn ON heater`.
  - *Example*: Vacuum cleaner: `IF current square is Dirty THEN Suck`.
- **Fatal Limitation**: Only works if the environment is **fully observable**. In partially observable worlds, lack of memory leads to **infinite loops** (e.g., oscillating back and forth between two rooms).

```
   Sensors ──> [ What the world is like now ] ──> [ Condition-Action Rules ] ──> Actuators
```

---

### 2. Model-Based Reflex Agent
- **Core Concept**: Maintains an **Internal State** (memory) to track parts of the environment that cannot be seen right now.
- **Mechanism**: Uses two models of the world:
  1. **Transition Model**: Knowledge of *how the world evolves on its own*.
  2. **Sensor Model**: Knowledge of *how the agent's own actions affect the world*.
  - *Example*: Self-driving car remembers that an overtaking red sports car is currently in its blind spot, even though the rear camera cannot see it right now.
  - *Example*: Robot vacuum maintains an internal grid map of which rooms are already clean.

```
   Sensors ──> [ What the world is like now ]
                      │
                      ▼
               [ Internal State ] <─── [ Model of World & Own Actions ]
                      │
                      ▼
               [ Condition-Action Rules ] ──> Actuators
```

---

### 3. Goal-Based Agent
- **Core Concept**: Knowing the current state is not enough; the agent also needs explicit **Goal Information** describing desired future situations.
- **Mechanism**: Combines state knowledge with **Search and Planning** algorithms to find a sequence of actions that leads to the goal.
  - *Example*: GPS Navigation system. Given start location and destination, it searches paths to find a valid route.
  - *Advantage over reflex*: High flexibility! If the destination changes, you simply update the goal description, and the agent re-plans. A reflex agent would require re-writing thousands of condition-action rules.

```
   Sensors ──> [ Current State & Model ] ──> [ What will it be like if I do action X? ]
                                                            │
                                                            ▼
                                                     [ Goal State ]
                                                            │
                                                            ▼
                                                     [ Choose Action ] ──> Actuators
```

---

### 4. Utility-Based Agent
- **Core Concept**: Goals are binary (either reached or not reached). But in the real world, multiple paths reach the goal, or goals conflict! A **Utility Function** maps any state to a real number measuring **"how happy/desirable"** that state is:
  $$U: S \to \mathbb{R}$$
- **Mechanism**: Maximizes **Expected Utility** under uncertainty.
  - *Example*: Uber / Google Maps route selection. Three routes reach the destination:
    * Route A: 20 mins, high toll cost, smooth highway.
    * Route B: 22 mins, zero toll, bumpy road.
    * Route C: 15 mins, dangerous narrow road.
    A utility-based agent balances speed, cost, and passenger comfort to select the best trade-off.

```
   Sensors ──> [ Current State & Model ] ──> [ Predict Outcomes of Actions ]
                                                            │
                                                            ▼
                                                    [ Utility Function ]
                                                 (Calculates desirability)
                                                            │
                                                            ▼
                                                 [ Maximize Utility ] ──> Actuators
```

---

### 5. Learning Agent
- **Core Concept**: In complex or unknown environments, designers cannot program all knowledge upfront. A learning agent improves its performance through experience over time.
- **The 4 Distinct Components (Examiners LOVE this diagram!)**:
  1. **Performance Element**: The operational agent program that accepts percepts and selects external actions (corresponds to any of the earlier agents).
  2. **Critic**: Observes the environment and evaluates the agent's behavior against an external, fixed performance standard. Provides feedback.
  3. **Learning Element**: Responsible for making improvements to the performance element based on the Critic's feedback.
  4. **Problem Generator**: Suggests **exploratory actions** that might lead to new and better experiences, even if they seem suboptimal in the short term.

```
                  +--------------------------------------------------+
                  |                   LEARNING AGENT                 |
                  |                                                  |
     Sensors ───> | ───> [ Critic ] <─── [ Fixed Performance Standard]
                  |          │                                       |
                  |     (Feedback)                                   |
                  |          ▼                                       |
                  |   [ Learning Element ] ──> (Learning Goals)      |
                  |          │                      │                |
                  |   (Improvements)                ▼                |
                  |          ▼              [ Problem Generator ]    |
                  |   [ Performance Element ] <─────┘                |
                  |          │                                       |
                  +----------│---------------------------------------+
                             ▼
                         Actuators
```

---

### 📊 Master Agent Architectures Comparison Table

| Agent Architecture | Has Memory? | Looks into Future? | Balances Conflicting Goals? | Learns / Adapts? | Textbook Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Simple Reflex** | ❌ No | ❌ No | ❌ No | ❌ No | Bimetallic Thermostat |
| **Model-Based Reflex** | ✅ Yes | ❌ No | ❌ No | ❌ No | Robot vacuum with map |
| **Goal-Based** | ✅ Yes | ✅ Yes (Planning) | ❌ No | ❌ No | GPS Navigation |
| **Utility-Based** | ✅ Yes | ✅ Yes | ✅ Yes (Utility trade-offs)| ❌ No | Ride-sharing route selector |
| **Learning Agent** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes | AlphaGo, Spotify recommender |

---

## 9. Solved Exercises from Lecture Slides

### Exercise 1: Identify the Agent Architecture
1. *A washing machine that always runs the same fixed 45-minute wash cycle regardless of load.*  
   👉 **Simple Reflex Agent**: No internal model of dirt or clothes weight; purely condition-action timer.
2. *A robot vacuum that remembers which rooms it has already cleaned.*  
   👉 **Model-Based Reflex Agent**: Uses internal memory/state to track cleaned areas not currently visible.
3. *A self-driving car that weighs fuel cost, travel time, and passenger comfort before selecting a route.*  
   👉 **Utility-Based Agent**: Explicitly optimizes trade-offs between conflicting criteria using a utility function.
4. *A streaming service whose recommendations improve as it observes more of your viewing history.*  
   👉 **Learning Agent**: Uses user feedback as a critic to update its internal recommendation models.
5. *An elevator system that decides which floor to visit next based on active floor requests.*  
   👉 **Goal-Based Agent**: Plans its dispatching sequence to satisfy all goal requests efficiently.

---

## 🎯 Exam Practice Checklist: Questions to Master
- [ ] Draw the 2×2 matrix of the 4 definitions of AI and explain the Turing Test.
- [ ] Write the PEAS specification for an Automated Taxi Driver and a Medical Diagnosis System.
- [ ] Explain the 6 dimensions of task environments with examples for each.
- [ ] Why is Rationality not the same as Omniscience or Perfection? Give the pedestrian meteor example.
- [ ] Draw the block diagram of a Learning Agent with its 4 components and explain each.

---
*Next Topic: [Module 02: Problem Formulation and Classic AI Problems](./02_Problem_Formulation_and_Classic_Problems.md)*
