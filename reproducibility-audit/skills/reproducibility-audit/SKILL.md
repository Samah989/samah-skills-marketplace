---
name: reproducibility-audit
description: Audit a research repository or project folder for computational reproducibility  -  environment pinning (renv, uv, poetry, conda), random-seed usage, data provenance, hard-coded paths, notebook cell-order integrity, README completeness, code-data linkage, container/Dockerfile hygiene, and CI verification. Produces a severity-ranked report with specific fixes so a stranger (or your future self) can rerun the analysis in six months. Trigger this skill whenever the user mentions reproducibility, reproducible research, "run this analysis," "why does my analysis give different results," pinned dependencies, environment.yml, renv.lock, uv.lock, Dockerfile audit, notebook hygiene, code review for reproducibility, sharing my code with reviewers, or asks whether a repository is "publication-ready." Also trigger when a user is preparing code for peer review, journal submission with data-availability statements, or funder-mandated open-science compliance.
---

# Reproducibility Audit

Author: Samah Alshrief · https://github.com/Samah989

Audit a research repository for computational reproducibility and produce a severity-ranked fix list. Applies to Python, R, Julia, and mixed-language projects; extra checks for Jupyter/Quarto notebooks; container-hygiene checks for Docker/Apptainer.

## When to use this skill

- The user says "reproducibility," "reproducible research," "can I hand this off," "why does this give different numbers on your machine"
- The user is preparing a repo for peer review, journal submission with a data-availability statement, or funder-mandated open-science compliance
- The user has a notebook that runs top-to-bottom on their machine but breaks anywhere else
- The user asks about environment pinning (renv, uv, poetry, conda, pixi), Dockerfile hygiene, or CI setup for a research project
- The user mentions journals like *Biostatistics*, *AJPS*, *QSS*, *Journal of Statistical Software*, or *Nature*'s reproducibility policy

## Guiding principles

**Reproducibility is not a feature you add at the end.** It's a property that emerges from a handful of small habits: pinning versions, setting seeds, avoiding absolute paths, running the notebook top-to-bottom regularly, and writing a README that lists every command needed to go from `git clone` to `results/final_table.csv`. The audit's job is to name the specific habit each finding maps to.

**Rank findings by "how badly does this block someone else."** A missing seed on a small deterministic calculation is minor. A hardcoded `/Users/alice/project/data.csv` on the very first cell is a blocker  -  nobody else can run the code past line 1.

