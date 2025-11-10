# RL Notes: Planning by DP (David Silver, Lecture 3)

# 3. Planning by Dynamic Programming

So, what's Dynamic Programming (DP)? It's a way to solve complex problems by breaking them down into smaller, simpler subproblems. It's super useful for planning in an MDP, but there's a catch: you need to know the entire model of the MDP (all the states, actions, transition probabilities, and rewards).

DP works for problems that have two key properties, and lucky for us, MDPs have both:

1.  **Optimal substructure**: The optimal solution for a problem contains optimal solutions for its subproblems. The Bellman equation is our recursive way of expressing this.
2.  **Overlapping subproblems**: The same subproblems are encountered over and over. We can solve them once and store the answer (in the value function) to reuse later.

With DP, we can do two main things:

- **Prediction (Policy Evaluation)**: Given an MDP and a policy π, figure out the value function `v_π`.
- **Control**: Given an MDP, find the optimal value function `v_*` and the optimal policy `π_*`.

## Policy Evaluation (Prediction)

The goal here is to figure out "how good is this policy π?". We want to find its value function, `v_π`.

The method we use is called **Iterative Policy Evaluation**.

- We start with a random value function `v_0`.
- Then we repeatedly update it using the Bellman expectation equation until it converges.
- The update looks like this: `v_{k+1}(s) = Σ_a π(a|s) (R_s^a + γ Σ_{s'} P_{ss'}^a v_k(s'))`
- Basically, for each state, the new value is the expected value of what you'd get by following the policy for one step.
- This is a "synchronous backup" because we calculate all the new values for `v_{k+1}` using the old values from `v_k`.

## Policy Iteration (Control)

Now for the main event: finding the best policy, `π_*`. Policy Iteration is an algorithm that does this by repeating two steps until the policy stops changing.

1.  **Policy Evaluation**: Take the current policy `π` and evaluate it. We run iterative policy evaluation to find `v_π`. We don't always have to wait for it to fully converge; a few iterations can be enough.
2.  **Policy Improvement**: Now that we have `v_π`, we can improve our policy. For each state, we look one step ahead and choose the action that leads to the best value, according to `v_π`. This is just being greedy with respect to the value function.
    - `π'(s) = argmax_a q_π(s, a) = argmax_a (R_s^a + γ Σ_{s'} P_{ss'}^a v_π(s'))`

We just keep bouncing between these two steps. We evaluate, we improve, we evaluate the improved policy, we improve again, and so on. This process is guaranteed to converge to the optimal policy `π_*`.

## Value Iteration (Control)

Value Iteration is another way to find `π_*`, and it's often more straightforward. It combines the evaluation and improvement steps into a single update.

The idea is to directly use the Bellman optimality equation to update our value function.

- `v_{k+1}(s) = max_a (R_s^a + γ Σ_{s'} P_{ss'}^a v_k(s'))`

Notice the `max` over actions. Instead of averaging over the policy's actions (like in policy evaluation), we just take the best possible action at each state.

We just repeat this update for all states until the value function converges to `v_*`. Once we have `v_*`, we can easily find the optimal policy `π_*` by acting greedily with respect to `v_*`.

Value Iteration is basically a simplified version of policy iteration where we only do one iteration of evaluation before improving.

## Extensions to DP

The DP methods we've seen are "synchronous" and use "full-width backups". This can be computationally heavy.

**Asynchronous DP** offers a more flexible approach:

- Instead of updating all states at once, we can update them one by one, in any order.
- This can save a lot of computation.
- Some cool ideas here are:
  - **In-place DP**: Update values using a single value function array, which can speed up convergence.
  - **Prioritised Sweeping**: Focus updates on states where the value function is changing the most (i.e., states with a large Bellman error).
  - **Real-time DP**: Only update the states the agent actually visits.

## Contraction Mapping

This is the math that proves DP methods actually work. The key idea is that the Bellman backup operators (`T^π` for evaluation and `T^*` for value iteration) are **γ-contractions**.

A contraction mapping is an operator that, when applied, always brings any two points in a space closer together.

Because the Bellman operators are contractions, repeatedly applying them is guaranteed to converge to a single, unique fixed point.

- For iterative policy evaluation, this fixed point is `v_π`.
- For value iteration, this fixed point is `v_*`.

This guarantees that our algorithms will converge to the correct solution.
