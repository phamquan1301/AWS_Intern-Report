---
title: "Tuần 12 Worklog"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Mục tiêu Tuần 12

- Vượt ra khỏi phần căn bản Tuần 11 và thành thạo các khả năng nâng cao của Amazon SageMaker cho hệ thống ML production.
- Hiểu cách thiết kế và chạy distributed training job có thể mở rộng cho các mô hình deep learning lớn.
- Nắm vững các mẫu SageMaker MLOps: Pipeline, Model Registry, tích hợp CI/CD và trigger retraining tự động.
- Thành thạo chiến lược endpoint scaling: chính sách auto-scaling, multi-model endpoint và inference recommender.
- Hiểu SageMaker Clarify cho phát hiện bias và giải thích mô hình (SHAP value).
- Học cách tối ưu chi phí inference bằng SageMaker Neo compiler và Graviton instance.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Nghiên cứu chuyên sâu distributed training trong SageMaker: data parallelism với SMDDP, model parallelism với SMP, và huấn luyện trên cluster EC2 multi-GPU/multi-node. | 03/23/2026 | 03/23/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Xây dựng SageMaker Pipeline hoàn chỉnh: ProcessingStep → TrainingStep → EvaluationStep → ConditionStep → RegisterModelStep → DeployStep với cổng phê duyệt và theo dõi metadata lần chạy. | 03/24/2026 | 03/24/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Cấu hình SageMaker Model Registry: versioning mô hình, luồng phê duyệt thủ công và tự động, chia sẻ mô hình cross-account và tích hợp EventBridge để tự động kích hoạt pipeline triển khai. | 03/25/2026 | 03/25/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Triển khai chiến lược endpoint scaling: chính sách Application Auto Scaling, multi-model endpoint (MME) cho hosting mô hình tiết kiệm chi phí và SageMaker Inference Recommender. | 03/26/2026 | 03/26/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Khám phá SageMaker Clarify: phát hiện bias trước huấn luyện trên dataset, bias metric sau huấn luyện (DPL, DImpact), feature importance dựa trên SHAP và tích hợp với Model Monitor để theo dõi explainability drift. | 03/27/2026 | 03/27/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Khảo sát tối ưu chi phí inference: SageMaker Neo compilation và tối ưu phần cứng, Graviton instance cho inference, và so sánh cost-per-inference qua các cấu hình triển khai. | 03/28/2026 | 03/28/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 12

##### Distributed Training Quy mô Lớn

Huấn luyện mô hình lớn trên một instance duy nhất nhanh chóng trở thành nút thắt. SageMaker cung cấp hai chiến lược song song bổ trợ nhau:

**Data Parallelism (SageMaker Distributed Data Parallel — SMDDP):**

Mỗi worker node giữ một bản sao đầy đủ của mô hình nhưng xử lý một tập con (shard) khác nhau của dữ liệu huấn luyện. Sau mỗi backward pass, gradient được tổng hợp qua tất cả worker bằng **AllReduce** trước bước optimizer:

```
Node 1: Batch A → Forward → Backward → Gradient A ─┐
Node 2: Batch B → Forward → Backward → Gradient B ─┤─► AllReduce ─► Gradient Trung bình ─► Cập nhật Trọng số
Node 3: Batch C → Forward → Backward → Gradient C ─┘
```

- SMDDP dùng thuật toán AllReduce tối ưu (vượt trội Horovod 25% trên fabric mạng AWS).
- Phù hợp nhất: mô hình vừa với bộ nhớ GPU đơn, nhưng dữ liệu huấn luyện quá lớn cho một máy.
- Kích hoạt: thêm `distribution={'smdistributed': {'dataparallel': {'enabled': True}}}` vào SageMaker Estimator.

**Model Parallelism (SageMaker Model Parallel — SMP):**

Chia tách chính mô hình qua nhiều GPU — cần thiết cho mô hình quá lớn để vừa trong một GPU đơn (ví dụ: LLM với hàng tỷ tham số):

