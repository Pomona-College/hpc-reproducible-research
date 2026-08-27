# Instructor Notes

Teaching guides for delivering the Reproducible Research Pipelines workshop.

## Workshop Overview

This is a capstone workshop bringing together seven tools and practices essential for reproducible research. Learners should have foundational knowledge of Unix shell and Git before attending, though the workshop does review key concepts.

**Total duration:** 2-3 days (6-8 hours instruction + exercise time)
**Difficulty:** Intermediate to advanced
**Target audience:** Researchers with programming experience seeking to professionalize their workflows

## Pedagogical Approach

### Philosophy

1. **Motivation First**: Episode 1 emphasizes *why* reproducibility matters through real examples of scientific failures
2. **Build Incrementally**: Each episode builds on previous ones; learners apply cumulative knowledge
3. **Hands-On Practice**: All concepts are immediately practiced; learners do, not just watch
4. **Real-World Examples**: Examples use realistic data and workflows, not toy problems
5. **Open Community**: This is designed as Carpentries Incubator material; community contributions are welcome

### Teaching Strategy

- **Connect to pain points:** Ask learners about their current struggles (messy projects, unclear code) and show how each episode's content solves those problems
- **Use learner examples:** If possible, have learners bring their own projects and apply lessons to them
- **Encourage questions:** Reproducibility is about transparency; model asking and answering questions
- **Celebrate small wins:** Praise learners when they successfully automate a workflow or clean up a project
- **Normalize mistakes:** Show your own examples of poorly organized research and how you fixed it

## Episode-by-Episode Teaching Notes

### Episode 1: Why Reproducibility Matters

**Learning objectives:**
- Understand the reproducibility crisis
- Recognize personal and professional benefits of reproducible work
- Know the three levels of reproducibility

**Key teaching points:**

1. **Open with a story**: The Duke cancer research scandal is powerful. Spend 5 minutes on it. Ask: "Why did this happen?" (Implicit: Poor documentation, no code review, unclear analysis)

2. **Make it personal**: Ask learners: "Have you ever reopened your own analysis six months later and wondered what you did?" Most will say yes. This is your hook.

3. **Three levels**: These are abstract; make them concrete with examples:
   - **Numerical:** Run code, get same numbers
   - **Logical:** Understand *why* those numbers matter
   - **Process:** Understand the journey to get there

4. **Don't overclaim**: Reproducibility won't fix bad science, but it will catch errors and make good science verifiable

**Common misconceptions:**

- *"Reproducibility is just about sharing code"*: Clarify: sharing code is necessary but not sufficient; environment, data, documentation all matter
- *"Perfect reproducibility is impossible"*: True for simulations with randomness, but you can control randomness via seeds
- *"My field doesn't value this"*: Journals increasingly require code/data; funders expect it; labs doing this work have higher impact

**Timing:** 45 minutes lecture + 15 minutes discussion

### Episode 2: Project Organization

**Learning objectives:**
- Design a standard project structure
- Explain why separating concerns matters
- Make raw data read-only
- Use relative paths

**Key teaching points:**

1. **Before/After**: Show a messy project, then the same project organized. Visual impact is huge.

2. **Raw data is sacred**: Emphasize: Raw data never changes. All transformations are reproducible. This is foundational.

3. **Relative paths**: This is a common mistake. Spend extra time here. Show:
   ```python
   # Wrong
   df = pd.read_csv("/home/alice/project/data.csv")  # Only works on Alice's machine

   # Right
   df = pd.read_csv(Path(__file__).parent.parent / "data" / "raw" / "data.csv")
   ```

4. **README is not optional**: Future readers (including yourself) will not understand your project without it.

**Exercise tips:**
- Have learners sketch their own project structure
- If they're working on real projects, apply this immediately
- Check for relative paths in their code; this is often missed

**Common mistakes:**
- Absolute paths in scripts (test by moving the project to a different location)
- Putting raw data in git (use .gitignore)
- Editing processed data files manually instead of in scripts

**Timing:** 40 minutes lecture + 20 minutes exercises

### Episode 3: Environment Capture

**Learning objectives:**
- Explain why environments differ
- Create and export conda environments
- Document HPC module dependencies
- Use requirements files effectively

**Key teaching points:**

1. **"Works on my machine"**: A real story: TensorFlow API changed, broke reproducibility. Version pinning would have caught it.

