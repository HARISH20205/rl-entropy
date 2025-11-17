# Model Free Control

currently, we know about evaluating a fixed policy (prediction). now let's find the best policy (control).

## On Policy vs Off Policy

- **on-policy**: learn about the policy you're actually following.
  sample and evaluate at the same time.
- **off-policy**: learn about a different policy than the one you're following. like learning the best policy while exploring randomly.

## Model Free Policy Improvement

- greedy policy improvement on state value needs model of the environment. how to do without model?

- but we can use action-value functions instead of state-value functions. since action-values give value of taking specific actions in states, we can directly improve policy without knowing transitions.

- when used with Greedy policy improvement, we run into the problem of insufficient exploration. if we always pick the best-known action, we might miss out on better actions.

## ε-greedy policy

- ensures continuous exploration by choosing a random action with probability ε, and the best-known action with probability 1-ε.
- like with probability ε, pick random action; with probability 1-ε, pick greedy action.
- thiis way, we keep exploring while still exploiting the most promising actions.
- either take the best action or explore randomly.

  $$
  \pi(a\mid s)=
  \begin{cases}
  1-\epsilon+\dfrac{\epsilon}{\lvert A(s)\rvert}, & \text{if } a=\arg\max_{a} Q(s,a),\\[6pt]
  \dfrac{\epsilon}{\lvert A(s)\rvert}, & \text{otherwise.}
  \end{cases}
  $$

- also ε-policy improvement theorem: if $π'$ is ε-greedy wrt $Qπ$, then
  $$Vπ' ≥ Vπ.$$

## Greedy in the Limit of Infinite Exploration (GLIE)

- all state-action pairs are visited infinitely often.

$$ \lim\_{k\to\infty} N_k(s,a) = \infty. $$

- policy converges to greedy policy.

$$ \lim*{k\to\infty} \pi_k(a\mid s) = 1{\{a=\arg\max*{a} Q(s,a)\}}. $$

- $ \epsilon$ -greedy policies with $ \epsilon_k = \frac{1}{k} $ satisfy GLIE conditions.
