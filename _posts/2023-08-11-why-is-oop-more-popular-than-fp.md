---
layout: post
title: Why is OOP More Popular Than FP?
date: "2023-08-11 13:30"
description: Just look at human languages.
---

Just look at human languages.

There are lots of (S, V, O) and (S, O, V) human languages, but very few (V/O, *, *) languages - at least, few contemporary ones. Since OOP languages (especially Java) tend to read like human languages: `Subject Verb Object = Foo.bar(baz)`, they can be picked up more easily.

However, in many Functional Programming languages, functions (verbs) are typically elevated to first-class citizens, meaning (especially in Lisps) you often see large functions that start with `Verb Subject Object = (def x y...)`. And the part of our brain that handles language will get tripped up whenever it sees something like that, because it starts with a verb.

In order to get past it, you have to retrain your brain by studying languages that AREN'T human languages - mathematical formalism, or functional programming languages, or whatever. I don't know if it's a coincidence or causality either way that functional programming gurus tend to have deeply studied some branch of mathematics, but it would certainly add up.
