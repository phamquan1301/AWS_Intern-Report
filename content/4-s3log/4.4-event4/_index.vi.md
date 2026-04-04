---
title: "Sự Kiện 4"
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---
# Cloud Mastery 2026 - #2

**Thời gian:** Ngày 04 tháng 04 năm 2026  
**Địa điểm:** Lô E2a-7, Đường D1, Khu Công nghệ cao, P. Long Thạnh Mỹ, TP. Thủ Đức, TP.HCM

---

### Giới thiệu sự kiện

Sự kiện "Cloud Mastery 2026 - #2" mang đến những kiến thức chuyên sâu về tự động hóa hạ tầng đám mây, quản trị container và các giải pháp xây dựng hệ thống chịu lỗi cao. Trải qua ba phiên trình bày, các chuyên gia sẽ hướng dẫn chi tiết từ cách thức triển khai Infrastructure as Code (IaC), xây dựng kiến trúc vi dịch vụ với Kubernetes, cho đến việc ứng dụng sức mạnh của ngôn ngữ Elixir để tối ưu hóa hiệu năng và chi phí vận hành.

#### 09:00 AM - 10:00 AM | Infrastructure as Code với Terraform trên AWS
**Diễn giả:** Thịnh Nguyễn

Phiên đầu tiên giải quyết những bài toán về quản lý hạ tầng thủ công (ClickOps) và đi sâu vào phương pháp quản lý hạ tầng dưới dạng mã nguồn (IaC).

![MCP Demo](/images/z7690434959781_5995c1f23896281068e12c8291828764.jpg)

**Các điểm chính:**
* **Hạn chế của ClickOps:** Dễ dẫn đến lỗi do con người, thiếu tính nhất quán và gây khó khăn trong việc cộng tác cũng như mở rộng quy mô.
* **AWS CloudFormation & CDK:** Giới thiệu công cụ IaC tích hợp sẵn của AWS sử dụng template YAML/JSON, cùng với AWS Cloud Development Kit (CDK) cho phép định nghĩa hạ tầng bằng các ngôn ngữ lập trình quen thuộc thông qua 3 cấp độ Construct (L1, L2, L3).
* **Sức mạnh của Terraform:** Khám phá công cụ mã nguồn mở từ HashiCorp hỗ trợ đa nền tảng (Multi-Cloud). Terraform sử dụng ngôn ngữ HCL, có khả năng theo dõi trạng thái (State Tracking) và dễ dàng quản lý cấu trúc thư mục dự án.
* **Lệnh cơ bản:** Các thao tác quan trọng trong Terraform như `init`, `validate`, `plan`, `apply` và `destroy` để quản lý vòng đời tài nguyên.

---

#### 10:00 AM - 11:00 AM | Kiến trúc Đám mây với Kubernetes
**Diễn giả:** Bảo Huỳnh

Phiên thứ hai tập trung vào việc giải quyết thách thức trong vận hành container ở quy mô lớn và giới thiệu toàn diện về nền tảng điều phối Kubernetes.

![MCP Demo](/images/z7690435228232_086a56675e6aceba29f20c40dd0784ae.jpg)
![MCP Demo](/images/z7690434965400_cae89d3f8e93d1f220c7d1f9b6cf3c53.jpg)
![MCP Demo](/images/z7690434968587_e13530a2c89be891b4b71505947256d4.jpg)
![MCP Demo](/images/z7690434973622_2f1925766e43a519731b9bdaaee316e6.jpg)
![MCP Demo](/images/z7690434979666_8f76d527eb5884f65c86517060490543.jpg)

**Các điểm chính:**
* **Giải quyết bài toán mở rộng:** Kubernetes (K8s) giúp tự động hóa quá trình triển khai, mở rộng (Scaling) và tự phục hồi (Self-Healing) cho các ứng dụng container hóa.
* **Kiến trúc K8s:** Hệ thống được chia thành Control Plane (đầu não với etcd, kube-apiserver, kube-scheduler, controller-manager) và Worker Nodes (nơi chạy ứng dụng với kubelet, kube-proxy, container runtime).
* **Các đối tượng cốt lõi:** Đi sâu vào chức năng của Pods (đơn vị nhỏ nhất), ReplicaSet, Deployment (hỗ trợ cập nhật và rollback), ConfigMap, Secret (lưu trữ thông tin nhạy cảm) và Jobs.
* **Công cụ hệ sinh thái:** Hướng dẫn sử dụng file Manifest YAML, lệnh `kubectl`, và giới thiệu các công cụ hỗ trợ như Helm (quản lý package), K9S (quản lý qua terminal) cùng các giải pháp chạy K8s nội bộ như Minikube, K3S, K3D.

---

#### 11:00 AM - 12:00 PM | Elixir: Giải pháp hợp nhất cho hạ tầng DevOps chịu lỗi cao cấp
**Diễn giả:** Nguyễn Tạ Minh Triết

Phiên cuối cùng mang đến một góc nhìn mới về việc sử dụng ngôn ngữ lập trình hàm Elixir trên máy ảo BEAM để xây dựng các hệ thống đồng thời và chịu lỗi cực tốt.

![MCP Demo](/images/z7690434656220_0fa9e4ded78c55c5a36cb8a9f541590c.jpg)
![MCP Demo](/images/z7690434654020_7a1bcabf3cdd9e589b663ad6cfe4e25b.jpg)
![MCP Demo](/images/z7690434643660_bb9295a9bd21e54f1b0a726defda70d5.jpg)

**Các điểm chính:**
* **Sức mạnh của BEAM VM:** Elixir kế thừa kiến trúc từ Erlang, cho phép chạy hàng triệu tiến trình (lightweight processes) cực nhẹ, hoạt động độc lập và không chia sẻ bộ nhớ, giúp tối ưu xử lý đồng thời.
* **Triết lý "Let It Crash":** Thay vì sử dụng các khối `try/catch` phức tạp để phòng vệ, hệ thống sử dụng các Supervisor để giám sát. Khi một tiến trình (worker) lỗi, Supervisor sẽ để nó "chết" và tự động khởi động lại, mang lại khả năng chịu lỗi (Fault-Tolerance) tuyệt vời.
* **Hot Code Upgrades:** Khả năng cập nhật mã nguồn của hệ thống ngay trong lúc đang chạy mà không hề gây ra thời gian chết (downtime).
* **Hiệu quả thực tế:** Một case study thực tế cho thấy việc chuyển đổi hệ thống từ mô hình Serverless (AWS API Gateway + Lambda với Node.js) sang Elixir đã giúp giảm chi phí khổng lồ từ hơn $30,000/tháng xuống chỉ còn chưa tới $400/tháng khi phải xử lý hàng chục triệu request.