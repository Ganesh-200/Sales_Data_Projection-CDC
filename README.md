# Capture canges in sales data
## Overview
This project implements a Change Data Capture (CDC) pipeline to track and analyze sales data in real-time. It captures changes from source databases, processes the data, and projects insights for business decision-making.
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
Kinesis data stream:arn:aws:kinesis:[Region]:[AccountId]:stream/[StreamName]
lambda:arn:aws:lambda:[Region]:[AccountId]:function:lambda_transformer.py
File extension format:.json

IAM ROLES
1. kinesisFullAccess
2. S3FullAccess
```
