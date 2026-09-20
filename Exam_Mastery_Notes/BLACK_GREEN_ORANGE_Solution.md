# BLACK + GREEN = ORANGE — Complete Cryptarithmetic Solution

## Puzzle Statement

```
    B L A C K
  + G R E E N
  ---------
  O R A N G E
```

Each letter represents a unique digit from 0–9. No two letters share the same digit.
Find the unique assignment that makes the addition valid.

**Letters involved (10 total):** B, L, A, C, K, G, R, E, N, O
**Digits available:** {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
**Key observation:** Exactly 10 distinct letters = all 10 digits are used, each exactly once.

---

## Step 0: Column Setup with Carries

Write every column from right (units) to left (hundred-thousands), labeling carries:

```
Column:    6    5    4    3    2    1
                    B    L    A    C    K
                  + G    R    E    E    N
                  ─────────────────────────
Carry in:   c5   c4   c3   c2   c1
Carry out:  c5   c4   c3   c2   c1

Column 1 (Units):      K + N           = 10·c1 + E
Column 2 (Tens):       C + E + c1      = 10·c2 + G
Column 3 (Hundreds):   A + E + c2      = 10·c3 + N
Column 4 (Thousands):  L + R + c3      = 10·c4 + A
Column 5 (Ten-thousands): B + G + c4   = 10·c5 + R
Column 6 (Hundred-thousands): c5       = O
```

**Carry rules:** Every carry cᵢ ∈ {0, 1} because the max sum of two single digits plus a carry is 9 + 8 + 1 = 18, producing at most a carry of 1.

---

## Step 1: Determine O (Overflow Carry) — Instant Anchor

From Column 6: `c5 = O`

Since c5 is a carry from Column 5 (adding two single digits + a carry), c5 can only be 0 or 1.

- O cannot be 0, because O is the leading digit of the 6-digit sum ORANGE. A leading digit of 0 would make it a 5-digit number — contradiction.
- Therefore **O = 1** and **c5 = 1**.

**Eliminated from scratchpad:** O = 1  
**Remaining digits:** {0, 2, 3, 4, 5, 6, 7, 8, 9} for {B, L, A, C, K, G, R, E, N}

---

## Step 2: The "Bridge" Trick — Cancel Shared Letters

**Why this matters:** Two columns share letters E and N but place them on opposite sides of the equation. Adding those two column equations cancels both E and N, eliminating two variables at once.

**Column 1 (Units):** `K + N = 10·c1 + E` ... (Eq. 1)

**Column 3 (Hundreds):** `A + E + c2 = 10·c3 + N` ... (Eq. 3)

**Add Eq. 1 and Eq. 3:**

Left side: `(K + N) + (A + E + c2)` = `K + N + A + E + c2`

Right side: `(10·c1 + E) + (10·c3 + N)` = `10·c1 + E + 10·c3 + N`

**Cancel N** (appears on both left and right):
`K + A + E + c2 = 10·c1 + E + 10·c3`

**Cancel E** (appears on both sides):
`K + A + c2 = 10·c1 + 10·c3`

**Factor the right side:**
```
A + K + c2 = 10·(c1 + c3)          ... (Eq. Bridge)
```

### Analyze Eq. Bridge

**Left side range:**
- A and K are distinct digits from {0, 2, 3, 4, 5, 6, 7, 8, 9} (1 is already taken by O)
- Maximum A + K = 9 + 8 = 17
- c2 ∈ {0, 1}
- Maximum of left side = 17 + 1 = 18
- Minimum of left side = 0 + 2 + 0 = 2 (smallest two available digits are 0 and 2)

**Right side:** 10·(c1 + c3), where c1, c3 ∈ {0, 1}
- Possible values: 0, 10, or 20

**The only multiple of 10 in range [2, 18] is 10.**

Therefore:
```
c1 + c3 = 1          (exactly one carry is 1, the other is 0)
A + K + c2 = 10      (locked relationship)
```

**Tip:** This is the critical insight. The bridge trick collapsed two columns and eliminated two letters (E and N), giving us a hard constraint on A, K, and c2.

---

## Step 3: Branch on the Carry Split

Since `c1 + c3 = 1`, there are exactly **two branches**:

- **Branch 1:** c1 = 0, c3 = 1
- **Branch 2:** c1 = 1, c3 = 0

We test Branch 1 first to see if it leads to a contradiction.

---

### Branch 1: c1 = 0, c3 = 1

**Column 1 becomes:** `K + N = 10·0 + E` → `K + N = E` ... (Eq. 1a)

**Column 3 becomes:** `A + E + c2 = 10·1 + N` → `A + E + c2 = 10 + N` ... (Eq. 3a)

From Eq. 1a: `E = K + N`

Since O = 1, neither K nor N can be 1. The smallest available digits for K and N are 0 and 2 (or 2 and 0).

**Case: K = 0 or N = 0?** If K = 0, then E = N, which violates the uniqueness rule. If N = 0, then E = K, also a violation. So neither K nor N can be 0 either (since one of them being 0 forces E to equal the other).

Wait — let's check more carefully. If K = 0, E = 0 + N = N, so E = N. Contradiction (two letters same digit). If N = 0, E = K + 0 = K, so E = K. Contradiction.

So K ≥ 2, N ≥ 2 (both excluded from {0, 1}).

Minimum K + N = 2 + 3 = 5, so E ≥ 5.

**Now use Eq. Bridge: A + K + c2 = 10**

From Eq. 3a: `N = A + E + c2 - 10`

Substitute E = K + N from Eq. 1a into Eq. 3a:
```
A + (K + N) + c2 = 10 + N
A + K + N + c2 = 10 + N
A + K + c2 = 10
```
This is consistent (just gives us Eq. Bridge again, no new info).

**Column 2:** `C + E + c1 = 10·c2 + G` → `C + E + 0 = 10·c2 + G` → `C + E = 10·c2 + G` ... (Eq. 2a)

**Column 4:** `L + R + c3 = 10·c4 + A` → `L + R + 1 = 10·c4 + A` ... (Eq. 4a)

**Column 5:** `B + G + c4 = 10·1 + R` → `B + G + c4 = 10 + R` ... (Eq. 5a)

Now, if c2 = 0:
- Eq. 2a: `C + E = G`
- Since E ≥ 5 and C ≥ 0, G = C + E ≥ 5. But G must be distinct from E, and C + E ≤ 9 (since G is a single digit), so C ≤ 9 - E ≤ 4.
- Eq. Bridge: A + K = 10 (since c2 = 0)

If c2 = 1:
- Eq. 2a: `C + E = 10 + G` → `G = C + E - 10`
- Since E ≥ 5, G = C + E - 10. For G ≥ 0, need C ≥ 10 - E. If E = 5, C ≥ 5, but then G = C - 5, and G could equal E or other letters. This creates tight constraints.

Let's push further with c2 = 0:
- A + K = 10, possible pairs from remaining digits {0, 2, 3, 4, 5, 6, 7, 8, 9}: (2,8), (3,7), (4,6)
- E ≥ 5, so E ∈ {5, 6, 7, 8, 9}
- C + E = G, with C ≥ 2 (since 0, 1 restricted), G must be a valid digit ≤ 9

For E = 5: C + 5 = G, C ≥ 2, so G ≥ 7. Available: C ∈ {2,3,4}, G ∈ {7,8,9}. But A + K = 10 already uses two digits. If (A,K) = (2,8), then C can be 3 or 4, G = 8 or 9. G = 8 conflicts with K = 8. So C = 4, G = 9. Then N and K are related by E = K + N: 5 = K + N. But K = 2 or 8. If K = 2, N = 3 (but A = 2 already used). If K = 8, N = -3 (impossible).

For E = 6: C + 6 = G. If (A,K) = (2,8): C ∈ {0,3,4,5}, G = C+6. C=0→G=6=E (conflict). C=3→G=9. C=4→G=10 (invalid). C=5→G=11 (invalid). So C=3, G=9. Then E = K + N → 6 = 8 + N → N = -2 (impossible).

For E = 7: (A,K) = (2,8): 7 = 8 + N → N = -1 (impossible). (A,K) = (3,7): E = 7 conflicts with K = 7. (A,K) = (4,6): 7 = 6 + N → N = 1, but O = 1 (conflict).

For E = 8: (A,K) = (2,8): E = 8 = K, conflict. (A,K) = (3,7): 8 = 7 + N → N = 1 = O, conflict. (A,K) = (4,6): 8 = 6 + N → N = 2. Then C + 8 = G. Available digits for C, G from {0, 5, 9}. C = 0 → G = 8 = E, conflict. C = 5 → G = 13 (invalid). Dead end.

For E = 9: (A,K) = (2,8): 9 = 8 + N → N = 1 = O, conflict. (A,K) = (3,7): 9 = 7 + N → N = 2. C + 9 = G → G ≥ 9, but 9 is taken. Dead end. (A,K) = (4,6): 9 = 6 + N → N = 3. C + 9 = G → same issue, G ≥ 9, only 9 available which is taken. Dead end.

**Branch 1 is completely exhausted with c2 = 0.**

Now test Branch 1 with c2 = 1:
- Eq. 2a: C + E = 10 + G → G = C + E - 10
- Eq. Bridge: A + K + 1 = 10 → A + K = 9
- Possible (A,K) pairs summing to 9: (0,9), (2,7), (3,6), (4,5) — avoiding 1
- E ≥ 5 (from earlier: K ≥ 2, N ≥ 2, E = K + N ≥ 4, but actually K,N ≥ 2 and distinct, so min E = 2+3 = 5)

For (A,K) = (0,9): E = 9 + N, but K = 9, so E = 9 + N ≥ 11 (invalid for single digit). Dead end.

For (A,K) = (2,7): E = 7 + N. E must be ≥ 5 and ≤ 9. N ≥ 2 but N ≠ 2 (A=2), N ≠ 7 (K=7), N ≠ 1 (O=1). So N ∈ {3,4,5,6,8,9}. E = 7+N: N=3→E=10 (invalid). Dead end since minimum N gives E > 9.

For (A,K) = (3,6): E = 6 + N. N ≥ 2, N ≠ 1,3,6. N=2→E=8. N=4→E=10 (invalid). So only N=2, E=8. G = C + 8 - 10 = C - 2. Available digits for C: {0, 4, 5, 7, 9}. G = C-2 must be ≥ 0 and available. C=4→G=2=N conflict. C=5→G=3=A conflict. C=7→G=5. C=9→G=7=K conflict. So C=7, G=5. Remaining digits for {B, L, R}: {0, 4, 9}. Column 4: L + R + 1 = 10·c4 + 3 → L + R = 10·c4 + 2. If c4=0: L+R=2, from {0,4,9} min sum = 0+4=4 > 2. If c4=1: L+R=12, from {0,4,9}: 0+9=9≠12, 4+9=13≠12, 0+4=4≠12. Dead end.

For (A,K) = (4,5): E = 5 + N. K=5, so E = 5+N. N≠1,4,5. N=2→E=7. N=3→E=8. N=6→E=11 (invalid).
  - Sub-case N=2, E=7: G = C+7-10 = C-3. Available digits for C from {0,3,6,8,9}. G=C-3≥0→C≥3. C=3→G=0. C=6→G=3. C=8→G=5=K conflict. C=9→G=6. Check Column 4: L + R + 1 = 10·c4 + 4 → L + R = 10·c4 + 3. Remaining for {B,L,R}: whatever is left. If C=3,G=0: remaining {6,8,9} for {B,L,R}. L+R = 10c4+3. Pairs: 6+8=14, 6+9=15, 8+9=17. None equal 3 or 13. Dead end. If C=6,G=3: remaining {0,8,9}. L+R: 0+8=8, 0+9=9, 8+9=17. Need 3 or 13. Dead end. If C=9,G=6: remaining {0,3,8}. L+R: 0+3=3 ✓ (c4=0). So L+R=3, c4=0. Then Column 5: B + G + c4 = 10 + R → B + 6 + 0 = 10 + R → B = R + 4. Remaining {B,L,R} = {0,3,8} with L+R=3 and B=R+4. If R=0→B=4=A conflict. If R=3→B=7 (not available, 7 not in remaining). If R=8→B=12 (invalid). Dead end.
  - Sub-case N=3, E=8: G = C+8-10 = C-2. Available for C from {0,2,6,7,9}. C≥2→G≥0. C=2→G=0. C=6→G=4=A conflict. C=7→G=5=K conflict. C=9→G=7. Remaining {B,L,R}: if C=2,G=0: remaining {6,7,9}. Column 4: L+R = 10c4+4. 6+7=13, 6+9=15, 7+9=16. Need 4 or 14. Dead end. If C=9,G=7: remaining {0,2,6}. L+R = 10c4+4. 0+2=2, 0+6=6, 2+6=8. Need 4 or 14. Dead end.

**Branch 1 is entirely dead. Move to Branch 2.**

---

### Branch 2: c1 = 1, c3 = 0 ✓

**Column 1:** `K + N = 10·1 + E` → `K + N = 10 + E` → `E = K + N - 10` ... (Eq. 1b)

**Column 3:** `A + E + c2 = 10·0 + N` → `N = A + E + c2` ... (Eq. 3b)

**Bridge equation:** `A + K + c2 = 10` (still holds) ... (Eq. Bridge)

Since E = K + N - 10 from Eq. 1b, and N = A + E + c2 from Eq. 3b:

Substitute N into Eq. 1b:
```
E = K + (A + E + c2) - 10
E = K + A + E + c2 - 10
0 = K + A + c2 - 10
A + K + c2 = 10
```
Consistent with Eq. Bridge. Good — we need another approach to make progress.

**Column 2:** `C + E + c1 = 10·c2 + G` → `C + E + 1 = 10·c2 + G` → `G = C + E + 1 - 10·c2` ... (Eq. 2b)

**Column 4:** `L + R + c3 = 10·c4 + A` → `L + R + 0 = 10·c4 + A` → `L + R = 10·c4 + A` ... (Eq. 4b)

**Column 5:** `B + G + c4 = 10·1 + R` → `B + G + c4 = 10 + R` → `B = R + 10 - G - c4` ... (Eq. 5b)

---

## Step 4: Determine c2

Now we need to decide c2 ∈ {0, 1}.

**If c2 = 1:**
- Eq. Bridge: A + K = 9
- Eq. 2b: G = C + E + 1 - 10 = C + E - 9
- Eq. 3b: N = A + E + 1
- Eq. 1b: E = K + N - 10

From N = A + E + 1 and E = K + N - 10:
E = K + (A + E + 1) - 10 → 0 = K + A + 1 - 10 → A + K = 9 ✓ (consistent)

So N = A + E + 1. Since N ≤ 9 and E ≥ 0, A ≤ 8. Also N = A + E + 1 ≥ E + 2 (since A ≥ 0 and A ≠ E... actually A could be 0).

G = C + E - 9. For G ≥ 0: C ≥ 9 - E. For G ≤ 9: C + E ≤ 18 (always true).

L + R = 10·c4 + A. c4 ∈ {0, 1}.

This branch has many possibilities. Let's check c2 = 0 first since it's often simpler.

**If c2 = 0:**
- Eq. Bridge: A + K = 10
- Eq. 2b: G = C + E + 1 (since 10·0 = 0)
- Eq. 3b: N = A + E
- Eq. 1b: E = K + N - 10

From N = A + E and E = K + N - 10:
E = K + (A + E) - 10 → 0 = K + A - 10 → A + K = 10 ✓

So N = A + E. Since N must be a single digit (≤ 9): A + E ≤ 9, so E ≤ 9 - A.

G = C + E + 1. Since G ≤ 9: C + E + 1 ≤ 9 → C + E ≤ 8.

**Available digits for A and K (A + K = 10, neither is 1):**
- (A, K) ∈ {(2, 8), (3, 7), (4, 6), (6, 4), (7, 3), (8, 2)}

Note: A cannot be 0 because A is the leading digit of the 5-digit number BLACK (wait — actually B is the leading digit, A is in the middle). So A can be 0. Let's check: A = 0 → K = 10 (impossible, not a digit). So A ≠ 0.

Similarly K = 0 → A = 10 (impossible). So K ≠ 0 either.

**Valid (A, K) pairs:** {2,8}, {3,7}, {4,6}, {6,4}, {7,3}, {8,2}

---

## Step 5: Solve with c2 = 0 — Try Each (A, K) Pair

### Sub-branch 5a: A = 2, K = 8

Scratchpad: O=1, A=2, K=8. Remaining: {0, 3, 4, 5, 6, 7, 9}

**N = A + E = 2 + E**

E must be from remaining digits such that N = 2 + E is also a remaining digit and N ≠ E.

Try E = 0: N = 2 = A. Conflict.
Try E = 3: N = 5. ✓ Both available.
Try E = 4: N = 6. ✓ Both available.
Try E = 5: N = 7. ✓ Both available.
Try E = 6: N = 8 = K. Conflict.
Try E = 7: N = 9. ✓ Both available.
Try E = 9: N = 11. Invalid (not single digit).

So possible (E, N) pairs: (3,5), (4,6), (5,7), (7,9)

**G = C + E + 1** and **C + E ≤ 8** (so G ≤ 9).

For each (E, N) pair, find valid C:

#### (E=3, N=5):
G = C + 4. C + 3 ≤ 8 → C ≤ 5. Available C from {0, 4, 6, 7, 9} (excluding N=5):
- C = 0 → G = 4. ✓ Available.
- C = 4 → G = 8 = K. Conflict.
- C = 6 → G = 10. Invalid.
Dead end for C > 0.

So C=0, G=4. Remaining for {B, L, R}: {6, 7, 9}.

**Column 4:** L + R = 10·c4 + A = 10·c4 + 2
**Column 5:** B = R + 10 - G - c4 = R + 10 - 4 - c4 = R + 6 - c4

If c4 = 0: L + R = 2. From {6,7,9}: min sum = 6+7 = 13 ≠ 2. Impossible.
If c4 = 1: L + R = 12. From {6,7,9}: 6+7=13≠12, 6+9=15≠12, 7+9=16≠12. Impossible.

**Dead end.**

#### (E=4, N=6):
G = C + 5. C + 4 ≤ 8 → C ≤ 4. Available C from {0, 3, 5, 7, 9}:
- C = 0 → G = 5. ✓
- C = 3 → G = 8 = K. Conflict.

So C = 0, G = 5. Remaining for {B, L, R}: {3, 7, 9}.

**Column 4:** L + R = 10·c4 + 2
**Column 5:** B = R + 10 - 5 - c4 = R + 5 - c4

If c4 = 0: L + R = 2. From {3,7,9}: min = 3+7 = 10 ≠ 2. Impossible.
If c4 = 1: L + R = 12. From {3,7,9}: 3+9 = 12 ✓. So {L,R} = {3,9} in some order.

Column 5: B = R + 5 - 1 = R + 4.
- If R = 3: B = 7. ✓ (7 is available). Then L = 9.
- If R = 9: B = 13. Invalid.

So **R = 3, B = 7, L = 9**.

**Verification of all columns:**

```
    B L A C K       7 9 2 0 8
  + G R E E N     + 5 3 4 4 6
  ---------       ---------
  O R A N G E     1 3 2 6 5 4
```

Column 1: 8 + 6 = 14. Write 4 (=E ✓), carry c1 = 1 ✓
Column 2: 0 + 4 + 1 = 5 (=G ✓), carry c2 = 0 ✓
Column 3: 2 + 4 + 0 = 6 (=N ✓), carry c3 = 0 ✓
Column 4: 9 + 3 + 0 = 12. Write 2 (=A ✓), carry c4 = 1 ✓
Column 5: 7 + 5 + 1 = 13. Write 3 (=R ✓), carry c5 = 1 ✓
Column 6: c5 = 1 (=O ✓) ✓

**SOLUTION FOUND:** B=7, L=9, A=2, C=0, K=8, G=5, R=3, E=4, N=6, O=1

Let's continue checking other sub-branches to confirm uniqueness.

#### (E=5, N=7):
G = C + 6. C + 5 ≤ 8 → C ≤ 3. Available C from {0, 3, 4, 6, 9}:
- C = 0 → G = 6. ✓
- C = 3 → G = 9. ✓

**Case C=0, G=6:** Remaining {B,L,R} from {3,4,9}.
Column 4: L + R = 10·c4 + 2. Column 5: B = R + 10 - 6 - c4 = R + 4 - c4.
If c4=0: L+R=2. Min from {3,4,9} = 7. Impossible.
If c4=1: L+R=12. From {3,4,9}: 3+9=12 ✓. So {L,R}={3,9}.
B = R + 4 - 1 = R + 3. R=3→B=6=G conflict. R=9→B=12 invalid. Dead end.

**Case C=3, G=9:** Remaining {B,L,R} from {0,4,6}.
Column 4: L + R = 10·c4 + 2. Column 5: B = R + 10 - 9 - c4 = R + 1 - c4.
If c4=0: L+R=2. From {0,4,6}: 0+4=4≠2, 0+6=6≠2. Impossible.
If c4=1: L+R=12. From {0,4,6}: max = 4+6=10≠12. Impossible.
Dead end.

#### (E=7, N=9):
G = C + 8. C + 7 ≤ 8 → C ≤ 1. Available C from {0, 3, 4, 5, 6}:
- C = 0 → G = 8 = K. Conflict.
Dead end.

### Sub-branch 5a is complete: Only one valid solution from (A=2, K=8): **B=7, L=9, A=2, C=0, K=8, G=5, R=3, E=4, N=6, O=1**

---

### Sub-branch 5b: A = 3, K = 7

Scratchpad: O=1, A=3, K=7. Remaining: {0, 2, 4, 5, 6, 8, 9}

N = 3 + E. E ≤ 6 (since N ≤ 9).

E=0→N=3=A conflict. E=2→N=5✓. E=4→N=7=K conflict. E=5→N=8✓. E=6→N=9✓. E=8→N=11 invalid. E=9→N=12 invalid.

Possible (E,N): (2,5), (5,8), (6,9)

G = C + E + 1. C + E ≤ 8.

#### (E=2, N=5):
G = C + 3. C ≤ 5. Available C from {0,4,6,8,9}:
- C=0→G=3=A conflict. C=4→G=7=K conflict. C=6→G=9✓. But C+E=6+2=8≤8✓.
  Remaining {B,L,R}: {0,4,8}. Column 4: L+R=10c4+3. Column 5: B=R+10-9-c4=R+1-c4.
  c4=0: L+R=3. From {0,4,8}: 0+4=4≠3. Impossible.
  c4=1: L+R=13. From {0,4,8}: 4+8=12≠13, 0+8=8≠13. Impossible. Dead end.
- C=8→G=11 invalid. Dead end.

#### (E=5, N=8):
G = C + 6. C ≤ 2. Available C from {0,2,4,6,9}:
- C=0→G=6✓. Remaining {B,L,R}: {2,4,9}. Column 4: L+R=10c4+3. Column 5: B=R+10-6-c4=R+4-c4.
  c4=0: L+R=3. From {2,4,9}: min=6. Impossible.
  c4=1: L+R=13. From {2,4,9}: 4+9=13✓. {L,R}={4,9}. B=R+4-1=R+3. R=4→B=7=K conflict. R=9→B=12 invalid. Dead end.
- C=2→G=8=N conflict. Dead end.

#### (E=6, N=9):
G = C + 7. C ≤ 1. Available C from {0,2,4,5,8}:
- C=0→G=7=K conflict. Dead end.

**Sub-branch 5b: No solutions.**

---

### Sub-branch 5c: A = 4, K = 6

Scratchpad: O=1, A=4, K=6. Remaining: {0, 2, 3, 5, 7, 8, 9}

N = 4 + E. E ≤ 5.

E=0→N=4=A conflict. E=2→N=6=K conflict. E=3→N=7✓. E=5→N=9✓. E=7→N=11 invalid.

Possible (E,N): (3,7), (5,9)

#### (E=3, N=7):
G = C + 4. C ≤ 4. Available C from {0,2,5,8,9}:
- C=0→G=4=A conflict. C=2→G=6=K conflict. C=5→G=9✓. Remaining {B,L,R}: {0,8,9}. Column 4: L+R=10c4+4. Column 5: B=R+10-9-c4=R+1-c4.
  c4=0: L+R=4. From {0,8,9}: 0+8=8≠4. Impossible.
  c4=1: L+R=14. From {0,8,9}: 8+9=17≠14, 0+9=9≠14. Impossible. Dead end.
- C=8→G=12 invalid. Dead end.

#### (E=5, N=9):
G = C + 6. C ≤ 2. Available C from {0,2,3,7,8}:
- C=0→G=6=K conflict. C=2→G=8✓. Remaining {B,L,R}: {0,3,7}. Column 4: L+R=10c4+4. Column 5: B=R+10-8-c4=R+2-c4.
  c4=0: L+R=4. From {0,3,7}: 0+3=3≠4, 0+7=7≠4. Impossible.
  c4=1: L+R=14. From {0,3,7}: max=3+7=10≠14. Impossible. Dead end.

**Sub-branch 5c: No solutions.**

---

### Sub-branch 5d: A = 6, K = 4

Scratchpad: O=1, A=6, K=4. Remaining: {0, 2, 3, 5, 7, 8, 9}

N = 6 + E. E ≤ 3.

E=0→N=6=A conflict. E=2→N=8✓. E=3→N=9✓. E=5→N=11 invalid.

Possible (E,N): (2,8), (3,9)

#### (E=2, N=8):
G = C + 3. C ≤ 5. Available C from {0,3,5,7,9}:
- C=0→G=3✓. Remaining {B,L,R}: {5,7,9}. Column 4: L+R=10c4+6. Column 5: B=R+10-3-c4=R+7-c4.
  c4=0: L+R=6. From {5,7,9}: min=5+7=12≠6. Impossible.
  c4=1: L+R=16. From {5,7,9}: 7+9=16✓. {L,R}={7,9}. B=R+7-1=R+6. R=7→B=13 invalid. R=9→B=15 invalid. Dead end.
- C=3→G=6=A conflict. C=5→G=8=N conflict. C=7→G=10 invalid. Dead end.

#### (E=3, N=9):
G = C + 4. C ≤ 4. Available C from {0,2,5,7,8}:
- C=0→G=4=K conflict. C=2→G=6=A conflict. C=5→G=9=N conflict. Dead end.

**Sub-branch 5d: No solutions.**

---

### Sub-branch 5e: A = 7, K = 3

Scratchpad: O=1, A=7, K=3. Remaining: {0, 2, 4, 5, 6, 8, 9}

N = 7 + E. E ≤ 2.

E=0→N=7=A conflict. E=2→N=9✓. E=4→N=11 invalid.

Possible (E,N): (2,9)

G = C + 3. C ≤ 5. Available C from {0,4,5,6,8}:
- C=0→G=3=K conflict. C=4→G=7=A conflict. C=5→G=8✓. Remaining {B,L,R}: {0,4,6}. Column 4: L+R=10c4+7. Column 5: B=R+10-8-c4=R+2-c4.
  c4=0: L+R=7. From {0,4,6}: 0+4=4≠7, 0+6=6≠7, 4+6=10≠7. Impossible.
  c4=1: L+R=17. From {0,4,6}: max=4+6=10≠17. Impossible. Dead end.
- C=6→G=9=N conflict. C=8→G=11 invalid. Dead end.

**Sub-branch 5e: No solutions.**

---

### Sub-branch 5f: A = 8, K = 2

Scratchpad: O=1, A=8, K=2. Remaining: {0, 3, 4, 5, 6, 7, 9}

N = 8 + E. E ≤ 1. E≠1 (O=1). E=0→N=8=A conflict. E=3→N=11 invalid.

**No valid (E,N) pairs.** Dead end.

---

## Step 6: Confirm Uniqueness with c2 = 1

For completeness, let's verify c2 = 1 with Branch 2 produces no solutions.

**Branch 2, c2 = 1:**
- A + K = 9 (from Eq. Bridge)
- G = C + E - 9 (from Eq. 2b: G = C + E + 1 - 10)
- N = A + E + 1 (from Eq. 3b)
- E = K + N - 10 (from Eq. 1b)

From N = A + E + 1 and E = K + N - 10: E = K + A + E + 1 - 10 → A + K = 9 ✓

N = A + E + 1 ≤ 9 → E ≤ 8 - A.
G = C + E - 9 ≥ 0 → C ≥ 9 - E.
G ≤ 9 → C + E ≤ 18 (always true).

(A, K) pairs summing to 9, neither is 1: (0,9), (2,7), (3,6), (4,5), (5,4), (6,3), (7,2), (9,0)

#### (A,K) = (0,9):
N = E + 1. E ≤ 8. E ≠ 1 (O), E ≠ 0 (A), E ≠ 9 (K).
G = C + E - 9. C ≥ 9 - E.

E=2→N=3. C≥7. C∈{4,5,6,7,8}: C=7→G=0=A conflict. C=8→G=1=O conflict. Dead end.
E=3→N=4. C≥6. C∈{2,5,6,7,8}: C=6→G=0=A conflict. C=7→G=1=O conflict. Dead end.
E=4→N=5. C≥5. C∈{2,3,6,7,8}: C=6→G=1=O conflict. C=7→G=2✓. Remaining {B,L,R}: {3,6,8}. L+R=10c4+0=10c4. B=R+10-G-c4=R+10-2-c4=R+8-c4.
  c4=0: L+R=0. Min=3+6=9≠0. Impossible.
  c4=1: L+R=10. From {3,6,8}: 3+6=9≠10, 3+8=11≠10, 6+8=14≠10. Dead end.
E=5→N=6. C≥4. C∈{2,3,4,7,8}: C=4→G=0=A conflict. C=7→G=3✓. Remaining {B,L,R}: {2,4,8}. L+R=10c4+0. B=R+10-3-c4=R+7-c4.
  c4=0: L+R=0. Impossible.
  c4=1: L+R=10. From {2,4,8}: 2+8=10✓. {L,R}={2,8}. B=R+7-1=R+6. R=2→B=8=L conflict. R=8→B=14 invalid. Dead end.
  C=8→G=4✓. Remaining {B,L,R}: {2,3,7}. L+R=10c4. B=R+10-4-c4=R+6-c4.
  c4=1: L+R=10. From {2,3,7}: 3+7=10✓. B=R+5. R=3→B=8=A conflict. R=7→B=12 invalid. Dead end.
E=6→N=7. C≥3. C∈{2,3,4,5,8}: C=3→G=0=A conflict. C=4→G=1=O conflict. C=5→G=2✓. Remaining {B,L,R}: {3,4,8}. L+R=10c4+0. B=R+10-2-c4=R+8-c4.
  c4=1: L+R=10. From {3,4,8}: max=4+8=12, 3+8=11≠10, 3+4=7≠10. Dead end.
E=7→N=8. C≥2. C∈{2,3,4,5,6}: C=2→G=0=A conflict. C=3→G=1=O conflict. C=4→G=2✓. Remaining {B,L,R}: {3,5,6}. L+R=10c4. B=R+10-2-c4=R+8-c4.
  c4=1: L+R=10. From {3,5,6}: 5+6=11≠10, 3+6=9≠10, 3+5=8≠10. Dead end.
E=8→N=9=K conflict. Dead end.

#### (A,K) = (2,7):
N = E + 3. G = C + E - 9. E ≤ 6. E ∉ {1,2,7}.
E=0→N=3. C≥9. C∈{4,5,6,8,9}: C=9→G=0=E conflict. C=8→G=7=K conflict. C=6→G=5✓. Remaining {B,L,R}: {4,8,9}. L+R=10c4+2. B=R+10-5-c4=R+5-c4.
  c4=0: L+R=2. Min=4+8=12≠2. Impossible.
  c4=1: L+R=12. From {4,8,9}: 4+8=12✓. B=R+4. R=4→B=8✓(available). Then L=9. Check: B=8, R=4, L=9. All distinct ✓. Let me verify the full addition:
  B=8, L=9, A=2, C=6, K=7, G=5, R=4, E=0, N=3, O=1.
  ```
      8 9 2 6 7
    + 5 4 0 0 3
    ---------
    1 4 2 3 5 0
  ```
  Column 1: 7+3=10. Write 0=E✓, carry 1✓
  Column 2: 6+0+1=7. Write 7=G? No, G=5. **CONFLICT!** 7≠5.
  
  Wait — let me recheck. Column 2: C + E + c1 = 6 + 0 + 1 = 7. This should equal 10·c2 + G = 10·1 + 5 = 15. But 7 ≠ 15. **Dead end.**

  Hmm, I made an error. c2 = 1 means the carry OUT of column 2 is 1, so the equation is C + E + c1 = 10·c2 + G → 6 + 0 + 1 = 10 + 5 → 7 = 15. Contradiction. This is invalid.

  R=8→B=12 invalid. Dead end.
  C=5→G=4✓. Remaining {B,L,R}: {6,8,9}. L+R=10c4+2. B=R+10-4-c4=R+6-c4.
  c4=1: L+R=12. From {6,8,9}: 6+8=14≠12, 6+9=15≠12, 8+9=17≠12. Dead end.
  C=4→G=3=N conflict. Dead end.

E=3→N=6. C≥6. C∈{0,4,5,6,8,9}: C=6→G=0✓. Remaining {B,L,R}: {4,5,8,9}\{6}... wait, remaining after O=1,A=2,K=7,E=3,N=6,C=6,G=0: {4,5,8,9} for {B,L,R}. But that's 4 digits for 3 letters. Let me recount.

Used: O=1,A=2,K=7,E=3,N=6,C=6? Wait, C=6 and N=6 conflict! Dead end.

C=8→G=2=A conflict. C=9→G=3=E conflict. C=5→G=3=E conflict. C=4→G=2=A conflict. Dead end.

E=4→N=7=K conflict. Dead end.
E=5→N=8. C≥4. C∈{0,3,4,6,8,9}: C=4→G=0✓. Remaining {B,L,R}: {3,6,9}. L+R=10c4+2. B=R+10-0-c4... wait, G=0. B=R+10-G-c4=R+10-0-c4=R+10-c4.
  c4=1: B=R+9. R=3→B=12 invalid. Dead end.
  c4=0: L+R=2. Min=3+6=9≠2. Dead end.
C=6→G=2=A conflict. C=8→G=4=E conflict. C=9→G=5✓. Remaining {B,L,R}: {0,3,6}. L+R=10c4+2. B=R+10-5-c4=R+5-c4.
  c4=0: L+R=2. Min=0+3=3≠2. Dead end.
  c4=1: L+R=12. From {0,3,6}: max=3+6=9≠12. Dead end.
C=3→G=1=O conflict. C=0→G=3=N conflict (wait, N=8, G=3 is fine). Remaining {B,L,R}: {6,8,9}. But N=8, so 8 is taken. Wait, N=8 and K=7, A=2, E=5. Remaining: {0,3,6,9}. G=C+E-9=0+5-9=-4. Invalid.

Actually C=0: G = 0+5-9 = -4. Invalid. Dead end.

E=6→N=9. C≥3. C∈{0,3,4,5,8}: C=3→G=0✓. Remaining {B,L,R}: {4,5,8}. L+R=10c4+2. B=R+10-0-c4=R+10-c4.
  c4=0: L+R=2. Min=4+5=9≠2. Dead end.
  c4=1: L+R=12. From {4,5,8}: 4+8=12✓. B=R+9. R=4→B=13 invalid. R=8→B=17 invalid. Dead end.
C=4→G=1=O conflict. C=5→G=2=A conflict. C=8→G=4✓. Remaining {B,L,R}: {0,3,6}. L+R=10c4+2. B=R+10-4-c4=R+6-c4.
  c4=0: L+R=2. Min=0+3=3≠2. Dead end.
  c4=1: L+R=12. From {0,3,6}: max=3+6=9≠12. Dead end.

#### (A,K) = (3,6):
N = E + 4. G = C + E - 9. E ≤ 5. E ∉ {1,3,6}.
E=0→N=4. C≥9. C∈{2,5,7,8,9}: C=9→G=0=E conflict. C=8→G=7✓. Remaining {B,L,R}: {2,5,9}. L+R=10c4+3. B=R+10-7-c4=R+3-c4.
  c4=0: L+R=3. From {2,5,9}: 2+5=7≠3. Impossible.
  c4=1: L+R=13. From {2,5,9}: 5+9=14≠13, 2+9=11≠13. Dead end.
  C=7→G=5✓. Remaining {B,L,R}: {2,5,9}. But G=5 and R is in remaining. Wait, used: O=1,A=3,K=6,E=0,N=4,C=7,G=5. Remaining: {2,8,9}. L+R=10c4+3. B=R+10-5-c4=R+5-c4.
  c4=0: L+R=3. Min=2+8=10≠3. Dead end.
  c4=1: L+R=13. From {2,8,9}: 8+9=17≠13, 2+9=11≠13, 2+8=10≠13. Dead end.
E=2→N=6=K conflict. Dead end.
E=4→N=8. C≥5. C∈{0,2,5,7,9}: C=5→G=0✓. Remaining {B,L,R}: {2,7,9}. L+R=10c4+3. B=R+10-0-c4=R+10-c4.
  c4=0: L+R=3. Min=2+7=9≠3. Dead end.
  c4=1: L+R=13. From {2,7,9}: 7+9=16≠13, 2+9=11≠13. Dead end.
  C=7→G=2✓. Remaining {B,L,R}: {0,5,9}. L+R=10c4+3. B=R+10-2-c4=R+8-c4.
  c4=1: L+R=13. From {0,5,9}: 5+9=14≠13, 0+9=9≠13. Dead end.
  C=9→G=4... wait, E=4, G=4 conflict. Dead end.
E=5→N=9. C≥4. C∈{0,2,4,7,8}: C=4→G=0✓. Remaining {B,L,R}: {2,7,8}. L+R=10c4+3. B=R+10-0-c4=R+10-c4.
  c4=1: L+R=13. From {2,7,8}: 7+8=15≠13, 2+8=10≠13. Dead end.
  C=7→G=3=A conflict. C=8→G=4✓. Remaining {B,L,R}: {0,2,7}. L+R=10c4+3. B=R+10-4-c4=R+6-c4.
  c4=1: L+R=13. From {0,2,7}: max=2+7=9≠13. Dead end.
  C=2→G=3... wait, G=C+E-9=2+5-9=-2. Invalid. Dead end.
  C=0→G=5+0-9=-4. Invalid. Dead end.

#### (A,K) = (4,5):
N = E + 5. G = C + E - 9. E ≤ 4. E ∉ {1,4,5}.
E=0→N=5=K conflict. E=2→N=7. C≥7. C∈{0,3,6,7,8,9}: C=7→G=0=E conflict. C=8→G=1=O conflict. C=9→G=2=E... wait E=2, G=2 conflict. C=6→G=5=K conflict. C=3→G=3+2-9=-4 invalid. C=0→G=-9 invalid. Dead end.
E=3→N=8. C≥6. C∈{0,2,6,7,9}: C=6→G=0✓. Remaining {B,L,R}: {2,7,9}. L+R=10c4+4. B=R+10-0-c4=R+10-c4.
  c4=1: L+R=14. From {2,7,9}: 7+9=16≠14, 2+9=11≠14. Dead end.
  c4=0: L+R=4. Min=2+7=9≠4. Dead end.
  C=7→G=1=O conflict. C=9→G=3=E conflict. C=2→G=-4 invalid. C=0→G=-9 invalid. Dead end.

#### (A,K) = (5,4):
N = E + 6. G = C + E - 9. E ≤ 3. E ∉ {1,4,5}.
E=0→N=6. C≥9. C∈{2,3,7,8,9}: C=9→G=0=E conflict. C=8→G=7✓. Remaining {B,L,R}: {2,3,9}. L+R=10c4+5. B=R+10-7-c4=R+3-c4.
  c4=0: L+R=5. From {2,3,9}: 2+3=5✓. {L,R}={2,3}. B=R+3. R=2→B=5=A conflict. R=3→B=6... but N=6. Conflict. Dead end.
  c4=1: L+R=15. From {2,3,9}: max=3+9=12≠15. Dead end.
  C=7→G=7... wait G=C+E-9=7+0-9=-2. Invalid. Hmm, I need C≥9. So only C=9 works, which we already checked. Dead end.
E=2→N=8. C≥7. C∈{0,3,6,7,9}: C=7→G=0✓. Remaining {B,L,R}: {0,3,6,9}\{0}... used: O=1,A=5,K=4,E=2,N=8,C=7,G=0. Remaining: {3,6,9}. L+R=10c4+5. B=R+10-0-c4=R+10-c4.
  c4=0: L+R=5. From {3,6,9}: 3+6=9≠5. Dead end.
  c4=1: L+R=15. From {3,6,9}: 6+9=15✓. {L,R}={6,9}. B=R+9. R=6→B=15 invalid. R=9→B=18 invalid. Dead end.
  C=9→G=2=E conflict. C=6→G=-3 invalid. C=3→G=-6 invalid. C=0→G=-9 invalid. Dead end.
E=3→N=9. C≥6. C∈{0,2,6,7,8}: C=6→G=0✓. Remaining {B,L,R}: {2,7,8}. L+R=10c4+5. B=R+10-0-c4=R+10-c4.
  c4=1: L+R=15. From {2,7,8}: 7+8=15✓. {L,R}={7,8}. B=R+9. R=7→B=16 invalid. R=8→B=17 invalid. Dead end.
  C=7→G=1=O conflict. C=8→G=2✓. Remaining {B,L,R}: {0,6,7}. L+R=10c4+5. B=R+10-2-c4=R+8-c4.
  c4=1: L+R=15. From {0,6,7}: max=6+7=13≠15. Dead end.
  C=2→G=-4 invalid. C=0→G=-9 invalid. Dead end.

#### (A,K) = (6,3):
N = E + 7. E ≤ 2. E ∉ {1,3,6}.
E=0→N=7. C≥9. C∈{2,4,5,8,9}: C=9→G=0=E conflict. C=8→G=7=N conflict. C=5→G=-4 invalid. Dead end.
E=2→N=9. C≥7. C∈{0,4,5,7,8}: C=7→G=0✓. Remaining {B,L,R}: {4,5,8}. L+R=10c4+6. B=R+10-0-c4=R+10-c4.
  c4=0: L+R=6. From {4,5,8}: 4+5=9≠6. Dead end.
  c4=1: L+R=16. From {4,5,8}: max=5+8=13≠16. Dead end.
  C=8→G=1=O conflict. C=5→G=-2 invalid. C=4→G=-3 invalid. C=0→G=-7 invalid. Dead end.

#### (A,K) = (7,2):
N = E + 8. E ≤ 1. E ∉ {1}. So E=0→N=8.
G = C - 9. C ≥ 9. C∈{3,4,5,6,9}: C=9→G=0=E conflict. C=6→G=-3 invalid. All others give G<0. Dead end.

#### (A,K) = (9,0):
N = E + 10. But N ≤ 9 and E ≥ 0, so N ≥ 10. Impossible. Dead end.

**All c2 = 1 sub-branches exhausted. No solutions found.**

---

## Final Answer

The unique solution to BLACK + GREEN = ORANGE is:

| Letter | Digit |
|--------|-------|
| **B**  | **7** |
| **L**  | **9** |
| **A**  | **2** |
| **C**  | **0** |
| **K**  | **8** |
| **G**  | **5** |
| **R**  | **3** |
| **E**  | **4** |
| **N**  | **6** |
| **O**  | **1** |

```
    7 9 2 0 8
  + 5 3 4 4 6
  ---------
  1 3 2 6 5 4
```

**Verification:** 79208 + 53446 = 132654 ✓

---

## Summary of Tricks Used

1. **Overflow Anchor:** Two 5-digit numbers sum to a 6-digit number → leading digit O = 1.
2. **Bridge Trick:** Add two columns that share letters on opposite sides → shared variables cancel, yielding A + K + c2 = 10(c1 + c3).
3. **Carry Branching:** Carries are binary (0 or 1), not 10-way. Branch on carries, not digits.
4. **Digit Exhaustion:** With c2 = 0, systematically test (A,K) pairs, then (E,N), then C, then (L,R), then B — each step eliminates branches via carry constraints and digit uniqueness.
5. **Uniqueness Proof:** All 6 (A,K) pairs with c2 = 0 were tested; 5 yielded no solutions. All (A,K) pairs with c2 = 1 were also tested; none yielded solutions. The solution is unique.
