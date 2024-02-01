---
title: "summarize practice, `count()`, `across()`"
date: "2024-02-01"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Produce clear, concise summaries using a variety of functions in `dplyr` and `janitor.`  
2. Use the `across()` operator to produce summaries across multiple variables.  

## Load the libraries

```r
library("tidyverse")
library("janitor")
library("skimr")
library("palmerpenguins")
```

## Review
The summarize() and group_by() functions are powerful tools that we can use to produce clean summaries of data. Especially when used together, we can quickly group variables of interest and save time. Let's do some practice with the [palmerpenguins(https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data to refresh our memory.

```r
glimpse(penguins)
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

As biologists, a good question that we may ask is how do the measured variables differ by island (on average)?

```r
names(penguins)
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```

```r
levels(penguins$island)
```

```
## [1] "Biscoe"    "Dream"     "Torgersen"
```
Because islands is a factor we can use the levels command


```r
penguins %>% 
  group_by(island) %>% 
  summarize(mean_island=mean(island))
```

```
## Warning: There were 3 warnings in `summarize()`.
## The first warning was:
## ℹ In argument: `mean_island = mean(island)`.
## ℹ In group 1: `island = Biscoe`.
## Caused by warning in `mean.default()`:
## ! argument is not numeric or logical: returning NA
## ℹ Run `dplyr::last_dplyr_warnings()` to see the 2 remaining warnings.
```

```
## # A tibble: 3 × 2
##   island    mean_island
##   <fct>           <dbl>
## 1 Biscoe             NA
## 2 Dream              NA
## 3 Torgersen          NA
```


```r
penguins %>% 
  group_by(island) %>% 
  summarize(mean_body_mass=mean(body_mass_g),
            n=n())
```

```
## # A tibble: 3 × 3
##   island    mean_body_mass     n
##   <fct>              <dbl> <int>
## 1 Biscoe               NA    168
## 2 Dream              3713.   124
## 3 Torgersen            NA     52
```

Why do we have NA here? Do all of these penguins lack data?

```r
penguins %>% 
  group_by(island) %>% 
  summarize(number_NA=sum(is.na(body_mass_g)))
```

```
## # A tibble: 3 × 2
##   island    number_NA
##   <fct>         <int>
## 1 Biscoe            1
## 2 Dream             0
## 3 Torgersen         1
```

Nope, we have just 1 penguin messing up the entire calculation 
Well, that won't work so let's remove the NAs and recalculate.

```r
penguins %>% 
  filter(!is.na(body_mass_g)) %>% #take out all the NA and only pick out the things with values. AKA pull out all the observations with a number and leave the NAs behind 
  group_by(island) %>% 
 summarize(mean_body_mass=mean(body_mass_g),
            n=n())
```

```
## # A tibble: 3 × 3
##   island    mean_body_mass     n
##   <fct>              <dbl> <int>
## 1 Biscoe             4716.   167
## 2 Dream              3713.   124
## 3 Torgersen          3706.    51
```

What if we are interested in the number of observations (penguins) by species and island?

```r
penguins %>% 
  group_by(species, island) %>% 
  summarize(n=n(), .groups= 'keep')#the .groups argument here just prevents a warning message
```

```
## # A tibble: 5 × 3
## # Groups:   species, island [5]
##   species   island        n
##   <fct>     <fct>     <int>
## 1 Adelie    Biscoe       44
## 2 Adelie    Dream        56
## 3 Adelie    Torgersen    52
## 4 Chinstrap Dream        68
## 5 Gentoo    Biscoe      124
```
`n=n()` pulls out the total number of individuals
Group by is only going to work with a categorical variable, we have to give it categories 

continuous variable is a number 


## Counts
Although these summary functions are super helpful, oftentimes we are mostly interested in counts. The [janitor package](https://garthtarr.github.io/meatR/janitor.html) does a lot with counts, but there are also functions that are part of dplyr that are useful.  

AKA how many ___. 

`count()` is an easy way of determining how many observations you have within a column. It acts like a combination of `group_by()` and `n()`.

```r
penguins %>% 
  count(island, sort = T) #sort=T sorts the column in descending order
