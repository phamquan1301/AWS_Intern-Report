---
title : "Bản đề xuất"
date: 2026-01-10
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

## 1. Tóm tắt điều hành
**AnTiScaQ** là nền tảng cảnh báo mối đe dọa (threat intelligence) tích hợp, được thiết kế để bảo vệ người dùng bằng cách tự động xác minh độ uy tín và mức độ rủi ro của số điện thoại, nội dung email và tên miền. Hệ thống tự động hóa toàn bộ quy trình từ việc thu thập dữ liệu (scraping), tiếp nhận báo cáo từ cộng đồng, chấm điểm rủi ro. Đây là **dự án in-house** do team tự phát triển, tập trung vào **MVP với chi phí tối ưu dưới $50/tháng** trong giai đoạn đầu, sử dụng Lambda, API Gateway và DynamoDB để đảm bảo hiệu năng cao khi có đột biến traffic và giữ chi phí vận hành ở mức thấp nhất.

## 2. Tuyên bố vấn đề

**Vấn đề hiện tại** 
* Người dùng thường phải tìm kiếm Google thủ công hoặc dùng các cơ sở dữ liệu phân mảnh để kiểm tra số điện thoại/link đáng ngờ, gây tốn thời gian và rủi ro.
* Quá trình thu thập dữ liệu cảnh báo mối đe dọa **chưa được tích hợp** và làm thủ công là chính.
* Không có **workflow xác minh tự động** để đối chiếu các báo cáo lừa đảo.
* Chi phí quá cao để mua API từ các nền tảng Threat Intel doanh nghiệp (như Recorded Future, VirusTotal).
* Báo cáo yếu, **không real-time** khi các chiến dịch phishing mới bùng nổ.

**Giải pháp đề xuất**
Hệ thống sử dụng **AWS Serverless Architecture** để tối ưu chi phí và tự động mở rộng:
* **Compute:** AWS Lambda (pay-per-use).
* **API:** API Gateway REST API, HTTP API.
* **Database:** DynamoDB (on-demand billing cho tốc độ truy vấn cực nhanh), Amazon RDS lưu trữ dữ liệu có cấu trúc.
* **Cache:** ElastiCache Redis (cache.t3.micro) - optional cho phase 2 để cache các domain lừa đảo phổ biến.
* **Authentication:** AWS Cognito (free tier <50K MAU) quản lý API key và quyền admin.
* **Storage:** S3 lưu trữ log scraping và bằng chứng hình ảnh, CloudFront CDN.
* **CI/CD:** GitHub Actions tự động deploy.
* **Monitoring:** CloudWatch (free tier), SNS.
* **Security:** Route 53, WAF (cost-optimized rules chống abuse API), IAM Roles.
* **AI/LLM:** Amazon Bedrock dùng để xây dựng chatbot , hỗ trợ giải thích kết quả phát hiện lừa đảo, tư vấn và tương tác với người dùng.
* **Backend Service:** AWS Elastic Beanstalk (EC2) triển khai backend (Spring Boot), xử lý business logic và điều phối hệ thống.
* **CDN & Delivery:** Amazon CloudFront phân phối nội dung frontend (static web từ S3), giảm latency và tăng tốc độ tải toàn cầu.

**Tính năng chính**
* **Single Sign-On** (Google, Microsoft 365) cho Admin/Moderator.
* **RBAC chi tiết** (Admin, Moderator, Public User).
* **Tra cứu real-time** cho domains, emails, và số điện thoại.
* **Workflow phê duyệt** báo cáo lừa đảo từ người dùng để tránh false positive.
* **Web portal** cho phép public search và crowdsourced reporting.
* **Extension** có thể tra cứu số điện thoại, tên miền hoặc nội dung email.

**Lợi ích**
* Tiết kiệm **70%** thời gian kiểm tra thủ công.
* Giảm **90%** sai sót (false positive) nhờ đối chiếu đa nguồn.
* Chi phí chỉ **dưới $50/tháng** cho quy mô đầu (rẻ hơn 90% so với mua nguồn Threat Intel thương mại).
* **In-house development** - không tốn phí mua data hay outsourcing.

## 3. Kiến trúc giải pháp

**Dịch vụ AWS sử dụng**

