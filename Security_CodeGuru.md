
# Security in Amazon CodeGuru Profiler
   AWS_WhitePaper_Summary

- Amazon CodeGuru is a developer tool that provides recommendations to improve code quality and identify an application’s most expensive lines of code. 
- Customers can integrate CodeGuru into existing software development workflows to automate code reviews during application development. 
- Continuously monitor application performance in production. 
- It uses machine learning to identify critical issues, security vulnerabilities, and hard-to-find bugs during application development to improve code quality.
- It applies the Shared Responsibilty Principle in Security. 
- AWS is responsible for managing CodeGuru patching and maintenance of the underlying compute functions. 
- Also, managing service availability, and data encryption in transit and at rest.


### Profiler high-level flow is:
 1. The profiler agent is instantiated (either within your application or via the CLI)
 2. The agent begins sampling data to send to the service at five-minute intervals
 3. CodeGuru profiler analyzes this data to generate visualizations and actionable recommendations


### Data Captured 
- The CodeGuru Profiler agent is responsible for collecting data from a local instance, and sending it to the service endpoint for analysis.
- It does this by using language appropriate mechanisms in either the Java Virtual Machine (JVM), or through Python interfaces.
- The profiler agent does not collect or store your applications source code in any way.
-There are only two types of data the tool collects to send to the profiler service – stack traces and heap summaries.

#### Stack Traces
- A stack trace is a sequence of function or method names in execution. 
- It does not have access to the names or values of function parameters, or the values of variables or application data.
- It sends only method names, and profiling statistics such as CPU, memory, etc. 
- It detects anomalies to recommend improvements in code quality. 

#### Heap summary (JVM only) 
- Heap memory is allocated to JVM and is shared by all threads in the application.
- The profiling agent can collect information about objects in the heap over time and securely share this with the service for analysis.
- Heap summary offers a consolidated view of memory use per object type (e.g. String, int, char[]), and custom types, during a given time frame (usually five minutes). 
- The data is sampled approximately in line with when a full garbage collection is done, and is sent to the CodeGuru Profiler service every 5 minutes. 


### Encryption of Data in Transit
-  When the profiling agent communicates with the CodeGuru Profiler service, all communication is secured with TLS connections. 
-  All CodeGuru Profiler endpoints are secured with SHA-256 certificates. 


### Customer Configurable Security in CodeGuru Profiler
- The profiling agent will operate within your compute instances - such as Amazon Elastic Compute Cloud (Amazon EC2) instances, containers running in Amazon Elastic Container Service (Amazon ECS) or Amazon Elastic Kubernetes Service (Amazon EKS), or serverless functions in AWS Lambda.

- You should ensure that appropriate network security controls are implemented to meet your specific security requirements that relate to how the agents can communicate with the CodeGuru service.

- These could include controls such as the use of VPC endpoints, firewalls or security groups, and other such mechanisms specific to your use cases and network configuration. 


### Considerations for using the profiler agent on-premises
- Customers can choose to enable the CodeGuru Profiler service to instrument applications running outside of their AWS accounts. 
- Agent will run in your on-premises servers and will continue to post data to the AWS account where your CodeGuru profiling group exists. 
- You should consider networking between your application servers and AWS and authentication credentials through direct internet access, VPN and AWS Direct Connect. 

### Using AWS Config for Security 
- AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources.
- It monitors and records your AWS resource configurations and provides you with the ability to define rules for provisioning & configuring resources
- Currently there are no AWS Config Managed Rules for events related to CodeGuru, but you can use Rule Development Kit (RDK) to develop a rule for tracking and alerting in CodeGuru Profiler. 

### Access management
- CodeGuru Profiler service is integrated with IAM to allow for permissions control of CodeGuru Profiler resources.
- IAM provides two policy types for resource access authorization:
  1. Resource policies
  2. Identity-based policies

- CodeGuru Profiler provides two managed policies:
  1. AmazonCodeGuruProfilerReadOnlyAccess
  2. AmazonCodeGuruProfilerFullAccess
  
### Policy Best Practices
- Get started using AWS managed policies 
- Grant least privilege
- Enable MFA for sensitive operations
- Use policy conditions for extra security 

#### Permissions required by the CodeGuru Profiler profiling agent 
- The following permissions are required to submit data to CodeGuru Profiler:  
   1. codeguru-profiler:ConfigureAgent
   2. codeguru-profiler:PostAgentProfile 
  
```py
{  "Statement": [{
     "Effect": "Allow",
     "Action": [
       "codeguru-profiler:ConfigureAgent",
       "codeguru-profiler:PostAgentProfile"
     ],
     "Resource": "arn:aws:codeguru-profiler:<region>:<accountID>:profilingGroup/profilingGroupName"
  }]
}

```

#### Permissions required to access CodeGuru Profiler data 
- Data collected and submitted to CodeGuru Profiler by an agent is used to create application profiles for visualizations: 
   1. codeguru-profiler:GetProfile
   2. codeguru-profiler:DescribeProfilingGroup 
 
```py
{"Statement": [{
    "Effect": "Allow",
    "Action": [
      "codeguru-profiler:GetProfile",
      "codeguru-profiler:DescribeProfilingGroup"
    ],
    "Resource": "arn:aws:codeguru-profiler:<region>:<accountID>:profilingGroup/profilingGroupName"
 }]
}
```

#### Service-linked roles for CodeGuru Profiler
- CodeGuru Profiler uses AWS Identity and Access Management (IAM) service-linked roles. 
- Service-linked roles are predefined by CodeGuru and include all the permissions that the service requires to call other AWS services on your behalf like following: 

```py
{ "Version": "2012-10-17",
  "Statement": [{
     "Sid": "AllowSNSPublishToSendNotifications",
     "Effect": "Allow",
     "Action": [ "sns:Publish" ],
     "Resource": "*"
  }]
}

```


### Logging
- CodeGuru Profiler is integrated with AWS CloudTrail.
- By enabling CloudTrail in the AWS Management Console, you can receive a history of CodeGuru API calls made on your account.

### Monitoring
- You can use Amazon CloudWatch to monitor the number of recommendations created for your source code for an associated profiling group.
- You can set a CloudWatch alarm that notifies you when the number of recommendations exceeds a threshold you set.
- You can specify that an Amazon SNS notification is sent when more than 25 recommendations are generated for a branch in a repository within an hour. 


