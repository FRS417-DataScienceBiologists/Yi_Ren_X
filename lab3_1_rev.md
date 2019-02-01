---
title: "dplyr::select(), mutate(), and pipes"
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
In the last section you were introduced to the **tidyverse** and the function `filter()`. `filter()` works by looking through rows, but what about columns? Recall that each column is a vector, so we need the new function `select()` to separate columns of interest. As you may expect, we can use `filter()` and `select()` together to build data frames focused on our variables of interest.

## Resources
- [R for Data Science](https://r4ds.had.co.nz/)

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Use the `filter()`, `select()`, and `arrange()` functions of dplyr to organize and sort data frames.  
2. Combine dplyr functions to organize and sort data frames using pipes.  

## Libraries

```r
library("tidyverse")
```

## Load the fish scale data
Here we load a .csv file with data on fish scales. We open the file using `read_csv()`. 

```r
fish <- readr::read_csv("~/Desktop/class_files-master/Gaeta_etal_CLC_data.csv")
```



```r
fish %>%
  filter(lakeid == "AL") %>% 
  filter(length == 167)
```

```
## # A tibble: 3 x 6
##   lakeid fish_id annnumber length radii_length_mm scalelength
##   <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
## 1 AL         299 EDGE         167            2.70        2.70
## 2 AL         299 2            167            2.04        2.70
## 3 AL         299 1            167            1.31        2.70
```



## Dplyr: select()
Select allows you to build a new data frame by selecting your columns (variables) of interest. Our fish data only has six columns, but this should give you some ideas especially when you have large data frames with lots of columns.  

We are only interested in lakeid and scalelength.

```r
select(fish, lakeid, scalelength)
```

```
## # A tibble: 4,033 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```


#The `-` operator is useful in select. It allows us to select everything except the specified variables.

```r
select(fish, -fish_id, -annnumber, -length, -radii_length_mm)
```

```
## # A tibble: 4,033 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

#For very large data frames with lots of variables, `select()` uses many different operators to make things easier. Let's say we are only interested in the variables that deal with length.
#contains: means to select for any columns that contain the word in their names

```r
select(fish, contains("length"))
```

```
## # A tibble: 4,033 x 3
##    length radii_length_mm scalelength
##     <dbl>           <dbl>       <dbl>
##  1    167            2.70        2.70
##  2    167            2.04        2.70
##  3    167            1.31        2.70
##  4    175            3.02        3.02
##  5    175            2.67        3.02
##  6    175            2.14        3.02
##  7    175            1.23        3.02
##  8    194            3.34        3.34
##  9    194            2.97        3.34
## 10    194            2.29        3.34
## # … with 4,023 more rows
```

### Some additional options to select columns based on a specific criteria include:  
1. ends_with() = Select columns that end with a character string  
2. contains() = Select columns that contain a character string  
3. matches() = Select columns that match a regular expression  
4. one_of() = Select columns names that are from a group of names  

## Practice
1. What are the names of the columns in the `fish` data?

```r
names (fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


2. We are only interested in the variables `lakeid`, `length`, and `scalelength`. Use `select()` to build a new dataframe focused on these variables.

```r
select(fish,lakeid, length, scalelength)
```

```
## # A tibble: 4,033 x 3
##    lakeid length scalelength
##    <chr>   <dbl>       <dbl>
##  1 AL        167        2.70
##  2 AL        167        2.70
##  3 AL        167        2.70
##  4 AL        175        3.02
##  5 AL        175        3.02
##  6 AL        175        3.02
##  7 AL        175        3.02
##  8 AL        194        3.34
##  9 AL        194        3.34
## 10 AL        194        3.34
## # … with 4,023 more rows
```


## Dplyr: Can we combine filter() and select()?
Absolutely. This is one of the strengths of the tidyverse, it uses the same grammar to specify commands.

```r
fish2 <- select(fish, lakeid, scalelength)
```


```r
filter(fish2, lakeid=="AL")
```

```
## # A tibble: 383 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 373 more rows
```

## Dplyr: Pipes %>% 
The code above works fine but there is a more efficient way. We need to learn pipes `%>%`. Pipes allow you to feed the output from one function to the input of another function. We are going to use pipes from here on to keep things cleaner. (command+shift+m)

```r
fish%>%
  select(lakeid, scalelength)%>%
  filter(lakeid=="AL")
```

```
## # A tibble: 383 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 373 more rows
```

## Practice
1. Build a dataframe focused on the lakes `AL` and `AR` and looking at radii length between 2 and 4 only. Use pipes!


```r
fish %>% 
  filter(lakeid == "AL" | lakeid == "AR") %>% 
  filter(radii_length_mm >=2 & radii_length_mm <=4)
```

```
## # A tibble: 253 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         300 EDGE         175            3.02        3.02
##  4 AL         300 3            175            2.67        3.02
##  5 AL         300 2            175            2.14        3.02
##  6 AL         301 EDGE         194            3.34        3.34
##  7 AL         301 3            194            2.97        3.34
##  8 AL         301 2            194            2.29        3.34
##  9 AL         302 4            324            3.72        6.07
## 10 AL         302 3            324            3.23        6.07
## # … with 243 more rows
```


## Dplyr: arrange()
The `arrange()` command allows us to sort values in a column.

```r
fish %>% 
  arrange(desc(scalelength))
```

```
## # A tibble: 4,033 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 WS         180 EDGE         403           11.0         11.0
##  2 WS         180 16           403           10.6         11.0
##  3 WS         180 15           403           10.3         11.0
##  4 WS         180 14           403            9.93        11.0
##  5 WS         180 13           403            9.56        11.0
##  6 WS         180 12           403            9.17        11.0
##  7 WS         180 11           403            8.62        11.0
##  8 WS         180 10           403            8.15        11.0
##  9 WS         180 9            403            7.49        11.0
## 10 WS         180 8            403            6.97        11.0
## # … with 4,023 more rows
```

#Count

```r
fish %>% 
  count(length)
```

```
## # A tibble: 236 x 2
##    length     n
##     <dbl> <int>
##  1     58     1
##  2     64     1
##  3     74     1
##  4     78     1
##  5     92     1
##  6    104     2
##  7    105     2
##  8    107     2
##  9    118     3
## 10    119     3
## # … with 226 more rows
```



It can be very helpful in combination with the other functions.

```r
fish %>% 
  select(lakeid, length, fish_id, scalelength) %>% 
  filter(lakeid=="AL") %>% 
  arrange(fish_id)
```

```
## # A tibble: 383 x 4
##    lakeid length fish_id scalelength
##    <chr>   <dbl>   <dbl>       <dbl>
##  1 AL        167     299        2.70
##  2 AL        167     299        2.70
##  3 AL        167     299        2.70
##  4 AL        175     300        3.02
##  5 AL        175     300        3.02
##  6 AL        175     300        3.02
##  7 AL        175     300        3.02
##  8 AL        194     301        3.34
##  9 AL        194     301        3.34
## 10 AL        194     301        3.34
## # … with 373 more rows
```

## Practice
1. Build a dataframe focused on the scalelengths of `fish_id` 300 and 301. Use `arrange()` to sort from smallest to largest scalelength. Use pipes!

## Dplyr: mutate()
`mutate()` is used to add new columns to a data frame. When you use mutate() the original data used are preserved. We will briefly practice `mutate()` here and come back to it in more detail later.  


```r
fish %>% 
  select(lakeid, length, fish_id, scalelength) %>% 
  filter(lakeid=="AL") %>% 
  arrange(fish_id) %>% 
  mutate(scale_ratio=(length/scalelength))
```

```
## # A tibble: 383 x 5
##    lakeid length fish_id scalelength scale_ratio
##    <chr>   <dbl>   <dbl>       <dbl>       <dbl>
##  1 AL        167     299        2.70        61.9
##  2 AL        167     299        2.70        61.9
##  3 AL        167     299        2.70        61.9
##  4 AL        175     300        3.02        58.0
##  5 AL        175     300        3.02        58.0
##  6 AL        175     300        3.02        58.0
##  7 AL        175     300        3.02        58.0
##  8 AL        194     301        3.34        58.0
##  9 AL        194     301        3.34        58.0
## 10 AL        194     301        3.34        58.0
## # … with 373 more rows
```


## Practice
1. Add a new column to the fish data that is radii_length_mm divided by scalelength. Add another column that scales this number to a percentage.


```r
fish %>%
  mutate(radsca_ratio = (radii_length_mm/scalelength)) %>% 
  mutate(radsca_ratio_percentage = (radsca_ratio * 100))
```

```
## # A tibble: 4,033 x 8
##    lakeid fish_id annnumber length radii_length_mm scalelength radsca_ratio
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>        <dbl>
##  1 AL         299 EDGE         167            2.70        2.70        1    
##  2 AL         299 2            167            2.04        2.70        0.755
##  3 AL         299 1            167            1.31        2.70        0.486
##  4 AL         300 EDGE         175            3.02        3.02        1    
##  5 AL         300 3            175            2.67        3.02        0.886
##  6 AL         300 2            175            2.14        3.02        0.709
##  7 AL         300 1            175            1.23        3.02        0.408
##  8 AL         301 EDGE         194            3.34        3.34        1    
##  9 AL         301 3            194            2.97        3.34        0.888
## 10 AL         301 2            194            2.29        3.34        0.686
## # … with 4,023 more rows, and 1 more variable:
## #   radsca_ratio_percentage <dbl>
```
