# Introduction

* A serverless data lake architecture enables agile and self-service data onboarding and analytics for all data consumer roles across a company. By using AWS serverless technologies as building blocks, you can rapidly and interactively build data lakes and data processing pipelines to ingest, store, transform, and analyze petabytes of structured and unstructured data from batch and streaming sources, without needing to manage any storage or compute infrastructure.

* This architecture includes a data lake, data processing pipelines, and a consumption layer that enables several ways to analyze the data in the data lake without moving it, including business intelligence (BI) dashboarding, exploratory interactive SQL, big data processing, predictive analytics, and ML.

# Logical architecture of modern data lake centric analytics platforms

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ljbs5k29hz0kp8ca8oy8.png)
*Architecture of a data lake centric analytics platform*

* You can think of a data lake centric analytics architecture as a stack of six logical layers, where each layer is composed of multiple components. A layered, component-oriented architecture promotes separation of concerns, decoupling of tasks, and flexibility. 

* This provides the agility needed to quickly integrate new data sources, support new analytics methods, and add tools required to keep up with the accelerating pace of changes in the analytics landscape. In the following sections, we look at the key responsibilities, capabilities, and integrations of each logical layer.
 
* The **ingestion layer** is responsible for bringing data into the data lake. It provides the ability to connect to internal and external data sources over a variety of protocols. It can ingest batch and streaming data into the storage layer.

* The **storage layer** is responsible for providing durable, scalable, secure, and money- saving components to store vast quantities of data.

* The **cataloging and search layer** is responsible for storing business and technical metadata about datasets hosted in the storage layer. 

* The **processing layer** is responsible for transforming data into a consumable state through data validation, cleanup, normalization, transformation, and enrichment. 

* The **consumption layer** is responsible for providing scalable and performant tools to gain insights from the vast amount of data in the data lake. 

* The **security and governance layer** is responsible for protecting the data in the storage layer and processing resources in all other layers. 

# Serverless data lake centric analytics architecture

* To compose the layers described in our logical architecture, AWS introduces a reference architecture that uses AWS serverless and managed services. In this approach, AWS services provide the following capabilities:
 * Providing and managing scalable, resilient, secure, and cost-effective infrastructural components
 * Ensuring infrastructural components natively integrate with each other

* This reference architecture enables you to focus more time on rapidly building data and analytics pipelines. It significantly accelerates new data onboarding and driving insights from your data. 

* The AWS serverless and managed components enable self-service across all data consumer roles by providing the following key benefits:
 * Easy configuration-driven use
 * Freedom from infrastructure management
 * Pay-per-use pricing model
The following diagram illustrates this architecture.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jg5npvnxdja05jnkmhul.png)
*AWS Serverless Data Analytics Pipeline Reference Architecture* 

# Ingestion layer

* The ingestion layer in the presented serverless architecture is composed of a set of purpose-built AWS services to enable data ingestion from a variety of sources. 

* Each of these services enables simple self-service data ingestion into the data lake landing zone and provides integration with other AWS services in the storage and security layers. 

* Individual purpose-built AWS services match the unique connectivity, data format, data structure, and data velocity requirements of operational database sources, streaming data sources, and file sources.

## -> Operational database sources

* Typically, organizations store their operational data in various relational and NoSQL databases. AWS Data Migration Service (AWS DMS) can connect to a variety of operational RDBMS and NoSQL databases and ingest their data into Amazon Simple Storage Service (Amazon S3) buckets in the data lake landing zone. 

* With AWS DMS, you can first perform a one-time import of the source data into the data lake and replicate ongoing changes happening in the source database. 

* AWS DMS encrypts S3 objects using AWS Key Management Service (AWS KMS) keys as it stores them in the data lake. AWS DMS is a fully managed, resilient service and provides a wide choice of instance sizes to host database replication tasks.

* AWS Lake Formation provides a scalable, serverless alternative, called blueprints, to ingest data from AWS native or on-premises database sources into the landing zone in the data lake. 

* A Lake Formation blueprint is a predefined template that generates a data ingestion AWS Glue workflow based on input parameters such as source database, target Amazon S3 location, target dataset format, target dataset partitioning columns, and schedule. 

