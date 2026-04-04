---
title: "Tuần 10 Worklog"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Mục tiêu Tuần 10

- Hiểu sâu hơn về các mô hình serverless analytics trên AWS và cách xây dựng giải pháp BI có thể mở rộng với chi phí hiệu quả.
- Thành thạo Amazon QuickSight từ kết nối dataset đến publish dashboard và nhúng analytics.
- Hiểu cách hoạt động của SPICE và khi nào nên dùng Direct Query so với SPICE import mode.
- Học cách xây dựng calculated field, parameter và dynamic control trong QuickSight cho dashboard tương tác.
- Khám phá QuickSight ML Insights: phát hiện bất thường, dự báo và tóm tắt tường thuật.
- Hiểu cách triển khai BI đa tenant với row-level security và column-level security trong QuickSight.
- Làm quen với việc nhúng QuickSight dashboard vào ứng dụng web bên ngoài bằng Embedding SDK.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Kết nối QuickSight với dataset Athena trên Data Lake từ Tuần 9; cấu hình lịch SPICE import và so sánh hiệu năng truy vấn giữa Direct Query và SPICE mode. | 03/09/2026 | 03/09/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Xây dựng calculated field, custom date hierarchy và quy tắc conditional formatting; thiết kế dashboard điều hành nhiều sheet với KPI card, bar chart, line chart và heat map. | 03/10/2026 | 03/10/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Triển khai control tương tác: parameter, filter action, URL action và custom tooltip; bật điều hướng cross-sheet và drill-down hierarchy cho self-service analytics. | 03/11/2026 | 03/11/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Khám phá QuickSight ML Insights: cấu hình phát hiện bất thường trên chỉ số doanh thu, xây dựng dự báo chuỗi thời gian với confidence interval và thêm tóm tắt Auto-Narrative cho sheet điều hành. | 03/12/2026 | 03/12/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Triển khai Row-Level Security (RLS) bằng file quy tắc dataset và Column-Level Security (CLS) để thực thi kiểm soát truy cập dữ liệu theo người dùng; kiểm thử cách ly truy cập qua các danh tính QuickSight khác nhau. | 03/13/2026 | 03/13/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 |**Sự Kiện:** Cloud Mastery 2026 - #1 AI From Scratch.<br> Nhúng QuickSight dashboard vào ứng dụng web Next.js bằng QuickSight Embedding SDK; triển khai anonymous và authenticated embedding với per-session parameter override. | 03/14/2026 | 03/14/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 10

##### Amazon QuickSight — Kiến trúc và Kết nối Dữ liệu

QuickSight kết nối với nhiều nguồn dữ liệu và hỗ trợ hai chế độ thực thi truy vấn khác nhau, đánh đổi giữa hiệu năng và độ tươi mới của dữ liệu:

**Kết nối Nguồn Dữ liệu:**

| Loại Nguồn | Ví dụ |
|---|---|
| Dịch vụ AWS | Athena, Redshift, RDS, Aurora, S3, OpenSearch |
| SaaS | Salesforce, ServiceNow, Adobe Analytics, Jira |
| Database | MySQL, PostgreSQL, SQL Server, Oracle (qua VPC) |
| File | CSV, Excel, JSON (upload trực tiếp vào SPICE) |

**Direct Query vs. SPICE:**

| Khía cạnh | Direct Query | SPICE Import |
|---|---|---|
| **Độ tươi dữ liệu** | Luôn live — truy vấn trực tiếp nguồn | Snapshot tại thời điểm import |
| **Hiệu năng** | Phụ thuộc tốc độ truy vấn nguồn | Dưới giây (in-memory) |
| **Chi phí** | Chi phí truy vấn nguồn (ví dụ: quét Athena) | Chi phí dung lượng SPICE |
| **Mở rộng** | Giới hạn bởi concurrency nguồn | Mở rộng đến hàng triệu reader |
| **Phù hợp nhất** | Chỉ số vận hành thời gian thực, dataset nhỏ | Dashboard phân tích, nhiều reader |

