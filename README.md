
<!-- README.md is generated from README.Rmd. Please edit that file -->

# transportr

<!-- badges: start -->
<!-- badges: end -->

## Installation

You can install the development version of transport from
[GitHub](https://github.com/) with:

``` r
# install.packages("pak")
pak::pak("nt-williams/transportr")
```

## Example

``` r
library(transportr)

gendata <- function(n, A = NULL) {
  W <- rbinom(n, 1, 0.5)
  V <- rbinom(n, 1, 0.66)
  Z <- rbinom(n, 1, 0.33)

  if (is.null(A)) A <- rbinom(n, 1, 0.5)

  S <- rbinom(n, 1, 0.4 + 0.5*W - 0.3*Z)

  Yi <- rnorm(n, A + W + A*V + 2.5*A*Z, sqrt((0.1 + 0.8*W)^2))
  Y <- ifelse(S == 1, Yi, NA_real_)

  data.frame(W = W,
             V = V,
             Z = Z,
             S = S,
             A = A,
             Y = Y,
             Yi = Yi)
}

set.seed(123)
n <- 250

tmp <- gendata(n)

transport_ate(data = tmp,
              trt = "A",
              outcome = "Y",
              covar = c("W", "V", "Z"),
              pop = "S",
              estimator = "collaborative",
              folds = 1)
#> ══ Results from `transport_ate()` ═══════════════════════════════════════════════════════════════
#> 
#>       Estimate: 3.33
#>     Std. error: 0.26
#> 95% Conf. int.: 2.81, 3.84
```
