

# iOS CI/CD Build and Test Pipeline

Automate building and testing of iOS apps using AWS CodePipeline, Amazon EC2 Mac instance, and AWS Device Farm. 


- A developer initiates a build or test activity in AWS CodePipeline by pushing a code change to AWS CodeCommit.

- AWS CodePipeline detects the change in AWS CodeCommit and invokes AWS Step Functions to start the build process.

- An AWS Lambda function loads required build scripts from an Amazon S3 bucket and triggers an AWS Systems Manager Run command.

- The AWS Systems Manager Run command runs the build scripts on an Amazon EC2 Mac instance and sends run logs to Amazon CloudWatch

- Build scripts initiate an iOS build on the Amazon EC2 Mac instance and put the build artifacts in an Amazon S3 bucket.

- AWS CodePipeline detects the built artifacts in Amazon S3 and initiates AWS Device Farm for testing the app on multiple real devices.

- AWS Device Farm generates test results, logs, and recording for all tests for review through the AWS Device Farm console or through AWS S3 pre-signed URLs. 



![1](https://user-images.githubusercontent.com/23625821/143169736-355e120a-c0a1-4e2e-9a0c-5290e699b8fd.png)








<a href="https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/ios-cicd-build-test-pipeline-ra.pdf?did=wp_card&trk=wp_card"> Diagram </a> 






