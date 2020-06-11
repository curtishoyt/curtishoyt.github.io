---
layout: post
title: Programming Advice
date: "2020-06-11 11:00"
description: This is literally everything I've learned about programming.
---

This is literally everything I've learned about programming.

## General Advice

- **Constants** (Immutable, Irreplaceable) > **Caches** (Immutable) > **State** (Mutable)
- **Data** (No Side-effects, No Cost) > **Pure functions** (No Side-effects) > **Procedures**
- **Static analysis** (Cannot Be Ignored) > **Linting** (Hard To Ignore) > **Advice**
- **Data models** > **Data structures** > **Algorithms**
- **Correct** > **Clear** > **Concise** > **Fast**
- Use as few frameworks, libraries, and language features as possible.
- Consider scalability in as many dimensions as possible.
- Aim for simplicity by using a small number of orthogonal concepts.
- 3 is usually a sufficient number of parameters.
- Design abstractions with everyone but yourself in mind.

## Frontend Advice

- **Icons** (Cheap, I18n) > **Images** (I18n) > **Text**
- **Inline errors** > **Tooltips** (Require hovering) > **Alerts** (Intrusive)
- Every user action must be met with some kind of feedback.
- Keep libraries and components completely isolated from the main codebase.
- Stick to one form of asynchrony.
- Priority and modularity will always conflict with one another.
- Preserve the user's work at all costs.
- Data-binding is the root of all evil.
- Feature creep is, somehow, also the root of all evil.
- Keeping the UI in sync with the underlying data model is hard.
