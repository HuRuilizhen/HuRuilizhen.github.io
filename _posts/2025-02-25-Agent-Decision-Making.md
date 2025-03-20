---
layout: post
title: "Agent Decision Making"
description: "A detailed discussion on the concept of agent decision making"
date: 2025-02-25
feature_image: images/intelligent-agents.png
tags: ['intelligent-agent']
---

**Agent decision making** is a crucial aspect of artificial intelligence. It is the process by which an autonomous entity, such as a robot or a computer program, makes decisions based on its available knowledge and perception of the environment. In this post, we will explore the different types of decision making, including **simple** and **complex decisions**, as well as sequential decision making, where the agent's utility depends on a sequence of decisions. We will also discuss the various techniques used in decision making.

<!--more-->

## Table of Contents

- [Making Simple Decisions](#making-simple-decisions)
  - [Utility Theory](#utility-theory)
  - [Multi-Attribute Utility Functions](#multi-attribute-utility-functions)
  - [Decision Networks](#decision-networks)
  - [The Value of Information](#the-value-of-information)
- [Making Complex Decisions](#making-complex-decisions)
  - [Make a sequence of decisions](#make-a-sequence-of-decisions)
  - [Markov Property](#markov-property)

---

# Making Simple Decisions

## Utility Theory

Remember what we have discussed on utility in the last post [Ray - Intelligent Agent Prolegomenon]({{ site.url }}{{ site.baseurl }}/Intelligent-Agent-Prolegomenon). We can say that a **utility function** is a function that assigns a **value** to a **state** or a **run** (in this post we more specifically mean a **state**). Each run will have a **probability** under which **agent** will be placed in the given **environment**. The concept of **expected utility** is defined as follows: (in this post we will use some simplified notations)

$$  
    EU(A \mid E) = \sum_{i} P(Result_i | E, Do(A)) U(Result_i)
$$

The principle of **maximizing expected utility** is to **choose action** $A$ with highest $EU(A \mid E)$.

We want our agent to be rational, that is to obey the constraints. And to have behavior describable as maximization of expected utility. To formulate utility theory we need a few more definitions or notations:

**Lottery** $L$ is a collection of possible outcomes with associated probabilities:

$$
    L = \{p_1, S_1; p_2, S_2; \ldots; p_n, S_n\} \quad \text{ where } \sum_{i=1}^n p_i = 1
$$

Different outcomes (lets say $S_i, S_j$) have following ordering relations:

- $S_i \succ S_j$: $S_i$ is preferred to $S_j$.
- $S_i \succeq S_j$: $S_j$ is not preferred to $S_i$.
- $S_i \sim S_j$: $S_i$ and $S_j$ are indifferent.

The constraints are defined as follows:

- **Orderability**: $(A \succ B) \vee (B \succ A) \vee (A \sim B)$.
- **Transitivity**: $(A \succ B) \land (B \succ C) \implies (A \sim C)$.
- **Continuity**: $(A \succ B \succ C) \implies \exists p \in [0,1] \text{ such that } B \sim pA + (1-p)C$.
- **Substitutability**: $(A \sim B) \implies \{p, A; 1-p, C\} \sim \{p, B; 1-p, C\}$.
- **Monotonicity**: $(A \succ B) \implies \{p, A; 1-p, C\} \succ \{p, B; 1-p, C\}$.
- **Decomposability**: $\{p, A; 1-p, \{ q, B; 1-q, C \} \} \sim \{ p, A; (1-p)q, B; (1-p)(1-q), C \}$

Utility Principle can be defined as follows:

$$
    U(A) > U(B) \implies A \succ B
$$

Maximum Expected Utility principle can be defined as follows:

$$
    U(\{ p_1, S_1; p_2, S_2; \ldots; p_n, S_n \}) = \sum_{i=1}^n p_i U(S_i)
$$

**Utility functions** indicates the goals that the agent’s actions aim to accomplish, which can be determined by analyzing the agent’s preferences.

## Multi-Attribute Utility Functions

## Decision Networks

## The Value of Information

---

# Making Complex Decisions

## Make a sequence of decisions

## Markov Property

---