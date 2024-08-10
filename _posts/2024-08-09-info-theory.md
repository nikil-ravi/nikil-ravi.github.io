---
layout: post
title: Notes on information theory
date: 2024-08-10
description: info theory fundamental notions and theorems, in a concise form
tags: math notes
categories: notes
---

# Introduction
Information theory assumes that all information can be represented using *bits*; a bit consists of either a 0 or a 1. We'd like ways to measure information, and also want techniques to represent and communicate it efficiently. In this post, we'll be going over fundamental, well-established notions in information theory.

# Measures of Information

Intuitively, a good measure of information would tell us only what we don't know- the idea is that if you only saw what you already knew, then the content wasn’t very informative. In other words, a measure of information should take into account how "surprising" the information is. 

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

TODOShannons, Hartleys

We will also generalize $H(U)$ by defining $H_q(U)$; for a PMF $q$, which is possibly equal to $p$, $%$H\_q(U)$ := $\sum_{u = 1}^{r} p(u) \cdot log \frac{1}{q(u)}$$.  Intuitively, $H_q$ measures how surprised you are if you thought that $U$ comes from distribution $q$, but it was actually from $p$. 

When $q = p$, we these quantities are equal ($H_q = H$); in fact, we can even go further and say that $H(U) \leq H_q(U)$, with equality iff $q = p$ (the proof is tied to the notion of relative entropy, which immediately follows this section). All this is saying is that you're probably going to be more surprised if you got the distribution wrong.

## Relative Entropy

Now that we introduced a distribution $q$, it is useful to define a version of entropy that is *relative*, instead of absolute. This notion, **relative entropy**, can also be thought of as a measure of the divergence of the two distributions- in other words, how different is $q$ with respect to $p$?

$$\begin{align}
D(p||q) &= H_q(U) - H(U)\\
        &= \mathbb{E}[\log \frac{1}{p(u)} - \log \frac{1}{q(u)}]\\
        &= \sum_{u=1}^{r} p(u) \cdot \left[ \log \frac{1}{p(u)} - \log \frac{1}{q(u)} \right]\\
        &= \sum_{u=1}^{r} p(u) \cdot \left( \log \frac{q(u)}{p(u)} \right)

\end{align}$$

This quantity is also known as the Kullback-Leibler Divergence, or simply KL Divergence. Note that it is not symmetric; $D(p ||q)$ is not equal to $D(q||p)$ unless $q = p$. This is why we refer to it as a *divergence*, and not a distance in the sense of a metric.

