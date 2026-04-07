---
title: "Đóng gói ứng dụng với Docker"
weight: 1
chapter: false
pre: " <b> 4.5.1. </b> "
---

### Đóng gói ứng dụng với Docker
Trước khi triển khai lên AWS Elastic Beanstalk, chúng ta cần đóng gói ứng dụng Spring Boot thành một Docker image.

### Tạo Dockerfile
Trong thư mục gốc của ứng dụng, tạo một file có tên là Dockerfile

![Deploy Session](/AWS_Intern-Report/images/dockerfile.png)

![Deploy Session](/AWS_Intern-Report/images/dockerfiledetail.png)

Tiếp theo, chúng ta cần build file .jar bằng lệnh: 
- mvn clean package -DskipTests

Bạn có thể thấy file jar được tạo trong thư mục **target/**:
![Deploy Session](/AWS_Intern-Report/images/buildjarfile.png)


Sau đó, chúng ta có thể test ứng dụng ở môi trường local trước khi deploy bằng các lệnh:
- docker build -t my-app .
- docker run -p 8080:8080 my-app

Truy cập **http://localhost:8080** để xem ứng dụng của bạn.
