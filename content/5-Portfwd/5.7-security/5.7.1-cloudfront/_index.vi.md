---
title: "Cấu hình S3 & CloudFront & WAF"
weight: 1
chapter: false
pre: " <b> 4.6.1. </b> "
---

## Cấu hình S3 bucket for frontend  
Đầu tiên, chạy lệnh sau để build project React thành static files: **npm run build**  
Một thư mục **dist** sẽ được tạo ra, chứa toàn bộ file deploy (HTML, CSS, JS).

Truy cập **AWS Management Console**, tìm **S3**, chọn **Create Bucket**

Chọn **Global namespace** cho bucket namespace, tại bucket name nhập **scam-detection-fe**

![Cloudfront Session](/AWS_Intern-Report/images/createfes3bucket.png)

Chọn **ACLs disabled**, nhớ tick chọn **Block all public access**. 

![Cloudfront Session](/AWS_Intern-Report/images/configs3fe.png)

Giữ nguyên các cấu hình còn lại, sau đó nhấn **Create**

Truy cập vào bucket vừa tạo, chọn **Upload**

![Cloudfront Session](/AWS_Intern-Report/images/clickuploads3.png)

Chọn **Add files**, thêm tất cả file trong thư mục **dist** vừa build  
Chọn **Add folder**, thêm toàn bộ folder trong **dist** (ví dụ: `/assets`)  
Sau đó kéo xuống dưới và nhấn **Upload**

![Cloudfront Session](/AWS_Intern-Report/images/addfilefolder.png)

Bạn sẽ thấy tất cả file và folder trong **dist** đã xuất hiện trong tab **Objects**

## Configure Cloudfront & WAF
Truy cập **Amazon CloudFront console**, chọn **Create distribution**, chọn **Free plan**

![Cloudfront Session](/AWS_Intern-Report/images/cloudfrontfree.png)

Nhấn **Next**, nhập **anti-scaq** cho **Distribution name**, chọn loại **Single website or app**

Tại **Origin type**, chọn **Amazon S3**, sau đó chọn bucket vừa tạo

![Cloudfront Session](/AWS_Intern-Report/images/originfront.png)

Cấu hình các thông số theo hình minh họa:

![Cloudfront Session](/AWS_Intern-Report/images/cloudfrontconfig1.png)

![Cloudfront Session](/AWS_Intern-Report/images/cloudfrontconfig2.png)

Nhấn **Next**, kiểm tra lại cấu hình, sau đó chọn **Create distribution**. Hệ thống sẽ mất khoảng **4–5 phút** để khởi tạo.

Sau khi tạo xong, vào tab **General**, ở phần Settings chọn **Edit**, tại mục **Default root object - optional**, điền vào **index.html**. Sau đó, nhấn **Save changes**

![Cloudfront Session](/AWS_Intern-Report/images/editsetting.png)

Để cấp quyền cho CloudFront truy cập file trong S3, vào tab **Permissions** của bucket → chọn **Edit bucket policy**
Thêm policy theo format sau rồi nhấn **Save changes**:

![Cloudfront Session](/AWS_Intern-Report/images/statementpermission.png)

Bây giờ bạn có thể truy cập UI React thông qua Distribution domain name của CloudFront.
