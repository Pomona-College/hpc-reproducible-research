---
title: "Sharing Your Research"
teaching: 25
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I license my research code?
- How do I publish code and data for maximum impact?
- What are persistent identifiers and why do they matter?
- How do I make research citable?

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Choose appropriate licenses for research code
- Understand data repositories and persistent identifiers (DOIs)
- Publish research with proper attribution and citation
- Use CITATION.cff to make your work citable

::::::::::::::::::::::::::::::::::::::::::::::::::

## The Publication Problem

A researcher publishes a paper with interesting findings. The paper is read by thousands; it's cited in grant proposals and follow-up studies. But then:

- A collaborator wants to reproduce the results and asks for code
- A graduate student wants to build on the work and needs the data
- Five years later, the researcher needs to remember what they did

In each case, good documentation is essential. Yet many published papers lack available code, explanation of how to run it, documentation of data provenance, or a license specifying how others can use the work.

## Licensing Your Research

Software licenses tell users what they can do with your code, whether they must share improvements, and your liability (usually, you have none).

### Common Licenses for Research

**MIT License**: Most permissive
- Allow commercial use, modification, distribution
- Only requirement: include license and attribution
- **Good for:** Most academic research

**Apache 2.0**: Permissive with patent protection
- Like MIT, but includes explicit patent license
- **Good for:** Projects with concerns about patent litigation

**GPL (General Public License)**: Restrictive
- Requires derivative works to also be open source
- **Good for:** When you want improvements shared back

**Creative Commons BY 4.0**: For data and documentation
- For research data and written documentation (not software)
- Requires attribution

### Choosing a License

Use [choosealicense.com](https://choosealicense.com/):
- Want commercial use of your work? -- MIT or Apache 2.0
- Want to protect against patents? -- Apache 2.0
- Want others to share improvements? -- GPL
- Licensing data (not code)? -- CC-BY 4.0

### How to Add a License

Create a `LICENSE` file in your repository root, choose a license, and reference it in your README.

## Publishing for Reproducibility: Beyond GitHub

### Step 1: GitHub Repository

Your research code should live on GitHub (or GitLab). But GitHub repositories can disappear if the account is deleted.

### Step 2: Zenodo Archive

**Zenodo** (zenodo.org) is a free repository run by CERN that archives your code and data permanently, assigns a persistent identifier (DOI), indexes your work in search engines, and preserves it for decades.

To archive on Zenodo:
1. Create a GitHub release tag
2. Sign in to Zenodo with your GitHub account
3. Enable Zenodo integration for your repository
4. Create a release on GitHub
5. Zenodo automatically archives it and creates a DOI

### Step 3: Data Repositories

Raw data and large results belong in a data repository:

- **Zenodo**: General-purpose, free, 50 GB per dataset
- **Figshare**: Good for figures and supplementary materials
- **Dryad**: Specialized for research data, integrates with journals
- **Field-Specific**: NCBI GEO (genomics), OpenNeuro (imaging), PANGAEA (climate)

### Step 4: DOI and Citation

Once published on Zenodo, your work gets a **DOI** (Digital Object Identifier) -- a permanent, citable identifier. Include it in your paper:

```
Code and data available: doi: 10.5281/zenodo.7654321
```

## CITATION.cff File

Modern repositories use **CITATION.cff** files to specify how to cite your project:

```yaml
cff-version: 1.2.0
title: Gene Expression Analysis Pipeline
authors:
  - family-names: Smith
    given-names: Alice
    affiliation: University of California
date-released: 2024-03-05
version: 1.0.0
doi: 10.5281/zenodo.7654321
repository-code: https://github.com/mylab/gene-expression
license: MIT
keywords:
  - gene expression
  - reproducible research
```

GitHub automatically reads this and displays citation options.

## Open Science Practices

### Preprints

Share your results before formal publication on bioRxiv (biomedical), medRxiv (medicine), or arXiv (physics, CS, math). This establishes priority, gets early feedback, and makes your work open immediately.

### Open Access Publishing

Publish in open-access journals so your work is freely accessible. Higher citation rates, more impact. Many granting agencies now require open-access publication.

::::::::::::::::::::::::::::::::::::: challenge

## Exercise: Create a CITATION.cff

Create a `CITATION.cff` file for a project (real or hypothetical) with:
- Your name and affiliation
- A title and version
- A license
- At least two keywords

Then verify that GitHub would display it correctly by checking the YAML is valid.

:::::::::::::::::::::::::::::::::::::: solution

## Solution

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

After pushing to GitHub, confirm that the repository page shows
"Cite this repository" and generates a proper BibTeX entry from
the CITATION.cff metadata.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Choose an appropriate license (MIT for most research code, CC-BY for data)
- Archive code on Zenodo (or similar) to get a permanent DOI
- Publish data in appropriate repositories with clear access instructions
- Use CITATION.cff to specify how your work should be cited
- Include DOI and reproducibility statements in published papers

::::::::::::::::::::::::::::::::::::::::::::::
