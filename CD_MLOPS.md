
# MLOps: Operationalizing ML on AWS 

  ## AWS_WhitePaper_Summary 

- In modern software development, continuous delivery (CD) principles have significantly improved the throughput of delivering software to production in a safe, continuous, and reliable way and helped to avoid big, disruptive, and error prone deployments. 
- Creating a process to operationalize ML systems enables organizations to leverage the new and endless opportunities of machine learning to optimize processes and products. 
- Using ML models in software development makes it difficult to achieve versioning, quality control, reliability, reproducibility, explainability, and audibility in that process.
- This happens because there are a higher number of: 
   1. changing artifacts to be managed. 
   2. In addition to the software code, such as the datasets, the machine learning models, and the parameters and hyperparameters used by such models.
   3. And the size and portability of such artifacts can be orders of magnitude higher than the software code.
  
- There are also organizational challenges.
   1. Different teams might own different parts of the process and have their own ways of working.
   2. Data engineers might be building pipelines to make data accessible, while data scientists can be researching and exploring better models.
   3. Machine learning engineers or developers then have to worry about how to integrate that model and release it to production.
   4. When these groups work in separate siloes, there is a high risk of creating friction in the process and delivering suboptimal results.

## Continuous Delivery for Machine Learning (CD4ML) 
- CD4ML is a software engineering approach in which a cross-functional team produces machine learning applications based on code, data, and models in small and safe increments that can be reproduced and reliably released at any time, in short adaptation cycles. 
- it requires a feedback loop: The real- world data is continuously changing and the models in productions are continuously monitored, leading to adaptations and improvements by re-training of the models and the re-iteration of the whole process. 

### CD4ML Process Steps
![1](https://user-images.githubusercontent.com/23625821/125156449-bb954300-e165-11eb-947c-e9bce2502e84.png)

#### Model Building 
- Once the need for a machine learning system is found, data scientists will research and experiment to develop the best model, by trying different combinations of algorithms, and tuning their parameters and hyperparameters.
- 
#### Model Evaluation
- As the data science process is very research-oriented, it is common that there will be multiple experiments running in parallel, and many of them will never make their way to production.
- Your CD4ML architecture will need to support tracking, visualizing, comparing results from different runs, as well as to support the graduation and promotion of models that prove to be useful.

#### Productionize the Model
- There will always be an implicit contract (API) between the model and how it is consumed.

#### Testing & Quality
- While you cannot write a deterministic test to assert the model score from a given training run, the CD4ML process can automate the collection of such metrics and track their trend over time.
- This allows you to introduce quality gates that fail when they cross a configurable threshold and to ensure that models don’t degrade against known performance baselines.

#### Deployment
- Once a good candidate model is found, it must be deployed to production.
-  There are different approaches to do that with minimal disruption: 
    1. You can have multiple models performing the same task for different partitions of the problem.
    2. You can have a shadow model deployed side by side with the current one to monitor its performance before promoting it.
    3. You can have competing models being actively used by different segments of the user base.
    4. Or you can have online learning models that are continuously improving with the arrival of new data.

- Elastic cloud infrastructure is a key enabler for implementing these different deployment scenarios while minimizing any potential downtime, allowing you to scale the infrastructure up and down on-demand, as they are rolled out.

#### Monitoring and observability and closing the feedback loop 
- Once the model is live, you will need the monitoring and observability infrastructure to understand how it is performing in production against real data.
- By capturing this data, you can close the data feedback loop. A human in the loop can analyze the new data captured from production, curate, and label it to create new training datasets for improving future models. 
- This enables models to adapt and creates a process of continuous improvement.

### CD4ML Technical Components 

![2](https://user-images.githubusercontent.com/23625821/125157024-dddc9000-e168-11eb-9742-9e69e6498bac.png)

- This whitepaper showcases MLOps solutions from AWS and the following AWS Partner Network (APN) companies that can deliver on the previously mentioned requirements:
  - Alteryx
  - Dataiku
  - Domino Data Lab
  - KNIME

- These solutions offer a broad spectrum of experiences that cater to builders and those who desire no-to-low-code experiences.
- AWS and the APN provide you with choices and the ability to tailor solutions that best fit your organization.

#### Alteryx
- Alteryx Connect – a collaborative data cataloging tool
- Alteryx Designer – desktop software that enables the assembly of code-free analytic workflows and apps

- Alteryx Server – an analytical hub that allows users to scale their analytic capabilities in the cloud or on premises on enterprise hardware
- Alteryx Promote – a deployable, containerized solution that enables the easy deployment of machine learning models as highly available REST APIs

![1](https://user-images.githubusercontent.com/23625821/125181648-3d897880-e207-11eb-8d21-defccf3741e1.png)

#### Dataiku 
- Dataiku is one of the world’s leading AI and machine learning platforms, supporting agility in organizations’ data efforts via collaborative, elastic, and responsible AI, all at enterprise scale.
- Dataiku provides a unified user interface (UI) to orchestrate the entire machine learning lifecycle, from data connectivity, preparation, and exploration to machine learning model building, deployment, monitoring, and everything in between.

![2](https://user-images.githubusercontent.com/23625821/125205365-1f149300-e282-11eb-975e-0ad3c305bb8e.png)

#### Domino Data Lab 
- It was founded to accelerate the work of code-first data scientists & help organizations run core components of their business on the models they develop. 
- It pioneered the Data Science Platforms category, and today powers data science research at over 20% of Fortune 100 companies.

- Domino brings order to the chaos of enterprise data science through:
  - Instant access to compute – IT-approved, self-service, elastic
  - Data, code, environment, and model management – automatically versioned, searchable, shareable, and always accessible
  - Openness – open source and proprietary IDEs and analytical software, all containerized under one platform and deployed on premises or in the cloud
  - Full reproducibility of research – central knowledge management framework, with all units of work tied together
  - Collaboration – for teams and for enterprises, with access to comprehensive search capabilities to share all key assets and collaborate on projects
  - Easy deployment – models, web apps, other data products
  - Platform and project management – best-in-class governance and security for IT and data science leaders

#### KNIME
- KNIME is software used to create and productionize data science in one easy and intuitive environment.
- This enables every stakeholder in the data science process to focus on what they do best.
- KNIME supports the data science lifecycle from data discovery to production model deployment, all with a single workflow paradigm.
- There are two tools to cover the entire data science lifecycle: 
   - KNIME Analytics Platform
   - KNIME Server
 
- KNIME on AWS: 
  - Both KNIME Analytics Platform and KNIME Server are offered in various forms in the <a href="https://docs.knime.com/2019-06/aws_marketplace_server_guide/index.html#introduction"> AWS Marketplace </a>. 
   
### AWS Reference Architecture

![1](https://user-images.githubusercontent.com/23625821/125401941-1cb25600-e3b4-11eb-8dfb-cc67db76ac41.png)

### Conclusion
- The APN ML community provides customers with a choice of outstanding solutions for MLOps.
- The following links can help you facilitate the next steps.
  - AWS – Connect with vetted MLOps consultants and try Amazon SageMaker for free.
  - Alteryx – Get started with the Alteryx Intelligence Suite Starter Kit.
  - Dataiku – Launch a free-trial of Dataiku DSS from the AWS Marketplace.
  - Domino Data Lab – Launch a free-trial of Domino Data Lab’s managed offering.
  - KNIME – Launch a free-trial of the KNIME Server from the AWS Marketplace.
  - ThoughtWorks – Learn more about ThoughtWorks’ CD4ML.
  