**SPICE** (Super-fast, Parallel, In-memory Calculation Engine) thu thập dữ liệu từ nguồn một lần và lưu ở định dạng cột trong bộ nhớ do QuickSight quản lý. Làm mới SPICE có thể lên lịch theo giờ, ngày hoặc kích hoạt qua API — cho phép dashboard gần thời gian thực mà không cần liên tục truy vấn nguồn bên dưới.

##### Xây dựng Dashboard — Thiết kế Trực quan và Tính toán

**Loại Sheet và Visual:**

Sheet QuickSight tổ chức visual thành các trang. Mỗi sheet tương ứng với một vùng tập trung (ví dụ: "Tổng quan Điều hành", "Phân tích Theo Vùng", "Phân tích Xu hướng"). Các loại visual chính được sử dụng:

| Visual | Phù hợp nhất cho |
|---|---|
| KPI Card | Một chỉ số với so sánh kỳ và màu điều kiện |
| Bar / Stacked Bar | So sánh danh mục hoặc phân tích theo thời gian |
| Line Chart | Xu hướng theo thời gian; hỗ trợ dual-axis cho chỉ số phụ |
| Heat Map | Tương quan giữa hai chiều trên một giá trị |
| Pivot Table | Bảng chéo với tổng và tổng phụ |
| Scatter Plot | Phát hiện ngoại lệ trên hai chiều số |
| Funnel Chart | Tỷ lệ chuyển đổi qua các giai đoạn có thứ tự |

**Calculated Field:**

Ngôn ngữ biểu thức của QuickSight hỗ trợ bộ hàm phong phú:

```
# Tỷ lệ tăng trưởng doanh thu so với kỳ trước
(sum({revenue}) - sum({revenue_prior})) / sum({revenue_prior}) * 100

# Số ngày kể từ đơn hàng cuối (phân khúc khách hàng)
dateDiff(parseDate({last_order_date}, 'yyyy-MM-dd'), now(), 'DD')

# Phân loại có điều kiện
ifelse({margin_pct} >= 0.3, 'Cao', {margin_pct} >= 0.1, 'Trung bình', 'Thấp')
```

**Conditional Formatting**: áp dụng cho KPI card và bảng — màu chữ, màu nền và chỉ thị icon được điều khiển bởi ngưỡng calculated field, giúp các bất thường hiển thị ngay lập tức mà không cần người đọc tự giải thích giá trị thô.

##### Control Tương tác — Parameter và Action

Dashboard tĩnh có giá trị hạn chế cho self-service analytics. Các tính năng tương tác của QuickSight cho phép người dùng khám phá dữ liệu một cách linh hoạt:

**Parameter:**
- Biến đặt tên có thể gắn với control lọc (dropdown, date picker, text input).
- Điều khiển tiêu đề động (ví dụ: "Báo cáo Doanh số cho {selected_region}"), URL action và filter liên kết.
- Có thể truyền qua URL lúc embed để dashboard được lọc sẵn.

**Filter Action:**
- Click-to-filter: nhấp vào một thanh trong bar chart sẽ lọc tất cả visual khác trên sheet theo giá trị chiều đó.
- Cross-sheet action: sự kiện nhấp trên Sheet 1 điều hướng đến Sheet 2 với filter đã áp dụng sẵn (mô hình drill-through).

**URL Action:**
- Khi nhấp vào phần tử visual, mở URL tùy chỉnh với các giá trị trường được nội suy — dùng để liên kết từ dashboard đến bản ghi nguồn trong CRM hoặc hệ thống ticket bên ngoài.

**Drill-Down Hierarchy:**
- Định nghĩa hierarchy ngày (Năm → Quý → Tháng → Ngày) hoặc hierarchy địa lý (Quốc gia → Vùng → Thành phố) để người dùng khám phá dần từ tổng quan đến chi tiết trong một visual.

##### QuickSight ML Insights

