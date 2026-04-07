---
title: "Build & Push Docker Image lên Amazon ECR"
weight: 2
chapter: false
pre: " <b> 4.5.2. </b> "
---

## Build & Push lên Amazon ECR
Đầu tiên, chúng ta sẽ tạo một repository để lưu trữ và quản lý Docker container images.
Truy cập vào AWS Console, tìm kiếm ECR:

![Deploy Session](/AWS_Intern-Report/images/searchecr.png)

Bạn sẽ thấy giao diện của ECR, nhấn **Create**

![Deploy Session](/AWS_Intern-Report/images/createecr.png)

Điền tên repository, tại mục **Image tag mutability** chọn **Mutable**

![Deploy Session](/AWS_Intern-Report/images/configecr1.png)

Ở phần Encryption configuration, chọn **AES-256**, sau đó nhấn **Create repository**:

![Deploy Session](/AWS_Intern-Report/images/configecr2.png)

Sau khi tạo xong, bạn sẽ thấy repository private mới trên console, kèm theo một **URI**

## Login Docker into AWS
Để đăng nhập Docker vào AWS, chúng ta cần tạo một IAM user và gán policy **AmazonEC2ContainerRegistryFullAccess**

![Deploy Session](/AWS_Intern-Report/images/searchiam.png)

![Deploy Session](/AWS_Intern-Report/images/createiamuser.png)

![Deploy Session](/AWS_Intern-Report/images/attachpolicy.png)

Sau đó, vào phần **Security Credentials**, tạo Access Key và Secret Key để sử dụng với AWS CLI

Quay lại repository ECR vừa tạo, click vào repository, bạn sẽ thấy tab Images, chọn View push commands:

![Deploy Session](/AWS_Intern-Report/images/imagepushcommand.png)

Sau khi truy cập AWS CLI, hãy thực thi lần lượt các lệnh trong phần “View push commands” để build Docker image và push lên repository ECR.

Cuối cùng, quay lại ECR console, trong tab Images, bạn sẽ thấy image mới đã được upload thành công.
