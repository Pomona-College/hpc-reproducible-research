---
title: "Why Reproducibility Matters"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What is computational reproducibility?
- Why does reproducibility matter for science and for you?
- What are the personal benefits of reproducible research?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Define computational reproducibility and distinguish it from replicability
- Understand the three levels of reproducibility
- Recognize personal benefits of reproducible research practices

::::::::::::::::::::::::::::::::::::::::::::::::::

## What Is Computational Reproducibility?

Computational research has become the lifeblood of modern science. From genomics to climate modeling, from economics to physics, researchers rely on software to process data, run simulations, and draw conclusions. Yet there's a growing problem: **many computational results cannot be reproduced**.

Let's define what we mean carefully. There are several related but distinct concepts:

### Reproducibility vs. Replicability

- **Reproducibility** (sometimes called "computational reproducibility"): Can another researcher take your code, data, and documentation and get the same numerical results you got?
- **Replicability** (sometimes called "experimental replicability"): Can another researcher independently conduct similar experiments or observations and reach the same scientific conclusions?

Replicability is what most scientists think of as "reproducibility" in the traditional sense: it's about independent verification of findings. Computational reproducibility is a prerequisite for replicability but not sufficient by itself. You can reproduce code perfectly and still have the results be wrong due to flawed experimental design.

**This workshop focuses on computational reproducibility:** ensuring your code, data, and environment are documented and executable so others (or your future self) can run your exact analysis and get your exact results.

### Determinism vs. Documentation

Another distinction:

- **Deterministic reproducibility**: The code gives the exact same numerical results every time it runs (like sorting a list or computing a deterministic statistic)
- **Stochastic reproducibility**: The code involves randomness, but you've controlled the random seed so anyone can reproduce your random choices
- **Environmental reproducibility**: The code runs the same way regardless of where it's executed (different machines, operating systems, installed packages)

In research, you need all three. A stochastic algorithm that gives different results each time isn't useful. Code that works on your laptop but breaks on the cluster is a problem. Code that depends on a package you'll update next month might break later.

## Three Levels of Reproducibility

Let's think about reproducibility in practical layers:

### Level 1: Same Results (Numerical Reproducibility)

Someone follows your documentation, uses your code and data, and gets the same numbers you did.

**What you need:**
- Code (scripts, programs, Jupyter notebooks)
- Raw data
- Complete documentation of the analysis steps
- Specification of software versions
- The computing environment (what libraries, what versions)

### Level 2: Same Conclusions (Logical Reproducibility)

Someone understands your analysis methodology, adapts it to their own data or context, and reaches the same scientific conclusions.

**What you need:**
- Clear explanation of your methodology (written description, not just code)
- Justification for your choices (why this statistical test? why this cutoff value?)
- Understanding of limitations and assumptions
- Code that's understandable and well-commented (not just runnable)

### Level 3: Same Workflow (Process Reproducibility)

Someone can understand how you went from raw question to final figure, including the dead ends, iterations, and decision points.

**What you need:**
- Version control showing the history of your analysis
- Lab notebook or documented decisions explaining why you did things a certain way
- Scripts and code in a logical order matching your analysis progression
- Clear separation of exploration vs. final analysis

## Personal Benefits: Why Reproduce Your Own Work?

Beyond scientific integrity, there are selfish reasons to care about reproducibility:

### Your Future Self Will Thank You

Imagine: it's three years later. You're writing a grant application that references your published work. A reviewer asks for details about how you did that analysis. You open the folder... and find five scripts with unclear names, Excel files with manually adjusted numbers, and data files named `mouse_brain_backup`. You can't even recreate your own figure.

This happens to most researchers. The solution: **be the kind of colleague you'd want to collaborate with**. Document as if you're leaving your project behind tomorrow.

### Easier Collaboration

When you have reproducible code and a clear project structure:
- A collaborator can check out your repository and run your analysis without emailing you questions
- They can extend your code confidently, knowing what the baseline behavior is
- You can merge their improvements back in without breaking things

### Faster Debugging and More Impactful Publications

When something breaks, you can identify exactly when it broke by looking at code history. You can reproduce the bug reliably and test whether your fix actually works.

Sharing code and data gets you more citations, builds your reputation as rigorous and trustworthy, and allows others to use your tools and methods.

::::::::::::::::::::::::::::::::::::: challenge

## Discussion: Your Reproducibility Challenge

In your own research (or research you're familiar with):
1. Think of a recent analysis you completed or reviewed
2. Imagine someone else (or you, 3 years from now) trying to reproduce it
3. What would be hard to reconstruct?
4. What would be easy?

Discuss with a neighbor or note down 2-3 key obstacles.

:::::::::::::::::::::::::::::::::::::: solution

## Solution

Common discussion points participants typically raise:

**Hard to reconstruct:**
- Which version of a script produced the final results
- What software versions were installed at the time
- Manual steps performed in Excel or a GUI tool
- Why certain data points were excluded
- Parameter choices that were tested but not documented

**Easier to reconstruct:**
- Which raw data files were used (if well-named)
- The general analytical approach (if documented in a paper)
- Final figures and tables (if saved)

**Key takeaway:** If you find yourself saying "I think I used..." or
"I'm not sure which version...", that is a reproducibility gap this
workshop will help you close.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Computational reproducibility means someone can take your code, data, and documentation and get the same results
- There are three levels: numerical reproducibility (same numbers), logical reproducibility (same conclusions), and process reproducibility (same workflow)
- Reproducibility benefits you first: it's easier to debug, extend, and share reproducible work

::::::::::::::::::::::::::::::::::::::::::::::
