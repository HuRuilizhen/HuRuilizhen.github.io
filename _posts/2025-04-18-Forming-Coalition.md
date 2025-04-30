---
layout: post
title: "Forming Coalition"
description: "This series of posts will discusses on the self-interested agents, and how they work together. This post discusses on coalition"
date: 2025-04-18
feature_image: images/coalition.jpg
tags: ['intelligent-agent']
---

Coalitional games are a type of game theoretical model that study scenarios where agents can reap mutual benefits from cooperation. In such games, a coalition is a group of agents that work together and obtain a payoff for their collective effort. The agents are considered "cooperative" since cooperation yields them more payoffs than non-cooperation. This post discusses on coalition in Lecture 11 in the `SC4003` course in NTU. See details below!

<!--more-->

## Table of Contents
- [Coalitional Game is a Cooperative Game](#coalitional-game-is-a-cooperative-game)
  - [Coalition Structure Generation](#coalition-structure-generation)
  - [Coalition Values - Benefit of Cooperation](#coalition-values---benefit-of-cooperation)
  - [Dividing Coalition Values](#dividing-coalition-values)
- [Some solutions](#some-solutions)
  - [The Core](#the-core)
  - [The Shapley Value](#the-shapley-value)
- [Applications](#applications)

---

# Coalitional Game is a Cooperative Game

Key Issues in Coalitional Games (Sandholm 1999):
- **Coalition Structure Generation**: The process of identifying and forming optimal coalitions among agents to maximize collective benefits.
- **Coalition Values**: Assessment of the benefits derived from cooperation within a coalition, representing the total value created by the coalition.
- **Division of Coalition Values**: Determination of a fair and equitable distribution of the total coalition value among its members based on predefined rules or principles.

## Coalition Structure Generation

When agents join coalitions, the overall state is a coalition structure. And there are two types of coalition structure:

- Allowing overlapping
- No overlapping, also called partitions.Partition agents into disjoint coalitions. The overall partition is a coalition structure. In this post we will mainly focus on non-overlapping coalitions.

## Coalition Values - Benefit of Cooperation

Given a set of players or agents:

$$
    Ag = \{1, 2, \ldots, n\}
$$

Coalition values have a form of a characteristic function:

$$
    v: 2^{Ag} \rightarrow \mathbb{R}
$$

There are two types of coalition values: 
- Transferable coalition value, a.k.a  transferable utility (TU), which we will focus on.
- Non-transferable coalition value, which is a bit more complex.

## Dividing Coalition Values

Having gained the coalition values, how to allocate the values to each agent is a key issue in coalition games. Define the coalition $S$ a subset of agent set:

$$
    S \subseteq Ag
$$

The allocation value to each agent $i \in S$ is a vector of size $\|S\|$:

$$
    \mathbf{x} = \langle x_1, x_2, \ldots, x_{|S|} \rangle \in \mathbb{R}^{|S|}
$$

A feasible distribution should satisfy:

$$
    \sum_{i \in S} x_i \leq v(S)
$$

Another issue is how to make the coalition stable. Will any agents defect from the coalition? If coalition try to give individuals a bad payoff, individuals can always walk away.

---

# Some solutions

## The Core

The core of a coalitional game is a set of **feasible distributions** of payoffs to members of a coalition that **no sub-coalition can reasonably object** to the grand coalition. Define the grand coalition as subset $S^\prime$ of all agents:

$$
    S^\prime \subseteq Ag
$$


A distribution $\mathbf{x}$ is called **core** if it is satisfies **efficiency**, the total distribution cannot exceed the total value that the grand coalition can bring:

$$
    \sum_{i \in S^\prime} x_i = v(S^\prime)
$$

and **coalitional rationalit**, which means for any subset $S_{sub}$ of $S^\prime$, the total benefit allocated to them should not be less than what they can obtain by working alone.

$$
    \sum_{i \in S_{sub}} x_i \geq v(S_{sub})
$$

It the a subset $S_{sub}$ of $S^\prime$ satisfies following condition, we say it is a **objection**:

$$
    x^{sub}_i > x^\prime_i \quad \forall i \in S_{sub}
$$

which means we have a sub-coalition that can object to the grand coalition because it gets more benefit from working alone. If the grand coalition has no objection, then the distribution is called **core**.

We can tell that the core is not always unique, suppose:

$$
    Ag = \{1, 2\}, \quad v(\{1\}) = 5, \quad v(\{2\}) = 5, \quad v(\{1, 2\}) = 20
$$

Then, by the definition of core, we actually have many cores. And one of them maybe:

$$
    \langle 5, 15 \rangle
$$

But it seems not fair to agent $1$. The following one is much more fair:

$$
    \langle 10, 10 \rangle
$$

Hence, we need to find a way to fairly distribute the coalition values.

## The Shapley Value

The Shapley value is a best known attempt to define how to divide coalition value fairly. The Shapley value of an agent i is the average
amount of agent $i$'s marginal contribution:

$$
    \phi_i(v) = \sum_{S_{sub} \subseteq S^\prime - \{i\}} \frac{\|S_{sub}\|!(\|S^\prime - \{i\}\| - \|S_{sub}\| - 1)!}{\|S^\prime - \{i\}\|!} (v(S_{sub} \cup \{i\}) - v(S_{sub}))
$$

where $n!$ is the factorial of $n$



Imagine the coalition being formed one agent at a time, with each agent demanding their contribution:

$$
    v(S_{sub} \cup \{i\}) - v(S_{sub})
$$

as a fair compensation, and then averaging over the possible different permutations in which the coalition can be formed.

The Shapley value has four properties:
- **Efficiency**: the total payoff is distributed

    $$
        \sum_{i \in S^\prime} \phi_i(v) = v(S^\prime)
    $$

- **Symmetry**: if agents $i$ and $j$ are equivalent in a sense that

    $$
      S_{sub} \cap \{i, j\} = \emptyset \wedge v(S_{sub} \cup \{i\}) = v(S_{sub} \cup \{j\}) \implies\phi_i(v) = \phi_j(v)
    $$

- **Additivity**:

    $$
        \phi_i(v) + \phi_i(w) = \phi(v+w)
    $$

- **Dummy agent (null agent)**:

    $$
        v(S^\prime \cup ag_0) = v(S^\prime)
    $$

The Shapley value **may or may not be in the Core**, as it is a **fairness-oriented solution concept** that does not necessarily consider the stability of the coalition. The Core, on the other hand, is a **stability-oriented solution concept** that ensures no subset of agents can do better by forming a coalition outside of the grand coalition. Therefore, there is no direct relationship between the Shapley value and the Core.

According to Shapley value equation, naive, obvious representation of coalitional game is **exponential** in the size of $n$. Not practical. Hence we introduce **Induced subgraph** (succinct but incomplete) and **Marginal contribution nets** (not succinct but complete).

For **Induced subgraph**, we represents $v$ as an undirected graph on $S^\prime$, with integer weights $w_{i,j}$ between ndoes $i, j \in S^\prime$:

$$
    w_{i,j} \in \mathbb{Z} \quad i, j \in S^\prime
$$

Value of a coalition $S_{sub}$ is then:

$$
    v(S_{sub}) = \sum_{(i, j) \in S_{sub}} w_{i,j}
$$

The Shapley value of agent $i$ is:

$$
    \phi_i(v) = \frac{1}{2} \sum w_{i, j}
$$

If there is a self-loop, the self-loop weight is added to the agent's Shapley value. Lets take an example:

{% include image_caption.html imageurl="/images/induced-graph.png" title="Induced Graph" caption="induced graph" %}

For **Marginal contribution nets**, the basic idea is to use a set of rules to describe the payoffs of coalitions:

$$
    \text{pattern} \rightarrow \text{value}
$$

The value of a coalition $S_{sub}$ is the sum of all the payoffs of rules satisfied by that coalition.

$$
    v(S_{sub}) = \sum_{r \in \{Rules\}} \text{value}_{r}
$$

The Shapley value of an agent in an MCN is equal to the sum of the Shapley values of the agent over each rule:

$$
    \phi_i(v) = \sum_{r \in \{Rules\}} \phi_i^r(v), \quad \phi_i^r(v) = \frac{value_r}{\|r\|}
$$

An Example, let's have a set of rules:

$$
    \{a,b\} \rightarrow 5, \quad \{b\} \rightarrow 2
$$

Then we get that:

$$
    v(\{a,b\}) = 5, \quad v(\{b\}) = 2, \quad v(\{a\}) = 0
$$

The Shapley value of agent $a$ is:

$$
    \phi_a(v) = \frac{5}{2} + \frac{0}{1} = 2.5
$$

The Shapley value of agent $b$ is:

$$
    \phi_b(v) = \frac{5}{2} + \frac{2}{1} = 4.5
$$

---

# Applications

**Multi-Agent Systems Coordination** and **Explainable Artificial Intelligence** are two areas where Shapley values are being applied.

In multi-agent systems coordination, Shapley values are used to fairly allocate tasks and resources among self-interested agents. This is particularly useful in **team formation** and **collaborative computing**, where the goal is to maximize the overall performance of the team.

In explainable AI, Shapley values are used to assign importance scores to features in machine learning models. This is useful in high-stakes applications where the decisions made by the model need to be **interpretable** and **transparent**. The Shapley value approach was introduced by SHAP in 2017 and has since become a widely used method for explaining complex models. It has been used to interpret even very large and highly complex models.

---

Related Posts / Websites 👇

📑 [Ray - NTU SC4003 Lecture 3 Note: Intelligent Agent Prolegomenon](/Intelligent-Agent-Prolegomenon)

📑 [Ray - NTU SC4003 Lecture 4 Note: Agent Decision Making](/Agent-Decision-Making)

📑 [Ray - NTU SC4003 Lecture 5 Note: Agent Architecture](/Agent-Architecture)

📑 [Ray - NTU SC4003 Lecture 7 Note: Working Together Benevolent/Cooperative Agents](/Working-Together-Benevolent-Cooperative-Agents)

📑 [Ray - NTU SC4003 Lecture 8 Note: Game Theory Foundations](/Self-Interested-Agents-Game-Theory-Foundation)

📑 [Ray - NTU SC4003 Lecture 9 Note: Allocating Scarce Resources: Auction](Allocating-Scarce-Resources-Auction#vickrey-auctions)

📑 [Ray - NTU SC4003 Lecture 10 Note: Voting Mechanism](/Making-Group-Decisions-Voting)