
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ProbSup

<!-- badges: start -->
<!-- badges: end -->

ProbSup employs Bayesian statistics to analyze multi-environment trials’
data, and uses its outputs to computate the marginal, pairwise, and
conditional probability of superior performance of the genotypes. The
method is thoroughly described at
<https://doi.org/10.1007/s00122-022-04041-y>. This is a work in
progress.

## Installation

You can install the development version of ProbSup from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("saulo-chaves/ProbSup")
```

## Example

A basic workflow using the available data is:

``` r
library(ProbSup)

df = read.csv('maize_dataset.csv')

mod = bayes_met(data = df, 
          gen = c("Hybrid", "normal", "cauchy"), 
          env = c("Location", "normal", "cauchy"),
          rept = list(c("Rep", "normal", "cauchy"), c("Block", "normal", "cauchy")),
          reg = list(c("Region", "normal", "cauchy"), c("normal", "cauchy")),
          sigma.dist = c("cauchy", "cauchy"), mu.dist = c("normal", "cauchy"),
          gli.dist = c("normal", "normal"), trait = "GY", hyperparam = "default",
          iter = 4000, cores = 4, chain = 4) # It will take a while

outs = extr_outs(data = df, trait = "GY", gen = "Hybrid", model = mod,
                 effects = c("r", "b", "l", "m", "g", "gl", "gm"),
                 nenv = 16, res.het = TRUE, check.stan.diag = TRUE)

margs_pair = marg_prob(data = df, trait = "GY", gen = "Hybrid", env = "Location", 
                       extr_outs = outs, int = .2, save.df = F, interactive = F)

conds = marg_prob(data = df, trait = "GY", gen = "Hybrid",env = "Location", 
                  extr_outs = outs, reg = "Region", int = .2,
                  save.df = F, interactive = F)
```
