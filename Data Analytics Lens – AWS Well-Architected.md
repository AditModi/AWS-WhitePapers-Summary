# Introduction

* This document describes the AWS Well-Architected Data Analytics Lens, a collection of customer-proven best practices for designing well-architected analytics workloads. The Data Analytics Lens contains insights that AWS has gathered from real-world case studies, and helps you learn the key design elements of well-architected analytics workloads along with recommendations for improvement. 

## How to use the lens

* The AWS Well-Architected Framework helps you understand the pros and cons of decisions you make while building systems on AWS.

* By using the framework, you learn architectural best practices for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud. It provides a way for you to consistently measure your architectures against best practices and identify areas for improvement. We believe that having well-architected systems greatly increases the likelihood of business success.

* In this “Lens” we focus on how to design, deploy, and architect your analytics application workloads in the AWS Cloud. For brevity, we have only covered details from the Well-Architected Framework that are specific to analytics workloads. You should still consider best practices and questions that have not been included in this document when designing your architecture. We recommend that you read the AWS Well-Architected Framework.

* This document is intended for those in technology roles, such as chief technology officers (CTOs), architects, developers, and operations team members. After reading this document, you will understand AWS best practices and strategies to use when designing architectures for analytics applications and environment.

# Well-Architected design principles 

* This section describes the design principles, best practices, and improvement suggestions that are relevant when designing your workload architecture. For brevity, only questions that are specific to analytics workloads are included in the Analytics Lens. We recommend you also read and apply the guidance found in each Well-Architected pillar, which includes foundational best practices for operational excellence, security, performance efficiency, reliability, and cost optimization that are relevant to all workloads.

**Pillars**

1. Operational excellence
2. Security
3. Reliability
4. Performance efficiency
5. Cost optimization

## Operational excellence

* The Operational Excellence pillar includes the ability to support development and run workloads effectively, gain insight into their operations, and to continually improve supporting processes and procedures to deliver business value.

### 1 - Monitor the health of the analytics pipelines

**How do you measure the health of your analytics workload?**

* Data analytics workloads often involve multiple systems and process steps working in coordination. It is imperative that you monitor not only individual components but also the interaction of dependent processes to ensure a healthy data analytics workload.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 1.1	|Required	|Validate the data quality of source systems before moving to analytics|
|☐ BP 1.2	|Required	|Monitor the planned arrival of recent data from the source systems|
|☐ BP 1.3	|Required	|Define and measure the schedules of recurring analytics jobs|
|☐ BP 1.4	|Required	|Define and measure the runtime durations of analytics jobs|

> BP - Best Practices

### 2 - Modernize deployment of the analytics jobs and applications

**How do you deploy ETL jobs and applications in a controlled and reproducible way?**
 
* Using modern development practices, such as continuous integration/continuous delivery (CI/CD), can help ensure that changes are rolled out in a controlled and repeatable way. Use test automation to verify infrastructure, code, and data changes in stages to ensure the operational stability of the entire analytics pipeline.


|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 2.1	|Recommended	|Use version control for job and application changes|
|☐ BP 2.2	|Recommended	|Set up testing data and a staging environment
|☐ BP 2.3	|Recommended	|Test and validate analytics jobs and application deployments|
|☐ BP 2.4	|Recommended	|Roll back failed deployments and backfill the data|
|☐ BP 2.5	|Recommended	|Build standard operating procedures for deployment, test, rollback, and backfill tasks|
|☐ BP 2.6	|Recommended	|Automate the standard operating procedures|

> BP - Best Practices

### 3 – Build financial accountability models for data and workload usage

**How do you measure and attribute the analytics workload financial accountability?** 

* As your business continues to evolve, so will your analytics workload and your stakeholders’ use of data analytics. Data analytics systems and the data generated from these systems will grow over time into a mix of both broadly-shared and isolated-team resources. Establish a financial attribution model for these resources. Teams will then understand how their use of data analytics influences costs to the business and this promotes a culture of accountability and frugality.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 3.1	|Recommended	|Ensure personnel capability
|☐ BP 3.2	|Recommended	|Ensure your cloud operating model matches your operational aims|
|☐ BP 3.3	|Recommended	|Share design standards and educate new support personnel in procedures|
|☐ BP 3.4	|Recommended	|Use runbooks to perform SAP landscape operations|
|☐ BP 3.5	|Recommended	|Use playbooks to investigate issues|

> BP - Best Practices

## Security

* The security pillar encompasses the ability to protect data, systems, and assets to take advantage of cloud technologies to improve your security.

