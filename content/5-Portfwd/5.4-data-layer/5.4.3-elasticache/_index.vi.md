---
title: "Thiết lập AWS Bedrock Knowledge Base"
weight: 3
chapter: false
pre: " <b> 4.4.3. </b> "
---

1. **Tải dữ liệu (PDF)** lên bucket `rag-data`.
2. Mở bảng điều khiển **Amazon Bedrock console**.

<img src="/images/Picture1.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

3. **Chọn Knowledge Base:** Nhấp vào nút *Create Knowledge Base*.

<img src="/images/Picture2.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

4. **Cấu hình chung (General Configuration):**

<img src="/images/Picture3.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

   - **Tên nguồn dữ liệu (Data source name):** Nhập `RAG-data`.
   - **S3 URL:** Chọn tệp đã tải lên bucket ở Bước 1.
5. **Chiến lược phân tích (Parsing strategy):** Chọn *Amazon Bedrock default parser* (Trình phân tích mặc định).
6. **Chiến lược phân đoạn (Chunking strategy):** Chọn *Semantic chunking* (Phân đoạn theo ngữ nghĩa).
7. **Chọn mô hình nhúng (Embedding model):** Chọn mô hình mà bạn mong muốn.

<img src="/images/Picture4.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

8. **Lưu trữ vector (Vector store):**
   - Chọn *Quick create a new vector store* (Tạo nhanh lưu trữ vector mới).
   - **Loại lưu trữ vector:** Chọn *Amazon S3 vector*.
9. Nhấp vào nút **'Create knowledge base'** (Tạo cơ sở tri thức).