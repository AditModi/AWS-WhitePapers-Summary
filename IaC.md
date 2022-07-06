# Infrastructure as code

- Infrastructure as Code has emerged as a best practice for automating the provisioning of infrastructure services. 
- This paper describes the benefits of Infrastructure as Code.
- And how to leverage the capabilities of AWS in this realm to support DevOps initiatives.


## Introduction to Infrastructure as Code

- Infrastructure management is a process associated with software engineering.
- Organizations have traditionally “racked and stacked” hardware, and then installed and configured operating systems and applications to support their technology needs. 
- Cloud computing takes advantage of virtualization to enable the on-demand provisioning of compute, network, and storage resources that constitute technology infrastructures.

- Infrastructure managers have often performed such provisioning manually. 
- The manual processes have certain disadvantages, including:
    
     - Higher cost because they require human capital that could otherwise go toward more important business needs.
     - Inconsistency due to human error, leading to deviations from configuration standards.
    
     - Lack of agility by limiting the speed at which your organization can release new versions of services in response to customer needs and market drivers.
     - Difficulty in attaining and maintaining compliance to corporate or industry standards due to the absence of repeatable processes.


## The Infrastructure Resource Lifecycle

![image](https://user-images.githubusercontent.com/23625821/133917214-9c20040a-f476-4857-8aa8-2ca9fec2e7f9.png)

1. Resource provisioning. Administrators provision the resources according to the specifications they want.
2. Configuration management. The resources become components of a configuration management system that supports activities such as tuning and patching.

3. Monitoring and performance. Monitoring and performance tools validate the operational status of the resources by examining items such as metrics, synthetic transactions, and log files.
4. Compliance and governance. Compliance and governance frameworks drive additional validation to ensure alignment with corporate and industry standards, as well as regulatory requirements.

5. Resource optimization. Administrators review performance data and identify changes needed to optimize the environment around criteria such as performance and cost management.

### AWS CloudFormation
AWS CloudFormation gives developers and systems administrators an easy way to create, manage, provision, and update a collection of related AWS resources in an orderly and predictable way. AWS CloudFormation uses templates written in JSON or YAML format to describe the collection of AWS resources (known as a stack), their associated dependencies, and any required runtime parameters. You can use a template repeatedly to create identical copies of the same stack consistently across AWS Regions.

#### Template anatomy

```yaml
---
AWSTemplateFormatVersion: "version date"
    Description:
        String
    Parameters:
        set of parameters
    Mappings:
        set of mappings
    Conditions:
        set of conditions
    Transform:
        set of transforms
    Resources:
        set of resources
    Outputs:
        set of outputs

```


![1](https://user-images.githubusercontent.com/23625821/134113464-e3422ed2-8465-4787-b857-c3459c37000f.png)

#### Change Sets

- You can update AWS CloudFormation templates with application source code to add, modify, or delete stack resources. 
- The change sets feature enables you to preview proposed changes to a stack without performing the associated updates.
- You can control the ability to create and view change sets using AWS IAM.
- The change sets capability allows you to go beyond version control in AWS CloudFormation by enabling you to keep track of what will actually change from one version to the next. 
- Developers and administrators can gain more insight into the impact of changes before promoting them and minimize the risk of introducing errors.

#### Reusable Templates

- When designing the architecture of your AWS CloudFormation stacks, you can group the stacks logically by function. 
- Instead of creating a single template that includes all the resources you need, such as virtual private clouds (VPCs), subnets, and security groups, you can use nested stacks or cross-stack references. 

- The nested stack feature allows you to create a new AWS CloudFormation stack resource within an AWS CloudFormation template and establish a parent-child relationship between the two stacks. 
- Each time you create an AWS CloudFormation stack from the parent template, AWS CloudFormation also creates a new child stack. 

- Cross-stack references enable an AWS CloudFormation stack to export values that other AWS CloudFormation stacks can then import. 
- Cross-stack references promote a service-oriented model with loose coupling that allows you to share a single set of resources across multiple projects.

#### Template Linting

- The goal of linting is to determine whether the code is syntactically correct, identify potential errors, and evaluate adherence to specific style guidelines. - - In AWS CloudFormation, linting validates that a template is correctly written in either JSON or YAML.

```sh
aws cloudformation validate-template --template-url s3://examplebucket/example_template.template
```


## Amazon EC2 Systems Manager

- It is a collection of capabilities that simplifies common maintenance, management, deployment, and execution of operational tasks on EC2 instances and servers or virtual machines (VMs) in on-premises environments. 

- It helps you easily understand and control the current state of your EC2 instance and OS configurations. 
- You can track and remotely manage system configuration, OS patch levels, application configurations, and other details about deployments as they occur over time.


![1](https://user-images.githubusercontent.com/23625821/134293188-cda39ce9-6599-4029-9456-67201d5f3a9b.png)


### Document structure

- The following is an example of a custom document for a Windows-based host. 
- The document uses the ipconfig command to gather the network configuration of the node and then installs MySQL.


```yaml

{
    "schemaVersion": "2.0",
    "description": "Sample version 2.0 document v2",
    "parameters": {},
    
    "mainSteps": [
            {
                "action": "aws:runPowerShellScript",
                "name": "runShellScript",
                "inputs": {
                "runCommand": ["ipconfig"]
            }
           },
           {
                "action": "aws:applications",
                "name": "installapp",
                "inputs": {
                "action": "Install",
                "source":
                "http://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.6.22.0.msi"
          }
        }
    ]
}


```


## AWS OpsWorks for Chef Automate

- AWS OpsWorks for Chef Automate brings the capabilities of Chef, a configuration management platform, to AWS. OpsWorks for Chef Automate further builds on Chef’s capabilities by providing additional features that support DevOps capabilities at scale.

- OpsWorks for Chef Automate expands the capabilities of Chef to enable your organization to implement DevOps at scale. OpsWorks for Chef Automate provides three key capabilities that you can configure to support DevOps practices: workflow, compliance, and visibility.


### Recipe Anatomy
- A Chef recipe consists of a set of resource definitions. The definitions describe the desired state of the resources and how Chef can bring them to that state. Chef supports over 60 resource types. A list of common resource types appears below.

![1](https://user-images.githubusercontent.com/23625821/134759673-d98e85be-1847-4597-9705-598b401ec449.png)


```rb
package 'apache2' do
    case node[:platform]
    when 'centos','redhat','fedora','amazon'
        package_name 'httpd'
    when 'debian','ubuntu'
        package_name 'apache2'
    end
    action :install
end


```


### Linting with Rubocop and Foodcritic

- Linting can be done on infrastructure code such as Chef recipes using tools such as Rubocop and Foodcritic. 
- Rubocop performs static analysis on Chef recipes based on the Ruby style guide. (Ruby is the language used to create Chef recipes.) 

- This tool is part of the Chef Development Kit and can be integrated into the software development workflow. 
- Foodcritic checks Chef recipes for common syntax errors based on a set of built-in rules, which can be extended by community contributions.



### Unit Testing with ChefSpec

- ChefSpec can provide unit testing on Chef cookbooks. 
- These tests can determine whether Chef is being asked to do the appropriate tasks to accomplish the desired goals. 
- ChefSpec requires a configuration test specification that is then evaluated against a recipe.
- For example, ChefSpec would not actually check whether Chef installed the Apache package, but instead checks whether a Chef recipe asked to install Apache. 
- The goal of the test is to validate whether the recipe reflects the intentions of the programmer.


### Governance and Compliance

#### AWS Config
- It enables you to assess, audit, and evaluate the configurations of your AWS resources. 
- It automatically builds an inventory of your resources and tracks changes made to them. 
- It also provides a clear view of the resource change timeline, including changes in both the resource configurations and the associations of those resources to other AWS resources. 
- With AWS Config rules, every change triggers an evaluation by the rules associated with the resources. 

- AWS provides a collection of managed rules for common requirements such as IAM users having good passwords, groups and policies, or for determining if EC2 instances are on the correct VPCs and Security Groups. 
- AWS Config rules can quickly identify noncompliant resources and help with reporting and remediation.

- When a custom rule is invoked through AWS Config rules, the associated Lambda function receives the configuration events, processes them, and returns results. 
- The following function determines if Amazon Virtual Private Cloud (Amazon VPC) flow logs are enabled on a given Amazon VPC.


![1](https://user-images.githubusercontent.com/23625821/134855064-a224f460-b4a8-419d-8347-74a04a4787ca.png)


### Resource Optimization

- In this stage, administrators review performance data and identify changes needed to optimize the environment around criteria such as security, performance, and cost management. 
- It’s important for all application stakeholders to regularly evaluate the infrastructure to determine if it is being used optimally.

- AWS Trusted Advisor helps you observe best practices by scanning your AWS resources and comparing their usage against AWS best practices in four ategories: cost optimization, performance, security, and fault tolerance. 
- As part of ongoing improvement to your infrastructure and applications, taking advantage of Trusted Advisor can help keep your resources provisioned optimally.


![image](https://user-images.githubusercontent.com/23625821/135028073-87a67fa2-dca2-45d5-8a85-c35963fd71e8.png)



## Conclusion 

Here are some key actions you can take as you implement Infrastructure as Code in your organization:

- Start by using a managed source control service, such as AWS CodeCommit, for your infrastructure code.
- Incorporate a quality control process via unit tests and static code analysis before deployments.

- Remove the human element and automate infrastructure provisioning, including infrastructure permission policies.
- Create idempotent infrastructure code that you can easily redeploy.
- Roll out every new update to your infrastructure via code by updating your idempotent stacks. Avoid making one-off changes manually.

- Embrace end-to-end automation.
- Include infrastructure automation work as part of regular product sprints.

- Make your changes auditable, and make logging mandatory.
- Define common standards across your organization and continuously optimize.





#### References:

<a href="https://d0.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf"> Original paper </a>


