---
title : "Create SQS Queue"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---

#### Create SQS Queue

In this step, you will create an SQS Standard Queue to receive file upload events from the S3 Audio Bucket.

SQS helps decouple the file upload process from the Transcribe processing workflow. When a file is uploaded to S3, the event is sent to the queue. Then, the Lambda Processor reads messages from the queue and starts converting the audio into text.

![SQS overview](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-1.png)

### 1. Access Amazon SQS

1. Sign in to the **AWS Management Console**.
2. In the search bar, enter **SQS**.
3. Select **Simple Queue Service**.

![Open SQS](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-2.png)

### 2. Create a Queue

1. In the **SQS** console, choose **Create queue**.
2. In **Type**, select **Standard**.
3. In **Name**, enter:

```text
audio-processing-queue
```

![Create queue](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-3.png)

### 3. Configure the Queue

Keep the following settings at their default values:

- **Visibility timeout**
- **Message retention period**
- **Delivery delay**
- **Maximum message size**
- **Receive message wait time**

![Queue configuration](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-4.png)

### 4. Configure the Access Policy for S3

In **Access policy**, select **Advanced** and paste the policy below.

Replace the following values before saving:

- `<account-id>`: your AWS Account ID.
- `<region>`: the region you are using, for example `ap-southeast-1`.
- `<input-bucket-name>`: the name of the Audio Bucket created in section 5.2.1.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3SendMessageToAudioProcessingQueue",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "sqs:SendMessage",
      "Resource": "arn:aws:sqs:<region>:<account-id>:audio-processing-queue",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "<account-id>"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:::<input-bucket-name>"
        }
      }
    }
  ]
}
```

![SQS access policy](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-5.png)

### 5. Finish Creating the Queue

1. Keep **Redrive allow policy** and **Dead-letter queue** at their default settings.
2. Choose **Create queue**.
3. After the queue is created, open the `audio-processing-queue` queue.
4. Record the queue **ARN** to use when configuring the S3 Event Notification and Lambda trigger.

![SQS result](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-6.png)

### 6. Verify the Connection with S3

After creating the queue, return to the Event Notification configuration for the Audio Bucket:

1. Open the S3 Audio Bucket.
2. Go to the **Properties** tab.
3. Create an event notification for **All object create events**.
4. Select `audio-processing-queue` as the destination.
5. Save the configuration.

When a user uploads a file to the Audio Bucket, S3 will send a message to this queue for Lambda to process.