| Chiến lược | Mô tả |
|---|---|
| **Tensor Parallelism** | Chia ma trận trọng số layer riêng lẻ qua GPU |
| **Pipeline Parallelism** | Chia layer mô hình thành stage; mỗi GPU xử lý một stage |
| **Optimized Sharding (ZeRO)** | Chia optimizer state, gradient và tham số qua GPU |

- SMP v2 hỗ trợ Hugging Face Transformers và PyTorch FSDP một cách tự nhiên.
- Cho phép huấn luyện mô hình 70B+ tham số trên instance `p4d.24xlarge` (8x A100 GPU) hoặc `p5.48xlarge` (8x H100 GPU).

##### SageMaker Pipelines — Tự động hóa MLOps

SageMaker Pipeline production là DAG các bước liên kết tự động hóa toàn bộ luồng công việc ML từ dữ liệu thô đến mô hình đã triển khai:

```
S3 Dữ liệu Thô
    │
    ▼
ProcessingStep (Data Wrangler / SKLearn script)
    │  Đầu ra: train.csv, validation.csv, test.csv
    ▼
TrainingStep (XGBoost / PyTorch Training Job)
    │  Đầu ra: model.tar.gz
    ▼
EvaluationStep (Processing Job: tính chỉ số trên test set)
    │  Đầu ra: evaluation.json { "f1": 0.93, "auc": 0.97 }
    ▼
ConditionStep (nếu F1 >= 0.90)
    ├── [Đúng]  → RegisterModelStep (Model Registry — PendingApproval)
    │               └── [Được duyệt] → DeployStep (SageMaker Endpoint)
    └── [Sai]   → FailStep (thông báo nhóm qua SNS)
```

**Thuộc tính Thực thi Pipeline:**
- Pipeline được **kiểm soát phiên bản** — mỗi lần chạy tạo bản ghi thực thi bất biến với metadata (tham số, chỉ số, lineage artifact).
- **Caching**: các bước có artifact đầu vào không thay đổi bị bỏ qua khi chạy lại, tăng tốc đáng kể thử nghiệm lặp lại.
- **Tham số**: có thể cấu hình lúc thực thi (`TrainingInstanceType`, `ModelApprovalStatus`, `F1Threshold`) mà không sửa định nghĩa pipeline.
- **Tích hợp CI/CD**: pipeline có thể được kích hoạt bởi CodePipeline khi commit repository hoặc qua lịch EventBridge cho retraining tự động.

##### SageMaker Model Registry — Quản trị Mô hình

Model Registry cung cấp catalog tập trung của tất cả phiên bản mô hình đã huấn luyện, cho phép quản trị và triển khai có kiểm soát:

**Model Package Group:**
- Mỗi model package group tương ứng với một use case (ví dụ: `FraudDetectionModel`, `ChurnPredictionModel`).
- Nhiều phiên bản mô hình nằm trong cùng một group, mỗi phiên bản có metadata, chỉ số và tham chiếu artifact.

**Luồng Phê duyệt:**
- Phiên bản mô hình mới đăng ký với trạng thái `PendingManualApproval`.
- Người đánh giá (data scientist, ML engineer) đánh giá chỉ số trong Model Registry UI và đặt trạng thái thành `Approved` hoặc `Rejected`.
- **Phê duyệt tự động**: Lambda function kích hoạt bởi EventBridge rule đánh giá chỉ số mô hình theo chương trình và tự phê duyệt mô hình đáp ứng ngưỡng chất lượng (ví dụ: F1 > 0.90).

**Triển khai Cross-Account:**

```
Account A (Dev)               Account B (Staging)        Account C (Prod)
  ↓ Đăng ký mô hình           ↓ Lấy mô hình đã duyệt    ↓ Lấy mô hình đã duyệt
  SageMaker Registry  →→→ Cross-account IAM role  →→→ SageMaker Endpoint
```

Model Registry hỗ trợ chia sẻ mô hình cross-account qua resource-based policy — mẫu quan trọng cho nhóm ML platform quản lý mô hình qua ranh giới đơn vị tổ chức.

