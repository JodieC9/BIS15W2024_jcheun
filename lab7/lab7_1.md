---
title: "`summarize()` and `group_by()`"
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
1. Use a combination of `select()`, `filter()`, and `mutate()` to transform data frames.  
2. Use the `skimr` package to produce summaries of data.  
3. Produce clean summaries of data using `summarize()`.  
4. Use `group_by()` in combination with `summarize()` to produce grouped summaries of data.  

## Load the tidyverse and janitor

```r
library("tidyverse")
library("janitor")
```

## Install `skimr`

```r
#install.packages("skimr")
library("skimr")
```

## Load the data
For this lab, we will use the built-in data on mammal sleep patterns. From: _V. M. Savage and G. B. West. A quantitative, theoretical framework for understanding mammalian sleep. Proceedings of the National Academy of Sciences, 104 (3):1051-1056, 2007_.

```r
?msleep #putting a`?` in front of embedded datasets tells you more information about it 
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

```r
msleep <- msleep
```

We will also use [palmerpenguins](https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data in the second part of lab.

```r
library("palmerpenguins")
```

## dplyr Practice
Let's do a bit more practice to make sure that we understand `select()`, `filter()`, and `mutate()`. Start by building a new data frame `msleep24` from the `msleep` data that: contains the `name` and `vore` variables along with a new column called `sleep_total_24` which is the amount of time a species sleeps expressed as a proportion of a 24-hour day. Restrict the `sleep_total_24` values to less than or equal to 0.3. Arrange the output in descending order.  

```r
summary(msleep)
```

```
##      name              genus               vore              order          
##  Length:83          Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  conservation        sleep_total      sleep_rem      sleep_cycle    
##  Length:83          Min.   : 1.90   Min.   :0.100   Min.   :0.1167  
##  Class :character   1st Qu.: 7.85   1st Qu.:0.900   1st Qu.:0.1833  
##  Mode  :character   Median :10.10   Median :1.500   Median :0.3333  
##                     Mean   :10.43   Mean   :1.875   Mean   :0.4396  
##                     3rd Qu.:13.75   3rd Qu.:2.400   3rd Qu.:0.5792  
##                     Max.   :19.90   Max.   :6.600   Max.   :1.5000  
##                                     NA's   :22      NA's   :51      
##      awake          brainwt            bodywt        
##  Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##                  NA's   :27
```


```r
msleep24<- msleep %>% 
  mutate(sleep_total_24= sleep_total/24) %>% 
  select(name,vore, sleep_total, sleep_total_24) %>%
  filter(sleep_total_24<=0.3) %>% 
  arrange(desc(sleep_total_24))

