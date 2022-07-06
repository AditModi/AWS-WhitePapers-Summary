Today, industrial companies want to ingest, store, and analyze IoT data, closer to the point where data is generated to enable predictive maintenance, improve quality control, enhance worker safety, and more. Industrial companies can greatly benefit from the industrial edge’s ability to solve for use cases which require low latency, optimized bandwidth utilization, need for offline or autonomous operation, and adherence to regulatory guidelines. This whitepaper outlines the design considerations for industrial IoT architectures which use the industrial edge.

# Introduction

* Industrial Edge computing involves hardware and software technologies that enable storage, computing, processing, and networking close to the industrial devices that generates or consumes data within a factory or industrial environment. 

* While Edge computing moves compute closer to the data generation source, the edge can range from device hardware to edge gateways to local nodes to cell tower nodes to local data centers. 

* This whitepaper focuses on industrial IoT use cases where edge gateways are placed in a stationary location in an industrial environment and play the role of intermediary between on premise Operational Technology (OT) systems and the cloud and catalog common industrial edge architecture patterns to provide prescriptive guidance to customers implementing industrial IoT systems. 

* Edge gateways fill the critical role of intermediary processing nodes and integrator between industrial assets and the Amazon Web Services (AWS) Cloud, and is heavily influenced by edge computing imperatives. 

* In cases where OT systems are not capable of supporting authentication, authorization, and encryption techniques, the edge gateway can act as a guardian to locally interface with these less-capable systems, bridging them to cloud services with strong security patterns. 

* The following section reviews edge computing imperatives.

# Edge computing imperatives

* Use cases have emerged across various industries that need to combine cloud resources with local processing and storage of data under certain conditions described as edge computing imperatives. 

* Edge computing moves processing and analysis closer to endpoints where data is generated, delivering real-time responsiveness and reducing costs associated with transferring large amounts of information.
Industrial edge computing imperatives are as follows:
•	Data volume/bandwidth considerations
•	A need for autonomous or disconnected operation
•	Privacy, security and data sovereignty concerns
•	Low-latency requirements
•	Network cost considerations

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kaafg1u94o4leyazvdl8.png)
*Edge architecture influenced by edge computing imperatives*

* Industrial IoT deployments consist of a combination of plant and local Operational Technology (OT), plant/local Information Technology (IT), and remote IT resources which may be in the public cloud or an enterprise datacenter. 

* The benefit of splitting workloads between local and remote processing is to balance the timeliness and high bandwidth of local resources with the scale and elasticity of remote resources. This introduces architectural complexity around managing the different properties of local and remote resources. 

* An Edge Gateway enables you to extend cloud capabilities into industrial environments and help monitor data flows between networks with different properties where direct connectivity between all participants is not recommended. 

* Some of these differences might include network performance (latency and throughput), or security and administrative domain. For example, it is common in global deployments to use an Edge Gateway between a low-latency local-area network (LAN) and resources on the high-latency wide-area network (WAN). 

* Another example, is an Edge Gateway might mediate between security and administrative domains such as in a Perdue Enterprise Network Architecture (PERA), and ANSI/ISA-95 network segmentation.
Industrial edge deployments are heavily influenced by what AWS calls the Three Laws of distributed computing: the law of physics, which constrain the latency, throughput and availability of network connectivity; the law of economics which determine the cost-effectiveness of transferring ever-increasing volumes of data; and the law of the land which regulates how data is handled and where it can be stored.

* In addition to understanding the network properties that need to be mediated, and how the three laws effect the deployment, industrial edge architectures must account for the properties and requirements of the data flows involved. 

* In AWS’ experience, the most important aspects to consider are the type of data flow, the direction of data flow, and the quality of data flow. While there are many variants within these axes, AWS has identified useful architecture patterns as a starting point for architectural discussions.

* This document describes how to navigate the combinations of network properties and data flow types to get the optimal outcomes, subject to the constraints of the Three Laws and provide common industrial edge architecture patterns.

# Industrial edge design considerations

## Data flow types

* There are three primary types of data flows in industrial edge environments: system, telemetry, and object data.

* **System data** is information such as system state, configuration and critical alerts. It is relatively small (less than a gigabyte even in large installations) but it is critical that the data is consistent and highly-available within the local environment, this means that the law of physics prevents this data from residing primarily in the cloud. 

* Changes to the system data should be propagated quickly when the WAN allows it, and must not be dropped if the WAN is down. Most Industrial IoT systems include at least some system data flow, even if the primary purpose of the system is to support other data flows.

* However, since it is unsafe to have critical control loops depend on WAN connectivity, these flows are low-traffic, such as ANSI/ISA-95 Level 3 and Level 4 (MES, ERP).

* System data flows typically support traffic rates up to around 10-100 kb per second, although actual usage is typically much lower. Inbound traffic (such as from the remote resource to the edge gateway) may happen at any time, so polling architectures is not recommended and care must be taken to coordinate with local Network Address Translation (NAT) and firewall policies. 

