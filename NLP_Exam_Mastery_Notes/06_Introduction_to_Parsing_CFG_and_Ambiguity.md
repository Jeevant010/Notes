# 🌳 Module 06: Introduction to Parsing — CFG, Derivations & Ambiguity
**Reference Source**: `Introduction_to_Parsing.pdf` — CS702/CS703/CS913 (Shreya Agarwal, IIIT Surat)
**Estimated Study Time**: 50 Minutes
**Exam Importance**: Very High (Guaranteed 10–15 marks: CFG 4-tuple, leftmost/rightmost derivation tables, ambiguity, dangling-else fix, top-down vs bottom-up)

---

## 🧭 Executive Summary: What This Module Is About

POS tagging told us **what each word is**. Parsing asks the harder question: **how do the words fit together?** This module builds that capability from the ground up:

1. **Why we need grammars** — regular expressions and DFAs are not powerful enough.
2. **Context-Free Grammars (CFGs)** — the formal model $G = (S, N, T, P)$.
3. **Derivations** — the two systematic orders (**leftmost** and **rightmost**) and their **parse trees**.
4. **Ambiguity** — one sentence, two parse trees, two meanings. And how to **fix** it.
5. **Top-down vs Bottom-up** parsing strategies (the roadmap for building actual parsers).

> **The single most important idea**: *Parsing is the process of discovering a **derivation** for a sentence.* Everything else in this module is machinery that supports that one sentence.

---

## 1. Where Parsing Sits: The Front End of a Compiler (and of NLP)

The slides present parsing in the classic **compiler front-end** pipeline — and the analogy to NLP is exact (words ≈ tokens, POS ≈ token classes):

```
   Source code ──► [ SCANNER ] ──► tokens ──► [ PARSER ] ──► IR
   (raw text)      (lexer)                    (this module)
```

### What the Parser Does (from the slides)

| Job | Description |
| :--- | :--- |
| **Checks the stream of words and their parts of speech** | Uses the token/word sequence provided by the **scanner** (in NLP, the POS tagger/lexer). |
| **Determines if the input is syntactically well formed** | Tests membership in the language $L(G)$. |
| **Guides checking at deeper levels than syntax** | Provides the structure that semantics/pragmatics analysis will consume. |
| **Builds an IR (Intermediate Representation) of the code** | In NLP, the IR is the **parse tree / dependency graph**. |

> **Definition**: **Parsing** = the process of discovering a **derivation** for some sentence. It is, in the words of the slides, *"the mathematics of diagramming sentences."*

### The Study of Parsing — What We Need

To parse, we need exactly **two** things:

1. A **mathematical model of syntax** — a **grammar $G$**.
2. An **algorithm for testing membership in $L(G)$** — the parser.

> ⚠️ **From the slides**: *"Need to keep in mind that our goal is **building parsers**, not studying the mathematics of arbitrary languages."* Always tie the theory back to building a working parser.

### Roadmap for Our Study of Parsing

| # | Topic | Parser Family |
| :-: | :--- | :--- |
| 1 | **Context-free grammars and derivations** | Foundation (this module) |
| 2 | **Top-down parsing** | **Generated LL(1) parsers** & **hand-coded recursive descent parsers** (Lab 2) |
| 3 | **Bottom-up parsing** | **Generated LR(1) parsers** |

---

## 2. Specifying Syntax with a Grammar

> **Context-free syntax is specified with a context-free grammar.**

**Introductory example (SheepNoise):**
$$\textit{SheepNoise} \rightarrow \textit{SheepNoise}\ \text{baa} \mid \text{baa}$$

This **CFG** defines the set of noises sheep normally make. It is written in a variant of **Backus–Naur Form (BNF)**.

### The Formal Definition — The 4-Tuple (Memorize This)

> **Formally, a grammar is a four tuple:**
> $$G = (S,\ N,\ T,\ P)$$

| Symbol | Name | Meaning |
| :--- | :--- | :--- |
| $S$ | **Start symbol** | The set of strings in $L(G)$ — where every derivation begins |
| $N$ | **Nonterminal symbols** | The **syntactic variables** (e.g., `Expr`, `Term`, `Factor`, `NounPhrase`) |
| $T$ | **Terminal symbols** | The **words** (actual symbols of the language) |
| $P$ | **Productions / rewrite rules** | $P: N \rightarrow (N \cup T)^*$ — how a nonterminal may be rewritten |

