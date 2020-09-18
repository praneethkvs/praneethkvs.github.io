---
layout: post
title: Getting Last Modified File in a Folder
---

Get the last modified file in a folder.

```R
setwd("folder/location")

file_list <- file.info(list.files(pattern = "*.csv"),extra_cols = FALSE) #you can optionally filter by file_type/pattern
file_name <- rownames(file_list[order(as.POSIXct(file_list$mtime),decreasing = T),])[1]
```


