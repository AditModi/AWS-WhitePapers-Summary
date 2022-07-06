# Security of AWS CloudHSM Backups

- AWS CloudHSM clusters provide high availability and redundancy by distributing cryptographic operations across all hardware security modules (HSMs) in the cluster.
- Backup and restore is the mechanism by which a new HSM in a cluster is synchronized.
- This whitepaper provides details on thecryptographic mechanisms supporting backup and restore functionality, and the security mechanisms protecting the Amazon Web Services (AWS)-managed backups.
- It also provides in-depth information on how backups are protected in all three phases of the CloudHSM backup lifecycle process: Creation, Archive, and Restore.
- For the purposes of this whitepaper, it is assumed that you have a basic understanding of AWS CloudHSM and cluster architecture.

- AWS offers two options for securing cryptographic keys in the AWS Cloud: AWS Key Management Service (AWS KMS) and AWS CloudHSM.
- AWS KMS is a managed service that uses hardware security modules (HSMs) to protect the security of your encryption keys. 
- AWS CloudHSM delivers fully managed HSMs in the AWS Cloud, which allows you to add secure, validated key storage and high-performance crypto acceleration to your AWS applications.
- CloudHSM offers you the option of single-tenant access and control over your HSMs.
- CloudHSM is based on Federal Information Processing Standards (FIPS) 140-2 Level 3 validated hardware.
- CloudHSM delivers fully managed HSMs in the AWS Cloud. CloudHSM delivers all the benefits of traditional HSMs including secure generation, storage, and management of cryptographic keys used for data encryption that are controlled and accessible only by you.
- HSM capacity can be scaled quickly by adding and removing HSMs from your cluster on demand. 
- The backup and restore functionality of CloudHSM is what enables scalability, reliability, and high availability in CloudHSM. A key aspect of the backup and restore feature is a secure backup protocol that CloudHSM uses to back up your cluster. 
- This paper takes an in-depth look at the security mechanisms in place around this feature.

## AWS CloudHSM: Managed by AWS, controlled by you
- AWS CloudHSM provides HSMs in a cluster. A cluster is a collection of individual HSMs that AWS CloudHSM keeps in sync. 
- You can think of a cluster as one logical HSM. When you perform a key generation task or operation on one HSM in a cluster, the other HSMs in that cluster are automatically kept up to date. 
- Each HSM in a cluster is a single-tenant HSM under your control. 
- At the hardware level, each HSM includes hardware-enforced isolation of crypto operations and key storage. 
- Each HSM runs on dedicated cryptographic cores.
- Each HSM appears as a network resource in your virtual private cloud (VPC).

- AWS manages the HSM on your behalf, performing functions such as health checks, backups, and synchronization of HSMs within a cluster
- However, you alone control the user accounts, passwords, login policies, key rotation procedures, and all aspects of configuring and using the HSMs.
- The implication of this control is that your cryptographic data is secure from external compromise. 
- This is important to financial applications subject to PCI regulations, healthcare applications subject to HIPAA regulations, and streaming video solutions subject to contractual DRM requirements.

- You interact with the HSMs in a cluster via the AWS CloudHSM client.
- Communication occurs over an end-to-end encrypted channel. 
- AWS does not have visibility into your communication with your HSM, which occurs within this end-to-end encrypted channel.

## High availability
- CloudHSM makes scalability and high availability simple without compromising security.
- When you use CloudHSM you begin by creating a cluster in a particular AWS Region. 
- A cluster can contain multiple individual HSMs. For idle workloads, you can delete all HSMs and simply retain the empty cluster. 
- For production workloads, you should have at least two HSMs spread across multiple Availability Zones. 

- CloudHSM automatically synchronizes and load balances the HSMs within a cluster.
- The CloudHSM client load-balances cryptographic operations across all HSMs in the cluster based on the capacity of each HSM for additional processing. 
- If a cluster requires additional throughput, you can expand your cluster by adding more HSMs through a single API call or a click in the CloudHSM console.

