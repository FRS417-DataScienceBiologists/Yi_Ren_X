---
title: "Lab 1 Homework"
author: "Yi Ren"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
  pdf_document:
    toc: no
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

1. Navigate to the R console and calculate the following expressions.  
  + 5 - 3 * 2  
  + 8 / 2 ** 2  
  

```r
5 - 3*2
```

```
## [1] -1
```

```r
8/2**2
```

```
## [1] 2
```

  
2. Did any of the results in #4 surprise you? Write two programs that calculate each expression such that the result for the first example is 4 and the second example is 16.  

#These results did not surprise me, as R followed the conventional operation order (power first, multiplication & division second, addition & subtraction last, etc.) that we learned. 


```r
(5-3)*2
```

```
## [1] 4
```

```r
(8/2)**2
```

```
## [1] 16
```


3. You have decided to use your new analytical powers in R to become a professional gambler. Here are your winnings and losses this week.

```r
blackjack <- c(140, -20, 70, -120, 240)
roulette <- c(60, 50, 120, -300, 10)
```

a. Build a new vector called `days` for the days of the week (Monday through Friday). 

```r
days <- c("Monday","Tuesday","Wednesday","Thursday","Friday")
```

We will use `days` to name the elements in the poker and roulette vectors.

```r
names(blackjack) <- days
names(roulette) <- days
```

b. Calculate how much you won or lost in blackjack over the week.

```r
total_blackjack <- sum(blackjack)
total_blackjack
```

c. Calculate how much you won or lost in roulette over the week.  

```r
total_roulette <- sum(roulette)
total_roulette
```

d. Build a `total_week` vector to show how much you lost or won on each day over the week. Which days seem lucky or unlucky for you?

#Regarding naming vectors, I wonder if there's a easier way other than typing everything out.

```r
MonSum <- (blackjack[1]+roulette[1])
TueSum <- (blackjack[2]+roulette[2])
WednSum <- (blackjack[3]+roulette[3])
ThuSum <- (blackjack[4]+roulette[4])
FriSum <- (blackjack[5]+roulette[5])

daily_sum <- c(MonSum,TueSum,WednSum,ThuSum,FriSum)

total_week <- c(blackjack,roulette,daily_sum)
names(total_week) <- c("Mon_jackblack","Tue_jackblack","Wedn_jackblack","Thu_jackblack","Fri_jackblack","Mon_roulette","Tue_roulette","Wedn_roulette","Thu_roulette","Fri_roulette","Mon_total","Tue_total","Wedn_total","Thu_total","Fri_total")

total_week
```

#Friday seems to be the luckiest day and Thursday seems to be unlucky.


e. Should you stick to blackjack or roulette? Write a program that verifies this below.

#Here I tried to perform the if...else statement with R, to find out which one gives greater profit. I learned some basic grammar such as to use braces after "if" and "else" command

```r
if (total_blackjack > total_roulette){
  print ("Stick to blackjack")
}else{
  print ("Stick to roulette")
}
```
 
## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
