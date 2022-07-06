# Introduction

* Businesses today receive data at massive scale and speed due to the explosive growth of data sources that continuously generate streams of data. Whether it is log data from application servers, clickstream data from websites and mobile apps, or telemetry data from Internet of Things (IoT) devices, it all contains information that can help you learn about what your customers, applications, and products are doing right now.

# Real-time and near-real-time application scenarios

* You can use streaming data services for real-time and near-real-time applications such as application monitoring, fraud detection, and live leaderboards. Real-time use cases require millisecond end-to-end latencies– from ingestion, to processing, all the way to emitting the results to target data stores and other systems.

* For example, Netflix uses Amazon Kinesis Data Streams to monitor the communications between all its applications so it can detect and fix issues quickly, ensuring high service uptime and availability to its customers. While the most commonly applicable use case is application performance monitoring, there are an increasing number of real-time applications in ad tech, gaming, and IoT that fall under this category.


# Difference between batch and stream processing

* You need a different set of tools to collect, prepare, and process real-time streaming data than those tools that you have traditionally used for batch analytics. With traditional analytics, you gather the data, load it periodically into a database, and analyze it hours, days, or weeks later. 

* Analyzing real-time data requires a different approach. Stream processing applications process data continuously in real-time, even before it is stored. Streaming data can come in at a blistering pace and data volumes can vary up and down at any time. 

* Stream data processing platforms have to be able to handle the speed and variability of incoming data and process it as it arrives, often millions to hundreds of millions of events per hour.

# Stream processing challenges

building and operating your own custom streaming data pipelines is complicated and resource-intensive:

* You have to build a system that can cost-effectively collect, prepare, and transmit data coming simultaneously from thousands of data sources.

* You need to fine-tune the storage and compute resources so that data is batched and transmitted efficiently for maximum throughput and low latency.

* You have to deploy and manage a fleet of servers to scale the system so you can handle the varying speeds of data you are going to throw at it.

Version upgrade is a complex and costly process. After you have built this platform, you have to monitor the system and recover from any server or network failures by catching up on data processing from the appropriate point in the stream, without creating duplicate data. You also need a dedicated team for infrastructure management.

# Streaming data solutions: examples

## Scenario 1: Internet offering based on location

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n0c84wqpnqb86ajor393.png)

### Summary

* Company InternetProvider leveraged Amazon Kinesis Data Streams to stream user details and location. The stream of record was consumed by AWS Lambda to enrich the data with bandwidth options stored in the function’s library. After the enrichment, AWS Lambda published the bandwidth options back to the application. 

* Amazon Kinesis Data Streams and AWS Lambda handled provisioning and management of servers, enabling company InternetProvider to focus more on business application development. 

## Scenario 2: Near-real-time data for security teams

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bk8940ee3sob5xnmvz3t.png)
 
### Summary

* Kinesis Data Firehose can persistently deliver your streaming data to a supported destination. It’s a fully-managed solution, requiring little or no development. 

* For Company ABC2Badge, using Kinesis Data Firehose was a natural choice. They were already using Amazon Redshift as their data warehouse solution. Because their data sources continuously wrote to transaction logs, they were able to leverage the Amazon Kinesis Agent to stream that data without writing any additional code. 

* Now that company ABC2Badge has created a stream of sensor records and are receiving these records via Kinesis Data Firehose, they can use this as the basis for the security team use case.

## Scenario 3: Preparing clickstream data for data insights processes

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j8soujtj9z0donbpd5xh.png)
 

### Summary

* Amazon Kinesis Data Streams makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. 

* Combined with the AWS Glue serverless data integration service, you can create real-time event streaming applications that prepare and combine data for ML.

* Because both Kinesis Data Streams and AWS Glue services are fully managed, AWS takes away the undifferentiated heavy lifting of managing infrastructure for your big data platform, letting you focus on generating data insights based on your data.

* Fast Sneakers can utilize real-time event processing and ML to enable their website to make fully automated real-time price adjustments, to maximize their product stock. This brings the most value to their business while avoiding the need to create and maintain a big data platform.

## Scenario 4: Device sensors real-time anomaly detection and notifications

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6x35n8vmwgd7oo6tl261.png)

### Summary

* By making use of the AWS streaming services Amazon Kinesis Data Streams, Amazon Kinesis Data Analytics, and Amazon Kinesis Data Firehose,

* ABC4Logistics can detect anomalous patterns in temperature readings and notify the driver and the fleet management team in real-time, preventing major accidents such as complete vehicle breakdown or fire. 

## Scenario 5: Real time telemetry data monitoring with Apache Kafka



![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hgreo2ppy3sedjl6hg72.png)

### Summary

* With Apache Kafka offered as a managed service on AWS, you can focus on consumption rather than on managing the coordination between the brokers, which usually requires a detailed understanding of Apache Kafka. Features such as high availability, broker scalability, and granular access control are managed by the Amazon MSK platform.

* ABC1Cabs utilized these services to build production application without needing infrastructure management expertise. They could focus on the processing layer to consume data from Amazon MSK and further propagate to visualization layer.

* Spark Streaming on Amazon EMR can help real-time analytics of streaming data, and publishing on OpenSearch Dashboards on Amazon OpenSearch Service for the visualization layer. 

# Conclusion

* This document reviewed several scenarios for streaming workflows. In these scenarios, streaming data processing provided the example companies with the ability to add new features and functionality.

* By analyzing data as it gets created, you will gain insights into what your business is doing right now. AWS streaming services enable you to focus on your application to make time-sensitive business decisions, rather than deploying and managing the infrastructure.

# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/streaming-data-solutions-amazon-kinesis/streaming-data-solutions-amazon-kinesis.pdf#conclusion-and-contributors)
