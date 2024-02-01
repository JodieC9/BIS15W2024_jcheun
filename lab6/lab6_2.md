---
title: "dplyr Superhero"
date: "2024-01-31"
output:
  html_document: 
    theme: spacelab
    toc: true
    toc_float: true
    keep_md: true
---

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. This lab doubles as your homework. Please complete the lab and push your final code to GitHub.  

## Load the libraries

```r
library("tidyverse")
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
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
In the read call, we specify the values we want R to interpret as NA. Do not add this function to your read calls until you really understand the data 
## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here. Before you do anything, first have a look at the names of the variables. You can use `rename()` or `clean_names()`.    

```r
superhero_info <- clean_names(superhero_info)
superhero_info
```

```
## # A tibble: 734 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… <NA>       good     
##  2 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  3 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  4 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  5 Abra… Male   blue      Cosm… Black          NA Marvel C… <NA>       bad      
##  6 Abso… Male   blue      Human No Hair       193 Marvel C… <NA>       bad      
##  7 Adam… Male   blue      <NA>  Blond          NA NBC - He… <NA>       good     
##  8 Adam… Male   blue      Human Blond         185 DC Comics <NA>       good     
##  9 Agen… Female blue      <NA>  Blond         173 Marvel C… <NA>       good     
## 10 Agen… Male   brown     Human Brown         178 Marvel C… <NA>       good     
## # ℹ 724 more rows
## # ℹ 1 more variable: weight <dbl>
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

1. Who are the publishers of the superheros? Show the proportion of superheros from each publisher. Which publisher has the highest number of superheros?  


```r
summary(superhero_info)
```

```
##      name              gender           eye_color             race          
##  Length:734         Length:734         Length:734         Length:734        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##   hair_color            height       publisher          skin_color       
##  Length:734         Min.   : 15.2   Length:734         Length:734        
##  Class :character   1st Qu.:173.0   Class :character   Class :character  
##  Mode  :character   Median :183.0   Mode  :character   Mode  :character  
##                     Mean   :186.7                                        
##                     3rd Qu.:191.0                                        
##                     Max.   :975.0                                        
##                     NA's   :217                                          
##   alignment             weight     
##  Length:734         Min.   :  2.0  
##  Class :character   1st Qu.: 61.0  
##  Mode  :character   Median : 81.0  
##                     Mean   :112.3  
##                     3rd Qu.:108.0  
##                     Max.   :900.0  
##                     NA's   :239
```

```r
summary(superhero_powers)
```

