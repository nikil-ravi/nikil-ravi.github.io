---
layout: post
title: Information Theory
date: 2024-01-14 10:14:00-0400
description: 
tags: formatting toc sidebar
categories: sample-posts
giscus_comments: true
related_posts: false
toc:
  sidebar: left
---

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>


This post shows how to add a table of contents as a sidebar.

# Introduction
Information theory assumes that all information can be represented using *bits*; a bit consists of either a 0 or a 1. We'd like ways to measure information, and also want techniques to represent and communicate it efficiently. In this post, we'll be going over fundamental, well-established notions in information theory.

# Measures of Information

Intuitively, a good measure of information would tell us only what we don't know- in other words, a measure of information should take into account how "surprising" the information is.

Suppose that $$U$$ is a random variable that may take on values in $$\mathbb{U} := \{1, 2, \cdots r\}$$. 

## Self-information

The smaller the probability of $$u \in \mathbb{U}$$, the more surprised we'd be if that value were actually realized by $$U$$. 

This is formalized by the notion of **self-information**, also known as the "surprise function" or simply as the "log loss".

$$s(u) := \log \frac{1}{p(u)}$$

## Entropy

The **entropy** of $$U$$ is defined as the expected surprise over all the possible realizations of $U$.

$$H(U) := \mathbb{E}_{U}[s(u)]$$

This can be expanded out to its most canonical form:

$$H(U) := \sum_{u = 1}^{r} p(u) \cdot log \frac{1}{p(u)}$$

Like other measures of quantities, entropy also has units- we refer to the units as **bits**/symbol for b = 2, **nats**/symbol for b = e, and **bans**/symbol for b = 10.

\TODO{Shannons, Hartleys}

We will also generalize $$H(U)$$ by defining $$H_q(U)$$; for a PMF $$q$$, which is possibly equal to $$p$$, $$H_q(U)$ := $\sum_{u = 1}^{r} p(u) \cdot log \frac{1}{q(u)}$$.  Intuitively, $$H_q$$ measures how surprised you are if you thought that $$U$$ comes from distribution $$q$$, but it was actually from $p$. 

When $$q = p$$, we these quantities are equal ($$H_q = H$$); in fact, we can even go further and say that $$H(U) \leq H_q(U)$$, with equality iff $$q = p$$ (the proof is tied to the notion of relative entropy, which immediately follows this section). All this is saying is that you're probably going to be more surprised if you got the distribution wrong.

## Relative Entropy

Now that we introduced a distribution $q$, it is useful to define a version of entropy that is *relative*, instead of absolute. This notion, **relative entropy**, can also be thought of as a measure of the divergence of the two distributions- in other words, how different is $$q$$ with respect to $$p$$?

$$
\begin{align}
D(p||q) &= H_q(U) - H(U)\\
        &= \mathbb{E}[\log \frac{1}{p(u)} - \log \frac{1}{q(u)}]\\
        &= \sum_{u=1}^{r} p(u) \cdot \left[ \log \frac{1}{p(u)} - \log \frac{1}{q(u)} \right]\\
        &= \sum_{u=1}^{r} p(u) \cdot \left( \log \frac{q(u)}{p(u)} \right)

\end{align}
$$

This quantity is also known as the Kullback-Leibler Divergence, or simply KL Divergence. Note that it is not symmetric; $$D(p ||q)$$ is not equal to $$D(q||p)$$ unless $$q = p$$. This is why we refer to it as a *divergence*, and not a distance in the sense of a metric.

The entropy of a random variable $$U$$ is upper-bounded by $$\log r$$ (proof idea: apply Jensen's inequality, since we have an expectation and a log).

## Joint Entropy

## Mutual Information
lol

# Typicality and the Asymptotic Equipartition Property

Now that we've defined some basic notions of information and its measurement, we can ask questions of the following kind: what are the characteristics of unsurprising information? What kinds of sequences occur typically?

An answer to this question emerges in the notion of *$\epsilon$-typical sequences*.

**Definition of $\epsilon$-typical sequence:**
- A source sequence \( U^n \) is \( \epsilon \)-typical if 
$$
  \left|\frac{-1}{n} \log P(U^n) - H(U)\right| \leq \epsilon.
$$

Let $A_\epsilon^{(n)}$ denote the “$\epsilon$-typical set”, that is the set of all source sequences $U^n$ that are $\epsilon$-typical.

By some simple algebraic rearrangements, we arrive at n equivalent formulation of $\epsilon$-typicality. Sometimes, this is a useful way to think about it. What it says is that the joint probability of the whole sequence is approximately $2^{-n \cdot H(U)}$

$$
 2^{-n(H(U) + \epsilon)} \leq P(U^n) \leq 2^{-n(H(U) - \epsilon)}.
$$