### 4 - Classify and protect data

**How do you classify and protect data in analytics workload?** 

* Because analytics workloads ingest data from source systems, the owner of the source data should define the data classifications. As the analytics workload owner, you should honor the source data classifications and implement the corresponding data protection policies of your organization. 

* Share the data classifications with the downstream data consumers to permit them to honor the data classifications in their organizations and policies as well. Data classification helps to categorize organizational data based on sensitivity and criticality, which then helps determine appropriate protection and retention controls on that data.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 4.1	|Required	|Understand data classifications and their protection policies|
|☐ BP 4.2	|Required	|Identify the source data owners and have them set the data classifications|
|☐ BP 4.3	|Required	|Record data classifications into the Data Catalog so that analytics workload can understand|
|☐ BP 4.4	|Required	|Implement encryption policies for each class of data in the analytics workload|
|☐ BP 4.5	|Required	|Implement retention and backup policies for each class of data in the analytics workload|
|☐ BP 4.6	|Recommended	|Require downstream systems honor the data classifications|

> BP - Best Practices

### 5 - Control the data access

**How do you manage access to data within source, analytics, and downstream systems?** 

* An analytics workload is a centralized repository of data from source systems, and distributes the data to downstream systems. The upstream and downstream systems, therefore, might be owned by different teams. As the analytics workload owner, you should honor the source systems’ access management policies when connecting to the source systems, secure the access of the data in the analytics workload, and securely share the data to downstream systems without violating the data classification policies of the source systems

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 5.1	|Required	|Allow data owners to determine who or which systems can access data in analytics and downstream workloads|
|☐ BP 5.2	|Required	|Build user identity solutions that uniquely identify people and systems|
|☐ BP 5.3	|Required	|Implement the required data authorization models|
|☐ BP 5.4	|Recommended	|Establish an emergency access process to the source, analytics, and downstream systems|

> BP - Best Practices

### 6 – Control the access to workload infrastructure

**How do you protect the infrastructure of the analytics workload**

* Analytics environments change based on the evolving requirements of data processing and distribution. Ensure that the environment is accessible with the least permissions necessary. Automate auditing of environment changes and alert in case of abnormal environment access.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 6.1	|Required	|Prevent unintended access to the infrastructure|
|☐ BP 6.2	|Required	|Implement least permissions policy for source and downstream systems|
|☐ BP 6.3	|Required	|Monitor the infrastructure changes and the user activities against the infrastructure|
|☐ BP 6.4	|Required	|Secure the audit logs that record every data or resource access in analytics infrastructure|

> BP - Best Practices

## Reliability

* The reliability pillar includes the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfiguration or transient network issues. It also includes the ability to understand the full travel path and change history of the data and keep the data safe when storage failure occurs.

### 7 - Design resiliency for analytics workload

**How do you design analytics workloads to withstand failures?** 

* Every extract, transform, and load (ETL) job has corresponding business requirements, such as providing daily, weekly, or monthly sales flash reports. The analytics workload should meet the business SLAs by designing the ETL pipelines, data storage and metadata catalog to be resilient from failure.


|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 7.1	 |Required       |Understand the business requirements of analytics and ETL jobs|
|☐ BP 7.2	|Required       |Monitor analytics systems to detect analytics or ETL job failures|
|☐ BP 7.3	|Required	|Notify stakeholders about analytics or ETL job failures|
|☐ BP 7.4	|Recommended	|Automate the recovery of analytics and ETL job failures|
|☐ BP 7.5	|Recommended	|Build a disaster recovery (DR) plan for the analytics infrastructure and the data|

> BP - Best Practices

### 8 - Govern data and metadata changes

**How do you govern data and metadata changes?** 

* Controlled changes are not only necessary for infrastructure, but also required for data quality assurance. If the data changes are uncontrolled, it becomes difficult to anticipate the impact of these changes. It even makes downstream systems harder to manage data quality issues of their own.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 8.1	|Required	|Build a central Data Catalog to store, share, and track metadata changes|
|☐ BP 8.2	|Required	Monitor for data quality anomalies|
|☐ BP 8.3	|Required	Trace the data lineage|

> BP - Best Practices

## Performance efficiency

* The performance efficiency pillar focuses on the efficient use of computing resources to meet requirements and the maintenance of that efficiency as demand changes and technologies evolve. Remember, performance optimization is not a one-time activity. 

* It is an incremental and continual process of confirming business requirements, measuring the workload performance, identifying under-performing components, and tuning the components to meet your business needs.

### 9 - Choose the best-performing compute solution