```
##   hero_names         Agility        Accelerated Healing Lantern Power Ring
##  Length:667         Mode :logical   Mode :logical       Mode :logical     
##  Class :character   FALSE:425       FALSE:489           FALSE:656         
##  Mode  :character   TRUE :242       TRUE :178           TRUE :11          
##  Dimensional Awareness Cold Resistance Durability       Stealth       
##  Mode :logical         Mode :logical   Mode :logical   Mode :logical  
##  FALSE:642             FALSE:620       FALSE:410       FALSE:541      
##  TRUE :25              TRUE :47        TRUE :257       TRUE :126      
##  Energy Absorption   Flight        Danger Sense    Underwater breathing
##  Mode :logical     Mode :logical   Mode :logical   Mode :logical       
##  FALSE:590         FALSE:455       FALSE:637       FALSE:646           
##  TRUE :77          TRUE :212       TRUE :30        TRUE :21            
##  Marksmanship    Weapons Master  Power Augmentation Animal Attributes
##  Mode :logical   Mode :logical   Mode :logical      Mode :logical    
##  FALSE:548       FALSE:562       FALSE:659          FALSE:642        
##  TRUE :119       TRUE :105       TRUE :8            TRUE :25         
##  Longevity       Intelligence    Super Strength  Cryokinesis    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:554       FALSE:509       FALSE:307       FALSE:648      
##  TRUE :113       TRUE :158       TRUE :360       TRUE :19       
##  Telepathy       Energy Armor    Energy Blasts   Duplication    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:575       FALSE:659       FALSE:520       FALSE:651      
##  TRUE :92        TRUE :8         TRUE :147       TRUE :16       
##  Size Changing   Density Control  Stamina        Astral Travel  
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:612       FALSE:652       FALSE:378       FALSE:663      
##  TRUE :55        TRUE :15        TRUE :289       TRUE :4        
##  Audio Control   Dexterity        Omnitrix       Super Speed    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:660       FALSE:661       FALSE:666       FALSE:418      
##  TRUE :7         TRUE :6         TRUE :1         TRUE :249      
##  Possession      Animal Oriented Powers Weapon-based Powers Electrokinesis 
##  Mode :logical   Mode :logical          Mode :logical       Mode :logical  
##  FALSE:659       FALSE:627              FALSE:609           FALSE:645      
##  TRUE :8         TRUE :40               TRUE :58            TRUE :22       
##  Darkforce Manipulation Death Touch     Teleportation   Enhanced Senses
##  Mode :logical          Mode :logical   Mode :logical   Mode :logical  
##  FALSE:657              FALSE:660       FALSE:595       FALSE:578      
##  TRUE :10               TRUE :7         TRUE :72        TRUE :89       
##  Telekinesis     Energy Beams      Magic         Hyperkinesis   
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:606       FALSE:625       FALSE:623       FALSE:666      
##  TRUE :61        TRUE :42        TRUE :44        TRUE :1        
##     Jump         Clairvoyance    Dimensional Travel Power Sense    
##  Mode :logical   Mode :logical   Mode :logical      Mode :logical  
##  FALSE:602       FALSE:663       FALSE:644          FALSE:664      
##  TRUE :65        TRUE :4         TRUE :23           TRUE :3        
##  Shapeshifting   Peak Human Condition Immortality     Camouflage     
##  Mode :logical   Mode :logical        Mode :logical   Mode :logical  
##  FALSE:601       FALSE:637            FALSE:598       FALSE:646      
##  TRUE :66        TRUE :30             TRUE :69        TRUE :21       
##  Element Control  Phasing        Astral Projection Electrical Transport
##  Mode :logical   Mode :logical   Mode :logical     Mode :logical       
##  FALSE:659       FALSE:636       FALSE:638         FALSE:666           
##  TRUE :8         TRUE :31        TRUE :29          TRUE :1             
##  Fire Control    Projection      Summoning       Enhanced Memory
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:635       FALSE:665       FALSE:663       FALSE:642      
##  TRUE :32        TRUE :2         TRUE :4         TRUE :25       
##   Reflexes       Invulnerability Energy Constructs Force Fields   
##  Mode :logical   Mode :logical   Mode :logical     Mode :logical  
##  FALSE:503       FALSE:550       FALSE:629         FALSE:581      
##  TRUE :164       TRUE :117       TRUE :38          TRUE :86       
##  Self-Sustenance Anti-Gravity     Empathy        Power Nullifier
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:630       FALSE:666       FALSE:648       FALSE:663      
##  TRUE :37        TRUE :1         TRUE :19        TRUE :4        
##  Radiation Control Psionic Powers  Elasticity      Substance Secretion
##  Mode :logical     Mode :logical   Mode :logical   Mode :logical      
##  FALSE:660         FALSE:618       FALSE:656       FALSE:650          
##  TRUE :7           TRUE :49        TRUE :11        TRUE :17           
##  Elemental Transmogrification Technopath/Cyberpath Photographic Reflexes
##  Mode :logical                Mode :logical        Mode :logical        
##  FALSE:661                    FALSE:644            FALSE:664            
##  TRUE :6                      TRUE :23             TRUE :3              
##  Seismic Power   Animation       Precognition    Mind Control   
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:664       FALSE:662       FALSE:645       FALSE:645      
##  TRUE :3         TRUE :5         TRUE :22        TRUE :22       
##  Fire Resistance Power Absorption Enhanced Hearing Nova Force     
##  Mode :logical   Mode :logical    Mode :logical    Mode :logical  
##  FALSE:649       FALSE:655        FALSE:595        FALSE:665      
##  TRUE :18        TRUE :12         TRUE :72         TRUE :2        
##   Insanity       Hypnokinesis    Animal Control  Natural Armor  
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:662       FALSE:644       FALSE:658       FALSE:631      
##  TRUE :5         TRUE :23        TRUE :9         TRUE :36       
##  Intangibility   Enhanced Sight  Molecular Manipulation Heat Generation
##  Mode :logical   Mode :logical   Mode :logical          Mode :logical  
##  FALSE:647       FALSE:642       FALSE:625              FALSE:643      
##  TRUE :20        TRUE :25        TRUE :42               TRUE :24       
##  Adaptation       Gliding        Power Suit      Mind Blast     
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:662       FALSE:657       FALSE:634       FALSE:655      
##  TRUE :5         TRUE :10        TRUE :33        TRUE :12       
##  Probability Manipulation Gravity Control Regeneration    Light Control  
##  Mode :logical            Mode :logical   Mode :logical   Mode :logical  
##  FALSE:658                FALSE:651       FALSE:639       FALSE:652      
##  TRUE :9                  TRUE :16        TRUE :28        TRUE :15       
##  Echolocation    Levitation      Toxin and Disease Control   Banish       
##  Mode :logical   Mode :logical   Mode :logical             Mode :logical  
##  FALSE:665       FALSE:641       FALSE:657                 FALSE:666      
##  TRUE :2         TRUE :26        TRUE :10                  TRUE :1        
##  Energy Manipulation Heat Resistance Natural Weapons Time Travel    
##  Mode :logical       Mode :logical   Mode :logical   Mode :logical  
##  FALSE:615           FALSE:624       FALSE:609       FALSE:634      
##  TRUE :52            TRUE :43        TRUE :58        TRUE :33       
##  Enhanced Smell  Illusions       Thirstokinesis  Hair Manipulation
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical    
##  FALSE:635       FALSE:629       FALSE:666       FALSE:666        
##  TRUE :32        TRUE :38        TRUE :1         TRUE :1          
##  Illumination    Omnipotent       Cloaking       Changing Armor 
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:665       FALSE:660       FALSE:660       FALSE:666      
##  TRUE :2         TRUE :7         TRUE :7         TRUE :1        
##  Power Cosmic    Biokinesis      Water Control   Radiation Immunity
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical     
##  FALSE:660       FALSE:666       FALSE:654       FALSE:657         
##  TRUE :7         TRUE :1         TRUE :13        TRUE :10          
##  Vision - Telescopic Toxin and Disease Resistance Spatial Awareness
##  Mode :logical       Mode :logical                Mode :logical    
##  FALSE:624           FALSE:619                    FALSE:666        
##  TRUE :43            TRUE :48                     TRUE :1          
##  Energy Resistance Telepathy Resistance Molecular Combustion Omnilingualism 
##  Mode :logical     Mode :logical        Mode :logical        Mode :logical  
##  FALSE:660         FALSE:634            FALSE:665            FALSE:646      
##  TRUE :7           TRUE :33             TRUE :2              TRUE :21       
##  Portal Creation Magnetism       Mind Control Resistance Plant Control  
##  Mode :logical   Mode :logical   Mode :logical           Mode :logical  
##  FALSE:663       FALSE:656       FALSE:655               FALSE:659      
##  TRUE :4         TRUE :11        TRUE :12                TRUE :8        
##    Sonar         Sonic Scream    Time Manipulation Enhanced Touch 
##  Mode :logical   Mode :logical   Mode :logical     Mode :logical  
##  FALSE:663       FALSE:661       FALSE:647         FALSE:660      
##  TRUE :4         TRUE :6         TRUE :20          TRUE :7        
##  Magic Resistance Invisibility    Sub-Mariner     Radiation Absorption
##  Mode :logical    Mode :logical   Mode :logical   Mode :logical       
##  FALSE:661        FALSE:645       FALSE:647       FALSE:660           
##  TRUE :6          TRUE :22        TRUE :20        TRUE :7             
##  Intuitive aptitude Vision - Microscopic  Melting        Wind Control   
##  Mode :logical      Mode :logical        Mode :logical   Mode :logical  
##  FALSE:666          FALSE:648            FALSE:665       FALSE:664      
##  TRUE :1            TRUE :19             TRUE :2         TRUE :3        
##  Super Breath    Wallcrawling    Vision - Night  Vision - Infrared
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical    
##  FALSE:644       FALSE:633       FALSE:633       FALSE:645        
##  TRUE :23        TRUE :34        TRUE :34        TRUE :22         
##  Grim Reaping    Matter Absorption The Force       Resurrection   
##  Mode :logical   Mode :logical     Mode :logical   Mode :logical  
##  FALSE:664       FALSE:656         FALSE:661       FALSE:652      
##  TRUE :3         TRUE :11          TRUE :6         TRUE :15       
##  Terrakinesis    Vision - Heat   Vitakinesis     Radar Sense    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:665       FALSE:648       FALSE:665       FALSE:661      
##  TRUE :2         TRUE :19        TRUE :2         TRUE :6        
##  Qwardian Power Ring Weather Control Vision - X-Ray  Vision - Thermal
##  Mode :logical       Mode :logical   Mode :logical   Mode :logical   
##  FALSE:665           FALSE:659       FALSE:644       FALSE:644       
##  TRUE :2             TRUE :8         TRUE :23        TRUE :23        
##  Web Creation    Reality Warping Odin Force      Symbiote Costume
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical   
##  FALSE:653       FALSE:651       FALSE:665       FALSE:658       
##  TRUE :14        TRUE :16        TRUE :2         TRUE :9         
##  Speed Force     Phoenix Force   Molecular Dissipation Vision - Cryo  
##  Mode :logical   Mode :logical   Mode :logical         Mode :logical  
##  FALSE:666       FALSE:666       FALSE:666             FALSE:665      
##  TRUE :1         TRUE :1         TRUE :1               TRUE :2        
##  Omnipresent     Omniscient     
##  Mode :logical   Mode :logical  
##  FALSE:665       FALSE:665      
##  TRUE :2         TRUE :2
```


