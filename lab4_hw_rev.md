---
title: "Lab 4 Homework"
author: "Yi Ren"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Life History
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

```r
#install.packages("skimr")
```


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


```r
summary(life_history)
```

```
##     order              family             genus          
##  Length:1440        Length:1440        Length:1440       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##    species               mass             gestation      
##  Length:1440        Min.   :     -999   Min.   :-999.00  
##  Class :character   1st Qu.:       50   1st Qu.:-999.00  
##  Mode  :character   Median :      403   Median :   1.05  
##                     Mean   :   383577   Mean   :-287.25  
##                     3rd Qu.:     7009   3rd Qu.:   4.50  
##                     Max.   :149000000   Max.   :  21.46  
##     newborn             weaning          wean_mass       
##  Min.   :   -999.0   Min.   :-999.00   Min.   :    -999  
##  1st Qu.:   -999.0   1st Qu.:-999.00   1st Qu.:    -999  
##  Median :      2.6   Median :   0.73   Median :    -999  
##  Mean   :   6703.1   Mean   :-427.17   Mean   :   16049  
##  3rd Qu.:     98.0   3rd Qu.:   2.00   3rd Qu.:      10  
##  Max.   :2250000.0   Max.   :  48.00   Max.   :19075000  
##       AFR             max_life       litter_size         litters_yr      
##  Min.   :-999.00   Min.   :-999.0   Min.   :-999.000   Min.   :-999.000  
##  1st Qu.:-999.00   1st Qu.:-999.0   1st Qu.:   1.000   1st Qu.:-999.000  
##  Median :   2.50   Median :-999.0   Median :   2.270   Median :   0.375  
##  Mean   :-408.12   Mean   :-490.3   Mean   : -55.634   Mean   :-477.141  
##  3rd Qu.:  15.61   3rd Qu.: 147.2   3rd Qu.:   3.835   3rd Qu.:   1.155  
##  Max.   : 210.00   Max.   :1368.0   Max.   :  14.180   Max.   :   7.500
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

The code shows there's no NA's in the data, which is incorrect. The NA's in this data set is represented as -999 (not -999.00, as reflected in the spreadsheet, because it changes the format automatically).

3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?

NA's represented by -999 
Weaning mass has the most NA's. This is probably because for many mammals, it is hard to weight them at the age of weaning due to inaccessibility.  


```r
life_historyna2 <-
  life_history %>% 
  na_if("-999")     #change all the -999 to NA, so we can count
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
  skimr::skim() #notice the mean, sd stuff now changed, because -999 is removed from calculation.
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


```r
life_history %>%
  group_by(order) %>% 
  summarize(total = n()) 
```

```
## # A tibble: 17 x 2
##    order          total
##    <chr>          <int>
##  1 Artiodactyla     161
##  2 Carnivora        197
##  3 Cetacea           55
##  4 Dermoptera         2
##  5 Hyracoidea         4
##  6 Insectivora       91
##  7 Lagomorpha        42
##  8 Macroscelidea     10
##  9 Perissodactyla    15
## 10 Pholidota          7
## 11 Primates         156
## 12 Proboscidea        2
## 13 Rodentia         665
## 14 Scandentia         7
## 15 Sirenia            5
## 16 Tubulidentata      1
## 17 Xenarthra         20
```



5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.

```r
life_historyna2 %>%
  mutate(lifespan=max_life/12) %>%  #change lifespan unit from month to years
  group_by(order) %>%
  summarize(min=min(lifespan, na.rm=TRUE), #remove all the NA's
            max=max(lifespan, na.rm=TRUE),
            mean=mean(lifespan, na.rm=TRUE),
            sd=sd(lifespan, na.rm=TRUE),
            total=n())
```

```
## # A tibble: 17 x 6
##    order            min    max  mean     sd total
##    <chr>          <dbl>  <dbl> <dbl>  <dbl> <int>
##  1 Artiodactyla    6.75  61    20.7   7.70    161
##  2 Carnivora       5     56    21.1   9.42    197
##  3 Cetacea        13    114    48.8  27.7      55
##  4 Dermoptera     17.5   17.5  17.5  NA         2
##  5 Hyracoidea     11     12.2  11.6   0.884     4
##  6 Insectivora     1.17  13     3.85  2.90     91
##  7 Lagomorpha      6     18     9.02  3.85     42
##  8 Macroscelidea   3      8.75  5.69  2.39     10
##  9 Perissodactyla 21     50    35.5  10.2      15
## 10 Pholidota      20     20    20    NA         7
## 11 Primates        8.83  60.5  25.7  11.0     156
## 12 Proboscidea    70     80    75     7.07      2
## 13 Rodentia        1     35     6.99  5.30    665
## 14 Scandentia      2.67  12.4   8.86  5.38      7
## 15 Sirenia        12.5   73    43.2  30.3       5
## 16 Tubulidentata  24     24    24    NA         1
## 17 Xenarthra       9     40    21.3   9.93     20
```


6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

Longest mean gestation: proboscidea. Common name elephants. 

```r
life_historyna2 %>% 
  group_by(order) %>% 
  summarize(mean_gestation=mean(gestation, na.rm=TRUE),
            mean_newborn_mass=mean(newborn, na.rm=TRUE),
            mean_wean_mass=mean(wean_mass, na.rm=TRUE),
            total=n()) %>% 
  arrange(desc(mean_gestation))
```

```
## # A tibble: 17 x 5
##    order          mean_gestation mean_newborn_mass mean_wean_mass total
##    <chr>                   <dbl>             <dbl>          <dbl> <int>
##  1 Proboscidea             21.3           99523.         600000       2
##  2 Perissodactyla          13.0           27015.         382191.     15
##  3 Cetacea                 11.8          343077.        4796125      55
##  4 Sirenia                 10.8           22878.          67500       5
##  5 Hyracoidea               7.4             231.            500       4
##  6 Artiodactyla             7.26           7082.          51025.    161
##  7 Tubulidentata            7.08           1734            6250       1
##  8 Primates                 5.47            287.           2115.    156
##  9 Xenarthra                4.95            314.            420      20
## 10 Carnivora                3.69           3657.          21020.    197
## 11 Pholidota                3.63            276.           2006.      7
## 12 Dermoptera               2.75             35.9           NaN       2
## 13 Macroscelidea            1.91             24.5           104.     10
## 14 Scandentia               1.63             12.8           102.      7
## 15 Rodentia                 1.31             35.5           135.    665
## 16 Lagomorpha               1.18             57.0           715.     42
## 17 Insectivora              1.15              6.06           33.1    91
```


## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
