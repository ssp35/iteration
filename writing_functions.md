Writing Functions
================
Saryu Patel
11/2/2021

## Something simple

``` r
x_vec <- rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -0.61703636  0.71219696  0.60771476  0.12344370  0.07407680  2.08771570
    ##  [7]  1.18653059  0.47921032  0.67796955 -0.39970896  0.62321807  0.02179314
    ## [13]  1.81631968 -0.28897743 -0.28618812 -0.58199478  1.00750451 -1.46394172
    ## [19]  0.12855069 -0.11140422 -2.48071600  0.13907747  0.67337383  0.38488802
    ## [25] -0.71770910 -0.02682900 -2.41228237 -0.44396666 -0.41252567 -0.50030341

I want a function to compute z-scores.

``` r
z_scores <- function(x) {
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  z <- (x - mean(x))/sd(x)
  return(z)
}

z_scores(x_vec)
```

    ##  [1] -0.61703636  0.71219696  0.60771476  0.12344370  0.07407680  2.08771570
    ##  [7]  1.18653059  0.47921032  0.67796955 -0.39970896  0.62321807  0.02179314
    ## [13]  1.81631968 -0.28897743 -0.28618812 -0.58199478  1.00750451 -1.46394172
    ## [19]  0.12855069 -0.11140422 -2.48071600  0.13907747  0.67337383  0.38488802
    ## [25] -0.71770910 -0.02682900 -2.41228237 -0.44396666 -0.41252567 -0.50030341

Try the function on some other things.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is saryu")
```

    ## Error in z_scores("my name is saryu"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric
