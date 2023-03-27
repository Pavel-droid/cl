These are used variables

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
