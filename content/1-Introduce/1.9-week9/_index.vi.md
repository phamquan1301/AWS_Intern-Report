---
title: "Tuần 9 Worklog"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Mục tiêu Tuần 9

- Chuyển từ kiến thức lý thuyết sang triển khai thực tế các khái niệm data engineering trên AWS.
- Thiết kế và xây dựng kiến trúc Data Lake hoàn chỉnh trên Amazon S3 với các zone lưu trữ phân lớp hợp lý.
- Triển khai pipeline ETL end-to-end bằng AWS Glue để thu thập, biến đổi và publish dữ liệu.
- Cấu hình và sử dụng Amazon Athena để truy vấn dữ liệu đã biến đổi hiệu quả với phân vùng và định dạng cột.
- Xây dựng pipeline thu thập dữ liệu thời gian thực bằng Amazon Kinesis Data Firehose.
- Giám sát, bảo mật và quản trị Data Lake bằng AWS Lake Formation, IAM và CloudWatch.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Thiết kế S3 Data Lake 3 zone (raw, curated, consumption), cấu hình bucket policy, lifecycle rule, S3 Intelligent-Tiering và bật mã hóa phía server bằng KMS. | 03/02/2026 | 03/02/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Xây dựng và chạy AWS Glue Crawler trên dữ liệu vùng raw, kiểm chứng schema inference trong Glue Data Catalog và giải quyết các vấn đề phân vùng động cho dataset phân vùng theo ngày lớn. | 03/03/2026 | 03/03/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Viết Glue ETL job nhiều bước: đọc CSV từ raw zone, áp dụng đổi tên cột, ép kiểu, loại trùng lặp và phân vùng theo ngày, sau đó ghi Parquet vào curated zone. | 03/04/2026 | 03/04/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Thiết lập pipeline thu thập thời gian thực bằng Kinesis Data Firehose: cấu hình Python producer script, biến đổi record bằng Lambda inline và deliver file Parquet nén vào S3. | 03/05/2026 | 03/05/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Triển khai AWS Lake Formation: đăng ký S3 Data Lake, cấp quyền theo cột và theo hàng cho IAM role, kiểm chứng kiểm soát truy cập từ Athena trên nhiều danh tính người dùng khác nhau. | 03/06/2026 | 03/06/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Tinh chỉnh hiệu năng truy vấn Athena: áp dụng partition pruning, viết lại truy vấn dùng columnar projection, phân tích execution plan với EXPLAIN, thiết lập workgroup cost control và tái sử dụng kết quả truy vấn. | 03/07/2026 | 03/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 9

##### Thiết kế S3 Data Lake — Kiến trúc Ba Zone

Một Data Lake trên S3 có cấu trúc tốt được tổ chức thành ba zone lưu trữ logic, mỗi zone phục vụ một mục đích riêng biệt trong vòng đời dữ liệu:

| Zone | Mục đích | Trạng thái dữ liệu | Định dạng điển hình |
|---|---|---|---|
| **Raw Zone** | Vùng tiếp nhận tất cả dữ liệu đến đúng như gốc | Chưa xử lý, bất biến | CSV, JSON, log, nhị phân |
| **Curated Zone** | Dữ liệu đã làm sạch, kiểm chứng và biến đổi sẵn sàng cho analytics | Đã xử lý, phân vùng | Parquet, ORC |
| **Consumption Zone** | Dataset tổng hợp hoặc theo domain được tối ưu cho từng use case | Tối ưu truy vấn | Parquet (phân vùng theo khóa nghiệp vụ) |

Các cấu hình S3 chính áp dụng cho Data Lake:

- **Bucket policy**: hạn chế quyền ghi trực tiếp vào raw zone chỉ cho các role thu thập được ủy quyền; ngăn consumer bỏ qua curated layer.
- **S3 Lifecycle Rule**: tự động chuyển object trong raw zone sang S3-IA sau 30 ngày và Glacier sau 90 ngày để giảm thiểu chi phí lưu trữ dài hạn.
- **S3 Intelligent-Tiering**: áp dụng cho curated zone nơi access pattern khó đoán, tự động chuyển object giữa tầng truy cập thường xuyên và không thường xuyên.
- **Mã hóa**: tất cả zone được mã hóa khi lưu trữ bằng **SSE-KMS** với Customer Managed Key, cho phép chính sách xoay key riêng theo từng zone.
- **Versioning**: bật trên raw zone để cung cấp cơ chế phục hồi khi ghi đè hoặc xóa nhầm.

