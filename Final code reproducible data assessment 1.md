---
title: "PA1_template"
author: "Fides"
date: "2/25/2021"
output: html_document
---

#1-Loading and preprocessing the data
```{r}
activity <- read.csv(file="~/Desktop/activity.csv", header=TRUE, sep=",", na.strings = "NA") #load data

library("dplyr")
library("ggplot2")
library("data.table")
library("lubridate")
library("httpuv")
```
##Clean up date class
```{r}
activity$date <- ymd(activity$date)
```
##Remove missing values
```{r}
activity1 <- na.omit(activity)
```
##Create new variables
```{r}
activity$monthly <- as.numeric(format(activity$date, "%m"))
activity$daily <- as.numeric(format(activity$date, "%d"))
```
##Check data
```{r}
summary(activity1)
```
## 2-Calculate total number of steps
```{r}
# Summarize data for ggplot
activity2 <- summarize(group_by(activity1, date), daily.step=sum(steps))
mean.activity <- as.integer(mean(activity2$daily.step))
median.activity <- as.integer(median(activity2$daily.step))

# Plot histogram
plot.steps.day <- ggplot(activity2, aes(x=daily.step)) +
  geom_histogram(binwidth = 1000, aes(y=..count.., fill=..count..)) +
  geom_vline(xintercept=mean.activity, colour="red", linetype="dashed", size=1) +
  geom_vline(xintercept=median.activity, colour="green" , linetype="dotted", size=1) +
  labs(title="Histogram of Number of Steps taken each day", y="Frequency", x="Daily Steps")
plot.steps.day
```
# Mean total number of steps taken per day
```{r}
mean.activity
```
# Median total number of steps taken per day
```{r}
median.activity
```