2. **Conda vs. pip**: Simple rule:
   - conda: Use by default for research (handles system libraries)
   - pip: For pure Python if you prefer
   - Both: Often used together

3. **Version pinning**: Show the difference:
   ```yaml
   # Loose (easier, less reproducible)
   dependencies:
     - python=3.11
     - numpy>=1.24

   # Strict (less flexible, fully reproducible)
   dependencies:
     - python=3.11.7
     - numpy=1.24.3
   ```

   Recommendation: Start loose during development, lock at publication

4. **HPC module system**: On Sagehen HPC, `module load miniconda3` is needed. Document this in `setup_modules.sh`.

**Exercise tips:**
- Have learners create their own environment with their actual packages
- Export it and show the version specifications
- Test: can they recreate the environment on a different machine/account?

**Common issues:**
- Forgetting to remove the `prefix` line when sharing `environment.yml`
- Pinned versions becoming unavailable over time (answer: conda-forge is more stable than defaults)
- Confusion between conda channels (show: `conda config --show channels`)

**Timing:** 45 minutes lecture + 20 minutes exercises

### Episode 4: Workflow Automation with Make

**Learning objectives:**
- Understand Make targets, prerequisites, recipes
- Write a multi-step Makefile
- Use Make variables and special variables
- Combine Make with SLURM

**Key teaching points:**

1. **Why Make, not custom scripts?**: Make tracks dependencies. If you change a script, only affected steps re-run. With shell scripts, you re-run everything or manually track what's needed.

2. **The TAB rule**: This trips everyone. Spend time on it. Mistakes here cause mysterious "missing separator" errors.

3. **Variables for DRY**: Show the wrong way (hardcoded paths everywhere) and right way (use `$(VAR)`).

4. **Special variables** are powerful but confusing:
   - Introduce gradually; don't overwhelm with all at once
   - Use examples: `$@` in the first rule, then `$<` in the second, then `$^` in a merge rule

5. **Phony targets**: Common mistake: students write `make clean` but a file named `clean` exists. Explain `.PHONY: clean` solves this.

**Exercise:**
- Start simple: two-step pipeline (raw data → cleaned data)
- Expand: add analysis step
- Demonstrate incremental re-running by changing one script

**Common mistakes:**
- Using spaces instead of tabs (causes "missing separator" error)
- Not using relative paths (pipeline breaks when moved)
- Not testing `.PHONY: clean` actually removes everything
- Circular dependencies (target A depends on B depends on A): rare but catastrophic

**Timing:** 50 minutes lecture + 30 minutes exercises

### Episode 5: Version Control for Research

**Learning objectives:**
- Use Git for research code and decisions
- Write meaningful commit messages
- Use branches for experiments
- Leverage history as documentation

**Key teaching points:**

1. **Git is not just for code**: It's for tracking research decisions. Each commit can explain a choice.

2. **Commit messages matter**: Show bad vs. good:
   ```
   Bad:  "update script"
   Good: "Increase expression threshold from 5 to 10 per domain expert feedback
         This reduces false positives without losing true positives."
   ```

3. **Branches for safety**: Safe to experiment on a branch without breaking main. Excellent for trying new statistical methods.

4. **What NOT to track:**
   - Raw data (too large, immutable)
   - Results (regeneratable)
   - Credentials (security risk)
   - Use `.gitignore` liberally

5. **Git history as lab notebook**: `git log` tells the story of your research. This is powerful.

**Exercise:**
- Initialize a git repo for their project
- Create a branch for a feature
- Write meaningful commit messages
- Merge the branch back to main

**Common issues:**
- Committing large data files (lesson: use `.gitignore` preemptively)
- Non-descriptive commit messages (lesson: write for yourself 1 year later)
- Fear of branching ("Will I lose my work?"): reassure: branches are safe sandboxes

**Timing:** 45 minutes lecture + 25 minutes exercises

### Episode 6: SLURM Pipelines

**Learning objectives:**
- Submit jobs with `sbatch`
- Use job dependencies for sequential workflows
- Integrate Make with SLURM
- Debug failed jobs

**Key teaching points:**

1. **Why SLURM?**: On a laptop, 1 core processes 100 samples sequentially (hours). On HPC with 128 cores, 100 samples in parallel (minutes).

