---
title: "Worklog Tuần 5"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Học về Database concepts và các dịch vụ database trên AWS.
* Hiểu rõ về Amazon RDS, Aurora, Redshift và Elasticache.
* Thực hành triển khai hạ tầng với VPC, Security Groups, và Database instances.
* Nắm vững AWS Data Pipeline services (Glue, Kinesis, Athena, QuickSight).
* Thực hành Lambda automation và cost optimization strategies.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Học module 06-01:** Database Concepts review <br> - **Học module 06-02:** Amazon RDS & Amazon Aurora <br> - **Học module 06-03:** Redshift - Elasticache | 18/05/2026 | 18/05/2026 | <https://www.youtube.com/watch?v=OQD2RwWu4T_LWtox2a3ElknwzU_urND8hLn1i&index=217> <br> <https://www.youtube.com/watch?v=qbroBQZrokY&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=218> <br> <https://www.youtube.com/watch?v=UvdiRW34aNI&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=220> |
| 3 | - **Làm lab 5:** <br>&emsp; + Create a VPC <br>&emsp; + Create EC2 Security Group <br>&emsp; + Create RDS Security group <br>&emsp; + Create DB Subnet Group <br>&emsp; + Create EC2 instance <br>&emsp; + Create RDS database instance <br>&emsp; + Application Deployment <br>&emsp; + Backup and restore <br>&emsp; + Clean up resources | 19/05/2026 | 19/05/2026 | <https://000005.awsstudygroup.com/vi/2-prerequisite/1-create-vpc/> |
| 4 | - **Làm lab 35:** <br>&emsp; + Creating an IAM Role <br>&emsp; + Create Policy <br>&emsp; + Create S3 Bucket <br>&emsp; + Creating a Delivery Stream <br>&emsp; + Create Sample Data <br>&emsp; + Create Glue Crawler <br>&emsp; + Data Check <br>&emsp; + Data Transformation <br>&emsp; + Analysis with Athena <br>&emsp; + Visualize with QuickSight <br>&emsp; + Clean up resources | 20/05/2026 | 20/05/2026 | <https://000035.awsstudygroup.com/vi/2-prerequisite/2.1-createiamrole/> |
| 5 | - **Làm lab 40:** <br>&emsp; + Preparing the database <br>&emsp; + Building a database <br>&emsp; + Data in the Table <br>&emsp; + Cost <br>&emsp; + Tagging and Cost Allocation <br>&emsp; + Usage <br>&emsp; + Clean up resources | 21/05/2026 | 21/05/2026 | <https://000040.awsstudygroup.com/vi/2-prerequisite/2.1-preparedatabase/> |
| 6 | - **Làm lab 22:** <br>&emsp; + Create VPC <br>&emsp; + Create Security Group <br>&emsp; + Create EC2 instance <br>&emsp; + Incoming Web-hooks slack <br>&emsp; + Create Tag for Instance <br>&emsp; + Create Role for Lambda <br>&emsp; + Function stop instance <br>&emsp; + Function start instance <br>&emsp; + Check Result <br>&emsp; + Clean up resources | 22/05/2026 | 22/05/2026 | <https://000022.awsstudygroup.com/vi/2-prerequisite/2.1-createvpc/> |


### Kết quả đạt được tuần 5:
* Hiểu sâu sắc về khái niệm và kiến trúc Database:
  * Database quan hệ (Relational) và phi quan hệ (Non-relational)
  * Key-value store
  * Bộ nhớ đệm trong bộ nhớ (in-memory cache)

* Nắm vững các dịch vụ database trên AWS:
  * Amazon RDS (Relational Database Service)
  * Amazon Aurora - database tương thích với MySQL và PostgreSQL
  * Amazon Redshift - kho dữ liệu (data warehouse)
  * Amazon Elasticache - bộ nhớ đệm trong bộ nhớ (in-memory caching)

* Thực hành triển khai hạ tầng database:
  * Tạo VPC và security group
  * Tạo RDS instance với sao lưu & khôi phục (backup & recovery)
  * Triển khai ứng dụng cùng với database

* Hiểu biết về các dịch vụ Data Pipeline của AWS:
  * AWS Glue - trích xuất, chuyển đổi, tải dữ liệu (ETL)
  * Amazon Kinesis - truyền dữ liệu theo thời gian thực (real-time data streaming)
  * Amazon Athena - truy vấn dữ liệu bằng SQL
  * Amazon QuickSight - trực quan hóa dữ liệu

* Thực hành tối ưu chi phí và gắn thẻ (tagging):
  * Thiết lập thẻ phân bổ chi phí (cost allocation tags)
  * Theo dõi việc sử dụng và phân tích chi phí
  * Các chiến lược tối ưu database

* Thực hành tự động hóa với Lambda:
  * Tạo Lambda function
  * Tích hợp với SNS/Slack webhook
  * Tự động hóa quản lý tài nguyên (khởi động/dừng instance)

* Thực hành thành công các lab:
  * Lab 5: Hoàn thành triển khai database với VPC
  * Lab 35: Data pipeline với Glue, Athena, QuickSight
  * Lab 40: Phân tích và tối ưu chi phí database
  * Lab 22: Tự động hóa Lambda tích hợp với Slack

* Có khả năng triển khai và quản lý hạ tầng database đầy đủ.
* Khả năng tự động hóa các tác vụ AWS bằng Lambda và kiến trúc hướng sự kiện (event-driven architecture).
  * Xem dịch vụ EC2
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy

* Có khả năng kết hợp giữa giao diện web và CLI để quản lý tài nguyên AWS song song.


