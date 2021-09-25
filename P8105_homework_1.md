homework 1
================
Weize Sun
9/25/2021

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## Problem 1

``` r
hw1_df = 
  tibble(
    sample = rnorm(10),
    gr_than_0 = sample > 0,
    operator = c("Thorns", "Surtr", "SilverAsh", "Ash", "Blaze", "Ifrit", "Mountain", "Archetto", "Dusk", "Broca"),
    job = factor(c("Guard", "Guard", "Guard", "Sniper", "Guard", "Caster", "Guard", "Sniper", "Caster", "Guard"))
  )

hw1_df
```

    ## # A tibble: 10 × 4
    ##     sample gr_than_0 operator  job   
    ##      <dbl> <lgl>     <chr>     <fct> 
    ##  1 -0.576  FALSE     Thorns    Guard 
    ##  2 -1.82   FALSE     Surtr     Guard 
    ##  3 -0.0297 FALSE     SilverAsh Guard 
    ##  4 -0.667  FALSE     Ash       Sniper
    ##  5 -0.0711 FALSE     Blaze     Guard 
    ##  6  1.06   TRUE      Ifrit     Caster
    ##  7  1.17   TRUE      Mountain  Guard 
    ##  8  0.174  TRUE      Archetto  Sniper
    ##  9 -1.24   FALSE     Dusk      Caster
    ## 10 -1.70   FALSE     Broca     Guard

``` r
mean(pull(hw1_df, sample))
```

    ## [1] -0.3701212

``` r
mean(pull(hw1_df, gr_than_0))
```

    ## [1] 0.3

``` r
mean(pull(hw1_df, operator))
```

    ## Warning in mean.default(pull(hw1_df, operator)): argument is not numeric or
    ## logical: returning NA

    ## [1] NA

``` r
mean(pull(hw1_df, job))
```

    ## Warning in mean.default(pull(hw1_df, job)): argument is not numeric or logical:
    ## returning NA

    ## [1] NA

When I tried to take the mean of each variable in my dataframe, I found
only sample mean looks normal; logical vector’s mean is 0.5, so I guess
they took “true” and “false” as 0 and 1; both character vector
(operator) and factor vector (job) couldn’t give the means.

``` r
as.numeric(pull(hw1_df, gr_than_0))
as.numeric(pull(hw1_df, operator))
as.numeric(pull(hw1_df, job))
```

After I used the *as.numeric* and *pull* function, I found logical
vectors have been changed to 0 and 1 (which confirmed my guess), because
they only have “true” and “false” these two variables; character vectors
have been changed to all “NA”s, because they are all different
characters so that couldn’t be changed to numbers; factor vectors have
been changed to 1, 2, and 3, because there are total three categories so
that 1, 2, and 3 could represent those categories.

## Problem 2

``` r
data("penguins", package = "palmerpenguins")
names(penguins)
```

    ## [1] "species"           "island"            "bill_length_mm"   
    ## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
    ## [7] "sex"               "year"

``` r
head(penguins)
```

    ## # A tibble: 6 × 8
    ##   species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
    ##   <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    ## 1 Adelie  Torge…           39.1          18.7              181        3750 male 
    ## 2 Adelie  Torge…           39.5          17.4              186        3800 fema…
    ## 3 Adelie  Torge…           40.3          18                195        3250 fema…
    ## 4 Adelie  Torge…           NA            NA                 NA          NA <NA> 
    ## 5 Adelie  Torge…           36.7          19.3              193        3450 fema…
    ## 6 Adelie  Torge…           39.3          20.6              190        3650 male 
    ## # … with 1 more variable: year <int>

``` r
skimr::skim(penguins)
```

|                                                  |          |
|:-------------------------------------------------|:---------|
| Name                                             | penguins |
| Number of rows                                   | 344      |
| Number of columns                                | 8        |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |          |
| Column type frequency:                           |          |
| factor                                           | 3        |
| numeric                                          | 5        |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |          |
| Group variables                                  | None     |

Data summary

**Variable type: factor**

| skim\_variable | n\_missing | complete\_rate | ordered | n\_unique | top\_counts                 |
|:---------------|-----------:|---------------:|:--------|----------:|:----------------------------|
| species        |          0 |           1.00 | FALSE   |         3 | Ade: 152, Gen: 124, Chi: 68 |
| island         |          0 |           1.00 | FALSE   |         3 | Bis: 168, Dre: 124, Tor: 52 |
| sex            |         11 |           0.97 | FALSE   |         2 | mal: 168, fem: 165          |

**Variable type: numeric**

| skim\_variable      | n\_missing | complete\_rate |    mean |     sd |     p0 |     p25 |     p50 |    p75 |   p100 | hist  |
|:--------------------|-----------:|---------------:|--------:|-------:|-------:|--------:|--------:|-------:|-------:|:------|
| bill\_length\_mm    |          2 |           0.99 |   43.92 |   5.46 |   32.1 |   39.23 |   44.45 |   48.5 |   59.6 | ▃▇▇▆▁ |
| bill\_depth\_mm     |          2 |           0.99 |   17.15 |   1.97 |   13.1 |   15.60 |   17.30 |   18.7 |   21.5 | ▅▅▇▇▂ |
| flipper\_length\_mm |          2 |           0.99 |  200.92 |  14.06 |  172.0 |  190.00 |  197.00 |  213.0 |  231.0 | ▂▇▃▅▂ |
| body\_mass\_g       |          2 |           0.99 | 4201.75 | 801.95 | 2700.0 | 3550.00 | 4050.00 | 4750.0 | 6300.0 | ▃▇▆▃▂ |
| year                |          0 |           1.00 | 2008.03 |   0.82 | 2007.0 | 2007.00 | 2008.00 | 2009.0 | 2009.0 | ▇▁▇▁▇ |

``` r
nrow(penguins)
```

    ## [1] 344

``` r
ncol(penguins)
```

    ## [1] 8

``` r
ggplot(penguins, aes(x = bill_length_mm, y = flipper_length_mm, color = species)) + geom_point()
```

    ## Warning: Removed 2 rows containing missing values (geom_point).

![](P8105_homework_1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
ggsave("penguin_scatter_plot.pdf", height = 4, width = 6)
```

    ## Warning: Removed 2 rows containing missing values (geom_point).
