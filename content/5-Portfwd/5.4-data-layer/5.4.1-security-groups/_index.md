---
title: "Setup S3 bucket"
weight: 1
chapter: false
pre: " <b> 4.4.1. </b> "
---

### Bucket Names
You will need to create 3 buckets:
* `rag-data-ACCOUNT_ID-REGION` – store data for model
* `user-report-ACCOUNT_ID-REGION` – store user report image
* `chatbot-image-ACCOUNT_ID-REGION` – store image user send to chatbot

---

### Instructions

**1. Open the Amazon S3 Console**
* Navigate to [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/)
* Or: **AWS Management Console** → **Services** → **S3**

<img src="/images/Picture5.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**2. Click on "Create bucket"**

**3. General configuration:**
* **Bucket name:** Enter `rag-data-ACCOUNT_ID-REGION`
  * *Example:* `rag-data-123456789012-us-east-1`
* **AWS Region:** Select your target region (e.g., *US East (N. Virginia) us-east-1*)

<img src="/images/Picture6.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**4. Object Ownership:**
* Keep default: **ACLs disabled (recommended)**

**5. Block Public Access settings for this bucket:**
* Check **"Block all public access"**
* Ensure all 4 sub-options are checked:
  * ✓ Block public access to buckets and objects granted through *new* access control lists (ACLs)
  * ✓ Block public access to buckets and objects granted through *any* access control lists (ACLs)
  * ✓ Block public access to buckets and objects granted through *new* public bucket or access point policies
  * ✓ Block public and cross-account access to buckets and objects through *any* public bucket or access point policies

**6. Bucket Versioning:**
* Select **"Enable"**

<img src="/images/Picture7.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**7. Tags (optional):**
* Add tags if desired
* *Example:* `Key=Purpose`, `Value=IncidentResponse`

**8. Default encryption:**
* **Encryption type:** Select **"Server-side encryption with Amazon S3 managed keys (SSE-S3)"**
* **Bucket Key:** Keep default (**Enabled**)

**9. Advanced settings:**
* Keep all defaults

**10. Click "Create bucket"**

<img src="/images/Picture8.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

**11. Verify bucket creation:**
* You should see a success message
* The bucket should appear in your S3 buckets list

> **Note:** Repeat steps 2-11 for the remaining 2 buckets:
> * `user-report-ACCOUNT_ID-REGION`
> * `chatbot-image-ACCOUNT_ID-REGION`

<img src="/images/Picture9.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">
