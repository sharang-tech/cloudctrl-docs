# Neon

Neon is a **serverless PostgreSQL** platform that separates storage and compute, enabling instant branching, autoscaling, and bottomless storage. With CloudCTL, you can manage PostgreSQL projects, branches, and compute endpoints—all from a single, consistent CLI.

No more juggling dashboards or writing custom scripts. CloudCTL brings **universal control** to your Neon infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Neon services:

| Resource Type | Maps To           | Description                                                     |
| ------------- | ----------------- | --------------------------------------------------------------- |
| `database`  | Projects          | Create and manage PostgreSQL databases                          |
| `branch`    | Database Branches | Spin up isolated branches for development, staging, and testing |
| `endpoint`  | Compute Endpoints | Control compute instances attached to branches                  |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Neon in three steps.

### 1. Generate an API Key

1. Log in to the [Neon Dashboard](https://console.neon.tech)
2. Go to **Settings → API Keys**
3. Click **Generate API Key**
4. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets neon api_key
```

You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands

### Deploy a PostgreSQL Database (Project)

```bash
cloudctl deploy database analytics-db \
  --provider neon \
  --name analytics-db \
  --region aws-us-east-2 \
  --plan free
```

* Creates a new PostgreSQL project
* Uses the Free Tier by default
* Returns project ID, connection URI, and status

### Create a Database Branch

```bash
cloudctl deploy branch dev \
  --provider neon \
  --project_id prj-123abc456def \
  --parent_id br-main
```

* Creates an isolated branch from `main`
* Ideal for testing schema changes without affecting production
* Each branch has its own compute endpoint

### Create a Staging Branch

```bash
cloudctl deploy branch staging \
  --provider neon \
  --project_id prj-123abc456def \
  --parent_id br-main
```

* Used for pre-production testing
* Supports custom compute scaling

### Deploy a Compute Endpoint

```bash
cloudctl deploy endpoint api-endpoint \
  --provider neon \
  --project_id prj-123abc456def \
  --branch_id br-dev-789xyz \
  --type read_write \
  --autoscaling enabled
```

* Attaches compute to a branch
* Supports `read_write` or `read_only` endpoints
* Enables autoscaling to zero when idle

---

## List Resources

List any deployed resources:

```bash
# List all databases (projects)
cloudctl list database --provider neon

# List branches for a project
cloudctl list branch --provider neon --filter project_id=prj-123abc456def

# List compute endpoints
cloudctl list endpoint --provider neon --filter project_id=prj-123abc456def
```

Output includes:

* ID
* Name
* Status
* Region
* Created At
* Parent Branch (for branches)
* Endpoint Type (for endpoints)

---

## Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy prj-123abc456def --provider neon
cloudctl destroy br-dev-789xyz --provider neon
cloudctl destroy ep-abc123def456 --provider neon
```

> ⚠️ Warning: Deletion is irreversible and results in data loss. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy prj-123abc456def --provider neon --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Neon’s pricing:

```bash
cloudctl cost --provider neon
```

Output:

```bash

Provider: neon
Resource: analytics-db (database)
Plan: free
Estimated Monthly Cost: $0.00 USD

Resource: dev (branch)
Type: development
Estimated Monthly Cost: $0.00 USD

```

* Free Tier: 3 projects, 10 branches, 25 GB storage, 100 compute hours/month
* Pro Tier: $25/month (unlimited branches, 500 GB storage, 2000 compute hours)

Costs are based on compute usage and storage.

---

## Authentication & Security

CloudCTL uses Neon API Keys with encrypted storage.

* Keys are account-scoped
* Full audit log in Neon dashboard
* Never stored in config files

---

## Best Practices

* Use separate API keys for CI/CD and personal access
* Use branches for all development and testing
* Enable autoscaling to reduce costs
* Use --dry-run before production changes
* Monitor compute usage to optimize costs
