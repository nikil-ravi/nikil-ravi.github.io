---
title: "Information Theory"
date: 2024-01-14
draft: true
tags: ["information-theory", "mathematics", "computer-science"]
categories: ["technical"]
description: "Fundamental concepts in information theory including entropy and self-information"
ShowToc: true
TocOpen: false
math: true
---

# Introduction
Information theory assumes that all information can be represented using *bits*; a bit consists of either a 0 or a 1. We'd like ways to measure information, and also want techniques to represent and communicate it efficiently. In this post, we'll be going over fundamental, well-established notions in information theory.

# Measures of Information

Intuitively, a good measure of information would tell us only what we don't know- the idea is that if you only saw what you already knew, then the content wasn't very informative. In other words, a measure of information should take into account how "surprising" the information is. 

In order to formalize the surprising-ness of information, we will first need to define a prior distribution with respect to which we define this quantity.

Suppose that $U$ is a random variable that may take on values in $\mathbb{U} := \{1, 2, \cdots r\}$. Note the subtle difference in notation between them- this is going to be used throughout.

## Self-information

The smaller the probability of $u \in \mathbb{U}$, the more surprised we'd be if that value were actually realized by $U$. 

This is formalized by the notion of **self-information**, also known as the "surprise function" or simply as the "log loss".

$$s(u) := \log \frac{1}{p(u)}$$

## Entropy

The **entropy** of $U$ is defined as the expected surprise over all the possible realizations of $U$.

$$H(U) := \mathbb{E}_{U}[s(u)]$$

This can be expanded out to its most canonical form:

$$H(U) := \sum_{u = 1}^{r} p(u) \cdot log \frac{1}{p(u)}$$

Like other measures of quantities, entropy also has units- we refer to the units as **bits**/symbol for b = 2, **nats**/symbol for b = e, and **bans**/symbol for b = 10.

## Joint Entropy

## Conditional Entropy

## Mutual Information

Consider the scenario in which there are two random variables $U$ and $V$. Suppose you have no prior knowledge about the values of these random variables. In this case the joint entropy $H(U, V)$ gives you the amount of information required to describe both of them. 

Alternatively, let's say that you already know the value of $U$ but not that of $V$. In this case, the amount of additional information required to also describe $V$ is the conditional entropy $H(V | U)$.

From the chain rule, we know that $H(U, V) = H(U) + H(V | U)$. Therefore, knowing $U$ reduces the amount of information required to describe $V$ by exactly $H(V) - H(V | U)$. This quantity is called the mutual information:

$$I(U; V) := H(V) - H(V | U)$$

Mutual information is always non-negative; this aligns with our intuition that knowing one random variable should never increase the surprise (i.e., required information) of another.


