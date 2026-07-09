---
title: "Proposal"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# AWS Audio To Text
## An Audio-to-Text System on AWS

### 1. Project Overview
The AWS Audio To Text project is a serverless system on AWS that allows users to upload audio or video files such as MP3, MP4, WAV, and M4A to automatically convert spoken content into text.

Users access the website through Amazon CloudFront, sign in with Amazon Cognito, and then upload files to Amazon S3 via presigned URLs. After upload, the audio file is processed automatically through S3 events, SQS, Lambda, and Amazon Transcribe. The transcript result is stored in the S3 output bucket, job status is stored in DynamoDB, and the system sends email notifications through Amazon SNS.

In this workshop, the team will present the entire process of building the system, from architecture design and AWS service deployment to Lambda code development, React frontend integration, pipeline testing, and website deployment on CloudFront.

### 2. Objectives
The main objective of the project is to build an audio-to-text system that operates automatically on the AWS platform.

Specific objectives include:
- Building a serverless system using Lambda, S3, SQS, API Gateway, and DynamoDB to reduce server management.
- Supporting audio/video uploads so users can upload MP3, MP4, WAV, and M4A files.
- Converting audio to text using Amazon Transcribe.
- Managing users with Amazon Cognito for registration, sign-in, and authentication.
- Saving conversion history, including metadata, job status, and transcript paths in DynamoDB.
- Sending notifications using Amazon SNS to email users when a job is completed.
- Deploying the website online by hosting the React frontend in an S3 frontend bucket and CloudFront.
- Monitoring and debugging with CloudWatch Logs.

### 3. Problem to Be Solved
*Current situation*  
After meetings, study sessions, or online interviews, users often have to listen to entire audio or video files again to note down the main points. This is time-consuming, prone to missing information, and difficult to manage when there are many files. In addition, audio/video files are often large, so uploading them through a traditional backend can cause overload. The process of converting audio to text also takes time, so a system is needed to handle this automatically, asynchronously, and securely for each user.

*Solution*  
The AWS Audio To Text system uses a serverless architecture on AWS to automatically convert audio to text. Users sign in with Amazon Cognito, then upload files directly to Amazon S3 via presigned URLs. When a file is uploaded, an S3 event sends a message to Amazon SQS, and AWS Lambda then calls Amazon Transcribe to convert the audio to text. The results are stored in an S3 output bucket, while processing status and history are stored in DynamoDB. Amazon SNS also sends email notifications when the process is complete. The website is deployed using an S3 frontend bucket and CloudFront, making it easy for users to access, view transcripts, download results, and manage their file history.

### 4. Solution Architecture
The system is designed using a serverless model with the following main layers: frontend, authentication, API, processing, storage, database, notification, and monitoring.

![AWS Audio To Text](/images/2-Proposal/AWS-AudioToText_Architech.jpg)<small style="display: block; text-align: center;">*Architecture diagram of the project*</small>

*AWS services used*  
- *Amazon CloudFront*: Distributes the React website over HTTPS.
- *Amazon S3*: Stores the React frontend build files, stores users’ uploaded audio/video files, and stores transcript.json and transcript.txt files (three buckets).
- *Amazon Cognito*: Manages registration, sign-in, and JWT token issuance.
- *Amazon API Gateway*: Provides APIs for the frontend to call the backend.
- *AWS Lambda*: Implements serverless backend logic (five functions).
- *Amazon SQS*: Queue for asynchronous audio processing.
- *Amazon Transcribe*: Converts audio into text.
- *Amazon DynamoDB*: Stores job information, processing status, and file history.
- *Amazon SNS*: Sends email notifications when processing is complete.
- *Amazon CloudWatch*: Stores logs and helps debug Lambda functions.

*Component design*  
- *Web interface*: React is built and stored in an Amazon S3 frontend bucket, then distributed via Amazon CloudFront so users can access the system over HTTPS.
- *User management*: Amazon Cognito handles registration, sign-in, and JWT token issuance for API authentication.
- *API gateway*: Amazon API Gateway receives requests from the frontend, validates Cognito tokens, and forwards requests to the appropriate Lambda functions.
- *File upload*: lambda-upload-url creates a presigned URL so users can upload audio/video files directly to the Amazon S3 audio bucket while also creating job information in DynamoDB.
- *Input storage*: The Amazon S3 audio bucket stores MP3, MP4, WAV, and M4A files uploaded by users.
- *Processing queue*: Amazon SQS receives events from the S3 audio bucket after a file is uploaded, enabling asynchronous processing and avoiding overload.
- *Conversion processing*: lambda-processor reads messages from SQS and calls Amazon Transcribe to convert audio content into text.
- *Result storage*: The S3 output bucket stores transcript results, including transcript.json generated by Transcribe and transcript.txt processed for easier reading.
- *Result handling*: lambda-result-handler reads transcript.json, creates transcript.txt, updates the job status in DynamoDB, and sends notifications through SNS.
- *Status management*: Amazon DynamoDB stores job details, processing status, user_id, file name, creation time, and transcript paths.
- *Result retrieval*: lambda-get-job reads the job status from DynamoDB, retrieves transcript.txt from the S3 output bucket, and returns it to the web interface.
- *Conversion history*: lambda-list-jobs retrieves the list of files uploaded and converted by the currently logged-in user.
- *Notifications*: Amazon SNS sends email notifications when the audio-to-text conversion is complete.
- *Monitoring*: Amazon CloudWatch records Lambda logs to support system inspection, debugging, and monitoring.

