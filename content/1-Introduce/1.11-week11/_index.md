---
title: "Week 11 Worklog"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Week 11 Objectives

- Understand the fundamental concepts of Artificial Intelligence and Machine Learning and how they relate to AWS services.
- Distinguish between AI, ML, Deep Learning, and Generative AI, and know where each fits in the AWS landscape.
- Learn the end-to-end ML workflow: data preparation, model training, evaluation, deployment, and monitoring.
- Master Amazon SageMaker as the central platform for building, training, and deploying ML models at scale.
- Explore AWS AI services (Rekognition, Textract, Comprehend, Polly, Transcribe, Translate) and when to use them vs. building custom models.
- Get familiar with Amazon Bedrock for accessing foundation models and building Generative AI applications on AWS.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Studied AI/ML fundamentals: supervised vs. unsupervised vs. reinforcement learning, key algorithms (linear regression, decision trees, neural networks), and the ML problem framing process. | 03/16/2026 | 03/16/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Explored the ML workflow end-to-end: data collection and labeling, feature engineering, train/validation/test splits, bias-variance tradeoff, model evaluation metrics (accuracy, precision, recall, F1, AUC-ROC). | 03/17/2026 | 03/17/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Learned Amazon SageMaker core components: SageMaker Studio IDE, Data Wrangler for feature engineering, Experiments for run tracking, Training Jobs, Hyperparameter Tuning, and Model Registry. | 03/18/2026 | 03/18/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Studied SageMaker deployment options: real-time inference endpoints, serverless inference, asynchronous inference, batch transform; explored auto-scaling, A/B testing with multi-variant endpoints, and Model Monitor. | 03/19/2026 | 03/19/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Surveyed AWS AI Services: Rekognition (image/video), Textract (document OCR), Comprehend (NLP), Transcribe (speech-to-text), Polly (text-to-speech), Translate, and Forecast — understanding when pre-built AI APIs are preferable to custom models. | 03/20/2026 | 03/20/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | **Event:** FCAJ Community Day.<br>Explored Amazon Bedrock: foundation models (Claude, Titan, Llama, Mistral, Stable Diffusion), Retrieval-Augmented Generation (RAG) with Knowledge Bases, Agents for orchestration, and Guardrails for responsible AI. | 03/21/2026 | 03/21/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 11 Achievements

##### AI/ML Fundamentals — Concepts and Landscape

**The AI Hierarchy:**

```
Artificial Intelligence (AI)
└── Machine Learning (ML) — learns patterns from data
    └── Deep Learning (DL) — multi-layer neural networks
        └── Generative AI (GenAI) — creates new content (text, image, code, audio)
```

**Types of Machine Learning:**

| Type | How It Learns | AWS Example Use Cases |
|---|---|---|
| **Supervised** | Labeled training data (input → expected output) | Fraud detection, image classification, sales forecasting |
| **Unsupervised** | Unlabeled data — discovers hidden patterns | Customer segmentation, anomaly detection, topic modeling |
| **Reinforcement** | Agent learns by interacting with environment and receiving rewards | Robotics, game playing, recommendation optimization |
| **Self-supervised** | Labels derived from the data itself | Foundation models like GPT, BERT, Stable Diffusion |

**AWS AI/ML Stack (three layers):**

| Layer | Description | AWS Services |
|---|---|---|
| **AI Services** | Pre-built APIs, no ML expertise needed | Rekognition, Textract, Comprehend, Forecast |
| **ML Platform** | Build, train, and deploy custom models | Amazon SageMaker |
| **ML Frameworks** | Open-source frameworks on AWS infrastructure | TensorFlow, PyTorch, MXNet on EC2/SageMaker |

##### The ML Workflow — End to End

Building a production ML system is a full lifecycle process, not just model training:

**1. Problem Framing:**
- Define the ML objective: is it a classification, regression, clustering, or ranking problem?
- Identify the success metric: accuracy, F1, RMSE, NDCG?
- Define the baseline (simple rule-based or random model to beat).

**2. Data Collection and Labeling:**
- Gather training data from databases, S3, APIs, or web scraping.
- Label data using **Amazon SageMaker Ground Truth** (managed data labeling with human workforce or automated labeling using active learning).
- Ensure sufficient data volume and class balance — address class imbalance via oversampling (SMOTE) or cost-sensitive learning.

