# Introduction

* Customers want to realize the value held in the data their organization generates. Common use cases include helping them expedite decision making, publishing data externally to foster innovation, or creating new revenue streams by monetizing the data.

* Organizations that successfully generate business value from their data, will outperform their peers. An Aberdeen survey saw organizations who implemented a Data Lake outperforming similar companies by 9% in organic revenue growth. 

* These leaders were able to do new types of analytics like machine learning over new sources like log files, data from click-streams, social media, and internet connected devices stored in the data lake. 

* This helped them to identify, and act upon opportunities for business growth faster by attracting and retaining customers, boosting productivity, proactively maintaining devices, and making informed decisions.

* To best realize this value using data lakes, customers need a technology cost breakdown for their budget to build a solution. But without building the solution, they don’t know how much it will cost. This is a common paradox that delays many customers from starting their data lake projects.

* Customers need a platform that increases agility, lowers cost of experimentation, and provides a technical breadth to support all their use cases, through an innovative platform. Ideally, the platform can rapidly validate or discount solutions against business objectives. 

* This encourages a culture of failing fast, which enables further experimentation to optimize solution matching against business imperatives.

* A data lake is a common way to realize these goals. There are many considerations along this journey, such as team structure, data culture, technology stack, governance risk, and compliance.
Costing data lakes requires a different approach than delivering them. 

* Customers must focus on identifying and measuring business value early on so that they can start their projects quickly and demonstrate the value back to the business quickly and incrementally.

## What should the business team focus on?

 * **Think big** – It’s important to create a vision that your organization can rally around to help guide decision making in-line with a common goal. At Amazon, we have a phrase: “Stubborn on vision; flexible on details”. This sums up the importance of creating an environment that allows autonomous decision-making, while everyone pulls in the same direction and is flexible about the implementation details.
 * **Measure business value** – Without measuring business value, it’s hard to justify any expenditure or drive any value from your testing early on in the project.
 * **Prototype rapidly** – Focus your energy on driving business outcomes with any experiments you run.
 * **Understand what influences costs** – Analytics projects generally have similar stages, ingestion, processing, analytics, and visualization. Each of these stages has key factors that influence the cost.
 * **Cost model a small set of experiments** – Your analytics project is a journey. As you expand your knowledge, your capabilities will change. The sooner you can start experimenting, the sooner your knowledge will grow. Build a cost model that covers to smallest amount of work to impact your business outcomes and iterate.
 * **Avoid wasting energy building technology stacks** – Building solutions from the ground up is expensive, time-consuming, and very rarely provides any direct value to your organization.

# Defining the approach to cost modeling data lakes

* IT projects historically have well-defined milestones that make cost modeling a fairly simple process. Selected software is usually a commercial off-the-shelf (COTS) product, which can be costed (including licenses) and based on predictable metrics, such as number of users and number of CPUs.

* The effort to implement the software is well within the standard skill sets of the IT department, with plenty of experience in similar deployment models to draw on to get an accurate picture of the implementation time. This will all feed into a large design document that can be used to calculate costs.

* Therefore, you can expect to use the same cost modeling you have previously used for an analytics project. The challenge here is that an analytics project often doesn’t have a clear end. 

* It’s a journey where you explore and prepare data in line, with some aspirational business outcomes. This makes it’s difficult to know the following:
 * How much data will be ingested
 * Where that data will come from
 * How much storage or compute capacity will be needed to make sense of that data
 * Which services are required to exploit the data to realize and deliver value throughout the business

* To experience the business values of analytics project, you must first get started. An on- premises data lake requires a large upfront expenditure, which can be hard to justify when you are unclear on the return on investment.

* However, with the AWS Cloud, you can deploy AWS services in minutes, run your experiments, and then shut down the infrastructure, paying only for what you use. 

* With no capacity constraints and a broad array of analytics services, you can run many exploratory experiments concurrently to find the right solution. This, coupled with the ability to turn off your experiments, lowers the cost of experimentation while increasing speed.


# Measuring business value

* The first project on your data lake journey starts by stating what your business goals are. These are often extracted from your business strategy. Business goals are high- level aspirations that support the business strategy, such as “improve customer intimacy”.

* Once these business goals are identified, together with your team write a set of business outcomes that would have a positive impact against these goals. Outcomes are targeted and measurable, such as reduce cost of customer acquisition.

* Finally, we identify a number of metrics that can be measured to validate the success of the experiments. This helps ensure that the right business value is being achieved.

* A good example of this is Hyatt Hotels, a leading global hospitality company. Hyatt wanted to improve customer loyalty, improve the success rate of upsells and add-ons, and better guide the users with accommodation recommendations. 

* This fed directly into their business goal to improve their American Customer Satisfaction Index (ACSI). To achieve this, the team at Hyatt identified a requirement to build personalized connections with their customers.
Some example business outcomes could be:
 * Number of return visits per 1000 customers per month
 * Number of upsells per 100 customers per month
 * Number of add-ons per 100 customers per month
 * Number of bookings made per day
 * Number of negative customer reviews per weeks

* This is only an example of one piece of work a data lake could support. After the initial piece of work is delivered, the team can then iterate. 

