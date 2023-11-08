---
title: How to restore an Elasticsearch Snapshot locally
date: "2023-10-03T18:12:03.284Z"
categories:
    - blog
tags:
  - howto
  - elastic
  - kibana
draft: false
---
![Pawel Czerwinski](https://images.unsplash.com/photo-1541972289615-de502ba75939?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80)

## Introduction:
This guide outlines the process of restoring a copy of an Elasticsearch snapshot on your local machine. This can be especially useful when you need to troubleshoot issues in a database for which you don't have direct access, without having to create an API for interaction. Since this procedure lacks [documentation][2], we aim to provide you with a clear step-by-step guide.

## Procedure:

1. Copy the Elasticsearch Snapshot:

To begin, retrieve the Elasticsearch snapshot from its repository. 
If you're using Google Cloud Platform (GCP), follow these steps:
- Use the 'gscopy' command to copy all files from the repository. 
```bash
gsutil -m cp -r gs://${BUCKET_NAME}/${SNAPSHOT_NAME}/* ./
```
- Compress all the copied files into a zip archive. 
```bash
zip -r ${ZIP_FILE} ./${SNAPSHOT_NAME}
```
- Download the zip file to your local machine. 
```bash
gcloud compute scp ${ZIP_FILE} YOUR_LOCAL_MACHINE_USERNAME@YOUR_LOCAL_MACHINE_IP:~/ --zone=YOUR_LOCAL_MACHINE_ZONE
```
- From your terminal, extract the contents of the zip file. 
```bash
unzip ${ZIP_FILE} -d ${LOCAL_PATH}
```
- Note down the path to the extracted snapshot.


2. Update Your Elasticsearch Configuration:
- Locate the Elasticsearch configuration YAML file on your local machine. Depending on your setup, the configuration file is commonly named elasticsearch.yml.
- Open the configuration file using a text editor of your choice. You may need administrative privileges to modify this file.
- Find the section where you can specify the Elasticsearch repositories. It often looks like this:

```yaml
path:
  repositories:
    - /path/to/your/backup/repository
```
- Add the path to the extracted snapshot you noted down from step 1. Ensure it matches your Elasticsearch configuration's expected format. It should resemble something like this:

```yaml

path:
  repositories:
    - /path/to/your/backup/repository
    - /path/to/the/extracted/snapshot
```
- Save and close the configuration file.

3. Restore the Index:
Utilize the Curl command to interact with the Elasticsearch restore index API. 
- Query the [Reindex API][1]:
```bash
curl -X POST "http://localhost:9200/_snapshot/your_repository_name/snapshot_name/_restore"
```
- Replace your_repository_name and snapshot_name with the appropriate values for your snapshot.
- Monitor the response for success. A successful response indicates that the index has been restored.

## Conclusion:
By following these steps, you'll be able to restore an Elasticsearch snapshot locally on your machine, facilitating troubleshooting and analysis without the need for direct access to the original database.

Ciao :)

[1]: https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html
[2]: https://discuss.elastic.co/t/issue-in-restoring-an-elastic-snapshot/343329/4 
