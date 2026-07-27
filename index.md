---
site: sandpaper::sandpaper_site
---

# Reproducible Research Pipelines

Welcome to the **Reproducible Research Pipelines** workshop! This is a capstone course designed to equip researchers with the practical skills needed to create, scale, and share computational research workflows that produce consistent, verifiable results.

## About This Workshop

Computational research has become increasingly central to scientific discovery, but with that opportunity comes a significant challenge: **How do we ensure our results are reproducible?** When code runs on different machines, with different software versions, or with subtle variations in how analysis steps are ordered, results can diverge; sometimes subtly, sometimes dramatically.

This workshop teaches you to build research pipelines that:
- **Run the same way every time** on your machine, your collaborator's machine, or an HPC cluster
- **Capture all dependencies** so you know exactly what versions of software were used
- **Automate complex workflows** so nothing is forgotten or done manually
- **Scale to thousands of jobs** on high-performance computing clusters
- **Are easy to understand and modify** so your future self (and others) can build on your work
- **Can be shared and published** with data and code supporting your publications

## Who Is This Workshop For?

This is an **advanced workshop** designed for researchers who:
- Have basic Unix shell and Python/R programming skills
- May have some experience with version control using Git
- Want to move beyond "it works on my machine" to truly reproducible science
- Plan to run analyses on HPC clusters like Sagehen

If you haven't yet used Git or the Unix command line, we recommend starting with [The Carpentries Unix Shell Workshop](https://swcarpentry.github.io/shell-novice/) and [Git workshop](https://swcarpentry.github.io/git-novice/) before diving in here.

## Workshop Overview

Over the course of this workshop, you will:

1. **Understand reproducibility**: What it means, why it matters, and the common obstacles
2. **Organize your research**: Implement a standard project structure that separates raw data, code, and results
3. **Capture environments**: Use conda and environment files to lock dependencies so your analysis stays reproducible
4. **Automate workflows**: Write Makefiles that orchestrate multi-step analyses automatically
5. **Version your research**: Use Git not just for code, but for experiments and analysis decisions
6. **Scale to HPC**: Harness SLURM job scheduling to run pipelines across compute clusters efficiently
7. **Share and publish**: Document your work and release it with DOIs so others can cite and extend your research

## Key Technologies

This workshop focuses on tools that are widely available and form the backbone of reproducible research in academia and industry:

- **Git**: Version control for code and research decisions
- **conda**: Environment management for capturing software dependencies
- **Make**: Workflow automation that explicitly encodes analysis steps
- **SLURM**: Job scheduling for parallel and high-performance computing
- **Sagehen HPC cluster**: The platform we'll use for practical examples

## Learning Outcomes

By the end of this workshop, you will be able to:

- Explain the three levels of computational reproducibility
- Design a project structure that supports long-term reproducibility
- Create conda environments and export them for sharing
- Write Makefiles that automate research workflows
- Manage code versions and track experimental branches in Git
- Submit and orchestrate HPC jobs using SLURM dependencies
- Create comprehensive documentation for sharing reproducible research
- Publish reproducible research with proper citations and licensing

## How to Use This Material

Each episode contains:
- **Learning objectives** at the start
- **Narrative explanations** and key concepts
- **Code examples** you can run on Sagehen
- **Hands-on exercises** for practice
- **Key points** summarizing the main ideas

Episodes build on each other, so we recommend working through them in order. However, if you have prior experience with some topics, you can jump to relevant sections.

## Prerequisites

::::::::::::::::::::::::::::::::: prereq

Before starting, you should:
- Be comfortable with the Unix shell (navigating directories, running commands, redirecting output)
- Have basic programming experience in Python or R
- Have access to Sagehen (sagehen.hpc.pomona.edu) or another SLURM-based HPC cluster
- Have Git installed and a basic understanding of `git add`, `git commit`, and `git push`

If you need a refresher, see the [Setup](learners/setup.md) page.

::::::::::::::::::::::::::::::::::::::

## Cluster Setup

All examples assume you're running on **Sagehen** with:
- Lmod module system
- conda available via `module load conda`
- Standard Unix tools (Make, git, etc.)

If you're using a different cluster, the commands should transfer with minor adjustments to module names or job submission parameters.

## Support & Questions

- **Workshop contact:** its-hpc@pomona.edu
- **Issues & contributions:** See our [GitHub repository](https://github.com/pomona-college-hpc/reproducible-research)
- **Carpentries community:** Connect with us on [The Carpentries community forum](https://carpentries.org/)

This is an **open, community-contributable** workshop. If you find errors, have suggestions, or want to add examples relevant to your field, please open an issue or pull request!

## License

This material is made available under the Creative Commons Attribution 4.0 license. See [LICENSE](LICENSE) for details.

---

## Workshop Chapters

<div class="row">
  <div class="col-md-6">
    <h3>Core Content</h3>
    <ul>
    {% for episode in site.episodes %}
      <li><a href="{{ site.baseurl }}/{{ episode.url }}">{{ episode.title }}</a></li>
    {% endfor %}
    </ul>
  </div>
  <div class="col-md-6">
    <h3>Resources</h3>
    <ul>
      <li><a href="{{ site.baseurl }}/learners/setup.html">Setup Instructions</a></li>
      <li><a href="{{ site.baseurl }}/learners/reference.html">Reference</a></li>
      <li><a href="{{ site.baseurl }}/learners/learner-profiles.html">Learner Profiles</a></li>
      <li><a href="{{ site.baseurl }}/instructors/instructor-notes.html">Instructor Notes</a></li>
    </ul>
  </div>
</div>
