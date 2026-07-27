# Learner Profiles

This workshop was designed for researchers who fit one or more of these profiles. Use this to understand if you'll get the most out of the material.

## Profile 1: Postdoc with Domain Expertise

**Background:**
- PhD in your field (biology, chemistry, physics, etc.)
- Years of research experience
- Strong knowledge of your domain
- Published papers using computational analysis

**Current challenges:**
- You have research code scattered across folders with names like `script_v1.py`, `script_v2_better.py`, `script_FINAL.py`
- When you change something, you're not sure what broke
- Collaborators ask for your code and you're embarrassed to share it
- You've spent days trying to reproduce your own results from 6 months ago
- Your laptop and the HPC cluster behave differently; code works one place but not the other

**What this workshop will teach you:**
- How to organize your projects so everything is clear and organized
- How to automate your analysis so you don't repeat steps manually
- How to track changes to code and understand your decisions
- How to ensure your results are reproducible on any machine
- How to run large analyses on HPC clusters without micromanaging

**Time to engage:** You'll be productive immediately. By Episode 2, you'll restructure your current projects. By Episode 4, you'll have automated your entire workflow.

---

## Profile 2: Graduate Student New to HPC

**Background:**
- Recently completed coursework
- Starting a research project
- Know Unix basics and can write Python/R code
- Your advisor suggested you use the HPC cluster

**Current challenges:**
- Don't know how to submit jobs to SLURM
- Unsure what goes in a bash script vs. Python script
- Run 100 jobs and have no idea which output came from which input
- When a job crashes, don't know how to debug it
- Your results vary between runs for unclear reasons
- Want to learn best practices before developing bad habits

**What this workshop will teach you:**
- The "right way" to organize a computational research project
- How to write reproducible code from the start
- How to submit and manage jobs on HPC clusters
- How to automate workflows so you don't have to manually run 100 jobs
- How to document your analysis so you can explain what you did
- How to work with version control as part of your daily workflow

**Time to engage:** Each episode builds on the previous one. By the end, you'll have the skills to conduct research properly. You'll save hundreds of hours of debugging later.

---

## Profile 3: Research Software Engineer

**Background:**
- Software engineering or computer science background
- Work in a research group or research computing center
- Help other researchers with their code
- Want to promote reproducible practices in your lab/center

**Current challenges:**
- Other researchers hand you messy code and ask you to fix it
- Difficult to teach good practices without sounding preachy
- Your institution wants reproducibility but doesn't have training
- Need concrete examples to show researchers "why it matters"

**What this workshop will teach you:**
- Complete, practical examples of reproducible research projects
- How to teach reproducibility to researchers with different backgrounds
- How to integrate open science into institutional practices
- Materials you can adapt and teach to your colleagues

**Time to engage:** This workshop is a resource you can use directly in training others. Each episode has exercises and instructor notes. You can teach it as-is or customize it for your institution's needs. Modify examples for your field.

---

## Profile 4: Early-Career Researcher Preparing for Publication

**Background:**
- Have research results ready to publish
- Want to release code and data with your paper
- Value reproducibility but unsure how to implement it
- Haven't used version control in a meaningful way

**Current challenges:**
- Not sure how to make code "publication-ready"
- Where should I put data?
- How do I ensure others can run my code?
- What license should I use?
- How do I make my work citable?

**What this workshop will teach you:**
- How to clean up and document code for publication
- Where to archive code and data (Zenodo, Figshare, etc.)
- How to get permanent citations (DOIs) for your work
- How to write READMEs that help users understand and run your analysis
- Best practices for licensing research code

**Time to engage:** Focus on Episodes 2 (organization), 5 (Git for tracking decisions), and 7 (documentation and sharing). These directly support publication. After the workshop, you'll have publication-ready documentation and code.

---

## Profile 5: Method Developer

**Background:**
- Developing new statistical, computational, or analytical methods
- Want others to use your method
- Need to demonstrate that your method works

**Current challenges:**
- Users can't reproduce your published results
- Uncertainty about whether they're using your method correctly
- Want to package your method but unsure where to start
- Need examples people can run to verify the method works

**What this workshop will teach you:**
- How to structure code for clarity and reproducibility
- How to automate testing and validation of your method
- How to document usage with clear examples
- How to version your method as it improves
- How to publish your work so others can discover and cite it

**Time to engage:** Episodes 2-4 are most relevant. Focus on organization, automation, and version control. These ensure your method is clear and verifiable.

---

## Profile 6: Researcher Returning After a Break

**Background:**
- Took time off from research (career break, sabbatical, etc.)
- Coming back to old projects
- Want to restart cleanly with modern practices

**Current challenges:**
- Looking at old code and can't remember how it works
- Data files are scattered in different formats
- Don't remember what analysis choices you made
- Want to do things better this time

**What this workshop will teach you:**
- Modern workflows for organizing and automating research
- How to document your work so you (and others) understand it later
- Tools that are now standard in research (conda, Make, Git for research)
- How to avoid the mess you got into before

**Time to engage:** All episodes are relevant as you rebuild your research practices. Treat this as an opportunity to establish good habits from the start of your next project.

---

## Profile 7: Lab Principal Investigator / Group Leader

**Background:**
- Lead a research group
- Want to establish good practices
- Your students have varying levels of computational skill
- Want reproducible, maintainable research output

**Current challenges:**
- Students produce inconsistent, hard-to-understand code
- Papers retract because results can't be reproduced
- Institutional review or audits of research methods
- Want to train your group but unsure how

**What this workshop will teach you:**
- A complete framework for reproducible research
- Standards you can require your students to follow
- Concrete examples you can show as "this is what good looks like"
- Tools that improve research quality and student training

**Time to engage:** This workshop is directly usable as training material for your group. Encourage all your students to take it (or adapt it for your field). The practices reduce problems downstream and make your lab's research more impactful.

---

## Which Profile Is You?

**Profile 1 (Postdoc):** Episodes 1-2 will make you productive immediately.

**Profile 2 (Graduate Student):** Take the workshop in order; each builds on the previous.

**Profile 3 (RSE):** Use as training material for your institution.

**Profile 4 (Early Career):** Focus on Episodes 2, 5, and 7 before publishing.

**Profile 5 (Method Developer):** Episodes 2-4 are essential for your method's usability.

**Profile 6 (Returning):** All episodes; use as a reset to modern practices.

**Profile 7 (PI/Group Leader):** Adapt and teach to your team.

---

## How to Use This Workshop

### For Self-Study
Work through episodes in order. Code along with examples. Do all exercises. Budget ~6-8 hours total.

### For Institutional Training
This workshop is designed for The Carpentries Incubator and is adaptable for your institution:
- Modify examples for your field
- Adjust Sagehen-specific content for your cluster
- Add domain-specific examples in the exercises
- Teach it as a full-day or multi-day workshop

### For Your Research Group
- Have your students work through it individually before joining your group
- Use it as a onboarding training
- Reference it when discussing code review or reproducibility
- Adapt examples to match your field and cluster

---

## What You'll Be Able to Do After This Workshop

Regardless of your profile, you'll be able to:

- **Organize** a research project following best practices
- **Automate** multi-step analyses with Make
- **Track** code changes with Git and understand your research evolution
- **Capture** your computational environment so others can reproduce your work
- **Run** and orchestrate jobs on HPC clusters
- **Document** your work so others can use it
- **Publish** your research with proper attribution and permanent identifiers

You'll have the foundation for a research career built on reproducible, transparent, collaborative science.
