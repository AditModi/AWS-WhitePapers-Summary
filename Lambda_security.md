# Security Overview of AWS Lambda

 #AWS_WhitePaper_Summary 

- The managed runtime environment model further reduces the attack surface while making cloud security simpler. 
- This whitepaper presents the underpinnings of that model, along with best practices, to developers, security analysts, security and compliance teams, and other stakeholders.

## The Shared Responsibility Model
- Security and Compliance is a shared responsibility between AWS and the customer. 
- This shared responsibility model can help relieve your operational burden, as AWS operates, manages, and controls the components from the host operating system and virtualization layer, down to the physical security of the facilities in which the service operates.

![1](https://user-images.githubusercontent.com/23625821/127966363-024e9080-3a6c-4d36-ae80-4652256a5999.png)

## Lambda Functions and Layers
- With Lambda, you can run code virtually with zero administration of the underlying infrastructure.
- You are responsible only for the code that you provide Lambda, and the conﬁguration of how Lambda runs that code on your behalf.
- Today, Lambda supports two types of code resources: Functions and Layers.
- Layers can be used to share common code or data across diﬀerent functions or AWS accounts. 
- You can also control the entire lifecycle of your functions and layers through Lambda's control plane APIs. 
- For example, you can choose to delete your function by calling DeleteFunction, or revoke permissions from another account by calling RemovePermission.

## Lambda Invoke Modes
- The Invoke API can be called in two modes: event mode and request-response mode. 
  - Event mode queues the payload for an asynchronous invocation.
  - Request-response mode synchronously invokes the function with the provided payload and returns a response immediately.
  - In both cases, the function execution is always performed in a Lambda execution environment, but the payload takes diﬀerent paths.
  
 
![image](https://user-images.githubusercontent.com/23625821/128178489-e7c349a8-64e0-48c9-a18e-9588dbeb7729.png)
  
  - Eventinvocation mode payloads are always queued for processing before invocation.
  - All payloads are queued for processing in an Amazon Simple Queue Service (Amazon SQS) queue. 
  - Queued events are always secured in-transit with TLS 1.2+, but they are not currently encrypted at-rest.
  - Queued events are retrieved in batches by Lambda’s poller ﬂeet. 
  - The poller ﬂeet is a group of EC2 instances whose purpose is to process queued event invocations which have not yet been processed.
  
  - If the invocation cannot be performed, the poller ﬂeet will temporarily store the event, in-memory, on the host until it is either able to successfully              complete the execution, or until the number of run retry attempts have been exceeded.
  
## Lambda Executions
- The Lambda service is split into the control plane and the data plane. 
- Each plane serves a distinct purpose in the service. 
- The control plane provides the management APIs (for example, CreateFunction, UpdateFunctionCode, PublishLayerVersion, and so on), and manages integrations with all AWS services. 
- Communications to Lambda's control plane are protected in-transit by TLS. 
- All customer data stored within Lambda's control plane is encrypted at-rest through the use of AWS KMS, which is designed to protect it from unauthorized disclosure or tampering.
- The data plane is Lambda's Invoke API that triggers the invocation of Lambda functions. 
- When a Lambda function is invoked, the data plane allocates an execution environmenton an AWS Lambda Worker (or simply Worker, a type of Amazon EC2 instance) to that function version, or chooses an existing execution environment that has already been set up for that function version, which it then uses to complete the invocation.

### Lambda execution environments
- Each invocation is routed by Lambda's invoke service to an execution environment on a Worker that is able to service the request. 
- Other than through data plane, customers and other users cannot directly initiate inbound/ingress network communications with an execution environment. 
- This helps to ensure that communications to your execution environment are authenticated and authorized.
- Each execution environment may only be used for one concurrent invocation at a time, and they may be reused across multiple invocations of the same function version for performance reasons.

### Execution role
- Each Lambda function must also be conﬁgured with an execution role, which is an IAM role that is assumed by the Lambda service when performing control plane and data plane operations related to the function.

### Lambda MicroVMs and Workers
- Lambda will create its execution environments on a ﬂeet of Amazon EC2 instances called AWS Lambda Workers. 
- Workers are bare metalEC2 Nitro instances which are launched and managed by Lambda in a separate isolated AWS account which is not visible to customers.
- Workers have one or more hardware-virtualized Micro Virtual Machines (MVM) created by Firecracker. 
- Firecracker is an open-source Virtual Machine Monitor (VMM) that uses Linux’s Kernel-based Virtual Machine (KVM) to create and manage MVMs. 
- It is purpose-built for creating and managing secure, multi-tenant container and function-based services that provide serverless operational models.
- As a part of the shared responsibility model, Lambda is responsible for maintaining the security conﬁguration, controls, and patching level of the Workers. 
- The Lambda team uses Amazon Inspector to discover known potential security issues, as well as other custom security issue notiﬁcation mechanisms and pre-disclosure lists, so that customers don’t need to manage the underlying security posture of their execution environment.

![image](https://user-images.githubusercontent.com/23625821/128300028-bb770510-8453-41b0-9220-22946dadd39e.png)

### Lambda Isolation Technologies
- Lambda uses a variety of open-source and proprietary isolation technologies to protect Workers and execution environments. 
- Each execution environment contains a dedicated copy of the following items:
   - The code of the particular function version
   - Any AWS Lambda Layers selected for your function version
   - The chosen function runtime (for example, Java 11, NodeJS 12, Python 3.8, and so on) or the function's custom runtime
   - A writeable /tmp directory
   - A minimal Linux user space based on Amazon Linux 2

### Storage and state
- Execution environments are never reused across diﬀerent function versions or customers. 
- But a single environment can be reused between invocations of the same function version.
- Customers that want to persist data to the ﬁle system outside of the execution environment should consider using Lambda’s integration with Amazon EFS.

### Runtime Maintenance in Lambda
- Lambda tests and deploys the runtime updates without any involvement from customers.
- Typically, no action is required to pick up the latest patches for supported Lambda runtimes, but sometimes action might be required to test patches before they are deployed (for example, known incompatible runtime patches).
- The Lambda team uses Amazon Inspector to discover known security issues. 

### Monitoring and Auditing Lambda Functions
 - Cloudwatch 
 - X-Ray
 - Cloudtrail
 - Config 
 
### Architecting and Operating Lambda Functions
- This section relates to the best practices of well designing and architecting Lambda Functions according to the 5 principles of Well-Architecting Framework. 
   - Operational Excellence Pillar 
   - Security Pillar 
   - Reliability Pillar  
   - Performance Eﬃciency Pillar 
   - Cost Optimization Pillar 
   

### Lambda and Compliance
- You are responsible for determining which compliance regime applies to your data. 
- After you have determined your compliance regime needs, you can use the various Lambda features to match those controls.
- You can contact AWS experts (such as Solution Architects, domain experts, technical account managers and other human resources) for assistance.


### Lambda Event Sources
- Lambda integrates with more than 140 AWS services via direct integration and the Amazon EventBridge event bus.


## References

- <a href="https://docs.aws.amazon.com/whitepapers/latest/security-overview-aws-lambda/security-overview-aws-lambda.pdf"> Original White Paper </a>

