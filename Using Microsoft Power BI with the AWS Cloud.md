* This whitepaper discusses how to integrate and use Microsoft Power BI (Desktop, Report Server, Service, and on-premises data gateway) with the Amazon Web Services (AWS) Cloud. It presents options for customers looking to connect Microsoft Power BI products to AWS Services such as Amazon Redshift, Amazon Athena, Amazon RDS, Amazon OpenSearch, and AWS Lake Formation, with a focus on connectivity, security, performance, and cost optimization.

* This whitepaper is for IT decision makers and architects looking to quickly understand Microsoft Power BI concepts and what options exist to make use of those technologies when using AWS Services as data sources.

# Introduction

* Customers with businesses off all sizes are using AWS products and services to store their data reliably, cost effectively, and securely. This is due in part to the broad ecosystem of mature data storage and analytics offerings that are available. Some of these offerings include the following services:

 * **Amazon Simple Storage Service (Amazon S3)** provides a simple, scalable, secure, and cost-effective data repository. It has become an industry standard for storing application data, as well as a first choice for customer data lakes.

 * **Amazon Athena** is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL.

 * **Amazon Relational Database Service (Amazon RDS)** makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching, and backups. SQL Server, Oracle Database, MySQL, MariaDB, and PostgreSQL engines are available.

 * **Amazon Redshift** is fully managed, massively scalable data warehouse that makes it easy to analyze both structured and unstructured datasets.

 * **Amazon QuickSight** is a fast, cloud-powered business intelligence service that makes it easy to deliver insights to everyone in your organization.

 * **Amazon OpenSearch (successor to Amazon Elasticsearch Service)** is a fully-managed service that makes it easy for you to deploy, secure, and run Elasticsearch cost-effectively and at scale.

 * **AWS Lake Formation** is a service that makes it easy to set up a secure data lake in days.

* To better understand how services relate to one another, we often label data services as either being data sources or data consumers. A data source allows customers and applications to store and retrieve data from the service. Frequently, data sources also have built-in compute and can provide computational analysis and filtering. But ultimately, data is loaded into these data sources and eventually data is retrieved from them by data consumers. Amazon S3, Amazon Athena, and Amazon Redshift are good examples of data sources.

* Data consumers, on the other hand, access the data from data sources and, typically, process it. They might optionally display it too. Amazon QuickSight and the Microsoft Power BI suite are good examples of data consumers. They read from data sources, and then assist in the analysis, visualization, and publication of information.

* AWS gives customers full flexibility in mixing the technologies they prefer for their data needs. While many customers choose Amazon QuickSight for their business intelligence (BI) needs, other customers choose vendors such as Microsoft Power BI, Tableau, and Qlik.

* This document focuses on the Microsoft Power BI suite of products and services, and how to use them in combination with AWS Services.

# The Microsoft Power BI suite

* To reduce confusion due to product naming similarities, this whitepaper presents what each Microsoft Power BI product and service is.

### Power BI Desktop

* Power BI Desktop is a free application you install on your local computer. It lets you connect to, transform, and visualize your data. With Power BI Desktop, you can connect to multiple different sources of data and combine them (often called modeling) into a data model. This data model lets you build visuals and collections of visuals you can share as reports with other people inside your organization.

* Power BI Desktop can connect to any supported data source that is available locally or over the network. For supported data sources, see the Appendix: Microsoft Power BI supported AWS data sources.

* Most users who work on business intelligence projects use Power BI Desktop to create reports. Then they push content to either Power BI Report Server or the Power BI service in order to share their reports with others. The act of pushing content from Power BI Desktop to the Power BI Report Server or the Power BI service is known as publishing. For more information, see What is Power BI Desktop?

> * Note:
> Power BI Desktop is a Windows-only application. It is not available for Linux, macOS, or other operating systems.

### Power BI Service

* Power BI is a collection of software services, apps, and connectors that work together to help you create, share, and consume business insights in a way that serves you and your business most effectively. The Power BI service, sometimes referred to as Power BI online, is the software as a service (SaaS) part of Power BI. For more information, see What is the Power BI service?

* The Power BI service is a cloud-based service. It supports light report editing and collaboration for teams and organizations. You can connect to data sources in the Power BI service too, but modeling is limited.

* Most report designers who work on business intelligence projects use Power BI Desktop to create reports, and then use the Power BI service to distribute their reports with others. For additional information on this crucial component, refer to Connecting the Microsoft Power BI service to AWS data sources.