* Session protocols that originate in the local facility and create a bidirectional connection, such as MQTT or AMQP, are ideal for this type of data. With AWS IoT Greengrass, for example, the MQTT spooler performs this task on system data.

* **Telemetry data** is structured, time-series information about the performance and actions taken by local devices, and is often formatted specifically to fit into existing monitoring and alarming infrastructure. Because these are time-series data, they can grow without bounds and are often much larger than system data. 

* Individual telemetry streams are usually fairly small, in the range of 10s to 100s of kilobytes per second, but the sum of all the telemetry streams in a facility can be quite large. Unlike system data, passing telemetry data to remote resources can tolerate high latencies ranging from seconds to hours in some applications.

* In architectures where the source is capable enough and the network segmentation allows, telemetry data can be moved directly from the source to the remote resource after receiving authentication and authorization (often in the form of short-term credentials) from the edge gateway. However, most telemetry sources cannot do this, and some enterprise network security policies disallow it. 

* In those cases, telemetry data can be routed through the edge gateway itself. When doing so, gateways support the higher data rates by taking advantage of lax latency requirements. This allows the gateways to aggregate telemetry data to reduce the number of round-trip transactions to remote resources. 

* This aggregation may include policies on data quality, such as allowing data to be dropped to maintain freshness or queuing data to prevent loss (at the cost of freshness). With AWS IoT Greengrass, for example, the Streams Manager component performs this task.

* **Object data** is unstructured time-series data such as video feeds or analogue sensor streams. The format and contents of this data is specific to the application or sensor, and it is common to apply special algorithms to post-process the data into a more structured metric data. 

* For example, vibration data is a high-resolution audio signal usually analyzed by Fourier or Wavelet analysis, a computationally expensive task.

* Video and LIDAR sensors are even more data and compute intensive. These object data flows are generally hundreds of Kbs to dozens of Mbs, and in plants with large numbers of sensors the aggregate data bandwidth can be terabits per second. 

* The edge gateway can’t process this data, and the WAN can’t usually support more than a small fraction of it. As a result, the edge gateway’s role in managing object data is as a director: understanding the inventory and purpose of the sensors and using telemetry and system data to inform local processors how to manage it. 

* The edge gateway may also at times direct some useful subset of the object data through the WAN, for example, to provide remote debugging capabilities. The processing application and data flows for object data are not generalized, and few standards exist, meaning custom application development is necessary.

## Data flow direction

* Directional analogy is used for data flows around the edge gateway, where separate data flows into North-South flows, as shown in the following figure across the boundaries of different networks (such as from LAN to WAN) and East-West flows within the local area network. 

* While an edge gateway is not necessary to manage East- West flows, it is often convenient to have them manage these data flows in cases where they are already in place to support north-south flows. When both are present, it is called a North-South / East-West gateway, which places additional resource load on the edge gateway. 

* As data volume grows, it is common to need multiple gateways with specialized roles, increasing architectural complexity to achieve the correct performance. In the early stages of cloud adoption, it is common for data flows to be primarily northbound, meaning that data is moving from the local plant to a remote, centralized data lake for asynchronous processing.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0wzakcex0qf75gucwri6.png) 
*Data Flow Direction*

## Data flow quality

* Data flow quality has several dimensions: latency, ordering, and loss. In most situations, the Three Laws force difficult tradeoffs in data flow quality. 

* The laws of physics and economics often prevent you from having the ideal latency and ordering properties, and the law of the land may impact whether data loss is acceptable. It is important not to confuse the abstract concept of data flow quality with protocol-level concerns such as MQTT QoS, AMQP QoS, TCP retransmission, TLS retransmission, or OPC-UA metric quality values. 

* It’s important to define the abstract data quality needs for the use case, such as data must arrive in order or data loss cannot be tolerated. It’s also important to understand the protocols already in place to determine how to achieve the data quality requirements within the constraints of the network protocols.
 
* A key concern for data flow quality is the appropriate behavior in exceptional circumstances, such as long-term network disruption. Because edge gateways are installed at connection points between networks, it is often not cost-effective or physically feasible to provide fully redundant connections. The Edge Gateway architecture must ensure that system requirements are met even in the face of hours- or days-long disruptions.


## Performance considerations

* Industrial IoT (IIoT) systems consist of a mix of general-purpose IT components such as relational databases and message brokers, and specialized applications which are purpose-built for a particular task. 

* The performance of specialized applications is recommended rather than attempting the same task with general-purpose components, but assembling general-purpose components is often faster and cheaper. 

* For example, a purpose-build packet-handling pipeline written in C and running in the operating system kernel can easily move 10 GBs on a common Intel-class PC, whereas a general-purpose message broker applied to the same task may only be able to move 1 MBs on the same hardware. 

* The following architecture patterns can be used as-is but it is common to need to modify them to accommodate specialist components for signals- processing, computer vision, machine-learning, or high-data-rate workloads.


## Protocol considerations

* IIoT deployments typically implement a number of communications protocols, which creates complexity. In general, consolidating on a small number of protocols helps reduce that complexity. 

