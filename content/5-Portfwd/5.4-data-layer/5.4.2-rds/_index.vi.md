---
title: "Nhập Dữ liệu từ S3 vào DynamoDB"
weight: 2
chapter: false
pre: " <b> 4.4.2. </b> "
---


1. Mở bảng điều khiển **Amazon DynamoDB Console**.
2. **Chọn nhập từ S3 (Import from S3):** Nhấp vào nút *Import from S3*.

<img src="/images/Picture10.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

3. **URL nguồn S3 (Source S3 URL):** Chọn bucket đang lưu trữ dữ liệu của bạn.
4. **Chủ sở hữu bucket S3 (S3 bucket owner):** Chọn *This AWS account* (Tài khoản AWS này).
5. **Nén tệp nhập (Import file compression):** Chọn *None* (Không nén).
6. **Định dạng tệp nhập (Import file format):** Chọn định dạng khớp với dữ liệu của bạn (ví dụ: CSV, JSON).

<img src="/images/Picture11.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

7. **Chi tiết bảng:** Nhập tên bảng (Table name) mà bạn muốn tạo.
8. **Khóa phân vùng (Partition key):** Chọn khóa phân vùng phù hợp cho dữ liệu của bạn.
9.  **Cài đặt bảng (Table settings):** Chọn *Default settings* (Cài đặt mặc định) và nhấp vào **'Next'**.

<img src="/images/Picture12.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

10. Nhấp vào nút **'Import'** (Nhập).
11. **Xác minh quá trình tạo bảng:**
    * Bạn sẽ thấy một thông báo thành công.
    * Bảng mới sẽ xuất hiện trong danh sách các bảng DynamoDB của bạn.