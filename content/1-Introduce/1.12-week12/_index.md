---
title: "Week 12 Worklog"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Week 12 Objectives

- Go beyond Week 11 fundamentals and master the advanced capabilities of Amazon SageMaker for production ML systems.
- Understand how to design and run scalable distributed training jobs for large deep learning models.
- Learn SageMaker MLOps patterns: Pipelines, Model Registry, CI/CD integration, and automated retraining triggers.
- Master SageMaker endpoint scaling strategies: auto-scaling policies, multi-model endpoints, and inference recommender.
- Understand SageMaker Clarify for bias detection and model explainability (SHAP values).
- Learn how to optimize inference costs using SageMaker's Neo model compiler and inference containers.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Studied SageMaker distributed training in depth: data parallelism with SageMaker Distributed Data Parallel (SMDDP), model parallelism with SageMaker Model Parallel (SMP), and training on multi-GPU/multi-node EC2 clusters. | 03/23/2026 | 03/23/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Built a complete SageMaker Pipeline: ProcessingStep (Data Wrangler export) → TrainingStep → EvaluationStep → ConditionStep → RegisterModelStep → DeployStep with approval gates and run metadata tracking. | 03/24/2026 | 03/24/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Configured SageMaker Model Registry: model versioning, manual and automated approval workflows, cross-account model sharing, and integration with EventBridge to trigger downstream deployment pipelines automatically. | 03/25/2026 | 03/25/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Implemented endpoint scaling strategies: Application Auto Scaling policies (target tracking, step scaling), multi-model endpoints (MME) for cost-efficient model hosting, and SageMaker Inference Recommender for optimal instance selection. | 03/26/2026 | 03/26/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Explored SageMaker Clarify: pre-training bias detection on datasets, post-training bias metrics (DPL, DImpact), SHAP-based feature importance for model explainability, and Clarify integration with Model Monitor for continuous explainability drift tracking. | 03/27/2026 | 03/27/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Investigated inference cost optimization: SageMaker Neo for model compilation and hardware-specific optimization, graviton-based inference instances, elastic inference accelerators, and comparing cost-per-inference across deployment configurations. | 03/28/2026 | 03/28/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 12 Achievements

##### Distributed Training at Scale

Training large models on a single instance quickly becomes a bottleneck. SageMaker provides two complementary parallelism strategies:

**Data Parallelism (SageMaker Distributed Data Parallel — SMDDP):**

Each worker node holds a full copy of the model but processes a different subset (shard) of the training data. After each backward pass, gradients are aggregated across all workers using **AllReduce** before the optimizer step:

```
Node 1: Batch A → Forward → Backward → Gradient A ─┐
Node 2: Batch B → Forward → Backward → Gradient B ─┤─► AllReduce ─► Average Gradient ─► Update Weights
Node 3: Batch C → Forward → Backward → Gradient C ─┘
```

- SMDDP uses an optimized AllReduce algorithm (outperforms Horovod by 25% on AWS networking fabric).
- Best for: models that fit in a single GPU's memory, but training data is too large for one machine.
- Activation: add `distribution={'smdistributed': {'dataparallel': {'enabled': True}}}` to the SageMaker Estimator.

**Model Parallelism (SageMaker Model Parallel — SMP):**

Splits the model itself across multiple GPUs — necessary for models too large to fit in a single GPU's memory (e.g., LLMs with billions of parameters):

| Strategy | Description |
|---|---|
| **Tensor Parallelism** | Split individual layer weight matrices across GPUs |
| **Pipeline Parallelism** | Split model layers into stages; each GPU handles a stage |
| **Optimized Sharding (ZeRO)** | Shard optimizer states, gradients, and parameters across GPUs |

- SMP v2 supports Hugging Face Transformers and PyTorch FSDP natively.
- Enables training 70B+ parameter models on `p4d.24xlarge` (8x A100 GPU) or `p5.48xlarge` (8x H100 GPU) instances.

##### SageMaker Pipelines — MLOps Automation

A production SageMaker Pipeline is a DAG of interconnected steps that automates the full ML workflow from raw data to deployed model:

