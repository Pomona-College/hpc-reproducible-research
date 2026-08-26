---
title: "Advanced Make Patterns"
teaching: 25
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- What are pattern rules and automatic variables in Make?
- How do I use Make variables effectively?
- When should I use Make vs. other workflow tools?
- How do I debug Makefiles?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use Make variables for DRY (Don't Repeat Yourself) code
- Apply pattern rules and automatic variables for complex pipelines
- Debug common Makefile errors
- Understand when to use Make vs. other workflow tools

::::::::::::::::::::::::::::::::::::::::::::::::::

## Pattern Rules and Automatic Variables

For complex projects with many similar steps, use **pattern rules**:

```makefile
# Convert all CSV files to JSON
%.json: %.csv
	python src/csv_to_json.py $< -o $@
```

This single rule means any `.csv` file can be converted to `.json`.

### Special Variables

- `$@`: The target
- `$<`: The first prerequisite
- `$^`: All prerequisites
- `$?`: Prerequisites newer than target

Example:

```makefile
# Merge multiple data files
data/merged.csv: data/raw/file1.csv data/raw/file2.csv src/merge.py
	python src/merge.py $^ -o $@
```

## A Complete Advanced Makefile

```makefile
# Reproducible analysis pipeline
# Usage: make              (run entire pipeline)
#        make clean        (clean results)
#        make help         (show help)

PYTHON := python3
SHELL := /bin/bash
.SHELLFLAGS := -eu -o pipefail -c

# Input and output files
RAW_DATA := data/raw/data.csv
PROCESSED_DATA := data/processed/data_clean.csv
ANALYSIS_RESULTS := results/analysis_results.csv
FIGURES := results/figures/main_plot.pdf results/figures/secondary.pdf
TABLES := results/tables/stats.txt

# Default target
.PHONY: all
all: $(FIGURES) $(TABLES)

# Download data (only if doesn't exist)
$(RAW_DATA):
	@echo "Downloading data..."
	mkdir -p data/raw
	$(PYTHON) src/download_data.py -o $@

# Clean data
$(PROCESSED_DATA): $(RAW_DATA) src/clean_data.py
	@echo "Cleaning data..."
	mkdir -p data/processed
	$(PYTHON) src/clean_data.py $< -o $@

# Run analysis
$(ANALYSIS_RESULTS): $(PROCESSED_DATA) src/analyze.py
	@echo "Running analysis..."
	mkdir -p results
	$(PYTHON) src/analyze.py $< -o $@

# Generate figures
$(FIGURES): $(ANALYSIS_RESULTS) src/visualize.py
	@echo "Generating figures..."
	mkdir -p results/figures
	$(PYTHON) src/visualize.py $< -o results/figures/

# Generate summary table
$(TABLES): $(ANALYSIS_RESULTS) src/summarize.py
	@echo "Generating summary table..."
	mkdir -p results/tables
	$(PYTHON) src/summarize.py $< -o $@

# Clean targets
.PHONY: clean clean-all help
clean:
	rm -rf results/

clean-all: clean
	rm -rf data/processed/

help:
	@echo "Reproducible Analysis Pipeline"
	@echo "============================="
	@echo ""
	@echo "Available targets:"
	@echo "  make all        - Run entire pipeline (default)"
	@echo "  make clean      - Remove results/"
	@echo "  make clean-all  - Remove results/ and processed data"
	@echo "  make help       - Show this message"
```

### Using the Pipeline

```bash
# First run: does everything
make

# Change a script, rerun -- only modified steps are re-run
make

# Clean and start over
make clean-all
make
```

## Make + HPC: Orchestrating SLURM Jobs

Make can orchestrate not just local commands but job submissions to HPC clusters:

```makefile
SLURM_ARGS := --time=04:00:00 --cpus-per-task=8 --mem=32G

.PHONY: submit-processing
submit-processing:
	sbatch $(SLURM_ARGS) --job-name=process \
		--output=logs/process.log \
		src/process.sh data/raw/samples.csv data/processed/

.PHONY: submit-analysis
submit-analysis: submit-processing
	sbatch $(SLURM_ARGS) --job-name=analyze \
		--dependency=singleton \
		--output=logs/analyze.log \
		src/analyze.sh data/processed/ results/
```

We'll cover SLURM integration in depth in later episodes.

## Debugging Makefiles

### Recipe doesn't run

Check for TAB vs spaces:

```makefile
target: prereq
	echo "OK"       # TAB - works
    echo "FAIL"     # spaces - doesn't work
```

### Variable not expanding

Makefile variables use `$(VAR)` not `$VAR`:

```makefile
FILES := *.csv
	echo $FILES      # Wrong: prints "$FILES"
	echo $(FILES)    # Right: prints actual file names
```

### Prerequisites not rebuilding

Check file timestamps:

```bash
make -d          # Debug output
make -n          # Dry run (show what would happen)
touch src/script.py  # Update timestamp to force rebuild
```

## When NOT to Use Make

Make is great for simple-to-moderate workflows but has limitations:

- **No built-in parallelization control**: Hard to parallelize interdependent jobs
- **Complex data dependencies**: Hard to express "if any file in this directory changes"
- **Interactive feedback needed**: Make doesn't check for user input mid-workflow

For complex bioinformatics or machine learning pipelines with hundreds of jobs, consider:

- **Snakemake**: Python-based workflow tool, better parallelization
- **Nextflow**: Domain-specific workflow language, excellent HPC support
- **Dask/Luigi**: Python libraries for workflow orchestration

But for most research analyses, Make is simple, effective, and universal (available on virtually every Unix system).

## Tips and Best Practices

1. **Include a `.PHONY: all` target**: Defines the default workflow
2. **Use variables**: Makes your Makefile DRY and maintainable
3. **Add @echo statements**: Provides user feedback on what's happening
4. **Include help target**: Document your pipeline
5. **Separate concerns**: One rule per logical step
6. **Use relative paths**: Makes Makefiles portable
7. **Test your Makefile**: Run `make clean` then `make` to ensure reproducibility
8. **Version your Makefile**: Track it in git like any other source code

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Incremental Rebuilds

Given a working Makefile with targets for `process`, `analyze`, and `visualize`:

1. Run `make` to build everything
2. Touch only `src/visualize.py` (`touch src/visualize.py`)
3. Run `make -n` to see what would be rebuilt

**Question:** Why does Make only re-run the visualization step and not the earlier steps?

Then modify the Makefile to add a `clean-all` target that removes both results and processed data.

:::::::::::::::::::::::::::::::::::::: solution

## Solution

![`make -n` on Sagehen HPC previews every command in the four-stage pipeline without running anything.](fig/08-make-dry-run-pipeline.png){alt='Terminal in a make-demo directory on Sagehen HPC where make -n has been run. The dry-run output lists mkdir and python3 commands for four pipeline stages: cleaning data, analysis, figure generation, and table generation.'}

```bash
$ touch src/visualize.py
$ make -n
python3 src/visualize.py results/analysis.csv results/figure.png
```

Make only re-runs the visualization step because:

- `src/visualize.py` has a newer timestamp than `results/figure.png`
- But `src/analyze.py` has NOT changed, so `results/analysis.csv` is still up-to-date
- And `src/process.py` has NOT changed, so `data/processed/data_clean.csv` is still up-to-date

Make traces the dependency graph and only re-runs targets whose
prerequisites are newer than their outputs.

The `clean-all` target:

```makefile
.PHONY: clean-all
clean-all: clean
	rm -rf data/processed/
```

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Variables, .PHONY targets, and special variables ($@, $<, $^) make Makefiles clean and maintainable
- Pattern rules let you define one rule for many similar transformations
- Make can orchestrate HPC job submissions for parallel processing
- Debug with `make -n` (dry run) and `make -d` (debug output)
- A good Makefile with help and clean targets makes your workflow self-documenting
- For complex pipelines with hundreds of jobs, consider Snakemake or Nextflow

::::::::::::::::::::::::::::::::::::::::::::::
