# Introduction

* The AWS Well-Architected Framework helps you understand trade-offs for decisions you make while building workloads on AWS. By using the Framework, you will learn current architectural best practices for designing and operating reliable, secure, efficient, and cost-effective workloads in the cloud. 

* It provides a way for you to consistently measure your workload against best practices and identify areas for improvement. We believe that having well-architected workloads greatly increases the likelihood of business success.

* The framework is based on five pillars:

 * Operational Excellence
 * Security
 * Reliability
 * Performance Efficiency
 * Cost Optimization

* This paper focuses on the security pillar. This will help you meet your business and regulatory requirements by following current AWS recommendations. It’s intended for those in technology roles, such as chief technology officers (CTOs), chief information security officers (CSOs/CISOs), architects, developers, and operations team members.

# Security Foundations

The security pillar describes how to take advantage of cloud technologies to protect data, systems, and assets in a way that can improve your security posture. This paper provides in-depth, best-practice guidance for architecting secure workloads on AWS.

### Design Principles

* In the cloud, there are a number of principles that can help you strengthen your workload security:

 * **Implement a strong identity foundation:** Implement the principle of least privilege and enforce separation of duties with appropriate authorization for each interaction with your AWS resources. Centralize identity management, and aim to eliminate reliance on long-term static credentials.

 * **Enable traceability:** Monitor, alert, and audit actions and changes to your environment in real time. Integrate log and metric collection with systems to automatically investigate and take action.

 * **Apply security at all layers:** Apply a defense in depth approach with multiple security controls. Apply to all layers (for example, edge of network, VPC, load balancing, every instance and compute service, operating system, application, and code).

 * **Automate security best practices:** Automated software-based security mechanisms improve your ability to securely scale more rapidly and cost-effectively. Create secure architectures, including the implementation of controls that are defined and managed as code in version-controlled templates.

 * **Protect data in transit and at rest:** Classify your data into sensitivity levels and use mechanisms, such as encryption, tokenization, and access control where appropriate.

 * **Keep people away from data:** Use mechanisms and tools to reduce or eliminate the need for direct access or manual processing of data. This reduces the risk of mishandling or modification and human error when handling sensitive data.

 * **Prepare for security events:** Prepare for an incident by having incident management and investigation policy and processes that align to your organizational requirements. Run incident response simulations and use tools with automation to increase your speed for detection, investigation, and recovery.

## Shared Responsibility

* Security and Compliance is a shared responsibility between AWS and the customer. This shared model can help relieve the customer’s operational burden as AWS operates, manages, and controls the components from the host operating system and virtualization layer down to the physical security of the facilities in which the service operates. 

* The customer assumes responsibility and management of the guest operating system (including updates and security patches), and other associated application software in addition to the configuration of the AWS provided security group firewall. 

* Customers should carefully consider the services they choose as their responsibilities vary depending on the services used, the integration of those services into their IT environment, and applicable laws and regulations. 

* The nature of this shared responsibility also provides the flexibility and customer control that permits the deployment. As shown in the following chart, this differentiation of responsibility is commonly referred to as Security “of” the Cloud versus Security “in” the Cloud.

 * **AWS responsibility “Security of the Cloud”**– AWS is responsible for protecting the infrastructure that runs all of the services offered in the AWS Cloud. This infrastructure is composed of the hardware, software, networking, and facilities that run AWS Cloud services.

 * **Customer responsibility “Security in the Cloud”**– Customer responsibility will be determined by the AWS Cloud services that a customer selects. This determines the amount of configuration work the customer must perform as part of their security responsibilities. 

 * For example, a service such as Amazon Elastic Compute Cloud (Amazon EC2) is categorized as Infrastructure as a Service (IaaS) and, as such, requires the customer to perform all of the necessary security configuration and management tasks. 

 * Customers that deploy an Amazon EC2 instance are responsible for management of the guest operating system (including updates and security patches), any application software or utilities installed by the customer on the instances, and the configuration of the AWS-provided firewall (called a security group) on each instance. 

 * For abstracted services, such as Amazon S3 and Amazon DynamoDB, AWS operates the infrastructure layer, the operating system, and platforms, and customers access the endpoints to store and retrieve data. 

 * Customers are responsible for managing their data (including encryption options), classifying their assets, and using IAM tools to apply the appropriate permissions.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vnc390nfee3ae3id8pai.png)
