# Review: "Markov Decision Processes and Multi-Armed Bandits: Overview and Worked Examples"

**Reviewed by:** Claude Opus 4.6 (automated multi-agent review)
**Date:** February 26, 2026
**Purpose:** Pre-publication review for ResearchGate

---

## Overall Assessment

The paper is a well-structured, competent pedagogical survey. The writing is clear, the progression of models is logical, and the worked examples with Python code are a genuine strength. The "pedagogical in intent; presents no new results" framing is appropriate for ResearchGate. An expert would find this a useful teaching reference — **provided the issues below are addressed.** Most of the math is correct; the main risks are one numerical error in a worked example, several conceptual claims that need qualification, and some missing citations that experts would expect.

---

## TIER 1: FIX BEFORE PUBLISHING (would embarrass the author)

### 1. LinUCB Numerical Trace Is Incorrect (lines 1159–1170)

**Severity: HIGH — this is a verifiable computational error.**

The UCB values in the LinUCB worked example do not follow from the stated feature vectors and parameters (alpha=1, lambda=1). At round 1 with V=I, theta_hat=[0,0], Sports user:

| Arm | Feature phi | Paper's UCB | Correct UCB |
|-----|------------|-------------|-------------|
| Tech | [0, 0.5] | 1.41 | 0.50 |
| Sports | [1, 0] | 1.41 | 1.00 |
| General | [0.5, 0.5] | 1.58 | 0.71 |

The paper's values (1.41 ≈ sqrt(2), 1.58 ≈ sqrt(2.5)) suggest feature vectors with norms sqrt(2) and sqrt(2.5) were used during trace construction, but these don't match the stated features. The entire 6-round trace is inconsistent with the stated setup. The Python code provided (lines 1176–1205) also produces different results from the trace (it generates a different user sequence with seed 42).

**Fix:** Recompute the entire LinUCB trace from scratch using the stated features, or change the features to match the trace. Ensure the Python code reproduces the trace.

### 2. "Single-State MDP" Characterization Contradicts Gittins Treatment (line 708)

**Severity: HIGH — an expert would immediately notice.**

The paper says "The MAB is a degenerate MDP with a single state" but then discusses the Gittins index, which only makes sense because the Bayesian state (posterior) evolves. Two sentences later the paper acknowledges the POMDP view where the hidden state is the arm means — directly contradicting "single state."

**Fix:** Qualify: "In the frequentist formulation, the MAB can be viewed as a degenerate MDP with a single observable state. In the Bayesian formulation, however, the agent's posterior beliefs evolve, making it an MDP over belief states — and it is this richer formulation that gives rise to the Gittins index."

### 3. Gittins Index Optimality Conditions Never Stated (line 820)

**Severity: HIGH — missing conditions that experts know well.**

The paper says the Gittins index gives "the optimal policy" and "the Bayes-optimal policy" without stating the required conditions:
- Arms must be **independent**
- Arms must be **rested** (frozen when not played)
- Objective must be **infinite-horizon discounted** (not finite-horizon or average-reward)
- Prior must be **known** (Bayesian setup)

**Fix:** Add: "This optimality result holds specifically for the discounted infinite-horizon Bayesian bandit with independent, rested arms and a known prior."

### 4. PSPACE-Completeness Citation for POMDPs Needs Correction (line 587)

**Severity: HIGH — incorrect citation for a precise complexity claim.**

The text says "exact finite-horizon POMDP planning is PSPACE-complete [Papadimitriou and Tsitsiklis, 1987]." The 1987 paper primarily proves MDPs are P-complete; its POMDP results establish PSPACE-**hardness**, not PSPACE-**completeness** (the upper bound requires additional argument).

**Fix:** Either change "PSPACE-complete" to "PSPACE-hard" (which the cited paper does prove for the unobservable case), or add a reference that establishes the matching upper bound (e.g., Mundhenk et al. 2000).

---

## TIER 2: SHOULD FIX (notable omissions experts would notice)

### 5. No Q-Function (Action-Value Function) Anywhere

The paper defines V*(s) but never introduces Q*(s,a), arguably the single most important concept in modern RL (Q-learning is named after it). For a "self-contained reference" that covers model-free RL in Section 7, this is a conspicuous gap.

