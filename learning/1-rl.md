# RL Notes: RL (David Silver, Lecture 1)

# referenceed sources

- https://davidstarsilver.wordpress.com/wp-content/uploads/2025/04/intro_rl.pdf
- https://www.youtube.com/watch?v=2pWv7GOvuf0

## 1. What's Different About RL

- nothing is right or wrong. Just rewards.
- feedback shows up late. Not instant like supervised learning.
- time actually matters here. Can't shuffle the data.
- actions mess with the environment.

## Reward Hypothesis

- everything = maximizing cumulative reward.
- **All goals can be described by the maximisation of expected cumulative reward**
- kinda weird, all goals, preferences, whatever => just rewards.
- debatable tbh, but that's the core assumption ig.

## Sequential Decisions

- pick actions to max future reward, not just right now.
- take a hit from game boss to setup a win later.

## How It Works

agent and environment loop:
at time **{$t$}**:

- agent does **{$A_t$}**
- gets observation **{$O_t$}**
- gets reward **{$R_t$}**
- environment updates **{$E_t$}**
- repeat

## State vs History

- **history**: everything that happened => observations, actions, rewards.
- **state**: the relevant info that determines what's next.

**Markov State** = future only depends on now, not the whole past.
$ P[S_{t+1} | S_t] = P[S_{t+1} | S_1, ..., S_t] $\
basically,

> **current state is all you need**.

### Fully Observable (MDP)

you see everything. agent state = environment state.
the normal markov decision process (MDP).

### Partially Observable (POMDP)

don't see the full picture.
agent builds its own guess of state (not clear on how it does that yet).

## Agent Parts

three main things:

**Policy**: what the agent does. State => action.

- Deterministic: $a = \pi(s)$
- Stochastic: $\pi(a|s)$ gives probabilities

**Value Function**: how good is this state? Predicts future reward.
$ v*\pi(s) = E*\pi[R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... | S_t = s] $

**Model**: predicts environment behavior. transitions + rewards.
not always needed.

## Agent Types

**Value-Based**: no policy, just value function. implicit behavior.
**Policy-Based**: explicit policy, no value function.
**Actor-Critic**: both. Policy + value function.

**Model-Free**: no environment model. just learn from experience.
**Model-Based**: builds a model of how things work.

## RL vs Planning

**RL**: don't know how environment works. trial and error. learn while doing.

**Planning**: you have the model. just compute best moves. no actual interaction needed.
like tree search or DP.

## Exploration vs Exploitation

- **Explore**: try new stuff, might learn something useful.
- **Exploit**: stick with what works, max reward now.

kinda like games safe move represents exploitation, risky experiments represent exploration.\
need to balaance both.

- pure exploitation = might miss better options.
- pure exploration = waste time on garbage.

## Prediction vs Control

- **Prediction**: given a policy, how good is it? evaluate future. \
- **Control**: find the best policy. optimize.
