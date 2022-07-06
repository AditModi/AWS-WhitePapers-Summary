# Introduction

* This whitepaper describes how AWS approaches machine learning (ML) in a regulated environment and provides guidance on good ML practices using AWS products.

* The pharmaceutical industry, which is sometimes slow to adopt the latest technologies, is witnessing a massive change. The industry is looking to technologies such as artificial intelligence/machine learning (AI/ML), Internet of Things (IoT), blockchain, and other Industry 4.0 technologies. With this adoption of new technology comes regulatory challenges.

* Machine Learning in particular has garnered some focus recently with the publishing of a discussion paper by the FDA. It explores the use of AI/ML in the context of medical devices but many of the same topics arise when bringing up ML adoption with executive leadership in any company: How do you trust ML models to not make important business decisions based on erroneous or unstable values? 
How do you know you have good hygiene in managing your ML environments? Are you prepared for (or capable of) a retrospective analysis if anything goes wrong?

* One particularly strong example is seen in pharmaceutical companies, who must abide by mandatory reporting standards for any patient-relevant “Adverse Events” that are viewed by any employee or ingested by the company. 

* This means that creating a new Twitter account for a particular market, or releasing a digital therapeutic app for patient health, can result in a surge of new natural language data that needs to be reviewed, adjudicated, and in some cases reported to the FDA.

* This flood of data can completely overwhelm manual review teams and risk delays in reporting to the FDA within the mandated time limits, resulting in the potential for formal warnings or even legal action. 

* ML has the potential to address the immediate needs of scale in these scenarios, and triage obvious problem cases from innocuous cases.

* However, before deploying these models, you may need to evaluate a few requirements relevant to your stakeholders or regulators, such as model reproducibility, model explainability, decision support tooling, and how this all ties into templatized ML workflows.

# Benefits of machine learning

* Regulatory agencies such as the FDA acknowledge that ML-based technologies hold the potential to transform healthcare through their ability to derive new and important insights from vast amounts of data. 

* One of the technology’s greatest strengths is its ability to continually learn from real-world data, and its capability to improve its performance.  

* The ability for AI/ML software to learn from real-world feedback (training) and improve its performance (adaptation) makes these technologies uniquely situated among software as a medical device (SaMD). 

* To help customers realize the benefits of ML, this whitepaper covers the points raised by the FDA while also drawing from AWS resources.

* The Life Sciences industry (encompassing, but not exclusively, bio-pharma, genomics, medical diagnostics, and medical devices) is governed by a set of regulatory guidelines called Good Laboratory Practice, Good Clinical Practice, Good Manufacturing Practice, and Good Machine Learning Practices (commonly referred to as “GxP”).

* The [GxP Systems on AWS]() whitepaper provides information on how AWS approaches GxP-related compliance and security, and provides customers guidance on using AWS products in the context of GxP.

* The [AWS Well-Architected Framework]() helps you understand the pros and cons of decisions you make while building systems on AWS. Using the Framework enables you to learn architectural best practices for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud. 

* The [Machine Learning Lens]() whitepaper focuses on how to design, deploy, and architect your ML workloads in the AWS Cloud. This lens adds to the best practices included in the Well-Architected Framework, but also covers ML scenarios and identifies key elements to ensure that your workloads are architected according to best practices.

# Life sciences at AWS

* AWS started its dedicated Genomics and Life Sciences Practice in 2014 in response to the growing demand for an experienced and reliable life sciences cloud industry leader.

* The AWS Genomics and Life Sciences practice serves a large ecosystem of life sciences customers, including some of the largest enterprise pharmaceutical, biotechnology, medical device, genomics, start-ups, university, and government institutions, as well as healthcare payers and providers.

* In addition to the resources available within the Genomics and Life Science practice at AWS, you can also work with AWS Life Sciences Competency Partners to drive innovation and improve efficiency across the life sciences value chain, including cost- effective storage and compute capabilities, advanced analytics, and patient personalization mechanisms.