* A blueprint-generated AWS Glue workflow implements an optimized and parallelized data ingestion pipeline consisting of crawlers, multiple parallel jobs, and triggers connecting them based on conditions. 

## -> Streaming data sources

* The ingestion layer uses Amazon Kinesis Data Firehose to receive streaming data from internal and external sources. With a few clicks, you can configure a Kinesis Data Firehose API endpoint where sources can send streaming data. 

* This streaming data can be clickstreams, application and infrastructure logs and monitoring metrics, and IoT data such as devices telemetry and sensor readings. Kinesis Data Firehose does the following:
 * Buffers incoming streams
 * Batches, compresses, transforms, and encrypts the streams
 * Stores the streams as S3 objects in the landing zone in the data lake

* Kinesis Data Firehose natively integrates with the security and storage layers and can deliver data to Amazon S3, Amazon Redshift, and Amazon Elasticsearch Service (Amazon ES) for real-time analytics use cases. 

* Kinesis Data Firehose is serverless, requires no administration, and has a cost model where you pay only for the volume of data you transmit and process through the service. Kinesis Data Firehose automatically scales to adjust to the volume and throughput of incoming data.

## -> File sources

* Many applications store structured and unstructured data in files that are hosted on Network Attached Storage (NAS) arrays. Organizations also receive data files from partners and third-party vendors. Analyzing data from these file sources can provide valuable business insights

### Internal file shares

* AWS DataSync can ingest hundreds of terabytes and millions of files from NFS and SMB enabled NAS devices into the data lake landing zone. 

* DataSync automatically handles scripting of copy jobs, scheduling and monitoring transfers, validating data integrity, and optimizing network utilization. 

* DataSync can perform one-time file transfers and monitor and sync changed files into the data lake. DataSync is fully managed and can be set up in minutes.

### Partner data files

* FTP is most common method for exchanging data files with partners. The AWS Transfer Family is a serverless, highly available, and scalable service that supports secure FTP endpoints and natively integrates with Amazon S3

## -> Data APIs

* Organizations today use SaaS and partner applications such as Salesforce, Marketo, and Google Analytics to support their business operations. 

* Analyzing SaaS and partner data in combination with internal operational application data is critical to gaining 360- degree business insights. Partner and SaaS applications often provide API endpoints to share data.

### SaaS APIs

* The ingestion layer uses Amazon AppFlow to easily ingest SaaS applications data into the data lake. With a few clicks, you can set up serverless data ingestion flows in Amazon AppFlow. 

* Your flows can connect to SaaS applications (such as Salesforce, Marketo, and Google Analytics), ingest data, and store it in the data lake. You can schedule Amazon AppFlow data ingestion flows or trigger them by events in the SaaS application. Ingested data can be validated, filtered, mapped, and masked before storing in the data lake. 

* Amazon AppFlow natively integrates with authentication, authorization, and encryption services in the security and governance layer.

### Partner APIs

* To ingest data from partner and third-party APIs, organizations build or purchase custom applications that connect to APIs, fetch data, and create S3 objects in the landing zone by using AWS SDKs. These applications and their dependencies can be packaged into Docker containers and hosted on AWS Fargate. 

* AWS Glue Python shell jobs also provide serverless alternative to build and schedule data ingestion jobs that can interact with partner APIs by using native, open-source, or partner-provided Python libraries. 

* AWS Glue provides out-of-the-box capabilities to schedule singular Python shell jobs or include them as part of a more complex data ingestion workflow built on AWS Glue workflows.

### Third-party data sources

* Your organization can gain a business edge by combining your internal data with third- party datasets such as historical demographics, weather data, and consumer behavior data. 

* AWS Data Exchange provides a serverless way to find, subscribe to, and ingest third-party data directly into Amazon S3 buckets in the data lake landing zone. 

# Storage layer

* Amazon S3 provides the foundation for the storage layer in our architecture. Amazon S3 provides virtually unlimited scalability at low cost for our serverless data lake. 

