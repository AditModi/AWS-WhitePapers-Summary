# Moderating Image Content in Slack with Amazon Rekognition and Amazon AppFlow

## #AWS_WhitePaper_Summary

- This document shows you how to use Amazon Rekognition and Amazon AppFlow to build a fully serverless content moderation pipeline for messages posted in a Slack channel. 

- The content moderation strategy identiﬁes images that violate sample chosen guidelines:
  - Images that contain themes of tobacco or alcohol.
  - Images that contain the following disallowed words:
     - medical
     - private

- Amazon Rekognition content moderation is a deep learning-based service that can detect inappropriate, or oﬀensive images & videos, making it easier to ﬁnd and remove such content at scale.
- It provides a detailed taxonomy of moderation categories. 
-  Such as Explicit Nudity, Suggestive, Violence, and Visually Disturbing.
-  You can now detect six new categories: Drugs, Tobacco, Alcohol, Gambling, Rude Gestures, and Hate Symbols.

- Amazon AppFlow is a fully managed integration service that enables you to securely transfer data between Software-as-a-Service (SaaS) applications like Salesforce, Marketo, Slack, and ServiceNow, and AWS services like S3, Redshift, in just a few clicks.
-  This solution leverages Amazon AppFlow to capture the content posted in Slack channels for analysis using Amazon Rekognition.
- It's supposed that reader of this document has an account/workspace on AWS and Slack, also S3 bucket accessibility.
- Also, client credetials for this services will be used.  
- This solution doesn't require any prior machine learning (ML) expertise, or development of your own custom ML models.

## Architecture overview
- This solution uses serverless technologies and managed services to be scalable and cost-eﬀective.
-  By using an event-driven architecture that incorporates AWS Lambda and SQS, you can decouple image detection and image processing without provisioning or managing any servers.

