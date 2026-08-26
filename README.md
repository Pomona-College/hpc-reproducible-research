# Reproducible Research Pipelines Workshop

A comprehensive Carpentries Incubator workshop on building reproducible computational research workflows using Git, conda, Make, and SLURM.

## About This Workshop

This workshop teaches researchers to create end-to-end reproducible computational pipelines that are:
- **Automated**: Entire analyses run with a single command
- **Documented**: Code, environment, and decisions are clear and traceable
- **Portable**: Analyses work on laptops, HPC clusters, and collaborators' machines
- **Scalable**: Leverages multi-core CPUs and HPC job schedulers
- **Shareable**: Published with proper documentation and permanent identifiers (DOIs)

The workshop integrates:
- **Git**: Version control for code and research decisions
- **conda**: Reproducible environment management
- **Make**: Workflow automation
- **SLURM**: HPC job scheduling and orchestration
- **Documentation best practices**: READMEs, licensing, archiving

## Quick Start

### For Learners

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Pomona-College/hpc-reproducible-research.git
   cd reproducible-research
   ```

2. **Complete setup:**
   - Follow [Setup Instructions](learners/setup.md)
   - Verify all prerequisites are installed
   - Test HPC access

3. **Start with Episode 1:**
   - Work through episodes in order
   - Do all exercises hands-on
   - Use the [Reference](learners/reference.md) as needed

4. **Time estimate:** 6-8 hours total (can be distributed over days)

### For Instructors

1. **Review materials:**
   - Read [Instructor Notes](instructors/instructor-notes.md) for teaching strategies
   - Review each episode for pacing and timing
   - Check troubleshooting appendix

2. **Adapt for your context:**
   - Modify Sagehen HPC references to match your HPC cluster
   - Use field-specific examples in your domain
   - Customize exercise data

3. **Test everything:**
   - Work through all exercises yourself
   - Verify conda/git/Make examples on your system
   - Test SLURM examples on your cluster

4. **Set up workshop environment:**
   - Ensure learners can install software
   - Provide HPC access (or alternative cluster)
   - Prepare shared troubleshooting document

## Workshop Structure

### Episodes

1. **[Why Reproducibility Matters](episodes/01-why-reproducibility.md)** (30 min)

2. **[The Reproducibility Crisis](episodes/02-reproducibility-crisis.md)** (30 min)

3. **[Project Directory Structure](episodes/03-project-directory-structure.md)** (40 min)

4. **[README and Documentation Files](episodes/04-readme-documentation.md)** (35 min)

5. **[Conda Environment Management](episodes/05-conda-environments.md)** (40 min)

6. **[pip, Docker, and HPC Modules](episodes/06-pip-and-containers.md)** (30 min)

7. **[Introduction to Makefiles](episodes/07-intro-makefiles.md)** (40 min)

8. **[Advanced Make Patterns](episodes/08-advanced-make.md)** (40 min)

9. **[Git for Research Projects](episodes/09-git-for-research.md)** (40 min)

10. **[Branching and Collaboration](episodes/10-branching-collaboration.md)** (40 min)

11. **[SLURM Pipeline Basics](episodes/11-slurm-pipeline-basics.md)** (40 min)

12. **[Multi-Step SLURM Workflows](episodes/12-multistep-slurm.md)** (50 min)

13. **[Sharing Your Research](episodes/13-sharing-research.md)** (35 min)

14. **[Putting It All Together](episodes/14-putting-it-together.md)** (40 min)

### Learner Materials

- **[Setup](learners/setup.md)**: Installation and prerequisite verification
- **[Reference](learners/reference.md)**: Quick reference for tools and commands
- **[Learner Profiles](learners/learner-profiles.md)**: Who this workshop is for and how to use it

### Instructor Materials

- **[Instructor Notes](instructors/instructor-notes.md)**: Teaching strategies, common issues, timing guidance

## Key Features

✓ **Hands-on practice**: All concepts practiced with real exercises
✓ **Comprehensive**: Covers entire reproducible workflow
✓ **Real-world examples**: Gene expression analysis case studies
✓ **HPC integration**: Specifically designed for Sagehen and SLURM clusters
✓ **Field-agnostic**: Adaptable to biology, chemistry, physics, data science, etc.
✓ **Modern tools** :  git, conda, Make, SLURM (current best practices)
✓ **Open and community-driven** :  Contributions welcome

## Prerequisites

### For Learners

- Basic Unix shell skills (navigating directories, running commands)
- Basic programming experience (Python or R)
- Familiarity with `git add`, `git commit`, `git push`
- Access to HPC cluster (Sagehen or equivalent)

**Recommended:**
- Have completed [Software Carpentry Unix Shell](https://swcarpentry.github.io/shell-novice/) and [Git](https://swcarpentry.github.io/git-novice/) lessons

### For Instructors

- Experience with reproducible research practices
- Familiarity with all tools covered (git, conda, Make, SLURM)
- Access to an HPC cluster for live demonstrations
- Teaching experience (ideally with Carpentries training)

## Technical Requirements

### Software (Install Before Workshop)

- **git** :  Version control
- **conda** (Miniconda recommended) :  Environment management
- **Make** :  Workflow automation
- **Text editor** :  VS Code, Sublime, nano, vim, etc.
- **Terminal/Shell** :  bash or zsh

### Hardware

- Laptop for local development
- HPC cluster access (Sagehen or equivalent with SLURM)

### Cluster Setup

This workshop uses Sagehen (sagehen.hpc.pomona.edu):
- SLURM job scheduler
- Lmod module system
- conda available via `module load miniconda3`

Adaptable to other SLURM clusters by changing module names and paths.

## Customization and Contribution

### For Your Institution

This workshop is designed for adaptation. To customize:

1. **Replace Sagehen references** with your cluster details
2. **Use field-specific examples** in your domain (see instructor notes)
3. **Adjust timing** based on learner backgrounds
4. **Add institutional examples** in exercises
5. **Share improvements** via pull requests

### Contributing Back

We welcome contributions! Please:

- **Report issues** on GitHub
- **Submit pull requests** for improvements
- **Share adaptations** for other fields/clusters
- **Provide feedback** on what works and what needs improvement

The workshop improves through community involvement.

## Licensing and Attribution

This work is licensed under **Creative Commons Attribution 4.0 (CC-BY 4.0)**.

**You are free to:**
- Share and use this material
- Adapt it for your needs
- Teach it to others

**You must:**
- Provide attribution to the original authors
- Include a copy of this license

**Citation:**

```
Wilson, A. (2024). Reproducible Research Pipelines.
The Carpentries Incubator.
Retrieved from https://github.com/Pomona-College/hpc-reproducible-research
```

## Contact and Support

### Questions or Issues

- **GitHub Issues:** https://github.com/Pomona-College/hpc-reproducible-research/issues
- **Email:** its-hpc@pomona.edu (HPC support at Pomona College)

### Acknowledgments

This workshop is designed as a Carpentries Incubator lesson, building on years of community experience teaching reproducible research practices.

### External Resources

- **The Turing Way** :  Handbook for reproducible research: https://the-turing-way.netlify.app/
- **The Carpentries** :  Community of educators: https://carpentries.org/
- **FAIR Data Principles** :  Best practices for data: https://www.go-fair.org/

## Instructor Community

This workshop is part of The Carpentries Incubator. Connect with other instructors:

- **Carpentries Discourse:** https://discourse.carpentries.org/
- **Incubator Forum:** https://github.com/carpentries-incubator/proposals/discussions

## Version History

- **v1.0** (March 2026) :  Initial release
  - 14 episodes covering complete reproducible workflow
  - Learner and instructor materials
  - Sagehen-specific examples
  - Tested with beta learners

## Roadmap

Future improvements (contributions welcome):

- [ ] Docker episode for container-based reproducibility
- [ ] Nextflow episode for complex multi-node workflows
- [ ] Field-specific adaptations (bioinformatics, climate science, physics)
- [ ] Interactive notebooks for browser-based learning
- [ ] Video recordings of live workshops
- [ ] Extended examples and case studies

---

## Getting Help

### I'm a learner and have questions

- Review the [Reference](learners/reference.md) for quick answers
- Check the [Learner Profiles](learners/learner-profiles.md) to see if your situation is covered
- Open an [issue on GitHub](https://github.com/Pomona-College/hpc-reproducible-research/issues) with your question

### I'm an instructor and need teaching materials

- Read [Instructor Notes](instructors/instructor-notes.md) for strategies and timing
- Check the troubleshooting appendix for common issues
- Adapt examples for your field using guidance in the notes

### I want to contribute

- Fork the repository
- Create a branch for your changes
- Submit a pull request with a description of improvements
- Follow the Contributing Guidelines (if present)

---

**Happy reproducible researching!**

This workshop represents years of experience in computational science and HPC education. We hope it helps you and your research community adopt practices that make science faster, clearer, and more trustworthy.

## Acknowledgments

**Andrew Wilson** — Director of Research Computing and Digital Scholarship,
Pomona College. Workshop design and development.

**Andrei Motchenko** — testing, editing, cleanup and screenshots across the
Pomona College HPC Workshop Series.