* For example, as a by-product of delivering the previous feature, they could have identified important information. For example, perhaps Hyatt discovered that commonly searched for add-on purchases (for example, specific spa treatments) were not offered in the chosen location. Or, they might have discovered that customers were searching for accommodation in areas that Hyatt doesn’t yet have a footprint in.

* The team could go on to develop new features and service offerings that would help them deliver a better customer experience or help them make decisions that would improve their chances of choosing the right location to scale their footprint globally to help them deliver against their business goal.

# Establishing an agile delivery process

* Once the measurable metrics are captured, teams can start running a number of experiments to assess technology, methodologies, and, most importantly, explore the data to indicate whether a solution to deliver the business goals is possible and, if so, a possible design of that solution.

* The breadth of AWS services helps remove the complexity and burden of designing and operationalizing the solution. This enables organizations to rapidly deploy solutions that typically would take months. It also allows organizations to focus on solving the business needs through deriving value from their data.

* The AWS serverless services (such as, Amazon Athena, Amazon Kinesis, and AWS Glue) allow for data manipulation and exploration without the need to deploy anything. These services can be consumed immediately in an on-demand basis.

* You can also deploy capacity provisioning services, where clusters can be provisioned in a matter of minutes. These clusters are then ready to process customer data for supporting analytic services such as Amazon EMR (Hadoop), Amazon Elasticsearch, Amazon Managed Streaming for Apache Kafka (Amazon MSK), and Amazon Redshift. If the experiment fails, you can shut down the services or just stop using the services to prevent incurring ongoing costs.

* This allows you to experiment rapidly. This was the case with Xylem, a leading water technology company. Xylem used AWS to increase innovation, allowing them to support their customer by creating smart technologies that meet the world’s water, wastewater, and energy needs.


# Building data lakes

* Because the aim is to get started on your data lake project, let’s break down your experiments into the phases that are typical in data analytics projects:
 * Data ingestion
 * Processing and transformation
 * Analytics
 * Visualization, data access, and machine learning

* By breaking down the problem into these phases, you reduce the complexity of the overall challenge. This makes the number of variables in each experiment lower, enabling you to get model costs more quickly and accurately.

* We recommend that you start your analytics project by implementing the foundation of a data lake. This gives you a good structure to tackle analytics challenges and allow great flexibility for the evolution of the platform.

* A data lake is a single store of enterprise data that includes raw copies from various source systems and processed data that is consumed for various analytics and machine learning activities that provide business value.

* Choosing the right storage to support a data lake is critical to its success. Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry- leading scalability, data availability, security, and performance. This means customers of all sizes and industries can use it to store and protect any amount of data.

* Amazon S3 provides easy-to-use management features so you can organize your data and configure finely tuned access controls to meet your specific business, organizational, and compliance requirements. Amazon S3 is designed for 99.999999999% (11 9's) of durability, and stores data for millions of applications for companies all around the world.

* Data lakes generally support two types of processing: batch and real time. It is common for more advanced users to handle both types of processing within their data lake.

* However, they often use different tooling to deliver these capabilities. We will explore common architectures for both patterns and discuss how to estimates costs with both.

## Batch processing

* Batch processing is an efficient way to process large volumes of data. The data being processed is typically not time-critical and is usually processed over minutes, hours, and in some cases, days. 

* Generally, batch processing systems automate the steps of gathering the data, extracting it, processing it, enriching it, and formatting it in a way that can be used by business applications, machine learning applications, or business intelligence reports.
 
* Before we get started, let’s look at a common set of services that customers use to build data lakes for processing batch data.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gjp8s0tscscuz4xgdk5g.png)
*Figure 1 – Common services used to build data lakes for batch data*

* The following example architecture is relatively common. It uses AWS Glue, Amazon Athena, Amazon S3, and Amazon QuickSight.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e5bnrprjt9jzl72nhrg9.png)
*Figure 2 – Example architecture for batch processing*

* The preceding example shows a typical pipeline to ingest raw data from CSV files. AWS Glue automatically infers a schema to allow the data to be queried. 

* AWS Glue jobs are used to extract, clean, curate, and rewrite the data in an optimized format (Parquet) before exposing visualizations to end users. This is all achieved using serverless technologies that reduce the operational burden to the analytics team.

* We are going to explore each of these steps in more detail, in addition to the things you need to consider along the way. But, before we do, let’s take a quick look at the other form of processing.

## Real-time processing

* Real-time processing is a way of processing an unbounded stream of data in order to generate real-time (or nearly real-time) alerts or business decisions. The response time for real-time processing can vary from milliseconds to minutes.

* Real-time processing has its own ingestion components and has a streaming layer to stream data for further processing. Examples of real-time processing are:
 * Processing IoT sensor data to generate alerts for predictive maintenance
 * Trading data for financial analytics
 * Identifying sentiment using a real-time Twitter feed

* Before we get started, let’s look at a common set of services that customers use to build data lakes for processing real-time data.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hfd3q75le1k61vhgdvng.png)
*Figure 3 – Common services used to build data lakes for real-time data*
 
