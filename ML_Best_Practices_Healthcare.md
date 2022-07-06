
# Machine Learning Best Practices in Healthcare and Life Sciences


## An overview of security and good machine learning compliance practices using AWS


- This whitepaper describes how AWS approaches machine learning (ML) in a regulated environment and provides guidance on good ML practices using AWS products.

- This whitepaper takes into consideration the principles described in the Proposed <a href="https://www.fda.gov/files/medical%20devices/published/US-FDA-Artificial-Intelligence-and-Machine-Learning-Discussion-Paper.pdf"> Regulatory Framework </a> for Modifications to AI/ML-Based Software as a Medical Device (SaMD) discussion paper.

- This content has been developed based on experience with and feedback from AWS pharmaceutical and medical device customers, as well as software partners. 


### Life sciences at AWS

AI/ML in the life sciences industry fall under five main categories:


- Research and discovery — Use cases include molecular structure prediction, antibody binding affinity prediction, outcome prediction for patients with certain
biomarkers, patient data enrichment and search, the classification of tissue samples, and so on.

- Clinical development — Use cases include forecasting and optimization of trial timelines, optimization of inclusion/exclusion criteria and site selection, protocol document enrichment, and so on.

- Manufacturing and supply chain — Use cases include predictive maintenance of equipment, computer vision for effective line clearance, optimization of manufacturing process staging, vial inspection, and so on.

- Commercial — Use cases include predicting healthcare providers with relevant patient bases, identifying next best action for sales and marketing, annotation and management of existing promotional materials, and so on. 

- Post-market surveillance and patient support — Use cases include forecasting patient cost, automation of adverse event detection from social media or call centers, patient outcome prediction, and so on.



##### Reference Implementation of Good machine learning practices

![1](https://user-images.githubusercontent.com/23625821/143559110-27689442-ddf2-48e0-9925-85dacc30f14c.png)

This framework relies on the principle of a “predetermined change control plan.” 

The predetermined change control plan includes the types of anticipated modifications, (SaMD Pre-Specifications) based on the retraining and model update strategy. 

- And the associated methodology (Algorithm Change Protocol) being used to implement those changes in a controlled manner that manages risks to patients. 



- SaMD Pre-Specifications (SPS) — A SaMD manufacturer’s anticipated modifications to “performance” or “inputs,” or changes related to the “intended use” of AI/ML-based SaMD. These are the types of changes the manufacturer plans to achieve when the SaMD is in use. The SPS draws a “region of potential changes” around the initial specifications and labeling of the original device. This is what the manufacturer intends the algorithm to become as it learns.


- Algorithm Change Protocol (ACP) — Specific methods that a manufacturer has in place to achieve and appropriately control the risks of the anticipated types of modifications delineated in the SPS. The ACP is a step-by-step delineation of the data and procedures to be followed so that the modification achieves its goals and the device remains safe and effective after the modification. The preceding figure provides a general overview of the components of an ACP. 


### Challenges to support AI/ML enabled GxP workloads

Developing AI/ML-enabled GxP workloads raise the following challenges:


- Building a secure infrastructure that complies with a stringent regulatory process working on the public cloud and aligning to the FDA framework for AI/ML

- Supporting an AI/ML-enabled solution for GxP workloads covering:

    - Reproducibility
    - Traceability
    - Data integrity

- Monitoring the Machine Learning model with respect to various changes to parameters and data

- Handling model uncertainty and confidence calibration

- Making AI/ML models interpretable



### Provision a secure and GXP-compliant ML environment

For healthcare and life sciences customers, the security and privacy of an ML environment is paramount. 

It is therefore strongly recommended that you protect your environment against unauthorized access, privilege escalation, and data exfiltration. 

Common considerations when you set up a secure ML environment include:

    - Platform qualification
    - Compute and network isolation
    - Authentication and authorization 


#### Platform qualification

The validated state of ML models may be brought into question if the underlying IT infrastructure is not maintained in a demonstrable state of control and regulatory compliance. 

Similarly, data integrity can also be affected by problems related to IT infrastructure. 

Therefore, to support a culture of quality and operational excellence, it is important to qualify your underlying platform and then maintain it under a state of control.



#### Compute and network isolation








<a href="https://d1.awsstatic.com/whitepapers/ML-best-practices-health-science.pdf?did=wp_card&trk=wp_card">  Reference </a>




