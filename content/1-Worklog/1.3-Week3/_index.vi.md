---
title: "Worklog Tuần 3"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Thành thạo CloudFormation để Infrastructure as Code.
* Hiểu rõ về Migration Services và cách di chuyển hạ tầng từ On-premises sang AWS.
* Nắm vững các dịch vụ lưu trữ AWS (S3, EFS, Storage Gateway).
* Hiểu biết toàn diện về AWS Security và Identity Management.
* Thực hành các lab thực tế liên quan đến Network, Storage, Security.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Làm lab 19:** <br>&emsp; + Initialize CloudFormation Templates <br>&emsp; + Create Security group <br>&emsp; + Create EC2 instance <br>&emsp; + Update Network ACL (NACLs) <br>&emsp; + Create a peering connection <br>&emsp; + Configure Route tables <br>&emsp; + Enable Cross-Peer DNS <br>&emsp; + Clean up resources | 04/05/2026 | 04/05/2026 | <https://000019.awsstudygroup.com/vi/2-prerequisite/2.1-launchcloudformation/> |
| 3 | - **Làm lab 14:** <br>&emsp; + VMWare Workstation <br>&emsp; + Export Virtual Machine from On-premises <br>&emsp; + Upload virtual machine to AWS <br>&emsp; + Import virtual machine to AWS <br>&emsp; + Deploy Instance from AMI <br>&emsp; + Setting up S3 bucket ACL <br>&emsp; + Export virtual machine from Instance <br>&emsp; + Resource Cleanup on AWS Cloud | 05/05/2026 | 05/05/2026 | <https://000014.awsstudygroup.com/vi/1-deploy-application-server/> |
| 4 | - **Học lý thuyết:** <br>&emsp; + Module 4: Amazon Simple Storage Service (S3) - Access Point - Storage Class <br>&emsp; + S3 Static Website & CORS - Control Access - Object Key & Performance - Glacier <br>&emsp; + Snow Family - Storage Gateway - Backup <br> - **Làm lab 25:** <br>&emsp; + Create Environment <br>&emsp; + Create an SSD Multi-AZ file system <br>&emsp; + Create an HDD Multi-AZ file system <br>&emsp; + Create new file shares <br>&emsp; + Test Performance <br>&emsp; + Monitor Performance <br>&emsp; + Enable data deduplication <br>&emsp; + Enable shadow copies <br>&emsp; + Manage user sessions and open files <br>&emsp; + Enable user storage quotas <br>&emsp; + Scale throughput capacity <br>&emsp; + Scale storage capacity | 06/05/2026 | 06/05/2026 | <https://www.youtube.com/watch?v=yunIkwcAwc8&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=104> <br> <https://www.youtube.com/watch?v=mPBjB6LtI_Q&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=105> <br> <https://www.youtube.com/watch?v=YXn8Q_Hpsu4&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=106> <br> <https://000025.awsstudygroup.com/2-prerequisite/2.1-cloudformation/> |
| 5 | - **Học lý thuyết Module 5:** <br>&emsp; + Share Responsibility Model <br>&emsp; + Amazon Identity and access management <br>&emsp; + Amazon Identity and access management <br>&emsp; + Amazon Cognito <br>&emsp; + AWS Organization <br>&emsp; + AWS Identity Center <br>&emsp; + Amazon Key Management Service <br>&emsp; + AWS Security Hub | 07/05/2026 | 07/05/2026 | <https://www.youtube.com/watch?v=tsObAlSq19g&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=152> <br> <https://www.youtube.com/watch?v=N_vUJGAqZxo&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=151> <br> <https://www.youtube.com/watch?v=N_vUJGAqZxo&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=151> <br> <https://www.youtube.com/watch?v=pZ2fqEFK3Vs&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=152> <br> <https://www.youtube.com/watch?v=5oQY8Rogz9Y&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=153> <br> <https://www.youtube.com/watch?v=NW1xrMkNMjU&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=154> <br> <https://www.youtube.com/watch?v=GMihNQojhZc&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=155> <br> <https://www.youtube.com/watch?v=clj2E0rNBEs&list=PLahN4TLwtox2a3ElknwzU_urND8hLn1i&index=156> |
| 6 | - **Làm lab 18:** <br>&emsp; + Enable Security Hub <br>&emsp; + Score for each set of criteria | 08/05/2026 | 08/05/2026 | <https://000018.awsstudygroup.com/vi/2-enable-sec-hub/> |
| 7 | - Tham dự Event của FCJ | 09/05/2026 | 09/05/2026 |


### Kết quả đạt được tuần 3:

* Thành thạo Infrastructure as Code (IaC) với CloudFormation:
  * Tạo và quản lý template
  * Deployment tự động hạ tầng AWS
  * Version control cho infrastructure

* Hiểu toàn diện về Migration Services:
  * VMware migration tools
  * Server Migration Service (SMS)
  * Cách chuyển máy ảo từ On-premises sang AWS
  * Import/Export EC2 instances

* Nắm vững các dịch vụ lưu trữ AWS:
  * Amazon S3 (Simple Storage Service)
  * EFS (Elastic File System)
  * Storage Gateway
  * Snow Family
  * Cấu hình Static Website, CORS, Access Control

* Hiểu biết toàn diện về AWS Security:
  * Shared Responsibility Model
  * IAM (Identity and Access Management)
  * Amazon Cognito
  * AWS Organizations
  * AWS Identity Center
  * KMS (Key Management Service)
  * AWS Security Hub

* Thực hành thành công các lab phức tạp:
  * Lab 19: CloudFormation, VPC Peering, NACLs
  * Lab 14: On-premises migration to AWS
  * Lab 25: EFS multi-AZ file systems, performance optimization
  * Lab 18: Security Hub enablement và assessment

* Có khả năng triển khai hạ tầng từ On-premises sang AWS một cách an toàn.
* Nắm vững các best practices về security và compliance trong AWS.
  * Xem dịch vụ EC2
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy


* Có khả năng kết nối giữa giao diện web và CLI để quản lý tài nguyên AWS song song.



