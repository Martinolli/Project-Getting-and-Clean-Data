---
output:
  html_document: default
  pdf_document: 
    keep_tex: yes
    number_sections: yes
    toc: yes
---
# Getting and Cleaning Data

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.

## The tasks are:

- Merges the training and the test sets to create one data set.
- Extracts only the measurements on the mean and standard deviation for each measurement.
- Uses descriptive activity names to name the activities in the data set
- Appropriately labels the data set with descriptive variable names.
- From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## The following files were used to perform the tasks:

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

## The following steps were done using the script describes in "READ.md" file at repo.

- 1 Download file "getdata_dataset.zip"
  - Create a diretory named "UCI HAR Dataset"
  - Unzip the file.
  
- 2 Reading data to create a table with X_train and X_Test
  - Create a data set with X_train and X_test named "Data"
  
- 3 Reading "features.txt" to name rows of Data
  - Named columms of Data with Features
  
- 4 Extracts only the measurements on the mean and standard deviation for each measurement
  - In this point some variables weren't used, like one which has "Mean" and "meanfreq"
  - Extracting variables names wich contain "mean" and "std", were ignored variable names with "Mean", like "AccJerkMag-meanFreq, gravityMean, etc
  - The new file "Data" contain just columns with mean() and std() like variables
  
- 5 Reading and assign names to Activities data
  - Read the acitivity lables "txt" file
  - Reading the y_train "txt" file
  - Transform the variables in "Acitivity_y_train" in characters
  - For all variables in Activity_y_train are assigned the names from "Activity" file
  - The same routine for file "Activity_y_test
  - Merging two datas in just one called Activities
  
- 6:
  - Named column from Activities with Activity_class
  - 