msleep24
```

```
## # A tibble: 20 × 4
##    name                 vore  sleep_total sleep_total_24
##    <chr>                <chr>       <dbl>          <dbl>
##  1 Vesper mouse         <NA>          7           0.292 
##  2 Gray hyrax           herbi         6.3         0.262 
##  3 Genet                carni         6.3         0.262 
##  4 Gray seal            carni         6.2         0.258 
##  5 Common porpoise      carni         5.6         0.233 
##  6 Rock hyrax           <NA>          5.4         0.225 
##  7 Goat                 herbi         5.3         0.221 
##  8 Tree hyrax           herbi         5.3         0.221 
##  9 Bottle-nosed dolphin carni         5.2         0.217 
## 10 Brazilian tapir      herbi         4.4         0.183 
## 11 Cow                  herbi         4           0.167 
## 12 Asian elephant       herbi         3.9         0.162 
## 13 Sheep                herbi         3.8         0.158 
## 14 Caspian seal         carni         3.5         0.146 
## 15 African elephant     herbi         3.3         0.137 
## 16 Donkey               herbi         3.1         0.129 
## 17 Roe deer             herbi         3           0.125 
## 18 Horse                herbi         2.9         0.121 
## 19 Pilot whale          carni         2.7         0.112 
## 20 Giraffe              herbi         1.9         0.0792
```


Did `dplyr` do what we expected? How do we check our output? Remember, just because your code runs it doesn't mean that it did what you intended.

```r
summary(msleep24)
```

```
##      name               vore            sleep_total    sleep_total_24   
##  Length:20          Length:20          Min.   :1.900   Min.   :0.07917  
##  Class :character   Class :character   1st Qu.:3.250   1st Qu.:0.13542  
##  Mode  :character   Mode  :character   Median :4.200   Median :0.17500  
##                                        Mean   :4.455   Mean   :0.18563  
##                                        3rd Qu.:5.450   3rd Qu.:0.22708  
##                                        Max.   :7.000   Max.   :0.29167
```
First check that the minimum and maximium values align with your requirment 
Try out the new function `skim()` as part of the `skimr` package.

```r
skim(msleep24)
```


Table: Data summary

|                         |         |
|:------------------------|:--------|
|Name                     |msleep24 |
|Number of rows           |20       |
|Number of columns        |4        |
|_______________________  |         |
|Column type frequency:   |         |
|character                |2        |
|numeric                  |2        |
|________________________ |         |
|Group variables          |None     |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|name          |         0|           1.0|   3|  20|     0|       20|          0|
|vore          |         2|           0.9|   5|   5|     0|        2|          0|


**Variable type: numeric**

|skim_variable  | n_missing| complete_rate| mean|   sd|   p0|  p25|  p50|  p75| p100|hist  |
|:--------------|---------:|-------------:|----:|----:|----:|----:|----:|----:|----:|:-----|
|sleep_total    |         0|             1| 4.46| 1.45| 1.90| 3.25| 4.20| 5.45| 7.00|▃▇▂▇▅ |
|sleep_total_24 |         0|             1| 0.19| 0.06| 0.08| 0.14| 0.17| 0.23| 0.29|▃▇▂▇▅ |
Skim is another way to get an overview of the data, similar to summary and glimpse 
Histograms are also a quick way to check the output.

```r
hist(msleep24$sleep_total_24)
```

![](lab7_1_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
Highest value being 0.30 is good since it is was we specified in the code. In this `hist` command you have to specify the specific variable you want to make a histogram of. 


## `summarize()`
`summarize()` will produce summary statistics for a given variable in a data frame. For example, if you are asked to calculate the mean of `sleep_total` for large and small mammals you could do this using a combination of commands, but it isn't very efficient or clean. We can do better!  

```r
head(msleep)
```

```
## # A tibble: 6 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cheetah Acin… carni Carn… lc                  12.1      NA        NA      11.9
## 2 Owl mo… Aotus omni  Prim… <NA>                17         1.8      NA       7  
## 3 Mounta… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
## 4 Greate… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
## 5 Cow     Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## 6 Three-… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

For example, if we define "large" as having a `bodywt` greater than 200 then we get the following:

```r
large <- msleep %>% 
  select(name, genus, bodywt, sleep_total) %>% 
  filter(bodywt > 200) %>% 
  arrange(desc(bodywt))
large
```

```
## # A tibble: 7 × 4
##   name             genus         bodywt sleep_total
##   <chr>            <chr>          <dbl>       <dbl>
## 1 African elephant Loxodonta      6654          3.3
## 2 Asian elephant   Elephas        2547          3.9
## 3 Giraffe          Giraffa         900.         1.9
## 4 Pilot whale      Globicephalus   800          2.7
## 5 Cow              Bos             600          4  
## 6 Horse            Equus           521          2.9
## 7 Brazilian tapir  Tapirus         208.         4.4
```

Rm is a way to remove something from the enviornment panel in case you overrode the code somewhere else

```r
mean(large$sleep_total)
```

```
## [1] 3.3
```

We can accomplish the same task using the `summarize()` function to make things cleaner.

