---
title : "Bản đề xuất"
date: 2026-01-10
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

## 1. Tóm tắt điều hành
**AnTiScaQ** là hệ thống kiểm tra nguy cơ lừa đảo trực tuyến, hỗ trợ người dùng phát hiện sớm các dấu hiệu lừa đảo thông qua nhiều loại dữ liệu đầu vào như số điện thoại, domain, nội dung email bằng cách xác minh độ uy tín và cảnh báo theo mức độ rủi ro. Hệ thống không đưa ra kết luận pháp lý mà chỉ đóng vai trò cảnh báo, giải thích và hướng dẫn phòng tránh. Hệ thống thu thập dữ liệu, tiếp nhận báo cáo từ cộng đồng, chấm điểm rủi ro.

## 2. Tuyên bố vấn đề

**Vấn đề hiện tại** 
* Lừa đảo trực tuyến đang gia tăng nhanh chóng và ngày càng tinh vi, gây ra thiệt hại lớn cho người dùng và doanh nghiệp.
* Không có hệ thống đáng tin cậy, dễ đưa ra quyết định sai.
* Người dùng thường phải tìm kiếm thủ công qua các nền tảng trình duyệt (Google, Microsoft Edge, ...) hoặc dùng các cơ sở dữ liệu phân mảnh để kiểm tra số điện thoại/link đáng ngờ, gây tốn thời gian và rủi ro.
* Các công cụ hiện có thường không có **phân tích ngữ cảnh nội dung**
  - Thiếu dữ liệu **từ cộng đồng người dùng**
  - Không cung cấp **giải thích rõ ràng về mức độ rủi ro**
* Chi phí quá cao để mua API từ các nền tảng Threat Intel doanh nghiệp (như Recorded Future, VirusTotal).
* Báo cáo thiếu sót, **không real-time** khi các chiến dịch phishing mới bùng nổ.

**Giải pháp đề xuất**
Để giải quyết các vấn đề về lừa đảo trực tuyến ngày càng gia tăng và thiếu hệ thống kiểm chứng đáng tin cậy, AnTiScaQ đề xuất một nền tảng dựa trên kiến trúc **Hybrid Cloud-Native** với các định hướng chính:

- **Tự động hóa phát hiện rủi ro:**  
  Phân tích đa nguồn (số điện thoại, domain, URL, nội dung email) kết hợp rule-based và AI để phát hiện dấu hiệu lừa đảo sớm.

- **Phân tích ngữ cảnh bằng AI:**  
  Sử dụng Amazon Bedrock để hiểu nội dung và cung cấp giải thích rõ ràng về mức độ rủi ro, giúp người dùng dễ dàng ra quyết định.

- **Khai thác sức mạnh cộng đồng:**  
  Xây dựng hệ thống báo cáo và chấm điểm uy tín dựa trên đóng góp người dùng, giúp dữ liệu luôn được cập nhật và phản ánh thực tế.

- **Tối ưu chi phí & khả năng mở rộng:**  
  Áp dụng kiến trúc Hybrid:
  - Elastic Beanstalk (Docker) cho backend chính  
  - Lambda cho AI & xử lý linh hoạt  
  → Giảm chi phí so với hệ thống enterprise truyền thống

- **Tích hợp hệ sinh thái AWS:**  
  Sử dụng Cognito, RDS, DynamoDB, S3, CloudFront, WAF… để đảm bảo bảo mật, hiệu năng và khả năng mở rộng.

- **Trải nghiệm người dùng đơn giản:**  
  Cung cấp một nền tảng duy nhất thay thế việc tìm kiếm thủ công, giúp kiểm tra nhanh chóng và chính xác.