| Dịch vụ AWS | Chức năng chính |
| :--- | :--- |
| **AWS Lambda** | Chatbot service |
| **API Gateway** | Request validation |
| **Amazon DynamoDB** | NoSQL database |
| **AWS Cognito** | Authentication | 
| **Amazon S3** | Document Storage |
| **CloudFront** | CDN cho static assets và S3|
| **Route 53** | DNS management |
| **AWS WAF** | API protection |
| **CloudWatch** | Logs, monitoring |
| **Bedrock** | AI Model |
| **EC2** |  |
| **Elastic Bean Stalk** | Quản lý credentials cho proxy và 3rd-party APIs |
| **Secrets Manager** | API keys, credentials |
| **RDS my SQL** |  |


**Thiết kế thành phần**
* **Authentication Layer:** Cognito User Pools với JWT (RS256). Lambda authorizer cho API Gateway để validate API keys.
* **AI Layer:** AWS Lambda functions (Logic Handler) integrated with Amazon Bedrock for LLM processing and RAG via S3 Vector. API Gateway serves as a Webhook Endpoint and REST API for Telegram and web
* **Compute layer - AWS Elastic Beanstalk (or ECS):** Leverages the Docker platform to package and deploy Java Spring Boot applications. This simplifies infrastructure management and automates Auto Scaling and Elastic Load Balancing.
* **Business Logic (Lambda Functions):**
  * Threat lookups (đọc từ DynamoDB/Redis).
  * Data Aggregation (Python web scrapers cào dữ liệu từ public blacklists).
  * Report management (CRUD cho các báo cáo do người dùng gửi).
  * Email notifications (SES free tier cảnh báo mối đe dọa).
* **Data Layer - DynamoDB Tables:**
  * `Users` - GSI on email
  * `ThreatIntel` - GSI on `indicator_type` (phone/email/domain) + `risk_score`
  * `ScamReports` - GSI on `target_indicator` + `date`
  * `ScrapingLogs` - GSI on `source` + `status`
* **Storage Layer:** S3 Standard cho evidence mới (<30 days). S3 Lifecycle → Glacier Deep Archive (>90 days). Presigned URLs cho secure upload. CloudFront distribution.
* **Frontend:** React.js 14 (React 18) + TypeScript - Static export. Material-UI components. Hosted trên CloudFront + S3 (no server cost).

## 4. Triển khai kỹ thuật

* **Step 1:** Nền tảng & Thiết lập (AWS setup, Cognito, DynamoDB tables, S3, Lambda, EC2). Authentication UI. Threat CRUD APIs + admin dashboard.
* **Step 2:** Public lookup APIs. Next.js web portal MVP. Workflow báo cáo lừa đảo thủ công. Basic trending dashboard.
* **Step 3:** Dữ liệu & Phân tích chuyên sâu. Xử lý logic cốt lõi.
* **Step 4:** Hoàn thiện Web Portal & Tính năng Front-end nâng cao.
* **Step 5:** Kiểm thử, Bảo mật & Tối ưu hóa (Security, Load testing, Auto-scaling).
* **Step 6:** Tài liệu hóa, Đào tạo & Bàn giao (Documentation, Demo, Handover).

**Yêu cầu Kỹ thuật**

* **Frontend & Bảng điều khiển:** Giao diện sử dụng framework Next.js, được lưu trữ tĩnh trên Amazon S3 và phân phối tốc độ cao toàn cầu qua mạng CDN CloudFront.
* **Backend & Xử lý Logic:** Logic cốt lõi được lập trình bằng Python 3.12 chạy trên nền tảng AWS Lambda và được định tuyến thông qua Amazon API Gateway.
* **Dữ liệu & Lưu trữ:** Dữ liệu hệ thống sử dụng Amazon DynamoDB để đảm bảo tốc độ truy vấn siêu tốc, kết hợp với Amazon S3 để lưu trữ an toàn các tệp tin bằng chứng.
* **Cơ sở hạ tầng (IaC):** Toàn bộ tài nguyên hạ tầng AWS được định nghĩa và quản lý tự động hoàn toàn bằng mã thông qua AWS Cloud Development Kit (CDK).
* **Bảo mật & Giám sát:** Hệ thống áp dụng xác thực đa lớp AWS Cognito, quản lý quyền hạn chặt chẽ qua IAM và được giám sát, ghi log liên tục bởi Amazon CloudWatch.

## 5. Lộ trình & Mốc triển khai

