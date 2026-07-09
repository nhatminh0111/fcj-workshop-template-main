---
title: "Create SNS"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.2.6 </b> "
---

#### Create SNS

In this step, you will create an Amazon SNS Topic to send email notifications when the audio-to-text conversion process is complete.

The `lambda-result-handler` Lambda will publish a message to the SNS Topic after the transcript is created successfully.


### 1. Access Amazon SNS

1. Sign in to the **AWS Management Console**.
2. In the search bar, enter **SNS**.
3. Select **Simple Notification Service**.

![SNS](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-1.png)

### 2. Create a Topic

> Because I have already done this before, I will guide you through the steps.

1. In the left menu, select **Topics**.
2. Choose **Create topic**.
3. In **Type**, select **Standard**.
4. In **Name**, enter:

```text
job-completed-topic
```

5. Keep the remaining settings as default.
6. Choose **Create topic**.

![Create SNS topic](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-2.png)

![Create SNS topic](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-3.png)

### 3. Create an Email Subscription

1. Open the `job-completed-topic` topic.
2. Choose **Create subscription**.
3. In **Protocol**, select **Email**.
4. In **Endpoint**, enter the email address that will receive notifications.
5. Choose **Create subscription**.

![Create subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-4.png)

![Create subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-5.png)

### 4. Confirm the Email

1. Open the email inbox you entered.
2. Find the confirmation email from AWS Notifications.
3. Choose **Confirm subscription**.
4. Return to SNS and verify that the subscription status has changed to **Confirmed**.

![Confirm subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-6.png)

![Confirm subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-7.png)

### 5. Record the Topic ARN

Open the topic and record the **ARN** to configure the `lambda-result-handler` Lambda.

```text
SNS_TOPIC_ARN=arn:aws:sns:<region>:<account-id>:audio-transcription-notification
```

![SNS ARN](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-8.png)

### 6. Send a Test Notification

1. In the topic, choose **Publish message**.
2. Enter the subject:

```text
AWS Audio To Text Test
```

3. Enter the message:

```text
SNS notification test for AWS Audio To Text workshop.
```

4. Choose **Publish message**.
5. Check that the test notification was received in your email.

![Publish test message](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-9.png)

![Publish test message](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-10.png)
