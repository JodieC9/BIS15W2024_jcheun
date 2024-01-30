---
title: "`mutate()`, and `if_else()`"
date: "2024-01-30"
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
1. Use `mutate()` to add columns in a dataframe.  
2. Use `mutate()` and `if_else()` to replace values in a dataframe.  

## Load the libraries

```r
library("tidyverse")
library("janitor")
```

## Load the data
For this lab, we will use the following two datasets:  

1. 1. Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative.   [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)  

2. S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.   [link](http://esapubs.org/archive/ecol/E084/093/)  

## Pipes `%>%` 
Recall that we use pipes to connect the output of code to a subsequent function. This makes our code cleaner and more efficient. One way we can use pipes is to attach the `clean_names()` function from janitor to the `read_csv()` output.  

```r
fish <- readr::read_csv("data/Gaeta_etal_CLC_data.csv")
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
mammals <- read_csv("data/mammal_lifehistories_v2.csv") %>% clean_names()
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
Note the use of the `clean_names` command in conjuction with the intial `read`of the dataset you are loading 
## `mutate()`  


Mutate allows us to create a new column from existing columns in a data frame. We are doing a small introduction here and will add some additional functions later. Let's convert the length variable from cm to millimeters and create a new variable called length_mm.  

```r
fish %>% 
  mutate(length_mm = length*10) %>% 
  select(fish_id, length, length_mm)
```

```
## # A tibble: 4,033 × 3
##    fish_id length length_mm
##      <dbl>  <dbl>     <dbl>
##  1     299    167      1670
##  2     299    167      1670
##  3     299    167      1670
##  4     300    175      1750
##  5     300    175      1750
##  6     300    175      1750
##  7     300    175      1750
##  8     301    194      1940
##  9     301    194      1940
## 10     301    194      1940
## # ℹ 4,023 more rows
```
Loading fish, one of the variable names in fish is emasured in cm, multiplying and creating a new column with the manipulated data created a new length measurment in mm 

## Practice
1. Use `mutate()` to make a new column that is the half length of each fish: length_half = length/2. Select only fish_id, length, and length_half.


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
fish %>% 
  mutate(length_half = length/2)
```

```
## # A tibble: 4,033 × 7
##    lakeid fish_id annnumber length radii_length_mm scalelength length_half
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70        83.5
##  2 AL         299 2            167            2.04        2.70        83.5
##  3 AL         299 1            167            1.31        2.70        83.5
##  4 AL         300 EDGE         175            3.02        3.02        87.5
##  5 AL         300 3            175            2.67        3.02        87.5
##  6 AL         300 2            175            2.14        3.02        87.5
##  7 AL         300 1            175            1.23        3.02        87.5
##  8 AL         301 EDGE         194            3.34        3.34        97  
##  9 AL         301 3            194            2.97        3.34        97  
## 10 AL         301 2            194            2.29        3.34        97  
## # ℹ 4,023 more rows
```

## `mutate_all()`
This last function is super helpful when cleaning data. With "wild" data, there are often mixed entries (upper and lowercase), blank spaces, odd characters, etc. These all need to be dealt with before analysis.  



Here is an example that changes all entries to lowercase (if present).  

```r
mammals
```

```
## # A tibble: 1,440 × 13
##    order  family genus species   mass gestation newborn weaning wean_mass    afr
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
## # ℹ 3 more variables: max_life <dbl>, litter_size <dbl>, litters_year <dbl>
```


```r
mammals %>%
  mutate_all(tolower)
```

```
## # A tibble: 1,440 × 13
##    order    family genus species mass  gestation newborn weaning wean_mass afr  
##    <chr>    <chr>  <chr> <chr>   <chr> <chr>     <chr>   <chr>   <chr>     <chr>
##  1 artioda… antil… anti… americ… 45375 8.13      3246.36 3       8900      13.53
##  2 artioda… bovid… addax nasoma… 1823… 9.39      5480    6.5     -999      27.27
##  3 artioda… bovid… aepy… melamp… 41480 6.35      5093    5.63    15900     16.66
##  4 artioda… bovid… alce… busela… 1500… 7.9       10166.… 6.5     -999      23.02
##  5 artioda… bovid… ammo… clarkei 28500 6.8       -999    -999    -999      -999 
##  6 artioda… bovid… ammo… lervia  55500 5.08      3810    4       -999      14.89
##  7 artioda… bovid… anti… marsup… 30000 5.72      3910    4.04    -999      10.23
##  8 artioda… bovid… anti… cervic… 37500 5.5       3846    2.13    -999      20.13
##  9 artioda… bovid… bison bison   4976… 8.93      20000   10.71   157500    29.45
## 10 artioda… bovid… bison bonasus 5e+05 9.14      23000.… 6.6     -999      29.99
## # ℹ 1,430 more rows
## # ℹ 3 more variables: max_life <chr>, litter_size <chr>, litters_year <chr>
```

Taking all the observations and making them in lowercase form, avoids formatting issues 

Using the across function we can specify individual columns.

```r
mammals %>% 
  mutate(across(c("order", "family"), tolower))
```

```
## # A tibble: 1,440 × 13
##    order  family genus species   mass gestation newborn weaning wean_mass    afr
##    <chr>  <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>  <dbl>
##  1 artio… antil… Anti… americ… 4.54e4      8.13   3246.    3         8900   13.5
##  2 artio… bovid… Addax nasoma… 1.82e5      9.39   5480     6.5       -999   27.3
##  3 artio… bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900   16.7
##  4 artio… bovid… Alce… busela… 1.5 e5      7.9   10167.    6.5       -999   23.0
##  5 artio… bovid… Ammo… clarkei 2.85e4      6.8    -999  -999         -999 -999  
##  6 artio… bovid… Ammo… lervia  5.55e4      5.08   3810     4         -999   14.9
##  7 artio… bovid… Anti… marsup… 3   e4      5.72   3910     4.04      -999   10.2
##  8 artio… bovid… Anti… cervic… 3.75e4      5.5    3846     2.13      -999   20.1
##  9 artio… bovid… Bison bison   4.98e5      8.93  20000    10.7     157500   29.4
## 10 artio… bovid… Bison bonasus 5   e5      9.14  23000.    6.6       -999   30.0
## # ℹ 1,430 more rows
## # ℹ 3 more variables: max_life <dbl>, litter_size <dbl>, litters_year <dbl>
```
You can specify with columns specifically will have their observations changed without having to create a new object. The output of this command is not stored anywhere. `clean_name` from the janitor only cleans up the column names, it will not mess with the observations. 

## `if_else()`
We will briefly introduce `if_else()` here because it allows us to use `mutate()` but not have the entire column affected in the same way. In a sense, this can function like find and replace in a spreadsheet program. With `ifelse()`, you first specify a logical statement, afterwards what needs to happen if the statement returns `TRUE`, and lastly what needs to happen if it's  `FALSE`.  

Sort of like a `find and replace` in excel. Finds observations of interest and replaces them with data you want. 

Have a look at the data from mammals below. Notice that the values for newborn include `-999.00`. This is sometimes used as a placeholder for NA (but, is a really bad idea). We can use `if_else()` to replace `-999.00` with `NA`.  

```r
mammals %>% 
  select(genus, species, newborn) %>% 
  arrange(newborn)
```

```
## # A tibble: 1,440 × 3
##    genus       species        newborn
##    <chr>       <chr>            <dbl>
##  1 Ammodorcas  clarkei           -999
##  2 Bos         javanicus         -999
##  3 Bubalus     depressicornis    -999
##  4 Bubalus     mindorensis       -999
##  5 Capra       falconeri         -999
##  6 Cephalophus niger             -999
##  7 Cephalophus nigrifrons        -999
##  8 Cephalophus natalensis        -999
##  9 Cephalophus leucogaster       -999
## 10 Cephalophus ogilbyi           -999
## # ℹ 1,430 more rows
```


```r
names(mammals)
```

```
##  [1] "order"        "family"       "genus"        "species"      "mass"        
##  [6] "gestation"    "newborn"      "weaning"      "wean_mass"    "afr"         
## [11] "max_life"     "litter_size"  "litters_year"
```


```r
mammals %>% 
  select(genus, species, newborn) %>%
  mutate(newborn_new = ifelse(newborn == -999.00, NA, newborn))%>% 
  arrange(newborn)
```

```
## # A tibble: 1,440 × 4
##    genus       species        newborn newborn_new
##    <chr>       <chr>            <dbl>       <dbl>
##  1 Ammodorcas  clarkei           -999          NA
##  2 Bos         javanicus         -999          NA
##  3 Bubalus     depressicornis    -999          NA
##  4 Bubalus     mindorensis       -999          NA
##  5 Capra       falconeri         -999          NA
##  6 Cephalophus niger             -999          NA
##  7 Cephalophus nigrifrons        -999          NA
##  8 Cephalophus natalensis        -999          NA
##  9 Cephalophus leucogaster       -999          NA
## 10 Cephalophus ogilbyi           -999          NA
## # ℹ 1,430 more rows
```
Here, we have authors using -999 to stand for NA. But having numeric values messes with our calculations. We want to replace these -999 numeric values with NA. 

creates a new variable of your choice, if new_born is equal to -999.00 put NA its in place. If it is anything but -999 leave that observation alone and keep the original `new_born` value.Always want to create a new varibale (addition of column to the dataset) because this lets you track yoru changes, and show which observations were orginally -999 but YOU are not considering NA

the last `newborn` is us saying anything that does not fit this critiria, dont touch it 


## Practice
1. We are interested in the family, genus, species and max life variables. Because the max life span for several mammals is unknown, the authors have use -999 in place of NA. Replace all of these values with NA in a new column titled `max_life_new`. Finally, sort the date in descending order by max_life_new. Which mammal has the oldest known life span?

```r
summary(mammals)
```

```
##     order              family             genus             species         
##  Length:1440        Length:1440        Length:1440        Length:1440       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##       mass             gestation          newborn             weaning       
##  Min.   :     -999   Min.   :-999.00   Min.   :   -999.0   Min.   :-999.00  
##  1st Qu.:       50   1st Qu.:-999.00   1st Qu.:   -999.0   1st Qu.:-999.00  
##  Median :      403   Median :   1.05   Median :      2.6   Median :   0.73  
##  Mean   :   383577   Mean   :-287.25   Mean   :   6703.1   Mean   :-427.17  
##  3rd Qu.:     7009   3rd Qu.:   4.50   3rd Qu.:     98.0   3rd Qu.:   2.00  
##  Max.   :149000000   Max.   :  21.46   Max.   :2250000.0   Max.   :  48.00  
##    wean_mass             afr             max_life       litter_size      
##  Min.   :    -999   Min.   :-999.00   Min.   :-999.0   Min.   :-999.000  
##  1st Qu.:    -999   1st Qu.:-999.00   1st Qu.:-999.0   1st Qu.:   1.000  
##  Median :    -999   Median :   2.50   Median :-999.0   Median :   2.270  
##  Mean   :   16049   Mean   :-408.12   Mean   :-490.3   Mean   : -55.634  
##  3rd Qu.:      10   3rd Qu.:  15.61   3rd Qu.: 147.2   3rd Qu.:   3.835  
##  Max.   :19075000   Max.   : 210.00   Max.   :1368.0   Max.   :  14.180  
##   litters_year     
##  Min.   :-999.000  
##  1st Qu.:-999.000  
##  Median :   0.375  
##  Mean   :-477.141  
##  3rd Qu.:   1.155  
##  Max.   :   7.500
```


```r
mammals %>% 
  select(family, genus, species, max_life) %>% 
  mutate(max_life_new = ifelse(max_life==-999, NA, max_life)) %>% 
  arrange(desc(max_life_new))
```

```
## # A tibble: 1,440 × 5
##    family          genus        species      max_life max_life_new
##    <chr>           <chr>        <chr>           <dbl>        <dbl>
##  1 Balaenopteridae Balaenoptera physalus         1368         1368
##  2 Balaenopteridae Balaenoptera musculus         1320         1320
##  3 Balaenidae      Balaena      mysticetus       1200         1200
##  4 Delphinidae     Orcinus      orca             1080         1080
##  5 Ziphiidae       Berardius    bairdii          1008         1008
##  6 Elephantidae    Elephas      maximus           960          960
##  7 Balaenopteridae Megaptera    novaeangliae      924          924
##  8 Physeteridae    Physeter     catodon           924          924
##  9 Balaenopteridae Balaenoptera borealis          888          888
## 10 Dugongidae      Dugong       dugon             876          876
## # ℹ 1,430 more rows
```

## That's it! Let's take a break and then move on to part 2! 

-->[Home](https://jmledford3115.github.io/datascibiol/)  
