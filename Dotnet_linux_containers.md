# Modernize .NET Applications with Linux Containers

- Since the introduction of .NET Framework in 2002, more than six million developers have adopted the .NET programming ecosystem to build their applications.  
- In its initial form, .NET was built to run exclusively on the Windows operating system. 

- This resulted in large portfolios of applications running on .NET and Windows, particularly in the enterprise.
- Many organizations simultaneously want to move their VM-based deployments to containers, to predictably deploy their applications across environments, maximize the efficiency of their resource consumption, and introduce DevOps practices to automate their development lifecycle.

- Gartner predicts that by 2022, more than 75% of global organizations will be running containerized applications in production, up from less than 30% in 2020. 
- An IDC survey found that 45% of respondents’ application portfolio is running in containers today, and that is expected to increase to 60% in three years

- This paper walks through this use case of modernizing a .NET Framework application running on Windows VMs to .NET 5, and Linux containers running on Amazon Elastic Container Service (Amazon ECS) and AWS Fargate.
- This guide uses AWS Fargate for ECS to deploy containerized .NET Framework applications on AWS.


## Choosing container orchestration

- When choosing your container orchestration option, you should start with the question, “How much of the container infrastructure do I want to manage?” The following options are available to you:

- Self-Managed Containers on Amazon Elastic Compute Cloud (Amazon EC2)
- Amazon Elastic Container Service 
- Amazon Elastic Kubernetes Service 
- AWS Fargate

## Cost considerations

### Cloud Computing 
- Cloud computing helps businesses reduce costs and complexity, adjust capacity on-demand, accelerate time-to-market, increase opportunities for innovation, and enhance security.
- Weighing the financial considerations of operating an on-premises data center versus using cloud infrastructure is not as simple as comparing hardware, storage, and compute costs.

### AWS pricing model
- AWS offers a simple, consistent, pay-as-you-go pricing model, so you are charged only for the resources you consume.
- With this model, there are no upfront fees, no minimum commitments, and no long-term contracts required.
- There is also flexibility to choose the pricing model that best fits your needs if the pay-as-you-go model is not optimal for your use case.
- Short descriptions of all of these pricing models are found below.
    
    - On-Demand Instance — With On-Demand Instances, you pay for compute capacity by the hour, with no minimum commitments required.
    - Reserved Instance — For longer-term savings, you can purchase in advance. 
    
    - In addition to providing a significant discount (up to 60 percent) compared to On-Demand Instance pricing, Reserved Instances allow you to reserve capacity.
    - Spot Instance — You can request unused Amazon Elastic Compute Cloud (Amazon EC2) capacity. 
    - Instances are charged the Spot Price, which is set by Amazon EC2 and fluctuates, depending on supply and demand. 

