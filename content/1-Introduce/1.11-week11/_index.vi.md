---
title: "Tuần 11 Worklog"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu Tuần 11

- Hiểu các khái niệm cơ bản về Trí tuệ Nhân tạo và Machine Learning và mối liên hệ với các dịch vụ AWS.
- Phân biệt AI, ML, Deep Learning và Generative AI, và biết từng loại phù hợp với vị trí nào trong hệ sinh thái AWS.
- Nắm vững luồng công việc ML end-to-end: chuẩn bị dữ liệu, huấn luyện mô hình, đánh giá, triển khai và giám sát.
- Thành thạo Amazon SageMaker như nền tảng trung tâm để xây dựng, huấn luyện và triển khai mô hình ML ở quy mô lớn.
- Khám phá các dịch vụ AI của AWS (Rekognition, Textract, Comprehend, Polly, Transcribe, Translate) và khi nào nên dùng chúng so với xây mô hình tùy chỉnh.
- Làm quen với Amazon Bedrock để truy cập foundation model và xây dựng ứng dụng Generative AI trên AWS.

---

### Kế hoạch công việc trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|------|-----------|--------------|-----------------|---------------------|
| 1 | Nghiên cứu căn bản AI/ML: supervised vs. unsupervised vs. reinforcement learning, các thuật toán chính (hồi quy tuyến tính, cây quyết định, mạng nơ-ron) và quy trình xác định bài toán ML. | 03/16/2026 | 03/16/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Khám phá luồng công việc ML end-to-end: thu thập và gắn nhãn dữ liệu, feature engineering, phân chia train/validation/test, đánh đổi bias-variance, các chỉ số đánh giá mô hình (accuracy, precision, recall, F1, AUC-ROC). | 03/17/2026 | 03/17/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Tìm hiểu các thành phần cốt lõi của Amazon SageMaker: SageMaker Studio IDE, Data Wrangler, Experiments, Training Job, Hyperparameter Tuning và Model Registry. | 03/18/2026 | 03/18/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Nghiên cứu các tùy chọn triển khai SageMaker: real-time endpoint, serverless inference, asynchronous inference, batch transform; khám phá auto-scaling, A/B testing và Model Monitor. | 03/19/2026 | 03/19/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Khảo sát AWS AI Services: Rekognition (ảnh/video), Textract (OCR tài liệu), Comprehend (NLP), Transcribe (speech-to-text), Polly (text-to-speech), Translate và Forecast — hiểu khi nào AI API dựng sẵn tốt hơn mô hình tùy chỉnh. | 03/20/2026 | 03/20/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | **Sự kiện:** FCAJ Community Day.<br> Khám phá Amazon Bedrock: foundation model (Claude, Titan, Llama, Mistral, Stable Diffusion), Retrieval-Augmented Generation (RAG) với Knowledge Base, Agent điều phối và Guardrail cho AI có trách nhiệm. | 03/21/2026 | 03/21/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Thành quả Tuần 11

##### Căn bản AI/ML — Khái niệm và Toàn cảnh

**Phân cấp AI:**

```
Trí tuệ Nhân tạo (AI)
└── Machine Learning (ML) — học mẫu từ dữ liệu
    └── Deep Learning (DL) — mạng nơ-ron nhiều lớp
        └── Generative AI (GenAI) — tạo nội dung mới (văn bản, ảnh, code, âm thanh)
```

**Các loại Machine Learning:**

| Loại | Cách học | Ví dụ use case trên AWS |
|---|---|---|
| **Supervised** | Dữ liệu có nhãn (đầu vào → đầu ra kỳ vọng) | Phát hiện gian lận, phân loại ảnh, dự báo doanh thu |
| **Unsupervised** | Dữ liệu không nhãn — khám phá mẫu ẩn | Phân khúc khách hàng, phát hiện bất thường, topic modeling |
| **Reinforcement** | Agent học qua tương tác môi trường và nhận phần thưởng | Robot, game, tối ưu đề xuất |
| **Self-supervised** | Nhãn được suy ra từ chính dữ liệu | Foundation model như GPT, BERT, Stable Diffusion |

**Stack AI/ML của AWS (ba lớp):**

| Lớp | Mô tả | Dịch vụ AWS |
|---|---|---|
| **AI Services** | API dựng sẵn, không cần chuyên môn ML | Rekognition, Textract, Comprehend, Forecast |
| **ML Platform** | Xây dựng, huấn luyện và triển khai mô hình tùy chỉnh | Amazon SageMaker |
| **ML Frameworks** | Framework mã nguồn mở trên hạ tầng AWS | TensorFlow, PyTorch, MXNet trên EC2/SageMaker |

##### Luồng Công việc ML — End-to-End

