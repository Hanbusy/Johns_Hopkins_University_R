---
title: "Reading Tabular Data"
author: "Jinsu Han"
date: "Tuesday, February 17, 2015"
output: html_document
---

## Reading Data
There are a few principal functions reading data into R.  

* `read.table, read.csv`, for reading tabular data   
* `readLines`, for reading lines of a text file  
* `source`, for reading in R code files(`inverse` of dump)  
* `dget', for reading in R code files(`inverse` of `dput`)  
* `load`, for reading in saved workspaces   
* `unserialize`, for reading single R objects in binary form   

## Writing Data

There are analogous functions for writing data to files   

* `write.tables`  
* `writeLines`  
* `dump`  
* `dput`  
* `save`  
* `serialize`  

## Reading Data Files with read.table

The `read.table` function is one of the most commonly used functions for reading data. It has a few important arguments:  

* `file`, the name of a file, or a connection  
* `header`, logical indicating if the file has a header line  
* `sep`, a string indicating how the columns are separated  
* `colClasses`, a character vector indicating the class of each column in the dataset  
* `nrows`, the number of rows in the dataset  
* `comment.char`, a character string indicating the comment character  
* `skip`, the number of lines to skip from the beginning  
* `stringsAsFactors`, should character variables be coded as factors?  


## read.table

For small to moderately sized datasets, you can usually call read.table without specifying any other arguments

```
data <- read.table("foo.txt")
```

R will automatically   

* skip lines that begin with a #  
* figure out how many rows there are (and how much memory needs to be allocated)  
* figure what type of variable is in each column of the table Telling R all these things directly makes R run faster and more efficiently  
* `read.csv` is identical to read.table except that the default separator is a comma  

## Reading in Larger Datasets with read.table  

With much larger datasets, doing the following things will make your life easier and will prevent R from choking.  

* Read the help page for read.table, which contains many hints  
* Make a rough calculation of the memory required to store your dataset.If the dataset is larger than the amount of RAM on your computer, you can probbly stop right here  
* Set `comment.char` = "" if there are no commented lines in your file  
* Use the `colClasses` argument. Specifying this option instead of using the default can make 'read.table' run MUCH faster, often twice as fast. In order to use this option, you have to know the class of each column in your data frame. If all of the columns are "numeric", for example, then you can just set `cloClasses = numeric"`. A quick an dirty way to figure out the classes of each column is the following  
```
initial <- read.table("datatable.txt", nrows = 100)
classes <- sapply(initial, class)
tabAll <- read.table("datatable.txt", colClasses = classes)
```  

* Set `nrows`. This doesn't make R run faster but it helps with memory usage. A mild overestimate is okay. You can use the Unix tool `wc` to calculate the number of lines in a file   

## Know Thy System  

In general, when using R with larger datasets, it's useful to know a few things about your system.   
* How much memory is available?  
* What other applications are in use?  
* Are there other users logged into the same system?  
* What operating system?  
* Is the OS 32 ir 64 bit?  

## Calculating Memory Requirements  

I have a data frame with 1,500,000 rows and 120 columns, all of which are numeric data. Roughly, how much memory is required to store this data frame?  

1,500,000 x 120 x 8 bytes/numeric
= 1.34 GB


## Textual Formats  

* `dumping`, and dputing are useful because the resulting textual format is edit-able, and in the case of corruption, potentially recoverable  
* `Unlike` writing out a table or csv file, `dump` and `dput` preserve the metadata(sacrificing some readability), so that another user doesn't have to specify it all over again  
* `Textual` formats can work much better with version control programs like subversion or git which can only track changes meaning fully in text files.  
* Textual formats can be longer-lived; if there is corruption somewhere in the file, it can be easier to fix the problem  
* Textual formats adhere to the "Unix philosophy"  
* Downside: The format is not very space-efficient  

## dput-ting R Objects

Another way to pass data around is by deparsing the R object with dput and reading it back in using dget.


```r
y <- data.frame(a = 1, b ="a")
dput(y)
```

```
## structure(list(a = 1, b = structure(1L, .Label = "a", class = "factor")), .Names = c("a", 
## "b"), row.names = c(NA, -1L), class = "data.frame")
```

```r
dput(y,file = "y.R")
new.y <-dget("y.R")
new.y
```

```
##   a b
## 1 1 a
```

## Dumping R Objects

Multiple objects can be deparsed using the dump function and read back in using source  


```r
x <- "foo"
y <- data.frame(a = 1, b = "a")
dump(c("x","y"), file = "data.R")
rm(x,y)
source("data.R")
y
```

```
##   a b
## 1 1 a
```

```r
x
```

```
## [1] "foo"
```

## Interfaces to the Outside World

Data are read in using connection interfaces. Connections can be made to files (moset common) or to other more exotic things.   

* `file`, opens a connection to a file  
* `gzfile`, opens a connection to a file compressed with gzip  
* `bzfile`, opens a connection to a file compressed with bzip2  
* `url`, opens a connection to a webpage

## File Connections

```r
str(file)
```

```
## function (description = "", open = "", blocking = TRUE, encoding = getOption("encoding"), 
##     raw = FALSE)
```

* `description` is the name of the file  
* `open` is a code indicating  

        - "r" read only  
        - "w" writing ( and initializing a new file)
        - "a" appending
        - "rb","wb","ab" reading, writing, or appending in binary mode(Windows)


## Connections
In general, connections are powerful tools that let you navigate files or other external objects. In practice, we often don't need to deal with the connection interface directly  
```
con <- file("foo.txt", "r")
data <- read.csv(con)
close(con)
```
is the same as
```
data <- read.csv("foo.txt")
```

## Reading Lines of a Text File
```
con <-gzfile("words.gz")
x <- readLines(con,10)
x
```

`writeLines` takes a character vector and writes each element one line at a time to a text file

`readLines` can be useful for reading in lines of webpages

```r
con <- url("http://www.jhsph.edu","r")
x <- readLines(con)
head(x)
```

```
## [1] "<!DOCTYPE html>"                                               
## [2] "<html lang=\"en\">"                                            
## [3] ""                                                              
## [4] "<head>"                                                        
## [5] "<meta charset=\"utf-8\" />"                                    
## [6] "<title>Johns Hopkins Bloomberg School of Public Health</title>"
```
