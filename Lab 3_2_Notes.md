---
title: "Tidy data"
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

> **For Big-Data Scientists, ‘Janitor Work’ Is Key Hurdle to Insights.**  
> "Data scientists, according to interviews and expert estimates, spend from 50 percent to 80 percent of their time mired in the mundane labor of collecting and preparing data, before it can be explored for useful information."  
> [New York Times (2014)](http://www.nytimes.com/2014/08/18/technology/for-big-data-scientists-hurdle-to-insights-is-janitor-work.html)

## Overview
The  quote sums above sums up much of the work in data science. Simply put, most of the data that you end up working with will be messy, disorganized, and not **tidy**. By the end of the course, you will become a master of wrangling messy data into tidy data that are organized and ready for analysis.

## Resources
- [dplyr-tidyr tutorial](http://tclavelle.github.io/dplyr-tidyr-tutorial/)
- [Data wrangling cheatsheet (`dplyr`,`tidyr`)](http://ucsb-bren.github.io/refs/cheatsheets/data-wrangling-cheatsheet.pdf)
- [Tidyr, R for Data Science](https://r4ds.had.co.nz/tidy-data.html#unite)

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Explain the difference between tidy and messy data.  
2. Evaluate a dataset as tidy or untidy.  
3. Use the functions of tidyr to transform messy data to tidy data.

## Load the library

```r
library(tidyverse)
```

## Tidy data
Most data that you will encounter are organized incorrectly for work in R and, as you might expect, the tools used to transform data are a core part of the tidyverse. I will attempt to summarize the most important points below, but you should read chapter 12 of the data science text (https://r4ds.had.co.nz/tidy-data.html).  

`Tidy` data in the sense of the tidyverse follows three conventions:   
(1) each variable has its own column  
(2) each observation has its own row  
(3) each value has its own cell  


I think this, and many other, explanations of tidy data are good but they don't emphasize a key point: R was built around working with vectors (which are stored as columns) and this is inherent in how it uses data. So, in order for many of the functions in R to work data need to be organized accordingly. The package used to tidy data is called **tidyr** and is a core part of the tidyverse.  

## Tidyr
+ `gather()` and `spread()` convert data between wide and long format  
+ `separate()` and `unite()` separates or unites information in columns  

The following data are results from a drug trial that shows the effect of four different treatments on six patients. The values represent resting heart rate.

```r
non_tidy1 <- data.frame(
  patient= c("Margaret", "Frank", "Hawkeye", "Trapper", "Radar", "Henry"),
  a= c(72, 84, 64, 60, 74, 88),
  b= c(74, 84, 66, 58, 72, 87),
  c= c(80, 88, 68, 64, 78, 88),
  d= c(68, 76, 64, 58, 70, 72)
)
non_tidy1
```

```
##    patient  a  b  c  d
## 1 Margaret 72 74 80 68
## 2    Frank 84 84 88 76
## 3  Hawkeye 64 66 68 64
## 4  Trapper 60 58 64 58
## 5    Radar 74 72 78 70
## 6    Henry 88 87 88 72
```
Let's assess whether or not these data are tidy.  

(1) each variable has its own column  
*No. The columns are actually variables.*  
(2) each observation has its own row  
*No. The observations are grouped in a single row by patient.*  
(3) each value has its own cell  
*Yes. There are no unusual combinations of data in each cell.*    

Because rules 1 and 2 are violated, these data are **not** tidy. We need to use tidyr to make them useable in R. As a final demonstration, let's plot these data. We haven't covered plots yet but this is a good first example.

```r
plot(non_tidy1)
```

![](lab3_2_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
R cannot make sense of these data; the plot is nonsense. Even though I gave no specific plotting instructions, R should be able to produce something intelligible or ask you to identify axes.  

## Why are data untidy? 
Scientists frequently use excel spreadsheets that are organized to make data entry efficient. This is typically referred to as **wide format**. From an R perspective, this creates problems because R fundamentally works with vectors. If you have a column of mixed classes or values that correspond to more than a single variable then many of the important R functions will not work.  

By far, the most common problem is to have **column names actually represent values of a variable**. Our `non_tidy1` example has columns a, b, c, and d that actually represent different drug trials.  

## gather()
gather() is the function used most when dealing with non-tidy data. It allows us to transform wide data into long data.  

```r
non_tidy1
```

```
##    patient  a  b  c  d
## 1 Margaret 72 74 80 68
## 2    Frank 84 84 88 76
## 3  Hawkeye 64 66 68 64
## 4  Trapper 60 58 64 58
## 5    Radar 74 72 78 70
## 6    Henry 88 87 88 72
```

The syntax to gather() is a bit strange. From the [Tidyr, R for Data Science](https://r4ds.had.co.nz/tidy-data.html#unite) text, you need to know three things in order to use the function:  

(1) The set of columns that represent values, not variables.  
*In our case, these are the different drug treatment columns a, b, c, d.*
(2) The name of the variable whose values form the column names.  
*This is called the key, and in our data this is the drug. What is the new column name?*
(3) The name of the variable whose values are spread over the cells.  
*This is called the value, and in our case is the heart rate.*


```r
tidy1<-non_tidy1 %>% 
  gather(a, b, c, d, key="drug", value="heartrate")
tidy1
```

```
##     patient drug heartrate
## 1  Margaret    a        72
## 2     Frank    a        84
## 3   Hawkeye    a        64
## 4   Trapper    a        60
## 5     Radar    a        74
## 6     Henry    a        88
## 7  Margaret    b        74
## 8     Frank    b        84
## 9   Hawkeye    b        66
## 10  Trapper    b        58
## 11    Radar    b        72
## 12    Henry    b        87
## 13 Margaret    c        80
## 14    Frank    c        88
## 15  Hawkeye    c        68
## 16  Trapper    c        64
## 17    Radar    c        78
## 18    Henry    c        88
## 19 Margaret    d        68
## 20    Frank    d        76
## 21  Hawkeye    d        64
## 22  Trapper    d        58
## 23    Radar    d        70
## 24    Henry    d        72
```

Now, we can reverify by doing our tidy check.  

(1) each variable has its own column  
*Yes. Patient, drug, and heartrate are separated into individual columns.*  
(2) each observation has its own row  
*Yes. Each row has exactly one observation broken down by drug.*  
(3) each value has its own cell  
*Yes. There are no unusual combinations of data in each cell.*   

And, as a final check let's try the plot command again. I do need to tell R the x and y axes.

```r
plot(tidy1$patient, tidy1$heartrate)
```

![](lab3_2_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## Practice
The data below track tuberculosis infection rates by year and country.

```r
country <- c("Afghanistan", "Brazil", "China")
`1999` <- c(745, 37737, 212258)
`2000` <- c(2666, 80488, 213766)
tb_data <- data.frame(country=country, `1999`=`1999`, `2000`=`2000`)
tb_data
```

```
##       country  X1999  X2000
## 1 Afghanistan    745   2666
## 2      Brazil  37737  80488
## 3       China 212258 213766
```

1. Are these data tidy? Why not? Identify the specific problem(s).
No. Because the columns are variables.

2. Use gather() to tidy the data.

```r
tb_data %>% 
  gather (X1999, X2000, key = "Year", value = "rate")
```

```
##       country  Year   rate
## 1 Afghanistan X1999    745
## 2      Brazil X1999  37737
## 3       China X1999 212258
## 4 Afghanistan X2000   2666
## 5      Brazil X2000  80488
## 6       China X2000 213766
```



## separate()
In our next example, we have  the sex of each patient included with their name. Are these data tidy? No, there is more than one value per cell in the patient column and the columns a, b, c, d once again represent values.

```r
non_tidy2 <- data.frame(
  patient= c("Margaret_f", "Frank_m", "Hawkeye_m", "Trapper_m", "Radar_m", "Henry_m"),
  a= c(72, 84, 64, 60, 74, 88),
  b= c(74, 84, 66, 58, 72, 87),
  c= c(80, 88, 68, 64, 78, 88),
  d= c(68, 76, 64, 58, 70, 72)
)
non_tidy2
```

```
##      patient  a  b  c  d
## 1 Margaret_f 72 74 80 68
## 2    Frank_m 84 84 88 76
## 3  Hawkeye_m 64 66 68 64
## 4  Trapper_m 60 58 64 58
## 5    Radar_m 74 72 78 70
## 6    Henry_m 88 87 88 72
```

We need to start by separating the patient names from their sexes. `separate()` needs to know which column you want to split, the names of the new columns, and what to look for in terms of breaks in the data.

```r
non_tidy2 %>% 
  separate(patient, into= c("patient", "sex"), sep = "_")
```

```
##    patient sex  a  b  c  d
## 1 Margaret   f 72 74 80 68
## 2    Frank   m 84 84 88 76
## 3  Hawkeye   m 64 66 68 64
## 4  Trapper   m 60 58 64 58
## 5    Radar   m 74 72 78 70
## 6    Henry   m 88 87 88 72
```

This is great; we have separated sex from patient. Are the data tidy? Not yet. We still need to use `gather()`.

```r
tidy2 <- non_tidy2 %>% 
  separate(patient, into= c("patient", "sex"), sep = "_") %>% 
  gather(a, b, c, d, key="drug", value="heartrate")
tidy2
```

```
##     patient sex drug heartrate
## 1  Margaret   f    a        72
## 2     Frank   m    a        84
## 3   Hawkeye   m    a        64
## 4   Trapper   m    a        60
## 5     Radar   m    a        74
## 6     Henry   m    a        88
## 7  Margaret   f    b        74
## 8     Frank   m    b        84
## 9   Hawkeye   m    b        66
## 10  Trapper   m    b        58
## 11    Radar   m    b        72
## 12    Henry   m    b        87
## 13 Margaret   f    c        80
## 14    Frank   m    c        88
## 15  Hawkeye   m    c        68
## 16  Trapper   m    c        64
## 17    Radar   m    c        78
## 18    Henry   m    c        88
## 19 Margaret   f    d        68
## 20    Frank   m    d        76
## 21  Hawkeye   m    d        64
## 22  Trapper   m    d        58
## 23    Radar   m    d        70
## 24    Henry   m    d        72
```

## unite()
`unite()` is the opposite of separate(). It's syntax is relatively straightforward. You only need to identify the key and the value.

```r
tidy2 %>% 
  spread(key="drug", value="heartrate") %>% 
  unite(patient_sex, patient, sex, sep="_")
```

```
##   patient_sex  a  b  c  d
## 1     Frank_m 84 84 88 76
## 2   Hawkeye_m 64 66 68 64
## 3     Henry_m 88 87 88 72
## 4  Margaret_f 72 74 80 68
## 5     Radar_m 74 72 78 70
## 6   Trapper_m 60 58 64 58
```

## rename()
The rename function is actually part of *dplyr*, but I put it here because I think of it as part of transforming untidy data.

```r
tidy2
```

```
##     patient sex drug heartrate
## 1  Margaret   f    a        72
## 2     Frank   m    a        84
## 3   Hawkeye   m    a        64
## 4   Trapper   m    a        60
## 5     Radar   m    a        74
## 6     Henry   m    a        88
## 7  Margaret   f    b        74
## 8     Frank   m    b        84
## 9   Hawkeye   m    b        66
## 10  Trapper   m    b        58
## 11    Radar   m    b        72
## 12    Henry   m    b        87
## 13 Margaret   f    c        80
## 14    Frank   m    c        88
## 15  Hawkeye   m    c        68
## 16  Trapper   m    c        64
## 17    Radar   m    c        78
## 18    Henry   m    c        88
## 19 Margaret   f    d        68
## 20    Frank   m    d        76
## 21  Hawkeye   m    d        64
## 22  Trapper   m    d        58
## 23    Radar   m    d        70
## 24    Henry   m    d        72
```

Let's say I don't like the names of the columns. They can be renamed, just remember to replace or build a new object to keep things clean. (syntax is *new name* = *old name*)

```r
tidy3 <- tidy2 %>% 
  dplyr::rename(
    MASH_character = patient,
    Sex = sex,
    Drug = drug,
    Heartrate_bpm = heartrate)
tidy3
```

```
##    MASH_character Sex Drug Heartrate_bpm
## 1        Margaret   f    a            72
## 2           Frank   m    a            84
## 3         Hawkeye   m    a            64
## 4         Trapper   m    a            60
## 5           Radar   m    a            74
## 6           Henry   m    a            88
## 7        Margaret   f    b            74
## 8           Frank   m    b            84
## 9         Hawkeye   m    b            66
## 10        Trapper   m    b            58
## 11          Radar   m    b            72
## 12          Henry   m    b            87
## 13       Margaret   f    c            80
## 14          Frank   m    c            88
## 15        Hawkeye   m    c            68
## 16        Trapper   m    c            64
## 17          Radar   m    c            78
## 18          Henry   m    c            88
## 19       Margaret   f    d            68
## 20          Frank   m    d            76
## 21        Hawkeye   m    d            64
## 22        Trapper   m    d            58
## 23          Radar   m    d            70
## 24          Henry   m    d            72
```

## Practice
In this example study, ten participants were asked to categorize three face styles by clicking various buttons that represent three different categories (face 1, face 2, face 3). The time it took to click a button is in milliseconds.

```r
faces <- data.frame(
  ParticipantID_sex = c("001_m", "002_f", "003_f", "004_f", "005_m", "006_f", "007_m", "008_m", "009_m", "010_f"),
  Face_1 = c(411,723,325,456,579,612,709,513,527,379),
  Face_2 = c(123,300,400,500,600,654,789,906,413,567),
  Face_3 = c(1457,1000,569,896,956,2345,780,599,1023,678)
)
faces
```

```
##    ParticipantID_sex Face_1 Face_2 Face_3
## 1              001_m    411    123   1457
## 2              002_f    723    300   1000
## 3              003_f    325    400    569
## 4              004_f    456    500    896
## 5              005_m    579    600    956
## 6              006_f    612    654   2345
## 7              007_m    709    789    780
## 8              008_m    513    906    599
## 9              009_m    527    413   1023
## 10             010_f    379    567    678
```

1. Are these data tidy? Why or why not?  
No. The columns are variables. 

2. Tidy the data and place them into a new dataframe.


```r
tidy4 <- faces %>% 
  separate(ParticipantID_sex, into= c("ParticipantID", "sex"), sep = "_") %>% 
  gather(Face_1, Face_2, Face_3, key="Category", value="Time")
tidy4
```

```
##    ParticipantID sex Category Time
## 1            001   m   Face_1  411
## 2            002   f   Face_1  723
## 3            003   f   Face_1  325
## 4            004   f   Face_1  456
## 5            005   m   Face_1  579
## 6            006   f   Face_1  612
## 7            007   m   Face_1  709
## 8            008   m   Face_1  513
## 9            009   m   Face_1  527
## 10           010   f   Face_1  379
## 11           001   m   Face_2  123
## 12           002   f   Face_2  300
## 13           003   f   Face_2  400
## 14           004   f   Face_2  500
## 15           005   m   Face_2  600
## 16           006   f   Face_2  654
## 17           007   m   Face_2  789
## 18           008   m   Face_2  906
## 19           009   m   Face_2  413
## 20           010   f   Face_2  567
## 21           001   m   Face_3 1457
## 22           002   f   Face_3 1000
## 23           003   f   Face_3  569
## 24           004   f   Face_3  896
## 25           005   m   Face_3  956
## 26           006   f   Face_3 2345
## 27           007   m   Face_3  780
## 28           008   m   Face_3  599
## 29           009   m   Face_3 1023
## 30           010   f   Face_3  678
```


3. Use `rename()` to rename a few columns for practice.


```r
tidy5 <- tidy4 %>% 
  dplyr::rename(
    PaID = ParticipantID,
    Sex = sex,
    Faces_Category = Category,
    AveTimeTook = Time)
tidy5
```

```
##    PaID Sex Faces_Category AveTimeTook
## 1   001   m         Face_1         411
## 2   002   f         Face_1         723
## 3   003   f         Face_1         325
## 4   004   f         Face_1         456
## 5   005   m         Face_1         579
## 6   006   f         Face_1         612
## 7   007   m         Face_1         709
## 8   008   m         Face_1         513
## 9   009   m         Face_1         527
## 10  010   f         Face_1         379
## 11  001   m         Face_2         123
## 12  002   f         Face_2         300
## 13  003   f         Face_2         400
## 14  004   f         Face_2         500
## 15  005   m         Face_2         600
## 16  006   f         Face_2         654
## 17  007   m         Face_2         789
## 18  008   m         Face_2         906
## 19  009   m         Face_2         413
## 20  010   f         Face_2         567
## 21  001   m         Face_3        1457
## 22  002   f         Face_3        1000
## 23  003   f         Face_3         569
## 24  004   f         Face_3         896
## 25  005   m         Face_3         956
## 26  006   f         Face_3        2345
## 27  007   m         Face_3         780
## 28  008   m         Face_3         599
## 29  009   m         Face_3        1023
## 30  010   f         Face_3         678
```


## Wrap-up
Please review the learning goals and be sure to use the code here as a reference when completing the homework.

See you next time!

