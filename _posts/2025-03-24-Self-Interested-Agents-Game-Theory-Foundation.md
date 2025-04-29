---
layout: post
title: "Multiagent Decision Making among Self-Interested Agents: Game Theory Foundations"
description: "This series of posts will discusses on the self-interested agents, and how they work together. This post discusses the game theory foundations of self-interested agents"
date: 2025-03-24
feature_image: images/game-theory.png
tags: ['intelligent-agent', 'game-theory']
---

As we delve into the realm of multiagent decision making, game theory emerges as a powerful tool for understanding strategic interactions between self-interested agents. Born from the intersection of economics, mathematics, and computer science, game theory provides a robust framework for analyzing complex decision-making processes. In this post, we'll embark on an exploration of the game theory foundations that underlie the intricate dance of cooperation and competition among self-interested agents. This journey will cover the essential concepts and theories that form the backbone of Lecture 8 in the `SC4003` course at NTU. Buckle up, and let's dive into the fascinating world of game theory!

<!--more-->

## Table of Contents
- [Notations and Definitions](#notations-and-definitions)
  - [Utilities and Preferences](#utilities-and-preferences)
  - [Multiagent Encounters](#multiagent-encounters)
  - [Rational Action](#rational-action)
  - [Zero-Sum Game and Nonzero-Sum Game](#zero-sum-game-and-nonzero-sum-game)
- [Solution Concepts](#solution-concepts)
- [The Prisoner's Dilemma](#the-prisoners-dilemma)

---

# Notations and Definitions

Before we start, let's have a short definition of game theory notations, which will make the rest of this post easier to understand.

## Utilities and Preferences

Say we have two agents:

$$
    Ag = \{i, j\}
$$

They are assumed to be **self-interested**, which means they have preferences over how the environment is. The **set of "outcomes"** that agents have preferences over is denoted as:

$$
    \Omega = \{\omega_1, \omega_2, \ldots, \omega_n\}
$$

We capture preferences by **utility functions**:

$$
    U_i: \Omega \to \mathbb{R}, \quad U_j: \Omega \to \mathbb{R}
$$

Utility functions lead to preference **orderings** over outcomes:

$$
\begin{align*}
    \omega \succ_i \omega' &\iff U_i(\omega) > U_i(\omega') \\
    \omega \succeq_i \omega' &\iff U_i(\omega) \geq U_i(\omega') \\
\end{align*}
$$

Note that utility is not money, it is a **measure of desirability**. But it is a useful analogy.

## Multiagent Encounters

We need a model of the environment in which these agents will act. Agents simultaneously choose an action in set $Ac$ to perform, and as a result of the actions they select, an outcome $\omega$ in $\Omega$ will result. The actual outcome depends on the **combination of actions**. Assume each agent has just two possible actions that it can perform, cooperate $C$ and defect $D$. Environment behavior given by state transformer function:

$$
    \tau: Ac \times Ac \to \Omega
$$

For convenience, we can also express **utility functions** as follows. In fact, this notation will be used throughout this post instead of the one above.

$$
    U_{agent}: Ac \times Ac \to \mathbb{R}
$$

Classically, we will encounter following situation:

- $\tau(C, C) = \omega_1$, $\tau(C, D) = \omega_2$, $\tau(D, C) = \omega_3$, $\tau(D, D) = \omega_4$: the environment is sensitive to actions of both agents.
- $\tau(C, C) = \omega_1$, $\tau(C, D) = \omega_1$, $\tau(D, C) = \omega_1$, $\tau(D, D) = \omega_1$: neither agent has any influence in this environment.
- $\tau(C, C) = \omega_1$, $\tau(C, D) = \omega_1$, $\tau(D, C) = \omega_2$, $\tau(D, D) = \omega_2$: the environment is sensitive to actions of only one agent.

## Rational Action

Suppose we have the case where both agents can influence the outcome, and they have utility functions as follows:

$$
    U_i(C, C) = 4, \quad U_i(C, D) = 4, \quad U_i(D, C) = 1, \quad U_i(D, D) = 1
$$

$$
    U_j(C, C) = 4, \quad U_j(C, D) = 1, \quad U_j(D, C) = 4, \quad U_j(D, D) = 1
$$

The preferences of agent i are:

$$
    \langle C, C \rangle \succeq_i \langle C, D \rangle \succ_i \langle D, C \rangle \succeq_i \langle D, D \rangle
$$

Cooperate is a better choice for $i$, and $i$ will choose cooperate.

We can characterize the previous scenario in a **payoff matrix**:

{% include image_caption.html imageurl="/images/payoff-matrix.png" title="Payoff Matrix" caption="payoff matrix" %}

or in a more simplified form:

$$
    \left(
        \begin{matrix}
            (U_i(D, D), U_j(D, D)) & (U_i(C, D), U_j(C, D)) \\
            (U_i(D, C), U_j(D, C)) & (U_i(C, C), U_j(C, C))
        \end{matrix}
    \right)
    =
    \left(
        \begin{matrix}
            (1, 1) & (4, 1) \\
            (1, 4) & (4, 4)
        \end{matrix}
    \right)
$$

where agent $i$ is the column player, and agent $j$ is the row player.

## Zero-Sum Game and Nonzero-Sum Game

A game is **zero-sum** if the sum of utilities across all agents for every possible outcome is zero:

$$
    \forall (ac_i, ac_j) \in Ac \times Ac \rightarrow U_i(ac_i, ac_j) + U_j(ac_i, ac_j) = 0
$$

or more broadly defined as:

$$
    \forall \omega \in \Omega \rightarrow \sum_i U_i(\omega) = 0
$$

An example payoff matrix for a zero-sum game:

$$
    \left(
        \begin{matrix}
            (1, -1) & (-3, 3) \\
            (0, 0) & (-4, 4)
        \end{matrix}
    \right)
$$

In this **strict competition** scenario, pie is fixed. One player win necessarily leads to the other player losing, and vice versa.

A game is **non-zero-sum** if there exists at least one outcome where utilities do not sum to zero:

$$
    \exists (ac_i, ac_j) \in Ac \times Ac \rightarrow U_i(ac_i, ac_j) + U_j(ac_i, ac_j) \neq 0
$$

or more broadly defined as:

$$
    \exists \omega \in \Omega \rightarrow \sum_i U_i(\omega) \neq 0
$$

This scenario may allow cooperative outcomes (variable-pie). It is a more general case in the real world.

---

# Solution Concepts

---

# The Prisoner's Dilemma

---

Related Posts / Websites 👇

📑 [Ray - NTU SC4003 Lecture 3 Note: Intelligent Agent Prolegomenon](/Intelligent-Agent-Prolegomenon)

📑 [Ray - NTU SC4003 Lecture 4 Note: Agent Decision Making](/Agent-Decision-Making)

📑 [Ray - NTU SC4003 Lecture 5 Note: Agent Architecture](/Agent-Architecture)

📑 [Ray - NTU SC4003 Lecture 7 Note: Working Together Benevolent/Cooperative Agents](/Working-Together-Benevolent-Cooperative-Agents)