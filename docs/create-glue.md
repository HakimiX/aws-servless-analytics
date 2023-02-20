# Create Glue

## Steps
- [Create Glue](#create-glue)
  - [Steps](#steps)
    - [1. Create IAM Role](#1-create-iam-role)
    - [2. Glue Network Configuration](#2-glue-network-configuration)


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