The entropy of a random variable $U$ is upper-bounded by $\log r$ (proof idea: apply Jensen's inequality, since we have an expectation and a log).

## Joint Entropy

## Mutual Information
lol

# Typicality and the Asymptotic Equipartition Property

Now that we've defined some basic notions of information and its measurement, we can ask questions of the following kind: what are the characteristics of unsurprising information? What kinds of sequences occur typically?

An answer to this question emerges in the notion of *$\epsilon$-typical sequences*.

**Definition of $\epsilon$-typical sequence:**
- A source sequence ( U^n ) is ( epsilon )-typical if
$$\left|\frac{-1}{n} \log P(U^n) - H(U)\right| \leq \epsilon.$$

Let $A_\epsilon^{(n)}$ denote the “$\epsilon$-typical set”, that is the set of all source sequences $U^n$ that are $\epsilon$-typical.

By some simple algebraic rearrangements, we arrive at n equivalent formulation of $\epsilon$-typicality. Sometimes, this is a useful way to think about it. What it says is that the joint probability of the whole sequence is approximately $2^{-n \cdot H(U)}$

$$2^{-n(H(U) + \epsilon)} \leq P(U^n) \leq 2^{-n(H(U) - \epsilon)}.$$

# Variable Length Lossless Compression

## Asymptotic Equipartition Property (AEP) Review

For a memoryless source U₁, U₂, ..., Uₙ ~ iid U ∈ 𝕌, we have:

- P(Uⁿ ∈ A^(n)\_ε) ≈ 1
- ∀uⁿ ∈ A^(n)\_ε: p(uⁿ) ≈ 2^(-nH(U))
- |A^(n)\_ε| ≈ 2^(nH(U))

This implies that H(U) bits per source symbol are needed for a near-lossless fixed length scheme.

## Near-lossless Fixed-length Compression

### Encoder & Decoder Design

Two important theorems:

1. **Direct theorem**: ∀R \> H(U) and ∀δ \> 0, ∃ a large enough N and a scheme with rate r \< R and P\_e \< δ.

2. **Converse theorem**: If R \< H(U) then for all sequences of schemes with rate r ≤ R, P\_e → 1 as n → ∞.

Drawbacks of this framework:
1. Nonzero error probability
2. Inefficient enumeration and searching of an exponentially large set
3. Large block length N introduces significant delay

## Variable Length Lossless Compression

### Prefix Codes

A prefix code is a code where no codeword is the prefix of any other. Properties:
- Uniquely decodable
- Can be decoded efficiently and on the fly

### Dyadic Distributions

A source is dyadic if p(u) = 2^(-n\_u), where n\_u is an integer ∀u ∈ 𝕌.

#### Procedure for Constructing Prefix Codes for Dyadic Sources:

1. Choose 2 symbols with n\_u = n\_max and merge them into a single symbol.
2. Repeat until left with just one symbol.

This procedure induces a binary tree where codewords are the leaves.

### Shannon Codes

For a general source, define n\*\_u = ⌈log(1/p(u))⌉ ∀u ∈ 𝕌.

Steps to construct Shannon code:
1. Create a new source p*(u) = 2^(-n*\_u)
2. Extend to 𝕌\* ⊇ 𝕌 such that p*(u) is dyadic over 𝕌*
3. Construct a prefix code for p\* using the dyadic source procedure

Properties of Shannon code:
- l(u) = n\*\_u = ⌈log(1/p(u))⌉ ∀u ∈ 𝕌
- Expected length: l ≤ H(U) + 1

To get closer to entropy, work with blocks of source symbols:

For U₁, U₂, ... iid ~ U, construct a Shannon code for Uᴺ = (U₁, U₂, ..., Uₙ).
Then (1/N)E[l(Uᴺ)] ≤ H(U) + 1/N

As N becomes large, the bits used per source symbol approach the entropy.

# Huffman Codes

Huffman coding is an optimal prefix coding method that minimizes the expected codeword length.

## Huffman Coding Algorithm

1. Start with a set of symbols and their probabilities
2. Take the two symbols with lowest probabilities
3. Merge them into a new symbol with combined probability
4. Repeat until only one symbol remains
5. Trace back to assign 0 and 1 to each branch

## Properties of Huffman Codes

1. Optimal: Minimizes E[L] among all prefix codes
2. Near-optimal: E[L] \< H(X) + 1
3. If source probabilities are negative powers of 2, Huffman code achieves H(X) exactly

# Kraft-McMillan Inequality

For any uniquely decodable code with codeword lengths {l\_i},

Σ 2^(-l\_i) ≤ 1

Conversely, given a set of lengths satisfying this inequality, there exists a prefix code with these lengths.

## Implications

- Provides necessary and sufficient conditions for the existence of a uniquely decodable code
- Links code design to probability distributions
- Useful in proving optimality of certain codes

# Source Coding Theorem for Variable-Length Codes

For a discrete memoryless source X with entropy H(X):

1. For any uniquely decodable code, E[L] ≥ H(X)
2. There exists a code with E[L] \< H(X) + 1

This theorem establishes H(X) as the fundamental limit for lossless compression, even with variable-length codes.

# Universal Source Coding

Universal source coding aims to compress data without prior knowledge of the source distribution.

## Lempel-Ziv Algorithm

A popular universal source coding method:
1. Parse the input into unique phrases
2. Replace phrases with (pointer, length) pairs
3. Asymptotically achieves the entropy rate for stationary ergodic sources

Properties:
- Adaptive: Learns the source statistics on the fly
- Simple to implement
- Widely used in practice (e.g., gzip, PNG)

# Rate-Distortion Theory

Extends source coding to lossy compression, where some distortion is allowed.

## Key Concepts

- Distortion measure d(x,x̂): Quantifies the cost of representing x by x̂
- Rate-distortion function R(D): Minimum rate required to achieve average distortion D
- Shannon's Rate-Distortion Theorem: Characterizes the fundamental trade-off between rate and distortion

This framework provides the theoretical basis for modern lossy compression techniques used in image, audio, and video coding.
