This is a readme with code and description

# Load necessary libraries
library(dplyr)

# Set the working directory
setwd("UCI HAR Dataset")

# Read in the data
features <- read.table("features.txt")
activity_labels <- read.table("activity_labels.txt")

X_train <- read.table("train/X_train.txt")
y_train <- read.table("train/y_train.txt")
subject_train <- read.table("train/subject_train.txt")

X_test <- read.table("test/X_test.txt")
y_test <- read.table("test/y_test.txt")
subject_test <- read.table("test/subject_test.txt")

# Step 1: Merge the training and test sets to create one data set
X <- rbind(X_train, X_test)
y <- rbind(y_train, y_test)
subject <- rbind(subject_train, subject_test)
merged_data <- cbind(subject, y, X)

# Step 2: Extract only the measurements on the mean and standard deviation for each measurement
mean_std_features <- features[grep("mean\\(\\)|std\\(\\)", features$V2), ]
mean_std_data <- merged_data[, c(1, 2, mean_std_features$V1 + 2)]

# Step 3: Use descriptive activity names to name the activities in the data set
names(mean_std_data) <- c("subject", "activity", as.character(mean_std_features$V2))
mean_std_data$activity <- factor(mean_std_data$activity, levels = activity_labels$V1, labels = activity_labels$V2)

# Step 4: Appropriately label the data set with descriptive variable names
names(mean_std_data) <- gsub("^t", "time", names(mean_std_data))
names(mean_std_data) <- gsub("^f", "frequency", names(mean_std_data))
names(mean_std_data) <- gsub("Acc", "Accelerometer", names(mean_std_data))
names(mean_std_data) <- gsub("Gyro", "Gyroscope", names(mean_std_data))
names(mean_std_data) <- gsub("Mag", "Magnitude", names(mean_std_data))
names(mean_std_data) <- gsub("BodyBody", "Body", names(mean_std_data))

# Step 5: Create a second, independent tidy data set with the average of each variable for each activity and each subject
tidy_data <- mean_std_data %>%
  group_by(subject, activity) %>%
  summarise_all(mean)

# Write the output to a file
write.table(tidy_data, "tidy_data.txt", row.names = FALSE)
