---
title : "Tạo S3 Bucket"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

#### Tạo S3 Bucket

Trong bước này, bạn sẽ tạo 3 bucket S3 cho hệ thống:

- **Frontend Bucket**: lưu file build của React frontend.
- **Audio Bucket**: lưu file audio/video người dùng upload.
- **Output Bucket**: lưu kết quả transcript do Amazon Transcribe tạo ra.

### 1. Truy cập Amazon S3

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **S3**.
3. Chọn dịch vụ **S3**.

![Open S3](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-1.png)

### 2. Tạo Frontend Bucket

1. Chọn **Create bucket**.
2. Tại mục **Bucket name**, nhập:

```text
frontend
```

3. Tại mục **AWS Region**, chọn region bạn sử dụng cho workshop, ví dụ `ap-southeast-1`.
4. Ở phần **Object Ownership**, giữ giá trị mặc định.
5. Ở phần **Block Public Access settings**, giữ bật toàn bộ tùy chọn chặn public access.
6. Giữ các cấu hình còn lại mặc định.
7. Chọn **Create bucket**.

![Create frontend bucket](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-2.png)

### 3. Tạo Audio Bucket

1. Chọn **Create bucket**.
2. Nhập tên bucket:

```text
audio-bucket
```

3. Chọn cùng region với Frontend Bucket.
4. Giữ bật **Block all public access**.
5. Chọn **Create bucket**.

![Create input bucket](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-3.png)

### 4. Tạo Output Bucket

1. Chọn **Create bucket**.
2. Nhập tên bucket:

```text
output-bucket
```

3. Chọn cùng region với các bucket còn lại.
4. Giữ bật **Block all public access**.
5. Chọn **Create bucket**.

![Create output bucket](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-4.png)

### 5. Cấu hình CORS cho Audio Bucket

Frontend sẽ upload file trực tiếp lên Audio Bucket bằng presigned URL, vì vậy cần cấu hình CORS cho bucket này.

1. Mở bucket `audio-bucket`.
2. Chọn tab **Permissions**.
3. Kéo xuống phần **Cross-origin resource sharing (CORS)**.
4. Chọn **Edit**.
5. Dán cấu hình sau:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["PUT", "POST", "GET"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": ["ETag"]
  }
]
```

6. Chọn **Save changes**.

![S3 CORS](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-5.png)

### 6. Cấu hình Event Notification cho Audio Bucket

Sau khi người dùng upload file, Audio Bucket sẽ gửi sự kiện đến SQS Queue để Lambda xử lý bất đồng bộ.

> Bạn có thể quay lại bước này sau khi đã tạo SQS Queue ở phần 5.2.3.

1. Mở bucket `audio-bucket`.
2. Chọn tab **Properties**.
3. Kéo xuống phần **Event notifications**.
4. Chọn **Create event notification**.
5. Nhập tên event:

```text
audio-uploaded-event
```
6. Ở phần **Event types**, chọn **All object create events**.

![S3 event notification](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-6.png)

7. Ở phần **Destination**, chọn **SQS queue**.
8. Chọn queue `audio-processing-queue`.
9. Chọn **Save changes**.

![S3 event notification](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-7.png)

### 7. Kiểm tra kết quả

Sau khi hoàn thành, bạn cần có 3 bucket:

- Frontend Bucket dùng để deploy React app.
- Audio Bucket dùng để nhận file upload.
- Output Bucket dùng để lưu transcript.

![S3 buckets result](/images/5-Workshop/5.2-Prerequisite/CreateBucket/s3-8.png)
