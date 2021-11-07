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
    ## -2.25707 -0.63866 -0.04074 -0.07224  0.53017  2.16027

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
    ##  [1] 2.5485924 3.2364514 2.0286178 2.9563750 2.6407506 4.4166866 3.6300718
    ##  [8] 1.0516249 2.3472475 2.8950207 3.0503075 3.4888910 3.0885311 0.7105815
    ## [15] 1.7276763 3.3506274 1.0333151 5.2415672 4.6925212 2.9699855
    ## 
    ## $b
    ##  [1]  -4.3138745   2.2448296   5.6345704   0.2537878   7.0933136   6.9904632
    ##  [7]  -7.1317811   3.1342653   3.2004882  -1.8968858  -5.4257070   0.7525117
    ## [13]  -6.8138922 -12.5784746  10.5941532   3.8852411   2.9981899   7.7529444
    ## [19]  -3.3514508   1.9558959   5.3994859  -5.3951569   3.1555403  -1.7272483
    ## [25]   2.5589904  -1.1004946  -0.1280173   3.7489171  -0.3435728  -6.8378682
    ## 
    ## $c
    ##  [1]  9.752621  9.709708 10.179339  9.972659 10.176930  9.922435 10.321384
    ##  [8]  9.579923  9.981774 10.077224  9.683211  9.942246 10.071548  9.770902
    ## [15]  9.949436  9.727371  9.864233 10.181942 10.134882  9.851578  9.904506
    ## [22] 10.074721  9.868994 10.305837  9.891772 10.079443  9.874385  9.821061
    ## [29] 10.155917 10.165329  9.818093 10.209100 10.138695  9.763962  9.798868
    ## [36]  9.870998 10.092642 10.111288 10.389965 10.179217
    ## 
    ## $d
    ##  [1] -3.703499 -2.243254 -2.935997 -3.489890 -3.672877 -3.769297 -1.161667
    ##  [8] -2.954243 -2.153689 -2.374735 -2.370594 -3.271922 -2.414752 -3.537013
    ## [15] -1.616318 -3.423648 -3.804495 -3.757357 -3.067618 -3.205783

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
    ## 1  2.86  1.18

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.477  5.28

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.98 0.195

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.95 0.769

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
    ##  [1] 2.5485924 3.2364514 2.0286178 2.9563750 2.6407506 4.4166866 3.6300718
    ##  [8] 1.0516249 2.3472475 2.8950207 3.0503075 3.4888910 3.0885311 0.7105815
    ## [15] 1.7276763 3.3506274 1.0333151 5.2415672 4.6925212 2.9699855
    ## 
    ## $b
    ##  [1]  -4.3138745   2.2448296   5.6345704   0.2537878   7.0933136   6.9904632
    ##  [7]  -7.1317811   3.1342653   3.2004882  -1.8968858  -5.4257070   0.7525117
    ## [13]  -6.8138922 -12.5784746  10.5941532   3.8852411   2.9981899   7.7529444
    ## [19]  -3.3514508   1.9558959   5.3994859  -5.3951569   3.1555403  -1.7272483
    ## [25]   2.5589904  -1.1004946  -0.1280173   3.7489171  -0.3435728  -6.8378682
    ## 
    ## $c
    ##  [1]  9.752621  9.709708 10.179339  9.972659 10.176930  9.922435 10.321384
    ##  [8]  9.579923  9.981774 10.077224  9.683211  9.942246 10.071548  9.770902
    ## [15]  9.949436  9.727371  9.864233 10.181942 10.134882  9.851578  9.904506
    ## [22] 10.074721  9.868994 10.305837  9.891772 10.079443  9.874385  9.821061
    ## [29] 10.155917 10.165329  9.818093 10.209100 10.138695  9.763962  9.798868
    ## [36]  9.870998 10.092642 10.111288 10.389965 10.179217
    ## 
    ## $d
    ##  [1] -3.703499 -2.243254 -2.935997 -3.489890 -3.672877 -3.769297 -1.161667
    ##  [8] -2.954243 -2.153689 -2.374735 -2.370594 -3.271922 -2.414752 -3.537013
    ## [15] -1.616318 -3.423648 -3.804495 -3.757357 -3.067618 -3.205783

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
    ## 1  2.86  1.18

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.477  5.28

What about using map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.86  1.18
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.477  5.28
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.98 0.195
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.95 0.769

Add a list column.

``` r
listcol_df <- 
  listcol_df %>% 
    mutate(summary = map(samp, mean_and_sd),
           medians = map_dbl(samp, median))
```

## Weather Data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2021-10-04 17:13:50 (7.602)

    ## file min/max dates: 1869-01-01 / 2021-10-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2021-10-04 17:13:56 (1.697)

    ## file min/max dates: 1965-01-01 / 2020-02-29

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2021-10-04 17:14:01 (0.912)

    ## file min/max dates: 1999-09-01 / 2021-09-30

Get list columns.

``` r
weather_nest <- 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

``` r
weather_nest$data[[1]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

Regress `tmax` on `tmin` for each station.

``` r
lm(tmax ~ tmin, weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

Write a function.

``` r
weather_lm <- function(df) {
  lm(tmax ~ tmin, data = df)
}

weather_lm(weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
output <- vector("list", 3)

for (i in 1:3) {
  output[[i]] <- weather_lm(weather_nest$data[[i]])
}
```

What about a map?

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list column?

``` r
weather_nest <- 
  weather_nest %>% 
    mutate(models = map(data, weather_lm))

weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221
