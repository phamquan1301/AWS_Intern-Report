---
title: "Giám sát & Vận hành"
weight: 8
chapter: false
pre: " <b> 4.7. </b> "
---

### Tổng quan
Để duy trì sự ổn định cho một hệ thống như **AnTiScaQ**, việc thiết lập cơ chế **Giám sát (Monitoring)** và **Cảnh báo (Alerting)** tự động là yêu cầu bắt buộc. Việc quản trị không nên phụ thuộc vào kiểm tra thủ công, mà cần một quy trình phản hồi tức thì để đảm bảo các luồng phân tích dữ liệu lừa đảo luôn hoạt động thông suốt.

Trong phần này, chúng ta sẽ xây dựng hệ thống quản lý vận hành tập trung cho **AnTiScaQ** dựa trên các giải pháp từ AWS:

* **Amazon CloudWatch:** Công cụ theo dõi và phân tích các thông số vận hành (**Metrics**) chi tiết từ hạ tầng (EC2, RDS, ELB), giúp đánh giá sức khỏe hệ thống theo thời gian thực.
* **Amazon SNS (Simple Notification Service):** Kênh truyền tin cậy giúp kết nối hệ thống giám sát với đội ngũ quản trị. Khi có chỉ số vượt ngưỡng an toàn hoặc xảy ra lỗi dịch vụ, hệ thống sẽ tự động kích hoạt thông báo qua **Email** để xử lý kịp thời.

### Nội dung thực hiện
* [Giám sát với CloudWatch](5.8.1-cloudwatch//)