* AI/ML in the life sciences industry fall under five main categories:
 * **Research and discovery** — Use cases include molecular structure prediction, antibody binding affinity prediction, outcome prediction for patients with certain biomarkers, patient data enrichment and search, the classification of tissue samples, and so on.
 * **Clinical development** — Use cases include forecasting and optimization of trial timelines, optimization of inclusion/exclusion criteria and site selection, protocol document enrichment, and so on.
 * **Manufacturing and supply chain** — Use cases include predictive maintenance of equipment, computer vision for effective line clearance, optimization of manufacturing process staging, vial inspection, and so on.
 * **Commercial** — Use cases include predicting healthcare providers with relevant patient bases, identifying next best action for sales and marketing, annotation and management of existing promotional materials, and so on.
 * **Post-market surveillance and patient support** — Use cases include forecasting patient cost, automation of adverse event detection from social media or call centers, patient outcome prediction, and so on.

# Current regulatory situation

* Traditional Computer Systems Validation (CSV) is a point-in-time exercise where the resultant computer system was tested against the requirements to verify that the system satisfies its intended use. Whenever there was a change, the system went through revalidation.

* However, the FDA’s view is that AI/ML-based SaMD exist on a spectrum, from locked to nearly continuously learning and changing.

* The FDA proposes a Total Product Lifecycle (TPLC) approach which enables the evaluation and monitoring of a software product from its premarket development to postmarket performance, along with continued demonstration of the organization’s excellence.

* With this approach, the FDA will assess the following:
Culture of quality and organizational excellence — to gain a reasonable assurance of the high quality of the organization’s software development, testing, and performance monitoring capabilities
 * Pre-market assurance of safety and effectiveness
 * Review of SaMD pre-specifications and algorithm change protocol
 * Real-world performance monitoring


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/az5cp29v5fssnm6erfct.png)
*Reference Implementation of Good machine learning practices* 

* This framework relies on the principle of a “predetermined change control plan.” The predetermined change control plan includes the types of anticipated modifications, (SaMD Pre-Specifications) based on the retraining and model update strategy, and the associated methodology (Algorithm Change Protocol) being used to implement those changes in a controlled manner that manages risks to patients.
 * **SaMD Pre-Specifications (SPS)** — A SaMD manufacturer’s anticipated modifications to “performance” or “inputs,” or changes related to the “intended use” of AI/ML-based SaMD. These are the types of changes the manufacturer plans to achieve when the SaMD is in use. The SPS draws a “region of potential changes” around the initial specifications and labeling of the original device. This is what the manufacturer intends the algorithm to become as it learns.
 * **Algorithm Change Protocol (ACP)** — Specific methods that a manufacturer has in place to achieve and appropriately control the risks of the anticipated types of modifications delineated in the SPS. The ACP is a step-by-step delineation of the data and procedures to be followed so that the modification achieves its goals and the device remains safe and effective after the modification. 
The preceding figure provides a general overview of the components of an ACP. This is how the algorithm will learn and change while remaining safe and effective.


# Challenges to support AI/ML enabled GxP workloads

* Developing AI/ML-enabled GxP workloads raise the following challenges:
 * Building a secure infrastructure that complies with a stringent regulatory process working on the public cloud and aligning to the FDA framework for AI/ML
 * Supporting an AI/ML-enabled solution for GxP workloads covering:
  * Reproducibility
  * Traceability
  * Data integrity
 * Monitoring the Machine Learning model with respect to various changes to parameters and data
  * Handling model uncertainty and confidence calibration
  * Making AI/ML models interpretable

# Provision a secure and GXP-compliant machine learning environment

* For healthcare and life sciences customers, the security and privacy of an ML environment is paramount. It is therefore strongly recommended that you protect your environment against unauthorized access, privilege escalation, and data exfiltration. Common considerations when you set up a secure ML environment include:
 * Platform qualification
 * Compute and network isolation
 * Authentication and authorization
