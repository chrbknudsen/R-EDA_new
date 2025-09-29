---
title: "Exploring with summary statistics"
teaching: 30
exercises: 10
---


::::questions
- "How do I get means, medians, IQRs and other summary statistics on my data?"
- "How do I get summary statistics for different groups in my data?"
::::

::::objectives
- "Learn the summarise and group_by functions for efficient summary statistics."
::::








## Summary statistics

What is the average delay on the departure of these flights?

Most of the functions we use for these operations comes from the library
dplyr, which is part of the tidyverse package.

The function to get summary statistics (average/mean, median, standard deviation etc)
from dataframes is `summarise`. It works like this, remember to add `na.rm = T`
to handle missing values:


``` r
flightdata %>% 
  summarise(avg_dep_delay = mean(dep_delay, na.rm = T),
            med_dep_delay = median(dep_delay, na.rm = T),
            sd_dep_delay = sd(dep_delay, na.rm = T),
            max_dep_delay = max(dep_delay, na.rm = T),
            min_dep_delay = min(dep_delay, na.rm  =T),
            iqr = IQR(dep_delay, na.rm = T))
```

``` output
# A tibble: 1 × 6
  avg_dep_delay med_dep_delay sd_dep_delay max_dep_delay min_dep_delay   iqr
          <dbl>         <dbl>        <dbl>         <dbl>         <dbl> <dbl>
1          12.6            -2         40.2          1301           -43    16
```

::::callout
## IQR
Lining up all values of departure delay in order from the lowest to the highest,
we can split up the observations in quartiles, each containing 25% of the 
observations. The Inter Quartile Range, tells us the range in which the middle
50% of the observations are. It is a measure of the spread of the observations
around the median.
::::

## Which airline is most delayed?

Adding `group_by()` we can get the summary statistics for all 
airlines:


``` r
flightdata %>% 
  group_by(carrier) %>% 
  summarise(avg_dep_delay = mean(dep_delay, na.rm = T),
            med_dep_delay = median(dep_delay, na.rm = T),
            sd_dep_delay = sd(dep_delay, na.rm = T),
            max_dep_delay = max(dep_delay, na.rm = T),
            min_dep_delay = min(dep_delay, na.rm  =T),
            iqr = IQR(dep_delay, na.rm = T))
```

``` output
# A tibble: 16 × 7
   carrier avg_dep_delay med_dep_delay sd_dep_delay max_dep_delay min_dep_delay
   <chr>           <dbl>         <dbl>        <dbl>         <dbl>         <dbl>
 1 9E              16.7           -2           45.9           747           -24
 2 AA               8.59          -3           37.4          1014           -24
 3 AS               5.80          -3           31.4           225           -21
 4 B6              13.0           -1           38.5           502           -43
 5 DL               9.26          -2           39.7           960           -33
 6 EV              20.0           -1           46.6           548           -32
 7 F9              20.2            0.5         58.4           853           -27
 8 FL              18.7            1           52.7           602           -22
 9 HA               4.90          -4           74.1          1301           -16
10 MQ              10.6           -3           39.2          1137           -26
11 OO              12.6           -6           43.1           154           -14
12 UA              12.1            0           35.7           483           -20
13 US               3.78          -4           28.1           500           -19
14 VX              12.9            0           44.8           653           -20
15 WN              17.7            1           43.3           471           -13
16 YV              19.0           -2           49.2           387           -16
# ℹ 1 more variable: iqr <dbl>
```

A second step would be to sort the carriers by the average departure delay. The `arrange()` function does this:


``` r
flightdata %>% 
  group_by(carrier) %>% 
  summarise(avg_dep_delay = mean(dep_delay, na.rm = T),
            med_dep_delay = median(dep_delay, na.rm = T),
            sd_dep_delay = sd(dep_delay, na.rm = T),
            max_dep_delay = max(dep_delay, na.rm = T),
            min_dep_delay = min(dep_delay, na.rm  =T),
            iqr = IQR(dep_delay, na.rm = T)) %>% 
  arrange(avg_dep_delay)
```

``` output
# A tibble: 16 × 7
   carrier avg_dep_delay med_dep_delay sd_dep_delay max_dep_delay min_dep_delay
   <chr>           <dbl>         <dbl>        <dbl>         <dbl>         <dbl>
 1 US               3.78          -4           28.1           500           -19
 2 HA               4.90          -4           74.1          1301           -16
 3 AS               5.80          -3           31.4           225           -21
 4 AA               8.59          -3           37.4          1014           -24
 5 DL               9.26          -2           39.7           960           -33
 6 MQ              10.6           -3           39.2          1137           -26
 7 UA              12.1            0           35.7           483           -20
 8 OO              12.6           -6           43.1           154           -14
 9 VX              12.9            0           44.8           653           -20
10 B6              13.0           -1           38.5           502           -43
11 9E              16.7           -2           45.9           747           -24
12 WN              17.7            1           43.3           471           -13
13 FL              18.7            1           52.7           602           -22
14 YV              19.0           -2           49.2           387           -16
15 EV              20.0           -1           46.6           548           -32
16 F9              20.2            0.5         58.4           853           -27
# ℹ 1 more variable: iqr <dbl>
```

The carrier "US" does best. What carrier is that actually?

Before doing that, let us save the result in an object:

``` r
summary_delays <- flightdata %>% 
  group_by(carrier) %>% 
  summarise(avg_dep_delay = mean(dep_delay, na.rm = T),
            med_dep_delay = median(dep_delay, na.rm = T),
            sd_dep_delay = sd(dep_delay, na.rm = T),
            max_dep_delay = max(dep_delay, na.rm = T),
            min_dep_delay = min(dep_delay, na.rm  =T),
            iqr = IQR(dep_delay, na.rm = T)) %>% 
  arrange(avg_dep_delay)
```

Next up - how to figure out the name of the carrier "US".

::::keypoints
- "Summary statistics tells us about the distribution of data"
::::
