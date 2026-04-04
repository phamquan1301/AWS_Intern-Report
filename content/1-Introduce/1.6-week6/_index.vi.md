---
title: "Tuần 6 Worklog"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu Tuần 6

- Nắm vững các khái niệm cơ bản về Cơ sở dữ liệu và phân biệt rõ RDBMS với NoSQL.
- Phân biệt hai loại hệ thống xử lý dữ liệu OLTP và OLAP, xác định loại ứng dụng phù hợp với từng hệ thống.
- Hiểu rõ cấu trúc và vai trò của Primary Key, Foreign Key, Index, Partition, Buffer và DB Log.
- Tìm hiểu các dịch vụ cơ sở dữ liệu trên AWS như Amazon RDS, Aurora, Redshift và ElastiCache.
- Biết cách tối ưu, phục hồi, mở rộng và bảo mật cơ sở dữ liệu trên nền tảng AWS.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Ôn tập các khái niệm nền tảng về cơ sở dữ liệu: Database, Session, Primary/Foreign Key, Index, Partition, Buffer, Execution Plan và DB Log. | 02/09/2026 | 02/09/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Tìm hiểu RDBMS (mô hình quan hệ, SQL) và các loại NoSQL — Document, Key-Value, Wide-column, Graph — cùng các trường hợp sử dụng tương ứng. | 02/10/2026 | 02/10/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | So sánh OLTP và OLAP, xác định loại ứng dụng phù hợp với từng mô hình và tìm hiểu vai trò của Data Warehouse trong pipeline phân tích dữ liệu. | 02/11/2026 | 02/11/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Tìm hiểu chuyên sâu Amazon RDS: kiến trúc managed, backup tự động, Read Replica, Multi-AZ failover, auto scaling storage và các tùy chọn mã hóa. | 02/12/2026 | 02/12/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Khám phá Amazon Aurora: kiến trúc đọc/ghi hiệu năng cao, Backtrack, Clone, Global Database và khả năng ghi đa node Multi-Master. | 02/13/2026 | 02/13/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Tổng quan Amazon Redshift và Amazon ElastiCache — vai trò của từng dịch vụ trong phân tích OLAP và caching in-memory cho các ứng dụng yêu cầu hiệu năng cao. | 02/14/2026 | 02/14/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 6

##### Các khái niệm cơ bản về Cơ sở dữ liệu

**Cơ sở dữ liệu (Database)** là hệ thống thông tin có cấu trúc hoặc bán cấu trúc, được lưu trữ trên thiết bị vật lý, thiết kế để phục vụ truy cập đồng thời từ nhiều ứng dụng. Các thành phần then chốt bao gồm:

| Khái niệm | Mô tả |
|---|---|
| **Session** | Khoảng thời gian từ khi thiết lập kết nối với hệ thống cơ sở dữ liệu cho đến khi ngắt kết nối. |
| **Primary Key** | Cột (hoặc tổ hợp cột) xác định duy nhất mỗi bản ghi trong bảng. |
| **Foreign Key** | Cột tham chiếu đến Primary Key của bảng khác, đảm bảo toàn vẹn tham chiếu giữa các quan hệ. |
| **Index** | Cấu trúc dữ liệu tăng tốc truy vấn đọc, nhưng tiêu tốn thêm bộ nhớ và làm chậm thao tác ghi. |
| **Partition** | Chia bảng lớn thành các phân đoạn nhỏ hơn để cải thiện hiệu năng truy vấn trên tập con dữ liệu. |
| **Execution Plan** | Kế hoạch thực thi truy vấn do query optimizer tạo ra, chọn con đường hiệu quả nhất để xử lý câu lệnh SQL. |
| **DB Log** | Bản ghi trước khi ghi (write-ahead log) lưu lại toàn bộ thay đổi, dùng để phục hồi sau sự cố và đồng bộ giữa primary và replica. |
| **Buffer** | Vùng bộ nhớ tạm lưu trữ các khối dữ liệu vừa đọc hoặc vừa ghi trước khi flush xuống đĩa, giảm thiểu độ trễ I/O. |

##### RDBMS & NoSQL

**RDBMS (Hệ quản trị Cơ sở dữ liệu Quan hệ):**

- Tổ chức dữ liệu thành các bảng với hàng và cột; mối quan hệ giữa các thực thể được biểu diễn qua khóa chính và khóa ngoại.
- Sử dụng **SQL** làm ngôn ngữ chuẩn để truy vấn và thao tác dữ liệu.
- Đảm bảo thuộc tính **ACID** — Atomicity, Consistency, Isolation, Durability — phù hợp với các ứng dụng đòi hỏi tính nhất quán cao.

**NoSQL (Not Only SQL):**

- Thoát khỏi mô hình bảng cứng nhắc, thay vào đó sử dụng cấu trúc dữ liệu linh hoạt, tùy chọn schema.
- Các kiểu NoSQL phổ biến:

| Loại | Ví dụ |
|---|---|
| Dạng Document | MongoDB |
| Dạng Key-Value | Redis, DynamoDB |
| Dạng Wide-column | Cassandra |
| Dạng Graph | Neo4j |

- Phù hợp nhất cho các ứng dụng big data, dữ liệu phi cấu trúc, hoặc cần mở rộng ngang linh hoạt.

##### OLTP vs. OLAP

