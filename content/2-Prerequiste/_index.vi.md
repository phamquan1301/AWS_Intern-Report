---
title : "Bản đề xuất"
date: 2026-01-10
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

## 1. Tóm tắt điều hành
**AnTiScaQ** là hệ thống kiểm tra nguy cơ lừa đảo trực tuyến, hỗ trợ người dùng phát hiện sớm các dấu hiệu lừa đảo thông qua nhiều loại dữ liệu đầu vào như **số điện thoại, tên miền (domain), và nội dung email** bằng cách xác minh độ uy tín và cảnh báo theo mức độ rủi ro. Hệ thống không đưa ra kết luận pháp lý mà chỉ đóng vai trò cảnh báo, giải thích và hướng dẫn phòng tránh. Hệ thống hoạt động dựa trên việc thu thập dữ liệu, tiếp nhận báo cáo từ cộng đồng và chấm điểm rủi ro.

## 2. Tuyên bố vấn đề

#### Vấn đề hiện tại
* Lừa đảo trực tuyến đang gia tăng nhanh chóng và ngày càng tinh vi, gây ra thiệt hại lớn cho người dùng và doanh nghiệp.
* Thiếu vắng một hệ thống đáng tin cậy, khiến người dùng dễ đưa ra quyết định sai lầm.
* Người dùng thường phải tìm kiếm thủ công qua các nền tảng trình duyệt (Google, Microsoft Edge, ...) hoặc dùng các cơ sở dữ liệu phân mảnh để kiểm tra số điện thoại/đường link đáng ngờ, gây tốn thời gian và rủi ro.
* Các công cụ hiện có thường thiếu **khả năng phân tích ngữ cảnh nội dung**.
  - Thiếu hụt dữ liệu **từ cộng đồng người dùng**.
  - Không cung cấp **giải thích rõ ràng về mức độ rủi ro**.
* Chi phí quá đắt đỏ để mua API từ các nền tảng Threat Intel doanh nghiệp (như Recorded Future, VirusTotal).
* Báo cáo thường bị thiếu sót, **không theo thời gian thực (not real-time)** khi các chiến dịch phishing mới bùng nổ.

#### Giải pháp đề xuất

Để giải quyết các vấn đề về lừa đảo trực tuyến ngày càng gia tăng và sự thiếu hụt một hệ thống kiểm chứng đáng tin cậy, AnTiScaQ đề xuất một nền tảng dựa trên kiến trúc **Hybrid Cloud-Native** với các định hướng chính:

- **Tự động hóa phát hiện rủi ro:** Phân tích đa nguồn (số điện thoại, domain, URL, nội dung email) kết hợp rule-based và AI để phát hiện sớm các dấu hiệu lừa đảo.

- **Phân tích ngữ cảnh bằng AI:** Sử dụng Amazon Bedrock để "hiểu" nội dung và cung cấp giải thích rõ ràng về mức độ rủi ro, giúp người dùng dễ dàng ra quyết định.

- **Khai thác sức mạnh cộng đồng:** Xây dựng hệ thống báo cáo và chấm điểm uy tín dựa trên đóng góp của người dùng, giúp dữ liệu luôn được cập nhật và phản ánh đúng thực tế.

- **Tối ưu chi phí & Khả năng mở rộng:** Áp dụng kiến trúc Hybrid:
  - Elastic Beanstalk (Docker) cho backend chính 
  - Lambda cho AI & linh hoạt xử lý 
  → Giảm thiểu chi phí so với các hệ thống enterprise truyền thống.

- **Tích hợp hệ sinh thái AWS:** Sử dụng Cognito, RDS, DynamoDB, S3, CloudFront, WAF… để đảm bảo bảo mật, hiệu năng và khả năng tự động mở rộng.

- **Trải nghiệm người dùng đơn giản:** Cung cấp một nền tảng duy nhất thay thế việc tìm kiếm thủ công, giúp kiểm tra nhanh chóng và chính xác.

**Lợi ích**
- Phát hiện sớm các dấu hiệu lừa đảo từ nhiều nguồn dữ liệu (điện thoại, domain, nội dung).
- Cung cấp cảnh báo rủi ro rõ ràng, dễ hiểu.
- Tiết kiệm thời gian nhờ quy trình kiểm tra tự động.
- Tận dụng dữ liệu cộng đồng để gia tăng độ chính xác.
- Bảo vệ người dùng an toàn khi duyệt web.
- Nâng cao nhận thức và kỹ năng phòng tránh scam.
- Cải thiện bảo mật: Thông qua giám sát liên tục và cảnh báo thời gian thực.

