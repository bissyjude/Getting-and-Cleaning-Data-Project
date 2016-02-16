# Getting and Cleaning Data Course Assignment
Goal
Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone.

A full description is available at the site where the data was obtained:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
The data is available at:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
The aim of the project is to clean and extract usable data from the above zip file. R script called run_Analysis.R that does the following: 

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

In this project, you find:
run_Analysis.R : the R-code run on the data set
Tidy.txt : the clean data extracted from the original data using run_analysis.R
CodeBook.md : the CodeBook reference to the variables in Tidy.txt
README.md : the analysis of the code in run_Analysis.R

## Getting Started
Basic Assumption
The R code in run_Analysis.R proceeds under the assumption that the zip file available athttps://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip is downloaded and extracted in the R Home Directory.

Libraries Used
The libraries used in this operation are data.table , dplyr and  tidyr
dplyr is used to aggregate variables to create the tidy data.
library(data.table)
library(dplyr)

### Read Supporting Metadata
The supporting metadata in this data are the name of the features and the name of the activities. They are loaded into variables featureNames and activityLabels.

####1.Merges the training and the test sets to create one data set.
#Reading Data
featureNames <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /features.txt")
activityLabels <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /activity_labels.txt", header = FALSE)

Format training and test data sets
Both training and test data sets are split up into subject, activity and features. They are present in three different files.
Read training data
subjectTrain <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /train/subject_train.txt", header = FALSE)
activityTrain <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /train/y_train.txt", header = FALSE)
featuresTrain <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /train/X_train.txt", header = FALSE)

Read test data
subjectTest <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /test/subject_test.txt", header = FALSE)
activityTest <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /test/y_test.txt", header = FALSE)
featuresTest <- read.table("/Users/Bissy/Desktop/UCI\ HAR\ Dataset\ /test/X_test.txt", header = FALSE)

#####Part 1 - Merge the training and the test sets to create one data set
We can use combine the respective data in training and test data sets corresponding to subject, activity and features. The results are stored in subject, activity and features.
subject <- rbind(subjectTrain, subjectTest)
activity <- rbind(activityTrain, activityTest)
features <- rbind(featuresTrain, featuresTest)
Naming the columns
The columns in the features data set can be named from the metadata in featureNames
colnames(features) <- t(featureNames[2])
Merge the data
The data in features,activity and subject are merged and the complete data is now stored in FinalMergeData.
colnames(activity) <- "Activity"
colnames(subject) <- "Subject"
FinalMergeData <- cbind(features,activity,subject)
######Part 2 - Extracts only the measurements on the mean and standard deviation for each measurement
Extract the column indices that have either mean or std in them.
columnsWithMeanSTD <- grep(".*Mean.*|.*Std.*", names(FinalMergeData), ignore.case=TRUE)
Add activity and subject columns to the list and look at the dimension of FinalMergeData
requiredColumns <- c(columnsWithMeanSTD, 562, 563)
dim(FinalMergeData)
## [1] 10299   563
We create extractedData with the selected columns in requiredColumns. And again, we look at the dimension of requiredColumns.
extractedData <- FinalMergeData[,requiredColumns]
dim(extractedData)
## [1] 10299    88
#######Part 3 - Uses descriptive activity names to name the activities in the data set
The activity field in extractedData is originally of numeric type. We need to change its type to character so that it can accept activity names. The activity names are taken from metadata activityLabels.
extractedData$Activity <- as.character(extractedData$Activity)
for (i in 1:6){
extractedData$Activity[extractedData$Activity == i] <- as.character(activityLabels[i,2])
}
We need to factor the activity variable, once the activity names are updated.
extractedData$Activity <- as.factor(extractedData$Activity)
########Part 4 - Appropriately labels the data set with descriptive variable names
Here are the names of the variables in extractedData
names(extractedData)
[1] "tBodyAcc-mean()-X"                    "tBodyAcc-mean()-Y"                   
 [3] "tBodyAcc-mean()-Z"                    "tBodyAcc-std()-X"                    
 [5] "tBodyAcc-std()-Y"                     "tBodyAcc-std()-Z"                    
 [7] "tGravityAcc-mean()-X"                 "tGravityAcc-mean()-Y"                
 [9] "tGravityAcc-mean()-Z"                 "tGravityAcc-std()-X"                 
