---
title: "Putting It All Together"
teaching: 20
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- What documentation makes research reproducible?
- How do I create a complete reproducibility package?
- What checklist should I follow before publication?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Write comprehensive documentation for a research project
- Create a complete reproducibility package ready for publication
- Apply the reproducibility checklist to your own work

::::::::::::::::::::::::::::::::::::::::::::::::::

## The Complete README

The centerpiece of research documentation is the README. A publication-ready README contains these sections:

### 1. Overview and Quick Start

````markdown
# Gene Expression Analysis Pipeline

Analysis of differential gene expression in Alzheimer's disease.

## Quick Start

```bash
git clone https://github.com/mylab/gene-expression.git
cd gene-expression
conda env create -f environment.yml
conda activate gene-analysis
bash data/raw/download.sh
make all
```

Analysis completes in ~4 hours. Results are in `results/`.
````

### 2. Methods and Results

Detailed explanation of analysis approach, software versions, and summary of findings with pointers to output files.

### 3. Project Structure, Citation, and License

Help users navigate the layout. Make it easy for others to cite your work. Clearly state how others can use it.

## Reproducibility Statement

Include in your paper or supplementary material:

```
## Reproducibility Statement

This analysis was conducted using reproducible computational methods.
All code is available at [GitHub URL] and archived at [Zenodo DOI].
Raw data is available from [Data Repository]. Analysis was performed
on Linux using Python 3.11 with software versions specified in
environment.yml.

All results can be reproduced by:
1. Obtaining raw data (see methods)
2. Setting up environment: conda env create -f environment.yml
3. Running: make all

Computation time: ~4 hours on a standard laptop.
```

## A Reproducible Research Checklist for Publication

Before submitting your paper or releasing your code:

- [ ] **Code is version controlled** in Git with meaningful commit history
- [ ] **README explains** project, how to run it, what to expect
- [ ] **Environment file** (environment.yml or requirements.txt) specifies all dependencies
- [ ] **Data is documented** with README explaining provenance, location, access
- [ ] **License is explicit** (MIT, Apache 2.0, GPL, or CC-BY for data)
- [ ] **Results are regeneratable**: Running code produces same outputs
- [ ] **Code is archived** on Zenodo or similar with DOI
- [ ] **DOI is cited** in paper and supplementary materials
- [ ] **Citation format** is provided (CITATION.cff or bibtex)
- [ ] **Instructions are tested**: Another person can follow them
- [ ] **Documentation includes** methods, software versions, computational requirements
- [ ] **Raw data** is accessible with clear instructions for obtaining it
- [ ] **Analysis steps** are documented and automated (Makefile or equivalent)

## Example: Complete Documentation Package

A publication-ready research project:

```
my-research/
├── README.md              # Main documentation
├── CITATION.cff           # Citation metadata
├── LICENSE                # MIT License
├── METHODS.md             # Detailed methods
├── RESULTS.md             # Results interpretation
│
├── environment.yml        # Reproducible environment
├── environment-lock.yml   # Pinned versions for maximum reproducibility
├── Makefile               # Automation
│
├── data/
│   └── raw/README.md      # Data source and access instructions
│
├── src/                   # Analysis code
│   ├── analysis.py
│   └── utils.py
│
├── results/               # Generated outputs
│   ├── figures/
│   └── tables/
│
└── .gitignore
```

When published with this documentation, your research is discoverable, citable, and reproducible.

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Create a Complete Documentation Package

Create a complete documentation package for a hypothetical or real research project.

### Step 1: Write a README

Create `README.md` with at least these sections:

- Overview (2-3 sentences)
- Quick Start (requirements, installation, run)
- Methods (brief explanation of analysis)
- Results (summary of findings)
- License

### Step 2: Create a LICENSE

```bash
curl https://opensource.org/licenses/MIT | head -20 > LICENSE
echo "Copyright (c) 2024 [Your Name]" >> LICENSE
```

### Step 3: Create CITATION.cff

```yaml
cff-version: 1.2.0
title: [Your Project]
authors:
  - family-names: [Last Name]
    given-names: [First Name]
date-released: 2024-03-05
license: MIT
keywords:
  - reproducible research
```

### Step 4: Verify

```bash
git add README.md LICENSE CITATION.cff
git commit -m "Add documentation and license"
```

On GitHub, verify:

- README displays on repository home
- License is recognized
- CITATION.cff provides citation format


:::::::::::::::::::::::::::::::::::::: solution

## Solution

A completed documentation package should contain these three files:

**README.md:**

````markdown
# Sagehen Climate Trend Analysis

Analysis of 30-year temperature trends in Southern California
using station data from NOAA.

## Quick Start

```bash
git clone https://github.com/mylab/climate-trends.git
cd climate-trends
conda env create -f environment.yml
conda activate climate-trends
make all
```

Results appear in `results/` (~10 minutes).

## Methods

Linear regression on monthly mean temperatures (1993-2023).
Significance tested with Mann-Kendall trend test (p < 0.05).

## Results

Warming trend of +0.3C per decade (p = 0.002).
See `results/figures/trend_plot.pdf`.

## License

MIT License (see LICENSE)
````

**CITATION.cff:**
```yaml
cff-version: 1.2.0
title: Sagehen Climate Trend Analysis
authors:
  - family-names: Smith
    given-names: Alice
    affiliation: Pomona College
date-released: 2025-03-15
version: 1.0.0
license: MIT
keywords:
  - climate
  - reproducible research
```

**Verify on GitHub:**
```bash
$ git add README.md LICENSE CITATION.cff
$ git commit -m "Add documentation and license"
[main f1a2b3c] Add documentation and license
 3 files changed, 45 insertions(+)
```

After pushing, confirm that GitHub displays the README on the
repository page and shows "Cite this repository" from CITATION.cff.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Comprehensive README is the foundation of research documentation
- Include overview, quick start, methods, results, and license in your README
- A reproducibility checklist ensures nothing is missed before publication
- The complete package includes README, LICENSE, CITATION.cff, environment.yml, and Makefile
- Checklist before publication ensures your research is discoverable, citable, and reproducible

::::::::::::::::::::::::::::::::::::::::::::::