These considerations are detailed in the following sections.

## Platform qualification

* The validated state of ML models may be brought into question if the underlying IT infrastructure is not maintained in a demonstrable state of control and regulatory compliance. 

* Similarly, data integrity can also be affected by problems related to IT infrastructure. Therefore, to support a culture of quality and operational excellence, it is important to qualify your underlying platform and then maintain it under a state of control.

* Details about platform qualification and the approach taken by many AWS customers are described in GxP whitepaper. 

* Customers will often provide a qualified MLOps platform to their teams, along with a selection of pre-qualified building blocks to support their specific needs. 

## Compute and network isolation

* A well-governed and secure ML workflow begins with establishing a private and isolated compute and network environment. The virtual private cloud (VPC) that hosts Amazon SageMaker and its associated components, such as Jupyter notebooks, training instances, and hosting instances, should be deployed in a private network with no internet connectivity.

* Connectivity between SageMaker and other AWS services, such as Amazon Simple Storage Service (Amazon S3), should be established using VPC endpoints or even AWS PrivateLink. 

* Additionally, when creating a VPC endpoint, you can attach an endpoint policy to further control access to specific resources for which you are connecting.

# Authentication and authorization

* After you create an isolated and private network environment, the next step is to ensure that only authorized users can access the appropriate AWS services. 

* AWS Identity and Access Management (IAM) can help you create preventive controls for many aspects of your ML environment, including access to SageMaker resources, access to your data in S3, and access to API endpoints.

* You can access AWS using a RESTful API, and every API call is authorized by IAM. You grant explicit permissions through IAM policy documents, which specify the principal (who), the actions (API calls), and the resources (such as S3 objects) that are allowed, as well as the conditions under which the access is granted.

* Access to AWS Glue and Amazon SageMaker resources is also governed by IAM. While each organization has different authentication and access requirements, you should configure permissions in line with IAM best practices and your own internal policies and controls by granting least privilege access, or granting only the permissions required to perform a particular task.

* Role-based access control (RBAC) is an approach used commonly by customers in financial services for ensuring only authorized parties have access to desired system controls. 

* Creating roles based on job function policies, and using AWS Config to monitor and periodically audit IAM policies attached to users, is a recommended best practice for viewing configuration changes over time.

* AWS Config offers conformance packs, which provide a general-purpose compliance framework designed to enable you to create security, operational or cost-optimization governance checks using managed or custom AWS Config rules and AWS Config remediation actions.

## Data encryption

* Because ML environments can contain sensitive data and intellectual property, one of the considerations for a secure ML environment is data encryption. AWS recommends that you enable data encryption, both at rest and in transit.

* For at rest encryption, AWS provides tools for creating an encrypted file system using open standard algorithms. For instance, customers can encrypt data stored in Amazon S3 and the data stored in Amazon Elastic Block Store (Amazon EBS) volumes.

* Some healthcare customers require the use of AWS KMS keys. Additionally, AWS recommends enabling Amazon S3 default encryption so that all new objects are encrypted when they are stored in the Amazon S3 bucket.

# Machine learning lifecycle

* Building and operating a typical ML workload is an iterative process, and consists of multiple phases. We identify these phases loosely based on the open standard process model for Cross Industry Standard Process Data Mining (CRISP-DM) as a general guideline. 

* CRISP-DM is used as a baseline because it’s a proven tool in the industry and is application neutral, which makes it an easy-to-apply methodology that is applicable to a wide variety of ML pipelines and workloads. 

* The end-to-end ML process includes the following phases:
 * Business goal identification 
 * ML problem framing
 * Data collection
 * Data integration and preparation
 * Feature engineering
 * Model training
 * Model validation
 * Business evaluation
 * Production deployment (model deployment and model inference)

