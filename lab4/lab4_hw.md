---
title: "Lab 4 Homework"
author: "Please Add Your Name Here"
date: "2024-01-23"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---
## Jan 23 Prof NOTE: Only work on questions 1 through 6 for Lab4HW submission 



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```

```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fishe…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprini…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <dbl> 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  


**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  


**5. The variable `taxon` identifies the common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  


**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  


**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  


**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  




**9. Make a new dataframe `owls` that is limited to the mean mass, log10 mass, family, genus, and species of owls in the database. Which is the smallest owl? What is its common name? Do a little bit of searching online to see what you can learn about this species and provide a link below** 


**10. As measured by the data, which bird species has the largest homerange? Show all of your work, please. Look this species up online and tell me about it!**.  


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