## 3. Kiến trúc giải pháp

![ConnectPrivate](/AWS_Intern-Report/images/aws_architecture.drawio.png?width=60rem) 

**Dịch vụ AWS sử dụng**

| Dịch vụ AWS | Chức năng chính |
| :--- | :--- |
| **AWS Elastic Beanstalk** | Triển khai và quản lý ứng dụng backend (Spring Boot) dưới dạng Docker container. |
| **Amazon RDS for MySQL** | Cơ sở dữ liệu quan hệ, lưu trữ dữ liệu có cấu trúc (user, report, domain, history, ...). |
| **AWS Lambda** | Xử lý các tác vụ serverless (AI chatbot, phân tích nội dung, xử lý bất đồng bộ). |
| **Amazon API Gateway** | Quản lý và expose API, xử lý định tuyến request, validation và bảo mật endpoint. |
| **Amazon DynamoDB** | NoSQL database, lưu trữ dữ liệu tạm/thời gian thực (OTP). |
| **AWS Cognito** | Xác thực và phân quyền người dùng (Authentication & Authorization, hỗ trợ SSO Google). |
| **Amazon S3** | Lưu trữ file (ảnh, tài liệu, static assets). |
| **Amazon CloudFront** | CDN phân phối nội dung (frontend, file từ S3), giảm độ trễ và tăng tốc độ tải trang. |
| **Amazon Route 53** | Quản lý DNS, mapping domain tới hệ thống. |
| **AWS WAF** | Bảo vệ API khỏi các đợt tấn công (rate limiting, chống bot, chống abuse). |
| **Amazon CloudWatch** | Giám sát hệ thống, lưu log, metrics và gửi cảnh báo (alert). |
| **Amazon SNS** | Gửi thông báo (email, SMS) cho hệ thống và người dùng. |
| **Amazon Bedrock** | Cung cấp AI/LLM cho chatbot và phân tích nội dung lừa đảo. |
| **AWS Secrets Manager** | Quản lý bảo mật thông tin nhạy cảm (API keys, credentials). |
| **AWS CodePipeline** | Tự động hóa CI/CD pipeline (build → test → deploy). |
| **AWS CodeBuild** | Service chịu trách nhiệm build và test code trong CI/CD pipeline. |

#### Thiết kế thành phần

**1. Lớp Thu Thập Dữ Liệu & Phát Hiện (Data Collection & Detection Layer)**
* **Nguồn dữ liệu:** Thu thập dữ liệu từ nhiều nguồn bao gồm input người dùng (phone, domain, email content), dữ liệu cộng đồng (reports, feedback), và API bên ngoài (WHOIS domain).
* **Tiếp nhận:** Backend (Spring Boot trên Elastic Beanstalk) tiếp nhận và chuẩn hóa dữ liệu, tự động kích hoạt (trigger) các tác vụ phân tích khi có dữ liệu mới.

**2. Lớp Xử Lý Sự Kiện (Event Processing Layer)**
* **Định tuyến & Logic:** API Gateway tiếp nhận request từ client và định tuyến đến backend để xử lý validation, business logic, và phân loại request.
* **Bất đồng bộ & Sự kiện:** Các tác vụ bất đồng bộ (phân tích AI, xử lý nội dung) được đẩy sang AWS Lambda, kết hợp xử lý hướng sự kiện (event-driven) thông qua SNS (thông báo) hoặc các service nội bộ.

**3. Lớp Điều Phối & Xử Lý Nghiệp Vụ (Orchestration & Business Logic Layer)**
* **Điều phối cốt lõi:** Backend đóng vai trò điều phối chính, quản lý mượt mà luồng xử lý từ input → phân tích → trả kết quả.
* **Tác vụ xử lý:** Tổng hợp dữ liệu từ nhiều nguồn (RDS, DynamoDB, API ngoài), gọi Lambda để phân tích nội dung/vi phạm thông qua AI, tính toán risk score và phân loại mức độ rủi ro.

