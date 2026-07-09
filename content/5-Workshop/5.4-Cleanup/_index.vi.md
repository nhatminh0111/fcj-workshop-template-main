---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Dọn dẹp tài nguyên

Sau khi hoàn thành workshop, bạn nên xóa các tài nguyên đã tạo để tránh phát sinh chi phí không cần thiết.

Các tài nguyên cần dọn dẹp gồm:

- CloudFront Distribution
- S3 buckets và object bên trong bucket
- API Gateway
- Lambda functions
- SQS Queue
- SNS Topic và Subscription
- DynamoDB Table
- Cognito User Pool
- CloudWatch Log Groups
- IAM Role và IAM Policy đã tạo cho Lambda
- Amazon Transcribe jobs còn lưu trong tài khoản


### 1. Xóa CloudFront Distribution

1. Truy cập **CloudFront**.
2. Chọn distribution dùng cho frontend.
3. Chọn **Disable**.
4. Chờ trạng thái distribution chuyển sang **Disabled**.
5. Chọn **Delete**.

> CloudFront có thể cần vài phút để disable trước khi cho phép xóa.

### 2. Xóa object trong S3 buckets

Trước khi xóa bucket, bạn cần xóa toàn bộ object bên trong.

Thực hiện cho 3 bucket:

- `frontend-<account-id>-<region>`
- `audio-bucket-<account-id>-<region>`
- `audio-output-<account-id>-<region>`

Các bước:

1. Truy cập **Amazon S3**.
2. Mở từng bucket.
3. Chọn toàn bộ object.
4. Chọn **Delete**.
5. Nhập `permanently delete` nếu console yêu cầu xác nhận.


### 3. Xóa S3 buckets

Sau khi bucket đã rỗng:

1. Quay lại danh sách bucket.
2. Chọn từng bucket.
3. Chọn **Delete**.
4. Nhập tên bucket để xác nhận.
5. Chọn **Delete bucket**.


### 4. Xóa API Gateway

1. Truy cập **API Gateway**.
2. Chọn API `audio-to-text-api`.
3. Chọn **Actions** hoặc menu delete.
4. Chọn **Delete**.
5. Xác nhận xóa API.


### 5. Xóa Lambda functions

Truy cập **Lambda** và xóa các function sau:

- `lambda-upload-url`
- `lambda-processor`
- `lambda-result-handler`
- `lambda-get-job`
- `lambda-list-jobs`

Các bước:

1. Chọn function cần xóa.
2. Chọn **Actions**.
3. Chọn **Delete**.
4. Xác nhận xóa.


### 6. Xóa SQS Queue

1. Truy cập **Amazon SQS**.
2. Chọn queue `audio-processing-queue`.
3. Chọn **Delete**.
4. Xác nhận xóa queue.


### 7. Xóa SNS Topic

1. Truy cập **Amazon SNS**.
2. Chọn **Topics**.
3. Chọn topic `job-completed-topic`.
4. Chọn **Delete**.
5. Xác nhận xóa topic.


### 8. Xóa DynamoDB Table

1. Truy cập **DynamoDB**.
2. Chọn **Tables**.
3. Chọn bảng `jobs`.
4. Chọn **Delete**.
5. Xác nhận xóa bảng.


### 9. Xóa Cognito User Pool

1. Truy cập **Amazon Cognito**.
2. Chọn **User pools**.
3. Mở User Pool của workshop.
4. Chọn **Delete user pool**.
5. Nhập tên User Pool để xác nhận nếu console yêu cầu.
6. Chọn **Delete**.


### 10. Xóa CloudWatch Log Groups

Mỗi Lambda sẽ tạo một log group trong CloudWatch Logs. Xóa các log group này để dọn sạch môi trường.

1. Truy cập **CloudWatch**.
2. Chọn **Log groups**.
3. Tìm các log group:

```text
/aws/lambda/lambda-upload-url
/aws/lambda/lambda-processor
/aws/lambda/lambda-result-handler
/aws/lambda/lambda-get-job
/aws/lambda/lambda-list-jobs
```

4. Chọn log group.
5. Chọn **Delete**.


### 11. Xóa IAM Role và Policy

1. Truy cập **IAM**.
2. Chọn **Roles**.
3. Tìm role:

```text
audio-to-text-lambda-role
```

4. Gỡ các policy đã gắn vào role nếu cần.
5. Chọn **Delete role**.

Nếu bạn có tạo inline policy hoặc customer managed policy riêng cho workshop, hãy xóa policy đó sau khi role đã được xóa.


### 12. Kiểm tra Amazon Transcribe

1. Truy cập **Amazon Transcribe**.
2. Chọn **Transcription jobs**.
3. Xóa các job test nếu không cần giữ lại.

