# Introduction

* As the world becomes more digital, the amount of data created and collected constantly grows and accelerates. Analysis of this ever-growing data becomes a challenge with traditional analytical tools.

* Big data tools and technologies offer opportunities to analyze data efficiently so you can better understand customer preferences, gain a competitive advantage in the marketplace, and grow your business.

* AWS provides a broad platform of managed services to help you build, secure, and seamlessly scale end-to-end big data applications quickly and with ease.

# The AWS advantage in big data analytics

* Analyzing large datasets requires significant compute capacity that can vary in size, based on the amount of input data and the type of analysis. 

* For mission-critical applications on a more traditional infrastructure, system designers have no choice but to over-provision, because a surge in additional data due to an increase in business needs must be something the system can handle.

* on AWS, you can provision more capacity and compute in a matter of minutes, meaning that your big data applications grow and shrink as demand dictates, and your system runs as close to optimal efficiency as possible.

* In addition, you get flexible computing on a global infrastructure with access to the many different geographic Regions that AWS offers, along with the ability to use other scalable services that augment to build sophisticated big data applications. These other services include:

 * Amazon Simple Storage Service (Amazon S3) to store data

 * AWS Glue to orchestrate jobs to move and transform the data easily

 * AWS IoT, which lets connected devices interact with cloud applications and other connected devices

* As the amount of data being generated continues to grow, AWS has many options to get that data to the cloud, including secure devices like AWS Snow Family to accelerate petabyte-scale data transfers, delivery streams with Amazon Kinesis Data Firehose to load streaming data continuously, migrating databases using AWS Database Migration Service, and scalable private connections through AWS Direct Connect.

* As mobile continues to rapidly grow in usage, you can use the suite of services within the AWS Mobile Hub to collect and measure app usage and data, or export that data to another service for further custom analysis.

* These capabilities of AWS make it an ideal fit for solving big data problems, and many customers have implemented successful big data analytics workloads on AWS.

* The following services for collecting, processing, storing, and analyzing big data are described in order:

## Amazon Kinesis

* Amazon Kinesis is a platform for streaming data on AWS that makes it easy to load and analyze streaming data. 

* Amazon Kinesis also enables you to build custom streaming data applications for specialized needs. 

* With Kinesis, you can ingest real-time data such as application logs, website clickstreams, Internet of Things (IoT) telemetry data, and more into your databases, data lakes, and data warehouses, or build your own real-time applications using this data. 

* Amazon Kinesis enables you to process and analyze data as it arrives and respond in real-time instead of having to wait until all your data is collected before the processing can begin.

* Currently there are four pieces of the Kinesis platform that can be utilized based on your use case:

 * Amazon Kinesis Data Streams enables you to build custom applications that process or analyze streaming data.

 * Amazon Kinesis Video Streams enables you to build custom applications that process or analyze streaming video.

 * Amazon Kinesis Data Firehose enables you to deliver real-time streaming data to AWS destinations such as such as Amazon S3, Amazon Redshift, OpenSearch Service, and Splunk.

 * Amazon Kinesis Data Analytics enables you to process and analyze streaming data with standard SQL or with Java (managed Apache Flink).

## Amazon Managed Streaming for Apache Kafka (Amazon MSK)

* Amazon MSK is a fully managed service that makes it easy for you to build and run applications that use Apache Kafka to process streaming data. 

* Apache Kafka is an open-source platform for building real-time streaming data pipelines and applications. 

* With Amazon MSK, you can use native Apache Kafka APIs to populate data lakes, stream changes to and from databases, and power machine learning and analytics applications

## AWS Lambda

* AWS Lambda enables you to run code without provisioning or managing servers. You pay only for the compute time you consume – there is no charge when your code is not running. 

* With Lambda, you can run code for virtually any type of application or backend service – all with zero administration. Just upload your code and Lambda takes care of everything required to run and scale your code with high availability. 

* You can set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app.

## Amazon Elastic Map Reduce (Amazon EMR)

* Amazon EMR is the industry-leading cloud big data platform for processing vast amounts of data using open source tools such as Apache Spark, Apache Hive, Apache HBase, Apache Flink, Apache Hudi, and Presto. 

* Amazon EMR makes it easy to set up, operate, and scale your big data environments by automating time-consuming tasks like provisioning capacity and tuning clusters.

