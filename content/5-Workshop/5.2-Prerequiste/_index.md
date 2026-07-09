---
title : "Prepare the Environment"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

### Overview

In this section, you will prepare all backend resources for the **AWS Audio To Text** system.

The resources created in section 5.2 include:

- Amazon S3 to store frontend files, input audio/video files, and output transcripts.
- Amazon DynamoDB to store job information, processing status, and conversion history.
- Amazon SQS to receive upload events from S3 and process them asynchronously.
- Amazon Cognito to manage registration, sign-in, and JWT token issuance for the frontend.
- AWS Lambda to create presigned URLs, process Transcribe jobs, update results, and return data to the frontend.
- Amazon SNS to send email notifications when the conversion process is complete.
- Amazon API Gateway so the frontend can call Lambda functions through HTTP APIs.

After completing this section, you will have a complete serverless backend that allows the frontend to sign in, upload files, track job status, and view transcript results.

### Content

- [Create S3 Bucket](CreateBucket/)
- [Create DynamoDB](CreateDynamoDB/)
- [Create SQS Queue](CreateSQS/)
- [Create Cognito](CreateCognito/)
- [Create Lambda Function and Configure Lambda](CreateLambda/)
- [Create SNS](CreateSNS/)
- [Create API Gateway](CreateAPI/)
