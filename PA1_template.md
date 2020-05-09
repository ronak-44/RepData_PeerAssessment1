---
title: "assignment"
author: "ronak"
date: "08/05/2020"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
  word_document: default
---
# Programming Assignment

## Loading Data
Load the data (i.e. read.csv())


```r
data<-read.csv('activity.csv')
```

## Mean and Median 
Plot of daily step count
Mean and Median of daily step count


```r
library(ggplot2)
steps<-aggregate(steps ~ date, data = data, sum)
g<-ggplot(steps,aes(steps)) + geom_histogram()
print(g)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](PA1_template_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
mean(steps$steps,na.rm=TRUE)
```

```
## [1] 10766.19
```

```r
median(steps$steps,na.rm=TRUE)
```

```
## [1] 10765
```

## 5 minute interval
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
avg<-aggregate(steps~interval,data,FUN=mean)
g<-ggplot(avg,aes(interval,steps)) + geom_line()
print(g)
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
avg[avg$steps==max(avg$steps),1:2]
```

```
##     interval    steps
## 104      835 206.1698
```

## Inputing missing data
The total amount of NA's and the percentage of missing step data.
Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)
Devise a strategy for filling in all of the missing values in the dataset.Replacing NAs with mean of % minute interval.
Create a new dataset that is equal to the original dataset but with the missing data filled in.
Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
nas <- is.na(data$steps)
sum(nas)
```

```
## [1] 2304
```

```r
mean(nas)
```

```
## [1] 0.1311475
```

```r
check<-is.na(data$steps)
data[check,1]<-avg[avg$interval==data[check,]$interval,"steps"]
steps2<-aggregate(steps ~ date, data = data, sum)
g1<-ggplot(steps2,aes(steps)) + geom_histogram()
print(g1)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
mean(steps2$steps,na.rm=TRUE)
```

```
## [1] 10766.19
```

```r
median(steps2$steps,na.rm=TRUE)
```

```
## [1] 10765.59
```
Marginal change in mean an median



## Weekday vs Weekend
Create a new factor variable in the dataset with two levels -- "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.
Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). The plot should look something like the following, which was created using simulated data:

```r
data$date<-as.Date(data$date)
data$day<-weekdays(data$date)
data$w<-data$day %in% c('Monday','Tuesday','Wednesday','Thursday','Friday')
data$w[data$w=="TRUE"]<-'weekday'
data$w[data$w==FALSE]<-'weekend'
averages <- aggregate(steps ~ interval + w, data, mean)
ggplot(averages, aes(interval, steps)) + geom_line() + facet_grid(w ~ .) 
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

