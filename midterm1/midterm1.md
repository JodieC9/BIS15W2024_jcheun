---
title: "Midterm 1 W24"
author: "Jodie Cheun"
date: "2024-02-06"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document. You may use your notes, labs, and homework to help you complete this exam. Do not use any other resources- including AI assistance.  

Don't forget to answer any questions that are asked in the prompt!  

Be sure to push your completed midterm to your repository. This exam is worth 30 points.  

## Background
In the data folder, you will find data related to a study on wolf mortality collected by the National Park Service. You should start by reading the `README_NPSwolfdata.pdf` file. This will provide an abstract of the study and an explanation of variables.  

The data are from: Cassidy, Kira et al. (2022). Gray wolf packs and human-caused wolf mortality. [Dryad](https://doi.org/10.5061/dryad.mkkwh713f). 

## Load the libraries

```r
library("tidyverse")
library("janitor")
```

## Load the wolves data
In these data, the authors used `NULL` to represent missing values. I am correcting this for you below and using `janitor` to clean the column names.

```r
wolves <- read.csv("data/NPS_wolfmortalitydata.csv", na = c("NULL")) %>% clean_names()
```

## Questions
Problem 1. (1 point) Let's start with some data exploration. What are the variable (column) names?  

```r
names(wolves)
```

```
##  [1] "park"         "biolyr"       "pack"         "packcode"     "packsize_aug"
##  [6] "mort_yn"      "mort_all"     "mort_lead"    "mort_nonlead" "reprody1"    
## [11] "persisty1"
```

Problem 2. (1 point) Use the function of your choice to summarize the data and get an idea of its structure.  

```r
glimpse(wolves)
```

```
## Rows: 864
## Columns: 11
## $ park         <chr> "DENA", "DENA", "DENA", "DENA", "DENA", "DENA", "DENA", "…
## $ biolyr       <int> 1996, 1991, 2017, 1996, 1992, 1994, 2007, 2007, 1995, 200…
## $ pack         <chr> "McKinley River1", "Birch Creek N", "Eagle Gorge", "East …
## $ packcode     <int> 89, 58, 71, 72, 74, 77, 101, 108, 109, 53, 63, 66, 70, 72…
## $ packsize_aug <dbl> 12, 5, 8, 13, 7, 6, 10, NA, 9, 8, 7, 11, 0, 19, 15, 12, 1…
## $ mort_yn      <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ mort_all     <int> 4, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ mort_lead    <int> 2, 2, 0, 0, 0, 0, 1, 2, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, …
## $ mort_nonlead <int> 2, 0, 2, 2, 2, 2, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, …
## $ reprody1     <int> 0, 0, NA, 1, NA, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1…
## $ persisty1    <int> 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, …
```

```r
summary(wolves)
```

```
##      park               biolyr         pack              packcode     
##  Length:864         Min.   :1986   Length:864         Min.   :  2.00  
##  Class :character   1st Qu.:1999   Class :character   1st Qu.: 48.00  
##  Mode  :character   Median :2006   Mode  :character   Median : 86.50  
##                     Mean   :2005                      Mean   : 91.39  
##                     3rd Qu.:2012                      3rd Qu.:133.00  
##                     Max.   :2021                      Max.   :193.00  
##                                                                       
##   packsize_aug       mort_yn          mort_all         mort_lead      
##  Min.   : 0.000   Min.   :0.0000   Min.   : 0.0000   Min.   :0.00000  
##  1st Qu.: 5.000   1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.00000  
##  Median : 8.000   Median :0.0000   Median : 0.0000   Median :0.00000  
##  Mean   : 8.789   Mean   :0.1956   Mean   : 0.3715   Mean   :0.09552  
##  3rd Qu.:12.000   3rd Qu.:0.0000   3rd Qu.: 0.0000   3rd Qu.:0.00000  
##  Max.   :37.000   Max.   :1.0000   Max.   :24.0000   Max.   :3.00000  
##  NA's   :55                                          NA's   :16       
##   mort_nonlead        reprody1        persisty1     
##  Min.   : 0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.: 0.0000   1st Qu.:1.0000   1st Qu.:1.0000  
##  Median : 0.0000   Median :1.0000   Median :1.0000  
##  Mean   : 0.2641   Mean   :0.7629   Mean   :0.8865  
##  3rd Qu.: 0.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :22.0000   Max.   :1.0000   Max.   :1.0000  
##  NA's   :12        NA's   :71       NA's   :9
```

Problem 3. (3 points) Which parks/ reserves are represented in the data? Don't just use the abstract, pull this information from the data. 

```r
table(wolves$park)
```

```
## 
## DENA GNTP  VNP  YNP YUCH 
##  340   77   48  248  151
```
The parks included in the data, and how many times they are counted, are shown above. 

Problem 4. (4 points) Which park has the largest number of wolf packs?

```r
tabyl(wolves,packsize_aug) %>% 
  arrange(desc(packsize_aug))
```

```
##  packsize_aug  n     percent valid_percent
##          37.0  1 0.001157407   0.001236094
##          35.0  1 0.001157407   0.001236094
##          33.0  1 0.001157407   0.001236094
##          28.0  1 0.001157407   0.001236094
##          27.0  3 0.003472222   0.003708282
##          26.4  1 0.001157407   0.001236094
##          26.0  2 0.002314815   0.002472188
##          25.0  1 0.001157407   0.001236094
##          24.0  2 0.002314815   0.002472188
##          23.0  6 0.006944444   0.007416564
##          22.0  5 0.005787037   0.006180470
##          21.0  6 0.006944444   0.007416564
##          20.4  1 0.001157407   0.001236094
##          20.0  7 0.008101852   0.008652658
##          19.0  9 0.010416667   0.011124845
##          18.0 15 0.017361111   0.018541409
##          17.0 13 0.015046296   0.016069221
##          16.8  5 0.005787037   0.006180470
##          16.0 12 0.013888889   0.014833127
##          15.6  8 0.009259259   0.009888752
##          15.0 23 0.026620370   0.028430161
##          14.4  6 0.006944444   0.007416564
##          14.0 20 0.023148148   0.024721879
##          13.2  5 0.005787037   0.006180470
##          13.0 30 0.034722222   0.037082818
##          12.0 45 0.052083333   0.055624227
##          11.0 31 0.035879630   0.038318912
##          10.8  6 0.006944444   0.007416564
##          10.0 44 0.050925926   0.054388133
##           9.6  6 0.006944444   0.007416564
##           9.0 44 0.050925926   0.054388133
##           8.4  4 0.004629630   0.004944376
##           8.0 60 0.069444444   0.074165637
##           7.2  3 0.003472222   0.003708282
##           7.0 59 0.068287037   0.072929543
##           6.0 71 0.082175926   0.087762670
##           5.0 51 0.059027778   0.063040791
##           4.8  6 0.006944444   0.007416564
##           4.0 42 0.048611111   0.051915946
##           3.6  5 0.005787037   0.006180470
##           3.0 46 0.053240741   0.056860321
##           2.4  7 0.008101852   0.008652658
##           2.0 65 0.075231481   0.080346106
##           1.0 12 0.013888889   0.014833127
##           0.0 18 0.020833333   0.022249691
##            NA 55 0.063657407            NA
```

```r
n_distinct(wolves$packsize_aug)
```

```
## [1] 46
```



```r
wolves %>% 
  select(park, pack,packsize_aug) %>% 
  arrange(desc(packsize_aug))
```

```
##     park                          pack packsize_aug
## 1    YNP                         druid         37.0
## 2    YNP                      junction         35.0
## 3   DENA                     East Fork         33.0
## 4    YNP                       leopold         28.0
## 5   DENA                     East Fork         27.0
## 6    YNP                         8mile         27.0
## 7    YNP                         druid         27.0
## 8   GNTP                       Buffalo         26.4
## 9    YNP                   gibbon/mary         26.0
## 10   YNP                       leopold         26.0
## 11   YNP                   gibbon/mary         25.0
## 12   YNP                         oxbow         24.0
## 13  YUCH                       70 Mile         24.0
## 14  DENA                      100 Mile         23.0
## 15  DENA                   Birch Creek         23.0
## 16  DENA                   Little Bear         23.0
## 17   YNP                        wapiti         23.0
## 18   YNP                       mollies         23.0
## 19   YNP                          rose         23.0
## 20   YNP                     yelldelta         22.0
## 21   YNP                      nezperce         22.0
## 22   YNP                        slough         22.0
## 23   YNP                         8mile         22.0
## 24   YNP                          rose         22.0
## 25   YNP                      junction         21.0
## 26   YNP                         druid         21.0
## 27   YNP                       leopold         21.0
## 28   YNP                          rose         21.0
## 29   YNP                        slough         21.0
## 30   YNP                        wapiti         21.0
## 31  GNTP                   Huckleberry         20.4
## 32  DENA                     East Fork         20.0
## 33  DENA                        McLeod         20.0
## 34   YNP                         8mile         20.0
## 35   YNP                         agate         20.0
## 36   YNP                       leopold         20.0
## 37   YNP                         oxbow         20.0
## 38   YNP                          swan         20.0
## 39  DENA                     East Fork         19.0
## 40  DENA               McKinley Slough         19.0
## 41   YNP                         8mile         19.0
## 42   YNP                      junction         19.0
## 43   YNP                       leopold         19.0
## 44   YNP                      nezperce         19.0
## 45   YNP                        wapiti         19.0
## 46   YNP                     yelldelta         19.0
## 47   YNP                     yelldelta         19.0
## 48  GNTP                       Buffalo         18.0
## 49   YNP                       phantom         18.0
## 50   YNP                        cougar         18.0
## 51   YNP                         8mile         18.0
## 52   YNP                         druid         18.0
## 53   YNP                         druid         18.0
## 54   YNP                   gibbon/mary         18.0
## 55   YNP                       mollies         18.0
## 56   YNP                      nezperce         18.0
## 57   YNP                      nezperce         18.0
## 58   YNP                          swan         18.0
## 59   YNP                        wapiti         18.0
## 60   YNP                     yelldelta         18.0
## 61  YUCH                       70 Mile         18.0
## 62  YUCH                Lower Charley1         18.0
## 63  DENA                     East Fork         17.0
## 64  DENA                   Riley Creek         17.0
## 65  DENA                       Bearpaw         17.0
## 66  DENA                   Pinto Creek         17.0
## 67  DENA                   Pinto Creek         17.0
## 68   YNP                         8mile         17.0
## 69   YNP                     blacktail         17.0
## 70   YNP                       mollies         17.0
## 71   YNP                       mollies         17.0
## 72   YNP                       mollies         17.0
## 73   YNP                     yelldelta         17.0
## 74   YNP                     yelldelta         17.0
## 75   YNP                     yelldelta         17.0
## 76  GNTP                       Buffalo         16.8
## 77  GNTP                 Pacific Creek         16.8
## 78  GNTP                 Pinnacle Peak         16.8
## 79  GNTP                 Pinnacle Peak         16.8
## 80  GNTP                         Teton         16.8
## 81  DENA                       Bearpaw         16.0
## 82  DENA                     East Fork         16.0
## 83  DENA                     East Fork         16.0
## 84  DENA                   Grant Creek         16.0
## 85   YNP                       phantom         16.0
## 86   YNP                     blacktail         16.0
## 87   YNP                         druid         16.0
## 88   YNP                       leopold         16.0
## 89   YNP                       mollies         16.0
## 90   YNP                          rose         16.0
## 91  YUCH                    Cottonwood         16.0
## 92  YUCH                    Cottonwood         16.0
## 93  GNTP                       Buffalo         15.6
## 94  GNTP               Phantom Springs         15.6
## 95  GNTP                 Pinnacle Peak         15.6
## 96  GNTP                         Teton         15.6
## 97  GNTP                   Huckleberry         15.6
## 98  GNTP                 Pacific Creek         15.6
## 99  GNTP                 Pacific Creek         15.6
## 100 GNTP                 Pinnacle Peak         15.6
## 101 DENA                     East Fork         15.0
## 102 DENA                     East Fork         15.0
## 103 DENA                   Birch Creek         15.0
## 104 DENA                     East Fork         15.0
## 105 DENA               Iron Creek West         15.0
## 106 DENA                   John Hansen         15.0
## 107 DENA               McKinley Slough         15.0
## 108 DENA               McKinley Slough         15.0
## 109 DENA               McKinley Slough         15.0
## 110 DENA                        McLeod         15.0
## 111 DENA                     Sanctuary         15.0
## 112  YNP                      junction         15.0
## 113  YNP                         8mile         15.0
## 114  YNP                       mollies         15.0
## 115  YNP                        cougar         15.0
## 116  YNP                         druid         15.0
## 117  YNP                       leopold         15.0
## 118  YNP                       mollies         15.0
## 119  YNP                       mollies         15.0
## 120  YNP                       mollies         15.0
## 121  YNP                        slough         15.0
## 122  YNP                     yelldelta         15.0
## 123  YNP                     yelldelta         15.0
## 124 GNTP                         Teton         14.4
## 125 GNTP             Lower Gros Ventre         14.4
## 126 GNTP                 Pacific Creek         14.4
## 127 GNTP                 Pinnacle Peak         14.4
## 128 GNTP                 Pinnacle Peak         14.4
## 129 GNTP                         Teton         14.4
## 130 DENA                     East Fork         14.0
## 131 DENA                   Pinto Creek         14.0
## 132 DENA                      100 Mile         14.0
## 133 DENA                     East Fork         14.0
## 134 DENA                   Grant Creek         14.0
## 135 DENA                  Headquarters         14.0
## 136 DENA                  Nenana River         14.0
## 137 DENA                   Riley Creek         14.0
## 138  YNP                         agate         14.0
## 139  YNP                         agate         14.0
## 140  YNP                        cougar         14.0
## 141  YNP                        cougar         14.0
## 142  YNP                        cougar         14.0
## 143  YNP                       leopold         14.0
## 144  YNP                      nezperce         14.0
## 145 YUCH                       70 Mile         14.0
## 146 YUCH                    Cottonwood         14.0
## 147 YUCH                  Fisher Creek         14.0
## 148 YUCH                Lower Charley1         14.0
## 149 YUCH                Step Mountain2         14.0
## 150 GNTP                   Huckleberry         13.2
## 151 GNTP                 Pacific Creek         13.2
## 152 GNTP               Phantom Springs         13.2
## 153 GNTP                 Pinnacle Peak         13.2
## 154 GNTP                         Teton         13.2
## 155 DENA                     East Fork         13.0
## 156 DENA                     East Fork         13.0
## 157 DENA                   Grant Creek         13.0
## 158 DENA                   Grant Creek         13.0
## 159 DENA                        McLeod         13.0
## 160 DENA                        McLeod         13.0
## 161 DENA                        McLeod         13.0
## 162  YNP                         lamar         13.0
## 163  YNP                      junction         13.0
## 164  YNP                         snake         13.0
## 165  YNP                         agate         13.0
## 166  YNP                     blacktail         13.0
## 167  YNP                         8mile         13.0
## 168  YNP                         8mile         13.0
## 169  YNP                         agate         13.0
## 170  YNP                         agate         13.0
## 171  YNP                         agate         13.0
## 172  YNP                         druid         13.0
## 173  YNP                       mollies         13.0
## 174  YNP                      prospect         13.0
## 175  YNP                     yelldelta         13.0
## 176 YUCH                    Lost Creek         13.0
## 177 YUCH                  Three Finger         13.0
## 178 YUCH                       70 Mile         13.0
## 179 YUCH                       70 Mile         13.0
## 180 YUCH                    Cottonwood         13.0
## 181 YUCH               Hard Luck Creek         13.0
## 182 YUCH                Lower Charley1         13.0
## 183 YUCH                Step Mountain2         13.0
## 184 YUCH                  Three Finger         13.0
## 185 DENA               McKinley River1         12.0
## 186 DENA                     East Fork         12.0
## 187 DENA                   Mt Margaret         12.0
## 188 DENA                      100 Mile         12.0
## 189 DENA              Chitsia Mountain         12.0
## 190 DENA                     East Fork         12.0
## 191 DENA                       Foraker         12.0
## 192 DENA                   Grant Creek         12.0
## 193 DENA                     Highpower         12.0
## 194 DENA                   Little Bear         12.0
## 195 DENA                   Little Bear         12.0
## 196 DENA                   Little Bear         12.0
## 197 DENA                        McLeod         12.0
## 198 DENA                        McLeod         12.0
## 199 DENA                  Straightaway         12.0
## 200 GNTP             Lower Gros Ventre         12.0
## 201 GNTP                 Pinnacle Peak         12.0
## 202 GNTP                 Pinnacle Peak         12.0
## 203 GNTP                 Pinnacle Peak         12.0
## 204 GNTP                   Huckleberry         12.0
## 205 GNTP                   Huckleberry         12.0
## 206 GNTP                 Pinnacle Peak         12.0
## 207  YNP                    cottonwood         12.0
## 208  YNP                       bechler         12.0
## 209  YNP                       mollies         12.0
## 210  YNP                         druid         12.0
## 211  YNP                        everts         12.0
## 212  YNP                    geode/hell         12.0
## 213  YNP                   gibbon/mary         12.0
## 214  YNP                         lamar         12.0
## 215  YNP                       leopold         12.0
## 216  YNP                       mollies         12.0
## 217  YNP                      nezperce         12.0
## 218  YNP                         oxbow         12.0
## 219  YNP                      prospect         12.0
## 220 YUCH                  Three Finger         12.0
## 221 YUCH                       70 Mile         12.0
## 222 YUCH                       70 Mile         12.0
## 223 YUCH                       70 Mile         12.0
## 224 YUCH                    Cottonwood         12.0
## 225 YUCH                Edwards Creek1         12.0
## 226 YUCH               Hard Luck Creek         12.0
## 227 YUCH                    Lost Creek         12.0
## 228 YUCH                  Nation River         12.0
## 229 YUCH                Step Mountain2         12.0
## 230 DENA                       Chitsia         11.0
## 231 DENA                     East Fork         11.0
## 232 DENA                        Somber         11.0
## 233 DENA                  Straightaway         11.0
## 234 DENA                      100 Mile         11.0
## 235 DENA                   Birch Creek         11.0
## 236 DENA                       Chitsia         11.0
## 237 DENA                  Headquarters         11.0
## 238 DENA                     Highpower         11.0
## 239 DENA                   McLeod West         11.0
## 240 DENA                   Mt Margaret         11.0
## 241  YNP                      junction         11.0
## 242  YNP                       biscuit         11.0
## 243  YNP                        cougar         11.0
## 244  YNP                        cougar         11.0
## 245  YNP                   gibbon/mary         11.0
## 246  YNP                      junction         11.0
## 247  YNP                         lamar         11.0
## 248  YNP                       leopold         11.0
## 249  YNP                       mollies         11.0
## 250  YNP                      nezperce         11.0
## 251  YNP                          rose         11.0
## 252  YNP                          rose         11.0
## 253  YNP                     yelldelta         11.0
## 254 YUCH                    Lost Creek         11.0
## 255 YUCH                       70 Mile         11.0
## 256 YUCH                  Fisher Creek         11.0
## 257 YUCH                   Godge Creek         11.0
## 258 YUCH                Lower Charley1         11.0
## 259 YUCH                  Three Finger         11.0
## 260 YUCH                  Three Finger         11.0
## 261 GNTP                       Buffalo         10.8
## 262 GNTP               Phantom Springs         10.8
## 263 GNTP                   Huckleberry         10.8
## 264 GNTP                 Pacific Creek         10.8
## 265 GNTP               Phantom Springs         10.8
## 266 GNTP               Phantom Springs         10.8
## 267 DENA                         Pinto         10.0
## 268 DENA                     East Fork         10.0
## 269 DENA                       Bearpaw         10.0
## 270 DENA                       Bearpaw         10.0
## 271 DENA                      Bearpaw1         10.0
## 272 DENA                     East Fork         10.0
## 273 DENA                     East Fork         10.0
## 274 DENA                  Headquarters         10.0
## 275 DENA                        Herron         10.0
## 276 DENA                     Highpower         10.0
## 277 DENA                    Hot Slough         10.0
## 278 DENA               Iron Creek West         10.0
## 279 DENA                McKinley River         10.0
## 280 DENA               McKinley Slough         10.0
## 281 DENA                        McLeod         10.0
## 282 DENA                   Mt Margaret         10.0
## 283 DENA                   Mt Margaret         10.0
## 284 DENA                   Pinto Creek         10.0
## 285 DENA                     Sanctuary         10.0
## 286 DENA                      Stampede         10.0
## 287 DENA                    Starr Lake         10.0
## 288 DENA                  Straightaway         10.0
## 289  YNP                     642Fgroup         10.0
## 290  YNP                      prospect         10.0
## 291  YNP                     blacktail         10.0
## 292  YNP                        canyon         10.0
## 293  YNP                         agate         10.0
## 294  YNP                        cougar         10.0
## 295  YNP                   gibbon/mary         10.0
## 296  YNP                       leopold         10.0
## 297  YNP                       mollies         10.0
## 298  YNP                       mollies         10.0
## 299  YNP                          rose         10.0
## 300  YNP                        slough         10.0
## 301  YNP                          swan         10.0
## 302 YUCH                Lower Charley2         10.0
## 303 YUCH                   Woodchopper         10.0
## 304 YUCH                    Yukon Fork         10.0
## 305 YUCH                    Cottonwood         10.0
## 306 YUCH                    Cottonwood         10.0
## 307 YUCH                    Lost Creek         10.0
## 308 YUCH                Step Mountain2         10.0
## 309 YUCH                  Three Finger         10.0
## 310 YUCH                 Webber Creek1         10.0
## 311 GNTP               Phantom Springs          9.6
## 312 GNTP                      Antelope          9.6
## 313 GNTP                    Flat Creek          9.6
## 314 GNTP             Lower Gros Ventre          9.6
## 315 GNTP                      Antelope          9.6
## 316 GNTP                 Pinnacle Peak          9.6
## 317 DENA                       Savage1          9.0
## 318 DENA                  Nenana River          9.0
## 319 DENA                       Savage1          9.0
## 320 DENA                  Headquarters          9.0
## 321 DENA                     Sanctuary          9.0
## 322 DENA                   Beaver Fork          9.0
## 323 DENA              Chitsia Mountain          9.0
## 324 DENA                     East Fork          9.0
## 325 DENA                     East Fork          9.0
## 326 DENA                     East Fork          9.0
## 327 DENA                     East Fork          9.0
## 328 DENA                       Foraker          9.0
## 329 DENA                       Foraker          9.0
## 330 DENA                    Iron Creek          9.0
## 331 DENA               McKinley River1          9.0
## 332 DENA                  Pirate Creek          9.0
## 333 DENA                   Riley Creek          9.0
## 334 DENA                    Starr Lake          9.0
## 335 DENA                  Straightaway          9.0
## 336  YNP                   gibbon/mary          9.0
## 337  YNP                      nezperce          9.0
## 338  YNP                          rose          9.0
## 339  YNP                         agate          9.0
## 340  YNP                        canyon          9.0
## 341  YNP                         druid          9.0
## 342  YNP                        everts          9.0
## 343  YNP                    geode/hell          9.0
## 344  YNP                    geode/hell          9.0
## 345  YNP                        hayden          9.0
## 346  YNP                       mollies          9.0
## 347  YNP                        slough          9.0
## 348  YNP                          swan          9.0
## 349  YNP                        wapiti          9.0
## 350  YNP                     yelldelta          9.0
## 351 YUCH                       70 Mile          9.0
## 352 YUCH                    Cottonwood          9.0
## 353 YUCH                Edwards Creek1          9.0
## 354 YUCH                       70 Mile          9.0
## 355 YUCH               Hard Luck Creek          9.0
## 356 YUCH                Lower Charley2          9.0
## 357 YUCH                Lower Charley2          9.0
## 358 YUCH                   Snowy Peak2          9.0
## 359 YUCH                Step Mountain2          9.0
## 360 YUCH                  Three Finger          9.0
## 361 GNTP             Lower Gros Ventre          8.4
## 362 GNTP                   Huckleberry          8.4
## 363 GNTP                   Huckleberry          8.4
## 364 GNTP                   Huckleberry          8.4
## 365 DENA                   Eagle Gorge          8.0
## 366 DENA                      100 Mile          8.0
## 367 DENA                    Hot Slough          8.0
## 368 DENA                   Little Bear          8.0
## 369 DENA                      100 Mile          8.0
## 370 DENA                       Bearpaw          8.0
## 371 DENA                       Bearpaw          8.0
## 372 DENA                       Chitsia          8.0
## 373 DENA              Chitsia Mountain          8.0
## 374 DENA              Chitsia Mountain          8.0
## 375 DENA                    Clearwater          8.0
## 376 DENA                     East Fork          8.0
## 377 DENA                     East Fork          8.0
## 378 DENA                     Ewe Creek          8.0
## 379 DENA                       Foraker          8.0
## 380 DENA                       Foraker          8.0
## 381 DENA                       Foraker          8.0
## 382 DENA                       Foraker          8.0
## 383 DENA                        Herron          8.0
## 384 DENA                     Highpower          8.0
## 385 DENA                    Iron Creek          8.0
## 386 DENA                   John Hansen          8.0
## 387 DENA                   John Hansen          8.0
## 388 DENA               Kantishna River          8.0
## 389 DENA               Kantishna River          8.0
## 390 DENA               McKinley River1          8.0
## 391 DENA               McKinley Slough          8.0
## 392 DENA               McKinley Slough          8.0
## 393 DENA               McKinley Slough          8.0
## 394 DENA                  Nenana River          8.0
## 395 DENA                   Pinto Creek          8.0
## 396 DENA                       Savage1          8.0
## 397 DENA                        Somber          8.0
## 398 DENA                      Stampede          8.0
## 399 DENA                  Turtle Hill1          8.0
## 400 DENA                   Windy Creek          8.0
## 401  YNP                         lamar          8.0
## 402  YNP                       mollies          8.0
## 403  YNP                        canyon          8.0
## 404  YNP                        cougar          8.0
## 405  YNP                         druid          8.0
## 406  YNP                   gibbon/mary          8.0
## 407  YNP                      junction          8.0
## 408  YNP                         lamar          8.0
## 409  YNP                       mollies          8.0
## 410  YNP                       mollies          8.0
## 411  YNP                       mollies          8.0
## 412  YNP               specimen/silver          8.0
## 413  YNP                     thorofare          8.0
## 414  YNP                     yelldelta          8.0
## 415  YNP                     yelldelta          8.0
## 416 YUCH                       70 Mile          8.0
## 417 YUCH                    Cottonwood          8.0
## 418 YUCH                    Cottonwood          8.0
## 419 YUCH                   Godge Creek          8.0
## 420 YUCH                    Lost Creek          8.0
## 421 YUCH                Lower Charley1          8.0
## 422 YUCH                Lower Charley2          8.0
## 423 YUCH                   Sheep Bluff          8.0
## 424 YUCH                Step Mountain2          8.0
## 425 GNTP                         Teton          7.2
## 426 GNTP                   Huckleberry          7.2
## 427 GNTP                   Huckleberry          7.2
## 428 DENA                       Foraker          7.0
## 429 DENA                 Castle Rocks2          7.0
## 430 DENA                   Grant Creek          7.0
## 431 DENA                   John Hansen          7.0
## 432 DENA               McKinley River1          7.0
## 433 DENA                      100 Mile          7.0
## 434 DENA                 Castle Rocks3          7.0
## 435 DENA                       Chitsia          7.0
## 436 DENA                   Corner Lake          7.0
## 437 DENA                     East Fork          7.0
## 438 DENA                     East Fork          7.0
## 439 DENA                       Foraker          7.0
## 440 DENA                  Headquarters          7.0
## 441 DENA                        Herron          7.0
## 442 DENA                    Hot Slough          7.0
## 443 DENA                    Hot Slough          7.0
## 444 DENA               Kantishna River          7.0
## 445 DENA                   Little Bear          7.0
## 446 DENA               McKinley Slough          7.0
## 447 DENA                        McLeod          7.0
## 448 DENA                        McLeod          7.0
## 449 DENA                   Mt Margaret          7.0
## 450 DENA                   Mt Margaret          7.0
## 451 DENA                  Nenana River          7.0
## 452 DENA                   Otter Creek          7.0
## 453 DENA                      Stampede          7.0
## 454 DENA                      Stampede          7.0
## 455 DENA                    Starr Lake          7.0
## 456 DENA                         Stony          7.0
## 457 DENA                  Turtle Hill1          7.0
## 458  VNP                   nebraskabay          7.0
## 459  VNP                 Shoepack Lake          7.0
## 460  VNP                     Half-Moon          7.0
## 461  YNP                        cougar          7.0
## 462  YNP                      prospect          7.0
## 463  YNP                        cougar          7.0
## 464  YNP                      nezperce          7.0
## 465  YNP                        cougar          7.0
## 466  YNP                        cougar          7.0
## 467  YNP                    geode/hell          7.0
## 468  YNP                        hayden          7.0
## 469  YNP                         heart          7.0
## 470  YNP                         lamar          7.0
## 471  YNP                       leopold          7.0
## 472  YNP                       mollies          7.0
## 473  YNP                      nezperce          7.0
## 474  YNP                      quadrant          7.0
## 475  YNP                      quadrant          7.0
## 476  YNP                          swan          7.0
## 477 YUCH               Copper Mountain          7.0
## 478 YUCH                    Cottonwood          7.0
## 479 YUCH                   Godge Creek          7.0
## 480 YUCH                   Godge Creek          7.0
## 481 YUCH                    Lost Creek          7.0
## 482 YUCH                 Poverty Creek          7.0
## 483 YUCH                Step Mountain2          7.0
## 484 YUCH                Step Mountain2          7.0
## 485 YUCH                Sterling Creek          7.0
## 486 YUCH                  Three Finger          7.0
## 487 DENA                  Headquarters          6.0
## 488 DENA                       McLeod2          6.0
## 489 DENA                       Bearpaw          6.0
## 490 DENA                       Bearpaw          6.0
## 491 DENA                       Bearpaw          6.0
## 492 DENA                   Beaver Fork          6.0
## 493 DENA                 Castle Rocks3          6.0
## 494 DENA                 Chilchukabena          6.0
## 495 DENA                    Clearwater          6.0
## 496 DENA                    Clearwater          6.0
## 497 DENA                       Foraker          6.0
## 498 DENA                   Grant Creek          6.0
## 499 DENA                   Grant Creek          6.0
## 500 DENA                   Grant Creek          6.0
## 501 DENA               Iron Creek West          6.0
## 502 DENA               Iron Creek West          6.0
## 503 DENA                   Jenny Creek          6.0
## 504 DENA               Kantishna River          6.0
## 505 DENA               Kantishna River          6.0
## 506 DENA               Kantishna River          6.0
## 507 DENA                McKinley River          6.0
## 508 DENA                McKinley River          6.0
## 509 DENA                        McLeod          6.0
## 510 DENA                   Mt Margaret          6.0
## 511 DENA                        Myrtle          6.0
## 512 DENA                  Nenana River          6.0
## 513 DENA                   Otter Creek          6.0
## 514 DENA                   Riley Creek          6.0
## 515 DENA                     Sanctuary          6.0
## 516 DENA                 Sandless Lake          6.0
## 517 DENA                        Savage          6.0
## 518 DENA                        Somber          6.0
## 519 DENA                        Somber          6.0
## 520 DENA                      Stampede          6.0
## 521 DENA                    Starr Lake          6.0
## 522 DENA                    Starr Lake          6.0
## 523 DENA                         Stony          6.0
## 524 GNTP               Phantom Springs          6.0
## 525 GNTP                   Huckleberry          6.0
## 526 GNTP             Lower Gros Ventre          6.0
## 527 GNTP             Lower Gros Ventre          6.0
## 528 GNTP             Lower Gros Ventre          6.0
## 529  VNP                     tomcodbay          6.0
## 530  VNP                     tomcodbay          6.0
## 531  YNP                       mollies          6.0
## 532  YNP                        canyon          6.0
## 533  YNP                        canyon          6.0
## 534  YNP                        canyon          6.0
## 535  YNP                        canyon          6.0
## 536  YNP                        cougar          6.0
## 537  YNP                        cougar          6.0
## 538  YNP                        cougar          6.0
## 539  YNP                        cougar          6.0
## 540  YNP                      grayling          6.0
## 541  YNP                     yelldelta          6.0
## 542  YNP                     yelldelta          6.0
## 543 YUCH                  Fisher Creek          6.0
## 544 YUCH                   Hanna Creek          6.0
## 545 YUCH                Step Mountain2          6.0
## 546 YUCH                Step Mountain2          6.0
## 547 YUCH                    Cottonwood          6.0
## 548 YUCH               Crescent Creek2          6.0
## 549 YUCH                Edwards Creek1          6.0
## 550 YUCH                Edwards Creek2          6.0
## 551 YUCH                Edwards Creek2          6.0
## 552 YUCH               Hard Luck Creek          6.0
## 553 YUCH               Kathul Mountain          6.0
## 554 YUCH                  Nation River          6.0
## 555 YUCH                   Sheep Bluff          6.0
## 556 YUCH                Step Mountain2          6.0
## 557 YUCH                   Woodchopper          6.0
## 558 DENA                 Birch Creek N          5.0
## 559 DENA                     Ewe Creek          5.0
## 560 DENA                   Grant Creek          5.0
## 561 DENA                   Grant Creek          5.0
## 562 DENA                   Mt Margaret          5.0
## 563 DENA                        Myrtle          5.0
## 564 DENA                       Bearpaw          5.0
## 565 DENA                       Bearpaw          5.0
## 566 DENA                   Eagle Gorge          5.0
## 567 DENA                   Grant Creek          5.0
## 568 DENA                   Grant Creek          5.0
## 569 DENA                  Headquarters          5.0
## 570 DENA               Iron Creek East          5.0
## 571 DENA               Kantishna River          5.0
## 572 DENA               Kantishna River          5.0
## 573 DENA               McKinley Slough          5.0
## 574 DENA                  Nenana River          5.0
## 575 DENA                    North Fork          5.0
## 576 DENA                    North Fork          5.0
## 577 DENA                         Pinto          5.0
## 578 DENA                     Sanctuary          5.0
## 579 DENA                Slippery Creek          5.0
## 580 DENA                        Somber          5.0
## 581 DENA                        Somber          5.0
## 582 DENA                   White Creek          5.0
## 583 DENA                   Windy Creek          5.0
## 584  VNP                   cruiserlake          5.0
## 585  VNP                    Sand Point          5.0
## 586  YNP                    1118Fgroup          5.0
## 587  YNP                       phantom          5.0
## 588  YNP                         druid          5.0
## 589  YNP                        canyon          5.0
## 590  YNP                        cougar          5.0
## 591  YNP                         druid          5.0
## 592  YNP                         druid          5.0
## 593  YNP                        hayden          5.0
## 594  YNP                          lava          5.0
## 595  YNP                       leopold          5.0
## 596  YNP                       mollies          5.0
## 597  YNP                       mollies          5.0
## 598  YNP               specimen/silver          5.0
## 599  YNP                     yelldelta          5.0
## 600 YUCH                Edwards Creek2          5.0
## 601 YUCH               Copper Mountain          5.0
## 602 YUCH               Copper Mountain          5.0
## 603 YUCH                Edwards Creek2          5.0
## 604 YUCH                    Flat Creek          5.0
## 605 YUCH               Hard Luck Creek          5.0
## 606 YUCH                  Lower Kandik          5.0
## 607 YUCH                 Webber Creek1          5.0
## 608 YUCH                    Yukon Fork          5.0
## 609 GNTP                         Teton          4.8
## 610 GNTP                      Antelope          4.8
## 611 GNTP       Horsetail Creek (Murie)          4.8
## 612 GNTP                   Huckleberry          4.8
## 613 GNTP             Lower Gros Ventre          4.8
## 614 GNTP               Phantom Springs          4.8
## 615 DENA                     East Fork          4.0
## 616 DENA                   Hauke Creek          4.0
## 617 DENA                   Birch Hills          4.0
## 618 DENA                 Caribou Creek          4.0
## 619 DENA                       Chitsia          4.0
## 620 DENA              Chitsia Mountain          4.0
## 621 DENA              Chitsia Mountain          4.0
## 622 DENA                    Clearwater          4.0
## 623 DENA                   Corner Lake          4.0
## 624 DENA                   Corner Lake          4.0
## 625 DENA                     East Fork          4.0
## 626 DENA                   Grant Creek          4.0
## 627 DENA               McKinley Slough          4.0
## 628 DENA                   Muddy River          4.0
## 629 DENA                    North Fork          4.0
## 630 DENA                  Straightaway          4.0
## 631 DENA                   White Creek          4.0
## 632  YNP                     963Fgroup          4.0
## 633  YNP                         snake          4.0
## 634  YNP                         agate          4.0
## 635  YNP                       bechler          4.0
## 636  YNP                   buffalofork          4.0
## 637  YNP                        canyon          4.0
## 638  YNP                    cottonwood          4.0
## 639  YNP                        cougar          4.0
## 640  YNP                        cougar          4.0
## 641  YNP                        hayden          4.0
## 642  YNP                         lamar          4.0
## 643  YNP                      quadrant          4.0
## 644  YNP                         tower          4.0
## 645  YNP                     yelldelta          4.0
## 646 YUCH              Washington Creek          4.0
## 647 YUCH                  Three Finger          4.0
## 648 YUCH                Edwards Creek2          4.0
## 649 YUCH                Edwards Creek2          4.0
## 650 YUCH                  Fisher Creek          4.0
## 651 YUCH                Lower Charley2          4.0
## 652 YUCH                Lower Charley2          4.0
## 653 YUCH                  Lower Kandik          4.0
## 654 YUCH                  Nation River          4.0
## 655 YUCH                Step Mountain2          4.0
## 656 YUCH                  Three Finger          4.0
## 657 GNTP               Phantom Springs          3.6
## 658 GNTP       Horsetail Creek (Murie)          3.6
## 659 GNTP                   Huckleberry          3.6
## 660 GNTP             Lower Gros Ventre          3.6
## 661 GNTP                         Teton          3.6
## 662 DENA                     East Fork          3.0
## 663 DENA                   Muddy River          3.0
## 664 DENA                      Stampede          3.0
## 665 DENA                    Starr Lake          3.0
## 666 DENA                      100 Mile          3.0
## 667 DENA                   Beaver Fork          3.0
## 668 DENA                   Beaver Fork          3.0
## 669 DENA                     Boot Lake          3.0
## 670 DENA                       Brooker          3.0
## 671 DENA                  Death Valley          3.0
## 672 DENA                    Hot Slough          3.0
## 673 DENA                    Hot Slough          3.0
## 674 DENA                    Hot Slough          3.0
## 675 DENA                    Hot Slough          3.0
## 676 DENA                   John Hansen          3.0
## 677 DENA               McKinley River1          3.0
## 678 DENA               McKinley Slough          3.0
## 679 DENA                   Mt Margaret          3.0
## 680 DENA                     Sanctuary          3.0
## 681 DENA                      Stampede          3.0
## 682 DENA                      Stampede          3.0
## 683 DENA                    Starr Lake          3.0
## 684 DENA                    Starr Lake          3.0
## 685 DENA                         Stony          3.0
## 686 DENA                         Stony          3.0
## 687 DENA                  Turtle Hill           3.0
## 688 DENA                  Turtle Hill           3.0
## 689 DENA                   White Creek          3.0
## 690  VNP                     bluemoose          3.0
## 691  YNP                    1155Mgroup          3.0
## 692  YNP                      grayling          3.0
## 693  YNP                   buffalofork          3.0
## 694  YNP                      cinnabar          3.0
## 695  YNP                         lamar          3.0
## 696  YNP                          lava          3.0
## 697  YNP                          swan          3.0
## 698 YUCH               Copper Mountain          3.0
## 699 YUCH               Crescent Creek2          3.0
## 700 YUCH                Edwards Creek2          3.0
## 701 YUCH                Fourth of July          3.0
## 702 YUCH                Edwards Creek2          3.0
## 703 YUCH                Step Mountain2          3.0
## 704 YUCH                  Three Finger          3.0
## 705 YUCH        Upper Black River Pair          3.0
## 706 YUCH              Washington Creek          3.0
## 707 YUCH                 Webber Creek1          3.0
## 708 GNTP               Phantom Springs          2.4
## 709 GNTP             Lower Gros Ventre          2.4
## 710 GNTP Lower Slide Lake (781M Group)          2.4
## 711 GNTP Lower Slide Lake (781M Group)          2.4
## 712 GNTP Lower Slide Lake (781M Group)          2.4
## 713 GNTP                 Pinnacle Peak          2.4
## 714 GNTP                         Teton          2.4
## 715 DENA               Iron Creek West          2.0
## 716 DENA                   Mt Margaret          2.0
## 717 DENA                      Stampede          2.0
## 718 DENA                       Bearpaw          2.0
## 719 DENA                       Bearpaw          2.0
## 720 DENA                     Boot Lake          2.0
## 721 DENA                       Brooker          2.0
## 722 DENA                 Castle Rocks3          2.0
## 723 DENA                  Death Valley          2.0
## 724 DENA                  Death Valley          2.0
## 725 DENA                   Grant Creek          2.0
## 726 DENA                   Grant Creek          2.0
## 727 DENA                  Headquarters          2.0
## 728 DENA                  Headquarters          2.0
## 729 DENA               Iron Creek East          2.0
## 730 DENA               Kantishna River          2.0
## 731 DENA                McKinley River          2.0
## 732 DENA               McKinley Slough          2.0
## 733 DENA               McKinley Slough          2.0
## 734 DENA               McKinley Slough          2.0
## 735 DENA               McKinley Slough          2.0
## 736 DENA               McKinley Slough          2.0
## 737 DENA                   Moose Creek          2.0
## 738 DENA                   Muddy River          2.0
## 739 DENA                   Muddy River          2.0
## 740 DENA                    Otter Lake          2.0
## 741 DENA              Riley Creek West          2.0
## 742 DENA                        Somber          2.0
## 743 DENA                        Somber          2.0
## 744 DENA                      Stampede          2.0
## 745 DENA                      Stampede          2.0
## 746 DENA                      Stampede          2.0
## 747 DENA                      Stampede          2.0
## 748 DENA                    Starr Lake          2.0
## 749 DENA                    Starr Lake          2.0
## 750 DENA                         Stony          2.0
## 751 DENA                   White Creek          2.0
## 752  VNP                      Paradise          2.0
## 753  VNP                    Sand Point          2.0
## 754  YNP                        canyon          2.0
## 755  YNP                     755Mgroup          2.0
## 756  YNP                         heart          2.0
## 757  YNP                       mollies          2.0
## 758  YNP                     thorofare          2.0
## 759  YNP                         tower          2.0
## 760  YNP                        wapiti          2.0
## 761 YUCH                    Flat Creek          2.0
## 762 YUCH                    Yukon Fork          2.0
## 763 YUCH             Andrew Creek Pair          2.0
## 764 YUCH              Birch Creek Pair          2.0
## 765 YUCH          Crescent Creek1 Pair          2.0
## 766 YUCH                  Fisher Creek          2.0
## 767 YUCH                  Fisher Creek          2.0
## 768 YUCH                    Flat Creek          2.0
## 769 YUCH                    Flat Creek          2.0
## 770 YUCH                   Godge Creek          2.0
## 771 YUCH                  Lower Kandik          2.0
## 772 YUCH                  Lower Kandik          2.0
## 773 YUCH                  Nation River          2.0
## 774 YUCH                 Poverty Creek          2.0
## 775 YUCH                    Rock Creek          2.0
## 776 YUCH                Step Mountain2          2.0
## 777 YUCH        Upper Black River Pair          2.0
## 778 YUCH              Washington Creek          2.0
## 779 YUCH                    Yukon Fork          2.0
## 780 DENA               Kantishna River          1.0
## 781 DENA                       Tonzona          1.0
## 782 DENA                       Bearpaw          1.0
## 783 DENA                 Castle Rocks2          1.0
## 784 DENA                   Corner Lake          1.0
## 785 DENA                       Foraker          1.0
## 786 DENA               Kantishna River          1.0
## 787 DENA                Slippery Creek          1.0
## 788  YNP                          lava          1.0
## 789 YUCH                Lower Charley1          1.0
## 790 YUCH                Edwards Creek1          1.0
## 791 YUCH        Upper Black River Pair          1.0
## 792 DENA                  Death Valley          0.0
## 793 DENA                   Birch Hills          0.0
## 794 DENA                   Hauke Creek          0.0
## 795 DENA                    Hot Slough          0.0
## 796 DENA                   Little Bear          0.0
## 797 DENA                   Moose Creek          0.0
## 798  YNP                     682Mgroup          0.0
## 799  YNP                     694Fgroup          0.0
## 800  YNP                         agate          0.0
## 801  YNP                     carnelian          0.0
## 802  YNP                         druid          0.0
## 803  YNP                   gibbon/mary          0.0
## 804  YNP                      lonestar          0.0
## 805  YNP                      nezperce          0.0
## 806  YNP                     thorofare          0.0
## 807 YUCH               Hard Luck Creek          0.0
## 808 YUCH                   Snowy Peak1          0.0
## 809 YUCH                   Woodchopper          0.0
## 810 DENA                        Savage           NA
## 811 DENA                   Pinto Creek           NA
## 812 DENA                       Foraker           NA
## 813 DENA                       Foraker           NA
## 814 GNTP                 Wildcat Ridge           NA
## 815  VNP                   cruiserlake           NA
## 816  VNP                   Moose River           NA
## 817  VNP                   Moose River           NA
## 818  VNP                   Sheep Ranch           NA
## 819  VNP                   Sheep Ranch           NA
## 820  VNP                    Bowman Bay           NA
## 821  VNP                    Fawn Creek           NA
## 822  VNP                   locatorlake           NA
## 823  VNP               middlepeninsula           NA
## 824  VNP               middlepeninsula           NA
## 825  VNP               mooserivergrade           NA
## 826  VNP                     brownsbay           NA
## 827  VNP                     brownsbay           NA
## 828  VNP                   nebraskabay           NA
## 829  VNP                       ratroot           NA
## 830  VNP                     Ash River           NA
## 831  VNP                     Ash River           NA
## 832  VNP                    Bowman Bay           NA
## 833  VNP                    Sand Point           NA
## 834  VNP                     Ash River           NA
## 835  VNP                    Bowman Bay           NA
## 836  VNP                    Sand Point           NA
## 837  VNP                     Ash River           NA
## 838  VNP                    Bowman Bay           NA
## 839  VNP                     Lightfoot           NA
## 840  VNP                   Moose River           NA
## 841  VNP                    Sand Point           NA
## 842  VNP                    Bowman Bay           NA
## 843  VNP                    Fawn Creek           NA
## 844  VNP                     Lightfoot           NA
## 845  VNP                   Moose River           NA
## 846  VNP                    Sand Point           NA
## 847  VNP                   Sheep Ranch           NA
## 848  VNP                 Cranberry Bay           NA
## 849  VNP                       Nashata           NA
## 850  VNP                      Paradise           NA
## 851  VNP                       Nashata           NA
## 852  VNP                      Windsong           NA
## 853  YNP                     682Mgroup           NA
## 854  YNP                     blacktail           NA
## 855  YNP                       crevice           NA
## 856  YNP                   gibbon/mary           NA
## 857  YNP                      quadrant           NA
## 858 YUCH                       70 Mile           NA
## 859 YUCH                    Cottonwood           NA
## 860 YUCH                   Sheep Bluff           NA
## 861 YUCH                Webber Creek 2           NA
## 862 YUCH                    Lost Creek           NA
## 863 YUCH                  Upper Kandik           NA
## 864 YUCH                  Upper Kandik           NA
```
As shown, YNP park from the druid pack has the largest pack size at 37.0. 

Problem 5. (4 points) Which park has the highest total number of human-caused mortalities `mort_all`?


```r
wolves %>% 
  select(park,pack,mort_all) %>% 
  arrange(desc(mort_all))
```

```
##     park                          pack mort_all
## 1   YUCH                       70 Mile       24
## 2   YUCH                    Cottonwood       14
## 3   YUCH                    Lost Creek       12
## 4   YUCH                   Sheep Bluff       12
## 5   YUCH                    Yukon Fork       10
## 6   YUCH                    Lost Creek        8
## 7   YUCH               Copper Mountain        5
## 8   YUCH               Hard Luck Creek        5
## 9   DENA               McKinley River1        4
## 10  GNTP               Phantom Springs        4
## 11  GNTP                   Huckleberry        4
## 12   YNP                    cottonwood        4
## 13  YUCH                       70 Mile        4
## 14  YUCH                Webber Creek 2        4
## 15  YUCH                   Woodchopper        4
## 16  GNTP             Lower Gros Ventre        3
## 17   YNP                         8mile        3
## 18   YNP                        canyon        3
## 19   YNP                      junction        3
## 20   YNP                      junction        3
## 21   YNP                     yelldelta        3
## 22  YUCH                    Cottonwood        3
## 23  DENA                 Birch Creek N        2
## 24  DENA                   Eagle Gorge        2
## 25  DENA                     East Fork        2
## 26  DENA                       Foraker        2
## 27  DENA                  Headquarters        2
## 28  DENA                         Pinto        2
## 29  DENA                        Savage        2
## 30  DENA                       Savage1        2
## 31  GNTP                       Buffalo        2
## 32  GNTP               Phantom Springs        2
## 33  GNTP               Phantom Springs        2
## 34  GNTP                 Pinnacle Peak        2
## 35  GNTP                 Pinnacle Peak        2
## 36  GNTP                         Teton        2
## 37   VNP                     tomcodbay        2
## 38   VNP                   Sheep Ranch        2
## 39   YNP                     642Fgroup        2
## 40   YNP                         8mile        2
## 41   YNP                       bechler        2
## 42   YNP                         lamar        2
## 43   YNP                       phantom        2
## 44   YNP                       phantom        2
## 45   YNP                      prospect        2
## 46   YNP                        wapiti        2
## 47   YNP                         druid        2
## 48   YNP                      junction        2
## 49  YUCH                       70 Mile        2
## 50  YUCH                       70 Mile        2
## 51  YUCH               Copper Mountain        2
## 52  YUCH                    Cottonwood        2
## 53  YUCH                Fourth of July        2
## 54  YUCH                Lower Charley1        2
## 55  YUCH                Lower Charley2        2
## 56  YUCH              Washington Creek        2
## 57  YUCH                    Yukon Fork        2
## 58  DENA                      100 Mile        1
## 59  DENA                 Castle Rocks2        1
## 60  DENA                       Chitsia        1
## 61  DENA                  Death Valley        1
## 62  DENA                     East Fork        1
## 63  DENA                     East Fork        1
## 64  DENA                     East Fork        1
## 65  DENA                     East Fork        1
## 66  DENA                     East Fork        1
## 67  DENA                     East Fork        1
## 68  DENA                     East Fork        1
## 69  DENA                     East Fork        1
## 70  DENA                     East Fork        1
## 71  DENA                     Ewe Creek        1
## 72  DENA                   Grant Creek        1
## 73  DENA                   Grant Creek        1
## 74  DENA                   Grant Creek        1
## 75  DENA                    Hot Slough        1
## 76  DENA               Iron Creek West        1
## 77  DENA                   John Hansen        1
## 78  DENA               Kantishna River        1
## 79  DENA               McKinley River1        1
## 80  DENA                       McLeod2        1
## 81  DENA                   Mt Margaret        1
## 82  DENA                   Mt Margaret        1
## 83  DENA                   Mt Margaret        1
## 84  DENA                   Muddy River        1
## 85  DENA                        Myrtle        1
## 86  DENA                  Nenana River        1
## 87  DENA                   Pinto Creek        1
## 88  DENA                   Pinto Creek        1
## 89  DENA                   Riley Creek        1
## 90  DENA                       Savage1        1
## 91  DENA                        Somber        1
## 92  DENA                      Stampede        1
## 93  DENA                      Stampede        1
## 94  DENA                    Starr Lake        1
## 95  DENA                  Straightaway        1
## 96  DENA                       Tonzona        1
## 97  DENA                     East Fork        1
## 98  DENA                   Hauke Creek        1
## 99  DENA                  Headquarters        1
## 100 DENA                   Little Bear        1
## 101 DENA                     Sanctuary        1
## 102 GNTP                      Antelope        1
## 103 GNTP                       Buffalo        1
## 104 GNTP                       Buffalo        1
## 105 GNTP                       Buffalo        1
## 106 GNTP                    Flat Creek        1
## 107 GNTP             Lower Gros Ventre        1
## 108 GNTP             Lower Gros Ventre        1
## 109 GNTP               Phantom Springs        1
## 110 GNTP               Phantom Springs        1
## 111 GNTP               Phantom Springs        1
## 112 GNTP                 Pinnacle Peak        1
## 113 GNTP                 Pinnacle Peak        1
## 114 GNTP                         Teton        1
## 115 GNTP                         Teton        1
## 116 GNTP                         Teton        1
## 117  VNP                     tomcodbay        1
## 118  VNP                   cruiserlake        1
## 119  VNP                   Moose River        1
## 120  VNP                   Moose River        1
## 121  VNP                   Sheep Ranch        1
## 122  VNP                    Bowman Bay        1
## 123  VNP                    Fawn Creek        1
## 124  YNP                    1118Fgroup        1
## 125  YNP                    1155Mgroup        1
## 126  YNP                         8mile        1
## 127  YNP                     963Fgroup        1
## 128  YNP                     blacktail        1
## 129  YNP                        cougar        1
## 130  YNP                        cougar        1
## 131  YNP                      junction        1
## 132  YNP                         lamar        1
## 133  YNP                       mollies        1
## 134  YNP                       mollies        1
## 135  YNP                       phantom        1
## 136  YNP                      prospect        1
## 137  YNP                         snake        1
## 138  YNP                         snake        1
## 139  YNP                         agate        1
## 140  YNP                     blacktail        1
## 141  YNP                     blacktail        1
## 142  YNP                        canyon        1
## 143  YNP                        cougar        1
## 144  YNP                         druid        1
## 145  YNP                   gibbon/mary        1
## 146  YNP                   gibbon/mary        1
## 147  YNP                      grayling        1
## 148  YNP                       mollies        1
## 149  YNP                       mollies        1
## 150  YNP                       mollies        1
## 151  YNP                      nezperce        1
## 152  YNP                      nezperce        1
## 153  YNP                      nezperce        1
## 154  YNP                          rose        1
## 155  YNP                          rose        1
## 156  YNP                        slough        1
## 157 YUCH                    Cottonwood        1
## 158 YUCH               Crescent Creek2        1
## 159 YUCH                Edwards Creek1        1
## 160 YUCH                Edwards Creek2        1
## 161 YUCH                Edwards Creek2        1
## 162 YUCH                  Fisher Creek        1
## 163 YUCH                    Flat Creek        1
## 164 YUCH                   Hanna Creek        1
## 165 YUCH                Step Mountain2        1
## 166 YUCH                Step Mountain2        1
## 167 YUCH                  Three Finger        1
## 168 YUCH                  Three Finger        1
## 169 YUCH                  Three Finger        1
## 170 DENA                      100 Mile        0
## 171 DENA                      100 Mile        0
## 172 DENA                      100 Mile        0
## 173 DENA                      100 Mile        0
## 174 DENA                      100 Mile        0
## 175 DENA                      100 Mile        0
## 176 DENA                      100 Mile        0
## 177 DENA                       Bearpaw        0
## 178 DENA                       Bearpaw        0
## 179 DENA                       Bearpaw        0
## 180 DENA                       Bearpaw        0
## 181 DENA                       Bearpaw        0
## 182 DENA                       Bearpaw        0
## 183 DENA                       Bearpaw        0
## 184 DENA                       Bearpaw        0
## 185 DENA                       Bearpaw        0
## 186 DENA                       Bearpaw        0
## 187 DENA                       Bearpaw        0
## 188 DENA                       Bearpaw        0
## 189 DENA                       Bearpaw        0
## 190 DENA                       Bearpaw        0
## 191 DENA                      Bearpaw1        0
## 192 DENA                   Beaver Fork        0
## 193 DENA                   Beaver Fork        0
## 194 DENA                   Beaver Fork        0
## 195 DENA                   Beaver Fork        0
## 196 DENA                   Birch Creek        0
## 197 DENA                   Birch Creek        0
## 198 DENA                   Birch Creek        0
## 199 DENA                   Birch Hills        0
## 200 DENA                   Birch Hills        0
## 201 DENA                     Boot Lake        0
## 202 DENA                     Boot Lake        0
## 203 DENA                       Brooker        0
## 204 DENA                       Brooker        0
## 205 DENA                 Caribou Creek        0
## 206 DENA                 Castle Rocks2        0
## 207 DENA                 Castle Rocks3        0
## 208 DENA                 Castle Rocks3        0
## 209 DENA                 Castle Rocks3        0
## 210 DENA                 Chilchukabena        0
## 211 DENA                       Chitsia        0
## 212 DENA                       Chitsia        0
## 213 DENA                       Chitsia        0
## 214 DENA                       Chitsia        0
## 215 DENA              Chitsia Mountain        0
## 216 DENA              Chitsia Mountain        0
## 217 DENA              Chitsia Mountain        0
## 218 DENA              Chitsia Mountain        0
## 219 DENA              Chitsia Mountain        0
## 220 DENA              Chitsia Mountain        0
## 221 DENA                    Clearwater        0
## 222 DENA                    Clearwater        0
## 223 DENA                    Clearwater        0
## 224 DENA                    Clearwater        0
## 225 DENA                   Corner Lake        0
## 226 DENA                   Corner Lake        0
## 227 DENA                   Corner Lake        0
## 228 DENA                   Corner Lake        0
## 229 DENA                  Death Valley        0
## 230 DENA                  Death Valley        0
## 231 DENA                  Death Valley        0
## 232 DENA                   Eagle Gorge        0
## 233 DENA                     East Fork        0
## 234 DENA                     East Fork        0
## 235 DENA                     East Fork        0
## 236 DENA                     East Fork        0
## 237 DENA                     East Fork        0
## 238 DENA                     East Fork        0
## 239 DENA                     East Fork        0
## 240 DENA                     East Fork        0
## 241 DENA                     East Fork        0
## 242 DENA                     East Fork        0
## 243 DENA                     East Fork        0
## 244 DENA                     East Fork        0
## 245 DENA                     East Fork        0
## 246 DENA                     East Fork        0
## 247 DENA                     East Fork        0
## 248 DENA                     East Fork        0
## 249 DENA                     East Fork        0
## 250 DENA                     East Fork        0
## 251 DENA                     East Fork        0
## 252 DENA                     East Fork        0
## 253 DENA                     Ewe Creek        0
## 254 DENA                       Foraker        0
## 255 DENA                       Foraker        0
## 256 DENA                       Foraker        0
## 257 DENA                       Foraker        0
## 258 DENA                       Foraker        0
## 259 DENA                       Foraker        0
## 260 DENA                       Foraker        0
## 261 DENA                       Foraker        0
## 262 DENA                       Foraker        0
## 263 DENA                       Foraker        0
## 264 DENA                       Foraker        0
## 265 DENA                       Foraker        0
## 266 DENA                   Grant Creek        0
## 267 DENA                   Grant Creek        0
## 268 DENA                   Grant Creek        0
## 269 DENA                   Grant Creek        0
## 270 DENA                   Grant Creek        0
## 271 DENA                   Grant Creek        0
## 272 DENA                   Grant Creek        0
## 273 DENA                   Grant Creek        0
## 274 DENA                   Grant Creek        0
## 275 DENA                   Grant Creek        0
## 276 DENA                   Grant Creek        0
## 277 DENA                   Grant Creek        0
## 278 DENA                   Grant Creek        0
## 279 DENA                   Hauke Creek        0
## 280 DENA                  Headquarters        0
## 281 DENA                  Headquarters        0
## 282 DENA                  Headquarters        0
## 283 DENA                  Headquarters        0
## 284 DENA                  Headquarters        0
## 285 DENA                  Headquarters        0
## 286 DENA                  Headquarters        0
## 287 DENA                        Herron        0
## 288 DENA                        Herron        0
## 289 DENA                        Herron        0
## 290 DENA                     Highpower        0
## 291 DENA                     Highpower        0
## 292 DENA                     Highpower        0
## 293 DENA                     Highpower        0
## 294 DENA                    Hot Slough        0
## 295 DENA                    Hot Slough        0
## 296 DENA                    Hot Slough        0
## 297 DENA                    Hot Slough        0
## 298 DENA                    Hot Slough        0
## 299 DENA                    Hot Slough        0
## 300 DENA                    Hot Slough        0
## 301 DENA                    Hot Slough        0
## 302 DENA                    Iron Creek        0
## 303 DENA                    Iron Creek        0
## 304 DENA               Iron Creek East        0
## 305 DENA               Iron Creek East        0
## 306 DENA               Iron Creek West        0
## 307 DENA               Iron Creek West        0
## 308 DENA               Iron Creek West        0
## 309 DENA               Iron Creek West        0
## 310 DENA                   Jenny Creek        0
## 311 DENA                   John Hansen        0
## 312 DENA                   John Hansen        0
## 313 DENA                   John Hansen        0
## 314 DENA                   John Hansen        0
## 315 DENA               Kantishna River        0
## 316 DENA               Kantishna River        0
## 317 DENA               Kantishna River        0
## 318 DENA               Kantishna River        0
## 319 DENA               Kantishna River        0
## 320 DENA               Kantishna River        0
## 321 DENA               Kantishna River        0
## 322 DENA               Kantishna River        0
## 323 DENA               Kantishna River        0
## 324 DENA               Kantishna River        0
## 325 DENA                   Little Bear        0
## 326 DENA                   Little Bear        0
## 327 DENA                   Little Bear        0
## 328 DENA                   Little Bear        0
## 329 DENA                   Little Bear        0
## 330 DENA                   Little Bear        0
## 331 DENA                McKinley River        0
## 332 DENA                McKinley River        0
## 333 DENA                McKinley River        0
## 334 DENA                McKinley River        0
## 335 DENA               McKinley River1        0
## 336 DENA               McKinley River1        0
## 337 DENA               McKinley River1        0
## 338 DENA               McKinley Slough        0
## 339 DENA               McKinley Slough        0
## 340 DENA               McKinley Slough        0
## 341 DENA               McKinley Slough        0
## 342 DENA               McKinley Slough        0
## 343 DENA               McKinley Slough        0
## 344 DENA               McKinley Slough        0
## 345 DENA               McKinley Slough        0
## 346 DENA               McKinley Slough        0
## 347 DENA               McKinley Slough        0
## 348 DENA               McKinley Slough        0
## 349 DENA               McKinley Slough        0
## 350 DENA               McKinley Slough        0
## 351 DENA               McKinley Slough        0
## 352 DENA               McKinley Slough        0
## 353 DENA               McKinley Slough        0
## 354 DENA               McKinley Slough        0
## 355 DENA                        McLeod        0
## 356 DENA                        McLeod        0
## 357 DENA                        McLeod        0
## 358 DENA                        McLeod        0
## 359 DENA                        McLeod        0
## 360 DENA                        McLeod        0
## 361 DENA                        McLeod        0
## 362 DENA                        McLeod        0
## 363 DENA                        McLeod        0
## 364 DENA                        McLeod        0
## 365 DENA                        McLeod        0
## 366 DENA                   McLeod West        0
## 367 DENA                   Moose Creek        0
## 368 DENA                   Moose Creek        0
## 369 DENA                   Mt Margaret        0
## 370 DENA                   Mt Margaret        0
## 371 DENA                   Mt Margaret        0
## 372 DENA                   Mt Margaret        0
## 373 DENA                   Mt Margaret        0
## 374 DENA                   Mt Margaret        0
## 375 DENA                   Mt Margaret        0
## 376 DENA                   Muddy River        0
## 377 DENA                   Muddy River        0
## 378 DENA                   Muddy River        0
## 379 DENA                        Myrtle        0
## 380 DENA                  Nenana River        0
## 381 DENA                  Nenana River        0
## 382 DENA                  Nenana River        0
## 383 DENA                  Nenana River        0
## 384 DENA                  Nenana River        0
## 385 DENA                    North Fork        0
## 386 DENA                    North Fork        0
## 387 DENA                    North Fork        0
## 388 DENA                   Otter Creek        0
## 389 DENA                   Otter Creek        0
## 390 DENA                    Otter Lake        0
## 391 DENA                         Pinto        0
## 392 DENA                   Pinto Creek        0
## 393 DENA                   Pinto Creek        0
## 394 DENA                   Pinto Creek        0
## 395 DENA                   Pinto Creek        0
## 396 DENA                  Pirate Creek        0
## 397 DENA                   Riley Creek        0
## 398 DENA                   Riley Creek        0
## 399 DENA                   Riley Creek        0
## 400 DENA              Riley Creek West        0
## 401 DENA                     Sanctuary        0
## 402 DENA                     Sanctuary        0
## 403 DENA                     Sanctuary        0
## 404 DENA                     Sanctuary        0
## 405 DENA                     Sanctuary        0
## 406 DENA                 Sandless Lake        0
## 407 DENA                        Savage        0
## 408 DENA                       Savage1        0
## 409 DENA                Slippery Creek        0
## 410 DENA                Slippery Creek        0
## 411 DENA                        Somber        0
## 412 DENA                        Somber        0
## 413 DENA                        Somber        0
## 414 DENA                        Somber        0
## 415 DENA                        Somber        0
## 416 DENA                        Somber        0
## 417 DENA                        Somber        0
## 418 DENA                      Stampede        0
## 419 DENA                      Stampede        0
## 420 DENA                      Stampede        0
## 421 DENA                      Stampede        0
## 422 DENA                      Stampede        0
## 423 DENA                      Stampede        0
## 424 DENA                      Stampede        0
## 425 DENA                      Stampede        0
## 426 DENA                      Stampede        0
## 427 DENA                      Stampede        0
## 428 DENA                      Stampede        0
## 429 DENA                    Starr Lake        0
## 430 DENA                    Starr Lake        0
## 431 DENA                    Starr Lake        0
## 432 DENA                    Starr Lake        0
## 433 DENA                    Starr Lake        0
## 434 DENA                    Starr Lake        0
## 435 DENA                    Starr Lake        0
## 436 DENA                    Starr Lake        0
## 437 DENA                    Starr Lake        0
## 438 DENA                         Stony        0
## 439 DENA                         Stony        0
## 440 DENA                         Stony        0
## 441 DENA                         Stony        0
## 442 DENA                         Stony        0
## 443 DENA                  Straightaway        0
## 444 DENA                  Straightaway        0
## 445 DENA                  Straightaway        0
## 446 DENA                  Straightaway        0
## 447 DENA                  Turtle Hill         0
## 448 DENA                  Turtle Hill         0
## 449 DENA                  Turtle Hill1        0
## 450 DENA                  Turtle Hill1        0
## 451 DENA                   White Creek        0
## 452 DENA                   White Creek        0
## 453 DENA                   White Creek        0
## 454 DENA                   White Creek        0
## 455 DENA                   Windy Creek        0
## 456 DENA                   Windy Creek        0
## 457 GNTP                      Antelope        0
## 458 GNTP                      Antelope        0
## 459 GNTP                       Buffalo        0
## 460 GNTP       Horsetail Creek (Murie)        0
## 461 GNTP       Horsetail Creek (Murie)        0
## 462 GNTP                   Huckleberry        0
## 463 GNTP                   Huckleberry        0
## 464 GNTP                   Huckleberry        0
## 465 GNTP                   Huckleberry        0
## 466 GNTP                   Huckleberry        0
## 467 GNTP                   Huckleberry        0
## 468 GNTP                   Huckleberry        0
## 469 GNTP                   Huckleberry        0
## 470 GNTP                   Huckleberry        0
## 471 GNTP                   Huckleberry        0
## 472 GNTP                   Huckleberry        0
## 473 GNTP                   Huckleberry        0
## 474 GNTP                   Huckleberry        0
## 475 GNTP             Lower Gros Ventre        0
## 476 GNTP             Lower Gros Ventre        0
## 477 GNTP             Lower Gros Ventre        0
## 478 GNTP             Lower Gros Ventre        0
## 479 GNTP             Lower Gros Ventre        0
## 480 GNTP             Lower Gros Ventre        0
## 481 GNTP             Lower Gros Ventre        0
## 482 GNTP Lower Slide Lake (781M Group)        0
## 483 GNTP Lower Slide Lake (781M Group)        0
## 484 GNTP Lower Slide Lake (781M Group)        0
## 485 GNTP                 Pacific Creek        0
## 486 GNTP                 Pacific Creek        0
## 487 GNTP                 Pacific Creek        0
## 488 GNTP                 Pacific Creek        0
## 489 GNTP                 Pacific Creek        0
## 490 GNTP                 Pacific Creek        0
## 491 GNTP               Phantom Springs        0
## 492 GNTP               Phantom Springs        0
## 493 GNTP               Phantom Springs        0
## 494 GNTP               Phantom Springs        0
## 495 GNTP                 Pinnacle Peak        0
## 496 GNTP                 Pinnacle Peak        0
## 497 GNTP                 Pinnacle Peak        0
## 498 GNTP                 Pinnacle Peak        0
## 499 GNTP                 Pinnacle Peak        0
## 500 GNTP                 Pinnacle Peak        0
## 501 GNTP                 Pinnacle Peak        0
## 502 GNTP                 Pinnacle Peak        0
## 503 GNTP                 Pinnacle Peak        0
## 504 GNTP                         Teton        0
## 505 GNTP                         Teton        0
## 506 GNTP                         Teton        0
## 507 GNTP                         Teton        0
## 508 GNTP                         Teton        0
## 509 GNTP                 Wildcat Ridge        0
## 510  VNP                   locatorlake        0
## 511  VNP               middlepeninsula        0
## 512  VNP               middlepeninsula        0
## 513  VNP                   cruiserlake        0
## 514  VNP               mooserivergrade        0
## 515  VNP                     brownsbay        0
## 516  VNP                     brownsbay        0
## 517  VNP                   nebraskabay        0
## 518  VNP                   nebraskabay        0
## 519  VNP                     bluemoose        0
## 520  VNP                       ratroot        0
## 521  VNP                     Ash River        0
## 522  VNP                     Ash River        0
## 523  VNP                    Bowman Bay        0
## 524  VNP                    Sand Point        0
## 525  VNP                     Ash River        0
## 526  VNP                    Bowman Bay        0
## 527  VNP                    Sand Point        0
## 528  VNP                     Ash River        0
## 529  VNP                    Bowman Bay        0
## 530  VNP                     Lightfoot        0
## 531  VNP                   Moose River        0
## 532  VNP                    Sand Point        0
## 533  VNP                    Bowman Bay        0
## 534  VNP                    Fawn Creek        0
## 535  VNP                     Lightfoot        0
## 536  VNP                   Moose River        0
## 537  VNP                    Sand Point        0
## 538  VNP                   Sheep Ranch        0
## 539  VNP                 Cranberry Bay        0
## 540  VNP                       Nashata        0
## 541  VNP                      Paradise        0
## 542  VNP                    Sand Point        0
## 543  VNP                 Shoepack Lake        0
## 544  VNP                     Half-Moon        0
## 545  VNP                       Nashata        0
## 546  VNP                      Paradise        0
## 547  VNP                    Sand Point        0
## 548  VNP                      Windsong        0
## 549  YNP                     682Mgroup        0
## 550  YNP                     682Mgroup        0
## 551  YNP                     694Fgroup        0
## 552  YNP                     755Mgroup        0
## 553  YNP                         8mile        0
## 554  YNP                         8mile        0
## 555  YNP                         8mile        0
## 556  YNP                         8mile        0
## 557  YNP                         8mile        0
## 558  YNP                         8mile        0
## 559  YNP                         agate        0
## 560  YNP                         agate        0
## 561  YNP                         agate        0
## 562  YNP                         agate        0
## 563  YNP                         agate        0
## 564  YNP                         agate        0
## 565  YNP                         agate        0
## 566  YNP                         agate        0
## 567  YNP                         agate        0
## 568  YNP                         agate        0
## 569  YNP                       bechler        0
## 570  YNP                       biscuit        0
## 571  YNP                     blacktail        0
## 572  YNP                     blacktail        0
## 573  YNP                   buffalofork        0
## 574  YNP                   buffalofork        0
## 575  YNP                        canyon        0
## 576  YNP                        canyon        0
## 577  YNP                        canyon        0
## 578  YNP                        canyon        0
## 579  YNP                        canyon        0
## 580  YNP                        canyon        0
## 581  YNP                        canyon        0
## 582  YNP                        canyon        0
## 583  YNP                     carnelian        0
## 584  YNP                      cinnabar        0
## 585  YNP                    cottonwood        0
## 586  YNP                        cougar        0
## 587  YNP                        cougar        0
## 588  YNP                        cougar        0
## 589  YNP                        cougar        0
## 590  YNP                        cougar        0
## 591  YNP                        cougar        0
## 592  YNP                        cougar        0
## 593  YNP                        cougar        0
## 594  YNP                        cougar        0
## 595  YNP                        cougar        0
## 596  YNP                        cougar        0
## 597  YNP                        cougar        0
## 598  YNP                        cougar        0
## 599  YNP                        cougar        0
## 600  YNP                        cougar        0
## 601  YNP                        cougar        0
## 602  YNP                        cougar        0
## 603  YNP                       crevice        0
## 604  YNP                         druid        0
## 605  YNP                         druid        0
## 606  YNP                         druid        0
## 607  YNP                         druid        0
## 608  YNP                         druid        0
## 609  YNP                         druid        0
## 610  YNP                         druid        0
## 611  YNP                         druid        0
## 612  YNP                         druid        0
## 613  YNP                         druid        0
## 614  YNP                         druid        0
## 615  YNP                         druid        0
## 616  YNP                         druid        0
## 617  YNP                        everts        0
## 618  YNP                        everts        0
## 619  YNP                    geode/hell        0
## 620  YNP                    geode/hell        0
## 621  YNP                    geode/hell        0
## 622  YNP                    geode/hell        0
## 623  YNP                   gibbon/mary        0
## 624  YNP                   gibbon/mary        0
## 625  YNP                   gibbon/mary        0
## 626  YNP                   gibbon/mary        0
## 627  YNP                   gibbon/mary        0
## 628  YNP                   gibbon/mary        0
## 629  YNP                   gibbon/mary        0
## 630  YNP                   gibbon/mary        0
## 631  YNP                      grayling        0
## 632  YNP                        hayden        0
## 633  YNP                        hayden        0
## 634  YNP                        hayden        0
## 635  YNP                        hayden        0
## 636  YNP                         heart        0
## 637  YNP                         heart        0
## 638  YNP                      junction        0
## 639  YNP                      junction        0
## 640  YNP                      junction        0
## 641  YNP                      junction        0
## 642  YNP                         lamar        0
## 643  YNP                         lamar        0
## 644  YNP                         lamar        0
## 645  YNP                         lamar        0
## 646  YNP                         lamar        0
## 647  YNP                         lamar        0
## 648  YNP                          lava        0
## 649  YNP                          lava        0
## 650  YNP                          lava        0
## 651  YNP                       leopold        0
## 652  YNP                       leopold        0
## 653  YNP                       leopold        0
## 654  YNP                       leopold        0
## 655  YNP                       leopold        0
## 656  YNP                       leopold        0
## 657  YNP                       leopold        0
## 658  YNP                       leopold        0
## 659  YNP                       leopold        0
## 660  YNP                       leopold        0
## 661  YNP                       leopold        0
## 662  YNP                       leopold        0
## 663  YNP                       leopold        0
## 664  YNP                      lonestar        0
## 665  YNP                       mollies        0
## 666  YNP                       mollies        0
## 667  YNP                       mollies        0
## 668  YNP                       mollies        0
## 669  YNP                       mollies        0
## 670  YNP                       mollies        0
## 671  YNP                       mollies        0
## 672  YNP                       mollies        0
## 673  YNP                       mollies        0
## 674  YNP                       mollies        0
## 675  YNP                       mollies        0
## 676  YNP                       mollies        0
## 677  YNP                       mollies        0
## 678  YNP                       mollies        0
## 679  YNP                       mollies        0
## 680  YNP                       mollies        0
## 681  YNP                       mollies        0
## 682  YNP                       mollies        0
## 683  YNP                       mollies        0
## 684  YNP                       mollies        0
## 685  YNP                       mollies        0
## 686  YNP                      nezperce        0
## 687  YNP                      nezperce        0
## 688  YNP                      nezperce        0
## 689  YNP                      nezperce        0
## 690  YNP                      nezperce        0
## 691  YNP                      nezperce        0
## 692  YNP                      nezperce        0
## 693  YNP                      nezperce        0
## 694  YNP                         oxbow        0
## 695  YNP                         oxbow        0
## 696  YNP                         oxbow        0
## 697  YNP                      prospect        0
## 698  YNP                      prospect        0
## 699  YNP                      quadrant        0
## 700  YNP                      quadrant        0
## 701  YNP                      quadrant        0
## 702  YNP                      quadrant        0
## 703  YNP                          rose        0
## 704  YNP                          rose        0
## 705  YNP                          rose        0
## 706  YNP                          rose        0
## 707  YNP                          rose        0
## 708  YNP                          rose        0
## 709  YNP                        slough        0
## 710  YNP                        slough        0
## 711  YNP                        slough        0
## 712  YNP                        slough        0
## 713  YNP               specimen/silver        0
## 714  YNP               specimen/silver        0
## 715  YNP                          swan        0
## 716  YNP                          swan        0
## 717  YNP                          swan        0
## 718  YNP                          swan        0
## 719  YNP                          swan        0
## 720  YNP                          swan        0
## 721  YNP                     thorofare        0
## 722  YNP                     thorofare        0
## 723  YNP                     thorofare        0
## 724  YNP                         tower        0
## 725  YNP                         tower        0
## 726  YNP                        wapiti        0
## 727  YNP                        wapiti        0
## 728  YNP                        wapiti        0
## 729  YNP                        wapiti        0
## 730  YNP                        wapiti        0
## 731  YNP                     yelldelta        0
## 732  YNP                     yelldelta        0
## 733  YNP                     yelldelta        0
## 734  YNP                     yelldelta        0
## 735  YNP                     yelldelta        0
## 736  YNP                     yelldelta        0
## 737  YNP                     yelldelta        0
## 738  YNP                     yelldelta        0
## 739  YNP                     yelldelta        0
## 740  YNP                     yelldelta        0
## 741  YNP                     yelldelta        0
## 742  YNP                     yelldelta        0
## 743  YNP                     yelldelta        0
## 744  YNP                     yelldelta        0
## 745  YNP                     yelldelta        0
## 746  YNP                     yelldelta        0
## 747  YNP                     yelldelta        0
## 748 YUCH                       70 Mile        0
## 749 YUCH                       70 Mile        0
## 750 YUCH                       70 Mile        0
## 751 YUCH                       70 Mile        0
## 752 YUCH                       70 Mile        0
## 753 YUCH                       70 Mile        0
## 754 YUCH                       70 Mile        0
## 755 YUCH                       70 Mile        0
## 756 YUCH                       70 Mile        0
## 757 YUCH             Andrew Creek Pair        0
## 758 YUCH              Birch Creek Pair        0
## 759 YUCH               Copper Mountain        0
## 760 YUCH               Copper Mountain        0
## 761 YUCH                    Cottonwood        0
## 762 YUCH                    Cottonwood        0
## 763 YUCH                    Cottonwood        0
## 764 YUCH                    Cottonwood        0
## 765 YUCH                    Cottonwood        0
## 766 YUCH                    Cottonwood        0
## 767 YUCH                    Cottonwood        0
## 768 YUCH                    Cottonwood        0
## 769 YUCH                    Cottonwood        0
## 770 YUCH          Crescent Creek1 Pair        0
## 771 YUCH               Crescent Creek2        0
## 772 YUCH                Edwards Creek1        0
## 773 YUCH                Edwards Creek1        0
## 774 YUCH                Edwards Creek1        0
## 775 YUCH                Edwards Creek2        0
## 776 YUCH                Edwards Creek2        0
## 777 YUCH                Edwards Creek2        0
## 778 YUCH                Edwards Creek2        0
## 779 YUCH                Edwards Creek2        0
## 780 YUCH                Edwards Creek2        0
## 781 YUCH                  Fisher Creek        0
## 782 YUCH                  Fisher Creek        0
## 783 YUCH                  Fisher Creek        0
## 784 YUCH                  Fisher Creek        0
## 785 YUCH                  Fisher Creek        0
## 786 YUCH                    Flat Creek        0
## 787 YUCH                    Flat Creek        0
## 788 YUCH                    Flat Creek        0
## 789 YUCH                   Godge Creek        0
## 790 YUCH                   Godge Creek        0
## 791 YUCH                   Godge Creek        0
## 792 YUCH                   Godge Creek        0
## 793 YUCH                   Godge Creek        0
## 794 YUCH               Hard Luck Creek        0
## 795 YUCH               Hard Luck Creek        0
## 796 YUCH               Hard Luck Creek        0
## 797 YUCH               Hard Luck Creek        0
## 798 YUCH               Hard Luck Creek        0
## 799 YUCH               Kathul Mountain        0
## 800 YUCH                    Lost Creek        0
## 801 YUCH                    Lost Creek        0
## 802 YUCH                    Lost Creek        0
## 803 YUCH                    Lost Creek        0
## 804 YUCH                    Lost Creek        0
## 805 YUCH                Lower Charley1        0
## 806 YUCH                Lower Charley1        0
## 807 YUCH                Lower Charley1        0
## 808 YUCH                Lower Charley1        0
## 809 YUCH                Lower Charley1        0
## 810 YUCH                Lower Charley2        0
## 811 YUCH                Lower Charley2        0
## 812 YUCH                Lower Charley2        0
## 813 YUCH                Lower Charley2        0
## 814 YUCH                Lower Charley2        0
## 815 YUCH                  Lower Kandik        0
## 816 YUCH                  Lower Kandik        0
## 817 YUCH                  Lower Kandik        0
## 818 YUCH                  Lower Kandik        0
## 819 YUCH                  Nation River        0
## 820 YUCH                  Nation River        0
## 821 YUCH                  Nation River        0
## 822 YUCH                  Nation River        0
## 823 YUCH                 Poverty Creek        0
## 824 YUCH                 Poverty Creek        0
## 825 YUCH                    Rock Creek        0
## 826 YUCH                   Sheep Bluff        0
## 827 YUCH                   Sheep Bluff        0
## 828 YUCH                   Snowy Peak1        0
## 829 YUCH                   Snowy Peak2        0
## 830 YUCH                Step Mountain2        0
## 831 YUCH                Step Mountain2        0
## 832 YUCH                Step Mountain2        0
## 833 YUCH                Step Mountain2        0
## 834 YUCH                Step Mountain2        0
## 835 YUCH                Step Mountain2        0
## 836 YUCH                Step Mountain2        0
## 837 YUCH                Step Mountain2        0
## 838 YUCH                Step Mountain2        0
## 839 YUCH                Step Mountain2        0
## 840 YUCH                Step Mountain2        0
## 841 YUCH                Step Mountain2        0
## 842 YUCH                Sterling Creek        0
## 843 YUCH                  Three Finger        0
## 844 YUCH                  Three Finger        0
## 845 YUCH                  Three Finger        0
## 846 YUCH                  Three Finger        0
## 847 YUCH                  Three Finger        0
## 848 YUCH                  Three Finger        0
## 849 YUCH                  Three Finger        0
## 850 YUCH                  Three Finger        0
## 851 YUCH        Upper Black River Pair        0
## 852 YUCH        Upper Black River Pair        0
## 853 YUCH        Upper Black River Pair        0
## 854 YUCH                  Upper Kandik        0
## 855 YUCH                  Upper Kandik        0
## 856 YUCH              Washington Creek        0
## 857 YUCH              Washington Creek        0
## 858 YUCH                 Webber Creek1        0
## 859 YUCH                 Webber Creek1        0
## 860 YUCH                 Webber Creek1        0
## 861 YUCH                   Woodchopper        0
## 862 YUCH                   Woodchopper        0
## 863 YUCH                    Yukon Fork        0
## 864 YUCH                    Yukon Fork        0
```
The YUCH park has the highest number of human-caused mortalities, at 24 mortalities

The wolves in [Yellowstone National Park](https://www.nps.gov/yell/learn/nature/wolf-restoration.htm) are an incredible conservation success story. Let's focus our attention on this park.  

Problem 6. (2 points) Create a new object "ynp" that only includes the data from Yellowstone National Park. 

```r
names(wolves)
```

```
##  [1] "park"         "biolyr"       "pack"         "packcode"     "packsize_aug"
##  [6] "mort_yn"      "mort_all"     "mort_lead"    "mort_nonlead" "reprody1"    
## [11] "persisty1"
```



```r
ynp <- select(wolves, park, mort_all, biolyr, mort_lead, pack, mort_nonlead,packcode,reprody1,packsize_aug,persisty1,mort_yn) %>% 
  filter(park=="YNP")

ynp
```

```
##     park mort_all biolyr mort_lead            pack mort_nonlead packcode
## 1    YNP        4   2009         1      cottonwood            3       23
## 2    YNP        3   2016         0           8mile            3       11
## 3    YNP        3   2017         3          canyon            0       20
## 4    YNP        3   2012         0        junction            3       33
## 5    YNP        3   2016         0        junction            3       33
## 6    YNP        2   2011         1       642Fgroup            1        5
## 7    YNP        2   2012         0           8mile            2       11
## 8    YNP        2   2012         0         bechler            2       16
## 9    YNP        2   2012         1           lamar            1       34
## 10   YNP        2   2019         0         phantom            2       41
## 11   YNP        2   2020         0         phantom            2       41
## 12   YNP        2   2017         1        prospect            1       42
## 13   YNP        2   2020         0          wapiti            2       51
## 14   YNP        1   2018         0      1118Fgroup            1        2
## 15   YNP        1   2020         1      1155Mgroup            0        3
## 16   YNP        1   2014         0           8mile            1       11
## 17   YNP        1   2017         1       963Fgroup            0       13
## 18   YNP        1   2012         0       blacktail            1       18
## 19   YNP        1   2014         1          cougar            0       24
## 20   YNP        1   2017         0          cougar            1       24
## 21   YNP        1   2014         0        junction            1       33
## 22   YNP        1   2018         0           lamar            1       34
## 23   YNP        1   2012         0         mollies            1       38
## 24   YNP        1   2019         0         mollies            1       38
## 25   YNP        1   2018         1         phantom            0       41
## 26   YNP        1   2014         0        prospect            1       42
## 27   YNP        1   2012         1           snake            0       46
## 28   YNP        1   2017        NA           snake           NA       46
## 29   YNP        3   2007         0       yelldelta            3       52
## 30   YNP        2   1997         1           druid            1       26
## 31   YNP        2   2019         0        junction            2       33
## 32   YNP        1   2003         0           agate            1       15
## 33   YNP        1   2009         0       blacktail            1       18
## 34   YNP        1   2011         0       blacktail            1       18
## 35   YNP        1   2013         0          canyon            1       20
## 36   YNP        1   2015         1          cougar            0       24
## 37   YNP        1   2000         0           druid            1       26
## 38   YNP        1   2004         0     gibbon/mary            1       29
## 39   YNP        1   2009         0     gibbon/mary            1       29
## 40   YNP        1   2010         1        grayling            0       30
## 41   YNP        1   2007         0         mollies            1       38
## 42   YNP        1   2011         0         mollies            1       38
## 43   YNP        1   2013         0         mollies            1       38
## 44   YNP        1   1996         0        nezperce            1       39
## 45   YNP        1   1997         0        nezperce            1       39
## 46   YNP        1   2000         0        nezperce            1       39
## 47   YNP        1   1995         0            rose            1       44
## 48   YNP        1   1998         0            rose            1       44
## 49   YNP        1   2007         1          slough            0       45
## 50   YNP        0   2009         0       682Mgroup            0        6
## 51   YNP        0   2010         0       682Mgroup            0        6
## 52   YNP        0   2009         0       694Fgroup            0        8
## 53   YNP        0   2013         0       755Mgroup            0        9
## 54   YNP        0   2011         0           8mile            0       11
## 55   YNP        0   2013         0           8mile            0       11
## 56   YNP        0   2015         0           8mile            0       11
## 57   YNP        0   2017         0           8mile            0       11
## 58   YNP        0   2018         0           8mile            0       11
## 59   YNP        0   2020         0           8mile            0       11
## 60   YNP        0   2002         0           agate            0       15
## 61   YNP        0   2004         0           agate            0       15
## 62   YNP        0   2005         0           agate            0       15
## 63   YNP        0   2006         0           agate            0       15
## 64   YNP        0   2007         0           agate            0       15
## 65   YNP        0   2008         0           agate            0       15
## 66   YNP        0   2009         0           agate            0       15
## 67   YNP        0   2010         0           agate            0       15
## 68   YNP        0   2011         0           agate            0       15
## 69   YNP        0   2012         0           agate            0       15
## 70   YNP        0   2002         0         bechler            0       16
## 71   YNP        0   2004         0         biscuit            0       17
## 72   YNP        0   2008         0       blacktail            0       18
## 73   YNP        0   2010         0       blacktail            0       18
## 74   YNP        0   2002         0     buffalofork            0       19
## 75   YNP        0   2003         0     buffalofork            0       19
## 76   YNP        0   2008         0          canyon            0       20
## 77   YNP        0   2009         0          canyon            0       20
## 78   YNP        0   2010         0          canyon            0       20
## 79   YNP        0   2011         0          canyon            0       20
## 80   YNP        0   2012         0          canyon            0       20
## 81   YNP        0   2014         0          canyon            0       20
## 82   YNP        0   2015         0          canyon            0       20
## 83   YNP        0   2016         0          canyon            0       20
## 84   YNP        0   2021         0       carnelian            0       21
## 85   YNP        0   2016         0        cinnabar            0       22
## 86   YNP        0   2008         0      cottonwood            0       23
## 87   YNP        0   2001         0          cougar            0       24
## 88   YNP        0   2002         0          cougar            0       24
## 89   YNP        0   2003         0          cougar            0       24
## 90   YNP        0   2004         0          cougar            0       24
## 91   YNP        0   2005         0          cougar            0       24
## 92   YNP        0   2006         0          cougar            0       24
## 93   YNP        0   2007         0          cougar            0       24
## 94   YNP        0   2008         0          cougar            0       24
## 95   YNP        0   2009         0          cougar            0       24
## 96   YNP        0   2010         0          cougar            0       24
## 97   YNP        0   2011         0          cougar            0       24
## 98   YNP        0   2012         0          cougar            0       24
## 99   YNP        0   2013         0          cougar            0       24
## 100  YNP        0   2016         0          cougar            0       24
## 101  YNP        0   2018         0          cougar            0       24
## 102  YNP        0   2019         0          cougar            0       24
## 103  YNP        0   2020         0          cougar            0       24
## 104  YNP        0   2017         0         crevice            0       25
## 105  YNP        0   1996         0           druid            0       26
## 106  YNP        0   1998         0           druid            0       26
## 107  YNP        0   1999         0           druid            0       26
## 108  YNP        0   2001         0           druid            0       26
## 109  YNP        0   2002         0           druid            0       26
## 110  YNP        0   2003         0           druid            0       26
## 111  YNP        0   2004         0           druid            0       26
## 112  YNP        0   2005         0           druid            0       26
## 113  YNP        0   2006         0           druid            0       26
## 114  YNP        0   2007         0           druid            0       26
## 115  YNP        0   2008         0           druid            0       26
## 116  YNP        0   2009         0           druid            0       26
## 117  YNP        0   2010         0           druid            0       26
## 118  YNP        0   2008         0          everts            0       27
## 119  YNP        0   2009         0          everts            0       27
## 120  YNP        0   2002         0      geode/hell            0       28
## 121  YNP        0   2003         0      geode/hell            0       28
## 122  YNP        0   2004         0      geode/hell            0       28
## 123  YNP        0   2005         0      geode/hell            0       28
## 124  YNP        0   2003         0     gibbon/mary            0       29
## 125  YNP        0   2005         0     gibbon/mary            0       29
## 126  YNP        0   2006         0     gibbon/mary            0       29
## 127  YNP        0   2007         0     gibbon/mary            0       29
## 128  YNP        0   2008         0     gibbon/mary            0       29
## 129  YNP        0   2010         0     gibbon/mary            0       29
## 130  YNP        0   2011         0     gibbon/mary            0       29
## 131  YNP        0   2012         0     gibbon/mary            0       29
## 132  YNP        0   2009         0        grayling            0       30
## 133  YNP        0   2004         0          hayden            0       31
## 134  YNP        0   2005         0          hayden            0       31
## 135  YNP        0   2006         0          hayden            0       31
## 136  YNP        0   2007         0          hayden            0       31
## 137  YNP        0   2019         0           heart            0       32
## 138  YNP        0   2020         0           heart            0       32
## 139  YNP        0   2015         0        junction            0       33
## 140  YNP        0   2017         0        junction            0       33
## 141  YNP        0   2018         0        junction            0       33
## 142  YNP        0   2020         0        junction            0       33
## 143  YNP        0   2010         0           lamar            0       34
## 144  YNP        0   2011         0           lamar            0       34
## 145  YNP        0   2014         0           lamar            0       34
## 146  YNP        0   2015         0           lamar            0       34
## 147  YNP        0   2016         0           lamar            0       34
## 148  YNP        0   2017         0           lamar            0       34
## 149  YNP        0   2008         0            lava            0       35
## 150  YNP        0   2009         0            lava            0       35
## 151  YNP        0   2010         0            lava            0       35
## 152  YNP        0   1996         0         leopold            0       36
## 153  YNP        0   1997         0         leopold            0       36
## 154  YNP        0   1998         0         leopold            0       36
## 155  YNP        0   1999         0         leopold            0       36
## 156  YNP        0   2000         0         leopold            0       36
## 157  YNP        0   2001         0         leopold            0       36
## 158  YNP        0   2002         0         leopold            0       36
## 159  YNP        0   2003         0         leopold            0       36
## 160  YNP        0   2004         0         leopold            0       36
## 161  YNP        0   2005         0         leopold            0       36
## 162  YNP        0   2006         0         leopold            0       36
## 163  YNP        0   2007         0         leopold            0       36
## 164  YNP        0   2008         0         leopold            0       36
## 165  YNP        0   1996         0        lonestar            0       37
## 166  YNP        0   1995         0         mollies            0       38
## 167  YNP        0   1996         0         mollies            0       38
## 168  YNP        0   1997         0         mollies            0       38
## 169  YNP        0   1998         0         mollies            0       38
## 170  YNP        0   1999         0         mollies            0       38
## 171  YNP        0   2000         0         mollies            0       38
## 172  YNP        0   2001         0         mollies            0       38
## 173  YNP        0   2002         0         mollies            0       38
## 174  YNP        0   2003         0         mollies            0       38
## 175  YNP        0   2004         0         mollies            0       38
## 176  YNP        0   2005         0         mollies            0       38
## 177  YNP        0   2006         0         mollies            0       38
## 178  YNP        0   2008         0         mollies            0       38
## 179  YNP        0   2009         0         mollies            0       38
## 180  YNP        0   2010         0         mollies            0       38
## 181  YNP        0   2014         0         mollies            0       38
## 182  YNP        0   2015         0         mollies            0       38
## 183  YNP        0   2016         0         mollies            0       38
## 184  YNP        0   2017         0         mollies            0       38
## 185  YNP        0   2018         0         mollies            0       38
## 186  YNP        0   2020         0         mollies            0       38
## 187  YNP        0   2006         0        nezperce            0       39
## 188  YNP        0   1998         0        nezperce            0       39
## 189  YNP        0   1999         0        nezperce            0       39
## 190  YNP        0   2001         0        nezperce            0       39
## 191  YNP        0   2002         0        nezperce            0       39
## 192  YNP        0   2003         0        nezperce            0       39
## 193  YNP        0   2004         0        nezperce            0       39
## 194  YNP        0   2005         0        nezperce            0       39
## 195  YNP        0   2006         0           oxbow            0       40
## 196  YNP        0   2007         0           oxbow            0       40
## 197  YNP        0   2008         0           oxbow            0       40
## 198  YNP        0   2015         0        prospect            0       42
## 199  YNP        0   2016         0        prospect            0       42
## 200  YNP        0   2007         0        quadrant            0       43
## 201  YNP        0   2008         0        quadrant            0       43
## 202  YNP        0   2009         0        quadrant            0       43
## 203  YNP        0   2010         0        quadrant            0       43
## 204  YNP        0   1996         0            rose            0       44
## 205  YNP        0   1997         0            rose            0       44
## 206  YNP        0   1999         0            rose            0       44
## 207  YNP        0   2000         0            rose            0       44
## 208  YNP        0   2001         0            rose            0       44
## 209  YNP        0   2002         0            rose            0       44
## 210  YNP        0   2003         0          slough            0       45
## 211  YNP        0   2004         0          slough            0       45
## 212  YNP        0   2005         0          slough            0       45
## 213  YNP        0   2006         0          slough            0       45
## 214  YNP        0   2004         0 specimen/silver            0       47
## 215  YNP        0   2010         0 specimen/silver            0       47
## 216  YNP        0   2000         0            swan            0       48
## 217  YNP        0   2001         0            swan            0       48
## 218  YNP        0   2002         0            swan            0       48
## 219  YNP        0   2003         0            swan            0       48
## 220  YNP        0   2004         0            swan            0       48
## 221  YNP        0   2005         0            swan            0       48
## 222  YNP        0   1996         0       thorofare            0       49
## 223  YNP        0   1997         0       thorofare            0       49
## 224  YNP        0   1998         0       thorofare            0       49
## 225  YNP        0   2001         0           tower            0       50
## 226  YNP        0   2002         0           tower            0       50
## 227  YNP        0   2014         0          wapiti            0       51
## 228  YNP        0   2016         0          wapiti            0       51
## 229  YNP        0   2017         0          wapiti            0       51
## 230  YNP        0   2018         0          wapiti            0       51
## 231  YNP        0   2019         0          wapiti            0       51
## 232  YNP        0   1995         0       yelldelta            0       52
## 233  YNP        0   1996         0       yelldelta            0       52
## 234  YNP        0   1997         0       yelldelta            0       52
## 235  YNP        0   1998         0       yelldelta            0       52
## 236  YNP        0   1999         0       yelldelta            0       52
## 237  YNP        0   2000         0       yelldelta            0       52
## 238  YNP        0   2001         0       yelldelta            0       52
## 239  YNP        0   2002         0       yelldelta            0       52
## 240  YNP        0   2003         0       yelldelta            0       52
## 241  YNP        0   2004         0       yelldelta            0       52
## 242  YNP        0   2005         0       yelldelta            0       52
## 243  YNP        0   2006         0       yelldelta            0       52
## 244  YNP        0   2009         0       yelldelta            0       52
## 245  YNP        0   2010         0       yelldelta            0       52
## 246  YNP        0   2011         0       yelldelta            0       52
## 247  YNP        0   2012         0       yelldelta            0       52
## 248  YNP        0   2013         0       yelldelta            0       52
##     reprody1 packsize_aug persisty1 mort_yn
## 1          0           12         0       1
## 2          1           20         1       1
## 3          0            2         0       1
## 4          1           11         1       1
## 5          1           15         1       1
## 6          0           10         0       1
## 7          1           15         1       1
## 8          1           12         1       1
## 9          1           13         1       1
## 10         1           18         1       1
## 11         1           16         1       1
## 12         0           10         0       1
## 13         1           23         1       1
## 14         0            5         0       1
## 15         0            3         0       1
## 16         1           27         1       1
## 17         0            4         0       1
## 18         0           10         1       1
## 19         1           18         1       1
## 20         1            7         1       1
## 21         1           13         1       1
## 22         1            8         1       1
## 23         1            6         1       1
## 24         1           12         1       1
## 25         1            5         1       1
## 26         1            7         1       1
## 27         1            4         1       1
## 28         0           13         1       1
## 29         1           22         1       1
## 30         1            5         1       1
## 31         1           21         1       1
## 32         1           13         1       1
## 33         1           13         1       1
## 34         1           16         1       1
## 35         0           10         1       1
## 36         1            7         1       1
## 37         1           27         1       1
## 38         1            9         1       1
## 39         1           26         1       1
## 40        NA            3         0       1
## 41         1           15         1       1
## 42         0           23         1       1
## 43         1            8         1       1
## 44         1            9         1       1
## 45         1            7         1       1
## 46         1           22         1       1
## 47         1            9         1       1
## 48         1           23         1       1
## 49         1           22         1       1
## 50        NA           NA         1       0
## 51        NA            0         0       0
## 52         0            0         0       0
## 53         1            2         0       0
## 54         1           17         1       0
## 55         1           19         1       0
## 56         1           13         1       0
## 57         1           18         1       0
## 58         1           13         1       0
## 59         1           22         1       0
## 60         1           10         1       0
## 61         1           13         1       0
## 62         1           14         1       0
## 63         1           13         1       0
## 64         1           20         1       0
## 65         1           14         1       0
## 66         1            4         1       0
## 67         1            9         1       0
## 68         1           13         1       0
## 69         0            0         0       0
## 70         1            4         1       0
## 71         0           11         1       0
## 72         1           NA         1       0
## 73         1           17         1       0
## 74         1            4         1       0
## 75         0            3         0       0
## 76         1            6         1       0
## 77         1            4         1       0
## 78         1            6         1       0
## 79         1            8         1       0
## 80         1            9         1       0
## 81         1            5         1       0
## 82         1            6         1       0
## 83         1            6         1       0
## 84         0            0         0       0
## 85         1            3         1       0
## 86         1            4         1       0
## 87         1            6         1       0
## 88         1           11         1       0
## 89         1           14         1       0
## 90         1           15         1       0
## 91         1           14         1       0
## 92         1            4         1       0
## 93         0            7         1       0
## 94         1            5         1       0
## 95         1            6         1       0
## 96         1            4         1       0
## 97         1            7         1       0
## 98         1           11         1       0
## 99         1           14         1       0
## 100        1            8         1       0
## 101        1           10         1       0
## 102        1            6         1       0
## 103        1            6         1       0
## 104        1           NA         1       0
## 105        1            5         1       0
## 106        1            8         1       0
## 107        1            9         1       0
## 108        1           37         1       0
## 109        1           16         1       0
## 110        1           18         1       0
## 111        1           13         1       0
## 112        1            5         1       0
## 113        1           15         1       0
## 114        1           18         1       0
## 115        1           21         1       0
## 116        0           12         0       0
## 117        0            0        NA       0
## 118        1            9         1       0
## 119        0           12         0       0
## 120        1            9         1       0
## 121        1            9         1       0
## 122        1           12         1       0
## 123        1            7         1       0
## 124        1           NA         1       0
## 125        1           10         1       0
## 126        1           12         1       0
## 127        1           18         1       0
## 128        1           25         1       0
## 129        1            8         1       0
## 130        1           11         1       0
## 131        0            0         0       0
## 132       NA            6         1       0
## 133        1            4         1       0
## 134        1            5         1       0
## 135        1            7         1       0
## 136       NA            9         0       0
## 137        1            2         1       0
## 138        1            7         1       0
## 139        1           19         1       0
## 140        1            8         1       0
## 141        1           11         1       0
## 142        1           35         1       0
## 143        1            7         1       0
## 144        1           11         1       0
## 145        1            8         1       0
## 146        1           12         1       0
## 147        1            4         1       0
## 148        1            3         1       0
## 149        1            5         1       0
## 150        1            3         1       0
## 151        0            1         0       0
## 152        1            5         1       0
## 153        1           10         1       0
## 154        1           12         1       0
## 155        1           11         1       0
## 156        1           15         1       0
## 157        1           14         1       0
## 158        1           16         1       0
## 159        1           21         1       0
## 160        1           28         1       0
## 161        1           26         1       0
## 162        1           20         1       0
## 163        1           19         1       0
## 164        0            7         0       0
## 165        0            0         0       0
## 166        1            5         1       0
## 167        1            2         1       0
## 168        1            8         1       0
## 169        1           16         1       0
## 170        0           15         1       0
## 171        1            5         1       0
## 172        1           10         1       0
## 173        1           13         1       0
## 174        1            8         1       0
## 175        0            9         1       0
## 176        1            7         1       0
## 177        1           11         1       0
## 178        1           15         1       0
## 179        1           17         1       0
## 180        1           17         1       0
## 181        1           12         1       0
## 182        1           17         1       0
## 183        1           18         1       0
## 184        1           15         1       0
## 185        1           10         1       0
## 186        1            8         1       0
## 187        0            0        NA       0
## 188        1            7         1       0
## 189        1           12         1       0
## 190        1           19         1       0
## 191        1           18         1       0
## 192        1           18         1       0
## 193        1           14         1       0
## 194        0           11         0       0
## 195        1           12         1       0
## 196        1           24         1       0
## 197        0           20         0       0
## 198        1           13         1       0
## 199        1           12         1       0
## 200        1           NA         1       0
## 201        1            4         1       0
## 202        1            7         1       0
## 203        0            7         1       0
## 204        1           11         1       0
## 205        1           16         1       0
## 206        1           22         1       0
## 207        1           21         1       0
## 208        1           10         1       0
## 209        1           11         1       0
## 210        1           10         1       0
## 211        1           21         1       0
## 212        1           15         1       0
## 213        1            9         1       0
## 214       NA            5         1       0
## 215        0            8         0       0
## 216        1            7         1       0
## 217        1            9         1       0
## 218        1           18         1       0
## 219        1           20         1       0
## 220        1           10         1       0
## 221        1            3         1       0
## 222        1            2         1       0
## 223        0            8         0       0
## 224       NA            0        NA       0
## 225        1            4         1       0
## 226        0            2         0       0
## 227        1            2         1       0
## 228        1            9         1       0
## 229        1           21         1       0
## 230        1           18         1       0
## 231        1           19         1       0
## 232        1            6         1       0
## 233        1            5         1       0
## 234        0            8         1       0
## 235        0            8         1       0
## 236        1            6         1       0
## 237        1           15         1       0
## 238        1           18         1       0
## 239        1           17         1       0
## 240        1           17         1       0
## 241        1           19         1       0
## 242        1           19         1       0
## 243        1           15         1       0
## 244        1            4         1       0
## 245        1            9         1       0
## 246        0           13         1       0
## 247        1           11         1       0
## 248        1           17         1       0
```


Problem 7. (3 points) Among the Yellowstone wolf packs, the [Druid Peak Pack](https://www.pbs.org/wnet/nature/in-the-valley-of-the-wolves-the-druid-wolf-pack-story/209/) is one of most famous. What was the average pack size of this pack for the years represented in the data?


```r
ynp %>% 
  filter(pack=="druid") %>% 
  summarise(mean_packsize=mean(packsize_aug),
            total=n())
```

```
##   mean_packsize total
## 1      13.93333    15
```
Mean pack size for the years included in the YNP data is 13.93 for the druid pack. 

Problem 8. (4 points) Pack dynamics can be hard to predict- even for strong packs like the Druid Peak pack. At which year did the Druid Peak pack have the largest pack size? What do you think happened in 2010?

```r
summary(ynp)
```

```
##      park              mort_all          biolyr       mort_lead      
##  Length:248         Min.   :0.0000   Min.   :1995   Min.   :0.00000  
##  Class :character   1st Qu.:0.0000   1st Qu.:2003   1st Qu.:0.00000  
##  Mode  :character   Median :0.0000   Median :2008   Median :0.00000  
##                     Mean   :0.2903   Mean   :2008   Mean   :0.06478  
##                     3rd Qu.:0.0000   3rd Qu.:2013   3rd Qu.:0.00000  
##                     Max.   :4.0000   Max.   :2021   Max.   :3.00000  
##                                                     NA's   :1        
##      pack            mort_nonlead       packcode        reprody1     
##  Length:248         Min.   :0.0000   Min.   : 2.00   Min.   :0.0000  
##  Class :character   1st Qu.:0.0000   1st Qu.:24.00   1st Qu.:1.0000  
##  Mode  :character   Median :0.0000   Median :34.00   Median :1.0000  
##                     Mean   :0.2227   Mean   :32.53   Mean   :0.8506  
##                     3rd Qu.:0.0000   3rd Qu.:41.00   3rd Qu.:1.0000  
##                     Max.   :3.0000   Max.   :52.00   Max.   :1.0000  
##                     NA's   :1                        NA's   :7       
##   packsize_aug     persisty1         mort_yn      
##  Min.   : 0.00   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.: 6.00   1st Qu.:1.0000   1st Qu.:0.0000  
##  Median :10.00   Median :1.0000   Median :0.0000  
##  Mean   :11.24   Mean   :0.8939   Mean   :0.1976  
##  3rd Qu.:15.50   3rd Qu.:1.0000   3rd Qu.:0.0000  
##  Max.   :37.00   Max.   :1.0000   Max.   :1.0000  
##  NA's   :5       NA's   :3
```



```r
ynp %>% 
  select(park,pack,biolyr,packsize_aug,) %>% 
  filter(pack=="druid") %>% 
  arrange(desc(packsize_aug))
```

```
##    park  pack biolyr packsize_aug
## 1   YNP druid   2001           37
## 2   YNP druid   2000           27
## 3   YNP druid   2008           21
## 4   YNP druid   2003           18
## 5   YNP druid   2007           18
## 6   YNP druid   2002           16
## 7   YNP druid   2006           15
## 8   YNP druid   2004           13
## 9   YNP druid   2009           12
## 10  YNP druid   1999            9
## 11  YNP druid   1998            8
## 12  YNP druid   1997            5
## 13  YNP druid   1996            5
## 14  YNP druid   2005            5
## 15  YNP druid   2010            0
```
The Druid pack had the largest pack size during 2001 at 37 packs. 


```r
ynp %>% 
  select(park,pack,biolyr,packsize_aug,) %>% 
  filter(biolyr==2010) %>% 
  filter(pack=="druid")
```

```
##   park  pack biolyr packsize_aug
## 1  YNP druid   2010            0
```
By specifying a biolyr only equal to 2010, we can observe the pack size during this time. Surprisingly, the pack size is 0. Compared to the pack size of 12 in 2009 and 21 in 2008 of the years prior, some large scale extinction event or series of stressors must have occurred. A quick online search shows that this wolf pack was under lot of stress by mange, competition, and natural dispersal.Factors that dwindled the pack size for Druids down to 0. 


Problem 9. (5 points) Among the YNP wolf packs, which one has had the highest overall persistence `persisty1` for the years represented in the data? Look this pack up online and tell me what is unique about its behavior- specifically, what prey animals does this pack specialize on?  


```r
ynp %>% 
  group_by(pack,biolyr) %>% 
  summarise(totalpersistance=mean(persisty1), 
            total=n()) %>% 
  arrange(desc(totalpersistance))
```

```
## `summarise()` has grouped output by 'pack'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 248 × 4
## # Groups:   pack [46]
##    pack      biolyr totalpersistance total
##    <chr>      <int>            <dbl> <int>
##  1 682Mgroup   2009                1     1
##  2 8mile       2011                1     1
##  3 8mile       2012                1     1
##  4 8mile       2013                1     1
##  5 8mile       2014                1     1
##  6 8mile       2015                1     1
##  7 8mile       2016                1     1
##  8 8mile       2017                1     1
##  9 8mile       2018                1     1
## 10 8mile       2020                1     1
## # ℹ 238 more rows
```

```r
n_distinct(ynp$pack)
```

```
## [1] 46
```

```r
table(ynp$pack)
```

```
## 
##      1118Fgroup      1155Mgroup       642Fgroup       682Mgroup       694Fgroup 
##               1               1               1               2               1 
##       755Mgroup           8mile       963Fgroup           agate         bechler 
##               1               9               1              11               2 
##         biscuit       blacktail     buffalofork          canyon       carnelian 
##               1               5               2              10               1 
##        cinnabar      cottonwood          cougar         crevice           druid 
##               1               2              20               1              15 
##          everts      geode/hell     gibbon/mary        grayling          hayden 
##               2               4              10               2               4 
##           heart        junction           lamar            lava         leopold 
##               2               8               8               3              13 
##        lonestar         mollies        nezperce           oxbow         phantom 
##               1              26              11               3               3 
##        prospect        quadrant            rose          slough           snake 
##               4               4               8               5               2 
## specimen/silver            swan       thorofare           tower          wapiti 
##               2               6               3               2               6 
##       yelldelta 
##              18
```

```r
tabyl(ynp$pack) %>% 
  arrange(desc(n))
```

```
##         ynp$pack  n     percent
##          mollies 26 0.104838710
##           cougar 20 0.080645161
##        yelldelta 18 0.072580645
##            druid 15 0.060483871
##          leopold 13 0.052419355
##            agate 11 0.044354839
##         nezperce 11 0.044354839
##           canyon 10 0.040322581
##      gibbon/mary 10 0.040322581
##            8mile  9 0.036290323
##         junction  8 0.032258065
##            lamar  8 0.032258065
##             rose  8 0.032258065
##             swan  6 0.024193548
##           wapiti  6 0.024193548
##        blacktail  5 0.020161290
##           slough  5 0.020161290
##       geode/hell  4 0.016129032
##           hayden  4 0.016129032
##         prospect  4 0.016129032
##         quadrant  4 0.016129032
##             lava  3 0.012096774
##            oxbow  3 0.012096774
##          phantom  3 0.012096774
##        thorofare  3 0.012096774
##        682Mgroup  2 0.008064516
##          bechler  2 0.008064516
##      buffalofork  2 0.008064516
##       cottonwood  2 0.008064516
##           everts  2 0.008064516
##         grayling  2 0.008064516
##            heart  2 0.008064516
##            snake  2 0.008064516
##  specimen/silver  2 0.008064516
##            tower  2 0.008064516
##       1118Fgroup  1 0.004032258
##       1155Mgroup  1 0.004032258
##        642Fgroup  1 0.004032258
##        694Fgroup  1 0.004032258
##        755Mgroup  1 0.004032258
##        963Fgroup  1 0.004032258
##          biscuit  1 0.004032258
##        carnelian  1 0.004032258
##         cinnabar  1 0.004032258
##          crevice  1 0.004032258
##         lonestar  1 0.004032258
```
By making a sub data set, we can see which packs had a total persistence of 1.0, which from the readme file, is the highest value for the persistence. Then, to account for the `highest total overall pesistance` we need to check which wolf pack from the sub data set is the most abundant (with persistence values of 1.0). Using tabyl to see the number of packs, we see that the mollies wolf pack is the most abundant pack at 26, all with persistence of 1.0. Hence, mollies is the wolf pack with the highest overall persistence

An online search shows that the mollie's pack have unique behavior of hunting bison and having regular interactions with bears. This is strange behavior since wolves in YNP tend to avoid bison. 

Problem 10. (3 points) Perform one analysis or exploration of your choice on the `wolves` data. Your answer needs to include at least two lines of code and not be a summary function.  

Analysis: What was the total number of human caused mortalities (`mortALL`) for the Druid pack in 2003 from YNP? 


```r
ynp %>% 
  select(pack,mort_all,biolyr) %>% 
  filter(biolyr==2003) %>% 
  filter(pack=="druid") %>% 
  arrange(desc(mort_all))
```

```
##    pack mort_all biolyr
## 1 druid        0   2003
```
As shown, the druid pack had no mortalities dring the year 2003. 

At the end of the exam, open up the knitted file (orginally html) and save it as a pdf. Then uploading this pdf file into gradescope. 
