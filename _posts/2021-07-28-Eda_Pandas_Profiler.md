---
layout: post
title: Python EDA with Package pandas_profiling
---

  
```python
from pandas_profiling import ProfileReport

#Pandas Profiling
profile = ProfileReport(df)
profile.to_file("pandas_profile.html",silent=False)
```  
  
    
    