**Lợi ích**
* Phát hiện sớm các dấu hiệu lừa đảo từ nhiều nguồn dữ liệu (phone, domain, nội dung)  
* Cung cấp cảnh báo rủi ro rõ ràng, dễ hiểu  
* Tiết kiệm thời gian nhờ kiểm tra tự động  
* Tận dụng dữ liệu cộng đồng để tăng độ chính xác  
* Bảo vệ người dùng khi duyệt web  
* Nâng cao nhận thức và kỹ năng phòng tránh scam                                                                                                         
* Cải thiện bảo mật: Thông qua giám sát liên tục và cảnh báo thời gian thực.

## 3. Kiến trúc giải pháp

**Dịch vụ AWS sử dụng**

| Dịch vụ AWS | Chức năng chính |
| :--- | :--- |
| **AWS Elastic Beanstalk** | Triển khai và quản lý ứng dụng backend (Spring Boot) dưới dạng Docker container|
| **Amazon RDS for MySQL** | Cơ sở dữ liệu quan hệ, lưu trữ dữ liệu có cấu trúc (user, report, domain, history, ...) |
| **AWS Lambda** | Xử lý tác vụ serverless (AI chatbot, phân tích nội dung, xử lý bất đồng bộ) |
| **Amazon API Gateway** | Quản lý và expose API, xử lý request routing, validation và bảo mật endpoint |
| **Amazon DynamoDB** | NoSQL database, lưu trữ dữ liệu tạm/thời gian thực (OTP) |
| **AWS Cognito** | Xác thực và phân quyền người dùng (Authentication & Authorization, hỗ trợ SSO Google) |
| **Amazon S3** | Lưu trữ file (ảnh, tài liệu, static assets) |
| **Amazon CloudFront** | CDN phân phối nội dung (frontend, file từ S3), giảm độ trễ và tăng tốc độ tải |
| **Amazon Route 53** | Quản lý DNS, mapping domain tới hệ thống |
| **AWS WAF** | Bảo vệ API khỏi tấn công (rate limiting, chống bot, chống abuse) |
| **Amazon CloudWatch** | Giám sát hệ thống, log, metrics và alert |
| **Amazon SNS** | Gửi thông báo (email, SMS) cho hệ thống và người dùng |
| **Amazon Bedrock** | Cung cấp AI/LLM cho chatbot và phân tích nội dung lừa đảo |
| **AWS Secrets Manager** | Quản lý bảo mật thông tin nhạy cảm (API keys, credentials) |
| **AWS CodePipeline** | Tự động hóa CI/CD pipeline (build → test → deploy) |
| **AWS CodeBuild** | Service build và test code trong pipeline CI/CD |


**Thiết kế thành phần**

* **1. Lớp Thu Thập Dữ Liệu & Phát Hiện**
- Thu thập dữ liệu từ nhiều nguồn:
  - Input người dùng (phone, domain, email content)
  - Dữ liệu cộng đồng (report, feedback)
  - API bên ngoài (WHOIS domain)
- Backend (Spring Boot trên Elastic Beanstalk) đóng vai trò tiếp nhận và chuẩn hóa dữ liệu
- Trigger các tác vụ phân tích khi có dữ liệu mới

* **2. Lớp Xử Lý Sự Kiện**
- API Gateway tiếp nhận request từ client và định tuyến đến backend
- Backend xử lý validation, business logic và phân loại request
- Các tác vụ bất đồng bộ được đẩy sang AWS Lambda (AI analysis, content processing)
- Event-driven xử lý thông qua SNS (notification) hoặc internal service

* **3. Lớp Điều Phối & Xử Lý Nghiệp Vụ (Orchestration)**
- Backend (Spring Boot) đóng vai trò điều phối chính:
  - Gọi Lambda để phân tích nội dung, kiểm tra ngôn từ vi phạm tiêu chuẩn cộng đồng (AI)
  - Tổng hợp dữ liệu từ nhiều nguồn (RDS, DynamoDB, API ngoài)
  - Tính toán risk score và phân loại mức độ rủi ro
- Quản lý flow xử lý từ input → phân tích → trả kết quả

