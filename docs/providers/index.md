# Providers

CloudCTL supports **25+ providers** across cloud computing, AI/ML, managed databases, Kubernetes, and frontend hosting. Each provider is deeply integrated—supporting full lifecycle management with `deploy`, `list`, and `destroy` operations, real-time cost estimation, and dry-run safety.

Below is a comprehensive index of all supported providers, grouped by category, with supported actions, resource types, and links to detailed guides.

---

## ☁️ Cloud & Compute

Providers offering virtual machines, bare metal, storage, and networking.

### [AWS](aws.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (EC2)
  - `database` (RDS)
  - `k8s` (EKS)
  - `storage` (S3)
  - `load-balancer` (ELB)
  - `cdn` (CloudFront)
- **Authentication**: `access_key_id`, `secret_access_key`
- [→ AWS Guide](aws.md)

### [Google Cloud (GCP)](gcp.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (Compute Engine)
  - `k8s` (GKE)
  - `database` (Cloud SQL)
  - `storage` (Cloud Storage)
  - `serverless` (Cloud Run)
  - `ai` (Vertex AI)
- **Authentication**: Service Account JSON key
- [→ GCP Guide](gcp.md)

### [DigitalOcean](digitalocean.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (Droplets)
  - `k8s` (DOKS)
  - `database` (Managed DBs)
  - `storage` (Spaces)
  - `load-balancer`
  - `cdn`
- **Authentication**: `api_token`
- [→ DigitalOcean Guide](digitalocean.md)

### [Linode](linode.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (Linodes)
  - `k8s` (LKE)
  - `database` (Managed DBs)
  - `storage` (Object Storage)
  - `load-balancer` (NodeBalancers)
  - `domain` (DNS)
- **Authentication**: `api_key`
- [→ Linode Guide](linode.md)

### [Vultr](vultr.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (Cloud Instances)
  - `bare-metal`
  - `k8s` (VKE)
  - `database` (PostgreSQL, Redis)
  - `storage` (Object Storage)
- **Authentication**: `api_key`
- [→ Vultr Guide](vultr.md)

### [Alibaba Cloud](alibaba.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (ECS)
  - `k8s` (ACK)
  - `database` (RDS)
  - `storage` (OSS)
  - `serverless` (Function Compute)
- **Authentication**: `access_key_id`, `secret_access_key`
- [→ Alibaba Cloud Guide](alibaba.md)

### [Tencent Cloud](tencent.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (CVM)
  - `k8s` (TKE)
  - `database` (CDB)
  - `storage` (COS)
  - `serverless` (SCF)
- **Authentication**: `secret_id`, `secret_key`
- [→ Tencent Cloud Guide](tencent.md)

### [Huawei Cloud](huawei.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (ECS)
  - `k8s` (CCE)
  - `database` (RDS)
  - `storage` (OBS)
  - `serverless` (FunctionGraph)
- **Authentication**: `access_key`, `secret_key`
- [→ Huawei Cloud Guide](huawei.md)

### [Scaleway](scaleway.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `vm` (Instance)
  - `k8s` (Kapsule)
  - `database` (RDB)
  - `storage` (Object Storage)
  - `serverless` (Functions)
- **Authentication**: `access_key`, `secret_key`, `organization_id`
- [→ Scaleway Guide](scaleway.md)

---

## 🤖 AI & GPU Computing

Providers specializing in AI model inference, training, and GPU-powered workloads.

### [RunPod](runpod.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `ai` (GPU Pods)
  - `serverless` (Serverless Functions)
  - `storage` (Volumes)
  - `worker` (Private Workers)
- **Authentication**: `api_key`
- [→ RunPod Guide](runpod.md)

### [Vast.ai](vast.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `ai` (GPU Instances)
  - `storage` (Persistent Volumes)
  - `bid` (Rent Bids)
- **Authentication**: `api_key`
- [→ Vast.ai Guide](vast.md)

### [Modal](modal.md)

