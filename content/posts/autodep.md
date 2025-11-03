---
title: "Diagnosing autoformalizations via implication graph checks"
date: 2025-11-03T00:00:00-05:00
draft: true
tags: ["lean", "theorem-prover", "artificial-intelligence", "formal-verification"]
categories: ["technical"]
description: "An approach that helps catch misformalizations"
ShowToc: true
TocOpen: false
math: true
---

# Introduction

Autoformalization is the process of converting informal statements into formal programs automatically. While this in itself is not a very precise definition, in essence it refers to the process of removing ambiguity from a statement. There are multiple reasons why autoformalization is important: for instance, in order to accelerate mathematical advancement via artificial intelligence, one needs to be able to automatically (formally) state problems one is interested in. It is also likely that improved autoformalization capabilities will help us make progress with the issues surrounding AI safety- in order to see this, readers are urged to reframe the problem of autoformalization as the problem of trying address the following issue: *"Can a sufficiently motivated adversary deliberately or accidentally misinterpret a natural language statement that is given to it?"*. Formalization gives us a mechanism to guarantee only one possible interpretation of a statement, and therefore is likely to be a useful accelerant in finding good solutions to the problem of aligning AI systems.

In recent years, work in autoformalization has increasingly leveraged large language models (LLMs), and systems that scaffold LLMs, to achieve improvements in how well we can autoformalize statements. 

These advances, while wonderful, still do not address one of the main bottlenecks of autoformalization-- how can we evaluate an autoformalized statement? Unlike automated theorem-proving, or even many coding tasks, where we can have a clear and unambiguous success signal, autoformalization does not come with any such simple metric of success. In other words, given a natural language statement $s$, and an autoformalization system $A(\cdot)$, that produces a formalization $f$, it is not clear what it means to conclude that $f$ is correct or incorrect. Currently, humans simply manually verify the formalization.

One way to help diagnose issues is to observe that autoformalization is often done one not just a single statements but multiple related statements (eg. a chapter of a book, a mathematical research paper, etc.). When autoformalizing multiple statements at a time instead of just one, we can leverage dependencies between statements to identify potential mis-formalizations.

Concretely, suppose we have two informal statements, say $s_1$ and $s_2$, with formalizations $f_1 = A(s_1)$ and $f_2 = A(s_2)$. Suppose also that we know (as an informal statement) that one implies the other (say $s_1 \Rightarrow s_2$). Then, if we are able to find a proof that $f_1$ does not necessarily imply $f_2$, then we know that at least one of the formalizations must be incorrect. While this does not say which of them is incorrect, it can serve as a useful mechanism for users of an autoformalization system to (a) have increased confidence in the system, and (b) to identify incorrect formalizations of the form above.

An example of the intended approach:

```lean
import Mathlib.Data.Nat.Prime.Basic
import Mathlib.Data.Nat.Basic
import Mathlib.Tactic

/-- Mis-formalized version of Euclid's lemma dropping the `Prime p` assumption. -/
def A_bad (p a b : ℕ) : Prop :=
  p ∣ a * b → (p ∣ a ∨ p ∣ b)

/-- Derived statement B: if p divides a * b and p does not divide a, then p divides b. -/
def B_bad (p a b : ℕ) : Prop :=
  p ∣ a * b → ¬ (p ∣ a) → (p ∣ b)

/-- There exist p, a, b such that both A_bad and B_bad fail. This shows that there's a misformalization in one of the statements above -/
theorem counterexample_A_bad_and_B_bad :
  ∃ (p a b : ℕ), ¬ A_bad p a b ∧ ¬ B_bad p a b := by
  -- choose p = 4, a = 2, b = 2
  use 4, 2, 2
  have hdiv : 4 ∣ (2 * 2) := by norm_num
  -- show ¬ A_bad 4 2 2
  have hNotA : ¬ A_bad 4 2 2 := by
    rw [A_bad]
    intro h
    have : 4 ∣ 2 ∨ 4 ∣ 2 := h hdiv
    norm_num at this
  -- show ¬ B_bad 4 2 2
  have hNotB : ¬ B_bad 4 2 2 := by
    intro hB
    -- hB : 4 ∣ (2 * 2) → ¬ (4 ∣ 2) → 4 ∣ 2
    have h₂ := hB hdiv (by norm_num)
    -- h₂ : 4 ∣ 2, but 4 ∤ 2
    norm_num at h₂
  exact ⟨hNotA, hNotB⟩
```