* **4. Lớp Xử Lý Dữ Liệu & Lưu Trữ**
- Amazon RDS (MySQL): Lưu dữ liệu chính (user, report, domain, history, ...)
- Amazon DynamoDB: Lưu dữ liệu nhanh (OTP)
- Amazon S3: Lưu trữ file (ảnh, tài liệu)
- Lambda hỗ trợ xử lý dữ liệu (ETL nhẹ, preprocessing cho AI)

* **5. Lớp AI & Phân Tích**
- Amazon Bedrock cung cấp mô hình AI:
  - Phân tích nội dung email, số điện thoại, tên miền
  - Giải thích mức độ rủi ro
  - Chatbot tư vấn phòng tránh lừa đảo
- Lambda đóng vai trò trung gian gọi Bedrock và xử lý kết quả

* **6. Lớp Trình Bày & Tương Tác Người Dùng**
- Frontend (React) được deploy qua S3 + CloudFront
- Giao tiếp với backend qua API Gateway
- Xác thực người dùng qua AWS Cognito (JWT)

* **7. Lớp Bảo Mật & Giám Sát**
- **Authentication:** AWS Cognito (SSO, JWT)
- **Security:** WAF, IAM, ACM (SSL), Route 53 (DNS)
- **Monitoring:** CloudWatch (logs, metrics), SNS (alert)
- **Secrets:** AWS Secrets Manager quản lý credentials


## 4. Triển khai kỹ thuật

* **Step 1:** Hạ tầng & Thiết lập nền tảng
- Thiết lập **VPC**:
  - Tạo **Public Subnet** (cho Load Balancer, Internet access)
  - Tạo **Private Subnet** (cho RDS)
  - Cấu hình **Internet Gateway** để cho phép truy cập internet
- Cấu hình **Security Group**:
  - Backend (Beanstalk) cho phép HTTP/HTTPS
  - RDS chỉ cho phép truy cập từ backend
- Triển khai:
  - Backend Spring Boot dưới dạng **Docker container trên AWS Elastic Beanstalk**
  - **Amazon RDS (MySQL)** cho dữ liệu chính
  - **DynamoDB** cho OTP 
  - **S3** cho lưu trữ file, ảnh
- Thiết lập **AWS Cognito** (Authentication + Google SSO)
- Xây dựng API cơ bản (Auth, Threat CRUD, Report)
- Xây dựng Admin Dashboard (backend APIs)

* **Step 2:** API & Web Portal MVP
- Xây dựng **Public Lookup APIs**:
  - Check phone, domain, content
- Triển khai frontend:
  - React.js deploy lên **S3 + CloudFront**
- Xây dựng:
  - Workflow báo cáo lừa đảo (user report)
  - Dashboard thống kê cơ bản
- Cấu hình **Route 53** để mapping domain

* **Step 3:** Xử lý dữ liệu & Logic cốt lõi
- Tích hợp **WHOIS API** để kiểm tra domain
- Xây dựng hệ thống:
  - Risk scoring (rule-based + cộng đồng)
  - Phân loại mức độ rủi ro
- Tích hợp **AWS Lambda + Bedrock**:
  - Phân tích nội dung email
  - Chatbot tư vấn
- Xử lý bất đồng bộ (async processing)

* **Step 4:** Hoàn thiện hệ thống & Frontend nâng cao
- Hoàn thiện UI/UX:
  - Dashboard chi tiết
  - Lịch sử kiểm tra
- Tích hợp:
  - Browser extension (cảnh báo website)
- Tối ưu API response & trải nghiệm người dùng

* **Step 5:** Kiểm thử, Bảo mật & Tối ưu
- Kiểm thử:
  - Unit test, integration test
  - Load testing
- Bảo mật:
  - AWS WAF chống abuse API
  - IAM Roles phân quyền
  - AWS ACM (SSL/TLS)
- Tối ưu:
  - Auto Scaling (Beanstalk)
  - CloudFront caching
  - Query optimization (RDS)

**Yêu cầu Kỹ thuật**

