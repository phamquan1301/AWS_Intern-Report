---
title: "Tuần 5 Worklog"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu Tuần 5

- Nắm rõ Mô hình Trách nhiệm Chia sẻ và ranh giới trách nhiệm bảo mật giữa AWS và khách hàng.
- Thành thạo cơ chế Quản lý Danh tính và Truy cập (IAM) — bao gồm user, group, role và policy.
- Tìm hiểu AWS Organizations, AWS IAM Identity Center và AWS KMS trong việc quản lý bảo mật ở quy mô doanh nghiệp.
- Biết cách phân quyền, áp dụng chiến lược mã hóa và triển khai kiểm toán bảo mật trong môi trường AWS.
- Làm quen với Amazon Cognito để xác thực người dùng ở tầng ứng dụng và AWS Security Hub để đánh giá liên tục trạng thái bảo mật.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Tìm hiểu Mô hình Trách nhiệm Chia sẻ: phân chia trách nhiệm bảo mật giữa AWS và khách hàng; phân loại dịch vụ theo mức độ quản lý — Infrastructure, Hybrid và Fully Managed. | 02/02/2026 | 02/02/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Nghiên cứu Root Account và các thực hành tốt nhất: tạo tài khoản IAM administrator chuyên dụng, bảo vệ thông tin đăng nhập root, đảm bảo gia hạn domain và email định kỳ. | 02/03/2026 | 02/03/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Tìm hiểu chuyên sâu IAM Users, Groups, Roles và Policies — cơ chế đánh giá quyền, trust policy và nguyên tắc Deny tường minh luôn được ưu tiên. | 02/04/2026 | 02/04/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Thực hành tạo IAM Role, assume role và sử dụng AWS STS (Security Token Service) để cấp phát thông tin xác thực tạm thời có thời hạn. | 02/05/2026 | 02/05/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Tìm hiểu Amazon Cognito (User Pool & Identity Pool) — luồng đăng ký và đăng nhập người dùng, xác thực dựa trên token, và cách người dùng đã xác thực truy cập tài nguyên AWS. | 02/06/2026 | 02/06/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Khảo sát AWS Organizations, IAM Identity Center, AWS KMS và AWS Security Hub — quản trị đa tài khoản, quản lý truy cập tập trung, vòng đời khóa mã hóa và kiểm toán tuân thủ tự động. | 02/07/2026 | 02/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 5

##### Mô hình Trách nhiệm Chia sẻ

AWS và khách hàng đều có nghĩa vụ bảo mật riêng biệt, tạo ra ranh giới rõ ràng giữa bảo vệ cơ sở hạ tầng và cấu hình tầng ứng dụng:

- **AWS** chịu trách nhiệm về bảo mật *của* đám mây — bao gồm phần cứng vật lý, hạ tầng mạng toàn cầu, lớp hypervisor và nội bộ của các dịch vụ managed.
- **Khách hàng** chịu trách nhiệm về bảo mật *trong* đám mây — bao gồm cấu hình dịch vụ, phân loại dữ liệu, lựa chọn mã hóa và quản lý danh tính.
- Mức độ trách nhiệm của khách hàng tỷ lệ thuận với quyền kiểm soát hạ tầng:
  - **Dịch vụ Infrastructure** (ví dụ: EC2, VPC, EBS) → khách hàng gánh trách nhiệm nặng nhất.
  - **Dịch vụ Managed** (ví dụ: RDS, EKS) → trách nhiệm được chia sẻ giữa hai bên.
  - **Dịch vụ Fully Managed** (ví dụ: S3, Lambda) → AWS đảm nhận phần lớn gánh nặng.

##### AWS Root Account

Tài khoản root có toàn quyền truy cập vào mọi dịch vụ và tài nguyên AWS, và phải được xem là thông tin xác thực dự phòng khẩn cấp:

- Thông tin đăng nhập root cần được lưu trữ an toàn và chỉ sử dụng khi thực sự cần thiết.
- **Thực hành tốt nhất:**
  - Tạo tài khoản IAM administrator chuyên dụng và sử dụng cho mọi thao tác hằng ngày.
  - Phân chia trách nhiệm quản lý root account cho hai quản trị viên khác nhau.
  - Chủ động theo dõi và gia hạn domain cùng địa chỉ email gắn với tài khoản root.

##### AWS Identity and Access Management (IAM)

IAM là dịch vụ kiểm soát truy cập nền tảng của AWS, xác định *ai* được làm *gì* trên *tài nguyên nào*:

- **Các Principal** được IAM hỗ trợ bao gồm: AWS Root user, IAM user, Federated user (qua SAML hoặc OAuth), IAM role, dịch vụ AWS và người dùng ẩn danh.
- **IAM User:**
  - Mặc định không có quyền nào và phải được cấp phép tường minh.
  - Có thể xác thực qua AWS Management Console hoặc lập trình bằng cặp Access Key / Secret Key.
  - Không phù hợp để quản lý ứng dụng, dịch vụ hay hệ điều hành trực tiếp.
