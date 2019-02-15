---
title: "Dealing with NA's"
author: "Joel Ledford"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: yes
    toc_float: yes
  pdf_document:
    toc: yes
---

## Review
In the last section we practiced wrangling untidy data using `tidyr`. We also learned the `summarize()` function and `group_by()` to produce useful summaries of our data. But, we ended the last session with the discovery of NA's and how they can impact analyses. This is a big issue in data science and we will spend the remainder of this lab learning how to manage data with missing values.  

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Produce summaries of the number of NA's in a data set.  
2. Replace values with `NA` in a data set as appropriate.  

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.3.0
## ✔ tibble  2.0.1     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ───────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Dealing with NA's
In almost all scientific data, there are missing observations. These can be tricky to deal with, partly because you first need to determine how missing values were treated in the original study. Scientists use different conventions in showing missing data; some use blank spaces, others use "-", etc. Worse yet, some scientists indicate **missing data with numerics like -999.0!**  

## Practice
1. What are some possible problems if missing data are indicated by "-999.0"?
Going to give false feedback for numerical calculations. 

## Load the `msleep` data into a new object

```r
msleep <- msleep
```

## Are there any NA's?
Let's first check to see if our data has any NA's. is.na() is a function that determines whether a value in a data frame is or is not an NA. This is evaluated logically as TRUE or FALSE.

```r
is.na(msleep)
```

```
##        name genus  vore order conservation sleep_total sleep_rem
##  [1,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
##  [2,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##  [3,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [4,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [5,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [6,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##  [7,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [8,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE
##  [9,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [10,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [11,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [12,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [13,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [14,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [15,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [16,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [17,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [18,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [19,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [20,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [21,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [22,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [23,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [24,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [25,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [26,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [27,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [28,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [29,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [30,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [31,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [32,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [33,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [34,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [35,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [36,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [37,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [38,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [39,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [40,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [41,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [42,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [43,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [44,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [45,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [46,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [47,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [48,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [49,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [50,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [51,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [52,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [53,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [54,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [55,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE
## [56,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [57,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE
## [58,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [59,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [60,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [61,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [62,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [63,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE
## [64,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [65,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [66,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [67,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [68,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [69,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [70,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [71,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [72,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [73,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [74,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [75,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [76,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [77,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [78,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [79,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [80,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [81,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [82,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [83,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##       sleep_cycle awake brainwt bodywt
##  [1,]        TRUE FALSE    TRUE  FALSE
##  [2,]        TRUE FALSE   FALSE  FALSE
##  [3,]        TRUE FALSE    TRUE  FALSE
##  [4,]       FALSE FALSE   FALSE  FALSE
##  [5,]       FALSE FALSE   FALSE  FALSE
##  [6,]       FALSE FALSE    TRUE  FALSE
##  [7,]       FALSE FALSE    TRUE  FALSE
##  [8,]        TRUE FALSE    TRUE  FALSE
##  [9,]       FALSE FALSE   FALSE  FALSE
## [10,]        TRUE FALSE   FALSE  FALSE
## [11,]        TRUE FALSE   FALSE  FALSE
## [12,]       FALSE FALSE   FALSE  FALSE
## [13,]        TRUE FALSE    TRUE  FALSE
## [14,]       FALSE FALSE   FALSE  FALSE
## [15,]        TRUE FALSE   FALSE  FALSE
## [16,]        TRUE FALSE   FALSE  FALSE
## [17,]       FALSE FALSE   FALSE  FALSE
## [18,]       FALSE FALSE   FALSE  FALSE
## [19,]        TRUE FALSE   FALSE  FALSE
## [20,]       FALSE FALSE   FALSE  FALSE
## [21,]        TRUE FALSE   FALSE  FALSE
## [22,]       FALSE FALSE   FALSE  FALSE
## [23,]       FALSE FALSE   FALSE  FALSE
## [24,]        TRUE FALSE   FALSE  FALSE
## [25,]       FALSE FALSE   FALSE  FALSE
## [26,]        TRUE FALSE   FALSE  FALSE
## [27,]        TRUE FALSE    TRUE  FALSE
## [28,]       FALSE FALSE   FALSE  FALSE
## [29,]       FALSE FALSE   FALSE  FALSE
## [30,]        TRUE FALSE    TRUE  FALSE
## [31,]        TRUE FALSE    TRUE  FALSE
## [32,]        TRUE FALSE   FALSE  FALSE
## [33,]        TRUE FALSE   FALSE  FALSE
## [34,]       FALSE FALSE   FALSE  FALSE
## [35,]        TRUE FALSE    TRUE  FALSE
## [36,]        TRUE FALSE   FALSE  FALSE
## [37,]        TRUE FALSE    TRUE  FALSE
## [38,]       FALSE FALSE   FALSE  FALSE
## [39,]        TRUE FALSE    TRUE  FALSE
## [40,]       FALSE FALSE   FALSE  FALSE
## [41,]        TRUE FALSE    TRUE  FALSE
## [42,]       FALSE FALSE   FALSE  FALSE
## [43,]       FALSE FALSE   FALSE  FALSE
## [44,]        TRUE FALSE    TRUE  FALSE
## [45,]        TRUE FALSE   FALSE  FALSE
## [46,]        TRUE FALSE    TRUE  FALSE
## [47,]        TRUE FALSE    TRUE  FALSE
## [48,]       FALSE FALSE   FALSE  FALSE
## [49,]        TRUE FALSE   FALSE  FALSE
## [50,]       FALSE FALSE   FALSE  FALSE
## [51,]        TRUE FALSE    TRUE  FALSE
## [52,]        TRUE FALSE   FALSE  FALSE
## [53,]        TRUE FALSE    TRUE  FALSE
## [54,]       FALSE FALSE   FALSE  FALSE
## [55,]        TRUE FALSE   FALSE  FALSE
## [56,]        TRUE FALSE    TRUE  FALSE
## [57,]        TRUE FALSE    TRUE  FALSE
## [58,]        TRUE FALSE   FALSE  FALSE
## [59,]        TRUE FALSE    TRUE  FALSE
## [60,]        TRUE FALSE    TRUE  FALSE
## [61,]        TRUE FALSE    TRUE  FALSE
## [62,]        TRUE FALSE   FALSE  FALSE
## [63,]        TRUE FALSE   FALSE  FALSE
## [64,]       FALSE FALSE   FALSE  FALSE
## [65,]        TRUE FALSE    TRUE  FALSE
## [66,]        TRUE FALSE   FALSE  FALSE
## [67,]       FALSE FALSE   FALSE  FALSE
## [68,]       FALSE FALSE   FALSE  FALSE
## [69,]        TRUE FALSE   FALSE  FALSE
## [70,]        TRUE FALSE   FALSE  FALSE
## [71,]       FALSE FALSE   FALSE  FALSE
## [72,]        TRUE FALSE    TRUE  FALSE
## [73,]       FALSE FALSE   FALSE  FALSE
## [74,]       FALSE FALSE   FALSE  FALSE
## [75,]        TRUE FALSE   FALSE  FALSE
## [76,]        TRUE FALSE    TRUE  FALSE
## [77,]       FALSE FALSE   FALSE  FALSE
## [78,]        TRUE FALSE   FALSE  FALSE
## [79,]       FALSE FALSE   FALSE  FALSE
## [80,]        TRUE FALSE    TRUE  FALSE
## [81,]        TRUE FALSE   FALSE  FALSE
## [82,]        TRUE FALSE   FALSE  FALSE
## [83,]       FALSE FALSE   FALSE  FALSE
```