**4. Lớp Xử Lý Dữ Liệu & Lưu Trữ (Data Processing & Storage Layer)**
* **Lưu trữ dữ liệu:** Sử dụng Amazon RDS (MySQL) cho dữ liệu chính (user, report, domain, history), Amazon DynamoDB cho dữ liệu truy xuất nhanh (OTP), và Amazon S3 để lưu trữ file (ảnh, tài liệu).
* **Hỗ trợ xử lý:** AWS Lambda hỗ trợ xử lý dữ liệu phụ trợ, bao gồm các tác vụ ETL nhẹ và tiền xử lý (preprocessing) cho AI.

**5. Lớp AI & Phân Tích (AI & Analysis Layer)**
* **Năng lực AI:** Amazon Bedrock cung cấp các mô hình AI mạnh mẽ để phân tích nội dung email, số điện thoại, tên miền, đồng thời giải thích mức độ rủi ro và vận hành chatbot tư vấn phòng tránh lừa đảo.
* **Tích hợp:** AWS Lambda đóng vai trò trung gian bảo mật để gọi Bedrock và xử lý các kết quả phân tích trả về.

**6. Lớp Trình Bày & Tương Tác Người Dùng (Presentation & User Interaction Layer)**
* **Giao diện:** Frontend (React) được deploy qua S3 + CloudFront, giao tiếp bảo mật với backend thông qua API Gateway.
* **Quản lý truy cập:** Xác thực người dùng và phiên đăng nhập được quản lý bằng AWS Cognito (JWT).

**7. Lớp Bảo Mật & Giám Sát (Security & Monitoring Layer)**
* **Bảo mật & Truy cập:** Triển khai Authentication qua AWS Cognito (SSO, JWT) và quản lý bảo mật tổng thể sử dụng AWS WAF, IAM, ACM (SSL), và Route 53 (DNS).
* **Vận hành:** Hệ thống giám sát được xử lý bởi Amazon CloudWatch (logs, metrics) và SNS (alerts), trong khi các thông tin danh tính (credentials) được mã hóa và quản lý bởi AWS Secrets Manager.

## 4. Lộ trình Triển khai Kỹ thuật

**Bước 1: Hạ tầng & Thiết lập nền tảng**
* **Mạng & Bảo mật:** Thiết lập VPC (Public Subnet cho Load Balancer/Internet, Private Subnet cho RDS), cấu hình Internet Gateway, và thiết lập Security Groups (Beanstalk: cho phép HTTP/HTTPS, RDS: chỉ cho phép truy cập từ backend).
* **Triển khai cốt lõi:** Deploy Spring Boot Backend dưới dạng Docker container trên AWS Elastic Beanstalk. Cung cấp Amazon RDS (MySQL) cho dữ liệu chính, DynamoDB cho OTP, và S3 cho lưu trữ file/ảnh.
* **Dịch vụ cơ sở:** Thiết lập AWS Cognito (Authentication + Google SSO), xây dựng các API cơ bản (Auth, Threat CRUD, Report), và xây dựng Admin Dashboard APIs.

**Bước 2: API & Web Portal MVP**
* **Dịch vụ công khai:** Xây dựng Public Lookup APIs (kiểm tra SĐT, domain, nội dung).
* **Frontend:** Triển khai ứng dụng React.js lên Amazon S3 + CloudFront.
* **Tính năng & Định tuyến:** Xây dựng luồng báo cáo lừa đảo (workflow), dashboard thống kê cơ bản, và cấu hình Route 53 để mapping domain.

**Bước 3: Xử lý dữ liệu & Logic cốt lõi**
* **Hệ thống Logic:** Tích hợp WHOIS API để kiểm tra domain. Xây dựng hệ thống chấm điểm rủi ro (rule-based + cộng đồng) và phân loại mức độ rủi ro.
* **AI & Bất đồng bộ:** Tích hợp AWS Lambda + Amazon Bedrock để xử lý phân tích nội dung email, Chatbot tư vấn, và xử lý bất đồng bộ (async).

**Bước 4: Hoàn thiện hệ thống & Frontend nâng cao**
* **Tinh chỉnh UI/UX:** Hoàn thiện giao diện Detailed Dashboard và Lịch sử kiểm tra.
* **Tích hợp:** Phát triển và tích hợp Browser extension (tiện ích trình duyệt) để cảnh báo website theo thời gian thực.
* **Nâng cấp:** Tối ưu hóa API response và trải nghiệm người dùng tổng thể.

