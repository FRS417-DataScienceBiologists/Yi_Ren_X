---
title: "Lab 2 Homework"
author: "Yi Ren"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Mammals Sleep
For this assignment, we are going to use built-in data on mammal sleep patterns.  

```r
msleep
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

1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.

For more information on built-in data sets: http://www.sthda.com/english/wiki/r-built-in-data-sets
#If want to learn more about a specific data set: use "?"

```r
?msleep
```

This is an updated and expanded version of the mammals sleep dataset. Updated sleep times and weights were taken from V. M. Savage and G. B. West. A quantitative, theoretical framework for understanding mammalian sleep. Proceedings of the National Academy of Sciences, 104 (3):1051-1056, 2007.


2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.

Summary and Str functions might be the most useful when providing summary information on a data set. They provide the # of rows n variables, the names of each column, type of data, and some basic info on the numerals (min, max, mean, etc.). 

```r
summary(msleep)
```

```
##      name              genus               vore          
##  Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##     order           conservation        sleep_total      sleep_rem    
##  Length:83          Length:83          Min.   : 1.90   Min.   :0.100  
##  Class :character   Class :character   1st Qu.: 7.85   1st Qu.:0.900  
##  Mode  :character   Mode  :character   Median :10.10   Median :1.500  
##                                        Mean   :10.43   Mean   :1.875  
##                                        3rd Qu.:13.75   3rd Qu.:2.400  
##                                        Max.   :19.90   Max.   :6.600  
##                                                        NA's   :22     
##   sleep_cycle         awake          brainwt            bodywt        
##  Min.   :0.1167   Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:0.1833   1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :0.3333   Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :0.4396   Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:0.5792   3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :1.5000   Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##  NA's   :51                       NA's   :27
```


```r
str(msleep)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	83 obs. of  11 variables:
##  $ name        : chr  "Cheetah" "Owl monkey" "Mountain beaver" "Greater short-tailed shrew" ...
##  $ genus       : chr  "Acinonyx" "Aotus" "Aplodontia" "Blarina" ...
##  $ vore        : chr  "carni" "omni" "herbi" "omni" ...
##  $ order       : chr  "Carnivora" "Primates" "Rodentia" "Soricomorpha" ...
##  $ conservation: chr  "lc" NA "nt" "lc" ...
##  $ sleep_total : num  12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
##  $ sleep_rem   : num  NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
##  $ sleep_cycle : num  NA NA NA 0.133 0.667 ...
##  $ awake       : num  11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
##  $ brainwt     : num  NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
##  $ bodywt      : num  50 0.48 1.35 0.019 600 ...
```


3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal. Sort the data in descending order by body weight.

To sort data in a particular order, use arrange command.

```r
msleep %>%
  select(name, genus, bodywt) %>% 
  arrange(desc(bodywt))
```

```
## # A tibble: 83 x 3
##    name                 genus         bodywt
##    <chr>                <chr>          <dbl>
##  1 African elephant     Loxodonta      6654 
##  2 Asian elephant       Elephas        2547 
##  3 Giraffe              Giraffa         900.
##  4 Pilot whale          Globicephalus   800 
##  5 Cow                  Bos             600 
##  6 Horse                Equus           521 
##  7 Brazilian tapir      Tapirus         208.
##  8 Donkey               Equus           187 
##  9 Bottle-nosed dolphin Tursiops        173.
## 10 Tiger                Panthera        163.
## # … with 73 more rows
```


4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total. Make two new dataframes (large and small) based on these parameters. Sort the data in descending order by body weight.

#1. To create a new data frame from filtered and selected result
#2. To select, filter, and arrange
#3. Print the new data frame

4-1. Small mammals

```r
smallmammals <- data.frame(
msleep %>% 
  select (name,bodywt,sleep_total) %>% 
  filter (bodywt <= 1) %>% 
  arrange(desc(bodywt))
)
smallmammals
```

```
##                              name bodywt sleep_total
## 1       African giant pouched rat  1.000         8.3
## 2          Arctic ground squirrel  0.920        16.6
## 3                          Tenrec  0.900        15.6
## 4               European hedgehog  0.770        10.1
## 5                 Squirrel monkey  0.743         9.6
## 6                      Guinea pig  0.728         9.4
## 7                 Desert hedgehog  0.550        10.3
## 8                      Owl monkey  0.480        17.0
## 9                      Chinchilla  0.420        12.5
## 10           Thick-tailed opposum  0.370        19.4
## 11                 Laboratory rat  0.320        13.0
## 12           Round-tailed muskrat  0.266        14.6
## 13                           Degu  0.210         7.7
## 14 Golden-mantled ground squirrel  0.205        15.9
## 15                         Galago  0.200         9.8
## 16                     Cotton rat  0.148        11.3
## 17                       Mole rat  0.122        10.6
## 18                 Golden hamster  0.120        14.3
## 19      Eastern american chipmunk  0.112        15.8
## 20                     Tree shrew  0.104         8.9
## 21 Thirteen-lined ground squirrel  0.101        13.8
## 22          Eastern american mole  0.075         8.4
## 23      Western american chipmunk  0.071        14.9
## 24                Star-nosed mole  0.060        10.3
## 25               Mongolian gerbil  0.053        14.2
## 26                     Musk shrew  0.048        12.8
## 27                   Vesper mouse  0.045         7.0
## 28          African striped mouse  0.044         8.7
## 29                          Vole   0.035        12.8
## 30     Northern grasshopper mouse  0.028        14.5
## 31                  Big brown bat  0.023        19.7
## 32                    House mouse  0.022        12.5
## 33                     Deer mouse  0.021        11.5
## 34     Greater short-tailed shrew  0.019        14.9
## 35               Little brown bat  0.010        19.9
## 36      Lesser short-tailed shrew  0.005         9.1
```


4-2. large mammals

```r
largemammals <- data.frame(
msleep %>% 
  select (name,bodywt,sleep_total) %>% 
  filter (bodywt >= 200) %>% 
  arrange(desc(bodywt))
)
largemammals
```

```
##               name   bodywt sleep_total
## 1 African elephant 6654.000         3.3
## 2   Asian elephant 2547.000         3.9
## 3          Giraffe  899.995         1.9
## 4      Pilot whale  800.000         2.7
## 5              Cow  600.000         4.0
## 6            Horse  521.000         2.9
## 7  Brazilian tapir  207.501         4.4
```




5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?

```r
#mean(large$sleep_total)
lmavesleep <- mean(largemammals$sleep_total)
lmavesleep
```

```
## [1] 3.3
```

6. What is the average sleep duration for small mammals as we have defined them?
 

```r
smavesleep <- mean(smallmammals$sleep_total)
smavesleep
```

```
## [1] 12.65833
```


So do large mammals sleep longer than small mammals?


```r
if (lmavesleep > smavesleep){
  print ("Large mammals sleep, on average, longer than small mammals.")
}else{
  print ("Small mammals sleep, on average, longer than large mammals.")
}
```

```
## [1] "Small mammals sleep, on average, longer than large mammals."
```


7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total. Sort by order and sleep total.


```r
msleep %>% 
  select(name, genus, order, sleep_total) %>% 
  filter(sleep_total >= 18) %>% 
  arrange(order, sleep_total)
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 Big brown bat          Eptesicus  Chiroptera             19.7
## 2 Little brown bat       Myotis     Chiroptera             19.9
## 3 Giant armadillo        Priodontes Cingulata              18.1
## 4 North American Opossum Didelphis  Didelphimorphia        18  
## 5 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
```


## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
