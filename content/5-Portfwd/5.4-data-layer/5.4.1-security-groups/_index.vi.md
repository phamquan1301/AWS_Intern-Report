---
title: "Thiết lập Amazon S3 Bucket"
weight: 1
chapter: false
pre: " <b> 4.4.1. </b> "
---

### Danh sách các Bucket cần tạo

Bạn sẽ cần tạo 3 bucket với định dạng tên sau (thay thế `ACCOUNT_ID` và `REGION` bằng thông tin thực tế của bạn):

* `rag-data-ACCOUNT_ID-REGION`: Lưu trữ dữ liệu cho model.
* `user-report-ACCOUNT_ID-REGION`: Lưu trữ hình ảnh báo cáo từ người dùng.
* `chatbot-image-ACCOUNT_ID-REGION`: Lưu trữ hình ảnh người dùng gửi cho chatbot.

---

### Hướng dẫn tạo Bucket

**1. Mở Amazon S3 Console**
* Truy cập trực tiếp qua liên kết: [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/)
* Hoặc điều hướng từ menu: **AWS Management Console** → **Services** → **S3**.

<img src="images/Picture5.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**2. Bắt đầu tạo**
* Nhấp vào nút **Create bucket**.

**3. General configuration (Cấu hình chung)**
* **Bucket name:** Nhập tên bucket (Ví dụ: `rag-data-123456789012-us-east-1`).
* **AWS Region:** Chọn khu vực đích của bạn (Ví dụ: *US East (N. Virginia) us-east-1*).

<img src="images/Picture6.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**4. Object Ownership**
* Giữ tùy chọn mặc định: **ACLs disabled (recommended)**.

**5. Block Public Access settings for this bucket**
* Tích chọn **Block all public access**.
* Đảm bảo tất cả 4 tùy chọn phụ bên dưới đều được đánh dấu (✓):
    * ✓ Block public access to buckets and objects granted through *new* access control lists (ACLs)
    * ✓ Block public access to buckets and objects granted through *any* access control lists (ACLs)
    * ✓ Block public access to buckets and objects granted through *new* public bucket or access point policies
    * ✓ Block public and cross-account access to buckets and objects through *any* public bucket or access point policies

**6. Bucket Versioning**
* Chọn **Enable** (Kích hoạt).

<img src="images/Picture7.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**7. Tags (Tùy chọn)**
* Thêm các thẻ đánh dấu nếu muốn (Ví dụ: `Key=Purpose`, `Value=IncidentResponse`).

**8. Default encryption (Mã hóa mặc định)**
* **Encryption type:** Chọn **Server-side encryption with Amazon S3 managed keys (SSE-S3)**.
* **Bucket Key:** Giữ tùy chọn mặc định (**Enabled**).

**9. Advanced settings**
* Giữ nguyên tất cả các cài đặt mặc định.

**10. Hoàn tất**
* Kéo xuống cuối trang và nhấp vào **Create bucket**.

<img src="images/Picture8.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**11. Xác minh**
* Bạn sẽ thấy thông báo tạo thành công và bucket mới sẽ xuất hiện trong danh sách S3 buckets của bạn.

> **Lưu ý quan trọng:** Hãy lặp lại các bước từ 2 đến 11 cho 2 bucket còn lại (`user-report-ACCOUNT_ID-REGION` và `chatbot-image-ACCOUNT_ID-REGION`).

<img src="images/Picture9.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">