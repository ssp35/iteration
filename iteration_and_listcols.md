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
    ## -2.72611 -0.59715 -0.20936 -0.08778  0.41563  3.41765

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
    ##  [1] 3.273566 3.118172 4.228474 3.218479 3.685145 2.200199 2.800690 1.624075
    ##  [9] 3.931696 4.707736 1.412230 4.082072 3.255918 1.616787 2.823639 2.239093
    ## [17] 4.968556 3.021810 3.122336 2.175414
    ## 
    ## $b
    ##  [1]   4.0354480   1.9377598  -2.2637208  -2.8143257 -12.8899142  -0.2788235
    ##  [7]  10.3293289  -0.5096591  -2.5867830  -3.7141945   6.5113106  -2.7368202
    ## [13]   2.4251057   0.3496894  -3.7252568   9.2682548  -3.1900532   3.2654041
    ## [19]   4.3922191  -3.3130003  -4.4600947   2.7444790   1.5505805 -10.2076469
    ## [25]  -3.6614237  -3.4353714   1.4603029  -2.9184103   7.3228527  -2.2500529
    ## 
    ## $c
    ##  [1] 10.386535 10.045287  9.742632 10.078119 10.013770 10.056585 10.098168
    ##  [8]  9.906121 10.207613  9.942970 10.081023 10.011342 10.081741  9.976794
    ## [15]  9.713421  9.932660 10.338877  9.793927 10.020639  9.785828  9.661867
    ## [22] 10.203846  9.791008 10.106724 10.411915  9.655276 10.102254 10.018173
    ## [29] 10.058855  9.950082  9.706230  9.884964  9.922588 10.400568 10.313124
    ## [36] 10.006398 10.178083  9.920658  9.805047  9.836427
    ## 
    ## $d
    ##  [1] -4.315388 -6.242456 -1.727086 -2.253662 -3.700457 -4.161872 -2.607941
    ##  [8] -2.692395 -3.933738 -3.803978 -2.110679 -3.411926 -2.512973 -3.536184
    ## [15] -2.290556 -3.994570 -3.555949 -4.217104 -3.363252 -2.935903

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
    ## 1  3.08  1.01

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.312  5.12

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.201

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.37  1.03

Let’s use a for loop:

``` r
output <- vector("list", length = 4)

output[[1]] <- mean_and_sd(list_norm[[1]])

for (i in 1:4) {
  output[[i]] <- mean_and_sd(list_norm[[i]])
}
```
