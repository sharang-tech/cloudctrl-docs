# Huawei Cloud

Huawei Cloud (Huawei Cloud) is a leading cloud provider in China and globally, offering a comprehensive suite of services including computing, AI, databases, and networking. With CloudCTL, you can manage Elastic Cloud Servers (ECS), Cloud Container Engine (CCE), Relational Database Service (RDS), Object Storage Service (OBS), and FunctionGraph—all from a single, consistent CLI.

No more juggling dashboards or writing custom API scripts. CloudCTL brings **universal control** to your Huawei Cloud infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Huawei Cloud services:

| Resource Type  | Maps To                           | Description                                                      |
| -------------- | --------------------------------- | ---------------------------------------------------------------- |
| `vm`         | ECS (Elastic Cloud Server)        | Virtual machines with flexible instance types and global regions |
| `k8s`        | CCE (Cloud Container Engine)      | Managed Kubernetes clusters                                      |
| `database`   | RDS (Relational Database Service) | MySQL, PostgreSQL, SQL Server, and SQL Server instances          |
| `storage`    | OBS (Object Storage Service)      | S3-compatible object storage for files and backups               |
| `serverless` | FunctionGraph                     | Event-driven serverless functions                                |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Huawei Cloud in four steps.

### 1. Generate Access Keys

1. Log in to the [Huawei Cloud Console](https://console.huaweicloud.com)
2. Go to **IAM → Users → Your Account**
3. Click **Create Access Key**
4. Download the **Access Key ID** and **Secret Access Key**

> 🔐 Store these securely—they will not be shown again.

### 2. Save Credentials Securely

```bash
cloudctl secrets huawei access_key
cloudctl secrets huawei secret_key
```

You’ll be prompted to enter each value. They are stored encrypted in your system keyring—never in plaintext.

### 3. Set Default Region

Edit `~/.cloudctl/config.yaml`:

```yaml
providers:
  huawei:
    enabled: true
    region: cn-north-4
```

Huawei Cloud regions include `cn-north-4` (Beijing), `cn-east-3` (Shanghai), `ap-southeast-3` (Singapore), and more.

---

## Example Commands

### Deploy an ECS Instance (VM)

```bash
cloudctl deploy vm web-server \
  --provider huawei \
  --region cn-north-4 \
  --type s3.small.1 \
  --image Standard_CentOS_7_latest \
  --name my-ecs-instance \
  --vpc_id vpc-12345678 \
  --subnet_id subnet-87654321
```

* Uses Huawei’s public CentOS image
* Requires VPC and subnet IDs
* Returns instance ID, private/public IP, and status

### Create a CCE Kubernetes Cluster

```bash
cloudctl deploy k8s cluster-1 \
  --provider huawei \
  --region cn-east-3 \
  --version v1.25 \
  --node_pools '[{"type":"s6.large.2","count":2,"root_volume":{"size":40,"volumetype":"SATA"},"data_volumes":[{"size":100,"volumetype":"SAS"}]}]' \
  --name managed-k8s
```

* Fully managed control plane
* Auto-configures networking and storage
* Returns cluster ID and endpoint

### Launch a PostgreSQL Database on RDS

```bash
cloudctl deploy database production-db \
  --provider huawei \
  --engine postgresql \
  --type rds.pg.s1.small \
  --region cn-north-4 \
  --name pg-cluster
```

* Enables automated backups, monitoring, and read replicas
* Supports high availability and disaster recovery
* Returns connection string and port

### Create an OBS Bucket (Object Storage)

```bash
cloudctl deploy storage user-uploads \
  --provider huawei \
  --region cn-north-4 \
  --name my-assets-bucket
```

* Creates a globally unique S3-compatible bucket
* Returns endpoint: `https://my-assets-bucket.obs.cn-north-4.myhuaweicloud.com`
* Supports lifecycle rules and CORS

### Deploy a FunctionGraph Function

```bash
cloudctl deploy serverless hello-world \
  --provider huawei \
  --runtime Python3.6 \
  --handler "main.handler" \
  --code_type inline \
  --func_code "aW1wb3J0IGpzb24KZGVmIGhhbmRsZXIgKGV2ZW50LCBjb250ZXh0KTogICAgIAogICAgcmV0dXJuIHsiYm9keSI6ICJIZWxsbyBXb3JsZCIpfQ=="
```

* Event-driven serverless execution
* Auto-scales with traffic
* Integrates with API Gateway, OBS, and SMN

---

## List Resources

List any deployed resources:

```bash
# List ECS instances
cloudctl list vm --provider huawei

# List RDS databases
cloudctl list database --provider huawei

# List CCE clusters
cloudctl list k8s --provider huawei

# List OBS buckets
cloudctl list storage --provider huawei
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
cloudctl destroy 12345678-90ab-cdef-1234-567890abcdef --provider huawei
cloudctl destroy pg-cluster --provider huawei
cloudctl destroy my-assets-bucket --provider huawei
```

> ⚠️ Warning: Deletion is irreversible. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy 12345678-90ab-cdef-1234-567890abcdef --provider huawei --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Huawei Cloud’s pricing:

```bash
cloudctl cost --provider huawei
```

Output:

```bash
Provider: huawei
Resource: my-ecs-instance (vm)
Type: s3.small.1
Estimated Monthly Cost: $4.50 USD

Resource: pg-cluster (database)
Type: rds.pg.s1.small
Estimated Monthly Cost: $30.00 USD
```

Estimates include compute, storage, and data transfer.

---

## Authentication & Security

CloudCTL uses Huawei Cloud Access Key + Secret Key with encrypted storage.

* Keys are scoped to your IAM user
* Full audit log via Cloud Trace Service (CTS)
* Never stored in config files

---

## Best Practices

Use IAM users with least-privilege policies
Enable MFA for root account
Use `--dry-run` before production changes
Tag resources for cost allocation
Use VPCs for secure internal networking
