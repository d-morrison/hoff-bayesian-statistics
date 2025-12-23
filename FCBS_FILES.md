# Files from Peter Hoff's "A First Course in Bayesian Statistical Methods"

This repository contains all the supplementary files from the book's website: https://www2.stat.duke.edu/~pdh10/FCBS/

## Directory Structure

### Exercises/
Contains data files used in the book's exercises (33 files total):
- `.dat` files with datasets for end-of-chapter exercises
- Examples: `school1.dat` through `school8.dat`, `bluecrab.dat`, `orangecrab.dat`, `swim.dat`, etc.

### Inline/
Contains data files and R code used for inline examples in the book (23 files total):
- Data files referenced in chapter examples
- Short R scripts for specific chapters (`chapter7.R` through `chapter11.R`)
- Various data formats (plain text, R data dumps)
- Special datasets like `vostok.icecore.co2.dat` for ice core analysis

### Misc/
Contains miscellaneous book-related materials (3 files total):
- `bookcover.jpg` - Cover image of the book
- `econjournal_review.pdf` - Review from an economics journal
- `errata.txt` - List of known errors in the book

### Replication/
Contains R code and data for replicating all figures in the book (106 files total):
- `chapter1.R` through `chapter12.R` - Complete R code for each chapter
- `fig*_*.pdf` - PDF versions of all figures from the book
- `.RData` files - Preprocessed datasets used in examples
- Helper functions: `hdr2d.r`, `rlreg.R`, `regression_gprior.R`, `backselect.R`

## Usage

These files are now available locally in the repository and can be referenced directly instead of downloading from the web. For example:

```r
# Old way (requires internet):
school1 = scan('https://www2.stat.duke.edu/~pdh10/FCBS/Exercises/school1.dat')

# New way (local files):
school1 = scan('Exercises/school1.dat')
```

## Original Source

All files were downloaded from: https://www2.stat.duke.edu/~pdh10/FCBS/

Book: Hoff, Peter D. (2009). *A First Course in Bayesian Statistical Methods*. Springer.
