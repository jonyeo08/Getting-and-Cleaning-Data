# Getting-and-Cleaning-Data
Please see comments next to the # for details on how the code works. 


This projects involves five goals:
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard
deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. From the data set in step 4, creates a second, independent 
tidy data set with the average of each variable for each activity
and each subject.

#set working directory
setwd("/Users/jyeo/datasciencecoursera/Getting and Cleaning Data/UCI HAR Dataset")

#Merge the training and testing sets
y_train<-read.table("train/y_train.txt")
y_test<-read.table("test/y_test.txt")

x_train<-read.table("train/x_train.txt")
x_test<-read.table("test/x_test.txt")

subject_train<-read.table("train/subject_train.txt")
subject_test<-read.table("test/subject_test.txt")

# combine test dataset with test dataset and train dataset with train dataset
x_data <- rbind(x_train, x_test)
y_data <- rbind(y_train, y_test)

#label subject variable
subject_data<-rbind(subject_train,subject_test)
names(subject_data) = "subject"


# Extract only the measurements on the
#mean and standard deviation for each measurement

features <- read.table("features.txt")[,2]
mean_and_sd_features <- grepl("mean|std", features)
x_data <- x_data[,mean_and_sd_features]
length(x_data)


#Uses descriptive activity names to name the activities in the data set
activity <- read.table("activity_labels.txt")[,2]
y_data[,2] = activity[y_data[,1]]
names(y_data) = c("activity_ID", "activity")

features_label<-read.table("features.txt")
names(x_data)<-features_label[,2]
  
#Combine all data
data <- cbind(x_data, y_data, subject_data)

#From the data set in step 4, creates a second, independent 
#tidy data set with the average of each variable for each activity
#and each subject.
library(plyr)
mean_data <- ddply( data, .(activity, subject),  function(x) colMeans(x[, 1:79]) ) 

#write tidy dataset file. 
write.table(mean_data, file = "tidy_dataset.txt")












