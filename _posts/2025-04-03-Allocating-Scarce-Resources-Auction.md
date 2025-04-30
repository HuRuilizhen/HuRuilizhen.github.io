---
layout: post
title: "Allocating Scarce Resources: Auction"
description: "This series of posts will discusses on the self-interested agents, and how they work together. This post discusses the auction mechanism"
date: 2025-04-03
feature_image: images/auction.jpeg
tags: ['intelligent-agent']
---

The allocation of **scarce resources**, such as physical objects or computing resources, among multiple agents is a crucial aspect of multi-agent systems. When resources are abundant or uncontested, allocation is straightforward. However, in reality, we often face scarcity and competition, which is where **auction mechanisms** come into play. Once considered rare, auctions have become a common method for resource allocation in various domains. We will cover the auction mechanism in Lecture 9 in the `SC4003` course at NTU. Get ready!

<!--more-->

## Table of Contents
- [Basis of Auction Mechanism](#basis-of-auction-mechanism)
  - [Limit Price](#limit-price)
  - [Market Institution](#market-institution)
- [The Zoology of Auctions](#the-zoology-of-auctions)
  - [Single vs. Multi-Dimensional Auctions](#single-vs-multi-dimensional-auctions)
  - [Single vs Double-Sided Auctions](#single-vs-double-sided-auctions)
  - [Open-Cry vs. Sealed-Bid Auctions](#open-cry-vs-sealed-bid-auctions)
  - [Single vs. Multi-Unit Auctions](#single-vs-multi-unit-auctions)
  - [First vs K-th Price Auctions](#first-vs-k-th-price-auctions)
  - [Single vs. Multi-Item Auctions](#single-vs-multi-item-auctions)
- [Standard Auction Types](#standard-auction-types)
  - [English Auctions](#english-auctions)
  - [Dutch Auctions](#dutch-auctions)
  - [First-Price Sealed Bid Auctions](#first-price-sealed-bid-auctions)
  - [Vickrey Auctions](#vickrey-auctions)
- [Combinatorial Auctions](#combinatorial-auctions)
  - [Notations and Definitions](#notations-and-definitions)
  - [Bidding Languages](#bidding-languages)
  - [The VCG Mechanism](#the-vcg-mechanism)

---

# Basis of Auction Mechanism

The auction mechanism is concerned with the allocation of **goods and money between traders**. It assumes an initial allocation, and then allows for the free exchange of goods and money between traders to alter their allocations. The **goods** being traded are assumed to be **indivisible**, while the **money** is **divisible**.

## Limit Price

Each trader has a **value** or **limit price** that they place on the good. A buyer who exchanges more than their limit price for a good makes a loss while a seller who exchanges less than their limit price for a good makes a profit. We can tell that limit prices clearly have an effect on the behavior of traders.

There are several models, embodying different assumptions about the nature of the good. Following are typical models that often found literature:
- **Private Value**: Good has independent value for each trader.
- **Common Value**: Good has the same value for all traders, but  estimates may be different.
- **Correlated Value**: Values are related. E.g., the more you are prepared to pay, the more I should be prepared to pay.

## Market Institution

A market institution defines how the exchange takes place. It not only define what **messages** can be exchanged, but also defines how the final **allocation** depends on the messages. The change of allocation is so called **market clearing**. Say

> An **auction** is a market institution in which messages from traders include some price information — this information may be an offer to buy at a given price, in the case of a **bid**, or an offer to sell at a given price, in the case of an **ask** — and which gives priority to higher bids and lower asks.

---

# The Zoology of Auctions

Auctions can be divided into a number of different categories. Being good computer scientists, we draw up a **taxonomy**. This gives us a handle on all the kinds there might be, suggests parameterization, and help us to think about implementation. This way of classification is a little bit like **zoology**, but still a good way to think about things.

## Single vs. Multi-Dimensional Auctions

Single-dimensional auctions refer to the case where the only content of an offer are the price and quantity of some specific type of good. Whereas multi-dimensional auctions refer to the case where offers can relate to many different aspects of many different goods.

> "I'll bid \$200 for those 2 chairs" 

This is a single-dimensional since the price is only about the goods them. 

> "I'm prepared to pay \$200 for those two red chairs, but 300 if you can deliver them tomorrow."

This is a multi-dimensional since the price is also about the delivery time not only the goods.

## Single vs Double-Sided Auctions

Single-sided markets refer to either **one buyer** and many sellers (buy-side markets), or **one seller** and many buyers (sell-side markets). The latter is the thing we normally think of as an auction. On its opposite, double-sided markets refer to either many buyers and many sellers.

For example, the stock market is a double-sided market, where there are many buyers and many sellers. The same can be said for the housing market. On the other hand, if we have a company that is the only buyer of a particular product or service, then we have a one buyer market.

## Open-Cry vs. Sealed-Bid Auctions

In an **open-cry auction**, all traders **announce** their offers to **each other**, so bidders have **more information** about the market. This can be contrasted with a **sealed-bid auction**, where **only the auctioneer** sees the offers. In some auction forms, traders may even pay for preferential access to information. For example, in an open-cry auction, a bidder may be able to observe the offers made by other traders and adjust their own bid accordingly. In a sealed-bid auction, bidders do not have access to this information and must make their bids based on their own valuations.

## Single vs. Multi-Unit Auctions

In a **single-unit auction**, only one unit of the same good is allowed to be bid for at a time. If there are multiple units to be sold, the auctioneer may repeat the bidding process until all units are sold. For example, in an English auction selling a house, each bidder can keep bidding until a highest bidder is found, and if the highest bidder does not reach the reserve price, the auctioneer may then start another round of bidding. In contrast, in a **multi-unit auction**, bidders can bid on both the price and quantity of the units they want. The auctioneer will then determine the successful bidders and the quantity each gets. Note that the **unit** refers to the **indivisible unit** that we are **selling**, such as a single fish versus a box of fish.

## First vs K-th Price Auctions

The core difference between the two is how the price paid by the auction winner is determined. First-price auctions are auctions where the winner pays his or her own bid (i.e., the highest bid), and bidders need to **weigh "probability of winning" and "profit"**. The winner of the K-th highest-price auction pays the K-th highest bid (when K=2, it is the "second-highest-price auction"), and bidders are motivated to **bid according to the true valuation**. Second-highest-price auctions are widely used in online advertising.

## Single vs. Multi-Item Auctions

A single-item auction is an auction where there is only one item for sale and the highest bidder wins the item. The valuation of the item is simple and direct. In contrast, a multi-item auction is an auction where there are multiple items for sale and bidders can bid on combinations of items. The valuation of the combination **can be complex and may not be equal to the sum of the valuations of the individual items**. There are two types of multi-item auctions: simultaneous auctions and combinatorial auctions. In simultaneous auctions, multiple items are auctioned off at the same time, and the allocation of the items is determined by complex algorithms. In combinatorial auctions, bidders can bid on all possible combinations of items, and the allocation is determined by the VCG mechanism (see [VCG](#the-vcg-mechanism)). **The key difference between single-item and multi-item auctions is the complexity of the valuation of the items and the allocation of the items.**

--- 

# Standard Auction Types

By applying the above zoology, we can get a list of standard auction types which typically contains English Auctions, Dutch Auctions, First-Price Sealed Bid Auctions, Vickrey Auctions. We will start with English Auctions. And then in the next section we will move on to Combinatorial Auctions.

## English Auctions

| Auction Type | Dimensionality | Sidedness    | Units Available | Pricing Rule | Items Available |
| ------------ | -------------- | ------------ | --------------- | ------------ | --------------- |
| Open-cry     | Single         | Single-sided | Single unit     | First-price  | Single item     |

An **English auction** is the most common type of auction, where a single item is sold to the highest bidder. The auction process typically starts with an initial bid, which is then **increased by subsequent bidders** until a final price is reached. In some cases, the auctioneer may call out prices and buyers indicate their acceptance of the price. The seller may set a reserve price, which is the lowest acceptable price. The auction ends when a **fixed time is reached** (e.g. in internet auctions) or when there is **no more bidding** activity. The last bidder pays their bid, which is the final price.

## Dutch Auctions

| Auction Type | Dimensionality | Sidedness    | Units Available | Pricing Rule | Items Available |
| ------------ | -------------- | ------------ | --------------- | ------------ | --------------- |
| Open-cry     | Single         | Single-sided | Single unit     | First-price  | Single item     |

A Dutch auction, also known as a "descending clock" auction, is a type of auction where the **price starts high** and is **gradually lowered** until a bidder accepts the current price. Some auctions use a clock to display the prices, and the auctioneer calls out descending prices until one bidder claims the good by indicating the current price is acceptable. If there are multiple bidders who are willing to pay the same price, the auctioneer restarts the descent from a slightly higher price than the tie occurred at. The winner pays the price at which they "stop the clock".

We can tell that since auction proceeds **swiftly**, Dutch auctions can be used to sell **high volume** in a short period of time. It is often used to sell **perishable goods** like, flowers in the Netherlands, fish in Spain, and tobacco in Canada.

## First-Price Sealed Bid Auctions

| Auction Type | Dimensionality | Sidedness    | Units Available | Pricing Rule | Items Available |
| ------------ | -------------- | ------------ | --------------- | ------------ | --------------- |
| Sealed-bid   | Single         | Single-sided | Single unit     | First-price  | Single item     |

In contrast to English auctions, where bidders can **gain valuable information about the market from others' bids**, sealed-bid auctions like the First-Price Sealed Bid (FPSB) auction do not provide such information. Instead, the highest bidder wins the auction and pays the price they bid, with **no opportunity to adjust their bid based on others' valuations**.

Governments often use this mechanism to sell treasury bonds, e.g. UK still does while US recently changed to SPSB (Second Price Sealed Bid, a.k.a. Vickrey Auction). Property can also be sold this way as in Scotland.

## Vickrey Auctions

| Auction Type | Dimensionality | Sidedness    | Units Available | Pricing Rule | Items Available |
| ------------ | -------------- | ------------ | --------------- | ------------ | --------------- |
| Sealed-bid   | Single         | Single-sided | Single unit     | Second-price | Single item     |

The Vickrey auction is a **sealed-bid auction** mechanism in which the winning bidder pays the amount of the **second-highest bid**. This design is incentive compatible, meaning that bidders have an incentive to bid their true value. However, it is not a panacea and has been known to have issues in practice, such as when the New Zealand government used it to auction off spectrum licenses.

The Vickrey auction's incentive compatibility stems from the fact that **bidding truthfully is a dominant strategy**. If a bidder bids more than their valuation, they may win the good but risk paying more than they think it's worth. On the other hand, if they bid less than their valuation, they stand less chance of winning but may still end up paying the same price if they do win. Therefore, **there is no point in bidding above or below one's true valuation**, as it will not affect the outcome or the price paid.

---

# Combinatorial Auctions

Combinatorial auctions are particularly useful for allocating **bundles of goods**, such as spectrum licenses. For example, a telecommunications company may want to acquire licenses for the same bandwidth in different regions, such as Brooklyn, Manhattan, Queens, and Staten Island. In this case, the **licenses for the same bandwidth are the most valuable, but a different bandwidth license is still more valuable than no license at all**. The Federal Communications Commission (FCC) has used combinatorial auctions to allocate spectrum licenses in the past, but the auctions were not designed to handle bundles of goods.

## Notations and Definitions

Before we go further, we need to define some notations and definitions. The set of items to be auctioned is denoted as:

$$
    \mathcal{Z} = \{z_1, z_2, \ldots, z_m\}
$$

Then, we give the usual set of agents:

$$
    \mathcal{Ag} = \{1, 2, \ldots, n\}
$$

Capture preferences of agent $i$ with the valuation function:

$$
    v_i: 2^\mathcal{Z} \rightarrow \mathbb{R}
$$

which means that for every possible bundle of goods $Z \subseteq \mathcal{Z}$, agent $i$ has a valuation $v_i(Z)$. To specify the extreme cases, $v_i(\emptyset) = 0$.

Another useful idea is **free disposal**, an agent is never worse off having more stuff.

$$
    Z_1 \subseteq Z_2 \implies v_i(Z_1) \leq v_i(Z_2)
$$

Formally an allocation is a list of sets $Z_1 , Z_2, \ldots, Z_n$, one for each agent $Ag_i$ with the stipulation that:

$$
    Z_i, Z_j \subseteq \mathcal{Z} \quad Z_i \cap Z_j = \emptyset \quad \forall i \neq j
$$

Thus no good is allocated to more than one agent. The set of all allocations of $Z$ to agents $Ag$ is denoted as:

$$
    alloc(Z, Ag)
$$

If we design the auction, we get to say how the allocation is determined. One natural way is to maximize social welfare which is the sum of utilities of all the agents. The social welfare is denoted as:

$$
    sw(Z_1, Z_2, \ldots, Z_n;v_1, v_2, \ldots, v_n) = \sum_{i=1}^n v_i(Z_i)
$$

Given this, we can define a combinatorial auction. Given a set of goods $Z$ and a collection of valuation functions $v_1, v_2, \ldots, v_n$, one for each agent $I \in Ag$, the goal is to find an allocation:

$$
    Z_1^*, Z_2^*, \ldots, Z_n^* = \text{argmax}_{(Z_1, Z_2, \ldots, Z_n) \in alloc(Z, Ag)} sw(Z_1, Z_2, \ldots, Z_n;v_1, v_2, \ldots, v_n)
$$

Figuring this out is winner determination. We can achieve this by having every agent $i$ to declare their valuation $\hat{v}_i$. Then we just look at all the possible allocations and figure out what the best one is. However, on the one hand the hat denotes that this is what the agent **says**, not what it necessarily is. And on the other hand, the problem is representation, valuations are **exponential**:

$$
    v_i: 2^\mathcal{Z} \rightarrow \mathbb{R}
$$

which means that searching through is computationally intractable.

## Bidding Languages

Rather than exhaustive evaluations, allow bidders to construct valuations from the bids they want to mention. Atomic bids are denoted as:

$$
    (Z, p), \quad Z \subseteq \mathcal{Z}, \quad p \in \mathbb{R}
$$

A bundle $Z^\prime$ **satisfies** a bid $(Z, p)$ if $Z \subseteq Z^\prime$. In other words a bundle satisfies a bid if it contains at least the things in the bid. Atomic bids define valuations as follows:

$$
    v_\beta(Z^\prime) = \begin{cases}
        p & \text{if } Z^\prime \subseteq Z \\
        0 & \text{otherwise}
    \end{cases}
$$

But atomic bids alone don’t allow us to construct very interesting valuations. To construct more complex valuations, atomic bids can be combined into more complex bids. One approach is $XOR$ bids. Here we give an example of it:

$$
    \beta_1 = (\{a, b\}, 3) \text{ XOR } (\{b, c\}, 5)
$$

which can be read to mean I would pay $3$ for a bundle that contains a and $b$ but not $c$ and $d$, and I will pay $5$ for a bundle that contains $c$ and $d$ but not $a$ and $b$, and I will pay $5$ for a bundle that contains $a$, $b$, $c$ and $d$. Constructing valuations:

$$
\begin{align*}
    v_{\beta_1}(\{a\}) &= 0\\
    v_{\beta_1}(\{b\}) &= 0\\
    v_{\beta_1}(\{a, b\}) &= 3\\
    v_{\beta_1}(\{c, d\}) &= 5\\
    v_{\beta_1}(\{a, b, c, d\}) &= 5\\
\end{align*}
$$

More formally, a bid like this:

$$
    \beta = (Z_1, p_1) \text{ XOR } (Z_2, p_2) \text{ XOR } \ldots \text{ XOR } (Z_n, p_n)
$$

Defines a valuation $v_\beta$ as follows:

$$
    v_\beta(Z^\prime) = \begin{cases}
        \max \{p_i | Z_i \subseteq Z^\prime \} & \text{if } Z_i \subseteq Z^\prime \\
        0 & \text{otherwise}
    \end{cases}
$$

XOR bids are **fully expressive**, that they can express any valuation function over a set of good. To do that, we may need an exponentially large number of atomic bids. However, the valuation of a bundle can be computed in polynomial time.

## The VCG Mechanism

In general, we do not know whether the $\hat{v}_i$ are the true valuations. Life would be easier if they were! In a generalization of the Vickrey auction (Vickrey/Clarke/Groves Mechanism). Mechanism is incentive compatible: telling the truth is a dominant strategy.

To demonstrate this, we need to define more notations:

Indifferent valuation function is that

$$
    v^0(Z) = 0, \quad \forall Z \subseteq \mathcal{Z}
$$

$sw_{-i}$ is the social welfare function without $i$ is:

$$
    sw_{-i}(Z_1, \ldots, Z_n; \hat{v}_1, \ldots, \hat{v}_n) = \sum_{j \neq i} v_j(Z_j)
$$

Now we can define the VCG mechanism:

- Every agents simultaneously declares a valuation $\hat{v}_i$.
- The mechanism computes as followsm, and the allocation $Z_1^*, Z_2^*, \ldots, Z_n^*$ is chosen.

    $$
        Z_1^*, \ldots, Z_n^* = \text{argmax}_{(Z_1, \ldots, Z_n) \in alloc(\mathcal{Z}, \mathcal{Ag})} sw(Z_1, \ldots, Z_n; \hat{v}_1, \ldots, \hat{v}_n)
    $$

- The mechanism also computes, for each agent $i$:
    
    $$
        Z_1^\prime, \ldots, Z_n^\prime = \text{argmax}_{(Z_1, \ldots, Z_n) \in alloc(\mathcal{Z}, \mathcal{Ag})} sw(Z_1, \ldots, Z_n; \hat{v}_1, \ldots, v^0, \ldots, \hat{v}_n)
    $$

- Every agent $i$ pays $p_i$, where

    $$
        p_i = sw_{-i}(Z_1^\prime, \ldots, Z_n^\prime; \hat{v}_1, \ldots, v^0, \ldots, \hat{v}_n) - sw_{-i}(Z_1^\prime, \ldots, Z_n^\prime; \hat{v}_1, \ldots, \hat{v}_n)
    $$

In essence, each agent pays the cost to other agents for participating in the auction. This mechanism is **incentive compatible**, similar to the Vickrey auction. If an agent bids above their true valuation and wins, they end up paying what the good is worth to others, which exceeds its value to them. Conversely, if an agent bids conservatively, they reduce their chances of winning, yet still pay what others deem the good is worth, thus not saving any money. Consequently, a dominant strategy emerges for each agent, ensuring the maximization of social welfare.

---

Related Posts / Websites 👇

📑 [Ray - NTU SC4003 Lecture 3 Note: Intelligent Agent Prolegomenon](/Intelligent-Agent-Prolegomenon)

📑 [Ray - NTU SC4003 Lecture 4 Note: Agent Decision Making](/Agent-Decision-Making)

📑 [Ray - NTU SC4003 Lecture 5 Note: Agent Architecture](/Agent-Architecture)

📑 [Ray - NTU SC4003 Lecture 7 Note: Working Together Benevolent/Cooperative Agents](/Working-Together-Benevolent-Cooperative-Agents)

📑 [Ray - NTU SC4003 Lecture 8 Note: Game Theory Foundations](/Self-Interested-Agents-Game-Theory-Foundation)

📑 [Ray - NTU SC4003 Lecture 10 Note: Voting Mechanism](/Making-Group-Decisions-Voting)