![image](https://user-images.githubusercontent.com/23625821/131245382-1b1f9bc9-c7c2-4e7d-9b4c-2118a4209939.png)

### Summary by service and operating system
- The following table represents the summary of running the preceding application on each service, and compares the monthly cost of running on Windows versus Linux.
- These figures include the cost of compute, the Amazon VPC, and ELB. 
- Running on Linux costs less than running on Windows for each service.

![1](https://user-images.githubusercontent.com/23625821/131245526-e43511a1-117b-4443-8db2-38ef235b0a07.png)

- You can find more details <a href="https://calculator.aws/#/"> here </a> : 

#### AWS Fargate pricing
- Windows containers are not supported to run on AWS Fargate at this time.

![1](https://user-images.githubusercontent.com/23625821/131245653-0d54e740-469a-486e-8da6-39b6967bf7d8.png)

## Architecture overview

![image](https://user-images.githubusercontent.com/23625821/131450846-0e662d1b-c39a-4990-a17e-ce11db94b14e.png)

![image](https://user-images.githubusercontent.com/23625821/131450978-0d2123ab-bec8-4128-9524-04deb699743e.png)

![image](https://user-images.githubusercontent.com/23625821/131451141-93794e14-051f-49ce-9db2-17ba6a96e3fa.png)


## Replatforming from Windows VMs to Linux containers

- Prerequisites
    - Install Docker. See Docker Desktop.
    - Install the AWS CDK. See AWS CDK Toolkit ( cdk command).
    - Install the AWS CLI. See AWS Command Line Interface.

### Containerize the application
- Before you deploy the application to AWS, you will containerize it locally and ensure that it builds.
- Create a file with the following content and name it Dockerfile . Save it in the same folder as the MvcMusicStore.csproj file.

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["MvcMusicStore.csproj", ""]
RUN dotnet restore "./MvcMusicStore.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "MvcMusicStore.csproj" -c Release -o /app/build
FROM build AS publish
RUN dotnet publish "MvcMusicStore.csproj" -c Release -o
/app/publish
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MvcMusicStore.dll"]

```

- Ensure Docker is started locally, and from the command line, set the working directory to the folder containing the above Dockerfile.
- Run the following command to ensure that the application container image builds successfully.

```sh
docker build . -t music-store

```

Run the docker images command from the command line. You should see a newly created mvcmusicstore Docker image in the output:

```sh
REPOSITORY
music-store
TAG
latest
IMAGE ID
19fa5c6fb0c3
CREATED
2 minutes ago
SIZE
233MB

```

### Create the infrastructure as code

- There are various ways to create a Fargate service, such as using AWS Management Console, AWS CLI , AWS CloudFormation template, and AWS Cloud Development Kit (AWS CDK). 
- AWS CDK is an open-source software development framework to define your cloud application resources using familiar programming languages. 

### Logging and monitoring

- Monitoring is an important part of maintaining the reliability, availability, and performance of your applications running on Amazon ECS. 
- You should collect monitoring data from all of the parts of your AWS stack so that you can more easily debug a multi-point failure if one occurs. 
- AWS provides several tools for monitoring your Amazon ECS resources and responding for potential incidents.

![1](https://user-images.githubusercontent.com/23625821/131959081-7e0fa10b-e0ee-4bd9-9b88-dfc09522423a.png)

### Security
- Shared responsibility model with the customer: AWS manages and controls the components from the host operating system and virtualization layer down to the physical security of the facilities in which the services operate, and AWS customers are responsible for building secure applications.

#### User to application authentication and authorization
- Amazon also provides an ASP.NET Core Identity Provider for Amazon Cognito, which allows ASP.NET Core applications to easily integrate with Amazon Cognito in their web applications for user authentication and authorization.
- The following integrations are also supported:
    - User authentication through an identity provider (IdP) that is OpenID Connect (OICD) compliant
    - User authentication through well-known IdPs such as Amazon, Facebook, and Google through Amazon Cognito user pools
    - User authentication through corporate identities using SAML, LDAP, or Microsoft Active Directory through Amazon Cognito user pools

#### Application to database authentication and authorization
- Amazon RDS supports both Windows and SQL Server authentication modes when connecting an application to the database.

##### Windows Authentication
- Windows (or Integrated) Authentication is the common mechanism for clients and applications to connect to SQL Server databases.
- The best practice is to use a separate Amazon ECS task as a ticket renewal “sidecar,” which stores the Kerberos ticket in Fargate task storage for the application, which reads the ticket from Fargate task storage and connects to the database using Windows Authentication. 

![image](https://user-images.githubusercontent.com/23625821/132082682-b9fc9ff0-2c39-443b-a274-105ea51f3a4b.png)

### Identity and access management for Amazon ECS

![image](https://user-images.githubusercontent.com/23625821/132116549-2e140581-eba4-4505-8ce1-d270d5e59a84.png)

#### Service-linked role

- There are multiple activities that the Amazon ECS service runs as it orchestrates your container workloads. 

- Amazon ECS uses a service-linked role for the permissions it requires to call other AWS services on your behalf. 

- These include services like Amazon EC2 to manage elastic network interfaces, ELB to manage targets, and Amazon Route 53 for creating health checks, amongst others


![image](https://user-images.githubusercontent.com/23625821/132116576-b1c2ff77-a407-429f-b954-62ac7a5d7381.png)

#### Container Instance role

![image](https://user-images.githubusercontent.com/23625821/132116585-6e6a920e-4730-4173-a332-7078fb3242b9.png)

#### Task execution role
- The task execution role grants the Amazon ECS container agent permission to make AWS API calls on your behalf when an Amazon ECS task is started.

![image](https://user-images.githubusercontent.com/23625821/132116596-b91f328c-2d78-425d-a7cd-eecef7d88ae7.png)

#### Task role

![image](https://user-images.githubusercontent.com/23625821/132116616-8e8da03e-d5d5-47ba-b161-1b13774a53ce.png)


<a href="https://d1.awsstatic.com/whitepapers/modernize-dotnet-apps-with-linux-containers.pdf?did=wp_card&trk=wp_card"> Original paper </a>