### Power BI Report Server

* Power BI Report Server is a private report server with a web portal in which you display and manage reports and KPIs. Customers use Power BI Report Server in cases where they do not want their reports published to the Power BI service. Although it was originally intended for on-premises environments, the Power BI Report Server can run on AWS as well. For additional information, refer to Using Microsoft Power BI Report Server in AWS.

### On-premises data gateway

* The Microsoft on-premises data gateway is a commonly-deployed component that can increase the security and performance of Power BI deployments. It allows the Power BI service to access privatized data sources, which are located in another facility and accessible by internal network connectivity between the data source and the data gateway. Although it is typically installed as a server component, you can also install a personal mode on your local computer as an application. This whitepaper focuses only on the standard (server) mode. For additional information, refer to Connecting the Microsoft Power BI service to AWS data sources.

# Connecting Microsoft Power BI Desktop to AWS data sources

* Most often customers who start with Microsoft Power BI Desktop are interested in how they can connect to AWS data sources from their on-premises computers and network. The desktop application is typically running on their local Windows laptop and physical and logical connectivity to AWS data sources are the biggest perceived barriers to entry.

* However, another option exists, which is to run the Microsoft Power BI Desktop in the AWS Cloud. This option significantly reduces connectivity barriers to AWS data sources, but also requires some additional considerations. Both models are discussed in this section. We examine the implications of each in relation to connectivity, security, performance, and costs so that you can decide which option is best for you. The options presented in this section illustrate Amazon RDS, Amazon Redshift, and Amazon Athena. For a full discussion of all AWS data sources, refer to Appendix: Microsoft Power BI supported AWS data sources.

## Using Power BI Desktop on premises

* If you plan on using Power BI desktop on premises with data sources are that stored within the AWS Cloud, Power BI can access these sources in one of three ways:

 * Connecting to data sources using the internet.

 * Connecting to data sources using AWS Virtual Private Network (AWS AWS VPN).

 * Connecting to data sources using AWS Direct Connect.

* Details about Each method can be found [here](https://docs.aws.amazon.com/en_us/whitepapers/latest/using-power-bi-with-aws-cloud/using-power-bi-desktop-on-premises.html).


## Using Microsoft Power BI Desktop in the AWS Cloud

* Using the Microsoft Power BI Desktop in the AWS Cloud is a popular solution for many of the challenges described in the previous section. In this model, customers host the Microsoft Power BI Desktop in the AWS Cloud, and then access it remotely on premises. The following diagram shows an example.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xxfounbqadar8vxfywb9.png)
*Microsoft Power BI Desktop deployed in the AWS Cloud*

* Although the diagram depicts user connectivity to the desktop occurring through the internet, AWS VPN and Direct Connect are both valid connection types too. Because only graphical management traffic is transmitted, the bandwidth requirements are well suited for typical internet connections.

* In this model the Microsoft Power BI Desktop is hosted within the Amazon VPC in a public subnet and has direct network connectivity to data sources with private IP addresses, such as Amazon RDS and Amazon Redshift. You can connect to Amazon Athena and other regional services by using a VPC endpoint connection as the destination (pictured in the diagram), or by using the regional public service endpoint.