* With EMR you can run petabyte-scale analysis at less than half of the cost of traditional on-premises solutions and over 3x faster than standard Apache Spark.

* Amazon EMR does all the work involved with provisioning, managing, and maintaining the infrastructure and software of a Hadoop cluster.

## AWS Glue

* AWS Glue is a serverless data integration service that makes it easy to discover, prepare, and combine data for analytics, machine learning, and application development. 

* AWS Glue provides all of the capabilities needed for data integration. It both visual and code-based interfaces to make data integration easier.

## AWS Lake Formation

* AWS Lake Formation is an integrated data lake service that makes it easy for you to ingest, clean, catalog, transform, and secure your data and make it available for analysis and machine learning.

* Lake Formation gives you a central console where you can discover data sources, set up transformation jobs to move data to an S3 data lake, remove duplicates and match records, catalog data for access by analytic tools, configure data access and security policies, and audit and control access from AWS analytic and machine learning services.

## Amazon Machine Learning

* AWS offers the broadest and deepest set of machine learning services and supporting cloud infrastructure, putting machine learning in the hands of every developer, data scientist, and expert practitioner. 

* When you build an ML-based workload in AWS, you can choose from three different levels of ML services to balance speed-to-market with level of customization and ML skill level:

 * Artificial Intelligence (AI) services

 * ML services

 * ML frameworks and infrastructure

## Amazon DynamoDB

* Amazon DynamoDB is a fast, fully-managed NoSQL database service that makes it simple and cost effective to store and retrieve any amount of data, and serve any level of request traffic. 

* DynamoDB helps offload the administrative burden of operating and scaling a highly-available distributed database cluster. This storage alternative meets the latency and throughput requirements of highly demanding applications by providing single-digit millisecond latency and predictable performance with seamless throughput and storage scalability.

## Amazon Redshift

* Amazon Redshift is a fast, fully-managed, petabyte-scale data warehouse service that makes it simple and cost-effective to analyze all your data efficiently using your existing business intelligence tools. 

* It is optimized for data sets ranging from a few hundred gigabytes to a petabyte or more, and is designed to cost less than a tenth of the cost of most traditional data warehousing solutions.

## Amazon OpenSearch Service (OpenSearch Service)

* Amazon OpenSearch Service (OpenSearch Service) makes it easy to deploy, operate, and scale OpenSearch Service for log analytics, full text search, application monitoring, and more. 

* OpenSearch Service is a fully managed service that delivers the OpenSearch Service easy-to-use APIs and real-time capabilities along with the availability, scalability, and security required by production workloads. 

* The service offers built-in integrations with OpenSearch Dashboards, Logstash, and AWS services including Amazon Kinesis Data Firehose, AWS Lambda, and Amazon CloudWatch so that you can go from raw data to actionable insights quickly.

## Amazon QuickSight

* Amazon QuickSight is a scalable, serverless, embeddable, machine learning-powered business intelligence (BI) service built for the cloud. It makes it easy for all employees within an organization to build visualizations, perform ad hoc analysis, and quickly get business insights fromtheir data, anytime, on any device.  

* Amazon QuickSight enables organizations to scale their business analytics capabilities to hundreds of thousands of users, and delivers fast and responsive query performance by using a robust in-memory engine (SPICE).

## Amazon Compute Services 
> (Amazon Elastic Compute Cloud (Amazon EC2) instances, Amazon Elastic Container Service (Amazon ECS), and Amazon Elastic Kubernetes Service (Amazon EKS) are available for self-managed big data applications.)

* Amazon EC2, with instances acting as AWS virtual machines, provides an ideal platform for operating your own self-managed big data analytics applications on AWS infrastructure. 

* Almost any software you can install on Linux or Windows virtualized environments can be run on Amazon EC2, and you can use the pay-as-you-go pricing model.

## Amazon Athena

* Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. 

* Athena is serverless, so there is no infrastructure to setup or manage, and you can start analyzing data immediately. You don’t need to load your data into Athena, as it works directly with data stored in S3. Just log into the Athena Console, define your table schema, and start querying. 

* Amazon Athena uses Presto with full ANSI SQL support and works with a variety of standard data formats, including CSV, JSON, ORC, Apache Parquet, and Apache Avro.