- When you expand a cluster, CloudHSM automatically provisions a new HSM as a clone of the other HSMs in the cluster. 
- This is done by taking a backup of an existing HSM and restoring it to the newly added HSM. When you delete an HSM from a cluster, a backup is automatically taken. 

- This way, when you create a new HSM later, you can pick up where you left off. 
- Should an HSM fail for any reason, the service will automatically replace it with a new, healthy HSM. 
- This HSM is restored from a backup of another HSMs in the cluster if available. 
- Otherwise, the new HSM is restored from the last available backup taken for the cluster.

## CloudHSM cluster backups
- Backups are initiated, archived, and restored by CloudHSM. 
- A backup is a complete, encrypted snapshot of the HSM.
- Each AWS-managed backup contains the entire contents of the HSM, including keys, certificates, users, policies, quorum settings, and configuration options.
- This includes:

    • Certificates on the HSM, including the cluster certificate.  
    • All HSM users (COs, CUs, and AU).  
    • All key material on the HSM.  
    • HSM configurations and policies.  

### Archiving a backup 
- CloudHSM stores the cluster backups in a service-controlled Amazon S3 location in the same AWS Region as your cluster.

![image](https://user-images.githubusercontent.com/23625821/130732655-561b5e18-acf2-496b-a00e-0650b2eacee8.png)

### Restoring a backup
- Backups are used in two scenarios: 
   - When you provision a new cluster using an existing backup.
   - When a second (or subsequent) HSM is added to a cluster, or when CloudHSM automatically replaces an unhealthy HSM.

- In both scenarios, the backup is restored to a newly created HSM.
- During restoration, the backup is decrypted within an HSM. 
- The decryption relies on a set of keys available only within an authentic hardware instance from the original manufacturer, installed in the AWS Cloud.

- Note that while CloudHSM manages backups, the service does not have any access to the data, cryptographic material, user information, and the keys encapsulated within the backup. 
- Specifically, AWS has no way to recover your keys if you lose your access credentials to log in to the HSM.

### Security of backups
- The CloudHSM backup mechanism has been validated under <a href="https://csrc.nist.gov/csrc/media/projects/cryptographic-module-validation-program/documents/security-policies/140sp2850.pdf"> FIPS 140-2 Level 3 </a> .
- A backup taken by an HSM configured in FIPS-mode cannot be restored to an HSM that is not also in FIPS-mode.
- An HSM in FIPS-mode is running production firmware provided by the manufacturer and signed with a FIPS production key.
- This ensures other parties cannot forge the firmware.

#### Key hierarchy
- A backup is encrypted within the HSM before it is provided to CloudHSM for archival. The backup is encrypted using a backup encryption key, described in the following section.

![image](https://user-images.githubusercontent.com/23625821/130733652-3c935b13-782f-4ae2-bee7-1549153de7d7.png)

#### Manufacturer’s key backup key (MKBK)
- The manufacturer’s key backup key (MKBK) exists in the HSM hardware provided by the manufacturer.
- The MKBK cannot be accessed or used by any user or for any purpose other than the generation of the backup encryption key.
- Specifically, AWS does not have access to or visibility into the MKBK.


#### AWS key backup key (AKBK)
- The AWS key backup key (AKBK) is securely installed by the CloudHSM service when the hardware is placed into operation within the CloudHSM fleet.
- This key is unique to hardware installed by AWS within our CloudHSM infrastructure.
- The AKBK is generated securely within an offline FIPS-compliant hardware security module, and loaded under two-person control into newly commissioned CloudHSM hardware.


#### Backup Encryption Key (BEK) 
- The backup of the HSM is encrypted using a backup encryption key (BEK). 
- The BEK is an AES-256 key that is generated within the HSM when a backup is requested. 
- The HSM uses the BEK to encrypt its backup. 
- The encrypted backup includes a wrapped copy of the BEK.
- The BEK is wrapped with an AES 256-bit wrapping key using a FIPS-approved
AES key wrapping method.


### Refernces

<a href="https://d1.awsstatic.com/whitepapers/Security/security-of-aws-cloudhsm-backups.pdf"> Original paper </a>