**Distinguish reproducibility from replicability.** The Turing Way's definitions matter here:
- **Reproducible**: same data + same code = same results (this skill's target).
- **Replicable**: same procedure, different data or different code = compatible findings.
- **Robust**: same procedure, sensitivity checks = same substantive conclusions.
- **Generalizable**: findings extend to other populations/settings.

This skill audits the first. Say so to the user.

## Workflow

### 1. Intake

Ask for:
1. Path to the repo (or the user connects a folder).
2. Primary language(s) (Python, R, Julia, mix).
3. Notebook system if any (Jupyter, Quarto, R Markdown).
4. Compute environment (local laptop, HPC/OSG, cloud, containerized).
5. Data location  -  in-repo, external repository (Zenodo, GEO, ICPSR), or restricted access.
6. Intended reproducibility bar  -  "peer reviewer can rerun" vs "regulator can rerun in 2035."

### 2. Enumerate the checks

Walk through each of the sections below against the repo. For every issue found, record: **file:line, category, severity, explanation, fix**.

### Category A  -  Environment pinning (blocking-tier)

The single most-violated principle. Every project must pin its dependencies with exact versions.

- **Python**
  - Preferred (2026): **`uv`** with `pyproject.toml` + `uv.lock`. Modern, fast, lockfile is deterministic.
  - Also acceptable: **Poetry** (`pyproject.toml` + `poetry.lock`), **pip-tools** (`requirements.in` + `requirements.txt` with hashes), **conda** with `environment.yml` pinned exactly, or **Pixi** for conda-forge/PyPI hybrids.
  - **Not acceptable alone**: an unpinned `requirements.txt` with only package names (`pandas`, `numpy`); versions drift and results change.
- **R**
  - Preferred: **`renv`**  -  `renv.lock` pins every package to a version and repository snapshot.
  - Acceptable: **`rig` + `renv`**, or **`groundhog`** for date-anchored installs.
  - Not acceptable: freeform `install.packages(...)` in the code.
- **Julia**  -  `Project.toml` + `Manifest.toml` (Manifest is the pin).
- **System-level**  -  CUDA versions, BLAS variants, GDAL/proj, latex distributions. These belong in a Dockerfile or Apptainer definition (see Category F).

### Category B  -  Random seeds and stochasticity

- Every stochastic step (random splits, bootstrap, Monte Carlo, model init) must set a seed. Python: `np.random.default_rng(seed)`. R: `set.seed()`. Torch: `torch.manual_seed()` plus CUDA-specific seeds if GPU.
- Beware NumPy/SciPy operations that use hidden randomness (e.g., some `sklearn` cross-validators).
- If GPU: results are usually not bit-exact reproducible even with seeds; note this in the README rather than pretending otherwise.

### Category C  -  Paths and file I/O (blocking-tier)

- No absolute paths. `~/Downloads/data.csv` breaks on every machine that isn't the author's.
- Use `pathlib.Path(__file__).parent / "data" / "input.csv"` (Python) or `here::here("data", "input.csv")` (R) so paths resolve relative to the project root.
- No hidden `os.chdir` calls buried in a notebook  -  they make cell order load-bearing in confusing ways.
- Data that lives outside the repo should be documented with URL + version/DOI + a `download_data.sh` (or equivalent) so anyone can retrieve it.

### Category D  -  Notebook hygiene

Notebooks are the most common source of "works on my machine" bugs.

- Cells must execute top-to-bottom without error. Check for out-of-order execution counts (`[1]`, `[3]`, `[2]`, `[5]` = red flag) or empty cells.
- No hidden global state carried over from a manual cell run.
- Prefer parameterization via `papermill` (Jupyter) or Quarto parameters over hardcoded constants.
- Long-running or resource-heavy cells should be extractable into a `.py`/`.R` script that the notebook imports.
- Consider **Marimo** or **Quarto** over classic Jupyter for research code  -  they don't have out-of-order-execution bugs by design.

### Category E  -  Data provenance and versioning

- Every input dataset has: source (URL/DOI), version/date, retrieval script or documented manual step, license.
- Intermediate artifacts have a script that regenerates them from raw data.
- Consider **DVC** (Data Version Control), **git-lfs**, or **git-annex** for large binary data. The pattern: raw data in an external repo (Zenodo, S3), a tiny lockfile in git.
- The `data/` directory should have a README listing every file, its schema, and its origin.

### Category F  -  Containers

- If a Dockerfile exists: pin the base image to a specific digest (`FROM python@sha256:...`), not `python:3.11-slim` which changes over time.
- Layer order matters  -  copy lockfiles + install deps *before* copying source code, so rebuilds cache correctly.
- Apptainer/Singularity `.def` files: pin the bootstrap image similarly.
- Rocker (`rocker/verse:4.4.1`) is a great starting point for R projects.
- For OSPool: prefer OSG-VO base images with a versioned tag; pull-through cache via CVMFS.

### Category G  -  README and one-command reproducibility

The README must include, at minimum:
- Project purpose (2-3 sentences)
- Requirements (Python 3.11, R 4.4, ~8 GB RAM, GPU or not)
- Exact `uv sync` / `renv::restore()` / `docker build` commands
- One-liner to reproduce the full pipeline (e.g., `make all`, `snakemake --cores 4`, `bash run.sh`)
- Expected outputs and where they land
- Approximate runtime and disk footprint
- Citation and license

If the user has to email the author to figure out how to run the code, the README fails.

### Category H  -  Pipeline orchestration

- For anything beyond a single notebook, use a workflow manager: **Snakemake**, **Nextflow**, **Make**, **just**, **Prefect**, **Airflow**, or R's **`targets`** package.
- The workflow file becomes the executable documentation of the analysis.
- Pin the workflow-manager version too (Snakemake >= 8.4, Nextflow >= 24.10, etc.).

### Category I  -  Continuous integration

- A CI job that runs the pipeline (or a smoke-test subset) on every commit catches drift immediately.
- GitHub Actions is the default. For heavy compute, trigger a smoke test in CI and a full run manually on HPC.
- Rerunning the CI job monthly on `main` catches silent breakage from upstream dependency updates.

### Category J  -  Documentation of decisions

- Every analytic choice deserves an inline comment: why this preprocessing step, why this hyperparameter, why this test set split.
- A `CHOICES.md` or `NOTES.md` capturing debates and dead ends is under-used but hugely valuable when returning to a project after a year.

## Severity tiers

- **Blocker**  -  Nobody can run the code as-is (hardcoded paths, no dependency file, private data with no obtainment instructions).
- **High**  -  Runs on one machine but not reliably reproducible (unpinned versions, no seeds on stochastic steps, notebook with out-of-order cells).
- **Medium**  -  Runs but is fragile (partial pinning, weak README, no CI, hardcoded constants).
- **Low**  -  Best-practice gap (no `CHOICES.md`, missing citation, license unclear, no container).

## Output format

Produce a report using exactly this structure:

```markdown
# Reproducibility audit  -  <repo name>
_Date: <YYYY-MM-DD>_ · _Standard: Turing Way + Journal of Open Source Science checklists_

## Summary
- **Overall grade**: <one line, e.g., "Not reproducible as-is. 3 blockers must be fixed before external users can rerun.">
- **Blockers**: <count>  ·  **High**: <count>  ·  **Medium**: <count>  ·  **Low**: <count>
- **Estimated remediation effort**: <e.g., "1 day for blockers, 2-3 days for full remediation">
- **What you're auditing to**: <the target  -  peer-reviewer rerun / regulator-grade / journal-of-open-source-science>

## Blockers (fix first)

### 1. <Short title>
- **Category**: <A-J>
- **File**: `<path:line>`
- **Finding**: <what's wrong>
- **Why it blocks**: <plain-English explanation of who this affects>
- **Fix**:
  ```
  <exact code or command>
  ```

## High-severity findings
## Medium-severity findings
## Low-severity findings

## Passed checks (spot-check confirmation)
- <bulleted list of things you deliberately checked and found solid>

## What you couldn't audit
- <e.g., "GPU determinism (no GPU available)", "container build (no Docker on audit machine)", "external data availability (deposit is under embargo)">

## Recommended next steps
1. <priority-ordered actions>
```

## Reference checklists to align with

- **The Turing Way  -  Guide for Reproducible Research**: <https://the-turing-way.netlify.app/reproducible-research>
- **Journal of Open Source Software (JOSS) review checklist**: <https://joss.readthedocs.io/en/latest/review_checklist.html>
- **Rescience C** submission checklist
- **NSF DMSP** and **NIH DMSP** artifact expectations
- **ACM Reproducibility Badges**  -  Artifacts Available, Artifacts Evaluated (Functional / Reusable), Results Reproduced

## Tone and delivery

- Be concrete. Every finding names a file, a line, a fix.
- Distinguish "won't run on your machine" (blocker) from "runs but might drift in a year" (high).
- If the repo is close to reproducible, say so  -  a graded verdict with three fixes is far more useful than a wall of yellow flags.
- Recommend a workflow manager (Snakemake, Nextflow, `targets`) once a project has more than a handful of intermediate artifacts.
- Frame the audit as "protecting your future self and your citations." Reproducibility isn't for the reviewer  -  it's for you six months from now trying to figure out which script generated Figure 3.
