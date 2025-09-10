# DigitalOcean

DigitalOcean is a developer-friendly cloud platform known for its simplicity, predictable pricing, and robust API. With CloudCTL, you can manage Droplets, Kubernetes clusters, managed databases, Spaces (object storage), Load Balancers, and CDN—all from a single, consistent CLI.

No more switching between dashboards or writing custom scripts. CloudCTL brings **universal control** to your DigitalOcean infrastructure, with full support for `deploy`, `list`, `destroy`, and real-time cost estimation—all with `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following DigitalOcean services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `vm` | Droplets | Virtual machines with SSD storage, IPv6, and global regions |
| `k8s` | Kubernetes (DOKS) | Managed Kubernetes clusters |
| `database` | Managed Databases | PostgreSQL, MySQL, Redis, and MongoDB clusters |
| `storage` | Spaces | S3-compatible object storage |
| `load-balancer` | Load Balancers | TCP/HTTP(S) balancing with health checks |
| `cdn` | CDN Endpoints | Global content delivery with custom domains |

All resources support full lifecycle management: creation, listing, and deletion—with dry-run previews and cost tracking.

---

## Quick Start

Get started with DigitalOcean in three steps.

### 1. Generate an API Token

1. Go to the [DigitalOcean API Settings](https://cloud.digitalocean.com/account/api/tokens)
2. Click **"Generate New Token"**
3. Give it a name (e.g., `cloudctl-token`) and select **Read and Write** access
4. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets digitalocean api_token
```
You’ll be prompted to enter the token. It is stored encrypted using your system’s keyring—never in plaintext.

### 3. (Optional) Set Default Region
Edit `~/.cloudctl/config.yaml`:

```YAML
providers:
  digitalocean:
    enabled: true
    region: nyc1
```
Or pass `--region` in every command.

---

## Example Commands
### Deploy a Droplet (VM)
```bash
cloudctl deploy vm web-server \
  --provider digitalocean \
  --region sfo3 \
  --type s-1vcpu-1gb \
  --model ubuntu-22-04-x64 \
  --name my-droplet
```

* Uses default SSH keys (if configured)
* Returns Droplet ID, IP, status, and console URL
* Supports custom images, snapshots, and backups

### Create a Kubernetes Cluster
```bash
cloudctl deploy k8s staging-cluster \
  --provider digitalocean \
  --version 1.28.6-do.0 \
  --node_pools '[{"type":"s-1vcpu-2gb","count":2,"name":"pool-1"}]' \
  --region nyc1
```

* Auto-configures kubectl context
* Returns kubeconfig download link and cluster endpoint
* Node pools support auto-scaling (when enabled)

### Launch a Managed PostgreSQL Database
```bash
cloudctl deploy database production-db \
  --provider digitalocean \
  --engine pg \
  --version 14 \
  --type db-s-1vcpu-1gb \
  --region nyc3 \
  --name prod-cluster
```

* Auto-generates connection string
* Enables private networking by default
* Returns primary endpoint, port, and status

### Create a Space (Object Storage)
```bash
cloudctl deploy storage user-uploads \
  --provider digitalocean \
  --region nyc3 \
  --name my-assets-space
```

* Creates a globally unique bucket (`my-assets-space.nyc3.digitaloceanspaces.com`)
* Enables CORS and lifecycle rules (configurable)
* Returns endpoint URL

### Deploy a Load Balancer
```bash
cloudctl deploy load-balancer api-lb \
  --provider digitalocean \
  --region nyc1 \
  --droplet_ids 12345678,87654321 \
  --forwarding_rules '[{"entry_protocol":"http","entry_port":80,"target_protocol":"http","target_port":80}]'
```

* Supports HTTP, HTTPS, TCP, and SSL termination
* Auto-attaches to Droplets
* Configurable health checks and session persistence

### Create a CDN Endpoint
```bash
cloudctl deploy cdn cdn-assets \
  --provider digitalocean \
  --origin my-space.nyc3.digitaloceanspaces.com \
  --ttl 3600
```

* Caches content at global edge locations
* Supports custom domains and SSL
* Returns CDN domain (e.g., `cdn-assets.fra1.cdn.digitaloceanspaces.com`)

---

## List Resources
List any deployed resources:

```bash
# List all Droplets
cloudctl list vm --provider digitalocean

# List Kubernetes clusters
cloudctl list k8s --provider digitalocean

# List Managed Databases
cloudctl list database --provider digitalocean

# List Spaces
cloudctl list storage --provider digitalocean
```

Output includes:

* ID
* Name
* Status
* Region
* Size/Type
* Creation Date

---

## Destroy Resources
Safely terminate resources:

```bash
cloudctl destroy 12345678 --provider digitalocean
cloudctl destroy staging-cluster --provider digitalocean
cloudctl destroy prod-cluster --provider digitalocean
```

> ⚠️ Warning: Deletion is irreversible. Use --dry-run first. 

### Dry-Run Mode
Preview changes safely:

```bash
cloudctl destroy 12345678 --provider digitalocean --dry-run
```

---

##  Cost Estimation
CloudCTL estimates monthly costs based on DigitalOcean’s pricing:

```bash
cloudctl cost --provider digitalocean
```

Output:
```bash
Provider: digitalocean
Resource: my-droplet (vm)
Type: s-1vcpu-1gb
Estimated Monthly Cost: $5.00 USD

Resource: prod-cluster (database)
Type: db-s-1vcpu-1gb
Estimated Monthly Cost: $15.00 USD
```

Estimates include compute, storage, and transfer where applicable.

---

## Authentication & Security
CloudCTL uses DigitalOcean Personal Access Tokens with encrypted storage.

* Tokens are scoped to your account
* Full audit log in DigitalOcean console
* Never stored in config files

---

# Best Practices
* Use team tokens for shared access
* Enable two-factor authentication (2FA)
* Use `--dry-run` before production changes
* Tag resources with `--tag` for organization
* Use Spaces with CDN for static site performance