<<<<<<< HEAD
__Loading and preprocessing the data__ 

```r
activity <- read.csv(file="activity.csv", colClasses = c("numeric", "character", "numeric"))
head(activity)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
names(activity)
```

```
## [1] "steps"    "date"     "interval"
```

```r
library(lattice)
activity$date <- as.Date(activity$date, "%Y-%m-%d")
```
__What is mean total number of steps taken per day? __
 Calculate the total number of steps taken per day and produce histogram

```r
TotalSteps <- aggregate(steps ~ date, data = activity, sum, na.rm = TRUE)
hist(TotalSteps$steps, main = "Total steps taken per day", xlab = "day", col = "red")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

the mean and median are

```r
TotalSteps <- aggregate(steps ~ date, data = activity, sum, na.rm = TRUE)
mean(TotalSteps$steps)
```

```
## [1] 10766.19
```

```r
median(TotalSteps$steps)
```

```
## [1] 10765
```
__The average daily activity pattern (making a time series plot) and the average number of steps taken__

```r
time_series <- tapply(activity$steps, activity$interval, mean, na.rm = TRUE)
plot(row.names(time_series), time_series, type = "l", xlab = "5min interval", ylab = "Average number of steps across all days", main = "Average daily activity pattern")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 

The maximum number of steps in a 5-minute interval (on average across all days in dataset)


```r
max_interval <- which.max(time_series)
names(max_interval)
```

```
## [1] "835"
```

__Inputting missing values__
Calculating and reporting the total number of missing values in the dataset

```r
activity_NA <- sum(is.na(activity))
activity_NA
```

```
## [1] 2304
```
A method for filling in all of the missing values in the dataset -- replacing the missing values (NA's) with the mean of the 5min interval

```r
StepsAverage <- aggregate(steps ~ interval, data = activity, FUN = mean)
fillNA <- numeric()
for(i in 1:nrow(activity)){
  obs <- activity[i, ]
  if(is.na(obs$steps)){
    steps <- subset(StepsAverage, interval == obs$interval)$steps
  } else{
    steps <- obs$steps
  }
  fillNA <- c(fillNA, steps)
}
```

Creating a new dataset that is equal to the original dataset, but with the missing data filled in

```r
new_activity <- activity
new_activity$steps <- fillNA
```

Make a histogram of the total number of steps taken each day, calculate & report the mean and median

```r
StepsTotal2 <- aggregate(steps ~ date, data = new_activity, sum, na.rm = TRUE)
hist(StepsTotal2$steps, main = "Total number of steps taken each day", xlab = "day", col = "red")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png) 

```r
mean(StepsTotal2$steps)
```

```
## [1] 10766.19
```

```r
median(StepsTotal2$steps)
```

```
## [1] 10766.19
```
The mean is the same, but the median is different

__Are there differences in activity patterns between weekdays and weekends?__
Creating a new factor variable in the dataset with two levels, weekday and weekend

```r
day <- weekdays(activity$date)
daylevel <- vector()
for(i in 1:nrow(activity)){
  if(day[i]== "Saturday"){
    daylevel[i] <- "Weekend"
  } else if (day[i]== "Sunday"){
    daylevel[i] <- "Weekend"
  } else {
    daylevel[i] <- "Weekday"
  }
}
activity$daylevel <- daylevel
activity$dayleel <- factor(activity$daylevel)

StepsByDay <- aggregate(steps ~ interval + daylevel, data = activity, mean)
names(StepsByDay) <- c("interval", "daylevel", "steps")
```

make a panel plot containing a time series plot of the 5 minute interval and the average number of steps taken across all weekday days or weekend days

```r
xyplot(steps ~ interval | daylevel, StepsByDay, type = "l", layout = c(1,2), xlab="Interval", ylab= "Number of Steps")
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png) 


=======
---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data



## What is mean total number of steps taken per day?



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