- **Supported Actions**: `deploy`, `list`, `cost`
- **Resource Types**:
  - `ai` (Serverless Functions)
  - `storage` (Volumes)
- **Authentication**: `token_id`, `token_secret`
- [→ Modal Guide](modal.md)

### [Together.ai](together.md)

- **Supported Actions**: `deploy`, `cost`
- **Resource Types**:
  - `ai` (Inference API)
- **Authentication**: `api_key`
- [→ Together.ai Guide](together.md)

### [Fireworks.ai](fireworks.md)

- **Supported Actions**: `deploy`, `cost`
- **Resource Types**:
  - `ai` (High-performance Inference)
- **Authentication**: `api_key`
- [→ Fireworks.ai Guide](fireworks.md)

### [Fal.ai](fal.md)

- **Supported Actions**: `deploy`, `cost`
- **Resource Types**:
  - `ai` (Serverless AI Functions)
- **Authentication**: `api_key`
- [→ Fal.ai Guide](fal.md)

---

## 💾 Databases & Data

Providers offering managed databases, streaming platforms, and observability tools.

### [Aiven](aiven.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `database` (PostgreSQL, MySQL, Redis)
  - `streaming` (Kafka)
  - `analytics` (ClickHouse, InfluxDB)
  - `observability` (Grafana)
- **Authentication**: `auth_token`, `project`
- [→ Aiven Guide](aiven.md)

### [PlanetScale](planetscale.md)

- **Supported Actions**: `deploy`, `list`, `cost`
- **Resource Types**:
  - `database` (MySQL)
  - `branch` (Database Branch)
- **Authentication**: `api_token`, `organization`
- [→ PlanetScale Guide](planetscale.md)

### [Neon](neon.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `database` (PostgreSQL)
  - `branch` (Branch)
  - `endpoint` (Compute Endpoint)
- **Authentication**: `api_key`
- [→ Neon Guide](neon.md)

### [Render](render.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `frontend` (Web Services)
  - `worker` (Background Jobs)
  - `database` (PostgreSQL, Redis)
  - `service` (Private Services)
- **Authentication**: `api_token`
- [→ Render Guide](render.md)

### [Fly.io](fly.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `app` (Fly Apps)
  - `vm` (Machines)
  - `database` (PostgreSQL, Redis)
  - `storage` (Volumes)
- **Authentication**: `access_token`
- [→ Fly.io Guide](fly.md)

---

## 🌐 Frontend & App Hosting

Git-driven platforms for deploying websites, full-stack apps, and APIs.

### [Vercel](vercel.md)

- **Supported Actions**: `deploy`, `list`, `cost`
- **Resource Types**:
  - `frontend` (Static & Serverless Apps)
- **Authentication**: `api_token`
- **Notes**: GitHub account must be linked in dashboard
- [→ Vercel Guide](vercel.md)

### [Netlify](netlify.md)

- **Supported Actions**: `deploy`, `list`, `cost`
- **Resource Types**:
  - `frontend` (Sites)
- **Authentication**: `api_token`
- **Notes**: GitHub account must be linked in dashboard
- [→ Netlify Guide](netlify.md)

### [Koyeb](koyeb.md)

- **Supported Actions**: `deploy`, `list`, `destroy`, `cost`
- **Resource Types**:
  - `app` (Web Services)
  - `worker` (Background Workers)
  - `storage` (Volumes)
  - `domain` (Custom Domains)
- **Authentication**: `api_token`
- [→ Koyeb Guide](koyeb.md)

### [Railway](railway.md)

- **Supported Actions**: `deploy`, `list`, `cost`
- **Resource Types**:
  - `app` (Projects)
  - `database` (PostgreSQL, Redis)
  - `domain` (Custom Domains)
- **Authentication**: `api_token`
- **Notes**: Deletion not supported via API
- [→ Railway Guide](railway.md)

---

This index represents the full scope of CloudCTL’s universal infrastructure control. Each provider guide includes setup instructions, CLI examples, and integration details.
