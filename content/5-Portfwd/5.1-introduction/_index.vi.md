---
title: "Giới thiệu"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
### Giới thiệu AnTiScaQ (Nền tảng cảnh báo Chống lừa đảo)

**AnTiScaQ** là một nền tảng **cảnh báo chống lừa đảo tự động** hiện đại, được thiết kế để nhằm bảo vệ người dùng khỏi những mối đe dọa an ninh mạng ngày càng tinh vi. Cốt lõi của hệ thống nằm ở sức mạnh **thu thập dữ liệu nguồn mở** thông qua các công cụ thu thập dữ liệu mạnh mẽ, từ đó phân tích sâu và xác minh mức độ rủi ro của từng **số điện thoại, nội dung email và tên miền** nghi ngờ. Điểm khác biệt mang tính đột phá của AnTiScaQ là khả năng cung cấp **điểm số rủi ro theo thời gian thực**, giúp phát hiện, đánh giá và đưa ra cảnh báo tức thì ngay khi các chiến dịch lừa đảo vừa bùng phát, qua đó liên tục làm giàu cơ sở dữ liệu nhận dạng mối đe dọa để bảo vệ an toàn tối đa cho người dùng và cộng đồng.

### Tổng quan Kiến trúc Giải pháp trên AWS

**Kiến trúc giải pháp:**

Hệ sinh thái **AnTiScaQ** được thiết kế dựa trên mô hình kiến trúc **Serverless**, tận dụng toàn diện các dịch vụ tinh hoa của nền tảng điện toán đám mây **AWS** nhằm mang lại khả năng mở rộng linh hoạt và **giảm thiểu tối đa chi phí phát sinh**. Bám sát các trụ cột cốt lõi của **AWS Well-Architected Framework**, giải pháp này đảm bảo hệ thống luôn duy trì trạng thái vận hành bền bỉ, an toàn và sẵn sàng đáp ứng tức thì khối lượng truy cập khổng lồ mỗi khi có các chiến dịch lừa đảo bùng phát.

* **Compute & Logic Layer (Tầng Tính toán & Logic)**
Nằm ở tâm điểm của lớp xử lý, nền tảng sử dụng sức mạnh kết hợp giữa **Amazon API Gateway** và **AWS Lambda** làm động cơ tính toán chính mà **hoàn toàn không cần quản lý máy chủ**. Trong kiến trúc này, các hàm tính toán đảm nhiệm trọng trách xử lý các truy vấn REST API tốc độ cao cho cơ sở dữ liệu mối đe dọa, song song với đó là các hàm Python chuyên biệt phụ trách thực thi những luồng tác vụ thu thập dữ liệu nguồn mở bất đồng bộ một cách trơn tru.

* **Data & Persistence Layer (Tầng Dữ liệu & Lưu trữ)**
Để giải bài toán về hiệu suất truy xuất thời gian thực, hệ thống tin dùng **Amazon DynamoDB** làm cơ sở dữ liệu NoSQL trung tâm, áp dụng kỹ thuật thiết kế bảng đơn (single-table design) để lưu trữ các hồ sơ tình báo với độ trễ phản hồi dưới một giây. Đồng hành cùng lớp cơ sở dữ liệu là **Amazon S3**, được thiết lập để làm kho lưu trữ an toàn cho các tệp dữ liệu phi cấu trúc như hình ảnh bằng chứng, đồng thời đóng vai trò máy chủ lưu trữ toàn bộ tài nguyên tĩnh cho giao diện web portal.

* **Security & Authentication (Bảo mật & Xác thực)**
Nhằm thiết lập một vành đai phòng thủ kiên cố, kiến trúc tích hợp sâu **Amazon Cognito** để quản lý danh tính, hỗ trợ xác thực và phân quyền truy cập dựa trên vai trò (RBAC) một cách cực kỳ chi tiết. Đi kèm với đó, **AWS Secrets Manager** đóng vai trò như một két sắt an toàn giúp tự động lưu trữ và xoay vòng các thông tin nhạy cảm như khóa cơ sở dữ liệu hay token API, trong khi t
ường lửa **AWS WAF** hoạt động ở tuyến đầu để bảo vệ các điểm cuối API khỏi sự lạm dụng của botnet và các đợt tấn công từ chối dịch vụ (DDoS).

* **DevOps & Monitoring (Vận hành & Giám sát)**
Để duy trì nhịp độ phát hành tính năng và đảm bảo chất lượng, toàn bộ quy trình đóng gói và triển khai mã nguồn (CI/CD) đều được tự động hóa xuyên suốt thông qua **GitHub Actions**. Ở khía cạnh vận hành mạng, **Amazon CloudFront** CDN được triển khai kết hợp với **Amazon S3** để phân phối nội dung giao diện web với tốc độ tối đa đến người dùng toàn cầu. Cuối cùng, mọi trạng thái hệ thống từ giới hạn tỷ lệ API đến các tiến trình thu thập dữ liệu đều được theo dõi sát sao bởi bộ đôi **Amazon CloudWatch** và **Amazon SNS**, tự động kích hoạt các luồng cảnh báo tức thì khi hệ thống gặp lỗi hoặc phát hiện ngưỡng chi phí bất thường.