```r
msleep %>% 
  filter(bodywt>200) %>% 
  summarize(mean_sleep_lg=mean(sleep_total))
```

```
## # A tibble: 1 × 1
##   mean_sleep_lg
##           <dbl>
## 1           3.3
```

You can also combine functions to make useful summaries for multiple variables.

```r
msleep %>% 
  filter(bodywt>200) %>% 
  summarize(mean_sleep_lg=mean(sleep_total),
            min_sleep_lg=min(sleep_total),
            max_sleep_lg=max(sleep_total),
            sd_sleep_lg=sd(sleep_total), 
            total=n()) ##This tells you the total number of observations, aka the number of rows 
```

```
## # A tibble: 1 × 5
##   mean_sleep_lg min_sleep_lg max_sleep_lg sd_sleep_lg total
##           <dbl>        <dbl>        <dbl>       <dbl> <int>
## 1           3.3          1.9          4.4       0.870     7
```

## Practice
1. What is the mean, min, and max `bodywt` for the taxonomic order Primates? Provide the total number of observations.


```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```


```r
msleep %>% 
  filter(order=="Primates") %>% 
  summarize(mean_bodywt=mean(bodywt),
            min_bodywt=min(bodywt),
            max_bodywt=max(bodywt),
            total=n()) ##This tells you the total number of observations, aka the number of rows 
```

```
## # A tibble: 1 × 4
##   mean_bodywt min_bodywt max_bodywt total
##         <dbl>      <dbl>      <dbl> <int>
## 1        13.9        0.2         62    12
```

`n_distinct()` is a very handy way of cleanly presenting the number of distinct observations. Here we show the number of distinct genera over 100 in body weight.  

Notice that there are multiple genera with over 100 in body weight. 

```r
msleep %>% 
  filter(bodywt > 100)
```

```
## # A tibble: 11 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  2 Asian… Elep… herbi Prob… en                   3.9      NA        NA      20.1
##  3 Horse  Equus herbi Peri… domesticated         2.9       0.6       1      21.1
##  4 Donkey Equus herbi Peri… domesticated         3.1       0.4      NA      20.9
##  5 Giraf… Gira… herbi Arti… cd                   1.9       0.4      NA      22.1
##  6 Pilot… Glob… carni Ceta… cd                   2.7       0.1      NA      21.4
##  7 Afric… Loxo… herbi Prob… vu                   3.3      NA        NA      20.7
##  8 Tiger  Pant… carni Carn… en                  15.8      NA        NA       8.2
##  9 Lion   Pant… carni Carn… vu                  13.5      NA        NA      10.5
## 10 Brazi… Tapi… herbi Peri… vu                   4.4       1         0.9    19.6
## 11 Bottl… Turs… carni Ceta… <NA>                 5.2      NA        NA      18.8
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

n_distinct() is a very handy way of cleanly presenting the number of distinct observations. Here we show the number of distinct genera over 100 in body weight.THIS IS ON THE EXAM 

Way to pull out unique values in a category of data. Difficult to get a count of the column since there are duplicates. 

```r
msleep %>% 
  summarize(n_genera=n_distinct(genus)) ##this is going to count the number of genera in msleep 
```

```
## # A tibble: 1 × 1
##   n_genera
##      <int>
## 1       77
```

```r
glimpse(msleep)
```

```
## Rows: 83
## Columns: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Greater shor…
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bos", "Bra…
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi", "carn…
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorpha", "Art…
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", NA, "dome…
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1, 3.0, 5…
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0.6, 0.8, …
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.3833333, N…
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9, 21.0, 1…
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0.07000, 0…
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.490, 0.04…
```
Every row should be its own unique data Because distinct gave back less than the total number of row wehave confirmation that there were indeed repeats. 


There are many other useful summary statistics, depending on your needs: sd(), min(), max(), median(), sum(), n() (returns the length of a column), first() (returns first value in a column), last() (returns last value in a column) and n_distinct() (number of distinct values in a column).

## Practice
1. How many genera are represented in the msleep data frame?

```r
msleep %>% 
  tabyl(genus)
