---
title: "Tuần 8 Worklog"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu Tuần 8

- Nắm vững các khái niệm cơ bản về phân tích dữ liệu trên AWS và sự khác biệt so với cách tiếp cận xử lý dữ liệu truyền thống.
- Hiểu Data Lake là gì, khác với Data Warehouse như thế nào, và khi nào nên sử dụng từng loại.
- Thành thạo các dịch vụ analytics cốt lõi trên AWS: Amazon Athena, AWS Glue, Amazon Kinesis, Amazon EMR và Amazon QuickSight.
- Hiểu vai trò của AWS Glue Data Catalog như một kho metadata thống nhất.
- Biết cách xây dựng pipeline analytics end-to-end trên AWS — từ thu thập dữ liệu đến trực quan hóa.
- Làm quen với các mô hình xử lý luồng dữ liệu thời gian thực bằng Amazon Kinesis.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Nghiên cứu khái niệm Data Lake: định nghĩa, các tầng kiến trúc (raw, curated, consumption) và sự khác biệt so với Data Warehouse về schema, tính linh hoạt và chi phí. | 02/23/2026 | 02/23/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Khám phá AWS Glue: schema discovery dựa trên crawler, tạo ETL job, Glue Data Catalog, Glue Studio visual editor và Glue DataBrew cho chuẩn bị dữ liệu không cần code. | 02/24/2026 | 02/24/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Tìm hiểu Amazon Athena: dịch vụ truy vấn tương tác serverless, SQL trên S3 dựa trên Presto, tối ưu phân vùng và định dạng cột (Parquet, ORC), và kiểm soát chi phí qua giới hạn dữ liệu quét. | 02/25/2026 | 02/25/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Khảo sát Amazon Kinesis: Data Streams (thu thập thời gian thực), Data Firehose (deliver đến S3/Redshift/OpenSearch), Data Analytics (SQL trên stream) và Video Streams cho luồng media. | 02/26/2026 | 02/26/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Tìm hiểu Amazon EMR: cluster Hadoop/Spark có quản lý, cấu hình instance fleet, EMR Serverless và EMR on EKS, cùng các mẫu xử lý big data phổ biến ở quy mô lớn. | 02/27/2026 | 02/27/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Khám phá Amazon QuickSight: SPICE in-memory engine, kết nối dataset, các loại biểu đồ, ML Insights (phát hiện bất thường, dự báo) và nhúng dashboard vào ứng dụng. | 02/28/2026 | 02/28/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 8

##### Data Lake vs. Data Warehouse — Khái niệm Nền tảng

**Data Lake** là kho lưu trữ tập trung chứa dữ liệu thô ở định dạng gốc với quy mô lớn — có cấu trúc, bán cấu trúc và phi cấu trúc — với chi phí thấp hơn đáng kể so với Data Warehouse:

| Chiều so sánh | Data Lake | Data Warehouse |
|---|---|---|
| **Schema** | Schema-on-read (áp dụng lúc truy vấn) | Schema-on-write (bắt buộc lúc nạp dữ liệu) |
| **Loại dữ liệu** | Mọi định dạng (JSON, CSV, Parquet, ảnh, log) | Chỉ dữ liệu có cấu trúc, quan hệ |
| **Chi phí** | Thấp (lưu trữ dựa trên S3) | Cao hơn (tính toán và lưu trữ đặt cùng nhau) |
| **Linh hoạt** | Cao — lưu trước, định nghĩa cấu trúc sau | Thấp — schema phải xác định từ đầu |
| **Độ trễ truy vấn** | Cao hơn với ad hoc query | Thấp hơn với báo cáo đã tổng hợp sẵn |
| **Dịch vụ AWS** | S3 + Glue + Athena | Amazon Redshift |

**Kiến trúc Lake House** — khuyến nghị hiện đại của AWS — kết hợp cả hai: Data Lake trên S3 làm kho lưu trữ trung tâm, với Redshift làm Data Warehouse cho analytics có cấu trúc, kết nối qua Redshift Spectrum và Athena cho các truy vấn liên dịch vụ.

##### AWS Glue — ETL Serverless và Data Catalog

AWS Glue là dịch vụ ETL (Extract, Transform, Load) serverless được quản lý hoàn toàn, tạo nên xương sống của hầu hết các pipeline dữ liệu trên AWS:

- **Glue Crawler**: tự động quét các nguồn dữ liệu (S3, RDS, Redshift, DynamoDB), suy luận schema và điền vào **Glue Data Catalog** — kho metadata trung tâm tương thích với Apache Hive Metastore.
- **Glue ETL Job**: script Python hoặc Scala chạy trên Apache Spark, tự động mở rộng tính toán (DPU — Data Processing Unit) mà không cần quản lý cluster.
- **Glue Studio**: giao diện kéo-thả trực quan để xây dựng pipeline ETL không cần viết code — tự động tạo script PySpark.
- **Glue DataBrew**: công cụ chuẩn bị dữ liệu không cần code cho data analyst — áp dụng hơn 250 phép biến đổi có sẵn, lập hồ sơ dataset và phát hiện vấn đề chất lượng theo cách tương tác.
- **Glue Data Catalog**: đóng vai trò là registry schema thống nhất cho Athena, EMR, Redshift Spectrum và Lake Formation — cung cấp nguồn sự thật duy nhất về dữ liệu tồn tại và cấu trúc của nó.
- **Glue Workflow**: điều phối pipeline ETL nhiều bước với trigger (on-demand, theo lịch hoặc theo sự kiện).

##### Amazon Athena — Truy vấn Tương tác Serverless

Athena là dịch vụ truy vấn tương tác serverless cho phép chạy SQL tiêu chuẩn trực tiếp trên dữ liệu trong Amazon S3 mà không cần quản lý bất kỳ hạ tầng nào:

- **Engine dựa trên Presto**: thực thi ANSI SQL với hỗ trợ join phức tạp, window function và subquery trên petabyte dữ liệu trong S3.
- **Tính phí theo lượng dữ liệu quét**: $5 mỗi TB dữ liệu được quét — khiến việc tối ưu truy vấn trực tiếp liên quan đến giảm chi phí.
- **Chiến lược tối ưu chi phí:**
  - **Partitioning**: tổ chức dữ liệu S3 theo prefix (ví dụ: `year=/month=/day=`) để Athena chỉ quét các partition liên quan.
  - **Định dạng cột**: chuyển CSV/JSON sang **Parquet** hoặc **ORC** để giảm lượng dữ liệu quét đến 60–90%.
  - **Nén**: Snappy hoặc Gzip tiếp tục giảm dữ liệu đọc từ S3.
- **Federated query**: Athena Data Source Connector cho phép truy vấn dữ liệu trong RDS, DynamoDB, Redshift và nguồn tùy chỉnh cùng với S3.
- **Athena for Apache Spark**: chạy ứng dụng Spark tương tác không cần quản lý cluster.
- **Tích hợp**: kết quả có thể ghi lại vào S3, trực quan hóa trong QuickSight hoặc sử dụng trong các pipeline downstream.

##### Amazon Kinesis — Xử lý Luồng Dữ liệu Thời gian Thực

Kinesis là bộ dịch vụ thu thập, xử lý và phân tích dữ liệu streaming thời gian thực ở quy mô lớn:

**Kinesis Data Streams (KDS):**
- Thu thập dữ liệu thời gian thực với độ trễ dưới giây.
- Dữ liệu được phân vùng qua các **shard** (1 MB/s đầu vào, 2 MB/s đầu ra mỗi shard); số shard quyết định thông lượng và chi phí.
- Dữ liệu được giữ 24 giờ (có thể mở rộng đến 365 ngày).
- Consumer bao gồm Lambda, Kinesis Data Analytics, ứng dụng KCL (Kinesis Client Library) và Apache Flink.

**Kinesis Data Firehose:**
- Pipeline delivery được quản lý hoàn toàn — không cần ứng dụng consumer.
- Buffer, tùy chọn biến đổi (qua Lambda) và deliver đến S3, Redshift, Amazon OpenSearch, Splunk hoặc bất kỳ HTTP endpoint nào.
- Delivery gần thời gian thực (buffer window: 60 giây hoặc 1–128 MB).

**Kinesis Data Analytics:**
- Chạy ứng dụng Apache Flink hoặc truy vấn SQL trên luồng dữ liệu trực tiếp theo thời gian thực.
- Phát hiện mẫu, phân chia theo phiên sự kiện, tính toán tổng hợp liên tục và kích hoạt cảnh báo.

