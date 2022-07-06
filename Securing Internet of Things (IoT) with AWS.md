This whitepaper is a detailed look at how customers can use AWS security services to secure their Internet of Things (IoT) workloads in consumer and industrial environments. This paper is intended for senior-level program owners, decision makers, and security practitioners considering secure enterprise adoption of consumer and industrial IoT (IIoT) solutions.

# Introduction

* IoT technology allows organizations to optimize processes, enhance product offerings, and transform customer experiences in a variety of ways. 

* Although business leaders are excited about the way in which their businesses can benefit from this technology, it is important for them to consider the complexity and security risks associated with deploying IoT solutions. This is due, in part, to a lack of understanding of how to adopt security best practices to the new technologies, as well as a struggle with disparate, incompatible, and sometimes immature security offerings that fail to properly secure deployments, leading to an increased risk for customer or business owner data. 

* This paper provides guidance on how to understand, approach and meet your security, risk and compliance objectives when deploying IoT solutions with AWS.

* Organizations are eager to deliver smart services that can drastically improve the quality of life for populations, business operations and intelligence, quality of care from service providers, smart city resilience, environmental sustainability, and a host of scenarios yet to be imagined. 

* Most recently, AWS has seen an increase in IoT adoption from manufacturing, the healthcare sector and municipalities, with other industries expected to follow in the near term. Many municipalities are early adopters and are taking the lead when it comes to integrating modern technologies, such as IoT. For example:
 * **Kansas City, Missouri** – Kansas City created a unified smart city platform to manage new systems operating along its KC streetcar corridor. Video sensors, pavement sensors, connected street lights, a public Wi-Fi network, and parking and traffic management have supported a 40% reduction in energy costs, $1.7 billion in new downtown development, and 3,247 new residential units.
 * **City of Chicago, Illinois** – Chicago is installing sensors and cameras in intersections to detect pollen count and air quality for its citizens.
 * **City of Catania, Italy** – Catania developed an application to let commuters know where the closest open parking spot is on the way to their destination.
 * **City of Recife, Brazil** – Recife uses tracking devices placed on each waste collection truck and cleaning trolley. The city was able to reduce cleaning costs by $250,000 per month, while improving service reliability and operational efficiency. 
 * **City of Newport, Wales, UK** – Newport deployed smart city IoT solutions to improve air quality, flood control, and waste management in just a few months.
 * **Jakarta, Indonesia** – Being a city of 28 million residents that often deals with flooding, Jakarta is harnessing IoT to detect water levels in canals and lowlands, and is using social media to track citizen sentiment. Jakarta is also able to provide early warning and evacuation to targeted neighborhoods so that the government and first responders know which areas are most in need and can coordinate the evacuation process.

* At AWS, security is our highest priority, and this mandate includes supporting AWS IoT services and customers. AWS invests significant resources into ensuring that security is incorporated into every layer of its services, extending that security out to devices with IoT. 

* Helping to protect the confidentiality, integrity, and availability of customer systems and data, while providing a safe, scalable, and secure platform for IoT solutions is a priority for AWS. 

* AWS also provides design principles for deploying IoT securely on AWS. Found in the Security pillar of the AWS IoT Lens for the Well-Architected Framework, the design principles are:
Manage device security lifecycle holistically – Data security starts at the design phase, and ends with the retirement and destruction of the hardware and data. It is important to take a complete approach to the security lifecycle of your IoT solution to maintain your competitive advantage and retain customer trust.
 * **Ensure least privilege permissions** – Devices should all have fine-grained access permissions that limit which topics a device can use for communication. By restricting access, one compromised device will have fewer opportunities to impact any other devices.
 * **Secure device credentials at rest** – Devices should securely store credential information at rest using mechanisms such as a dedicated crypto element or secure flash.
 * **Implement device identity lifecycle management** – Devices maintain a device identity from creation through end of life. A well-designed identity system will keep track of a device’s identity, track the validity of the identity, and proactively extend or revoke IoT permissions over time. 
 * **Take a holistic view of data security** – IoT deployments involving a large number of remotely deployed devices present a significant attack surface for data theft and privacy loss. Use a model such as the Open Trusted Technology Provider Standard to systemically review your supply chain and solution design for risk and then apply appropriate mitigations.

* Although the IoT Lens provides a checklist and some examples for these design principles, it does not offer prescriptive guidance for securing industrial and consumer IoT applications, which this whitepaper will do.

