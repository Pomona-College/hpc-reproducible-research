---
title: "Conda Environment Management"
teaching: 25
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- What is a computational environment?
- Why do software versions matter for reproducibility?
- How do I create and export conda environments?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand what comprises a computational environment
- Explain why "it works on my machine" happens
- Create and export conda environments
- Use environment files for reproducibility

::::::::::::::::::::::::::::::::::::::::::::::::::

## The "Works on My Machine" Problem

A researcher publishes code and data accompanying a paper. A graduate student tries to run it:

```bash
$ python analyze.py
ModuleNotFoundError: No module named 'tensorflow'
```

They install TensorFlow, but results don't match. The paper reports accuracy of 97.3%; they get 96.8%. After hours of debugging, they discover the paper used TensorFlow 2.8.0 while they installed 2.13.0 -- a bug fix changed numerical behavior slightly.

This is the "works on my machine" problem. Your analysis depends not just on your code, but on a specific configuration of software. Sharing code without sharing environment is like sharing a recipe but not specifying ingredient quantities.

## What Is a Computational Environment?

Your computational environment includes everything needed to run your code:

1. **Programming language version**: Python 3.11 vs 3.9 (backward compatibility isn't guaranteed)
2. **System libraries**: The version of BLAS, LAPACK, GSL that your compiled code links to
3. **Package versions**: numpy 1.24 vs 1.20 (API changes, numerical algorithms change)
4. **Operating system**: Linux vs Mac vs Windows (path separators, line endings, floating-point implementations)
5. **Environment variables**: Paths to data, API keys, configuration
6. **Compiler versions**: GCC 9 vs GCC 12 (optimization flags produce different binaries)
7. **Module system** (on HPC): Which modules are loaded (whether OpenMP, MPI, CUDA are available)

### Why Versions Matter

Consider `pandas.read_csv()`. Between versions:
- **pandas 1.3**: parses dates automatically if they look like dates
- **pandas 1.4**: by default, dates are NOT parsed automatically
- **pandas 1.5**: date parsing is more lenient, accepts more formats

Your code `df = pd.read_csv("data.csv")` produces a datetime column on one environment and a string column on another. Downstream code breaks or produces different results.

## Conda: Managing Environments

**conda** is a package and environment manager. It manages not just Python packages, but also system libraries and other dependencies.

conda comes in two flavors:
- **Anaconda**: Full distribution with hundreds of pre-installed packages (larger download)
- **Miniconda**: Minimal distribution, you install what you need (recommended for HPC)

On Sagehen, conda is available via:
```bash
module load conda
```

### Creating an Environment

```bash
conda create -n my-project python=3.11 numpy pandas scipy
```

This creates a directory (in `~/.conda/envs/my-project/`) containing Python 3.11 and the specified packages with compatible versions. Activate it:

```bash
conda activate my-project
```

Your shell prompt changes (shows `(my-project)` prefix), and `python`, `pip`, etc. now refer to this isolated environment.

### Exporting Your Environment

When you're happy with your analysis, export the environment specification:

```bash
conda env export --no-builds | grep -v "^prefix" > environment.yml
```

This creates a file like:

```yaml
name: my-project
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11.7
  - numpy=1.24.3
  - pandas=2.0.1
  - scipy=1.10.1
  - matplotlib=3.7.1
  - scikit-learn=1.2.2
  - pip
  - pip:
    - tensorflow==2.8.0
    - pytest==7.3.1
```

### Sharing Environments

Include `environment.yml` in your repository. Someone else can recreate your exact environment:

```bash
conda env create -f environment.yml
conda activate my-project
```

### Pinning Versions: Flexibility vs. Reproducibility

A practical approach:

1. **During development:** Be flexible
   ```yaml
   dependencies:
     - python=3.11
     - numpy>=1.20
   ```

2. **Before publishing:** Pin exact versions
   ```yaml
   dependencies:
     - python=3.11.7
     - numpy=1.24.3
   ```

3. **For future-proofing:** Include both loose and strict versions in separate files (`environment.yml` and `environment-lock.yml`)

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Export Your Environment

### Step 1: Create an Environment
```bash
conda create -n my-analysis python=3.11 numpy pandas scipy matplotlib
conda activate my-analysis
```

### Step 2: Install Additional Packages
```bash
pip install scikit-learn jupyter
```

### Step 3: Export
```bash
conda env export --no-builds | grep -v "^prefix" > environment.yml
```

### Step 4: Test Recreation
```bash
conda deactivate
conda env create -f environment.yml -n test-import
conda activate test-import
python -c "import numpy, pandas, scipy; print('Success!')"
```

:::::::::::::::::::::::::::::::::::::: solution

## Solution

Expected output from each step:

```bash
$ module load conda
$ conda create -n my-analysis python=3.11 numpy pandas scipy matplotlib
Solving environment: done
Proceed ([y]/n)? y

$ conda activate my-analysis
(my-analysis) $ pip install scikit-learn jupyter
Successfully installed scikit-learn-1.2.2 jupyter-...

(my-analysis) $ conda env export --no-builds | grep -v "^prefix" > environment.yml
(my-analysis) $ cat environment.yml
name: my-analysis
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11.7
  - numpy=1.24.3
  - pandas=2.0.1
  ...

$ conda deactivate
$ conda env create -f environment.yml -n test-import
$ conda activate test-import
(test-import) $ python -c "import numpy, pandas, scipy; print('Success!')"
Success!
```

The key verification: the `test-import` environment runs identically
to `my-analysis`, confirming your `environment.yml` is complete.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Your computational environment includes language versions, package versions, system libraries, and OS configuration
- "It works on my machine" happens when environments differ; reproducibility requires capturing your environment
- conda is the recommended tool for research; it manages Python packages, R packages, and system dependencies
- Always export your environment with `conda env export` and include it in your repository
- Pin exact versions before publishing for maximum reproducibility

::::::::::::::::::::::::::::::::::::::::::::::
