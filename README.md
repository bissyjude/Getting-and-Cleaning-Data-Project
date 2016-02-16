#Getting and Cleaning Data Project

run_Analysis.R
The cleanup script (run_Analysis.R) does the following:
Merges the training and the test sets to create one data set.
Extracts only the measurements on the mean and standard deviation for each measurement.
Uses descriptive activity names to name the activities in the data set
Appropriately labels the data set with descriptive activity names.
Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

What happens when you source run_Analysis.r
If you don't have either dplyr,  tidyr and dat.table  installed,Install those library.
It will load those libraries
Set the working directory for the script to the working directory of the run_Analysis.R file.
download the zip file and unzip it to that directory.
A file is generated to complete Objective 5 that conforms to the tidy data principles.

##R Files
run_Analysis.R
This is the main file and the one you need to run. Once you source it, it will execute.
Data Files
Tidy.txt
Tidy data that can be read into R with read.table("Tidy.txt", header=TRUE)
The rest of the data files (the actual dataset) is downloaded when you run run_Analysis.R

###Datset Txt files
- 'features_info.txt': Shows information about the variables used on the feature vector.
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'train/y_train.txt': Training labels.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.
The following files are available for the train and test data. Their descriptions are equivalent. 
- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 
- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 
- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

####Documentation Files
README.md
This file. Explains the overall information about the repository, how to run and what the files are.

CODEBOOK.md
The codebook which defines the column names, methodology and how the data was processed.
Package Dependencies
The packages should get auto-installed if you don't have them.
dplyr
tidyr
install.packages(c("dplyr", "tidyr"))
Cleaned Data
The resulting clean dataset is in this repository at: Tidy.txt. It contains one row for each subject/activity pair and columns for subject, activity, and each feature that was a mean or standard deviation from the original dataset.

#####How to read the generated tidy data file for evaluation.
RStudio:
View(read.table("grouped_means.txt", header=TRUE))

