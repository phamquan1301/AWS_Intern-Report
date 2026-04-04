---
title: "Tuần 4 Worklog"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu Tuần 4

- Nắm vững kiến thức về Amazon S3 và các dịch vụ lưu trữ liên quan.
- Hiểu rõ lưu trữ dạng object và phân biệt với block storage.
- Thành thạo các storage class, lifecycle policy, CORS và cơ chế versioning.
- Tìm hiểu Amazon Glacier, Snow Family, AWS Storage Gateway và AWS Backup.
- Nắm bắt khái niệm RTO/RPO và các chiến lược Disaster Recovery trên AWS.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Giới thiệu Amazon S3: kiến trúc object storage, nhân bản dữ liệu đa AZ, độ bền 99.999999999%, khả dụng 99.99%. | 01/26/2026 | 01/26/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Các khái niệm cốt lõi của S3: bucket, object, key, access point. Các storage class: Standard, IA, Intelligent-Tiering, One Zone, Glacier, Deep Archive. | 01/27/2026 | 01/27/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Cấu hình lifecycle policy để tự động chuyển storage class. Multipart upload và event trigger khi tạo/xóa object. | 01/28/2026 | 01/28/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | **Sự kiện**: AWS re:Invent Recap HCMC - Track 2: Analytics/ML/AI & Innovation| 01/29/2026 | 01/29/2026 | [AWS re:Invent](https://reinvent-recap-hcmc.splashthat.com/) |
| 5 | S3 Versioning, S3 Endpoint, tối ưu hiệu năng bằng prefix và partition. Tìm hiểu sâu Amazon Glacier: Expedited, Standard, Bulk. | 01/30/2026 | 01/30/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Snow Family (Snowball, Snowball Edge, Snowmobile). AWS Storage Gateway (File, Volume, Tape). AWS Backup, RTO/RPO và các DR strategy. | 01/31/2026 | 01/31/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 4

##### Amazon S3 — Khái niệm cơ bản

S3 là dịch vụ lưu trữ object được quản lý hoàn toàn, khác biệt đáng kể so với block storage:

- Object không thể chỉnh sửa từng phần — mọi thay đổi đều yêu cầu tải lại toàn bộ object.
- Dữ liệu được tự động nhân bản qua **3 Availability Zone** trong cùng region.
- Hỗ trợ event trigger, multipart upload cho file lớn, versioning và cấu hình CORS.

##### Các Storage Class của S3

| Storage Class | Phù hợp với |
|---|---|
| S3 Standard | Dữ liệu truy cập thường xuyên |
| S3 Standard-IA / One Zone-IA | Truy cập không thường xuyên, tiết kiệm chi phí |
| S3 Intelligent-Tiering | Dữ liệu có tần suất truy cập không dự đoán được |
| S3 Glacier / Deep Archive | Lưu trữ dài hạn, chấp nhận độ trễ lấy dữ liệu |

##### Lifecycle Policy

- Tự động chuyển object sang storage class khác sau một số ngày được định nghĩa sẵn.
- Lên lịch xóa object tự động sau khi hết thời gian lưu giữ.

##### CORS và Cơ chế phân quyền

- **CORS** (Cross-Origin Resource Sharing) cho phép ứng dụng web truy cập tài nguyên S3 từ domain khác.
- **S3 ACL**: phân quyền cơ bản ở cấp bucket hoặc object.
- **Bucket Policy / IAM Policy**: chính sách JSON chi tiết để kiểm soát truy cập linh hoạt.

##### Versioning

- Khi bật versioning, mỗi lần ghi đè hoặc xóa sẽ tạo phiên bản mới thay vì xóa vĩnh viễn.
- Dễ dàng khôi phục dữ liệu bị xóa nhầm hoặc ghi đè không mong muốn.

##### Tối ưu hiệu năng S3

- Sử dụng tiền tố (prefix) ngẫu nhiên giúp phân tán request qua nhiều partition, tránh nghẽn cổ chai.
- Hiểu cơ chế hash partition của S3 giúp thiết kế key naming hiệu quả hơn.

##### Amazon Glacier

Lưu trữ lâu dài với chi phí cực thấp. Ba mức độ lấy dữ liệu:

- **Expedited**: 1–5 phút
- **Standard**: 3–5 giờ
- **Bulk**: 5–12 giờ

##### Snow Family

Thiết bị phần cứng chuyên dụng để di chuyển dữ liệu quy mô lớn (TB đến EB) từ on-premises lên AWS:

- **Snowball / Snowball Edge**: thiết bị truyền dữ liệu bền vững, tùy chọn có khả năng tính toán.
- **Snowmobile**: giải pháp cấp xe tải cho di chuyển dữ liệu ở quy mô exabyte.

Dữ liệu được mã hóa, nén và nhập trực tiếp vào S3 hoặc Glacier sau khi chuyển.

##### AWS Storage Gateway

Kết nối hạ tầng on-premises với cloud storage. Ba loại gateway:

- **File Gateway** (NFS/SMB) — lưu file trực tiếp vào S3.
- **Volume Gateway** (iSCSI) — block storage với hỗ trợ snapshot EBS.
- **Tape Gateway** (VTL) — thay thế thư viện băng vật lý, dùng S3 và Glacier làm backend.

##### AWS Backup và Disaster Recovery

- **RTO** (Recovery Time Objective): thời gian tối đa chấp nhận được để khôi phục hoạt động sau sự cố.
- **RPO** (Recovery Point Objective): lượng dữ liệu tối đa có thể mất đi, tính theo thời gian.

Bốn DR strategy theo thứ tự tăng dần về độ phức tạp và chi phí:

1. **Backup & Restore** — chi phí thấp nhất, thời gian khôi phục dài nhất.
2. **Pilot Light** — hạ tầng tối thiểu luôn sẵn sàng trên AWS.
3. **Warm Standby** — môi trường thu nhỏ chạy liên tục.
4. **Multi-Site Active-Active** — toàn bộ capacity ở nhiều region, RTO/RPO gần bằng 0.

AWS Backup hỗ trợ lập lịch và quản lý tập trung cho EBS, EC2, RDS, DynamoDB, EFS và Storage Gateway.

##### Sự kiện: AWS re:Invent Recap HCMC — Track 2: Analytics/ML/AI & Innovation

Tham dự cả ngày với các phiên tập trung vào AI/ML và đổi mới dữ liệu, bao gồm Vector Database trên S3, Natural Query Language trong OpenSearch và các khả năng mới nhất của Nova & Bedrock cho ứng dụng GenAI. Phiên ấn tượng nhất được dẫn dắt bởi Prapti Gupta — phong cách trình bày đầy năng lượng của cô khiến nội dung trở nên dễ tiếp thu và truyền cảm hứng. Buổi chiều tập trung vào các chủ đề ứng dụng thực tế: dùng MCP để cấp cho AI agent quyền truy cập tài nguyên AWS, multimodal retrieval cho Bedrock Knowledge Bases, và các best practice về MLOps với SageMaker AI. Nhìn chung, track mang lại góc nhìn toàn diện về cách AWS thu hẹp khoảng cách giữa hạ tầng dữ liệu và các ứng dụng thông minh.
