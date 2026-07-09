---
title : "Clean Up Resources"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Clean Up Resources

After completing the workshop, you should delete the resources you created to avoid unnecessary costs.

Resources to clean up include:

- CloudFront Distribution
- S3 buckets and the objects inside them
- API Gateway
- Lambda functions
- SQS Queue
- SNS Topic and Subscription
- DynamoDB Table
- Cognito User Pool
- CloudWatch Log Groups
- IAM Role and IAM Policy created for Lambda
- Amazon Transcribe jobs that remain in the account


### 1. Delete the CloudFront Distribution

1. Access **CloudFront**.
2. Select the distribution used for the frontend.
3. Choose **Disable**.
4. Wait until the distribution status changes to **Disabled**.
5. Choose **Delete**.

> CloudFront may take a few minutes to disable before it allows deletion.

### 2. Delete Objects in S3 Buckets

Before deleting a bucket, you need to delete all objects inside it.

Do this for the 3 buckets:

- `frontend-<account-id>-<region>`
- `audio-bucket-<account-id>-<region>`
- `audio-output-<account-id>-<region>`

Steps:

1. Access **Amazon S3**.
2. Open each bucket.
3. Select all objects.
4. Choose **Delete**.
5. Enter `permanently delete` if the console asks for confirmation.


### 3. Delete S3 Buckets

After the buckets are empty:

1. Return to the bucket list.
2. Select each bucket.
3. Choose **Delete**.
4. Enter the bucket name to confirm.
5. Choose **Delete bucket**.


### 4. Delete API Gateway

1. Access **API Gateway**.
2. Select the `audio-to-text-api` API.
3. Choose **Actions** or the delete menu.
4. Choose **Delete**.
5. Confirm the API deletion.


### 5. Delete Lambda Functions

Access **Lambda** and delete the following functions:

- `lambda-upload-url`
- `lambda-processor`
- `lambda-result-handler`
- `lambda-get-job`
- `lambda-list-jobs`

Steps:

1. Select the function you want to delete.
2. Choose **Actions**.
3. Choose **Delete**.
4. Confirm the deletion.


### 6. Delete the SQS Queue

1. Access **Amazon SQS**.
2. Select the `audio-processing-queue` queue.
3. Choose **Delete**.
4. Confirm the queue deletion.


### 7. Delete the SNS Topic

1. Access **Amazon SNS**.
2. Select **Topics**.
3. Select the `job-completed-topic` topic.
4. Choose **Delete**.
5. Confirm the topic deletion.


### 8. Delete the DynamoDB Table

1. Access **DynamoDB**.
2. Select **Tables**.
3. Select the `jobs` table.
4. Choose **Delete**.
5. Confirm the table deletion.


### 9. Delete the Cognito User Pool

1. Access **Amazon Cognito**.
2. Select **User pools**.
3. Open the workshop User Pool.
4. Choose **Delete user pool**.
5. Enter the User Pool name to confirm if the console asks.
6. Choose **Delete**.


### 10. Delete CloudWatch Log Groups

Each Lambda creates a log group in CloudWatch Logs. Delete these log groups to fully clean up the environment.

1. Access **CloudWatch**.
2. Select **Log groups**.
3. Find the following log groups:

```text
/aws/lambda/lambda-upload-url
/aws/lambda/lambda-processor
/aws/lambda/lambda-result-handler
/aws/lambda/lambda-get-job
/aws/lambda/lambda-list-jobs
```

4. Select a log group.
5. Choose **Delete**.


### 11. Delete IAM Role and Policy

1. Access **IAM**.
2. Select **Roles**.
3. Find the role:

```text
audio-to-text-lambda-role
```

4. Detach the policies attached to the role if needed.
5. Choose **Delete role**.

If you created an inline policy or a separate customer managed policy for the workshop, delete that policy after the role has been deleted.


### 12. Check Amazon Transcribe

1. Access **Amazon Transcribe**.
2. Select **Transcription jobs**.
3. Delete test jobs if you do not need to keep them.
