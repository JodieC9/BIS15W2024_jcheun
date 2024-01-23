---
title: "Transforming data 2: `filter()`"
date: "2024-01-23"
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
1. Use the functions of dplyr (filter, select, arrange) to organize and sort data frames.  
2. Use `mutate()` to calculate a new column from existing columns.  

## Review  
In the previous lab, we used `select()` to extract columns of interest from a data frame. This helps us focus our attention on the variables relevant to our question. However, it doesn't allow us to extract observations from within the data frame. The `filter()` function allows us to extract data that meet specific criteria. When combined with `select()`, we have the power to transform, shape, and explore data with the potential to make new discoveries.  

## Load the tidyverse

```r
library("tidyverse")
```

## Load the data
For these exercises we will use the data from lab 4_1.  

```r
fish <- read_csv("data/Gaeta_etal_CLC_data.csv")
```

```
## Rows: 4033 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): lakeid, annnumber
## dbl (4): fish_id, length, radii_length_mm, scalelength
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
mammals <- read_csv("data/mammal_lifehistories_v2.csv")
```

```
## Rows: 1440 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): order, family, Genus, species
## dbl (9): mass, gestation, newborn, weaning, wean mass, AFR, max. life, litte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

To keep things tidy, I am going to rename the variables in the mammals data.  

```r
mammals <- rename(mammals, genus=Genus, wean_mass="wean mass", max_life="max. life", litter_size="litter size", litters_per_year="litters/year")
```

```r
mammals
```

```
## # A tibble: 1,440 × 13
##    order  family genus species   mass gestation newborn weaning wean_mass    AFR
##    <chr>  <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>  <dbl>
##  1 Artio… Antil… Anti… americ… 4.54e4      8.13   3246.    3         8900   13.5
##  2 Artio… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5       -999   27.3
##  3 Artio… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900   16.7
##  4 Artio… Bovid… Alce… busela… 1.5 e5      7.9   10167.    6.5       -999   23.0
##  5 Artio… Bovid… Ammo… clarkei 2.85e4      6.8    -999  -999         -999 -999  
##  6 Artio… Bovid… Ammo… lervia  5.55e4      5.08   3810     4         -999   14.9
##  7 Artio… Bovid… Anti… marsup… 3   e4      5.72   3910     4.04      -999   10.2
##  8 Artio… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13      -999   20.1
##  9 Artio… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500   29.4
## 10 Artio… Bovid… Bison bonasus 5   e5      9.14  23000.    6.6       -999   30.0
## # ℹ 1,430 more rows
## # ℹ 3 more variables: max_life <dbl>, litter_size <dbl>, litters_per_year <dbl>
```

## `filter()`
Unlike `select()`, `filter()` allows us to extract data that meet specific criteria within a variable. Let's say that we are interested only in the fish that occur in lake "AL". We can use `filter()` to extract these observations.  

```r
filter(fish, lakeid == "AL")
```

```
## # A tibble: 383 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # ℹ 373 more rows
```
Filter command requires use of "" 


Similarly, if we are only interested in fish with a length greater than or equal to 350 we can use `filter()` to extract these observations.  

```r
filter(fish, length >= 350)
```

```
## # A tibble: 890 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         306 EDGE         350            6.94        6.94
##  2 AL         306 10           350            6.46        6.94
##  3 AL         306 9            350            6.16        6.94
##  4 AL         306 8            350            5.88        6.94
##  5 AL         306 7            350            5.42        6.94
##  6 AL         306 6            350            4.90        6.94
##  7 AL         306 5            350            4.46        6.94
##  8 AL         306 4            350            3.75        6.94
##  9 AL         306 3            350            2.93        6.94
## 10 AL         306 2            350            2.14        6.94
## # ℹ 880 more rows
```

+ `filter()` allows all of the expected operators; i.e. >, >=, <, <=, != (not equal), and == (equal).  

Using the `!` operator allows for the exclusion of specific observations.(Pull out all the fish that are NOT "AL)

```r
not_AL <- filter(fish, lakeid != "AL")
not_AL
```

```
## # A tibble: 3,650 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AR         269 EDGE         140            2.01        2.01
##  2 AR         269 1            140            1.48        2.01
##  3 AR         270 EDGE         193            2.66        2.66
##  4 AR         270 3            193            2.39        2.66
##  5 AR         270 2            193            2.03        2.66
##  6 AR         270 1            193            1.42        2.66
##  7 AR         271 EDGE         220            3.50        3.50
##  8 AR         271 5            220            3.13        3.50
##  9 AR         271 4            220            2.86        3.50
## 10 AR         271 3            220            2.63        3.50
## # ℹ 3,640 more rows
```

## Using `filter()` with multiple observations  
Filtering multiple values within the same variable requires the `%in%` [operator](https://www.tutorialspoint.com/r/r_operators.htm).    

Filter fish within the variable length , all the fish with a length of 167 or 175. Searching for multiple values within the same variable

```r
filter(fish, length %in% c(167, 175))
```

```
## # A tibble: 18 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167           2.70         2.70
##  2 AL         299 2            167           2.04         2.70
##  3 AL         299 1            167           1.31         2.70
##  4 AL         300 EDGE         175           3.02         3.02
##  5 AL         300 3            175           2.67         3.02
##  6 AL         300 2            175           2.14         3.02
##  7 AL         300 1            175           1.23         3.02
##  8 BO         397 EDGE         175           2.67         2.67
##  9 BO         397 3            175           2.39         2.67
## 10 BO         397 2            175           1.59         2.67
## 11 BO         397 1            175           0.830        2.67
## 12 LSG         45 EDGE         175           3.21         3.21
## 13 LSG         45 3            175           2.92         3.21
## 14 LSG         45 2            175           2.44         3.21
## 15 LSG         45 1            175           1.60         3.21
## 16 RD         103 EDGE         167           2.80         2.80
## 17 RD         103 2            167           2.10         2.80
## 18 RD         103 1            167           1.31         2.80
```

Alternatively, you can use `between` if you are looking for a range of specific values.  Look at the fish dataset and look at scalelength values between 2.5 and 2.55 --> chopping up the data horizontally 

```r
filter(fish, between(scalelength, 2.5, 2.55))
```

```
## # A tibble: 10 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 LSG         56 EDGE         145            2.55        2.55
##  2 LSG         56 2            145            1.94        2.55
##  3 LSG         56 1            145            1.20        2.55
##  4 LSG         57 EDGE         143            2.52        2.52
##  5 LSG         57 2            143            2.13        2.52
##  6 LSG         57 1            143            1.19        2.52
##  7 UB          80 EDGE         153            2.55        2.55
##  8 UB          80 3            153            2.10        2.55
##  9 UB          80 2            153            1.62        2.55
## 10 UB          80 1            153            1.14        2.55
```

You can also extract observations "near" a certain value but you need to specify a tolerance.  Look for radii length near value of 2 but within 0.2 of the value of 2 

```r
filter(fish, near(radii_length_mm, 2, tol = 0.2))
```

```
## # A tibble: 291 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 2            167            2.04        2.70
##  2 AL         300 2            175            2.14        3.02
##  3 AL         302 2            324            2.19        6.07
##  4 AL         303 2            325            2.04        6.79
##  5 AL         306 2            350            2.14        6.94
##  6 AL         308 2            355            1.86        6.67
##  7 AL         312 2            367            2.17        6.81
##  8 AL         313 2            367            2.06        6.47
##  9 AL         315 2            372            2.04        6.47
## 10 AL         316 2            372            1.82        6.35
## # ℹ 281 more rows
```

## Practice
1. Filter the `fish` data to include the samples from lake "BO".


2. Filter the data to include all lakes except "AR".


3. Filter the fish data to include all fish with a scalelength within 0.25 of 8.


4. Filter the fish data to include fish with a scalelength between 2 and 4.




## Using `filter()` on multiple conditions
You can also use `filter()` to extract data based on multiple conditions. Below we extract only the fish that have lakeid "AL" and length >350.

```r
filter(fish, lakeid == "AL" & length > 350)
```

```
## # A tibble: 314 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         307 EDGE         353            7.55        7.55
##  2 AL         307 13           353            7.28        7.55
##  3 AL         307 12           353            6.98        7.55
##  4 AL         307 11           353            6.73        7.55
##  5 AL         307 10           353            6.48        7.55
##  6 AL         307 9            353            6.22        7.55
##  7 AL         307 8            353            5.92        7.55
##  8 AL         307 7            353            5.44        7.55
##  9 AL         307 6            353            5.06        7.55
## 10 AL         307 5            353            4.37        7.55
## # ℹ 304 more rows
```

Notice that the `|` operator generates a different result.

```r
filter(fish, lakeid == "AL" | length > 350)
```

```
## # A tibble: 948 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # ℹ 938 more rows
```

Rules:  
+ `filter(condition1, condition2)` will return rows where both conditions are met.  
+ `filter(condition1, !condition2)` will return all rows where condition one is true but condition 2 is not.  
+ `filter(condition1 | condition2)` will return rows where condition 1 or condition 2 is met.  
+ `filter(xor(condition1, condition2)` will return all rows where only one of the conditions is met, and not when both conditions are met.  

In this case, we filter out the fish with a length over 400 and a scale length over 11 or a radii length over 8.

```r
filter(fish, length > 400, (scalelength > 11 | radii_length_mm > 8))
```

```
## # A tibble: 23 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         324 EDGE         406            8.21        8.21
##  2 AL         327 EDGE         413            8.33        8.33
##  3 AL         327 15           413            8.11        8.33
##  4 AL         328 EDGE         420            8.71        8.71
##  5 AL         328 16           420            8.41        8.71
##  6 AL         328 15           420            8.14        8.71
##  7 WS         180 EDGE         403           11.0        11.0 
##  8 WS         180 16           403           10.6        11.0 
##  9 WS         180 15           403           10.3        11.0 
## 10 WS         180 14           403            9.93       11.0 
## # ℹ 13 more rows
```

## Practice  
1. Have a look at the mammals data using the summary `functions` of your choosing.    


2. What are the names of the variables in the mammals data?  


3.  From the `mammals` data, filter all members of the family Bovidae with a mass greater than 450000.


4. Let's say we are only interested in family, genus, species, mass, and gestation. Relimit the mammals data frame to these variables.  


5. Use the output from #4 to focus on the family Bovidae with a mass greater than 450000.






6. From the `mammals` data, build a data frame that compares `mass`, `gestation`, and `newborn` among the primate genera `Lophocebus`, `Erythrocebus`, and `Macaca`. Among these genera, which species has the smallest `newborn` mass?




## Wrap-up  

Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