> **Exam Tip**: The 4-tuple $G=(S,N,T,P)$ is an almost-guaranteed 2-mark question. Note the constraint in $P$: the **left-hand side is a single nonterminal** — this is what makes the grammar *context-free*.

### Deriving Syntax = Using Productions as Rewriting Rules

We can use the `SheepNoise` grammar to create sentences by using the productions as **rewriting rules**:

$$\textit{SheepNoise} \Rightarrow \textit{SheepNoise}\ \text{baa} \Rightarrow \textit{SheepNoise}\ \text{baa}\ \text{baa} \Rightarrow \text{baa}\ \text{baa}\ \text{baa}$$

> **From the slides, with honesty**: *"While this example is cute, it quickly runs out of intellectual steam…"* — so we move to a real grammar.

---

## 3. Why Not Just Use Regular Languages & DFAs?

This is a very common question: *"Regex works fine for so many things — why do we need CFGs?"*

### 3.1 The Theory: RL ⊂ CFL ⊂ CSL

$$\text{Regular Languages (RL)} \subset \text{Context-Free Languages (CFL)} \subset \text{Context-Sensitive Languages (CSL)}$$

**Not all languages are regular.** You **cannot construct DFAs** to recognize these languages:

| Language | Why It's Not Regular |
| :--- | :--- |
| $L = \{p^k q^k\}$ (**parenthesis languages**) | Requires **counting** an unbounded number of matched pairs. A DFA has finite memory, so it cannot remember an arbitrary count. |
| $L = \{w c w^r \mid w \in \Sigma^*\}$ | Requires **remembering an arbitrary string** $w$ and matching its reverse $w^r$ — needs a stack. |

> **Why**: *"To recognize these features requires an **arbitrary amount of context** (left or right …)."*

### 3.2 But Be Careful — Some Things Regular Languages CAN Do

> **From the slides**: *"But, this issue is somewhat subtle."* You **can** construct DFAs for:
> - Strings with alternating 0's and 1's: $(\epsilon \mid 1)(01)^*(\epsilon \mid 0)$
> - Strings with an **even number** of 0's and 1's
>
> **Regular expressions can count bounded sets and bounded differences.**

### 3.3 The Advantages and the Hard Limit

| ✅ Advantages of Regular Expressions | ❌ The Hard Limit |
| :--- | :--- |
| **Simple & powerful notation** for specifying patterns | **Cannot add parenthesis, brackets, begin-end pairs** — anything with **unbounded nesting**. |
| **Automatic construction of fast recognizers** (DFAs) | Cannot express recursive, self-embedding structure. |
| **Many kinds of syntax can be specified with REs** | Cannot enforce that every `(` has a matching `)` at arbitrary depth. |

**The slide's regex example** — a regular expression for arithmetic expressions:

$$\begin{aligned}
\textit{Term} &\rightarrow [a\text{-}zA\text{-}Z]\ ([a\text{-}zA\text{-}Z] \mid [\text{0-9}])^* \\
\textit{Op} &\rightarrow + \mid - \mid * \mid / \\
\textit{Expr} &\rightarrow (\textit{Term}\ \textit{Op})^*\ \textit{Term}
\end{aligned}$$

> *"Of course, this would generate a DFA … If REs are so useful … Why not use them for everything? ⇒ **Cannot add parenthesis, brackets, begin-end pairs, …**"*

> **Exam Tip**: The single sentence that answers *"Why use CFG instead of regex?"* is: **"REs cannot handle nested/recursive structures such as parentheses, brackets, and begin-end pairs, which require unbounded counting."**

---

## 4. Context-Free Grammars — What Makes a Grammar "Context Free"?

> **The SheepNoise grammar has a specific form**: $\textit{SheepNoise} \rightarrow \textit{SheepNoise}\ \text{baa} \mid \text{baa}$
>
> **Productions have a single nonterminal on the left-hand side, which makes it impossible to encode left or right context. ⇒ The grammar is context free.**
>
> **A context-sensitive grammar can have ≥ 1 nonterminal on the left-hand side.**

