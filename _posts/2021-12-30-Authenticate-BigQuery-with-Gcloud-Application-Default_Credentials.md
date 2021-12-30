---
layout: post
title: Authenticate BigQuery with Gcloud using Application Default Credentials
---

You need to have Google Cloud Shell SDK installed on your local machine - https://cloud.google.com/sdk/docs/install  
Open Google Cloud SDK Shell and type the below command.  
```bash
gcloud auth application-default login
```
This will open up a browser where you can authenticate.
The above command then create an 'application_default_credentials.json' credential file in the User Config Directory. 
You can find the config directory path using `gcloud info`. Look for the User Config Directory path.

You can use this to authenticate bigrquery by specifying the path to the credentials file.  
```R
library(bigrquery)
bigrquery::bq_auth(path="C:/~/gcloud/application_default_credentials.json")
```