# Solving big data problems on AWS

## Example 1: Queries against an Amazon S3 data lake

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/15bucjnqb49uqbb1ptng.png)
 
* Data lakes are an increasingly popular way to store and analyze both structured and unstructured data. 
* If you use an Amazon S3 data lake, AWS Glue can make all your data immediately available for analytics without moving the data. 
* AWS Glue crawlers can scan your data lake and keep the AWS Glue Data Catalog in sync with the underlying data. 
* You can then directly query your data lake with Amazon Athena and Amazon Redshift Spectrum. 
* You can also use the AWS Glue Data Catalog as your external Apache Hive Metastore for big data applications running on Amazon EMR.

## Example 2: Capturing and analyzing sensor data

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0d2gir7ovg4niiqexpym.png)

1. The process begins with each A/C unit providing a constant data stream to Amazon Kinesis Data Streams. 

2. Using the Amazon Kinesis Data Streams-provided tools such as the Kinesis Client Library or SDK, a simple application is built on Amazon EC2 to read data as it comes into Amazon Kinesis Data Streams, analyze it, and determine if the data warrants an update to the real-time dashboard. 

3. This data flow needs to occur in near real time so that customers and maintenance teams can be alerted quickly if there is an issue with the unit. Additionally, there will be lots of potential access to this data from the following sources:

* Customers checking on their system via a mobile device or browser

* Maintenance teams checking the status of its fleet

* Data and intelligence algorithms and analytics in the reporting platform spot trends that can be then sent out as alerts, such as if the A/C fan has been running unusually long with the building temperature not going down.

4. DynamoDB was chosen to store this near real-time data set because it is both highly available and scalable; throughput to this data can be easily scaled up or down to meet the needs of its consumers as the platform is adopted and usage grows.

5. The reporting dashboard is a custom web application that is built on top of this data set and run on Amazon EC2. It provides content based on the system status and trends as well as alerting customers and maintenance crews of any issues that may come up with the unit.

6. The customer accesses the data from a mobile device or a web browser to get the current status of the system and visualize historical trends.

7. To read from the Amazon Kinesis stream, there is a separate Amazon Kinesis-enabled application that probably runs on a smaller EC2 instance that scales at a slower rate. 

8. The data is transformed by the Amazon Kinesis-enabled application into a format that is suitable for long-term storage, for loading into its data warehouse, and storing on Amazon S3.

9. Amazon Redshift is again used as the data warehouse for the larger data set. 

10. For visualizing the analytics, one of the many partner visualization platforms can be used via the OBDC/JDBC connection to Amazon Redshift. 
 
## Example 3: sentiment analysis of social media

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vzonoafjs85myfhms1ax.png)

1. First, deploy an Amazon EC2 instance in an Amazon VPC that ingests tweets from Twitter.

2. Next, create an Amazon Kinesis Data Firehose delivery stream that loads the streaming tweets into the raw prefix in the solution's S3 bucket.

3. S3 invokes a Lambda function to analyze the raw tweets using Amazon Translate to translate non-English tweets into English, and Amazon Comprehend to use natural language-processing (NLP) to perform entity extraction and sentiment analysis.

4. A second Kinesis Data Firehose delivery stream loads the translated tweets and sentiment values into the sentiment prefix in the S3 bucket. A third delivery stream loads entities in the entities prefix in the S3 bucket.

5. This architecture also deploys a data lake that includes AWS Glue for data transformation, Amazon Athena for data analysis, and Amazon QuickSight for data visualization. AWS Glue Data Catalog contains a logical database used to organize the tables for the data in S3. Athena uses these table definitions to query the data stored in S3 and return the information to an Amazon QuickSight dashboard. 

# Conclusion

* As more and more data is generated and collected, organizations are facing a growing big data ecosystem where new tools emerge and become outdated very quickly.

* With a broad set of managed services to collect, process, and analyze big data, AWS makes it easier to build, deploy, and scale big data applications. This enables you to focus on business problems instead of updating and managing these tools.

* AWS provides many solutions to address your big data analytic requirements. Most big data architecture solutions use multiple AWS tools to build a complete solution. The result is a flexible big data architecture that is able to scale along with your business.

# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/big-data-analytics-options/welcome.html?did=wp_card&trk=wp_card)