**Bước 5: Kiểm thử, Bảo mật & Tối ưu**
* **Kiểm thử:** Thực hiện Unit tests, Integration tests, và Load testing.
* **Bảo mật:** Triển khai AWS WAF để ngăn chặn lạm dụng API, cấu hình IAM Roles để phân quyền chặt chẽ, và cung cấp AWS ACM cho chứng chỉ SSL/TLS.
* **Tối ưu hóa:** Bật Auto Scaling cho Elastic Beanstalk, cấu hình CloudFront caching, và tối ưu hóa truy vấn (Query optimization) trên RDS.

#### Yêu cầu Kỹ thuật

| Thành phần | Mô tả |
| :--- | :--- |
| **Frontend & Bảng điều khiển** | Giao diện sử dụng framework Next.js, được lưu trữ tĩnh trên Amazon S3 và phân phối tốc độ cao toàn cầu qua mạng CDN CloudFront. |
| **Backend & Xử lý Logic** | Logic cốt lõi được lập trình bằng Python 3.12 chạy trên nền tảng AWS Lambda và được định tuyến thông qua Amazon API Gateway. |
| **Dữ liệu & Lưu trữ** | Dữ liệu hệ thống sử dụng Amazon DynamoDB để đảm bảo tốc độ truy vấn siêu tốc, kết hợp với Amazon S3 để lưu trữ an toàn các tệp tin bằng chứng. |
| **Cơ sở hạ tầng (IaC)** | Toàn bộ tài nguyên hạ tầng AWS được định nghĩa và quản lý tự động hoàn toàn bằng mã thông qua AWS Cloud Development Kit (CDK). |
| **Bảo mật & Giám sát** | Hệ thống áp dụng xác thực đa lớp AWS Cognito, quản lý quyền hạn chặt chẽ qua IAM và được giám sát, ghi log liên tục bởi Amazon CloudWatch. |
| **CI/CD** | AWS CodePipeline (pipeline tự động), AWS CodeBuild (build & test). |

## 5. Lộ trình & Mốc triển khai

| Tuần | Giai đoạn | Hoạt động chính | Sản phẩm bàn giao |
| :--- | :--- | :--- | :--- |
| **1-2** | Nền tảng (MVP Core) | Thiết lập AWS (VPC, Subnet, Security Group), deploy Beanstalk + RDS, cấu hình Cognito, xây dựng API cơ bản. | Hoàn thành Auth, Threat API, Report; backend deploy thành công. |
| **3-4** | API & Tự động hóa | Xây dựng API tra cứu (phone, domain, URL), tích hợp WHOIS, cấu hình thông báo (SNS), setup CI/CD. | API tra cứu hoạt động, Web portal MVP, CI/CD pipeline. |
| **5-6** | AI & Nâng cao | Tích hợp Lambda + Bedrock, phân tích nội dung, chấm điểm rủi ro bằng AI, xử lý bất đồng bộ. | Chatbot AI, phân tích nội dung, risk scoring nâng cao. |
| **7-8** | Dashboard & Tích hợp | Xây dựng admin dashboard, API thống kê, hoàn thiện frontend, tích hợp extension. | Admin dashboard, giao diện hoàn chỉnh. |
| **9-10** | Tối ưu hệ thống | Tối ưu hiệu năng (Auto Scaling, CloudFront), tối ưu truy vấn database, cải thiện API. | Hệ thống ổn định, hiệu năng được cải thiện. |
| **11** | Bảo mật | Cấu hình WAF, IAM, SSL (ACM), logging với CloudWatch, kiểm tra bảo mật. | Hệ thống bảo mật hoàn chỉnh. |
| **12** | Kiểm thử | Unit test, integration test, load test, kiểm thử end-to-end, sửa lỗi. | Báo cáo kiểm thử, hệ thống ổn định. |
| **13** | Bàn giao | Viết tài liệu (API, kiến trúc), demo hệ thống, chuẩn hóa GitHub. | Tài liệu hoàn chỉnh, demo, bàn giao dự án. |

## 6. Ước tính ngân sách

**Chi phí AWS hàng tháng**
*Kiến trúc Serverless - Chi phí tối ưu*

