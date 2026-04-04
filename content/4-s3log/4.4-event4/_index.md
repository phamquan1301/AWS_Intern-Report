---
title: "Event 4"
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---
# Cloud Mastery 2026 - #2

**Time:** April 04, 2026  
**Location:** Lot E2a-7, D1 Street, Hi-Tech Park, Long Thanh My Ward, Thu Duc City, HCMC

---

### Event Introduction

The "Cloud Mastery 2026 - #2" event brought in-depth knowledge about cloud infrastructure automation, container orchestration, and solutions for building highly fault-tolerant systems. Across three sessions, experts guided attendees in detail on how to deploy Infrastructure as Code (IaC), build microservices architectures with Kubernetes, and leverage the power of the Elixir language to optimize performance and operational costs.

#### 09:00 AM - 10:00 AM | Infrastructure as Code with Terraform on AWS
**Speaker:** Thinh Nguyen

The first session addressed the challenges of manual infrastructure management (ClickOps) and delved into the method of managing infrastructure as source code (IaC).

![MCP Demo](images/z7690434959781_5995c1f23896281068e12c8291828764.jpg)

**Key Takeaways:**
* **Limitations of ClickOps:** Prone to human errors, lacks consistency, and causes difficulties in collaboration as well as scaling up.
* **AWS CloudFormation & CDK:** Introduced AWS's built-in IaC tool using YAML/JSON templates, along with the AWS Cloud Development Kit (CDK), which allows defining infrastructure using familiar programming languages through 3 Construct levels (L1, L2, L3).
* **The Power of Terraform:** Explored the open-source, multi-cloud supported tool from HashiCorp. Terraform uses the HCL language, features State Tracking, and makes it easy to manage project directory structures.
* **Basic Commands:** Important operations in Terraform such as `init`, `validate`, `plan`, `apply`, and `destroy` to manage the resource lifecycle.

---

#### 10:00 AM - 11:00 AM | Cloud Architecture with Kubernetes
**Speaker:** Bao Huynh

The second session focused on solving the challenges of operating containers at scale and provided a comprehensive introduction to the Kubernetes orchestration platform.

![MCP Demo](images/z7690435228232_086a56675e6aceba29f20c40dd0784ae.jpg)
![MCP Demo](images/z7690434965400_cae89d3f8e93d1f220c7d1f9b6cf3c53.jpg)
![MCP Demo](images/z7690434968587_e13530a2c89be891b4b71505947256d4.jpg)
![MCP Demo](images/z7690434973622_2f1925766e43a519731b9bdaaee316e6.jpg)
![MCP Demo](images/z7690434979666_8f76d527eb5884f65c86517060490543.jpg)

**Key Takeaways:**
* **Solving the Scaling Problem:** Kubernetes (K8s) automates the deployment, scaling, and self-healing processes for containerized applications.
* **K8s Architecture:** The system is divided into the Control Plane (the brain with etcd, kube-apiserver, kube-scheduler, controller-manager) and Worker Nodes (where applications run with kubelet, kube-proxy, container runtime).
* **Core Objects:** Delved into the functions of Pods (the smallest unit), ReplicaSet, Deployment (supports updates and rollbacks), ConfigMap, Secret (stores sensitive information), and Jobs.
* **Ecosystem Tools:** Instructions on using YAML Manifest files, the `kubectl` command, and introducing supporting tools like Helm (package manager), K9S (terminal UI), along with local K8s runtimes like Minikube, K3S, and K3D.

---

#### 11:00 AM - 12:00 PM | Elixir: A Unified Solution for Highly Fault-Tolerant DevOps Infrastructure
**Speaker:** Nguyen Ta Minh Triet

The final session brought a new perspective on using the functional programming language Elixir on the BEAM virtual machine to build highly concurrent and fault-tolerant systems.

![MCP Demo](images/z7690434656220_0fa9e4ded78c55c5a36cb8a9f541590c.jpg)
![MCP Demo](images/z7690434654020_7a1bcabf3cdd9e589b663ad6cfe4e25b.jpg)
![MCP Demo](images/z7690434643660_bb9295a9bd21e54f1b0a726defda70d5.jpg)

**Key Takeaways:**
* **The Power of BEAM VM:** Elixir inherits its architecture from Erlang, allowing millions of extremely lightweight processes to run independently without sharing memory, helping to optimize concurrent processing.
* **The "Let It Crash" Philosophy:** Instead of using complex `try/catch` blocks for defense, the system uses Supervisors for monitoring. When a worker process crashes, the Supervisor lets it "die" and automatically restarts it, providing excellent Fault-Tolerance.
* **Hot Code Upgrades:** The ability to update the system's source code while it is running without causing any downtime.
* **Real-world Efficiency:** A practical case study showed that migrating a system from a Serverless model (AWS API Gateway + Lambda with Node.js) to Elixir helped reduce massive costs from over $30,000/month to under $400/month while handling tens of millions of requests.
