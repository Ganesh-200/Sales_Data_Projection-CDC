# Capture canges in sales data
## Overview
This project implements a Change Data Capture (CDC) pipeline to track and analyze sales data in real-time. It captures changes from source databases, processes the data, and projects insights for business decision-making.
## Workflow
![Sales_Data-CDC](https://github.com/user-attachments/assets/c06bd4a5-fcb0-47a8-ac8c-d3520844c082)
## Prerequisites
AWS CLI installed in local machine
boto3 library installed in local machine
## STEPS
### 1.DynamoDB
Create a DynamoDB table with CDC enabled
```ini
partition key:'orderid'
view type:'New image'
```