### 5. Implementation Timeline
- Day 1: Analyze requirements, define the main functions, and draw the architecture diagram and workflow.
- Day 2: Create S3 buckets, Cognito User Pool, App Client, and configure user sign-in.
- Day 3: Create DynamoDB, SQS, API Gateway, and IAM roles and policies for Lambda.
- Day 4: Implement lambda-upload-url, lambda-processor, and lambda-result-handler.
- Day 5: Implement lambda-get-job and lambda-list-jobs, and integrate API Gateway with Cognito Authorizer.
- Day 6: Build the React frontend, integrate Cognito, upload files, display transcripts, and add download buttons.
- Day 7: Perform end-to-end testing, configure SNS email notifications, deploy the frontend to S3 and CloudFront, and verify CloudWatch logs.

### 6. Budget Estimate
You can view the estimated cost on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=b998e2ea0ba1c7f9369be3ab26ea8cde51f8f7c6).

*Infrastructure costs*  
- *Amazon CloudFront*: $0.12/month (1 GB data transfer out to the internet, 1,000 HTTPS requests/month).
- *Amazon S3*: $0.03/month (1 GB S3 Standard, 500 PUT/COPY/POST/LIST requests, 1,000 GET/SELECT/other requests).
- *Amazon Cognito*: $0.00/month (10 active users per month).
- *Amazon API Gateway*: $0.00/month (HTTP API with 0.001 million requests/month, equivalent to 1,000 requests and an average size of 34 KB/request).
- *AWS Lambda*: $0.00/month (1,000 requests/month, x86 architecture, buffered invoke mode, 512 MB ephemeral storage).
- *Amazon SQS*: $0.00/month (0.001 million Standard Queue requests/month, equivalent to 1,000 requests).
- *Amazon Transcribe*: $0.60/month (100 minutes of batch processing with standard Amazon Transcribe).
- *Amazon DynamoDB*: $0.03/month (Standard table class, 0.1 GB storage, average item size 1 KB).
- *Amazon SNS*: $0.00/month (1,000 requests/month, 1,000 EMAIL/EMAIL-JSON notifications).
- *Amazon CloudWatch*: $0.07/month (0.1 GB Standard Logs ingested).

*Total*: $0.85/month, $10.20/12 months

### 7. Risk Assessment
*Risk matrix*  
- IAM errors: High impact, medium probability. Lambda may not be able to access S3, DynamoDB, Transcribe, or SNS.
- CORS errors: Medium impact, high probability. The frontend may fail to call the API or upload files to S3.
- S3 Event/SQS not working: High impact, medium probability. Files upload successfully but are not passed into the processing pipeline.
- Transcribe errors: High impact, medium probability. Files with incorrect formats, poor audio quality, or incorrect language codes may fail to generate transcripts.
- Exceeding AWS budget: Medium impact, low probability. Costs may increase if many large files are tested or many Transcribe jobs are run.
- Cognito errors: Medium impact, medium probability. Users may fail to log in or APIs may return authentication errors.

*Mitigation strategies*  
- IAM: Grant the correct permissions to each Lambda function and check errors through CloudWatch Logs.
- CORS: Configure CORS for API Gateway, the S3 audio bucket, and the S3 output bucket.
- S3 Event/SQS: Verify event notifications, SQS policies, and Lambda triggers.
- Transcribe: Allow only MP3, MP4, WAV, and M4A formats and test with short, clear audio files.
- Cost: Use small test files, set AWS Budget Alerts, and remove unnecessary resources after the demo.
- Cognito: Check the User Pool ID, App Client ID, JWT Authorizer, and Authorization header.

*Contingency plan*  
- If the automatic pipeline fails, the team will check each step manually from S3, SQS, Lambda, Transcribe, and DynamoDB.
- If triggers are not working, Lambda can be tested using sample events.
- If the frontend fails after deployment, the team will revert to local testing to verify the API first, then rebuild and redeploy to S3/CloudFront.
- If costs increase, the team will temporarily stop triggers, delete test files, and check billing.