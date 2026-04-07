---
title: "Khởi tạo Elastic Beanstalk"
weight: 3
chapter: false
pre: " <b> 4.5.3. </b> "
---

## Cấu hình Elastic Beanstalk

Đầu tiên, tạo một file tên là **Dockerrun.aws.json** (đây là file cấu hình được AWS Elastic Beanstalk sử dụng để tự động chạy Docker container), file này nằm cùng cấp với Dockerfile.

![Deploy Session](/AWS_Intern-Report/images/dockerrun.png)

![Deploy Session](/AWS_Intern-Report/images/dockerrundetail.png)

Thay thế image name bằng **Image URI** mà bạn vừa push lên ECR, sau đó nén file lại thành file **.zip**

Truy cập AWS Management Console, tìm kiếm **elastic beanstalk**

![Deploy Session](/AWS_Intern-Report/images/searchelb.png)

Bạn sẽ thấy giao diện Elastic Beanstalk, nhấn Create application:

![Deploy Session](/AWS_Intern-Report/images/createapplication.png)

Thiết lập cấu hình theo như hình minh họa:

![Deploy Session](/AWS_Intern-Report/images/configelb1.png)

![Deploy Session](/AWS_Intern-Report/images/configelb2.png)

![Deploy Session](/AWS_Intern-Report/images/configelb3.png)

Chọn **Upload your code**, -> Local file, sau đó upload file **Dockerrun.aws.json.zip** đã nén trước đó:

![Deploy Session](/AWS_Intern-Report/images/configelb4.png)

![Deploy Session](/AWS_Intern-Report/images/configvpcelb.png)

Chọn VPC mà bạn đã tạo trước đó, ở phần Subnets, chọn 2 public subnets trong VPC:

![Deploy Session](/AWS_Intern-Report/images/ec2sgelb.png)

Chọn EC2 Security Group đã tạo:

![Deploy Session](/AWS_Intern-Report/images/ec2sgelb.png)

Phần Environment type, chọn **Load balanced** (Elastic Beanstalk sẽ sử dụng Application Load Balancer):

![Deploy Session](/AWS_Intern-Report/images/loadbalancerelb.png)

Chọn 2 public subnets cho Load Balancer:

![Deploy Session](/AWS_Intern-Report/images/loadbalancer1.png)

Ở phần Instance types, chọn **t3.micro**

![Deploy Session](/AWS_Intern-Report/images/instanceec2elb.png)

Nhấn Next, kiểm tra lại toàn bộ cấu hình. Nếu mọi thứ đã đúng, nhấn **Create**. Hệ thống sẽ mất khoảng 5–7 phút để khởi tạo môi trường.
