# RL Notes: Model-Free Prediction (David Silver, Lecture 4)

# 4. Model-Free Prediction

alright, so now we're ditching the model. no more knowing everything about the MDP. gotta figure stuff out purely from experience.

## What's Model-Free Even Mean?

- you don't know the transition probabilities or rewards beforehand.
- just interact with the environment, see what happens.
- last lecture was planning (you had the map). now you're exploring blind.
- two big problems: **prediction** (how good is my policy?) and **control** (find the best policy).
- this lecture = prediction. next one = control.

## Monte Carlo (MC) Methods

just run episodes and average the results. that's it.

### How It Works

- need **complete episodes**. can't do this with non-terminating environments.
- no bootstrapping = don't use estimates to update estimates.
- value = average return you actually got from real experience.
- formula: $$ v\_\pi(s) \approx \text{average of all returns starting from } s $$

### First-Visit vs Every-Visit

- **First-visit**: only count the first time you see a state in an episode.
- **Every-visit**: count every single time you visit that state.
- both converge to the true value as you get infinite data (law of big numbers ftw).

### Incremental Updates

instead of storing all returns and recalculating the mean every time, just update incrementally:

$$ V(s) \leftarrow V(s) + \alpha (G_t - V(s)) $$

- $G_t$ = actual return you got.
- $\alpha$ = learning rate (step size).
- for non-stationary problems, use a fixed $\alpha$ instead of $1/N(s)$ so you forget old stuff.

### MC Pros and Cons

**pros:**

- simple. just average returns.
- unbiased (you're using real data, no guesses).
- works even in non-Markov environments.

**cons:**

- high variance (returns can be all over the place).
- need complete episodes (pain if episodes are long).
- can't learn online step-by-step.

## Temporal Difference (TD) Learning

this is where things get cool. learn from incomplete episodes by bootstrapping.

### TD(0) - The Simplest TD

update your value estimate using the immediate reward + your estimate of the next state's value:

$$ V(S*t) \leftarrow V(S_t) + \alpha (R*{t+1} + \gamma V(S\_{t+1}) - V(S_t)) $$

- $R_{t+1} + \gamma V(S_{t+1})$ = **TD target** (your guess of what you should've gotten).
- $\delta_t = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$ = **TD error** (how wrong you were).

### Why TD is Better (Usually)

- can learn **online**, after every single step.
- works with **incomplete episodes**.
- works in **continuing environments** (non-terminating).
- **lower variance** than MC (only one random step instead of a whole episode).
- but... it's **biased** (you're using an estimate to update an estimate).

### MC vs TD Trade-offs

- **MC**: high variance, zero bias. waits till the end.
- **TD**: low variance, some bias. learns immediately.
- TD usually converges faster in practice.
- MC is better if your environment isn't really Markov.

### Batch Learning

if you have a fixed set of episodes and keep learning from them:

- **MC** converges to the solution that best fits the observed returns (minimize squared error).
- **TD** converges to the solution of the **maximum likelihood Markov model**.
- basically, TD assumes the environment is Markov and exploits that. MC doesn't assume anything.

## n-Step TD

why pick between MC (look all the way to the end) and TD (look one step ahead)? look n steps ahead.

### n-Step Returns

$$ G*t^{(n)} = R*{t+1} + \gamma R*{t+2} + ... + \gamma^{n-1} R*{t+n} + \gamma^n V(S\_{t+n}) $$

- n=1 → TD(0)
- n=∞ → Monte Carlo
- anything in between = compromise between bias and variance.

### Picking n

- small n = low variance, more bias, faster learning.
- large n = high variance, less bias, slower learning.
- depends on the problem. usually something like n=3 to n=10 works well.

## TD(λ) - The Big Leagues

instead of picking one n, why not use all of them? that's TD(λ).

### λ-Return

combine all n-step returns with exponentially decaying weights:

$$ G*t^\lambda = (1 - \lambda) \sum*{n=1}^{\infty} \lambda^{n-1} G_t^{(n)} $$

- λ=0 → TD(0)
- λ=1 → Monte Carlo
- λ in between = smooth blend of short-term and long-term.

### Forward vs Backward View

**forward view:**

- look into the future to compute $G_t^\lambda$.
- theoretical, not practical (need complete episodes).

**backward view (eligibility traces):**

- keep track of which states are "eligible" for updates.
- update all states based on current TD error, weighted by eligibility.

### Eligibility Traces

$$ E*t(s) = \gamma \lambda E*{t-1}(s) + \mathbb{1}(S_t = s) $$

- decays over time ($\gamma \lambda$).
- increments when you visit the state.
- combines **frequency** (visited a lot) and **recency** (visited recently).

### Backward TD(λ) Update

$$ \delta*t = R*{t+1} + \gamma V(S\_{t+1}) - V(S_t) $$
$$ V(s) \leftarrow V(s) + \alpha \delta_t E_t(s) $$

- every state gets updated every step, not just the current one.
- states you visited recently get bigger updates.
- way more efficient than forward view.

### Why TD(λ) is Dope

- **online** learning (don't need to wait for episode to end).
- **efficient** credit assignment (spread the blame/credit properly).
- forward and backward views are **equivalent** for offline updates.
- λ lets you tune the bias-variance trade-off smoothly.

## Quick Comparison

| Method | Bootstraps? | Samples? | Needs Episodes? | Bias | Variance |
| ------ | ----------- | -------- | --------------- | ---- | -------- |
| DP     | Yes         | No       | No              | Low  | Low      |
| MC     | No          | Yes      | Yes             | Zero | High     |
| TD     | Yes         | Yes      | No              | Some | Low      |

## Takeaways

- **MC** = simple, unbiased, but high variance and needs full episodes.
- **TD** = learn online, low variance, but biased.
- **n-step TD** = compromise between MC and TD.
- **TD(λ)** = best of all worlds, smooth trade-off between short-term and long-term.
- eligibility traces = secret sauce for efficient credit assignment.

basically, if you can't wait for episodes to finish, use TD. if you want the most accurate estimates and can wait, use MC. if you're fancy, use TD(λ).
