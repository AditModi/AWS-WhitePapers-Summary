# Introduction

* Microservice application requirements have changed dramatically in recent years. Many modern applications are expected to handle petabytes of data, require close to 100% uptime, and deliver sub-second response time to users. 

* Typical N-tier applications can’t deliver on these requirements. Today reactive architectures and reactive systems have been adopted by a growing number of enterprises, because it is necessary to design applications in a highly scalable and responsive way. 

**But what exactly is a reactive system?**

* *The Reactive Manifesto, describes the essential characteristics of reactive systems including: responsiveness, resiliency, elasticity, and being message driven.*

* Being message driven is perhaps the most important characteristic of reactive systems. Asynchronous messaging helps in the design of loosely coupled systems, which is a key factor for scalability. In order to build highly resilient systems, it is important to isolate services from each other. 

* Microservices are an architectural and organizational approach to software development where software is composed of small independent services that communicate over well-defined APIs. 

* These services are owned by small, self-contained teams. Isolation and decoupling are an important aspect of the microservices pattern as well. This makes reactive systems and microservices a natural fit.


# Characteristics of reactive systems

* The following sections outline the essential characteristics of reactive systems.

## Responsive

* Responsiveness means that systems respond in a timely manner. It is crucial to reduce latency between request and response. Latency is an important factor that can, under certain circumstances, be decisive for the success or failure of a service. It directly influences the behavior of the consumer of the website or webservice. 

* Imagine, you want to check out your shopping cart on an e-commerce website and processing of the payment takes a significant amount of time or even crashes. This results in an overall bad user experience. Latency is often correlated with conversion rate. Responding in a timely manner ensures these systems deliver a consistent behavior and quality of service.

## Resilient

* A resilient system stays responsive in the face of failure. A resilient workload has the capability to recover when stressed by load, attacks, and failure of any component in the workload's components. 

* If a failure occurs in one component, this must be contained in the source of the failure by isolating it from others. As a result, this prevents a small failure from causing a major outage of the complete system.

## Elastic

* A reactive architecture remains responsive under varying workload. This elasticity requires you to be able to scale a system dynamically and add or remove resources to react to changes which leads to the avoidance of bottlenecks. These systems support predictive as well as reactive scaling based on metrics which leads to an efficient use of resources.

## Message driven

* In order to establish boundaries between services, reactive systems rely on asynchronous message-passing. This helps ensure loose coupling, isolation, and location transparency. 

* Location transparency can be achieved by using a message queue or a similar system to access data without the explicit knowledge of the location. 

* This can be achieved using network resources like DNS. The main advantage of this approach is that it’s not important where exactly the resource is located. This is key for resiliency and elasticity, because a failover mechanism can be implemented that abstracts the actual resource behind a network name. 

* In reactive systems, asynchronous message-passing is also used to distribute information of failures or other errors as messages. By using this pattern, the system enables better load management by monitoring the message queues and implementing a mechanism for backpressure.

* This allows non-blocking communication which leads to less system overhead. One of the biggest challenges to effectively scale large systems is the bottleneck that is introduced by shared resources. This communication pattern helps minimizing concurrent access.

## Observability and tracing

* As already outlined, reactive systems are defined as message-driven and resilient, which means, a reactive system - by nature - will be a distributed system. This necessarily includes additional network communication. 

* In a traditional monolithic application (for example, a JavaEE application) the complete application resides in the same memory on one device, potentially with full redundant copies on other machines to allow for failover. 

* In many cases, monolithic applications have better latency compared to distributed application but with limitations in scalability and availability.

* This tracing header can be used to correlate events with log entries. In addition, you can use services such as AWS X-Ray to trace and analyze requests as they travel through your entire system, from Application Load Balancer or Amazon API Gateway APIs to the underlying services, making it much easier to identify issues and visualize service calls. 

* AWS X-Ray helps developers analyze and debug production, distributed applications, which are built using a microservices architecture. X-Ray’s service maps let you see relationships between services and resources in your application in real
time. 

* You can easily detect where high latencies are occurring, visualize node and edge latency distribution for services, and then drill down into the specific services and paths impacting application performance.

## Reactive programming and reactive streams

* It is important to make a distinction between reactive systems and reactive programming because these two completely different concepts are often confused.

* Reactive systems are responsive, resilient, elastic, and message driven as per preceding paragraphs. This description shows that reactive systems are an architectural approach to design distributed responsive systems. 

* Reactive programming, however, is a software design approach which focusses on asynchronous data streams. Everything in the application is seen as a stream of data which can be observed (observer pattern).

