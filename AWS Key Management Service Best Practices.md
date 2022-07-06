
# AWS Key Management Service Best Practices


AWS Key Management Service (AWS KMS) is a managed service that allows you to concentrate on the cryptographic needs of your applications while Amazon Web Services (AWS) manages availability, physical security, logical access control, and maintenance of the underlying infrastructure. 

Further, AWS KMS allows you to audit usage of your keys by providing logs of all API calls made on them to help you meet compliance and regulatory requirements.

Customers want to know how to effectively implement AWS KMS in their environment. 

This whitepaper discusses how to use AWS KMS for each capability described in the AWS Cloud Adoption Framework Security Perspective whitepaper, including the differences between the different types of customer master keys, using AWS KMS key policies to ensure least privilege, auditing the use of the keys, and listing some use cases that work to protect sensitive information within AWS.


### Identity and Access Management

The Identity and Access Management capability provides guidance on determining the controls for access management within AWS KMS to secure your infrastructure according to established best practices and internal policies.

#### AWS KMS and IAM Policies

You can use AWS Identity and Access Management (IAM) policies in combination with key policies to control access to your customer master keys (CMKs) in AWS KMS. 

This section discusses using IAM in the context of AWS KMS.  It doesn’t provide detailed information about the IAM service. 


Policies attached to IAM identities (that is, users, groups, and roles) are called identity-based policies (or IAM policies). 

Policies attached to resources outside of IAM are called resource-based policies. 

In AWS KMS, you must attach resource-based policies to your customer master keys (CMKs). 


These are called key policies. All KMS CMKs have a key policy, and you must use it to control access to a CMK. 

IAM policies by themselves are not sufficient to allow access to a CMK, although you can use them in combination with a CMK key policy. 

To do so, ensure that the CMK key policy includes the policy statement that enables IAM policies.


#### Key Policies

Key policies are the primary way to control access to CMKs in AWS KMS. Each CMK has a key policy attached to it that defines permissions on the use and management of the key. 

The default policy enables any principals you define, as well as enables the root user in the account to add IAM policies that reference the key. 

We recommend that you edit the default CMK policy to align with your organization’s best practices for least privilege. 

To access an encrypted resource, the principal needs to have permissions to use the resource, as well as to use the encryption key that protects the resource. 

If the principal does not have the necessary permissions for either of those actions, the request to use the encrypted resource will be denied.

#### Least Privilege / Separation of Duties

Key policies specify a resource, action, effect, principal, and conditions to grant access to CMKs. 

Key policies allow you to push more granular permissions to CMKs to enforce least privilege. 

For example, an application might make a KMS API call to encrypt data but there is no use case for that same application to decrypt data. 

In that use case, a key policy could grant access to the kms:Encrypt action but not kms:Decrypt and reduce the possibility for exposure. 

Additionally, AWS allows you to separate the usage permissions from administration permissions associated with the key. 

This means that an individual may have the ability to manipulate the key policy, but might not have the necessary permissions to use the key for cryptographic functions.

### Cross Account Sharing of Keys

Delegation of permissions to a CMK within AWS KMS can occur when you include the root principal of a trusted account within the CMK key policy. 

The trusted account then has the ability to further delegate these permissions to IAM users and roles within their own account using IAM policies. 

While this approach may simplify the management of the key policy, it also relies on the trusted accounts to ensure that the delegated permissions are managed. 

The other approach would be to explicitly manage permissions to all authorized users using only the KMS key policy, which could make the key policy complex and less manageable. 

Regardless of the approach you take, the specific trust should be broken out on a per key basis to ensure that you adhere to the least privilege model.



### CMK Grants

Key policy changes follow the same permissions model used for policy editing elsewhere in AWS. 

That is, users either have permission to change the key policy or they do not. 

Users with the PutKeyPolicy permission for a CMK can completely replace the key policy for a CMK with a different key policy of their choice. 

You can use key policies to allow other principals to access a CMK, but key policies work best for relatively static assignments of permissions. 

To enable more granular permissions management, you can use grants. 

Grants are useful when you want to define scoped-down, temporary permissions to use your CMK on your behalf in the absence of a direct API call from you.

### Encryption Context

