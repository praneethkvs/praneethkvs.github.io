---
layout: post
title: Setting Up R Environment
---

Refs:   
https://rstats.wtf/r-startup.html#renviron  
http://www.dartistics.com/googleanalytics/setup.html#:~:text=Renviron%20file%20in%20your%20Home,Renviron%20.    

Two main Setup files.  
<b>`.Renviron` </b> - which contains environment variables to be set in R sessions.  
<b>`.Rprofile` </b> - which contains R code to be run in each session.  
  
These files are R specific instances of a broader family of customization files commonly referred to as dotfiles. These are used to tailor the behavior of many programs, particularly those with roots in the unix command line.
You can store Authentication Keys and other global variables.  


If you don't already have one setup, create a new file in R's home directory `Sys.getenv("HOME")` and save it as `.Renviron`.
You can check if you have one using the command ```normalizePath("~/.Renviron", mustWork = FALSE)```  

A simple example of a .Renviron file is

```txt
R_HISTSIZE=100000
GITHUB_PAT=abc123
R_LIBS_USER=~/R/%p/%v
GCS_DEFAULT_BUCKET" = "bucket_name"
GCS_AUTH_FILE" = "path/to/key/file.json"
GCP_PROJECT" = "project_name"
```

The `R_LIBS_USER` specifies the default location of R libraries. All packages will be installed to this folder.

In R, you can access the environment variables by using the `Sys.getenv()` function. If you don’t add an argument to the function, it will return all the environment variables. To get access to NAME, you do `Sys.getenv('NAME')`

If you want to set Environment Variables only for your current session, instead of writing the variables to the .Renviron file, you can use `Sys.setenv()`.  
```R
Sys.setenv("GCS_DEFAULT_BUCKET" = "bucket_name",
           "GCS_AUTH_FILE" = "path/to/key/file.json",
           "GCP_PROJECT" = "project_name")
```           
