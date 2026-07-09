---
title: "Deploy Frontend on CloudFront"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

# AWS Audio To Text

#### Deploy Frontend on CloudFront

In this section, you will build the React frontend, upload the build files to the S3 Frontend Bucket, and distribute the website with Amazon CloudFront.

After completing this section, users can access the system through the CloudFront URL, sign in with Cognito, upload audio/video files, and view transcript results.

![Deploy frontend overview](/images/5-Workshop/5.3-DeployFrontend/deploy-1.png)

### 1. Prepare Backend Information

Before building the frontend, you need the following information from section 5.2:

- API Gateway Invoke URL.
- Cognito User Pool ID.
- Cognito App Client ID.
- AWS Region.
- S3 Frontend Bucket name.

Example:

```text
VITE_HTTP_API_BASE=https://tr0pxa2af8.execute-api.ap-southeast-1.amazonaws.com/prod
VITE_USER_ID=demo-user
VITE_AWS_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_usOq0wncO
VITE_COGNITO_APP_CLIENT_ID=es27s0fdh69m14ttvnkr7vrmi
```

### 2. Configure Frontend Environment Variables

In the frontend source directory, create or update the `.env` file.

```text
VITE_HTTP_API_BASE=https://tr0pxa2af8.execute-api.ap-southeast-1.amazonaws.com/prod
VITE_USER_ID=demo-user
VITE_AWS_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_usOq0wncO
VITE_COGNITO_APP_CLIENT_ID=es27s0fdh69m14ttvnkr7vrmi
```

If the frontend uses Create React App instead of Vite, change the `VITE_` prefix to `REACT_APP_` according to your project structure.

### 3. Install Dependencies

Open a terminal in the frontend directory and run:

```bash
npm install
```

### 4. Run the Frontend Locally

Run the frontend locally to test the Cognito and API Gateway connection.

```bash
npm run dev
```

Or:

```bash
npm start
```

Open a browser and access the local URL, for example:

```text
http://localhost:5173
```

Check the following features:

- Register or sign in with Cognito.
- Upload a short audio/video file.
- Check that a job is created in DynamoDB.
- Check that the file is uploaded to the S3 Audio Bucket.
- Check that a message goes to SQS and Lambda processes it.

### 5. Build the Frontend

After the local test succeeds, build the frontend:

```bash
npm run build
```

The build directory is usually:

- `dist` if you use Vite.
- `build` if you use Create React App.


### 6. Upload Build Files to the S3 Frontend Bucket

> Because I have already done this before, I will guide you through the steps.

1. Access **Amazon S3**.
2. Open the `frontend-<account-id>-<region>` bucket.
3. Choose **Upload**.
4. Upload all contents inside the build directory, not the parent directory itself.
5. Choose **Upload**.

![Upload build to S3](/images/5-Workshop/5.3-DeployFrontend/deploy-2.png)

Drag the **assets** folder and the **index.html** file, then choose **Upload**.

![Upload build to S3](/images/5-Workshop/5.3-DeployFrontend/deploy-3.png)

### 7. Create a CloudFront Distribution

1. Access **CloudFront**.
2. Choose **Create distribution**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-4.png)

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-5.png)

3. In **Choose a plan**, select the **Free** plan. Choose **Next**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-6.png)

4. In **Get started**, set **Distribution name** to `audio-to-text-frontend`. Choose **Next**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-7.png)

5. In **Specify origin**, set **Origin type** to **Amazon S3**. For **Origin**, choose **Browse S3**, select the **frontend bucket**, and choose **Next** until you reach **Review and create**.
   
![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-8.png)

6. In **Review and create**, do not change anything and choose **Create distribution**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-9.png)

After the distribution is created, you will receive a domain.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-10.png)

After opening the domain in a browser, you will see your website like this:

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-11.png)

### 8. Update the Bucket Policy for CloudFront

After creating the distribution, CloudFront will show the policy that needs to be added to the S3 bucket.

1. Copy the policy suggested by CloudFront.
2. Open the S3 Frontend Bucket.
3. Select the **Permissions** tab.
4. In **Bucket policy**, choose **Edit**.
5. Paste the policy and choose **Save changes**.

```json
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::frontend-594470794140-ap-southeast-1-an/*",
            "Condition": {
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cloudfront::594470794140:distribution/E21RVUI13QXFH2"
                }
            }
        }
    ]
}
```

![Bucket policy](/images/5-Workshop/5.3-DeployFrontend/deploy-12.png)


### 9. Update CORS

After CloudFront finishes deploying, copy the **Distribution domain name**, for example:

```text
d3cvbj64u8o2eh.cloudfront.net
```

Update CORS:

- API Gateway CORS: add the CloudFront domain to **Access control allowed origin**.

![Update CORS](/images/5-Workshop/5.3-DeployFrontend/deploy-13.png)

### 11. Test the AWS Audio To Text Project

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-1.png)
<small style="display: block; text-align: center;">1. Register a user.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-2.png)
<small style="display: block; text-align: center;">2. Receive the verification code by email.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-3.png)
<small style="display: block; text-align: center;">3. Enter the verification code.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-4.png)
<small style="display: block; text-align: center;">4. Home page.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-5.png)
<small style="display: block; text-align: center;">5. Select an audio file to convert to text.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-6.png)
<small style="display: block; text-align: center;">6. Upload the file successfully.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-7.png)
<small style="display: block; text-align: center;">7. Result after converting the audio file to text.</small>