![1](https://user-images.githubusercontent.com/23625821/126063940-44fdd366-c750-4643-9dd7-ccf5bfd0ca69.png)

### Create Amazon AppFlow Integration with your Slack workspace
- First you have to create a Slack app, <a href="https://api.slack.com/authentication/basics"> for more details </a> . 
- Now follow these steps to conﬁgure the Amazon AppFlow integration: 
   1. Navigate to the Amazon AppFlow console and choose Create ﬂow.
   2. In Step 1 of the creation process, enter a Flow name, and optionally, a description. 
   3. For the purposes of this demo, leave the Data  encryption setting as it is.
   4. Optionally, enter any tags you’d like for the ﬂow.
   5. Choose Next.

![2](https://user-images.githubusercontent.com/23625821/126064238-b2f9535e-6989-4941-b5e0-f2dfbe7d13eb.png)
   
   6. In Step 2 (Conﬁgure ﬂow), choose the Source name dropdown list and choose Slack from the list of options:

  ![3](https://user-images.githubusercontent.com/23625821/126064279-d5b32388-de1a-4216-b1c2-27d485cfaf3f.png) 
   7. A Choose Slack connection dropdown list appears. From this list, choose Create new connection:
   ![4](https://user-images.githubusercontent.com/23625821/126064307-fdc5e466-2301-4358-89c1-9a69d651ec10.png)
   
   8.  Enter your Slack workspace address (for example, testingslackdevgroup.slack.com), and Client ID and Client Secret generated when created the Slack App.
   9.  Give your connection a name on the Connect to Slack popup window.
   10. Choose Continue.
   
   ![5](https://user-images.githubusercontent.com/23625821/126064352-d3841ee3-5111-485a-bf60-40e9a4785111.png)
   
   11. A window pops up with a conﬁrmation prompt to allow permissions. Choose Allow.
   12. Your new connection is conﬁgured & displayed in the Choose Slack connection dropdown list, and a new Choose Slack object dropdown list appears directly below it. Choose Conversations.
   13. A new dropdown appears directly below Choose Slack channel. From this list, choose the Slack channel that you would like to perform content moderation on.
   
   ![6](https://user-images.githubusercontent.com/23625821/126064515-dfbdbdcf-4ae2-4925-93ad-f48d08e7bb4d.png)
   
   14. With the Slack workspace connected, and the channel for moderation selected, you can move on to conﬁguring the Destination details. First, choose Amazon S3 from the Destination name dropdown list. Select S3. 
    
  ![7](https://user-images.githubusercontent.com/23625821/126064535-32d96be2-af4f-46b7-85f4-79dd2e178748.png)
  15. A new section titled Flow trigger appears with two options: Run on demand or Run ﬂow on schedule. Choose the second option, and conﬁgure the schedule to run every one (1) minute.
  16. When you choose this option, the Incremental Transfer option is auto-selected. Enter a value for Starting at and Start date.
  17. In Step 3 (Map data ﬁelds), you have the option to perform transformations on the data ﬁelds. Choose Manually map ﬁelds.
  18. From the Source ﬁeld name dropdown, select Map all ﬁelds directly. This creates a mapping of all the ﬁelds without any transformations.
  19. Choose Next.
  20. In Step 4 (Add ﬁlters), you have the option to perform ﬁltering on the data. Do not add any ﬁlters here, simply choose Next to continue.
  21. On the Review and Create screen, a summary of all your selections from previous steps is shown. Review these for accuracy, then scroll to the bottom of page and choose Create ﬂow.
  22.After the ﬂow has been created, on the following screen, choose the Activate ﬂow button.
 

### Create a Lambda function to process ﬁles in the S3 bucket that contain new Slack messages
- Because the Lambda function needs to store the image URLs it ﬁnds into a new SQS queue, ﬁrst create that queue by following the steps outlined in <a href="https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-getting-started.html"> Getting started with Amazon SQS </a> . Name this queue new-image-findings.
- Navigate to the Lambda console. Choose Create Function and choose the option to Use a blueprint, then provide a ﬁlter called hello. This displays the hello-world-python blueprint in the results at the bottom.
- Choose configure button. 
![1](https://user-images.githubusercontent.com/23625821/126112592-817babf5-ac06-454d-b011-a9461f09c8da.png)

- On the next screen, provide a name for your new function called process-new-messages, and create a new IAM role called process-new-messages-lambda-role using the available “Amazon S3 object read-only permissions” template. This role will need to be customized in a later step.
![1](https://user-images.githubusercontent.com/23625821/126112771-428e2b13-8607-41d0-95bd-d260feb831e8.png)

- After the function has been created, choose the Permissions tab. 
- Choose the role name to open a second window where you can view the two policies applied to this role.
![1](https://user-images.githubusercontent.com/23625821/126113013-01ef6b4c-2c16-4a9b-a380-00545db4f01f.png)

- Expand each policy to view the permissions details. The policy named AWSLambdaBasicExecutionRole-* grants the necessary permissions for the function to log
information in CloudWatch. The policy named AWSLambdaS3ExecutionRole-* provides S3 permissions and needs to be modiﬁed. To modify the policy, choose Edit Policy and switch to the JSON view to customize this policy. The ﬁnal permissions statement should appear as follows:

```json 
"Statement": [{
  "Action": [
   "s3:GetObject*",
   "s3:GetBucket*",
   "s3:List*"
 ],
  "Resource": [
     "arn:aws:s3:::slack-moderation-output",
     "arn:aws:s3:::slack-moderation-output/*"
 ],
  "Effect": "Allow"
}]
```

The preceding statement follows the principle of least privilege, and limits the permissions of this Lambda function to only the bucket you created for this exercise. Save the change you’ve made to this policy.

For this function to write messages to the new-image-findings SQS queue, an additional minimally scoped IAM policy needs to be added to this role.

To add the IAM policy:

1. Choose Add inline policy and switch to the JSON view to create the following permissions. Note that the following Resource element needs to be updated with the correct Amazon Resource Name (ARN) for the new-image-findings SQS queue which contains your actual account number.

```json
{
"Version": "2012-10-17",
 "Statement": [{
  "Action": [
   "sqs:SendMessage",
   "sqs:GetQueueAttributes",
   "sqs:GetQueueUrl"
 ], 
  "Resource": "arn:aws:sqs:us-east-1:111111111111:new-image-findings",
  "Effect": "Allow"
 }]

}

```
2. Choose Review policy, then enter a name for this policy and choose Create policy.
3. With the permissions properly conﬁgured, switch back to the Conﬁguration tab in the Lambda function window, and paste the following code into the Function code section:

```py
import boto3
from urllib.parse import unquote_plus
import json
s3_client = boto3.client('s3')
s3 = boto3.resource('s3')
sqs = boto3.client('sqs')


def sendToSqS(attributes, queueurl):
  sqs = boto3.client('sqs')
  sqs.send_message(
    QueueUrl=queueurl,
    MessageBody='Image to Check',
    MessageAttributes={ "url": { "StringValue": attributes["image_url"], "DataType": 'String'
}, "slack_msg_id": { "StringValue": attributes["client_msg_id"], "DataType": 'String' } } )


def lambda_handler(event, context):

  image_processing_queueurl = "https://queue.amazonaws.com/111111111111/new-image-findings”

  for record in event['Records']:
    bucket = record['s3']['bucket']['name']
    key = unquote_plus(record['s3']['object']['key'])
    file_lines = s3.Object(bucket, key).get()
    ['Body'].read().decode('utf-8').splitlines()
    attachment_list = []
    for line in file_lines:
        if line: # Check for blank lines
        jsonline = json.loads(line)
            if "attachments" in jsonline.keys(): # Check for lines with attachements
            for attachment in jsonline["attachments"]:
            if "image_url" in attachment.keys():
            if "client_msg_id" in jsonline.keys():
            thisdict = {
            "image_url": attachment["image_url"],
            "client_msg_id": jsonline["client_msg_id"]
            }
            attachment_list.append(thisdict.copy())
        else:
        thisdict = {
        "image_url": attachment["image_url"],
        "client_msg_id": "None Found"
        }
        attachment_list.append(thisdict.copy())
        for item in attachment_list:
        sendToSqS(item, image_processing_queueurl)



```
4. After you have pasted the code, update the image_processing_queueurl variable in the function handler with the correct ARN for the new-image-findings SQS queue which contains your actual account number.
5. Choose Deploy to deploy the updated code.

### Conﬁgure the Lambda function to be invoked when new objects are added to your S3 bucket

With your Lambda function (process-new-messages) created, the next step is to conﬁgure bucket notiﬁcations on your S3 bucket, and subscribe this Lambda function to the notiﬁcations.

To create the S3 / Lambda event integration:

Conﬁgure event notiﬁcations on your S3 bucket by following the steps outlined in this <a href="https://docs.aws.amazon.com/AmazonS3/latest/user-guide/enable-event-notifications.html"> User Guide </a> .

- In Step 5 of the conﬁguration, choose the All object create events option.
- In Step 6, choose your Lambda function named process-new-messages.

![1](https://user-images.githubusercontent.com/23625821/126266685-b67660a1-96df-465c-893e-4025db311e80.png)

### Create a Lambda function to process messages where image references were found (via SQS queue)

Your ﬁrst Lambda function (process-new-messages) is now being invoked, and any image references found in Slack messages have been stored in the new-image-findings SQS queue.

The next step is to create and invoke another Lambda function (process-new-images) that will use Amazon Rekognition to determine if there are any policy violations in the content.

To conﬁgure the SQS / Lambda / Amazon Rekognition Integration:
1. Because the Lambda function you are about to create needs to store any content violations found into a new SQS queue, ﬁrst create that queue by following the steps outlined in Getting started with Amazon SQS.
2. Name this queue new-violation-findings.
3. Navigate to the Lambda console and choose Create Function.
4. Choose the Use a blueprint option and provide a ﬁlter called hello. This will display the hello-world-python blueprint in the results at the bottom.
5. Choose the Conﬁgure button. Name the new Lambda function process-new-images.
6. Create a new execution role with basic Lambda permissions.
7. After the function has been created, choose the Permissions tab.
8. Choose IAM Role to open a second window where you can view the policy attached to this role.
9. Choose Attach Policies.
10. Search for AmazonRekognitionReadOnlyAccess and choose Attach Policy to complete the action. This allows your Lambda function permissions to call Amazon Rekognition.
11. he function also needs permissions to read from the new-image-findings queue and write new messages to the new-violation-findings queue. Choose Add inline policy and switch to the JSON view to create the following permissions.


Note that the following Resource elements need to be updated with the correct ARNs for the new-image-findings and new-violation-findings SQS queues respectively, which contain your actual account number:

```json
{
"Version": "2012-10-17",
"Statement": [
{
"Action": [
"sqs:ReceiveMessage",
"sqs:ChangeMessageVisibility",
"sqs:GetQueueUrl",
"sqs:DeleteMessage",
"sqs:GetQueueAttributes"
],
"Resource": "arn:aws:sqs:us-east-1:111111111111:new-image-findings",
"Effect": "Allow"
},
{
"Action": [
"sqs:SendMessage",
"sqs:GetQueueAttributes",
"sqs:GetQueueUrl"
],
"Resource": "arn:aws:sqs:us-east-1:111111111111:new-violation-findings",
"Effect": "Allow"


 }]

}

```

12. Choose Review policy.
13. Enter a name for this policy and choose Create policy.
14. With the permissions conﬁgured, switch back to the Conﬁguration tab in the Lambda function window, and paste the following code into the Function code section:

```py
import urllib.request
import boto3

sqs = boto3.client('sqs')
rekognition = boto3.client('rekognition')

def analyze_themes(file, min_confidence=80):
  with open(file, 'rb') as document:
    imageBytes = bytearray(document.read())
    response = rekognition.detect_moderation_labels(Image={'Bytes': imageBytes}, MinConfidence=min_confidence)

  found_high_confidence_labels = []
  
  for label in response['ModerationLabels']:
    found_high_confidence_labels.append(str(label['Name']))
  
  return found_high_confidence_labels


def analyze_text(file):
  with open(file, 'rb') as document:
    imageBytes = bytearray(document.read())
    
    response = rekognition.detect_text(Image={'Bytes': imageBytes})
    
    textDetections = response['TextDetections']
    found_text = ""
    
    for text in textDetections:
      found_text += text['DetectedText']
    
    return found_text



def sendToSqS(words, attributes, queueurl):
   sqs.sendMessage(
      QueueUrl=queueurl,
      MessageBody='Image with "' + words + '" found',
      MessageAttributes={
        "url": {
          "StringValue": attributes["image_url"],
          "DataType": 'String'
        },
        "slack_msg_id": {
          "StringValue": attributes["slack_msg_id"],
          "DataType": 'String'
        }
      }
   )



def lambda_handler(event, context):
   violations = "https://queue.amazonaws.com/111111111111/new-violation-findings"
   disallowed_words = ["medical", "private"]
   disallowed_themes = ["Tobacco", "Alcohol"] # Case Sensitive 
   file_name = "/tmp/image.jpg"
   
   for record in event['Records']:
      print(record)
      receiptHandle = record["receiptHandle"]
      image_url = record["messageAttributes"]["url"]["stringValue"]
      slack_msg_id = record["messageAttributes"]["slack_msg_id"]["stringValue"]
      eventSourceARN = record["eventSourceARN"]
      arn_elements = eventSourceARN.split(':')
      
      img_queue_url = sqs.get_queue_url(
          QueueName=arn_elements[5],
          QueueOwnerAWSAccountId=arn_elements[4]
      )
      
      sqs.delete_message(
          QueueUrl=img_queue_url["QueueUrl"],
          ReceiptHandle=receiptHandle
      )
      
      urllib.request.urlretrieve(image_url, file_name)
      detected_text = analyze_text(file_name)
      
      print("Detected Text: " + detected_text)
      found_words = []
      for disallowed_word in disallowed_words:
        if disallowed_word.lower() in detected_text.lower():
            found_words.append(disallowed_word)
            print("WORD VIOLATION: " + disallowed_word.lower() + " found in " + detected_text.lower())
        
        violating_words = ", ".join(found_words)
        if not violating_words == "":
          attributes_json = {}
          attributes_json["slack_msg_id"] = slack_msg_id
          attributes_json["image_url"] = image_url
          sendToSqS(violating_words, attributes_json, violations)
        
        detected_themes = analyze_themes(file_name)
        print("Detected Themes: " + ", ".join(detected_themes))
        
        found_themes = []
        for disallowed_theme in disallowed_themes:
          if disallowed_theme in detected_themes:
              found_themes.append(disallowed_theme)
              print("THEME VIOLATION: " + disallowed_theme + " found in image")
          
        violating_themes = ", ".join(found_themes)
          if not violating_themes == "":
              attributes_json = {}
              attributes_json["slack_msg_id"] = slack_msg_id
              attributes_json["image_url"] = image_url
              
              sendToSqS(violating_themes, attributes_json, violations)
              
```

- After you have pasted the code, update the violations variable in the function handler with the correct ARN for the new-violation-findings SQS queue which  contains your actual account number.

- Choose Deploy.

To ensure that your SQS queues cannot be accessed by resources outside the account, SQS permissions policies can be applied to each of the queues.

To apply permissions policies:

1. Navigate to the SQS console and choose the new-violation-ﬁndings queue.
2. Choose the Access policy tab.
3. Choose the Edit button and paste in the following policy.


Note that the following Resource elements need to be updated with the correct ARN for the new-violation-findings SQS queues respectively, which contain your actual account number.


```json 
{
}
"Version": "2012-10-17",
"Statement": [
{
"Sid": "QueueOwnerOnlyAccess",
"Effect": "Allow",
"Principal": {
"AWS": "arn:aws:iam:: 111111111111:root"
},
"Action": [
"sqs:DeleteMessage",
"sqs:ReceiveMessage",
"sqs:SendMessage",
"sqs:GetQueueAttributes",
"sqs:RemovePermission",
"sqs:AddPermission",
"sqs:SetQueueAttributes"
],
"Resource": "arn:aws:sqs:us-east-1: 111111111111:new-violation-findings"
},
{
"Sid": "HttpsOnly",
"Effect": "Deny",
"Principal": "*",
"Action": "SQS:*",
"Resource": "arn:aws:sqs:us-east-1: 111111111111:new-violation-findings",
"Condition": {
"Bool": {
"aws:SecureTransport": "false"
  }}}]
}

```

Repeat the preceding steps for the new-image-findings queue. Remember to use the new-image-findings ARN in the policy.

You can now conﬁgure our SQS queue to trigger your Lambda function.

To conﬁgure your SQS queue:

1. In the SQS Console, choose the new-image-findings queue from the Lambda triggers tab.
2. Choose Conﬁgure Trigger for Lambda Function.
3. From the dropdown list, choose the function you just created.

![1](https://user-images.githubusercontent.com/23625821/126373428-ee705b0c-4735-4839-936d-ecd21e6930cd.png)

Trigger the Lambda function you created. 


### Test the solution

You can now post some messages to your moderated Slack channel for testing. You can easily change the content violation policies in the Python code by modifying the disallowed_words and disallowed_themes variables.


#### To test the solution:

1. Post sample images that will be used to trigger violations for the current conﬁgured policies:
  - Post this image which contains the disallowed word "private": https://i.imgur.com/662ptww.png
  - Post this image which contains a "Tobacco" theme: https://i.imgur.com/XgAtyWU.png

2. After creating those posts, wait 2-3 minutes and then navigate to the SQS Console. View the queues and choose the new-violation-findings queue.
3. Choose the Send and receive messages button.
4. At the bottom of the screen, choose the Poll for messages button.

5. After a few seconds you should see two messages pop up. You can choose each message to interrogate the contents.
6. Choose the Message ID. The body of the message contains information about what violation was triggered. The Attributes show the image URL and “slack_msg_id” for the oﬀending item.




### References

<a href="https://docs.aws.amazon.com/whitepapers/latest/moderating-image-content-in-slack/moderating-image-content-in-slack.pdf#moderating-image-content-in-slack"> Original White Paper </a>