* Below section presents a high-level overview of the various phases of an end-to-end ML lifecycle, which helps frame our discussion around security, compliance, and operationalization of Amazon Web Services ML best practices in healthcare and life science.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/96ypw40tmtixt9k4gbm4.png)
*The machine learning lifecycle process* 

* More Details about various phases of an end-to-end Ml lifecycle can be found [here]().

# Best practices for ML lifecycle stages

 * Data collection
 * Data integration and preparation
 * Feature engineering
 * Model training
 * Model validation
 * Additional considerations for AI/ML compliance
 * Auditability
 * Traceability
 * Reproducibility
 * Model interpretability
 * Model monitoring
 * Operationalize AI/ML workloads

* More Details about best practices for Different Lifecycle stages can be found [here]().

# Reference architectures

## Training pipeline

* The following shows a specific model training and tracking architecture that leverages AWS native tools and services.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1vs8kt1dj8ubkdhj09qd.png)
*Model training and tracking architecture*


* **Step 1: Train Model Handler (Lambda) | Train Model Queue (SQS)** — Model training is initiated; training could be initiated in multiple ways:
 * Manually (follows human in the loop design)
 * Automated (by scheduling relevant jobs)
* **Steps 2, 3: AWS Glue** — Reads and processes data from multiple sources. Intermediate files like train/test/validation datasets are stored in a training repository (S3)
 
* **Steps 4, 5: Amazon SageMaker** — Trains the ensemble models and saves the model file and train and test results into S3 and DynamoDB training results tables
* **Step 6: Training Results Queue (SQS)** — Upon completion of model training, a notification is sent to an analyst/data scientist to review the model performance results, based on the decision made by the reviewer that an action would be taken
* **Step 7, 8, 9: Endpoint Update Handler (Lambda)** — Depending on the decision of the reviewer, either
 * The endpoint is updated with the new model, or
 * The existing endpoint is not deleted or reused
* Step 10: After the endpoint is updated, an email notification will be sent to the client.

## Inference pipeline

* The following figure shows a specific model inference architecture that leverages AWS native tools and services.
 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0qmor52o658tn2cx2iv3.png)
*Specific model inference architecture that leverages AWS native tools and services*

* Step1: When the input data is available, ingest handler is invoked and necessary preprocessing steps are performed. Data is stored in S3 and a message is passed to Records Queue
* Step 2: Inference Handler is involved by the Records Queue, and data is read from S3
* Step 3: Inference Handler performs inference using Amazon SageMaker resources
* Step 4: After performing inference, the results are saved in the DynamoDB results table and the message is passed to the Results Queue
* Step 5: Results Handler then publishes the results to the Client application or a QuickSight dashboard

## Orchestration

* General guidelines for creating pipelines include using an orchestration layer, such as AWS CodePipeline.
To incorporate traceability, use the following services listed in the following architecture:
 * Amazon SageMaker, a fully-managed ML service that lets data scientists and developers build, train, and deploy ML models to production
 * Amazon S3, a highly scalable, reliable service to store and retrieve any amount of data, at any time, from anywhere on the web
 * Amazon DynamoDB, a fully managed key-value and document database that delivers single-digit millisecond performance at any scale
 * AWS Lambda, a serverless compute service to run code without provisioning or managing servers
 * AWS Glue, a fully-managed data preparation service for ETL operation
 * AWS CodePipeline, a fully managed nearly continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates

* The following figure highlights only the key components for tracking data and model lineage:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/udni85ip86m1mqyuqsvm.png)
*Key components for tracking data and model lineage*

 * Latest code changes; commit_id can be extracted from AWS CodeCommit or any GitHub toolkit.
 * AWS Glue Crawlers, AWS Glue jobs, or any ETL services can be used to extract raw data, run preprocessing, split data into train/test/validation datasets, and export data to S3 and metadata to DynamoDB tables.
 * Amazon SageMaker can be used for model development and evaluation steps. Necessary model training results and model artifacts can be sent to S3. Necessary metadata can be sent to DynamoDB tables.
 * Model performance results can be reviewed by data scientists or other human reviewers to decide if the model can be promoted to the production environment, in which case the management table model version status can be set to ACTIVE.
 * After data is scored, input data, predicted probability, predicted label, and the model version used for performing inference can be saved in a DynamoDB inference results table, and necessary results can be sent to S3.

