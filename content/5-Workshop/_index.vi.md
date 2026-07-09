---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# AWS Audio To Text

#### Tổng quan

Dự án AWS Audio To Text là một hệ thống serverless trên AWS cho phép người dùng upload các file audio hoặc video như MP3, MP4, WAV, M4A để tự động chuyển đổi nội dung giọng nói thành văn bản.

Người dùng truy cập website thông qua Amazon CloudFront, đăng nhập bằng Amazon Cognito, sau đó upload file lên Amazon S3 bằng presigned URL. File audio sau khi upload sẽ được xử lý tự động qua S3 Event, SQS, Lambda và Amazon Transcribe. Kết quả transcript được lưu lại trong S3 Output Bucket, trạng thái job được lưu trong DynamoDB, và hệ thống gửi email thông báo thông qua Amazon SNS.

Trong workshop này, nhóm sẽ trình bày toàn bộ quá trình xây dựng hệ thống từ thiết kế kiến trúc, triển khai các dịch vụ AWS, viết code Lambda, tích hợp frontend React, kiểm thử pipeline và deploy website lên CloudFront. 

![overview](/images/5-Workshop/aws-audio-to-text.jpg)
  
#### Các dịch vụ AWS sử dụng trong dự án
- *Amazon CloudFront*: Phân phối website React qua HTTPS.  
- *Amazon S3*: Lưu file build của frontend React, lưu file audio/video người dùng upload, lưu transcript.json và transcript.txt (3 bucket).  
- *Amazon Cognito*: Quản lý đăng ký, đăng nhập và cấp JWT token.
- *Amazon API Gateway*: Cung cấp API cho frontend gọi backend.
- *AWS Lambda*: Xử lý logic backend serverless (5 hàm).
- *Amazon SQS*: Hàng đợi xử lý file audio bất đồng bộ.
- *Amazon Transcribe*: Chuyển đổi audio thành văn bản.
- *Amazon DynamoDB*: Lưu thông tin job, trạng thái xử lý và lịch sử file.
- *Amazon SNS*: Gửi email thông báo khi xử lý hoàn tất.
- *Amazon CloudWatch*: Lưu log và hỗ trợ debug Lambda.  
#### Nội dung

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Triển khai Frontend trên CloudFront](5.3-DeployFrontend/)
4. [Dọn dẹp tài nguyên](5.4-Cleanup/)