More Details can be found [here](https://docs.aws.amazon.com/en_us/whitepapers/latest/using-power-bi-with-aws-cloud/using-microsoft-power-bi-desktop-in-the-aws-cloud.html).

## Summary of Microsoft Power BI Desktop connectivity options

* For a small number of users with light dataset requirements, running Microsoft Power BI Desktop on premises and connecting securely over the internet, or using AWS VPN, might be an adequate solution. Make sure that security is configured and maintained in this model. We also recommend testing this configuration to determine if it meets users' performance expectations

* As the number of users increase, we recommend that you consider connectivity through AWS Direct Connect. Direct Connect provides a better user experience when loading larger datasets. Make sure that users are aware of the cost implications of transferring large datasets.

* We recommend that you evaluate running Microsoft Power BI Desktop in the AWS Cloud. This is likely to provide both the best performance experience for the end user and the best management experience for cloud administrators. Amazon WorkSpaces in particular can scale from a small number of users to thousands of users. These Services also provide significant security and management benefits.

# Connecting the Microsoft Power BI service to AWS data sources

* Microsoft Power BI service (SaaS) can be connected directly to internet-accessible data sources, or to private data sources in an Amazon VPC. Connection to private data sources requires an application component called Microsoft on-premises data gateway. The Microsoft on-premises data gateway is downloaded and installed on an Amazon EC2 instance in the VPC and configured with Microsoft Power BI credentials. The gateway establishes an outbound connection to the Microsoft Azure Service Bus over the internet, and is configured in Microsoft Power BI to connect to data sources that it can access. Larger deployments can use multiple on-premises data gateways to balance load or increase fault tolerance.

More Details can be found [here](https://docs.aws.amazon.com/en_us/whitepapers/latest/using-power-bi-with-aws-cloud/connecting-the-microsoft-power-bi-service-to-aws-data-sources.html).

# Using Microsoft Power BI Report Server in AWS

* Microsoft Power BI Report Server provides a private report server that Microsoft Power BI Desktop users can publish reports to and then share with a wider audience. Although it has traditionally been deployed on premises, Microsoft Power BI Report Server can reside within the AWS Cloud as well. This enables you to avoid hosting it in your data center while still making it accessible to both Microsoft Power BI Desktop users and the audience that needs to view published reports.

More Details can be found [here](https://docs.aws.amazon.com/en_us/whitepapers/latest/using-power-bi-with-aws-cloud/using-microsoft-power-bi-report-server-in-aws.html).

# Using Amazon QuickSight

* Customers considering using the Microsoft Power BI Suite with AWS are encouraged to evaluate Amazon QuickSight as an alternative. This fully managed cloud service natively connects to data sources in AWS, reducing the complexity and cost when compared to other BI solutions.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x9y5t04lr3v728hrztq6.png) 
*How Amazon QuickSight works*

* When compared to other BI solutions, Amazon QuickSight has the following benefits:

 * With Amazon QuickSight, there’s no need to download and install a client application. All functionality, including authoring and reporting, can be accessed from any platform (Windows, Mac, Linux, and so forth) by a web browser.

 * Amazon QuickSight is delivered as a fully managed, cloud-native SaaS application and is simple to build and deploy dashboards to production. The service is serverless, which means that you do not need to calculate how many nodes and servers you need to support your users. QuickSight also takes full advantage of high availability features provided by AWS for resiliency.

 * It’s easy to get started in small or large settings, with the ability to add users from a point-and-click interface within QuickSight. No external administrator intervention needed.

 * Amazon QuickSight is powered by Super-fast, Parallel, In-memory Calculation Engine (SPICE) for a fast response time (in the milliseconds) and interactive visualizations. Datasets can currently scale up to 200 GB.

 * Amazon QuickSight pricing is simple, inexpensive, and has two components: report authors and report readers. Report authors, who create and publish interactive dashboards, are priced per user. If users do not log in during a given month, there are no charges for those users. Report readers are charged per 30-minute session, with a maximum of $5.00 per reader per month. A free trial allows you to evaluate Amazon QuickSight without any charges. For more information, see Amazon QuickSight Pricing.

# Conclusion

 * If you’re looking to use Microsoft Power BI Desktop, we generally find that customers start experimenting with the software on premises, connecting to data sources over the internet. While private connectivity options exist for using AWS VPN and Direct Connect, many customers have concluded that running Microsoft Power BI Desktop in Amazon WorkSpaces provides a better performing experience.

 * If you want to connect data sources in AWS to Microsoft Power BI Service, you should feel comfortable knowing that this is an established architectural pattern. You can install the Microsoft on-premises data gateway within an Amazon VPC and connect data sources such as Amazon RDS, Amazon Redshift, Amazon Athena, Amazon OpenSearch, and AWS Lake Formation seamlessly to the service.

 * If you plan on using Microsoft Power BI Report Server in AWS, there is also an established path forward. You can install the Microsoft Power BI Report Server within an Amazon VPC, close to AWS data sources, and there are connectivity options for both authors and report users.

 * If you want a solution that provides the same business outcomes, without the added complexity of installing, configuring, patching, and scaling self-managed BI solutions, we recommend Amazon QuickSight. This fully managed service combines all the required functionality in a simple web browser experience with pay-per-user pricing. There is nothing to install and no additional components are required.

 * Hopefully, this is just the start of your business intelligence journey with AWS. For additional resources to help you get started, see the Appendix: Microsoft Power BI supported AWS data sources.

# Reference

[Original paper](https://docs.aws.amazon.com/en_us/whitepapers/latest/using-power-bi-with-aws-cloud/using-power-bi-with-aws-cloud.html)