In addition to limiting permission to the AWS KMS APIs, AWS KMS also gives you the ability to add an additional layer of authentication for your KMS API calls utilizing encryption context. 

The encryption context is a key-value pair of additional data that you want associated with AWS KMS-protected information. 

This is then incorporated into the additional authenticated data (AAD) of the authenticated encryption in AWS KMS-encrypted ciphertexts. 

If you submit the encryption context value in the encryption operation, you are required to pass it in the corresponding decryption operation. 

You can use the encryption context inside your policies to enforce tighter controls for your encrypted resources.

Because the encryption context is logged in CloudTrail, you can get more insight into the usage of your keys from an audit perspective. 

Be aware that the encryption context is not encrypted and will be visible within CloudTrail logs. 

The encryption context should not be considered sensitive information and should not require secrecy.


### Multi-Factor Authentication

To provide an additional layer of security over specific actions, you can implement an additional layer of protection using multi-factor authentication (MFA) on critical KMS API calls. 

Some of those calls are PutKeyPolicy, ScheduleKeyDeletion, DeleteAlias, and DeleteImportedKeyMaterial. 

This can be accomplished through a conditional statement within the key policy that checks for when or if an MFA device was used as part of authentication.

If someone attempts to perform one of the critical AWS KMS actions, the following CMK policy will validate that their MFA was authenticated within the last 300 seconds, or 5 minutes, before performing the action.


```json
{
 "Sid": "MFACriticalKMSEvents",
 "Effect": "Allow",
 "Principal": {
    "AWS": "arn:aws:iam::111122223333:user/ExampleUser"
 },
 "Action": [
   "kms:DeleteAlias",
   "kms:DeleteImportedKeyMaterial",
   "kms:PutKeyPolicy",
   "kms:ScheduleKeyDeletion"
 ],
 "Resource": "*",
 "Condition":{
    " NumericLessThan ":{"aws: MultiFactorAuthAge":"300"}
  }
}

```


### Detective Controls
 
The Detective Controls capability ensures that you properly configure AWS KMS to log the necessary information you need to gain greater visibility into your environment.

#### CMK Auditing

AWS KMS is integrated with CloudTrail. To audit the usage of your keys in AWS KMS, you should enable CloudTrail logging in your AWS account. 

This ensures that all KMS API calls made on keys in your AWS account are automatically logged in files that are then delivered to an Amazon Simple Storage Service (S3) bucket that you specify. Using the information collected by CloudTrail, you can determine what request was made, the source IP address from which the request was made, who made the request, when it was made, and so on. 

AWS KMS integrates natively with many other AWS services to make monitoring easy. 

You can use these AWS services, or your existing security tool suite, to monitor your CloudTrail logs for specific actions such as ScheduleKeyDeletion, PutKeyPolicy, DeleteAlias, DisableKey, DeleteImportedKeyMaterial on your KMS key. Furthermore, AWS KMS emits Amazon CloudWatch Events when your CMK is rotated, deleted, and imported key material in your CMK expires.


#### CMK Use Validation

In addition to capturing audit data associated with key management and use, you should ensure that the data you are reviewing aligns with your established best practices and policies. One method is to continuously monitor and verify the CloudTrail logs as they come in. 

Another method is to use AWS Config rules. By using AWS Config rules you can ensure that the configuration of many of the AWS services are set up appropriately. 

For example, with EBS volumes you can use the AWS Config rule ENCRYPTED_VOLUMES to validate that attached EBS volumes are encrypted.


A CMK can have a tag applied to it for a variety of purposes. 

The most common use is to correlate a specific CMK back to a business category (such as a cost center, application name, or owner). 

The tags can then be used to verify that the correct CMK is being used for a given action. 

For example, in CloudTrail logs, for a given KMS action you can verify that the CMK being used belongs to the same business category as the resource that it’s being used on. Previously, this might have required a look up within a resource catalog, but now this external lookup is not required because of tagging within AWS KMS as well as many of the other AWS services.


### Infrastructure Security

The Infrastructure Security capability provides you with best practices on how to configure AWS KMS to ensure that you have an agile implementation that can scale with your business while protecting your sensitive information.




