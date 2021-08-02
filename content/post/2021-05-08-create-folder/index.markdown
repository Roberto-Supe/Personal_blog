---
title: Create folders and files with for-loop
author: admin
date: '2021-08-01'
slug: []
categories:
  - R
  - tidyverse
tags:
  - folders
  - text
  - Excel
  - for-loop
  - tutorials
  - Academic
  - write files
  - RMarkdown
  - blogdown
  - Rmd
  - knitr
subtitle: ''
summary: ''
authors: []
lastmod: '2021-08-01T11:57:57+08:00'
featured: no
disable_jquery: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


As an environmentalist, I have worked with historical data and modeling. Some programs will need you to have individual folders and individual data files with certain extensions for each day, month, year, or study location that you want to analyze. What if your data is in a single file 🤯. Making 10files will be ok, but 2000 will take a while nonetheless; thanks to the power of R, we can do that in a few minutes ⏱.

What you will learn:

* For-loops
* Work with excel files
* Create folder
* Subset data
* Write and save text files

## Libraries

Generally, I like to work with CSV or TXT files. Nonetheless, it is common to have data in excel files. To avoid changing the format, I will use the package `readxl` to import .xlsx files.


```r
#install.packages("readxl")
library(readxl)
library(tidyverse)
library(utils)
```

## Excel files

The example file will have two locations, one sheet for each site. For each area, there are multiple variables with daily data between 1979 and 2100. You can get the {{< staticref "post/climate_data.xlsx" "newtab" >}}Dataset for this tutorial{{< /staticref >}}

> Objective: Have individual text files for each year and locations stored in the corresponding year folder.


### Read excel files
Let's begin by reading the file


```r
data<- readxl::read_excel("climate_data.xlsx",sheet = 1,skip = 0)
```

With the `read_excel` you can specify the sheet as well as skip a few rows.
### Subset your data

Now, we need to filter the data. We will select the first 4 columns and data from 2000.

```r
dt1 <- dplyr::filter(data[,1:4],Year==2002) #if you get some problems when filtering years, you can use Year=factor(year)
head(dt1)
```

```
## # A tibble: 6 x 4
##   Year  Day   Maxtemp Mintemp
##   <chr> <chr>   <dbl>   <dbl>
## 1 2002  1       -15.6   -23.2
## 2 2002  2       -14.7   -21.5
## 3 2002  3       -11     -18.3
## 4 2002  4       -11.2   -19.4
## 5 2002  5       -11.2   -18
## 6 2002  6        -8.3   -17.5
```
There are many ways to subset your data, but one of the most straightforward ways is using `filter` from `dplyr`. As I did with data[,1:4], this specifies the dimensions that I want to use data[rows,collumns]. Tip: To remember which one goes first, think of a <span style="color: red;">**R**ed **C**hicken</span> (<span style="color: red;">**R**</span>ows, <span style="color: red;">**C**</span>olumns).

> This is also one of my favorite packages because you can integrate extended code for data analysis and prosses.

Now that we selected our year, we can write the text file that we need.


```r
write.table(dt1[,2:4], #remove additional variables from your filtered data
            file="station_1_year_2000.txt", #filemane of your file.
            #if nested folders then add the folder_name/file_name for each file
            col.names=T,row.names = F,quote = FALSE) #remove all the row.names and " " from col.names
```


## For-loop

As I said before, we will have many years and locations, so we want to run that for all our data. When something is the same repeated, then for-loops are a great alternative.


```r
study_year<-min(data$Year):max(data$Year) #store all the years in a new vector
tail(study_year) # have a look at last five variables
```

```
## [1] 2095 2096 2097 2098 2099 2100
```

### Create folders

Let's create our folders and have a simple example of what we will do with the for-loop. The structure that we need to follow is this `for (variable in vector)`.


```r
#for (variable in vector)
for(year in # year can be any text
    study_year[1:2]){ #our vector
  print(as.character(year)) # show me which year the loop is running
  dir.create(path = paste(year)) # create the folder named by year
}
```

### Magic

It is common to use guides with for-loops to know which iteration the loop is doing; I used `print` to display a message.


```r
for (j in 1:2) {  # how many pages you have 1:n pages
  data<-readxl::read_excel("climate_data.xlsx",sheet = j)
  filename = paste("station ",j,".txt",sep = "") #file name for each i year

  for (i in c(1:length(study_year))){ #i in as many years we got
  dt1 <- dplyr::filter(data[,1:4],Year==study_year[i]) #filter the data
  write.table(dt1[,2:4], #remove additional variables from your filtered data
            file="station_1_year_2000.txt", #filemane of your file.
            #if nested folders then add the folder_name/file_name for each file
            col.names=T,row.names = F,quote = FALSE) #remove all the row.names and " " from col.names
  print(paste("done with year",study_year[i],"Station",j)) # message
  }
}
```


## Summary

Writing files is easy with R and will save you a lot of time. Some of the take away messages and points to follow are:

* Read your long files 📚.
* Filter your information 📋.
* Create folders 📂.
* Use a for-loop to filter and save to the right folder ✅.

***

These were some of the steps that I followed to create over 2000 files. You can replace the extension make sure to change the function, e.g., write.csv(). Hopefully, you have a long data set and you need to create new files soon. You got it under control, and it will just take a few minutes. If you liked this post, share it with your friends and on your social media. Constantly check my page for more practical and efficient R tutorials 👏.


