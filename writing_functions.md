Writing Functions
================
Saryu Patel
11/2/2021

## Something simple

``` r
x_vec <- rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -1.457615854 -0.296015165  0.767570710  0.289809318  0.904224105
    ##  [6] -0.721260332  1.730083581 -0.750977375 -1.513678212 -0.784149143
    ## [11] -0.388829346  1.247974498 -0.469268165  1.751965722 -0.780166996
    ## [16]  0.709733007  0.401928483 -0.837191874  0.628647329  0.027895943
    ## [21]  0.001561368  1.037187322  0.328107820 -0.542176271  0.873950237
    ## [26] -2.023163820  0.079834153  1.551001845 -1.447852649 -0.319130241

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

    ##  [1] -1.457615854 -0.296015165  0.767570710  0.289809318  0.904224105
    ##  [6] -0.721260332  1.730083581 -0.750977375 -1.513678212 -0.784149143
    ## [11] -0.388829346  1.247974498 -0.469268165  1.751965722 -0.780166996
    ## [16]  0.709733007  0.401928483 -0.837191874  0.628647329  0.027895943
    ## [21]  0.001561368  1.037187322  0.328107820 -0.542176271  0.873950237
    ## [26] -2.023163820  0.079834153  1.551001845 -1.447852649 -0.319130241

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
    ## 1  3.03  3.97

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
    ## 1  3.83  3.18

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
    ## 1  6.50  3.15

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.76  2.90

``` r
sim_mean_sd(100)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.26  4.19

## Let’s review Napoleon Dynamite

``` r
url <-  "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html <-  read_html(url)

review_titles <-  
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars <-  
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text <-  
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews_page1 <-  tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews?

``` r
url <-  "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

dynamite_html <- read_html(url)

review_titles <-  
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars <-  
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text <-  
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews_page2 <- tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

Let’s turn that code into a function.

``` r
read_page_reviews <- function(url) {
  html = read_html(url)

  review_titles <-  
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()

  review_stars <-  
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()

  review_text <-  
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()

  reviews <- tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )
  
  reviews
}
```

Try the function.

``` r
dynamite_url <- "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=9"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 × 3
    ##    title                                       stars text                       
    ##    <chr>                                       <dbl> <chr>                      
    ##  1 Feed Tina!!                                     5 "Great movie"              
    ##  2 Soooo funny                                     5 "I love this movie."       
    ##  3 Making Memories!!!                              5 "My grandson has never see…
    ##  4 Such a fun movie!!                              5 "Such a fun movie! We watc…
    ##  5 Funny movie about growing up in the midwest     5 "Love this movie, glad to …
    ##  6 Can’t stop laughing when watching it.           5 "My favorite show for a go…
    ##  7 Love it                                         5 "This film is gold."       
    ##  8 Funny                                           4 "Probably not all people a…
    ##  9 One of my favorite movies!                      5 "One of my favorite movies…
    ## 10 Wonderful                                       5 "Was funny when it came ou…

Let’s read a few pages of reviews.

``` r
dynamite_url_base <- "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls <-  str_c(dynamite_url_base, 1:5)

dynamite_reviews <- 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[3]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
  )
```

## Scoping example

``` r
f <- function(x) {
  z = x + y
  z
}

x <- 1
y <- 2

f(x = y)
```

    ## [1] 4
