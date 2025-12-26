# hoff-bayesian-statistics

These are (fully reproducible!) Quarto lecture notes for [Peter D. Hoff, "A
First Course in Bayesian Statistical
Methods"](https://www2.stat.duke.edu/~pdh10/FCBS/), completed as
part of a 1-semester independent study course. Only Chapters 1-8 are complete right now.

Each note includes summaries of chapter sections, with math and explanations modified to better fit my understanding and the occasional link to external resources. I also reproduce many figures in the book in a ggplot/tidyverse style, and tackle some of the exercises at the end of each chapter (correctness not guaranteed).

If you find an error or would like to improve the notes, please let me know/submit a PR!

## Building the Website

This repository is now a Quarto website. To build and preview it:

1. Install [Quarto](https://quarto.org/docs/get-started/)
2. Run `quarto preview` to preview the website locally
3. Run `quarto render` to build the static website (output in `_site/`)

## Contributing

When submitting a pull request:

1. Make your changes to the relevant `.qmd` files
2. **Always run `quarto render`** to ensure the website can be rendered successfully
3. Check that the rendering completes without errors or warnings
4. Review the generated output to verify your changes appear as intended
5. Submit your PR once you've confirmed successful rendering

This helps maintain the quality of the website and streamlines the review process.

## Previous R Markdown Version

Previously, these notes were individual R Markdown files that could be knitted in RStudio. They have been converted to Quarto format (.qmd files) which are compatible with both RStudio and Quarto.

As a small final project, I also implemented R code for the basic binary
relation version of the Infinite Relational Model, described in [Kemp et al.
(2006), "Learning Systems of Concepts with an Infinite Relational
Model"](https://www.andrew.cmu.edu/user/ckemp/papers/kempetal06.pdf).