**How do you select the best-performing compute options for your analytics workload?** 

* The definition of best-performing will mean different things to different stakeholders, so gathering all stakeholders’ input in the decision process is key. Define performance and cost goals by balancing business and application requirements, then evaluate the overall efficiency of the compute solution against those goals using metrics emitted from the solution.


|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 9.1	|Highly Recommended	|Let business stakeholders confirm the business performance criteria of analytics processing|
|☐ BP 9.2	|Highly Recommended	|Choose analytics and ETL solutions that achieve the business purpose within operations constraints|
|☐ BP 9.3	|Highly Recommended	|Provision the compute resources to the location of the data storage|
|☐ BP 9.4	|Recommended	|Define and measure the computing performance metrics|
|☐ BP 9.5	|Recommended	|Periodically identify under-performing components and fine-tune the infrastructure or application logic|

> BP - Best Practices

### 10 - Choose the best-performing storage solution

**How do you select the best-performing storage options for your workload?** 

* An analytics workload’s optimal storage solution is determined by various aspects such as compute engine (Amazon EMR, Amazon Redshift, Amazon RDS, etc.), access patterns (random or sequential), required throughput, access frequency (online, offline, archival), update patterns (append only, in-place update), and availability and durability requirements. Choose the best-performing storage solution for your analytics workload’s own characteristics.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 10.1	|Highly Recommended	|Identify critical performance criteria of storage for the workload: I/O latency, I/O throughput, or data size growth|
|☐ BP 10.2	|Highly Recommended	|Evaluate and confirm the available storage options per compute solution of your choice|
|☐ BP 10.3	|Recommended	|Choose the optimal storage based on access patterns, data growth, and the performance metrics|

> BP - Best Practices

### 11 - Choose the best-performing file format and partitioning

**How do you select the best-performing file formats and partitioning?** 

* Selecting the best-performing file format and partitioning of data at rest can have a large impact on the overall analytics workload efficiency. Optimize the performance of the analytics workload by storing data with the use case in mind. Also choose the right data compression format and use data partitioning according to the data access patterns of the analytics workload.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 11.1	|Highly Recommended	|Select format based on data write frequency and patterns (append-only vs. in-place update)|
|☐ BP 11.2	|Highly Recommended	|Select format based on data read frequency and patterns|
|☐ BP 11.3	|Recommended	|Use file compression to reduce number of files and to improve file I/O efficiency|
|☐ BP 11.4	|Recommended	|Partition the data to avoid unnecessary file reads|

> BP - Best Practices

## Cost optimization

### 12 - Choose cost-effective compute and storage solutions based on workload usage patterns

**How do you select the compute and storage solution for your analytics workload?** 

* Your initial design choice could have significant cost impact. Understand the resource requirements of your workload, including its steady-state and spikiness, and then select the solution and tools that meet your requirements. Avoid over-provisioning to allow more cost optimization opportunities.


|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 12.1	|Recommended	|Decouple storage from compute|
|☐ BP 12.2	|Recommended	|Plan and provision capacity for predictable workload usage|
|☐ BP 12.3	|Recommended	Use On-Demand Instance capacity for unpredictable workload usage|
|☐ BP 12.4	|Recommended	Avoid data duplication|

> BP - Best Practices

### 13 - Manage cost over time

**How do you manage the cost of your workload over time?** 

* To ensure that you always have the most cost-efficient workload, periodically review your workload to discover opportunities to implement new services, features, and components. It is common for analytics workloads to have an ever-growing number of users and exponential growth of data volume. 

* You should implement a standardized process across your organization to identify and remove unused resources, such as unused data, infrastructure, and ETL jobs. Do not forget to notify downstream system owners of any deletions, so that they can anticipate impacts to their systems.

|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 13.1	|Recommended	|Delete unused data, infrastructure, or jobs|
|☐ BP 13.2	|Recommended	|Reduce the use of over-provisioned infrastructure|
|☐ BP 13.3	|Recommended	|Reduce repeating analytics jobs with no business value|
|☐ BP 13.4	Recommended	|Evaluate and adopt new cost-effective solutions|

> BP - Best Practices

### 14 - Use optimal pricing models based on infrastructure usage patterns

**How do you choose the financially-optimal pricing models of the infrastructure?** 

* Consult with your finance team and choose optimal purchasing options. such as On-Demand Instances, Reserved Instances, or Spot Instances. Once you understand the infrastructure usage patterns of the analytics workload, you can optimize the cost by purchasing reserved capacity with upfront payment, by using Spot Instances, or by paying Amazon EC2 usage via on-demand pricing models. 

