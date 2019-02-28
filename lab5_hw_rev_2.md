---
title: "Lab 5 Homework"
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
Let's revisit the mammal life history data to practice our `ggplot` skills. Some of the tidy steps will be a repeat from the homework, but it is good practice. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*

1. Load the data.

```r
mlh <- readr::read_csv("~/Desktop/class_files-master/data/mammal_lifehistories_v2.csv")
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


2. Use your preferred function to have a look. Do you notice any problems?

There are NA's represented as -999.


```r
library("skimr")
mlh %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ─────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     Genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ───────────────────────────────────────────────────────
##      variable missing complete    n      mean         sd   p0  p25     p50
##           AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##     gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##   litter size       0     1440 1440    -55.63     234.88 -999    1    2.27
##  litters/year       0     1440 1440   -477.14     500.03 -999 -999    0.38
##          mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max. life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##       newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##     wean mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##       weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
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
summary(mlh)
```

```
##     order              family             Genus          
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
##     newborn             weaning          wean mass       
##  Min.   :   -999.0   Min.   :-999.00   Min.   :    -999  
##  1st Qu.:   -999.0   1st Qu.:-999.00   1st Qu.:    -999  
##  Median :      2.6   Median :   0.73   Median :    -999  
##  Mean   :   6703.1   Mean   :-427.17   Mean   :   16049  
##  3rd Qu.:     98.0   3rd Qu.:   2.00   3rd Qu.:      10  
##  Max.   :2250000.0   Max.   :  48.00   Max.   :19075000  
##       AFR            max. life       litter size        litters/year     
##  Min.   :-999.00   Min.   :-999.0   Min.   :-999.000   Min.   :-999.000  
##  1st Qu.:-999.00   1st Qu.:-999.0   1st Qu.:   1.000   1st Qu.:-999.000  
##  Median :   2.50   Median :-999.0   Median :   2.270   Median :   0.375  
##  Mean   :-408.12   Mean   :-490.3   Mean   : -55.634   Mean   :-477.141  
##  3rd Qu.:  15.61   3rd Qu.: 147.2   3rd Qu.:   3.835   3rd Qu.:   1.155  
##  Max.   : 210.00   Max.   :1368.0   Max.   :  14.180   Max.   :   7.500
```

3. There are NA's. How are you going to deal with them?


```r
mlh2 <-
  mlh %>% 
  na_if("-999")     #change all the -999 to NA, so we can count
```

4. Where are the NA's? This is important to keep in mind as you build plots.

Mostly in wean mass. Also in max.life, litters/year, weaning, AFR, newborn, gestation, mass, and littersize.


```r
mlh2 %>%   
  purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 13 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 wean mass       1039
##  2 max. life        841
##  3 litters/year     689
##  4 weaning          619
##  5 AFR              607
##  6 newborn          595
##  7 gestation        418
##  8 mass              85
##  9 litter size       84
## 10 order              0
## 11 family             0
## 12 Genus              0
## 13 species            0
```

5. Some of the variable names will be problematic. Let's rename them here as a final tidy step.


```r
mlh_renamed <- mlh2 %>% 
  dplyr::rename(genus = Genus,
          wean_mass = `wean mass`,
          max_life = `max. life`,
          litter_size = `litter size`,
          litters_yr = `litters/year`)
```



##`ggplot()`
For the questions below, try to use the aesthetics you have learned to make visually appealing and informative plots. Make sure to include labels for the axes and titles.

```r
options(scipen=999) #cancels the use of scientific notation for the session
```

6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship. 

!!if there are NA's in the data, R won't plot them


```r
ggplot(data = mlh_renamed, mapping = aes(x=newborn, y=gestation)) +
  geom_point()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_rev_2_files/figure-html/unnamed-chunk-9-1.png)<!-- -->



7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn't talk about this in lab, but you can use `scale_x_log10()` as a layer to correct for this issue. This will log transform the y-axis values.


```r
ggplot(data = mlh_renamed, mapping = aes(x=newborn, y=gestation)) +
  scale_x_log10()+
  geom_point()+
  labs(title = "Gestation time vs. Newborn mass in Mammals",
      x = "Newborn mass (g)",
      y = "Gestation (months)")
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_rev_2_files/figure-html/unnamed-chunk-10-1.png)<!-- -->



8. Now that you have the basic plot, color the points by taxonomic order.


```r
  ggplot(data = mlh_renamed, mapping = aes(x=newborn, y=gestation, color=order))+
  scale_x_log10()+
  geom_point()+
  labs(title = "Gestation time vs. Newborn mass in Mammals",
       x = "Newborn mass (g)",
       y = "Gestation (months)")
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_rev_2_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


9. Lastly, make the size of the points proportional to body mass.


```r
  ggplot(data = mlh_renamed, mapping = aes(x=newborn, y=gestation, color=order, size=mass))+
  scale_x_log10()+
  geom_point()+
  labs(title = "Gestation time vs. Newborn mass in Mammals",
       x = "Newborn mass (g)",
       y = "Gestation (months)",
       color="Taxonomic Order")+
  scale_size(guide = "none")            #Exampled in teacher's HW. But what does this do? 
```

```
## Warning: Removed 691 rows containing missing values (geom_point).
```

![](lab5_hw_rev_2_files/figure-html/unnamed-chunk-12-1.png)<!-- -->



10. Make a plot that shows the range of lifespan by order.


```r
mlh_renamed %>% 
  ggplot(aes(x=order, y=max_life, fill=order))+  #if use "color" here, the box edge gets colored
  geom_boxplot()+
  coord_flip()+
  scale_y_log10()+
  labs(title = "Lifespan by Taxonomic Order",
       x = "Taxonomic Order",
       y = "Lifespan (months)",
       fill="Taxonomic Order")+     #does nothing...?
  theme(legend.position = "right")   #places legend at the right of the graph. Could change to left, top, bottom.
```

```
## Warning: Removed 841 rows containing non-finite values (stat_boxplot).
```

![](lab5_hw_rev_2_files/figure-html/unnamed-chunk-13-1.png)<!-- -->



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