| Tuần | Giai đoạn | Deliverable Chính |
| :--- | :--- | :--- |
| **1-2** | MVP Core | Auth, Threat Database, Public Search Portal, Báo cáo thủ công |
| **3-4** | Automation | Python Scraping Engine, Cấp phát API Keys, Alert notifications |
| **5-6** | Advanced & Launch | AI Scoring, Advanced Analytics, Security WAF, API Go-live |

## 6. Ước tính ngân sách

**Chi phí AWS hàng tháng (Phase 1: ~5,000 API lookups/ngày)**
*Kiến trúc Serverless - Chi phí tối ưu*

| Dịch vụ | Cấu hình | Chi phí/tháng |
| :--- | :--- | :--- |
| **AWS Lambda** | 15K invocations, 512MB, 4000ms avg | $0 *(Free tier)* |
↳ Free tier: 1M requests + 400K GB-seconds/month
| **API Gateway** | 15K REST API requests | $0 |
| **DynamoDB** | On-demand, 5GB storage, 1M reads, 0.5M write | $0.5 |
| **S3 Vector Bucket** | 2GB data, PUT/GET | $0.60 |
| **Bedrock** | Model: Claude Haiku 3, Input Token: 6GB, Output Token: 4GB | $6.5 |
| **CloudFront** | 10GB transfer, 200K requests | $1.00 |
| **Route 53** | 1 hosted zone | $0.90 |
| **CloudWatch**| Log và Auth cơ bản | $0 *(Free tier)* |
| **Secrets Manager**| Quản lý key proxy, gửi email | $0.85 |
| **Elastic Beanstalk** | 5GB to internet | $0.45 |
| **Cognito** | Buffer | $0.75 |
| **RDS my SQL** | Buffer | $0.75 |
| **WAF** | Buffer | $0.75 |
| **EC2** | Buffer | $0.75 |
| | **TỔNG AWS/THÁNG (Phase 1)** | **~$8.30** |


## 7. Đánh giá rủi ro & Giảm thiểu

| Rủi ro | Ảnh hưởng | Xác suất | Giảm thiểu |
| :--- | :--- | :--- | :--- |
| **Scraper bị block IP** | Cao | Cao | Sử dụng rotating proxy pools (lưu trong Secrets Manager) và randomize thời gian cào dữ liệu. |
| **False Positives (Báo cáo sai)** | Cao | Trung bình | Yêu cầu xác minh chéo từ nhiều nguồn, có workflow duyệt thủ công cho các domain lớn. |
| **DynamoDB costs spike** | Trung bình | Thấp | Dùng On-demand billing, set CloudWatch alarms ở mức $30. Dùng SQS batching khi ghi dữ liệu lớn. |
| **Lambda time limits** | Thấp | Trung bình | Chia nhỏ các task Python scraping nặng thành nhiều function; tối ưu bundle size. |

**Best practices tối ưu chi phí**
* **Lambda:** Bundle size <1MB, tái sử dụng DB connections để tránh cold starts.
* **DynamoDB:** Single-table design, dùng GSI cẩn thận, on-demand billing.
* **S3:** Lifecycle policies chuyển sang Glacier, dùng presigned URLs.
* **API Gateway:** Bật Response caching (30-60s) cho các API public lookup, setup throttling.

## 8. Kết quả kỳ vọng

Triển khai thành công nền tảng AnTiScaQ dự kiến sẽ mang lại những bước tiến đột phá không chỉ về mặt công nghệ mà còn đóng góp to lớn vào hiệu quả vận hành và chiến lược phát triển dài hạn.