| Đặc điểm | OLTP | OLAP |
|---|---|---|
| **Mục đích** | Xử lý giao dịch thời gian thực | Phân tích dữ liệu lịch sử tổng hợp |
| **Hồ sơ dữ liệu** | Cập nhật thường xuyên, hướng hàng | Đọc nhiều, tổng hợp, hướng cột |
| **Ứng dụng điển hình** | Ngân hàng, thương mại điện tử, đặt vé | BI, báo cáo, phân tích dữ liệu |
| **Tối ưu hóa** | Thông lượng giao dịch và đồng thời | Thời gian phản hồi truy vấn phức tạp |
| **Dịch vụ AWS** | RDS, Aurora | Redshift, Athena, QuickSight |

##### Amazon RDS (Relational Database Service)

Dịch vụ cơ sở dữ liệu quan hệ được quản lý hoàn toàn, giải phóng khỏi gánh nặng vận hành như cài đặt, vá lỗi và quản lý hạ tầng:

- **Engine được hỗ trợ:** Aurora, MySQL, PostgreSQL, MariaDB, Oracle và SQL Server.
- **Các tính năng chính:**
  - **Backup tự động** — sao lưu liên tục DB và log với cửa sổ giữ tối đa 35 ngày.
  - **Read Replica** — phân tải công việc đọc nặng (ví dụ: báo cáo) khỏi instance chính.
  - **Multi-AZ deployment** — replica đồng bộ ở AZ khác, tự động chuyển đổi khi primary gặp sự cố.
  - **Mã hóa** — bảo vệ dữ liệu khi lưu trữ (AES-256) và khi truyền tải (TLS).
  - **Auto Scaling** — tự động điều chỉnh dung lượng lưu trữ và kích thước instance theo nhu cầu thực tế.
  - **Bảo mật mạng** — kiểm soát truy cập qua Security Group và Network ACL.
- Chủ yếu phục vụ khối lượng công việc **OLTP** đòi hỏi tính nhất quán mạnh và bảo đảm giao dịch.

##### Amazon Aurora

Engine cơ sở dữ liệu quan hệ được xây dựng đặc biệt cho đám mây, tối ưu hiệu năng và độ khả dụng, tương thích hoàn toàn với MySQL và PostgreSQL:

- Kế thừa toàn bộ tính năng của RDS, đồng thời mở rộng thêm:
  - **Backtrack** — tua ngược cơ sở dữ liệu về một thời điểm chính xác mà không cần khôi phục từ snapshot.
  - **Clone** — tạo bản sao độc lập gần như tức thì với chi phí lưu trữ bổ sung tối thiểu.
  - **Global Database** — trải rộng nhiều AWS region với một primary writer và tối đa 16 read replica mỗi region.
  - **Multi-Master** — cho phép ghi đồng thời từ nhiều node, loại bỏ điểm nghẽn single-writer.
- Dữ liệu được phân tán qua **ba Availability Zone** với nhân bản 6 chiều, đảm bảo độ bền cao và phục hồi tự động liền mạch.

##### Amazon Redshift

Dịch vụ **Data Warehouse** được quản lý hoàn toàn bởi AWS, xây dựng trên nền PostgreSQL mở rộng đặc biệt cho phân tích OLAP quy mô lớn:

- Sử dụng kiến trúc **Massively Parallel Processing (MPP)**:
  - **Leader Node** — phân tích và điều phối thực thi truy vấn trên các compute node.
  - **Compute Node** — lưu trữ dữ liệu dạng cột và thực thi song song các phân mảnh truy vấn.
- **Columnar storage** giúp chỉ đọc đúng các cột được tham chiếu trong truy vấn, giảm đáng kể I/O cho các câu truy vấn phân tích.
- Các tính năng bổ sung:
  - Kết nối qua **SQL, JDBC và ODBC** chuẩn công nghiệp.
  - **Redshift Spectrum** — chạy truy vấn SQL trực tiếp trên file dữ liệu trong Amazon S3 mà không cần nạp vào cluster.
  - **Pause/Resume** (chế độ transient cluster) — dừng cluster khi không dùng để tránh chi phí compute không cần thiết.

##### Amazon ElastiCache

Dịch vụ caching in-memory được quản lý hoàn toàn, giúp giảm tải đáng kể cho cơ sở dữ liệu bên dưới và cải thiện thời gian phản hồi cho dữ liệu được truy cập thường xuyên:

- Hỗ trợ hai engine caching: **Redis** và **Memcached**.
- AWS tự động xử lý việc cung cấp node, vá lỗi, phát hiện lỗi và thay thế node bị hỏng.
- **Redis** thường được ưu tiên hơn nhờ bộ tính năng phong phú:
  - Nhân bản và phân cụm độ khả dụng cao.
  - Dữ liệu được duy trì qua các lần khởi động lại.
  - Hỗ trợ nhắn tin Pub/Sub và scripting Lua.
- Logic vô hiệu hóa cache ở tầng ứng dụng cần được thiết kế cẩn thận để ngăn phục vụ dữ liệu cũ sau khi cơ sở dữ liệu được cập nhật.

##### Thực hành

- Điều hướng trong AWS Management Console để xem các instance EC2 đang chạy và kiểm tra trạng thái dịch vụ.
- Tạo và quản lý EC2 key pair phục vụ truy cập SSH bảo mật.
- Kiểm tra cấu hình các dịch vụ AWS đang hoạt động.
- Củng cố khả năng phối hợp giữa AWS Management Console và AWS CLI để quản lý tài nguyên song song trên cả hai giao diện đồ họa lẫn lập trình.
