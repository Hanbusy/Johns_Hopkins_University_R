---
title: "Subsetting"
author: "Jinsu Han"
date: "Tuesday, February 17, 2015"
output: html_document
---

## Subsetting

There are a number of operators that can be used to extract subsets of R objects.  

* `[` always returns an object of the same class as the original; can be used to select more than one element(there is one exception)  
* `[[` is used to extract elements of a list or a data frame; it can only be used to extract a single element and the class of the returned object will not necessarily be a list or data frame  
* `$` is used to extract elements of a list or data frame by name; semantics are similar to that of `[[`  


```r
x <- c("a","b","c","c","d","a")
x[1]
```

```
## [1] "a"
```

```r
x[1:4]
```

```
## [1] "a" "b" "c" "c"
```

```r
x[x>"a"]
```

```
## [1] "b" "c" "c" "d"
```

```r
u <- x>"a"
u
```

```
## [1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE
```

```r
x[u]
```

```
## [1] "b" "c" "c" "d"
```

## Subsetting Lists

```r
x <- list(foo = 1:4, bar=0.6)
x[1]
```

```
## $foo
## [1] 1 2 3 4
```

```r
x[[1]]
```

```
## [1] 1 2 3 4
```

```r
x$bar
```

```
## [1] 0.6
```

```r
x[["bar"]]
```

```
## [1] 0.6
```

```r
x["bar"]
```

```
## $bar
## [1] 0.6
```


```r
x <- list(foo = 1:4, bar = 0.6, baz = "hello")
x[c(1,3)]
```

```
## $foo
## [1] 1 2 3 4
## 
## $baz
## [1] "hello"
```

The `[[` operator can be used with computed indices; `$` can inly be used with literal names.  

```r
x <- list(foo = 1:4, bar = 0.6, baz = "hello")
name <- "foo"
x[[name]] ## computed index for 'foo'
```

```
## [1] 1 2 3 4
```

```r
x$name  ## element 'name' doesn't exitst!
```

```
## NULL
```

```r
x$foo
```

```
## [1] 1 2 3 4
```

## Subsetting Nested Elements of a List
The `[[` can take an integer sequence.

```r
x <- list(a = list(10,12,14), b=c(3.14,2.81))
x[[c(1,3)]]
```

```
## [1] 14
```

```r
x[[1]][[3]]
```

```
## [1] 14
```

```r
x[[c(2,1)]]
```

```
## [1] 3.14
```

## Subsetting a Matrix

Matrices can be subsetted in the usual way with (i,j) type indices.  

```r
x <- matrix(1:6, 2,3)
x[1,2]
```

```
## [1] 3
```

```r
x[2,1]
```

```
## [1] 2
```
Indices can also be missing.

```r
x[1, ]
```

```
## [1] 1 3 5
```

```r
x[, 2]
```

```
## [1] 3 4
```
By default, when a single element of a matrix is retrieved, it is returned as a vector of length 1 rather than a 1 x 1 matrix. This behavior can be turned off by setting `drop = FALSE`.


```r
x <- matrix(1:6,2,3)
x[1,2]
```

```
## [1] 3
```

```r
x[1,2, drop = FALSE]
```

```
##      [,1]
## [1,]    3
```
Similarly, subsetting a single column or a single row will give you a vector, not a matrix (by default).

```r
x <- matrix(1:6,2,3)
x[1, ]
```

```
## [1] 1 3 5
```

```r
x[1,,drop = FALSE]
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
```

## Partial Matching
Partial matching of names is allowed with `[[` and `$`

```r
x <- list(aardvark = 1:5)
x$a
```

```
## [1] 1 2 3 4 5
```

```r
x[["a"]]
```

```
## NULL
```

```r
x[["a", exact = FALSE]]
```

```
## [1] 1 2 3 4 5
```
## Removing NA Values
A common task is to remove missing values(NAS).

```r
x <- c(1,2,NA,4,NA,5)
bad <- is.na(x)
x[!bad]
```

```
## [1] 1 2 4 5
```
What if there are multiple things and you want to take the subset with no missing values?

```r
x <- c(1,2,NA,4,NA,5)
y <- c("a","b",NA,"d",NA,"f")
good <- complete.cases(x,y)
good
```

```
## [1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE
```

```r
x[good]
```

```
## [1] 1 2 4 5
```

```r
y[good]
```

```
## [1] "a" "b" "d" "f"
```

```r
airquality[1:6, ]
```

```
##   Ozone Solar.R Wind Temp Month Day
## 1    41     190  7.4   67     5   1
## 2    36     118  8.0   72     5   2
## 3    12     149 12.6   74     5   3
## 4    18     313 11.5   62     5   4
## 5    NA      NA 14.3   56     5   5
## 6    28      NA 14.9   66     5   6
```

```r
good <- complete.cases(airquality)
airquality[good, ][1:6, ]
```

```
##   Ozone Solar.R Wind Temp Month Day
## 1    41     190  7.4   67     5   1
## 2    36     118  8.0   72     5   2
## 3    12     149 12.6   74     5   3
## 4    18     313 11.5   62     5   4
## 7    23     299  8.6   65     5   7
## 8    19      99 13.8   59     5   8
```