* Our example architecture is relatively simple and uses the following services: Amazon Kinesis, AWS Lambda, AWS Glue, Amazon Athena, Amazon S3 and Amazon QuickSight.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/il8r02xp2w4r2j9d0vp7.png)
*Figure 4 – Example architecture for real-time processing*

* Figure 4 shows that many IoT devices send their telemetry to AWS IoT Core. AWS IoT allows users to securely manage billions of connected devices and route those messages to other AWS endpoints.

* In this case, AWS IoT Core passes the messages Amazon Kinesis, which ingests streaming data at any scale. The raw data is split into two streams, one that writes the raw data to Amazon S3 and a second that uses AWS Lambda (a serverless compute service) to filter, aggregate, and transform the data before again storing it on Amazon S3. 

* The manipulated data is then cataloged in AWS Glue and made available to end users to run ad hoc queries using Amazon Athena and create visualizations using Amazon QuickSight.
  
# Understanding what influences data lakes costs

* Across both real-time and batch processing, data flows through different stages. For each stage, there is an option to use managed services from AWS or to use compute, storage, or network services from AWS with third-party or open source software installed on top it.

* In the case of managed services, AWS provides service features like high availability, backups, and management of underlying infrastructure at additional cost. 

* In some cases, the managed services are on-demand serverless based solutions, where the customer is charged only when the service is used.

More details on What Influences data lake costs can be found [here]().

# Monitoring data lakes costs

* Once the data lake is built to provide its intended features, we recommend that you measure the cost and tie it back to business value it provides. This enables you to perform a return-on-investment analysis on your analytics portfolio. 

* To track the cost utilization for your analytic workloads, you need to define your cost allocation strategy. Cost-allocation tagging ensures that you tag your AWS resources with metadata key-value pairs that reflect the business unit the data lake pipeline was built for.

* Tags enable you to generate billing reports for the resources associated with a particular tag. This lets you to either do charge-back or return-on-investment analysis.

* Another strategy to track your costs is to use multiple AWS accounts and manage them using AWS Organizations. In this approach, every business unit owns their AWS account and provisions and manages their own resources. This lets them track all cost associated with that account for their data lake needs.

* By tracking costs and tying it back to your business value, you can complete your cost modeling for your first analytics workload. This process also lets you iterate and repeat the process again of deciding business use cases, defining KPI and building data lake features on top of your already built data lake foundation while monitoring the cost associated with it.

# Cost modeling your first experiments

* To help give confidence to cost model your first analytics project, we are going to do the following:
 * Generate a couple of fictitious customer scenarios for analytics experiments
 * Walk through our very lightweight cost model
 * Build the actual solution and demonstrate how close we got to the actual costs.

* AWS offers a free tier for most of its services. This lets you experiment with no cost. For the cost modeling that follows, we discuss both free tier and the actual costs.


## Scenario 1: Improving healthcare KPIs

* In this scenario, we are a health trust company with a goal to improve the health of the community we are serving. The following are a few key performance indicators (KPIs) that we want to achieve in near future.
 * Reduce health-related crime rate by 5% over 12 months
 * Reduce health-related crime rate by 15% over 36 months To achieve these KPIs, we decide to analyze:
1. Drugs that have a strong correlation to criminal activity
2. Health conditions that have a strong correlation to criminal activity
3. Unique citizens who are using the drugs that have top five correlations to criminal activity
4. Unique citizens who have health conditions that have top five correlations to criminal activity

* Our plan is to use these and work with identified citizens and provide them alternate drugs or provide them counseling or any other treatments as applicable, to prevent them from committing any crime. 

* We believe doing this will improve the overall mental health of our community, in addition to other activities we have been working on.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y1b48ptltqm7omgzuyku.jpg)
*Figure 1 – Example architecture for Scenario 1* 

More details on this scenario available [here]().

## Scenario 2: Improving manufacture turnaround time

* In this second scenario, we are a manufacturing company with a goal to improve the timing of our delivery. The purchasing team of this manufacturing company is looking to improve the turnaround time of the parts required for their orders by 20%

* To achieve this KPI, we decided to analyze:
 * Customer orders and line items, to identify parts that are frequently ordered
 * Suppliers data to identify suppliers that kept orders waiting
 * Customer data to identify large volume customers
 * Supplier shipping modes and order priority
 * Correlation of any returned items with supplier

* Our plan is to identify the suppliers with good track record and use them for parts requested by large volume customers. This should bring down the turnaround time for majority of the parts requested overall.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mw1dcyewlnz31l19obtp.png)
*Figure 2 – Example architecture for Scenario 2* 

More details on this scenario available [here]().

# Conclusion

 * Customers struggle with starting their analytics projects because it is difficult to estimate costs when you have no knowledge or foresight of their unique requirements as an organization. 

 * Without a cost estimate, projects fail to get funding and organizations miss the enormous value making data driven decisions can offer.

 * You can use the scenarios as templates to build out a picture of what your analytics experiment will look like. The scenarios can help your organization start a journey towards making data-based decisions and drive business value, offering benefits for your organization and its customers.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/cost-modeling-data-lakes.pdf?did=wp_card&trk=wp_card)