
#Step 1 - Process Train Data & Add Activity/Subject Columns

run_analysis<-function() { setwd("C:/Users/Gupta/Desktop/Coursera/UCI HAR Dataset/train") train.data<-read.table("X_train.txt",sep="") train.activity<-read.table("y_train.txt",sep="") train.subject<-read.table("subject_train.txt",sep="") train.data<-cbind(train.subject,train.activity,train.data)

#Step 2 - A) Process Test Data & Add Activity/Subject Columns B) Merge Test & Train Data

setwd("C:/Users/Gupta/Desktop/Coursera/UCI HAR Dataset/test") test.data<-read.table("X_test.txt",sep="") test.activity<-read.table("y_test.txt",sep="") test.subject<-read.table("subject_test.txt",sep="") test.data<-cbind(test.subject,test.activity,test.data) total.data<-rbind(test.data,train.data)

#Step 3 - Assign Column Names & Orders Merged Data

setwd("C:/Users/Gupta/Desktop/Coursera/UCI HAR Dataset") read.labels<-read.table("features.txt", sep="") activity.labels<-read.table("activity_labels.txt",sep="") read.labels<-as.matrix(read.labels[2]) read.labels<-rbind("subject","activity",read.labels) colnames(total.data)<-read.labels

total.data<-total.data[order(total.data$subject,total.data$activity),]

#Step 4- Assigns Descriptive Activity Names (qdap package using mgub)

total.data$activity<-mgsub(activity.labels[,1],activity.labels[,2],total.data$activity)

#Step 5- Extracts Mean/Std Data
mean.std.data<-total.data[, grepl( "[Mm]ean|[Ss]td|subject|activity" , names(total.data)) ]

#Step 6 - Average of Each Variable by Acitivity/Subject (dplr package)

new.set <- summarise_each(group_by(mean.std.data, subject, activity), funs(mean))

#Step 7: Write Text File
write.table(new.set,file="final_data.txt", row.names=FALSE)

final result file
return(new.set)
