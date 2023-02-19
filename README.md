# AWS Serverlesss Analytics Architecture

**Technologies**<br>
AWS Athena, Glue, Quicksight, and Redshift Spectrum

## Table of Contents
- [AWS Serverlesss Analytics Architecture](#aws-serverlesss-analytics-architecture)
  - [Table of Contents](#table-of-contents)
    - [Ecosystem Overview](#ecosystem-overview)
    - [Architectural Overview](#architectural-overview)
    - [Data](#data)
    - [Technologies](#technologies)
      - [AWS Glue](#aws-glue)
    - [Setup](#setup)

### Ecosystem Overview
**How does data go from source to reporting?**<br>
1. Data from web logs, clickstream, sensors, etc. is uploaded to S3 (data lake). 
2. Build a data catalog from the S3 data (i.e. transforming data, ETL).
3. Query the data using a query engine (Athena, Elasticsearch, RedShift, etc.)
4. Report to the enduser using Quicksight, Notebooks, or an API Gateway for programmatic access. 
  
![](resources/ecosystem-overview.png)

### Architectural Overview
Interoperating a data lake and a data warehouse. 
![](resources/architecture-overview.png)

### Data
Date comes from multiple sources and formats: 
![](resources/data-sources.png)

### Technologies
#### AWS Glue
* Automatically discover and categorize the data making it immediately searchable and queryable across data sources. 
* Generate code to clean, enrich, and reliably move data between various data sources. 
* Run jobs on a serverless, fully managed environment.
* Components:
  * Data Catalog: central data repository integrated with aws services.
  * Job authoring: auto-generate ETL code, edit, and debug. 
  * Deploy: serverless execution, scheduling, monitoring, and alerting. 

Move data across storage systems:
![](resources/aws-glue-overview.png)

> Glue is a middleware (ETL Layer) between data source and 
> query/reporting tools like AWS Athena and Quicksight.

:warning: Each AWS account has one AWS Data Catalog.

### Setup

1. Create bucket and upload `/s3-data`
2. Create Redshift cluster. 
3. Setup Glue (ETL and Data catalog)
   