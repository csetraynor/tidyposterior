# `tidyposterior`

[![Travis build status](https://travis-ci.org/topepo/tidyposterior.svg?branch=master)](https://travis-ci.org/topepo/tidyposterior)
[![Coverage status](https://codecov.io/gh/topepo/tidyposterior/branch/master/graph/badge.svg)](https://codecov.io/github/topepo/tidyposterior?branch=master)
[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/tidyposterior)](http://cran.r-project.org/web/packages/tidyposterior)
[![Downloads](http://cranlogs.r-pkg.org/badges/tidyposterior)](http://cran.rstudio.com/package=tidyposterior)
![](https://img.shields.io/badge/lifecycle-maturing-blue.svg)

This package can be used to conduct _post hoc_ analyses of resampling results generated by models. 

For example, if two models are evaluated with the root mean squared error (RMSE) using 10-fold cross-validation, there are 10 paired statistics. These can be used to make comparisons between models without involving a test set. 

There is a rich literature on the analysis of model resampling results such as McLachlan's [_Discriminant Analysis and Statistical Pattern Recognition_](https://books.google.com/books?id=O_qHDLaWpDUC&lpg=PR7&ots=6GJnIREXZM&dq=%22Discriminant%20Analysis%20and%20Statistical%20Pattern%20Recognition%22&lr&pg=PR7#v=onepage&q=%22Discriminant%20Analysis%20and%20Statistical%20Pattern%20Recognition%22&f=false) and the references therein. This package follows _the spirit_ of [Benavoli _et al_ (2017)](http://people.idsia.ch/~marco/papers/2017jmlr-tests.pdf). 

`tidyposterior` uses Bayesian generalized linear models for this purpose and can be considered an upgraded version of the [`caret::resamples`](https://topepo.github.io/caret/model-training-and-tuning.html#exploring-and-comparing-resampling-distributions) function. The package works with [`rsample`](https://topepo.github.io/rsample/index.html) objects natively but any results in a data frame can be used. 

## Installation

You can install `tidyposterior` from github with:

``` r
# install.packages("devtools")
devtools::install_github("topepo/tidyposterior")
```

## Example

``` r
library(tidyposterior)
# See ? precise_example
data(precise_example)

# Get classification accuracy results for analysis

library(dplyr)
accuracy <- precise_example %>%
   select(id, contains("Accuracy")) %>%
   setNames(tolower(gsub("_Accuracy$", "", names(.)))) 
accuracy

# Model the accuracy results
acc_model <- perf_mod(accuracy, seed = 13311, verbose = FALSE)   

# Extract posterior distributions:
accuracy_dists <- tidy(acc_model)

# Credible intervals for accuracy per model
summary(accuracy_dists)
```