| Grammar Type | LHS of a Production | Example |
| :--- | :--- | :--- |
| **Context-Free (CFG)** | **Exactly one nonterminal**, e.g. $A \rightarrow \gamma$ | `NP → Det N` |
| **Context-Sensitive (CSG)** | **≥ 1 nonterminal**, possibly with surrounding context, e.g. $\alpha A \beta \rightarrow \alpha \gamma \beta$ | `a X b → a Y b` (X becomes Y only between a and b) |

> **Definition (classic)**: **Any language that can be recognized by a push-down automaton is a context-free language.**

**Machine hierarchy:**

```
   REGULAR LANGUAGES  ──recognized by──►  FINITE STATE AUTOMATON (no memory)
   CONTEXT-FREE (CFL) ──recognized by──►  PUSH-DOWN AUTOMATON (stack = unbounded memory)
   CONTEXT-SENSITIVE  ──recognized by──►  LINEAR BOUNDED AUTOMATON
   RECURSIVELY ENUM.  ──recognized by──►  TURING MACHINE
```

> ⚠️ **A cute subtlety from the slides**: `L(SheepNoise)` is actually a **regular** language (`baa+`) — the *grammar* is context-free, but the *language* it generates happens to be regular. Grammar class and language class are not the same thing!

---

## 5. A More Useful Grammar Than Sheep Noise

To explore CFGs properly we need a real grammar. Here is the **ambiguous expression grammar** (we will use it throughout):

$$G_1: \quad
\begin{aligned}
0:\ & \textit{Expr} \rightarrow \textit{Expr}\ \textit{Op}\ \textit{Expr} \\
1:\ & \textit{Expr} \rightarrow \text{number} \\
2:\ & \textit{Expr} \rightarrow \text{id} \\
3:\ & \textit{Op} \rightarrow + \\
4:\ & \textit{Op} \rightarrow - \\
5:\ & \textit{Op} \rightarrow * \\
6:\ & \textit{Op} \rightarrow /
\end{aligned}$$

### Worked Derivation of $x - 2 * y$

| Rule | Sentential Form |
| :--- | :--- |
| — | $\textit{Expr}$ |
| 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 2 | $\langle id, x \rangle\ \textit{Op}\ \textit{Expr}$ |
| 4 | $\langle id, x \rangle - \textit{Expr}$ |
| 0 | $\langle id, x \rangle - \textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 1 | $\langle id, x \rangle - \langle num, 2 \rangle\ \textit{Op}\ \textit{Expr}$ |
| 5 | $\langle id, x \rangle - \langle num, 2 \rangle * \textit{Expr}$ |
| 2 | $\langle id, x \rangle - \langle num, 2 \rangle * \langle id, y \rangle$ |

> **Such a sequence of rewrites is called a *derivation*.** The process of discovering a derivation is called **parsing**.
>
> **We denote this derivation**: $\textit{Expr} \stackrel{*}{\Rightarrow} id - num * id$

---

## 6. Derivations: Leftmost vs Rightmost

> **The point of parsing is to construct a derivation.** At each step, we choose a nonterminal to replace. **Different choices can lead to different derivations.**

**Two derivations are of interest:**

| Derivation | Rule |
| :--- | :--- |
| **Leftmost derivation** | Replace the **leftmost NT** at each step |
| **Rightmost derivation** | Replace the **rightmost NT** at each step |