```r
superhero_info %>% 
  tabyl(publisher) %>% 
  arrange(desc(percent))
```

```
##          publisher   n     percent valid_percent
##      Marvel Comics 388 0.528610354   0.539638387
##          DC Comics 215 0.292915531   0.299026426
##       NBC - Heroes  19 0.025885559   0.026425591
##  Dark Horse Comics  18 0.024523161   0.025034771
##               <NA>  15 0.020435967            NA
##       George Lucas  14 0.019073569   0.019471488
##       Image Comics  14 0.019073569   0.019471488
##      HarperCollins   6 0.008174387   0.008344924
##          Star Trek   6 0.008174387   0.008344924
##               SyFy   5 0.006811989   0.006954103
##       Team Epic TV   5 0.006811989   0.006954103
##        ABC Studios   4 0.005449591   0.005563282
##     IDW Publishing   4 0.005449591   0.005563282
##        Icon Comics   4 0.005449591   0.005563282
##           Shueisha   4 0.005449591   0.005563282
##          Wildstorm   3 0.004087193   0.004172462
##      Sony Pictures   2 0.002724796   0.002781641
##      Hanna-Barbera   1 0.001362398   0.001390821
##      J. K. Rowling   1 0.001362398   0.001390821
##   J. R. R. Tolkien   1 0.001362398   0.001390821
##          Microsoft   1 0.001362398   0.001390821
##          Rebellion   1 0.001362398   0.001390821
##         South Park   1 0.001362398   0.001390821
##        Titan Books   1 0.001362398   0.001390821
##  Universal Studios   1 0.001362398   0.001390821
```
With a combination of `tabyl` and `desc` to specify the data we want from the dataframe, we observe that Marvel Comics publisher has the highest percentage of superheros (n-value). 

