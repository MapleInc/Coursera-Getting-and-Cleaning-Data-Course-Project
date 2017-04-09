### To create one R script called run_analysis.R that does the following.
Merges the training and the test sets to create one data set.
Extracts only the measurements on the mean and standard deviation for each measurement.
Uses descriptive activity names to name the activities in the data set
Appropriately labels the data set with descriptive variable names.
From the data set in step 4, creates a second, independent tidy data set, averages.txt, with the average of each variable for each activity and each subject.

Tidy Mean and Standard Deviation Data from the UCI HAR Data Set
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
```R
#Load required packages: dplyr and reshape2

library(plyr)
library(reshape2)
```
### Read files
Please make sure tgat you have data in working directory.
```R
#setwd("/Coursera/Getting and Cleanning data week 4/Assignment/UCI HAR Dataset")
getwd()
xTrain <- read.table("train/X_train.txt")
yTrain <- read.table("train/y_train.txt")
subjectTrain <- read.table("train/subject_train.txt")
xTest <- read.table("test/X_test.txt")
yTest <- read.table("test/y_test.txt")
subjectTest <- read.table("test/subject_test.txt")
```
### Merges the training and the test sets to create one data set.
##### create 'x' data set
##### create 'y' data set
##### create 'subject' data set

```R
xData <- rbind(xTrain, xTest)
yData <- rbind(yTrain, yTest)
subjectData <- rbind(subjectTrain, subjectTest)
```
### Extract only the measurements on the mean and standard deviation for each measurement
##### get columns with names containing "mean()" or "std()"
```R
features <- read.table("features.txt")
head(features, 10)
meanStdFeatures <- grep("(mean|std)\\(\\)", features[, 2])
xData <- xData[, meanStdFeatures]
names(xData) <- features[meanStdFeatures, 2]
```
#### Uses descriptive activity names to name the activities in the data set
#### update values with correct activity names
### Appropriately labels the data set with descriptive variable names.
#### correct column name
#### merge a single data set

```R
activities <- read.table("activity_labels.txt")
yData[, 1] <- activities[yData[, 1], 2]
names(yData) <- "activity"
names(subjectData) <- "subject"
tidyData <- cbind(xData, yData, subjectData)

```

####  create a second, independent tidy data set with the average of each variable for each activity and each subject.
```R
meltData <- melt(tidyData,c("subject","activity"))
avgData <- dcast(meltData, subject + activity ~ variable, mean)
write.table(avgData, file="averages.txt", row.name=FALSE) 
write.csv(avgData, "Dataset.csv")
```
####  create a codeBook.csv
```R
codeBook<-as.data.frame(names(avgData))
write.csv(codeBook, "codeBook.csv")
```
# Code Book
#### This code book summarizes the resulting data fields

### Identifiers

* 'subject' - The id of the subject
* 'activity' - The descriptive name of the activity performedwhen the corresponding measurements were taken

### Measurements

* `tBodyAccMeanX`
* `tBodyAccMeanY`
* `tBodyAccMeanZ`
* `tBodyAccStdX`
* `tBodyAccStdY`
* `tBodyAccStdZ`
* `tGravityAccMeanX`
* `tGravityAccMeanY`
* `tGravityAccMeanZ`
* `tGravityAccStdX`
* `tGravityAccStdY`
* `tGravityAccStdZ`
* `tBodyAccJerkMeanX`
* `tBodyAccJerkMeanY`
* `tBodyAccJerkMeanZ`
* `tBodyAccJerkStdX`
* `tBodyAccJerkStdY`
* `tBodyAccJerkStdZ`
* `tBodyGyroMeanX`
* `tBodyGyroMeanY`
* `tBodyGyroMeanZ`
* `tBodyGyroStdX`
* `tBodyGyroStdY`
* `tBodyGyroStdZ`
* `tBodyGyroJerkMeanX`
* `tBodyGyroJerkMeanY`
* `tBodyGyroJerkMeanZ`
* `tBodyGyroJerkStdX`
* `tBodyGyroJerkStdY`
* `tBodyGyroJerkStdZ`
* `tBodyAccMagMean`
* `tBodyAccMagStd`
* `tGravityAccMagMean`
* `tGravityAccMagStd`
* `tBodyAccJerkMagMean`
* `tBodyAccJerkMagStd`
* `tBodyGyroMagMean`
* `tBodyGyroMagStd`
* `tBodyGyroJerkMagMean`
* `tBodyGyroJerkMagStd`
* `fBodyAccMeanX`
* `fBodyAccMeanY`
* `fBodyAccMeanZ`
* `fBodyAccStdX`
* `fBodyAccStdY`
* `fBodyAccStdZ`
* `fBodyAccMeanFreqX`
* `fBodyAccMeanFreqY`
* `fBodyAccMeanFreqZ`
* `fBodyAccJerkMeanX`
* `fBodyAccJerkMeanY`
* `fBodyAccJerkMeanZ`
* `fBodyAccJerkStdX`
* `fBodyAccJerkStdY`
* `fBodyAccJerkStdZ`
* `fBodyAccJerkMeanFreqX`
* `fBodyAccJerkMeanFreqY`
* `fBodyAccJerkMeanFreqZ`
* `fBodyGyroMeanX`
* `fBodyGyroMeanY`
* `fBodyGyroMeanZ`
* `fBodyGyroStdX`
* `fBodyGyroStdY`
* `fBodyGyroStdZ`
* `fBodyGyroMeanFreqX`
* `fBodyGyroMeanFreqY`
* `fBodyGyroMeanFreqZ`
* `fBodyAccMagMean`
* `fBodyAccMagStd`
* `fBodyAccMagMeanFreq`
* `fBodyBodyAccJerkMagMean`
* `fBodyBodyAccJerkMagStd`
* `fBodyBodyAccJerkMagMeanFreq`
* `fBodyBodyGyroMagMean`
* `fBodyBodyGyroMagStd`
* `fBodyBodyGyroMagMeanFreq`
* `fBodyBodyGyroJerkMagMean`
* `fBodyBodyGyroJerkMagStd`
* `fBodyBodyGyroJerkMagMeanFreq`
