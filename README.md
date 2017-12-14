# Getting and Cleaning Data Project

Getting and Cleaning Data Project for Coursera
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.

The dataset includes the following files:

- 'README.txt'

- 'features_info.txt': Shows information about the variables used on the feature vector.

- 'features.txt': List of all features.

- 'activity_labels.txt': Links the class labels with their activity name.

- 'train/X_train.txt': Training set.

- 'train/y_train.txt': Training labels.

- 'test/X_test.txt': Test set.

- 'test/y_test.txt': Test labels.

- 'subject_train.txt': Subjects performed train activities

- 'subject_test.txt': Subjects performed tests activities

## The tasks requested were:

- You should create one R script called run_analysis.R that does the following.

- Merges the training and the test sets to create one data set.
- Extracts only the measurements on the mean and standard deviation for each measurement.
- Uses descriptive activity names to name the activities in the data set
- Appropriately labels the data set with descriptive variable names.
- From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## The scrip used to perform the task is:
- title: "Project Getting and Cleanning Data"
- author: "Martinolli"
- date: "December 9, 2017"
- output: html_document
## Peer-graded Assignment: Getting and Cleaning Data Course Project

## Getting and Cleaning Data Course Project

```{r warning=FALSE}
library(stringr)
library(data.table)
library(dplyr)
```


## Download files to work

```{r}
Data <- "getdata_dataset.zip"

if (!file.exists(Data)){
        fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
        download.file(fileUrl,Data)
}
if (!file.exists("UCI HAR Dataset"))
{unzip(Data)
}

```
## 1 - Merges the training and the test sets to create one data set.

## Reading Data to create a table with X_train and X_test and assign names from Features to columms

```{r warning=FALSE}

## Reading data to create a table with X_train and X_Test

X_train <- read.table("./UCI HAR Dataset/train/X_train.txt",header = F,stringsAsFactors = F)

X_test <- read.table("./UCI HAR Dataset/test/X_test.txt",header = F,stringsAsFactors = F)

## Create a table with X_train and X_test

Data <- rbind(X_train,X_test)

## Reading Features to name rows of Data

Features <- read.table("./UCI HAR Dataset/features.txt",header = F,stringsAsFactors = F)

Features <- select(Features,c(2))

colnames(Features) <- "Feature"

## create a vector with Features to name Data

Names <- Features$Feature

## Named columms of Data with Features

colnames(Data) <- Names

head(Data,4)[1:5]

dim(Data)

```


## 2 - Extracts only the measurements on the mean and standard deviation for each measurement.

```{r warning=FALSE}

## Extracting Names from columns in Data set

Names <- colnames(Data)

## Extracting variables names wich contain "mean" and "std", were ignored        variable names with "Mean",
## like "AccJerkMag-meanFreq, gravityMean, etc."


Data <- Data[ ,grep("(mean|std)\\(\\)",colnames(Data))]

## The new file Data contain, just columns with mean() and std() like variables.

head(Data,4)[1:3]

tail(Data,4)[1:3]

dim(Data)

```

## Reading and assign names to Activities data



```{r warning=FALSE}

## Reading the acitivity lables "txt" file.

Activity <- read.table("./UCI HAR Dataset/activity_labels.txt",header = F,stringsAsFactors = F)

## Reading the y_train "txt" file.

Activity_y_train <- read.table("~/UCI HAR Dataset/train/y_train.txt",header=F)

## Transform the variables in "Acitivity_y_train" in characters. 

Activity_y_train$V1 <- as.character(Activity_y_train$V1)

## For all variables in Activity_y_train are assigned the names from "Activity" file.

for(i in 1:length(Activity_y_train$V1)){Activity_y_train$V1[i] <- Activity$V2[as.numeric(Activity_y_train$V1[i])]}

## The same routine for file "Activity_y_test.txt"

Activity_y_test <- read.table("~/UCI HAR Dataset/test/y_test.txt",header=F)

Activity_y_test$V1 <- as.character(Activity_y_test$V1)

for(i in 1:length(Activity_y_test$V1)){Activity_y_test$V1[i] <- Activity$V2[as.numeric(Activity_y_test$V1[i])]}

## Merging two datas in just one called Activities

Activities <- rbind(Activity_y_train,Activity_y_test)

## Named column from Activities with Activity_class

names(Activities) <- "Activity_class"

## Printing file "Activities"

head(Activities)

tail(Activities)

## The Activities file

str(Activities)
```


## 3 - Uses descriptive activity names to name the activities in the data set

```{r warning=FALSE}

Subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt",header = F,stringsAsFactors = F)

Subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt",header = F,stringsAsFactors = F)

Subject <- rbind(Subject_train,Subject_test)

colnames(Subject) <- "Subject"

Data1 <- cbind(Activities,Subject)

head(Data1,4)

tail(Data1,4)

```

## Merge two Datas, Data set and Data Subjects and Activities

```{r warning=FALSE}

Data <- cbind(Data,Data1)

head(Data,3)[65:68]

tail(Data,3)[65:68]

dim(Data)

```
## 4 - Appropriately labels the data set with descriptive variable names.

```{r warning=FALSE}

Names <- colnames(Data)

Names

## 1

Names <- gsub("-","_",Names)
Names <- gsub("[()]","",Names)
Names <- gsub("mean","Mean",Names)
Names <- gsub("std","Std",Names)
Names <- gsub("Mag","_Magnitude",Names)
Names <- gsub("Jerk","_Jerk",Names)
Names <- gsub("tBodyAcc","Time_Domain_Linear_Acceleration",Names)
Names <- gsub("tBodyGyro","Time_Domain_Angular_Velocity",Names)
Names <- gsub("tGravityAcc","Time_Domain_Gravity_Acceleration",Names)
Names <- gsub("fBodyAcc","Frequency_Domain_Linear_Acceleration",Names)
Names <- gsub("fBodyGyro","Frequency_Domain_Angular_Velocity",Names)
Names <- gsub("fBodyAcc","Frequency_Domain_Acceleration",Names)
Names <- gsub("fGravityAcc","Frequency_Domain_Gravity_Acceleration",Names)
Names <- gsub("fBodyGyro","Frequency_Domain_Angular_Velocity",Names)
Names <- gsub("fBodyBodyAcc","Frequency_Domain_Linear_Accleration",Names)
Names <- gsub("fBodyBodyGyro","Frequency_Domain_Angular_Velocity",Names)

Names


## Put the Names in the Data's variables


colnames(Data) <- Names

head(Data,3)[1:4]
tail(Data,3)[1:4]

```
## 5 - From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

```{r}
DataN <- aggregate(. ~ Subject + Activity_class,Data,mean)
head(DataN,2)[1:4]
dim(DataN)
write.table(DataN, file="TidyData.txt",row.names = FALSE)
```
