---
title: "Event 2"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Cloud Mastery 2026 - #1 AI From Scratch

**Date:** March 14th, 2026  
**Location:** Bitexco Financial Tower (26th Floor), No. 2, Hai Trieu Street, Sai Gon Ward, Ho Chi Minh City  

---

### About the Event

The "Cloud Mastery 2026 - #1 AI From Scratch" event delivered a comprehensive exploration of modern artificial intelligence and cloud-native architecture. Across three distinct sessions, industry experts broke down complex concepts ranging from autonomous AI agents to hardware-software AIoT integrations. 

#### 09:00 AM - 10:00 AM | Building AI Agent with Strands
**Speaker:** Banh Cam Vinh

This session addressed the limits of standalone LLMs and explored how to connect them to the real world using external tools. 

![MCP Demo](/images/z7672910003036_ed196b198e5a6f34e85285e46f6acef3.jpg)
![MCP Demo](/images/z7672909965679_02a0afa0fc4855bf7e543dbd8d430037.jpg)
![MCP Demo](/images/z7672910039736_54beec9e84171db16c7545910e9a6f0f.jpg)

**Key Takeaways:**
* **What Makes an AI Agent:** AI Agents are characterized by their ability to perform multi-step reasoning to execute complex workflows.
* **Agent Capabilities:** They feature autonomous behavior for decision-making and taking actions, tool integration for accessing APIs and databases, and real-time data access for fetching current information.
* **Dynamic Adaptation:** Agents provide dynamic responses that adapt based on the current context.
* **Strands Agents:** This framework enables the creation of agents using built-in tools, system prompts, an "Agentic Loop" for tool calling, and knowledge bases.

---

#### 10:00 AM - 11:00 AM | Automated Prompt Engineering: Enhancing LLM Output Quality
**Speaker:** Nguyen Tuan Thinh, DevOps Engineer

The second session focused on "The Art of Communicating with AI" and why prompt engineering is critical for cost and quality management.

![MCP Demo](/images/z7672922380929_2f993a44dd8f090fdf1de88125c166a0.jpg)
![MCP Demo](/images/z7672909984608_fcc5f42c25152e84cf4fcc9b2b5ed07e.jpg)

**Key Takeaways:**
* **The Cost of Poor Prompts:** Generic prompts lead to generic outputs, while wasted tokens result in increased costs. For context, the Claude Opus 4.6 model costs $5.00 per 1M input tokens and $25.00 per 1M output tokens.
* **Prompt Anatomy:** A highly effective prompt should include a Role, Instruction, Context, Input Data, Output Format, Examples, and Constraints.
* **Advanced Techniques:** Users should leverage methods like Chain-of-Thought (CoT) Prompting, Self-Consistency, Tree-of-Thoughts (ToT), Retrieval-Augmented Generation (RAG), and Role Prompting.
* **Proptimizer Showcase:** The speaker introduced "Proptimizer," a browser extension that optimizes prompts and allows chatting with AI anywhere on the web. The tool's architecture runs entirely on AWS, utilizing Amazon CloudFront and S3 for frontend delivery, Cognito for authentication, API Gateway and Lambda for backend logic, DynamoDB for storage, and Amazon Bedrock for AI integration.

---

#### 11:00 AM - 12:00 PM | AIoT Projects: Locker Management & Plutus
**Speakers:** Aiden Dinh (Operation Engineer at Katalon) and Tran Vu Bao Ngoc

The final session demonstrated practical applications combining Artificial Intelligence with the Internet of Things (AIoT).

![MCP Demo](/images/z7672910019188_393fdd830b5a921571353ab19025e0a2.jpg)

**Key Takeaways:**
* **Locker Management System:** Designed to replace redundant manual processes, this automated system allows club members to borrow items seamlessly. 
* **Hardware Integration:** The system uses a Raspberry Pi as the main controller and MQTT Broker, and an Arduino to collect data from sensors, including an RFID card reader for item IDs and a Reed Switch to detect locker openings.
* **Cloud Architecture:** The hardware connects securely to AWS IoT Core, which routes sensor events to Lambda, DynamoDB, and S3.
* **AI Integration:** AWS Rekognition is triggered to perform facial recognition on captured images, returning similarity confidence scores to identify the club member accessing the locker.
* **Plutus App:** An extra demonstration featured "Plutus," a Financial Budget App.