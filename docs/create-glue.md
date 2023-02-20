# Create Glue

## Steps
- [Create Glue](#create-glue)
  - [Steps](#steps)
    - [1. Create IAM Role](#1-create-iam-role)
    - [2. Glue Network Configuration](#2-glue-network-configuration)
    - [3. AWS Glue Data Catalog](#3-aws-glue-data-catalog)
    - [4. AWS Glue database](#4-aws-glue-database)
    - [5. AWS Glue Tables](#5-aws-glue-tables)


### 1. Create IAM Role

Create an IAM role with the following policies:
```shell
AWSGlueServiceRole
AmazonS3FullAccess
```

![](../resources/glue/glue-iam-trusted-entity.png)
![](../resources/glue/glue-iam-permissions.png)

### 2. Glue Network Configuration

:warning: AWS Glue must be able to access the data stores in order to run ETL jobs. Transforming data from S3 to S3 requires no additional configuration as that runs outside of the VPC. 

All JDBC data stores that are accessed by the ETL job must be available from the VPC subnet. To access S3 from within the VPC, a VPC endpoint is required. 

**If you need access to data stores in difference VPC's**
* Use VPC peering. 
* Use S3 bucket as an intermediary storage location. 

![](../resources/glue/glue-vpc-endpoints.png)
A VPC endpoint for S3 enables AWS Glue to use a private IP address
to access S3 with no exposure to the public internet. **Traffic between the VPC and the AWS Service does not leave the Amazon Network**. 
Any requests to the S3 endpoint within the region are routed to a private S3 endpoint within the Amazon Network. 
> AWS Glue does not require public IP addresses, and you don't need an internet gateway, a NAT device, or a virtual private gateway in the VPC. 

Create a VPC endpoint for S3. 

![](../resources/vpc-endpoints/s3-vpc-endpoint-1.png)
![](../resources/vpc-endpoints/s3-vpc-endpoint-2.png)
![](../resources/vpc-endpoints/s3-vpc-endpoint-3.png)

### 3. AWS Glue Data Catalog

The AWS Glue Data Catalog contains references to data that is used as sources and targets of the ETL jobs in Glue. 

*To create a data warehouse, you must catalog the data*

The AWS Glue Data Catalog is an index to the location, schema, and runtime metrics of the data. The information in the Data Catalog is used to create and monitor the ETL jobs. 

**AWS Glue is a central metadata catalog for the data lake**, it allows you to share metadata between Athena, Redshift Spectrum, and EMR. 

![](../resources/glue/glue-gather-data.png)

### 4. AWS Glue database

Create a database in AWS Glue to organize tables. The database can contain tables that define data from many different data stores (S3, Timestream, RDS, etc.). 
> A Database in the Glue Data Catalog is a container that holds tables. 
> :warning: Keep in mind that this is not an RDS or Redshift database. It is just a schema definition (top level object) with different schemas in the form of tables. 

Database are used to organize and categorize the data, i.e. a database for S3 data, database for Timestream data, etc. 

![](../resources/glue/glue-database-1.png)
![](../resources/glue/glue-database-2.png)

### 5. AWS Glue Tables

When you define a table in Glue, you specify the value of a classification field that indicates the type and format of the data that's stored in that table. 

If a crawler creates the table, these classiciations are determined by either a built-in classifier or a custom classifier. 

**When a crawler detects a change in table metadata**, a new version of the table is created in the Glue Data Datalog. 

Create a table for data in an S3 bucket where the classification is set to `csv` as the bucket contains csv files. 
![](../resources/glue/glue-table-for-s3.png)

Add a sample column for ID (more can be added).
![](../resources/glue/glue-table-column.png)

The table is created 
![](../resources/glue/glue-table-created.png)

Optionally add additional columns to the table (mapped to the csv data)
![](../resources/glue/glue-table-more-columns.png)

**Previewing Data**
You are redirected to AWS Athena if you want to view the data (action -> view data)
![](../resources/glue/glue-view-data.png)