# Security challenges and focus areas

* Security risks and vulnerabilities have the potential to compromise the security and privacy of customer data in an IoT application. Coupled with the growing number of connected devices, and the data generated, the potential for security events raises questions about how to address security risks posed by IoT devices and device communication to and from the cloud. 

* Common customer concerns regarding risks focus on the security and encryption of data while in transit to and from the cloud, or in transit from edge services to and from the device, along with patching of devices, device and user authentication, and access control. Another class of security risks stem from protecting physical devices. 

* Hardware-based security, such as using Trusted Platform Modules (TPMs), can protect the unique identities and sensitive data on a device and protect it from manipulative events such as probing of open interfaces on the device.

* Addressing these risks by securing IoT devices is essential, not only to maintain data integrity, but to also protect against security events that can impact the reliability of devices. 

* As devices can send large amounts of sensitive data over the internet, and end users are empowered to directly control a device, the security of “things” must permeate every layer of the solution. This whitepaper walks through the ability to integrate security into each of these layers using cloud-native tools and services.

* The foundation of an IoT solution must involve security throughout the process or else risking costly recalls or expensive retrofitting when poor security implementations lead to customer issues or downtimes. 

* Getting the right foundations in place makes it easier to adjust to changing conditions and makes it possible to layer on services capable of continuously auditing IoT configurations to ensure that they do not deviate from security best practices and respond if they do. After a deviation is detected, alerts should be raised so appropriate corrective action can be implemented—ideally, automatically.
 
* To keep up with the entry of connected devices into the marketplace, as well as the threats coming from online, it is best to implement services that address each part of the IoT ecosystem and overlap in their capability to secure and protect, audit and remediate, and manage fleet deployments of IoT devices (with or without connection to the cloud). 

* In addition, with the accelerated adoption of Industrial IoT (IIoT) connecting operational technologies (OT) such as industrial control systems (ICS) to the internet, new security challenges have arisen. 

* OT environments are leveraging more IT solutions to improve productivity and efficiency of production operations. This convergence of IT and OT systems creates risk management difficulties that need to be controlled.

* Operational technology controls physical assets and equipment such that if there is unintended access, it could impact outages of critical services. To address these emerging concerns, customers must evaluate the unique considerations these bring, and apply the appropriate security considerations. 

* In later sections, this whitepaper provides prescriptive guidance on addressing the security concerns related to various IoT use cases including consumer, enterprise, and industrial.


# AWS IoT services and compliance

* AWS serves a variety of customers, including those in regulated industries. Providing highly secure and resilient infrastructure and services to our customers is a top priority for AWS. 

* AWS has integrated a risk and compliance program throughout the organization, including AWS IoT services. This program aims to manage risk in all phases of service design and deployment and continually improve and reassess the organization’s risk-related activities. 

* AWS regularly undergoes independent third-party attestation audits to provide assurance that control activities are operating as intended. More specifically, AWS is audited against a variety of global and regional security frameworks dependent on region and industry. 

* AWS participates in over 50 different audit programs such as International Standards Organization 27001 (ISO), Payment Card Industry Data Security Standard (PCI), and the Service Organization Control (SOC) reports, among other international, national, and sectoral accreditations.
 
* AWS is sensitive to the fact that customers might have specific compliance requirements that must be demonstrated and complied with. Keeping this in mind, AWS continually adds services that align with compliance programs based on customer demand. 

* For more information, refer to AWS Services in Scope by Compliance Program and AWS Artifact for on-demand access to AWS’ compliance reports.


# Using provable security to enhance IoT – An industry differentiator

* New security services and technologies are being built at AWS to help enterprises secure their IoT and edge devices. In particular, AWS has recently launched checks within AWS IoT Device Defender, powered by an AI technology known as automated reasoning, which uses mathematical proofs for formal verification to determine if there is unintended access to the devices. 

* The AWS IoT Device Defender is an example of how customers can directly use automated reasoning to audit and monitor their own devices. Internally, AWS has used automated reasoning to verify the memory integrity of code running on FreeRTOS and to protect against malware. 

* Investment in automated reasoning to provide scalable assurance of secure software, referred to as provable security, allows customers to operate sensitive workloads on AWS.

* AWS Zelkova uses automated reasoning to prove that customer data access controls are operating as intended. The access control checks in AWS IoT Device Defender are powered by Zelkova, allowing customers to ensure their data is appropriately protected. 