2. Notice that we have some neutral superheros! Who are they? List their names below.  

```r
superhero_info %>% 
  select(name, alignment) %>% 
  filter(alignment=="neutral")
```

```
## # A tibble: 24 × 2
##    name         alignment
##    <chr>        <chr>    
##  1 Bizarro      neutral  
##  2 Black Flash  neutral  
##  3 Captain Cold neutral  
##  4 Copycat      neutral  
##  5 Deadpool     neutral  
##  6 Deathstroke  neutral  
##  7 Etrigan      neutral  
##  8 Galactus     neutral  
##  9 Gladiator    neutral  
## 10 Indigo       neutral  
## # ℹ 14 more rows
```
Notice that because we specify the dataframe used in the first line, we do no thave to include it with the `select` command in the piping steps afterwards. 

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```


```r
superhero_info %>% 
  select(name,alignment,race)
```

```
## # A tibble: 734 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ℹ 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info %>% 
  filter(race != "Human")
```

```
## # A tibble: 222 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  2 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  3 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  4 Abra… Male   blue      Cosm… Black          NA Marvel C… <NA>       bad      
##  5 Ajax  Male   brown     Cybo… Black         193 Marvel C… <NA>       bad      
##  6 Alien Male   <NA>      Xeno… No Hair       244 Dark Hor… black      bad      
##  7 Amazo Male   red       Andr… <NA>          257 DC Comics <NA>       bad      
##  8 Angel Male   <NA>      Vamp… <NA>           NA Dark Hor… <NA>       good     
##  9 Ange… Female yellow    Muta… Black         165 Marvel C… <NA>       good     
## 10 Anti… Male   yellow    God … No Hair        61 DC Comics <NA>       bad      
## # ℹ 212 more rows
## # ℹ 1 more variable: weight <dbl>
```
Please note that only superheros with `Human` listed in the `race` column were considered `Human` for this analysis. As such, human hybride superheros were considered `Not Human`

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- (superhero_info) %>% 
  select(name, alignment, race) %>% 
  filter(alignment== "good")
good_guys
```

