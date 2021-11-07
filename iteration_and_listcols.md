Iteration and Listcols
================
Saryu Patel
11/7/2021

## Lists

You can put anything in a list.

``` r
l <- list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.52534 -0.71449  0.03922 -0.04684  0.55645  2.57384

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm <- 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = 0.2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 2.772151 3.930081 1.683234 1.445807 2.652628 1.278347 2.861456 3.186684
    ##  [9] 1.562093 4.462540 4.068133 3.320726 3.395592 1.777384 3.775224 2.101062
    ## [17] 2.323058 2.345859 3.297272 2.129230
    ## 
    ## $b
    ##  [1]   6.2662971  10.1568978   9.0657473 -11.9646035   2.4442866   2.7598559
    ##  [7]  -4.1508749  -3.7526280  -7.4793847   2.1982846   9.9136151  -4.1960666
    ## [13]  -2.3991090  -3.4223029  -6.8669754  -3.1872429   9.2616726  -4.5337139
    ## [19]   3.9586232  -1.5157990  -6.1577615   0.4543349  -7.4330604  10.4656709
    ## [25]   3.6393554   4.4752569  -3.1754121   5.2836096  -5.0446797  -0.3631241
    ## 
    ## $c
    ##  [1] 10.078134 10.366986  9.835009 10.134175  9.906855 10.002984 10.048410
    ##  [8]  9.885963 10.001921  9.667203 10.132984 10.064568 10.212249  9.948503
    ## [15] 10.026332 10.158820 10.205600  9.975003  9.927405  9.977628  9.961209
    ## [22] 10.296604  9.759616 10.224557  9.950640  9.766475 10.162040 10.111274
    ## [29]  9.882979  9.856737 10.080298  9.962459  9.987000 10.421260  9.951484
    ## [36] 10.107641  9.910039 10.120722 10.224783 10.046885
    ## 
    ## $d
    ##  [1] -2.271620 -3.435092 -1.714591 -3.478395 -1.149151 -3.173446 -2.557187
    ##  [8] -3.636620 -3.814376 -3.457104 -2.217261 -1.527577 -1.634572 -2.373971
    ## [15] -1.989611 -3.395799 -2.343114 -2.655742 -1.367644 -1.848749

Bring back an old function.

``` r
mean_and_sd <- function(x) {
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x <- mean(x)
  sd_x <- sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.72 0.942

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.157  6.12

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.162

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.50 0.837

Let’s use a for loop:

``` r
output <- vector("list", length = 4)

output[[1]] <- mean_and_sd(list_norm[[1]])

for (i in 1:4) {
  output[[i]] <- mean_and_sd(list_norm[[i]])
}
```

## Map

``` r
output <- map(list_norm, mean_and_sd)
```

What if you want a different function?

``` r
output <- map(list_norm, median)
```