* The goal behind the Reactive Streams’ specification is to create the basis for compatible implementations that can communicate with each other. 

* This specification includes a minimal set of interfaces, methods, and protocols, that define operations and
entities that are necessary to implement a compatible version.


# Typical use-cases for reactive systems

**What do typical use cases for reactive systems have in common?** 

* Potentially millions of messages flow into the backend system. The impact of this requirement is a backend system that is able to quickly scale out and is resilient.

* Advertising technology (Ad tech) face an interesting challenge to collect or track a lot of information. Usually this is a tracking pixel on a website that ingests data into a system.

* Depending on the campaign and its success, the amount of data that needs to be collected and transformed may significantly vary and is not always predictable. The following sections take a closer look at an example implementation for a tracking system for ads.

* For eCommerce, it’s a similar situation: if a product goes viral on social media or an advertising campaign is surprisingly successful, a lot of people will visit the eCommerce website and order products.

* For IoT, you need to be able to manage millions of connected devices that produce a huge amount of sensor data. This data needs to be collected, stored, and analyzed. Often it is necessary to send additional data back to the connected devices to update the status. 

* The complete system has to be responsive in order to deliver feedback to users or trigger changes in the devices or machines within a short span of time.

# Example architecture

* To help explain how reactive principles can be applied to real world application, the following example architecture implements a reactive system for the following common ad-tech use case.

* Many ad-tracking companies collect large amounts of data in near-real-time. In many cases, these workloads are very spiky and heavily depend on the success of the ad- tech company’s customers; they also need to be responsive and resilient, as these systems form part of the backbone of many ad-tech companies.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dgkl4rgxsuft4dizll0l.png)

* In this example architecture the system can be logically divided into one part focused on data collection and another part focuses on core data updates. The data collection subsystem is responsible for collecting, validating, and persisting the tracking data, potentially enriching it with some core data in the process. 

* In the core data update subsystem, the data that is used to validate and enrich the incoming raw tracking data—this typically includes data on ad inventory, campaigns, publishers, publishers’ remaining campaign budgets, and etc.— receives updates from upstream back-office systems (not implemented in the example architecture) and notifies all subscribers about the changed and added data.
 
* Typically for an ad-tracking use case, the data collection subsystem can be separated further into a real-time part and a non-real-time part.  

* More details on Example architecture and using event driven and non-blocking framework available [here]().

# The reactive manifesto and AWS services

* The following section focuses on how to map compute and messaging services to the four principles of the reactive manifesto. This section only focuses on a limited set of services which are also used in the example implementation examined in the last chapter.

## Service introduction

* **Amazon Elastic Container Service (Amazon ECS)** is a fully managed container orchestration service. It is a shared state, optimistic concurrency system that provides flexible scheduling capabilities for your tasks and containers. 

* **Amazon Elastic Kubernetes Service (Amazon EKS)** gives you the flexibility to start, run, and scale Kubernetes applications in the AWS Cloud or on-premises. Amazon EKS helps you provide highly-available and secure clusters and automates key tasks such as patching, node provisioning, and updates. 

* **AWS Lambda** is a serverless compute service that lets you run code without provisioning or managing servers, creating workload-aware cluster scaling logic, maintaining event integrations, or managing runtimes. With Lambda, you can run code for virtually any type of application or backend service - all with zero administration.

* **Amazon Kinesis Data Streams** is a massively scalable and durable real-time data streaming service. 

* **Amazon ElastiCache** offers a fully managed in-memory data store, compatible with Redis or Memcached that enables you to power real-time applications with sub- millisecond latency. The service can scale-out, scale-in, and scale-up to meet fluctuating application demands. Write and memory scaling is supported with sharding. 

* **Amazon DynamoDB** is a key-value and document database that delivers single-digit millisecond performance at any scale. It is a fully managed database with scalability and high availability built in. 

## Principles and services

### Responsive

* As previously mentioned, being responsive means that systems respond in a timely manner, even under heavy load. To meet this requirement, special attention must be paid to latency. 

### Resilient

* According to the AWS Well-Architected Framework, resiliency is “The ability for a system to recover from a failure induced by load, attacks, and failures”. Depending on the service used, resiliency is either part of the service offering or has to be designed by the customer, depending on the specific requirements. The following section discusses resiliency for different AWS services and how this can be implemented.

### AWS Global Infrastructure

* The AWS Global Infrastructure consists of multiple building blocks that provide different levels of independent, redundant components. AWS partitions resources and requests via some dimension. 

### AWS Lambda