```

```
## # A tibble: 3 × 2
##   island        n
##   <fct>     <int>
## 1 Biscoe      168
## 2 Dream       124
## 3 Torgersen    52
```
Here we want to find out the number of penguins in each of the islands 


```r
tabyl(penguins$island)
```

```
##  penguins$island   n   percent
##           Biscoe 168 0.4883721
##            Dream 124 0.3604651
##        Torgersen  52 0.1511628
```


Compare this with `summarize()` and `group_by()`.

```r
penguins %>% 
  group_by(island) %>% 
  summarize(n=n())
```

```
## # A tibble: 3 × 2
##   island        n
##   <fct>     <int>
## 1 Biscoe      168
## 2 Dream       124
## 3 Torgersen    52
```

You can also use `count()` across multiple variables.

```r
penguins %>% 
  count(island, species, sort = T) # sort=T will arrange in descending order
```

```
## # A tibble: 5 × 3
##   island    species       n
##   <fct>     <fct>     <int>
## 1 Biscoe    Gentoo      124
## 2 Dream     Chinstrap    68
## 3 Dream     Adelie       56
## 4 Torgersen Adelie       52
## 5 Biscoe    Adelie       44
```
You can count the number of individuals in each species catgory in each island catgory. You can have multiple variables in the count. 

For counts, I also like `tabyl()`. Lots of options are supported in (Tabyl is part of janitor)  [tabyl](https://cran.r-project.org/web/packages/janitor/vignettes/tabyls.html)

```r
penguins %>% 
  tabyl(island,species)
```

```
##     island Adelie Chinstrap Gentoo
##     Biscoe     44         0    124
##      Dream     56        68      0
##  Torgersen     52         0      0
```
You can also specify different variables with tabyl as well. 

## Practice
1. How does the mean of `bill_length_mm` compare between penguin species?

```r
penguins%>% 
  group_by(species, bill_length_mm) %>% 
  summarize(mean_bill_length= mean(bill_length_mm,na.RM=T)) ##notice that the na.Rm=T is inside the command taking the mean because this is specifically where we want to remove the NA from for the calculations we want to do 
```

```
## `summarise()` has grouped output by 'species'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 210 × 3
## # Groups:   species [3]
##    species bill_length_mm mean_bill_length
##    <fct>            <dbl>            <dbl>
##  1 Adelie            32.1             32.1
##  2 Adelie            33.1             33.1
##  3 Adelie            33.5             33.5
##  4 Adelie            34               34  
##  5 Adelie            34.1             34.1
##  6 Adelie            34.4             34.4
##  7 Adelie            34.5             34.5
##  8 Adelie            34.6             34.6
##  9 Adelie            35               35  
## 10 Adelie            35.1             35.1
## # ℹ 200 more rows
```

2. For some penguins, their sex is listed as NA. Where do these penguins occur?

```r
penguins %>% 
  count(island,sex)
```

```
## # A tibble: 9 × 3
##   island    sex        n
##   <fct>     <fct>  <int>
## 1 Biscoe    female    80
## 2 Biscoe    male      83
## 3 Biscoe    <NA>       5
## 4 Dream     female    61
## 5 Dream     male      62
## 6 Dream     <NA>       1
## 7 Torgersen female    24
## 8 Torgersen male      23
## 9 Torgersen <NA>       5
```

```r
penguins %>% 
  group_by(island,sex)
```

```
## # A tibble: 344 × 8
## # Groups:   island, sex [9]
##    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
##  1 Adelie  Torgersen           39.1          18.7               181        3750
##  2 Adelie  Torgersen           39.5          17.4               186        3800
##  3 Adelie  Torgersen           40.3          18                 195        3250
##  4 Adelie  Torgersen           NA            NA                  NA          NA
##  5 Adelie  Torgersen           36.7          19.3               193        3450
##  6 Adelie  Torgersen           39.3          20.6               190        3650
##  7 Adelie  Torgersen           38.9          17.8               181        3625
##  8 Adelie  Torgersen           39.2          19.6               195        4675
##  9 Adelie  Torgersen           34.1          18.1               193        3475
## 10 Adelie  Torgersen           42            20.2               190        4250
## # ℹ 334 more rows
## # ℹ 2 more variables: sex <fct>, year <int>
```
Notice the difference. Group by gives you everything. And count gives you just the specified vairbales and their total counts

## `across()`
What about using `filter()` and `select()` across multiple variables? There is a function in dplyr called `across()` which is designed to work across multiple variables. Have a look at Rebecca Barter's awesome blog [here](http://www.rebeccabarter.com/blog/2020-07-09-across/).    

What if we wanted to apply `summarize()` in order to produce distinct counts over multiple variables; i.e. species, island, and sex? Although this isn't a lot of coding you can image that with a lot of variables it would be cumbersome.

```r
penguins %>%
  summarize(distinct_species = n_distinct(species),
            distinct_island = n_distinct(island),
            distinct_sex = n_distinct(sex))