- **IAM Group:**
  - Tập hợp các IAM user giúp đơn giản hóa việc quản lý quyền theo nhóm.
  - Không thể lồng group bên trong group khác.
- **IAM Policy (JSON):**
  - *Identity-based policy* được gắn trực tiếp vào principal (user, group, role).
  - *Resource-based policy* được gắn vào tài nguyên AWS cụ thể (ví dụ: S3 bucket).
  - Thứ tự đánh giá: **Deny tường minh luôn ghi đè mọi Allow**, bất kể các policy khác.
- **IAM Role:**
  - Không có thông tin xác thực cố định — quyền chỉ được cấp khi role được assume.
  - *Trust policy* xác định principal nào được phép assume role.
  - Thường dùng để ủy quyền cross-account hoặc cấp quyền cho dịch vụ AWS (như EC2) gọi các API AWS khác.
  - Tích hợp với AWS STS để cấp phát thông tin xác thực tạm thời, tự động hết hạn.

##### Amazon Cognito

Cognito cung cấp lớp quản lý danh tính được quản lý hoàn toàn cho ứng dụng web và di động, loại bỏ sự phức tạp khi tự xây dựng hệ thống xác thực:

- **User Pool** — xử lý đăng ký, đăng nhập, xác thực đa yếu tố và quản lý phiên người dùng.
- **Identity Pool** — ánh xạ danh tính đã xác thực (hoặc khách) thành thông tin xác thực IAM tạm thời để truy cập trực tiếp các dịch vụ AWS.
- Hỗ trợ đăng nhập bằng username/password hoặc đăng nhập liên kết qua bên thứ ba như Google, Facebook và Amazon.
- Mô hình phổ biến kết hợp cả hai: người dùng xác thực qua User Pool, sau đó trao đổi token lấy thông tin xác thực AWS qua Identity Pool.

##### AWS Organizations

AWS Organizations cho phép quản trị toàn bộ danh mục tài khoản AWS từ một tài khoản quản lý duy nhất:

- Các tài khoản có thể được nhóm vào **Organizational Units (OU)** để phản ánh đơn vị kinh doanh, môi trường hoặc phạm vi tuân thủ.
- **Consolidated Billing** tổng hợp mức sử dụng trên tất cả tài khoản, giúp hưởng lợi từ mức giá theo khối lượng.
- **Service Control Policy (SCP)** xác định giới hạn quyền tối đa cho OU hoặc tài khoản — SCP đóng vai trò rào chắn, không phải cơ chế cấp quyền: SCP không thể cấp quyền, nhưng Deny từ SCP có thể chặn cả quản trị viên.

##### AWS IAM Identity Center (trước đây là AWS SSO)

IAM Identity Center đơn giản hóa việc quản lý truy cập trên tất cả tài khoản AWS trong tổ chức và các ứng dụng doanh nghiệp bên ngoài:

- Hỗ trợ nhiều nguồn danh tính: AWS Managed Microsoft AD, Active Directory on-premises (qua trust relationship hoặc AD Connector), hoặc nhà cung cấp danh tính bên ngoài.
- **Permission Set** định nghĩa tập hợp các policy; khi được gán cho user hoặc group trên một tài khoản cụ thể, hệ thống tự động tạo IAM role tương ứng.

##### AWS Key Management Service (KMS)

KMS là dịch vụ managed để tạo, lưu trữ và kiểm soát vòng đời của các khóa mật mã:

- Cung cấp **Encryption at Rest** đáp ứng tiêu chuẩn tuân thủ FIPS 140-2 Level 2.
- **Customer Managed Key (CMK)** là tài nguyên chính; CMK được dùng để tạo **Data Key** thực hiện mã hóa dữ liệu thực tế trong các dịch vụ khác.
- KMS không bao giờ tiết lộ CMK dưới dạng plaintext ra bên ngoài biên giới phần cứng bảo mật — bản thân AWS cũng không thể xuất hay truy cập vật liệu khóa.

##### AWS Security Hub

Security Hub cung cấp đánh giá trạng thái bảo mật liên tục và tự động trên một tài khoản AWS hoặc toàn bộ tổ chức:

- Đánh giá cấu hình tài nguyên theo **AWS Foundational Security Best Practices** và các tiêu chuẩn ngành (ví dụ: CIS, PCI DSS).
- Tạo ra **điểm bảo mật** chuẩn hóa, ưu tiên hiển thị các phát hiện rủi ro cao nhất trước.
- Đóng vai trò là điểm tổng hợp trung tâm cho các phát hiện bảo mật từ các dịch vụ như Amazon GuardDuty, AWS Config, Amazon Inspector và nhiều dịch vụ khác.

##### Thực hành

- Điều hướng trong AWS Management Console để xem các instance EC2 đang chạy và hiểu rõ trạng thái dịch vụ.
- Tạo và quản lý EC2 key pair phục vụ truy cập SSH bảo mật.
- Kiểm tra cấu hình các dịch vụ AWS đang hoạt động.
- Hình thành khả năng phối hợp giữa AWS Management Console và AWS CLI để quản lý tài nguyên song song, củng cố cả quy trình thao tác đồ họa lẫn lập trình.