#### Customer Master Keys

Within AWS KMS, your key hierarchy starts with a CMK. A CMK can be used to directly encrypt data blocks up to 4 KB or it can be used to secure data keys, which protect underlying data of any size.


CMKs can be broken down into two general types: AWS-managed and customer-managed. 

An AWSmanaged CMK is created when you choose to enable server-side encryption of an AWS resource under the AWS-managed CMK for that service for the first time (e.g., SSE-KMS). 

The AWS-managed CMK is unique to your AWS account and the Region in which it’s used. 

An AWS-managed CMK can only be used to protect resources within the specific AWS service for which it’s created. It does not provide the level of granular control that a customer-managed CMK provides. 

For more control, a best practice is to use a customer-managed CMK in all supported AWS services and in your applications. A customer-managed CMK is created at your request and should be configured based upon your explicit use case.


![1](https://user-images.githubusercontent.com/23625821/141607731-83fbeec0-91e6-4d36-b18a-e75090acc6a1.png)



#### Key Creation and Management

Since AWS makes creating and managing keys easy through the use of AWS KMS, we recommend that you have a plan for how to use the service to best control the blast radius around individual keys.

Previously, you may have used the same key across different geographic regions, environments, or even applications. With AWS KMS, you should define data classification levels and have at least one CMK per level. 

For example, you could define a CMK for data classified as “Confidential,” and so on. This ensures that authorized users only have permissions for the key material that they require to complete their job.

You should also decide how you want to manage usage of AWS KMS. 

Creating KMS keys within each account that requires the ability to encrypt and decrypt sensitive data works best for most customers, but another option is to share the CMKs from a few centralized accounts. 

Maintaining the CMKs in the same account as the majority of the infrastructure using them helps users provision and run AWS services that use those keys. AWS services don’t allow for cross-account searching unless the principal doing the searching has explicit List* permissions on resources owned by the external account. 

This can also only be accomplished via the CLI or SDK, and not through service console-based searches. Additionally, by storing the credentials in the local accounts, it might be easier to delegate permissions to individuals who know the IAM principals that require access to the specific CMKs. 

If you were sharing the keys via a centralized model, the AWS KMS administrators would need to know the full Amazon Resource Name (ARN) for all users of the CMKs to ensure least privilege. 

Otherwise, the administrators might provide overly permissive permissions on the keys.


#### Key Aliases

A key alias allows you to abstract key users away from the underlying Region-specific key ID and key ARN. 

Authorized individuals can create a key alias that allows their applications to use a specific CMK independent of the Region or rotation schedule. 

Thus, multi-Region applications can use the same key alias to refer to KMS keys in multiple Regions without worrying about the key ID or the key ARN. 

You can also trigger manual rotation of a CMK by pointing a given key alias to a different CMK. Similar to how Domain Name Services (DNS) allows the abstraction of IP addresses, a key alias does the same for the key ID. 

When you are creating a key alias, we recommend that you determine a naming scheme that can be applied across your accounts such as alias/Environment-Function-Service Team.

It should be noted that CMK aliases can’t be used within policies. This is because the mapping of aliases to keys can be manipulated outside the policy, which would allow for an escalation of privilege. Therefore, key IDs must be used in KMS key policies, IAM policies, and KMS grants.
 

### Using AWS KMS at Scale
 
AWS recommends using envelope encryption to scale your KMS implementation. Envelope encryption is the practice of encrypting plaintext data with a unique data key, and then encrypting the data key with a key encryption key (KEK). Within AWS KMS, the CMK is the KEK. 

You can encrypt your message with the data key and then encrypt the data key with the CMK. Then the encrypted data key can be stored along with the encrypted message. 

You can cache the plaintext version of the data key for repeated use, reducing the number of requests to AWS KMS. Additionally, envelope encryption can help to design your application for disaster recovery. You can move your encrypted data as-is between Regions and only have to re-encrypt the data keys with the Region-specific CMKs.


### Data Protection

The Data Protection capability addresses some of the common AWS use cases for using AWS KMS within your organization to protect your sensitive information.

#### Common AWS KMS Use Cases

##### Encrypting PCI Data Using AWS KMS

Since security and quality controls in AWS KMS have been validated and certified to meet the requirements of PCI DSS Level 1 certification, you can directly encrypt Primary Account Number (PAN) data with an AWS KMS CMK. 

The use of a CMK to directly encrypt data removes some of the burden of managing encryption libraries. 

Additionally, a CMK can’t be exported from AWS KMS, which alleviates the concern about the encryption key being stored in an insecure manner. As all KMS requests are logged in CloudTrail, use of the CMK can be audited by reviewing the CloudTrail logs. 

It’s important to be aware of the requests per second limit when designing applications that use the CMK directly to protect Payment Card Industry (PCI) data.


##### Secret Management Using AWS KMS and Amazon S3

Although AWS KMS primarily provides key management functions, you can leverage AWS KMS and Amazon S3 to build your own secret management solution.

Create a new Amazon s3 bucket to hold your secrets. Deploy a bucket policy onto the bucket to limit access to only authorized individuals and services. The secrets stored in the bucket utilize a predefined prefix per file to allow for granular control of access to the secrets. 

Each secret, when placed in the S3 bucket, is encrypted using a specific customer-managed KMS key. Furthermore, due to the highly sensitive nature of the information being stored within this bucket, S3 access logging or CloudTrail Data Events are enabled for audit purposes. Then, when a user or service requires access to the secret, they assume an identity within AWS that has permissions to use both the object in the S3 bucket as well as the KMS key. An application that runs in an EC2 instance uses an instance role that has the necessary permissions.



##### Encrypting Lambda Environment Variables

By default, when you create or update Lambda functions that use environment variables, those variables are encrypted using AWS KMS. When your Lambda function is invoked, those values are decrypted and made available to the Lambda code. You have the option to use the default KMS key for Lambda or specify a specific CMK of your choice.

To further protect your environment variables, you should select the “Enable encryption helpers” checkbox. By selecting this option, your environment variables will also be individually encrypted using a CMK of your choice, and then your Lambda function will have to specifically decrypt each encrypted environment variable that is needed.



##### Encrypting Data within Systems Manager Parameter Store

Amazon EC2 Systems Manager is a collection of capabilities that can help you automate management tasks at scale. 

To efficiently store and reference sensitive configuration data such as passwords, license keys, and certificates, the Parameter Store lets you protect sensitive information within secure string parameters.

A secure string is any sensitive data that needs to be stored and referenced in a secure manner. 

If you have data that you don't want users to alter or reference in clear text, such as domain join passwords or license keys, then specify those values using the Secure String data type. You should use secure strings in the following circumstances:

  - You want to use data/parameters across AWS services without exposing the values as clear text in commands, functions, agent logs, or CloudTrail logs.
  - You want to control who has access to sensitive data.
  - You want to be able to audit when sensitive data is accessed using CloudTrail.
  - You want AWS-level encryption for your sensitive data and you want to bring your own encryption keys to manage access.


By selecting this option when you create your parameter, the Systems Manager encrypts that value when it’s passed into a command and decrypts it when processing it on the managed instance. The encryption is handled by AWS KMS and can be either a default KMS key for the Systems Manager or you can specify a specific CMK per parameter.


### Enforcing Data at Rest Encryption within AWS Services

Your organization might require the encryption of all data that meets a specific classification. Depending on the specific service, you can enforce data encryption policies through preventative or detective controls. For some services like Amazon S3, a policy can prevent storing unencrypted data. For other services, the most efficient mechanism is to monitor the creation of storage resources and check whether encryption is enabled appropriately. In the event that unencrypted storage is created, you have a number of possible responses ranging from deleting the storage resource to notifying an administrator.





### Conclusion 


AWS KMS provides your organization with a fully managed service to centrally control your encryption keys. Its native integration with other AWS services makes it easier for AWS KMS to encrypt the data that you store and process.

By taking the time to properly architect and implement AWS KMS, you can ensure that your encryption keys are secure and available for applications and their authorized users. Additionally, you can show your auditors detailed logs associated with your key usage.






#### Reference 

<a href="https://docs.aws.amazon.com/whitepapers/latest/kms-best-practices/kms-best-practices.pdf#welcome"> Original paper </a> 





