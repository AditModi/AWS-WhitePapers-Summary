
# ML Best Practices for Public Sector Organizations

- This whitepaper outlines some of the challenges for public sector agencies in adoption and implementation of ML. 
- And provides best practices to address thesechallenges. 
- The target audience for this whitepaper includes executive leaders and agency IT Directors.

## Challenges for public sector

- Data Ingestion and Preparation.
- Model Training and Tuning. 
- ML Operations (MLOps). 
- Governance. 

- Security & Compliance.
- Cost Optimization.
- Bias and Explainability.


## Best Practices

### Data Ingestion and Preparation
- The AWS Cloud enables public sector customers to overcome the challenge of connecting to and extracting data from both streaming and batch data, as described in the following:

  - For streaming data, Amazon Kinesis and AWS Managed Streaming for Apache Kafka (Amazon MSK) enable collection, processing, and analysis of data in real time. 
  - Amazon Kinesis provides a suite of capabilities to collect, process, and analyze real-time, streaming data. 
  - Amazon Kinesis Data Streams (KDS) is a service that enables ingestion of streaming data. 
  - Producers of data push data directly into a stream, which consists of a group of stored data units called records. 
  - The stored data is available for further processing or storage as part of the data pipeline. 
  
  - Ingestion of streaming videos can be done using Amazon Kinesis Video Streams.
  - There are a number of mechanisms available for data ingestion in batch format. 
  - With AWS Database Migration Services (AWS DMS), you can replicate and ingest existing databases while the source databases remain fully operational. 
  - The service supports multiple database sources and targets, including writing data directly to Amazon S3.

### Data Preparation
- Once the data is extracted, it needs to be transformed and loaded into a data store for feeding into an ML model. 
- It needs to be cataloged and organized so that it is available for consumption, and needs to enable data lineage for compliance with federal government guidelines. 
- AWS Cloud provides three services that provide these mechanisms.
   - AWS GLUE
   - Amazon Sagemaker data wrangler
   - Amazon EMR
   

### Model Training and Tuning
- It involves the selection of a ML model that is appropriate for the use case, followed by training and tuning of the ML model.
- One of the major challenges facing the public sector is the ability for team members to apply a consistent pattern or framework for working with multitudes of options that exist in this space.
- The AWS Cloud enables public sector customers to overcome challenges in model selection, training, and tuning as described in the following.

- You can use AWS Sagemaker built-in algorithms or your own script (script mode). 