* Evaluate the available purchasing models of the analytics infrastructure of your choice and determine the optimal payment models.


|ID	         |Priority       |Best Practice|
| -------------- |---------------| ------------|
|☐ BP 14.1	|Recommended	|Evaluate the infrastructure usage patterns then choose payment options accordingly|
|☐ BP 14.2	|Recommended	|Consult with your finance team and determine optimal payment models|

> BP - Best Practices

# Scenarios

* In this section, we cover the six key scenarios that are common in many analytics applications and how they influence the design and architecture of your analytics environment in AWS. 

* We present the assumptions made for each of these scenarios, the common drivers for the design, and a reference architecture for how these scenarios should be implemented.

## Modern data architecture (formerly Lake House)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e4s82l66nvhkpc9cu7v9.gif)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b4l59uxadfxk788uepoz.gif)
 

For more, refer to the [Derive Insights from AWS Lake House whitepaper](https://dev.to/awsmenacommunity/derive-insights-from-aws-lake-house-aws-white-paper-summary-2069-temp-slug-4844062?preview=3e3d48022cb5f10f590c6b22f0fead652f2402537f1566129c3bfdd99883afd1764d35c2d66dae135f08d89158c7f8c54c0996a9fd38b8e4d013ad66).
 
## Data mesh

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3ayqqcrhoislq8j9wr1d.png)

For more, refer to the [Data mesh Summary](https://dev.to/awsmenacommunity/derive-insights-from-aws-lake-house-aws-white-paper-summary-2069-temp-slug-4844062?preview=3e3d48022cb5f10f590c6b22f0fead652f2402537f1566129c3bfdd99883afd1764d35c2d66dae135f08d89158c7f8c54c0996a9fd38b8e4d013ad66). 

## Batch data processing

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/muh6dtqjanbtnyn8y7zx.png)
 
For more, refer to the [Batch data processing Summary](https://dev.to/awsmenacommunity/derive-insights-from-aws-lake-house-aws-white-paper-summary-2069-temp-slug-4844062?preview=3e3d48022cb5f10f590c6b22f0fead652f2402537f1566129c3bfdd99883afd1764d35c2d66dae135f08d89158c7f8c54c0996a9fd38b8e4d013ad66). 

## Streaming ingest and stream processing

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a9sru5hav4e1vs6xq7eo.png)
 
For more, refer to the [Streaming ingest and stream processing Summary](https://dev.to/awsmenacommunity/derive-insights-from-aws-lake-house-aws-white-paper-summary-2069-temp-slug-4844062?preview=3e3d48022cb5f10f590c6b22f0fead652f2402537f1566129c3bfdd99883afd1764d35c2d66dae135f08d89158c7f8c54c0996a9fd38b8e4d013ad66). 

## Operational analytics

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kt89xi6cetubucuo5o3v.png)

For more, refer to the [Operational analytics Summary](https://dev.to/awsmenacommunity/derive-insights-from-aws-lake-house-aws-white-paper-summary-2069-temp-slug-4844062?preview=3e3d48022cb5f10f590c6b22f0fead652f2402537f1566129c3bfdd99883afd1764d35c2d66dae135f08d89158c7f8c54c0996a9fd38b8e4d013ad66).  

## Data visualization

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y24ew3v78hk3yunedlzb.png) 

For more, refer to the [Data visualization Summary](https://dev.to/awsmenacommunity/derive-insights-from-aws-lake-house-aws-white-paper-summary-2069-temp-slug-4844062?preview=3e3d48022cb5f10f590c6b22f0fead652f2402537f1566129c3bfdd99883afd1764d35c2d66dae135f08d89158c7f8c54c0996a9fd38b8e4d013ad66). 

# Conclusion

* This lens provides architectural guidance for designing and building reliable, secure, efficient, and cost-effective analytics workloads in the cloud. We captured common architectures and overarching analytics design tenets. 

* The document also discussed the well-architected pillars through the Analytics Lens, providing you with a set of questions to consider for new or existing analytics architectures. 

* Applying the framework to your architecture helps you build robust, stable, and efficient systems, leaving you to focus on running analytics workloads and pushing the boundaries of the field to which you’re committed. 

* The analytics landscape is continuing to evolve as the number of tools and processes grows and matures. As this evolution occurs, we will continue to update this paper to help you ensure that your analytics applications are well-architected.

# Reference

[Original paper](https://docs.aws.amazon.com/wellarchitected/latest/analytics-lens/analytics-lens.html?did=wp_card&trk=wp_card)