---
title: "Triển khai Frontend trên CloudFront"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

# AWS Audio To Text

#### Triển khai Frontend trên CloudFront

Trong phần này, bạn sẽ build frontend React, upload file build lên S3 Frontend Bucket và phân phối website bằng Amazon CloudFront.

Sau khi hoàn thành, người dùng có thể truy cập hệ thống qua CloudFront URL, đăng nhập bằng Cognito, upload file audio/video và xem kết quả transcript.

![Deploy frontend overview](/images/5-Workshop/5.3-DeployFrontend/deploy-1.png)

### 1. Chuẩn bị thông tin backend

Trước khi build frontend, bạn cần có các thông tin sau từ phần 5.2:

- API Gateway Invoke URL.
- Cognito User Pool ID.
- Cognito App Client ID.
- AWS Region.
- Tên S3 Frontend Bucket.

Ví dụ:

```text
VITE_HTTP_API_BASE=https://tr0pxa2af8.execute-api.ap-southeast-1.amazonaws.com/prod
VITE_USER_ID=demo-user
VITE_AWS_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_usOq0wncO
VITE_COGNITO_APP_CLIENT_ID=es27s0fdh69m14ttvnkr7vrmi
```

### 2. Cấu hình biến môi trường frontend

Trong thư mục source frontend, tạo hoặc cập nhật file `.env`.

```text
VITE_HTTP_API_BASE=https://tr0pxa2af8.execute-api.ap-southeast-1.amazonaws.com/prod
VITE_USER_ID=demo-user
VITE_AWS_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_usOq0wncO
VITE_COGNITO_APP_CLIENT_ID=es27s0fdh69m14ttvnkr7vrmi
```

Nếu frontend sử dụng Create React App thay vì Vite, đổi prefix `VITE_` thành `REACT_APP_` theo cấu trúc dự án của bạn.

### 3. Cài đặt dependencies

Mở terminal tại thư mục frontend và chạy:

```bash
npm install
```

### 4. Chạy thử frontend ở local

Chạy frontend local để kiểm tra kết nối Cognito và API Gateway.

```bash
npm run dev
```

Hoặc:

```bash
npm start
```

Mở trình duyệt và truy cập URL local, ví dụ:

```text
http://localhost:5173
```

Kiểm tra các chức năng:

- Đăng ký hoặc đăng nhập bằng Cognito.
- Upload file audio/video ngắn.
- Kiểm tra job được tạo trong DynamoDB.
- Kiểm tra file được upload vào S3 Audio Bucket.
- Kiểm tra message đi vào SQS và Lambda xử lý.

### 5. Build frontend

Sau khi kiểm tra local thành công, build frontend:

```bash
npm run build
```

Thư mục build thường là:

- `dist` nếu dùng Vite.
- `build` nếu dùng Create React App.


### 6. Upload file build lên S3 Frontend Bucket
> Do mình đã làm trước đó rồi nên mình sẽ hướng dẫn các bạn làm.
1. Truy cập **Amazon S3**.
2. Mở bucket `frontend-<account-id>-<region>`.
3. Chọn **Upload**.
4. Upload toàn bộ nội dung trong thư mục build, không upload chính thư mục cha.
5. Chọn **Upload**.

![Upload build to S3](/images/5-Workshop/5.3-DeployFrontend/deploy-2.png)

Thả folder **assets** và file **index.html** rồi ấn **Upload**.

![Upload build to S3](/images/5-Workshop/5.3-DeployFrontend/deploy-3.png)

### 7. Tạo CloudFront Distribution

1. Truy cập **CloudFront**.
2. Chọn **Create distribution**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-4.png)

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-5.png)

3. Ở phần **Choose a plan**, chọn gói **Free**. Nhấn **Next**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-6.png)

4. Ở phần **Get started**, đặt **Distribution name** là `audio-to-text-frontend`. Nhấn **Next**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-7.png)

5. Ở phần **Specify origin**, chỗ **Origin type** chọn **Amazon S3**, chỗ **Origin** Nhấn **Browse S3** và chọn **bucket frontend** và nhấn **Next** đến phần **Review and create**.
   
![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-8.png)

6. Ở phần **Review and create** không thay đổi gì và nhấn **Create distribution**.

![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-9.png)

Sau khi tạo xong Bạn sẽ nhận được domain.
![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-10.png)

Sau khi mở domain bằng trình duyệt bạn sẽ thấy trang web của mình như này:
![Create CloudFront](/images/5-Workshop/5.3-DeployFrontend/deploy-11.png)

### 8. Cập nhật bucket policy cho CloudFront

Sau khi tạo distribution, CloudFront sẽ hiển thị policy cần thêm vào S3 bucket.

1. Copy policy được CloudFront gợi ý.
2. Mở S3 Frontend Bucket.
3. Chọn tab **Permissions**.
4. Tại **Bucket policy**, chọn **Edit**.
5. Dán policy và chọn **Save changes**.
```
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::frontend-594470794140-ap-southeast-1-an/*",
            "Condition": {
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cloudfront::594470794140:distribution/E21RVUI13QXFH2"
                }
            }
        }
    ]
}
```

![Bucket policy](/images/5-Workshop/5.3-DeployFrontend/deploy-12.png)


### 9. Cập nhật CORS

Sau khi CloudFront deploy xong, copy **Distribution domain name**, ví dụ:

```text
d3cvbj64u8o2eh.cloudfront.net
```
Cập nhật CORS
- API Gateway CORS: thêm CloudFront domain vào **Access control allowed origin**.

![Update CORS](/images/5-Workshop/5.3-DeployFrontend/deploy-13.png)

### 11. Kiểm thử dự án AWS Audio To Text

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-1.png)
<small style="display: block; text-align: center;">1. Đăng ký người dùng.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-2.png)
<small style="display: block; text-align: center;">2. Nhận mã xác thực qua Mail.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-3.png)
<small style="display: block; text-align: center;">3. Nhập mã xác thực.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-4.png)
<small style="display: block; text-align: center;">4. Trang chủ.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-5.png)
<small style="display: block; text-align: center;">5. Chọn file audio để chuyển thành văn bản.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-6.png)
<small style="display: block; text-align: center;">6. Upload file thành công.</small>

![End to end test](/images/5-Workshop/5.3-DeployFrontend/sp-7.png)
<small style="display: block; text-align: center;">7. Kết quả sau khi chuyển file audio thành văn bản.</small>