```
## # A tibble: 496 × 3
##    name         alignment race         
##    <chr>        <chr>     <chr>        
##  1 A-Bomb       good      Human        
##  2 Abe Sapien   good      Icthyo Sapien
##  3 Abin Sur     good      Ungaran      
##  4 Adam Monroe  good      <NA>         
##  5 Adam Strange good      Human        
##  6 Agent 13     good      <NA>         
##  7 Agent Bob    good      Human        
##  8 Agent Zero   good      <NA>         
##  9 Alan Scott   good      <NA>         
## 10 Alex Woolsly good      <NA>         
## # ℹ 486 more rows
```


```r
bad_guys <- (superhero_info) %>% 
  select(name, alignment, race) %>% 
  filter(alignment== "bad")
bad_guys
```

```
## # A tibble: 207 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 Abomination   bad       Human / Radiation
##  2 Abraxas       bad       Cosmic Entity    
##  3 Absorbing Man bad       Human            
##  4 Air-Walker    bad       <NA>             
##  5 Ajax          bad       Cyborg           
##  6 Alex Mercer   bad       Human            
##  7 Alien         bad       Xenomorph XX121  
##  8 Amazo         bad       Android          
##  9 Ammo          bad       Human            
## 10 Angela        bad       <NA>             
## # ℹ 197 more rows
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
good_guys <- (superhero_info) %>% 
  select(name, alignment, race) %>% 
  filter(alignment== "good") %>% 
  tabyl(race) %>% 
  arrange(desc(percent))
good_guys
```

```
##               race   n     percent valid_percent
##               <NA> 217 0.437500000            NA
##              Human 148 0.298387097   0.530465950
##             Mutant  46 0.092741935   0.164874552
##  Human / Radiation   8 0.016129032   0.028673835
##      God / Eternal   6 0.012096774   0.021505376
##              Alpha   5 0.010080645   0.017921147
##            Android   4 0.008064516   0.014336918
##          Atlantean   4 0.008064516   0.014336918
##            Inhuman   4 0.008064516   0.014336918
##         Kryptonian   4 0.008064516   0.014336918
##              Alien   3 0.006048387   0.010752688
##          Asgardian   3 0.006048387   0.010752688
##             Cyborg   3 0.006048387   0.010752688
##              Demon   3 0.006048387   0.010752688
##           Symbiote   3 0.006048387   0.010752688
##             Amazon   2 0.004032258   0.007168459
##             Animal   2 0.004032258   0.007168459
##           Demi-God   2 0.004032258   0.007168459
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##         Human-Kree   2 0.004032258   0.007168459
##            Vampire   2 0.004032258   0.007168459
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##             Gungan   1 0.002016129   0.003584229
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##    Kakarantharaian   1 0.002016129   0.003584229
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
```

7. Among the good guys, Who are the Vampires?

```r
good_guys_vampire <- select(superhero_info, name, alignment,race) %>% 
  filter(alignment== "good") %>% 
  filter(race=="Vampire")

good_guys_vampire
```

```
## # A tibble: 2 × 3
##   name  alignment race   
##   <chr> <chr>     <chr>  
## 1 Angel good      Vampire
## 2 Blade good      Vampire
```
As shown, Angel and Blade are the only Vampires with a `good` alignment.

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
tall_bad_guys <- select(superhero_info, name, gender,alignment, height, race) %>% 
  filter(alignment == "bad") %>% 
  filter(gender== "Male") %>% 
  filter(race=="Human") %>% 
  filter(height> 200)

