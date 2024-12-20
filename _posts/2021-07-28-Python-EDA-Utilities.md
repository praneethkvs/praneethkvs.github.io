---
layout: post
title: Python EDA Utilities
---

Automatic Exploratory Data Analysis (EDA) using <a href="https://pandas-profiling.github.io/pandas-profiling/docs/master/index.html">pandas-profiling </a> package.  
Warnings Tab is especially useful to spot data quality issues and remove unusable and strongly correlated features.  
  
```python
#!pip install pandas_profiling 
from pandas_profiling import ProfileReport

#Pandas Profiling
profile = ProfileReport(df,title="Pandas Profiling Report")
profile.to_file("pandas_profile.html",silent=False)
```  
  
Group Pandas Data Frame Features by type:      
    
```python
feats_dtypes_dict = df.columns.groupby(df.dtypes.astype(str))
feats_dtypes_dict.keys()
  
num_feats = feats_dtypes_dict["float64"].to_list() + feats_dtypes_dict["int64"].to_list()
cat_feats = feats_dtypes_dict["object"].to_list()
```      

Fetch all columns with missing values:
  
  ```python
  df.isna().sum().sort_values(ascending=False).where(lambda x: x!=0).dropna()
  ```  
    
      
      
