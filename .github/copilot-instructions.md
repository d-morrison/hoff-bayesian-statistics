# GitHub Copilot Instructions for hoff-bayesian-statistics

> [!IMPORTANT]
> **MANDATORY ENVIRONMENT SETUP - DO THIS FIRST, ALWAYS:**
> 
> At the START of EVERY work session on this repository:
> 1. **IMMEDIATELY install R 4.5.2+, Quarto, TinyTeX** (see Installation section below)
> 2. **Run `renv::restore()`** to install all R packages
> 3. **Verify tools are working** with `quarto --version` and `R --version`
>
> **MANDATORY TESTING BEFORE EVERY COMMIT:**
> 
> Before committing ANY changes to `.qmd`, `.R`, or config files:
> 
> 1. **Run `quarto render` on the FULL repository** (not individual files)
> 2. **Verify it completes successfully** (exit code 0, no errors)
> 3. **Check all 3 output formats exist** in `_site/`: HTML, RevealJS slides, PDF handouts
> 4. **Run linter on changed files**:
>    - For R files: `lintr::lint("path/to/file.R")`  
>    - For .qmd files with R code: Extract and lint R code chunks
>    - **Fix lint issues in code you wrote or modified** - ignore pre-existing issues in unchanged code
> 5. **Run spellcheck**: `spelling::spell_check_package()`
>    - **Fix spelling errors you introduced** - ignore pre-existing errors
>    - Add technical terms to `.aspell/defaults.pws` if needed
> 6. Only then commit your changes
>
> **CRITICAL RULES:**
> - **CI is NOT the test** - you must test locally BEFORE pushing
> - **NEVER rely on CI to discover rendering, lint, or spelling errors** - that's your job
> - **ALWAYS run full `quarto render`** - testing individual files is insufficient
> - **Only fix lint/spell issues in code YOU changed** - don't fix unrelated pre-existing issues
> - **This is a hard requirement - no exceptions, no excuses**

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

### General Principles

**CRITICAL**: Do not make assumptions about what code will do - always test it yourself.

**ENVIRONMENT SETUP IS MANDATORY:**
- At the START of EVERY work session, install R, Quarto, and TinyTeX IMMEDIATELY
- Never start making changes without having the full development environment ready
- CI is for final verification ONLY - you must test locally FIRST
- Your working environment should mirror the CI environment

**MANDATORY WORKFLOW FOR ANY `.qmd` OR CONFIG CHANGES:**
1. **FIRST: Install required tools** (R 4.5.2+, Quarto, TinyTeX) if not already installed
2. **ALWAYS run FULL `quarto render`** on the entire repository before committing
3. **Test individual files only for rapid iteration** - final verification MUST be full render
4. **NEVER commit changes without successful full `quarto render`**
5. **CI is NOT a substitute for local testing** - CI failures mean you failed to test properly

- **Install required software first**: Ensure all necessary tools (R, Quarto, TinyTeX) are installed before starting work
- **Test your changes**: Run the actual commands to verify functionality
- **Run `quarto render` on FULL repository**: Testing individual files misses cross-file issues
- **Verify output**: Check that expected files are created with correct content
- **Never claim success without evidence**: Only report that something works after you've confirmed it yourself

### Prerequisites