*Figure 1: AWS Shared Responsibility Model.*

 * This customer/AWS shared responsibility model also extends to IT controls. Just as the responsibility to operate the IT environment is shared between AWS and its customers, so is the management, operation, and verification of IT controls shared. 

 * AWS can help relieve customer burden of operating controls by managing those controls associated with the physical infrastructure deployed in the AWS environment that may previously have been managed by the customer. 

 * As every customer is deployed differently in AWS, customers can take advantage of shifting management of certain IT controls to AWS, which results in a (new) distributed control environment. 

 * Customers can then use the AWS control and compliance documentation available to them to perform their control evaluation and verification procedures as required. The following are examples of controls that are managed by AWS, AWS customers, or both.

 * **Inherited Controls** – Controls that a customer fully inherits from AWS.

     * Physical and Environmental controls

 * **Shared Controls** – Controls that apply to both the infrastructure layer and customer layers, but in separate contexts or perspectives. In a shared control, AWS provides the requirements for the infrastructure and the customer must provide their own control implementation within their use of AWS services. Examples include:

     * **Patch Management** – AWS is responsible for patching and fixing flaws within the infrastructure, but customers are responsible for patching their guest operating system and applications.

     * **Configuration Management** – AWS maintains the configuration of its infrastructure devices, but customers are responsible for configuring their own guest operating systems, databases, and applications.

     * **Awareness and Training** – AWS trains AWS employees, but customers must train their own employees.

 * **Customer Specific** – Controls that are solely the responsibility of the customer based on the application they are deploying within AWS services. Examples include:

* Service and Communications Protection or Zone Security, which might require a customer to route or zone data within specific security environments.

## AWS Response to Abuse and Compromise

* Abuse activities are observed behaviors of AWS customers' instances or other resources that are malicious, offensive, illegal, or could harm other internet sites. AWS works with you to detect and address suspicious and malicious activities from your AWS resources. 

* Unexpected or suspicious behaviors from your resources can indicate that your AWS resources have been compromised, which signals potential risks to your business. 

## Governance

* Security governance, as a subset of the overall approach, is meant to support business objectives by defining policies and control objectives to help manage risk. Achieve risk management by following a layered approach to security control objectives–each layer builds upon the previous one. 

* Understanding the AWS Shared Responsibility Model is your foundational layer. This knowledge provides clarity on what you are responsible for on the customer side and what you inherit from AWS. A beneficial resource is AWS Artifact, which gives you on-demand access to AWS’ security and compliance reports and select online agreements.

## Operating Your Workloads Securely

* Operating workloads securely covers the whole lifecycle of a workload from design, to build, to run, and to ongoing improvement. 

* One of the ways to improve your ability to operate securely in the cloud is by taking an organizational approach to governance. Governance is the way that decisions are guided consistently without depending solely on the good judgment of the people involved. 

* Your governance model and process are the way you answer the question “How do I know that the control objectives for a given workload are met and are appropriate for that workload?” Having a consistent approach to making decisions speeds up the deployment of workloads and helps raise the bar for the security capability in your organization.

### Resources

* Refer to the following resources to [learn more about operating your workload securely](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources.html).

## AWS Account Management and Separation

* We recommend that you organize workloads in separate accounts and group accounts based on function, compliance requirements, or a common set of controls rather than mirroring your organization’s reporting structure. 

* In AWS, accounts are a hard boundary. For example, account-level separation is strongly recommended for isolating production workloads from development and test workloads.

### Resources

* Refer to the following resources to [learn more about AWS recommendations for deploying and managing multiple AWS accounts](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-1.html).

# Identity and Access Management

* To use AWS services, you must grant your users and applications access to resources in your AWS accounts. As you run more workloads on AWS, you need robust identity management and permissions in place to ensure that the right people have access to the right resources under the right conditions. 

* AWS offers a large selection of capabilities to help you manage your human and machine identities and their permissions. The best practices for these capabilities fall into two main areas.

## Identity Management

