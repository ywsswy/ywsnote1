
=org.apache.log4cpp.RollingFileAppender #按照fileName来写日志，满足条件则移动/重命名为备份文件
=org.apache.log4cpp.TimeRollingFileAppender #直接按照"fileName""backupPattern"来写日志

.fileName= 
.maxFileSize=1000000000 #文件最大大小（字节）
.backupPattern=.%Y-%m-%d-%H
.maxFileAge=3600
.layout=org.apache.log4cpp.PatternLayout