##### Chiến lược Endpoint Scaling

**Application Auto Scaling:**

SageMaker endpoint tích hợp tự nhiên với Application Auto Scaling:

- **Target Tracking**: duy trì `SageMakerVariantInvocationsPerInstance` ở giá trị mục tiêu (ví dụ: 1000 lời gọi/phút/instance). Auto Scaling tự động thêm hoặc xóa instance.
- **Step Scaling**: định nghĩa các bước scaling rời rạc kích hoạt khi chỉ số vượt ngưỡng cụ thể (scale-out tích cực, scale-in thận trọng).
- **Scheduled Scaling**: pre-scale cho spike traffic đã biết (ví dụ: scale lên 10 instance lúc 08:00 thứ Hai, giảm về 2 lúc 22:00).

**Multi-Model Endpoint (MME):**

Host hàng nghìn mô hình trên một endpoint duy nhất, giảm đáng kể chi phí hosting cho kịch bản multi-tenant hoặc mô hình theo người dùng:

- Mô hình lưu trong S3 và được nạp vào bộ nhớ container theo nhu cầu (loại bỏ LRU khi bộ nhớ đầy).
- Một endpoint `ml.m5.xlarge` đơn có thể phục vụ hàng nghìn mô hình không đồng thời hot.
- Tính phí: trả cho một endpoint instance thay vì N endpoint riêng lẻ.
- Framework được hỗ trợ: MXNet, PyTorch, TensorFlow, scikit-learn, XGBoost (qua container SageMaker dựng sẵn).

**SageMaker Inference Recommender:**
- Công cụ benchmark tự động: kiểm thử mô hình trên tập hợp loại instance và batch size có thể cấu hình.
- Đầu ra là bảng khuyến nghị xếp hạng: `(instance_type, cost_per_hour, latency_p99, throughput)`.
- Loại bỏ đoán mò trong lựa chọn instance — đặc biệt có giá trị khi di chuyển từ on-premises sang cloud inference.

##### SageMaker Clarify — Phát hiện Bias và Giải thích Mô hình

AI có trách nhiệm đòi hỏi mô hình không chỉ chính xác mà còn công bằng và có thể giải thích:

**Phát hiện Bias Trước Huấn luyện:**

Clarify phân tích dataset huấn luyện trước khi huấn luyện mô hình để phát hiện bias thống kê trong bản thân dữ liệu:

| Chỉ số | Đo lường |
|---|---|
| **Class Imbalance (CI)** | Liệu phân phối nhãn có cân bằng qua các nhóm nhạy cảm không |
| **Difference in Proportions of Labels (DPL)** | Mức độ tỷ lệ nhãn khác nhau giữa nhóm đặc quyền và không đặc quyền |
| **Kullback-Leibler Divergence (KL)** | Khoảng cách thống kê giữa phân phối nhãn qua các nhóm |

**Bias Metric Sau Huấn luyện:**

Sau khi huấn luyện, Clarify đánh giá dự đoán của mô hình để tìm bias:

| Chỉ số | Đo lường |
|---|---|
| **DPPL** | Sự chênh lệch tỷ lệ dự đoán dương giữa các nhóm |
| **DImpact** | Bất bình đẳng trong kết quả dương so với nhóm tham chiếu |
| **Accuracy Difference** | Liệu mô hình có chính xác như nhau qua các nhóm nhân khẩu học không |

**Giải thích Mô hình (SHAP):**

SageMaker Clarify dùng **SHAP (SHapley Additive exPlanations)** để giải thích dự đoán cá nhân:
- Mỗi feature nhận SHAP value chỉ ra đóng góp (dương hoặc âm) của nó vào một dự đoán cụ thể.
- Biểu đồ thác nước SHAP hiển thị: `E[f(x)] + đóng_góp_feature_1 + đóng_góp_feature_2 + ... = f(x)`.
- Feature importance toàn cục: tổng hợp SHAP value qua test set để hiểu feature nào tổng thể điều khiển mô hình.

