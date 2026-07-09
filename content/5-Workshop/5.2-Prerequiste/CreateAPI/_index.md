---
title : "Create API Gateway"
date : 2024-01-01 
weight : 7
chapter : false
pre : " <b> 5.2.7 </b> "
---

#### Create API Gateway

In this step, you will create Amazon API Gateway so the React frontend can call the backend Lambda functions.

API Gateway will provide the main endpoints:

- `POST /upload-url`: creates a presigned URL to upload files.
- `GET /jobs`: retrieves the user's job list.
- `GET /jobs/{job_id}`: retrieves job details and the transcript.

The APIs will be protected by a Cognito JWT Authorizer.

### 1. Access API Gateway

1. Sign in to the **AWS Management Console**.
2. In the search bar, enter **API Gateway**.
3. Select **API Gateway**.

![Open API Gateway](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-1.png)

### 2. Create an HTTP API

> Because I have already done this before, I will guide you through the steps.

1. Select the **APIs** tab.
2. Choose **Create API**.
  
![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-2.png)

3. In **HTTP API**, choose **Build**.
4. In **API name**, enter:

```text
meeting-transcriber-http-api
```

5. Temporarily skip adding an integration if the console asks for one.
6. Choose **Next** until you reach the review step.
7. Choose **Create**.

![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-3.png)

![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-4.png)

![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-5.png)

### 3. Create Lambda Integrations

In the API you just created:

1. Select **Integrations**.
2. Choose **Manage integrations**.
3. Choose **Create**.
4. For Integration type, select **Lambda function**.
5. Select the region you are using.
6. Select the corresponding Lambda function.
7. Repeat these steps to create integrations for the following Lambda functions:

- `lambda-upload-url`
- `lambda-get-job`
- `lambda-list-jobs`

![Create integrations](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-6.png)

### 4. Create the POST /upload-url Route

1. Select **Routes**.
2. Choose **Create**.
3. For Method, select **POST**.
4. For Path, enter:

```text
/upload-url
```

5. Select the `lambda-upload-url` integration.
6. Choose **Create**.

![Upload route](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-7.png)

### 5. Create the GET /jobs Route

1. Choose **Create route**.
2. For Method, select **GET**.
3. For Path, enter:

```text
/jobs
```

4. Select the `lambda-list-jobs` integration.
5. Choose **Create**.

![List jobs route](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-8.png)

### 6. Create the GET /job Route

1. Choose **Create route**.
2. For Method, select **GET**.
3. For Path, enter:

```text
/job
```

4. Select the `lambda-get-job` integration.
5. Choose **Create**.

![Get job route](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-9.png)

### 7. Configure the Cognito JWT Authorizer

1. In API Gateway, select **Authorization**.
2. Choose **Manage authorizers**.
3. Choose **Create**.
4. For Authorizer type, select **JWT**.
5. Name:

```text
cognito-jwt-authorizer
```

6. Identity source:

```text
$request.header.Authorization
```

7. Issuer URL:

```text
https://cognito-idp.<region>.amazonaws.com/<user-pool-id>
```

8. For Audience, enter the Cognito **App Client ID**.
9. Choose **Create**.

> After it is created, the result will appear as shown in the image below.

![JWT authorizer](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-10.png)

### 8. Attach the Authorizer to Routes

1. Select each route:
   - `POST /upload-url`
   - `GET /jobs`
   - `GET /job`
2. In **Authorization**, choose **Detach authorizer**.
3. Select `cognito-jwt-authorizer`, then choose **Attach authorizer**.
4. Choose **Save**.

![Attach authorizer](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-11.png)

![Attach authorizer](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-12.png)

### 9. Configure CORS

1. Select **CORS**.
2. Choose **Configure**.
3. Add the Allowed origins:

```text
http://localhost:5173
http://127.0.0.1:5173
```

After deploying the frontend to CloudFront, you will add the CloudFront domain to this list.

3. Allowed methods:

```text
GET,POST,OPTIONS
```

4. Allowed headers:

```text
Authorization,Content-Type
```

5. Choose **Save**.

![CORS](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-13.png)

### 10. Record the Invoke URL

1. Select **Stages**.
2. Open the default stage, usually `$default`.
3. Record the **Invoke URL**.

![Invoke URL](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-14.png)
