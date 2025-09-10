# Tencent Cloud

Tencent Cloud is one of the largest cloud providers in China and the Asia-Pacific region, powering services like WeChat, QQ, and major gaming platforms. With CloudCTL, you can manage CVM (Cloud Virtual Machines), TKE (Tencent Kubernetes Engine), CDB (Cloud Databases), COS (Cloud Object Storage), and SCF (Serverless Cloud Functions)—all from a single, consistent CLI.

No more switching between dashboards or writing custom API scripts. CloudCTL brings **universal control** to your Tencent Cloud infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Tencent Cloud services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `vm` | CVM (Cloud Virtual Machine) | High-performance virtual machines with flexible instance types |
| `k8s` | TKE (Tencent Kubernetes Engine) | Managed Kubernetes clusters |
| `database` | CDB (Cloud Database) | MySQL, PostgreSQL, SQL Server, and Redis instances |
| `storage` | COS (Cloud Object Storage) | S3-compatible object storage for files, backups, and media |
| `serverless` | SCF (Serverless Cloud Functions) | Event-driven serverless execution |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Tencent Cloud in four steps.

### 1. Generate Secret Keys

1. Log in to the [Tencent Cloud Console](https://console.cloud.tencent.com)
2. Go to **Access Management → API Key Management**
3. Click **Create Key** (under a sub-user with proper permissions)
4. Copy the **SecretId** and **SecretKey**

> 🔐 These will not be shown again. Store them securely.

### 2. Save Credentials Securely

```bash
cloudctl secrets tencent secret_id
cloudctl secrets tencent secret_key
```

You’ll be prompted to enter each value. They are stored encrypted in your system keyring—never in plaintext.

### 3. Set Default Region
Edit `~/.cloudctl/config.yaml`:

```yaml
providers:
  tencent:
    enabled: true
    region: ap-beijing
```

Tencent Cloud regions include `ap-beijing`, `ap-shanghai`, `ap-guangzhou`, `ap-singapore`, and more.

---

## Example Commands
### Deploy a CVM Instance (VM)
```bash
cloudctl deploy vm web-server \
  --provider tencent \
  --region ap-guangzhou \
  --type S2.SMALL1 \
  --image img-ubuntu-20-04-lts-x86-64 \
  --name my-cvm-instance
```

* Uses Tencent’s public Ubuntu image
* Deploys in Guangzhou by default
* Returns instance ID, private/public IP, and status


### Create a TKE Kubernetes Cluster
```bash
cloudctl deploy k8s cluster-1 \
  --provider tencent \
  --region ap-shanghai \
  --version 1.28.6 \
  --node_pools '[{"type":"S2.SMALL2","count":3,"vpc_id":"vpc-12345678","subnet_id":"subnet-87654321"}]' \
  --name managed-k8s
```

* Fully managed control plane
* Auto-configures VPC and networking
* Returns cluster ID and endpoint


### Launch a MySQL Database on CDB
```bash
cloudctl deploy database production-db \
  --provider tencent \
  --engine MySQL \
  --type MYSQL.S2.SMALL \
  --region ap-beijing \
  --name mysql-cluster
```

* Creates a globally unique S3-compatible bucket
* Returns endpoint: `https://my-assets-bucket.cos.ap-singapore.myqcloud.com`
* Supports lifecycle rules, CORS, and versioning


### Deploy a Serverless Function (SCF)
```bash
cloudctl deploy serverless image-resize \
  --provider tencent \
  --runtime Python3.6 \
  --handler "index.main_handler" \
  --code '{"ZipFile": "UEsDBBQACAgIAK2XeWQ..."}'
```

* Event-driven serverless execution
* Auto-scales with traffic
* Integrates with COS, API Gateway, and CMQ

---

## List Resources
List any deployed resources:

```bash
# List CVM instances
cloudctl list vm --provider tencent

# List CDB databases
cloudctl list database --provider tencent

# List TKE clusters
cloudctl list k8s --provider tencent

# List COS buckets
cloudctl list storage --provider tencent
```

Output includes:

* ID
* Name
* Status
* Region
* Instance Type
* Creation Time

---

## Destroy Resources
Safely terminate resources:

```bash
cloudctl destroy ins-12345678 --provider tencent
cloudctl destroy mysql-cluster --provider tencent
cloudctl destroy my-assets-bucket --provider tencent
```

>⚠️ Warning: Deletion is irreversible. Use --dry-run first. 

### Dry-Run Mode
Preview changes safely:

```bash
cloudctl destroy ins-12345678 --provider tencent --dry-run
```

---

## Cost Estimation
CloudCTL estimates monthly costs based on Tencent Cloud’s pricing:

```bash
cloudctl cost --provider tencent
```

Output:
```bash
Provider: tencent
Resource: my-cvm-instance (vm)
Type: S2.SMALL1
Estimated Monthly Cost: $8.64 USD

Resource: mysql-cluster (database)
Type: MYSQL.S2.SMALL
Estimated Monthly Cost: $25.92 USD
```

Estimates include compute, storage, and data transfer.

---

## Authentication & Security
CloudCTL uses Tencent Cloud SecretId + SecretKey with encrypted storage.

* Keys are scoped to a sub-user with IAM policies
* Full audit log via Cloud Audit (CAM)
* Never stored in config files

---

## Best Practices

* Use sub-users with least-privilege CAM policies
* Enable MFA for root account
* Use `--dry-run` before production changes
* Tag resources for cost allocation and organization
* Use private networking (VPC) for internal services