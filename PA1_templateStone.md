
#  Reproducible Research Project #1
#  This assignment maks use of data frm a personnnel activity monitoring device. 
#  This device collects data at 5 minute intervals through out the day. 
#  Data collected during the months of Octber and November, 2012 

#  setwd("/Users/sstone25/datasciencecoursera/Reproducible Research")

#  Part 1 - Loading and preprocessing the data
#  =============================================
#  Show any code that is needed to:
#  Load the data (i.e. read.csv())
#  Process/transform the data (if necesary) into a format sutable for your analysis
#  download and store the file in the Working directory
#  Load data with read.csv(). Used colClasses to convert date column from Factor to Character
```{r}
activity<-read.csv("activity.csv",  header = TRUE, sep = ",", colClasses = c("numeric", "character", "integer"))
```
```{r}
activity2 <- na.omit(activity)
```
#  Part #2 - What is mean total number of steps taken per day?
#  ============================================================
#  Calculate the total number of steps taken per day
```{r}
activitySteps <- aggregate(activity2$steps, list(Date = activity2$date), FUN = "sum")$x
activitySteps
```
#  Calculate and report the mean of the total number of steps taken per day
```{r}
stepsMean <- mean(activitySteps)
stepsMean
```
#  Make a histogram of the total number of steps taken each day
```{r setoptions, echo=TRUE}
steps_hist <- hist(activity2$steps)
```
#  Calculate and report the median of the total number of steps taken per day
```{r}
stepsMedian <- median(activitySteps)
stepsMedian
```
#  Part #3 What is the average daily activity pattern?  
#  =================================================================================
#  Make a time series plot (i.e. type = "l")ofthe 5-minute interval (x-axis) and the average .  #  number of steps taken, averaged across all days (y-axis)
```{r}
activitySteps2<- aggregate(activity2$steps, list(interval = activity2$interval), FUN = "sum")$x
activitySteps2
library(ggplot2)
ggplot(activity2, aes(x = interval, y = steps)) + geom_line(color = "red") + labs(title = "Interval vs Steps")
```
#  Which 5-minute interval, on average across all the days in the dataset, contains the maximum #  number of steps?
```{r}
library(dplyr)
max_steps2 <- arrange(activity2, desc(steps))
head(max_steps2, n = 1L)
```
#  Part #4 Imputing missing values
==========================================================================
#  Note that there are a number of days/intervals where there are missing values (coded as NA). #  The presence of missing days may introduce bias into calculations or summaries of the data.
#  Calculate and report the total number of missing values in the dataset (i.e. the total # #  #  number of rows with NAs)
```{r}
na_dat <- sum(is.na(activity))
head(na_dat, n = 1L)
```
#  Devise a strategy for filling in all of the missing values in the dataset. The strategy does #  not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
#Create a new dataset that is equal to the original dataset but with the missing data filled in
```{r}
gnrlMean <- mean(activity2$steps)
gnrlMean
```
```{r}
new_activity <- activity 
for (i in 1:nrow(new_activity)) {
    if (is.na(new_activity$steps[i])) {
        new_activity$steps[i] <- 37.3
    }
}
View(new_activity$steps)
```

#  Make a histogram of the total number of steps taken each day. 
```{r}
steps_hist2 <- hist(activity2$steps)
```
#  Calculate and report the mean total number of steps taken per day. Do these #  #  #  values differ from the estimates from the first part of the assignment? What is #  the impact of imputing missing data #  on the estimates of the total daily number of steps?
```{r}
activitySteps2 <- aggregate(new_activity$steps, list(Date = activity$date), FUN = "sum")$x
stepsMean2 <- mean(activitySteps2)
stepsMean2
```
#  Calculate and report the median total number of steps taken per day.
```{r}
stepsMedian2 <- median(activitySteps2)
stepsMedian2
```
#  Do these values differ from the estimates from the first part of the assignment? 
#  Mean
#  Part #1 = 10766.19
#  Part #2 = 1981.278

#  Median
# Part #1 = 10765
# Part #2 = 1808

#  What is the impact of imputing missing data on estimates of total daily number of steps?
# The results for the part #1 of the excercise are much higher and the part #2.

#  Part #5 - Are there differences in activity patterns between weekdays and weekends?
============================================================================================
#  For this part the weekdays() function may be of some help here. Use the dataset with the #  #  filled-in missing values for this part.
#  Create a new factor variable in the dataset with two levels - "weekday" and "weekend" #  #  #  indicating whether a given date is a weekday or weekend day.
```{r}
new_activity[, "new_weekdays"] <- new_weekdays
str(new_weekdays)
```
#  Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval #  (x-axis) and the average number of steps taken, averaged across all weekday days or weekend #  days (y-axis). See the README file in the GitHub repository to see an example of what this #  plot should look like using simulated data.
```{r}
week_weekend <- new_activity[new_activity$new_weekdays == "Saturday" | new_activity$new_weekdays == "Sunday" | new_activity$new_weekdays == "Monday" | new_activity$new_weekdays == "Tuesday" | new_activity$new_weekdays == "Wednesday" | new_activity$new_weekdays == "Thursday" | new_activity$new_weekdays == "Friday", ] 
str(week_weekend)
```
```{r}
week_weekend$newDate <- NULL
newData <- filter(week_weekend, new_weekdays == "Saturday" | new_weekdays == "Sunday") 
ggplot(newData, aes(x = interval, y = steps)) + geom_line(color = "red") + labs(title = "Weekend Data Interval vs Steps")