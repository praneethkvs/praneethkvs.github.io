---
layout: post
title: Google CLoud Workflow Scripts!
---

1) **The workflow appends query results to a bigquery dataset using the BigQuery connector.**

```YAML
# The workflow appends query results to a bigquery dataset using the BigQuery connector.
# Expected successful output: "SUCCESS".

- init:
    assign:
    - project_id: ${sys.get_env("GOOGLE_CLOUD_PROJECT_ID")}
    - dataset_id: "my_dataset"
    - table_id: "my_table"
    - query: 'SELECT * FROM `bigquery-public-data.usa_names.usa_1910_2013` LIMIT 50;'
    - create_disposition: "CREATE_IF_NEEDED"  # create a new one if table doesn't exist
    - write_disposition: "WRITE_APPEND"  # Append to table it if the table already exists
- insert_table_into_dataset:
    call: googleapis.bigquery.v2.jobs.insert
    args:
      projectId: ${project_id}
      body:
        configuration:
          query:
            query: ${query}
            destinationTable:
              projectId: ${project_id}
              datasetId: ${dataset_id}
              tableId: ${table_id}
            create_disposition: ${create_disposition}
            write_disposition: ${write_disposition}
            allowLargeResults: true
            useLegacySql: false
- the_end:
    return: "SUCCESS"
```
  
  
  
2) **The workflow runs a bigQuery query, evaluates a conditional statement, raises an Error if the query returns 0 rows, otherwise proceeds and outputs the number of rows returned.**
```YAML
- init:
    assign:
    - project_id: ${sys.get_env("GOOGLE_CLOUD_PROJECT_ID")}
    - query: 'SELECT count(distinct name) FROM `bigquery-public-data.usa_names.usa_1910_2013`'
- runBQquery: 
    call: googleapis.bigquery.v2.jobs.query
    args:
        projectId: ${project_id}
        body:
            useLegacySql: false
            query: ${query}
    result: queryResult
- evaluate:
    switch:
        - condition: ${int(queryResult.rows[0].f[0].v) = 0}
          next: triggerError
    next: documentFound
- triggerError:
    raise: 'Please Check Query - 0 Rows returned'        
- documentFound:
    return: ${queryResult}
```  

To Trigger an email alert when a workflow fails, use **Google Cloud Monitoring** to create an alert with the below parameters and configure notification channels.   
**Metric** - Workflow - Finished Execution Count  
**Add Filters** - 
a)status = FAILED b)workflow_id = my_workflow_id  
**Condition Type** - Threshold (Any time series violates Above Threshold 0) 
  
    
    
3) **The workflow is similar to the one above except, it calls a Stored Procedure rather than specifying the SQL within the Workflow**  

First, Create a Stored Procedure with your SQL as shown below.  
```SQL
CREATE OR REPLACE PROCEDURE `my_project.my_dataset.my_stored_proc`()
BEGIN

SELECT count(distinct name) FROM `bigquery-public-data.usa_names.usa_1910_2013`

END;
```  
Then Call the Stored Procedure  using **call**.  

```YAML
- init:
    assign:
    - project_id: ${sys.get_env("GOOGLE_CLOUD_PROJECT_ID")}
- runBQquery: 
    call: googleapis.bigquery.v2.jobs.query
    args:
        projectId: ${project_id}
        body:
            useLegacySql: false
            query: call my_dataset.my_stored_procedure();
    result: queryResult
- evaluate:
    switch:
        - condition: ${int(queryResult.rows[0].f[0].v) = 0}
          next: triggerError
    next: documentFound
- triggerError:
    raise: 'Please Check Stored Procedure - 0 rows returned'        
- documentFound:
    return: ${queryResult}
```
  