```

```
##          genus n    percent
##       Acinonyx 1 0.01204819
##          Aotus 1 0.01204819
##     Aplodontia 1 0.01204819
##        Blarina 1 0.01204819
##            Bos 1 0.01204819
##       Bradypus 1 0.01204819
##    Callorhinus 1 0.01204819
##        Calomys 1 0.01204819
##          Canis 1 0.01204819
##      Capreolus 1 0.01204819
##          Capri 1 0.01204819
##          Cavis 1 0.01204819
##  Cercopithecus 1 0.01204819
##     Chinchilla 1 0.01204819
##      Condylura 1 0.01204819
##     Cricetomys 1 0.01204819
##      Cryptotis 1 0.01204819
##        Dasypus 1 0.01204819
##    Dendrohyrax 1 0.01204819
##      Didelphis 1 0.01204819
##        Elephas 1 0.01204819
##      Eptesicus 1 0.01204819
##          Equus 2 0.02409639
##      Erinaceus 1 0.01204819
##   Erythrocebus 1 0.01204819
##       Eutamias 1 0.01204819
##          Felis 1 0.01204819
##         Galago 1 0.01204819
##        Genetta 1 0.01204819
##        Giraffa 1 0.01204819
##  Globicephalus 1 0.01204819
##   Haliochoerus 1 0.01204819
##    Heterohyrax 1 0.01204819
##           Homo 1 0.01204819
##          Lemur 1 0.01204819
##      Loxodonta 1 0.01204819
##     Lutreolina 1 0.01204819
##         Macaca 1 0.01204819
##       Meriones 1 0.01204819
##   Mesocricetus 1 0.01204819
##       Microtus 1 0.01204819
##            Mus 1 0.01204819
##         Myotis 1 0.01204819
##       Neofiber 1 0.01204819
##      Nyctibeus 1 0.01204819
##        Octodon 1 0.01204819
##      Onychomys 1 0.01204819
##    Oryctolagus 1 0.01204819
##           Ovis 1 0.01204819
##            Pan 1 0.01204819
##       Panthera 3 0.03614458
##          Papio 1 0.01204819
##    Paraechinus 1 0.01204819
##   Perodicticus 1 0.01204819
##     Peromyscus 1 0.01204819
##      Phalanger 1 0.01204819
##          Phoca 1 0.01204819
##       Phocoena 1 0.01204819
##       Potorous 1 0.01204819
##     Priodontes 1 0.01204819
##       Procavia 1 0.01204819
##         Rattus 1 0.01204819
##      Rhabdomys 1 0.01204819
##        Saimiri 1 0.01204819
##       Scalopus 1 0.01204819
##       Sigmodon 1 0.01204819
##         Spalax 1 0.01204819
##   Spermophilus 3 0.03614458
##         Suncus 1 0.01204819
##            Sus 1 0.01204819
##   Tachyglossus 1 0.01204819
##         Tamias 1 0.01204819
##        Tapirus 1 0.01204819
##         Tenrec 1 0.01204819
##         Tupaia 1 0.01204819
##       Tursiops 1 0.01204819
##         Vulpes 2 0.02409639
```
Tabyl is another way to get the number of observations, and check if there are any duplicates 


2. What are the min, max, and mean `sleep_total` for all of the mammals? Be sure to include the total n.

```r
msleep %>% 
  summarize(mean_sleep_all=mean(sleep_total),
            min_sleep_all=min(sleep_total),
            max_sleep_all=max(sleep_total),
            total=n())
