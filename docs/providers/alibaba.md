# Alibaba Cloud

Alibaba Cloud (Aliyun) is the largest cloud provider in China and a global leader in infrastructure, serving millions of businesses across Asia and beyond. With CloudCTL, you can manage Elastic Compute Service (ECS), Kubernetes (ACK), Relational Database Service (RDS), Object Storage Service (OSS), and Serverless functions—all from a single, consistent CLI.

No more juggling the Alibaba Cloud Console or writing custom API scripts. CloudCTL brings **universal control** to your Alibaba infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Alibaba Cloud services:

| Resource Type  | Maps To                                | Description                                                      |
| -------------- | -------------------------------------- | ---------------------------------------------------------------- |
| `vm`         | ECS Instances                          | Virtual machines with flexible instance types and global regions |
| `k8s`        | ACK (Container Service for Kubernetes) | Managed Kubernetes clusters                                      |
| `database`   | RDS (MySQL, PostgreSQL, SQL Server)    | Fully managed relational databases                               |
| `storage`    | OSS (Object Storage Service)           | S3-compatible object storage for files and backups               |
| `serverless` | Function Compute                       | Event-driven serverless functions                                |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Alibaba Cloud in four steps.

### 1. Generate Access Keys

1. Log in to the [Alibaba Cloud Console](https://home.console.aliyun.com)
2. Go to **AccessKey Management** (under your account name)
3. Click **Create Access Key**
4. Download the **AccessKey ID** and **AccessKey Secret**

> 🔐 Store these securely—they will not be shown again.

### 2. Save Credentials Securely

```bash
cloudctl secrets alibaba access_key_id
cloudctl secrets alibaba secret_access_key
```

You’ll be prompted to enter each value. They are stored encrypted in your system keyring—never in plaintext.

### 3. Set Default Region

Edit `~/.cloudctl/config.yaml`:

```yaml
providers:
  alibaba:
    enabled: true
    region: cn-hangzhou
```

Alibaba Cloud uses region IDs like `cn-hangzhou`, `cn-beijing`, `ap-southeast-1`.

---

## Example Commands

### Deploy an ECS Instance (VM)

```bash
cloudctl deploy vm web-server \
  --provider alibaba \
  --region cn-hangzhou \
  --type ecs.t5-lc1m1.small \
  --image ubuntu_20_04_x64_20G_alibase_20230718.vhd \
  --name my-ecs-instance
```

* Uses Alibaba’s public Ubuntu image
* Deploys in Hangzhou by default
* Returns instance ID, private/public IP, and status

### Create an ACK Kubernetes Cluster

```bash
cloudctl deploy k8s cluster-1 \
  --provider alibaba \
  --region cn-beijing \
  --version 1.28.6 \
  --node_pools '[{"type":"ecs.g6.large","count":2,"zone":"cn-beijing-a"}]' \
  --name managed-k8s
```

* Fully managed control plane
* Auto-configures VPC and networking
* Returns cluster ID and endpoint

### Launch a PostgreSQL Database on RDS

```bash
cloudctl deploy database production-db \
  --provider alibaba \
  --engine PostgreSQL \
  --type rds.pg.t1.small \
  --region cn-shanghai \
  --name pg-cluster
```

* Enables automated backups and monitoring
* Supports read replicas and high availability
* Returns connection string and port

### Create an OSS Bucket (Object Storage)

```bash
cloudctl deploy storage user-uploads \
  --provider alibaba \
  --region cn-hangzhou \
  --name my-assets-bucket
```

* Creates a globally unique S3-compatible bucket
* Returns endpoint: `https://my-assets-bucket.oss-cn-hangzhou.aliyuncs.com`
* Supports lifecycle rules and CORS

### Deploy a Function Compute Function

```bash
cloudctl deploy serverless image-resize \
  --provider alibaba \
  --service serverless-service \
  --runtime python3 \
  --handler "index.handler"
```

* Event-driven serverless execution
* Auto-scales with traffic
* Integrates with OSS, API Gateway, and more

---

## List Resources

List any deployed resources:

```bash
# List ECS instances
cloudctl list vm --provider alibaba

# List RDS databases
cloudctl list database --provider alibaba

# List ACK clusters
cloudctl list k8s --provider alibaba

# List OSS buckets
cloudctl list storage --provider alibaba
```

Output includes:

* ID
* Name
* Status
* Region
* Instance Type
* Creation Time

---

### Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy i-1234567890abcdef0 --provider alibaba
cloudctl destroy pg-cluster --provider alibaba
cloudctl destroy my-assets-bucket --provider alibaba
```

> ⚠️ Warning: Deletion is irreversible. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy i-1234567890abcdef0 --provider alibaba --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Alibaba Cloud’s pricing:

```bash
cloudctl cost --provider alibaba
```

Output:

```bash
Provider: alibaba
Resource: my-ecs-instance (vm)
Type: ecs.t5-lc1m1.small
Estimated Monthly Cost: $7.20 USD

Resource: pg-cluster (database)
Type: rds.pg.t1.small
Estimated Monthly Cost: $28.00 USD
```

Estimates include compute, storage, and data transfer.

---

## Authentication & Security

CloudCTL uses Alibaba Cloud Access Key ID + Secret with encrypted storage.

* Keys are scoped to your RAM user
* Full audit log via ActionTrail
* Never stored in config files

---

## Best Practices

* Use RAM users with least-privilege policies
* Enable MFA for root account
* Use `--dry-run` before production changes
* Tag resources for cost allocation
* Use VPCs for secure internal networking
