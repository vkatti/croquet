
<!-- README.md is generated from README.Rmd. Please edit that file -->

# croquet

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/croquet)](https://CRAN.R-project.org/package=croquet)
<!-- badges: end -->

A collection of clinical research organization (CRO) miscellaneous
functions developed for [The Prostate Cancer Clinical Trials Consortium
(PCCTC)](http://pcctc.org/).

## Installation

⚠️ This package is under active development! Feedback is welcome,
consistency is not yet promised.

You can install the development version of croquet from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("pcctc/croquet")
```

## Exporting labelled data to excel

Brief variable names are useful for coding, but might not provide
sufficient context for collaborators. Here, we add variable labels data
and export those labels to an excel sheet to make content more readily
digestible.

### Libraries

``` r
library(croquet)
library(openxlsx)

# format all date and date time variables in the excel output 
options("openxlsx.dateFormat" = "yyyy-mm-dd")
options("openxlsx.datetimeFormat" = "yyyy-mm-dd")
```

### Example data

``` r
dat1 <- tibble::tibble(
  var_1 = 1:3,
  var_2 = LETTERS[1:3],
  var_3 = Sys.Date() - 0:2
  ) %>%
  labelled::set_variable_labels(
    var_1 = "Variable 1 (numeric)",
    var_2 = "Variable 2 (character)",
    var_3 = "Variable 3 (date)"
  )

dat2 <- tibble::tibble(
  var_1 = 4:6,
  var_2 = LETTERS[4:6],
  var_3 = Sys.Date() - 0:2
  ) %>%
  labelled::set_variable_labels(
    var_1 = "Variable 1 (numeric)",
    var_2 = "Variable 2 (character)",
    var_3 = "Variable 3 (date)"
  )
```

### Export single sheet

``` r
# initialize workbook
wb <- createWorkbook()

# default settings name sheet by name of input data
add_labelled_sheet(dat1)

# you can rename sheet to something more meaningful
add_labelled_sheet(dat1, "example sheet")

saveWorkbook(wb, "check-wb-1.xlsx")
```

### Export multiple sheets

``` r
# create named list
out <- tibble::lst(dat1, dat2)

# initialize workbook
wb <- createWorkbook()

# create labelled sheets from all input data
add_labelled_sheet(out)

saveWorkbook(wb, "check-wb-2.xlsx")
```

<img src="man/figures/check-wb-2.PNG" title="Shows two tabs named dat1 and dat2. Row 1 has light gray italics text and white background; row 2 has a black background and white text." alt="Shows two tabs named dat1 and dat2. Row 1 has light gray italics text and white background; row 2 has a black background and white text." width="100%" />

## Importing labelled data from excel

Needs more documentation!

This imports a single sheet that assumes variables labels are in row 1
and variable names are row 2. You can optionally specify a regex
expression for `date_detect` to identify variables that should be
explicitly imported as a date.

``` r
dat <- read_labelled_sheet(
  path = here::here(path, dsn1),
  sheet = "ae_listings",
  date_detect = "cyc1_visdat|cyc2_visdat"
)
```