* There are two types of identities you need to manage when approaching operating secure AWS workloads.

 * **Human Identities**: The administrators, developers, operators, and consumers of your applications require an identity to access your AWS environments and applications. These can be members of your organization, or external users with whom you collaborate, and who interact with your AWS resources via a web browser, client application, mobile app, or interactive command-line tools.

 * **Machine Identities**: Your workload applications, operational tools, and components require an identity to make requests to AWS services, for example, to read data. These identities include machines running in your AWS environment, such as Amazon EC2 instances or AWS Lambda functions. You can also manage machine identities for external parties who need access. Additionally, you might also have machines outside of AWS that need access to your AWS environment.

### Resources

* Refer to the following resources to [learn more about AWS best practices for protecting your AWS credentials](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-2.html).

## Permissions Management

* Manage permissions to control access to human and machine identities that require access to AWS and your workloads. Permissions control who can access what, and under what conditions. Set permissions to specific human and machine identities to grant access to specific service actions on specific resources. 

* Additionally, specify conditions that must be true for access to be granted. For example, you can allow developers to create new Lambda functions, but only in a specific Region. When managing your AWS environments at scale, adhere to the following best practices to ensure that identities only have the access they need and nothing more.

### Resources

* Refer to the following resources to [learn more about current AWS best practices for fine-grained authorization](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-3.html).

# Detection

* Detection consists of two parts: detection of unexpected or unwanted configuration changes, and the detection of unexpected behavior. The first can take place at multiple places in an application delivery lifecycle. 

* Using infrastructure as code (for example, a CloudFormation template), you can check for unwanted configuration before a workload is deployed by implementing checks in the CI/CD pipelines or source control. Then, as you deploy a workload into non-production and production environments, you can check configuration using native AWS, open source, or AWS Partner tools. 

* These checks can be for configuration that does not meet security principles or best practices, or for changes that were made between a tested and deployed configuration. For a running application, you can check whether the configuration has been changed in an unexpected fashion, including outside of a known deployment or automated scaling event.

* In AWS, there are a number of approaches you can use when addressing detective mechanisms. The following sections describe how to use these approaches:
 * Configure
 * Investigate

## Configure

* Configure service and application logging: A foundational practice is to establish a set of detection mechanisms at the account level. This base set of mechanisms is aimed at recording and detecting a wide range of actions on all resources in your account. 

* They allow you to build out a comprehensive detective capability with options that include automated remediation, and partner integrations to add functionality.

### Resources

* Refer to the following resources to [learn more about current AWS recommendations for capturing and analyzing logs](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-4.html).

## Investigate

* **Implement actionable security events**: For each detective mechanism you have, you should also have a process, in the form of a runbook or playbook, to investigate. For example, when you enable Amazon GuardDuty, it generates different findings. You should have a runbook entry for each finding type, for example, if a trojan is discovered, your runbook has simple instructions that instruct someone to investigate and remediate.

* **Automate response to events**: In AWS, investigating events of interest and information on potentially unexpected changes into an automated workflow can be achieved using Amazon EventBridge. This service provides a scalable rules engine designed to broker both native AWS event formats (such as CloudTrail events), as well as custom events you can generate from your application. Amazon GuardDuty also allows you to route events to a workflow system for those building incident response systems (Step Functions), or to a central Security Account, or to a bucket for further analysis.

### Resources

* Refer to the following resources to [learn more about current AWS best practices for integrating auditing controls with notification and workflow](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-5.html).


# Infrastructure Protection

* Infrastructure protection encompasses control methodologies, such as defense in depth, that are necessary to meet best practices and organizational or regulatory obligations. Use of these methodologies is critical for successful, ongoing operations in the cloud.

* Infrastructure protection is a key part of an information security program. It ensures that systems and services within your workload are protected against unintended and unauthorized access, and potential vulnerabilities. 

* For example, you’ll define trust boundaries (for example, network and account boundaries), system security configuration and maintenance (for example, hardening, minimization and patching), operating system authentication and authorizations (for example, users, keys, and access levels), and other appropriate policy-enforcement points (for example, web application firewalls and/or API gateways).

* Regions, Availability Zones, AWS Local Zones, and AWS Outposts

* Make sure you are familiar with Regions, Availability Zones, AWS Local Zones, and AWS Outposts, which are components of the AWS secure global infrastructure.

