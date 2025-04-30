---
layout: post
title: "Making Group Decisions: Voting"
description: "This series of posts will discusses on the self-interested agents, and how they work together. This post discusses on voting mechanism in group decision making"
date: 2025-04-12
feature_image: images/voting.jpg
tags: ['intelligent-agent']
---

In the previous post, we examined how agents make decisions in two-agent games, focusing on the **Nash Equilibrium**. In this post, we will explore how a group of agents make decisions, delving into the realm of social choice theory. A classic example of social choice theory is **voting**, where the challenge is to combine individual preferences to derive a social outcome. This post discusses voting mechanism in group decision making in Lecture 10 of the `SC4003` course in NTU. Let's begin!

<!--more-->

## Table of Contents

- [Notations and Definitions](#notations-and-definitions)
  - [Components of a Social Choice Model](#components-of-a-social-choice-model)
  - [Preferences](#preferences)
  - [Social Welfare Functions](#social-welfare-functions)
- [Voting Procedures: Plurality](#voting-procedures-plurality)
- [Sequential Majority Elections](#sequential-majority-elections)
- [Voting Procedures: Borda Count](#voting-procedures-borda-count)

---

# Notations and Definitions

Notation system make life easier when we are talking about something like voting. Let's have a short definition of game theory notations, which will make the rest of this post easier to understand.

## Components of a Social Choice Model

The set of voters (a.k.a agents in the context of the course) is defined as:

$$
    Ag = \{1, 2, \ldots, n\}
$$

Voters make group decisions with respect to a set of "outcomes" or "candidates":

$$
    \Omega = \{\omega_1, \omega_2, ... \omega_3\}
$$

If $\| \Omega \| = 2$, say we have a pairwise election.

## Preferences

Each voter has preferences over $\Omega$. It is an **ordering** over the set of possible outcomes $\Omega$. Take following wines as the example:

$$
    \Omega = \{ \text{gin}, \text{rum}, \text{brandy}, \text{whisky}\}
$$

The preference of agent $ray$ is denoted as:

$$
    \bar{\omega}_{ray} = \langle \text{whisky}, \text{rum}, \text{gin}, \text{brandy} \rangle
$$

which means that agent $ray$ prefers whisky to rum, rum to gin, gin to brandy, and brandy to whisky. These can also be written as:

$$
    \text{whisky} \succ_{ray} \text{rum} \succ_{ray} \text{gin} \succ_{ray} \text{brandy}
$$

The fundamental problem of **social choice theory** is to determine **how to aggregate individual preference orders** of voters into a collective decision that accurately represents the preferences of the group. This involves understanding how to merge diverse individual preferences into a coherent group preference. There are two primary approaches to preference aggregation: **social welfare functions**, which focus on ranking social states based on individual preferences, and **social choice functions**, which aim to select a single outcome from a set of alternatives.

## Social Welfare Functions

The set of preference orderings over $\Omega$ is denoted as:

$$
    \pi(\Omega)
$$

A social welfare function $f$ takes the **voter preferences order** and produces a **social preference order**, e.g. beauty contest:

$$
f: \pi(\Omega) \times \pi(\Omega) \times \ldots \times \pi(\Omega) \rightarrow \pi(\Omega)
$$

To denote the **outcome comparison** of a social welfare function:

$$
    \succ^*
$$

Sometimes, we want to select one of the possible candidates, rather than a social order. This gives social choice functions $f$, e.g. presidential election:

$$
f: \pi(\Omega) \times \pi(\Omega) \times \ldots \times \pi(\Omega) \rightarrow \Omega
$$

---

# Voting Procedures: Plurality

---

# Sequential Majority Elections

---

# Voting Procedures: Borda Count

---

Related Posts / Websites 👇

📑 [Ray - NTU SC4003 Lecture 3 Note: Intelligent Agent Prolegomenon](/Intelligent-Agent-Prolegomenon)

📑 [Ray - NTU SC4003 Lecture 4 Note: Agent Decision Making](/Agent-Decision-Making)

📑 [Ray - NTU SC4003 Lecture 5 Note: Agent Architecture](/Agent-Architecture)

📑 [Ray - NTU SC4003 Lecture 7 Note: Working Together Benevolent/Cooperative Agents](/Working-Together-Benevolent-Cooperative-Agents)

📑 [Ray - NTU SC4003 Lecture 8 Note: Game Theory Foundations](/Self-Interested-Agents-Game-Theory-Foundation)

📑 [Ray - NTU SC4003 Lecture 9 Note: Allocating Scarce Resources: Auction](Allocating-Scarce-Resources-Auction#vickrey-auctions)