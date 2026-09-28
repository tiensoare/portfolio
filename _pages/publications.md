---
layout: page
permalink: /publications/
title: research
description: Making AI a trustworthy partner for building software. I publish as <code>Tien Nguyen</code>.
nav: true
nav: true
nav_order: 1
---

### Research Statement

AI models like ChatGPT and Claude now help write a huge share of the world's code. But they make mistakes, sometimes subtle ones that slip past even experienced developers. My research asks three questions: Where do these models fail when we trust them with software? Why do they fail? And how can we make them more reliable?

**Why numbers are surprisingly hard for AI.** An AI model doesn't read 3.141592653589793 the way you do. It breaks the number into small text fragments, much like it breaks up words. For everyday tasks, that works fine. But scientific software, like weather forecasts, medical imaging, and financial models, depends on numbers being precise down to the last digit, and a tiny rounding error can snowball into a completely wrong answer. I study how the way numbers are written and split up affects an AI's ability to reason about them, and I am working toward teaching models to read numbers more like mathematicians do.

**Putting AI to work on software reliability.** I build tools that use AI to catch bugs before users do. For example, I have AI generate tests that push programs right up to the edge cases where they are most likely to break.

**Learning from real-world failures.** Good tools start with understanding real problems. I analyzed more than 42,000 computational notebooks, the interactive documents data scientists use to write and run code, to understand why they crash. This work was published and presented at MSR 2025.

I believe in open science, so I publicly release and maintain the code behind all of my projects.

### Talks

- **Are the Majority of Public Computational Notebooks Pathologically Non-Executable?**, MSR 2025, Ottawa, Canada. [[video](https://www.youtube.com/watch?v=dAaLlh7dFxY)]
- **Code Repair for Unstable Numerical Programs with LLMs**, Virginia Tech CCI Student Symposium 2025, Blacksburg, VA. [[slides](https://docs.google.com/presentation/d/12hHQEA_0DFD43L3z2vHa02NqNaB40wL6/edit?usp=sharing&ouid=115718852536365943565&rtpof=true&sd=true)]


### Publications

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

{% bibliography %}

</div>
