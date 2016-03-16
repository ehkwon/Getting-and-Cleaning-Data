## Load the required packages
packages <- c("data.table", "reshape2")
sapply(packages, require, character.only=TRUE, quietly=TRUE)

## Set the working directory
path<-getwd()

## Download the dataset
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
f<-"Dataset.zip"
if (!file.exists(path)) {dir.create(path)}
download.file(url, method="curl", file.path(path, f))

## Unzip the dataset and Create a variable for path
unzip(paste(paste0(path,"/",f)))
dtPath <-file.path(path, "UCI HAR Dataset")

## Read subject files
dtSubjectTrain <- fread(file.path(dtPath, "train", "subject_train.txt"))
dtSubjectTest  <- fread(file.path(dtPath, "test" , "subject_test.txt" ))

## Read activity files
dtActivityTrain <- fread(file.path(dtPath, "train", "y_train.txt"))
dtActivityTest  <- fread(file.path(dtPath, "test" , "y_test.txt" ))

#Read data files
fileToDataTable <- function (f) {
  df <- read.table(f)
  dt <- data.table(df)
}
dtTrain <- fileToDataTable(file.path(dtPath, "train", "X_train.txt"))
dtTest  <- fileToDataTable(file.path(dtPath, "test" , "X_test.txt" ))


### 1. Merges the training and the test sets to create one data set.

## Concatenate data tables
dtSubject <- rbind(dtSubjectTrain, dtSubjectTest)
setnames(dtSubject, "V1", "subject")
dtActivity <- rbind(dtActivityTrain, dtActivityTest)
setnames(dtActivity, "V1", "activityNum")
dt <- rbind(dtTrain, dtTest)

## Merge columns
dtSubject <- cbind(dtSubject, dtActivity)
dt <- cbind(dtSubject, dt)

##Set key
setkey(dt, subject, activityNum)


### 2. Extracts only the measurements on the mean and standard deviation for 
###   each measurement. 

## Read "features.txt"to get the variables that are measurements for the mean and sd.
dtFeatures <- fread(file.path(dtPath, "features.txt"))
setnames(dtFeatures, names(dtFeatures), c("featureNum", "featureName"))

## Subset mean and sd measurements
dtFeatures <- dtFeatures[grepl("mean\\(\\)|std\\(\\)", featureName)]

## Convert the column numbers to a vector of variable names matching columns in dt.
dtFeatures$featureCode <- dtFeatures[, paste0("V", featureNum)]

## Subset these variables using variable names.
tmp <- c(key(dt), dtFeatures$featureCode)
dt <- dt[, tmp, with=FALSE]


### 3. Uses descriptive activity names to name the activities in the data set

## Read "activity_labels.txt" to get descriptive names for the activities.
dtActivityNames <- fread(file.path(dtPath, "activity_labels.txt"))
setnames(dtActivityNames, names(dtActivityNames), c("activityNum", "activityName"))


### 4. Appropriately labels the data set with descriptive variable names. 

## Merge activity labels and Add activityName as a key
dt <- merge(dt, dtActivityNames, by="activityNum", all.x=TRUE)
setkey(dt, subject, activityNum, activityName)

## Melt the data table to reshape it from a short and wide format to a tall and narrow format
dt <- data.table(melt(dt, key(dt), variable.name="featureCode"))

## Merge activity name.
dt <- merge(dt, dtFeatures[, list(featureNum, featureCode, featureName)], by="featureCode", all.x=TRUE)

## Create a new variable
dt$activity <- factor(dt$activityName)
dt$feature <- factor(dt$featureName)

## Seperate features from featureName using the helper fuction grepthis.
grepthis <- function (regex) {
  grepl(regex, dt$feature)
}

## Features with 2 categories
n <- 2
y <- matrix(seq(1, n), nrow=n)
x <- matrix(c(grepthis("^t"), grepthis("^f")), ncol=nrow(y))
dt$featDomain <- factor(x %*% y, labels=c("Time", "Freq"))
x <- matrix(c(grepthis("Acc"), grepthis("Gyro")), ncol=nrow(y))
dt$featInstrument <- factor(x %*% y, labels=c("Accelerometer", "Gyroscope"))
x <- matrix(c(grepthis("BodyAcc"), grepthis("GravityAcc")), ncol=nrow(y))
dt$featAcceleration <- factor(x %*% y, labels=c(NA, "Body", "Gravity"))
x <- matrix(c(grepthis("mean()"), grepthis("std()")), ncol=nrow(y))
dt$featVariable <- factor(x %*% y, labels=c("Mean", "SD"))

## Features with 1 category
dt$featJerk <- factor(grepthis("Jerk"), labels=c(NA, "Jerk"))
dt$featMagnitude <- factor(grepthis("Mag"), labels=c(NA, "Magnitude"))

## Features with 3 categories
n <- 3
y <- matrix(seq(1, n), nrow=n)
x <- matrix(c(grepthis("-X"), grepthis("-Y"), grepthis("-Z")), ncol=nrow(y))
dt$featAxis <- factor(x %*% y, labels=c(NA, "X", "Y", "Z"))

r1 <- nrow(dt[, .N, by=c("feature")])
r2 <- nrow(dt[, .N, by=c("featDomain", "featAcceleration", "featInstrument", "featJerk", "featMagnitude", "featVariable", "featAxis")])
r1 == r2

### 5. From the data set in step 4, creates a second, independent tidy data set with 
###  the average of each variable for each activity and each subject.

setkey(dt, subject, activity, featDomain, featAcceleration, featInstrument, featJerk, featMagnitude, featVariable, featAxis)
dtTidy <- dt[, list(count = .N, average = mean(value)), by=key(dt)]

## Output to tidy dataset to file.
output <- file.path(path, "DHARUS.txt")
write.table(dtTidy, output, quote=FALSE, sep="\t", row.names=FALSE)