| Dịch vụ | Cấu hình | Chi phí/tháng |
| :--- | :--- | :--- |
| **AWS Lambda** | 15K invocations, 512MB, 4000ms avg | $0 *(Free tier)* |
↳ Free tier: 1M requests + 400K GB-seconds/month
| **Amazon API Gateway** | 15K REST API requests | $0 |
| **Amazon DynamoDB** | On-demand, 5GB storage, 1M reads, 0.5M write | $0.5 |
| **Amazon S3 Vectors** | 2GB data, PUT/GET | $0.60 |
| **Amazon Bedrock** | Model: Claude Haiku 3, Input Token: 6GB, Output Token: 4GB | $6.5 |
| **Amazon CloudFront** | 10GB transfer, 200K requests | $1.00 |
| **Amazon Route 53** | 1 hosted zone | $0.90 |
| **Amazon CloudWatch**| Log và Auth cơ bản | $0 *(Free tier)* |
| **AWS Secrets Manager**| Quản lý key proxy | $0.4 |
| **AWS Elastic Beanstalk** | t3.micro | $11.68 |
| **Amazon Cognito** | 1000 MAU | $0 |
↳ Free tier: < 50K MAU
| **Amazon RDS for MySQL** | instance db.t4g.micro, gp3 | $21.01 |
| | **TỔNG AWS/THÁNG** | **~$42.59** |


## 7. Đánh giá rủi ro & Giảm thiểu

| Rủi ro | Ảnh hưởng | Giảm thiểu |
| :--- | :--- | :--- |
| **Quá tải Backend & RDS (Spike traffic)** | Cao | Cấu hình Auto Scaling cho Elastic Beanstalk dựa trên CPU/Network. Sử dụng Connection Pooling (như HikariCP) cho Spring Boot để tránh làm sập RDS. |
| **Chi phí Amazon Bedrock (AI) tăng đột biến** | Cao | Đặt Rate Limiting chặt chẽ tại API Gateway/WAF. Giới hạn `max_tokens` cho model. Lưu cache các kết quả phân tích nội dung đã trùng lặp để tránh gọi lại API AI nhiều lần. |
| **False Positives (Báo cáo sai từ cộng đồng)** | Cao | Áp dụng hệ thống tính điểm uy tín (Trust score) cho người dùng. Yêu cầu xác minh chéo từ nhiều nguồn (WHOIS, Rule-based) trước khi AI đưa ra cảnh báo chính thức. |
| **Scraper / WHOIS API bị block IP hoặc Rate Limit** | Trung bình | Sử dụng rotating proxy pools (thông tin proxy lưu an toàn trong AWS Secrets Manager) và thiết lập cơ chế Retry (Exponential Backoff) khi gọi API bên ngoài. |
| **Lambda Timeout khi xử lý AI/Async** | Thấp | Tách rời các tác vụ gọi Bedrock nặng khỏi luồng API đồng bộ. Giao tiếp qua Amazon SNS để Lambda xử lý ngầm (async) và trả kết quả sau. Tối ưu bundle size của Lambda. |


#### Best practices tối ưu chi phí & Hiệu năng

* **Elastic Beanstalk & RDS:** Với instance `t3.micro`, bạn cần tối ưu hóa các câu truy vấn MySQL (đánh index chuẩn, tránh lỗi N+1). Giới hạn số lượng connection từ Spring Boot vào RDS để tối ưu bộ nhớ.
* **Lambda & Bedrock:** Giữ dung lượng file deploy (bundle size) nhỏ. Khởi tạo các kết nối (HTTP clients, DB connections) ở bên ngoài *handler function* để tận dụng cơ chế tái sử dụng, tránh lỗi *cold start*. Theo dõi sát sao chi phí Bedrock qua AWS Budgets.
* **DynamoDB:** Vì bạn dùng DynamoDB chủ yếu để lưu OTP, hãy cấu hình tính năng **TTL (Time to Live)** để hệ thống tự động xóa các mã OTP hết hạn, giúp tiết kiệm dung lượng lưu trữ tối đa. Dùng On-demand billing là hợp lý cho MVP.
* **S3 & CloudFront:** Thiết lập *Lifecycle policies* trên S3 để tự động chuyển các file ảnh bằng chứng cũ hoặc file log sang các lớp lưu trữ rẻ hơn (Glacier). Bật caching trên CloudFront để giảm số lượng request trực tiếp vào S3.
* **API Gateway & WAF:** Bật Response caching (30-60s) cho các API public lookup (tra cứu SĐT, Domain) để giảm tải cho backend. Bắt buộc thiết lập Throttling và WAF Rate-based rules để chặn bot spam hoặc các đợt DDoS làm kiệt quệ tài nguyên.

## 8. Kết quả kỳ vọng