**Tích hợp với Model Monitor:**
- Clarify có thể chạy như scheduled Model Monitor job trên live endpoint, phát hiện liệu tầm quan trọng tương đối của feature (explainability) có drift theo thời gian không — dấu hiệu cảnh báo sớm về sự lỗi thời của mô hình.

##### Tối ưu Chi phí Inference

**SageMaker Neo — Model Compilation:**

Neo biên dịch mô hình đã huấn luyện cho phần cứng mục tiêu cụ thể (kiến trúc CPU/GPU) để loại bỏ overhead framework:

- Chuyển đổi mô hình từ TensorFlow, PyTorch, MXNet, XGBoost, sklearn thành binary tối ưu bằng LLVM-based compilation.
- Tăng tốc điển hình: **độ trễ thấp hơn 25–40%** và **throughput đến gấp 2x** so với mô hình chưa biên dịch.
- Phần cứng mục tiêu: `ml_c5`, `ml_m5`, `ml_g4dn`, AWS Inferentia (INF1/INF2), ARM Graviton.

**AWS Inferentia (Instance Inf1 / Inf2):**
- Silicon tùy chỉnh AWS được thiết kế riêng cho ML inference.
- `inf2.xlarge` cung cấp 2x AWS Neuron Core; chi phí khoảng 40% thấp hơn mỗi inference so với instance GPU `g4dn.xlarge` tương đương cho mô hình Transformer.
- Yêu cầu biên dịch mô hình với **AWS Neuron SDK** (wrapper PyTorch Neuron / TensorFlow Neuron).

**Inference Dựa trên Graviton:**
- Instance `ml.m7g`, `ml.c7g` dùng bộ vi xử lý AWS Graviton3.
- Tốt hơn đến 40% price-performance cho mô hình inference CPU-bound (XGBoost, sklearn, mạng nơ-ron nhỏ) so với x86 tương đương.

**So sánh Cost-Per-Inference (ví dụ, mô hình XGBoost):**

| Cấu hình | Instance | Chi phí/giờ | Throughput (req/s) | Chi phí 1M request |
|---|---|---|---|---|
| Tiêu chuẩn | ml.m5.xlarge | $0.23 | 850 | $0.075 |
| Neo-compiled | ml.m5.xlarge | $0.23 | 1.190 | $0.054 |
| Graviton | ml.m7g.xlarge | $0.17 | 1.050 | $0.045 |
| Multi-Model | ml.m5.xlarge (dùng chung) | $0.023* | 850 | $0.0075* |

*Chi phí multi-model phân bổ qua 10 mô hình đang host đồng thời.

##### Thực hành

- Khởi chạy PyTorch training job multi-node trên 2x `ml.p3.8xlarge` instance bằng SageMaker Distributed Data Parallel; giám sát GPU utilization và AllReduce throughput qua CloudWatch.
- Xây dựng SageMaker Pipeline 6 bước với ConditionStep chặn đăng ký mô hình khi F1 ≥ 0.90; kích hoạt pipeline từ CodePipeline khi simulate commit repository code mô hình.
- Đăng ký 3 phiên bản mô hình vào Model Registry, cấu hình phê duyệt tự động qua Lambda evaluator function và xác minh EventBridge kích hoạt pipeline triển khai khi được phê duyệt.
- Triển khai Multi-Model Endpoint hosting 50 mô hình XGBoost riêng lẻ (một mô hình mỗi phân khúc khách hàng); đo độ trễ cold-load so với warm-cache mỗi lần gọi mô hình.
- Chạy SageMaker Clarify trên mô hình phân loại nhị phân: xác định bias DPL 12% trên feature `age_group` và tạo báo cáo SHAP global importance — 3 feature hàng đầu chiếm 78% variance dự đoán.
- Biên dịch mô hình XGBoost với SageMaker Neo nhắm mục tiêu `ml_m5`; benchmark giảm 38% độ trễ và cải thiện 40% throughput so với phiên bản chưa biên dịch.
