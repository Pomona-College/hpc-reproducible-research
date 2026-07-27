---
title: "Branching and Collaboration"
teaching: 25
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I use branches for parallel experiments?
- How do I tag versions for publications?
- How do I collaborate with others using Git?
- When should I NOT use Git?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use branches to explore different analysis approaches
- Tag specific commits as publication versions
- Collaborate using shared repositories and pull requests
- Recognize when to not use Git (large data files, sensitive data)

::::::::::::::::::::::::::::::::::::::::::::::::::

## Using Branches for Experiments

Branches are where Git becomes powerful for research.

### Scenario: Trying New Analysis Approaches

You have a working analysis on `main`. You want to try a different statistical test without breaking the working code:

```bash
git checkout -b experiment/new-statistical-test
# Now on a new branch
# Make changes, commits, etc.

# If it works, merge back to main
git checkout main
git merge experiment/new-statistical-test

# If it doesn't work, discard it
git branch -D experiment/new-statistical-test
# main is untouched
```

### Naming Branches

Use a convention:

```
main                           # Production / paper version
develop                        # Working version
feature/add-confidence-intervals
experiment/logistic-regression
bugfix/date-parsing-error
```

### The Flow

```bash
# Start with main (stable)
git checkout main

# Create feature branch for new work
git checkout -b feature/confidence-intervals

# Make changes, test, commit
echo "confidence interval code" >> src/analyze.py
git add src/analyze.py
git commit -m "Add confidence interval calculation

Using scipy.stats.t.ppf for parametric intervals and
scipy.stats.bootstrap for non-parametric bootstrapped CIs."

# When satisfied, merge to main
git checkout main
git merge feature/confidence-intervals
git branch -d feature/confidence-intervals
```

### Comparing Approaches

Branches let you compare analysis approaches:

```bash
# Compare two methods
git log main...feature/logistic-regression --oneline

# See what figures changed
git diff main feature/logistic-regression -- results/

# See what the code difference is
git diff main feature/logistic-regression -- src/analyze.py
```

This is documentation of your scientific decision-making process.

## Tagging: Versions for Publications

Tag specific commits as versions:

```bash
# Tag the version submitted to journal
git tag v1.0-submitted

# Tag revisions
git tag v1.1-revision-1

# Create an annotated release
git tag -a v2.0-published -m "Final published version"
```

Later, you can easily return to the published version:

```bash
git checkout v2.0-published
```

Or compare:

```bash
git diff v1.0-submitted v2.0-published -- results/
```

## Collaborating with Git

### Shared Repository Workflow

Your lab shares a private GitHub repository:

```bash
# Clone the repository
git clone git@github.com:mylab/project.git
cd project

# Create a feature branch
git checkout -b feature/new-analysis

# Make changes, commits
git add src/
git commit -m "Add new statistical test"

# Push to GitHub
git push origin feature/new-analysis

# Open a Pull Request on GitHub
# Lab members review, discuss, approve
# Merge to main when ready
```

### Staying in Sync

Before working, get latest changes:

```bash
git fetch origin
git rebase origin/main
```

This ensures you're building on the latest version.

## When NOT to Use Git

### Large Data Files

Git stores the entire history of every file. If you commit a 100 MB data file, then delete it, Git still stores it. Your repository becomes bloated.

**Solution:** Don't track raw data. Document how to obtain it.

### Binary Data

If you have large neuroimaging files (.nii), genomic data (.bam, .vcf), or simulation output (.pkl, .h5), Git diffs are meaningless for these. Document in README how to regenerate or where to find them.

### Sensitive Data

Never commit patient medical data, personally identifiable information (PII), API keys or credentials, or passwords.

### Confidential Research

If your project is pre-publication and confidential, use a **private repository** on GitHub/GitLab:

```bash
git remote add origin git@github.com:mylab/unpublished-project.git
# Repository is private; only invited collaborators can access
```

## Best Practices: Research Git Workflow

1. **Initialize early:** Start a project with `git init` on day one
2. **Commit often:** Don't wait for "finished" work. Commit incremental progress
3. **Meaningful messages:** Document *why* you made changes
4. **Use branches:** Experiment without breaking the main analysis
5. **Tag releases:** Mark versions submitted to journals, published versions, etc.
6. **Ignore appropriately:** Use `.gitignore` for raw data and large intermediate files
7. **Protect main:** Use GitHub rules to prevent accidental commits to main
8. **Document decisions:** In commit messages and in ANALYSIS_PLAN.md or README

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Branching and Merging

### Step 1: Create Two Feature Branches

```bash
# From main, create an analysis branch
git checkout -b feature/add-analysis
cat > src/analyze.py << 'EOF'
"""Example analysis script."""
print("Running analysis...")
EOF
git add src/analyze.py
git commit -m "Add initial analysis script"

# Go back to main, create a test branch
git checkout main
git checkout -b feature/add-tests
cat > src/test_analyze.py << 'EOF'
"""Tests for analysis."""
def test_imports():
    import src.analyze
    print("Import successful!")
EOF
git add src/test_analyze.py
git commit -m "Add test suite"
```

### Step 2: Explore and Merge

```bash
# View all branches and history
git log --graph --all --decorate --oneline

# Merge analysis branch to main
git checkout main
git merge feature/add-analysis
git log --oneline
```

What does the graph view show about how branches relate?

:::::::::::::::::::::::::::::::::::::: solution

## Solution

```bash
$ git log --graph --all --decorate --oneline
* h8i9j0k (HEAD -> feature/add-tests) Add test suite
| * d4e5f6g (feature/add-analysis) Add initial analysis script
|/
* a1b2c3d (main) Initialize project structure

$ git checkout main
$ git merge feature/add-analysis
Updating a1b2c3d..d4e5f6g
Fast-forward
 src/analyze.py | 2 ++
 1 file changed, 2 insertions(+)

$ git log --oneline
d4e5f6g (HEAD -> main) Add initial analysis script
a1b2c3d Initialize project structure
```

The graph view shows how each branch diverged from `main`
independently. The merge brought the analysis branch back into
main via fast-forward, while the tests branch remains separate.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Branches let you explore different analysis approaches without breaking working code
- Tag versions for submissions, revisions, and publications
- Collaborate using shared repositories, feature branches, and pull requests
- Git history demonstrates reproducibility: others can see exactly what changed and when
- Don't track large data, binary files, or sensitive information in Git

::::::::::::::::::::::::::::::::::::::::::::::