**Fix:** Add Q*(s,a) = r(s,a) + gamma * sum_j p_{ij}(a) V*(s_j) alongside the Bellman equation in Section 3.

### 6. Policy Iteration and LP Formulation Mentioned But Never Defined

Line 492 mentions "value iteration, policy iteration, or linear programming" as MDP solution methods, but only value iteration is ever explained. Policy iteration (Howard, 1960) is at least as important historically.

**Fix:** Add a brief paragraph on policy iteration (policy evaluation + policy improvement). At minimum, cite Howard (1960).

### 7. Bayesian vs. Frequentist Regret Distinction Never Made

The paper defines frequentist cumulative regret, discusses UCB1 and Thompson Sampling under that definition, then discusses the Gittins index in a Bayesian framework — but never distinguishes between Bayesian regret E_theta[R_T] and frequentist regret sup_theta E[R_T]. This is a fundamental distinction in bandit theory.

**Fix:** Add a remark after the regret definition clarifying the two notions and which results pertain to which.

### 8. Approximation Hardness Claim for Restless Bandits May Overclaim (line 1283)

"Even computing a nontrivially good approximate policy is PSPACE-hard in general" is a strong claim that needs a precise citation. The Papadimitriou & Tsitsiklis (1999) paper proves hardness of the exact optimal, but approximation hardness requires careful verification.

**Fix:** Soften to "computing the optimal policy is PSPACE-hard" or provide a specific citation for approximation hardness.

### 9. Policy Defined Only as Deterministic (line 224)

The definition pi: S -> A covers only deterministic stationary policies. But the CMDP section (line 1502) says "the optimal policy is generally a mixture of deterministic policies," which requires stochastic policies — never defined.

**Fix:** Add a sentence noting that policies can be stochastic (pi(a|s)) but for finite-state discounted MDPs, deterministic stationary policies suffice.

---

## TIER 3: NOTATION AND WRITING ISSUES

### 10. Symbol Collisions Not Flagged

The paper flags alpha/beta and b collisions, but misses several others:

| Symbol | Usage 1 | Usage 2 | Usage 3 |
|--------|---------|---------|---------|
| alpha | MC transition prob (Sec 2) | Beta params (Sec 4) | **LinUCB exploration param (Sec 5) — NOT flagged** |
| b_t | Bellman immediate return (Sec 3) | Belief state (Sec 4) | **LinUCB cumulative features (Sec 5) — NOT flagged, NOT in notation table** |
| mu_t | State distribution (Sec 2) | — | **Posterior mean of theta (Sec 5) — NOT flagged** |
| lambda | — | Regularization (Sec 5) | **Lagrange multiplier (Sec 6) — NOT flagged** |
| eta | — | Sub-Gaussian noise (Sec 5) | **EXP4 learning rate (Sec 5) — same section!** |
| s | State variable (everywhere) | — | **Time index in TS sum (Sec 5, line 1115)** |

### 11. Section 5 Notation Table Incomplete

Missing ~8 symbols used in equations: b_t, mu_t, Sigma_t, sigma^2, sigma_theta^2, eta_t, eta (EXP4), p_t(a), r_hat_t(a), theta_tilde.

### 12. Glossary Missing ~15 Terms

Missing despite pedagogical intent: ridge regression, Mahalanobis norm, conjugate prior, sub-Gaussian, importance weighting, design matrix, belief simplex, alpha-vector, principle of optimality, feature vector, consistent policy, Dec-POMDP, Nash equilibrium, options framework, Bayes' rule.

### 13. Minor Writing Issues

- Line 80: "but need not in general" — grammatically incomplete (should be "but need not be in general")
- Line 101: "transitions to a next state" → "transitions to the next state"
- Line 1078: "Where UCB1 assigns a fixed unknown parameter to each arm" — imprecise; UCB1 estimates the mean
- "M1, M2, M3" in restless bandit trace table (line 1367) used without being defined

### 14. Equation Numbering

Many equations are numbered but never cross-referenced. Only eq. (8) (belief update) is ever referenced by number. Consider making unreferenced equations unnumbered, or add more cross-references.

---

