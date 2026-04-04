---
title: "Tuần 7 Worklog"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu Tuần 7

- Hiểu sâu các tính năng nâng cao của cơ sở dữ liệu quan hệ trên AWS.
- Nắm rõ sự khác biệt kiến trúc giữa Amazon Aurora và RDS truyền thống, và lý do Aurora vượt trội ở quy mô lớn.
- Khám phá Aurora Serverless, Aurora Global Database và Aurora Multi-Master cho các khối lượng công việc phân tán toàn cầu.
- Tìm hiểu các tính năng nâng cao của RDS: Multi-AZ, Read Replica liên vùng và Performance Insights.
- Biết cách tinh chỉnh, giám sát và tối ưu cơ sở dữ liệu quan hệ trên AWS cho môi trường production.
- Làm quen với dịch vụ proxy cơ sở dữ liệu (RDS Proxy) và cách nó cải thiện quản lý kết nối và khả năng phục hồi.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Tìm hiểu kiến trúc lưu trữ phân tán của Aurora: nhân bản 6 chiều trên 3 AZ, tự phục hồi storage, và cách Aurora loại bỏ độ trễ nhân bản điển hình của RDS tiêu chuẩn. | 02/16/2026 | 02/16/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Nghiên cứu Aurora Serverless v1 và v2: tự động mở rộng on-demand, Aurora Capacity Unit (ACU), các trường hợp sử dụng cho khối lượng công việc không liên tục và biến động, cơ chế tính phí theo giây. | 02/17/2026 | 02/17/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Tìm hiểu Aurora Global Database: nhân bản liên vùng độ trễ dưới 1 giây, chuyển đổi dự phòng có quản lý và các mẫu disaster recovery cho ứng dụng phân tán toàn cầu. | 02/18/2026 | 02/18/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Khám phá chuyên sâu RDS Multi-AZ: nhân bản đồng bộ so với không đồng bộ, cơ chế chuyển đổi dự phòng tự động, maintenance window và vá lỗi với thời gian downtime tối thiểu. | 02/19/2026 | 02/19/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Tìm hiểu RDS Proxy: connection pooling, tích hợp xác thực IAM, xử lý failover cải tiến và giảm tải cho database từ các khối lượng công việc serverless và Lambda. | 02/20/2026 | 02/20/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Khảo sát các công cụ tối ưu hiệu năng: RDS Performance Insights, Enhanced Monitoring, CloudWatch metrics cho database, slow query log và chiến lược tối ưu parameter group. | 02/21/2026 | 02/21/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 7

##### Amazon Aurora — Đào sâu Kiến trúc

Aurora thiết kế lại cơ sở dữ liệu quan hệ cho đám mây với một lớp lưu trữ hoàn toàn khác biệt so với cơ sở dữ liệu truyền thống:

- **Lưu trữ phân tán**: Aurora tách biệt tầng tính toán khỏi tầng lưu trữ. Lớp storage tự động nhân bản dữ liệu trên **6 storage node** phân tán qua **3 Availability Zone**, cung cấp độ dư thừa 6 chiều.
- **Tự phục hồi**: Nếu bất kỳ storage node nào gặp sự cố, Aurora tự động phục hồi bằng cách sử dụng các bản sao còn lại — không cần can thiệp của DBA.
- **Loại bỏ độ trễ nhân bản**: Do tất cả instance Aurora đọc từ cùng một volume lưu trữ phân tán dùng chung, các read replica phản ánh dữ liệu ghi với độ trễ gần như bằng 0 (thường dưới 10ms), khác hẳn RDS tiêu chuẩn nơi replica áp dụng binlog event bất đồng bộ.
- **Phục hồi sau crash**: Aurora phát lại redo log lúc khởi động thay vì tái tạo toàn bộ buffer pool, giảm đáng kể thời gian phục hồi sau sự cố.

