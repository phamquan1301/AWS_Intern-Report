---
title: "Thiết lập cơ sở hạ tầng mạng"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

### Tổng quan
Trong giai đoạn này, chúng ta sẽ xây dựng hạ tầng mạng cơ sở (VPC) và thiết lập các lớp tường lửa bảo mật, tạo nền móng vững chắc cho việc triển khai ứng dụng và cơ sở dữ liệu trên AWS. Việc cấu hình mạng và bảo mật đúng chuẩn là yếu tố tiên quyết để hệ thống hoạt động an toàn và được cô lập hiệu quả trên Cloud.

Kiến trúc hạ tầng mạng sẽ được thiết lập bao gồm:
* **VPC (Virtual Private Cloud):** Khởi tạo vùng mạng riêng ảo trên AWS thông qua trình hướng dẫn "VPC and more", giúp thiết lập nhanh chóng hạ tầng mạng cốt lõi.
* **Security Group (Nhóm bảo mật):** Hoạt động như một tường lửa ảo (virtual firewall) cấp độ tài nguyên, giúp kiểm soát chặt chẽ các quy tắc mạng đầu vào (Inbound rules) cho các hệ thống EC2.
* **DB Subnet Group (Nhóm mạng con cơ sở dữ liệu):** Nhóm các subnet trong VPC lại với nhau để cấp phát không gian mạng lưới riêng biệt, an toàn dành cho các cụm cơ sở dữ liệu Amazon RDS hoặc Aurora.

### Nội dung Thực hiện
* [Tạo VPC](5.3.1-vpc-subnets/) 
* [Tạo Nhóm Bảo Mật (Security Group)](5.3.2-nat-gateway//)  
* [Tạo Nhóm Mạng Con RDS (DB Subnet Group)](5.3.3-route-table//)