---
title: "Create Lambda Functions and Configure Lambda"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.2.5 </b> "
---

#### Create Lambda Functions and Configure Lambda

In this step, you will create the Lambda functions for the serverless backend of the AWS Audio To Text system.

The system uses 5 main Lambda functions:

- `lambda-upload-url`: creates a presigned URL so the frontend can upload files to S3.
- `lambda-processor`: reads messages from SQS and calls Amazon Transcribe.
- `lambda-result-handler`: processes the transcript after Transcribe completes, updates DynamoDB, and sends SNS notifications.
- `lambda-get-job`: retrieves the details of a job and its transcript content.
- `lambda-list-jobs`: retrieves the job list for the currently signed-in user.

### 1. Access Lambda

1. Sign in to the **AWS Management Console**.
2. In the search bar, enter **Lambda**.
3. Select **Lambda**.

![Open Lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-1.png)

### 2. Create the upload-url Lambda

> Because I have already done this before, I will guide you through the steps.

1. Open the **Lambda** service.
2. Choose **Create function**.
3. Select **Author from scratch**.
4. Enter the function name:

```text
lambda-upload-url
```

1. For Runtime, select **Python 3.12**.
2. Choose **Create function**.

![Create upload lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-2.png)

![lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-3.png)

### 4. Configure Environment Variables for lambda-upload-url

1. In the **Configuration** tab, select **Environment variables**.
2. Choose **Edit**.
3. Add the following configurations in **Edit environment variables**:

```text
AUDIO_BUCKET = <your-name-audio-bucket>
JOBS_TABLE = jobs
URL_EXPIRES_SECONDS = 900
```

4. Choose **Save**.
   
This function receives file information from the frontend, creates a `job_id`, saves an item to DynamoDB, and returns a presigned URL.

![lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-4.png)

![lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-5.png)

### 5. Create the processor Lambda

Create a new function with the following name:

```text
lambda-processor
```

Configure the role the same way as the previous step and add the following environment variables:

```text
JOBS_TABLE = jobs
LANGUAGE_CODE = vi-VN
OUTPUT_BUCKET = <your-name-output-bucket>
```

This function reads messages from SQS, retrieves file information from S3, calls Amazon Transcribe, and updates the job status to `PROCESSING`.

![Processor lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-7.png)

### 6. Attach an SQS Trigger to lambda-processor

1. Open the `lambda-processor` function.
2. In the **Configuration** tab, select **Triggers**.
4. Choose **Add trigger**.
3. In **Source**, select **SQS**.
4. Select the `audio-processing-queue` queue.
5. Keep **Batch size** as default or set it to `1` for easier debugging.
6. Choose **Add**.

> After adding the trigger, the result will appear as shown in the image below.

![SQS trigger](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-8.png)

### 7. Create the result-handler Lambda

Create a new function with the following name:

```text
lambda-result-handler
```
   
Add the following environment variables:

```text
CONNECTIONS_TABLE = ams-connections
JOBS_TABLE = jobs
OUTPUT_BUCKET = <your-name-output-bucket>
SNS_TOPIC_ARN = <your-name-sns-topic>
```

This function reads the `transcript.json` file, extracts the text content, creates a `transcript.txt` file, updates DynamoDB to `COMPLETED`, and sends an email notification through SNS.

![Result handler lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-9.png)

### 8. Attach an S3 Trigger to lambda-result-handler

1. Open the `lambda-result-handler` function.
2. Choose **Add trigger**.
3. Select **S3** as the source.
4. Select the Output Bucket.
5. For Event type, select **All object create events**.
6. Prefix: **transcripts/**, Suffix: `.json`.
7. Choose **Add**.
   
> After adding the trigger, the result will appear as shown in the image below.

![S3 trigger](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-10.png)

### 9. Create the get-job Lambda

Create a new function with the following name:

```text
lambda-get-job
```

Add the following environment variables:

```text
DOWNLOAD_URL_EXPIRES_SECONDS = 900
JOBS_TABLE = jobs
OUTPUT_BUCKET = <your-name-output-bucket>
```

This function receives a `job_id`, checks the job in DynamoDB, and returns the status along with the transcript content if it has completed.

![Get job lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-11.png)

### 10. Create the list-jobs Lambda

Create a new function with the following name:

```text
lambda-list-jobs
```

Add the following environment variables:

```text
JOBS_TABLE = jobs
```

This function retrieves the list of jobs that belong to the currently signed-in user so the conversion history can be displayed on the frontend.

![List jobs lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-12.png)

### 11. Check CloudWatch Logs

After creating the functions, open the **Monitor** tab of each Lambda and choose **View CloudWatch logs**.

When testing the pipeline, CloudWatch Logs help you check:

- Whether Lambda receives the correct event.
- Whether Lambda can read from S3, SQS, and DynamoDB.
- Whether the Transcribe job is created successfully.
- Whether SNS sends the notification successfully.

![CloudWatch logs](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-13.png)
