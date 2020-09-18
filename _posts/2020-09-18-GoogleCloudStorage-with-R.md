Using the Google Cloud Storage API with R

```R

#Set Authentication and Default Bucket
Sys.setenv("GCS_DEFAULT_BUCKET" = "my_gcs_bucket_name",
           "GCS_AUTH_FILE" = "my_gcs_authfile_path",
           "GCP_PROJECT" = "my_gcs_project")

#Load package
library(googleCloudStorageR)

## check what the default bucket is
gcs_get_global_bucket()

# get object info in the default bucket
objects <- gcs_list_objects()

# set folder path in bucket
my_path <- 'my_folder/path'

# search for files/objects 
search_str <- ".csv"
search_res <- objects$name[grepl(search_str,objects$name)]
search_res

file_name <- search_res[1]

#Download Object and savetodisk
gcs_get_object(file_name, saveToDisk = strsplit(file_name,'/')[[1]][length(strsplit(file_name,'/')[[1]])])

#Upload Object
#Resumable uploads for files > 5MB up to 5TB

upload_file_name <- "my_file.csv"
upload_file_path <- paste0(my_path,upload_file_name)
upload_try <- gcs_upload(file=upload_file_name,name=upload_file_path)

## if successful, upload_try is an object metadata object
upload_try

#### if unsuccessful after 3 retries, upload_try is a Retry object
## you can retry to upload the remaining data using gcs_retry_upload()
try2 <- gcs_retry_upload(upload_try)

```

Ref: https://cran.r-project.org/web/packages/googleCloudStorageR/vignettes/googleCloudStorageR.html
