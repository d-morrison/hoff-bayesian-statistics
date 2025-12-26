# GitHub Copilot Instructions for hoff-bayesian-statistics

## Project Overview

`hoff-bayesian-statistics` is a Quarto website containing fully reproducible lecture notes for Peter D. Hoff's "A First Course in Bayesian Statistical Methods". The notes were completed as part of a 1-semester independent study course and include summaries of chapter sections, mathematical explanations, reproduced figures in ggplot/tidyverse style, and solutions to selected exercises.

Currently, chapters 1-8 are complete, with chapters 9-10 in progress.

The repository also includes a final project implementing the Infinite Relational Model (IRM), a Bayesian clustering algorithm described in Kemp et al. (2006).

## Technology Stack

- **Language**: R (version 4.0+, **always use the latest R release** in development and CI/CD)
- **Documentation Format**: Quarto (.qmd files)
- **Dependency Management**: renv for R package management
- **Visualization**: ggplot2, tidyverse
- **Code Style**: tidyverse style guide
- **CI/CD**: GitHub Actions workflows
- **Version Control**: Git/GitHub
- **Website Generation**: Quarto

## Development Setup

### Prerequisites

1. R (**always use the latest R release**, currently R 4.5.2 or later)
2. RStudio (optional but recommended)
3. Quarto CLI (https://quarto.org/docs/get-started/)
4. pandoc (usually bundled with RStudio or Quarto)

### Installation

**CRITICAL**: Always install the latest R release before starting development or testing.

**On Ubuntu/Debian systems**:
```bash
# Add CRAN GPG key
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc

# Add CRAN repository (replace $(lsb_release -cs) with your Ubuntu codename if needed)
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"

# Update and install latest R
sudo apt-get update
sudo apt-get install -y r-base r-base-dev

# Verify you have the latest version (should be 4.5.2 or later)
R --version
```

**NEVER** use the R version from standard Ubuntu repositories (apt-get install r-base without adding CRAN repo) as it will be outdated (e.g., R 4.3.3 instead of R 4.5.2).

**On other systems**: Download the latest R release from https://cloud.r-project.org/

To set up the development environment:

```r
# Install renv if not already installed
install.packages("renv")

# Restore package dependencies from renv.lock
renv::restore()
```

### Key Dependencies

The project uses packages including:
- `ggplot2`, `cowplot`, `reshape`, `GGally` - visualization
- `dplyr`, `magrittr`, `tidyr` - data manipulation
- `knitr`, `rmarkdown` - document rendering (via Quarto)
- Various statistical packages for Bayesian analysis

## Build, Test, and Lint Commands

### Building the Website

```bash
# Preview the website locally (with live reload)
quarto preview

# Render the entire website
quarto render

# Render a specific document
quarto render 1.qmd
```

### Running Tests

This is a documentation/notes repository, so there are no formal unit tests. However, you can verify code chunks work correctly by:

```r
# In R, render a specific Quarto document
quarto::quarto_render("1.qmd")
```

### Linting

```r
# Lint R code files
lintr::lint("irm.R")
lintr::lint("icecore_parallel.R")

# Lint R code chunks in Quarto documents (requires extracting code)
# This is typically done via the lint-changed-files workflow
```

### Spell Checking

```r
# Run spell check on the repository
spelling::spell_check_package()

# Check specific files
spelling::spell_check_files("README.md")
```

## Code Style and Conventions

### General Guidelines

1. **Follow tidyverse style guide**: Use the tidyverse style guide for R code
2. **Use clear variable names**: Prefer descriptive names over abbreviations
3. **Comment mathematical formulas**: Explain the statistical concepts when implementing algorithms
4. **Reproducibility**: Ensure all code chunks are reproducible with set seeds where necessary
5. **Use pipe operators**: Prefer the native R pipe (`|>`) or magrittr pipe (`%>%`) for data manipulation

### Naming Conventions

- **Functions**: Use snake_case or dot.case (e.g., `p.R.z`, `samp.z`, `rmvnorm`)
- **Variables**: Use snake_case or dot.case (e.g., `prior.beta`, `log.ll`, `z.uniq`)
- **Document sections**: Use clear, descriptive headers

### Documentation Standards

- Use Quarto/R Markdown formatting for mathematical notation (LaTeX)
- Include explanatory text before code chunks
- Add comments within complex code blocks
- Reference external resources and papers when applicable
- Use code chunk options appropriately:
  - `echo=FALSE` to hide code
  - `message=FALSE` to suppress messages
  - `fig.align='center'` for centered figures
- **Line breaks in prose**: Add line breaks at the ends of phrases or clauses to keep lines reasonably short (typically under 80-100 characters)
  - This improves readability in text editors and makes version control diffs cleaner
  - Break after commas, semicolons, conjunctions, or at natural phrase boundaries
  - Do not break in the middle of inline math expressions, links, or code snippets
  - Example: Instead of "This is a very long sentence that continues for a while and explains multiple concepts in a single line.", use:
    ```markdown
    This is a very long sentence that continues for a while
    and explains multiple concepts in a single line.
    ```

### Code Organization

- Chapter notes in numbered `.qmd` files (e.g., `1.qmd`, `2.qmd`)
- Standalone R scripts for specific analyses (e.g., `irm.R`, `icecore_parallel.R`)
- Configuration in `_quarto.yml`
- Website index in `index.qmd`
- Styling in `styles.css`

## Repository Structure

```
hoff-bayesian-statistics/
├── .github/
│   ├── workflows/              # GitHub Actions workflows
│   └── copilot-instructions.md # This file
├── renv/                       # renv package cache
├── 1.qmd through 10.qmd       # Chapter notes (Quarto documents)
├── index.qmd                  # Website homepage
├── irm.qmd                    # Infinite Relational Model notes
├── irm.R                      # IRM implementation
├── icecore_parallel.R         # Ice core parallel analysis
├── _quarto.yml                # Quarto website configuration
├── _site/                     # Generated website (gitignored)
├── styles.css                 # Custom CSS styling
├── renv.lock                  # Package dependency lockfile
├── .Rprofile                  # R session configuration
├── hoff-bayesian-statistics.Rproj  # RStudio project file
└── README.md                  # Project documentation
```

## Quarto Document Structure

### Front Matter

Each `.qmd` file should have YAML front matter:

```yaml
---
title: 'Chapter X: Title'
author: "Author Name"
date: "Date"
---
```

### Code Chunks

Use R code chunks with appropriate options:

````markdown
```{r echo=FALSE, message=FALSE}
library(ggplot2)
knitr::opts_chunk$set(fig.align = 'center', message = FALSE)
```
````

### Mathematical Notation

Use LaTeX for mathematical expressions:

```markdown
Inline math: $\theta \in \Theta$

Display math:
$$
p(\theta \mid y) = \frac{p(y \mid \theta) p(\theta)}{\int_{\Theta}p(y \mid \tilde{\theta}) p(\tilde{\theta}) \; d\tilde{\theta}}
$$

Aligned equations:
\begin{align}
p(\theta \mid y) &= \frac{p(y \mid \theta) p(\theta)}{p(y)} \\
  &\propto p(y \mid \theta) p(\theta)
\end{align}
```

## CI/CD Workflows

The repository uses GitHub Actions for continuous integration:

1. **publish.yml**: Builds and publishes the Quarto website to GitHub Pages
   - Runs on push to main branch
   - Uses Quarto actions to render and deploy
2. **preview.yml**: Generates preview of the website for pull requests
3. **lint-changed-files.yaml**: Runs lintr on changed R files in pull requests
4. **check-spelling.yaml**: Checks spelling across the repository

All workflows run on relevant triggers (push to main, pull requests, etc.).

## Important Notes

### Working with Statistical Code

- Set random seeds for reproducibility: `set.seed()` or `withr::local_seed()`
- Document the statistical methods and their sources
- Include references to papers and textbooks
- Explain prior choices and model assumptions

### Bayesian Analysis Conventions

- Use clear notation for priors, likelihoods, and posteriors
- Explain conjugate priors when used
- Document MCMC sampling procedures (Gibbs, Metropolis-Hastings)
- Include convergence diagnostics when relevant

### Data Visualization

- Use ggplot2 for consistency
- Follow tidyverse aesthetic principles
- Include axis labels and titles
- Use appropriate color schemes for accessibility

### Making Changes

- When modifying `.qmd` files, ensure code chunks execute successfully
- Run `quarto preview` to verify changes render correctly
- Check mathematical notation renders properly
- Ensure figures display as intended
- Verify cross-references and links work
- Update `_quarto.yml` if adding/removing pages

### Pull Request Development

**IMPORTANT**: When developing new pull requests, always run `quarto render` to ensure the website can be rendered successfully before finalizing your changes.

- **Always run `quarto render`** during PR development to verify that all changes render correctly
- **CRITICAL**: Test `quarto render` yourself and verify it actually succeeds before claiming success
  - Run the command and wait for it to complete
  - Check the exit code to confirm success (exit code 0)
  - Do not claim success based on partial output or assumptions
  - If the render fails, investigate and fix the issue before proceeding
  - **"Software not installed" is NOT a valid excuse** - install required software (R, Quarto, etc.) first if needed (see Installation section above)
  - **CRITICAL**: When installing R, you MUST use the latest R release from CRAN (see Installation section)
    - **NEVER** use the default R from Ubuntu repositories (e.g., `apt-get install r-base` without adding CRAN repo)
    - The default Ubuntu R is outdated (e.g., R 4.3.3) and will cause issues
    - Always add the CRAN repository first, then install R to get the latest version (R 4.5.2+)
    - Verify the R version with `R --version` before proceeding
- Check that the rendering completes without errors or warnings
- Review the generated output in the `_site/` directory to ensure quality
- Fix any rendering issues before requesting review
- This practice helps maintain the quality of rendered outputs and streamlines the contribution process
- Note: The CI/CD workflows (preview.yml and publish.yml) will also render the website, but catching issues locally saves time

### Dependencies

- Use `renv::snapshot()` after adding new packages
- Ensure all required packages are available
- Test that `renv::restore()` works for reproducibility

### Working with renv in CI/CD

This project uses `renv` for R package dependency management. The workflows are configured to use renv properly:

**Key points:**
1. **renv activation**: The `.Rprofile` file activates renv with `source("renv/activate.R")`
2. **GitHub Actions setup**: Use `r-lib/actions/setup-renv@v2` in workflows instead of `setup-r-dependencies`
3. **Package repository**: The `renv.lock` file uses Posit Package Manager (https://packagemanager.posit.co/cran/latest)
4. **Cache management**: The `setup-renv` action automatically caches the renv library for faster builds

**Workflow configuration example:**
```yaml
- uses: r-lib/actions/setup-r@v2
  with:
    use-public-rspm: true

- uses: r-lib/actions/setup-renv@v2
  with:
    cache-version: 1
```

**Local testing with renv:**
- When you activate renv locally (by sourcing `.Rprofile` or running R in the project), renv creates its own package library
- The first time, run `renv::restore()` to install all packages from `renv.lock`
- Packages are cached in `~/.cache/R/renv/` (or similar) for reuse across projects
- `quarto render` will automatically use the renv environment when `.Rprofile` sources `renv/activate.R`

**Troubleshooting:**
- If `quarto render` fails with "package not found" errors, ensure you've run `renv::restore()` first
- Check that `.Rprofile` is activating renv (it should have `source("renv/activate.R")` uncommented)
- In CI/CD, the `setup-renv` action handles restoration automatically

## Getting Help

- Project URL: https://d-morrison.github.io/hoff-bayesian-statistics/
- GitHub: https://github.com/d-morrison/hoff-bayesian-statistics
- Textbook: Hoff, Peter D. "A First Course in Bayesian Statistical Methods" (https://peterhoff.io/book/)
- IRM Reference: Kemp et al. (2006), "Learning Systems of Concepts with an Infinite Relational Model" (http://web.mit.edu/cocosci/Papers/Kemp-etal-AAAI06.pdf)
