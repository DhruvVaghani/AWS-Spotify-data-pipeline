# AWS-Spotify-data-pipeline
This is an event driven ETL data pipeline created using Amazon Web Services.
Architecture Overview
Data Staging Layer (S3):
Raw data is stored in an S3 bucket with folders for staging and data warehouse layers.
ETL Pipeline (AWS Glue):
AWS Glue is used to create an ETL pipeline that processes and transfers data from the staging layer to the data warehouse layer.
Data Crawler (AWS Glue Crawler):
A crawler creates a catalog by identifying schema and metadata for the transformed data.
Data Querying (AWS Athena):
Athena queries the data from the transformed tables for analysis.
Data Visualization (AWS QuickSight):
QuickSight visualizes the data to derive business insights.

![image](https://github.com/user-attachments/assets/31a9cfc4-7c21-4210-9d3b-defef42495c3)




**Step 1: Setting up an IAM User**  
Firstly I started with creating an IAM user and assigning this user with the necessary permissions. Now why did I not use the root account ? Here is why:

AWS Identity and Access Management (IAM) enables you to securely control access to AWS services and resources.By creating IAM users, you can allocate specific permissions to individuals or applications that require access to your AWS resources.Using an IAM user instead of your root account ensures enhanced security, as the root account has unrestricted access to all resources and should be reserved for account administration tasks.  


While assigning the permission I ensure that I keep in mind the Principle of Least Privilege
The principle of least privilege dictates that each user or application should only have the permissions necessary to complete their task and no more. This minimizes the risk of accidental or malicious actions that could affect your AWS resources.


**Step 2: Creating an S3 Bucket**  
Now the real work begins where I started with creating an S3 bucket to store my dataset. Since this is a project where the focus is to create a pipeline I manually uploaded my data to S3 bucket. But we can also ingest the data into buckets from several sources like dynamodb, Kinesis Data Firehose etc.  
**Why use S3 buckets?**  
Amazon Simple Storage Service (S3) is a scalable, reliable, and highly available storage solution for any type of data.S3 is particularly well-suited for storing large datasets, backups, and application data due to its durability (99.999999999% or "11 nines") and flexible pricing model.
I structure the staging data and transformed data in following manner
staging for raw data.
data_warehouse for transformed data.


![image](https://github.com/user-attachments/assets/43421fdf-d77f-4dae-a8af-a8499d9976aa)





Now after this I upload preprocessed CSV files (albums, artists, and tracks) into the staging folder.



![image](https://github.com/user-attachments/assets/9f58b785-eb16-443c-97cd-ecdf42a98e2b)






**Step 3:Configure AWS Glue ETL Pipeline**  
In this step I decided to use the Glue studio to create the ETL(extract transform and load) jobs to prepare the data as per requirement. This service of AWS helps us to create jobs without writing extensive lines of codes. AWS Glue Studio is a user-friendly interface designed to simplify the creation, deployment, and management of extract, transform, and load (ETL) jobs in AWS Glue. It allows users to design ETL workflows visually without needing to write code, making it accessible for both technical and non-technical users.  
**Some key features of AWS Glue Studio why I used it:**  
Visual ETL Designer  
Code Generation  
Serverless Integration  
Preview and Debugging  

ETL VISUALIZATION

![image](https://github.com/user-attachments/assets/dd6f9fdd-e94a-4921-bbfd-8261d672a32e)






**Step 4: Running Crawler to Create a Data Catalog**  
An AWS Glue crawler is a tool that connects to your data stores, analyzes the structure of your data, and creates or updates metadata tables in the AWS Glue Data Catalog. The Data Catalog serves as a central repository for your dataset's schema and structure, enabling efficient data discovery and integration.This step helps generate tables and schema for querying.
Output: The Data Catalog  
What It Contains: Table names, columns, data types, and partitions.  


![image](https://github.com/user-attachments/assets/7d92e4c9-25b6-4310-8d7e-e052fb8d04b5)




The main reason of using the crawler in this Data Pipeline was to help Athena query the data stored in S3 buckets. With the data catalogs created by this crawler athena can read the data from S3 as if it is structured and stored in athena itself. This is specially useful for ad-hoc serverless queries on S3 data without and hassle of data movement.  

**Step 5: Querying the Data in Amazon Athena**  
Amazon Athena is an interactive query service that allows users to analyze data stored in Amazon S3 using standard SQL. It is serverless, meaning you don’t need to set up or manage any infrastructure, and you pay only for the queries you run. Athena is particularly useful for ad hoc queries and exploratory data analysis.
With this I can query my data in S3 however I want to find any information however I please.  


![image](https://github.com/user-attachments/assets/9bfd56f9-84f2-4f83-b227-66c4105dca30)

![image](https://github.com/user-attachments/assets/a6e82055-0ace-46e7-90ad-90b7d0a95789)