QuickSight tích hợp machine learning trực tiếp vào lớp analytics, có thể sử dụng mà không cần chuyên môn data science:

**Anomaly Detection (Phát hiện Bất thường):**
- Được hỗ trợ bởi thuật toán **Random Cut Forest (RCF)** chạy liên tục trên dữ liệu SPICE.
- Xác định các điểm dữ liệu bất thường về mặt thống kê trong chỉ số chuỗi thời gian (ví dụ: sụt giảm doanh thu bất thường vào thứ Ba).
- Cấu hình độ nhạy (1–100) và phân tích đóng góp để hiển thị *chiều nào* gây ra bất thường.
- Có thể kích hoạt **QuickSight Alert** qua email khi bất thường vượt ngưỡng cấu hình.

**Forecasting (Dự báo):**
- Áp dụng **mô hình dự báo chuỗi thời gian** độc quyền của AWS (tương tự DeepAR) để chiếu chỉ số về phía trước.
- Cấu hình kỳ dự báo (ngày, tuần, tháng) và khoảng dự đoán (confidence band 80%, 90%, 95%).
- Đường dự báo được hiển thị trực tiếp trên visual line chart — không cần export hay công cụ bên ngoài.

**Auto-Narrative (Tường thuật Tự động):**
- Tự động tạo mô tả ngôn ngữ tự nhiên về những gì đã thay đổi trong dataset.
- Hỗ trợ template tường thuật tùy chỉnh với tham chiếu trường động.
- Đặc biệt hữu ích cho sheet tóm tắt điều hành nơi các stakeholder phi kỹ thuật cần nội dung ngôn ngữ bình thường thay vì phải tự giải thích biểu đồ.

##### Row-Level Security và Column-Level Security

QuickSight cung cấp kiểm soát truy cập tinh tế ở cấp dataset, cho phép một dashboard duy nhất phục vụ nhiều nhóm người dùng với các chế độ xem dữ liệu bị hạn chế thích hợp:

**Row-Level Security (RLS):**

RLS được triển khai qua *permission dataset* — bảng ánh xạ liên kết mỗi người dùng hoặc nhóm QuickSight với các giá trị chiều họ được phép xem:

```
| UserName          | region | department |
|-------------------|--------|------------|
| alice@company.com | APAC   | Sales      |
| bob@company.com   | EMEA   | Finance    |
| carol@company.com |        |            |  ← (rỗng = xem tất cả)
```

Khi alice đăng nhập và mở dashboard, chế độ xem của cô ấy tự động được lọc theo `region = 'APAC' AND department = 'Sales'` — không cần code ứng dụng.

**Column-Level Security (CLS):**
- Hạn chế các cột cụ thể (ví dụ: `salary`, `social_security_number`) không xuất hiện trong dataset đối với nhóm người dùng được chỉ định.
- Cột bị hạn chế là vô hình trong visual, bảng filter và export — ngăn mọi truy cập gián tiếp.

**Bảo mật theo hàng dựa trên Tag** (tích hợp QuickSight + Lake Formation): chính sách dựa trên tag của Lake Formation được thực thi trong Athena sẽ được QuickSight tôn trọng khi truy vấn qua Athena — cung cấp lớp kiểm soát truy cập nhất quán được quản lý tập trung.

##### Nhúng QuickSight Dashboard

Dashboard nhúng đã đăng ký cho phép QuickSight được hiển thị bên trong ứng dụng web hoặc di động bên ngoài, giúp nhóm sản phẩm cung cấp analytics trực tiếp trong trải nghiệm người dùng của họ:

**Embedding SDK (JavaScript):**

```javascript
import { createEmbeddingContext } from 'amazon-quicksight-embedding-sdk';

const embeddingContext = await createEmbeddingContext();

const frameOptions = {
  url: embeddedDashboardUrl,   // lấy từ GetDashboardEmbedUrl API
  container: '#dashboard-container',
  height: '700px',
  width: '100%',
};

const contentOptions = {
  parameters: [
    { Name: 'region', Values: ['APAC'] },  // override theo phiên
  ],
  locale: 'vi-VN',
  toolbarOptions: { export: true, undoRedo: false, reset: true },
};

const dashboard = await embeddingContext.embedDashboard(frameOptions, contentOptions);
```

