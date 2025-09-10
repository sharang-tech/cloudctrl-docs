# Vultr

Vultr is a global cloud infrastructure provider known for its high-frequency compute, bare metal servers, and developer-friendly API. With CloudCTL, you can manage Virtual Machines, Bare Metal servers, Kubernetes (VKE), Managed Databases, Object Storage, and Load Balancers—all from a single, consistent CLI.

No more context-switching between dashboards or writing custom automation scripts. CloudCTL brings **universal control** to your Vultr infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## 🔧 Supported Resource Types

CloudCTL supports the following Vultr services:

| Resource Type     | Maps To                       | Description                                                      |
| ----------------- | ----------------------------- | ---------------------------------------------------------------- |
| `vm`            | Cloud Instances               | Virtual machines with SSD/NVMe storage, IPv6, and global regions |
| `bare-metal`    | Bare Metal Servers            | Dedicated physical servers with high-performance CPUs and GPUs   |
| `k8s`           | VKE (Vultr Kubernetes Engine) | Managed Kubernetes clusters                                      |
| `database`      | Managed Databases             | PostgreSQL, Redis, and MongoDB clusters                          |
| `storage`       | Object Storage                | S3-compatible buckets                                            |
| `load-balancer` | Load Balancers                | TCP/HTTP(S) load balancing with health checks and SSL            |

All resources support full lifecycle management—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## 🚀 Quick Start

Get started with Vultr in three steps.

### 1. Generate an API Key

1. Log in to the [Vultr Customer Portal](https://my.vultr.com)
2. Go to **Account > API**
3. Click **Generate New API Key**
4. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets vultr api_key
```

You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

### 3. (Optional) Set Default Region

Edit `~/.cloudctl/config.yaml`:

```YAML
providers:
  vultr:
    enabled: true
    region: ewr  # New Jersey
```

Or pass `--region` in every command.

---

## Example Commands

### Deploy a Cloud Instance (VM)

```bash
cloudctl deploy vm web-server \
  --provider vultr \
  --region sfo \
  --type vc2-1c-1gb \
  --os_id 467  # Ubuntu 22.04 x64
  --name my-vm
```

* Uses default SSH keys (if configured)
* Returns instance ID, IP, status, and console URL
* Supports snapshots, backups, and private networking

### Deploy a Bare Metal Server

```bash
cloudctl deploy bare-metal db-server \
  --provider vultr \
  --region iad \
  --plan mb-1c-2gb \
  --os_id 467 \
  --name bare-metal-db
```

* Full dedicated hardware
* Ideal for high-I/O databases or AI workloads
* Returns server ID, IP, and provisioning status

### Create a VKE Kubernetes Cluster

```bash
cloudctl deploy k8s cluster-1 \
  --provider vultr \
  --region ord \
  --node_pools '[{"count":2,"label":"pool-1","plan":"vc2-2c-2gb"}]' \
  --version 1.28.6
```

* Auto-generates kubeconfig
* Returns cluster ID, endpoint, and status
* Supports auto-scaling node pools

### Launch a Managed PostgreSQL Database

```bash
cloudctl deploy database production-db \
  --provider vultr \
  --engine postgres \
  --type vc2-2c-2gb \
  --region ewr \
  --name prod-cluster
```

* Enables private networking by default
* Auto-configures connection string
* Returns primary endpoint, port, and status

### Create an Object Storage Bucket

```bash
cloudctl deploy storage user-uploads \
  --provider vultr \
  --region ewr \
  --name my-assets-bucket
```

* Creates an S3-compatible bucket
* Returns S3 endpoint and access credentials
* Supports CORS and lifecycle rules

### Deploy a Load Balancer

```bash
cloudctl deploy load-balancer api-lb \
  --provider vultr \
  --region ewr \
  --instances '["a1b2c3d4","e5f6g7h8"]' \
  --forwarding_rules '[{"frontend_protocol":"http","frontend_port":80,"backend_protocol":"http","backend_port":80}]' \
  --health_check '{"protocol":"tcp","port":22}'
```

* Supports HTTP, HTTPS, and TCP
* Configurable health checks and SSL certificates
* Returns public IP and status

---

## List Resources

```bash
# List all Cloud Instances
cloudctl list vm --provider vultr

# List Bare Metal servers
cloudctl list bare-metal --provider vultr

# List VKE clusters
cloudctl list k8s --provider vultr

# List Managed Databases
cloudctl list database --provider vultr

# List Object Storage buckets
cloudctl list storage --provider vultr
```

Output includes:

* ID
* Name
* Status
* Region
* Plan/Type
* IPv4/Endpoint

---

## Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy a1b2c3d4 --provider vultr
cloudctl destroy cluster-1 --provider vultr
cloudctl destroy prod-cluster --provider vultr
```

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy a1b2c3d4 --provider vultr --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Vultr’s pricing:

```bash
cloudctl cost
```

Output:

```bash
Provider: vultr
Resource: my-vm (vm)
Type: vc2-1c-1gb
Estimated Monthly Cost: $5.00 USD

Resource: prod-cluster (database)
Type: vc2-2c-2gb
Estimated Monthly Cost: $30.00 USD
```

Estimates include compute, storage, and transfer where applicable.

---

## Authentication & Security

CloudCTL uses Vultr API Keys with encrypted storage.

* Keys are scoped to your account
* Full audit log in Vultr dashboard
* Never stored in config files

---

## Best Practices

* Use separate API keys for different environments
* Enable two-factor authentication (2FA)
* Use --dry-run before production changes
* Tag resources with --tag for organization
* Use private networking for internal services
