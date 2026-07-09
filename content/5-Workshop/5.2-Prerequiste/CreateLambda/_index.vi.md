---
title: "Tạo Lambda Function và Cấu hình cho Lambda"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.2.5 </b> "
---

#### Tạo Lambda Function và Cấu hình cho Lambda

Trong bước này, bạn sẽ tạo các Lambda function cho backend serverless của hệ thống AWS Audio To Text.

Hệ thống sử dụng 5 Lambda chính:

- `lambda-upload-url`: tạo presigned URL để frontend upload file lên S3.
- `lambda-processor`: đọc message từ SQS và gọi Amazon Transcribe.
- `lambda-result-handler`: xử lý transcript sau khi Transcribe hoàn tất, cập nhật DynamoDB và gửi SNS.
- `lambda-get-job`: lấy chi tiết một job và nội dung transcript.
- `lambda-list-jobs`: lấy danh sách job của người dùng đang đăng nhập.

### 1. Truy cập Lambda

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **Lambda**.
3. Chọn **Lambda**.

![Open Lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-1.png)

### 2. Tạo Lambda upload-url
> Do mình đã làm trước đó rồi nên mình sẽ hướng dẫn các bạn làm
1. Truy cập dịch vụ **Lambda**.
2. Chọn **Create function**.
3. Chọn **Author from scratch**.
4. Nhập tên function:

```text
lambda-upload-url
```

1. Runtime: chọn **Python 3.12**.
2. Chọn **Create function**.

![Create upload lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-2.png)

![lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-3.png)

### 4. Cấu hình biến môi trường cho lambda-upload-url

1. Trong tab **Configuration**, chọn **Environment variables**.
2. Chọn **Edit**.
3. Thêm các cấu hình này vào ở **Edit environment variables**:

```text
AUDIO_BUCKET = <your-name-audio-bucket>
JOBS_TABLE = jobs
URL_EXPIRES_SECONDS = 900
```
4. Nhấn **Save**.
   
Function này nhận thông tin file từ frontend, tạo `job_id`, lưu item vào DynamoDB và trả về presigned URL.

![lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-4.png)

![lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-5.png)

### 5. Tạo Lambda processor

Tạo function mới với tên:

```text
lambda-processor
```

Cấu hình role giống bước trên và thêm biến môi trường:

```text
JOBS_TABLE = jobs
LANGUAGE_CODE = vi-VN
OUTPUT_BUCKET = <your-name-output-bucket>
```

Function này đọc message từ SQS, lấy thông tin file trên S3, gọi Amazon Transcribe và cập nhật trạng thái job sang `PROCESSING`.

![Processor lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-7.png)

### 6. Gắn SQS trigger cho lambda-processor

1. Mở function `lambda-processor`.
2. Trong tab **Configuration**, chọn **Triggers**.
4. Chọn **Add trigger**.
3. Ở phần **Source**, chọn **SQS**.
4. Chọn queue `audio-processing-queue`.
5. Giữ **Batch size** mặc định hoặc đặt là `1` để dễ debug.
6. Chọn **Add**.

>Sau khi Add trigger kết quả hiện ra như hình bên dưới.

![SQS trigger](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-8.png)

### 7. Tạo Lambda result-handler

Tạo function mới với tên:

```text
lambda-result-handler
```
   
Thêm biến môi trường:

```text
CONNECTIONS_TABLE = ams-connections
JOBS_TABLE = jobs
OUTPUT_BUCKET = <your-name-output-bucket>
SNS_TOPIC_ARN = <your-name-sns-topic>
```

Function này đọc file `transcript.json`, trích xuất nội dung text, tạo file `transcript.txt`, cập nhật DynamoDB sang `COMPLETED` và gửi email thông báo qua SNS.

![Result handler lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-9.png)

### 8. Gắn S3 trigger cho lambda-result-handler

1. Mở function `lambda-result-handler`.
2. Chọn **Add trigger**.
3. Chọn source là **S3**.
4. Chọn Output Bucket.
5. Event type: chọn **All object create events**.
6. Prefix: **transcripts/**, Suffix: `.json`.
7. Chọn **Add**.
   
>Sau khi Add trigger kết quả hiện ra như hình bên dưới.

![S3 trigger](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-10.png)

### 9. Tạo Lambda get-job

Tạo function mới với tên:

```text
lambda-get-job
```

Thêm biến môi trường:

```text
DOWNLOAD_URL_EXPIRES_SECONDS = 900
JOBS_TABLE = jobs
OUTPUT_BUCKET = <your-name-output-bucket>
```

Function này nhận `job_id`, kiểm tra job trong DynamoDB và trả về trạng thái cùng nội dung transcript nếu đã hoàn tất.

![Get job lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-11.png)

### 10. Tạo Lambda list-jobs

Tạo function mới với tên:

```text
lambda-list-jobs
```

Thêm biến môi trường:

```text
JOBS_TABLE = jobs
```

Function này lấy danh sách job thuộc về người dùng đang đăng nhập để hiển thị lịch sử chuyển đổi trên frontend.

![List jobs lambda](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-12.png)

### 11. Kiểm tra CloudWatch Logs

Sau khi tạo các function, mở tab **Monitor** của từng Lambda và chọn **View CloudWatch logs**.

Khi test pipeline, CloudWatch Logs sẽ giúp bạn kiểm tra:

- Lambda có nhận đúng event không.
- Lambda có đọc được S3, SQS và DynamoDB không.
- Transcribe job có được tạo thành công không.
- SNS có gửi thông báo thành công không.

![CloudWatch logs](/images/5-Workshop/5.2-Prerequisite/CreateLambda/lambda-13.png)
