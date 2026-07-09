---
title : "Chuẩn bị môi trường"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

### Tổng quan

Trong phần này, bạn sẽ chuẩn bị toàn bộ tài nguyên backend cho hệ thống **AWS Audio To Text**.

Các tài nguyên được tạo trong phần 5.2 gồm:

- Amazon S3 để lưu file frontend, file audio/video đầu vào và transcript đầu ra.
- Amazon DynamoDB để lưu thông tin job, trạng thái xử lý và lịch sử chuyển đổi.
- Amazon SQS để nhận sự kiện upload từ S3 và xử lý bất đồng bộ.
- Amazon Cognito để quản lý đăng ký, đăng nhập và cấp JWT token cho frontend.
- AWS Lambda để tạo presigned URL, xử lý Transcribe, cập nhật kết quả và trả dữ liệu cho frontend.
- Amazon SNS để gửi email thông báo khi quá trình chuyển đổi hoàn tất.
- Amazon API Gateway để frontend gọi các Lambda thông qua HTTP API.

Sau khi hoàn thành phần này, bạn sẽ có đầy đủ backend serverless để frontend có thể đăng nhập, upload file, theo dõi trạng thái job và xem kết quả transcript.

### Nội dung

- [Tạo S3 Bucket](CreateBucket/)
- [Tạo DynamoDB](CreateDynamoDB/)
- [Tạo SQS Queue](CreateSQS/)
- [Tạo Cognito](CreateCognito/)
- [Tạo Lambda Function và Cấu hình cho Lambda](CreateLambda/)
- [Tạo SNS](CreateSNS/)
- [Tạo API Gateway](CreateAPI/)