tall_bad_guys
```

```
## # A tibble: 5 × 5
##   name        gender alignment height race 
##   <chr>       <chr>  <chr>      <dbl> <chr>
## 1 Bane        Male   bad          203 Human
## 2 Doctor Doom Male   bad          201 Human
## 3 Kingpin     Male   bad          201 Human
## 4 Lizard      Male   bad          203 Human
## 5 Scorpion    Male   bad          211 Human
```
As shown, we have 5 bad guys with a height over 200 (no units provided)

9. Are there more good guys or bad guys with green hair?  

```r
superhero_hair_colors <- select(superhero_info, name, alignment, hair_color) %>% 
  filter(hair_color=="Green")
superhero_hair_colors
```

```
## # A tibble: 8 × 3
##   name           alignment hair_color
##   <chr>          <chr>     <chr>     
## 1 Beast Boy      good      Green     
## 2 Captain Planet good      Green     
## 3 Doc Samson     good      Green     
## 4 Hulk           good      Green     
## 5 Joker          bad       Green     
## 6 Lyja           good      Green     
## 7 Polaris        good      Green     
## 8 She-Hulk       good      Green
```
By making a new dataframe specifically containing only the variables name, alignment, and hair color we can drastically reduce the observations to analyze. Further reduction is done by filtering observations in the `hair_color` column to only include superheros with `Green`. From this, we can conclude there are more good guys with green hair. 

10. Let's explore who the really small superheros are. In the `superhero_info` data, which have a weight less than 50? Be sure to sort your results by weight lowest to highest.  

```r
small_superheros <- select(superhero_info, name, alignment, weight) %>% 
  filter(weight <50) %>% 
  arrange(weight)
small_superheros
```

```
## # A tibble: 19 × 3
##    name              alignment weight
##    <chr>             <chr>      <dbl>
##  1 Iron Monger       bad            2
##  2 Groot             good           4
##  3 Jack-Jack         good          14
##  4 Galactus          neutral       16
##  5 Yoda              good          17
##  6 Fin Fang Foom     good          18
##  7 Howard the Duck   good          18
##  8 Krypto            good          18
##  9 Rocket Raccoon    good          25
## 10 Dash              good          27
## 11 Longshot          good          36
## 12 Robin V           good          38
## 13 Wiz Kid           good          39
## 14 Violet Parr       good          41
## 15 Franklin Richards good          45
## 16 Swarm             bad           47
## 17 Hope Summers      good          48
## 18 Jolt              good          49
## 19 Snowbird          good          49
```
As shown, Iron Monger is the smallest superhero with a weight of 2 (no units specified), and they are a bad guy! 

11. Let's make a new variable that is the ratio of height to weight. Call this variable `height_weight_ratio`.    

```r
small_superheros <- select(superhero_info, name, alignment, height,weight) %>% 
  filter(weight <50) %>% 
  arrange(weight) %>% 
  mutate(height_weight_ratio=height/weight)
small_superheros
```

```
## # A tibble: 19 × 5
##    name              alignment height weight height_weight_ratio
##    <chr>             <chr>      <dbl>  <dbl>               <dbl>
##  1 Iron Monger       bad           NA      2               NA   
##  2 Groot             good         701      4              175.  
##  3 Jack-Jack         good          71     14                5.07
##  4 Galactus          neutral      876     16               54.8 
##  5 Yoda              good          66     17                3.88
##  6 Fin Fang Foom     good         975     18               54.2 
##  7 Howard the Duck   good          79     18                4.39
##  8 Krypto            good          64     18                3.56
##  9 Rocket Raccoon    good         122     25                4.88
## 10 Dash              good         122     27                4.52
## 11 Longshot          good         188     36                5.22
## 12 Robin V           good         137     38                3.61
## 13 Wiz Kid           good         140     39                3.59
## 14 Violet Parr       good         137     41                3.34
## 15 Franklin Richards good         142     45                3.16
## 16 Swarm             bad          196     47                4.17
## 17 Hope Summers      good         168     48                3.5 
## 18 Jolt              good         165     49                3.37
## 19 Snowbird          good         178     49                3.63
```

Using the mutate function, we can add a new column to our dataframe using already existing data. We can recall that ratios are simply a division problem between 2 values. In our case, we wish to find the height to weight ratio, as such, we would want to divide height by weight to obtain this ratio in a new column (to our pre-existing dataframe) termed `height_weight_ratio`.

12. Who has the highest height to weight ratio?  

```r
small_superheros <- select(superhero_info, name, alignment, height,weight) %>% 
  filter(weight <50) %>% 
  arrange(weight) %>% 
  mutate(height_weight_ratio=height/weight) %>% 
  arrange(desc(height_weight_ratio))
