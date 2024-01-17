---
title: "Lab 2 Homework"
author: "Jodie Cheun"
date: "2024-01-16"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  
  Vectors in RStudio are a type of data structure that contains a sequence of data elements that are the same type. The most frequent data types include numeric, character, integer, logical, and complex. Vectors can be created by using the "c" function.  

2. What is a data matrix in R?  
  Data matrices can be interpreted as several vectors stacked on top of each other. The contain several elements of the same data type (vectors) that get organized into a specified number of rows and columns, similar to a data table.  

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

```r
hot_springs <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
```


```r
hot_spring_matrix <- matrix(hot_springs,nrow=8, byrow=T)
hot_spring_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

```r
hot_spring_names <- c("spring_1", "spring_2", "spring_3", "spring_4", "spring_5", "spring_6", "spring_7", "spring_8")
```


```r
scientist <- c("Jill", "Steve", "Susan")
```


```r
colnames(hot_spring_matrix) <- scientist
```


```r
rownames(hot_spring_matrix) <- hot_spring_names
```


```r
hot_spring_matrix
```

```
##           Jill Steve Susan
## spring_1 36.25 35.40 35.30
## spring_2 35.15 35.35 33.35
## spring_3 30.70 29.65 29.20
## spring_4 39.70 40.05 38.65
## spring_5 31.85 31.40 29.30
## spring_6 30.20 30.65 29.75
## spring_7 32.90 32.50 32.80
## spring_8 36.80 36.45 33.15
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.

```r
real_hotspring_names <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", 
"Too Hot Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")
```


```r
rownames(hot_spring_matrix) <- real_hotspring_names
```


```r
hot_spring_matrix
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## Too Hot Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
Average_Spring <- rowSums(hot_spring_matrix)
```

7. Add this as a new column in the data matrix.  

```r
all_hot_spring_matrix <- cbind(hot_spring_matrix, Average_Spring)
```


```r
all_hot_spring_matrix
```

```
##                   Jill Steve Susan Average_Spring
## Bluebell Spring  36.25 35.40 35.30         106.95
## Opal Spring      35.15 35.35 33.35         103.85
## Riverside Spring 30.70 29.65 29.20          89.55
## Too Hot Spring   39.70 40.05 38.65         118.40
## Mystery Spring   31.85 31.40 29.30          92.55
## Emerald Spring   30.20 30.65 29.75          90.60
## Black Spring     32.90 32.50 32.80          98.20
## Pearl Spring     36.80 36.45 33.15         106.40
```


8. Show Susan's value for Opal Spring only.(Note to self, the first value is the row you want to select from, and the second value is the column you want to select from)

```r
all_hot_spring_matrix[2,3]
```

```
## [1] 33.35
```


9. Calculate the mean for Jill's column only. 

```r
Jill_Spring_Calculations <- all_hot_spring_matrix[,1]
```


```r
mean(Jill_Spring_Calculations)
```

```
## [1] 34.19375
```


10. Use the data matrix to perform one calculation or operation of your interest.
Goal: Find the average temperature of springs' average. 


```r
all_average_spring <- all_hot_spring_matrix [,4]
```


```r
all_average_spring
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##           106.95           103.85            89.55           118.40 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##            92.55            90.60            98.20           106.40
```



```r
mean(all_average_spring)
```

```
## [1] 100.8125
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
