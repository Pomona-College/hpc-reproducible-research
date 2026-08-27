---
title: "Multi-Step SLURM Workflows"
teaching: 30
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I integrate Make with SLURM for automated job submission?
- How do I ensure HPC workflows are reproducible?
- How do I monitor and debug multi-job workflows?
- How do I make pipeline reruns idempotent?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Integrate Make with SLURM for automated, reproducible job submission
- Capture environment state and input checksums for reproducibility
- Monitor and debug multi-job workflows on Sagehen HPC
- Make pipelines idempotent so reruns do not waste compute

::::::::::::::::::::::::::::::::::::::::::::::::::

## Why Multi-Step Pipelines Need a Pipeline Tool

Real research workflows almost never fit in one SLURM script. Preprocessing, training, and analysis each have different resource needs (CPU vs GPU, memory, time), and they must run in order. Doing this by hand means submitting one job, checking its status, manually submitting the next, and so on. The first time you forget to submit step 3 because you went to bed at 2 AM, you understand why pipeline tools exist.

Make is the simplest tool for this on HPC. It tracks dependencies between targets, only re-runs what has changed, and can submit SLURM jobs as part of building a target. The result: one `make` command that submits the whole pipeline, with proper dependencies between steps, and re-running it skips already-finished work.

## Integration: Make Submitting SLURM Jobs

The most reproducible approach: use Make to orchestrate SLURM job submission.

```makefile
# HPC-aware Makefile for gene expression pipeline

PYTHON := python3
# --parsable makes sbatch print ONLY the job ID (not "Submitted batch job 12345"),
# so the .jid files below contain something --dependency=afterok: can actually use.
SLURM := sbatch --parsable
SLURM_ARGS := --time=04:00:00 --cpus-per-task=8 --mem=32G

# Track job completion
JOB_DIR := .jobs
PREPROCESS_JOB := $(JOB_DIR)/preprocess.jid
ANALYSIS_JOBS := $(JOB_DIR)/analysis.jid
MERGE_JOB := $(JOB_DIR)/merge.jid
FIGURES_JOB := $(JOB_DIR)/figures.jid

.PHONY: all clean clean-all help status

all: $(FIGURES_JOB)
	@echo "Workflow submitted. Use 'make status' to check progress."

$(JOB_DIR):
	mkdir -p $(JOB_DIR)

# Step 1: Preprocess (must run first)
$(PREPROCESS_JOB): $(JOB_DIR) | data/raw/data.csv
	@echo "Submitting preprocessing job..."
	$(SLURM) $(SLURM_ARGS) --output=logs/preprocess.log \
		--job-name=preprocess \
		scripts/preprocess.sh > $@
	@echo "Preprocessing JID: $$(cat $@)"

# Step 2: Analysis (depends on preprocessing, runs in parallel)
$(ANALYSIS_JOBS): $(PREPROCESS_JOB)
	@echo "Submitting analysis jobs..."
	PJID=$$(cat $(PREPROCESS_JOB)) && \
	$(SLURM) $(SLURM_ARGS) --output=logs/analysis_%a.log \
		--job-name=analysis \
		--array=1-100 \
		--dependency=afterok:$$PJID \
		scripts/analyze.sh > $@

# Step 3: Merge results (depends on all analyses)
$(MERGE_JOB): $(ANALYSIS_JOBS)
	@echo "Submitting merge job..."
	AJID=$$(cat $(ANALYSIS_JOBS)) && \
	$(SLURM) $(SLURM_ARGS) --output=logs/merge.log \
		--job-name=merge \
		--dependency=afterok:$$AJID \
		scripts/merge.sh > $@

# Step 4: Generate figures (depends on merge)
$(FIGURES_JOB): $(MERGE_JOB)
	@echo "Submitting figures job..."
	MJID=$$(cat $(MERGE_JOB)) && \
	$(SLURM) $(SLURM_ARGS) --output=logs/figures.log \
		--job-name=figures \
		--dependency=afterok:$$MJID \
		scripts/figures.sh > $@
```

Two ideas in this Makefile do the heavy lifting. First, each step's "target" is a small file (`.jobs/preprocess.jid`) containing the SLURM job ID, not the actual results. Make tracks these JID files and uses them to express dependencies. Second, each step's `sbatch` call uses `--dependency=afterok:$$PJID` to wait for the previous step to finish before starting. SLURM enforces the ordering; Make just submits them in the right sequence.

The result: `make` submits the whole pipeline in seconds and returns. SLURM runs the jobs in dependency order over the next several hours. You can `make status` any time to see where things stand.

## Monitoring Multi-Job Workflows

