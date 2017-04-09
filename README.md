### To create one R script called run_analysis.R that does the following.
Merges the training and the test sets to create one data set.
Extracts only the measurements on the mean and standard deviation for each measurement.
Uses descriptive activity names to name the activities in the data set
Appropriately labels the data set with descriptive variable names.
From the data set in step 4, creates a second, independent tidy data set, averages.txt, with the average of each variable for each activity and each subject.

Tidy Mean and Standard Deviation Data from the UCI HAR Data Set
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

#Load required packages: dplyr and reshape2
library(plyr)
library(reshape2)
### Read files
Please make sure tgat you have data in working directory.
#setwd("/Coursera/Getting and Cleanning data week 4/Assignment/UCI HAR Dataset")
getwd()
xTrain <- read.table("train/X_train.txt")
yTrain <- read.table("train/y_train.txt")
subjectTrain <- read.table("train/subject_train.txt")
xTest <- read.table("test/X_test.txt")
yTest <- read.table("test/y_test.txt")
subjectTest <- read.table("test/subject_test.txt")

### Merges the training and the test sets to create one data set.
##### create 'x' data set
xData <- rbind(xTrain, xTest)
##### create 'y' data set
yData <- rbind(yTrain, yTest)
##### create 'subject' data set
subjectData <- rbind(subjectTrain, subjectTest)
### Extract only the measurements on the mean and standard deviation for each measurement
##### get columns with names containing "mean()" or "std()"
features <- read.table("features.txt")
head(features, 10)
##### get only columns with mean() or std() in their names
meanStdFeatures <- grep("(mean|std)\\(\\)", features[, 2])

xData <- xData[, meanStdFeatures]
names(xData) <- features[meanStdFeatures, 2]

#### Uses descriptive activity names to name the activities in the data set
activities <- read.table("activity_labels.txt")
#### update values with correct activity names
yData[, 1] <- activities[yData[, 1], 2]

### Appropriately labels the data set with descriptive variable names.
#### correct column name
names(yData) <- "activity"
names(subjectData) <- "subject"

#### merge a single data set

tidyData <- cbind(xData, yData, subjectData)

####  create a second, independent tidy data set with the average of each variable for each activity and each subject.
meltData <- melt(tidyData,c("subject","activity"))
avgData <- dcast(meltData, subject + activity ~ variable, mean)
write.table(avgData, file="averages.txt", row.name=FALSE) 
write.csv(avgData, "Dataset.csv")

####  create a codeBook.csv
codeBook<-as.data.frame(names(avgData))
write.csv(codeBook, "codeBook.csv")
