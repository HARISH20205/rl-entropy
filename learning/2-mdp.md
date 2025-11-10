# RL Notes: MDPs (David Silver, Lecture 2)

## 2. MDPs

- it describes RL problems where you know everything about the current state. no hidden stuff ig.
- if you can see the whole environment, it's an MDP. if not, you gotta deal with POMDPs (pain).
- most RL stuff can be squished into an MDP format, even if it's a bit hacky sometimes.

## Markov Property (aka, memoryless)

- the future only cares about now, not what happened before.
- if you know the current state, you can ignore the whole history. that's the "Markov" thing.
- formula: $$ P(S*{t+1} | S_t) = P(S*{t+1} | S_1, ..., S_t) $$
- basically, "what's next?" only depends on "where am I now?".

## Markov Process vs Markov Reward Process

- Markov Process: just states and transitions, no rewards.
- Markov Reward Process: same thing, but you get points (rewards) for landing in certain states.
- there's a discount factor ($\gamma$) so you care more about stuff happening now than later. if $\gamma$ is close to 0, you're short-sighted. if it's close to 1, you're planning for the future.

## Return and Value

- return = sum of all rewards you get, but future rewards get discounted.
- value function: "how good is this state?" if I start here, what's my total expected reward?
- you use the value function to figure out if you're doing well or just wasting time.

## Bellman Equation (the backbone)

- value of a state = immediate reward + discounted value of next state.
- formula: $$ v(s) = E[R_{t+1} + \gamma v(S_{t+1}) | S_t = s] $$
- you can solve this with matrices if the state space isn't huge. otherwise, gotta use iterative methods (DP, Monte Carlo, TD, etc.).

## MDPs: now with actions

- MDP = Markov Reward Process + you get to pick actions.
- tuple: $$ (S, A, P, R, \gamma) $$
- you choose actions to maximize your total reward. that's the whole game.

## Policies

- policy = your strategy. what do you do in each state?
- can be deterministic (always pick the same action) or stochastic (roll the dice).
- policies only care about the current state, not the history. so, no overthinking.

## State-Value and Action-Value Functions

- state-value: expected return if you start at state $s$ and follow your policy.
- action-value: expected return if you start at state $s$, take action $a$, then follow your policy.
- these help you figure out which states and actions are actually worth it.

## Bellman Expectation Equations

- for state-value: average over all possible actions and next states, weighted by your policy and transition probabilities.
- for action-value: same idea, but you start with a specific action.

## Optimality (finding the best moves)

- optimal value function = best possible expected return from any state.
- Bellman optimality equation: recursively defines the best value you can get.
- if you know the optimal value/action-value, you know the best policy. that's the dream.

## Solving MDPs

- small MDPs: just solve the equations directly.
- big MDPs: use value iteration, policy iteration, or RL algorithms like Q-learning, SARSA.
- most real problems are too big for brute force, as expected.

## POMDPs (when life sucks)

- if you can't see the whole state, you're in POMDP territory.
- you have to keep a "belief" (probability distribution) over possible states, based on what you've seen so far.
- way harder, but sometimes you can hack it by pretending it's an MDP.