* An AWS IoT policy is overly permissive if it grants access to resources outside of a customer’s intended security configuration. The Zelkova-powered controls integrated into AWS IoT Device Defender verify that policies don’t allow actions restricted by the customer’s security configuration and that intended resources have permissions to perform certain actions.

* Other automated reasoning tools have been used to help secure the AWS IoT infrastructure. The open source formal verification tool CBMC has been used to strengthen the foundations of the AWS IoT infrastructure by proving the memory safety of critical components of the FreeRTOS operating system. 

* A proof of memory safety minimizes the potential of certain security issues, allowing customers and developers to focus on securing other areas in their environment. The memory safety proofs are automatically checked every time a code change is made to FreeRTOS, providing both customers and AWS developers ongoing confidence in the security of these critical components.

* Automated reasoning continues to be implemented across a variety of AWS services and features, providing heightened levels of security assurance for critical components of the AWS Cloud. AWS continues to deploy automated reasoning to develop tools for customers as well as internal infrastructure verification technology for the AWS IoT stack.


# Implementing IoT security using AWS services

* As noted in the previous sections, IoT implementations can have some very unique challenges not present in traditional IT deployments. For example, deploying a consumer IoT device, such as what iRobot has done using AWS to handle scale and spikes, can introduce a new classification of threats to be addressed.

* Industrial deployments of IoT (IIoT) devices (such as how SKF and Volkswagen have used AWS IoT to optimize its production processes, reduce costs, and provide a better experience to its customers) offer another unique set of security considerations.

* Lastly, operational technology (OT) or SCADA-based IoT deployments, such as Enel using AWS IoT to get electricity to their customers can require more thought around reliability and anomaly detection. And this is not an exhaustive list. 

* For these use cases there are some common security best practices that can be addressed using AWS services. How enterprises choose to invest in each of these will be based on their risk model.

* The following are 10 best practices to build a secure IoT deployment.

 * 1. Conduct a formal security risk assessment using a common framework (such as MITRE ATT&CK). Use this to inform system design.

 * Maintain an asset inventory of all IoT assets, including IT assets required to maintain IoT operations. Categorize them by safety, criticality, ability to patch, and other actionable criteria.

 * 3. Provision IoT devices and systems with unique identities and credentials. Apply authentication and access control mechanisms at each system interface.

 * 4. Define appropriate update mechanisms for software and firmware updates.

 * 5. Encrypt persistent data at rest.

 * 6. Encrypt all data in transit, including sensor and device data, administration, provisioning, and deployments.

 * 7. Secure both the IoT environment and supporting IT environments to the same level of criticality following a well-documented standard. This is especially true for gateways that serve as boundaries between systems.

 * 8. Deploy security auditing and monitoring mechanisms across your IoT environment and relevant IT systems.

 * 9. Create incident response playbooks, and build automation as your security response matures.

 * 10. Create and test business continuity and recovery plans.

More details about each best practice can be found [here]().

# Augmenting security practices for industrial control systems, operational technology, and industrial IoT

* Industrial IoT is driving changes to the operational technology (OT) landscape, making it more connected. OT such as industrial control systems (ICS) and supervisory control and data acquisition systems (SCADA) is the use of hardware and software to monitor and control physical assets and production operation. 

* Industrial internet of things (IIoT) is the connection of ICS with enterprise systems, business processes, and analytics, and is a key enabler for smart manufacturing and Industry 4.0. 

* The convergence of IT and OT systems is creating a mix of technologies that were designed for remote network environments and ones that were not, which creates risk management difficulties that need to be addressed. This OT and IT convergence introduces new security risks and challenges in the industrial environment which need to be properly managed.

* To help companies plan their industrial digital transformation safely and securely, AWS recommends augmenting general best practices with these fundamentals in ICS and OT, and IIoT security. More details about it can be found [here]().


# Government contributions to IoT security

* Although private sector organizations are actively deploying IoT in use cases such as healthcare, industrial construction, and low-power consumer goods, governments at the national and local levels are beginning to address IoT adoption and security. Some key players and their roles include:
 
 * **National Institute of Standards and Technology** – Spearheading multiple whitepapers and industry efforts to define and reduce risk of IoT environments.
 * **US Department of Defense** – Providing policy recommendations for agencies addressing IoT risks.
 * **Federal Trade Commission** – Pursuing action against device manufacturers who fail to meet the reasonable data security bar.