Add a `status` target to your Makefile:

```makefile
status:
	@echo "=== Pipeline Status ===" && \
	squeue -u $$USER -h --format="%.10i %.10j %.2t" | \
		grep -E "preprocess|analyze|merge|figures" || \
		echo "(No jobs found)"

clean:
	rm -rf results/ .jobs

clean-all: clean
	rm -rf logs/

help:
	@echo "Reproducible HPC Pipeline" && \
	echo "  make           Submit full pipeline" && \
	echo "  make status    Check job status" && \
	echo "  make clean     Remove results"
```

`make status` shows the SLURM queue filtered to just this pipeline's jobs. Useful for at-a-glance "where am I in the pipeline?" without parsing `squeue` output by hand.

The `clean` target removes results and the `.jobs` directory; running `make` again resubmits the whole pipeline from scratch. `clean-all` also removes logs, useful when starting truly fresh.

::::::::::::::::::::::::::::::::::::: callout

## Idempotent reruns

A pipeline rerun should be safe: rerunning `make` after a partial completion should pick up where things stopped, not redo everything.

The Makefile pattern above is mostly idempotent because Make checks file modification times. If `.jobs/preprocess.jid` exists and is newer than the script, Make skips that step.

For full idempotence, the underlying scripts must also be idempotent. A preprocessing script that always overwrites `processed.csv` is idempotent. One that appends to a log file is not (rerunning duplicates the entries). Design scripts to write fresh outputs and never accumulate state across runs.

::::::::::::::::::::::::::::::::::::::::::::::::

## Reproducibility in HPC Workflows

To ensure HPC workflows are reproducible:

### Document Environment

```bash
# In job scripts
module list > logs/modules_$(date +%Y%m%d).txt
conda env export > logs/environment_$(date +%Y%m%d).yml
```

Six months later when a reviewer asks "which version of Python produced these results?", the captured `environment.yml` is the answer.

### Record Exact Inputs

```bash
# Archive input data state
find data/processed -name "*.csv" -exec md5sum {} \; > logs/input_checksums.txt
```

If someone replaces `data.csv` with a different version a year later, the saved checksums let you detect that the inputs are no longer the ones the original results were derived from.

### Include Computation Metadata

```bash
# In your results files, record provenance
echo "Generated: $(date)" >> results/metadata.txt
echo "Hostname: $(hostname)" >> results/metadata.txt
echo "SLURM Job ID: $SLURM_JOB_ID" >> results/metadata.txt
echo "Git commit: $(git rev-parse HEAD)" >> results/metadata.txt
```

The git commit hash is essentially mandatory. Without it, "the code that produced result.csv" is undefined six months later when the repo has moved on.

### Verify Reproducibility

```bash
# Re-run same job with same inputs
sbatch job_script.sh
# Compare outputs to original run
diff original_results.csv new_results.csv
```

If the diff is non-empty, your pipeline has a non-determinism somewhere (random seeds not pinned, parallel order affecting numerics, environment drift). Tracking these down is tedious but worthwhile for any result you plan to publish.

## Example: Complete Reproducible HPC Pipeline

A real research project structure:

```
project/
├── README.md
├── Makefile
├── environment.yml
├── scripts/
│   ├── preprocess.sh
│   ├── analyze.sh
│   ├── merge.sh
│   └── figures.sh
├── src/
│   ├── preprocess.py
│   ├── analyze.py
│   ├── merge.py
│   └── visualize.py
├── data/
│   ├── raw/README.md
│   └── processed/
├── results/
└── logs/
```

The Makefile:

```makefile
.PHONY: all status clean help

JOB_DIR := .jobs

all: $(JOB_DIR)/figures.jid
	@echo "Full pipeline submitted to SLURM"
	@echo "Track progress: make status"

$(JOB_DIR):
	@mkdir -p $(JOB_DIR) logs

$(JOB_DIR)/preprocess.jid: $(JOB_DIR) data/raw/*.csv
	@sbatch --job-name=preprocess \
		--output=logs/preprocess.log \
		--time=02:00:00 --mem=16G \
		scripts/preprocess.sh > $@

$(JOB_DIR)/analyze.jid: $(JOB_DIR)/preprocess.jid
	@PJID=$$(cat $<) && \
	sbatch --job-name=analyze \
		--array=1-100 \
		--output=logs/analyze_%a.log \
		--time=01:00:00 --mem=8G \
		--dependency=afterok:$$PJID \
		scripts/analyze.sh > $@

$(JOB_DIR)/merge.jid: $(JOB_DIR)/analyze.jid
	@AJID=$$(cat $<) && \
	sbatch --job-name=merge \
		--output=logs/merge.log \
		--dependency=afterok:$$AJID \
		scripts/merge.sh > $@

$(JOB_DIR)/figures.jid: $(JOB_DIR)/merge.jid
	@MJID=$$(cat $<) && \
	sbatch --job-name=figures \
		--output=logs/figures.log \
		--dependency=afterok:$$MJID \
		scripts/figures.sh > $@

status:
	@squeue -u $$USER -h --format="%.10i %.10j %.2t" | \
		grep -E "preprocess|analyze|merge|figures" || \
		echo "(No jobs found)"

clean:
	@rm -rf results/ .jobs

help:
	@echo "  make           Submit full pipeline"
	@echo "  make status    Check job status"
	@echo "  make clean     Remove results"
```

