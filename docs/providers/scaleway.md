# Scaleway

Scaleway is a European cloud provider headquartered in France, known for its commitment to sustainability, GDPR compliance, and high-performance infrastructure. With CloudCTL, you can manage virtual instances, Kubernetes clusters (Kapsule), managed databases (RDB), object storage, and serverless functions—all from a single, consistent CLI.

No more juggling dashboards or writing custom scripts. CloudCTL brings **universal control** to your Scaleway infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Scaleway services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `vm` | Instances | Virtual machines with ARM64 and x86_64 architectures |
| `k8s` | Kapsule | Managed Kubernetes clusters |
| `database` | RDB (Relational Database) | PostgreSQL, MySQL, and Redis instances |
| `storage` | Object Storage | S3-compatible buckets |
| `serverless` | Functions | Event-driven serverless execution |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Scaleway in three steps.

### 1. Generate API Credentials

1. Log in to the [Scaleway Console](https://console.scaleway.com)
2. Go to **IAM → API Keys**
3. Click **Create API Key**
4. Copy the **Access Key** and download the **Secret Key**

> 🔐 The Secret Key is only shown once. Store it securely.

### 2. Save Credentials Securely

```bash
cloudctl secrets scaleway access_key
cloudctl secrets scaleway secret_key
```
You’ll be prompted to enter each value. They are stored encrypted in your system keyring—never in plaintext.

### 3. Set Default Region and Organization
Edit `~/.cloudctl/config.yaml`:

```yaml
providers:
  scaleway:
    enabled: true
    region: fr-par
    organization_id: your-organization-id
```
Replace `your-organization-id` with your actual Scaleway organization ID (UUID).

---

## Example Commands
### Deploy an Instance (VM)
```bash
cloudctl deploy vm web-server \
  --provider scaleway \
  --region fr-par \
  --type DEV1-S \
  --image ubuntu_focal \
  --name my-instance
```

* Uses Scaleway’s public Ubuntu 20.04 image
* Deploys in Paris (`fr-par`) by default
* Returns instance ID, public IP, and status

### Create a Kapsule Kubernetes Cluster
```bash
cloudctl deploy k8s cluster-1 \
  --provider scaleway \
  --region nl-ams \
  --version 1.28 \
  --pools '[{"name":"pool-1","node_type":"gp1_ssd","size":1,"autoscaling":false}]' \
  --name managed-k8s
```

* Fully managed control plane
* Auto-configures networking and CNI
* Returns cluster ID and kubeconfig

### Launch a PostgreSQL Database on RDB
```bash
cloudctl deploy database production-db \
  --provider scaleway \
  --engine PostgreSQL \
  --node_type db-dev-s \
  --region fr-par \
  --name pg-cluster
```

* Enables automated backups and monitoring
* Supports high availability and read replicas
* Returns connection string and status

### Create an Object Storage Bucket
```bash
cloudctl deploy storage user-uploads \
  --provider scaleway \
  --region nl-ams \
  --name my-assets-bucket
```

* Creates a globally unique S3-compatible bucket
* Returns endpoint: `https://my-assets-bucket.s3.nl-ams.scw.cloud`
* Supports CORS and lifecycle rules

### Deploy a Serverless Function
```bash
cloudctl deploy serverless hello-world \
  --provider scaleway \
  --runtime python311 \
  --handler "main.handler" \
  --namespace_id fn123abc-def4-5678-90ab-cdef12345678
```

* Event-driven execution with auto-scaling
* Integrates with Object Storage and VPC
* Returns function ID and invocation URL

---

## List Resources
List any deployed resources:

```bash
# List Instances
cloudctl list vm --provider scaleway

# List Kapsule clusters
cloudctl list k8s --provider scaleway

# List RDB databases
cloudctl list database --provider scaleway

# List Object Storage buckets
cloudctl list storage --provider scaleway
```

Output includes:

* ID
* Name
* Status
* Region
* Type
* Creation Time

---

## Destroy Resources
Safely terminate resources:

```bash
cloudctl destroy 123e4567-e89b-12d3-a456-426614174000 --provider scaleway
cloudctl destroy pg-cluster --provider scaleway
cloudctl destroy my-assets-bucket --provider scaleway
```

>⚠️ Warning: Deletion is irreversible. Use --dry-run first. 

### Dry-Run Mode
Preview changes safely:

```bash
cloudctl destroy 123e4567-e89b-12d3-a456-426614174000 --provider scaleway --dry-run
```

---

## Cost Estimation
CloudCTL estimates monthly costs based on Scaleway’s pricing:

```bash
cloudctl cost --provider scaleway
```

Output
```bash
Provider: scaleway
Resource: my-instance (vm)
Type: DEV1-S
Estimated Monthly Cost: $5.99 USD

Resource: pg-cluster (database)
Type: db-dev-s
Estimated Monthly Cost: $15.00 USD
```

Estimates include compute, storage, and bandwidth.

---

## Authentication & Security
CloudCTL uses Scaleway Access Key + Secret Key with encrypted storage.

* Keys are scoped to your IAM policies
* Full audit log via Activity Log
* Never stored in config files

---

## Best Practices

* Use dedicated API keys for CI/CD and personal access
* Enable two-factor authentication (2FA)
* Use --dry-run before production changes
* Tag resources for organization and billing
* Use private networks (VPC) for internal services