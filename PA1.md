# Reproducible Research: Peer Assessment 1
LuFo  
August 10, 2015  
---
title: "Reproducible Research - Peer Assessment 1"
output: html_document
---

# 1. Loading and preprocessing the data
File has been downloaded <https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip> and unzipped to local drive (These steps are jumped over in this description, I experienced issues while unzipping remote files).

```r
download.file("https://d396qusza40orc.cloudfront.net/repdata/data/activity.zip", 
              "data.zip", 
              "wget", 
              quiet = FALSE, 
              mode = "w",
              cacheOK = TRUE,
              extra = getOption("download.file.extra"))
df <- read.csv(unz("data.zip", "activity.csv"))
```
Check data:

```r
str(df)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```
- column data is a factor and not as.Date()

```r
sum(complete.cases(df))
```

```
## [1] 15264
```
- there are 2304 incomplete observations (17568 rows - 15264 complete)

Keeping this in mind it seems to be ok for further processing.

# 2. What is mean total number of steps taken per day? 
While there are several ways of subsetting or sub summation I decide for convenient dplyr package.

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```
## 2.1 Calculate the total number of steps taken per day

```r
stepsperday <- df %>% na.omit() %>% group_by(date) %>% summarise(totalSteps=sum(steps))
```
List of steps per day is calculated, see head:

```r
head(stepsperday)
```

```
## Source: local data frame [6 x 2]
## 
##         date totalSteps
## 1 2012-10-02        126
## 2 2012-10-03      11352
## 3 2012-10-04      12116
## 4 2012-10-05      13294
## 5 2012-10-06      15420
## 6 2012-10-07      11015
```
## 2.2 Make a histogram of the total number of steps taken each day

```r
library(ggplot2)
```

```r
qplot(totalSteps,
      data=stepsperday,
      geom="histogram",
      binwidth=2500,
      main="Histogram on steps per day",
      ylab="number of days",
      xlab="steps per day"
     )
```

![](PA1_files/figure-html/unnamed-chunk-8-1.png) 

## 2.3 Calculate and report the mean and median of the total number of steps taken per day

# 3. What is the average daily activity pattern?
##  3.1 Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

## 3.2 Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


# 4. Inputing missing values
## 4.1 Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

## 4.2 Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

## 4.3 Create a new dataset that is equal to the original dataset but with the missing data filled in

## 4.4 Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

# 5. Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

## 5.1 Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

## 5.2 Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.