small_superheros
```

```
## # A tibble: 19 × 5
##    name              alignment height weight height_weight_ratio
##    <chr>             <chr>      <dbl>  <dbl>               <dbl>
##  1 Groot             good         701      4              175.  
##  2 Galactus          neutral      876     16               54.8 
##  3 Fin Fang Foom     good         975     18               54.2 
##  4 Longshot          good         188     36                5.22
##  5 Jack-Jack         good          71     14                5.07
##  6 Rocket Raccoon    good         122     25                4.88
##  7 Dash              good         122     27                4.52
##  8 Howard the Duck   good          79     18                4.39
##  9 Swarm             bad          196     47                4.17
## 10 Yoda              good          66     17                3.88
## 11 Snowbird          good         178     49                3.63
## 12 Robin V           good         137     38                3.61
## 13 Wiz Kid           good         140     39                3.59
## 14 Krypto            good          64     18                3.56
## 15 Hope Summers      good         168     48                3.5 
## 16 Jolt              good         165     49                3.37
## 17 Violet Parr       good         137     41                3.34
## 18 Franklin Richards good         142     45                3.16
## 19 Iron Monger       bad           NA      2               NA
```
As shown, Groot (a personal favorite of mine) has the largest height to weight ratio. 

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                     <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abi…
## $ Agility                        <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ `Accelerated Healing`          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, …
## $ `Lantern Power Ring`           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE…
## $ `Dimensional Awareness`        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Cold Resistance`              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ Durability                     <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE,…
## $ Stealth                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Absorption`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Flight                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Danger Sense`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Underwater breathing`         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ Marksmanship                   <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Weapons Master`               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Power Augmentation`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Animal Attributes`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Longevity                      <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE,…
## $ Intelligence                   <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, …
## $ `Super Strength`               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TR…
## $ Cryokinesis                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Telepathy                      <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Energy Armor`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Blasts`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Duplication                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Size Changing`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Density Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Stamina                        <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, F…
## $ `Astral Travel`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Audio Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Dexterity                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnitrix                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Super Speed`                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, …
## $ Possession                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Animal Oriented Powers`       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Weapon-based Powers`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Electrokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Darkforce Manipulation`       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Death Touch`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Teleportation                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Enhanced Senses`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Telekinesis                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Beams`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Magic                          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ Hyperkinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Jump                           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Clairvoyance                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Dimensional Travel`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Power Sense`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Shapeshifting                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Peak Human Condition`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Immortality                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE,…
## $ Camouflage                     <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE…
## $ `Element Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Phasing                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Astral Projection`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Electrical Transport`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Fire Control`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Projection                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Summoning                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Memory`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Reflexes                       <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ Invulnerability                <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE,…
## $ `Energy Constructs`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Force Fields`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Self-Sustenance`              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE…
## $ `Anti-Gravity`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Empathy                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Nullifier`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Radiation Control`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Psionic Powers`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Elasticity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Substance Secretion`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Elemental Transmogrification` <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Technopath/Cyberpath`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Photographic Reflexes`        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Seismic Power`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Animation                      <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE…
## $ Precognition                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Mind Control`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Fire Resistance`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Absorption`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Hearing`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Nova Force`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Insanity                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Hypnokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Animal Control`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Natural Armor`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Intangibility                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Sight`               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Molecular Manipulation`       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Heat Generation`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Adaptation                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Gliding                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Suit`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Mind Blast`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Probability Manipulation`     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Gravity Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Regeneration                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Light Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Echolocation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Levitation                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Toxin and Disease Control`    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Banish                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Manipulation`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Heat Resistance`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Natural Weapons`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Time Travel`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Smell`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Illusions                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Thirstokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Hair Manipulation`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Illumination                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnipotent                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Cloaking                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Changing Armor`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Cosmic`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ Biokinesis                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Water Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Radiation Immunity`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Telescopic`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Toxin and Disease Resistance` <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Spatial Awareness`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Resistance`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Telepathy Resistance`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Molecular Combustion`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnilingualism                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Portal Creation`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Magnetism                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Mind Control Resistance`      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Plant Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Sonar                          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Sonic Scream`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Time Manipulation`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Touch`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Magic Resistance`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Invisibility                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Sub-Mariner`                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Radiation Absorption`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Intuitive aptitude`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Microscopic`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Melting                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Wind Control`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Super Breath`                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE…
## $ Wallcrawling                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Night`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Infrared`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Grim Reaping`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Matter Absorption`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `The Force`                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Resurrection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Terrakinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Heat`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Vitakinesis                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Radar Sense`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Qwardian Power Ring`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Weather Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - X-Ray`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Thermal`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Web Creation`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Reality Warping`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Odin Force`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Symbiote Costume`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Speed Force`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Phoenix Force`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Molecular Dissipation`        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Cryo`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnipresent                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omniscient                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
```

13. How many superheros have a combination of agility, stealth, super_strength, stamina?

```r
superhero_powers <- clean_names(superhero_powers)
```


```r
fancy_superheros <- select(superhero_powers, hero_names,agility,stealth,super_strength,stamina) %>% 
  filter(agility==TRUE) %>% 
  filter(stealth==TRUE) %>% 
  filter(super_strength==TRUE) %>% 
  filter(stamina==TRUE)