OK, what are we supposed to do with that? Unless you have a small data frame, applying the is.na function universally is not helpful but we can use the code in another way. Let's incorporate it into the `summarize()` function.

```r
msleep %>% 
  summarize(number_nas= sum(is.na(msleep)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1        136
```

This is better, but we still don't have any idea of where those NA's are in our data frame. If there were a systemic problem in the data it would be hard to determine. In order to do this, we need to apply `is.na` to each variable of interest.

```r
msleep %>% 
  summarize(number_nas= sum(is.na(conservation)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1         29
```

What if we are working with hundreds or thousands (or more!) variables?! In order to deal with this problem efficiently we can use another package in the tidyverse called `purrr`.

```r
msleep_na <- 
  msleep %>%
  purrr::map_df(~ sum(is.na(.))) #map to a new data frame the sum results of the is.na function for all columns
msleep_na
```

```
## # A tibble: 1 x 11
##    name genus  vore order conservation sleep_total sleep_rem sleep_cycle
##   <int> <int> <int> <int>        <int>       <int>     <int>       <int>
## 1     0     0     7     0           29           0        22          51
## # … with 3 more variables: awake <int>, brainwt <int>, bodywt <int>
```

Don't forget that we can use `gather()` to make viewing this output easier.

```r
msleep %>% 
  purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 11 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 sleep_cycle       51
##  2 conservation      29
##  3 brainwt           27
##  4 sleep_rem         22
##  5 vore               7
##  6 name               0
##  7 genus              0
##  8 order              0
##  9 sleep_total        0
## 10 awake              0
## 11 bodywt             0
```

This is much better, but we need to be careful. R can have difficulty interpreting missing data. This is especially true for categorical variables. Always do a reality check if the output doesn't make sense to you. A quick check never hurts.  

You can explore a specific variable more intently using `count()`. This operates similarly to `group_by()`.

```r
msleep %>% 
  count(conservation)
```

```
## # A tibble: 7 x 2
##   conservation     n
##   <chr>        <int>
## 1 cd               2
## 2 domesticated    10
## 3 en               4
## 4 lc              27
## 5 nt               4
## 6 vu               7
## 7 <NA>            29
```

Adding the `sort=TRUE` option automatically makes a descending list.

```r
msleep %>% 
  count(conservation, sort=TRUE)
```

```
## # A tibble: 7 x 2
##   conservation     n
##   <chr>        <int>
## 1 <NA>            29
## 2 lc              27
## 3 domesticated    10
## 4 vu               7
## 5 en               4
## 6 nt               4
## 7 cd               2
```

It is true that all of this is redundant, but you want to be able to run multiple checks on the data. Remember, just because your code runs without errors doesn't mean it is doing what you intended.  