#### Cải tiến kỹ thuật
* **Tự động hóa & Tốc độ cao:** Thay thế hoàn toàn quy trình tra cứu thủ công rườm rà. Hệ thống phát hiện, phân tích và phản hồi kết quả gần như tức thì (real-time).
* **Đột phá với AI:** Ứng dụng thành công Amazon Bedrock để "hiểu" ngữ cảnh nội dung lừa đảo phức tạp, vượt trội hoàn toàn so với các bộ lọc từ khóa (rule-based) truyền thống.
* **Hạ tầng linh hoạt & Bền bỉ:** Kiến trúc Hybrid Cloud-Native (kết hợp Beanstalk và Lambda) giúp hệ thống tự động co giãn (Auto Scaling) mượt mà khi có lượng truy cập đột biến mà không bị sập hay nghẽn cổ chai.
* **Độ chính xác cao:** Giảm thiểu tối đa cảnh báo sai (False Positives) nhờ cơ chế đối chiếu chéo đa nguồn và hệ thống chấm điểm uy tín cộng đồng.

#### Giá trị kinh doanh
* **Tối ưu chi phí đột phá:** Vận hành toàn bộ hệ thống với chi phí siêu rẻ (chưa tới $30/tháng), tạo lợi thế cạnh tranh tuyệt đối so với việc phải mua API đắt đỏ từ các nền tảng Threat Intel doanh nghiệp.
* **Bảo vệ người dùng thiết thực:** Ngăn chặn kịp thời các thiệt hại về tài chính và rò rỉ dữ liệu cá nhân thông qua các cảnh báo rõ ràng, dễ hiểu.
* **Tiết kiệm thời gian:** Tiết kiệm hàng ngàn giờ tra cứu cho cộng đồng nhờ nền tảng kiểm tra "Tất cả trong một" (All-in-one).
* **Xây dựng hệ sinh thái tự duy trì:** Thu hút và hình thành một cộng đồng người dùng tích cực báo cáo, giúp dữ liệu tự động được làm giàu liên tục với chi phí thu thập bằng 0.

#### Tầm nhìn dài hạn 
* **Hệ sinh thái bảo mật toàn diện:** Mở rộng độ phủ sóng từ Web Portal sang tiện ích trình duyệt (Browser Extension) và AI Chatbot, bảo vệ người dùng ở mọi điểm chạm trên không gian mạng.
* **Kho dữ liệu Threat Intelligence độc quyền:** Sở hữu cơ sở dữ liệu nhận diện lừa đảo đặc thù và cập nhật nhanh nhất, mở ra cơ hội thương mại hóa (B2B) bằng cách cung cấp API ngược lại cho các tổ chức tài chính, ngân hàng.
* **Trí tuệ nhân tạo dự đoán:** Nâng cấp mô hình AI từ "phát hiện" (Detection) sang "dự báo" (Prediction), có khả năng nhận diện các chiến dịch phishing tinh vi ngay từ khi chúng mới bắt đầu hình thành.

## 9. Kết luận

Hệ thống **AnTiScaQ** với kiến trúc **Hybrid Cloud-Native** cung cấp:

* **Chi phí siêu tối ưu:** Vận hành cực kỳ tiết kiệm, chỉ khoảng **~$27.43/tháng** cho toàn bộ hệ thống.
* **No upfront cost:** Tận dụng tối đa AWS Free Tier và nguồn lực in-house, hoàn toàn không tốn chi phí đầu tư phần cứng ban đầu.
* **ROI khủng:** Loại bỏ hoàn toàn sự phụ thuộc và chi phí đắt đỏ khi phải mua API từ các nền tảng Threat Intel doanh nghiệp.
* **Scalable:** Khả năng Auto Scaling linh hoạt (Beanstalk) kết hợp Serverless (Lambda), xử lý mượt mà khi traffic bùng nổ do các chiến dịch phishing.
* **Tối ưu vận hành:** Tận dụng sức mạnh của các Managed Services và Serverless từ hệ sinh thái AWS, giảm thiểu tối đa gánh nặng quản trị server.
* **Fast development:** Lộ trình triển khai thần tốc, chỉ mất khoảng **13 tuần (~3 tháng)** từ lúc xây dựng đến khi hoàn thiện, kiểm thử và bàn giao.

Đây là giải pháp **lý tưởng, đột phá và thực tiễn** để xây dựng một nền tảng cảnh báo an toàn thông tin tự động, khai thác được sức mạnh cộng đồng và AI mà không đòi hỏi ngân sách hạ tầng khổng lồ.