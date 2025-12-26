---
title: "Scattered Thoughts on Evals"
date: 2024-01-01
draft: false
tags: ["evals"]
categories: ["technical"]
description: "Some musings on the current state of evaluations of AI"
ShowToc: true
TocOpen: false
math: true
---

This post is mostly to jot down some thoughts on the evaluations space in AI right now. I'm going to write these points down as bullet points, so these are going to be more structured thoughts, than a cohesive article.

- *My mental model of evals: a conversation with model developers*: I (currently) view evaluations as a conversation between
model developers and the broader community. Roughly speaking, a benchmark is just a way to organize a complaint about the capabilities of current models. The cycle is essentially of the form: model developer releases model x.v1 -> academia and evaluators release benchmarks showing areas to improve on -> model developer uses these benchmarks to release x.v2 that performs better on these benchmarks, and so on. Of course, the other role these benchmarks play is to serve as a marker of the capabilities of the models, and can be used to make business decisions about their use, and larger societal decisions about how they integrate into the world around us.

- *It is cool that they correlate with anecdotal experience*: I find it cool that benchmark scores roughly correlate with or impressions using these models. Not always, but the trend is that they do. I find it a nice property of our current evaluations that one can infer from a 15-point gap in SWE-bench which model is likely better for coding, and that this is actually mostly a reasonable conclusion to make. There is a parallel universe where we did not have this property. Benchmarks that have this property (significantly higher score => model is better to use for that task) are quite useful.


- *Role of an evaluator*: The role of a neutral third-party evaluator is:
    -- as models get released, run evaluations on them and surface the results to the broader community in as neutral and useful a way as possible.
    -- create new evaluations that surface surprising aspects of the systems that are evaluated; this includes both deficiencies as well as 
    things that the systems are surprisingly good at. 
    -- help the broader AI community and society interprets the results of the evaluations. Right now, we (the AI community) run a bunch of benchmarks, as a result of which we essentially get a table of numbers. This table of numbers is basically gibberish to the average person trying to get a sense of how good these systems are. Evaluators can help here by being the bridge between these numbers and the consumers of these numbers. 
    -- analyze and surface insights about aspects of the AI systems other than the raw evaluation scores; this includes things like cost, latency, interesting failure modes, whether the model weights, training/inference code, and scaffolding code is available publicly, and other such information.
    -- Make the traces corresponding to these evaluations public, and release tooling to be able to usefully egage with these traces
    -- Be transparent about the infrastructure used to evaluate models (endpoints, inference code, evaluation settings, scaffolding used, etc.)
    -- 


- *What paradigms are currently missing from evaluation today? What is the longer term way to evaluate AI systems?*: 


- *When we run evaluations on AI models, what are we actually measuring?*







