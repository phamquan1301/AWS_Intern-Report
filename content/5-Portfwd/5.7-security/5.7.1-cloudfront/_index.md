---
title: "Configure S3 & CloudFront & WAF"
weight: 1
chapter: false
pre: " <b> 4.6.1. </b> "
---

## Configure S3 bucket for frontend
First, run this command to build your React project to static files: **npm run build**
A **.dist** folder will appear, containing all the deploy files (HTML, CSS, JS).

Go to AWS Management Console, search for **S3**, click **Create Bucket**

Choose **Global namespace** for bucker namespace, for bucket name fill out **scam-detection-fe**

![Cloudfront Session](/AWS_Intern-Report/images/createfes3bucket.png)

Choose **ACLs disabled**, remember to tick **Block all public access**. 

![Cloudfront Session](/AWS_Intern-Report/images/configs3fe.png)

Keep the remaining configurations unchanged. Click **Create**

Access into the bucket just created, click **Upload**

![Cloudfront Session](/AWS_Intern-Report/images/clickuploads3.png)

Choose **Add files**, add all the files in **dist** folder that we just build before
Choose **Add folder**, add all folders in **dist** too, like folder /assets. Then, scroll down , click **Upload**

![Cloudfront Session](/AWS_Intern-Report/images/addfilefolder.png)

You can see all the files and folders in dist folder has been availabled in tab **Objects**

## Configure Cloudfront & WAF
Go to Amazon Cloudfront console, click **Create distribution**, choose **Free plan**

![Cloudfront Session](/AWS_Intern-Report/images/cloudfrontfree.png)

Click **Next**, type **anti-scaq** for **Distribution name**, choose type **Single website or app**

At **Origin type**, choose **Amazon S3**, for S3 origin browse the bucket that created

![Cloudfront Session](/AWS_Intern-Report/images/originfront.png)

Config settings as images demonstrate:

![Cloudfront Session](/AWS_Intern-Report/images/cloudfrontconfig1.png)

![Cloudfront Session](/AWS_Intern-Report/images/cloudfrontconfig2.png)

Click **Next**, review the configuration. Then, click **Create distribution**. The system will take approximately 4-5 minutes to initialize.

In General tab, at Settings click **Edit**, at **Default root object - optional**, fill out **index.html**. Then, click **Save changes**

![Cloudfront Session](/AWS_Intern-Report/images/editsetting.png)

In order to gain permission for Cloudfront to access S3 files, go to tab **Permisisions** in your bucker, click **Edit** bucket policy.
Use this format statement, then click **Save changes**

![Cloudfront Session](/AWS_Intern-Report/images/statementpermission.png)

Now, we can access to our UI React project with **Distribution domain name** of Cloudfront.