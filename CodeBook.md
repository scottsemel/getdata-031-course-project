

##Summarized Statistics from the Human Activity Recognition Using Smartphones Dataset##


The goal of the original study was to make a training set of 70% of the data and a test set of 30% of the data to see if you could build a classifier to predict if a subject was standing, sitting, laying down, etc. based on accelerometer readings from their wearable technology. The goal for us was to summarize the measurements by subject, activity, and just the variables that included means and standard deviations for the whole data set. 

First the vector with 561 features was cleaned so it could be processed in R. I removed the commas, parentheses, and dashes and replaced them with underscores. It was easiest to do this in wordpad with **Replace All**. Also some of the means had capitalized M and that was replaced with lowercase. The following steps were performed:

**1.** Grepped the text "std" and "mean" from the features to determine which variables were of interest in this study. There were 86 out of 561 variables to keep.

**2.** Load the trainset of 7352 rows , added columns for activity and subject number. Use just the 86 columns of interest

**3.** Loaded test set and added columns for activity and subject number. Use just the 86 columns of interest.

**4.** Append the test set to the training set.

**5.** Use the stringr library to replace the 6 numbers of the activities with text labels: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING.

**6.** Use the dplyr library to make a data table from the data frame.

**7.** Use the tidyr libary to gather the statistics into long form tidy data.

**8.** Use the dplyr library to group_by and summarize the statistics by subject. 

**9.** Create tidy summary of average of each variable for each activity for each subject. 86 variables times 30 subjects times 6 activities = 15480 rows with 4 columns. The columns are SubjectNumber, Activity, measure_stat, mean(reading)


###Notes:

Refer to Hadley Wickam's 
[Tidy Data paper and slides](https://github.com/justmarkham/tidy-data)

A full description is available at the [site](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones) where the data was obtained:

Here is the [data](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) for the project.

For more information about this dataset contact: activityrecognition@smartlab.ws

###License:

Use of this dataset in publications must be acknowledged by referencing the following publication [1] 

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.

Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012.

###Variables:

**features** - the 561 features from the features.txt

**activities** - the 6 activities the subjects were doing

**index1** - the index of the features that calculated a mean

**index2** - the index of the features that calculated a stdev

**Xtrain** - the table from Xtrain.txt

**ytrain** - the activities corresponding with each row in Xtrain

**subjecttrain** - the subject number in each row of Xtrain

**Xtest** - the table from Xtest.txt

**ytest** - the activities corresponding with each row in Xtest

**subjecttest** - the subject number in each row of Xtest

**trainset** - the 86 columns of variables of interest from 
Xtrain and columns for subject number and activity number

**testset** - the 86 columns of variables of interest from Xtest and columns for subject number and activity number

**fullset** - the testset appended to the training set

**tidyset** - a set with the 86 variables moved from columns to rows

**by_subject** - a set grouped by subject, activity, and variabl
e
**means** - a set with tidy summary of average of each variable for each activity for each subject.















