# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
activity <- read.csv(unzip("activity.zip"), header=TRUE, quote="\"", sep=",")
activity$date <- as.Date(activity$date, "%Y-%m-%d")
```

## What is mean total number of steps taken per day?

Remove rows with missing number of steps 

```r
act <- subset(activity, !is.na(steps))
```

Calculate the number of steps per day

```r
steps_per_day <- tapply(act$steps, act$date, mean)
```

Generate the histogram of the total number of steps taken each day

```r
hist(steps_per_day, 
     xlab="Number of steps", 
     main="Total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

Calculate the mean and median number of steps taken per day

```r
mean(steps_per_day)
```

```
## [1] 37.3826
```

```r
median(steps_per_day)
```

```
## [1] 37.37847
```

## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
