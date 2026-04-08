---
title: "Triển khai ứng dụng"
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---

### Tổng quan
Trong giai đoạn này, chúng ta sẽ tiến hành hiện đại hóa và tự động hóa quy trình triển khai ứng dụng. Cụ thể, ứng dụng Spring Boot sẽ được đóng gói thành một Docker container, lưu trữ an toàn trên kho lưu trữ của AWS, và cuối cùng được triển khai lên một môi trường có khả năng tự động mở rộng và cân bằng tải.

Kiến trúc triển khai sẽ được thiết lập bao gồm:
* **Docker:** Đóng gói ứng dụng cùng toàn bộ môi trường và thư viện phụ thuộc (dependencies) thành một Image đồng nhất, đảm bảo ứng dụng chạy ổn định ở bất kỳ đâu (từ local đến production).
* **Amazon ECR (Elastic Container Registry):** Một kho lưu trữ (repository) private, an toàn và có khả năng mở rộng trên AWS dùng để chứa và quản lý các Docker images của bạn.
* **AWS Elastic Beanstalk:** Dịch vụ nền tảng (PaaS) giúp đơn giản hóa việc triển khai. Nó sẽ tự động xử lý các tác vụ phức tạp như cấp phát tài nguyên (EC2), cấu hình cân bằng tải (Application Load Balancer), và tự động mở rộng quy mô (Auto Scaling) dựa trên cấu hình tệp `Dockerrun.aws.json`.

### Nội dung Thực hiện
* [Đóng gói ứng dụng với Docker](5.5.1-docker//) 
* [Build & Push Docker Image lên Amazon ECR](5.5.2-beanstalk//)  
* [Khởi tạo Elastic Beanstalk)](5.5.3-env-vars//)