> **These are the two *systematic* derivations.** *(We don't care about randomly-ordered derivations!)*

### The Formal Structure of a Derivation

$$S \Rightarrow \gamma_0 \Rightarrow \gamma_1 \Rightarrow \gamma_2 \Rightarrow \cdots \Rightarrow \gamma_{n-1} \Rightarrow \gamma_n \Rightarrow \textit{sentence}$$

**Each $\gamma_i$ is a *sentential form*:**

| Condition | Name |
| :--- | :--- |
| $\gamma$ contains **only terminal symbols** | $\gamma$ is a **sentence** in $L(G)$ |
| $\gamma$ contains **1 or more non-terminals** | $\gamma$ is a **sentential form** |
| Occurs in a **leftmost** derivation | **Left-sentential form** |
| Occurs in a **rightmost** derivation | **Right-sentential form** |

**To get $\gamma_i$ from $\gamma_{i-1}$**: expand some $NT\ A \in \gamma_{i-1}$ by using $A \rightarrow \beta$:
- Replace the occurrence of $A \in \gamma_{i-1}$ with $\beta$ to get $\gamma_i$.
- In a **leftmost** derivation, it would be the **first NT** $A \in \gamma_{i-1}$.

---

## 7. The Two Derivations for $x - 2 * y$ (Reproduce These Tables!)

### 7.1 Leftmost Derivation

| Rule | Sentential Form |
| :--- | :--- |
| — | $\textit{Expr}$ |
| 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 2 | $\langle id, x \rangle\ \textit{Op}\ \textit{Expr}$ |
| 4 | $\langle id, x \rangle - \textit{Expr}$ |
| 0 | $\langle id, x \rangle - \textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 1 | $\langle id, x \rangle - \langle num, 2 \rangle\ \textit{Op}\ \textit{Expr}$ |
| 5 | $\langle id, x \rangle - \langle num, 2 \rangle * \textit{Expr}$ |
| 2 | $\langle id, x \rangle - \langle num, 2 \rangle * \langle id, y \rangle$ |
| | **Leftmost derivation** $\textit{Expr} \stackrel{*}{\Rightarrow} id - num * id$ |

### 7.2 Rightmost Derivation

| Rule | Sentential Form |
| :--- | :--- |
| — | $\textit{Expr}$ |
| 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 2 | $\textit{Expr}\ \textit{Op}\ \langle id, y \rangle$ |
| 5 | $\textit{Expr} * \langle id, y \rangle$ |
| 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr} * \langle id, y \rangle$ |
| 1 | $\textit{Expr}\ \textit{Op}\ \langle num, 2 \rangle * \langle id, y \rangle$ |
| 4 | $\textit{Expr} - \langle num, 2 \rangle * \langle id, y \rangle$ |
| 2 | $\langle id, x \rangle - \langle num, 2 \rangle * \langle id, y \rangle$ |
| | **Rightmost derivation** |

> **⚠️ The bombshell**: *"In both cases, the two derivations produce **different parse trees**. The parse trees imply **different evaluation orders**!"*
>
> - **Leftmost** derivation evaluates as $x - (2 * y)$
> - **Rightmost** derivation evaluates as $(x - 2) * y$
>
> **This ambiguity is NOT good.**

### 7.3 The Two Parse Trees

```
   LEFTMOST →  x - (2 * y)          RIGHTMOST →  (x - 2) * y
                                          Op(*)  ◄── root is *
        Op(-)  ◄── root is -            ╱      ╲
       ╱     ╲                        Op(-)      y
      x     Op(*)                    ╱    ╲
           ╱    ╲                   x    Op(*)
          2      y                        ╱   ╲
                                         2     y
```

---

## 8. Derivations and Precedence

> **These two derivations point out a problem with the grammar**: **It has no notion of *precedence*, or implied order of evaluation.**

### How to Add Precedence (The Standard Recipe)

1. **Create a nonterminal for each level of precedence.**
2. **Isolate the corresponding part of the grammar.**
3. **Force the parser to recognize high-precedence subexpressions first.**

**For algebraic expressions:**

| Level | Operators | Priority |
| :-: | :--- | :--- |
| **Level 1** | **Parentheses** | Highest — first |
| **Level 2** | **Multiplication and division** | Next |
| **Level 3** | **Subtraction and addition** | Last |

### The Classic Expression Grammar (Memorize!)

$$G_2: \quad
\begin{aligned}
0:\ & \textit{Goal} \rightarrow \textit{Expr} \\
1:\ & \textit{Expr} \rightarrow \textit{Expr} - \textit{Term} \\
2:\ & \textit{Expr} \rightarrow \textit{Expr} + \textit{Term} \\
3:\ & \textit{Expr} \rightarrow \textit{Term} \\
4:\ & \textit{Term} \rightarrow \textit{Term}\ /\ \textit{Factor} \\
5:\ & \textit{Term} \rightarrow \textit{Term} * \textit{Factor} \\
6:\ & \textit{Term} \rightarrow \textit{Factor} \\
7:\ & \textit{Factor} \rightarrow \text{number} \\
8:\ & \textit{Factor} \rightarrow \text{id} \\
9:\ & \textit{Factor} \rightarrow (\ \textit{Expr}\ )
\end{aligned}$$

