##The whole transformations/work is done by “run_analysis.R” script 
No manual steps are necessary (it takes around 1 minute to run together with downloading the file). 

It will result in two csv files being written:

* measurements_mean_std.txt - containing only the mean and standard deviation for each measurement.
* activity_subject_means.txt - containing data set with the average of each variable for each activity and each subject.

### Please see CodeBook.md for shorter description of the extraction process and the columns in the two files.

#Descryption of "run_analysis.R" script:
## Check if "data" directory exists
if (!file.exists("data")) { 
## if directory does not exist create a directory "data" in the working directory
    dir.create("data") 
}


## Set URL target as variable "fileUrl"
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"


## Download to "data" directory as "download.zip" with no "curl" as I am running this script on Windows 7
download.file(fileUrl, destfile = "./data/download.zip") 


### Record date and time of the download as an object "dateDownloaded"
dateDownloaded <- date()
### Print date and time of the download
print(dateDownloaded) 


## Unzip files
unzip("./data/download.zip", exdir = "./data")


## load library for "Reshape Grouped Data"
library(reshape2)



## 1. Merge the training and the test sets to create one data set.

### Read each table into one variable from "train" and "test" folder by "x", "y" and "subject"
x_train_data <- read.table("./data/UCI HAR Dataset/train/X_train.txt")
x_test_data <- read.table("./data/UCI HAR Dataset/test/X_test.txt")
y_train_data <- read.table("./data/UCI HAR Dataset/train/y_train.txt")
y_test_data <- read.table("./data/UCI HAR Dataset/test/y_test.txt")
subject_train_data <- read.table("./data/UCI HAR Dataset/train/subject_train.txt")
subject_test_data <- read.table("./data/UCI HAR Dataset/test/subject_test.txt")

### Merge data sets to create one dataset for each "x", "y" and "subject".
joint_x <- rbind(x_train_data, x_test_data)
joint_y <- rbind(y_train_data, y_test_data)
joint_subject <- rbind(subject_train_data, subject_test_data)



## 2. Extract only the measurements on the mean and standard deviation for each measurement. 

### Read features into a table as variable "features"
features <- read.table("./data/UCI HAR Dataset/features.txt")[,"V2"]

### set descriptive names for all columns in merged "x", "y" and "subject"
colnames(joint_x) <- features
colnames(joint_y) <- "activity_ID"
colnames(joint_subject) <- "subject"

### select features containing measurements on the mean and standard deviation
means <- grep("-mean\\(\\)", features, value=TRUE)
std_dev <- grep("-std\\(\\)", features, value=TRUE)
measurements <- c(means, std_dev)
new_x <- joint_x[, measurements]



## 3. Use descriptive activity names to name the activities in the dataset

### Read activity descriptions into a table as variable "activities"
activities <- read.table("./data/UCI HAR Dataset/activity_labels.txt")

### Set the column names in "activities"
colnames(activities) <- c("activity_ID", "activity")

### join activity dataset with descriptive labels
new_y <- merge(joint_y, activities)



## 4. Appropriately labels the data set with descriptive activity names.

### Join measurements dataset with activity labels
data1 <- cbind(new_x, new_y["activity"])

### Write the resultant dataset to a table in "data" folder
write.table(data1, "./data/measurements_mean_std.txt")

### Join subject IDs
data2 <- cbind(data1, joint_subject)



## 5. From the data set in step 4 (data2), create a second, independent tidy data set 
## with the average of each variable for each activity and each subject.

### Summarise means per activity and subject
data2_melt <- melt(data2, id=c("subject", "activity"))

### create the average
data3 <- dcast(data2_melt, activity + subject ~ variable, mean)

### write second result to a table in "data" folder
write.table(data3, "./data/activity_subject_means.txt", row.name=FALSE)
