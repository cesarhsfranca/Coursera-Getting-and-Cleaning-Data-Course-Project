The Original data used for this project:

  The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy     data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You   will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing     the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean     up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the     scripts work and how they are connected.

  One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like   Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the       course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available   at the site where the data was obtained:

  http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

  Here are the data for the project:

  https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip


  0. Download and unzip the dataset;


 Project: Create one R script called run_analysis.R that does the following:

 
  1. Merges the training and the test sets to create one data set;
  2. Extracts only the measurements on the mean and standard deviation for each measurement;
  3. Uses descriptive activity names to name the activities in the data set;
  4. Appropriately labels the data set with descriptive variable names;
  5. From the data set in step 4, creates a second, independent tidy data set with the average
     of each variable for each activity and each subject.
 
 
  0. Downloading and unzipping dataset

    0.1. Set the working directory.
 
         setwd("/Users/Cesar/Desktop/IoT/COURSERA/R PACKAGES DOCUMENTS/R Files")
 
    0.2. Create the directory WearableComputing where the data will be downloaded under the setted working directory "R Files".
         The below command first checks if the directory exists to only then creates it.

         if(!file.exists("./WearableComputing")){dir.create("./WearableComputing")}
 
    0.3. Assign the WEB address where the data is to fileUrl
 
         fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

    0.4. Print in the console the content of "fileUrl" to confirm the address is correct assigned.This is not necessary but only used
         to double check.


         fileUrl
 
    0.5. Download the file "Dataset.zip" to the destination directory "WearableComputing".
         The below command line downloads the content assigned to the origin, i.e. the "fileUrl" to the destination directory created
         above in command line number 9.
 
 
         download.file(fileUrl,destfile="./WearableComputing/Dataset.zip")
 
    0.6. List the content of the directory WearableComputing.
 
         list.files("./WearableComputing") # Content of directory WearableComputing are: [1] "Dataset.zip"

    0.7. Unzip the dataset contained in the file "Dataset.zip" into the directory WearableComputing.

         unzip(zipfile="./WearableComputing/Dataset.zip",exdir="./WearableComputing")

    0.8. List once more the content of the directory WearableComputing in order to check that its content now also have the unzipped file.
 
         list.files("./WearableComputing") # Content of directory WearableComputing are: [1] "Dataset.zip"     "UCI HAR Dataset"
 
 
 
 
  1. Merges the training and the test sets to create one data set:
     Note: The read.table function reads a file(in the case of this project: x_train.txt, y_train.txt, subject_train.txt, x_test.txt,
           y_test.txt, subject_test.txt, features.txt and activity_labels.txt) in table format and creates a data frame from it, with cases
           corresponding to lines and variables to fields in the file.
 
           The content of the below files are assigned to its respective files into R:
 
              File                         R
           x_train.txt           ->     x_train
           y_train.txt           ->     y_train
           subject_train.txt     ->     subject_train
           x_test.txt            ->     x_test
           y_test.txt            ->     y_test
           subject_test.txt      ->     subject_test
           features.txt          ->     features
           activity_labels.txt   ->     activity_labels
      
        
    1.1. Assign the training data to its respective file to be later merged into one data set:
 
        1.1.0. Assign the training data set to the file "x_train";
               Note: It makes sense that the data set are aligned to the x axis.
 
               x_train <- read.table("./WearableComputing/UCI HAR Dataset/train/X_train.txt", header = FALSE) # The file "x_train.txt" contains
                                                                                                the trainning data set;
                                                                                                (7352 observations of 561 variables).
 
        1.1.1. Assign the training data labels to the file "y_train"; 
               Note: It makes sense that the labels are aligned to the y axis.

               y_train <- read.table("./WearableComputing/UCI HAR Dataset/train/y_train.txt", header = FALSE) # The file "y_train.txt" contains
                                                                                                the trainning data labels;
                                                                                                (7352 observations of 1 variable).

        1.1.2. Assign the subject data trainning file, with data of who performed the activity for each window sample.(70% of all subjects).
               Its range is from 1 to 30, to the file "subject_train"

               subject_train <- read.table("./WearableComputing/UCI HAR Dataset/train/subject_train.txt", header = FALSE) # (7352 observations of 1 variable).



              
    1.2. Read test tables:

        1.2.0. Assign the testing data set to the file "x_test";
               Note: It makes sense that the test set are aligned to the x axis.
 
                x_test <- read.table("./WearableComputing/UCI HAR Dataset/test/X_test.txt", header = FALSE) # (2947 observations of 561 variables).
 
 
        1.2.1. Assign the testing data labels to the file "y_test";
               Note: It makes sense that the test labels are aligned to the y axis.

                y_test <- read.table("./WearableComputing/UCI HAR Dataset/test/y_test.txt", header = FALSE) # (2947 observations of 1 variable).



        1.2.2. Assign the subject data test file, with data of who performed the activity for each window sample.(30% of all subjects).

                subject_test <- read.table("./WearableComputing/UCI HAR Dataset/test/subject_test.txt") # (2947 observations of 1 variable).





    1.3. Read the feature vector:


        features <- read.table('./WearableComputing/UCI HAR Dataset/features.txt', header = FALSE) # (561 observations of 2 variables).


    1.4. Reading activity labels:


        activityLabels = read.table('./WearableComputing/UCI HAR Dataset/activity_labels.txt', header = FALSE) # (6 observations of 2 variables).

    1.5. Print activity labels:

        activityLabels # The below observations are according to what is described in the file "README.txt".

    V1                 V2
  1  1            WALKING
  2  2   WALKING_UPSTAIRS
  3  3 WALKING_DOWNSTAIRS
  4  4            SITTING
  5  5           STANDING
  6  6             LAYING



    1.6. Assign columns names:

        
        colnames(x_train) <- features[,2] # Assign the names of the 561 observations of the vector features, i.e., names in its column 2, as names of the 561 variables of x_train respectively.
        colnames(y_train) <-"activityId"  # Assign the name activityID to the column of y_train.
        colnames(subject_train) <- "subjectId" # Assign the name subjectID to the column of subject_train.
        
        colnames(x_test) <- features[,2] # Assign the names of the 561 observations of the vector features, i.e., names in its column 2, as names of the 561 variables of x_test respectively.
        colnames(y_test) <- "activityId" # Assign the name activityID to the column of y_test.
        colnames(subject_test) <- "subjectId" # Assign the name subjectID to the column of subject_test.
        activityLabels # Printing activityLabels before the next command line to show the names of its columns V1 and V2 respectively.
        colnames(activityLabels) <- c('activityId','activityType') # Assign the names activityID and activityType to the two columns of activityLabels respectively.
        activityLabels # Printing activityLabels after the above command line to show that now the names of its two columns are now activityID and activityType respectively.


    1.7. Merge all the data in one data set:


    1.7.0. First confirming the classes of each file:


        class(y_train)

        [1] "data.frame"

        class(subject_train)

        [1] "data.frame"

        class(x_train)

        [1] "data.frame"

        class(y_test)

        [1] "data.frame"

        class(subject_test)

        [1] "data.frame"

        class(x_test)

        [1] "data.frame"

    All the R files are data frames.

 
    1.7.1. Second check the dimensions of each file:


        dim(y_train)

        [1] 7352    1

        dim(subject_train)

        [1] 7352    1

        dim(x_train)

        [1] 7352  561

        dim(y_test)

        [1] 2947    1

        dim(subject_test)

        [1] 2947    1

        dim(x_test)

        [1] 2947  561
 
    All the R files have the expected dimensions.

        
    1.7.2. Combine the columns of y_train, subject_train and x_train into the file merged_train.
       
        merged_train <- cbind(y_train, subject_train, x_train)

    1.7.2.0. Print the head of merged_train to check the result of the above command line.

        head(merged_train)
        dim(merged_train)
 
        [1] 7352  563

    1.7.3. Combine the columns of y_test, subject_test and x_test into the file merged_test.

        merged_test <- cbind(y_test, subject_test, x_test)

    1.7.3.0. Print the head of merged_test to check the result of the above command line.

        head(merged_test)
        dim(merged_test)

        [1] 2947  563

    1.7.4. Combine the rows of both files merged_train and merged_test into the file mergedAlldata.       


        mergedAlldata <- rbind(merged_train, merged_test)

    1.7.4.0. Print the head of mergedAlldata to check the result of the above command line.     

        head(mergedAlldata)
        dim(mergedAlldata)

        [1] 10299   563




    2. Extracts only the measurements on the mean and standard deviation for each measurement.

        2.1. Reading column names:

        colNames <- colnames(mergedAlldata)

        2.2. Create a vector for defining ID, mean and standard deviation:

        mean_and_std <- (grepl("activityId" , colNames) | 
                                 grepl("subjectId" , colNames) | 
                                 grepl("mean.." , colNames) |                                                                                                            grepl("std.." , colNames) 
        )
        mean_and_std


        2.3. Making nessesary subset from mergedAlldata:


        submergedAlldata <- mergedAlldata[ , mean_and_std == TRUE] # Assign a sub set of mergedAlldata for each element of mean_and_std equals TRUE.



    3. Uses descriptive activity names to name the activities in the data set:

        descActivityNames <- merge(submergedAlldata, activityLabels,
                                      by='activityId',
                                      all.x=TRUE)
        descActivityNames




    4. Appropriately labels the data set with descriptive variable names.


       
        prefix t is replaced by time
        Acc is replaced by Accelerometer
        Gyro is replaced by Gyroscope
        prefix f is replaced by frequency
        Mag is replaced by Magnitude
        BodyBody is replaced by Body


        names(descActivityNames) <- gsub("^t", "time", names(descActivityNames))
        names(descActivityNames) <- gsub("^f", "frequency", names(descActivityNames))
        names(descActivityNames) <- gsub("Acc", "Accelerometer", names(descActivityNames))
        names(descActivityNames) <- gsub("Gyro", "Gyroscope", names(descActivityNames))
        names(descActivityNames )<- gsub("Mag", "Magnitude", names(descActivityNames))
        names(descActivityNames) <- gsub("BodyBody", "Body", names(descActivityNames))


      
        names(descActivityNames)




    5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

        5.1. Create the second tidy data set:

        secTidyData <- aggregate(. ~subjectId + activityId, descActivityNames, mean)
        secTidyData <- secTidyData[order(secTidyData$subjectId, secTidyData$activityId),]
        secTidyData
        
        5.2. Write the second tidy data set into a TXT file:

        write.table(secTidyData, "secTidyData.txt", row.name=FALSE) # Writes the file secTidyData.txt into the working directory.