```

```
## # A tibble: 1 × 4
##   mean_sleep_all min_sleep_all max_sleep_all total
##            <dbl>         <dbl>         <dbl> <int>
## 1           10.4           1.9          19.9    83
```

## `group_by()`
The `summarize()` function is most useful when used in conjunction with `group_by()`. Although producing a summary of body weight for all of the mammals in the data set is helpful, what if we were interested in body weight by feeding ecology?

Group together categorical variables before doing the summary command 

```r
msleep %>%
  group_by(vore) %>% #we are grouping by feeding ecology, a categorical variable. Ie what do these specific values mean for omni, herb, and carn (by specifing the vore variable/column it gives you the min,mean,max for each vore type)
  summarize(min_bodywt = min(bodywt),
            max_bodywt = max(bodywt),
            mean_bodywt = mean(bodywt),
            total=n())
```

```
## # A tibble: 5 × 5
##   vore    min_bodywt max_bodywt mean_bodywt total
##   <chr>        <dbl>      <dbl>       <dbl> <int>
## 1 carni        0.028      800        90.8      19
## 2 herbi        0.022     6654       367.       32
## 3 insecti      0.01        60        12.9       5
## 4 omni         0.005       86.2      12.7      20
## 5 <NA>         0.021        3.6       0.858     7
```

```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

## Practice
1. Calculate mean brain weight by taxonomic order in the msleep data.

```r
msleep %>% 
  group_by(order) %>%
  summarize(min_brainwt = min(brainwt),
            max_brainwt = max(brainwt),
            mean_brainwt = mean(brainwt),
            total=n()) %>% 
  arrange(desc(mean_brainwt))
```

```
## # A tibble: 19 × 5
##    order           min_brainwt max_brainwt mean_brainwt total
##    <chr>                 <dbl>       <dbl>        <dbl> <int>
##  1 Proboscidea         4.60         5.71       5.16         2
##  2 Perissodactyla      0.169        0.655      0.414        3
##  3 Cingulata           0.0108       0.081      0.0459       2
##  4 Monotremata         0.025        0.025      0.025        1
##  5 Hyracoidea          0.0123       0.021      0.0152       3
##  6 Lagomorpha          0.0121       0.0121     0.0121       1
##  7 Erinaceomorpha      0.0024       0.0035     0.00295      2
##  8 Afrosoricida        0.0026       0.0026     0.0026       1
##  9 Scandentia          0.0025       0.0025     0.0025       1
## 10 Soricomorpha        0.00014      0.0012     0.000592     5
## 11 Chiroptera          0.00025      0.0003     0.000275     2
## 12 Artiodactyla       NA           NA         NA            6
## 13 Carnivora          NA           NA         NA           12
## 14 Cetacea            NA           NA         NA            3
## 15 Didelphimorphia    NA           NA         NA            2
## 16 Diprotodontia      NA           NA         NA            2
## 17 Pilosa             NA           NA         NA            1
## 18 Primates           NA           NA         NA           12
## 19 Rodentia           NA           NA         NA           22
```

2. What does `NA` mean? How are NA's being treated by the summarize function?

```r
msleep %>% 
  filter(order=="Carnivora") %>% 
  select(order, genus,brainwt)
```

```
## # A tibble: 12 × 3
##    order     genus        brainwt
##    <chr>     <chr>          <dbl>
##  1 Carnivora Acinonyx     NA     
##  2 Carnivora Callorhinus  NA     
##  3 Carnivora Canis         0.07  
##  4 Carnivora Felis         0.0256
##  5 Carnivora Haliochoerus  0.325 
##  6 Carnivora Panthera     NA     
##  7 Carnivora Panthera      0.157 
##  8 Carnivora Panthera     NA     
##  9 Carnivora Phoca        NA     
## 10 Carnivora Genetta       0.0175
## 11 Carnivora Vulpes        0.0445
## 12 Carnivora Vulpes        0.0504
```

3. Try running the code again, but this time add `na.rm=TRUE`. What is the problem with Cetacea? Compare this to Carnivora. 

## That's it! Let's take a break and then move on to part 2! 

-->[Home](https://jmledford3115.github.io/datascibiol/)  