##### AWS Glue — Xây dựng Pipeline ETL

**Cấu hình Crawler:**

Chạy Glue Crawler trên raw zone cho phép phát hiện tự động các thay đổi cấu trúc file mà không cần cập nhật schema thủ công:

- Crawler suy luận partition key tương thích Hive từ S3 prefix (ví dụ: `s3://bucket/raw/sales/year=2026/month=03/`).
- **Partition projection**: cấu hình trực tiếp trong Glue Data Catalog để tránh chạy lại crawler khi có partition mới — giảm đáng kể độ trễ làm mới catalog cho dữ liệu đến với tần suất cao.
- **Exclusion pattern**: cấu hình để bỏ qua file ẩn, marker `_SUCCESS` và file upload chưa hoàn chỉnh.

**ETL Job — Biến đổi CSV sang Parquet:**

Glue ETL job thực hiện các bước biến đổi sau bằng DynamicFrame API:

1. **Đọc**: nạp CSV thô từ S3 dùng `create_dynamic_frame.from_catalog()`.
2. **Ánh xạ**: đổi tên cột sang snake_case, ép kiểu timestamp dạng chuỗi thành kiểu `timestamp`, chuyển số dạng chuỗi sang `double`.
3. **Lọc**: loại bỏ bản ghi có primary key null hoặc ngày không hợp lệ bằng `Filter.apply()`.
4. **Loại trùng lặp**: dùng `toDF().dropDuplicates(['id', 'event_date'])` để xóa bản sao hoàn toàn.
5. **Phân vùng lại**: repartition theo ngày để căn chỉnh độ chi tiết file đầu ra với pattern truy vấn downstream.
6. **Ghi**: xuất Parquet với nén Snappy vào curated zone, phân vùng theo `year/month/day`.

**Job Bookmark**: bật để theo dõi các object S3 đã xử lý, đảm bảo các lần chạy tăng dần chỉ xử lý dữ liệu mới thay vì reprocess toàn bộ dataset.

##### Thu thập Thời gian Thực với Kinesis Data Firehose

Pipeline thu thập sự kiện thời gian thực được xây dựng để xử lý luồng dữ liệu liên tục:

**Kiến trúc:**

```
Python Producer (Boto3)
    │  Gọi API PutRecord
    ▼
Kinesis Data Firehose Delivery Stream
    │  Buffer: 60s / 5MB
    ├─► Lambda Transform (JSON → NDJSON, bổ sung trường)
    │
    ▼
S3 Raw Zone (phân vùng theo thời gian delivery: yyyy/MM/dd/HH)
    │
    ▼
Glue Crawler (kích hoạt theo lịch) → cập nhật Data Catalog
    │
    ▼
Athena (có thể truy vấn trong vài phút sau khi thu thập)
```

**Các cấu hình chính:**

- **Buffer hint**: đặt 60 giây buffer interval với ngưỡng kích thước 5 MB — đạt điều kiện nào trước sẽ kích hoạt S3 PUT.
- **Lambda transform**: function inline làm giàu record với `processing_timestamp` và chuyển JSON array thành newline-delimited JSON (NDJSON) tương thích Glue.
- **Error logging**: record biến đổi thất bại chuyển hướng đến S3 prefix riêng để kiểm tra dead-letter thủ công.
- **Nén**: Firehose cấu hình nén GZIP giảm khoảng 70% dung lượng lưu trữ S3.

##### AWS Lake Formation — Kiểm soát Truy cập Tinh tế

AWS Lake Formation nằm trên Glue Data Catalog và IAM để cung cấp kiểm soát truy cập theo kiểu database trên dữ liệu dựa trên S3:

- **Đăng ký Data Lake**: S3 location đăng ký với Lake Formation để ủy quyền quyết định truy cập ra khỏi bucket policy S3 thô.
- **Quyền theo bảng**: cấp `SELECT` trên các Glue table cụ thể cho IAM role analyst, đồng thời hạn chế hoàn toàn truy cập vào raw zone table.
- **Bảo mật theo cột**: ẩn các cột PII (ví dụ: `email`, `phone_number`) từ curated zone cho role analyst không có đặc quyền — cột đơn giản không xuất hiện trong kết quả truy vấn Athena.
- **Lọc theo hàng (RLS)**: áp dụng biểu thức filter dữ liệu (ví dụ: `region = 'VN'`) theo IAM principal, đảm bảo analyst chỉ thấy hàng liên quan đến domain nghiệp vụ của họ.
- **Kiểm soát truy cập dựa trên tag (TBAC)**: phân loại bảng bằng Lake Formation tag (ví dụ: `sensitivity=PII`, `domain=finance`) và cấp quyền ở cấp tag thay vì từng bảng — cho phép quản lý chính sách có thể mở rộng khi thêm bảng mới.

##### Amazon Athena — Tối ưu Truy vấn Trong Thực tế

Vượt ra ngoài truy vấn cơ bản, một số kỹ thuật tối ưu được áp dụng và đo benchmark:

**Partition Pruning:**
```sql
-- Trước: quét toàn bộ bảng (quét tất cả năm)
SELECT * FROM sales WHERE CAST(event_date AS DATE) = DATE '2026-03-01';

-- Sau: nhận biết partition (chỉ quét year=2026/month=03/day=01)
SELECT * FROM sales WHERE year='2026' AND month='03' AND day='01';
```
Kết quả: lượng dữ liệu quét giảm từ **18 GB → 420 MB** (giảm 98%).

**Columnar Projection:**
```sql
-- Chỉ chọn cột cần thiết — Parquet chỉ đọc các chunk cột đó
SELECT order_id, total_amount, customer_id
FROM sales
WHERE year='2026' AND status='completed';
```

**Tái sử dụng kết quả truy vấn**: bật trong cài đặt Athena Workgroup — truy vấn giống nhau trong cửa sổ 60 phút trả về kết quả cache tức thời với zero chi phí quét bổ sung.

**Cost control workgroup**: cấu hình giới hạn quét dữ liệu 1 GB mỗi truy vấn và ngân sách quét hàng tháng theo workgroup với cảnh báo SNS, ngăn các truy vấn không kiểm soát phát sinh chi phí ngoài dự kiến.

##### Chất lượng Dữ liệu và Giám sát

**AWS Glue Data Quality (quy tắc DQDL):**
- Định nghĩa các quy tắc như `ColumnValues "order_id" matches "[A-Z]{3}-[0-9]{6}"` và `Completeness "email" >= 0.95` để gắn cờ các record không đạt kiểm tra chất lượng.
- Record thất bại được cách ly vào S3 prefix riêng để xem xét thủ công.

**Giám sát CloudWatch:**
- Các chỉ số Glue Job (thời gian thực thi, DPU hour, số record ghi) theo dõi qua CloudWatch dashboard.
- Tỷ lệ thành công delivery Firehose và chỉ số `DeliveryToS3.Success` được giám sát với cảnh báo khi < 99%.
- Thời gian thực thi truy vấn Athena và dữ liệu quét theo workgroup theo dõi để phân bổ chi phí theo nhóm.

##### Thực hành

- Xây dựng cấu trúc S3 Data Lake 3 zone đầy đủ với quy ước đặt tên theo phong cách Terraform và áp dụng chính sách lifecycle và mã hóa.
- Chạy Glue Crawler và gỡ lỗi các vấn đề schema inference cho file JSON lồng nhau bằng custom classifier.
- Viết và chạy Glue ETL job end-to-end: 4,2 triệu bản ghi CSV thô xử lý thành 890 MB Parquet trong dưới 6 phút với 5 G.1X DPU.
- Triển khai Kinesis Firehose stream với Lambda transform; xác minh độ trễ end-to-end từ sự kiện producer đến record có thể truy vấn trong Athena dưới 90 giây.
- Cấu hình column-level masking của Lake Formation và kiểm chứng với IAM role thử nghiệm rằng các cột bị hạn chế không xuất hiện trong kết quả Athena.
- Đo benchmark hiệu năng truy vấn Athena trước và sau khi phân vùng và chuyển đổi Parquet; đạt mức giảm chi phí 97% trên cùng một truy vấn phân tích.