Xây dựng hệ thống ML production là toàn bộ vòng đời, không chỉ là huấn luyện mô hình:

**1. Xác định Bài toán:**
- Định nghĩa mục tiêu ML: đây là bài toán phân loại, hồi quy, phân cụm hay xếp hạng?
- Xác định chỉ số thành công: accuracy, F1, RMSE, NDCG?
- Định nghĩa baseline (mô hình dựa trên quy tắc đơn giản hoặc ngẫu nhiên để vượt qua).

**2. Thu thập và Gắn nhãn Dữ liệu:**
- Thu thập dữ liệu huấn luyện từ database, S3, API hoặc web scraping.
- Gắn nhãn bằng **Amazon SageMaker Ground Truth** (gắn nhãn có quản lý với lực lượng lao động con người hoặc gắn nhãn tự động bằng active learning).
- Đảm bảo đủ khối lượng dữ liệu và cân bằng lớp — xử lý mất cân bằng lớp qua oversampling (SMOTE) hoặc cost-sensitive learning.

**3. Feature Engineering:**
- **Feature số**: chuẩn hóa (min-max), chuẩn hóa z-score, biến đổi log cho phân phối lệch.
- **Feature phân loại**: one-hot encoding, target encoding, embedding cho trường cardinality cao.
- **Feature chuỗi thời gian**: lag feature, thống kê cửa sổ trượt, biến đổi Fourier cho tính mùa vụ.
- **Feature văn bản**: TF-IDF, word embedding (Word2Vec, GloVe) hoặc contextual embedding dựa trên BERT.
- Công cụ: **SageMaker Data Wrangler** — hơn 300 phép biến đổi tích hợp, báo cáo chất lượng dữ liệu và export trực tiếp vào SageMaker Pipelines.

**4. Huấn luyện Mô hình:**
- Phân chia dữ liệu: thường 70% train / 15% validation / 15% test.
- Giám sát **đánh đổi bias-variance**: underfitting (bias cao) → tăng độ phức tạp mô hình; overfitting (variance cao) → regularization (L1/L2), dropout hoặc thêm dữ liệu huấn luyện.
- Dùng **SageMaker Experiments** để theo dõi hyperparameter, chỉ số và artifact qua các lần chạy huấn luyện.

**5. Đánh giá Mô hình:**

| Chỉ số | Dùng khi |
|---|---|
| Accuracy | Phân phối lớp cân bằng |
| Precision / Recall | Lớp mất cân bằng (ví dụ: phát hiện gian lận) |
| F1 Score | Trung bình điều hòa của precision và recall |
| AUC-ROC | So sánh bộ phân loại qua các ngưỡng |
| RMSE / MAE | Bài toán hồi quy |
| NDCG / MAP | Hệ thống xếp hạng và đề xuất |

**6. Triển khai và Giám sát:**
- Triển khai mô hình; giám sát **data drift** và **concept drift** bằng SageMaker Model Monitor.
- Huấn luyện lại theo lịch hoặc kích hoạt retraining khi cảnh báo monitoring nổ.

##### Amazon SageMaker — Nền tảng ML End-to-End

SageMaker là nền tảng được quản lý hoàn toàn của AWS bao phủ mọi giai đoạn vòng đời ML:

**SageMaker Studio:**
- IDE dựa trên web được thiết kế riêng cho ML — notebook, khám phá dữ liệu, theo dõi experiment, đăng ký mô hình và tạo pipeline tất cả trong một môi trường.
- Chạy trên compute được quản lý; không cần cấp phát hạ tầng.

**SageMaker Data Wrangler:**
- Import dữ liệu từ S3, Athena, Redshift hoặc nguồn SaaS.
- Áp dụng hơn 300 phép biến đổi tích hợp trực quan và export luồng dữ liệu trực tiếp vào SageMaker Pipeline như một processing step.
- Tự động tạo báo cáo chất lượng dữ liệu (mất cân bằng lớp, giá trị thiếu, phân tích tương quan).

**SageMaker Training:**
- Huấn luyện phân tán có quản lý bằng thuật toán tích hợp (XGBoost, Linear Learner, k-NN, DeepAR, BlazingText) hoặc container tùy chỉnh (TensorFlow, PyTorch, MXNet).
- **Spot Training**: dùng EC2 Spot Instance cho training job — giảm đến 90% chi phí so với On-Demand, tự động checkpoint và tiếp tục khi bị gián đoạn.
- **SageMaker Distributed Training**: model parallelism (chia mô hình lớn qua nhiều GPU) và data parallelism (chia dataset qua các worker) cho khối lượng deep learning khổng lồ.

