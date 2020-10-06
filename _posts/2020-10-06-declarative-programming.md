---
layout: post
title: Declarative Programming
date: "2020-10-06 09:00"
description: Declarative Programming.
---

Declarative programming is coding without modifying non-local variables. This self-imposed limitation results in code that is:

- Less performant: Imperative code is faster, as it is a superset of declarative code. However, this can matter less than expected in practice, as code that starts as declarative can often be mostly optimized by adding a comparatively small amount of imperative code
- Easier to read: Declarative code is clearer and more concise, as restricting the scope of operations reduces the context which must be held in mind by the reader.
- Harder to write: Restricting the scope of execution usually entails re-thinking the program's structure, as imperative programming comes more easily to most programmers.
- More impressive: There is a prevailing view among programmers that imperative code is ugly, while declarative code is purer and more logically dense.
