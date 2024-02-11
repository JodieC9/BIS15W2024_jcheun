---
title: "Midterm Review and `across()` "
date: "2024-02-10"
output:
  html_document: 
    theme: spacelab
    toc: true
    toc_float: true
    keep_md: true
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Work through midterm 1 and practice some new approaches to solving common problems in R.    
2. Use the `across()` operator to produce summaries across multiple variables. 

## Load the libraries

```r
library("tidyverse")
library("skimr")
library("janitor")
library("palmerpenguins")
```

## Midterm 1 Review
Let's briefly review the questions from midterm 1 so you can get an idea of how I was thinking about the problems. Remember, there is more than one way to get at these answers, so don't worry if yours looks different than mine!  

## Penguins
We rushed through the second part of lab 7 last week, so let's do a quick review of some key functions. As you know, summarize() and group_by() are powerful tools that we can use to produce clean summaries of data. Especially when used together, we can quickly group variables of interest and save time. Let's do some practice with the [palmerpenguins(https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data to refresh our memory.


```r
?penguins
```

Recall that group_by() and summarize() work great together. Let's say we were interested in how body mass varied by island. It is reasonable to assume that the islands are different, so maybe the penguins are as well.

```r
names(penguins)
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```



```r
penguins %>% 
  group_by(island) %>% 
  summarize(mean_body_mass_g=mean(body_mass_g, na.rm=T)) # remember to remove those NA's!
```

```
## # A tibble: 3 × 2
##   island    mean_body_mass_g
##   <fct>                <dbl>
## 1 Biscoe               4716.
## 2 Dream                3713.
## 3 Torgersen            3706.
```

What if we are interested in the number of observations (penguins) by species and island?

```r
penguins %>% 
  group_by(island, species) %>% 
  summarize(n_penguins=n(), .groups = 'keep')
```

```
## # A tibble: 5 × 3
## # Groups:   island, species [5]
##   island    species   n_penguins
##   <fct>     <fct>          <int>
## 1 Biscoe    Adelie            44
## 2 Biscoe    Gentoo           124
## 3 Dream     Adelie            56
## 4 Dream     Chinstrap         68
## 5 Torgersen Adelie            52
```

Recall that that `count()` works like a combination of `group_by()` and `summarize()` but just shows the number of observations.

```r
penguins %>% 
  count(island, species)
```

```
## # A tibble: 5 × 3
##   island    species       n
##   <fct>     <fct>     <int>
## 1 Biscoe    Adelie       44
## 2 Biscoe    Gentoo      124
## 3 Dream     Adelie       56
## 4 Dream     Chinstrap    68
## 5 Torgersen Adelie       52
```

`tabyl()` will also produce counts, but the output is different. It is just a matter of personal preference.

```r
penguins %>% 
  tabyl(island, species)
```

```
##     island Adelie Chinstrap Gentoo
##     Biscoe     44         0    124
##      Dream     56        68      0
##  Torgersen     52         0      0
```
Tabyl is part of the janitor package. 

## Practice
1. Produce a summary of the mean for bill_length_mm, bill_depth_mm, flipper_length_mm, and body_mass_g within Adelie penguins only. Be sure to provide the number of samples.

```r
names(penguins)
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```



```r
penguins %>% 
  filter(species=="Adelie") %>% 
  summarise(bill_length_mm_mean=mean(bill_length_mm,na.rm=T),
            bill_depth_mm_mean=mean(bill_depth_mm,na.rm=T),
            flipper_length_mm=mean(flipper_length_mm,na.rm=T),
            body_mass_g_mean=mean(body_mass_g,na.rm=T),
            total=n())
```

```
## # A tibble: 1 × 5
##   bill_length_mm_mean bill_depth_mm_mean flipper_length_mm body_mass_g_mean
##                 <dbl>              <dbl>             <dbl>            <dbl>
## 1                38.8               18.3              190.            3701.
## # ℹ 1 more variable: total <int>
```

```r
#This is too long and is prone to mistakes. 
```

2. How does the mean of `bill_length_mm` compare between penguin species?

```r
penguins %>% 
  group_by(species) %>% 
  summarise(bill_length_mm_mean=mean(bill_length_mm,na.rm=T)) %>% 
  arrange(desc(bill_length_mm_mean))
```

```
## # A tibble: 3 × 2
##   species   bill_length_mm_mean
##   <fct>                   <dbl>
## 1 Chinstrap                48.8
## 2 Gentoo                   47.5
## 3 Adelie                   38.8
```
Here, we want to use `group_by` since we are comparing a value across ALL the species in the data. 

3. For some penguins, their sex is listed as NA. Where do these penguins occur?

```r
penguins %>% 
  group_by(species,island) %>% 
  count("NA")
```

```
## # A tibble: 5 × 4
## # Groups:   species, island [5]
##   species   island    `"NA"`     n
##   <fct>     <fct>     <chr>  <int>
## 1 Adelie    Biscoe    NA        44
## 2 Adelie    Dream     NA        56
## 3 Adelie    Torgersen NA        52
## 4 Chinstrap Dream     NA        68
## 5 Gentoo    Biscoe    NA       124
```

## `across()`
How do we use `filter()` and `select()` across multiple variables? There is a function in dplyr called `across()` which is designed to help make this efficient. 

What if we wanted to use `summarize()` to produce distinct counts over multiple variables; i.e. species, island, and sex? Although this isn't a lot of coding you can image that with a lot of variables it would be cumbersome.

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

By using `across()` we can reduce the clutter and make things cleaner. 

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
Summarizing across multiple variables at the same time. 
This is very helpful for continuous variables.

```r
penguins %>%
  summarize(across(contains("mm"), mean, na.rm=T)) ##summarize across ALL the variables that contain a "mm" in the name. A lot more efficient. 
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
  summarise_all(n_distinct)
```

```
## # A tibble: 1 × 8
##   species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##     <int>  <int>          <int>         <int>             <int>       <int>
## 1       3      3            165            81                56          95
## # ℹ 2 more variables: sex <int>, year <int>
```

Operators can also work, here I am summarizing `n_distinct()` across all variables except `species`, `island`, and `sex`.

```r
penguins %>%
  summarize(across(!c(species, island, sex, year), 
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
  summarise(across(starts_with("bill"), n_distinct))
```

```
## # A tibble: 1 × 2
##   bill_length_mm bill_depth_mm
##            <int>         <int>
## 1            165            81
```


```r
names(penguins)
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```

## Practice
1. Produce separate summaries of the mean and standard deviation for bill_length_mm, bill_depth_mm, and flipper_length_mm for each penguin species. Be sure to provide the number of samples.  

```r
#penguins %>% 
  #group_by(species) %>% 
  #summarise(across(contains("mm") mean,na.rm=T))
```
The mistake in this code is that you did not form the vector for the variables you want the `across` function to be applied to. AKA you did not include the `c` in front of the (contains("mm")) aspect. 


```r
penguins %>% 
  group_by(species) %>% 
  summarize(across(c(contains("mm"), body_mass_g), mean, na.rm=T ),
            n_samples=n())
```

```
## # A tibble: 3 × 6
##   species   bill_length_mm bill_depth_mm flipper_length_mm body_mass_g n_samples
##   <fct>              <dbl>         <dbl>             <dbl>       <dbl>     <int>
## 1 Adelie              38.8          18.3              190.       3701.       152
## 2 Chinstrap           48.8          18.4              196.       3733.        68
## 3 Gentoo              47.5          15.0              217.       5076.       124
```


```r
penguins %>% 
  group_by(species) %>% 
  summarize(across(c(contains("mm"), body_mass_g), sd, na.rm=T ),
            n_samples=n())
```

```
## # A tibble: 3 × 6
##   species   bill_length_mm bill_depth_mm flipper_length_mm body_mass_g n_samples
##   <fct>              <dbl>         <dbl>             <dbl>       <dbl>     <int>
## 1 Adelie              2.66         1.22               6.54        459.       152
## 2 Chinstrap           3.34         1.14               7.13        384.        68
## 3 Gentoo              3.08         0.981              6.48        504.       124
```

## That's it! Let's take a break and then move on to part 2! 

-->[Home](https://jmledford3115.github.io/datascibiol/)  
