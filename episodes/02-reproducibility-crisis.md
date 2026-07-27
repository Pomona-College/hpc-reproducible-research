---
title: "The Reproducibility Crisis"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- What evidence exists for a reproducibility crisis in science?
- What are the most common barriers to reproducibility?
- How can I assess my own reproducibility readiness?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain the reproducibility crisis with real-world examples
- Identify the most common barriers to computational reproducibility
- Use a reproducibility checklist to assess your own work

::::::::::::::::::::::::::::::::::::::::::::::::::

## Real-World Examples

In 2011, a team at Duke University led by cancer researcher Anil Potti published analyses suggesting they could predict which patients would respond to specific cancer drugs. These papers were published in top journals like *Nature* and appeared to offer real clinical value. However, when independent researchers tried to verify the results, they found critical errors:

- Raw data files were mislabeled and misaligned
- Statistical analyses were performed inconsistently
- The code wasn't available to examine
- Results couldn't be reproduced

This led to retracted papers, lawsuits, and real harm to cancer patients who had enrolled in clinical trials based on faulty analysis.

In 2012, researchers discovered that a widely-used genetics software tool had a bug in how it handled missing data. This bug had been silently present for years, affecting hundreds of published papers. Researchers had to spend months tracking down affected analyses to understand which results were compromised.

A 2015 survey in *Nature* found that over 70% of researchers had failed to reproduce another scientist's published experiment, and over 50% had failed to reproduce their own prior work.

### Why This Matters Beyond Science

- **For yourself:** Have you ever come back to your own analysis six months later and wondered what you did? Which version of the data did you use? What script produced this figure?
- **For your field:** Unreliable results damage scientific progress. If 10% of published results are wrong due to subtle bugs, the entire research literature becomes a noisy signal.
- **For society:** Research informs policy, medical decisions, and public understanding. Irreproducible research erodes public trust in science.
- **For your career:** Showing your work thoroughly, with code and data available, demonstrates rigor and builds your reputation.

## Common Obstacles to Reproducibility

Understanding why reproducibility is hard helps you solve these problems systematically:

### "It Works on My Machine"

Your laptop has Python 3.11, numpy 1.24.3, and pandas 2.0.1 installed. Your collaborator has Python 3.9, numpy 1.21.0, and pandas 1.3.5. The code behaves differently. Maybe not obviously different; maybe a function signature changed, or a default behavior shifted, or there's a subtle numerical difference in how floating-point operations are handled.

**Solution:** Capture your environment explicitly with conda or pip freeze, so you can recreate it exactly.

### Manual Steps

You've got a script that processes data, but there are a few steps you always do manually:
- Open the output CSV, check for weird rows, delete them
- Open the figure in Photoshop to adjust colors and fonts
- Run the script three times with slightly different parameters and combine the results

These manual steps are invisible to someone trying to reproduce your work.

**Solution:** Automate everything, even the small things. Encode decisions in code.

### Implicit Dependencies

Your analysis depends on someone having already installed a specific R package (but you forgot to document which version), a particular data file existing in a particular location, or a Python module you wrote last year but never published. When you share your code, these dependencies silently fail.

**Solution:** Make dependencies explicit. Use requirements files, configuration files, and clear documentation.

### Dead Ends and Iterations

Science is iterative. You tried ten things; nine didn't work. Your final code is clean and works, but it doesn't explain why you chose this statistical test, why you filtered data this way, or what other approaches you considered.

**Solution:** Use version control (git) to preserve the history. Document key decisions. Separate exploratory code from final analysis.

### Data Drift and Uncontrolled Randomness

You originally analyzed data in November 2023. The raw data file changes: more entries are added, some values are corrected. You run your analysis in June 2024 and get slightly different results. Which is "right"?

Your analysis uses random sampling, cross-validation, or Monte Carlo simulation. Each time you run it, you get slightly different results.

**Solutions:** Treat raw data as read-only. Version your data (or at least document its exact state). Set a random seed in your code and document it.

## A Reproducibility Checklist

Here's a practical checklist. Don't expect to check all boxes immediately -- that's what this workshop is for:

- [ ] I can write down the exact steps to reproduce my analysis
- [ ] My code and data are version-controlled (in git)
- [ ] I've documented my software versions (or captured them in an environment file)
- [ ] I can run my entire analysis with a single command
- [ ] I've separated raw data (read-only) from processed data from results
- [ ] My code is understandable by someone else (or me in 6 months)
- [ ] I've documented *why* I made key choices, not just *what* they are
- [ ] I can run my analysis on a different machine and get the same results
- [ ] I've identified which parts of my analysis are random and controlled randomness
- [ ] My code, data, and results are available for others to access and examine

By the end of this workshop, you'll be checking these boxes.

## Moving Forward

Reproducibility isn't about perfection or following arbitrary rules. It's a practical investment in your research. Code that's reproducible is easier to understand, easier to fix when it breaks, easier to share, and easier to build on.

The tools we'll teach you (Git, conda, Make, and SLURM) are used by researchers and engineers across the world precisely because they make reproducibility practical and efficient.

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Audit Your Own Work

Pick a recent analysis or project you have worked on.
Rate yourself (1 = not at all, 5 = fully) on each checklist item above.

1. What is your total score out of 50?
2. Which items scored lowest?
3. Which item would be easiest to improve first?

Share your lowest-scoring item with a neighbor and discuss strategies.

:::::::::::::::::::::::::::::::::::::: solution

## Solution

Most researchers score between 15 and 25 on their first self-audit.
Common weak spots include:

- **Single-command execution** (most people run scripts manually in sequence)
- **Environment capture** (most people have never exported an environment file)
- **Documentation of decisions** (code comments say *what*, not *why*)

The easiest first win is usually creating a `requirements.txt` or
`environment.yml` -- it takes five minutes and immediately improves
reproducibility.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- The reproducibility crisis is real, with major consequences for science and careers
- Common obstacles include environment differences, manual steps, implicit dependencies, and poor documentation
- A reproducibility checklist helps you identify and close gaps systematically
- Reproducibility is a practical investment in your research quality and impact

::::::::::::::::::::::::::::::::::::::::::::::
