# Linode

Linode, now part of Akamai, is a high-performance cloud provider known for its simplicity, global data centers, and developer-first approach. With CloudCTL, you can manage Linodes (VMs), Kubernetes (LKE), managed databases, object storage, NodeBalancers, and DNS—all from a single, consistent CLI.

No more juggling multiple tools or APIs. CloudCTL unifies Linode’s infrastructure under one command language, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Linode services:

| Resource Type     | Maps To                        | Description                                                            |
| ----------------- | ------------------------------ | ---------------------------------------------------------------------- |
| `vm`            | Linodes                        | High-performance virtual machines with NVMe storage and global regions |
| `k8s`           | LKE (Linode Kubernetes Engine) | Managed Kubernetes clusters                                            |
| `database`      | Managed Databases              | PostgreSQL, MySQL, and Redis clusters                                  |
| `storage`       | Object Storage                 | S3-compatible buckets                                                  |
| `load-balancer` | NodeBalancers                  | TCP/HTTP load balancing with health checks                             |
| `domain`        | Domains                        | DNS zone management                                                    |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Linode in three steps.

### 1. Generate a Personal Access Token

1. Log in to the [Linode Cloud Manager](https://cloud.linode.com)
2. Go to **Account > API Tokens**
3. Click **Create a Personal Access Token**
4. Grant **Read/Write** access to all services
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets linode api_key
```

You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

### 3. (Optional) Set Default Region

Edit `~/.cloudctl/config.yaml`:

```bash
providers:
  linode:
    enabled: true
    region: us-east
```

Or pass `--region` in every command.

---

## Example Commands

### Deploy a Linode (VM)

```bash
cloudctl deploy vm web-server \
  --provider linode \
  --region us-east \
  --type g6-nanode-1 \
  --image linode/ubuntu22.04 \
  --name my-linode
```

* Uses default SSH keys (if configured)
* Returns Linode ID, IP, status, and console URL
* Supports backups, private networking, and VLANs

### Create an LKE Kubernetes Cluster

```bash
cloudctl deploy k8s cluster-1 \
  --provider linode \
  --node_pools '[{"type":"g6-standard-1","count":3,"disks":[]}]' \
  --region us-central \
  --label my-lke-cluster
```

* Auto-generates kubeconfig
* Returns control plane status and endpoint
* Node pools support auto-scaling (when enabled)

### Launch a Managed PostgreSQL Database

```bash
cloudctl deploy database production-db \
  --provider linode \
  --engine postgresql \
  --type g6-standard-1 \
  --region us-east \
  --name prod-db-instance
```

* Enables private networking by default
* Auto-configures connection credentials
* Returns engine version, status, and endpoint

### Create an Object Storage Bucket

```bash
cloudctl deploy storage user-uploads \
  --provider linode \
  --region us-east-1 \
  --name my-assets-bucket
```

* Creates a globally unique S3-compatible bucket
* Returns endpoint (`https://my-assets-bucket.us-east-1.linodeobjects.com`)
* Supports CORS and lifecycle policies

### Deploy a NodeBalancer

```bash
cloudctl deploy load-balancer api-lb \
  --provider linode \
  --region us-east \
  --configs '[{"port":80,"protocol":"http","algorithm":"roundrobin","health_check":{"protocol":"tcp","port":22}}]' \
  --label web-lb
```

* Supports HTTP, HTTPS, and TCP
* Configurable health checks and SSL
* Returns public IP and status

### Manage a DNS Domain

```bash
cloudctl deploy domain example.com \
  --provider linode \
  --soa_email admin@example.com
```

* Creates a new DNS zone
* Auto-adds NS and SOA records
* Use `list domain` to view records

---

## List Resources

List any deployed resources:

```bash
# List all Linodes
cloudctl list vm --provider linode

# List LKE clusters
cloudctl list k8s --provider linode

# List Managed Databases
cloudctl list database --provider linode

# List Object Storage buckets
cloudctl list storage --provider linode

# List NodeBalancers
cloudctl list load-balancer --provider linode
```

Output includes:

* ID
* Name/Label
* Status
* Region
* Type
* IPv4/Endpoint

---

## Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy 1234567 --provider linode
cloudctl destroy cluster-1 --provider linode
cloudctl destroy prod-db-instance --provider linode
```

> ⚠️ Warning: Deletion is irreversible. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy 1234567 --provider linode --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Linode’s pricing:

```bash
cloudctl cost
```

Output:

```bash
Provider: linode
Resource: my-linode (vm)
Type: g6-nanode-1
Estimated Monthly Cost: $5.00 USD

Resource: prod-db-instance (database)
Type: g6-standard-1
Estimated Monthly Cost: $30.00 USD
```

Estimates include compute, storage, and transfer where applicable.

---

## Authentication & Security

CloudCTL uses Linode Personal Access Tokens with encrypted storage.

* Tokens are scoped to your account
* Full audit log in Linode Manager
* Never stored in config files

---

## Best Practices

* Use separate tokens for different environments (dev, prod)
* Enable two-factor authentication (2FA)
* Use `--dry-run` before production changes
* Tag resources with `--tags` for organization
* Use private IPs for internal communication
