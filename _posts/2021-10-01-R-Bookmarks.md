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
  
  