* In AWS, there are a number of approaches to infrastructure protection. The following sections describe how to use these approaches.
 * Protecting Networks
 * Protecting Compute

## Protecting Networks

* Users, both in your workforce and your customers, can be located anywhere. You need to pivot from traditional models of trusting anyone and anything that has access to your network. When you follow the principle of applying security at all layers, you employ a Zero Trust approach. 

* Zero Trust security is a model where application components or microservices are considered discrete from each other and no component or microservice trusts any other.

* The careful planning and management of your network design forms the foundation of how you provide isolation and boundaries for resources within your workload. Because many resources in your workload operate in a VPC and inherit the security properties, it’s critical that the design is supported with inspection and protection mechanisms backed by automation. 

* Likewise, for workloads that operate outside a VPC, using purely edge services and/or serverless, the best practices apply in a more simplified approach. Refer to the AWS Well-Architected Serverless Applications Lens for specific guidance on serverless security.

### Resources

* Refer to the following resources to [learn more about AWS best practices for protecting networks](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-6.html).

## Protecting Compute

* Compute resources include EC2 instances, containers, AWS Lambda functions, database services, IoT devices, and more. Each of these compute resource types require different approaches to secure them. However, they do share common strategies that you need to consider: defense in depth, vulnerability management, reduction in attack surface, automation of configuration and operation, and performing actions at a distance. 

* In this section, you will find general guidance for protecting your compute resources for key services. For each AWS service used, it’s important for you to check the specific security recommendations in the service documentation.

 * **Perform vulnerability management**: Frequently scan and patch for vulnerabilities in your code, dependencies, and in your infrastructure to help protect against new threats.

 * **Reduce attack surface: Reduce your exposure to unintended access by hardening operating systems and minimizing the components, libraries, and externally consumable services in use. Start by reducing unused components, whether they are operating system packages, applications, etc. (for EC2-based workloads) or external software modules in your code (for all workloads). 

 * **Enable people to perform actions at a distance**: Removing the ability for interactive access reduces the risk of human error, and the potential for manual configuration or management. For example, use a change management workflow to manage EC2 instances using tools such as AWS Systems Manager instead of allowing direct access, or via a bastion host. 

 * **Implement managed services**: Implement services that manage resources, such as Amazon RDS, AWS Lambda, and Amazon ECS, to reduce your security maintenance tasks as part of the shared responsibility model. For example, Amazon RDS helps you set up, operate, and scale a relational database, automates administration tasks such as hardware provisioning, database setup, patching, and backups. 

 * **Validate software integrity**: Implement mechanisms (e.g. code signing) to validate that the software, code and libraries used in the workload are from trusted sources and have not been tampered with. For example, you should verify the code signing certificate of binaries and scripts to confirm the author, and ensure it has not been tampered with since created by the author. 

 * **Automate compute protection**: Automate your protective compute mechanisms including vulnerability management, reduction in attack surface, and management of resources. The automation will help you invest time in securing other aspects of your workload, and reduce the risk of human error.

### Resources

* Refer to the following resources to [learn more about AWS best practices for protecting compute](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-7.html).

# Data Protection

* Before architecting any workload, foundational practices that influence security should be in place. For example, data classification provides a way to categorize data based on levels of sensitivity, and encryption protects data by way of rendering it unintelligible to unauthorized access. 

* These methods are important because they support objectives such as preventing mishandling or complying with regulatory obligations.

* In AWS, there are a number of different approaches you can use when addressing data protection. The following section describes how to use these approaches.

 * Data Classification
 * Protecting Data at Rest
 * Protecting Data in Transit

## Data Classification