* **Cải tiến kỹ thuật (Technical Improvements)**
* Khả năng mở rộng siêu tốc (Hyper-Scalability): Nhờ kiến trúc 100% Serverless (Lambda, API Gateway, DynamoDB), hệ thống có khả năng tự động mở rộng tức thì để xử lý các đợt bùng phát lưu lượng truy cập (traffic spikes) khi có chiến dịch lừa đảo mới mà không gặp tình trạng nghẽn cổ chai hay sập nguồn.
* Tự động hóa luồng dữ liệu tình báo: Chuyển đổi hoàn toàn từ việc tra cứu và thu thập dữ liệu thủ công sang quy trình tự động hóa (automated scraping & threat intelligence), kết hợp với quy trình CI/CD qua GitHub Actions giúp rút ngắn thời gian phát hành tính năng mới.
* Độ trễ thấp, hiệu năng cao: Việc kết hợp DynamoDB (truy xuất dữ liệu siêu tốc) và CloudFront CDN đảm bảo tốc độ phản hồi API cực nhanh trên toàn cầu, mang lại trải nghiệm mượt mà cho cả người dùng cuối và các đối tác tích hợp API.
* Bảo mật và toàn vẹn dữ liệu chuẩn Enterprise: Áp dụng RBAC chi tiết qua AWS Cognito, bảo vệ điểm cuối bằng AWS WAF và lưu trữ khóa bảo mật qua Secrets Manager giúp hệ thống tuân thủ các tiêu chuẩn bảo mật khắt khe nhất ngay từ giai đoạn MVP.

* **Giá trị kinh doanh (Business Value)**
* Tối ưu hóa chi phí đột phá (Cost Efficiency): Với mô hình *pay-per-use* của kiến trúc Serverless, chi phí vận hành hệ thống được ép xuống mức cực thấp (chỉ khoảng ~$8.30/tháng cho giai đoạn đầu), tiết kiệm hơn 90% ngân sách so với việc mua bản quyền dữ liệu từ các nền tảng Threat Intel thương mại lớn.
* Tăng cường năng suất vận hành: Giúp đội ngũ giảm thiểu 70% thời gian dành cho việc xác minh, kiểm tra thủ công các số điện thoại/domain đáng ngờ, từ đó tập trung vào các nghiệp vụ phân tích chuyên sâu hơn.
* Độ tin cậy của dữ liệu cao (Data Accuracy): Cơ chế đối chiếu đa nguồn tự động kết hợp với luồng phê duyệt (approval workflow) từ báo cáo của cộng đồng giúp giảm 90% tỷ lệ cảnh báo sai (false positive), cung cấp nguồn dữ liệu sạch và chính xác.
* Sở hữu tài sản trí tuệ (In-house IP): Việc tự phát triển nền tảng giúp tổ chức hoàn toàn làm chủ nguồn dữ liệu tình báo, không bị phụ thuộc vào bên thứ ba và có thể linh hoạt tùy biến tính năng theo nhu cầu nội bộ.

* **Tầm nhìn dài hạn (Long-term Vision)**
* Trở thành Trung tâm Dữ liệu Chống lừa đảo chuẩn mực: Xây dựng AnTiScaQ thành một hệ sinh thái dữ liệu tình báo cộng đồng (crowdsourced threat intel) lớn mạnh, có khả năng chia sẻ API cho các đối tác, ngân hàng, hoặc công ty viễn thông để bảo vệ người dùng cuối.
* Tích hợp sâu AI/Machine Learning: Mở rộng sử dụng Amazon Bedrock và các mô hình LLM tiên tiến để phân tích tự động nội dung email, tin nhắn, tự động gán nhãn mức độ rủi ro (AI Scoring) và dự đoán các chiến dịch lừa đảo trước khi chúng lan rộng.
* Mở rộng đa nền tảng: Phát triển thêm các công cụ hỗ trợ người dùng như tiện ích mở rộng trên trình duyệt (Browser Extensions), ứng dụng di động hoặc tích hợp trực tiếp vào hệ thống tổng đài tự động để cảnh báo thời gian thực ngay khi có cuộc gọi đến.

## 9. Kết luận

Hệ thống **AnTiScaQ** với **Serverless Architecture** cung cấp:
* ✅ **Chi phí:** Chỉ $8-12/tháng cho Phase 1.
* ✅ **No upfront cost:** Khoảng ~$30 setup, tận dụng team in-house.
* ✅ **ROI khủng:** Loại bỏ hoàn toàn chi phí mua Threat Intel feeds đắt đỏ bên ngoài.
* ✅ **Scalable:** Pay-as-you-go, tự động xử lý hàng triệu request khi traffic bùng nổ.
* ✅ **Zero maintenance:** Serverless = không cần quản trị server.
* ✅ **Fast development:** 6 tháng từ MVP đến production.

Đây là giải pháp **lý tưởng cho các startup/SME** hoặc team an toàn thông tin muốn xây dựng một công cụ Cybersecurity mạnh mẽ, hiện đại mà không cần ngân sách hạ tầng khổng lồ.