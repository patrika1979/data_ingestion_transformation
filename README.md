# Case 1

![alt text](https://github.com/patrika1979/data_ingestion_transformation/blob/main/on-premises_gcp.png)

Source https://www.databricks.com/blog/2021/05/04/databricks-on-google-cloud-now-generally-available.html

# Case 2

## ETL Sales Consolidation

### Contents

1-Create public bucket
	gcloud services enable dataproc.googleapis.com
	
	gcloud storage buckets create gs://$DEVSHELL_PROJECT_ID-data --location US-CENTRAL1


2-Take Service account credentials
```
{
   "type": "service_account",
   "project_id": "project_id",
   "private_key_id": "private_key_id",
   "private_key": "-----BEGIN PRIVATE KEY-----\private_key--END PRIVATE KEY-----\n",
   "client_email": "cli-ccount-1@cliente_email.iam.gserviceaccount.com",
   "client_id": "client_id",
   "auth_uri": "https://accounts.google.com/o/oauth2/auth",
   "token_uri": "https://oauth2.googleapis.com/token",
   "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
   "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/cli-service-accoohgfdxxccnt.com"
}
```
3-In the Databricks change the spark config - https://docs.databricks.com/external-data/gcs.html
```
spark.hadoop.google.cloud.auth.service.account.enable true
spark.hadoop.fs.gs.auth.service.account.email cli-ccount-1@cliente_email.iam.gserviceaccount.com
spark.hadoop.fs.gs.project.id project_id
spark.hadoop.fs.gs.auth.service.account.private.key -----BEGIN PRIVATE KEY-----\private_key--END PRIVATE KEY-----\n
spark.hadoop.fs.gs.auth.service.account.private.key.id private_key_id
```
4- Create cluster and install the following dependencies:
	
	GetOldTweets3==0.0.11
	snscrape==0.4.3.20220106
	com.crealytics:spark-excel-2.12.17-3.2.2_2.12:3.2.2_0.18.1
	
5- In the Cloud Edito Enable API
	gcloud services enable sqladmin.googleapis.com
	
6- Create Mysql Instance

	gcloud sql instances create instance_name \
	--database-version=MYSQL_8_0 \
	--cpu=1 \
	--memory=5376MB \
	--region=us-central \
	--authorized-networks 0.0.0.0/0 \
	--region=us-central1
	
7- Change password
	
	gcloud sql users set-password root \
	--host=% \
	--instance instance_name \
	--password passw
	
8-Create database
	
	gcloud sql databases create dados \
	--instance=instance_name

9-Connect to database
	gcloud sql connect instance_name --user=root 

10-Create database - alternative
	CREATE DATABASE dados;
	
11-Run Databricks scripts

12-Go to Shell Editor and connect to db

	gcloud sql connect instance_name --user=root
	
13-Connect to db

	connect dados;

14-Query table

	select * from tweets;
