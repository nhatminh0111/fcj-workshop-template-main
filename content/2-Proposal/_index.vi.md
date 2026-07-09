---
title: "Bản đề xuất"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AWS Audio To Text 
## Hệ thống chuyển đổi audio thành văn bản trên AWS

### 1. Tổng quan dự án

Dự án AWS Audio To Text là một hệ thống serverless trên AWS cho phép người dùng upload các file audio hoặc video như MP3, MP4, WAV, M4A để tự động chuyển đổi nội dung giọng nói thành văn bản.

Người dùng truy cập website thông qua Amazon CloudFront, đăng nhập bằng Amazon Cognito, sau đó upload file lên Amazon S3 bằng presigned URL. File audio sau khi upload sẽ được xử lý tự động qua S3 Event, SQS, Lambda và Amazon Transcribe. Kết quả transcript được lưu lại trong S3 Output Bucket, trạng thái job được lưu trong DynamoDB, và hệ thống gửi email thông báo thông qua Amazon SNS.

Trong workshop này, nhóm sẽ trình bày toàn bộ quá trình xây dựng hệ thống từ thiết kế kiến trúc, triển khai các dịch vụ AWS, viết code Lambda, tích hợp frontend React, kiểm thử pipeline và deploy website lên CloudFront.

### 2. Mục tiêu

Mục tiêu chính của dự án là xây dựng một hệ thống chuyển đổi audio thành văn bản hoạt động tự động trên nền tảng AWS.<br/>
Các mục tiêu cụ thể gồm:
- Xây dựng hệ thống serverless: Sử dụng Lambda, S3, SQS, API Gateway, DynamoDB để giảm việc quản lý server
- Hỗ trợ upload file audio/video: Cho phép người dùng upload MP3, MP4, WAV, M4A
- Chuyển đổi audio thành text: Sử dụng Amazon Transcribe để tạo transcript
- Quản lý người dùng: Dùng Amazon Cognito để đăng ký, đăng nhập và xác thực người dùng
- Lưu lịch sử chuyển đổi: Lưu metadata, trạng thái job và đường dẫn transcript trong DynamoDB
- Gửi thông báo	Dùng Amazon SNS để gửi email khi job hoàn tất
- Deploy website online	Host frontend React bằng S3 Frontend Bucket và CloudFront
- Theo dõi và debug	Sử dụng CloudWatch Logs để kiểm tra hoạt động của Lambda

### 3. Vấn đề cần giải quyết

*Vấn đề hiện tại* </br>
Sau các cuộc họp, buổi học hoặc phỏng vấn online, người dùng thường phải nghe lại toàn bộ file audio/video để ghi chú nội dung chính. Việc này tốn thời gian, dễ bỏ sót thông tin và khó quản lý khi có nhiều file. Ngoài ra, file audio/video thường có dung lượng lớn, nếu upload qua backend truyền thống có thể gây quá tải. Quá trình chuyển audio thành văn bản cũng mất thời gian, nên cần một hệ thống xử lý tự động, bất đồng bộ và bảo mật theo từng người dùng.

*Giải pháp* </br>
Hệ thống AWS Audio To Text sử dụng kiến trúc serverless trên AWS để tự động chuyển đổi audio thành văn bản. Người dùng đăng nhập bằng Amazon Cognito, sau đó upload file trực tiếp lên Amazon S3 thông qua presigned URL. Khi file được upload, S3 Event gửi message vào Amazon SQS, sau đó AWS Lambda gọi Amazon Transcribe để chuyển audio thành text. Kết quả được lưu vào S3 Output Bucket, trạng thái và lịch sử xử lý được lưu trong DynamoDB, đồng thời Amazon SNS gửi email thông báo khi hoàn tất. Website được triển khai bằng S3 Frontend Bucket và CloudFront, giúp người dùng truy cập, xem transcript, tải kết quả và quản lý lịch sử file dễ dàng.

### 4. Kiến trúc giải pháp
Hệ thống được thiết kế theo mô hình serverless, gồm các tầng chính: frontend, authentication, API, processing, storage, database, notification và monitoring.

![AWS Audio To Text](/images/2-Proposal/AWS-AudioToText_Architech.jpg)<small style="display: block; text-align: center;">*Sơ đồ kiến trúc của dự án*</small>

*Dịch vụ AWS sử dụng*  
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

*Thiết kế thành phần*  
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

