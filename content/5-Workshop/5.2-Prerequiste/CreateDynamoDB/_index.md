---
title : "Create DynamoDB"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---

#### Create DynamoDB

In this step, you will create a DynamoDB table to store the metadata and processing status of each audio/video file.

The DynamoDB table will be used by Lambda functions to:

- Create a job record when a user requests a presigned URL.
- Update the status when Lambda starts processing the file.
- Store the transcript path after Amazon Transcribe completes.
- Return the conversion history list to the user.

### 1. Access DynamoDB

1. Sign in to the **AWS Management Console**.
2. In the search bar, enter **DynamoDB**.
3. Select **DynamoDB**.

![Open DynamoDB](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-1.png)


### 2. Create the jobs Table

> Because I have already done this before, I will guide you through creating the table.

1. In the left menu, select **Tables**.
2. Choose **Create table**.
   
![Choose Table](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-2.png)

3. In **Table name**, enter:
  
```text
jobs 
```

4. In **Partition key**, enter:

```text
job_id
```

5. Select the **String** data type.
6. Keep the remaining settings as default.
7. Choose **Create table**.

![Create table](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-3.png)

### 3. Verify the Table Configuration

After the table is created, verify the main information:

> After creating the table, wait until the table status changes to Active. This means the table has been created successfully.

- **Table name**: `jobs` 
- **Partition key**: `job_id`
- **Capacity mode**: On-demand
- **Table status**: Active

![Table active](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-4.png)