## Replacing NA's
Once you have an idea of how NA's are represented in the data, you can replace them with `NA` so that R can better deal with them. The bit of code below is very handy, especially if the data has NA's represented as **actual values that you want replaced** or if you want to make sure any blanks are treated as NA.

```r
msleep_na2 <- 
  msleep %>% 
  na_if("") #replace x data with NA
msleep_na2
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
##  2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
##  3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
##  4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
##  6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
##  7 Nort… Call… carni Carn… vu                   8.7       1.4       0.383
##  8 Vesp… Calo… <NA>  Rode… <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn… domesticated        10.1       2.9       0.333
## 10 Roe … Capr… herbi Arti… lc                   3        NA        NA    
## # … with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

## Practice
1. Did replacing blanks with NA have any effect on the msleep data? Demonstrate this using some code.



## Practice and Homework
We will work together on the next part (time permitting) and this will end up being your homework. Make sure that you save your work and copy and paste your responses into the RMarkdown homework file.

Aren't mammals fun? Let's load up some more mammal data. This will be life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*  


```r
life_history <- readr::read_csv("~/Desktop/class_files-master/mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```

Rename some of the variables. Notice that I am replacing the old `life_history` data.

```r
life_history <- 
  life_history %>% 
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
```

1. Explore the data using the method that you prefer. Below, I am using a new package called `skimr`. If you want to use this, make sure that it is installed.

install.packages("skimr")
--> R code removed because Knit cannot function



```r
library("skimr")
```


```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ────────────────────────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
##     variable missing complete    n      mean         sd   p0  p25     p50
##          AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##    gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##  litter_size       0     1440 1440    -55.63     234.88 -999    1    2.27
##   litters_yr       0     1440 1440   -477.14     500.03 -999 -999    0.38
##         mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max_life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##      newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##    wean_mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##      weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
##      p75          p100     hist
##    15.61     210       ▆▁▁▁▁▁▇▁
##     4.5       21.46    ▃▁▁▁▁▁▁▇
##     3.83      14.18    ▁▁▁▁▁▁▁▇
##     1.15       7.5     ▇▁▁▁▁▁▁▇
##  7009.17       1.5e+08 ▇▁▁▁▁▁▁▁
##   147.25    1368       ▇▁▁▃▂▁▁▁
##    98    2250000       ▇▁▁▁▁▁▁▁
##    10          1.9e+07 ▇▁▁▁▁▁▁▁
##     2         48       ▆▁▁▁▁▁▁▇
```

2. Run the code below. Are there any NA's in the data? Does this seem likely?

```r
msleep %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```
The code reflects that there is no NA's in the data, which is incorrect as we can see multiple -999.00s in tehe data spreadsheet. 


3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?
Yes. 
When use NA to replace for the NA symbol (-999, na, NA, whatever), look for it from the skimr output. e.g. from the spreadsheet the -999 turns into -999.00, but it's actually -999


```r
life_historyna2 <-
  life_history %>% 
  na_if("-999") 
```


```r
life_historyna2 %>%   
  purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 13 x 2
##    variables   num_nas
##    <chr>         <int>
##  1 wean_mass      1039
##  2 max_life        841
##  3 litters_yr      689
##  4 weaning         619
##  5 AFR             607
##  6 newborn         595
##  7 gestation       418
##  8 mass             85
##  9 litter_size      84
## 10 order             0
## 11 family            0
## 12 genus             0
## 13 species           0
```


```r
life_historyna2 %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ────────────────────────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
##     variable missing complete    n      mean         sd    p0   p25    p50
##          AFR     607      833 1440     22.44      26.45  0.7   4.5   12   
##    gestation     418     1022 1440      3.86       3.62  0.49  0.99   2.11
##  litter_size      84     1356 1440      2.8        1.77  1     1.02   2.5 
##   litters_yr     689      751 1440      1.64       1.17  0.14  1      1   
##         mass      85     1355 1440 407701.39 5210474.99  2.1  61.15 606   
##     max_life     841      599 1440    224.03     189.74 12    84    192   
##      newborn     595      845 1440  12126.55  118408.21  0.21  4.4   43.7 
##    wean_mass    1039      401 1440  60220.5   953857.17  2.09 20.15 102.6 
##      weaning     619      821 1440      3.97       5.38  0.3   0.92   1.69
##      p75          p100     hist
##    28.24     210       ▇▂▁▁▁▁▁▁
##     6         21.46    ▇▂▂▁▁▁▁▁
##     4         14.18    ▇▃▂▁▁▁▁▁
##     2          7.5     ▇▂▃▁▁▁▁▁
##  8554          1.5e+08 ▇▁▁▁▁▁▁▁
##   288       1368       ▇▆▂▁▁▁▁▁
##   542.5  2250000       ▇▁▁▁▁▁▁▁
##  2000          1.9e+07 ▇▁▁▁▁▁▁▁
##     4.84      48       ▇▁▁▁▁▁▁▁
```



4. Compared to the `msleep` data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.


5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.


6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?


## Wrap-up
Please review the learning goals and be sure to use the code here as a reference when completing the homework.

See you next time!
