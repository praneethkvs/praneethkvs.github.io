---
layout: post
title: Getting the Last Modified File from a Folder (R/Python)
---

Getting the Last Modified File from a Folder

R:  
```R
setwd("folder/location")

file_list <- file.info(list.files(pattern = "*.csv"),extra_cols = FALSE) #you can optionally filter by file_type/pattern
file_name <- rownames(file_list[order(as.POSIXct(file_list$mtime),decreasing = T),])[1]
```  
   
   

Python:  

```python
def newest(path):
    files = os.listdir(path)
    files = [file for file in files if file.endswith(".csv")] #you can optionally filter by file_type/pattern
    paths = [os.path.join(path, basename) for basename in files]
    return max(paths, key=os.path.getctime)

file_name = newest("path_to_folder") #For file in current folder use newest(".")
print(file_name)
```
