# GCP

Google Cloud Platform (GCP) offers a powerful suite of cloud services, from Compute Engine and Kubernetes to AI, serverless, and managed databases. With CloudCTL, you can manage GCP resources seamlessly using a unified, dry-run-safe CLI—without needing the `gcloud` SDK.

Whether you're launching VMs, deploying Kubernetes clusters, or provisioning AI models, CloudCTL brings consistency and simplicity to your GCP workflow.

---

## Supported Resource Types

CloudCTL supports the following core GCP services:

| Resource Type  | Maps To                        | Description                                                   |
| -------------- | ------------------------------ | ------------------------------------------------------------- |
| `vm`         | Compute Engine                 | Virtual machines with custom images, machine types, and zones |
| `k8s`        | GKE (Google Kubernetes Engine) | Managed Kubernetes clusters                                   |
| `database`   | Cloud SQL                      | Managed PostgreSQL, MySQL, and SQL Server instances           |
| `storage`    | Cloud Storage                  | S3-compatible object storage (GCS)                            |
| `serverless` | Cloud Run                      | Fully managed serverless containers                           |
| `ai`         | Vertex AI                      | Deploy and manage AI/ML models at scale                       |

All resources support `deploy`, `list`, and `destroy` operations, with real-time cost estimation and full dry-run safety.

---

## Quick Start

Get started with GCP in four steps.

### 1. Enable Required APIs

In the [Google Cloud Console](https://console.cloud.google.com/), enable:

- Compute Engine API
- Kubernetes Engine API
- Cloud SQL API
- Cloud Storage API
- Cloud Run API
- Vertex AI API

### 2. Create a Service Account

1. Go to **IAM & Admin > Service Accounts**
2. Click **Create Service Account**
3. Assign roles: `Editor` (or granular roles like `Compute Admin`, `Cloud SQL Admin`, etc.)
4. Generate a JSON key file

### 3. Save Credentials Securely

```bash
cloudctl secrets gcp service_account_key
```

Paste the full JSON content when prompted. It is encrypted and stored in your system keyring.

### 4. Configure Project and Region

Edit `~/.cloudctl/config.yaml`:

```YAML
providers:
  gcp:
    enabled: true
    project_id: your-gcp-project-id
    region: us-central1
    zone: us-central1-a
```

---

## Example Commands

### Deploy a Compute Engine VM

```bash
cloudctl deploy vm web-server \
  --provider gcp \
  --type n1-standard-1 \
  --region us-central1 \
  --model ubuntu-2204-lts \
  --name web-vm
```

* Uses default network and firewall rules
* Returns instance ID, internal/external IP, and status

### Create a GKE Kubernetes Cluster

```bash
cloudctl deploy k8s cluster-1 \
  --provider gcp \
  --node_count 3 \
  --machine_type n1-standard-1 \
  --region us-central1
```

* Creates a regional cluster
* Auto-enables HTTP load balancing and monitoring
* Returns cluster name and endpoint

### Launch a Cloud SQL PostgreSQL Instance

```bash
cloudctl deploy database prod-db \
  --provider gcp \
  --engine postgres \
  --type db-n1-standard-1 \
  --region us-central1 \
  --name prod-db-instance
```

* Auto-generates root password
* Enables automatic backups and high availability
* Returns connection string and status

### Create a Cloud Storage Bucket

```bash
cloudctl deploy storage user-assets \
  --provider gcp \
  --name my-assets-bucket \
  --region us-central1
```

* Creates a globally unique bucket
* Sets uniform bucket-level access
* Returns bucket URL

### Deploy a Cloud Run Service

```bash
cloudctl deploy serverless api-service \
  --provider gcp \
  --image us-docker.pkg.dev/cloudrun/container/hello \
  --region us-central1 \
  --env API_KEY=secret123
```

* Deploys a container image
* Auto-scales to zero
* Returns service URL

### Deploy a Vertex AI Model

```bash
cloudctl deploy ai text-bison \
  --provider gcp \
  --model text-bison@001 \
  --region us-central1 \
  --name text-embedding-model
```

* Deploys a pre-trained or custom model
* Enables online prediction endpoint
* Returns model ID and status

---

## List Resources

```bash
# List VMs
cloudctl list vm --provider gcp

# List GKE clusters
cloudctl list k8s --provider gcp

# List Cloud SQL instances
cloudctl list database --provider gcp

# List Cloud Run services
cloudctl list serverless --provider gcp
```

Output includes:

* Resource ID
* Name
* Status
* Region/Zone
* Machine/Instance Type
* Creation Time

---

## Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy instance-123 --provider gcp
cloudctl destroy cluster-1 --provider gcp
cloudctl destroy prod-db-instance --provider gcp
```

> ⚠️ Warning: Deletion is irreversible. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy instance-123 --provider gcp --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on GCP pricing:

```bash
cloudctl cost --provider gcp
```

Example output:

```bash
Provider: gcp
Resource: web-vm (vm)
Type: n1-standard-1
Estimated Monthly Cost: $28.00 USD

Resource: prod-db-instance (database)
Type: db-n1-standard-1
Estimated Monthly Cost: $45.00 USD
```

Estimates include compute, storage, and networking where applicable.

---

## Authentication & Security

CloudCTL uses GCP Service Account keys (JSON) for authentication. Never use user credentials.

* Credentials are encrypted via system keyring
* IAM roles enforce least-privilege access
* JSON key file is never stored in plaintext

---

## Best Practices

* Use dedicated service accounts with minimal required permissions
* Rotate keys regularly
* Enable Audit Logs in Cloud Audit Logs
* Use `--dry-run` before production changes
* Tag resources with `--labels key=value`
* Store secrets in Secret Manager, not environment variables
