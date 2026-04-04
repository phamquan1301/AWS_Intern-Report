---
title: "Import Data from S3 to DynamoDB"
weight: 2
chapter: false
pre: " <b> 4.4.2. </b> "
---


1. Open the **Amazon DynamoDB Console**.
2. **Choose import from S3:** Click on *Import from S3*.

<img src="/images/Picture10.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

3. **Source S3 URL:** Choose the S3 bucket that stores your data.
4. **S3 bucket owner:** Choose *This AWS account*.
5. **Import file compression:** Choose *None* (No compression).
6. **Import file format:** Choose the format that matches your data (e.g., CSV, JSON).

<img src="/images/Picture11.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

7. **Table details:** Enter your desired table name.
8. **Partition key:** Choose the appropriate partition key for your data.
9.  **Table settings:** Choose *Default settings* and click **'Next'**.

<img src="/images/Picture12.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

10. Click **'Import'**.
11. **Verify table creation:**
    * You should see a success message.
    * The new table should appear in your DynamoDB tables list.