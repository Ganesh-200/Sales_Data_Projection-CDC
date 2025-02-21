# CDC in sales data
## Overview
This project implements a Change Data Capture (CDC) pipeline to track and analyze sales data in real-time. It captures changes from source databases, processes the data, and projects insights for business decision-making.
## Features
✅ **CDC Implementation** – Captures inserts, updates, and deletes from the sales database  
✅ **Data Pipeline** – Processes and stores data in a scalable architecture  
✅ **AWS Services Used** – DynamoDB, Kinesis, EventBridge, S3, Glue, Lambda, Athena  
✅ **Real-Time & Batch Processing** – Supports real-time event-driven workflows and scheduled data processing  
## Tech Stack
🔹 **AWS** (Kinesis, DynamoDB, Athena, Glue, Lambda, EventBridge, S3)  
🔹 **Apache Spark / AWS Glue** for data transformation  
🔹 **Python** (Boto3, PySpark)  
🔹 **SQL** (Athena)  
## Workflow
![Sales_Data-CDC](https://github.com/user-attachments/assets/c06bd4a5-fcb0-47a8-ac8c-d3520844c082)
## Prerequisites
AWS CLI, boto3 library installed in local machine.
## Configuration 
### 1.DynamoDB
Create a DynamoDB table with CDC enabled to store incoming mock data.
```
partition key:'orderid'
view type:'New image'
```
### Amazon EventBridge Pipes
Create a EventBridge pipe to create point-to-point integrations between event producers and consumers.
```
PIPE Config
Source:DynamoDB
Target:Kinesis
partition key:'$.eventid'

IAM ROLES
1. dynamoDBFullAccess
2. AmazonkinesisFullAccess
```
### Kinesis
Create Kinesis data stream to stream data from DynamoDB table to S3 bucket.
### Amazon S3
Create a storage bucket to store the streaming data as destination.
### Lambda
Create a lambda function for the transformation of streaming data and use lambda_transformer.py as transformation code.
### Amazon Data Firehose
Create a Firehose stream to processes and deliver streaming data to destinations.
```
FIREHOSE Config
source:kinesis stream
destination:Amazon S3
Kinesis data stream:'arn:aws:kinesis:[Region]:[AccountId]:stream/[StreamName]'
lambda:'arn:aws:lambda:[Region]:[AccountId]:function:lambda_transformer.py'
File extension format:'.json'

IAM ROLES
1. kinesisFullAccess
2. S3FullAccess
```
### AWS Glue
#### Classifiers
Create a classifier to classify data based on type and path.
```
type:JSON
path:'$.orderID,$.product_name,$.quantity,$.price,$.event_type,$.creation_time'
```
#### database
Create a database to store crawled data's metadata.
#### crawler
Create a crawler to crawl the output data stored in S3 location.
```
Data source:S3
database:Glue database
custom classifier:glue classifier

IAM ROLES
1. S3FullAccess
2. GlueFullAccess
```
### Athena
```
database:Glue database
queries:some select queris based on the modification
        (ex:SELECT * FROM glue_table WHERE product_name='laptop';
            SELECT * FROM glue_table WHERE product_name='laptop' AND quantity=5;)
```
## How to Run
1. Run dynamodb_mock_data_gen.py from the terminal of the local machine to ingest mock data to stream, process and store in S3 location.
2. Run the crawler to crawl the stored data in S3.
3. To check CDC is getting captured or not, mopdify any table item in DynamoDB table and see in athena by running same select queries before the modification and after the modification.

