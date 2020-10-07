---
layout: post
title: Order of Optimization
date: "2020-10-06 19:00"
description: Order of Optimization.
---

A suggestion of priorities in optimization.

- Correctness
- Easily understood by other programmers
- Minimizes code written
- Sufficiently performant

This is, however, only a starting point - these priorities should be driven by the context in which the code is written.

For example, code written with the expectation that it will be read many thousands of times might prioritize clarity, which might also drive the choice of language, framework, etc.

Similarly, code written against strict deadlines with the expectation of high turnover might prefer to optimize ease of coding.
