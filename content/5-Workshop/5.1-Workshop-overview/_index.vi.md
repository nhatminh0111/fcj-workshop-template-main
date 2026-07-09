---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về dự án AWS Audio To Text

Dự án AWS Audio To Text là một hệ thống serverless trên AWS cho phép người dùng upload các file audio hoặc video như MP3, MP4, WAV, M4A để tự động chuyển đổi nội dung giọng nói thành văn bản.

Người dùng truy cập website thông qua Amazon CloudFront, đăng nhập bằng Amazon Cognito, sau đó upload file lên Amazon S3 bằng presigned URL. File audio sau khi upload sẽ được xử lý tự động qua S3 Event, SQS, Lambda và Amazon Transcribe. Kết quả transcript được lưu lại trong S3 Output Bucket, trạng thái job được lưu trong DynamoDB, và hệ thống gửi email thông báo thông qua Amazon SNS.

#### Tổng quan về workshop
- *Giao diện web*: React được build và lưu trữ trên Amazon S3 Frontend Bucket, sau đó phân phối qua Amazon CloudFront để người dùng truy cập hệ thống bằng HTTPS.
- *Quản lý người dùng*: Amazon Cognito xử lý đăng ký, đăng nhập và cấp JWT token để xác thực người dùng khi gọi API.
- *Cổng API*: Amazon API Gateway nhận request từ frontend, kiểm tra token Cognito và chuyển request đến các Lambda phù hợp.
- *Upload file*: lambda-upload-url tạo presigned URL để người dùng upload file audio/video trực tiếp lên Amazon S3 Audio Bucket, đồng thời tạo thông tin job trong DynamoDB.
- *Lưu trữ file đầu vào*: Amazon S3 Audio Bucket lưu các file MP3, MP4, WAV hoặc M4A do người dùng upload lên.
- *Hàng đợi xử lý*: Amazon SQS nhận sự kiện từ S3 Audio Bucket sau khi file được upload, giúp hệ thống xử lý bất đồng bộ và tránh quá tải.
- *Xử lý chuyển đổi*: lambda-processor đọc message từ SQS và gọi Amazon Transcribe để chuyển nội dung audio thành văn bản.
- *Lưu trữ kết quả*: Amazon S3 Output Bucket lưu kết quả transcript gồm transcript.json do Transcribe tạo ra và transcript.txt đã được xử lý lại để người dùng dễ đọc.
- *Xử lý kết quả*: lambda-result-handler đọc file transcript.json, tạo file transcript.txt, cập nhật trạng thái job trong DynamoDB và gửi thông báo qua SNS.
- *Quản lý trạng thái*: Amazon DynamoDB lưu thông tin job, trạng thái xử lý, user_id, tên file, thời gian tạo và đường dẫn transcript.
- *Lấy kết quả*: lambda-get-job đọc trạng thái job từ DynamoDB, lấy nội dung transcript.txt từ S3 Output Bucket và trả kết quả về giao diện web.
- *Lịch sử chuyển đổi*: lambda-list-jobs lấy danh sách các file đã upload và chuyển đổi của người dùng đang đăng nhập.
- *Thông báo*: Amazon SNS gửi email thông báo khi quá trình chuyển đổi audio thành văn bản hoàn tất.
Giám sát hệ thống: Amazon CloudWatch ghi log của các Lambda để hỗ trợ kiểm tra, debug và theo dõi hoạt động của hệ thống. 