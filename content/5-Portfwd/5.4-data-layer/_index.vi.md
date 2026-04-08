---
title: "Triển khai tầng dữ liệu"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

### Tổng quan
Trong giai đoạn này, chúng ta sẽ thiết lập nền tảng lưu trữ và cơ sở tri thức (Knowledge Base) cho ứng dụng HRM sử dụng Generative AI. Quá trình này bao gồm việc cấu hình kho lưu trữ đối tượng an toàn (S3), nhập dữ liệu cấu trúc vào cơ sở dữ liệu NoSQL tốc độ cao (DynamoDB), và tích hợp sức mạnh của AWS Bedrock để xử lý dữ liệu RAG (Retrieval-Augmented Generation).

Kiến trúc lưu trữ và AI sẽ được thiết lập bao gồm:
* **Amazon S3 (Simple Storage Service):** Khởi tạo 3 bucket riêng biệt với các chính sách bảo mật nghiêm ngặt (chặn truy cập công khai, mã hóa tự động, bật versioning) để lưu trữ dữ liệu AI, báo cáo và hình ảnh.
* **Amazon DynamoDB:** Tạo bảng dữ liệu cấu trúc tự động bằng cách nhập (Import) trực tiếp nguồn dữ liệu có sẵn từ S3 bucket, giúp tiết kiệm thời gian cấu hình schema thủ công.
* **AWS Bedrock Knowledge Base:** Xây dựng bộ não cho AI bằng cách kết nối trực tiếp tài liệu PDF từ S3, cấu hình mô hình nhúng (Embedding) và tự động tạo Vector Store để chatbot có khả năng truy xuất thông tin ngữ nghĩa.

### Nội dung Thực hiện
* [Thiết lập Amazon S3 Bucket](5.4.1-security-groups//) 
* [Nhập Dữ liệu từ S3 vào DynamoDB](5.4.2-rds//)  
* [Thiết lập AWS Bedrock Knowledge Base)](5.4.3-elasticache//)