## TIER 4: BIBLIOGRAPHY ISSUES

### 15. Bellman Citation Is RAND Paper, Not Standard Reference (line 1662)

The bibliography cites RAND Paper P-1066, but the standard citation is the *Journal of Mathematics and Mechanics*, vol. 6, pp. 679–684, 1957 (or the 1957 book *Dynamic Programming*, Princeton UP). Readers may have difficulty locating the RAND paper.

**Fix:** Cite the journal version or both.

### 16. Gittins Page Range Includes Discussion (line 1704)

The citation gives pp. 148–177, but the article proper is pp. 148–164; pages 165–177 are the published discussion. Standard practice is pp. 148–164 or note "[with discussion]."

### 17. Missing Key References

An expert would expect these in a survey of this scope:

| Priority | Reference | Why |
|----------|-----------|-----|
| **High** | Howard (1960), *Dynamic Programming and Markov Processes* | Foundational MDP text, introduced policy iteration |
| **High** | Lattimore & Szepesvari (2020), *Bandit Algorithms* | Standard modern bandit reference |
| **Medium** | Bertsekas, *Dynamic Programming and Optimal Control* | Major DP/MDP textbook |
| **Medium** | Gittins, Glazebrook & Weber (2011), 2nd ed. book | Comprehensive Gittins index treatment |
| **Low** | Watkins & Dayan (1992) for Q-learning | If Q-learning is mentioned in Sec 7 |
| **Low** | Williams (1992) for REINFORCE | If REINFORCE is mentioned in Sec 7 |
| **Low** | Bubeck & Cesa-Bianchi (2012) survey | Excellent bandit survey |

### 18. Langford & Zhang Date

The paper cites 2008 (proceedings publication date), but the conference was NeurIPS 2007. The standard ML convention uses the conference year (2007).

### 19. PSPACE Citation Mismatch

Line 587 cites Papadimitriou & Tsitsiklis (1987) for POMDP PSPACE-completeness, but this paper's main results are about MDPs (P-completeness). See item 4 above.

### 20. Missing Citations for Claims

- Line 796: Decaying epsilon-greedy O(T^{2/3}) regret stated without citation
- Line 492: Policy iteration mentioned without citing Howard (1960)
- Section 7.5: Q-learning, SARSA, REINFORCE, PPO all mentioned without individual citations

---

## WHAT'S CORRECT (verified numerically)

These were all independently verified and confirmed correct:

- Two-state MDP: V*(s1) ≈ 64.59, V*(s2) ≈ 72.70 ✓
- Asset maintenance MDP: V*(Op) ≈ -3.21, V*(Fail) ≈ -17.89 ✓
- Tiger Problem belief updates: b'(TL) = 0.85, b''(TL) ≈ 0.97 ✓
- Epsilon-greedy trace: all 10 rounds verified ✓
- UCB1 trace: all index computations verified ✓
- Thompson Sampling trace: all posterior updates verified ✓
- Whittle indices: W(G) = 9/155 ≈ 0.058065, W(B) ≈ 1.216 ✓
- Restless bandit trace: all rewards verified ✓
- All regret bounds (Lai-Robbins, UCB1, TS, LinUCB, EXP3, EXP4) ✓
- All formal definitions (MDP, POMDP, MAB, contextual, restless) ✓
- Bellman equation, POMDP belief update, Whittle relaxation ✓
- All Python code (except LinUCB trace mismatch) ✓

---

## RECOMMENDED PRIORITY ORDER

1. **Fix the LinUCB trace** (item 1) — this is a verifiable error
2. **Qualify the single-state MAB claim** (item 2) — conceptual inconsistency
3. **State Gittins index conditions** (item 3) — missing important conditions
4. **Fix PSPACE citation** (item 4) — wrong/imprecise citation
5. **Add Q-function** (item 5) — major omission for a reference paper
6. **Add Howard (1960) and Lattimore & Szepesvari (2020) to bibliography** (item 17)
7. **Fix notation collisions** (item 10) — especially alpha, b_t, mu_t
8. **Complete Section 5 notation table** (item 11)
9. **Add Bayesian vs. frequentist regret distinction** (item 7)
10. Everything else (items 6, 8, 9, 12–20)