fancy_superheros
```

```
## # A tibble: 40 × 5
##    hero_names  agility stealth super_strength stamina
##    <chr>       <lgl>   <lgl>   <lgl>          <lgl>  
##  1 Alex Mercer TRUE    TRUE    TRUE           TRUE   
##  2 Angel       TRUE    TRUE    TRUE           TRUE   
##  3 Ant-Man II  TRUE    TRUE    TRUE           TRUE   
##  4 Aquaman     TRUE    TRUE    TRUE           TRUE   
##  5 Batman      TRUE    TRUE    TRUE           TRUE   
##  6 Black Flash TRUE    TRUE    TRUE           TRUE   
##  7 Black Manta TRUE    TRUE    TRUE           TRUE   
##  8 Brundlefly  TRUE    TRUE    TRUE           TRUE   
##  9 Buffy       TRUE    TRUE    TRUE           TRUE   
## 10 Cable       TRUE    TRUE    TRUE           TRUE   
## # ℹ 30 more rows
```

```r
count(fancy_superheros)
```

```
## # A tibble: 1 × 1
##       n
##   <int>
## 1    40
```
Under the specifications, we have 40 superheros with those powers. 

## Your Favorite
14. Pick your favorite superhero and let's see their powers!  

```r
superhero_powers %>% 
  filter(hero_names=="Iron Man") %>% 
  select_if(all)
```

```
## Warning in .p(column, ...): coercing argument of type 'character' to logical
```

```
## # A tibble: 1 × 21
##   accelerated_healing durability energy_absorption flight underwater_breathing
##   <lgl>               <lgl>      <lgl>             <lgl>  <lgl>               
## 1 TRUE                TRUE       TRUE              TRUE   TRUE                
## # ℹ 16 more variables: marksmanship <lgl>, super_strength <lgl>,
## #   energy_blasts <lgl>, stamina <lgl>, super_speed <lgl>,
## #   weapon_based_powers <lgl>, energy_beams <lgl>, reflexes <lgl>,
## #   force_fields <lgl>, power_suit <lgl>, radiation_immunity <lgl>,
## #   vision_telescopic <lgl>, magnetism <lgl>, invisibility <lgl>,
## #   vision_night <lgl>, vision_thermal <lgl>
```
The superpowers Iron Man possesses are shown above.  

15. Can you find your hero in the superhero_info data? Show their info! 

```r
superhero_info %>% 
  filter(name=="Iron Man")
```

```
## # A tibble: 1 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Iron … Male   blue      Human Black         198 Marvel C… <NA>       good     
## # ℹ 1 more variable: weight <dbl>
```
All data about Iron Man in the superhero_info is shown above.

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
