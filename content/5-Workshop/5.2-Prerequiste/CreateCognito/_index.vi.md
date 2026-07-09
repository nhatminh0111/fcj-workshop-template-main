---
title: "Tạo Cognito"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.4 </b> "
---

#### Tạo Cognito

Trong bước này, bạn sẽ tạo Amazon Cognito User Pool để quản lý đăng ký, đăng nhập và xác thực người dùng cho frontend React.

Sau khi đăng nhập thành công, Cognito sẽ cấp JWT token. Frontend dùng token này để gọi API Gateway, còn API Gateway dùng Cognito Authorizer để kiểm tra request.

### 1. Truy cập Amazon Cognito

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **Cognito**.
3. Chọn **Amazon Cognito**.

![Open Cognito](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-1.png)

### 2. Tạo User Pool
> Do mình đã tạo trước đó nên mình sẽ hướng dẫn các bạn làm
1. Chọn **User pools**.
2. Chọn **Create user pool**.

![Create user pool](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-2.png)

3. Ở phần **Application type**, chọn **Single-page application (SPA)**.
4. Ở phần **Name your application**, nhập:

```text
User pool
```

![Create user pool](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-3.png)

5. Ở phần **Options for sign-in identifiers**, chọn **Email**.
6. Ở phần **Self-registration**, giữ nguyên không đổi.
7. Ở phần **Required attributes for sign-up**, chọn trong Select attributes là **email**.
8. Nhấn **Create user directory**.

![Sign in options](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-4.png)

### 3. Cấu hình app client
> Do mình đã tạo trước đó rồi nên mình hướng dẫn các bạn làm.
1. Chọn **App clients**.
2. Chọn **Create app client**.

![App client](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-5.png)

3. Ở phần **Application type**, chọn **Single-page application (SPA)**.
4. Ở phần **Name your application**, đặt tên **audio-transcriber-user-pool**.
5. Nhấn **Create app client**.

![App client](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-6.png)

### 6. Ghi lại thông tin Cognito

Bạn cần ghi lại các giá trị sau để cấu hình frontend và API Gateway:

- **User Pool ID**
- **App Client ID**

![Cognito IDs](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-7.png)

![Cognito IDs](/images/5-Workshop/5.2-Prerequisite/CreateCognito/cognito-8.png)

