---
title: "Tuần 2 Worklog"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Mục tiêu Tuần 2

- Hiểu rõ kiến trúc Amazon VPC và cách các thành phần mạng cốt lõi hoạt động trong AWS.
- Thực hành cấu hình Subnet, Route Table, Security Group, NACL, VPC Endpoint, VPC Peering và Elastic Load Balancer.
- Xác định giải pháp kết nối phù hợp giữa hạ tầng on-premises và AWS Cloud.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Tổng quan Amazon VPC: cấu trúc, giới hạn tài khoản, CIDR IPv4/IPv6. Tạo VPC cơ bản qua Console. | 01/12/2026 | 01/12/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Subnet và Availability Zone: địa chỉ IP, tạo public/private subnet, gán route table tương ứng. | 01/13/2026 | 01/13/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Route Table, Internet Gateway, NAT Gateway, VPC Endpoint. Phân biệt Interface Endpoint và Gateway Endpoint. | 01/14/2026 | 01/14/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Security Group, Network ACL, VPC Flow Logs. Thực hành kiểm soát truy cập và ghi log giữa các subnet. | 01/15/2026 | 01/15/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | VPC Peering, Transit Gateway, VPN, Direct Connect và các loại Elastic Load Balancer (ALB, NLB, CLB, GLB). | 01/16/2026 | 01/16/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 2

##### VPC — Virtual Private Cloud

VPC là môi trường mạng riêng biệt và cô lập trong AWS — dùng để phân tách và quản lý tài nguyên theo môi trường (Production / Dev / Test / Staging).

- Giới hạn mặc định: **5 VPC** mỗi tài khoản AWS.
- Mỗi VPC bắt buộc có **CIDR IPv4**; IPv6 là tùy chọn.

##### Subnet

- Mỗi subnet nằm trong một **Availability Zone** cố định.
- CIDR của subnet phải nằm trong phạm vi CIDR của VPC cha.
- AWS tự động dành **5 địa chỉ IP đầu tiên** trong mỗi subnet cho mục đích nội bộ.

##### Route Table

- Quy định cách định tuyến lưu lượng trong và ra khỏi VPC.
- Mỗi VPC có một **route table mặc định** được tạo tự động (không thể xóa) để xử lý giao tiếp nội bộ.

##### Elastic Network Interface (ENI) & Elastic IP

- **ENI** là card mạng ảo có thể gắn vào EC2 và di chuyển giữa các instance.
- **Elastic IP** là địa chỉ IPv4 công khai tĩnh, gắn với ENI.

##### VPC Endpoint

Cho phép tài nguyên trong VPC kết nối với các dịch vụ AWS mà không cần internet. Có hai loại:

- **Interface Endpoint** — dùng ENI với địa chỉ IP riêng.
- **Gateway Endpoint** — định tuyến qua route table; chỉ hỗ trợ S3 và DynamoDB.

##### Internet Gateway & NAT Gateway

- **Internet Gateway**: cho phép EC2 trong public subnet giao tiếp với internet. Được AWS quản lý hoàn toàn — không cần cấu hình autoscale.
- **NAT Gateway**: cấp quyền truy cập internet một chiều (outbound) cho các instance trong private subnet.

##### Security Group và Network ACL

- **Security Group (SG)**: tường lửa có trạng thái (stateful) gắn vào ENI; chỉ hỗ trợ quy tắc cho phép (allow).
- **Network ACL (NACL)**: tường lửa không trạng thái (stateless) áp dụng ở cấp subnet; có cả quy tắc allow và deny, đọc từ trên xuống.
- Mặc định: SG chặn inbound, cho phép outbound; NACL cho phép tất cả.

##### VPC Flow Logs

- Ghi lại metadata lưu lượng IP đến/đi từ VPC, subnet hoặc ENI.
- Lưu trữ trong **CloudWatch Logs** hoặc **S3**. Nội dung gói tin không được ghi lại.

##### VPC Peering & Transit Gateway

- **VPC Peering**: kết nối trực tiếp 1-1 giữa hai VPC. Không hỗ trợ định tuyến bắc cầu; CIDR không được trùng nhau.
- **Transit Gateway**: trung tâm kết nối nhiều VPC và mạng on-premises với nhau.

##### VPN & AWS Direct Connect

- **VPN Site-to-Site**: kết nối data center on-premises với AWS qua Virtual Private Gateway và Customer Gateway.
- **VPN Client-to-Site**: cho phép một thiết bị đơn lẻ kết nối an toàn vào tài nguyên bên trong VPC.
- **AWS Direct Connect**: đường truyền vật lý chuyên dụng từ data center đến AWS (qua Direct Connect Partner tại Việt Nam). Độ trễ thấp (~20–30 ms), băng thông có thể điều chỉnh.

##### Elastic Load Balancing (ELB)

Phân phối lưu lượng đến nhiều EC2 hoặc container. Có bốn loại:

- **ALB (Application Load Balancer)** — Layer 7, hỗ trợ định tuyến theo path và host.
- **NLB (Network Load Balancer)** — Layer 4, hiệu năng cao, hỗ trợ IP tĩnh.
- **CLB (Classic Load Balancer)** — Layer 4 & 7, phiên bản cũ, ít dùng.
- **GLB (Gateway Load Balancer)** — Layer 3, dùng giao thức GENEVE (cổng 6081) cho thiết bị mạng ảo bên thứ ba.

Hỗ trợ **Sticky Session** và **Access Log** (lưu vào S3).

##### Thực hành CLI bổ sung

Trong tuần, tiếp tục rèn luyện kỹ năng CLI:

- Duyệt tài nguyên EC2 và kiểm tra trạng thái các dịch vụ đang chạy.
- Tạo và quản lý key pair.
- Truy vấn thông tin về các dịch vụ đang hoạt động.

Đến cuối tuần, đã tự tin chuyển đổi linh hoạt giữa Console và CLI tùy theo yêu cầu công việc.
