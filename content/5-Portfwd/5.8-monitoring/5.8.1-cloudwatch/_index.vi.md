---
title: "Giám sát với CloudWatch"
weight: 1
chapter: false
pre: " <b> 4.8.1. </b> "
---

## Monitor với SNS & CloudWatch

## Tạo SNS Topic:

Trong AWS Management Console, tìm **SNS**:

![Monitor Session](/AWS_Intern-Report/images/searchsns.png)

Vào tab **Topic**, chọn **Create topic**

Chọn loại **Standard**, nhập tên **Application-alerts**, sau đó nhấn **Create topic**

![Monitor Session](/AWS_Intern-Report/images/configsns1.png)

## Tạo Subscription

Chọn **Create subscription**, tại **Protocol** chọn **Email**, nhập email của bạn để nhận thông báo từ SNS

![Monitor Session](/AWS_Intern-Report/images/createsubscription.png)

Sau khi tạo xong, vào mail để xác nhận email.

![Monitor Session](/AWS_Intern-Report/images/confirmsns.png)

## Tạo CPU Alarm

Truy cập **CloudWatch** > **Alarms** > **Create alarm**

![Monitor Session](/AWS_Intern-Report/images/createalarm.png)

Chọn **Select metric** > EC2 > Per-Instance Metrics > chọn **InstanceID của Beanstalk** > **CPUUtilization**

![Monitor Session](/AWS_Intern-Report/images/selectmetric.png)

![Monitor Session](/AWS_Intern-Report/images/selectec2.png)

![Monitor Session](/AWS_Intern-Report/images/selectpermetric.png)

![Monitor Session](/AWS_Intern-Report/images/selectelbid.png)

**Condition**: CPUUtilization lớn hơn 70%

![Monitor Session](/AWS_Intern-Report/images/condition.png)

**Notification**: chọn Topic **Application-alerts**

![Monitor Session](/AWS_Intern-Report/images/confignotifi.png)

Kiểm tra lại cấu hình alarm, sau đó nhấn **Create alarm**

![Monitor Session](/AWS_Intern-Report/images/reviewalarm.png)