* Data is stored as S3 objects organized into raw, cleaned, and curated zone buckets, and prefixes. Amazon S3 encrypts data using keys managed in AWS KMS. IAM policies control granular zone-level and dataset-level access to various users and roles.

* Amazon S3 provides 99.99% of availability and 99.999999999% of durability, and charges only for the data it stores. To significantly reduce costs, Amazon S3 provides colder tier storage options called Amazon S3 Glacier & S3 Glacier Deep Archive. 

* To automate cost optimizations, Amazon S3 provides configurable lifecycle policies and S3 Intelligent-Tiering options to automate moving older data to colder tiers. AWS services in our ingestion, cataloging, processing, and consumption layers can natively read and write S3 objects. 

# Cataloging and search layer

* A data lake typically hosts many datasets which have evolving schema and new data partitions. A central data catalog that manages metadata for all the datasets in the data lake is crucial to enabling self-service discovery of data in the data lake. 

* Additionally, separating metadata from data into a central schema enables schema-on-read for the processing and consumption layer components.

* In the presented architecture, Lake Formation provides the central catalog to store and manage metadata for all datasets hosted in the data lake. 

* Organizations manage both technical metadata (such as versioned table schemas, partitioning information, physical data location, and update timestamps) and business attributes (such as data owner, data steward, column business definition, and column information sensitivity) of all their datasets in Lake Formation. 

* Services such as AWS Glue, Amazon EMR, and Amazon Athena natively integrate with Lake Formation and automate discovering and registering dataset metadata into the Lake Formation catalog. 

* Additionally, Lake Formation provides APIs to enable metadata registration and management using custom scripts and third-party products. AWS Glue crawlers in the processing layer can track evolving schemas and newly added partitions of datasets in the data lake, and add new versions of corresponding metadata in the Lake Formation catalog.
 
* Lake Formation provides the data lake administrator a central place to set up granular table and column level permissions for databases and tables hosted in the data lake. 

* After Lake Formation permissions are set up, users and groups can access only authorized tables and columns using multiple processing and consumption layer services such as Athena, Amazon EMR, AWS Glue, and Amazon Redshift Spectrum.

# Processing layer

* The processing layer in our architecture is composed of two types of components:
 * Components used to create multi-step data processing pipelines.
 * Components to orchestrate data processing pipelines on schedule or in response to event triggers (such as ingestion of new data into the landing zone).

* AWS Glue and AWS Step Functions provide serverless components to build, orchestrate, and run pipelines that can easily scale to process large data volumes. 

* Multi-step workflows built using AWS Glue and Step Functions can catalog, validate, clean, transform, and enrich individual datasets and advance them from raw to cleaned and cleaned to curated zones in the storage layer.

* AWS Glue is a serverless, pay-per-use ETL service for building and running Python or Spark jobs (written in Scala or Python) without requiring you to deploy or manage clusters. AWS Glue automatically generates the code to accelerate your data transformations and loading processes. 

* AWS Glue ETL builds on top of Apache Spark and provides commonly used out-of-the-box data source connectors, data structures, and ETL transformations to validate, clean, transform, and flatten data stored in many open-source formats such as CSV, JSON, Parquet, and Avro. AWS Glue ETL also provides capabilities to incrementally process partitioned data.

* Additionally, you can use AWS Glue to define and run crawlers that can crawl folders in the data lake, discover datasets and their partitions, infer schema, and define tables in the Lake Formation catalog. AWS Glue provides more than a dozen built-in classifiers that can parse a variety of data structures stored in open-source formats. 

* AWS Glue also provides triggers and workflow capabilities that you can use to build multi-step end- to-end data processing pipelines that include job dependencies and running parallel steps. You can schedule AWS Glue jobs and workflows or run them on demand. AWS Glue natively integrates with AWS services in storage, catalog, and security layers.
 
* To make it easy to clean and normalize data, Glue also provides a visual data preparation tool called Glue DataBrew which is an interactive, point-and-click visual interface without requiring to write any code.

* Step Functions is a serverless engine that you can use to build and orchestrate scheduled or event-driven data processing workflows. You use Step Functions to build complex data processing pipelines that involve orchestrating steps implemented by using multiple AWS services such as AWS Glue, AWS Lambda, Amazon Elastic Container Service (Amazon ECS) containers, and more. 