**What this achieves** (the slide's own summary):

| Property | Explanation |
| :--- | :--- |
| **This grammar is slightly larger** | *Takes more rewriting to reach some of the terminal symbols* |
| **Encodes expected precedence** | The nesting `Goal → Expr → Term → Factor` literally encodes the precedence levels |
| **Produces the same parse tree under leftmost & rightmost derivations** | Ambiguity resolved! |
| **Correctness trumps the speed of the parser** | Worth the extra rewrites |

### Rightmost Derivation of $x - 2 * y$ with the Precedence Grammar

| Rule | Sentential Form |
| :--- | :--- |
| — | $\textit{Goal}$ |
| 0 | $\textit{Expr}$ |
| 1 | $\textit{Expr} - \textit{Term}$ |
| 5 | $\textit{Expr} - \textit{Term} * \textit{Factor}$ |
| 8 | $\textit{Expr} - \textit{Term} * \langle id, y \rangle$ |
| 6 | $\textit{Expr} - \textit{Factor} * \langle id, y \rangle$ |
| 7 | $\textit{Expr} - \langle num, 2 \rangle * \langle id, y \rangle$ |
| 3 | $\textit{Term} - \langle num, 2 \rangle * \langle id, y \rangle$ |
| 6 | $\textit{Factor} - \langle num, 2 \rangle * \langle id, y \rangle$ |
| 8 | $\langle id, x \rangle - \langle num, 2 \rangle * \langle id, y \rangle$ |

**Parse tree:**

```
                 E
              ╱     ╲
            E        T
          ╱   ╲     ╱  ╲
         E   T    T   F
         │   │   ╱╲   │
         T   F  T F   y
         │   │  │ │
         F   2  F 2  ...  (structure encodes: x - (2 * y))
         │
         x
```

> ✅ **Result**: *"It derives $x - (2 * y)$, along with an appropriate parse tree. Both the leftmost and rightmost derivations give the same expression, because the grammar directly and explicitly encodes the desired precedence."*

---

## 9. Ambiguous Grammars

### 9.1 The Definition

> **If a grammar has more than one leftmost derivation for a single sentential form, the grammar is ambiguous.**
> **If a grammar has more than one rightmost derivation for a single sentential form, the grammar is ambiguous.**

⚠️ **Important nuance**: *"The leftmost and rightmost derivations for a sentential form **may differ, even in an unambiguous grammar** — **However, they must have the same parse tree!**"*

### 9.2 Two Leftmost Derivations for $x - 2 * y$ (Original Grammar $G_1$)

| | **Original choice** | | **New choice** |
| :--- | :--- | :--- | :--- |
| **Rule** | **Sentential Form** | **Rule** | **Sentential Form** |
| — | $\textit{Expr}$ | — | $\textit{Expr}$ |
| 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr}$ | 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 2 | $\langle id,x\rangle\ \textit{Op}\ \textit{Expr}$ | 0 | $\textit{Expr}\ \textit{Op}\ \textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 4 | $\langle id,x\rangle - \textit{Expr}$ | 2 | $\langle id,x\rangle\ \textit{Op}\ \textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 0 | $\langle id,x\rangle - \textit{Expr}\ \textit{Op}\ \textit{Expr}$ | 4 | $\langle id,x\rangle - \textit{Expr}\ \textit{Op}\ \textit{Expr}$ |
| 1 | $\langle id,x\rangle - \langle num,2\rangle\ \textit{Op}\ \textit{Expr}$ | 1 | $\langle id,x\rangle - \langle num,2\rangle\ \textit{Op}\ \textit{Expr}$ |
| 5 | $\langle id,x\rangle - \langle num,2\rangle * \textit{Expr}$ | 5 | $\langle id,x\rangle - \langle num,2\rangle * \textit{Expr}$ |
| 2 | $\langle id,x\rangle - \langle num,2\rangle * \langle id,y\rangle$ | 2 | $\langle id,x\rangle - \langle num,2\rangle * \langle id,y\rangle$ |
| | **Original choice** | | **New choice** *(different productions chosen on the second step)* |

> **Both derivations succeed in producing $x - 2 * y$.** This grammar allows **multiple leftmost derivations** — it is **hard to automate derivation if > 1 choice**. **The grammar is *ambiguous*.**

