---
title: Automated Data Download
author: admin
date: '2021-08-01'
slug: []
categories:
  - R
tags:
  - download data
  - Date variables
  - gsub
  - Excel
  - Loops
  - tutorials
  - RMarkdown
  - blogdown
  - Rmd
  - knitr
subtitle: ''
summary: ''
authors: []
lastmod: '2021-08-01T20:39:20+08:00'
featured: no
disable_jquery: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---



One of the advantages of R is the ability to get resources directly from source pages. This post will show you some helpful code when you want to get many files from web pages, saving a lot of time and structure everything nicely.

What you will learn

* Download pages
* Create files
* Replace text
* Unzip folders

## Libraries

There are over 100k packages in R. I will use the **pacman** package to load multiple libraries inside the function.


```r
#install.packages(pacman)
#install.packages(purrr)
pacman::p_load(purrr) #raster, rgdal, rgeos, stringr
```

Let's find a database. For this example, we will use rainfall data from [chirps](). All my files have links following a pattern, thus making this example plausible. We will identify those similar elements and replace them accordingly.

## Download a file

Let's begin by downloading the following file:

`https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_daily/tifs/p05/1987/chirps-v2.0.1987.01.04.tif.gz`

We will use the function download.file from utils. If you check its documentation, type `?download.file` in your console, the window will say: *This function can be used to download a file from the Internet*. Exactly what we want; now we need to add a few elements to make it work `url` and `destfile` at least.

```
file_url <- "https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_daily/tifs/p05/1982/chirps-v2.0.1982.01.01.tif.gz"
# file_url <- paste0("https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_daily/tifs/p05/",
#                    "1998/chirps-v2.0.1998.02.11.tif.gz")
download.file(url = file_url,
              destfile = basename(file_url))
```
I added two `file_url` options; there may be some error if you use the full link. If you do, then paste text without spaces, `paste0()`, to build the link.

## Multiple files

Our objective is to get multiple files; it could be months, days, or years of data. Then we can follow these steps:

### Identify similarities

Identify what is similar for all those paths and store that information as a vector; here, I used `pth`.

```r
pth <- 'https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_daily/tifs/p05/'
```

### Identify  differences

We see that years and dates are different among files, so we want to create a vector that stores all those values with `.` and `-` as required.


```r
year=2016
dates <- seq(as.Date(paste0(year, "-01-01")), as.Date(paste0(year, "-12-31")), by="days")
dates = gsub(pattern = '-', replacement = '.', x = as.character(dates))
data_source<-'/chirps-v2.0.'
file_extension<-'.tif.gz'
# join all the parts to have the final links
paths <- paste0(pth, year, data_source, dates, file_extension)
head(paths,2)
```

```
## [1] "https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_daily/tifs/p05/2016/chirps-v2.0.2016.01.01.tif.gz"
## [2] "https://data.chc.ucsb.edu/products/CHIRPS-2.0/global_daily/tifs/p05/2016/chirps-v2.0.2016.01.02.tif.gz"
```
Our links are ready so that we can place them inside the previous function.


```r
download.file(url = paths,
              destfile = basename(file_url))
```

## Function

We could have many years to build a function to make sure we have the correct files for each year.
- Date vector with seq and as.Date
- Replace strings with gsub
- Join vectors and strings with paste0



```r
download_Online_data <- function(year){
  #print(year) # leave some guides so you know which year, file
  dates <- seq(as.Date(paste0(year, "-01-01")), as.Date(paste0(year, "-12-31")), by="days")
  dates <- gsub('-', '.', as.character(dates))
  paths <- paste0(pth, year, '/chirps-v2.0.', dates, '.tif.gz')

  lapply(1:length(paths), function(k){
    download.file(url = paths[k],
                  destfile = paste0('.', basename(paths[k]))) # for this page do not add w as type="w" it will
  })
}
```

### Selected data

Let's test the above function to get daily data files between 1990 and 2020


```r
year <- 1990:2020
purrr::map(.x = year, .f = download_Online_data)
```

You will get a new window and the R will get you the files you need.

> To unzip these files use the following:


```r
R.utils::gunzip("path/file_name", remove = FALSE)
```


## Summary

* Find your links and check their structure ????.
* Create vectors that are match the required links ????.
* Use download.file to get your data ???	.
* Add everything into a function to get all the data you need ????	.

***

These were some of the steps that I followed to get rainfall data from [CHIRPS](https://www.chc.ucsb.edu/data/chirps). Multiple databases will have similar structures; you got some ideas to stop clicking every link you need ????	. If you liked this post, share it with your friends and on your social media. Constantly check my page for more practical and efficient R tutorials.