```
S3 Raw Data
    │
    ▼
ProcessingStep (Data Wrangler / SKLearn script)
    │  Outputs: train.csv, validation.csv, test.csv
    ▼
TrainingStep (XGBoost / PyTorch Training Job)
    │  Outputs: model.tar.gz
    ▼
EvaluationStep (Processing Job: compute metrics on test set)
    │  Outputs: evaluation.json { "f1": 0.93, "auc": 0.97 }
    ▼
ConditionStep (if F1 >= 0.90)
    ├── [True]  → RegisterModelStep (Model Registry — PendingApproval)
    │               └── [Approved] → DeployStep (SageMaker Endpoint)
    └── [False] → FailStep (notify team via SNS)
```

**Pipeline Execution Properties:**
- Pipelines are **version-controlled** — each run creates an immutable execution record with metadata (parameters, metrics, artifact lineage).
- **Caching**: steps whose input artifacts have not changed are skipped on re-run, significantly speeding up iterative experimentation.
- **Parameters**: configurable at execution time (`TrainingInstanceType`, `ModelApprovalStatus`, `F1Threshold`) without modifying the pipeline definition.
- **CI/CD Integration**: pipeline runs can be triggered by CodePipeline on repository commits or via EventBridge schedules for automated retraining.

##### SageMaker Model Registry — Model Governance

Model Registry provides a centralized catalog of all trained model versions, enabling governance and controlled deployment:

**Model Package Groups:**
- Each model package group corresponds to a use case (e.g., `FraudDetectionModel`, `ChurnPredictionModel`).
- Multiple model versions live under the same group, each with metadata, metrics, and artifact references.

**Approval Workflow:**
- New model versions registered with status `PendingManualApproval`.
- A human reviewer (data scientist, ML engineer) evaluates metrics in the Registry UI and sets status to `Approved` or `Rejected`.
- **Automated approval**: a Lambda function triggered by an EventBridge rule evaluates model metrics programmatically and auto-approves models meeting quality thresholds (e.g., F1 > 0.90).

**Cross-Account Deployment:**

```
Account A (Dev)          Account B (Staging)        Account C (Prod)
  ↓ Register model        ↓ Pull approved model       ↓ Pull approved model
  SageMaker Registry  →→→ Cross-account IAM role  →→→ SageMaker Endpoint
```

Model Registry supports cross-account model sharing via resource-based policies — a critical pattern for ML platform teams managing models across organizational unit boundaries.

##### Endpoint Scaling Strategies

**Application Auto Scaling:**

SageMaker endpoints integrate natively with Application Auto Scaling:

- **Target Tracking**: maintain `SageMakerVariantInvocationsPerInstance` at a target value (e.g., 1000 invocations/min/instance). Auto Scaling adds or removes instances automatically.
- **Step Scaling**: define discrete scaling steps triggered when metrics cross specific thresholds (aggressive scale-out, conservative scale-in).
- **Scheduled Scaling**: pre-scale for known traffic spikes (e.g., scale to 10 instances at 08:00 Monday, scale down to 2 at 22:00).

**Multi-Model Endpoints (MME):**

Host thousands of models on a single endpoint, dramatically reducing hosting costs for multi-tenant or per-user model scenarios:

- Models are stored in S3 and loaded into the container's memory on-demand (LRU eviction when memory is full).
- A single `ml.m5.xlarge` endpoint can serve thousands of models that are not simultaneously hot.
- Billing: pay for a single endpoint instance instead of N individual endpoints.
- Supported frameworks: MXNet, PyTorch, TensorFlow, scikit-learn, XGBoost (via SageMaker pre-built containers).

**SageMaker Inference Recommender:**
- Automated benchmarking tool: tests your model across a configurable set of instance types and batch sizes.
- Outputs a ranked recommendation table: `(instance_type, cost_per_hour, latency_p99, throughput)`.
- Removes guesswork from instance selection — particularly valuable when migrating from on-premises to cloud inference.

##### SageMaker Clarify — Bias Detection and Explainability

Responsible AI requires that models are not only accurate but also fair and interpretable:

**Pre-Training Bias Detection:**

Clarify analyzes the training dataset before model training to surface statistical biases in the data itself:

| Metric | Measures |
|---|---|
| **Class Imbalance (CI)** | Whether the label distribution is balanced across sensitive groups |
| **Difference in Proportions of Labels (DPL)** | How much the label rate differs between privileged and unprivileged groups |
| **Kullback-Leibler Divergence (KL)** | Statistical distance between label distributions across groups |

**Post-Training Bias Metrics:**

After training, Clarify evaluates the model's predictions for bias:

| Metric | Measures |
|---|---|
| **DPPL** | Difference in positive prediction rates between groups |
| **DImpact** | Disparity in positive outcomes relative to the reference group |
| **Accuracy Difference** | Whether the model is equally accurate across demographic groups |

**Model Explainability (SHAP):**

SageMaker Clarify uses **SHAP (SHapley Additive exPlanations)** to explain individual predictions:
- Each feature receives a SHAP value indicating its contribution (positive or negative) to a specific prediction.
- A SHAP waterfall plot shows: `E[f(x)] + feature_1_contribution + feature_2_contribution + ... = f(x)`.
- Global feature importance: aggregate SHAP values across the test set to understand which features overall drive the model.

**Integration with Model Monitor:**
- Clarify can run as a scheduled Model Monitor job on a live endpoint, detecting if the relative importance of features (explainability) is drifting over time — an early warning sign of model staleness.

##### Inference Cost Optimization

**SageMaker Neo — Model Compilation:**

Neo compiles trained models for specific hardware targets (CPU/GPU architectures) to eliminate framework overhead:

- Converts models from TensorFlow, PyTorch, MXNet, XGBoost, sklearn into an optimized binary using LLVM-based compilation.
- Typical speedup: **25–40% lower latency** and **up to 2x throughput** vs. uncompiled models.
- Target hardware: `ml_c5`, `ml_m5`, `ml_g4dn`, AWS Inferentia (INF1/INF2), ARM Graviton.

**AWS Inferentia (Inf1 / Inf2 instances):**
- Custom AWS silicon designed specifically for ML inference.
- `inf2.xlarge` provides 2x AWS Neuron Cores; cost ~40% lower per inference than equivalent `g4dn.xlarge` GPU instance for Transformer models.
- Requires compiling models with the **AWS Neuron SDK** (PyTorch Neuron / TensorFlow Neuron wrappers).

**Graviton-based Inference:**
- `ml.m7g`, `ml.c7g` instances using AWS Graviton3 processors.
- Up to 40% better price-performance for CPU-bound inference models (XGBoost, sklearn, small neural networks) vs. x86 equivalents.

**Cost-Per-Inference Comparison (example, XGBoost model):**

| Configuration | Instance | Cost/hr | Throughput (req/s) | Cost per 1M requests |
|---|---|---|---|---|
| Standard | ml.m5.xlarge | $0.23 | 850 | $0.075 |
| Neo-compiled | ml.m5.xlarge | $0.23 | 1,190 | $0.054 |
| Graviton | ml.m7g.xlarge | $0.17 | 1,050 | $0.045 |
| Multi-Model | ml.m5.xlarge (shared) | $0.023* | 850 | $0.0075* |

*Multi-model cost amortized across 10 concurrently hosted models.

##### Hands-On Activities

- Launched a multi-node PyTorch training job on 2x `ml.p3.8xlarge` instances using SageMaker Distributed Data Parallel; monitored GPU utilization and AllReduce throughput via CloudWatch.
- Built a 6-step SageMaker Pipeline with a ConditionStep gating model registration on F1 ≥ 0.90; triggered the pipeline from CodePipeline on a simulated model code repository commit.
- Registered 3 model versions in Model Registry, configured automated approval via a Lambda evaluator function, and verified EventBridge triggered the deployment pipeline on approval.
- Deployed a Multi-Model Endpoint hosting 50 individual XGBoost models (one per customer segment); measured cold-load latency vs. warm-cache latency per model invocation.
- Ran SageMaker Clarify on a binary classification model: identified 12% DPL bias on the `age_group` feature and generated a SHAP global importance report — top 3 features accounted for 78% of prediction variance.
- Compiled an XGBoost model with SageMaker Neo targeting `ml_m5`; benchmarked 38% latency reduction and 40% throughput improvement vs. the uncompiled counterpart.