---

## 10. The Dangling-Else Problem (The Classic Example)

### 10.1 The Ambiguous Grammar

$$\begin{aligned}
\textit{Stmt} \rightarrow\ & \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{Stmt} \\
\mid\ & \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{Stmt}\ \textbf{else}\ \textit{Stmt} \\
\mid\ & \ldots\ \text{other stmts} \ldots
\end{aligned}$$

> **This ambiguity is inherent in the grammar.**

**The ambiguous sentential form**: `if Expr₁ then if Expr₂ then Stmt₁ else Stmt₂`

```
   INTERPRETATION A                        INTERPRETATION B
   else binds to INNER if                  else binds to OUTER if
   (production 2, then production 1)       (production 1, then production 2)

        if E₁                                   if E₁
        └─ then if E₂                           └─ then if E₂
                └─ then S₁                              └─ then S₁
                └─ else S₂                      └─ else S₂   ◄── belongs here
```

> **Why it matters**: *"Part of the problem is that the structure built by the parser will determine the interpretation of the code, and these two forms have **different meanings**!"*

### 10.2 Removing the Ambiguity — Match Else to the Innermost If

> **Must rewrite the grammar to avoid generating the problem. Match each `else` to the innermost unmatched `if` (the common-sense rule).**

$$\begin{aligned}
0:\ & \textit{Stmt} \rightarrow \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{Stmt} \\
1:\ & \phantom{0:}\ \mid\ \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{WithElse}\ \textbf{else}\ \textit{Stmt} \\
2:\ & \phantom{0:}\ \mid\ \textit{Other Statements} \\[6pt]
3:\ & \textit{WithElse} \rightarrow \textit{Other Statements} \\
4:\ & \phantom{3:}\ \mid\ \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{WithElse}\ \textbf{else}\ \textit{WithElse}
\end{aligned}$$

### 10.3 Why This Fix Works — The Intuition

> **With this grammar, the example has only one rightmost derivation. Intuition: once into `WithElse`, we cannot generate an unmatched `else` … a final `if` without an `else` can only come through rule 2 …**

**The only rightmost derivation:**

| Rule | Sentential Form |
| :--- | :--- |
| — | $\textit{Stmt}$ |
| 0 | $\textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{Stmt}$ |
| 1 | $\textbf{if}\ \textit{Expr}\ \textbf{then}\ \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{WithElse}\ \textbf{else}\ \textit{Stmt}$ |
| 2 | $\textbf{if}\ \textit{Expr}\ \textbf{then}\ \textbf{if}\ \textit{Expr}\ \textbf{then}\ \textit{WithElse}\ \textbf{else}\ S_2$ |
| 4 | $\textbf{if}\ \textit{Expr}\ \textbf{then}\ \textbf{if}\ \textit{Expr}\ \textbf{then}\ S_1\ \textbf{else}\ S_2$ |
| ? | $\textbf{if}\ E_1\ \textbf{then}\ \textbf{if}\ E_2\ \textbf{then}\ S_1\ \textbf{else}\ S_2$ |

> ✅ **The grammar forces the structure to match the desired meaning** — the inner `if` gets the `else`.

> **Exam Tip**: If asked *"State the dangling-else problem and rewrite the grammar to remove the ambiguity"*, give (a) the ambiguous grammar, (b) the sentence `if E₁ then if E₂ then S₁ else S₂`, (c) the `Stmt`/`WithElse` rewrite above, and (d) the one-sentence intuition about `WithElse` never generating an unmatched else. That is a complete 10-mark answer.

---

## 11. Deeper Ambiguity — When Rewriting Is Not Enough

> **Ambiguity usually refers to confusion in the CFG. But overloading can create deeper ambiguity.**

**Example — function call vs array subscript:**
$$a = f(17)$$

> *"In many Algol-like languages, `f` could be either a **function** or a **subscripted variable**."*

**Disambiguating this one requires context**:
- Need **values of declarations** (is `f` declared as a function or an array?).
- **Really an issue of *type*, not context-free syntax.**
- **Requires an extra-grammatical solution (not in CFG).**
- **Must handle these with a different mechanism** — step **outside the grammar** rather than use a more complex grammar.

---

## 12. Ambiguity — The Final Word

