# Getting and Cleaning Data Course Project CodeBook
## This code book describes the work performed to clean up the data and additional columns in two files created by the script. 
The whole transformations/work is done by “run_analysis.R” script so no manual steps are necessary (it takes around 1 minute to run together with downloading the file). 
Please read README.md to see the script and its detailed description.

** Please note English is not my first language, so I do apologise for any spelling/grammar mistakes and hope all reads clear and is understandable.
## Source Files 
The data was obtained from: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
The description of the original data can be found at: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
The files in zip archive used for this analysis, as per each directory:
* features_info.txt: Shows information about the variables used on the feature vector.
* features.txt - List of all features.
* activity_labels.txt - Links the class labels with their activity name.
* train/X_train.txt - Training set.
* train/y_train.txt - Training labels.
* test/X_test.txt - Test set.
* test/y_test.txt - Test labels.
* train/subject_train.txt - Each row identifies the subject who performed the activity for each window sample.
* test/subject_train.txt - Each row identifies the subject who performed the activity for each window sample.
## Transformations
The whole transformations/work is done by “run_analysis.R” script so no manual steps are necessary (it takes around 1 minute to run together with downloading the file). 
Here I have described simply how “run_analysis.R” script works (details in README.md):
1) The scripts first checks if our working directory has a folder named “data” and if not, it creates a folder named “data”. 
2) Then sets URL target as variable "fileUrl" for the site with the data for the project.
3) Then downloads the file to "data" directory as "download.zip" and records date and time of the download as variable “dateDownloaded” and prints date and time of the download to console.
4) Then it unzips the file into “data” directory into directory “UCI HAR Dataset” (as per zip file content).
5) Then the script follows the 5 steps of the assignment:
* Merges the training and the test data sets to create one data set.
* Extracts only the measurements on the mean and standard deviation for each measurement.
* Renames activity names to name the activities in the data set to be descriptive.
* Labels appropriately the data set with descriptive variable names. 
* From the last data set (in step 4), it creates a second, independent tidy data set with the average of each variable for each activity and each subject.
6) Both files (“measurements_mean_std.txt” and “activity_subject_means.txt”) are saved by write.table() in “data” folder. They should be read by using read.table()

# The script’s output data
## The columns in “measurements_mean_std.txt” is as follows:
* Activity - descriptive label of activity
all the other columns list the measurements as in the “features_info.txt” file in the original raw data set. 

## The columns in “activity_subject_means.txt” is as follows:
* activity - descriptive label of activity
* subject - test subject id
all the other columns list the measurements as in the “features_info.txt” file in the original raw data set. 