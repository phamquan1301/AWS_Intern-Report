---
title : "Workshop"
date: 2026-01-10
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

# Triển khai AnTiScaQ (Nền tảng cảnh báo Chống lừa đảo) trên AWS

## Tổng quan

Workshop này hướng dẫn chi tiết cách triển khai **AnTiScaQ**, một nền tảng **cảnh báo chống lừa đảo**, trên đám mây **AWS** bằng kiến trúc **Serverless**. Hệ thống sử dụng Python để **thu thập dữ liệu nguồn mở** và cung cấp **điểm rủi ro theo thời gian thực** cho số điện thoại, email cùng tên miền, đồng thời tuân thủ chặt chẽ các trụ cột của **AWS Well-Architected Framework** nhằm **tối ưu hóa chi phí**, đảm bảo **tính toàn vẹn dữ liệu** và **khả năng mở rộng linh hoạt** khi có sự bùng phát. Ở lớp tính toán, nền tảng sử dụng **AWS Lambda** và **API Gateway** để xử lý mượt mà các truy vấn tốc độ cao cũng như các tác vụ thu thập dữ liệu bất đồng bộ mà **không cần quản lý máy chủ**. Trong khi đó, **Amazon DynamoDB** được ứng dụng làm cơ sở dữ liệu chính với tốc độ phản hồi cực nhanh, kết hợp cùng **Amazon S3** để lưu trữ bằng chứng lừa đảo và host giao diện web Next.js tĩnh. Lớp bảo mật của hệ thống được củng cố vững chắc bằng **Amazon Cognito** để phân quyền truy cập và **AWS Secrets Manager** để **tự động bảo vệ các dữ liệu nhạy cảm**. Thêm vào đó, toàn bộ quy trình đóng gói mã nguồn và triển khai ứng dụng đều được **tự động hóa** thông qua hệ thống CI/CD của **GitHub Actions**, đồng thời nền tảng cũng tận dụng sức mạnh của **CloudFront CDN** kết hợp cùng **Amazon S3** để **tăng tốc tối đa** việc phân phối nội dung giao diện đến người dùng. Mọi hoạt động của hệ thống, bảo vệ chống DDoS đến cảnh báo chi phí bất thường, đều được **giám sát chặt chẽ** bởi **AWS WAF**, **Amazon CloudWatch**. Cuối cùng, thông qua workshop này, bạn sẽ làm chủ hoàn toàn việc xây dựng, vận hành một hạ tầng backend an toàn, cũng như nắm vững **quy trình dọn dẹp tài nguyên** để **tránh phát sinh chi phí ngoài ý muốn**.

### Nội dung

**4.1.** [Giới thiệu](5.1-introduction/)

**4.2.** [Điều kiện tiên quyết](5.2-prerequisites/)

**4.3.** [Thiết lập cơ sở hạ tầng mạng](5.3-network-setup/)

**4.4.** [Triển khai tầng dữ liệu](5.4-data-layer/)

**4.5.** [Triển khai ứng dụng](5.5-deploy-app/)

**4.6.** [Tự động hóa CI/CD](5.6-cicd/)

**4.7.** [Tối ưu hóa & Bảo mật](5.7-security/)

**4.8.** [Giám sát & Vận hành](5.8-monitoring/)

**4.9.** [Dọn dẹp tài nguyên](5.9-cleanup/)