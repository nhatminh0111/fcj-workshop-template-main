---
title : "Tạo API Gateway"
date : 2024-01-01 
weight : 7
chapter : false
pre : " <b> 5.2.7 </b> "
---

#### Tạo API Gateway

Trong bước này, bạn sẽ tạo Amazon API Gateway để frontend React gọi backend Lambda.

API Gateway sẽ cung cấp các endpoint chính:

- `POST /upload-url`: tạo presigned URL để upload file.
- `GET /jobs`: lấy danh sách job của người dùng.
- `GET /jobs/{job_id}`: lấy chi tiết job và transcript.

Các API sẽ được bảo vệ bằng Cognito JWT Authorizer.

### 1. Truy cập API Gateway

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **API Gateway**.
3. Chọn **API Gateway**.

![Open API Gateway](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-1.png)

### 2. Tạo HTTP API
> Do mình đã làm trước đó nên mình sẽ hướng dẫn các bạn làm.
1. Chọn tab **APIs**.
2. Chọn **Create API**.
  
![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-2.png)

3. Ở phần **HTTP API**, chọn **Build**
4. Tại mục **API name**, nhập:

```text
meeting-transcriber-http-api
```

5. Tạm thời bỏ qua phần add integration nếu console yêu cầu.
6. Chọn **Next** đến bước review.
7. Chọn **Create**.

![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-3.png)

![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-4.png)

![Create HTTP API](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-5.png)

### 3. Tạo Lambda integration

Trong API vừa tạo:

1. Chọn **Integrations**.
2. Chọn **Manage integrations**.
3. Chọn **Create**.
4. Integration type: chọn **Lambda function**.
5. Chọn region đang dùng.
6. Chọn Lambda tương ứng.
7. Lặp lại để tạo integration cho các Lambda sau:

- `lambda-upload-url`
- `lambda-get-job`
- `lambda-list-jobs`

![Create integrations](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-6.png)

### 4. Tạo route POST /upload-url

1. Chọn **Routes**.
2. Chọn **Create**.
3. Method: chọn **POST**.
4. Path: nhập:

```text
/upload-url
```

5. Chọn integration `lambda-upload-url`.
6. Chọn **Create**.

![Upload route](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-7.png)

### 5. Tạo route GET /jobs

1. Chọn **Create route**.
2. Method: chọn **GET**.
3. Path: nhập:

```text
/jobs
```

4. Chọn integration `lambda-list-jobs`.
5. Chọn **Create**.

![List jobs route](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-8.png)

### 6. Tạo route GET /job

1. Chọn **Create route**.
2. Method: chọn **GET**.
3. Path: nhập:

```text
/job
```

4. Chọn integration `lambda-get-job`.
5. Chọn **Create**.

![Get job route](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-9.png)

### 7. Cấu hình Cognito JWT Authorizer

1. Trong API Gateway, chọn **Authorization**.
2. Chọn **Manage authorizers**.
3. Chọn **Create**.
4. Authorizer type: chọn **JWT**.
5. Name:

```text
cognito-jwt-authorizer
```

6. Identity source:

```text
$request.header.Authorization
```

7. Issuer URL:

```text
https://cognito-idp.<region>.amazonaws.com/<user-pool-id>
```

8. Audience: nhập **App Client ID** của Cognito.
9. Chọn **Create**.

> Sau khi tạo xong sẽ có kết quả như hình bên dưới
![JWT authorizer](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-10.png)

### 8. Gắn authorizer vào route

1. Chọn từng route:
   - `POST /upload-url`
   - `GET /jobs`
   - `GET /job`
2. Ở phần **Authorization**, chọn **Detach authorizer**.
3. Chọn `cognito-jwt-authorizer`, chọn **Atach authorizer**
4. Chọn **Save**.

![Attach authorizer](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-11.png)

![Attach authorizer](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-12.png)

### 9. Cấu hình CORS

1. Chọn **CORS**.
2. Chọn **Configure**.
3. Thêm Allowed origins:

```text
http://localhost:5173
http://127.0.0.1:5173
```

Sau khi deploy frontend lên CloudFront, bạn sẽ thêm domain CloudFront vào danh sách này.

3. Allowed methods:

```text
GET,POST,OPTIONS
```

4. Allowed headers:

```text
Authorization,Content-Type
```

5. Chọn **Save**.

![CORS](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-13.png)

### 10. Ghi lại Invoke URL

1. Chọn **Stages**.
2. Mở stage mặc định, thường là `$default`.
3. Ghi lại **Invoke URL**.

![Invoke URL](/images/5-Workshop/5.2-Prerequisite/CreateAPI/api-14.png)
