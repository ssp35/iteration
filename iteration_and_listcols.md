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
    ## -1.93471 -0.66290 -0.13624 -0.02923  0.52781  2.57046

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
    ##  [1] 3.537736 2.551033 4.187285 3.506563 1.245454 6.846822 3.300855 3.043086
    ##  [9] 3.355522 3.190763 2.017627 2.012362 1.944210 2.841817 4.225947 3.253036
    ## [17] 3.830996 2.777260 4.799406 3.628894
    ## 
    ## $b
    ##  [1] -3.2305921  0.8533668  9.6433153  2.5285410  7.1415106  3.5060977
    ##  [7]  5.3380356 -3.1921968  1.5468361 -0.4913228  3.4048839 -2.7263105
    ## [13] -2.9976968 -2.7895968  4.6907281 -6.7279990  3.2489192  2.7775236
    ## [19]  3.9468484  2.3275743 -8.8817689  7.2042040 -6.7448905  4.3343631
    ## [25]  4.4736507  1.0232795  3.3334663 -2.3870496  0.1109656 -2.7168715
    ## 
    ## $c
    ##  [1]  9.860445 10.105891  9.862246 10.247549  9.779358  9.872033 10.205399
    ##  [8] 10.003774  9.987313 10.080832 10.033677 10.072152 10.122812  9.378354
    ## [15] 10.064644  9.807039 10.114069  9.897156  9.815576 10.124925 10.057546
    ## [22]  9.873731  9.831528 10.003497  9.864452  9.705079  9.835451  9.915070
    ## [29]  9.824241 10.008659  9.880053  9.864966 10.220027 10.262915  9.966253
    ## [36]  9.701929  9.865843 10.013058 10.027804  9.428562
    ## 
    ## $d
    ##  [1] -3.268560 -3.759717 -2.320076 -2.401200 -2.345630 -3.016996 -2.224822
    ##  [8] -3.259143 -3.743054 -3.750922 -1.377797 -2.752878 -4.051077 -2.854183
    ## [15] -5.441812 -2.005323 -2.815978 -1.714310 -3.602870 -1.850916

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
    ## 1  3.30  1.20

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.952  4.44

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.94 0.190

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.93 0.966

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

``` r
output <- map_dbl(list_norm, median)
```

``` r
output <- map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns

``` r
listcol_df <- 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 3.537736 2.551033 4.187285 3.506563 1.245454 6.846822 3.300855 3.043086
    ##  [9] 3.355522 3.190763 2.017627 2.012362 1.944210 2.841817 4.225947 3.253036
    ## [17] 3.830996 2.777260 4.799406 3.628894
    ## 
    ## $b
    ##  [1] -3.2305921  0.8533668  9.6433153  2.5285410  7.1415106  3.5060977
    ##  [7]  5.3380356 -3.1921968  1.5468361 -0.4913228  3.4048839 -2.7263105
    ## [13] -2.9976968 -2.7895968  4.6907281 -6.7279990  3.2489192  2.7775236
    ## [19]  3.9468484  2.3275743 -8.8817689  7.2042040 -6.7448905  4.3343631
    ## [25]  4.4736507  1.0232795  3.3334663 -2.3870496  0.1109656 -2.7168715
    ## 
    ## $c
    ##  [1]  9.860445 10.105891  9.862246 10.247549  9.779358  9.872033 10.205399
    ##  [8] 10.003774  9.987313 10.080832 10.033677 10.072152 10.122812  9.378354
    ## [15] 10.064644  9.807039 10.114069  9.897156  9.815576 10.124925 10.057546
    ## [22]  9.873731  9.831528 10.003497  9.864452  9.705079  9.835451  9.915070
    ## [29]  9.824241 10.008659  9.880053  9.864966 10.220027 10.262915  9.966253
    ## [36]  9.701929  9.865843 10.013058 10.027804  9.428562
    ## 
    ## $d
    ##  [1] -3.268560 -3.759717 -2.320076 -2.401200 -2.345630 -3.016996 -2.224822
    ##  [8] -3.259143 -3.743054 -3.750922 -1.377797 -2.752878 -4.051077 -2.854183
    ## [15] -5.441812 -2.005323 -2.815978 -1.714310 -3.602870 -1.850916

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.30  1.20

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.952  4.44

What about using map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.30  1.20
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.952  4.44
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.94 0.190
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.93 0.966

Add a list column.

``` r
listcol_df <- 
  listcol_df %>% 
    mutate(summary = map(samp, mean_and_sd),
           medians = map_dbl(samp, median))
```
