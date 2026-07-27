# Reference

Quick reference for tools and concepts covered in this workshop.

## Git Essentials

### Common Commands

```bash
# Setup
git config user.name "Your Name"
git config user.email "your@email.com"

# Create/clone repository
git init
git clone <url>

# Check status
git status
git log --oneline
git diff

# Stage and commit
git add <file>
git add .
git commit -m "message"

# Branches
git branch <branch-name>
git checkout <branch-name>
git checkout -b <branch-name>
git merge <branch-name>

# Remote
git push origin <branch-name>
git pull origin <branch-name>
git fetch origin

# Tags
git tag v1.0
git tag -a v1.0 -m "Version 1.0"
git push origin --tags
```

### Writing Good Commit Messages

```
Fix bug in data filtering

The filter() function was excluding valid samples with missing
metadata. Changed from strict equality check to hasattr() to allow
samples with optional metadata.

Fixes issue #42.
```

**Format:** Subject line (50 chars) + blank line + detailed explanation

## Conda Essentials

### Common Commands

```bash
# Create environment
conda create -n env-name python=3.11 numpy pandas

# Activate/deactivate
conda activate env-name
conda deactivate

# Install packages
conda install package-name
pip install package-name

# List environments
conda env list

# Export environment
conda env export > environment.yml
conda env export --no-builds | grep -v "^prefix" > environment.yml

# Create from file
conda env create -f environment.yml

# Remove environment
conda env remove -n env-name
```

### environment.yml Template

```yaml
name: my-project
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11
  - numpy=1.24.3
  - pandas=2.0.1
  - scipy=1.10.1
  - matplotlib=3.7.1
  - pip
  - pip:
    - scikit-learn==1.2.2
    - tensorflow==2.8.0
```

## Make Essentials

### Basic Structure

```makefile
# Define variables
VAR_NAME := value

# Target: prerequisites
target: prereq1 prereq2
	command to create target

# Phony targets (not files)
.PHONY: all clean help

all: target1 target2

clean:
	rm -rf output/

help:
	@echo "Available targets:"
	@echo "  make all    - Run everything"
```

### Special Variables

| Variable | Meaning |
|----------|---------|
| `$@` | Current target |
| `$<` | First prerequisite |
| `$^` | All prerequisites |
| `$?` | Prerequisites newer than target |

### Pattern Rules

```makefile
# Convert all CSV to JSON
%.json: %.csv
	python convert.py $< -o $@
```

### Common Recipes

```makefile
# Create directory
results/:
	mkdir -p $@

# Python script
results/output.csv: data/input.csv src/process.py
	python src/process.py $< -o $@

# Multiple inputs
results/merged.csv: data/file1.csv data/file2.csv src/merge.py
	python src/merge.py $^ -o $@
```

## SLURM Essentials

### Job Submission

```bash
# Simple job
sbatch script.sh

# With resource requests
sbatch --time=04:00:00 --cpus-per-task=8 --mem=32G script.sh

# Job array
sbatch --array=1-100 script.sh

# With dependency
sbatch --dependency=afterok:12345 script.sh
```

### SLURM Directives

```bash
#!/bin/bash
#SBATCH --job-name=my-job
#SBATCH --output=logs/%j.log
#SBATCH --error=logs/%j.err
#SBATCH --time=01:00:00
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --array=1-10
#SBATCH --dependency=afterok:12345

# Your script here
```

### Job Management

```bash
# List jobs
squeue
squeue -u $USER

# Check specific job
squeue -j 12345

# Cancel job
scancel 12345

# Job history
sacct
sacct -j 12345 --format=State,ExitCode
```

### Dependency Types

```bash
afterok:JID    # Run after job JID succeeds
after:JID      # Run after job JID finishes (any status)
afterok:J1:J2  # Run after J1 and J2 succeed
afterok:J1,J2  # Run after all array tasks in J1,J2
singleton      # Run after previous job with same name
```

## Project Structure Template

```
project/
├── README.md
├── LICENSE
├── CITATION.cff
├── environment.yml
├── Makefile
│
├── data/
│   ├── raw/README.md
│   └── processed/.gitignore
│
├── src/
│   ├── module1.py
│   ├── module2.py
│   └── utils.py
│
├── results/
│   ├── figures/
│   └── tables/
│
├── notebooks/
│   └── exploration.ipynb
│
├── scripts/
│   ├── preprocess.sh
│   └── analyze.sh
│
└── .gitignore
```

## .gitignore Template

```
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
*.mat

# Results
results/
outputs/

# Python
__pycache__/
*.py[cod]
.env
venv/
env/

# R
.Rhistory
.RData

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
*.swp

# Job logs
*.log
.job_ids
slurm-*.out
```

## Reproducible Analysis Checklist

Before publishing:

- [ ] Code is in version control (git)
- [ ] README explains what the project does and how to run it
- [ ] environment.yml or requirements.txt specifies exact dependencies
- [ ] Makefile or equivalent automates the full analysis pipeline
- [ ] Raw data is documented and accessible (not in repository)
- [ ] All outputs are regeneratable from code
- [ ] LICENSE file specifies how others can use your code
- [ ] Code is archived (Zenodo, Figshare, etc.) with DOI
- [ ] Instructions have been tested by running from scratch
- [ ] Commit messages document analysis decisions
- [ ] .gitignore prevents accidental commits of large files

## Key Concepts

### Reproducibility Levels

1. **Numerical**: Same numbers from same code and data
2. **Logical**: Same conclusions from same methodology
3. **Process**: Understanding of the full workflow and decisions

### Environment Reproducibility

**Problem:** Code works on laptop but not on HPC, or works today but not next year

**Solution:** Lock down:
- Programming language version
- All package versions
- System libraries
- Compiler versions

### Dependency Tracking

```
Raw Data → [Script] → Processed Data → [Script] → Results
   ↑                        ↑                          ↑
   └─────────────────────────────────────────────────┘
              These relationships go in Makefile
```

### Separating Concerns

- **data/raw/**: Original, immutable data (read-only)
- **data/processed/**: Cleaned, prepared data (regeneratable)
- **src/**: Code that transforms data
- **results/**: Output figures, tables, summaries (regeneratable)

## Tools and Resources

| Tool | Purpose | Reference |
|------|---------|-----------|
| git | Version control | https://git-scm.com/ |
| conda | Environment management | https://docs.conda.io/ |
| Make | Workflow automation | https://www.gnu.org/software/make/ |
| SLURM | HPC job scheduling | https://slurm.schedmd.com/ |
| Zenodo | Code archiving, DOI | https://zenodo.org/ |
| OSF | Open Science Framework | https://osf.io/ |

## Useful References

- **The Turing Way**: Handbook for reproducible research: https://the-turing-way.netlify.app/
- **Carpentries Lessons**: Learn Unix, Git, Python: https://carpentries.org/
- **FAIR Data Principles**: Best practices for data: https://www.go-fair.org/
- **Reproducibility Guide**: Nature: https://www.nature.com/articles/d41586-020-02462-5

## Glossary

**Commit**: A version-controlled snapshot of your code

**Branch**: A parallel version of your repository for experiments

**Environment**: Specification of software dependencies and versions

**Deterministic**: Produces same output given same input

**Stochastic**: Involves randomness; controlled via random seeds

**DOI**: Digital Object Identifier; permanent, citable link to your work

**Makefile**: File defining automated analysis workflow steps

**SLURM**: Simple Linux Utility for Resource Management; HPC job scheduler

**Conda**: Package and environment manager for reproducible Python environments

**Repository**: Version-controlled directory (git repo)