**SageMaker Hyperparameter Tuning (HPO):**
- Tìm kiếm hyperparameter tự động bằng Bayesian optimization, random search hoặc Hyperband.
- Chạy N training job song song, tự động xác định cấu hình tối đa hóa chỉ số mục tiêu.

**SageMaker Pipelines:**
- Định nghĩa luồng công việc ML dưới dạng DAG (đồ thị tuần hoàn có hướng) với các bước: Processing → Training → Evaluation → Conditional → RegisterModel → DeployModel.
- Tích hợp với **SageMaker Model Registry** để versioning mô hình, luồng phê duyệt và quản lý metadata.
- Hỗ trợ CI/CD cho ML (MLOps) qua tích hợp với CodePipeline và EventBridge.

##### Triển khai SageMaker — Các Tùy chọn Inference

| Chế độ | Độ trễ | Thông lượng | Phù hợp nhất |
|---|---|---|---|
| **Real-time Endpoint** | Mili giây | Liên tục | API người dùng, scoring thời gian thực |
| **Serverless Inference** | ~100ms (cold start) | Đột biến | Traffic không thường xuyên, ứng dụng nhạy chi phí |
| **Asynchronous Inference** | Giây–phút | Payload lớn | Dự đoán chạy lâu (video, tài liệu lớn) |
| **Batch Transform** | Phút–giờ | Dataset khổng lồ | Scoring offline không cần endpoint bền vững |

**Multi-variant Endpoint (A/B Testing):**
- Phân phối traffic qua nhiều biến thể mô hình với tỷ lệ có thể cấu hình (ví dụ: 90% ModelV1 / 10% ModelV2).
- So sánh chỉ số production trước khi promote hoàn toàn phiên bản mô hình mới.

**SageMaker Model Monitor:**
- Liên tục giám sát real-time endpoint về data quality drift, model quality drift, bias drift và feature attribution drift.
- Kích hoạt cảnh báo CloudWatch khi thuộc tính thống kê của đầu vào/đầu ra inference lệch khỏi baseline huấn luyện quá ngưỡng cấu hình.

##### AWS AI Services — API Thông minh Dựng sẵn

Với nhiều use case, xây mô hình ML tùy chỉnh là không cần thiết. AWS AI Services cung cấp API thông minh sẵn dùng không yêu cầu chuyên môn ML:

| Dịch vụ | Khả năng | Use case phổ biến |
|---|---|---|
| **Amazon Rekognition** | Phân tích ảnh và video | Nhận diện/so sánh khuôn mặt, gắn nhãn vật thể, kiểm duyệt nội dung, phát hiện PPE |
| **Amazon Textract** | Trích xuất văn bản và cấu trúc tài liệu | Xử lý hóa đơn, số hóa hồ sơ y tế, trích xuất dữ liệu form |
| **Amazon Comprehend** | Xử lý ngôn ngữ tự nhiên (NLP) | Phân tích cảm xúc, nhận diện thực thể, topic modeling, phát hiện PII |
| **Amazon Transcribe** | Nhận diện giọng nói tự động (ASR) | Ghi chép cuộc họp, phân tích call center, tạo phụ đề |
| **Amazon Polly** | Chuyển văn bản thành giọng nói | Ứng dụng giọng nói, trợ năng, tạo audio sách |
| **Amazon Translate** | Dịch máy neural | Bản địa hóa nội dung thời gian thực, hỗ trợ chat đa ngôn ngữ |
| **Amazon Forecast** | Dự báo chuỗi thời gian | Dự báo nhu cầu, tối ưu tồn kho, lập kế hoạch năng lực |
| **Amazon Personalize** | Đề xuất dựa trên ML | Đề xuất sản phẩm, xếp hạng tìm kiếm cá nhân hóa |

**Khi nào dùng AI Services so với mô hình tùy chỉnh:**

- **Dùng AI Services** khi: tác vụ được định nghĩa rõ ràng và được API hiện có bao phủ, tốc độ ra thị trường là ưu tiên, dữ liệu huấn luyện có nhãn không có sẵn hoặc tốn kém.
- **Dùng mô hình SageMaker tùy chỉnh** khi: mẫu đặc thù domain không được API chung bao phủ, dữ liệu huấn luyện độc quyền tạo ra lợi thế cạnh tranh, cần kiểm soát chi tiết hành vi mô hình.

##### Amazon Bedrock — Foundation Model và Generative AI

Amazon Bedrock là dịch vụ được quản lý hoàn toàn cung cấp các foundation model (FM) hàng đầu qua API — không cần quản lý bất kỳ hạ tầng mô hình nào:

**Foundation Model Có sẵn:**

