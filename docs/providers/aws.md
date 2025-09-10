# AWS

Amazon Web Services (AWS) is the most comprehensive cloud platform, offering over 200 services. With CloudCTL, you gain unified access to core AWS infrastructure—including EC2, RDS, EKS, S3, ELB, and CloudFront—using a consistent, dry-run-safe CLI.

No more juggling the AWS Console, CLI, or SDKs. CloudCTL abstracts the complexity while preserving full functionality.

---

## Supported Resource Types

CloudCTL supports the following resource types on AWS:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `vm` | EC2 Instances | Virtual machines with customizable AMIs, instance types, and networking |
| `database` | RDS (Relational Database Service) | Managed PostgreSQL, MySQL, MariaDB, Oracle, and SQL Server |
| `k8s` | EKS (Elastic Kubernetes Service) | Managed Kubernetes clusters |
| `storage` | S3 Buckets | Object storage for files, backups, and static assets |
| `load-balancer` | ELB (Elastic Load Balancer) | Application and Network Load Balancers |
| `cdn` | CloudFront | Content delivery network for fast global distribution |

All resources support `deploy`, `list`, and `destroy` (where API allows), with real-time cost estimation.

---

## Quick Start

Get started with AWS in three steps.

### 1. Generate AWS Credentials

1. Go to the [IAM Console](https://console.aws.amazon.com/iam)
2. Create a new user with programmatic access
3. Attach policies: `AmazonEC2FullAccess`, `AmazonRDSFullAccess`, `AmazonEKSFullAccess`, `AmazonS3FullAccess`, `AWSElasticLoadBalancingV2FullAccess`, `CloudFrontFullAccess`
4. Save the **Access Key ID** and **Secret Access Key**

### 2. Save Credentials Securely

```bash
cloudctl secrets aws access_key_id
cloudctl secrets aws secret_access_key
```
You’ll be prompted to enter each value. They are stored encrypted via your system’s keyring.

### 3. Set Default Region (Optional)
Edit `~/.cloudctl/config.yaml`:
```YAML
providers:
  aws:
    enabled: true
    region: us-east-1
```
Or pass `--region` in every command.

---

### Example Commands

#### Deploy an EC2 Instance
```bash
cloudctl deploy vm ubuntu \
  --provider aws \
  --region us-west-2 \
  --type t3.micro \
  --name web-server \
  --model ami-0abcdef1234567890
```
* `--model` specifies the AMI ID
* Uses default VPC and security group
* Returns instance ID, public IP, and status

#### Launch an RDS PostgreSQL Database
```bash
cloudctl deploy database production-db \
  --provider aws \
  --engine postgres \
  --type db.t3.micro \
  --region us-east-1 \
  --name production-db
```

* Auto-generates credentials
* Enables public accessibility by default
* Returns endpoint, port, and status

#### Create an EKS Kubernetes Cluster
```bash
cloudctl deploy k8s cluster-1 \
  --provider aws \
  --version 1.28 \
  --node_pools '[{"type":"t3.medium","count":2,"labels":{"env":"prod"}}]' \
  --region us-east-1
```

* Creates a managed control plane
* Node pools are provisioned in default VPC
* Returns cluster ARN and status

#### Deploy an S3 Bucket
```bash
cloudctl deploy storage user-uploads \
  --provider aws \
  --region us-east-1 \
  --name user-uploads-bucket
```

* Creates a globally unique bucket
* Enables versioning and server-side encryption
* Returns bucket URL

#### Create an Application Load Balancer
```bash
cloudctl deploy load-balancer api-lb \
  --provider aws \
  --region us-east-1 \
  --subnets subnet-12345678,subnet-87654321 \
  --security_groups sg-12345678
```

* Supports HTTP/HTTPS listeners
* Attaches to specified subnets and security groups

#### Deploy a CloudFront CDN
```bash
cloudctl deploy cdn static-cdn \
  --provider aws \
  --origin my-bucket.s3.amazonaws.com \
  --region us-east-1
```

* Creates a distribution with default cache behavior
* Returns distribution domain (e.g., `d123.cloudfront.net`)

---

### List Resources
List any deployed resources:

```bash
# List all EC2 instances
cloudctl list vm --provider aws --region us-east-1

# List RDS databases
cloudctl list database --provider aws

# List EKS clusters
cloudctl list k8s --provider aws
```

Output includes:

* Resource ID
* Name
* Status
* Region
* Type
* Creation time

---

### Destroy Resources
Safely terminate resources:

```bash
cloudctl destroy i-0abcdef1234567890 --provider aws
cloudctl destroy my-cluster --provider aws
cloudctl destroy my-bucket --provider aws
```

> ⚠️ Warning: Deletion is irreversible. Use --dry-run first.

#### Dry-Run Mode
Preview destruction:

```bash
cloudctl destroy i-0abcdef1234567890 --provider aws --dry-run
```

---

### Cost Estimation
CloudCTL estimates monthly costs based on AWS pricing:

```bash
cloudctl cost --provider aws
```

Output:
```bash
Provider: aws
Resource: web-server (vm)
Type: t3.micro
Estimated Monthly Cost: $9.50 USD

Resource: production-db (database)
Type: db.t3.micro
Estimated Monthly Cost: $30.00 USD
```

Estimates include compute, storage, and data transfer where applicable.

---

### Authentication & Security
CloudCTL uses AWS IAM credentials with least-privilege access. Never use root keys.

Supported authentication:

* `access_key_id`
* `secret_access_key`

Credentials are never logged or exposed.

---

###  Best Practices

* Use IAM roles with minimal required permissions
* Enable MFA for root account
* Use `--dry-run` before production changes
* Tag resources with `--tags Key=Value`
* Store sensitive data in AWS Systems Manager or Secrets Manager