## Orchestration for SageMaker jobs

* Amazon SageMaker Pipelines can automate various tasks across the ML lifecycle. All infrastructure is fully managed. SageMaker Pipelines have the ability to:

 * Orchestrate SageMaker jobs and author reproducible ML pipelines
 * Deploy custom-build models for inference in real-time with low latency or offline inferences with Batch Transform
 * Monitor lineage of objects

* Through a simple interface, you can implement sound operating practices for deploying and monitoring production workflows, model artifact deployment, and artifact lineage tracking while adhering to safety and best practice paradigms for ML application creation.

* Certain use cases may necessitate a single, larger, end-to-end pipeline that can handle anything. Other use cases, such as the following, can necessitate multiple pipelines:

 * One pipeline for ETL and data transformation steps
 * One pipeline for model training, tuning, lineage, and depositing into the Model Registry
 * Possibly another pipeline for specific inference scenarios (such as real-time vs. batch)
 * One pipeline for triggering retraining by using SageMaker Model Monitor to detect model drift or data drift and trigger retraining.

* Some of the main components in Amazon SageMaker Pipelines include pipelines, Model Registry, and MLOps templates.

 * **Pipelines** – Model building pipelines are defined with a simple Python SDK. They can include any operation available in Amazon SageMaker, such as data preparation with Amazon SageMaker Processing or Amazon SageMaker Data Wrangler, model training, model deployment to a real-time endpoint, or batch transform. You can also add Amazon SageMaker Clarify to your pipelines to detect bias prior to training, or after the model has been deployed. Likewise, you can add Amazon SageMaker Model Monitor to detect data and prediction quality issues. 
 * After they are launched, model building pipelines are run as CI/CD pipelines. Every step is recorded, and detailed logging information is available for traceability and debugging purposes. You can also visualize pipelines in Amazon SageMaker Studio, and track their different runs in real-time.
 * **Model Registry** – The Model Registry lets you track and catalog your models. In SageMaker Studio, you can easily view model history, list and compare versions, and track metadata such as model evaluation metrics. You can also define which versions may or may not be deployed in production. You can even build pipelines that automatically trigger model deployment after approval has been given. You’ll find that the Model Registry is very useful in tracing model lineage, improving model governance, and strengthening your compliance posture.
 * **MLOps templates** – SageMaker Pipelines includes a collection of built-in CI/CD templates published via AWS Service Catalog for popular pipelines (build/train/deploy, deploy only, and so on). You can also add and publish your own templates, so that your teams can efficiently discover them and deploy them. Not only do templates save lots of time, they also make it easier for ML teams to collaborate from experimentation to deployment, using standard processes, and without having to manage any infrastructure. Templates also enable Ops teams to customize steps as needed, and give them full visibility for troubleshooting.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/exor00w8j57o3vzk7m0u.png)
*Directed acyclic graph of steps that orchestrate SageMaker jobs*
 
* The preceding figure is a Directed Acyclic Graph (DAG) of steps and conditions that orchestrate SageMaker jobs and resource creation. Sample notebooks are available to get you started.
   
# Conclusion

* This whitepaper discusses security and GxP compliance considerations for operationalizing AI/ML workloads in the life sciences industry. It references Good Machine Learning Practices (GMLP), as referenced by the FDA, and details best practices for implementing ML workflows. 

* However, you should work with your company’s regulatory and compliance team and understand your company’s policies and regulatory responsibilities before implementing these solutions, which may impact your use of ML.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/ML-best-practices-health-science.pdf?did=wp_card&trk=wp_card)