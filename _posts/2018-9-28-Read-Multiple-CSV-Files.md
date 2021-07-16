---
layout: post
title: Read Multiple CSV Files into a single Dataframe in R and Python
---

<h1>R</h1>  
To Read multiple CSV Files in R into a single Dataframe.  

1) Put all the files into a single directory.     
2) Set working directory to that directory.     
3) Use `do.call` to iteratively `read.csv` from the `file_list` which contains the list of the names of all the csv files and `rbind` the resulting output into a single dataframe.

```R
setwd("Directory/")
file_list <- dir()

data_frame <- do.call(rbind,lapply(filelist,function(x) {read.csv(x,stringsAsFactors = F)]}))
write.csv(data_frame,"filename.csv",row.names = F)
```
  
<h1>Python</h1>  
To Read multiple CSV Files in Python into a single Pandas Dataframe.  

1) Put all the files into a single directory.     
2) Set working directory to that directory in R.     
3) Use list comprehension to iteratively `pd.read_csv` from the `file_list` which contains the list of the names of all the csv files and `pd.concat` the resulting output into a single dataframe.
  
  
```python
import pandas as pd
import os

os.chdir("Directory/")
file_list = os.listdir()

data_frame = pd.concat((pd.read_csv(f) for f in file_list))
```  
 
If you need to filter out the `file_list` to only include a subset of files containing a specific substring:   
```python
files_sublist = [f for f in files_list if "substring" in f]
```  

If you are working with <b>Google Cloud Storage</b>:
```python
import pandas as pd
import os

from google.cloud import storage
bucket_name= "bucket_name" 
folder_path = "folder_path"

#Get list of objects in the folder path
client = storage.Client()
files_list = ["gs://"+bucket_name+"/"+str(blob.name) for blob in client.list_blobs(bucket_name, prefix=folder_path)]

#Get subset of files containing a substring and read them
files_sublist = [file for file in files_list if "good" in file]
data_frame = pd.concat((pd.read_csv(f) for f in files_sublist))
```
  
    
    
   
   
  
  
  
  
