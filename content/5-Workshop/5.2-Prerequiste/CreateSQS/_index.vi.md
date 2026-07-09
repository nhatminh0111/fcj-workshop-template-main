---
title : "Tạo SQS Queue"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---

#### Tạo SQS Queue

Trong bước này, bạn sẽ tạo SQS Standard Queue để nhận sự kiện upload file từ S3 Audio Bucket.

SQS giúp tách quá trình upload file khỏi quá trình xử lý Transcribe. Khi file được upload lên S3, sự kiện sẽ được gửi vào queue, sau đó Lambda Processor đọc message từ queue và bắt đầu chuyển đổi audio thành văn bản.

![SQS overview](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-1.png)

### 1. Truy cập Amazon SQS

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **SQS**.
3. Chọn **Simple Queue Service**.

![Open SQS](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-2.png)

### 2. Tạo queue

1. Trong giao diện **SQS**, chọn **Create queue**.
2. Tại mục **Type**, chọn **Standard**.
3. Tại mục **Name**, nhập:

```text
audio-processing-queue
```

![Create queue](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-3.png)

### 3. Cấu hình queue

Giữ các mục sau ở giá trị mặc định:

- **Visibility timeout**
- **Message retention period**
- **Delivery delay**
- **Maximum message size**
- **Receive message wait time**

![Queue configuration](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-4.png)

### 4. Cấu hình Access Policy cho S3

Ở phần **Access policy**, chọn **Advanced** và dán policy bên dưới.

Thay các giá trị sau trước khi lưu:

- `<account-id>`: AWS Account ID của bạn.
- `<region>`: region bạn đang dùng, ví dụ `ap-southeast-1`.
- `<input-bucket-name>`: tên Audio Bucket đã tạo ở phần 5.2.1.

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

### 5. Hoàn tất tạo queue

1. Giữ **Redrive allow policy** và **Dead-letter queue** ở trạng thái mặc định.
2. Chọn **Create queue**.
3. Sau khi tạo xong, mở queue `audio-processing-queue`.
4. Ghi lại **ARN** của queue để dùng khi cấu hình S3 Event Notification và Lambda trigger.

![SQS result](/images/5-Workshop/5.2-Prerequisite/CreateSQS/sqs-6.png)

### 6. Kiểm tra kết nối với S3

Sau khi tạo queue, quay lại phần cấu hình Event Notification của Audio Bucket:

1. Mở S3 Audio Bucket.
2. Vào tab **Properties**.
3. Tạo event notification cho sự kiện **All object create events**.
4. Chọn destination là `audio-processing-queue`.
5. Lưu cấu hình.

Khi người dùng upload file lên Audio Bucket, S3 sẽ gửi message vào queue này để Lambda xử lý.
