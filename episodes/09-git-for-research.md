---
title: "Git for Research Projects"
teaching: 25
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I use Git for research, not just code?
- What should and shouldn't go in version control?
- How do I write meaningful commit messages for research?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Apply version control best practices to research projects
- Understand what to track and what to exclude from Git
- Write meaningful commit messages documenting analysis decisions
- Create a research-appropriate .gitignore

::::::::::::::::::::::::::::::::::::::::::::::::::

## Git for Research: A Different Perspective

Most researchers use Git only for code, if at all. But Git is equally valuable for research data and analysis decisions.

Typically, code goes in Git. But the actual research (the decisions, experiments, versions of analysis) often lives in folders with names like `analysis_v1/`, `analysis_v2_better/`, `analysis_FINAL.txt`, `analysis_REAL_FINAL.txt`.

**This is fragile.** When someone asks, "What changed between v2 and v3?", you can't easily answer. You can't revert to an earlier version if the new one breaks. You can't compare approaches.

Git solves this, but we need to use it differently for research than for software development.

## What Goes in Git?

### YES: Track These

**Code (scripts, notebooks, Makefiles)**
```
src/analyze.py
src/visualize.py
Makefile
```

**Documentation (README, analysis plans)**
```
README.md
ANALYSIS_PLAN.md
METHODS.md
```

**Small configuration files**
```
config.yaml
environment.yml
.gitignore
```

**Results (figures, tables): But carefully**
- Small figures (PDF, PNG): OK to track
- Large figures or datasets: Better stored elsewhere
- Use git lfs (Large File Storage) for anything over a few MB

### NO: Don't Track These

**Raw Data** -- immutable, wastes storage:
```bash
# .gitignore
data/raw/
data/*.csv
*.fasta
*.bam
```

Instead, document where to obtain it in `data/raw/README.md`.

**Processed/Intermediate Data** -- regeneratable:
```bash
# .gitignore
data/processed/
results/
*.tmp
```

**Large Binary Files**:
```bash
*.pkl
*.npy
*.hdf5
*.mat
```

**Machine-Specific and Sensitive Files**:
```bash
.DS_Store
__pycache__/
.env
secrets.yml
api_key.txt
```

Never commit passwords, API keys, or credentials.

## Writing Meaningful Commit Messages

A commit message should explain not just *what* changed, but *why*.

### Bad Commit Messages

```
git commit -m "fix"
git commit -m "update script"
git commit -m "changes"
```

These are useless. Six months later, you won't remember what these were about.

### Good Commit Messages

```
git commit -m "Increase expression threshold from 5 to 10 to reduce noise

Rationale: Initial threshold produced too many low-significance hits.
Consultation with domain expert suggested 10 is standard in field.
Reduces output gene set from 4,500 to 1,200."
```

### Commit Message Best Practices

Structure: **Title (50 chars) + blank line + detailed explanation**

```
Fix edge case in date parsing

The original code assumed all dates were in YYYY-MM-DD format.
However, some rows had MM/DD/YYYY format due to regional settings.
Added format detection to handle both cases.
```

Why is this better?

1. **Searching history is useful:** `git log --grep="date parsing"` finds relevant commits
2. **Understanding evolution:** You understand not just code changes, but the reasoning
3. **Blame context:** When someone runs `git blame`, they see why a line exists

## Using Git History as a Lab Notebook

Your git log *is* a documented record of analysis evolution.

```bash
git log --oneline --all           # View history across all branches
git log --graph --all --decorate  # Visual history with branches
git log -p src/analyze.py         # See all changes to analyze.py with diffs
git log --author="Alice" --since="2024-01-01"  # See Alice's recent work
```

## A .gitignore Template for Research

```bash
# Operating system
.DS_Store
Thumbs.db

# Python
__pycache__/
*.py[cod]
.env
.venv

# R
.Rhistory
.RData

# Data
data/raw/
data/processed/
*.csv
*.xlsx
*.fasta
*.bam

# Large files
*.pkl
*.npy
*.h5
*.hdf5

# Results
results/
*.png
*.pdf

# IDE
.vscode/
.idea/

# Job logs
*.log
slurm-*.out

# Sensitive
secrets.yml
credentials.json
```

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Initialize a Research Project in Git

### Step 1: Create a Project
```bash
mkdir my-research && cd my-research
mkdir src data results
echo "# My Research Project" > README.md
```

### Step 2: Initialize Git
```bash
git init
git config user.name "Your Name"
git config user.email "your.email@institution.edu"
```

### Step 3: Create .gitignore and Commit
```bash
cat > .gitignore << 'EOF'
data/raw/
data/processed/
results/
*.log
__pycache__/
.DS_Store
EOF

git add README.md .gitignore
git commit -m "Initialize project structure

Setup standard directories for data, source code, and results.
Created .gitignore to prevent accidental commits of large data files."
```

### Step 4: Add a Script with a Meaningful Message
```bash
cat > src/analyze.py << 'EOF'
"""Example analysis script."""
print("Running analysis...")
EOF

git add src/analyze.py
git commit -m "Add initial analysis script"
```

Verify your history with `git log --oneline`.

:::::::::::::::::::::::::::::::::::::: solution

## Solution

```bash
$ git init
Initialized empty Git repository in /rhome/username/my-research/.git/

$ git add README.md .gitignore
$ git commit -m "Initialize project structure

Setup standard directories for data, source code, and results.
Created .gitignore to prevent accidental commits of large data files."
[main (root-commit) a1b2c3d] Initialize project structure
 2 files changed, 8 insertions(+)

$ git add src/analyze.py
$ git commit -m "Add initial analysis script"
[main d4e5f6g] Add initial analysis script
 1 file changed, 2 insertions(+)

$ git log --oneline
d4e5f6g Add initial analysis script
a1b2c3d Initialize project structure
```

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Git is valuable for research beyond just code; track analysis decisions, methods, and evolution
- Use meaningful commit messages documenting *why* you made changes
- Avoid tracking: large raw data files, credentials, or binary intermediate results
- Your git log is a lab notebook showing the progression of your research
- .gitignore keeps large/sensitive/irrelevant files out of version control

::::::::::::::::::::::::::::::::::::::::::::::
