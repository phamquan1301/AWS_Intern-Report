---
title: "Sự Kiện 2"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Cloud Mastery 2026 - #1 AI From Scratch

**Thời gian:** Ngày 14 tháng 3 năm 2026  
**Địa điểm:** Tòa nhà Bitexco Financial Tower (Tầng 26), Số 2 Hải Triều, Phường Bến Nghé (Sai Gon Ward), TP. Hồ Chí Minh  

---

### Giới thiệu sự kiện

Sự kiện "Cloud Mastery 2026 - #1 AI From Scratch" đã mang đến một cái nhìn toàn diện về trí tuệ nhân tạo hiện đại và kiến trúc điện toán đám mây. Trải qua ba phiên trình bày, các chuyên gia trong ngành đã phân tích các khái niệm từ AI Agent tự động cho đến sự kết hợp giữa phần cứng và phần mềm trong AIoT.


#### 09:00 AM - 10:00 AM | Xây dựng AI Agent với Strands
**Diễn giả:** Bành Cẩm Vinh

Phiên đầu tiên đã giải quyết các giới hạn của các mô hình LLM độc lập và cách kết nối chúng với thế giới thực bằng các công cụ bên ngoài.

![MCP Demo](images/z7672910003036_ed196b198e5a6f34e85285e46f6acef3.jpg)
![MCP Demo](images/z7672909965679_02a0afa0fc4855bf7e543dbd8d430037.jpg)
![MCP Demo](images/z7672910039736_54beec9e84171db16c7545910e9a6f0f.jpg)

**Các điểm chính:**
* **AI Agent là gì:** AI Agent đặc trưng bởi khả năng suy luận đa bước (multi-step reasoning) để thực thi các luồng công việc phức tạp.
* **Khả năng của Agent:** Chúng có hành vi tự chủ để đưa ra quyết định và hành động, tích hợp công cụ để truy cập API và cơ sở dữ liệu, và truy cập dữ liệu theo thời gian thực để lấy thông tin mới nhất.
* **Thích ứng linh hoạt:** Các Agent cung cấp các phản hồi linh hoạt dựa trên bối cảnh hiện tại.
* **Strands Agents:** Framework này cho phép tạo các agent sử dụng công cụ tích hợp sẵn (built-in tools), system prompts, "Agentic Loop" (vòng lặp gọi công cụ), và cơ sở tri thức (Knowledge Base).

---

#### 10:00 AM - 11:00 AM | Tự động hóa Prompt Engineering: Nâng cao chất lượng đầu ra của LLM
**Diễn giả:** Nguyễn Tuấn Thịnh, Kỹ sư DevOps

Phiên thứ hai tập trung vào "Nghệ thuật giao tiếp với AI" và lý do tại sao kỹ thuật thiết kế Prompt (Prompt Engineering) lại cực kỳ quan trọng đối với việc quản lý chất lượng và chi phí.

![MCP Demo](images/z7672922380929_2f993a44dd8f090fdf1de88125c166a0.jpg)
![MCP Demo](images/z7672909984608_fcc5f42c25152e84cf4fcc9b2b5ed07e.jpg)

**Các điểm chính:**
* **Cái giá của Prompt kém:** Các câu lệnh chung chung dẫn đến kết quả chung chung, gây lãng phí token và làm tăng chi phí. Ví dụ, mô hình Claude Opus 4.6 có giá $5.00 cho 1 triệu token đầu vào và $25.00 cho 1 triệu token đầu ra.
* **Cấu trúc Prompt chuẩn:** Một prompt hiệu quả cao cần bao gồm Vai trò (Role), Hướng dẫn (Instruction), Ngữ cảnh (Context), Dữ liệu đầu vào (Input Data), Định dạng đầu ra (Output Format), Ví dụ (Examples), và Ràng buộc (Constraints).
* **Kỹ thuật nâng cao:** Người dùng nên tận dụng các phương pháp như Chain-of-Thought (CoT), Self-Consistency, Tree-of-Thoughts (ToT), Retrieval-Augmented Generation (RAG) và Role Prompting.
* **Giới thiệu Proptimizer:** Diễn giả đã giới thiệu "Proptimizer", một tiện ích mở rộng trên trình duyệt giúp tối ưu hóa prompt và cho phép chat với AI ở bất kỳ đâu trên web. Kiến trúc của công cụ này chạy hoàn toàn trên AWS, sử dụng Amazon CloudFront và S3 cho frontend, Cognito để xác thực, API Gateway và Lambda cho logic backend, DynamoDB để lưu trữ, và Amazon Bedrock để tích hợp AI.

---

#### 11:00 AM - 12:00 PM | Các dự án AIoT: Quản lý Tủ đồ & Plutus
**Diễn giả:** Aiden Đinh (Kỹ sư Vận hành tại Katalon) và Trần Vũ Bảo Ngọc

Phiên cuối cùng trình diễn các ứng dụng thực tế kết hợp giữa Trí tuệ nhân tạo và Internet vạn vật (AIoT).

![MCP Demo](images/z7672910019188_393fdd830b5a921571353ab19025e0a2.jpg)

**Các điểm chính:**
* **Hệ thống quản lý tủ đồ:** Được thiết kế để thay thế các quy trình thủ công dư thừa, hệ thống tự động này cho phép các thành viên câu lạc bộ mượn đồ một cách liền mạch.
* **Tích hợp phần cứng:** Hệ thống sử dụng Raspberry Pi làm bộ điều khiển chính và MQTT Broker, cùng với một Arduino để thu thập dữ liệu từ các cảm biến, bao gồm đầu đọc thẻ RFID cho ID của vật dụng và Công tắc từ (Reed Switch) để phát hiện khi tủ được mở.
* **Kiến trúc Cloud:** Phần cứng kết nối bảo mật tới AWS IoT Core, dịch vụ này sẽ định tuyến các sự kiện cảm biến tới Lambda, DynamoDB và S3.
* **Tích hợp AI:** AWS Rekognition được kích hoạt để thực hiện nhận diện khuôn mặt trên các hình ảnh chụp được, trả về điểm số tin cậy tương đồng để xác định thành viên nào đang truy cập tủ đồ.
* **Ứng dụng Plutus:** Một màn trình diễn phụ đã giới thiệu "Plutus," một ứng dụng Quản lý Ngân sách Tài chính.