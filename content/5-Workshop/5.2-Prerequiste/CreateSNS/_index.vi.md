---
title: "Tạo SNS"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.2.6 </b> "
---

#### Tạo SNS

Trong bước này, bạn sẽ tạo Amazon SNS Topic để gửi email thông báo khi quá trình chuyển đổi audio thành văn bản hoàn tất.

Lambda `lambda-result-handler` sẽ publish message vào SNS Topic sau khi transcript được tạo thành công.


### 1. Truy cập Amazon SNS

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **SNS**.
3. Chọn **Simple Notification Service**.

![SNS](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-1.png)

### 2. Tạo topic
> Do mình đã làm trước đó nên mình sẽ hướng dẫn các bạn làm.
1. Trong menu bên trái, chọn **Topics**.
2. Chọn **Create topic**.
3. Ở phần **Type**, chọn **Standard**.
4. Tại mục **Name**, nhập:

```text
job-completed-topic
```

5. Giữ các cấu hình còn lại mặc định.
6. Chọn **Create topic**.

![Create SNS topic](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-2.png)

![Create SNS topic](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-3.png)

### 3. Tạo email subscription

1. Mở topic `job-completed-topic`.
2. Chọn **Create subscription**.
3. Ở phần **Protocol**, chọn **Email**.
4. Ở phần **Endpoint**, nhập email nhận thông báo.
5. Chọn **Create subscription**.

![Create subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-4.png)

![Create subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-5.png)

### 4. Xác nhận email

1. Mở hộp thư email đã nhập.
2. Tìm email xác nhận từ AWS Notifications.
3. Chọn **Confirm subscription**.
4. Quay lại SNS và kiểm tra trạng thái subscription đã chuyển sang **Confirmed**.

![Confirm subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-6.png)

![Confirm subscription](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-7.png)

### 5. Ghi lại Topic ARN

Mở topic và ghi lại **ARN** để cấu hình cho Lambda `lambda-result-handler`.

```text
SNS_TOPIC_ARN=arn:aws:sns:<region>:<account-id>:audio-transcription-notification
```

![SNS ARN](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-8.png)

### 6. Gửi thử thông báo

1. Trong topic, chọn **Publish message**.
2. Nhập subject:

```text
AWS Audio To Text Test
```

3. Nhập message:

```text
SNS notification test for AWS Audio To Text workshop.
```

4. Chọn **Publish message**.
5. Kiểm tra email đã nhận được thông báo test.

![Publish test message](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-9.png)

![Publish test message](/images/5-Workshop/5.2-Prerequisite/CreateSNS/sns-10.png)
