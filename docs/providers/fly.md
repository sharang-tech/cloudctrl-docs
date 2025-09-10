# Fly.io

Fly.io runs your applications close to users with minimal latency, using a global edge network. With CloudCTL, you can deploy and manage Fly.io apps, VMs (Machines), PostgreSQL and Redis databases, and persistent volumes—all from a single, consistent CLI.

No more juggling `flyctl` or the dashboard. CloudCTL brings **universal control** to your Fly.io infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Fly.io services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `app` | Fly Apps | Logical containers for services, databases, and workers |
| `vm` | Machines | VM-like instances with full control over lifecycle |
| `database` | PostgreSQL, Redis Clusters | Managed databases with built-in replication and backups |
| `storage` | Volumes | Persistent storage attached to Machines |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Fly.io in three steps.

### 1. Generate a Fly.io Access Token

1. Log in to the [Fly.io Dashboard](https://fly.io)
2. Go to **Account Settings → Access Tokens**
3. Click **Create Token**
4. Name it (e.g., `cloudctl-token`) and click **Create**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets fly access_token
```
You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

---

##  Example Commands
### Create a Fly.io App
```bash
cloudctl deploy app my-web-app \
  --provider fly \
  --org personal \
  --region ord
```

* Apps are required to group services and databases
* Uses your personal organization by default
* Returns app name and status

### Deploy a Machine (VM)
```bash
cloudctl deploy vm api-server \
  --provider fly \
  --app my-web-app \
  --region ord \
  --image ubuntu:22.04 \
  --size shared-cpu-1x
```

* Full control over startup and shutdown
* Supports custom Docker images
* Returns machine ID and status

### Launch a PostgreSQL Database
```bash
cloudctl deploy database production-db \
  --provider fly \
  --app my-db-app \
  --engine postgres \
  --region ord \
  --name primary-db
```

* Auto-configures replication and backups
* Enables private networking
* Returns connection string and internal hostname

### Deploy a Redis Instance
```bash
cloudctl deploy database cache-store \
  --provider fly \
  --engine redis \
  --app my-cache-app \
  --region ord \
  --name redis-cache
```

* In-memory store with persistence
* Ideal for sessions, queues, or caching
* Returns Redis URL

### Create a Persistent Volume
```bash
cloudctl deploy storage data-volume \
  --provider fly \
  --app my-web-app \
  --size 10 \
  --region ord \
  --name app-data
```

* Attached to Machines for persistent storage
* Size in GB (minimum 10GB)
* Mounted at `/data` by default

---

## List Resources
List any deployed resources:

```bash
# List all apps
cloudctl list app --provider fly

# List Machines
cloudctl list vm --provider fly

# List databases
cloudctl list database --provider fly

# List volumes
cloudctl list storage --provider fly
```
Output includes:

* ID
* Name/App
* Region
* Status
* Size/Type
* Creation Date

---

##  Destroy Resources
Safely terminate resources:

```bash
cloudctl destroy a1b2c3d4 --provider fly
cloudctl destroy my-db-app --provider fly
cloudctl destroy data-volume --provider fly
```

>⚠️ Warning: Deletion is irreversible. Use --dry-run first.

---

## Cost Estimation
CloudCTL estimates monthly costs based on Fly.io’s pricing:

```bash
cloudctl cost
```

Output:
```bash
Provider: fly
Resource: api-server (vm)
Size: shared-cpu-1x
Estimated Monthly Cost: $5.00 USD

Resource: primary-db (database)
Engine: postgres
Estimated Monthly Cost: $5.00 USD (free tier)
```

Estimates include compute, storage, and memory. Free-tier resources are marked accordingly.

---

##  Authentication & Security
CloudCTL uses Fly.io Access Tokens with encrypted storage.

* Tokens are account-scoped
* Full audit log in Fly.io dashboard
* Never stored in config files

---

##  Best Practices

* Use separate tokens for CI/CD and personal access
* Use `--dry-run` before production changes
* Store secrets using fly secrets set (not in environment variables)
* Use private networking for internal services
* Monitor logs via fly logs or external tools