| Nhà cung cấp | Mô hình | Điểm mạnh |
|---|---|---|
| **Anthropic** | Claude 3 (Haiku, Sonnet, Opus) | Lập luận phức tạp, ngữ cảnh dài, an toàn |
| **Amazon** | Titan Text, Titan Embeddings, Titan Image | Chi phí hiệu quả, tích hợp AWS gốc |
| **Meta** | Llama 3 | Mã nguồn mở, sinh code, đa ngôn ngữ |
| **Mistral** | Mistral 7B, Mixtral 8x7B | Lập luận hiệu quả, độ trễ thấp |
| **Stability AI** | Stable Diffusion XL | Tạo và chỉnh sửa ảnh |
| **Cohere** | Command, Embed | Văn bản doanh nghiệp, tìm kiếm ngữ nghĩa |

**Khả năng Chính của Bedrock:**

**Knowledge Base (Retrieval-Augmented Generation — RAG):**
- Kết nối FM với knowledge base độc quyền lưu trong S3.
- Bedrock tự động phân đoạn tài liệu, tạo embedding và lưu vào vector store tích hợp (Amazon OpenSearch Serverless hoặc Aurora pgvector).
- Lúc truy vấn, các đoạn tài liệu liên quan được lấy ra và đưa vào prompt FM làm ngữ cảnh — cho phép phản hồi chính xác có căn cứ mà không bị hallucination.

**Bedrock Agent:**
- Tạo agent tự trị nhiều bước có thể lập luận, lên kế hoạch, gọi API (qua Lambda action group) và hoàn thành tác vụ phức tạp.
- Agent điều phối: Lập luận → Gọi công cụ → Quan sát → Bước tiếp theo (mô hình ReAct).
- Ví dụ: agent dịch vụ khách hàng tra cứu trạng thái đơn hàng (gọi API), xử lý yêu cầu hoàn tiền (gọi API) và soạn phản hồi bằng ngôn ngữ của người dùng (FM).

**Bedrock Guardrail:**
- Áp dụng chính sách lọc nội dung: chặn chủ đề có hại, xóa PII, lọc ngôn từ tục tĩu, kiểm tra grounding (phát hiện hallucination).
- Thực thi hạn chế chủ đề (ví dụ: ngăn mô hình thảo luận về sản phẩm đối thủ).
- Áp dụng nhất quán trên tất cả tương tác FM trong tài khoản.

**Fine-tuning và Continued Pre-training:**
- Upload dữ liệu huấn luyện đặc thù domain để fine-tune các FM được hỗ trợ trên Bedrock mà không cần quản lý hạ tầng.
- Hữu ích để điều chỉnh mô hình đa dụng sang từ vựng chuyên biệt (y tế, pháp lý, kỹ thuật).

##### Thực hành

- Huấn luyện mô hình phân loại nhị phân bằng thuật toán XGBoost tích hợp của SageMaker trên dataset CSV từ S3; theo dõi experiment trong SageMaker Experiments và so sánh 5 cấu hình hyperparameter.
- Dùng SageMaker Data Wrangler để lập hồ sơ dataset bảng thô — xác định 3 feature có >20% giá trị thiếu và áp dụng phép biến đổi median imputation và one-hot encoding.
- Triển khai mô hình XGBoost đã huấn luyện vào real-time SageMaker endpoint và kiểm thử dự đoán qua Boto3 API `invoke_endpoint`.
- Gọi Amazon Textract trên file PDF hóa đơn mẫu: trích xuất các cặp khóa-giá trị (tên nhà cung cấp, tổng tiền, ngày) và kiểm chứng độ chính xác đầu ra.
- Kiểm thử Amazon Comprehend trên văn bản đánh giá sản phẩm: trích xuất điểm cảm xúc, thực thể được đề cập và phát hiện PII.
- Xây dựng ứng dụng RAG bằng Amazon Bedrock Knowledge Base: upload PDF tài liệu sản phẩm, cấu hình vector store OpenSearch Serverless và truy vấn Claude 3 Sonnet với phản hồi có căn cứ từ tài liệu.

##### Sự kiện: FCAJ Community Day

Tham dự một sự kiện toàn diện tập trung vào sự giao thoa giữa Platform Engineering, DevOps và Generative AI. Các phiên thảo luận đã khám phá vai trò của Platform Engineering trong hệ sinh thái đám mây hiện đại, giới thiệu GenAIOps qua phân tích thực tế với Bedrock Agent và EKS, cũng như tìm hiểu quy trình triển khai mã nguồn trong kỷ nguyên AI agentic. Sự kiện cũng trình bày các kiến trúc nâng cao cho Production-Grade Multimodal GenAI bằng Nova Embeddings và GraphRAG, khép lại với phiên hướng dẫn thực tế về cách xây dựng nền tảng biên (edge) an toàn, hiệu năng cao với Amazon CloudFront.
