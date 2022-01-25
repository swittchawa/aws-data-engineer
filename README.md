# E-commerce Data Pipeline with AWS

# Dataset
This is a transnational data set which contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail.The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers.

# Purpose
### Main Goal
- Process transactions from Stores in single location
- Give customer access to their purchase history
    - Overview of purches
    - Purchase details

### Secondary (Business Intelligence Goals)
- Averages sales per h/d/m/y
- Most sold products d/m/y
- Most valuable product d/m/y
- Highest grossing customer per d/m/y
- Highest invoices for the d/m/y

# Data Pipelines
- Ingestion
    - Client:
        - Simulates Streaming
        - Sends CSV Rows as JSON
    - API-Gateway
    - Lambda
    - Kinesis
- Stream to S3 Raw Storage
    - Kinesis insert triggers Lambda for S3
        - Lambda waits for some time
        - Writes all messages in queue to S3 Bucket as file
- Stream to DynamoDB
    - Kenesis insert triggers Lambda for DynamoDB
        - Lambda reformates/preprocesses messages
        - Lambda writes customer data (customer + invoices)
        - Lambda writes invoice data (invoice + stockcode)
- Batch Processing
    - Bulk import pipeline
    - Triggered through Cloudwatch
    - Lambda reads from S3 folder
    - Writes data into DynamoDB
    - Writes into Redshift
- Visualisation API
    - API for UI (Items in Invoice)
        - Data rests in DynamoDB table invoices
        - Client requests items for InvoiceNo (Request parameter)
        - Lambda triggered by API queries Dynamo with InvoiceNo
- Visualisation Redshift Data Warehouse
    - Kinesis Firehouse Delivery Stream connects to Kinesis Data Stream
    - Firehouse writes data into intermediate S3 Bucket
    - Kinesis Firehouse copies data into Redshift table
    - Tableau for Analyst access

# Platform
#### Client
- CSV file
- Python client

#### Connect
API Gateway

#### Buffer
- Kinesis
- Kafka

#### Processing
- Lambda
- CloudWatch

#### Store
- S3
- DynamoDB NoSQL
    - Wide Column Store
    - Backend
    - Transactions
- Redshift Data Warehouse
    - Analytics Layer
    - Distributed Storage and Processing

#### Visualise
- Tableau
    - Business Intelligence Tool
    - Connects to Redshift
- API Gateway
    - Access for Apps, UIs
    - Execute Queries and Transactions
    - Simple and Stateless
