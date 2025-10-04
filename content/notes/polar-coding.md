---
title: "Channel Polarization: A Method for Constructing Capacity-Achieving Codes for Symmetric Binary-Input Memoryless Channels"
date: 2025-10-03T12:00:00-07:00
draft: false
tags: ["information theory", "paper"]
description: "[IEEE Transactions on Information Theory, 2009]"
ShowToc: false
math: true
---

**Author:** Erdal Arıkan  
**Publication:** IEEE Transactions on Information Theory, 2009  
**Link:** [IEEE Xplore](https://ieeexplore.ieee.org/document/4787611)

---

This paper introduces **polar codes**, the first explicit and efficient code construction that achieves Shannon capacity for binary-input symmetric memoryless channels.

The main idea is **channel polarization**: by combining channels with a simple linear transform and applying the process recursively, one produces synthetic sub-channels that become either nearly perfect (capacity ≈ 1) or nearly useless (capacity ≈ 0)- hence the term polarization.  

Encoding is based on repeated Kronecker products of the matrix
\[
G_2 = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix},
\]
leading to a circuit with \(O(N \log N)\) complexity. Decoding is carried out via **successive cancellation**, where each bit is decoded sequentially using previously decoded bits to improve reliability.  

The intuition here is that the capacity is conserved, but we redistribute it using this construction such that the channels become polarized. This means that we eventually have sub-channels that have high capacity.

In particular, the result is that as \(N\) grows, the achievable rate approaches the channel capacity \(C(W)\) and the error probability vanishes. This provides a constructive and computationally efficient answer to Shannon’s existential theorem on reliable communication.  

Polar codes have also become practically important: they were adopted into the 5G wireless standard for control channels.
