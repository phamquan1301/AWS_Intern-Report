---
title: "Bedrock Knowledge Base"
weight: 3
chapter: false
pre: " <b> 4.4.3. </b> "
---
1. **Upload data (PDF)** to the `rag-data` bucket.
2. Open the **Amazon Bedrock console**.

<img src="/images/Picture1.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

3. **Choose Knowledge Base:** Click *Create Knowledge Base*.

<img src="/images/Picture2.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

4. **General Configuration:**

<img src="/images/Picture3.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

   - **Data source name:** Enter `RAG-data`.
   - **S3 URL:** Choose the file uploaded to the bucket in Step 1.
5. **Parsing strategy:** Choose *Amazon Bedrock default parser*.
6. **Chunking strategy:** Choose *Semantic chunking*.
7. **Choose embedding model:** Select your preferred model.

<img src="/images/Picture4.jpg" style="width: 100%; height: auto;" alt="Mô tả ảnh">

8. **Vector store:**
   - Choose *Quick create a new vector store*.
   - **Vector store type:** Choose *Amazon S3 vector*.
9. Click **'Create knowledge base'**.