* **Frontend & Bảng điều khiển:** Giao diện sử dụng framework Next.js, được lưu trữ tĩnh trên Amazon S3 và phân phối tốc độ cao toàn cầu qua mạng CDN CloudFront.
* **Backend & Xử lý Logic:** Logic cốt lõi được lập trình bằng Python 3.12 chạy trên nền tảng AWS Lambda và được định tuyến thông qua Amazon API Gateway.
* **Dữ liệu & Lưu trữ:** Dữ liệu hệ thống sử dụng Amazon DynamoDB để đảm bảo tốc độ truy vấn siêu tốc, kết hợp với Amazon S3 để lưu trữ an toàn các tệp tin bằng chứng.
* **Cơ sở hạ tầng (IaC):** Toàn bộ tài nguyên hạ tầng AWS được định nghĩa và quản lý tự động hoàn toàn bằng mã thông qua AWS Cloud Development Kit (CDK).
* **Bảo mật & Giám sát:** Hệ thống áp dụng xác thực đa lớp AWS Cognito, quản lý quyền hạn chặt chẽ qua IAM và được giám sát, ghi log liên tục bởi Amazon CloudWatch.

* **Frontend:** React.js, deploy qua Amazon S3 + CloudFront (CDN)
* **Backend & Xử lý Logic:** Java Spring Boot (Monolithic architecture), deploy bằng Docker trên AWS Elastic Beanstalk, xử lý AI & async bằng AWS Lambda + Bedrock
* **Dữ liệu & Lưu trữ:** Amazon RDS (MySQL) cho dữ liệu chính, Amazon DynamoDB cho OTP, Amazon S3 cho file, ảnh
* **Cơ sở hạ tầng:** VPC, Subnet, Security Group, Internet Gateway, Route 53, quản lý secrets bằng AWS Secrets Manager
* **Bảo mật & Giám sát:** AWS Cognito (Authentication, SSO Google), AWS IAM (phân quyền)
* **CI/CD:** AWS CodePipeline (pipeline tự động), AWS CodeBuild (build & test)

## 5. Lộ trình & Mốc triển khai

| Tuần | Giai đoạn | Hoạt động chính | Sản phẩm bàn giao |
| :--- | :--- | :--- | :--- |
| **1-2** | Nền tảng (MVP Core) | Thiết lập AWS (VPC, Subnet, Security Group), triển khai Beanstalk + RDS, cấu hình Cognito, xây dựng API cơ bản | Hoàn thành Auth, Threat API, Report, backend deploy thành công |
| **3-4** | API & Tự động hóa | Xây dựng API tra cứu (phone, domain, URL), tích hợp WHOIS, cấu hình thông báo (SNS), thiết lập CI/CD | API tra cứu hoạt động, Web portal MVP, pipeline CI/CD |
| **5-6** | AI & Nâng cao | Tích hợp Lambda + Bedrock, phân tích nội dung, chấm điểm rủi ro bằng AI, xử lý bất đồng bộ | Chatbot AI, phân tích nội dung, risk scoring nâng cao |
| **7-8** | Dashboard & Tích hợp | Xây dựng dashboard quản trị, API thống kê, hoàn thiện frontend, tích hợp extension | Dashboard quản trị, giao diện hoàn chỉnh |
| **9-10** | Tối ưu hệ thống | Tối ưu hiệu năng (Auto Scaling, CloudFront), tối ưu truy vấn database, cải thiện API | Hệ thống ổn định, hiệu năng được cải thiện |
| **11** | Bảo mật | Cấu hình WAF, IAM, SSL (ACM), logging với CloudWatch, kiểm tra bảo mật | Hệ thống bảo mật hoàn chỉnh |
| **12** | Kiểm thử | Unit test, integration test, load test, kiểm thử end-to-end, sửa lỗi | Báo cáo kiểm thử, hệ thống ổn định |
| **13** | Bàn giao | Viết tài liệu (API, kiến trúc), demo hệ thống, chuẩn hóa GitHub | Tài liệu hoàn chỉnh, demo, bàn giao dự án |

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