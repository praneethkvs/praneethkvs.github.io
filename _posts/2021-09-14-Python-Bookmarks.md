---
layout: post
title: Python Tips/Tricks/Bookmarks
---

•Force Python to use/import specific version of a package.  
Source - https://stackoverflow.com/questions/6445167/force-python-to-use-an-older-version-of-module-than-what-i-have-installed-now  

Make sure you have the version that you need already installed.  
```python
import pkg_resources
pkg_resources.require("scikit-learn==0.24.1")

import sklearn
sklearn.__version__
```

•Logging in Python.  
Use package <a href="https://docs.python.org/3/library/logging.html"> `logging` </a>    
```python
#Import and Configure Logger
import logging
logging.basicConfig(level=logging.INFO,format='%(asctime)s :: %(levelname)s :: %(message)s')

#Initialize logger
logger = logging.getLogger()

#Log Message
logger.info("Read {} lines from - {}".format(data_frame.shape[0],input_file))
```

  
    
    
