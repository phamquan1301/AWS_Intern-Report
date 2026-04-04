---
title: "Tuần 3 Worklog"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Mục tiêu Tuần 3

- Hiểu sâu về Amazon EC2 và các dịch vụ lưu trữ liên quan — EBS, EFS và FSx.
- Nắm rõ cơ chế hoạt động, cách cấu hình và so sánh các tùy chọn định giá của EC2.
- Thực hành khởi chạy EC2 instance, quản lý EBS volume và làm việc với snapshot.
- Áp dụng Auto Scaling, tìm hiểu các pricing option và làm quen với Amazon Lightsail.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Tổng quan EC2: khái niệm, khả năng co giãn, so sánh với máy chủ vật lý. Các loại Instance Type — thông số CPU, memory, network, storage. | 01/19/2026 | 01/19/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | AMI (Amazon Machine Image) và quy trình khởi tạo EC2. Các loại Hypervisor: KVM, HVM, PV và cách lựa chọn phù hợp. | 01/20/2026 | 01/20/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Cơ chế Key Pair và mã hóa thông tin đăng nhập cho Linux và Windows. Tìm hiểu EBS: các loại HDD/SSD, snapshot và nhân bản dữ liệu. | 01/21/2026 | 01/21/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Instance Store: đặc điểm, ưu nhược điểm và khi nào nên dùng. Thực hành: sao lưu EBS bằng snapshot và khôi phục dữ liệu. | 01/22/2026 | 01/22/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | User Data và Metadata trong EC2 — tự động hóa thiết lập khi khởi động instance. EC2 Auto Scaling: chính sách, multi-AZ và tích hợp Load Balancer. | 01/23/2026 | 01/23/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Các Pricing Option: On-Demand, Reserved Instance, Savings Plan, Spot Instance. Giới thiệu Lightsail, EFS, FSx và AWS MGN (Application Migration Service). | 01/24/2026 | 01/24/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 3

##### Amazon EC2 — Kiến trúc và cơ chế hoạt động

EC2 hoạt động tương tự máy ảo truyền thống nhưng linh hoạt và nhanh hơn nhiều:

- Instance có thể được khởi chạy trong vài phút và mở rộng tùy ý cho mọi loại workload — web server, backend ứng dụng, cơ sở dữ liệu, v.v.
- Kiến trúc nền tảng dựa trên **Nitro Hypervisor**, hỗ trợ chế độ ảo hóa **HVM** và **PV**.

##### Instance Type, AMI và Key Pair

- Mỗi **Instance Type** được định nghĩa bởi CPU, memory, throughput mạng và storage — lựa chọn đúng loại ảnh hưởng trực tiếp đến hiệu năng và chi phí.
- **AMI** là ảnh cấu hình sẵn gồm OS, phần mềm và cài đặt — xác định trạng thái ban đầu của instance khi khởi động.
- **Key Pair** dùng mã hóa bất đối xứng: AWS lưu public key, người dùng giữ private key để SSH (Linux) hoặc giải mã mật khẩu RDP (Windows).

##### EBS — Elastic Block Store

- Block storage gắn trực tiếp vào EC2 qua mạng riêng chuyên dụng.
- Các loại volume: **gp2/gp3** (SSD đa dụng), **io1/io2** (SSD hiệu năng cao), **st1/sc1** (HDD cho workload throughput lớn).
- **Snapshot** là bản lưu tăng dần trong S3 — chỉ lưu các block thay đổi kể từ snapshot trước.
- EBS volume tồn tại độc lập với vòng đời của instance.

##### Instance Store

- Lưu trữ tạm thời gắn vật lý với máy chủ host — IOPS cực cao.
- Dữ liệu bị mất khi instance bị dừng hoặc xóa. Phù hợp cho cache, buffer, dữ liệu tạm.

##### So sánh các loại Storage

| Loại Storage | Giao thức | Trường hợp sử dụng |
|---|---|---|
| EBS | Block | EC2 đơn lẻ, cơ sở dữ liệu |
| Instance Store | Block | Dữ liệu tạm/cache |
| EFS | NFS | Chia sẻ cho nhiều EC2 (Linux) |
| FSx | SMB / NTFS | Windows hoặc Linux hiệu năng cao |

##### EC2 Auto Scaling

- Tự động tăng/giảm số lượng instance theo nhu cầu hoặc lịch đặt sẵn.
- Trải rộng nhiều **Availability Zone** để đảm bảo tính khả dụng cao.
- Tích hợp với **Elastic Load Balancer** để phân phối traffic đến các instance đang hoạt động.
- Hỗ trợ kết hợp nhiều pricing option trong cùng một Auto Scaling Group.

##### Các Pricing Option của EC2

- **On-Demand**: thanh toán theo giây, không cam kết — linh hoạt nhất, chi phí cao nhất.
- **Reserved Instance & Savings Plan**: cam kết 1 hoặc 3 năm để tiết kiệm đáng kể (lên đến ~72%).
- **Spot Instance**: sử dụng capacity dư của AWS với giá rất rẻ — có thể bị ngắt với cảnh báo trước 2 phút.

##### Amazon Lightsail

Dịch vụ compute đơn giản hóa, phù hợp cho workload nhỏ, prototype và môi trường dev/test. Gộp compute, storage và networking vào một mức giá cố định hàng tháng.

##### AWS Application Migration Service (MGN)

Tự động hóa việc sao chép máy chủ vật lý hoặc ảo sang EC2 — nhân bản liên tục ở cấp block với thời gian downtime tối thiểu khi chuyển đổi.

##### Thực hành bổ sung

- Khám phá EC2 Console và CLI để kiểm tra tài nguyên.
- Tạo và quản lý key pair cho việc truy cập instance.
- Kiểm tra trạng thái dịch vụ đang chạy qua cả Console lẫn CLI.

Đến cuối tuần, đã tự tin quản lý toàn bộ vòng đời EC2 — từ khởi chạy, sao lưu đến mở rộng — trên cả trình duyệt lẫn dòng lệnh.