### 5. Thời gian triển khai  
- Ngày 1: Phân tích yêu cầu, xác định chức năng chính, vẽ sơ đồ kiến trúc và luồng hoạt động	
- Ngày 2: Tạo S3 buckets, Cognito User Pool, App Client, cấu hình đăng nhập người dùng
- Ngày 3: Tạo DynamoDB, SQS, API Gateway, IAM Role và policy cho Lambda
- Ngày 4: Code lambda-upload-url, lambda-processor, lambda-result-handler
- Ngày 5: Code lambda-get-job, lambda-list-jobs, tích hợp API Gateway với Cognito Authorizer
- Ngày 6: Code frontend React, tích hợp Cognito, upload file, hiển thị transcript, nút tải xuống
- Ngày 7: Test end-to-end, cấu hình SNS email, deploy frontend lên S3 + CloudFront, kiểm tra CloudWatch Logs

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=b998e2ea0ba1c7f9369be3ab26ea8cde51f8f7c6)  

*Chi phí hạ tầng*  
- *Amazon CloudFront*: 0,12 USD/tháng (1 GB data transfer out to internet, 1.000 HTTPS request/tháng).
- *Amazon S3*: 0,03 USD/tháng (1 GB S3 Standard, 500 PUT/COPY/POST/LIST request, 1.000 GET/SELECT/other request).
- *Amazon Cognito*: 0,00 USD/tháng (10 người dùng hoạt động hằng tháng).
- *Amazon API Gateway*: 0,00 USD/tháng (HTTP API 0,001 triệu request/tháng tương đương 1.000 request, kích thước trung bình 34 KB/request).
- *AWS Lambda*: 0,00 USD/tháng (1.000 request/tháng, kiến trúc x86, invoke mode Buffered, 512 MB ephemeral storage).
- *Amazon SQS*: 0,00 USD/tháng (0,001 triệu Standard Queue request/tháng tương đương 1.000 request).
- *Amazon Transcribe*: 0,60 USD/tháng (100 phút xử lý batch bằng standard Amazon Transcribe).
- *Amazon DynamoDB*: 0,03 USD/tháng (Table class Standard, dung lượng lưu trữ 0,1 GB, kích thước item trung bình 1 KB).
- *Amazon SNS*: 0,00 USD/tháng (1.000 request/tháng, 1.000 EMAIL/EMAIL-JSON notification).
- *Amazon CloudWatch*: 0,07 USD/tháng (0,1 GB Standard Logs ingested).

*Tổng*: 0,85 USD/tháng, 10,20 USD/12 tháng 

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- Lỗi IAM: Ảnh hưởng cao, xác suất trung bình. Lambda có thể không truy cập được S3, DynamoDB, Transcribe hoặc SNS.
- Lỗi CORS: Ảnh hưởng trung bình, xác suất cao. Frontend có thể không gọi được API hoặc không upload được file lên S3.
- S3 Event/SQS không hoạt động: Ảnh hưởng cao, xác suất trung bình. File upload thành công nhưng không được đưa vào pipeline xử lý.
- Transcribe lỗi: Ảnh hưởng cao, xác suất trung bình. File sai định dạng, âm thanh kém hoặc sai language code có thể khiến hệ thống không tạo transcript.
- Vượt ngân sách AWS: Ảnh hưởng trung bình, xác suất thấp. Chi phí có thể tăng nếu test nhiều file lớn hoặc chạy nhiều job Transcribe.
- Lỗi Cognito: Ảnh hưởng trung bình, xác suất trung bình. Người dùng có thể không đăng nhập được hoặc API trả lỗi xác thực.

*Chiến lược giảm thiểu*
- IAM: Cấp quyền đúng cho từng Lambda và kiểm tra lỗi qua CloudWatch Logs.
- CORS: Cấu hình CORS cho API Gateway, S3 Audio Bucket và S3 Output Bucket.
- S3 Event/SQS: Kiểm tra Event Notification, SQS policy và Lambda trigger.
- Transcribe: Chỉ cho phép các định dạng MP3, MP4, WAV, M4A và test bằng file audio ngắn, rõ tiếng.
- Chi phí: Dùng file test dung lượng nhỏ, đặt AWS Budget Alert và xóa tài nguyên không cần thiết sau khi demo.
- Cognito: Kiểm tra User Pool ID, App Client ID, JWT Authorizer và Authorization header.

*Kế hoạch dự phòng*
- Nếu pipeline tự động lỗi, nhóm sẽ kiểm tra từng bước thủ công từ S3, SQS, Lambda, Transcribe đến DynamoDB.
- Nếu trigger chưa hoạt động, có thể test Lambda bằng event mẫu. 
- Nếu frontend lỗi sau khi deploy, nhóm sẽ quay lại chạy local để kiểm tra API trước, sau đó build và deploy lại lên S3/CloudFront.
- Nếu chi phí tăng, nhóm sẽ tạm dừng trigger, xóa file test và kiểm tra Billing.