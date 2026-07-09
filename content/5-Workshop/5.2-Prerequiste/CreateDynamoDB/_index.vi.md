---
title : "Tạo DynamoDB"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---

#### Tạo DynamoDB

Trong bước này, bạn sẽ tạo bảng DynamoDB để lưu metadata và trạng thái xử lý của từng file audio/video.

Bảng DynamoDB sẽ được các Lambda sử dụng để:

- Tạo bản ghi job khi người dùng yêu cầu presigned URL.
- Cập nhật trạng thái khi Lambda bắt đầu xử lý file.
- Lưu đường dẫn transcript sau khi Amazon Transcribe hoàn tất.
- Trả danh sách lịch sử chuyển đổi cho người dùng.

### 1. Truy cập DynamoDB

1. Đăng nhập vào **AWS Management Console**.
2. Tại thanh tìm kiếm, nhập **DynamoDB**.
3. Chọn **DynamoDB**.

![Open DynamoDB](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-1.png)


### 2. Tạo bảng jobs
>Do mình đã làm trước đó rồi nên mình sẽ hướng dẫn các bạn tạo bảng
1. Trong menu bên trái, chọn **Tables**.
2.  Chọn **Create table**.
   
![Choose Table](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-2.png)

3. Tại mục **Table name**, nhập:
  
```text
jobs 
```

4. Tại mục **Partition key**, nhập:

```text
job_id
```

5. Chọn kiểu dữ liệu **String**.
6. Giữ các thiết lập còn lại mặc định.
7. Chọn **Create table**.

![Create table](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-3.png)

### 3. Kiểm tra cấu hình bảng

Sau khi bảng được tạo, kiểm tra các thông tin chính:
> Sau khi tạo bảng bạn hãy đợi Table status chuyển sang Active là thành công nhé.
- **Table name**: `jobs` 
- **Partition key**: `job_id`
- **Capacity mode**: On-demand
- **Table status**: Active

![Table active](/images/5-Workshop/5.2-Prerequisite/CreateDynamoDB/dynamodb-4.png)