* However, the security and performance differences between the LAN and the WAN may require the use of different protocols, especially in cases where the WAN is the public internet. 

* For example, while it is widely deployed inside controlled local-area networks, OPC-UA and OPC-DA are not safe to expose to the internet and when used in a LAN environment, its recommended to use the newer version of OPC with security features enabled and implement strong perimeter security. 

* Another example, synchronous communications such as MQTT in QoS 1 or QoS 2 modes have low performance as network latency increases.

* One of the key roles of the edge gateway is to mediate between the protocols that are suitable for each environment. For example, OPC-UA messages need to be proxied through secure tunnel to provide appropriate authentication and flow control. 

* Another example, an edge gateway can perform application-level message delivery validation for messages sent over QoS 0, achieving high performance on high-latency links while still achieving the delivery guarantees of QoS 1.

# Mapping to the ISA-95 Model

* ANSI/ISA-95 provides a common view of industrial IT systems, which can be helpful for understanding the role of edge gateways and hybrid cloud IIoT environments. 

* Because of the latency and availability characteristics of the WAN, it is not recommended to take a dependency on remote resources for Level 0-1 systems. 

* Critical Level 2 systems should also be local, but additional remote monitoring may be layered on so that operations staff can observe global trends. It is recommended that data delays and gaps for these remote Level 2 systems can cause only observability failures and not production impact.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sgt33aec0gw2q0rsaewf.png)
*The ANSI/ISA-95 Pyramid Showing Expected System Response Times*  

* Level 3 and Level 4 systems can benefit from the properties of remote computing, including the ability to aggregate data from multiple sites and the ability to perform compute-intensive analytics tasks.

* Edge gateways are generally found in layer 3 and OI/IT Gateways responsible for industrial protocol conversion are often found in layer 2 of the Purdue model

# Common IIoT Edge scenarios and use cases

## Telemetry export

* The most common type of IIoT use case, and the place where most IIoT initiatives start, is with a one-way transfer of telemetry data from the local site to a remote data lake where the data from multiple industrial sites is made available for analytics and monitoring workloads.
 
* These workloads include a control channel that is mostly used for managing the edge gateway itself, such as configuring data export and security policies. This system data is real-time when the WAN link is available and uses asynchronous state management through device shadows to guard against link failure. 

* Typically, the telemetry data is aggregated on the edge gateway and sent northbound in batches, with acknowledgement and retry to guard against data loss. 

* The following architecture shows system flow messages up to around 10 per second and telemetry messages up to around 1,000 per second, with enough local storage to avoid data loss for several hours of link failure.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wdym7d6t4z06mzp2ofgu.png)
*Telemetry export through edge gateway*
 
* There are some implementation choices to make for this workload, as the gateway needs to adapt to the specifics of the telemetry flows. Low-traffic telemetry may be sent directly through the system channel and managed by the IoT control systems, though depending on WAN latency this will create performance bottlenecks around 10-40 messages per second. 

* For higher-throughput data flows, the gateway will need to pre- process the data in transit. There are a variety of techniques for this. Where firewall rules allow multiple northbound connections, parallelizing the data flow can overcome the effects of network latency and allow high throughput. 

* Where firewall rules do not allow parallelism, the gateway must aggregate telemetry into batches that will efficiently utilize the high-latency northbound link. AWS IoT Greengrass supports this batching through its Stream Manager component.

* The following sections discuss industrial edge architecture variants for other IIoT use cases.

 * Variant 1: Adding East/West Telemetry Routing
 * Variant 2: Critical telemetry
 * Variant 3: Object Data
 * Variant 4: Adding remote control
 * Variant 5: Edge analytics/ML
 * Variant 6: Nested Gateways
 * Variant 7: Edge Gateway high availability
 * Variant 8: Edge Gateway running a soft PLC (Replacing traditional PLC’s)
 * Variant 9: Modern PLC’s acting as Edge Gateway
 * Variant 10: Cluster computing at the edge
 * Variant 11: Secondary sensing use case

* More Details about industrial edge architecture variants can be found [here]().

# Conclusion

* Industrial customers can greatly benefit from the industrial edge’s unique ability to solve for use cases which require reduced latency, optimized bandwidth utilization, offline or autonomous operation, and adherence to regulatory or security guidelines based on the physical placement of applications and data in an explicit location such as a province or country. 

* Since industrial IoT workloads can be diverse and complex, it is important to understand the use case requirements, customer’s industrial landscape, and desired business outcomes before identifying the edge gateway architecture pattern which best fits the project. 

* Selecting the right edge architecture pattern is an important step towards building a secure, high-performing, resilient, reliable, and cost effective industrial IoT solution. 

* While single edge gateway implementations for a proof of concept or pilot project can be straightforward, industrial customers need to plan for implementing such a system at scale, including enterprise-grade security, orchestration, life cycle management, and governance.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/industrial-iot-architecture-patterns.pdf?did=wp_card&trk=wp_card)