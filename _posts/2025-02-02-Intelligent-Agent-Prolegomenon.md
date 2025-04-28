---
layout: post
title: "Intelligent Agent Prolegomenon"
description: "A brief introduction to the concept of an intelligent agent"
date: 2025-02-02
feature_image: images/intelligent-agents.png
tags: ['intelligent-agent']
---

An **intelligent agent** is a system that perceives its environment and takes actions that maximize its chances of successfully achieving its goals. Agents are used to build a wide variety of applications, including web search engines, recommender systems, smart home devices, and autonomous vehicles. The agent paradigm is also used to study human decision-making and to build artificial intelligence systems that interact with humans. This post provides a comprehensive discussion of the concept of an intelligent agent in Lecture 3 of the `SC4003` course in NTU.

<!--more-->

## Table of Contents

- [Definition and Properties](#definition-and-properties)
  - [Reactivity](#reactivity)
  - [Proactiveness](#proactiveness)
  - [Social Ability](#social-ability)
  - [Other Properties](#other-properties)
- [Agents and Related Concepts](#agents-and-related-concepts)
  - [Agents and Objects](#agents-and-objects)
  - [Agents and Artificial Intelligence](#agents-and-artificial-intelligence)
  - [Agents and Expert Systems](#agents-and-expert-systems)
  - [Agents as Intensional Systems](#agents-as-intensional-systems)
- [Environment and its classification](#environment-and-its-classification)
  - [Accessibility and Inaccessibility](#accessibility-and-inaccessibility)
  - [Deterministic and Non-Deterministic](#deterministic-and-non-deterministic)
  - [Episodic and Non-Episodic](#episodic-and-non-episodic)
  - [Static and Dynamic](#static-and-dynamic)
  - [Discrete and Continuous](#discrete-and-continuous)
- [Abstract Architecture for Agents](#abstract-architecture-for-agents)
  - [Environment, Agent and System](#environment-agent-and-system)
  - [Pure Reactive Agents, Perception and Agent with State](#pure-reactive-agents-perception-and-agent-with-state)
  - [Task Evaluation](#task-evaluation)

---

# Definition and Properties

<div style="text-align: center;" class="mermaid">
graph LR
    classDef system fill:#EEE,stroke:#333,stroke-width:2px,rx:10;
    classDef environment fill:#EEF,stroke:#333,stroke-width:2px,rx:10;

    System[System]:::system
    Environment[Environment]:::environment

    System --> Environment
    Environment --> System
</div>

An agent is a computer system capable of **autonomous** action in some environment in order to meet its design objectives.

$$
\text{autonomous} \begin{cases}
    \text{capable of acting independently} \\
    \text{control over internal state}
\end{cases}
$$

An **intelligent** agent is a computer system capable of **flexible** autonomous action in some environment in order to meet its design objectives.

$$
\text{flexible} \begin{cases}
    \text{reactive}\\
    \text{pro-active}\\
    \text{social}
\end{cases}
$$

## Reactivity
If the environment is fixed, a program can simply follow its instructions without worrying about success or failure (like a compiler). However, in dynamic environments, a program must take into account the possible changes in the environment and be able to handle changes. A reactive system is one that maintains an **ongoing interaction with its environment** and **responds to changes in a timely manner**.

## Proactiveness
Proactive behavior involves **goal-directed** activity, where an agent takes the **initiative** to achieve its objectives rather than simply **reacting** to events. This capability is essential for autonomous systems, as it enables them to **recognize** opportunities and take advantage of them to achieve their goals.

A key challenge in designing autonomous systems is **balancing** the need for **reactivity** against the need for **proactiveness**. On the one hand, we want our agents to be reactive, responding to changing conditions in an appropriate (timely) fashion. On the other hand, we want our agents to systematically work towards long-term goals. These two considerations can be at odds with one another. Designing an agent that can balance the two remains an **open research problem**.

## Social Ability
In real-world multi-agent environments, goals cannot be achieved unilaterally without consideration for other agents. In many cases, **cooperation** with other agents is a necessary condition for achieving goals. This is also true for many computing environments, such as the Internet. Social ability in agents enables them to interact with other agents (and potentially humans) using an **agent-communication language**, and to collaborate with others when necessary.

## Other Properties
<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Mobility</td>
      <td>The ability of an agent to move around an electronic network.</td>
    </tr>
    <tr>
      <td>Veracity</td>
      <td>An agent will not knowingly communicate false information.</td>
    </tr>
    <tr>
      <td>Benevolence</td>
      <td>Agents do not have conflicting goals, and every agent will therefore always try to do what is asked of it.</td>
    </tr>
    <tr>
      <td>Rationality</td>
      <td>An agent will act in order to achieve its goals, and will not act in such a way as to prevent its goals from being achieved, as far as its beliefs permit.</td>
    </tr>
    <tr>
      <td>Learning/Adaptation</td>
      <td>Agents improve performance over time.</td>
    </tr>
  </tbody>
</table>

# Agents and Related Concepts

## Agents and Objects

> Q: Are agents just objects by another name?


Objects encapsulate some state and communicate through message passing, with methods that correspond to the operations that can be performed on that state. The primary distinction between agents and objects lies in the **level of autonomy** exhibited by agents. Agents are self-directed, making decisions about whether to execute requests from other agents. Moreover, agents are intelligent, possessing **flexible behavior capabilities** (reactivity, proactivity, and social ability) that are not addressed in the standard object model. Lastly, agents are active, with multi-agent systems being inherently multi-threaded, as each agent is assumed to have at least **one active control thread.**

- Agents act because of their own **desires**.
- Agents act because of their own **interests**.

## Agents and Artificial Intelligence

> Q: Aren't agents just the AI project? Isn’t building an agent what AI is all about?

Artificial intelligence strives to build systems that can understand natural language, recognize and interpret scenes, use common sense, think creatively, and more. These tasks are extremely challenging.

In contrast, when developing an agent, we aim to create a system that can select the right action in a **limited** domain. We do not need to solve all the problems of AI to build a useful agent. <font color="red">A little intelligence can go a long way!</font>

## Agents and Expert Systems

> Q: Aren't agents just expert systems by another name?

Expert systems typically focus on abstract domains of discourse, such as blood diseases. For example, [MYCIN](https://en.wikipedia.org/wiki/Mycin) is an expert system that knows about blood diseases in humans. It has a wealth of knowledge in the **form of rules**, and a doctor can obtain expert advice by giving MYCIN facts, answering questions, and posing queries.

The main differences between expert systems and agents are:

- Agents are **situated in an environment**, whereas MYCIN is not aware of the world and only obtains information by asking the user questions.
- Agents **act**, whereas MYCIN does not operate on patients. However, some real-time (typically process control) expert systems can be considered agents.

## Agents as Intensional Systems

The **intentional stance** is a powerful tool for abstracting and describing complex systems. By attributing beliefs, desires, and rationality to an entity, we can predict and explain its behavior. This stance is particularly useful when dealing with complex systems whose inner workings are not fully understood.

The **philosopher Daniel Dennett** identifies different grades of intentional systems. A first-order intentional system has beliefs and desires, but no beliefs and desires about beliefs and desires. A second-order intentional system is more sophisticated, with beliefs and desires about beliefs and desires.

It is interesting to note that the intentional stance can be applied to any object, even a light switch. However, most adults would find such a description absurd, as it is not a useful abstraction in this case. The reason is that we have a simpler, mechanistic description of the light switch's behavior, which makes the intentional stance unnecessary.

As computer systems become **increasingly complex**, we need more **powerful abstractions** and **metaphors** to explain their operation. The intentional stance is such an abstraction, which provides us with a convenient and familiar way of describing, explaining, and predicting the behavior of complex systems.

In the context of agent theory, the intentional stance is a fundamental concept. Agents are viewed as intentional systems, whose simplest consistent description requires the intentional stance. This means that agents are attributed with beliefs, desires, and rationality, which are used to predict and explain their behavior. The intentional stance is a powerful tool for abstracting and describing complex systems, and it is essential for understanding and designing agents.

# Environment and its classification

## Accessibility and Inaccessibility

An **accessible** environment is one in which the agent can obtain **complete**, **accurate**, **up-to-date** information about the environment's state. Most moderately complex environments (including, for example, the everyday physical world and the Internet) are **inaccessible**. The more accessible an environment is, the simpler it is to build agents to operate in it.

## Deterministic and Non-Deterministic

A **deterministic** environment is characterized by actions that consistently lead to a **single, predictable** outcome without any ambiguity regarding the resulting state. In contrast, the physical world, for all practical reasons, can be considered **non-deterministic**. Such **non-deterministic** environments pose significant challenges for those designing agents, as they must account for **uncertainty** and **variability** in outcomes.

## Episodic and Non-Episodic

In an **episodic environment**, the performance of an agent is dependent on a number of discrete episodes, with no link between the performance of an agent in different scenarios. **Episodic environments are simpler from the agent developer’s perspective** because the agent can decide what action to perform based only on the current episode — it need not reason about the interactions between this and future episodes.

## Static and Dynamic

A **static** environment is one that can be assumed to remain unchanged except by the performance of actions by the agent. In contrast, a **dynamic** environment is one that has other processes operating on it, and which hence changes in ways beyond the agent’s control. In such environments, other processes can **interfere** with the agent’s actions (as in concurrent systems theory). For example, the physical world is a highly dynamic environment.

## Discrete and Continuous

An environment is **discrete** if there are a **fixed**, **finite** number of actions and percepts in it. Russell and Norvig give a chess game as an example of a discrete environment, and taxi driving as an example of a continuous one. Continuous environments have a certain level of mismatch with computer systems. Discrete environments could in principle be handled by a kind of "lookup table".

# Abstract Architecture for Agents

## Environment, Agent and System

Assume the **environment** may be in any of a finite set $E$ of discrete, instantaneous **states**:

$$
    E = \{e_1, e_2, \ldots, e_n\}
$$

Agents are assumed to have a repertoire of possible **actions** available to them, which **transform** the state of the environment:

$$
    Ac = \{\alpha_1, \alpha_2, \ldots, \alpha_{n-1}\}
$$

A **run**, $r$, of an agent in an environment is a **sequence** of interleaved environment **states** and **actions**:

$$
    r = (e_1, \alpha_1, e_2, \alpha_2, \ldots) \in E \times (Ac \times E)^* \text{ or } (E \times Ac)^*
$$

$R$ is denoted as set of the all such **possible** finite **sequences** over $E$ and $Ac$

$$
    R = \{r, r', \ldots\}
$$

$R^{Ac}$ is the subset of $R$ that **end** with an **action**.

$$
    R^{Ac} = \{\forall r \in (E \times Ac)^*\}
$$

$R^{E}$ is the subset of $R$ that **end** with an **environment state**.

$$
    R^{E} = \{\forall r \in E \times (Ac \times E)^*\}
$$

A **state transformer function** represents **behavior** of the **environment**.

$$
    \tau : R^{Ac} \to \phi(E)
$$

In summary, an **environment** is formally defined as a triple, $\langle E, e_0, \tau \rangle$, where:
- $E$ is a set of environment states;
- $e_0 \in E$ is the initial state;
- $\tau$ is a state transformer function.

Note that **environments** are:
- **history dependent**: The current state of the environment is a result of all the previous actions and events.
- **non-deterministic**: The outcome of an action is not always certain and may lead to different new states.
- If $\tau(r)=\emptyset$, then there are no possible successor states to $r$. In this case, we say that the system has **ended its run**.

Agent is a function maps runs to actions:

$$
    Ag: R^{E} \to Ac
$$
An agent makes a decision about what action to perform based on the history of the system that it has witnessed to date. Let $\mathcal{AG}$ be the set of all agents.

A **system** is a pair containing an **agent** and an **environment**. Any system will have associated with it a set of possible **runs**; we denote the set of runs of agent $Ag$ in environment $Env$ by $R(Ag, Env)$. We assume $R(Ag, Env)$ contains only **terminated runs**, that is,

$$
    \forall r \in R(Ag, Env) \implies \tau(r) = \emptyset
$$

For a sequence of runs, $(e_1, \alpha_1, e_2, \alpha_2, \ldots)$, represents a run of an agent $Ag$ in environment $Env = \langle E, e_0, \tau \rangle$. If 
- $e_0$ is the initial state of the $Env$
- $\alpha_0 = Ag(e_0)$
- For $u > 0$, $e_u \in \tau((e_0, \alpha_0, \ldots, \alpha_{u-1}))$ and $\alpha_u \in Ag((e_0, \alpha_0, \ldots, e_u))$

## Pure Reactive Agents, Perception and Agent with State

Some agents decide what to do without reference to their history — they base their decision making entirely on the present, with no reference at all to the past. We call such agents **purely reactive**. Mathematically, a purely reactive agent is a function from the environment state space (instead of the run space) to the action space:

$$
    action(e) : E \to Ac
$$

A thermostat is a purely reactive agent: it simply responds to the current temperature and does not maintain any internal state:
$$
    action(e) = \begin{cases}
        \text{increase the temperature} & \text{if the temperature is too low}\\
        \text{decrease the temperature} & \text{if the temperature is too high}\\
        \text{do nothing} & \text{otherwise}
    \end{cases}
$$

<div style="text-align: center;" class="mermaid">
graph LR
  classDef system fill:#EEE,stroke:#333,stroke-width:2px,rx:10;
  classDef environment fill:#EEF,stroke:#333,stroke-width:2px,rx:10;

  See[See]:::system
  Action[Action]:::system
  Environment[Environment]:::environment

  Environment --> See
  See --> Action
  Action --> Environment
</div>

**Perception Systems** have a **see function** and an **action function**. The see function is the agent's ability to observe its environment, while the action function represents the agent's decision making process. The output of the **see function** is a **percept** which maps environment states to percepts:

$$
    see: E \to Per
$$

The **action function** is now a function which maps sequences of percepts to actions:

$$
    action: Per^* \to A
$$

We now consider agents that maintain state. These agents have some internal data structure, which is typically used to **record information about the environment state and history**. Let $I$ be the set of all internal states of the agent. The perception function $see$ for a state-based agent is unchanged:

$$
    see: E \to Per
$$

The action-selection function $action$ is now defined as a mapping from **internal states** to **actions**. 

$$
    action: I \to Ac
$$

An additional function $next$ is introduced, which maps an **internal state and percept** to an **internal state**:

$$
    next: I \times Per \to I
$$

The control loop for a state-based agent is as follows:
1. The agent starts in some initial internal state $i_0$.
2. The agent observes its environment state $e$ and generates a percept $see(e)$.
3. The internal state of the agent is then updated via the $next$ function, becoming $next(i_0, see(e))$.
4. The action selected by the agent is $action(next(i_0, see(e)))$.
5. Go to 2.

<div style="text-align: center;" class="mermaid">
graph LR
  classDef system fill:#EEE,stroke:#333,stroke-width:2px,rx:10;
  classDef environment fill:#EEF,stroke:#333,stroke-width:2px,rx:10;

  See[See]:::system
  Action[Action]:::system
  Environment[Environment]:::environment
  InternalState[Internal State]:::system
  Next[Next]:::system

  Environment --> See
  See --> InternalState
  InternalState --> Next
  Next --> InternalState
  InternalState --> Action
  Action --> Environment
</div>

## Task Evaluation

We build agents in order to carry out tasks for us. And task must be specified by us. Ideally we want to tell agents **what to do** without telling them **how to do it**.

One way to do this is to associate **utilities** with **individual states** — the task of the agent is then to **bring about states that maximize utility**:

$$
    u: E \to \mathbb{R}
$$

But when considering the value of a run based on the **utility over states**. It is hard to say what the **utility** of a run is. Maybe like follows:
- minimum utility of state on run
- maximum utility of state on run
- sum of utilities of states on run
- average of utilities of states on run

They are difficult to **specify a long term view** when assigning utilities to individual states! A more practical way is to assign a **utility** not to individual states, but to **runs themselves**. Such an approach takes an inherently long term view.

$$
    u: R \to \mathbb{R}
$$

However, we still have some difficulties with this approach. We don’t think in terms of utilities, so it is hard to formulate tasks in these terms. Next, we will define more terminology:

**Probability** $P(r \| Ag, Env)$ that run $r$ occurs when agent $Ag$ is placed in environment $Env$:

$$
    \sum_{r \in R(Ag, Env)} P(r | Ag, Env) = 1  
$$

Then **optimal agent** $Ag_{opt}$ in an environment $Env$ is the one that maximizes **expected utility**:

$$
    Ag_{opt} = \arg \max_{Ag \in \mathcal{AG}} \sum_{r \in R(Ag, Env)} u(r) P(r | Ag, Env)
$$

But some agents cannot be implemented on some computers (some of them maybe need more than available memory). Hence, we Write $\mathcal{AG}_{m}$ to denote the **agents that can be implemented on machine** $m$:

$$
    \mathcal{AG}_m = \{Ag \in \mathcal{AG} \text{ and } Ag \text{ can be implemented on } m \}
$$

Then we can modify the definition of **optimal agent** to be **bounded optimal agent**:

$$
    Ag_{opt} = \arg \max_{Ag \in \mathcal{AG}_m} \sum_{r \in R(Ag, Env)} u(r) P(r | Ag, Env)
$$

A special case of assigning utilities to histories is to assign $0$ (false) or $1$ (true) to a run. If a run is assigned 1, then the agent succeeds on that run, otherwise it fails. Call these **predicate task specification**s $\Psi$:

$$
    \Psi: R \to \{0, 1\}
$$

A **task environment** is a pair $\langle Env, \Psi \rangle$ where $E$ is the environment and $\Psi$ is the task specification. It specifies the properties of the system the agent will inhabit and the criteria by which an agent will be judged to have either failed or succeeded.

$R_{\Psi} (Ag, Env)$ to denote set of **all runs** of the agent $Ag$ in environment $Env$ that **satisfy** $\Psi$:

$$
    R_{\Psi} (Ag, Env) = \{r \in R(Ag, Env) \mid \Psi(r) = 1\}
$$

Say that an agent $Ag$ **succeeds** in task environment $\langle Env, \Psi \rangle$ if:

$$
    R_{\Psi} (Ag, Env) = R(Ag, Env)
$$

And let $\mathcal{TE}$ be the set of **all task environments**:

$$
    \mathcal{TE} = \{ \langle Env, \Psi \rangle \mid \Psi: R \to \{0, 1\} \}
$$

Typically, we have the most common types of tasks which are **achievement tasks** and **maintenance tasks**. Achievement tasks are those of the **form achieve state of affairs** $\phi$. Maintenance tasks are those of the **form maintain state of affairs** $\psi$.

An **achievement task** is specified by a set $G$ of "good" or "goal" states: $G \subseteq E$. The agent succeeds if it is **guaranteed to bring about at least one of these states** (we do not care which one it is, they are all considered equally good).

A **maintenance goal** is specified by a set $B$ of "bad" states: $B \subseteq E$. The agent succeeds in a particular environment if it **manages to avoid all states in** $B$ - if it never performs actions which result in any state in B occurring.

**Agent synthesis** is **automatic programming**. The goal is to have a program that will take a task environment, and from this task environment automatically **generate an agent that succeeds in this environment**:

$$
    syn: \mathcal{TE} \to (\mathcal{AG} \cup \{\perp\})
$$

**Synthesis algorithm** $syn$ is **sound** if whenever it returns an agent, then this agent **succeeds in the task environment** that is passed as input:

$$
    syn(\langle Env, \Psi \rangle) \implies R (Ag, Env) = R_{\Psi} (Ag, Env)
$$

and **complete** if it is **guaranteed to return an agent** whenever there exists an agent that will succeed in the task environment given as input:

$$
    \exists Ag \in \mathcal{AG} \text{ s.t. } R (Ag, Env) = R_{\Psi} (Ag, Env) \implies syn(\langle Env, \Psi \rangle) \neq \perp
$$

---

Related Posts / Websites 👇

📑 [Ray - NTU SC4003 Lecture 4 Note: Agent Decision Making](/Agent-Decision-Making)

📑 [Ray - NTU SC4003 Lecture 5 Note: Agent Architecture](/Agent-Architecture)