**Kinesis Video Streams:**
- Thu thập, lưu trữ và xử lý luồng video từ thiết bị (camera, cảm biến IoT) để phục vụ ML hoặc phát lại.

##### Amazon EMR — Xử lý Big Data Có Quản lý

Amazon EMR là dịch vụ đám mây được quản lý hoàn toàn để chạy các tác vụ xử lý dữ liệu phân tán quy mô lớn bằng các framework mã nguồn mở:

- **Framework được hỗ trợ**: Apache Spark, Hadoop MapReduce, Hive, HBase, Presto, Flink và nhiều hơn nữa.
- **Chế độ cluster**:
  - **Cluster chạy liên tục**: cluster bền vững cho khối lượng công việc liên tục.
  - **Cluster nhất thời**: khởi tạo, chạy job, kết thúc — chỉ trả tiền cho thời gian thực thi job.
- **EMR Serverless**: gửi Spark hoặc Hive job mà không cần quản lý cluster; EMR tự động phân bổ tài nguyên và scale về 0 khi nhàn rỗi.
- **EMR on EKS**: chạy Spark job trên Amazon EKS, chia sẻ hạ tầng Kubernetes với các khối lượng công việc container khác.
- **Instance Fleet**: kết hợp On-Demand và Spot Instance được EMR tối ưu để cân bằng chi phí và độ khả dụng.
- **Lưu trữ**: job xử lý dữ liệu trực tiếp từ S3 (tách biệt tính toán/lưu trữ), tùy chọn dùng HDFS trên instance storage cho dữ liệu trung gian.

##### Amazon QuickSight — Business Intelligence Gốc Đám Mây

QuickSight là dịch vụ BI và trực quan hóa dữ liệu gốc đám mây được quản lý hoàn toàn của AWS, truy cập qua trình duyệt hoặc thiết bị di động mà không cần triển khai server:

- **SPICE (Super-fast, Parallel, In-memory Calculation Engine)**: nhập và cache dataset trong bộ nhớ để phản hồi truy vấn dưới giây ở quy mô lớn, ngay cả khi nguồn dữ liệu gốc không khả dụng.
- **Kết nối dữ liệu**: kết nối trực tiếp với Athena, Redshift, RDS, S3, Salesforce, Jira và nhiều nguồn bên thứ ba.
- **ML Insights:**
  - **Anomaly Detection**: tự động phát hiện các đột biến hoặc sụt giảm bất thường trong chỉ số.
  - **Forecasting**: dự báo chuỗi thời gian dựa trên ML sử dụng dữ liệu lịch sử.
  - **Auto-Narratives**: tự động tạo tóm tắt ngôn ngữ tự nhiên từ dữ liệu biểu đồ.
- **Embedded analytics**: dashboard có thể nhúng vào ứng dụng web bằng tính năng 1-click embedding, cho phép nhóm phát triển sản phẩm tích hợp analytics trực tiếp vào sản phẩm của họ.
- **Mô hình tính phí**: thanh toán theo phiên (người xem) hoặc theo tác giả — không có chi phí hạ tầng.

##### Pipeline Analytics End-to-End trên AWS

Một pipeline analytics điển hình trên AWS theo luồng như sau:

```
Nguồn dữ liệu thô
    │
    ▼
Amazon Kinesis / S3 (Thu thập & Lưu trữ)
    │
    ▼
AWS Glue (Crawl, Catalog, ETL Transform)
    │
    ▼
S3 Data Lake (Curated / Parquet / Phân vùng)
    │
    ▼
Amazon Athena / Redshift (Truy vấn & Phân tích)
    │
    ▼
Amazon QuickSight (Trực quan hóa & Chia sẻ)
```

##### Thực hành

- Tạo Data Lake trên S3 với cấu trúc prefix phân lớp (raw/, curated/, consumption/).
- Chạy AWS Glue Crawler trên S3 để khám phá schema và điền vào Data Catalog.
- Tạo Glue ETL job để chuyển đổi dữ liệu CSV sang Parquet với phân vùng theo ngày.
- Truy vấn dataset Parquet đã được curate bằng Amazon Athena và so sánh chi phí quét so với CSV gốc.
- Thiết lập luồng delivery Kinesis Data Firehose thu thập sự kiện thử nghiệm từ Lambda producer vào S3.
- Xây dựng dashboard QuickSight kết nối với Athena, trực quan hóa các chỉ số tổng hợp với tính năng dự báo được bật.