```

```
## # A tibble: 1 × 3
##   distinct_species distinct_island distinct_sex
##              <int>           <int>        <int>
## 1                3               3            3
```

By using `across()` we can reduce the clutter and make things cleaner. Instead of using summarize seperatly for the variables we want 

```r
penguins %>%
  summarize(across(c(species, island, sex), n_distinct))
```

```
## # A tibble: 1 × 3
##   species island   sex
##     <int>  <int> <int>
## 1       3      3     3
```
n_distinct is used to count the number of distinct observations for these variables 
This is very helpful for continuous variables.

```r
penguins %>%
  summarize(across(contains("mm"), mean, na.rm=T))
```

```
## Warning: There was 1 warning in `summarize()`.
## ℹ In argument: `across(contains("mm"), mean, na.rm = T)`.
## Caused by warning:
## ! The `...` argument of `across()` is deprecated as of dplyr 1.1.0.
## Supply arguments directly to `.fns` through an anonymous function instead.
## 
##   # Previously
##   across(a:b, mean, na.rm = TRUE)
## 
##   # Now
##   across(a:b, \(x) mean(x, na.rm = TRUE))
```

```
## # A tibble: 1 × 3
##   bill_length_mm bill_depth_mm flipper_length_mm
##            <dbl>         <dbl>             <dbl>
## 1           43.9          17.2              201.
```
Looking for all the vairables with the "mm" in it 


```r
penguins %>%
  summarize(across(contains("mm"), \(x) mean(x, na.rm = TRUE))) ##Use this to correct the error R may give (the \(x)) thingy 
```

```
## # A tibble: 1 × 3
##   bill_length_mm bill_depth_mm flipper_length_mm
##            <dbl>         <dbl>             <dbl>
## 1           43.9          17.2              201.
```

`group_by` also works.

```r
penguins %>%
  group_by(sex) %>% 
  summarize(across(contains("mm"), mean, na.rm=T))
```

```
## # A tibble: 3 × 4
##   sex    bill_length_mm bill_depth_mm flipper_length_mm
##   <fct>           <dbl>         <dbl>             <dbl>
## 1 female           42.1          16.4              197.
## 2 male             45.9          17.9              205.
## 3 <NA>             41.3          16.6              199
```

Here we summarize across all variables.

```r
penguins %>%
  summarise_all(mean, na.rm=T)
```

```
## Warning: There were 3 warnings in `summarise()`.
## The first warning was:
## ℹ In argument: `species = (function (x, ...) ...`.
## Caused by warning in `mean.default()`:
## ! argument is not numeric or logical: returning NA
## ℹ Run `dplyr::last_dplyr_warnings()` to see the 2 remaining warnings.
```

```
## # A tibble: 1 × 8
##   species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##     <dbl>  <dbl>          <dbl>         <dbl>             <dbl>       <dbl>
## 1      NA     NA           43.9          17.2              201.       4202.
## # ℹ 2 more variables: sex <dbl>, year <dbl>
```

Operators can also work, here I am summarizing across all variables except `species`, `island`, `sex`, and `year`.

```r
penguins %>%
  summarise(across(!c(species, island, sex, year), 
                   mean, na.rm=T))
```

```
## # A tibble: 1 × 4
##   bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##            <dbl>         <dbl>             <dbl>       <dbl>
## 1           43.9          17.2              201.       4202.
```

All variables that include "bill"...all of the other dplyr operators also work.

```r
penguins %>%
  summarise(across(starts_with("bill"), mean, na.rm=T))
```

```
## # A tibble: 1 × 2
##   bill_length_mm bill_depth_mm
##            <dbl>         <dbl>
## 1           43.9          17.2
```

## Practice
1. Produce separate summaries of the mean and standard deviation for bill_length_mm, bill_depth_mm, and flipper_length_mm for each penguin species. Be sure to provide the number of samples.  


## Wrap-up  

Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