* Data classification provides a way to categorize organizational data based on criticality and sensitivity in order to help you determine appropriate protection and retention controls.

 * **Identify the data within your workload**: You need to understand the type and classification of data your workload is processing, the associated business processes, data owner, applicable legal and compliance requirements, where it’s stored, and the resulting controls that are needed to be enforced. 

 * **Define data protection controls**: By using resource tags, separate AWS accounts per sensitivity (and potentially also per caveat / enclave / community of interest), IAM policies, Organizations SCPs, AWS KMS, and AWS CloudHSM, you can define and implement your policies for data classification and protection with encryption. 

 * **Define data lifecycle management**: Your defined lifecycle strategy should be based on sensitivity level as well as legal and organization requirements. Aspects including the duration for which you retain data, data destruction processes, data access management, data transformation, and data sharing should be considered. 

 * **Automate identification and classification**: Automating the identification and classification of data can help you implement the correct controls. Using automation for this instead of direct access from a person reduces the risk of human error and exposure. You should evaluate using a tool, such as Amazon Macie, that uses machine learning to automatically discover, classify, and protect sensitive data in AWS. 

### Resources
Refer to the following resources to [learn more about data classification](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-8.html).

## Protecting Data at Rest

Data at rest represents any data that you persist in non-volatile storage for any duration in your workload. This includes block storage, object storage, databases, archives, IoT devices, and any other storage medium on which data is persisted. Protecting your data at rest reduces the risk of unauthorized access, when encryption and appropriate access controls are implemented.

Encryption and tokenization are two important but distinct data protection schemes.

> Tokenization is a process that allows you to define a token to represent an otherwise sensitive piece of information (for example, a token to represent a customer’s credit card number). A token must be meaningless on its own, and must not be derived from the data it is tokenizing–therefore, a cryptographic digest is not usable as a token. By carefully planning your tokenization approach, you can provide additional protection for your content, and you can ensure that you meet your compliance requirements. For example, you can reduce the compliance scope of a credit card processing system if you leverage a token instead of a credit card number.

> Encryption is a way of transforming content in a manner that makes it unreadable without a secret key necessary to decrypt the content back into plaintext. Both tokenization and encryption can be used to secure and protect information as appropriate. Further, masking is a technique that allows part of a piece of data to be redacted to a point where the remaining data is not considered sensitive. For example, PCI-DSS allows the last four digits of a card number to be retained outside the compliance scope boundary for indexing.

 * **Implement secure key management**: By defining an encryption approach that includes the storage, rotation, and access control of keys, you can help provide protection for your content against unauthorized users and against unnecessary exposure to authorized users. AWS KMS helps you manage encryption keys and integrates with many AWS services. 

 * **Enforce encryption at rest**: You should ensure that the only way to store data is by using encryption. AWS KMS integrates seamlessly with many AWS services to make it easier for you to encrypt all your data at rest. For example, in Amazon S3 you can set default encryption on a bucket so that all new objects are automatically encrypted. 

 * **Enforce access control**: Different controls including access (using least privilege), backups (see Reliability whitepaper), isolation, and versioning can all help protect your data at rest. Access to your data should be audited using detective mechanisms covered earlier in this paper including CloudTrail, and service level log, such as S3 access logs. You should inventory what data is publicly accessible, and plan for how you can reduce the amount of data available over time. 

 * **Audit the use of encryption keys**: Ensure that you understand and audit the use of encryption keys to validate that the access control mechanisms on the keys are appropriately implemented. For example, any AWS service using an AWS KMS key logs each use in AWS CloudTrail. 

 * **Use mechanisms to keep people away from data**: Keep all users away from directly accessing sensitive data and systems under normal operational circumstances. For example, use a change management workflow to manage EC2 instances using tools instead of allowing direct access or a bastion host. This can be achieved using AWS Systems Manager Automation, which uses automation documents that contain steps you use to perform tasks. 

 * **Automate data at rest protection**: Use automated tools to validate and enforce data at rest controls continuously, for example, verify that there are only encrypted storage resources. You can automate validation that all EBS volumes are encrypted using AWS Config Rules. 

### Resources
Refer to the following resources to [learn more about AWS best practices for protecting data at rest](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-9.html).

## Protecting Data in Transit