> **Ambiguity arises from two distinct sources:**
> 1. **Confusion in the context-free syntax** (e.g., the dangling `if-then-else`)
> 2. **Confusion that requires context to resolve** (e.g., overloading)

| Source | **How to resolve** |
| :--- | :--- |
| **Context-free ambiguity** | **Rewrite the grammar** (precedence nonterminals, `WithElse` trick). |
| **Context-sensitive ambiguity** | **Takes cooperation**: Knowledge of declarations, types, …; **Accept a superset of $L(G)$ & check it by other means** (e.g., a semantic-analysis pass or symbol table). This is a **language design problem**. |
| Sometimes | The compiler writer **accepts an ambiguous grammar** and uses **parsing techniques that "do the right thing"** (i.e., always select the same derivation — e.g., shift/reduce conflict resolution in yacc defaults to shift, which picks the dangling-else attachment). |

---

## 13. Top-Down vs Bottom-Up Parsing (The Roadmap)

The slides list these as the two families we will build. Here is the comparison you need.

```
   TOP-DOWN (predictive)                     BOTTOM-UP (reductive)
   Build the tree from the ROOT DOWN         Build the tree from the LEAVES UP

            S                                        S
          ╱   ╲                                    ╱   ╲
        NP     VP        ◄── predict             NP     VP
        │      │                                  │      │
      "the"   "dog"                            "the"   "dog"
        ▲                                         ▲
      start at S                              start at words,
      try to derive the sentence              reduce words into
                                              phrases into S
```

| Aspect | **Top-Down Parsing** | **Bottom-Up Parsing** |
| :--- | :--- | :--- |
| **Direction** | Root → leaves | Leaves → root |
| **Core question** | "Which production predicts this input?" | "Which symbols reduce to a nonterminal?" |
| **Typical algorithms** | **Recursive descent**, **LL(1)** (predictive table-driven) | **Shift-reduce**, **LR(1)**, SLR, LALR |
| **Stack behaviour** | Push **expected** symbols (goal-directed) | Push **seen** symbols; **reduce** on a handle |
| **Key problems** | **Left recursion** (infinite loop), requires **left factoring**; needs FIRST/FOLLOW sets | **Shift/reduce** and **reduce/reduce** conflicts |
| **Grammar power** | Weaker (LL is a proper subset of LR) | **Stronger** (handles a superset of LL grammars) |
| **Derivation discovered** | **Leftmost** | **Rightmost (in reverse)** |
| **Hand-written?** | ✅ Easy to hand-code (recursive descent) | ❌ Usually generated by a tool (yacc/bison) |
| **Slide's roadmap** | *"Generated LL(1) parsers & hand-coded recursive descent parsers — Lab 2"* | *"Generated LR(1) parsers"* |

> **Exam Tip**: The classic 2-marker is *"Which derivation does a top-down parser construct?"* → **Leftmost**. *"Which does a bottom-up parser construct?"* → **Rightmost, in reverse**.

### 13.1 Recursive Descent (Top-Down, Hand-Coded)

Write one function per nonterminal; each function tries the productions left to right:

```
parse_Expr():
    parse_Term()
    while next_token is '+' or '-':
        consume()
        parse_Term()

parse_Term():
    parse_Factor()
    while next_token is '*' or '/':
        consume()
        parse_Factor()

parse_Factor():
    if next_token is NUMBER or ID: consume()
    elif next_token is '(': consume(); parse_Expr(); expect(')')
```

**⚠️ Left recursion is fatal for this approach**: $A \rightarrow A\alpha \mid \beta$ causes infinite recursion. Fix by rewriting to $A \rightarrow \beta A'$ and $A' \rightarrow \alpha A' \mid \epsilon$.

---

## 14. The Chomsky Hierarchy (One-Line Reference)

| Type | Grammar | Automaton | Restriction on Productions |
| :--- | :--- | :--- | :--- |
| **Type 3** | Regular | Finite State Automaton | $A \rightarrow aB$ or $A \rightarrow a$ |
| **Type 2** | **Context-Free** | **Push-Down Automaton** | $A \rightarrow \gamma$, **single nonterminal LHS** |
| **Type 1** | Context-Sensitive | Linear Bounded Automaton | $\alpha A \beta \rightarrow \alpha \gamma \beta$, $\gamma \ne \epsilon$ |
| **Type 0** | Recursively Enumerable | Turing Machine | Unrestricted |