**Chế độ embedding:**
- **Xác thực (theo người dùng)**: reader đăng nhập qua identity provider → QuickSight ánh xạ đến người dùng đã đăng ký → RLS áp dụng tự động.
- **Ẩn danh (1-click)**: URL ẩn danh được backend tạo theo phiên; parameter điều khiển phạm vi dữ liệu; không cần tài khoản QuickSight.

**API GenerateEmbedUrlForAnonymousUser**: backend (Lambda hoặc server) gọi API này với session tag và giá trị parameter được phép, nhận URL embed có thời hạn và trả về cho frontend — giữ AWS credential hoàn toàn phía server.

##### Mô hình Chi phí Serverless Analytics

Hiểu mô hình chi phí QuickSight rất quan trọng khi thiết kế giải pháp BI tiết kiệm chi phí:

| Thành phần | Mô hình tính phí |
|---|---|
| **Author** | $18/tháng (standard) hoặc $54/tháng (enterprise) mỗi author |
| **Reader** | $0.30/phiên (tối đa $5/tháng/reader) |
| **SPICE** | $0.25/GB/tháng — 1 GB đầu miễn phí mỗi author |
| **Truy vấn Athena** | $5/TB quét (độc lập với QuickSight) |
| **Embedding** | Phiên reader tính theo phiên 30 phút |

Với dashboard có nhiều reader, **cơ chế giới hạn Reader** ($5/tháng tối đa mỗi reader) giúp QuickSight rẻ hơn đáng kể so với các công cụ BI truyền thống tính phí theo suất người dùng được đặt tên.

##### Thực hành

- Kết nối dataset QuickSight với Data Lake dựa trên Athena từ Tuần 9; cấu hình làm mới SPICE theo giờ và đo benchmark thời gian phản hồi Direct Query so với SPICE trên dataset 50 triệu hàng.
- Xây dựng dashboard điều hành 4 sheet: "Tổng quan", "Doanh số Theo Vùng", "Hiệu suất Sản phẩm" và "Phân tích Xu hướng" — sử dụng 12 loại visual với conditional formatting xuyên suốt.
- Triển khai filter tham số hóa cho Vùng, Khoảng Thời gian và Danh mục Sản phẩm với phụ thuộc filter liên kết.
- Bật Anomaly Detection trên chỉ số doanh thu hàng tuần và kích hoạt cảnh báo thử nghiệm; xác minh mô hình ML phát hiện bất thường được cài sẵn với độ tin cậy 94%.
- Thiết lập permission dataset RLS gán 5 người dùng thử nghiệm cho các tổ hợp vùng/bộ phận khác nhau; xác minh dashboard hiển thị số hàng khác nhau theo từng đăng nhập.
- Nhúng dashboard vào ứng dụng Next.js bằng Embedding SDK với anonymous session embedding và per-session parameter override cho phạm vi theo bộ phận.

##### Sự kiện: Cloud Mastery 2026 — #1 AI From Scratch

Tham dự phiên đầu tiên của Cloud Mastery 2026 với chủ đề xây dựng AI từ nền tảng, bao gồm ba phiên trọng tâm: *Building AI Agent with Strands*, *Automated Prompt Engineering: Enhancing LLM Output Quality* và *AI-Powered Projects*. Các phiên mang lại góc nhìn thực tế từ đầu đến cuối — từ thiết kế luồng agentic với Strands đến cải thiện có hệ thống chất lượng phản hồi LLM bằng kỹ thuật tối ưu hóa prompt. Phiên cuối về AI-Powered Projects cho thấy cách các công nghệ này hội tụ trong các ứng dụng thực tế, rút ngắn khoảng cách giữa prototype và hệ thống AI sẵn sàng cho sản xuất.
