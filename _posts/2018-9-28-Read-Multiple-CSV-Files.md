---
layout: post
title: Read Multiple CSV Files into R
---

If you want to Read multiple CSV Files into R into a single Dataframe.
Put all the files into a single directory.
Set working directory to that directory in R.
filelist contains the list of the names of all the csv files.

```R
setwd("Directory/")
filelist <- dir()

data <- do.call(rbind,lapply(filelist,function(x) {read.csv(x,stringsAsFactors = F)]}))
write.csv(data,"filename.csv",row.names = F)
```
