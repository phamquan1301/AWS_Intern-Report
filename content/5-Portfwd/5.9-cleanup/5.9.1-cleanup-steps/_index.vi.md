---
title: "Dọn dẹp tài nguyên"
weight: 1
chapter: false
pre: " <b> 4.8.1. </b> "
---

### Các bước dọn dẹp

Để tránh phát sinh chi phí không mong muốn sau khi hoàn thành Workshop, hãy xóa tài nguyên theo thứ tự sau:

**1. VPC**: Truy cập VPC, chọn VPC của ứng dụng, nhấn **Delete**

![Cleanup Session](/AWS_Intern-Report/images/deletevpc.png)

**2. RDS for MySQL**: Truy cập RDS, tab **Databases**, chọn database, nhấn **Delete**.  
Bỏ tick **Create final snapshot**, **Retain automated backups**, tick **I acknowledge ...**

![Cleanup Session](/AWS_Intern-Report/images/deleterds.png)

![Cleanup Session](/AWS_Intern-Report/images/deleterds1.png)

**3. ECR**: Truy cập ECR, tab **Repositories**, chọn repository chứa Docker image của ứng dụng, nhấn **Delete**

![Cleanup Session](/AWS_Intern-Report/images/deleteecr.png)

**4. Elastic Beanstalk**: Truy cập Elastic Beanstalk, tab **Environments**, chọn environment, sau đó nhấn **Terminate environment**

![Cleanup Session](/AWS_Intern-Report/images/deleteelb.png)

**5. S3 Bucket**: Truy cập S3, tab **General purpose buckets**, chọn bucket chứa frontend, nhấn **Delete**

![Cleanup Session](/AWS_Intern-Report/images/deletes3fe.png)

**6. CloudFront**: Truy cập CloudFront, chọn distribution, chọn **Disable**, sau đó **Delete**

![Cleanup Session](/AWS_Intern-Report/images/deletecloudfront.png)

**7. CloudWatch**: Truy cập CloudWatch, tab **Alarms**, chọn alarm đã tạo, nhấn **Delete**

![Cleanup Session](/AWS_Intern-Report/images/deletecloudwatch.png)
