---
layout: post
title: R Tips/Tricks/Bookmarks
---

<b> Find R Version you are running in RStudio </b> - `R.version["version.string"]`.  
<b> Find Version of Specific Package </b> - `packageVersion("package_name")`.  

<b> After Updating R Version </b>, if you have any packages that were built with an older version of R, you should reinstall those packages to avoid compatibility issues - Do `update.packages(ask = FALSE, checkBuilt = TRUE)`  
  
The following code should be run as root after upgrading R on Linux. It can also be run as a regular user, but in that case it will store the packages in the user’s personal library, and won’t help other users. Normally, the update.package() function will only reinstall packages for which a newer version available, but the checkBuilt=TRUE option tells it to also reinstall packages if they were built with an older version of R, even if the version of the package will remain the same.    
More Info about this <a href="https://shiny.rstudio.com/articles/upgrade-R.html"> here</a>.  
  
  

<b> Logging in R using `logger` </b>  
```R
library(logger)

#path to log_file. File will be created first time if it does not already exist. 
log_file_path <- "log_test.txt"

#Appends log to file specified in log_file_path
log_appender(appender_file(log_file_path))

#If you want to write logs to both console and file, use appender_tee as below
#log_appender(appender_tee(log_file_path))


log_info("Logs being written to file -{log_file_path}")
```  
  
  
<b> Using colClasses to Specify Data Type for a particular Column   </b>
```R
data <- read.csv('test.csv', colClasses=c("time"="character"))
```  

The above line of code will read the Column `time` as type `Character`. This approach is especially useful when trying to read quoted integers as character. https://stackoverflow.com/questions/2805357/specifying-colclasses-in-the-read-csv
  
 
<b> Unable to install packages Error </b>  
If you are trying to install R packages and you get an error similar to the one below - it is probably because you don't have permissions to write to that particular location.  
```R
install.packages('random')

Installing package into ‘/usr/local/lib/R/site-library’
(as ‘lib’ is unspecified)
Warning in install.packages("random") :
'lib = "/usr/local/lib/R/site-library"' is not writable
```  
So what you need to do is create a new directory in a place that doesn't require special write permissions- which you can do by running the following code  
```R
dir.create(Sys.getenv("R_LIBS_USER"), recursive = TRUE)  # create personal library
.libPaths(Sys.getenv("R_LIBS_USER"))  # add to the path

install.packages("randomForest")  # install like always
library(randomForest)  # use library like always
```
  
  Ref: https://stackoverflow.com/questions/32540919/library-is-not-writable
   
<b> Generate Automatic DocStrings for functions </b> - To generate automatic doc string templates for your R functions, place your cursor inside a function and hit `ctrl` + `shift` + `alt` + `R`.  

   
   
