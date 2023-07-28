---
layout: post
title: Coding Advice
date: "2022-1-25 17:00"
description: Coding Advice
---

Here is roughly everything I've learned over 20 years of programming.

### General Advice
- Constants (Immutable, Irreplaceable) > Caches (Immutable) > State (Mutable)
- Data (No Side-effects, No Cost) > Pure functions (No Side-effects) > Procedures
- Static analysis (Cannot Be Ignored) > Linting (Hard To Ignore) > Testing
- Data models > Data structures > Algorithms
- Correct > Clear > Concise > Fast
- Consider scalability in as many dimensions as possible.
- Aim for simplicity by using a small number of orthogonal concepts.
- 3 is usually a sufficient number of parameters.
- Design abstractions with everyone but yourself in mind.
- Each significant piece of functionality in a program should be implemented in just one place in the source code.

### Frontend Advice
- Icons (Cheap, I18n) > Images (I18n) > Text
- Inline errors > Tooltips (Require hovering) > Alerts (Intrusive)
- Every user action must be met with some kind of feedback.
- Keep libraries and components completely isolated from the main codebase.
- Preserve the user's work at all costs.
- Feature creep is bad.
- Keeping the UI in sync with the underlying data model is hard.