---

## 15. Solved Exercises

### Exercise 1: Identify the Grammar Type
*Is `aXb → aYb` context-free or context-sensitive?*
👉 **Context-sensitive** — the LHS has symbols **around** the nonterminal ($a$ and $b$), i.e., it encodes **context**. A CFG would need a single nonterminal on the LHS.

### Exercise 2: Why Can't a DFA Parse `a^n b^n`?
👉 Recognizing $a^n b^n$ requires the machine to **count** an unbounded number of `a`s and match them to `b`s. A **DFA has finite memory** (finitely many states) so it cannot store an arbitrary count. A **push-down automaton (stack)** can.

### Exercise 3: Derivation vs Parse Tree
*Two different leftmost derivations produce the same parse tree. Is the grammar ambiguous?*
👉 If there are **two distinct leftmost derivations** for a sentential form, the grammar **is ambiguous by definition**. If the parse tree is genuinely the same, then the derivations are the same — so check carefully: **ambiguity = more than one leftmost (or rightmost) derivation**.

### Exercise 4: Precedence Levels
*Why does the grammar $G_2$ produce the same parse tree from both leftmost and rightmost derivations, unlike $G_1$?*
👉 Because $G_2$ **explicitly encodes precedence** by introducing one nonterminal per precedence level (`Expr` for +/−, `Term` for *//, `Factor` for parentheses/atoms) and forcing **higher-precedence subexpressions to be derived first**. There is only one legal structure, so both derivation orders converge on the same tree.

### Exercise 5: Add Precedence
*Add exponentiation (`^`, right-associative, highest precedence) to grammar $G_2$.*
👉 Insert a new level between `Term` and `Factor`:
```
Term   → Term * Power | Term / Power | Power
Power  → Factor ^ Power | Factor      (right recursion ⇒ right associativity)
Factor → number | id | ( Expr )
```

### Exercise 6: Dangling Else
*Rewrite `Stmt → if E then Stmt | if E then Stmt else Stmt | other` to make `else` bind to the nearest `if`.*
👉 Use the `WithElse` split from Section 10.2 — the key is that the **then-branch that can be followed by an else must be a `WithElse`**, and `WithElse` can never generate an unmatched `if`.

### Exercise 7: Sentential Form or Sentence?
*Is `the dog chased` a sentential form or a sentence, given a grammar where `S → NP VP`, `NP → Det N`?*
👉 If every symbol is a terminal (a word), it is a **sentence** in $L(G)$. Here all symbols are terminals, so `the dog chased` as a terminal string is a **sentence**; but `the dog NP` would be a **sentential form** (contains the nonterminal `NP`).

---

## 🎯 Exam Practice Checklist: Questions to Master
- [ ] Define parsing and state the two things a parser needs (a grammar $G$ and a membership algorithm for $L(G)$).
- [ ] Write the grammar 4-tuple $G=(S,N,T,P)$ with the LHS restriction.
- [ ] Explain why regular languages/DFAs are insufficient (`p^k q^k`, `wcw^r`, nesting) and what REs *can* do.
- [ ] Define context-free vs context-sensitive grammars by their LHS.
- [ ] State the push-down automaton characterization of CFLs.
- [ ] Reproduce both the leftmost and rightmost derivation tables for $x - 2 * y$ with grammar $G_1$.
- [ ] Explain how the two derivations yield $x-(2*y)$ vs $(x-2)*y$ — and why that's bad.
- [ ] Write the classic expression grammar $G_2$ and the rightmost derivation of $x - 2 * y$.
- [ ] Define ambiguous grammar precisely (two leftmost or two rightmost derivations).
- [ ] Present the dangling-else ambiguity and the `Stmt`/`WithElse` fix with its intuition.
- [ ] Explain the `a = f(17)` deeper ambiguity and why it needs a non-CFG solution.
- [ ] Compare top-down and bottom-up parsing (direction, algorithms, derivation discovered, conflicts).

---
*Next Topic: [Module 07: Probabilistic Parsing — PCFG & Beyond](./07_Probabilistic_Parsing_PCFG_and_Beyond.md)*
