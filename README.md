


##Summarized Statistics from the Human Activity Recognition Using Smartphones Dataset

This is the final project for Coursera Getting and Cleaning Data class.

The goal of the original study was to make a train set of 70% of the data and a test set of 30% of the data to see if you could build a classifier to predict if a subject was standing, sitting, laying down based on accelerometer readings from their wearable technology. The goal for us was to summarize the measurements by subject, activity, and variable for the whole data set. 

Download the zip file from [here](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip).

Unzip the UCI HAR Dataset and put the run_analysis.R file in the UCI HAR Dataset directory and run the code.

###Output looks like this:


   SubjectNumber    Activity   measure_stat     mean(reading)

1              1   LAYING    tBodyAcc_mean_X    0.22159824

2              1   LAYING    tBodyAcc_mean_Y   -0.04051395

3              1   LAYING    tBodyAcc_mean_Z   -0.11320355

4              1   LAYING     tBodyAcc_std_X   -0.92805647

5              1   LAYING     tBodyAcc_std_Y   -0.83682741


..           ...      ...                ...           ...

###Code:

library(stringr) 
library(dplyr)
library(tidyr)

###Removed commas, parentheses, and dashes previously from features in Wordpad
features = read.table("newfeatures.txt")
activities = read.table("activity_labels.txt")

### Pull only readings with standard deviation and mean out of 561 columns
### Note a few had "Mean" with capital M which was changed to lowercase m
index1 = grep("std",features[,2])
index2 = grep("mean",features[,2])



### That's only 86 columns. Put them together in one variable
index = unique(sort(rbind(index1,index2)))

### Load the train set
Xtrain = read.table("./train/X_train.txt")
ytrain = read.table("./train/y_train.txt")
subjecttrain = read.table("./train/subject_train.txt")

### Make it one tidy table
Xtrain = Xtrain[,index]
colnames(ytrain) = c("Activity")
colnames(subjecttrain) = c("SubjectNumber")

### Use the names of features of interest as column headers
colnames(Xtrain) = features[index,2]
trainset = cbind(ytrain,subjecttrain,Xtrain)

### Load the test set 
Xtest = read.table("./test/X_test.txt")
ytest = read.table("./test/y_test.txt")
subjecttest = read.table("./test/subject_test.txt")

### Make it one tidy table
Xtest = Xtest[,index]
colnames(ytest) = c("Activity")
colnames(subjecttest) = c("SubjectNumber")

### Use the names of features of interest as column headers
colnames(Xtest) = features[index,2]
testset = cbind(ytest,subjecttest,Xtest)

### Append test set to the train set, 10299 rows altogether
fullset = rbind(testset,trainset)

### Rename the activities using text instead of numbers
fullset$Activity = as.character(fullset$Activity)
fullset$Activity = str_replace(fullset$Activity,"1", "WALKING")
fullset$Activity = str_replace(fullset$Activity,"2", "WALKING_UPSTAIRS")
fullset$Activity = str_replace(fullset$Activity,"3", "WALKING_DOWNSTAIRS")
fullset$Activity = str_replace(fullset$Activity,"4", "SITTING")
fullset$Activity = str_replace(fullset$Activity,"5", "STANDING")
fullset$Activity = str_replace(fullset$Activity,"6", "LAYING")



## Step 5  Create Tidy Summary 
###Create tidy summary of average of each variable for each activity for each subject.
### 86 variables times 30 subjects times 6 activities = 15480 rows with 4 columns


fullset = tbl_df(fullset)
tidyset = gather(fullset, measure_stat, reading, -Activity, -SubjectNumber)
by_subject = group_by(tidyset, SubjectNumber, Activity, measure_stat)
means = summarize(by_subject, mean(reading))

write.table(means, file = "tidyset.txt", row.names=FALSE)