* In the US, some states such as California are enacting their own rules, and globally, other countries such as the UK are advancing regulation as well. For more details on these developments, refer to Appendix 2 – Government involvement in IoT.


# Key IoT security takeaways

* Despite the number of best practices available, there is no one-size-fits-all approach to mitigating the risks to IoT solutions. 

* Depending on the device, system, service, and environment in which the devices are deployed, different threats, vulnerabilities, and risk tolerances exist for customers to consider. 
* Here are key takeaways to help incorporate complete security across data, devices, and cloud services:
 * **1.Incorporate security in the design phase.**

* The foundation of an IoT solution starts and ends with security. Because devices may send large amounts of sensitive data, and end users of IoT applications may also have the ability to directly control a device, the security of things must be a pervasive design requirement. Security is not a static formula; IoT applications must be able to continuously model, monitor, and iterate on security best practices.

* A challenge for IoT security is the lifecycle of a physical device and the constrained hardware for sensors, microcontrollers, actuators, and embedded libraries. These constrained factors may limit the security capabilities each device can perform. With these additional dynamics, IoT solutions must continuously adapt their architecture, firmware, and software to stay ahead of the changing security landscape. 

* Although the constrained factors of devices can present increased risks, hurdles, and potential tradeoffs between security and cost, building a secure IoT solution must be the primary objective for any organization.

 * **2.Build on recognized IT security and cybersecurity frameworks.**
 
* AWS supports an open, standards-based approach to promote secure IoT adoption. When considering the billions of devices and connection points necessary to support a robust IoT ecosystem for consumer, industrial, and public sector use, interoperability is vital. 

* Thus, AWS IoT services adhere to industry standard protocols and best practices. Additionally, AWS IoT Core supports other industry-standard and custom protocols, allowing devices to communicate with each other even if they are using different protocols. 

* AWS is a strong proponent of interoperability so that developers can build on top of existing platforms to support evolving customer needs. AWS also supports a thriving partner ecosystem to expand the menu of choices and stretch the limits of what is possible for customers. 

* Applying globally recognized best practices carries a number of benefits across all IoT stakeholders including:
 * Repeatability and reuse, instead of re-starting and re-doing
 * Consistency and consensus to promote the compatibility of technology and interoperability across geographical boundaries
 * Maximizing efficiencies to accelerate IT modernization and transformation

 * **3.Focus on impact to prioritize security measures.**

* Attacks or abnormalities are not identical and may not have the same impact on people, business operations, and data. Understanding customer IoT ecosystems and where devices will operate within this ecosystem informs decisions on where the greatest security risks are—within the device as part of the network or physical component.

* Focusing on the risk impact assessment and consequences is critical for determining where security efforts should be directed along with who is responsible for those efforts in the IoT ecosystem.

 * **4.Start with using zero-trust security principles.**

* Zero-trust principles are intended for an organization’s infrastructure, which includes operational technology (OT), IT systems, IoT, and Industrial Internet of Things (IIoT). 

* Traditional security models rely heavily on network segmentation and give high levels of trust to devices based on their network presence. In comparison, zero trust requires your users, devices, and systems to prove their trustworthiness, and it enforces fine- grained, identity-based rules that govern access to applications, data, and other assets. AWS provides guidance on how to implement zero trust IoT solutions with AWS IoT.

# Conclusion

* Along with an exponential growth in connected devices, each thing in IoT communicates packets of data that require reliable connectivity, storage, and security. With IoT, an organization is challenged with managing, monitoring, and securing immense volumes of data and connections from dispersed devices. But this challenge doesn’t have to be a roadblock in a cloud-based environment. 

* In addition to scaling and growing a solution in one location, cloud computing enables IoT solutions to scale globally and across different physical locations while lowering communication latency and allowing for better responsiveness from devices in the field.

* AWS offers a suite of IoT services with complete security, including services to operate and secure endpoints, gateways, platforms, and applications as well as the traffic traversing across these layers. 

* This integration simplifies secure use and management of devices and data that continually interact with each other, allowing organizations to benefit from the innovation and efficiencies IoT can offer while maintaining security as a priority. 

* AWS offers customers a defense in depth approach with multiple security services and an easier, faster and more cost-effective path towards comprehensive, continuous and scalable IoT security, compliance and governance solutions.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/Security/Securing_IoT_with_AWS.pdf?did=wp_card&trk=wp_card)