[11] "tGravityAcc-std()-Y"                  "tGravityAcc-std()-Z"                 
[13] "tBodyAccJerk-mean()-X"                "tBodyAccJerk-mean()-Y"               
[15] "tBodyAccJerk-mean()-Z"                "tBodyAccJerk-std()-X"                
[17] "tBodyAccJerk-std()-Y"                 "tBodyAccJerk-std()-Z"                
[19] "tBodyGyro-mean()-X"                   "tBodyGyro-mean()-Y"                  
[21] "tBodyGyro-mean()-Z"                   "tBodyGyro-std()-X"                   
[23] "tBodyGyro-std()-Y"                    "tBodyGyro-std()-Z"                   
[25] "tBodyGyroJerk-mean()-X"               "tBodyGyroJerk-mean()-Y"              
[27] "tBodyGyroJerk-mean()-Z"               "tBodyGyroJerk-std()-X"               
[29] "tBodyGyroJerk-std()-Y"                "tBodyGyroJerk-std()-Z"               
[31] "tBodyAccMag-mean()"                   "tBodyAccMag-std()"                   
[33] "tGravityAccMag-mean()"                "tGravityAccMag-std()"                
[35] "tBodyAccJerkMag-mean()"               "tBodyAccJerkMag-std()"               
[37] "tBodyGyroMag-mean()"                  "tBodyGyroMag-std()"                  
[39] "tBodyGyroJerkMag-mean()"              "tBodyGyroJerkMag-std()"              
[41] "fBodyAcc-mean()-X"                    "fBodyAcc-mean()-Y"                   
[43] "fBodyAcc-mean()-Z"                    "fBodyAcc-std()-X"                    
[45] "fBodyAcc-std()-Y"                     "fBodyAcc-std()-Z"                    
[47] "fBodyAcc-meanFreq()-X"                "fBodyAcc-meanFreq()-Y"               
[49] "fBodyAcc-meanFreq()-Z"                "fBodyAccJerk-mean()-X"               
[51] "fBodyAccJerk-mean()-Y"                "fBodyAccJerk-mean()-Z"               
[53] "fBodyAccJerk-std()-X"                 "fBodyAccJerk-std()-Y"                
[55] "fBodyAccJerk-std()-Z"                 "fBodyAccJerk-meanFreq()-X"           
[57] "fBodyAccJerk-meanFreq()-Y"            "fBodyAccJerk-meanFreq()-Z"           
[59] "fBodyGyro-mean()-X"                   "fBodyGyro-mean()-Y"                  
[61] "fBodyGyro-mean()-Z"                   "fBodyGyro-std()-X"                   
[63] "fBodyGyro-std()-Y"                    "fBodyGyro-std()-Z"                   
[65] "fBodyGyro-meanFreq()-X"               "fBodyGyro-meanFreq()-Y"              
[67] "fBodyGyro-meanFreq()-Z"               "fBodyAccMag-mean()"                  
[69] "fBodyAccMag-std()"                    "fBodyAccMag-meanFreq()"              
[71] "fBodyBodyAccJerkMag-mean()"           "fBodyBodyAccJerkMag-std()"           
[73] "fBodyBodyAccJerkMag-meanFreq()"       "fBodyBodyGyroMag-mean()"             
[75] "fBodyBodyGyroMag-std()"               "fBodyBodyGyroMag-meanFreq()"         
[77] "fBodyBodyGyroJerkMag-mean()"          "fBodyBodyGyroJerkMag-std()"          
[79] "fBodyBodyGyroJerkMag-meanFreq()"      "angle(tBodyAccMean,gravity)"         
[81] "angle(tBodyAccJerkMean),gravityMean)" "angle(tBodyGyroMean,gravityMean)"    
[83] "angle(tBodyGyroJerkMean,gravityMean)" "angle(X,gravityMean)"                
[85] "angle(Y,gravityMean)"                 "angle(Z,gravityMean)"                
[87] "Activity"                             "Subject"