2. **Dependencies are key**: Sample processing must finish before merge; merge must finish before analysis. SLURM dependencies ensure this ordering.

3. **sbatch output**: Show the format:
   ```
   Submitted batch job 12345
   ```
   The number is the Job ID; use it to track the job and express dependencies.

4. **SLURM directives**: Teach incrementally:
   - `--job-name` (human-readable label)
   - `--time` (CPU hours, calculate carefully)
   - `--mem` (memory needed)
   - `--cpus-per-task` (parallelism within job)
   - `--dependency` (wait for other jobs)
   - `--output` / `--error` (log files)

5. **Job arrays**: For 100 independent samples, don't submit 100 jobs individually. Use `--array=1-100`. SLURM creates them all at once.

6. **Integration with Make**: Makefile can submit jobs and capture Job IDs. This automates the entire workflow submission.

**Exercise:**
- Write a simple SLURM script
- Submit it; observe Job ID
- Submit a second job with dependency
- Monitor with `squeue`
- Check results

**Common issues:**
- `--time` too short (job is killed mid-execution)
- `--mem` too small (job crashes with out-of-memory)
- Incorrect job dependency syntax (job never starts)
- Logs write to wrong location if directory doesn't exist

**Timing:** 50 minutes lecture + 30 minutes exercises

### Episode 7: Documentation and Sharing

**Learning objectives:**
- Write comprehensive README
- Choose and apply appropriate license
- Archive code with DOI
- Publish reproducible research

**Key teaching points:**

1. **README is critical**: Before writing code, write the README. It clarifies your thinking.

2. **License is legal clarity**: MIT says "use freely, include license." GPL says "share improvements." CC-BY for data. Choose one and commit it.

3. **Archiving for permanence**: GitHub can disappear. Zenodo/Figshare preserve forever with DOI.

4. **DOI = citable forever**: URLs break. DOIs don't. Include DOI in papers and bios.

5. **Open science = more citations**: Research with code/data available gets ~30% more citations (statistically).

**Exercise:**
- Write a README for a sample project
- Choose a license
- Create a CITATION.cff file
- (Optional: show how to link to Zenodo, but actual archiving requires GitHub account)

**Common issues:**
- README is too technical for non-experts
- License file missing
- No instructions for getting data
- Code archived but data not

**Timing:** 40 minutes lecture + 20 minutes exercises

## Workshop Logistics

### Prerequisites Check

Before the workshop:
- Verify learners have git, conda, and Make installed (setup.md)
- Verify HPC access (test `ssh sagehen.hpc.pomona.edu`)
- Share the materials in advance

**Icebreaker:** In intro, ask learners about their current project and what frustrates them. Connect their answers to episodes.

### Timing and Pacing

Designed for 2-3 days:

**Option 1: Full 3-day workshop**
- Day 1: Episodes 1-2 (Overview and organization)
- Day 2: Episodes 3-5 (Environment, automation, version control)
- Day 3: Episodes 6-7 (HPC and sharing)

**Option 2: Intensive 2-day workshop**
- Day 1 morning: Episodes 1-3
- Day 1 afternoon: Episodes 4-5
- Day 2: Episodes 6-7

**Option 3: Self-paced or distributed**
- Learners work through episodes at own pace
- Hold office hours for questions
- This works well for summer workshops or distributed groups

### Managing Heterogeneous Groups

You'll have learners at different levels:

**For experienced developers:**
- Let them explore ahead; give them stretch exercises (e.g., "Can you write a Makefile with job arrays?")
- Pair them with less experienced learners as helpers

**For learners new to HPC:**
- Spend extra time on SLURM basics
- Have a "cheat sheet" of common commands
- Be patient with the learning curve; HPC concepts are new to many

**For researchers with no coding background:**
- Walk through Python/R code slowly
- Emphasize concepts over syntax
- Use their domain examples in explanations

### Equipment and Access

**For instructors:**
- Laptop with terminal, editor, git, conda, Make installed
- Access to Sagehen (or equivalent cluster) to demo live jobs
- Screen sharing setup if teaching remotely

