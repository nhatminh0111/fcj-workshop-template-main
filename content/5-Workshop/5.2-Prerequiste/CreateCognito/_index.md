---
title: "Create Cognito"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.4 </b> "
---

#### Create Cognito

In this step, you will create an Amazon Cognito User Pool to manage user registration, sign-in, and authentication for the React frontend.

After a user signs in successfully, Cognito will issue a JWT token. The frontend uses this token to call API Gateway, and API Gateway uses a Cognito Authorizer to validate the request.

### 1. Access Amazon Cognito

1. Sign in to the **AWS Management Console**.
2. In the search bar, enter **Cognito**.
3. Select **Amazon Cognito**.

![Open Cognito](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-1.png)

### 2. Create a User Pool

> Because I have already created one, I will guide you through the steps.

1. Select **User pools**.
2. Choose **Create user pool**.

![Create user pool](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-2.png)

3. In **Application type**, select **Single-page application (SPA)**.
4. In **Name your application**, enter:

```text
User pool
```

![Create user pool](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-3.png)

5. In **Options for sign-in identifiers**, select **Email**.
6. In **Self-registration**, keep the default settings.
7. In **Required attributes for sign-up**, choose **email** from **Select attributes**.
8. Choose **Create user directory**.

![Sign in options](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-4.png)

### 3. Configure the App Client

> Because I have already created one, I will guide you through the steps.

1. Select **App clients**.
2. Choose **Create app client**.

![App client](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-5.png)

3. In **Application type**, select **Single-page application (SPA)**.
4. In **Name your application**, set the name to **audio-transcriber-user-pool**.
5. Choose **Create app client**.

![App client](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-6.png)

### 6. Record the Cognito Information

You need to record the following values to configure the frontend and API Gateway:

- **User Pool ID**
- **App Client ID**

![Cognito IDs](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-7.png)

![Cognito IDs](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-8.png)