By examining extractedData, we can say that the following acronyms can be replaced:
Acc can be replaced with Accelerometer
Gyro can be replaced with Gyroscope
BodyBody can be replaced with Body
Mag can be replaced with Magnitude
Character f can be replaced with Frequency
Character t can be replaced with Time
names(extractedData)<-gsub("Acc", "Accelerometer", names(extractedData))
names(extractedData)<-gsub("Gyro", "Gyroscope", names(extractedData))
names(extractedData)<-gsub("BodyBody", "Body", names(extractedData))
names(extractedData)<-gsub("Mag", "Magnitude", names(extractedData))
names(extractedData)<-gsub("^t", "Time", names(extractedData))
names(extractedData)<-gsub("^f", "Frequency", names(extractedData))
names(extractedData)<-gsub("tBody", "TimeBody", names(extractedData))
names(extractedData)<-gsub("-mean()", "Mean", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("-std()", "STD", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("-freq()", "Frequency", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("angle", "Angle", names(extractedData))
names(extractedData)<-gsub("gravity", "Gravity", names(extractedData))
Here are the names of the variables in extractedData after they are edited
names(extractedData)
##[1] "TimeBodyAccelerometerMean()-X"                     "TimeBodyAccelerometerMean()-Y"                    
##[3] "TimeBodyAccelerometerMean()-Z"                     "TimeBodyAccelerometerSTD()-X"                     
##[5] "TimeBodyAccelerometerSTD()-Y"                      "TimeBodyAccelerometerSTD()-Z"                     
##[7] "TimeGravityAccelerometerMean()-X"                  "TimeGravityAccelerometerMean()-Y"                 
##[9] "TimeGravityAccelerometerMean()-Z"                  "TimeGravityAccelerometerSTD()-X"                  
##[11] "TimeGravityAccelerometerSTD()-Y"                   "TimeGravityAccelerometerSTD()-Z"                  
# [13] "TimeBodyAccelerometerJerkMean()-X"                 "TimeBodyAccelerometerJerkMean()-Y"                
# [15] "TimeBodyAccelerometerJerkMean()-Z"                 "TimeBodyAccelerometerJerkSTD()-X"                 
# [17] "TimeBodyAccelerometerJerkSTD()-Y"                  "TimeBodyAccelerometerJerkSTD()-Z"                 
# [19] "TimeBodyGyroscopeMean()-X"                         "TimeBodyGyroscopeMean()-Y"                        
# [21] "TimeBodyGyroscopeMean()-Z"                         "TimeBodyGyroscopeSTD()-X"                         
# [23] "TimeBodyGyroscopeSTD()-Y"                          "TimeBodyGyroscopeSTD()-Z"                         
# [25] "TimeBodyGyroscopeJerkMean()-X"                     "TimeBodyGyroscopeJerkMean()-Y"                    
# [27] "TimeBodyGyroscopeJerkMean()-Z"                     "TimeBodyGyroscopeJerkSTD()-X"                     
# [29] "TimeBodyGyroscopeJerkSTD()-Y"                      "TimeBodyGyroscopeJerkSTD()-Z"                     
# [31] "TimeBodyAccelerometerMagnitudeMean()"              "TimeBodyAccelerometerMagnitudeSTD()"              
# [33] "TimeGravityAccelerometerMagnitudeMean()"           "TimeGravityAccelerometerMagnitudeSTD()"           
# [35] "TimeBodyAccelerometerJerkMagnitudeMean()"          "TimeBodyAccelerometerJerkMagnitudeSTD()"          
# [37] "TimeBodyGyroscopeMagnitudeMean()"                  "TimeBodyGyroscopeMagnitudeSTD()"                  
# [39] "TimeBodyGyroscopeJerkMagnitudeMean()"              "TimeBodyGyroscopeJerkMagnitudeSTD()"              
# [41] "FrequencyBodyAccelerometerMean()-X"                "FrequencyBodyAccelerometerMean()-Y"               
# [43] "FrequencyBodyAccelerometerMean()-Z"                "FrequencyBodyAccelerometerSTD()-X"                
# [45] "FrequencyBodyAccelerometerSTD()-Y"                 "FrequencyBodyAccelerometerSTD()-Z"                
# [47] "FrequencyBodyAccelerometerMeanFreq()-X"            "FrequencyBodyAccelerometerMeanFreq()-Y"           
# [49] "FrequencyBodyAccelerometerMeanFreq()-Z"            "FrequencyBodyAccelerometerJerkMean()-X"           
# [51] "FrequencyBodyAccelerometerJerkMean()-Y"            "FrequencyBodyAccelerometerJerkMean()-Z"           
# [53] "FrequencyBodyAccelerometerJerkSTD()-X"             "FrequencyBodyAccelerometerJerkSTD()-Y"            
# [55] "FrequencyBodyAccelerometerJerkSTD()-Z"             "FrequencyBodyAccelerometerJerkMeanFreq()-X"       
# [57] "FrequencyBodyAccelerometerJerkMeanFreq()-Y"        "FrequencyBodyAccelerometerJerkMeanFreq()-Z"       
# [59] "FrequencyBodyGyroscopeMean()-X"                    "FrequencyBodyGyroscopeMean()-Y"                   
# [61] "FrequencyBodyGyroscopeMean()-Z"                    "FrequencyBodyGyroscopeSTD()-X"                    
# [63] "FrequencyBodyGyroscopeSTD()-Y"                     "FrequencyBodyGyroscopeSTD()-Z"                    
# [65] "FrequencyBodyGyroscopeMeanFreq()-X"                "FrequencyBodyGyroscopeMeanFreq()-Y"               
# [67] "FrequencyBodyGyroscopeMeanFreq()-Z"                "FrequencyBodyAccelerometerMagnitudeMean()"        
# [69] "FrequencyBodyAccelerometerMagnitudeSTD()"          "FrequencyBodyAccelerometerMagnitudeMeanFreq()"    
# [71] "FrequencyBodyAccelerometerJerkMagnitudeMean()"     "FrequencyBodyAccelerometerJerkMagnitudeSTD()"     
# [73] "FrequencyBodyAccelerometerJerkMagnitudeMeanFreq()" "FrequencyBodyGyroscopeMagnitudeMean()"            
# [75] "FrequencyBodyGyroscopeMagnitudeSTD()"              "FrequencyBodyGyroscopeMagnitudeMeanFreq()"        
# [77] "FrequencyBodyGyroscopeJerkMagnitudeMean()"         "FrequencyBodyGyroscopeJerkMagnitudeSTD()"         
# [79] "FrequencyBodyGyroscopeJerkMagnitudeMeanFreq()"     "Angle(TimeBodyAccelerometerMean,Gravity)"         
# [81] "Angle(TimeBodyAccelerometerJerkMean),GravityMean)" "Angle(TimeBodyGyroscopeMean,GravityMean)"         
# [83] "Angle(TimeBodyGyroscopeJerkMean,GravityMean)"      "Angle(X,GravityMean)"                             
# [85] "Angle(Y,GravityMean)"                              "Angle(Z,GravityMean)"                             
# [87] "Activity"                                          "Subject"


##########Part 5 - From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
Firstly, let us set Subject as a factor variable.
extractedData$Subject <- as.factor(extractedData$Subject)
extractedData <- data.table(extractedData)
We create tidyData as a data set with average for each activity and subject. Then, we order the enties in tidyData and write it into data fileTidy.txt that contains the processed data.
tidyData <- aggregate(. ~Subject + Activity, extractedData, mean)
tidyData <- tidyData[order(tidyData$Subject,tidyData$Activity),]
write.table(tidyData, file = "Tidy.txt", row.names = FALSE)

