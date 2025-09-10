# PlanetScale

PlanetScale is a **serverless MySQL-compatible database** platform built on Vitess, offering Git-like branching for zero-downtime schema changes. With CloudCTL, you can manage databases and branches across development, staging, and production environments—all from a single, consistent CLI.

No more juggling the dashboard or writing custom scripts. CloudCTL brings **universal control** to your PlanetScale infrastructure, with full support for `deploy`, `list`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: PlanetScale does not support deletion via its public API. Destructive actions must be performed through the dashboard.

---

## Supported Resource Types

CloudCTL supports the following PlanetScale services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `database` | Databases | Create and manage MySQL-compatible databases |
| `branch` | Database Branches | Spin up isolated branches for development and testing |

All resources support `deploy` and `list` operations, with real-time cost estimation and dry-run previews.

---

## Quick Start

Get started with PlanetScale in three steps.

### 1. Generate an API Token

1. Log in to the [PlanetScale Dashboard](https://app.planetscale.com)
2. Go to **Settings → API Tokens**
3. Click **Generate Token**
4. Select **Create databases and branches** or **Admin access**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets planetscale api_token
```

You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

### 3. Set Your Organization
Edit `~/.cloudctl/config.yaml`:
```YAML
providers:
  planetscale:
    enabled: true
    organization: your-org-name
```
Replace `your-org-name` with your PlanetScale organization slug.

---

## Example Commands
### Deploy a Database
```bash
cloudctl deploy database analytics-db \
  --provider planetscale \
  --name analytics-db \
  --region us-east
```

* Creates a new MySQL-compatible database
* Uses the Hobby (free) plan by default
* Returns database name and status

### Create a Database Branch
```bash
cloudctl deploy branch dev \
  --provider planetscale \
  --database analytics-db \
  --parent main
```

* Creates an isolated branch from `main`
* Ideal for testing schema changes
* Connect using the branch-specific connection string

### Create a Staging Branch
```bash
cloudctl deploy branch staging \
  --provider planetscale \
  --database analytics-db \
  --parent main
```

* Used for pre-production testing
* Can be promoted via deploy requests

---

## List Resources
List any deployed resources:

```bash
# List all databases
cloudctl list database --provider planetscale

# List branches for a database
cloudctl list branch --provider planetscale --filter database=analytics-db
```

Output includes:

* ID
* Name
* Status
* Region
* Created At
* Parent Branch (for branches)

---

## Destroy Resources

PlanetScale does not support deletion via its public API.

---

## Cost Estimation
CloudCTL estimates monthly costs based on PlanetScale’s pricing:

```bash
cloudctl cost --provider planetscale
```

Output:
```bash
Provider: planetscale
Resource: analytics-db (database)
Plan: hobby
Estimated Monthly Cost: $0.00 USD

Resource: dev (branch)
Type: development
Estimated Monthly Cost: $0.00 USD
```

* Hobby Plan: Free (1 database, unlimited branches)
* Pro Plan: $24/month (production-ready)

Branches are free; costs are based on the parent database plan.

---

## Authentication & Security
CloudCTL uses PlanetScale API Tokens with encrypted storage.

* Tokens are scoped to your organization
* Full audit log in PlanetScale dashboard
* Never stored in config files

---
## Best Practices

* Use separate tokens for CI/CD and personal access
* Use branches for all schema changes
* Promote changes via deploy requests (not direct merges)
* Use `--dry-run` to preview branch creation
* Monitor performance via PlanetScale’s built-in insights