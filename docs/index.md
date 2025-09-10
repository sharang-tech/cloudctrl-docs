---
title: Introduction
summary: What is CloudCTRL?
authors:
    - Waylan Limberg
    - Tom Christie
date: 2018-07-10
some_url: https://example.com
---
# CloudCTRL

In a world where infrastructure is fragmented across dozens of platforms—AWS, GCP, DigitalOcean, RunPod, Vercel, Railway, and more—**CloudCTL stands as a unifying force**. It is not just another command-line tool. It is a **universal interface** to the entire modern infrastructure stack, designed for developers, DevOps engineers, and AI practitioners who demand **simplicity, consistency, and control**—without sacrificing power.

CloudCTL is built on a simple but revolutionary idea: **one CLI to deploy, manage, and monitor everything**. Whether you're launching a GPU-powered AI model on RunPod, spinning up a PostgreSQL database on Neon, deploying a full-stack app on Koyeb, or provisioning a Kubernetes cluster on Alibaba Cloud, CloudCTL provides a **single, predictable, and dry-run-safe** command language across all providers.

No more switching between dashboards, learning new APIs, or writing custom scripts for each platform. With CloudCTL, you speak one language—and it understands them all.

## Supported Providers

CloudCTL supports **25+ providers** across cloud compute, AI/ML, managed databases, Kubernetes, and frontend hosting. Each is deeply integrated—not just wrapped—ensuring full access to core capabilities like VMs, databases, storage, serverless functions, and networking.

This is not a minimal abstraction. It is **deep integration**:

### Cloud & Compute

- **AWS** – EC2, RDS, EKS, S3, ELB, CloudFront
- **Google Cloud (GCP)** – Compute Engine, GKE, Cloud SQL, Cloud Run, Vertex AI
- **DigitalOcean** – Droplets, Kubernetes (DOKS), Managed Databases, Spaces
- **Linode** – Linodes, LKE, Object Storage, NodeBalancers
- **Vultr** – Cloud Instances, Bare Metal, Kubernetes, Object Storage
- **Alibaba Cloud** – ECS, ACK, RDS, OSS
- **Tencent Cloud** – CVM, TKE, CDB, COS
- **Huawei Cloud** – ECS, CCE, RDS, OBS
- **Scaleway** – Instance, Kapsule, RDB, Object Storage

### AI & GPU Computing

- **RunPod** – GPU Pods, Serverless, Persistent Storage
- **Vast.ai** – GPU Instances, Storage, Renting
- **Modal** – Serverless Python Functions, Volumes
- **Together.ai** – Inference API (Llama, Mistral, etc.)
- **Fireworks.ai** – High-performance AI inference
- **Fal.ai** – Serverless AI functions

### Databases & Data

- **Aiven** – PostgreSQL, Kafka, Redis, ClickHouse, Grafana
- **PlanetScale** – Serverless MySQL with branching
- **Neon** – Serverless PostgreSQL with autoscaling
- **Render** – PostgreSQL, Redis instances
- **Fly.io** – PostgreSQL, Redis clusters

### Frontend & App Hosting

- **Vercel** – Git-based frontend deployments
- **Netlify** – Static sites and functions
- **Render** – Full-stack apps, web services
- **Koyeb** – Git-driven apps and workers
- **Railway** – Full-stack projects with databases
- **Fly.io** – Global apps and services

With this breadth, CloudCTL becomes the **central nervous system** of your infrastructure—whether you're a solo developer or a platform team managing hundreds of services.

## Integrations

Infrastructure changes should never happen in silence. CloudCTL integrates natively with **Slack** and **Discord** to deliver real-time notifications when resources are deployed, destroyed, or scaled.

### Slack Integration

Configure incoming webhooks in your workspace and add them to `~/.cloudctl/config.yaml`. Every `deploy` or `destroy` triggers a formatted message with:

- Resource type and ID
- Provider and region
- Timestamp and status
- Cost estimate (when available)

Perfect for ops channels and incident response.

### Discord Integration

Similarly, use Discord webhooks to post alerts to your developer server. Messages include embeds with color-coded status (green for success, red for errors), making it easy to monitor infrastructure at a glance.

Both integrations respect `--dry-run`, sending preview messages so you can test workflows safely.

This is **observability by default**—no extra tools, no custom scripts.

## Team

CloudCTL is developed by **Sharang Tech Labs**, a next-generation infrastructure company dedicated to simplifying the complexity of modern cloud environments.

We believe that the future of infrastructure is not more tools, but **fewer, smarter ones**. Tools that don’t just automate, but unify. Tools that empower developers to move fast without breaking things.

Founded in 2024, InfraOne brings together engineers from cloud, AI, and DevOps backgrounds to build open, accessible, and powerful infrastructure solutions. CloudCTL is our first product—a CLI that doesn’t just manage infrastructure, but **reimagines how we interact with it**.

We are committed to:

- **Open design** – No vendor lock-in
- **Security** – Credentials stored in system keyring
- **Transparency** – Full dry-run support, cost tracking
- **Extensibility** – Plugin system for custom providers

CloudCTL is not just a tool. It is a **vision** — of a world where infrastructure is simple, consistent, and under your control.
