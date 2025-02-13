---
layout: post
title: "Deep Neural Networks"
description: "This post provides an overview of contents covered in the SC4001 course in NTU, including Deep Neural Networks."
date: 2025-02-13
feature_image: images/deep-neural-network.jpg
tags: ['deep-learning']
---

Deep Neural Networks (DNNs) are artificial neural networks with many hidden layers. They transform inputs through these layers to learn complex features from data. DNNs excel in tasks like image and speech recognition, and natural language processing. Their success comes from learning from large datasets and using backpropagation to reduce errors. With more computing power and data, DNNs have become essential in deep learning. This post summarizes the `SC4001` course at NTU, offering an overview of DNNs based on course notes and my insights.

<!--more-->

## Table of Contents
- [Chain Rule for Backpropagation of Gradients](#chain-rule-for-backpropagation-of-gradients)
  - [Chain Rule of Differentiation](#chain-rule-of-differentiation)
- [Training Deep Neural Networks](#training-deep-neural-networks)
  - [Normalization of Inputs](#normalization-of-inputs)
  - [Normalization of Outputs](#normalization-of-outputs)
  - [Width of Hidden Layers](#width-of-hidden-layers)
  - [Depth of Deep Neural Networks](#depth-of-deep-neural-networks)

---

# Chain Rule for Backpropagation of Gradients

## Chain Rule of Differentiation

Let $x, y$, and $J \in \mathbf{R}$ be one-dimentional variables, such that:

$$
    J = g(y), \quad y = f(x)
$$

Chain rule can be written as:

$$
    \frac{dJ}{dx} = \frac{dJ}{dy} \cdot \frac{dy}{dx} \text{ or } \frac{\partial J}{\partial x} = \frac{\partial J}{\partial y} \cdot \frac{\partial y}{\partial x}
$$

<div style="text-align: center;" class="mermaid">
graph LR
    x --> y
    y --> J
    J --> y
    y --> x
    J --> x
</div>

---

# Training Deep Neural Networks

## Normalization of Inputs

If inputs have **similar variations**, better approximation of inputs or prediction of outputs is achieved. Mainly, there are two approaches to normalization of inputs.

Suppose $i$th input $x_i \in [x_{i,min}, x_{i,max}]$ and has a mean $\mu_i$ and a standard deviation $\sigma_i$. If $\widetilde{x}$ denotes the normalized input, two approaches are:

- **Scaling** the inputs such that $\widetilde{x} \in [0, 1]$:

$$
    \widetilde{x}_i = \frac{x_i - x_{i, min}}{x_{i, max} - x_{i, min}}
$$

- **Normalization** the inputs such that $\widetilde{x} \sim \mathcal{N}(0, 1)$:

$$
    \widetilde{x}_i = \frac{x_i - \mu_i}{\sigma_i}
$$

## Normalization of Outputs

The normalization of outputs depends on the final activation function used.

For linear activation function, the convergence is usually improved if each output is normalized to have zero mean and unit standard deviation: 

$$
    \widetilde{y}_k = \frac{y_k - \mu_k}{\sigma_k}
$$

For sigmiod activation function, I think the normalization is not that necessary. But we can till scale $\widetilde{y}_k \in [0, 1]$:

$$
    \widetilde{y}_k = \frac{y_k - y_{k,min}}{y_{k,max} - y_{k,min}}
$$

## Width of Hidden Layers

The width of hidden layers has a significant impact on the performance of the network. With more parameters, the network is capable of remembering the training patterns better, but it may also lose its generalization ability on unseen data. As a result, the test error may decrease initially with more hidden units, but it may also increase beyond a certain point. Determining the optimal number of hidden units is often an empirical process, requiring trial and error.


## Depth of Deep Neural Networks

The depth of a deep neural network is dependent on the amount of available training data. Deeper networks have more parameters (weights and biases) to learn, and thus require more data to train. With sufficient training data, deeper networks can learn complex mappings more accurately. The optimal number of layers is often determined empirically, by finding the architecture that minimizes the error (training, testing and validation).

---