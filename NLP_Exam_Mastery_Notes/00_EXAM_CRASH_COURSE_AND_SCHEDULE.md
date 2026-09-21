# 🚀 NLP Exam Fast-Track Master Guide & Study Schedule
**Course**: Natural Language Processing (CS702 / B.Tech 7th Semester / CSE) — IIIT Surat
**Instructor**: Shreya Agarwal
**Target Audience**: Students preparing for an exam from scratch and wanting *one* resource that covers **everything**
**Total Recommended Prep Time**: 5.5 – 6.5 Hours

---

## 🎯 The "Zero to Exam Ready" Battle Plan

NLP exams (and this course in particular) follow a **very predictable pattern**. Almost every question falls into one of these five buckets:

1. **Definitions & Formal Models** (NLP vs formal language, corpus, types vs tokens, morphemes, CFG 4-tuple, noisy channel model).
2. **Mathematical Derivations** (Chain rule → n-gram approximation, MLE bigram probability, Laplace smoothing, perplexity, HMM joint probability, Bayes' rule for correction).
3. **Step-by-Step Numerical Traces** (Bigram probability calculation, Viterbi trace on `I fish`, perplexity computation, edit-distance candidate scoring for `acress`).
4. **Algorithm Explanations with Diagrams** (Hidden Markov Model tagging, Brill/Transformation-Based Tagging, Viterbi, Top-down vs Bottom-up parsing, CYK, GMM-UBM, HMM-GMM/DNN-HMM ASR).
5. **Comparisons & Trade-offs** (HMM vs TBT, Backoff vs Interpolation, PCFG vs Lexicalized PCFG, GMM-HMM vs DNN-HMM, Las vs CTC).

By reviewing the **plain-English explanations**, **worked numerical traces**, **drawn tables/diagrams**, and **exam answer templates** in these notes, you can walk into the exam hall fully prepared.

---

## 🗺️ The NLP Pipeline (The Big Picture You Must Have in Your Head)

Everything in this course slots into one master pipeline. If you remember this diagram, you can never get lost:

```
                          THE NLP PIPELINE
 ┌──────────────────────────────────────────────────────────────────────────┐
 │  RAW TEXT / SPEECH                                                       │
 └───────────────────────────────────┬──────────────────────────────────────┘
                                     ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │ 1. TEXT NORMALIZATION     Tokenization → Word-format Norm → Sentence Segmentation │
 │                           (Unit 1)                                       │
 └───────────────────────────────────┬──────────────────────────────────────┘
                                     ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │ 2. MORPHOLOGICAL ANALYSIS  FST / FSA: foxes → fox + es   (Unit 1)        │
 └───────────────────────────────────┬──────────────────────────────────────┘
                                     ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │ 3. LANGUAGE MODELLING      N-grams, MLE, Smoothing, Perplexity (Unit 1)  │
 └───────────────────────────────────┬──────────────────────────────────────┘
                                     ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │ 4. POS TAGGING             HMM + Viterbi  |  Brill TBT       (Unit 2)    │
 └───────────────────────────────────┬──────────────────────────────────────┘
                                     ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │ 5. PARSING                 CFG → PCFG → Lexicalized → Dependency         │
 │                            Top-down / Bottom-up / CYK        (Parsing)   │
 └───────────────────────────────────┬──────────────────────────────────────┘
                                     ▼
 ┌──────────────────────────────────────────────────────────────────────────┐
 │ 6. APPLICATIONS            Spelling Correction (Noisy Channel)           │
 │                            Speech (GMM-UBM, I-vectors, DNN-CTC, LAS)     │
 └──────────────────────────────────────────────────────────────────────────┘
```

---

## ⏱️ 6-Hour Emergency Cramming Schedule

| Block | Time | Topic & File | Goal / Deliverable |
| :--- | :--- | :--- | :--- |
| **Block 1** | **40 mins** | **Module 01: NLP Foundations, Languages & Corpus**<br>[`01_NLP_Foundations_Languages_and_Corpus.md`](./01_NLP_Foundations_Languages_and_Corpus.md) | Master the 10 NLP tasks, 6 levels of language knowledge, corpus/types/tokens, normalization, TreeBank, WordNet relations. |
| **Block 2** | **40 mins** | **Module 02: Morphology & FSTs**<br>[`02_Morphology_and_Finite_State_Transducers.md`](./02_Morphology_and_Finite_State_Transducers.md) | Distinguish inflectional vs derivational morphology; write the FST 6-tuple; know FST applications. |
| **Block 3** | **60 mins** | **Module 03: N-Gram Language Models & Perplexity**<br>[`03_N_Gram_Language_Models_and_Perplexity.md`](./03_N_Gram_Language_Models_and_Perplexity.md) | Chain rule → Markov assumption → MLE; compute bigram probabilities by hand; compute perplexity. |
| **Break** | **10 mins** | Rest eyes, drink water, stretch | Let the probability formulas settle. |
| **Block 4** | **45 mins** | **Module 04: Smoothing, Backoff, Interpolation & Entropy**<br>[`04_Smoothing_Backoff_Interpolation_and_Entropy.md`](./04_Smoothing_Backoff_Interpolation_and_Entropy.md) | Laplace/Add-k, Stupid Backoff, Katz Backoff, Interpolation; entropy ↔ perplexity relation. |
| **Block 5** | **50 mins** | **Module 05: POS Tagging — HMM, Viterbi & Brill TBT**<br>[`05_POS_Tagging_HMM_Viterbi_and_Brill_TBT.md`](./05_POS_Tagging_HMM_Viterbi_and_Brill_TBT.md) | Write emission/transition formulas; reproduce the `I fish` Viterbi trace; explain the 5-step TBT learning loop. |
| **Block 6** | **50 mins** | **Module 06 & 07: Parsing & Probabilistic Parsing**<br>[`06_Introduction_to_Parsing_CFG_and_Ambiguity.md`](./06_Introduction_to_Parsing_CFG_and_Ambiguity.md)<br>[`07_Probabilistic_Parsing_PCFG_and_Beyond.md`](./07_Probabilistic_Parsing_PCFG_and_Beyond.md) | CFG 4-tuple, leftmost/rightmost derivations, ambiguity + dangling-else fix; PCFG independence problems, lexicalization, PARSEVAL, search methods. |
| **Break** | **10 mins** | Short walk | Reset before the applications half. |
| **Block 7** | **35 mins** | **Module 08: Spelling Correction & Noisy Channel**<br>[`08_Spelling_Correction_and_Noisy_Channel.md`](./08_Spelling_Correction_and_Noisy_Channel.md) | Bayes' rule for spelling, edit-distance operations, the `acress` candidate example, pronunciation-variation Bayes. |
| **Block 8** | **50 mins** | **Module 09: Speech Processing & ASR**<br>[`09_Speech_Processing_and_ASR.md`](./09_Speech_Processing_and_ASR.md) | MFCC 39-dim features, GMM & GMM-UBM/MAP, JFA & I-vectors, HMM 3 problems, WER, CTC & LAS. |
| **Block 9** | **40 mins** | **Module 10: High-Yield Exam Review**<br>[`10_High_Yield_Exam_Questions_and_Answers.md`](./10_High_Yield_Exam_Questions_and_Answers.md) | Rapid-fire 2-mark definitions and 10-mark blueprints. |

---

## 📺 Handpicked YouTube Crash-Course Lectures

Watch these at 1.5× while reading the matching module:

| Topic | Recommended YouTube Lecture | Creator / Channel | What to Focus On |
| :--- | :--- | :--- | :--- |
| **NLP Intro & Pipeline** | *"Introduction to Natural Language Processing"* | **Gate Smashers** | The 5 phases of NLP and the ambiguity examples at each level. |
| **Morphology** | *"Morphology in NLP \| Inflectional vs Derivational"* | **Gate Smashers** | Stem vs affixes, the 4 word-formation processes. |
| **Finite State Transducers** | *"Finite State Transducer in NLP with Examples"* | **Gate Smashers** | Input tape / output tape, the `fox+N+PL → foxes` mapping. |
| **N-Gram Language Models** | *"N-Gram Language Model with Example"* | **Gate Smashers** / **Dr. Shahid** | Chain rule, Markov assumption, MLE bigram formula. |
| **Smoothing Techniques** | *"N-Gram Smoothing: Laplace, Backoff, Interpolation"* | **Gate Smashers** | Why zero counts break the model, and the fix formula for each method. |
| **Perplexity** | *"Perplexity in NLP / Language Models"* | **Gate Smashers** | Why lower perplexity = better model, and the log-probability form. |
| **POS Tagging & HMM** | *"Part of Speech Tagging using Hidden Markov Model"* | **Gate Smashers** | Emission vs transition probability, the two tables. |
| **Viterbi Algorithm** | *"Viterbi Algorithm Explained with Example"* | **Abdul Bari** / **Gate Smashers** | The trellis/table construction and back-pointers. |
| **Brill Tagging / TBT** | *"Transformation Based Learning (Brill Tagging)"* | **NPTEL / Lectures by Ravindrababu Ravula** | Initial tagging → rule templates → error-reduction loop. |
| **CFG & Parsing** | *"Context Free Grammar and Parse Tree in NLP"* | **Gate Smashers** | Leftmost vs rightmost derivation, ambiguity, dangling-else. |
| **PCFG** | *"Probabilistic Context Free Grammar (PCFG)"* | **Gate Smashers** | Rule probability computation and independence assumptions. |
| **Spelling Correction** | *"Noisy Channel Model for Spelling Correction"* | **NPTEL NLP** | Bayes decomposition into error model × language model. |
| **Speech Processing / ASR** | *"Automatic Speech Recognition: MFCC, GMM-HMM, CTC"* | **NPTEL / Deep Learning with Python** | 20–30 ms frames → MFCC → GMM/HMM → CTC → LAS. |

*(Pro-tip: search the exact quoted title on YouTube to land the top-ranked video immediately).*

---

## 📊 Master Cheat Sheet 1: Language Model Formulas

| Concept | Formula | Remember This |
| :--- | :--- | :--- |
| **Chain Rule** | $P(w_{1:n}) = \prod_{k=1}^{n} P(w_k \mid w_{1:k-1})$ | Exact, but computationally impossible for long histories. |
| **Bigram (Markov) Assumption** | $P(w_n \mid w_{1:n-1}) \approx P(w_n \mid w_{n-1})$ | Only one word of history is used. |
| **Bigram Sequence Probability** | $P(w_{1:n}) \approx \prod_{k=1}^{n} P(w_k \mid w_{k-1})$ | Multiply all bigram probabilities. |
| **MLE Bigram Estimate** | $P(w_n \mid w_{n-1}) = \dfrac{C(w_{n-1} w_n)}{C(w_{n-1})}$ | Relative frequency. **Zero if the bigram is unseen!** |
| **Laplace (Add-One)** | $P(w_n \mid w_{n-1}) = \dfrac{C(w_{n-1} w_n) + 1}{C(w_{n-1}) + V}$ | $V$ = vocabulary size (must be added to the denominator too!). |
| **Add-$k$** | $P(w_n \mid w_{n-1}) = \dfrac{C(w_{n-1} w_n) + k}{C(w_{n-1}) + kV}$ | $k=1$ gives Laplace; $k<1$ is gentler. |
| **Linear Interpolation** | $P(w_n \mid w_{n-2}w_{n-1}) = \lambda_1 P(w_n) + \lambda_2 P(w_n \mid w_{n-1}) + \lambda_3 P(w_n \mid w_{n-2}w_{n-1})$ | $\sum_i \lambda_i = 1$. All orders always mixed. |
| **Katz Backoff** | $P(w_n \mid w_{n-1}) = \begin{cases} P^*(w_n \mid w_{n-1}) & C > 0 \\ \alpha(w_{n-1}) P(w_n) & C = 0 \end{cases}$ | Back off **only when count is zero**. |
| **Stupid Backoff** | $S(w_i \mid w_{i-k+1:i-1}) = \begin{cases} \frac{C(w_{i-k+1:i})}{C(w_{i-k+1:i-1})} & C > 0 \\ 0.4\, S(\text{lower order}) & \text{otherwise} \end{cases}$ | Not a true probability (doesn't sum to 1) but works great at scale. |
| **Perplexity** | $PP(W) = P(w_1 w_2 \ldots w_N)^{-1/N} = \sqrt[N]{\dfrac{1}{P(w_1 w_2 \ldots w_N)}}$ | **Lower is better.** Inverse probability normalized by $N$. |
| **Perplexity (with chain rule)** | $PP(W) = \sqrt[N]{\prod_{i=1}^{N} \dfrac{1}{P(w_i \mid w_1 \ldots w_{i-1})}}$ | Wall-clock form. |
| **Perplexity from Entropy** | $PP(W) = 2^{H(W)}$ | Entropy in bits. Lower entropy ⇒ lower perplexity. |
| **Cross-Entropy** | $H(p,q) = -\sum_x p(x) \log_2 q(x)$ | Used when the true distribution $p$ is replaced by a model $q$. |

---

## 📊 Master Cheat Sheet 2: Smoothing Methods Side-by-Side

| Method | Core Idea | Uses Lower-Order Info? | True Probability? | Exam Relevance |
| :--- | :--- | :--- | :--- | :--- |
| **Laplace (Add-One)** | Add 1 to every count | ❌ No | ✅ Yes | Highest — easy numericals. |
| **Add-$k$** | Add $k < 1$ to every count | ❌ No | ✅ Yes | Medium. |
| **Stupid Backoff** | Multiply lower order by 0.4 | ✅ Yes | ❌ No | High — explicitly named in slides. |
| **Katz Backoff** | Back off only on zero counts, redistributed via $\alpha$ | ✅ Yes | ✅ Yes (with Good–Turing discounts) | High. |
| **Interpolation** | Always mix all orders with $\lambda$ weights | ✅ Yes | ✅ Yes | High — $\lambda$ learned on held-out corpus. |
| **Absolute Discounting / Kneser-Ney** | Subtract a fixed discount $d$, redistribute | ✅ Yes | ✅ Yes | Mention-level. |

---

## 📊 Master Cheat Sheet 3: Parsing & Grammar Roadmap

| Concept | One-Line Definition | Key Exam Point |
| :--- | :--- | :--- |
| **CFG** | Grammar $G=(S,N,T,P)$ specifying syntax | Single nonterminal on the LHS ⇒ context-free. |
| **Regular Languages** | Recognized by DFA | Cannot count unbounded nesting ($a^n b^n$, $wcw^r$). |
| **Derivation** | Sequence of rewrite steps from start symbol | **Leftmost** and **Rightmost** are the two systematic ones. |
| **Parse Tree** | Tree encoding a derivation | Same tree from leftmost & rightmost in an unambiguous grammar. |
| **Ambiguous Grammar** | ≥ 2 leftmost derivations for one sentential form | Classic example: dangling `else`. |
| **Top-Down Parser** | Starts at $S$, predicts downward | LL(1); recursive descent; suffers from left recursion. |
| **Bottom-Up Parser** | Starts at words, reduces upward | LR(1), shift-reduce; handles more grammars. |
| **PCFG** | CFG + probability on each rule | $P(T) = \prod$ rule probabilities. |
| **CYK** | Bottom-up DP parser for CNF grammars | $O(n^3)$ time; the "table" search method. |
| **PARSEVAL** | Evaluation metric for parsers | Precision/recall over constituents; ignores node labels & unary nodes. |

---

## 📊 Master Cheat Sheet 4: Speech & Bayesian Models

| Concept | Formula / Definition | Note |
| :--- | :--- | :--- |
| **Noisy Channel (Spelling)** | $\hat{c} = \arg\max_c P(w \mid c)\,P(c)$ | $P(w\|c)$ = error model, $P(c)$ = language model. |
| **Bayes for Pronunciation** | $P(V \mid A) = \dfrac{P(A \mid V) P(V)}{P(A)}$ | $V$ = variant, $A$ = acoustic observation. |
| **ASR Objective** | $\hat{W} = \arg\max_W P(W \mid Y) = \arg\max_W \underbrace{P(Y \mid W)}_{\text{acoustic}}\underbrace{P(W)}_{\text{LM}}$ | $Y$ = MFCC sequence, $W$ = word sequence. |
| **GMM-UBM** | Target model = UBM adapted via MAP | Works with limited target training data. |
| **I-vector** | Low-dim (e.g., 400) utterance representation | Variable-length speech → fixed vector. |
| **HMM 3 Problems** | Evaluation ($\alpha$, forward), Decoding (Viterbi), Learning (Baum-Welch/EM) | Classic exam question. |
| **WER** | $\text{WER} = \dfrac{S + I + D}{N}, \quad N = S + D + C$ | $S$=sub, $I$=insert, $D$=delete, $C$=correct. |
| **CTC Loss** | $P(Y \mid X) = \sum_{A \in \mathcal{A}(X,Y)} \prod_{t=1}^{T} p(a_t \mid X)$ | Sums over **all alignments** — no aligned data needed. |

---

## ✍️ How to Score Full Marks in NLP Descriptive Answers

Examiners love structured, technical answers. For any NLP question:

1. **1-Sentence Formal Definition**: State what it is and its family (e.g., *"An n-gram language model is a probabilistic model that approximates the probability of a word given its full history by conditioning only on the previous $n-1$ words."*).
2. **Mathematical Formulation**: Always write the equation (Chain rule, MLE, Bayes, Viterbi recurrence, perplexity).
3. **Assumptions Box**: Explicitly state the independence assumptions (Markov assumption for n-grams; bigram tag assumption for HMM; rule independence for PCFG). Marks are awarded *for naming the assumption*.
4. **Step-by-Step Algorithm / Process**: Bullet points showing initialization, iteration, termination.
5. **Draw a Trace Table or Diagram**: Never write walls of text. A 3-row Viterbi table or a small parse tree earns marks instantly.
6. **Numerical Sanity Check**: If the question gives numbers (counts, probabilities), plug them in and show the arithmetic.
7. **Properties / Comparison Box**: End with limitations and how the next model fixes them (e.g., *"Zeros in MLE → fixed by Laplace; PCFG context-blindness → fixed by lexicalization"*).

---

## 🧭 Suggested Order of Reading (If You Have Only 3 Hours)
1. **[Module 10: High-Yield Exam Q&A](./10_High_Yield_Exam_Questions_and_Answers.md)** — read the questions first to know what matters.
2. **[Module 03](./03_N_Gram_Language_Models_and_Perplexity.md)** + **[Module 04](./04_Smoothing_Backoff_Interpolation_and_Entropy.md)** — the numerical core.
3. **[Module 05](./05_POS_Tagging_HMM_Viterbi_and_Brill_TBT.md)** — the Viterbi trace is nearly guaranteed.
4. **[Module 06](./06_Introduction_to_Parsing_CFG_and_Ambiguity.md)** — derivation tables are easy marks.
5. Skim **[Module 09](./09_Speech_Processing_and_ASR.md)** for definitions (MFCC, GMM-UBM, I-vector, CTC, WER).

---

*Proceed directly to [Module 01: NLP Foundations, Languages & Corpus](./01_NLP_Foundations_Languages_and_Corpus.md) to begin studying!*