1. R (**always use the latest R release**, currently R 4.5.2 or later)
2. RStudio (optional but recommended)
3. Quarto CLI (https://quarto.org/docs/get-started/)
4. pandoc (usually bundled with RStudio or Quarto)
5. **TinyTeX** (required for PDF rendering - see installation below)

### Installation

**CRITICAL**: Always install the latest R release AND all required tools before starting development or testing.

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

**Install Quarto** (required for rendering):

On Ubuntu/Linux:
```bash
# Download and install Quarto (check https://quarto.org/docs/get-started/ for latest version)
wget https://github.com/quarto-dev/quarto-cli/releases/download/v1.4.550/quarto-1.4.550-linux-amd64.deb
sudo dpkg -i quarto-1.4.550-linux-amd64.deb

# Verify installation
quarto --version
```

On macOS/Windows: Download installer from https://quarto.org/docs/get-started/

**Install TinyTeX** (required for PDF rendering):

```bash
# Via Quarto (preferred method)
quarto install tinytex --no-prompt

# Verify installation
quarto list tools
```

Alternative via R:
```r
install.packages("tinytex")
tinytex::install_tinytex()
```

**Install R package dependencies**:

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

**CRITICAL**: Always lint changed files before requesting code review and before committing.

```r
# Lint R code files you modified
lintr::lint("path/to/modified/file.R")

# Examples:
lintr::lint("irm.R")
lintr::lint("icecore_parallel.R")

# Lint all R files in the repository
lintr::lint_dir()

# Lint Quarto documents (lints R code chunks)
lintr::lint("1.qmd")
lintr::lint("2.qmd")
```

**When to lint**:
- Before calling the `code_review` tool
- After making changes to R files (`.R`) or Quarto documents (`.qmd`)
- Before committing code changes

**Linting workflow for changed files**:
1. Identify which files you've changed
2. For `.R` files: Run `lintr::lint("filename.R")` on each changed file
3. For `.qmd` files: Run `lintr::lint("filename.qmd")` on each changed file to lint R code chunks
4. Fix any linting errors or warnings
5. Only then proceed to code review

**Important notes**:
- **Only fix lint issues in code you wrote or modified** - do not fix pre-existing issues in unchanged code
- If lint errors exist in code you didn't change, ignore them - they're not your responsibility
- The lint-changed-files workflow will flag issues, but you should catch them locally first

### Spell Checking

**CRITICAL**: Always run spellcheck locally BEFORE committing.

```r
# Run spell check on the entire repository
spelling::spell_check_package()

# Check specific files
spelling::spell_check_files("README.md")
spelling::spell_check_files("path/to/modified/file.qmd")

# Add technical terms to dictionary if needed
# Edit .aspell/defaults.pws to add project-specific words
```

**Spell checking workflow**:
1. Identify which files you've changed
2. Run `spelling::spell_check_package()` to check all documentation
3. Review any spelling errors found
4. Either fix the spelling errors OR add legitimate technical terms to `inst/WORDLIST` (create this file if it doesn't exist, one word per line)
5. Re-run spell check to verify all errors are resolved
6. Only then proceed to code review or commit

**Important notes**:
- **Only fix spelling errors you introduced** - ignore pre-existing errors in unchanged files
- Add legitimate technical terms (e.g., "Bayesian", "Gibbs", "MCMC") to `.aspell/defaults.pws`
- Don't add typos to the dictionary - fix the typos instead
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

Use R code chunks with Quarto's hashpipe syntax for options:

````markdown
```{r}
#| echo: false
#| message: false

library(ggplot2)
knitr::opts_chunk$set(fig.align = 'center', message = FALSE)
```
````

Note: While the repository currently uses the older R Markdown style (`{r echo=FALSE, message=FALSE}`), the Quarto hashpipe syntax (`#|`) is preferred for new and modified code.

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

### Debugging Workflow Failures

**CRITICAL**: When asked to fix workflow errors or when workflows fail:

1. **ALWAYS** read the workflow logs using GitHub MCP tools
2. Use `list_workflow_runs` to find recent runs
3. Use `get_job_logs` or similar tools to get detailed failure logs
4. **NEVER** assume what the error might be - always verify by reading the actual logs
5. Search for error messages in the logs to identify the root cause
6. Fix the specific error found in the logs, not what you think the error might be

This is a mandatory step - do not skip reading the logs when debugging workflow failures.

### Determining Responsibility for Workflow Failures

**IMPORTANT**: Not all workflow failures are your responsibility to fix.

**You ARE responsible for** fixing workflow failures if:
- The failure is in code or files you directly modified
- The failure is caused by changes you introduced (e.g., new dependencies, configuration changes)
- The error message indicates an issue with YOUR specific changes

**You are NOT responsible for** fixing workflow failures if:
- The failure is in pre-existing code you didn't modify
- Lint errors exist in unchanged files or unchanged sections of files
- Spelling errors exist in files you didn't modify
- The workflow itself has an environment or infrastructure issue (e.g., package installation failures, Docker issues)
- The error occurs in lines of code that existed before your changes

**How to determine responsibility**:
1. Check the workflow logs to identify which files/lines are causing the failure
2. Use `git diff` or `git show` to verify whether you modified those specific lines
3. If you only modified prose (text/comments) but errors are in code blocks, those are pre-existing
4. If the error is "file X line Y" and you didn't touch line Y, it's pre-existing

**What to do**:
- **If it's your responsibility**: Fix the issue and re-run the workflow
- **If it's NOT your responsibility**: Document it in your PR description and notify the repository maintainer
- **Never** fix unrelated pre-existing issues - focus on your changes only

**Example scenarios**:
- ✅ You added a new .bib file and spellcheck fails on a word in that file → Fix it
- ❌ You changed text in a .qmd file and lint fails on existing R code blocks you didn't touch → NOT your responsibility
- ✅ You modified R code and lint fails on your new lines → Fix it  
- ❌ Spellcheck workflow fails due to package installation error → NOT your responsibility (infrastructure issue)

### Validating Rendering Success

**CRITICAL**: Before declaring that rendering works or that fixes are successful:

1. **ALWAYS** test `quarto render` yourself in your working environment
2. Verify that **ALL** output formats are generated successfully:
   - HTML pages (`*.html`)
   - RevealJS slides (`*-slides.html`)  
   - PDF handouts (`*-handout.pdf`)
3. Check that files actually exist in the output directory (`_site/`)
4. Verify file sizes are reasonable (not 0 bytes, not truncated)
5. **NEVER** claim success based on assumptions or partial output
6. **NEVER** declare rendering works without actually testing it

**For this project specifically**: The default `quarto render` command generates HTML, RevealJS, and PDF outputs. All three formats must render successfully for the build to pass.

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

**CRITICAL - MANDATORY TESTING REQUIREMENT:**

**STEP 0 - ENVIRONMENT SETUP (DO THIS FIRST, EVERY SESSION):**
- Install R 4.5.2+, Quarto, TinyTeX if not already installed (see Installation section)
- Run `renv::restore()` to install all R packages
- Verify tools work: `quarto --version`, `R --version`
- **NEVER start making code changes without having the environment ready**

**BEFORE MAKING ANY COMMIT** with `.qmd`, `.R`, or configuration file changes, you MUST:

1. **Run FULL `quarto render`** on the entire repository (not individual files)
2. **Wait for it to complete** - do not interrupt or assume success
3. **Verify exit code is 0** (success) - rendering MUST complete without errors
4. **Check all three output formats** exist in `_site/` for ALL documents:
   - `{filename}.html` (website pages)
   - `{filename}-slides.html` (RevealJS presentations)
   - `{filename}-handout.pdf` (PDF handouts)
5. **Run linter on changed R files**:
   - `lintr::lint("path/to/modified/file.R")` for each modified R file
   - Extract and lint R code from modified .qmd files if you changed code chunks
   - **Only fix lint issues in code you modified** - ignore pre-existing issues
6. **Run spellcheck**:
   - `spelling::spell_check_package()` to check the entire repository
   - **Only fix spelling errors you introduced** - ignore pre-existing errors
   - Add technical terms to `.aspell/defaults.pws` if they are legitimate
7. **Only then** can you commit your changes

**CRITICAL RULES:**
- **CI is NOT the test** - it's final verification only
- **Testing individual files is insufficient** - always do full `quarto render`
- **Lint and spellcheck locally FIRST** - don't wait for CI to discover issues
- **Only fix issues in code YOU changed** - don't fix unrelated pre-existing problems
- **If CI fails, you failed to test properly** - this should never happen
- **No exceptions, no excuses** - this is a hard requirement

Additional guidelines:
- When modifying `.qmd` files, ensure code chunks execute successfully
- Use `quarto render file.qmd` for rapid iteration ONLY
- Always follow up with FULL `quarto render` before committing
- Run `quarto preview` to verify changes render correctly
- Check mathematical notation renders properly (especially in PDF format)
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
- **ALWAYS verify all changed hyperlinks are correct before requesting PR review**:
  - Check that internal links point to existing files/sections
  - Verify external URLs are accessible and correct
  - Test cross-references and anchor links
  - Ensure relative paths are correct
- This practice helps maintain the quality of rendered outputs and streamlines the contribution process
- Note: The CI/CD workflows (preview.yml and publish.yml) will also render the website, but catching issues locally saves time

### Code Review Workflow

**MANDATORY ORDER OF OPERATIONS** before finalizing a pull request:

1. **Lint changed files FIRST**
   - Run `lintr::lint()` on all changed `.R` files
   - Fix any linting errors or warnings
   - See the "Linting" section for detailed commands

2. **Then request code review**
   - Use the `code_review` tool to get automated feedback
   - The tool must be called AFTER linting is complete
   - Review and address any valid comments from the code review

3. **Finally run security checks**
   - Use the `codeql_checker` tool after code review
   - Address any security vulnerabilities found
   - Re-run if you make significant changes

**CRITICAL**: Never call `code_review` without linting changed files first. Linting catches basic style and syntax issues that should be fixed before more comprehensive code review.

**Workflow example**:
```r
# Step 1: Lint changed files
lintr::lint("irm.R")  # Fix any issues found

# Step 2: Then use code_review tool (via GitHub Copilot)
# Step 3: Then use codeql_checker tool (via GitHub Copilot)
```

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

### TinyTeX for PDF Rendering

**CRITICAL**: TinyTeX **MUST** be installed in your working environment when developing PRs for this project, as PDF format is included in the default website rendering.

**Installation**: See the "Installation" section above for TinyTeX installation instructions. This should be done at the start of PR development.

**When PDF output is required**:
- **ALWAYS at the start of PR development** - This is now a required step
- Before rendering PDF output formats
- Before running multi-format rendering that includes PDF
- When you see the error: "No TeX installation was detected"

**Important**: TinyTeX installation requires internet access to GitHub releases and CTAN mirrors. Without TinyTeX, the website rendering will fail when trying to generate PDF handouts.

**Note**: The separate `_quarto-handout.yml` profile exists as an alternative method for PDF rendering and can be used independently.

### Quarto Multi-Format Rendering

This project uses multi-format rendering to generate HTML, RevealJS slides, and PDF handouts simultaneously.

**Default website rendering** (`quarto render`):
- Generates **all three formats** in `_site/` directory:
  - `{filename}.html` - Website page
  - `{filename}-slides.html` - RevealJS presentation
  - `{filename}-handout.pdf` - PDF handout (requires TinyTeX)

**Alternative profile-based rendering**:
1. **RevealJS profile**: `QUARTO_PROFILE=revealjs quarto render` - Generates slides in `_slides/`
2. **PDF handout profile**: `QUARTO_PROFILE=handout quarto render` - Generates PDFs in `_handouts/`

**Implementation approach**:
Following the pattern from https://github.com/perellonieto/quarto_html_revealjs_test:
- **Project-level config** (`_quarto-website.yml`): Defines html, revealjs, and pdf formats
- **File-level frontmatter**: Each .qmd file specifies all three formats with `output-file` for non-html formats:
  ```yaml
  format:
    html: default
    revealjs:
      output-file: {filename}-slides.html
    pdf:
      output-file: {filename}-handout.pdf
  ```
- This generates three separate output files per source file, avoiding naming conflicts

**Key insights**:
- Both formats must be specified at two levels: project configuration AND individual file frontmatter
- The `output-file` parameter is used at the file level to avoid naming conflicts
- See: https://github.com/orgs/quarto-dev/discussions/1751

## Continuous Learning and Improvement

**IMPORTANT**: When you learn new skills, techniques, or encounter solutions to problems while working on this project, **you MUST update this instructions file** to document them for future reference.

This includes:
- New installation procedures or dependencies
- Solutions to rendering or build issues
- Workarounds for technical limitations
- New tools or commands that prove useful
- Configuration patterns that work well for this project type
- Debugging techniques specific to Quarto/R/renv

**How to update**:
1. Identify which section the new information belongs in (or create a new section if needed)
2. Add clear, concise instructions with examples where helpful
3. Include references to external resources (documentation, discussions, issues) when relevant
4. Use `store_memory` tool to save important facts about the codebase for future tasks

This ensures the instructions stay current and helpful for both yourself and other contributors.

## Common Quarto Patterns

### Chunk Options
Use Quarto's hashpipe (`#|`) syntax for chunk options at the top of code blocks:

````markdown
```{r}
#| echo: false
#| message: false
#| warning: false

# Your R code here
```
````

Common chunk options:
- `#| echo: false` - Hide code but show output (for figures)
- `#| include: false` - Run code but hide both code and output (for setup)
- `#| message: false` - Suppress messages
- `#| warning: false` - Suppress warnings
- `#| cache: true` - Cache results for expensive computations (use sparingly)
- `#| fig-width: 7` and `#| fig-height: 5` - Control figure dimensions
- `#| fig-cap: "Caption text"` - Add figure captions

Note: While the old R Markdown style (`{r echo=FALSE, message=FALSE}`) still works, prefer the Quarto hashpipe syntax for new code.

### Cross-References
- Sections: `# Section {#sec-label}` then reference with `@sec-label`
- Figures: Use div syntax with `:::` for cross-referenceable figures:
  ```markdown
  ::: {#fig-name}
  ![](image.png)
  
  Caption text
  :::
  ```
  Then reference with `@fig-name`
- Tables: Use div syntax with `:::` for cross-referenceable tables:
  ```markdown
  ::: {#tbl-name}
  | Col1 | Col2 |
  |------|------|
  | A    | B    |
  
  Caption text
  :::
  ```
  Then reference with `@tbl-name`
- Equations: `$$...$$ {#eq-label}` then reference with `@eq-label`

### Citations and References
- Use BibTeX for references when applicable
- Add `bibliography: references.bib` to YAML front matter
- Cite with `[@key]` for parenthetical or `@key` for textual citations

### Code Folding and Annotation
- The project uses `code-fold: true` by default (configured in `_quarto-website.yml`)
- Override per-document or per-chunk as needed
- Use `#| code-summary: "Descriptive text"` to customize the fold button text

## Common R/Statistics Patterns

### Reproducibility
- **ALWAYS** set seeds for random number generation: `set.seed(123)` or `withr::local_seed(123)`
- Document the seed value used for reproducibility
- Use `sessionInfo()` or `renv::snapshot()` to capture package versions

### Statistical Computing Best Practices
- Compute on log scale when possible to avoid numerical underflow/overflow
  - Example: `sum(log(likelihoods))` instead of `prod(likelihoods)`
- Use vectorized operations instead of loops when possible
- For MCMC: Monitor convergence, check trace plots, report effective sample size
- Document prior distributions clearly in both code comments and prose

### Data Visualization
- Always label axes with units where applicable
- Use colorblind-friendly palettes (e.g., `viridis`, `RColorBrewer`)
- Include informative titles and captions
- Consider both light and dark themes (project supports both)
- Use `theme_minimal()` or `theme_bw()` for clean, professional plots

### Mathematical Notation
- Use consistent notation throughout the document
- Define notation on first use
- Align related equations using `\begin{align}...\end{align}`
- Use `\text{}` for text within math mode
- **LaTeX Macros**: Use https://github.com/d-morrison/macros as a git submodule for more concise LaTeX expressions
  - When helpful new macros are needed, send PRs back to https://github.com/d-morrison/macros to add them
- Common symbols:
  - `\theta` for parameters
  - `\mathcal{Y}` for sample spaces
  - `\mid` for conditioning (not `|`)
  - `\sim` for "distributed as"
  - `\propto` for "proportional to"

## Getting Help

- Project URL: https://d-morrison.github.io/hoff-bayesian-statistics/
- GitHub: https://github.com/d-morrison/hoff-bayesian-statistics
- Textbook: Hoff, Peter D. "A First Course in Bayesian Statistical Methods" (https://peterhoff.io/book/)
- IRM Reference: Kemp et al. (2006), "Learning Systems of Concepts with an Infinite Relational Model" (http://web.mit.edu/cocosci/Papers/Kemp-etal-AAAI06.pdf)
- Quarto Documentation: https://quarto.org/docs/guide/
- R for Data Science: https://r4ds.hadley.nz/
- Bayesian Data Analysis (Gelman et al.): http://www.stat.columbia.edu/~gelman/book/
