---
title: "Tuần 1 Worklog"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Mục tiêu tuần 1

- Gặp gỡ và làm quen với đội ngũ tại **First Cloud Journey (FCJ)**.
- Nắm bắt kiến thức cơ bản về các dịch vụ AWS thiết yếu, **AWS Management Console** và **AWS CLI**.
---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Làm quen với đội ngũ FCJ. Nắm rõ nội quy và quy định của đơn vị thực tập. | 01/05/2026 | 01/05/2026 | — |
| 2 | Thiết lập ban đầu: truy cập AWS qua Console và cấu hình CLI. | 01/06/2026 | 01/06/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Đăng ký tài khoản AWS Free Tier. Tìm hiểu Console và CLI. Thực hành: tạo tài khoản, cài đặt & cấu hình CLI, chạy các lệnh cơ bản. | 01/07/2026 | 01/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Kiến thức cơ bản về EC2: loại instance, AMI, EBS. Các phương thức SSH. Tổng quan Elastic IP. | 01/08/2026 | 01/08/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Thực hành: khởi chạy EC2 instance, kết nối SSH và gắn EBS volume. | 01/09/2026 | 01/09/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---
### Thành quả Tuần 1

##### Sơ đồ kiến trúc AWS

Học cách vẽ sơ đồ kiến trúc đám mây bằng **draw.io**, sử dụng thư viện **AWS Architecture Icons** chính thức.


##### Tạo tài khoản AWS Free Tier

Hoàn thành toàn bộ quá trình đăng ký và thiết lập tài khoản AWS Free Tier:

- Phân biệt rõ vai trò của **Root Account** so với **IAM User** và lý do không nên dùng Root cho công việc hàng ngày.
- Tạo **IAM Group** với quyền admin, sau đó thêm **IAM User** mới vào group đó.
- Phân biệt **Access Key / Secret Key** (dùng cho CLI/API) và **Console Password** (dùng để đăng nhập qua trình duyệt).


##### AWS Management Console

Dành thời gian thao tác trực tiếp trên giao diện web của AWS:

- Đăng nhập và điều hướng qua các dashboard dịch vụ khác nhau.
- Tạo và chỉnh sửa **User Group**, **User** và các **Permission Policy** trong IAM.
- Nhận ra tầm quan trọng của việc bảo mật mật khẩu cho IAM user ngay từ đầu.


##### Cài đặt và cấu hình AWS CLI v2

Cài đặt AWS CLI v2 và kết nối với tài khoản AWS:

- Nhập **Access Key**, **Secret Key** và chọn **Region** mặc định khi cấu hình.
- Chạy lệnh sau để kiểm tra CLI đã kết nối thành công:

```bash
aws sts get-caller-identity
```


##### Các lệnh CLI cơ bản

Thực hành một loạt lệnh để quen với luồng làm việc qua CLI:

```bash
# Xác nhận thông tin tài khoản
aws sts get-caller-identity

# Lấy danh sách các AWS Region hiện có
aws ec2 describe-regions

# Xem thông tin EC2 instance hiện tại
aws ec2 describe-instances

# Tạo key pair mới
aws ec2 create-key-pair --key-name MyKeyPair

# Tải file lên S3 bucket
aws s3 cp <file-local> s3://<tên-bucket>/
```

Đến cuối tuần, đã tự tin chuyển đổi linh hoạt giữa **Console** và **CLI** tùy theo yêu cầu công việc.


##### Kiến thức tìm hiểu thêm

- Đăng ký tên miền tùy chỉnh qua **Route 53**.
- Tổng quan về **CloudFront CDN**, **AWS WAF** và các công cụ bảo mật biên khác.
- Xem xét chi phí và mức sử dụng qua **AWS Billing Dashboard**.
- So sánh các **AWS Support Plans** và nội dung từng gói hỗ trợ.