| Tính năng | RDS Tiêu chuẩn | Amazon Aurora |
|---|---|---|
| Nhân bản storage | 2 bản sao (1 AZ/engine) | 6 bản sao trên 3 AZ |
| Độ trễ read replica | Vài giây (bất đồng bộ) | Dưới 10ms (storage dùng chung) |
| Auto-scaling storage | Thay đổi thủ công | Tự động tăng theo bước 10 GB |
| Thời gian phục hồi crash | Vài phút | Vài giây |

##### Aurora Serverless

Aurora Serverless loại bỏ nhu cầu cấp phát hoặc quản lý dung lượng database — tính toán tự động mở rộng dựa trên mức sử dụng thực tế:

- **Aurora Capacity Unit (ACU)**: mỗi ACU đại diện cho khoảng 2 GiB bộ nhớ cùng CPU và mạng tương ứng. Bạn định nghĩa mức ACU tối thiểu và tối đa, Aurora tự mở rộng trong khoảng đó.
- **Serverless v2** (được khuyến nghị): mở rộng theo bước nhỏ 0.5 ACU, phản hồi nhu cầu trong vài phần trăm giây, hỗ trợ Multi-AZ cho độ khả dụng cao.
- **Serverless v1**: phù hợp cho khối lượng công việc không thường xuyên hoặc khó dự đoán; có thể scale về 0 và tạm dừng hoàn toàn khi không hoạt động, tiếp tục khi có kết nối mới (với độ trễ cold-start ngắn).
- **Tính phí**: tính theo ACU-giây tiêu thụ, cực kỳ tiết kiệm cho môi trường dev/test, ứng dụng theo mùa hoặc không liên tục.
- **Trường hợp lý tưởng**: ứng dụng mới với lưu lượng chưa xác định, môi trường development và staging, xử lý batch không thường xuyên và ứng dụng SaaS với tải biến động theo tenant.

##### Aurora Global Database

Aurora Global Database cho phép một cluster Aurora duy nhất trải rộng nhiều AWS region, cung cấp đọc toàn cầu độ trễ thấp và phục hồi thảm họa nhanh:

- **Kiến trúc**: một **region chính** nhận toàn bộ ghi; tối đa **5 region phụ** mỗi region duy trì các read-only replica.
- **Nhân bản**: sử dụng nhân bản ở tầng storage chuyên dụng (không phải binlog ở tầng database), đạt độ trễ nhân bản điển hình **dưới 1 giây**.
- **Failover có quản lý**: khi xảy ra sự cố khu vực, một region phụ có thể được thăng cấp lên primary trong khoảng **1 phút** (RTO < 1 phút, RPO < 1 giây).
- **Đọc toàn cầu**: ứng dụng ở region phụ có thể truy vấn replica cục bộ mà không cần vượt qua ranh giới region, giảm đáng kể độ trễ đọc cho người dùng phân tán địa lý.
- **Write forwarding (Serverless v2)**: region phụ có thể tùy chọn chuyển tiếp yêu cầu ghi đến region chính một cách trong suốt, đơn giản hóa logic ứng dụng.

##### RDS Multi-AZ — Chuyên sâu

Multi-AZ là cơ chế chính của AWS để đảm bảo tính khả dụng cao đồng bộ trong một region duy nhất:

- **Nhân bản đồng bộ**: mỗi lần ghi được commit trên primary đều được nhân bản đồng bộ đến standby trước khi xác nhận — không mất dữ liệu khi failover.
- **Failover tự động**: nếu primary không còn khỏe mạnh (lỗi instance, lỗi storage, crash OS hoặc AZ outage), RDS tự động thăng cấp standby và cập nhật CNAME DNS endpoint, thường hoàn tất trong **60–120 giây**.
- **Standby không đọc được**: Multi-AZ standby chỉ là mục tiêu failover thuần túy — không thể phục vụ traffic đọc (dùng Read Replica cho mục đích đó).
- **Vá lỗi với downtime tối thiểu**: RDS áp dụng bản vá cho standby trước, thực hiện failover, sau đó vá primary cũ — giảm thiểu cửa sổ bảo trì hiển thị với người dùng.

**So sánh Multi-AZ và Read Replica:**