::::::::::::::::::::::::::::::::::::: callout

## Beyond Make: Snakemake and Nextflow

For more sophisticated pipelines (hundreds of steps, conditional branches, integration with cluster scheduling beyond SLURM), specialized workflow managers like Snakemake (Python-based) and Nextflow (Groovy-based) are worth considering. Both have native SLURM integration and richer dependency tracking than Make.

The tradeoff is complexity: Make is six lines of Makefile per step, with no extra runtime. Snakemake adds a Python-based DSL and a runtime daemon. Use Make until you outgrow it; only adopt a workflow manager when the project is large enough to justify the learning curve.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Design a Multi-Step HPC Workflow

Sketch a Makefile for an HPC pipeline with these steps:

1. **Preprocess** -- 1 job, 2 hours, 16 GB RAM
2. **Analyze** -- 50 parallel jobs (array), 1 hour each, 8 GB RAM
3. **Summarize** -- 1 job, depends on all analysis jobs

For each step, specify:

- SLURM resource requests
- Dependency relationships
- Input and output verification in the job scripts

:::::::::::::::::::::::::::::::::::::: solution

## Solution

```makefile
JOB_DIR := .jobs

$(JOB_DIR)/preprocess.jid:
	@mkdir -p $(JOB_DIR) logs
	sbatch --time=02:00:00 --mem=16G \
		--job-name=preprocess \
		--output=logs/preprocess.log \
		scripts/preprocess.sh > $@

$(JOB_DIR)/analyze.jid: $(JOB_DIR)/preprocess.jid
	@PJID=$$(cat $<) && \
	sbatch --time=01:00:00 --mem=8G \
		--job-name=analyze --array=1-50 \
		--output=logs/analyze_%a.log \
		--dependency=afterok:$$PJID \
		scripts/analyze.sh > $@

$(JOB_DIR)/summarize.jid: $(JOB_DIR)/analyze.jid
	@AJID=$$(cat $<) && \
	sbatch --time=00:30:00 --mem=4G \
		--job-name=summarize \
		--output=logs/summarize.log \
		--dependency=afterok:$$AJID \
		scripts/summarize.sh > $@
```

Each job script should include `set -euo pipefail`, input file
checks (`[ ! -f "$INPUT" ] && exit 1`), and output verification.
The dependency chain ensures preprocess finishes before analysis
starts, and all 50 analysis jobs finish before summarize runs.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Audit a Pipeline for Reproducibility

You inherit a colleague's `Makefile` that produces `results/figure_3.pdf`. List four things you would check or add to make the pipeline fully reproducible six months later when they have left the lab.

:::::::::::::::::::::::::::::::::::::: solution

## Solution

1. Verify that an `environment.yml` (or similar) is committed and matches what the pipeline actually uses. Recreate the environment and check that running `make` produces identical output.

2. Check that input data is either committed (small) or has documented provenance with checksums (large). Replace any "find this CSV on Box somewhere" notes with explicit paths and md5 hashes.

3. Add git commit hashes to result file metadata so future-you can map any result back to the code that produced it.

4. Run the pipeline twice and `diff` the outputs. Any non-zero diff means there is non-determinism that would prevent reproduction; pin the offending random seed or sort order.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Integrate Make with SLURM for automated, reproducible job submission
- Track job IDs in files so downstream jobs can declare dependencies
- Use `--dependency=afterok:JID` so SLURM enforces ordering between steps
- Capture environment state, input checksums, and compute metadata for reproducibility
- Make HPC workflows trackable with clear logging and a `make status` target
- Idempotent reruns require both Make's modification-time tracking and idempotent scripts
- A complete HPC pipeline includes scripts, src, data, results, and logs directories
- Verify reproducibility by re-running and diffing outputs; non-zero diff means hidden non-determinism

::::::::::::::::::::::::::::::::::::::::::::::
