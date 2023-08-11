---
layout: post
title: Generative AI Config
date: "2023-08-11 15:00"
description: A spec for Generative AIs
---

A spec for Generative AIs

Has anyone developed a file format specifically designed to be read by Generative AIs? "Goals" and "Constraints" as the core concepts, "Variants" for when the AI needs to run the same "Generative Operation" multiple times until it hits some threshold on the Objective Function. Variants could be generated easily, either by perturbing the prompt or iterating more on a single prompt, which could translate easily to a concurrency model.