| Khía cạnh | Multi-AZ | Read Replica |
|---|---|---|
| Mục đích | Khả dụng cao / Failover | Mở rộng đọc / Báo cáo |
| Nhân bản | Đồng bộ | Bất đồng bộ |
| Đọc được | Không | Có |
| Phạm vi | Cùng region (AZ khác) | Cùng hoặc khác region |
| Failover | Tự động | Thăng cấp thủ công |

##### Amazon RDS Proxy

RDS Proxy là dịch vụ proxy database được quản lý hoàn toàn, độ khả dụng cao, nằm giữa ứng dụng và database RDS hoặc Aurora:

- **Connection pooling**: duy trì một pool các kết nối database đã được thiết lập sẵn và ghép kênh chúng qua các kết nối ứng dụng — quan trọng cho Lambda function và container workload thường xuyên mở và đóng kết nối.
- **Giảm tải database**: bằng cách tái sử dụng kết nối, RDS Proxy có thể hỗ trợ hàng nghìn kết nối ứng dụng đồng thời trong khi chỉ duy trì một phần nhỏ số lượng đó ở tầng database, giữ trong giới hạn max-connection của engine.
- **Xác thực IAM**: RDS Proxy hỗ trợ thông tin xác thực dựa trên IAM (không hardcode mật khẩu) cho xác thực database, cải thiện trạng thái bảo mật.
- **Tăng tốc failover**: trong các sự kiện Multi-AZ failover, RDS Proxy xếp hàng các kết nối đến và trong suốt chuyển hướng chúng đến primary mới, giảm thời gian failover hiệu quả mà ứng dụng nhận thấy.
- **Hỗ trợ**: MySQL, PostgreSQL, Aurora MySQL, Aurora PostgreSQL, MariaDB và SQL Server.

##### Tối ưu hiệu năng và Giám sát

AWS cung cấp hệ thống quan sát phong phú cho cơ sở dữ liệu quan hệ:

**RDS Performance Insights:**
- Trực quan hóa **DB Load** — số lượng phiên đang hoạt động trung bình tại bất kỳ thời điểm nào, phân tách theo wait event, câu lệnh SQL, host hoặc người dùng.
- Giúp xác định chính xác truy vấn hoặc nút thắt tài nguyên (CPU, I/O, lock wait) gây ra suy giảm hiệu năng.
- Lưu trữ 7 ngày lịch sử miễn phí; mở rộng đến 2 năm với gói lưu trữ trả phí.

**Enhanced Monitoring:**
- Stream OS-level metrics (CPU steal, áp lực bộ nhớ, file I/O) đến CloudWatch Logs với độ chi tiết 1 giây.
- Cần thiết để chẩn đoán các vấn đề ẩn ở tầng database (ví dụ: hiệu ứng noisy-neighbor trên host dùng chung).

**Parameter Group:**
- Kiểm soát cấu hình ở tầng engine (kích thước buffer pool, max connections, query cache, v.v.).
- Thay đổi tham số tĩnh yêu cầu khởi động lại DB instance; tham số động áp dụng ngay lập tức.
- Thực hành tốt: sử dụng parameter group tùy chỉnh thay vì mặc định để giữ quyền kiểm soát cài đặt trong quá trình nâng cấp.

**Slow Query Log:**
- Ghi lại các truy vấn vượt quá ngưỡng thời gian thực thi có thể cấu hình.
- Phân tích bằng công cụ như **pt-query-digest** (Percona Toolkit) hoặc trực tiếp qua CloudWatch Logs Insights để xác định các ứng viên cần tối ưu index.

##### Thực hành

- Khởi chạy Aurora MySQL cluster và quan sát hành vi của cluster endpoint so với reader endpoint dưới tải.
- Cấu hình Read Replica tại region phụ và kiểm chứng các chỉ số độ trễ nhân bản qua CloudWatch.
- Bật RDS Performance Insights và xác định các top wait event trên workload thử nghiệm.
- Tạo và áp dụng custom DB parameter group, điều chỉnh `max_connections` và `innodb_buffer_pool_size` cho instance tương thích MySQL.
- Kiểm tra hành vi connection pooling của RDS Proxy với Lambda function thực hiện burst call đến database.