* Data in transit is any data that is sent from one system to another. This includes communication between resources within your workload as well as communication between other services and your end users. By providing the appropriate level of protection for your data in transit, you protect the confidentiality and integrity of your workload’s data.

 * **Implement secure key and certificate management**: Store encryption keys and certificates securely and rotate them at appropriate time intervals with strict access control. The best way to accomplish this is to use a managed service, such as AWS Certificate Manager (ACM).  

 * **Enforce encryption in transit**: Enforce your defined encryption requirements based on appropriate standards and recommendations to help you meet your organizational, legal, and compliance requirements. AWS services provide HTTPS endpoints using TLS for communication, thus providing encryption in transit when communicating with the AWS APIs. 

 * **Authenticate network communications**: Using network protocols that support authentication allows for trust to be established between the parties. This adds to the encryption used in the protocol to reduce the risk of communications being altered or intercepted. Common protocols that implement authentication include Transport Layer Security (TLS), which is used in many AWS services, and IPsec, which is used in AWS Virtual Private Network (AWS VPN).

 * **Automate detection of unintended data access**: Use tools such as Amazon GuardDuty to automatically detect suspicious activity or attempts to move data outside of defined boundaries. For example, GuardDuty can detect S3 read activity that is unusual with the Exfiltration:S3/ObjectRead.Unusual finding. 

### Resources
Refer to the following resources to [learn more about AWS best practices for protecting data in transit](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-10.html).

# Incident Response

* Even with mature preventive and detective controls, your organization should implement mechanisms to respond to and mitigate the potential impact of security incidents. Your preparation strongly affects the ability of your teams to operate effectively during an incident, to isolate, contain and perform forensics on issues, and to restore operations to a known good state. 

* Putting in place the tools and access ahead of a security incident, then routinely practicing incident response through game days, helps ensure that you can recover while minimizing business disruption.

 * Design Goals of Cloud Response
 * Educate
 * Prepare
 * Simulate
 * Iterate
 * Resources

## Design Goals of Cloud Response

* **Establish response objectives**: Work with your stakeholders, legal counsel, and organizational leadership to determine the goal of responding to an incident. Some common goals include containing and mitigating the issue, recovering the affected resources, preserving data for forensics, and attribution.

* **Document plans**: Create plans to help you respond to, communicate during, and recover from an incident.

* **Respond using the cloud**: Implement your response patterns where the event and data occurs.

* **Know what you have and what you need**: Preserve logs, snapshots, and other evidence by copying them to a centralized security cloud account. Use tags, metadata, and mechanisms that enforce retention policies. For example, you might choose to use the Linux dd command or a Windows equivalent to make a complete copy of the data for investigative purposes.

* **Use redeployment mechanisms**: If a security anomaly can be attributed to a misconfiguration, the remediation might be as simple as removing the variance by redeploying the resources with the proper configuration. When possible, make your response mechanisms safe to execute more than once and in environments in an unknown state.

* **Automate where possible**: As you see issues or incidents repeat, build mechanisms that programmatically triage and respond to common situations. Use human responses for unique, new, and sensitive incidents.

* **Choose scalable solutions**: Strive to match the scalability of your organization's approach to cloud computing, and reduce the time between detection and response.

* **Learn and improve your process**: When you identify gaps in your process, tools, or people, implement plans to fix them. Simulations are safe methods to find gaps and improve processes.

In AWS, there are a number of different approaches you can use when addressing incident response. The following section describes how to use these approaches:

**Educate** your security operations and incident response staff about cloud technologies and how your organization intends to use them.

**Prepare** your incident response team to detect and respond to incidents in the cloud, enable detective capabilities, and ensure appropriate access to the necessary tools and cloud services. Additionally, prepare the necessary runbooks, both manual and automated, to ensure reliable and consistent responses. Work with other teams to establish expected baseline operations, and use that knowledge to identify deviations from those normal operations.

**Simulate** both expected and unexpected security events within your cloud environment to understand the effectiveness of your preparation.

**Iterate** on the outcome of your simulation to improve the scale of your response posture, reduce time to value, and further reduce risk.

## Resources

Refer to the following resources to [learn more about current AWS best practices for incident response](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/resources-11.html).

# Conclusion

* Security is an ongoing effort. When incidents occur, they should be treated as opportunities to improve the security of the architecture. Having strong identity controls, automating responses to security events, protecting infrastructure at multiple levels, and managing well-classified data with encryption provides defense in depth that every organization should implement. This effort is easier thanks to the programmatic functions and AWS features and services discussed in this paper.

* AWS strives to help you build and operate architectures that protect information, systems, and assets while delivering business value.

# Reference

[Original paper](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)