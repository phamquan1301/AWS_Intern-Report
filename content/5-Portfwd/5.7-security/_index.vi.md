---
title: "Tối ưu hóa & Bảo mật"
weight: 7
chapter: false
pre: " <b> 4.6. </b> "
---

## Tổng quan

Đối với một hệ thống như **AnTiScaQ**, việc đảm bảo tính **khả dụng cao** và **bảo mật tuyệt đối** là ưu tiên hàng đầu để bảo vệ người dùng trước các mối đe dọa trực tuyến. Trong phần này, chúng ta sẽ tối ưu hóa hạ tầng nhằm tăng tốc độ xử lý dữ liệu hình ảnh và thiết lập các lớp phòng thủ kiên cố trước những đòn tấn công mạng phức tạp.

### Các hạng mục thực hiện:

* **Offloading Static Assets (Tối ưu hóa tài nguyên tĩnh):**
    * Chuyển toàn bộ hình ảnh và dữ liệu tĩnh của hệ thống sang **Amazon S3**.
    * Sử dụng **Amazon CloudFront (CDN)** để phân phối nội dung, giúp giảm độ trễ phản hồi (latency) và giảm tải áp lực cho hệ thống xử lý trung tâm.

* **Security Hardening (Củng cố bảo mật):**
    * Triển khai **AWS WAF (Web Application Firewall)** làm lớp rào chắn phía trước CloudFront.
    * Ngăn chặn các nỗ lực tấn công khai thác lỗ hổng như **SQL Injection**, **XSS** và các hình thức xâm nhập trái phép vào hệ thống **AnTiScaQ**.

### Nội dung thực hiện
* [Cấu hình S3 & CloudFront & WAF](5.7.1-cloudfront//)