# Consumption layer

* The consumption layer in the presented architecture is composed using fully managed, purpose-built, analytics services that enable interactive SQL, BI dashboarding, batch processing, and ML.

## Interactive SQL

* Amazon Athena is an interactive query service that enables you to run complex ANSI SQL against terabytes of data stored in Amazon S3 without needing to first load it into a database. 

* Athena queries can analyze structured, semi-structured, and columnar data stored in open-source formats such as CSV, JSON, XML Avro, Parquet, and ORC. Athena uses table definitions from Lake Formation to apply schema-on-read to data read from Amazon S3.

## Data warehousing and batch analytics

* Amazon Redshift is a fully managed data warehouse service that can host and process petabytes of data and run thousands highly performant queries in parallel. 

* Amazon Redshift uses a cluster of compute nodes to run very low-latency queries to power interactive dashboards and high-throughput batch analytics to drive business decisions. 

* You can run Amazon Redshift queries directly on the Amazon Redshift console or submit them using the JDBC/ODBC endpoints provided by Amazon Redshift.

## Business intelligence

* Amazon QuickSight provides a serverless BI capability to easily create and publish rich, interactive dashboards. QuickSight enriches dashboards and visuals with out-of-the- box, automatically generated ML insights such as forecasting, anomaly detection, and narrative highlights. 

* QuickSight natively integrates with Amazon SageMaker to enable additional custom ML model-based insights to your BI dashboards. 

## Predictive analytics and ML

* Amazon SageMaker is a fully managed service that provides components to build, train, and deploy ML models using an interactive development environment (IDE) called Amazon SageMaker Studio. 

* In Amazon SageMaker Studio, you can upload data, create new notebooks, train and tune models, move back and forth between steps to adjust experiments, compare results, and deploy models to production, all in one place by using a unified visual interface. Amazon SageMaker also provides managed Jupyter notebooks that you can spin up with just a few clicks.

# Security and governance layer

* Components across all layers of the presented architecture protect data, identities, and processing resources by natively using the following capabilities provided by the security and governance layer.

## Authentication and authorization

* AWS Identity and Access Management (IAM) provides user-, group-, and role-level identity to users and the ability to configure fine-grained access control for resources managed by AWS services in all layers of our architecture. 

* IAM supports multi-factor authentication and single sign-on through integrations with corporate directories and open identity providers such as Google, Facebook, and Amazon.

## Encryption

* AWS KMS provides the capability to create and manage symmetric and asymmetric customer managed encryption keys. AWS services in all layers of our architecture natively integrate with AWS KMS to encrypt data in the data lake. 

* It supports both creating new keys and importing existing customer keys. Access to the encryption keys is controlled using IAM and is monitored through detailed audit trails in CloudTrail.

## Network protection

* Our architecture uses Amazon Virtual Private Cloud (Amazon VPC) to provision a logically isolated section of the AWS Cloud (called VPC) that is isolated from the internet and other AWS customers. 

* Amazon VPC provides the ability to choose your own IP address range, create subnets, and configure route tables and network gateways. AWS services from other layers in our architecture launch resources in this private VPC to protect all traffic to and from these resources.

## Monitoring and logging

* AWS services in all layers of our architecture store detailed logs and monitoring metrics in Amazon CloudWatch. CloudWatch provides the ability to analyze logs, visualize monitored metrics, define monitoring thresholds, and send alerts when thresholds are crossed.

* All AWS services in our architecture also store extensive audit trails of user and service actions in CloudTrail. CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. 

* This event history simplifies security analysis, resource change tracking, and troubleshooting. In addition, you can use CloudTrail to detect unusual activity in your AWS accounts. These capabilities help simplify operational analysis and troubleshooting.


# Conclusion

* With AWS serverless and managed services, you can build a modern, low-cost data lake centric analytics architecture in days. A decoupled, component-driven architecture enables you to start small and quickly add new purpose-built components to one of six architecture layers to address new requirements and data sources.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/aws-serverless-data-analytics-pipeline.pdf?did=wp_card&trk=wp_card)