![image](https://user-images.githubusercontent.com/23625821/135825866-e1b9c2a0-5852-47a7-ac9f-85f2f7e79077.png)


![image](https://user-images.githubusercontent.com/23625821/135813159-2c3dcf88-238f-4f04-b922-290217687b2b.png)

- Or you can use your own container (BYOC). 

![image](https://user-images.githubusercontent.com/23625821/135813365-1886b6cf-c529-41e8-a43e-976fdf2883f7.png)



### MLOps
- MLOps is the discipline of integrating ML workloads into release management, Continuous Integration / Continuous Delivery (CI/CD), and operations.
- One of the major hurdles facing government organizations is the ability to create a repeatable process for deployment that is consistent with their organizational best practices. 
- Using ML models in software development makes it difficult to achieve versioning, quality control, reliability, reproducibility, explainability, and audibility in that process.
- AWS Cloud provides a number of different options that solve these challenges, either by building an MLOps pipeline from scratch or by using managed services.
- You can use Amazon Sagemaker pipelines. 


### AWS CodePipeline and AWS Lambda
- For AWS programmers, teams that are already working with CodePipeline for deployment of other workloads, an option exists to utilize the same workflows for ML.

![image](https://user-images.githubusercontent.com/23625821/135985926-5dfdbfae-17b5-4d9d-bcc6-5f70a8cffe8f.png)


### AWS StepFunctions Data Science Software Development Kit (SDK)

- It is an open-source Python library that allows data scientists to create workflows that process and publish ML models using SageMaker and Step Functions. 
- This can be used by teams that are already comfortable using Python and AWS Step Functions.
- The SDK provides the ability to copy workflows, experiment with new options, and then put the refined workflow in production. 

- The SDK can also be used to create and visualize end-to-end data science workflows that perform tasks such as data pre-processing on AWS Glue and model training, hyperparameter tuning, and endpoint creation on Amazon SageMaker.
- Workflows can be reused in production by exporting AWS CloudFormation (infrastructure as code) templates.

### AWS MLOps Framework

![image](https://user-images.githubusercontent.com/23625821/135986339-23ac5cf0-a735-4dfa-846a-aa82aafd0c2d.png)

The solution provides a ready-made template to upload trained models (also referred to as a bring your own model), configure the orchestration of the pipeline, and monitor the pipeline's operations.

- You can deploy custom ML models on EC2, or EKS, etc. 
- Also, deploy at the Edge: AWS IoT Greengrass. 


### Management and Governance

- AWS Cloud provides several services that enable governance and control. 
- These include: 
   - AWS Control Tower.
   - License Manager. 
   - Resource Tagging. 

#### AWS Service Catalog 
- Deploying and setting up ML workspaces for a group or different groups of people is always a big challenge for public sector organizations.
- AWS Service Catalog provides a solution for this problem. It enables the central management of commonly deployed IT services, and achieves consistent governance and meets compliance requirements. 
- End users can quickly deploy only the approved IT services they need, following the constraints set by the organization.
- For example, AWS Service Catalog can be used with Amazon SageMaker notebooks to provide end users a template to quickly deploy and set up their ML Workspace.

![image](https://user-images.githubusercontent.com/23625821/136161153-083ad035-909a-4353-bcd3-297a75257a6f.png)


### Security and compliance
- Public sector organizations have a number of security challenges and concerns with hosting ML workloads in the cloud as these applications can contain sensitive customer data. 
- This includes personal information or proprietary information that must be protected over the entire data lifecycle.
- The specific concerns also include protecting the network and underlying resources such as compute, storage and databases; user authentication and authorization; logging, monitoring and auditing.

![image](https://user-images.githubusercontent.com/23625821/136161633-79480422-ba1d-4fe5-9fa6-1a767bff573f.png)

#### Compute and network isolation

- One of the major requirements with many public sector ML projects is the ability to keep the environments, data and workloads secure and isolated from internet access. 
- These can be achieved using the following methods: 
   - Provision ML components in an isolated VPC with no internet access, see more info <a href="https://aws.amazon.com/blogs/machine-learning/building-secure-machine-learning-environments-with-amazon-sagemaker/"> here </a>. 
   - Use VPC end-point and end-point policies to further limit access, see more info <a href="https://aws.amazon.com/blogs/machine-learning/securing-amazon-sagemaker-studio-connectivity-using-a-private-vpc/"> here </a>. 


![image](https://user-images.githubusercontent.com/23625821/136162671-107c9a7a-1851-4061-9f5a-592c222b4222.png)


### Data Protection
- Protect data at rest with KMS. 
- Protect data in transit with TLS/SSL. 
- Secure shared notebook instances. 


### Authentication and Authorization

- AWS IAM enables control of access to AWS resources. IAM administrators control who can be authenticated (signed in) and authorized (have permissions) to use SageMaker resources. 
- IAM can help create preventive controls for many aspects of your ML environment, including access to Amazon SageMaker resources, data in Amazon S3, and API endpoints.


### Artifact and model management

- The recommended best practice is to use version control to track code or other model artifacts. 
- If model artifacts are modified or deleted, either accidentally or deliberately, version control allows you to roll back to a previous stable release. 
- This can be used in cases where an unauthorized user gains access to the environment and makes changes to the model. 
- If model artifacts are stored in Amazon S3, versioning should be enabled.


### Security compliance

- Third-party auditors assess the security and compliance of Amazon SageMaker as part of multiple AWS compliance programs including FedRAMP, HIPAA, and others.
- AWS provides the following resources to help with compliance:
   - Security and Compliance Quick Start Guides – These deployment guides discuss architectural considerations and provide steps for deploying security- and compliance-focused baseline environments on AWS.
   - Architecting for HIPAA Security and Compliance – This  describes how organizations can use AWS to help create HIPAA-compliant applications.
   - AWS Compliance Resources – This collection of workbooks and guides might apply to the Organization’s industry and location.
   - AWS Config – This AWS service assesses how well resource configurations comply with internal practices, industry guidelines, and regulations. 
   - As an example, AWS Config can be used to create compliance rules that can scan AWS Key Management Service (AWS KMS) key policies to determine whether these policies align with the principle of granting least privilege to users. 
   - Please refer to the How to use AWS Config to determine compliance of AWS KMS key policies to your specifications, which outlines this process.
   - AWS Security Hub – This AWS service provides a comprehensive view of the security state within AWS that helps check compliance with security industry standards and best practices.



### Cost Optimization 

- Cost management is a primary concern for public sector organizations projects to ensure the best use of public funds while enabling agency missions. 
- AWS provides several mechanisms to manage costs in each phase of the ML lifecycle (Prepare, Build, Train & Tune, Deploy, and Manage) as described in this section.

#### Prepare

This step of the ML lifecycle includes storing the data, labeling the data, and processing the data. Cost control in this phase can be accomplished using the following techniques:

#### Data Storage: 

- ML requires extensive data exploration and transformation. 
- Multiple redundant copies of data are quickly generated, which can lead to exponential growth in storage costs. Therefore, it is essential to establish a cost control strategy at the storage level. 
- Processes can be established to regularly analyze source data and either remove duplicative data or archive data to lower cost storage based on compliance policies. 

- For example, for data stored in S3, S3 storage class analysis can be enabled on any group of objects (based on prefix or object tagging) to automatically analyze storage access patterns. This enables identification and transition of rarely-accessed data to S3 glacier, lowering costs. S3 intelligent storage can also be used to lower costs of data that has unpredictable usage patterns. 

- It works by monitoring and moving data between a data tier that is optimized for frequent access and another lower-cost tier that is optimized for infrequent access.


#### Data Labeling. 

- Data labeling is a key process of identifying raw data (such as images, text files, and videos) and adding one or more meaningful and informative labels to provide context so that an ML model can learn from it. 

- This process can be very time consuming and can quickly increase costs of a project.
- Amazon SageMaker Ground Truth can be used to reduce these costs. 
- Ground Truth’s automated data labeling utilizes the Active Learning ML technique to reduce the number of labels required for models, thereby lowering these costs. 
- Ground Truth also provides additional mechanisms such as crowdsourcing with Amazon Mechanical Turk or another vendor company, that can be chosen to lower the costs of labeling.


#### Data Wrangling. 

- In ML, a lot of time is spent in identifying, converting, transforming, and validating raw source data into features that can be used to train models and
make predictions. 

- Amazon SageMaker Data Wrangler can be used to reduce this time spent, lowering the costs of the project. With Data Wrangler, data can be imported from various data sources, and transformed without requiring coding. 

- Once data is prepared, fully automated ML workflows can be built with Amazon SageMaker Pipelines and saved for reuse in the Amazon SageMaker Feature Store, eliminating the costs incurred in preparing this data again.


### Build

- This step of the ML lifecycle involves building ML models. Cost control in this phase can be accomplished using the following techniques:
   - Notebook Utilization.
   - Test Code locally. 
   - Use Pipe mode (where applicable) to reduce training time. 
   - Find the right balance: Performance vs. accuracy. 
   - AWS Marketplace. 

### Train and Tune

- This step of the ML lifecycle involves providing the algorithm selected in the build phase with the training data to learn from, and setting the model parameters to optimize the training process. 
- Cost control in this phase can be accomplished using the following techniques:
    
    - Use Spot Instances. 
    - Hyperparameter optimization (HPO). 
    - Distributed Training. 
    - Monitor the performance of your training jobs to identify waste. 
 


### Deploy and Manage

- This step of the ML lifecycle involves deployment of the model to get predictions, and managing the model to ensure it meets functional and non-functional requirements of the application. 
- Cost control in this phase can be accomplished using the following techniques:
   
   - Endpoint deployment. 
   - Multi-model endpoints. 
   - Auto Scaling.
   - Amazon Elastic Inference for deep learning.
   - Analyzing costs with Cost Explorer.
   - AWS Budgets.
 
 
 
### Bias and Explainability 

- Demonstrating explainability is a significant challenge because complex ML models are hard to understand and even harder to interpret and debug. 
- There is an inherent tension between ML performance (predictive accuracy) and explainability; 
- Often the highest performing methods are the least explainable, and the most explainable are less accurate. 
- Hence, public sector organizations need to invest significant time with appropriate tools, techniques, 
- And mechanisms to demonstrate explainability and lack of bias in their ML models, which could be a deterrent to adoption.
- AWS Cloud provides the following capabilities and services to assist public sector organizations in resolving these challenges.



### Amazon SageMaker Debugger

- Amazon SageMaker Debugger provides visibility into the model training process for real-time and offline analysis. In the existing training code for TensorFlow, Keras, Apache MXNet, PyTorch, and XGBoost, 
- The new SageMaker Debugger SDK can be used to save the internal model state at periodic intervals in S3. 
- This state is composed of a number of components: 
    - The parameters being learned by the model (for example, weights and biases for neural networks), the changes applied to these parameters by the optimizer (gradients), optimization parameters, scalar values such as accuracies and losses, and outputs of each layer of a neural network.

- SageMaker Debugger provides three built-in tensor collections called feature importance, average_shap, and full_shap, to visualize and analyze captured tensors specifically for model explanation. 
- Feature importance is a technique that explains the features that make up the training data using a score (importance). 
- It indicates how useful or valuable the feature is, relative to other features.

- SHAP (SHapley Additive exPlanations) is an open-source technique based on game theory. 
- It explains an ML prediction by assuming that each feature value of training data instance is a player in a game in which the prediction is the payout. 
- Shapley values indicate how to distribute the payout fairly among the features.


### Conclusion

Public sector organizations have complex mission objectives and are increasingly adopting ML services to help with their initiatives. 
ML can transform the way government agencies operate, and enable them to provide improved citizen services. 
However, several barriers remain for these organizations to implement ML. 
This whitepaper outlined some of the challenges and provided best practices that can help address these challenges using AWS Cloud.


#### Reference

<a href="https://d1.awsstatic.com/whitepapers/machine-learning-best-practices-for-public-sector-organizations.pdf?did=wp_card&trk=wp_card"> Original paper </a>


