---
title: "Event 3"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# FCAJ Community Day

**Date:** March 21th, 2026  
**Location:** Bitexco Financial Tower (26th Floor), No. 2, Hai Trieu Street, Sai Gon Ward, Ho Chi Minh City  

---

### About the Event

This event brought together cloud architecture and AI experts to discuss optimizing DevOps infrastructure, establishing Platform Engineering, and deploying cutting-edge Generative AI applications. Below is a detailed summary broken down by presentation session.

#### 09:00 AM - 09:45 AM | Platform Engineering at GoTymeX 
**Speaker:** Phuc (Jason) Dang - Cloud Solution Architect

The opening session highlighted GoTymeX's strategic shift from "building the bank" to "running the platform," aiming to deliver replicable results customized for individual markets.

![MCP Demo](images/z7642514434225_e5c0049a164571b2c5dff1b6b781977b.jpg)
![MCP Demo](images/z7642512856250_ced471a157a60f23945de34edaaad1bb.jpg)
![MCP Demo](images/z7642512864463_b7ce01fea70935945c9da143b97de85d.jpg)
![MCP Demo](images/z7642512868983_cd84e0178c31dd5cccaf834ebe629da3.jpg)
![MCP Demo](images/z7642512874019_64df59f80b2f8c6fa04a25814a7d7ac5.jpg)

**Key Takeaways:**
* **Platform Engineering Model:** Divided into clear functional layers:
  * **Feature Teams:** Full-stack product teams (Alpha, Thanos, Saitama, Hawkeye) leveraging TymePipeline to build applications end-to-end.
  * **Application Platform Services:** Provides DevOps/SRE, Security & Compliance, and Integration Services.
  * **Platform Core & Cloud Enablement:** Manages AWS, Databricks, and Kafka resources while providing Integration Patterns and reference architectures.
* **"How We Work" Culture:**
  * **Self-Service:** Operating not as a support center, but encouraging the use of Runbooks and pair programming.
  * **Product Thinking:** Treating internal teams as customers through co-design and iteration.
  * **Learn & Share:** Promoting cross-functional rotations and TechTalks.

---

#### 09:45 AM - 10:30 AM | GenAIOps - Essential DevOps for Generative Applications 
**Speaker:** Nghi Danh, Phong Nguyen

The second session (hosted at Bitexco, HCMC) delved into applying DevOps philosophies to the Generative AI application development lifecycle.

![MCP Demo](images/z7642514437847_6881e24e6414a3ef9a58f3bf33892b96.jpg)
![MCP Demo](images/z7642514444867_4c00d02ce953abb6d8df7800a6f72393.jpg)
![MCP Demo](images/z7642512886902_a04daa19806c6cdd93fdb25a24e84bf2.jpg)
![MCP Demo](images/z7642512891608_38e715d657e377bc877c77eab0964ed0.jpg)
![MCP Demo](images/z7642512898776_85c31cd93fdf206650b81229d2ae851d.jpg)
![MCP Demo](images/z7642512903177_1f6a1b24fb292dab599aef54f0b3b4a4.jpg)
![MCP Demo](images/z7642512927086_026148c16b2427cd6aeb6b2ff6bf985d.jpg)

**Key Takeaways:**
* **DevOps & CI/CD Foundation:** DevOps revolves around 5 core capabilities (CI, CD, IaC, Monitoring, Collaboration). CI/CD is automated using AWS CodePipeline, CodeBuild, and CloudFormation to ship code from GitHub to AWS ECS.
* **Source Management with Git Worktree:** A technique to solve multi-tasking challenges by linking multiple branches (main, feature, hotfix) to independent working directories while sharing a single `.git` repository.
* **GenAIOps Pipeline:** A continuous cycle including: Scope use-case -> Model selection -> Model adaptation (Prompt engineering, RAG) -> Application integration -> Deployment -> Monitoring and iterative improvement.
* **AgentCore Architecture:** Enabling AI Agents at scale with an Agent Runtime (containing framework, instructions, local tools), memory management, identity, and third-party observability integration (Langfuse, Datadog).

---

#### 10:30 AM - 11:15 AM | Shipping Code in the Agentic Era 
**Speaker:** Phat Pham

The third session marked the transition from traditional Machine Learning to the era of Foundation Models and Intelligent AI Agents.

![MCP Demo](images/z7642514456387_d6c6c311b9852aeb4270eca38d8fb4b5.jpg)
![MCP Demo](images/z7642647579328_7b9e9cb8419d219babd0b5586335a1e5.jpg)
![MCP Demo](images/z7642647591188_b436ed0c937182ab9abb55ccb4d3a7a3.jpg)
![MCP Demo](images/z7642647591912_71d1624cadc6fa839cec914e5dc5c6ec.jpg)
![MCP Demo](images/z7642647598387_4273b9dc6075597f1bc7cd78c4963fec.jpg)
![MCP Demo](images/z7642647608977_ff4b7d0b61e3f27fda219f3e9baa3a0c.jpg)
![MCP Demo](images/z7642647616524_7b33b904f6e2361340ded9caadad6101.jpg)
![MCP Demo](images/z7642647620444_11a47f0908293bf9a9c38adb85c657d1.jpg)
![MCP Demo](images/z7642647626389_36d2611b8e17c232025facff63a036da.jpg)

**Key Takeaways:**
* **Foundation Models Era:** Instead of training isolated ML models for specific tasks, modern AI utilizes massive multi-tasking models (like Amazon Nova, Mistral, DeepSeek on Amazon Bedrock) that adapt with little to no additional training.
* **RAG & Multimodal Search:** Data is pre-processed into vector embeddings and stored in a Vector DB (OpenSearch, Pinecone, etc.). During a query, context is retrieved to feed the LLM. The system supports text, image, video, and audio processing via Amazon Nova Multimodal Embeddings.
* **GraphRAG:** Constructs a Connected Knowledge Graph from isolated data points to support multi-hop reasoning and reduce hallucinations when paired with Amazon Neptune.
* **Serverless AI Agent Architecture:** Building an intelligent agent (utilizing Prompt Templates, LLM, Memory, Tools) in a fully serverless environment using Claude, LangGraph, API Gateway, Lambda, and Amazon SageMaker.

---

#### 11:15 AM - 12:00 PM | The AI Application Stack & Multimodal Search 
**Speaker:** Phap Nguyen

The final session synthesized the big picture of structuring an AI application stack and dived into search algorithms.

![MCP Demo](images/z7642647604975_b89107f0986690cc5ca416f4befbc68b.jpg)
![MCP Demo](images/z7672978703404_666e22c69395b91ca96e98dd6aeaa85a.jpg)
![MCP Demo](images/z7672978717713_ae07ccd07a879c45d2e62936c22cf2c2.jpg)

**Key Takeaways:**
* **The AI Application Stack:** A comprehensive bottom-up architecture:
  * *Hardware/Infra:* Custom Silicon (Graviton5, Trainium3) and the Infrastructure Layer.
  * *Data Processing:* Embeddings layer (multimodal conversion) and Vector Store (semantic retrieval).
  * *High-level Applications:* RAG System (ensuring context and privacy) and AI Agents & Orchestration (managing reasoning and actions).
  * *Monitoring:* A vertical "Rails & Observability" axis protecting the entire stack.
* **Cosine Similarity Algorithm:** Used to rank search results by measuring the distance/similarity between a user's query and the stored data.
  * *Practical Example:* A query for "men's white summer t-shirt" scores highest (0.7325) for the exact match "men's white crewneck cotton t-shirt", while items like "blue jeans" or "sneakers" receive significantly lower scores.
