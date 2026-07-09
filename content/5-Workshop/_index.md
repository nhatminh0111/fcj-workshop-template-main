---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# AWS Audio To Text

#### Overview

The AWS Audio To Text project is a serverless system on AWS that allows users to upload audio or video files such as MP3, MP4, WAV, and M4A, then automatically convert the speech content into text.

Users access the website through Amazon CloudFront, sign in with Amazon Cognito, and then upload files to Amazon S3 by using a presigned URL. After an audio file is uploaded, it is processed automatically through an S3 Event, SQS, Lambda, and Amazon Transcribe. The transcript result is stored in an S3 Output Bucket, the job status is stored in DynamoDB, and the system sends email notifications through Amazon SNS.

In this workshop, the team will present the complete process of building the system, from designing the architecture and deploying AWS services to writing Lambda code, integrating the React frontend, testing the pipeline, and deploying the website to CloudFront.

![overview](/images/5-Workshop/aws-audio-to-text.jpg)
  
#### AWS Services Used in the Project

- *Amazon CloudFront*: Distributes the React website over HTTPS.
- *Amazon S3*: Stores the React frontend build files, uploaded user audio/video files, and `transcript.json` and `transcript.txt` files across 3 buckets.
- *Amazon Cognito*: Manages user registration, sign-in, and JWT token issuance.
- *Amazon API Gateway*: Provides APIs for the frontend to call the backend.
- *AWS Lambda*: Handles serverless backend logic with 5 functions.
- *Amazon SQS*: Provides an asynchronous queue for processing audio files.
- *Amazon Transcribe*: Converts audio into text.
- *Amazon DynamoDB*: Stores job information, processing status, and file history.
- *Amazon SNS*: Sends email notifications when processing is complete.
- *Amazon CloudWatch*: Stores logs and supports Lambda debugging.

#### Content

1. [Introduction](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Deploy Frontend on CloudFront](5.3-DeployFrontend/)
4. [Clean Up Resources](5.4-Cleanup/)
