---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

Load the data and convert the values in date column into an R dates

```r
activity <- read.csv(unzip("activity.zip"), header=TRUE, quote="\"", sep=",")
activity$date <- as.Date(activity$date, "%Y-%m-%d")
```

## What is mean total number of steps taken per day?

Calculate the number of steps per day

```r
steps_per_day <- tapply(activity$steps, activity$date, sum, na.rm = TRUE)
```

Generate the histogram of the total number of steps taken each day

```r
hist(steps_per_day, 
     xlab="Number of steps", 
     main="Total number of steps taken each day")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

Calculate the mean and median number of steps taken per day

```r
mean(steps_per_day, na.rm = TRUE)
```

```
## [1] 9354.23
```

```r
median(steps_per_day, na.rm = TRUE)
```

```
## [1] 10395
```

## What is the average daily activity pattern?

Calculate the average number of steps for each interval

```r
steps_per_interval <- tapply(activity$steps, activity$interval, mean, na.rm = TRUE)
```

Generate the time series chart

```r
plot(x = names(steps_per_interval), 
     y = steps_per_interval, 
     type="l",
     xlab = "Interval",
     ylab = "Number of steps",
     main = "Average number of steps per 5 minutes interval")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

Determine which interval contains the maximum number of steps

```r
max_step <- which(steps_per_interval == max(steps_per_interval))
names(max_step)
```

```
## [1] "835"
```

## Imputing missing values

Calculate the total number of missing values


```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```

Create a new dataset where missings value are replaced by the average for the interval

```r
activity_fixed <- activity
missing_steps <- is.na(activity_fixed$steps)
activity_fixed[missing_steps,]$steps <- steps_per_interval[as.character(activity_fixed[missing_steps,]$interval)]
```
Calculate the number of steps per day

```r
steps_per_day_fixed <- tapply(activity_fixed$steps, activity_fixed$date, sum)
```

Generate the histogram of the total number of steps taken each day

```r
hist(steps_per_day_fixed, 
     xlab="Number of steps", 
     main="Total number of steps taken each day")
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png)

Calculate the mean and median number of steps taken per day

```r
mean(steps_per_day_fixed)
```

```
## [1] 10766.19
```

```r
median(steps_per_day_fixed)
```

```
## [1] 10766.19
```

Imputing missing value has caused both the mean and median number of steps to go up.

## Are there differences in activity patterns between weekdays and weekends?

Create a new factor variable to differentiate week days and week ends.

```r
activity_fixed$day_type <- ifelse(weekdays(activity_fixed$date) %in% c("Saturday", "Sunday"),"weekend","weekday")
activity_fixed$day_type <- factor(activity_fixed$day_type)
```

Calculate the average number of steps for each interval

```r
activity_weekday <- subset(activity_fixed, day_type == "weekday")
activity_weekend <- subset(activity_fixed, day_type == "weekend")

steps_per_interval_weekday <- tapply(activity_weekday$steps, activity_weekday$interval, mean)

steps_per_interval_weekend <- tapply(activity_weekend$steps, activity_weekend$interval, mean)
```

Generate the time series charts

```r
plot(x = names(steps_per_interval_weekday), 
     y = steps_per_interval_weekday, 
     type="l",
     xlab = "Interval",
     ylab = "Number of steps",
     main = "Average number of steps on weekdays per 5 minutes interval",
     col = "red")
```

![plot of chunk unnamed-chunk-15](figure/unnamed-chunk-15-1.png)

```r
plot(x = names(steps_per_interval_weekend), 
     y = steps_per_interval_weekend, 
     type="l",
     xlab = "Interval",
     ylab = "Number of steps",
     main = "Average number of steps on weekends per 5 minutes interval",
     col = "blue")
```

![plot of chunk unnamed-chunk-15](figure/unnamed-chunk-15-2.png)

