---
layout: post
title: Trigger Google Cloud Workflow From Vertex AI Notebook 
---


```python
from google.auth import default
from google.auth.transport.requests import Request
import requests

credentials, project = default(scopes=["https://www.googleapis.com/auth/cloud-platform"])
credentials.refresh(Request())
access_token = credentials.token

# Construct the Workflow endpoint - You can grab this from the Workflow Details Page Invocation URL
url = f"https://workflowexecutions.googleapis.com/v1/projects/{project}/locations/{region}/workflows/{workflow_name}/executions"

# Trigger the workflow
headers = {"Authorization": f"Bearer {access_token}"}
response = requests.post(url, headers=headers)


if response.status_code == 200 or response.status_code == 201:
    print(f"Workflow {workflow_name} triggered successfully.")
else:
    print(f"Failed to trigger workflow: {response.status_code} {response.text}")

```