* In addition to the benefits of the AWS global infrastructure, AWS Lambda offers several features to help support your data resiliency and backup needs. Lambda runs instances of your function in multiple AZs to ensure that it is available to process events in case of a service interruption in a single zone. 

### Amazon ECS

* Amazon Elastic Container Service (Amazon ECS) schedulers leverage the same cluster state information provided by the Amazon ECS API to make appropriate placement decisions. 

### Amazon EKS

* Amazon Elastic Kubernetes Service (Amazon EKS) runs Kubernetes control plane instances across multiple Availability Zones to ensure high availability. Amazon EKS automatically detects and replaces unhealthy control plane instances, and it provides automated version upgrades and patching for them. 

### Amazon DynamoDB

* DynamoDB is a regional, cell-based service that offers a high degree of resiliency out of the box. The service provides on-demand backup capability. It enables you to store full backups of your tables for long-term retention and archival. 

### Amazon ElastiCache

* Amazon ElastiCache helps mitigate common failure modes that could affect your (external) caching layer and thus the overall system’s responsiveness and resiliency. 

### Amazon Kinesis Data Streams

* Amazon Kinesis Data Streams is a massively scalable and durable real-time data streaming service. High availability and durability are achieved by synchronously replicating data to three AZs.

### Elastic

* Elasticity is “The ability to acquire resources as you need them and release resources when you no longer need them. In the cloud, you want to do this automatically.” Depending on the service, elasticity is sometimes part of the service itself. 

* Other services require vertical scaling. A third group of services integrate with AWS Auto Scaling. The following section discusses elasticity for different AWS services and how this can be implemented.

### AWS Lambda

* AWS Lambda has elastic scalability already built in: the service executes your code only when needed and scales automatically, from a few requests per day to thousands per second. 

### Amazon ECS and Amazon EKS

* Amazon ECS cluster auto scaling gives you control over how you scale tasks within a cluster. Each cluster has one or more capacity providers and an optional default capacity provider strategy. The capacity providers determine the infrastructure to use for the tasks, and the capacity provider strategy determines how the tasks are spread across the capacity providers. 

* When you run a task or create a service, you can either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy.

### Amazon Kinesis Data Streams

* Amazon Kinesis Data Streams offers provisioned capacity: each data stream is composed of one or more shards that act as units of capacity. Shards make it easy to design and scale a streaming pipeline by providing a predefined write and read capacity.

### Message driven

* A message-driven architecture uses messages to communicate between decoupled services and is common in modern applications built with microservices, since asynchronous message-passing between loosely coupled services helps to ensure isolation and provides location transparency. The following section discusses different messaging patterns and how these can be implemented on AWS.

### Event sourcing and CQRS

* In the context of microservices architectures, event sourcing decouples different parts of an application by using a publish/subscribe pattern, and it feeds the same event data into different data models for separate microservices. 

### AWS messaging services

* AWS messaging services enable different software systems and end devices - often using different programming languages, and on different platforms - to communicate and exchange information. 

### Fanout

* The Fanout scenario is when a message published is replicated and pushed to multiple endpoints, which allows for parallel asynchronous processing. For example, you can develop an application that publishes a message to an SNS topic whenever an order is placed for a product. 

* Then, Amazon Simple Queue Service (Amazon SQS) queues that are subscribed to the Amazon Simple Notification Service (Amazon SNS) topic receive identical notifications for the new order. A service attached to one of the SQS queues can handle the processing or fulfillment of the order.


### Filtering

* By default, an Amazon SNS topic subscriber receives every message published to the topic. To receive a subset of the messages, a subscriber must assign a filter policy to the topic subscription. A filter policy is a simple JSON object containing attributes that define which messages the subscriber receives. 

* When you publish a message to a topic, Amazon SNS compares the message attributes to the attributes in the filter policy for each of the topic's subscriptions. If any of the attributes match, Amazon SNS sends the message to the subscriber. Otherwise, Amazon SNS skips the subscriber without sending the message.


# Conclusion

* After reviewing the concepts for reactive systems, you should have a good understanding of the landscape of potential AWS services you can choose to build a reactive system on AWS.

* The key patterns in this paper, highlighted the necessary concepts for implementing the foundational building block for reactive systems. The characteristics of reactive systems based on the definition of the reactive manifesto.

* The example architecture utilizes the same patterns on a micro-level which are being used on an architectural macro-level. The AWS services are a great starting point to implement reactive principles and microservices improve isolation of different workloads. In combination with managed services, you can focus on the business requirements and remove the undifferentiated heavy lifting.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/reactive-systems-on-aws.pdf?did=wp_card&trk=wp_card)