**3. Feature Engineering:**
- **Numerical features**: normalization (min-max), standardization (z-score), log transformation for skewed distributions.
- **Categorical features**: one-hot encoding, target encoding, embedding for high-cardinality fields.
- **Time-series features**: lag features, rolling window statistics, Fourier transforms for seasonality.
- **Text features**: TF-IDF, word embeddings (Word2Vec, GloVe), or BERT-based contextual embeddings.
- Tool: **SageMaker Data Wrangler** — 300+ built-in transforms, data quality reports, and direct export to SageMaker Pipelines.

**4. Model Training:**
- Split data: typically 70% train / 15% validation / 15% test.
- Monitor **bias-variance tradeoff**: underfitting (high bias) → increase model complexity; overfitting (high variance) → regularization (L1/L2), dropout, or more training data.
- Use **SageMaker Experiments** to track hyperparameters, metrics, and artifacts across training runs.

**5. Model Evaluation:**

| Metric | Use When |
|---|---|
| Accuracy | Balanced class distribution |
| Precision / Recall | Imbalanced classes (e.g., fraud detection) |
| F1 Score | Harmonic mean of precision and recall |
| AUC-ROC | Comparing classifiers across thresholds |
| RMSE / MAE | Regression problems |
| NDCG / MAP | Ranking and recommendation systems |

**6. Deployment and Monitoring:**
- Deploy model; monitor for **data drift** and **concept drift** using SageMaker Model Monitor.
- Retrain on schedule or trigger retraining when monitoring alerts fire.

##### Amazon SageMaker — The End-to-End ML Platform

SageMaker is AWS's fully managed platform covering every stage of the ML lifecycle:

**SageMaker Studio:**
- A web-based IDE purpose-built for ML — notebooks, data exploration, experiment tracking, model registration, and pipeline authoring all in one environment.
- Runs on managed compute; no infrastructure to provision.

**SageMaker Data Wrangler:**
- Import data from S3, Athena, Redshift, or SaaS sources.
- Apply 300+ built-in transforms visually and export the data flow directly to a SageMaker Pipeline as a processing step.
- Automatically generates data quality reports (class imbalance, missing values, correlation analysis).

**SageMaker Training:**
- Managed distributed training using built-in algorithms (XGBoost, Linear Learner, k-NN, DeepAR, BlazingText) or custom containers (TensorFlow, PyTorch, MXNet).
- **Spot Training**: use EC2 Spot Instances for training jobs — up to 90% cost reduction vs. On-Demand, with automatic checkpoint-and-resume on interruption.
- **SageMaker Distributed Training**: model parallelism (split large models across GPUs) and data parallelism (split dataset across workers) for massive deep learning workloads.

**SageMaker Hyperparameter Tuning (HPO):**
- Automated hyperparameter search using Bayesian optimization, random search, or Hyperband.
- Runs N training jobs in parallel, automatically identifying the configuration that maximizes the objective metric.

**SageMaker Pipelines:**
- Define ML workflows as DAGs (directed acyclic graphs) with steps: Processing → Training → Evaluation → Conditional → RegisterModel → DeployModel.
- Integrates with **SageMaker Model Registry** for model versioning, approval workflows, and metadata management.
- Supports CI/CD for ML (MLOps) via integration with CodePipeline and EventBridge.

##### SageMaker Deployment — Inference Options

| Mode | Latency | Throughput | Best For |
|---|---|---|---|
| **Real-time Endpoint** | Milliseconds | Continuous | User-facing APIs, real-time scoring |
| **Serverless Inference** | ~100ms (cold start) | Bursty | Infrequent traffic, cost-sensitive apps |
| **Asynchronous Inference** | Seconds–minutes | Large payloads | Long-running predictions (video, large doc) |
| **Batch Transform** | Minutes–hours | Massive datasets | Offline scoring without persistent endpoint |

**Multi-variant Endpoints (A/B Testing):**
- Route traffic across multiple model variants at configurable percentages (e.g., 90% ModelV1 / 10% ModelV2).
- Compare production metrics before fully promoting a new model version.

**SageMaker Model Monitor:**
- Continuously monitors real-time endpoints for data quality drift, model quality drift, bias drift, and feature attribution drift.
- Triggers CloudWatch alarms when statistical properties of inference inputs/outputs deviate from the training baseline by more than a configured threshold.

##### AWS AI Services — Pre-Built Intelligence APIs

For many use cases, building a custom ML model is unnecessary. AWS AI Services provide ready-to-use intelligence APIs requiring zero ML expertise:

| Service | Capability | Common Use Cases |
|---|---|---|
| **Amazon Rekognition** | Image and video analysis | Face detection/comparison, object labeling, content moderation, PPE detection |
| **Amazon Textract** | Document text and structure extraction | Invoice processing, medical record digitization, form data extraction |
| **Amazon Comprehend** | Natural language processing (NLP) | Sentiment analysis, entity recognition, topic modeling, PII detection |
| **Amazon Transcribe** | Automatic speech recognition (ASR) | Meeting transcription, call center analytics, subtitle generation |
| **Amazon Polly** | Text-to-speech | Voice-enabled apps, accessibility, audiobook generation |
| **Amazon Translate** | Neural machine translation | Real-time content localization, multilingual chat support |
| **Amazon Forecast** | Time-series forecasting | Demand forecasting, inventory optimization, capacity planning |
| **Amazon Personalize** | ML-based recommendation | Product recommendations, personalized search ranking |

**When to use AI Services vs. Custom Models:**

- **Use AI Services** when: the task is well-defined and covered by an existing API, speed-to-market is critical, labeled training data is unavailable or expensive.
- **Use Custom SageMaker Models** when: domain-specific patterns not covered by generic APIs, proprietary training data provides competitive differentiation, fine-grained control over model behavior is required.

##### Amazon Bedrock — Foundation Models and Generative AI

Amazon Bedrock is a fully managed service that makes leading foundation models (FMs) available via API — without managing any model infrastructure:

**Available Foundation Models:**

| Provider | Models | Strengths |
|---|---|---|
| **Anthropic** | Claude 3 (Haiku, Sonnet, Opus) | Complex reasoning, long context, safety |
| **Amazon** | Titan Text, Titan Embeddings, Titan Image | Cost-effective, AWS-native integration |
| **Meta** | Llama 3 | Open-source, code generation, multilingual |
| **Mistral** | Mistral 7B, Mixtral 8x7B | Efficient reasoning, low latency |
| **Stability AI** | Stable Diffusion XL | Image generation, editing |
| **Cohere** | Command, Embed | Enterprise text, semantic search |

**Key Bedrock Capabilities:**

**Knowledge Bases (Retrieval-Augmented Generation — RAG):**
- Connect FMs to proprietary knowledge bases stored in S3.
- Bedrock automatically chunks documents, generates embeddings, and stores them in an integrated vector store (Amazon OpenSearch Serverless or Aurora pgvector).
- At query time, the relevant document chunks are retrieved and injected into the FM prompt as context — enabling accurate, grounded responses without hallucination.

**Bedrock Agents:**
- Create multi-step autonomous agents that can reason, plan, call APIs (via Lambda action groups), and complete complex tasks.
- Agent orchestrates: Reasoning → Tool Call → Observation → Next Step (ReAct pattern).
- Example: a customer service agent that looks up order status (API call), processes refund requests (API call), and drafts responses in the user's language (FM).

**Bedrock Guardrails:**
- Apply content filtering policies: block harmful topics, PII redaction, profanity filtering, grounding checks (detect hallucinations).
- Enforce topic restrictions (e.g., prevent the model from discussing competitor products).
- Apply consistently across all FM interactions in the account.

**Fine-tuning and Continued Pre-training:**
- Upload domain-specific training data to fine-tune supported FMs on Bedrock with no infrastructure management.
- Useful for adapting a general-purpose model to specialized vocabulary (medical, legal, technical).

##### Hands-On Activities

- Trained a binary classification model using SageMaker's built-in XGBoost algorithm on a CSV dataset from S3; tracked experiments in SageMaker Experiments and compared 5 hyperparameter configurations.
- Used SageMaker Data Wrangler to profile a raw tabular dataset — identified 3 features with >20% missing values and applied median imputation and one-hot encoding transformations.
- Deployed the trained XGBoost model to a real-time SageMaker endpoint and tested predictions via the `invoke_endpoint` Boto3 API.
- Called Amazon Textract on a sample invoice PDF: extracted key-value pairs (vendor name, total amount, date) and validated output accuracy.
- Tested Amazon Comprehend on product review text: extracted sentiment scores, entity mentions, and detected PII.
- Built a RAG application using Amazon Bedrock Knowledge Bases: uploaded a product documentation PDF, configured an OpenSearch Serverless vector store, and queried Claude 3 Sonnet with grounded responses referencing the document.

##### Event: FCAJ Community Day.

Attended a comprehensive event focused on the intersection of Platform Engineering, DevOps, and Generative AI. Sessions explored the role of Platform Engineering in modern cloud ecosystems, showcased GenAIOps using Bedrock Agents and EKS, and provided deep dives into shipping code in the agentic era. The day also featured advanced architectures for Production-Grade Multimodal GenAI using Nova Embeddings and GraphRAG, rounding off with a session on building a robust, secure, and performant edge foundation with Amazon CloudFront.