**For learners:**
- Each needs terminal access and ability to install software
- Access to Sagehen (or their institution's HPC)
- Text editor

**If teaching remotely:**
- Use shared terminal sessions (tmux, screen sharing)
- Have a shared document for live coding (so learners can copy/paste)
- Record sessions for asynchronous learners

### Handling Technical Issues

**Common problems:**

1. **conda command not found after install**
   - Solution: Restart terminal or `source ~/.bashrc`

2. **Cannot connect to Sagehen**
   - Solution: Check with HPC support; may be firewall/VPN issue
   - Workaround: Do SLURM examples on any available HPC

3. **Makefile tab error**
   - Solution: Re-create the rule in editor (some editors convert tabs to spaces)

4. **Git push permission denied**
   - Solution: Check SSH key setup; may need to add key to GitHub

**Prevention:**
- Have working setup code ready to share
- Keep a troubleshooting document
- Build 15 minutes buffer into schedule for technical issues

## Assessment and Feedback

### During Workshop

- **Exercises:** Check that learners' code runs and produces expected output
- **Ask questions:** "What did we just do? Why?" - informal assessment
- **Pair programming:** Have experienced learners help others; identifies who's struggling

### After Workshop

- **Post-workshop survey** asking:
  - Which episodes were most useful?
  - What would you like more practice with?
  - Will you apply this to your research?
- **Follow-up:** Offer "office hours" for questions 2 weeks after workshop
- **Community feedback:** If teaching for multiple institutions, collect lessons learned

## Adapting for Your Field

This workshop is domain-agnostic but benefits from field-specific examples.

**For bioinformatics:**
- Use FASTQ/BAM data examples
- Show SLURM job arrays processing 1000 samples
- Reference tools: samtools, bwa, salmon

**For climate/earth science:**
- Use NetCDF data examples
- Show parallel processing of geographic regions
- Reference tools: nco, cdo, xarray

**For physics/materials science:**
- Use simulation output examples
- Show parameter sweeps with job arrays
- Reference tools: GROMACS, VASP submission scripts

**For social sciences:**
- Use survey/interview data examples
- Show privacy/anonymization in data handling
- Reference tools: R tidyverse, survey packages

To adapt:
1. Replace generic data files with your field's formats
2. Use domain terminology (instead of "samples," use "galaxies," "patients," "molecules")
3. Reference field-standard tools in examples
4. Adjust timing based on learners' background (e.g., statisticians may be faster with Episode 6)

## Resources for Teaching

- **Carpentries Handbook:** https://docs.carpentries.org/
- **Teaching Tips:** https://carpentries.org/teach-with-us/
- **This Workshop GitHub:** (to be created) for sharing teaching experiences and improvements

## Contributing Back

This is designed as Carpentries Incubator material. If you teach it, please:

1. File issues on GitHub for typos, errors, or improvements
2. Submit pull requests for enhanced examples
3. Share your field-specific adaptations
4. Provide feedback on what works and what doesn't

The workshop improves through community contributions.

---

## Appendix: Troubleshooting Common Problems

### Make Issues

**"make: missing separator"**
- Cause: Recipe line has spaces instead of tab
- Fix: Delete the line and re-type with TAB key

**"Target does not rebuild when source changes"**
- Cause: Target is `.PHONY` but shouldn't be (or vice versa)
- Fix: Check `.PHONY` declarations

### SLURM Issues

**"sbatch: command not found"**
- Cause: Not on login node, or module not loaded
- Fix: Make sure you are logged in to Sagehen -- SLURM commands work on the head node without loading any module

**"Job immediately exits with state FAILED"**
- Cause: Usually resource request too small or syntax error in script
- Fix: Check logs with `tail slurm-JOBID.out`; increase `--mem` or `--time`

**"Dependency job never starts"**
- Cause: Dependency syntax wrong or referenced job not found
- Fix: Check syntax: `--dependency=afterok:JOBID`; verify dependency job exists

### Git Issues

**"Everything is committed except my changes"**
- Cause: Forgot `git add`
- Fix: `git add <files>` then `git commit`

**"I committed to the wrong branch"**
- Fix: Cherry-pick to correct branch:
  ```bash
  git checkout correct-branch
  git cherry-pick wrong-branch-HEAD
  git checkout wrong-branch
  git reset --soft HEAD~1
  ```

### conda Issues

**"PackagesNotFoundError: The following packages are not available"**
- Cause: Package doesn't exist or is in different channel
- Fix: Try `conda search package-name`; may need to add channel

**"Environment conflicts"**
- Cause: Package versions incompatible
- Fix: Loosen version requirements or start fresh; test in new environment first
