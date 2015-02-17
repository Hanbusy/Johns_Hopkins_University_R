---
title: "Vectorized Operation"
author: "Jinsu Han"
date: "Tuesday, February 17, 2015"
output: html_document
---

## Vectorized Operations

Many operations in R are vectorized making code more efficient, concise, and easier to read.


```r
x <- 1:4; y<- 6:9
x + y
```

```
## [1]  7  9 11 13
```

```r
x >2
```

```
## [1] FALSE FALSE  TRUE  TRUE
```

```r
x >= 2
```

```
## [1] FALSE  TRUE  TRUE  TRUE
```

```r
y == 8
```

```
## [1] FALSE FALSE  TRUE FALSE
```

```r
x * y
```

```
## [1]  6 14 24 36
```

```r
x / y
```

```
## [1] 0.1666667 0.2857143 0.3750000 0.4444444
```

## Vectorized Matrix Operations

```r
x <- matrix(1:4, 2, 2); y<- matrix(rep(10,4),2,2)  
x * y   ## element-wise multiplication
```

```
##      [,1] [,2]
## [1,]   10   30
## [2,]   20   40
```

```r
x / y
```

```
##      [,1] [,2]
## [1,]  0.1  0.3
## [2,]  0.2  0.4
```

```r
x %*% y  ## true matrix multiplication
```

```
##      [,1] [,2]
## [1,]   40   40
## [2,]   60   60
```
