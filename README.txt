Below is the step by step code to get the tidied data:

require(reshape2)
##Read in test data set
a1=read.table("./UCI HAR Dataset/test/X_test.txt")

##This is the 
##Read in test labels
a2=read.table("./UCI HAR Dataset/test/y_test.txt")

##Read in test subjects
a3=read.table("./UCI HAR Dataset/test/subject_test.txt")

##Combining test data set, label and subject together
a4=cbind(a1,a2,a3)

##Read in training data set
b1=read.table("./UCI HAR Dataset/train/X_train.txt")

##Read in training labels
b2=read.table("./UCI HAR Dataset/train/y_train.txt")

##Read in training subjects
b3=read.table("./UCI HAR Dataset/train/subject_train.txt")

##Combining test data set, label and subject together
b4=cbind(b1,b2,b3)

##Merge training and test data set together
c=rbind(a4,b4)

##Read in features label
dname=read.table("./UCI HAR Dataset/features.txt")

##Rename the combined training+test data set variables with the features label
names(c)=c(as.character(dname[,2]), "activity","subject")

##Getting index of the variables on mean and standard deviation plus activity and subject 
index=grep(".*mean().*|.*std().*|activity|subject", names(c))

##Subset data set based on the index above
d=c[,index]


##Reading in acitvitiy descriptive names
activity_label=read.table("./UCI HAR Dataset/activity_labels.txt", col.names=c("index","activity_names"))

##Label the activity with descriptive names 
d=merge(d, activity_label, by.x="activity", by.y="index", all=TRUE)
d=d[,-d$activity]

##generating new data set with mean of each variable by subject and by activity
melted=melt(d, id.vars=c("subject", "activity_names"))
tidy= dcast(melted, subject+ activity_names~variable, mean)

