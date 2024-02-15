---
title: "Lab10WarmUp"
output: 
  html_document: 
    keep_md: yes
date: "2024-02-15"
---



## R Markdown

##Load the libraries 

```r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library(naniar)
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```


##Load the data 

```r
malaria <- read_csv("data/malaria.csv") %>% clean_names()
```

```
## Rows: 3038 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): location_name, Province, District
## dbl  (5): malaria_rdt_0-4, malaria_rdt_5-14, malaria_rdt_15, malaria_tot, newid
## date (2): data_date, submitted_date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(malaria)
```

```
## # A tibble: 6 × 10
##   location_name data_date  submitted_date province district malaria_rdt_0_4
##   <chr>         <date>     <date>         <chr>    <chr>              <dbl>
## 1 Facility 1    2020-08-11 2020-08-12     North    Spring                11
## 2 Facility 2    2020-08-11 2020-08-12     North    Bolo                  11
## 3 Facility 3    2020-08-11 2020-08-12     North    Dingo                  8
## 4 Facility 4    2020-08-11 2020-08-12     North    Bolo                  16
## 5 Facility 5    2020-08-11 2020-08-12     North    Bolo                   9
## 6 Facility 6    2020-08-11 2020-08-12     North    Dingo                  3
## # ℹ 4 more variables: malaria_rdt_5_14 <dbl>, malaria_rdt_15 <dbl>,
## #   malaria_tot <dbl>, newid <dbl>
```
The malaraia_rdt_0_4 is is a case and the age range. The obersvations for these variables is the number of cases in this category. 

Issue: The malaria rdt values are not tidy. The columns are observations, so they need to be switched to rows instead. 


```r
malaria_long <- malaria %>% 
  pivot_longer(col=starts_with("malaria_rdt"),
               names_to="age_class",
               values_to= "cases",
               values_drop_na=T) %>% 
  select(newid,data_date,location_name,province,district,age_class,cases)
```

Want to tranform the data that begins with the malaria_rdt column names. __)


##Which age had thehighest number of cases 


```r
malaria_long %>% 
  filter(data_date=="2020-07-30") %>% 
  group_by(district) %>% #since more than one district is represented in the data 
  summarise(tot_cases=sum(cases,na.rm=T)) %>% 
  arrange(-tot_cases)
```

```
## # A tibble: 4 × 2
##   district tot_cases
##   <chr>        <dbl>
## 1 Bolo           347
## 2 Dingo          218
## 3 Spring         140
## 4 Barnard         77
```
 the "-" in front of the variable is another way to set this data in descending order. Alt to using the arrange(desc()) function combination. 
