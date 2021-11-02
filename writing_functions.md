Writing Functions
================
Saryu Patel
11/2/2021

## Something simple

``` r
x_vec <- rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -1.39486358 -0.83166048  0.02090035 -0.81092627  0.49770141 -0.85050066
    ##  [7]  0.69354458  0.74701912 -0.20771855 -0.21751688 -0.29956229 -1.65633812
    ## [13] -0.24133062 -0.78556643  0.94260933 -1.08378322 -1.06297075  0.93654563
    ## [19]  1.14930306  1.27707074  0.05694463  2.16474084 -0.52208986 -1.65705944
    ## [25]  0.21243110  1.02249789  1.17244679 -1.10400769  0.87490511  0.95723427

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

    ##  [1] -1.39486358 -0.83166048  0.02090035 -0.81092627  0.49770141 -0.85050066
    ##  [7]  0.69354458  0.74701912 -0.20771855 -0.21751688 -0.29956229 -1.65633812
    ## [13] -0.24133062 -0.78556643  0.94260933 -1.08378322 -1.06297075  0.93654563
    ## [19]  1.14930306  1.27707074  0.05694463  2.16474084 -0.52208986 -1.65705944
    ## [25]  0.21243110  1.02249789  1.17244679 -1.10400769  0.87490511  0.95723427

Try the function on some other things. These should give errors.

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

## Multiple outputs

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

Check that the function works.

``` r
x_vec <- rnorm(1000, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  3.94

## Multiple inputs

I’d like to do this with a function.

``` r
sim_data <- 
  tibble(
    x = rnorm(100, mean = 4, sd = 3)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.35  2.98

``` r
sim_mean_sd <- function(samp_size, mu = 3, sigma = 4) {
  sim_data <- 
    tibble(
      x = rnorm(samp_size, mean = mu, sd = sigma)
    )
  
  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
}

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.76  2.99

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.82  3.12

``` r
sim_mean_sd(100)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.81  3.93
