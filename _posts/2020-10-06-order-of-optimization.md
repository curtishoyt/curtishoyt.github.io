---
layout: post
title: Order of Optimization
date: "2020-10-06 19:00"
description: Order of Optimization.
---

A suggestion of priorities in optimization.

1. Joy - The mood of the programmer who writes (or reads) a piece of code.
1. Correctness - The chance a piece of code does what it's expected to do.
1. Coherence - Minimizing the time it takes others to read your code.
1. Maintainability - The development cost of a piece of code after it's committed.
1. Brevity - The art of turning two lines of code into one.
1. Performance - Minimizing the cost of a program without changing its behavior.

This is, however, only a starting point - these priorities should be driven by the context in which the code is written.

For example, code written with the expectation that it will be read many thousands of times might prioritize clarity, which might also drive the choice of language, framework, etc.

Similarly, code written against strict deadlines with the expectation of high turnover might prefer to optimize ease of coding.
