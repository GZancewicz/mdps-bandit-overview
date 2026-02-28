# MDPs and Multi-Armed Bandits: An Overview

A self-contained survey of Markov-based sequential decision-making, spanning from Markov's original study of dependent random variables (1906) through Bellman's dynamic programming to modern bandit algorithms deployed in clinical trials, online advertising, and autonomous systems.

The paper covers six core model classes arranged in a progression of increasing generality:

1. **Markov Processes** -- A system evolves randomly among a finite set of states according to fixed transition probabilities, with no external control. The transition matrix and stationary distribution are the central objects.

2. **Markov Decision Processes** -- An agent chooses actions that influence both state transitions and rewards. The Bellman equation characterizes the optimal policy, and solution methods (value iteration, policy iteration, linear programming) provide constructive algorithms.

3. **Partially Observable MDPs** -- The agent can no longer observe the state directly and must maintain a belief distribution updated via Bayes' rule. The POMDP reduces to a continuous-state MDP over the belief simplex, at the cost of greatly increased computational complexity.

4. **Multi-Armed Bandits** -- The exploration-exploitation tradeoff in its purest form: a single-state problem where the agent must learn which arm is best by trying them. Covers epsilon-greedy, UCB1, Thompson Sampling, the Gittins index, successive elimination, KL-UCB, and EXP3, together with the Lai-Robbins regret lower bound.

5. **Contextual Bandits** -- Side information (a context vector) is observed before each arm selection, and reward distributions depend on the context. Covers LinUCB, Thompson Sampling for linear models, and EXP4 for adversarial settings.

6. **Restless Bandits** -- The classical bandit assumption that unplayed arms are frozen is removed: every arm evolves at every time step. The Whittle index policy provides a tractable heuristic for this PSPACE-hard problem.

An additional section surveys related model classes -- hidden Markov models, Markov games, constrained MDPs, semi-Markov decision processes, and model-free reinforcement learning -- that lie beyond the scope of this paper but occupy adjacent regions of the same theoretical landscape.

Each section follows a uniform structure: a notation table, formal definitions, key theoretical results, worked examples with explicit numerical computation, and a summary of key equations and applications. The paper is pedagogical in intent; it presents no new results but aims to collect, unify, and illustrate the core ideas in a single reference. Familiarity with linear algebra and basic probability is assumed; no prior exposure to dynamic programming or bandit theory is required.
